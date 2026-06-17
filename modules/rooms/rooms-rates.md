# RoomsRates Component - Pseudocode Logic

## 1. CONSTRUCTOR - Initialize Component

```
FUNCTION Constructor():
    
    // Get route parameters from URL
    routeParams = Get parameters from activated route {
        arrival_date,
        departure_date,
        offer_code,
        guest_count
    }
    
    // Validate dates format
    IF (arrival_date is invalid format) OR (departure_date is invalid format):
        GET current hotel date from PMS
        CREATE new default dates (today + tomorrow)
        REDIRECT to new URL with valid dates
        RETURN
    
    // Load property and room data
    propertyData = Get from WbePropertyTransformerService
    roomViewModel = Initialize room view model
    guestMessages = Extract from propertyData
    
    // Listen to parameter changes
    SUBSCRIBE to route.params changes:
        WHEN params change:
            UPDATE roomViewModel
            UPDATE filters
            UPDATE sorting
    
    // Enable reactive effects for data synchronization
    SETUP effects for:
        - Language changes
        - Currency changes
        - Cart data updates
        - Filter changes
        - Player login/logout
        - Multi-year calendar selection
    
    // Load advanced filter configuration
    IF property has advancedFilter permission:
        GET filter config from API
        STORE filters for UI
```

---

## 2. NG-ON-INIT - First Time Setup

```
FUNCTION ngOnInit():
    
    // Google Analytics tracking
    PUSH event to GTM:
        event_name = "roomsPageLoad"
        page_name = "Rooms and Rates"
        casino_id = currentPlayer.casinoNumber OR "NA"
    
    // Load offer description if available
    IF loyaltyOfferData has offerDescription:
        SET roomViewModel.offerDescription = offerData
    
    // Scroll to top
    SCROLL window to position 0
```

---

## 3. REACTIVE EFFECTS - Monitor Data Changes

```
EFFECT 1: Language Change Handler
    WHEN language changes:
        GET new language from InternationalizationTransformerService
        GET filter data in new language
        UPDATE roomViewModel with new messages
        REFRESH room data if offers enabled
        
EFFECT 2: Filter & Currency Monitor
    WHEN advanced filter changes:
        FILTER room list by selected filter criteria
        SAVE backup of original room list
        
    WHEN currency changes:
        RECALCULATE all room prices in new currency
        UPDATE display
        
    WHEN cart data changes:
        SYNC cart status with room availability
        IF cart empty AND casino user logged in:
            REFRESH room rates
            
    WHEN room filter data changes:
        UPDATE filter state
        RE-SORT rooms
        
EFFECT 3: Player Login/Logout Handler
    WHEN player logs in OR logs out:
        IF offers dropdown enabled:
            SHOW loading indicator
        FETCH new offers based on player status
        
EFFECT 4: Calendar Year Change Handler
    WHEN user selects year in multi-year calendar:
        CLEAR previous calendar data
        REFRESH room rates for new year
        
EFFECT 5: User Login Status Monitor
    WHEN casino/membership login status changes:
        UPDATE isUserLoggedIn flag
        ENABLE offer features if logged in
```

---

## 4. REFRESH ROOM RATES - Main Data Load

```
FUNCTION refreshRoomRates(routeParams, queryParams):
    
    // Extract parameters
    arrival = routeParams.arrival
    departure = routeParams.departure
    offer = routeParams.offer
    guest_count = routeParams.guest
    
    // Initialize transformer services
    SET RoomFilterTransformerService.pmsResponse = PMS data
    SET RoomRatesTransformerService.pmsResponse = PMS data
    
    // Process guest limits
    guestTypeLimits = InitializeGuestTypeLimits(routeParams, queryParams)
    
    // Create filter data params
    filterParams = {
        arrival,
        departure,
        offer,
        guest_count,
        ...routeParams
    }
    
    // Store current offer in session
    STORE in session storage:
        key = "tenantId-propertyId-userSelectedOffer"
        value = offer_code
    
    // Load room data
    IF player is logged in OR player just logged out:
        isPlayerAction = TRUE
    ELSE:
        isPlayerAction = FALSE
    
    // Fetch and transform room data
    roomViewModel = CallRoomDataAPI(isPlayerAction)
    
    // Load resort fee details if enabled
    IF roomViewModel.displayResortFeeDetails:
        GET resort fee data from API
        DISPLAY resort fee information
    
    // Set default offer for navigation
    defaultOfferForNavigation = propertyData.defaultOffer
```

---

## 5. SORT ROOMS - Sort Logic

