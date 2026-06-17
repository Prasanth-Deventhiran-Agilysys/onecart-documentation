# Complete Route Definitions

## All Routes in Layout Routing Module

---

## 1. Feature Module Routes (Lazy-Loaded)

### Route: `/wbe`
**Purpose:** Room/Well-Being & Accommodations Booking
```typescript
{
  path: 'wbe',
  resolve: { roomResolvedData: RoomResolverService },
  loadChildren: () => import('../room/room.module').then((m) => m.RoomModule),
}
```
- **Module:** RoomModule
- **Resolver:** RoomResolverService (preloads room data)
- **Resolved Data Key:** `roomResolvedData`
- **Type:** Lazy-loaded feature module
- **Example URL:** `/wbe`

---

### Route: `/spa`
**Purpose:** Spa Services Booking
```typescript
{
  path: 'spa',
  resolve: { message: SpaResolverService },
  loadChildren: () => import('../spa/spa.module').then((m) => m.SpaModule),
}
```
- **Module:** SpaModule
- **Resolver:** SpaResolverService
- **Resolved Data Key:** `message`
- **Type:** Lazy-loaded feature module
- **Example URL:** `/spa`

---

### Route: `/series`
**Purpose:** Spa Series/Package Booking
```typescript
{
  path: 'series',
  resolve: { message: SpaResolverService },
  loadChildren: () => import('../spa/spa.module').then((m) => m.SpaModule),
}
```
- **Module:** SpaModule
- **Resolver:** SpaResolverService
- **Resolved Data Key:** `message`
- **Type:** Lazy-loaded feature module
- **Example URL:** `/series`
- **Note:** Reuses SpaModule with different path

---

### Route: `/classes`
**Purpose:** Spa Classes/Workshops Booking
```typescript
{
  path: 'classes',
  resolve: { message: SpaResolverService },
  loadChildren: () => import('../spa/spa.module').then((m) => m.SpaModule),
}
```
- **Module:** SpaModule
- **Resolver:** SpaResolverService
- **Resolved Data Key:** `message`
- **Type:** Lazy-loaded feature module
- **Example URL:** `/classes`
- **Note:** Reuses SpaModule with different path

---

### Route: `/golf`
**Purpose:** Golf Course Booking & Reservations
```typescript
{
  path: 'golf',
  resolve: { message: GolfResolverService },
  loadChildren: () => import('../golf/golf.module').then((m) => m.GolfModule),
}
```
- **Module:** GolfModule
- **Resolver:** GolfResolverService
- **Resolved Data Key:** `message`
- **Type:** Lazy-loaded feature module
- **Example URL:** `/golf`

---

### Route: `/activities`
**Purpose:** Activity/Experience Booking
```typescript
{
  path: 'activities',
  resolve: { message: SeatResolveService },
  loadChildren: () =>
    import('../activities/activities.module').then((m) => m.ActivitiesModule),
}
```
- **Module:** ActivitiesModule
- **Resolver:** SeatResolveService
- **Resolved Data Key:** `message`
- **Type:** Lazy-loaded feature module
- **Example URL:** `/activities`

---

### Route: `/seat`
**Purpose:** Seating Reservations & Seat Selection
```typescript
{
  path: 'seat',
  resolve: { message: SeatResolveService },
  loadChildren: () => import('../seat/seat.module').then((m) => m.SeatModule),
}
```
- **Module:** SeatModule
- **Resolver:** SeatResolveService
- **Resolved Data Key:** `message`
- **Type:** Lazy-loaded feature module
- **Example URL:** `/seat`

---

### Route: `/unit`
**Purpose:** Unit/Property Rentals
```typescript
{
  path: 'unit',
  resolve: { message: UnitresolverService },
  loadChildren: () => import('../unit/unit.module').then((m) => m.UnitModule),
}
```
- **Module:** UnitModule
- **Resolver:** UnitresolverService
- **Resolved Data Key:** `message`
- **Type:** Lazy-loaded feature module
- **Example URL:** `/unit`

---

### Route: `/giftcard`
**Purpose:** Gift Card Management & Purchase
```typescript
{
  path: 'giftcard',
  resolve: { message: GiftcardResolveService },
  loadChildren: () =>
    import('../gift-card/gift-card.module').then((m) => m.GiftCardModule),
}
```
- **Module:** GiftCardModule
- **Resolver:** GiftcardResolveService
- **Resolved Data Key:** `message`
- **Type:** Lazy-loaded feature module
- **Example URL:** `/giftcard`

---

