# Complete API Routes for Hotel PMS

## Authentication & User Management
```
# Authentication
POST   /v1/auth/login              - Log in staff or guest
POST   /v1/auth/logout             - Log out current user
POST   /v1/auth/refresh-token      - Refresh authentication token
POST   /v1/auth/forgot-password    - Initiate password reset process
POST   /v1/auth/reset-password     - Reset password with token

# Staff Management
GET    /v1/staff                   - List all staff members
POST   /v1/staff                   - Create new staff member
GET    /v1/staff/:id               - Get staff details
PUT    /v1/staff/:id               - Update staff member
DELETE /v1/staff/:id               - Deactivate staff member
GET    /v1/staff/role/:role        - Get staff by role

# Guests
GET    /v1/guests                  - List all guests
POST   /v1/guests                  - Create new guest
GET    /v1/guests/:id              - Get guest details
PUT    /v1/guests/:id              - Update guest information
DELETE /v1/guests/:id              - Deactivate guest account
GET    /v1/guests/:id/history      - Get guest stay history
GET    /v1/guests/:id/invoices     - Get guest invoices

# Guest Vehicles
GET    /v1/guests/:id/vehicles     - List guest vehicles
POST   /v1/guests/:id/vehicles     - Add vehicle to guest
PUT    /v1/vehicles/:id            - Update vehicle information
DELETE /v1/vehicles/:id            - Remove vehicle
GET    /v1/vehicles/search         - Search vehicles by license plate
```

## Property Management
```
# Hotel Settings
GET    /v1/hotel                   - Get hotel information
PUT    /v1/hotel                   - Update hotel information
PUT    /v1/hotel/tax-settings      - Update tax settings

# Properties
GET    /v1/properties              - List all properties
POST   /v1/properties              - Add new property
GET    /v1/properties/:id          - Get property details
PUT    /v1/properties/:id          - Update property
DELETE /v1/properties/:id          - Deactivate property

# Room Types
GET    /v1/room-types              - List all room types
POST   /v1/room-types              - Create new room type
GET    /v1/room-types/:id          - Get room type details
PUT    /v1/room-types/:id          - Update room type
DELETE /v1/room-types/:id          - Deactivate room type
GET    /v1/properties/:id/room-types - Get room types by property

# Rooms
GET    /v1/rooms                   - List all rooms
POST   /v1/rooms                   - Create new room
GET    /v1/rooms/:id               - Get room details
PUT    /v1/rooms/:id               - Update room
DELETE /v1/rooms/:id               - Deactivate room
GET    /v1/properties/:id/rooms    - Get rooms by property
PUT    /v1/rooms/:id/status        - Update room status
GET    /v1/rooms/available         - Search available rooms
```

## Reservation & Booking
```
# Reservations
GET    /v1/reservations            - List all reservations
POST   /v1/reservations            - Create new reservation
GET    /v1/reservations/:id        - Get reservation details
PUT    /v1/reservations/:id        - Update reservation
DELETE /v1/reservations/:id        - Cancel reservation
GET    /v1/reservations/search     - Search reservations
GET    /v1/reservations/date/:date - Get reservations by date

# Room Reservations
GET    /v1/reservations/:id/rooms  - Get rooms for reservation
POST   /v1/reservations/:id/rooms  - Add room to reservation
PUT    /v1/room-reservations/:id   - Update room reservation
DELETE /v1/room-reservations/:id   - Remove room from reservation

# Room Moves
POST   /v1/reservations/:id/move   - Move guest to different room
GET    /v1/room-moves/:id          - Get room move details
GET    /v1/reservations/:id/moves  - Get room move history

# Check-in/Check-out
POST   /v1/reservations/:id/check-in  - Check in guest
POST   /v1/reservations/:id/check-out - Check out guest

# Active Bookings (Optimized)
GET    /v1/active-bookings         - Get all active bookings
GET    /v1/active-bookings/date/:date - Get active bookings by date
```

## Meal Management
```
# Meal Plans
GET    /v1/meal-plans              - List meal plan options (enum values)
GET    /v1/meal-plan-prices        - Get meal plan prices
POST   /v1/meal-plan-prices        - Create meal plan price
PUT    /v1/meal-plan-prices/:id    - Update meal plan price
DELETE /v1/meal-plan-prices/:id    - Delete meal plan price

# Meal Services
GET    /v1/meal-services/date/:date - Get meal services for date
POST   /v1/meal-services           - Create/update meal service
GET    /v1/room-reservations/:id/meals - Get meals for reservation
PUT    /v1/meal-services/:id       - Update meal service
DELETE /v1/meal-services/:id       - Delete meal service

# Kitchen Reports
GET    /v1/meal-summaries/date/:date - Get meal summary for date
GET    /v1/meal-summaries/property/:id/date/:date - Get meal summary for property
```