```
FUNCTION sortRooms(sortOrder):
    
    // Validate sorting is allowed
    IF sorting is disabled:
        RETURN
    
    // Get price values from DOM elements
    FOR EACH room in filtered rooms:
        FIND price element by room code ID
        EXTRACT price value from DOM text
        STORE price in room.calculatedPrice
    
    // Sort based on selected order
    SWITCH sortOrder:
        
        CASE "name_asc":
            SORT rooms by roomName (A to Z)
            FOR available rooms only (not blocked)
        
        CASE "name_dsc":
            SORT rooms by roomName (Z to A)
            FOR available rooms only
        
        CASE "price_asc":
            SORT rooms by calculatedPrice (low to high)
            Complementary rooms show first
            FOR available rooms only
        
        CASE "price_dsc":
            SORT rooms by calculatedPrice (high to low)
            Complementary rooms show first
            FOR available rooms only
        
        CASE "default_optn":
            RESET to original unsorted order
            RESTORE from backup
    
    // Cleanup - remove temporary price field
    FOR EACH room in filtered list:
        DELETE room.calculatedPrice
```

---

## 6. CALCULATE ROOM PRICE - Price Display

```
FUNCTION calculateAverageRateAndRoomTotal(room):
    
    // Start with base room total
    baseTotal = room.roomTotal
    
    // Add resort fees if enabled
    IF addResortFeeToRoomTotal is TRUE:
        resortFeeAmount = room.resortFeeTotal
    ELSE:
        resortFeeAmount = 0
    
    // Add other recurring fees if enabled
    IF addOtherFeeToRoomTotal is TRUE:
        otherFeeAmount = room.recurringChargesTotal
    ELSE:
        otherFeeAmount = 0
    
    // Calculate total with fees
    totalWithFees = baseTotal + resortFeeAmount + otherFeeAmount
    
    // Check display mode (TOTAL or AVERAGE per night)
    IF defaultPriceDisplay = "TOTAL":
        // Add taxes if enabled
        IF addTaxToCalendar is TRUE:
            totalTax = room.roomTaxTotal 
                     + room.resortFeeTotalTax 
                     + room.recurringChargesTotalTax
            totalWithFees = totalWithFees + totalTax
        
        RETURN totalWithFees
    
    ELSE IF defaultPriceDisplay = "AVERAGE":
        // Calculate average per night
        numberOfNights = room.rateSnapshot.length
        
        IF numberOfNights > 0:
            // Calculate average tax per night
            IF addTaxToCalendar is TRUE:
                totalTax = room.roomTaxTotal 
                         + room.resortFeeTotalTax 
                         + room.recurringChargesTotalTax
                averageTax = totalTax / numberOfNights
            ELSE:
                averageTax = 0
            
            // Calculate average total per night
            averageTotal = totalWithFees / numberOfNights
            averageTotal = averageTotal + averageTax
            
            RETURN averageTotal
        
        ELSE:
            THROW error "rateSnapshot is empty"
    
    ELSE:
        THROW error "Invalid price display mode"
```

---

## 7. BOOK ROOM - Booking Flow

```
FUNCTION openConfirmationPopup(room, source):
    
    // Check if confirmation popup is enabled
    IF bookingConfirmationPopup is disabled:
        // Skip popup and go directly to booking
        IF source = "CART":
            addToCart(room)
        ELSE:
            bookFromTop(room)
        RETURN
    
    // Get date range
    [arrival, departure] = GetArrivalAndDepartureDate()
    
    // Prepare dialog data
    dialogData = {
        room: room,
        selectedOffer: roomViewModel.selectedOffer,
        source: source,
        callbacks: {
            bookRoom: bookFromTop(),
            addToCart: addToCart(),
            getRoomDetails: getRoomDetails()
        },
        guestInfo: guestTypeLimit,
        dates: arrival + " - " + departure
    }
    
    // Open confirmation dialog
    OPEN MatDialog(RoomConfirmationDialogComponent, dialogData)
    
    // Track in Google Analytics
    EMIT google analytics event: "confirmation_popup_opened"
```

---

## 8. APPLY ROOM FILTERS - Advanced Filtering

```
FUNCTION onFilteredValueChanges(filterEvent):
    
    // Extract filter values from event
    filterValues = filterEvent.filterValueObj
    filters = filterEvent.filters
    
    // Check if any filter is active
    hasActiveFilter = ANY value in filterValues is not empty
    
    // Update filter count
    filterCount = COUNT of non-empty filter values
    
    // Enable clear button if filters exist
    showClearFilter = hasActiveFilter
    
    // Map filter values to display labels
    FOR EACH filter in filters:
        IF filter has matching values in filterValues:
            FIND selected values
            REPLACE key with label text
            STORE in displayFilterValues
    
    // Apply filters to room list
    FOR EACH room in allRooms:
        isMatched = TRUE
        
        FOR EACH filter in filters:
            filterKey = "searchText" + filter.mappingField + filter.order
            selectedValue = filterValues[filterKey]
            
            IF selectedValue exists:
                IF room does NOT match selectedValue:
                    isMatched = FALSE
                    BREAK
        
        IF isMatched:
            INCLUDE room in filteredRooms
        ELSE:
            EXCLUDE room
    
    // Update room display
    UPDATE roomViewModel.filteredItems = filteredRooms
    RE-SORT rooms
```