### Route: `/snc`
**Purpose:** Special Booking/SNC Module
```typescript
{
  path: 'snc',
  loadChildren: () =>
    import('../snc/snc.module').then((m) => m.SncModule),
  canActivate: [sncAuthguardGuard],
}
```
- **Module:** SncModule
- **Resolver:** None
- **Auth Guard:** sncAuthguardGuard
- **Type:** Lazy-loaded feature module with authentication
- **Example URL:** `/snc`
- **Access:** Requires authentication via sncAuthguardGuard

---

### Route: `/cart-v2`
**Purpose:** Shopping Cart Management (Version 2)
```typescript
{
  path: 'cart-v2',
  // resolve: { message: CartResolverService },
  loadChildren: () => import('../cart/cart.module').then((m) => m.CartModule),
}
```
- **Module:** CartModule
- **Resolver:** None (commented out)
- **Type:** Lazy-loaded feature module
- **Example URL:** `/cart-v2`
- **Note:** Resolver currently disabled

---

### Route: `/headless-filter`
**Purpose:** Headless Filter Interface for API/Frontend Decoupling
```typescript
{
  path: 'headless-filter',
  loadChildren: () => import('../headless/headless.module').then(m => m.HeadlessModule)
}
```
- **Module:** HeadlessModule
- **Resolver:** None
- **Type:** Lazy-loaded feature module
- **Example URL:** `/headless-filter`
- **Use Case:** Standalone filtering interface

---

## 2. Authentication & Access Control Routes

### Route: `/login/:tenant/:property`
**Purpose:** User Login Page
```typescript
{
  path: 'login/:tenant/:property',
  component: LoginComponent,
}
```
- **Component:** LoginComponent
- **Resolver:** None
- **Auth Guard:** None
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
- **Example URL:** `/login/acme/property-001`
- **Functionality:** Authenticates user to specific tenant/property

---

### Route: `/accessRestricted/:tenant/:property`
**Purpose:** Access Denied / Restricted Access Page
```typescript
{
  path: 'accessRestricted/:tenant/:property',
  component: AccessRestrictedComponent,
}
```
- **Component:** AccessRestrictedComponent
- **Resolver:** None
- **Auth Guard:** None
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
- **Example URL:** `/accessRestricted/acme/property-001`
- **Functionality:** Displays access denied message

---

### Route: `/profile-login/:tenant/:property`
**Purpose:** Profile Login Wrapper
```typescript
{
  path: 'profile-login/:tenant/:property',
  // resolve: { message: OnecartResolveService },
  component: ProfileWrapperComponent,
}
```
- **Component:** ProfileWrapperComponent
- **Resolver:** None (commented out)
- **Auth Guard:** None
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
- **Example URL:** `/profile-login/acme/property-001`
- **Functionality:** Wrapper for profile-based authentication

---

## 3. Profile Routes

### Route: `/profile/:tenant/:property`
**Purpose:** User Profile & Reservation Management
```typescript
{
  path: 'profile/:tenant/:property',
  loadChildren: () =>
    import('../shared/widgets/profile/profile.module').then((m) => m.ProfileModule),
  data: { route: 'reservation' },
  canActivate: [ProfileAuthGuardService],
}
```
- **Module:** ProfileModule (lazy-loaded)
- **Resolver:** None
- **Auth Guard:** ProfileAuthGuardService ✓
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
- **Route Data:**
  - `route: 'reservation'` - Identifies this as a reservation profile route
- **Example URL:** `/profile/acme/property-001`
- **Access:** Requires authentication
- **Functionality:** User profile dashboard and reservation management

---

## 4. Reservation View Routes

### Route: `/myres/:tenant/:property/:confno/:lname/:arrdate`
**Purpose:** View Specific Reservation with Full Details
```typescript
{
  path: 'myres/:tenant/:property/:confno/:lname/:arrdate',
  resolve: { message: CartResolverService },
  component: ViewmyreservationComponent,
}
```
- **Component:** ViewmyreservationComponent
- **Resolver:** CartResolverService ✓
- **Auth Guard:** None
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
  - `confno` (string) - Confirmation number
  - `lname` (string) - Guest last name
  - `arrdate` (string) - Arrival date (format: YYYY-MM-DD)
- **Resolved Data Key:** `message`
- **Example URL:** `/myres/acme/property-001/ABC123/Smith/2024-05-15`
- **Functionality:** Displays complete reservation details with all parameters

---

### Route: `/myres/:tenant/:property/:confno/:lname`
**Purpose:** View Reservation with Name & Confirmation
```typescript
{
  path: 'myres/:tenant/:property/:confno/:lname',
  component: ViewmyreservationComponent,
  resolve: { message: CartResolverService },
  data: { route: 'reservation' },
  canActivate: [ReservationRoutingPermisssionResolverService],
}
```
- **Component:** ViewmyreservationComponent
- **Resolver:** CartResolverService ✓
- **Auth Guard:** ReservationRoutingPermisssionResolverService ✓
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
  - `confno` (string) - Confirmation number
  - `lname` (string) - Guest last name