## Financial Management
```
# Payments
POST   /v1/payments                - Record payment
GET    /v1/payments/:id            - Get payment details
GET    /v1/reservations/:id/payments - Get payments for reservation
GET    /v1/payments/search         - Search payments

# Invoices
GET    /v1/invoices                - List invoices
POST   /v1/invoices                - Create invoice
GET    /v1/invoices/:id            - Get invoice details
PUT    /v1/invoices/:id            - Update invoice
DELETE /v1/invoices/:id            - Cancel invoice
GET    /v1/reservations/:id/invoices - Get invoices for reservation
GET    /v1/guests/:id/invoices     - Get invoices for guest
POST   /v1/invoices/:id/payments   - Record payment for invoice
GET    /v1/invoices/:id/pdf        - Generate invoice PDF

# Taxes
GET    /v1/tax-settings            - Get tax settings
POST   /v1/tax-settings            - Create tax setting
PUT    /v1/tax-settings/:id        - Update tax setting
DELETE /v1/tax-settings/:id        - Delete tax setting

# Fiscal Integration
POST   /v1/invoices/:id/fiscal     - Generate fiscal invoice
GET    /v1/fiscal-logs             - Get fiscal system logs
```

## Rate Management
```
# Rate Calendar
GET    /v1/rate-calendar           - Get rate calendar entries
POST   /v1/rate-calendar           - Create rate calendar entry
PUT    /v1/rate-calendar/:id       - Update rate calendar entry
DELETE /v1/rate-calendar/:id       - Delete rate calendar entry
GET    /v1/rate-calendar/room-type/:id - Get rates for room type
POST   /v1/rate-calendar/bulk      - Bulk update rates
GET    /v1/rate-calendar/search    - Search rates by date range

# Currency Management
GET    /v1/currencies              - List all currencies
POST   /v1/currencies              - Add new currency
PUT    /v1/currencies/:code        - Update currency rate
GET    /v1/currencies/:code        - Get currency details
```

## Staff Operations
```
# Shifts
GET    /v1/shifts                  - List all shifts
POST   /v1/shifts                  - Start new shift
PUT    /v1/shifts/:id              - Update shift
POST   /v1/shifts/:id/end          - End shift
GET    /v1/shifts/current          - Get current active shifts
GET    /v1/staff/:id/shifts        - Get staff shifts

# Cash Reconciliation
POST   /v1/shifts/:id/reconcile    - Reconcile shift cash
GET    /v1/shifts/:id/reconciliation - Get reconciliation details

# Time Tracking
GET    /v1/time-records            - List time records
POST   /v1/time-records            - Create time record
PUT    /v1/time-records/:id        - Update time record
DELETE /v1/time-records/:id        - Delete time record
GET    /v1/staff/:id/time-records  - Get staff time records
GET    /v1/time-records/date/:date - Get time records by date

# Staff Hours
GET    /v1/staff/:id/hours         - Get staff hours summary
GET    /v1/staff/:id/hours/:year/:month - Get monthly hours
```

## Room Management
```
# Amenities
GET    /v1/amenities               - List all amenities
POST   /v1/amenities               - Create new amenity
GET    /v1/amenities/:id           - Get amenity details
PUT    /v1/amenities/:id           - Update amenity
DELETE /v1/amenities/:id           - Delete amenity

# Room Type Amenities
GET    /v1/room-types/:id/amenities - Get amenities for room type
POST   /v1/room-types/:id/amenities - Add amenity to room type
DELETE /v1/room-types/:id/amenities/:amenityId - Remove amenity from room type

# Room Amenities
GET    /v1/rooms/:id/amenities     - Get amenities for room
POST   /v1/rooms/:id/amenities     - Add amenity to room
PUT    /v1/room-amenities/:id      - Update room amenity status
DELETE /v1/room-amenities/:id      - Remove amenity from room

# Room Items
GET    /v1/room-items              - List all room items
POST   /v1/room-items              - Create new room item
GET    /v1/room-items/:id          - Get room item details
PUT    /v1/room-items/:id          - Update room item
DELETE /v1/room-items/:id          - Deactivate room item
GET    /v1/room-item-categories    - List item categories
POST   /v1/room-item-categories    - Create item category

# Room Item Instances
GET    /v1/rooms/:id/items         - Get items in room
POST   /v1/rooms/:id/items         - Add item to room
PUT    /v1/room-item-instances/:id - Update room item instance
DELETE /v1/room-item-instances/:id - Remove item from room
PUT    /v1/room-item-instances/:id/status - Update item status
```