---

## 9. GET ROOM DETAILS - Modal Dialog

```
FUNCTION getRoomDetails(guestTypeLimit):
    
    // Mark guest count as edited
    isEditedGuestCount = TRUE
    
    // Store guest limit selection
    guestTypeLimit = guestTypeLimit
    
    // Fetch detailed room information
    roomDetails = CallRoomDetailsAPI(guestTypeLimit)
    
    // Update view model
    roomViewModel.roomDetails = roomDetails
```

---

## 10. ON DESTROY - Cleanup

```
FUNCTION ngOnDestroy():
    
    // Unsubscribe from all active subscriptions
    UNSUBSCRIBE from all room subscriptions
    
    // Reset component flags
    SET casinoOfferCallInitiated = FALSE
    
    // Reset DOM styles
    IF header element exists:
        REMOVE margin-top styling
    
    // Clear shared service data
    CLEAR calendarDetailsResponse
    CLEAR roomDataLoaded flag
    CLEAR all shared signals
    
    // Reset transformer service state
    CLEAR offer response
    CLEAR filter data
    SET offerlistapicalled = FALSE
    SET displayOfferDescription = FALSE
```

---

## 11. USER INTERACTION - Event Handlers

```
FUNCTION bookFromTop(room, index):
    // Set guest info in confirmation popup
    SetGuestTypeLimitInConfirmationPopUp(guestTypeLimit, isEditedGuestCount)
    
    // Track booking in Google Analytics
    EMIT google analytics event: "book_from_top"
    
    // Process booking
    booktheRoom(room, "BOOK")

FUNCTION addToCart(room, index):
    // Get updated guest info
    guestTypeLimit = GetCurrentGuestTypeLimit()
    
    // Add to cart via service
    addToCartOnBookingConfirmationPopUp(
        isEditedGuestCount,
        guestTypeLimit,
        room,
        index
    )
    
    // Process room booking
    booktheRoom(room, "CART")

FUNCTION roomDetail(room, index):
    // Track in Google Analytics
    EMIT google analytics event: "room_details_viewed"
    
    // Prepare dialog data
    dialogData = {
        room: room,
        selectedOffer: roomViewModel.selectedOffer,
        offerError: roomViewModel.offerInCartError,
        callbacks: {
            openConfirmation: openConfirmationPopup(),
            openPolicy: openCancellationPolicy()
        }
    }
    
    // Open room detail dialog
    OPEN MatDialog(RoomDetailComponent, dialogData)

FUNCTION resortFeeDetail():
    // Check if resort fee details are available
    IF isResortFeeDescAvailable:
        // Prepare dialog
        dialogData = {
            url: roomResortUrl,
            isImageConfigured: roomResortImgAvailable
        }
        
        // Open resort fee detail dialog
        OPEN MatDialog(ResortFeeDetailComponent, dialogData)

FUNCTION openCanellationDepositPolicy(room):
    // Prepare policy dialog
    dialogData = {
        depositPolicy: room.depositDescription,
        depositAmount: room.depositAmountDue,
        cancellationPolicy: room.cancelDescription,
        cancellationDueDate: room.cancelDueDate,
        cancellationAmount: room.cancelAmountDue
    }
    
    // Open policy dialog
    OPEN MatDialog(PolicymodelComponent, dialogData)
```

---

## 12. WINDOW RESIZE HANDLER

```
FUNCTION onWindowResize():
    
    // Detect mobile viewport
    isMobileView = (window.innerWidth <= 767)
    
    // Adjust header styling based on viewport
    IF isMobileView OR (banner should not display):
        SET header margin-top = "0"
    ELSE:
        SET header margin-top = "40px"
```

---

## Summary of Key Data Flows

1. **Initialization**: Load route params → Validate dates → Load property data → Initialize room view model
2. **Filtering**: Listen to filter changes → Apply filter criteria → Update room list
3. **Sorting**: Extract prices from DOM → Sort by selected order → Re-render
4. **Booking**: User clicks book → Open confirmation dialog → Add to cart or book → Navigate
5. **Reactive Updates**: Monitor signals for language/currency/cart changes → Update prices/filters → Re-render

---

## Key Services Used

- **RoomRatesTransformerService**: Main service for room data transformation
- **WbePropertyTransformerService**: Property and offer data management
- **AdvancedRoomFilterTransformerService**: Filter logic and application
- **PmsFilterService**: Filter state management
- **CommonCartLayer**: Shopping cart operations
- **GoogleAnalyticsEventsService**: Event tracking
- **SessionStorageService**: Store user preferences