- **Route Data:**
  - `route: 'reservation'` - Identifies as reservation route
- **Resolved Data Key:** `message`
- **Example URL:** `/myres/acme/property-001/ABC123/Smith`
- **Access:** Requires permission validation
- **Functionality:** Displays reservation without arrival date

---

### Route: `/myres/:tenant/:property/:confno`
**Purpose:** View Reservation with Confirmation Only
```typescript
{
  path: 'myres/:tenant/:property/:confno',
  component: ViewmyreservationComponent,
  resolve: { message: CartResolverService },
  data: { route: 'reservation' },
  // canActivate: [ReservationRoutingPermisssionResolverService],
}
```
- **Component:** ViewmyreservationComponent
- **Resolver:** CartResolverService ✓
- **Auth Guard:** None (commented out)
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
  - `confno` (string) - Confirmation number
- **Route Data:**
  - `route: 'reservation'` - Identifies as reservation route
- **Resolved Data Key:** `message`
- **Example URL:** `/myres/acme/property-001/ABC123`
- **Access:** No guard enforced
- **Functionality:** Displays reservation using confirmation number only
- **Note:** Guard is commented out - consider security implications

---

## 5. Reservation Modification Routes

### Route: `/modifyres/:tenant/:property/:confno/:lname/:arrdate`
**Purpose:** Modify Existing Reservation
```typescript
{
  path: 'modifyres/:tenant/:property/:confno/:lname/:arrdate',
  resolve: { message: CartResolverService },
  component: ModifyRoomReservationsComponent,
  canDeactivate: [CanDeactivateGuard]
}
```
- **Component:** ModifyRoomReservationsComponent
- **Resolver:** CartResolverService ✓
- **Deactivation Guard:** CanDeactivateGuard ✓
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
  - `confno` (string) - Confirmation number
  - `lname` (string) - Guest last name
  - `arrdate` (string) - Arrival date (format: YYYY-MM-DD)
- **Resolved Data Key:** `message`
- **Example URL:** `/modifyres/acme/property-001/ABC123/Smith/2024-05-15`
- **Navigation Guard:** Prevents leaving route with unsaved changes
- **Functionality:** Allows modification of room reservation details

---

## 6. Confirmation & Itinerary Routes

### Route: `/confirmation/:tenant/:property`
**Purpose:** Booking Confirmation Page
```typescript
{
  path: 'confirmation/:tenant/:property',
  resolve: { message: CartResolverService },
  // resolve: { message: OnecartResolveService },
  component: ConfirmationComponent,
}
```
- **Component:** ConfirmationComponent
- **Resolver:** CartResolverService ✓
- **Auth Guard:** None
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
- **Resolved Data Key:** `message`
- **Example URL:** `/confirmation/acme/property-001`
- **Functionality:** Displays booking confirmation details

---

### Route: `/itinerary/:tenant/:property`
**Purpose:** Create/View Itinerary
```typescript
{
  path: 'itinerary/:tenant/:property',
  // resolve: { message: OnecartResolveService },
  component: ItineraryComponent,
}
```
- **Component:** ItineraryComponent
- **Resolver:** None (commented out)
- **Auth Guard:** None
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
- **Example URL:** `/itinerary/acme/property-001`
- **Functionality:** Allows user to create or manage itinerary

---

### Route: `/view-itinerary/:tenant/:property`
**Purpose:** View Existing Itinerary
```typescript
{
  path: 'view-itinerary/:tenant/:property',
  // resolve: { message: OnecartResolveService },
  component: ViewItineraryComponent,
}
```
- **Component:** ViewItineraryComponent
- **Resolver:** None (commented out)
- **Auth Guard:** None
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
- **Example URL:** `/view-itinerary/acme/property-001`
- **Functionality:** Displays previously created itinerary

---

## 7. Maintenance Route

### Route: `/maintenance/:tenant/:property`
**Purpose:** Application Maintenance Page
```typescript
{
  path: 'maintenance/:tenant/:property',
  component: MaintenanceComponent,
}
```
- **Component:** MaintenanceComponent
- **Resolver:** None
- **Auth Guard:** None
- **URL Parameters:**
  - `tenant` (string) - Organization identifier
  - `property` (string) - Property identifier
- **Example URL:** `/maintenance/acme/property-001`
- **Functionality:** Displays maintenance message when app is down

---

## Summary Table: All 22 Routes