## Maintenance & Housekeeping
```
# Maintenance Issues
GET    /v1/maintenance-issues      - List maintenance issues
POST   /v1/maintenance-issues      - Report maintenance issue
GET    /v1/maintenance-issues/:id  - Get issue details
PUT    /v1/maintenance-issues/:id  - Update maintenance issue
PUT    /v1/maintenance-issues/:id/status - Update issue status
GET    /v1/rooms/:id/maintenance   - Get room maintenance issues
GET    /v1/maintenance-issues/assigned/:staffId - Get assigned issues

# Room Item Issues
GET    /v1/item-issues             - List item issues
POST   /v1/item-issues             - Report item issue
GET    /v1/item-issues/:id         - Get item issue details
PUT    /v1/item-issues/:id         - Update item issue
PUT    /v1/item-issues/:id/status  - Update issue status
GET    /v1/rooms/:id/item-issues   - Get room item issues
GET    /v1/item-issues/assigned/:staffId - Get assigned item issues

# Housekeeping
GET    /v1/housekeeping/daily      - Get daily housekeeping log
POST   /v1/housekeeping/logs       - Create housekeeping log
PUT    /v1/housekeeping/logs/:id   - Update housekeeping log
GET    /v1/rooms/:id/housekeeping  - Get room housekeeping logs

# Room Cleaning
GET    /v1/cleaning/schedule       - Get cleaning schedule
POST   /v1/cleaning/schedule       - Create cleaning schedule
PUT    /v1/cleaning/:id            - Update cleaning record
PUT    /v1/cleaning/:id/status     - Update cleaning status
GET    /v1/cleaning/property/:id/date/:date - Get property cleaning schedule
GET    /v1/cleaning/assigned/:staffId - Get assigned cleaning tasks
```

## Inventory Management
```
# Inventory Categories
GET    /v1/inventory/categories    - List inventory categories
POST   /v1/inventory/categories    - Create category
GET    /v1/inventory/categories/:id - Get category details
PUT    /v1/inventory/categories/:id - Update category
DELETE /v1/inventory/categories/:id - Delete category

# Inventory Items
GET    /v1/inventory/items         - List inventory items
POST   /v1/inventory/items         - Create inventory item
GET    /v1/inventory/items/:id     - Get item details
PUT    /v1/inventory/items/:id     - Update inventory item
DELETE /v1/inventory/items/:id     - Deactivate inventory item
GET    /v1/inventory/items/low-stock - Get low stock items

# Suppliers
GET    /v1/suppliers               - List all suppliers
POST   /v1/suppliers               - Create supplier
GET    /v1/suppliers/:id           - Get supplier details
PUT    /v1/suppliers/:id           - Update supplier
DELETE /v1/suppliers/:id           - Deactivate supplier
GET    /v1/inventory/items/:id/suppliers - Get item suppliers
POST   /v1/inventory/items/:id/suppliers - Add supplier for item

# Purchase Orders
GET    /v1/purchase-orders         - List purchase orders
POST   /v1/purchase-orders         - Create purchase order
GET    /v1/purchase-orders/:id     - Get purchase order details
PUT    /v1/purchase-orders/:id     - Update purchase order
PUT    /v1/purchase-orders/:id/status - Update order status
DELETE /v1/purchase-orders/:id     - Cancel purchase order
POST   /v1/purchase-orders/:id/items - Add item to purchase order
PUT    /v1/purchase-order-items/:id - Update purchase order item
DELETE /v1/purchase-order-items/:id - Remove item from purchase order
POST   /v1/purchase-orders/:id/receive - Receive purchase order

# Inventory Transactions
GET    /v1/inventory/transactions  - List inventory transactions
POST   /v1/inventory/transactions  - Create inventory transaction
GET    /v1/inventory/transactions/:id - Get transaction details
GET    /v1/inventory/items/:id/transactions - Get item transactions
POST   /v1/inventory/adjustments   - Create inventory adjustment
```

## Reports & Statistics
```
# Reports
GET    /v1/reports/occupancy       - Get occupancy report
GET    /v1/reports/revenue         - Get revenue report
GET    /v1/reports/staff-performance - Get staff performance report
GET    /v1/reports/inventory-usage - Get inventory usage report
GET    /v1/reports/maintenance     - Get maintenance report
GET    /v1/reports/housekeeping    - Get housekeeping report
GET    /v1/reports/financial       - Get financial report

# Dashboard Statistics
GET    /v1/dashboard/today         - Get today's statistics
GET    /v1/dashboard/occupancy     - Get occupancy statistics
GET    /v1/dashboard/revenue       - Get revenue statistics
GET    /v1/dashboard/maintenance   - Get maintenance statistics
```