| # | Path | Type | Component/Module | Resolver | Guard(s) | Public |
|---|------|------|------------------|----------|---------|--------|
| 1 | `/wbe` | Module | RoomModule | RoomResolverService | None | Yes |
| 2 | `/spa` | Module | SpaModule | SpaResolverService | None | Yes |
| 3 | `/series` | Module | SpaModule | SpaResolverService | None | Yes |
| 4 | `/classes` | Module | SpaModule | SpaResolverService | None | Yes |
| 5 | `/golf` | Module | GolfModule | GolfResolverService | None | Yes |
| 6 | `/activities` | Module | ActivitiesModule | SeatResolveService | None | Yes |
| 7 | `/seat` | Module | SeatModule | SeatResolveService | None | Yes |
| 8 | `/unit` | Module | UnitModule | UnitresolverService | None | Yes |
| 9 | `/giftcard` | Module | GiftCardModule | GiftcardResolveService | None | Yes |
| 10 | `/snc` | Module | SncModule | None | sncAuthguardGuard | **No** |
| 11 | `/cart-v2` | Module | CartModule | None | None | Yes |
| 12 | `/headless-filter` | Module | HeadlessModule | None | None | Yes |
| 13 | `/login/:tenant/:property` | Component | LoginComponent | None | None | Yes |
| 14 | `/accessRestricted/:tenant/:property` | Component | AccessRestrictedComponent | None | None | Yes |
| 15 | `/profile-login/:tenant/:property` | Component | ProfileWrapperComponent | None | None | Yes |
| 16 | `/profile/:tenant/:property` | Module | ProfileModule | None | ProfileAuthGuardService | **No** |
| 17 | `/myres/:tenant/:property/:confno/:lname/:arrdate` | Component | ViewmyreservationComponent | CartResolverService | None | Yes |
| 18 | `/myres/:tenant/:property/:confno/:lname` | Component | ViewmyreservationComponent | CartResolverService | ReservationRoutingPermisssionResolverService | **No** |
| 19 | `/myres/:tenant/:property/:confno` | Component | ViewmyreservationComponent | CartResolverService | None | Yes |
| 20 | `/modifyres/:tenant/:property/:confno/:lname/:arrdate` | Component | ModifyRoomReservationsComponent | CartResolverService | CanDeactivateGuard | Yes |
| 21 | `/confirmation/:tenant/:property` | Component | ConfirmationComponent | CartResolverService | None | Yes |
| 22 | `/maintenance/:tenant/:property` | Component | MaintenanceComponent | None | None | Yes |

**Additional Routes (Not in summary above):**
- `/itinerary/:tenant/:property` → ItineraryComponent
- `/view-itinerary/:tenant/:property` → ViewItineraryComponent

---

## Route Parameter Glossary

| Parameter | Type | Purpose | Example |
|-----------|------|---------|---------|
| `tenant` | string | Organization/business unit identifier | `acme`, `marriott`, `hyatt` |
| `property` | string | Specific resort/location/property code | `property-001`, `las-vegas`, `beach-resort` |
| `confno` | string | Booking confirmation number | `ABC123`, `RES-2024-001` |
| `lname` | string | Guest's last name | `Smith`, `Johnson`, `Williams` |
| `arrdate` | string | Arrival date in YYYY-MM-DD format | `2024-05-15`, `2024-12-25` |

---

## URL Pattern Examples

### Booking Flow
```
1. Login: /login/acme/property-001
2. Browse: /wbe (lazy-loaded)
3. Select: Various modules (/spa, /golf, /seat, etc.)
4. Confirm: /confirmation/acme/property-001
5. View: /myres/acme/property-001/ABC123/Smith/2024-05-15
6. Modify: /modifyres/acme/property-001/ABC123/Smith/2024-05-15
```

### Alternative View Paths
```
Detailed:   /myres/acme/property-001/ABC123/Smith/2024-05-15
With Name:  /myres/acme/property-001/ABC123/Smith
Quick View: /myres/acme/property-001/ABC123
```

---

## Module Dependencies

### Layout Routing Module provides routes to:
- RoomModule (via `/wbe`)
- SpaModule (via `/spa`, `/series`, `/classes`)
- GolfModule (via `/golf`)
- ActivitiesModule (via `/activities`)
- SeatModule (via `/seat`)
- UnitModule (via `/unit`)
- GiftCardModule (via `/giftcard`)
- SncModule (via `/snc`)
- CartModule (via `/cart-v2`)
- HeadlessModule (via `/headless-filter`)
- ProfileModule (via `/profile/:tenant/:property`)

---

## Security & Guards Summary

| Guard | Routes | Enforcement |
|-------|--------|-------------|
| ProfileAuthGuardService | `/profile/:tenant/:property` | Authentication required |
| ReservationRoutingPermisssionResolverService | `/myres/:tenant/:property/:confno/:lname` | Permission validation |
| sncAuthguardGuard | `/snc` | SNC module authentication |
| CanDeactivateGuard | `/modifyres/:tenant/:property/:confno/:lname/:arrdate` | Unsaved changes prevention |