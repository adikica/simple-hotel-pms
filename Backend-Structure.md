# Hotel PMS Backend Structure

/hotel-pms-backend
│
├── .env                           # Environment variables
├── .env.example                   # Example environment variables
├── .gitignore                     # Git ignore file
├── package.json                   # Node.js dependencies
├── tsconfig.json                  # TypeScript configuration (if using TypeScript)
├── README.md                      # Project documentation
│
├── src/                           # Source code directory
│   │
│   ├── server.js                  # Application entry point
│   ├── app.js                     # Express app setup
│   │
│   ├── config/                    # Configuration files
│   │   ├── database.js            # Database configuration
│   │   ├── auth.js                # Authentication configuration
│   │   ├── cors.js                # CORS configuration
│   │   ├── logger.js              # Logger configuration
│   │   └── constants.js           # Application constants
│   │
│   ├── api/                       # API routes, controllers, services
│   │   │
│   │   ├── auth/                  # Authentication module
│   │   │   ├── auth.routes.js     # Authentication routes
│   │   │   ├── auth.controller.js # Authentication controllers
│   │   │   ├── auth.service.js    # Authentication business logic
│   │   │   ├── auth.validator.js  # Authentication input validation
│   │   │   └── auth.test.js       # Authentication tests
│   │   │
│   │   ├── users/                 # User management module
│   │   │   ├── users.routes.js    # User routes
│   │   │   ├── users.controller.js # User controllers
│   │   │   ├── users.service.js   # User business logic
│   │   │   ├── users.validator.js # User input validation
│   │   │   └── users.test.js      # User tests
│   │   │
│   │   ├── staff/                 # Staff management module
│   │   │   ├── staff.routes.js    # Staff routes
│   │   │   ├── staff.controller.js # Staff controllers
│   │   │   ├── staff.service.js   # Staff business logic
│   │   │   ├── staff.validator.js # Staff input validation
│   │   │   └── staff.test.js      # Staff tests
│   │   │
│   │   ├── guests/                # Guest management module
│   │   │   ├── guests.routes.js   # Guest routes
│   │   │   ├── guests.controller.js # Guest controllers
│   │   │   ├── guests.service.js  # Guest business logic
│   │   │   ├── guests.validator.js # Guest input validation
│   │   │   └── guests.test.js     # Guest tests
│   │   │
│   │   ├── vehicles/              # Vehicle management module
│   │   │   ├── vehicles.routes.js # Vehicle routes
│   │   │   ├── vehicles.controller.js # Vehicle controllers
│   │   │   ├── vehicles.service.js # Vehicle business logic
│   │   │   ├── vehicles.validator.js # Vehicle input validation
│   │   │   └── vehicles.test.js   # Vehicle tests
│   │   │
│   │   ├── properties/            # Property management module
│   │   │   ├── properties.routes.js # Property routes
│   │   │   ├── properties.controller.js # Property controllers
│   │   │   ├── properties.service.js # Property business logic
│   │   │   ├── properties.validator.js # Property input validation
│   │   │   └── properties.test.js # Property tests
│   │   │
│   │   ├── hotel/                 # Hotel settings module
│   │   │   ├── hotel.routes.js    # Hotel routes
│   │   │   ├── hotel.controller.js # Hotel controllers
│   │   │   ├── hotel.service.js   # Hotel business logic
│   │   │   ├── hotel.validator.js # Hotel input validation
│   │   │   └── hotel.test.js      # Hotel tests
│   │   │
│   │   ├── rooms/                 # Room management module
│   │   │   ├── rooms.routes.js    # Room routes
│   │   │   ├── rooms.controller.js # Room controllers
│   │   │   ├── rooms.service.js   # Room business logic
│   │   │   ├── rooms.validator.js # Room input validation
│   │   │   └── rooms.test.js      # Room tests
│   │   │
│   │   ├── roomTypes/             # Room type management module
│   │   │   ├── roomTypes.routes.js # Room type routes
│   │   │   ├── roomTypes.controller.js # Room type controllers
│   │   │   ├── roomTypes.service.js # Room type business logic
│   │   │   ├── roomTypes.validator.js # Room type input validation
│   │   │   └── roomTypes.test.js  # Room type tests
│   │   │
│   │   ├── reservations/          # Reservation management module
│   │   │   ├── reservations.routes.js # Reservation routes
│   │   │   ├── reservations.controller.js # Reservation controllers
│   │   │   ├── reservations.service.js # Reservation business logic
│   │   │   ├── reservations.validator.js # Reservation input validation
│   │   │   └── reservations.test.js # Reservation tests
│   │   │
│   │   ├── roomReservations/      # Room reservation module
│   │   │   ├── roomReservations.routes.js # Room reservation routes
│   │   │   ├── roomReservations.controller.js # Room reservation controllers
│   │   │   ├── roomReservations.service.js # Room reservation business logic
│   │   │   ├── roomReservations.validator.js # Room reservation input validation
│   │   │   └── roomReservations.test.js # Room reservation tests
│   │   │
│   │   ├── roomMoves/             # Room move module
│   │   │   ├── roomMoves.routes.js # Room move routes
│   │   │   ├── roomMoves.controller.js # Room move controllers
│   │   │   ├── roomMoves.service.js # Room move business logic
│   │   │   ├── roomMoves.validator.js # Room move input validation
│   │   │   └── roomMoves.test.js  # Room move tests
│   │   │
│   │   ├── activeBookings/        # Active bookings module
│   │   │   ├── activeBookings.routes.js # Active bookings routes
│   │   │   ├── activeBookings.controller.js # Active bookings controllers
│   │   │   ├── activeBookings.service.js # Active bookings business logic
│   │   │   └── activeBookings.test.js # Active bookings tests
│   │   │
│   │   ├── mealPlans/             # Meal plan module
│   │   │   ├── mealPlans.routes.js # Meal plan routes
│   │   │   ├── mealPlans.controller.js # Meal plan controllers
│   │   │   ├── mealPlans.service.js # Meal plan business logic
│   │   │   ├── mealPlans.validator.js # Meal plan input validation
│   │   │   └── mealPlans.test.js  # Meal plan tests
│   │   │
│   │   ├── mealServices/          # Meal service module
│   │   │   ├── mealServices.routes.js # Meal service routes
│   │   │   ├── mealServices.controller.js # Meal service controllers
│   │   │   ├── mealServices.service.js # Meal service business logic
│   │   │   ├── mealServices.validator.js # Meal service input validation
│   │   │   └── mealServices.test.js # Meal service tests
│   │   │
│   │   ├── payments/              # Payment module
│   │   │   ├── payments.routes.js # Payment routes
│   │   │   ├── payments.controller.js # Payment controllers
│   │   │   ├── payments.service.js # Payment business logic
│   │   │   ├── payments.validator.js # Payment input validation
│   │   │   └── payments.test.js   # Payment tests
│   │   │
│   │   ├── invoices/              # Invoice module
│   │   │   ├── invoices.routes.js # Invoice routes
│   │   │   ├── invoices.controller.js # Invoice controllers
│   │   │   ├── invoices.service.js # Invoice business logic
│   │   │   ├── invoices.validator.js # Invoice input validation
│   │   │   └── invoices.test.js   # Invoice tests
│   │   │
│   │   ├── taxes/                 # Tax module
│   │   │   ├── taxes.routes.js    # Tax routes
│   │   │   ├── taxes.controller.js # Tax controllers
│   │   │   ├── taxes.service.js   # Tax business logic
│   │   │   ├── taxes.validator.js # Tax input validation
│   │   │   └── taxes.test.js      # Tax tests
│   │   │
│   │   ├── fiscal/                # Fiscal integration module
│   │   │   ├── fiscal.routes.js   # Fiscal routes
│   │   │   ├── fiscal.controller.js # Fiscal controllers
│   │   │   ├── fiscal.service.js  # Fiscal business logic
│   │   │   └── fiscal.test.js     # Fiscal tests
│   │   │
│   │   ├── currencies/            # Currency module
│   │   │   ├── currencies.routes.js # Currency routes
│   │   │   ├── currencies.controller.js # Currency controllers
│   │   │   ├── currencies.service.js # Currency business logic
│   │   │   ├── currencies.validator.js # Currency input validation
│   │   │   └── currencies.test.js # Currency tests
│   │   │
│   │   ├── rates/                 # Rate calendar module
│   │   │   ├── rates.routes.js    # Rate calendar routes
│   │   │   ├── rates.controller.js # Rate calendar controllers
│   │   │   ├── rates.service.js   # Rate calendar business logic
│   │   │   ├── rates.validator.js # Rate calendar input validation
│   │   │   └── rates.test.js      # Rate calendar tests
│   │   │
│   │   ├── shifts/                # Shift management module
│   │   │   ├── shifts.routes.js   # Shift routes
│   │   │   ├── shifts.controller.js # Shift controllers
│   │   │   ├── shifts.service.js  # Shift business logic
│   │   │   ├── shifts.validator.js # Shift input validation
│   │   │   └── shifts.test.js     # Shift tests
│   │   │
│   │   ├── cashReconciliation/    # Cash reconciliation module
│   │   │   ├── cashReconciliation.routes.js # Cash reconciliation routes
│   │   │   ├── cashReconciliation.controller.js # Cash reconciliation controllers
│   │   │   ├── cashReconciliation.service.js # Cash reconciliation business logic
│   │   │   ├── cashReconciliation.validator.js # Cash reconciliation input validation
│   │   │   └── cashReconciliation.test.js # Cash reconciliation tests
│   │   │
│   │   ├── timeRecords/           # Time tracking module
│   │   │   ├── timeRecords.routes.js # Time records routes
│   │   │   ├── timeRecords.controller.js # Time records controllers
│   │   │   ├── timeRecords.service.js # Time records business logic
│   │   │   ├── timeRecords.validator.js # Time records input validation
│   │   │   └── timeRecords.test.js # Time records tests
│   │   │
│   │   ├── amenities/             # Amenities module
│   │   │   ├── amenities.routes.js # Amenities routes
│   │   │   ├── amenities.controller.js # Amenities controllers
│   │   │   ├── amenities.service.js # Amenities business logic
│   │   │   ├── amenities.validator.js # Amenities input validation
│   │   │   └── amenities.test.js  # Amenities tests
│   │   │
│   │   ├── roomItems/             # Room items module
│   │   │   ├── roomItems.routes.js # Room items routes
│   │   │   ├── roomItems.controller.js # Room items controllers
│   │   │   ├── roomItems.service.js # Room items business logic
│   │   │   ├── roomItems.validator.js # Room items input validation
│   │   │   └── roomItems.test.js  # Room items tests
│   │   │
│   │   ├── roomItemInstances/     # Room item instances module
│   │   │   ├── roomItemInstances.routes.js # Room item instances routes
│   │   │   ├── roomItemInstances.controller.js # Room item instances controllers
│   │   │   ├── roomItemInstances.service.js # Room item instances business logic
│   │   │   ├── roomItemInstances.validator.js # Room item instances input validation
│   │   │   └── roomItemInstances.test.js # Room item instances tests
│   │   │
│   │   ├── maintenance/           # Maintenance module
│   │   │   ├── maintenance.routes.js # Maintenance routes
│   │   │   ├── maintenance.controller.js # Maintenance controllers
│   │   │   ├── maintenance.service.js # Maintenance business logic
│   │   │   ├── maintenance.validator.js # Maintenance input validation
│   │   │   └── maintenance.test.js # Maintenance tests
│   │   │
│   │   ├── roomItemIssues/        # Room item issues module
│   │   │   ├── roomItemIssues.routes.js # Room item issues routes
│   │   │   ├── roomItemIssues.controller.js # Room item issues controllers
│   │   │   ├── roomItemIssues.service.js # Room item issues business logic
│   │   │   ├── roomItemIssues.validator.js # Room item issues input validation
│   │   │   └── roomItemIssues.test.js # Room item issues tests
│   │   │
│   │   ├── housekeeping/          # Housekeeping module
│   │   │   ├── housekeeping.routes.js # Housekeeping routes
│   │   │   ├── housekeeping.controller.js # Housekeeping controllers
│   │   │   ├── housekeeping.service.js # Housekeeping business logic
│   │   │   ├── housekeeping.validator.js # Housekeeping input validation
│   │   │   └── housekeeping.test.js # Housekeeping tests
│   │   │
│   │   ├── cleaning/              # Room cleaning module
│   │   │   ├── cleaning.routes.js # Cleaning routes
│   │   │   ├── cleaning.controller.js # Cleaning controllers
│   │   │   ├── cleaning.service.js # Cleaning business logic
│   │   │   ├── cleaning.validator.js # Cleaning input validation
│   │   │   └── cleaning.test.js   # Cleaning tests
│   │   │
│   │   ├── inventory/             # Inventory module
│   │   │   ├── categories/        # Inventory categories sub-module
│   │   │   │   ├── categories.routes.js
│   │   │   │   ├── categories.controller.js
│   │   │   │   ├── categories.service.js
│   │   │   │   ├── categories.validator.js
│   │   │   │   └── categories.test.js
│   │   │   │
│   │   │   ├── items/             # Inventory items sub-module
│   │   │   │   ├── items.routes.js
│   │   │   │   ├── items.controller.js
│   │   │   │   ├── items.service.js
│   │   │   │   ├── items.validator.js
│   │   │   │   └── items.test.js
│   │   │   │
│   │   │   ├── transactions/      # Inventory transactions sub-module
│   │   │   │   ├── transactions.routes.js
│   │   │   │   ├── transactions.controller.js
│   │   │   │   ├── transactions.service.js
│   │   │   │   ├── transactions.validator.js
│   │   │   │   └── transactions.test.js
│   │   │   │
│   │   │   └── inventory.routes.js # Main inventory routes
│   │   │
│   │   ├── suppliers/             # Suppliers module
│   │   │   ├── suppliers.routes.js # Suppliers routes
│   │   │   ├── suppliers.controller.js # Suppliers controllers
│   │   │   ├── suppliers.service.js # Suppliers business logic
│   │   │   ├── suppliers.validator.js # Suppliers input validation
│   │   │   └── suppliers.test.js  # Suppliers tests
│   │   │
│   │   ├── purchaseOrders/        # Purchase orders module
│   │   │   ├── purchaseOrders.routes.js # Purchase orders routes
│   │   │   ├── purchaseOrders.controller.js # Purchase orders controllers
│   │   │   ├── purchaseOrders.service.js # Purchase orders business logic
│   │   │   ├── purchaseOrders.validator.js # Purchase orders input validation
│   │   │   └── purchaseOrders.test.js # Purchase orders tests
│   │   │
│   │   └── reports/               # Reports module
│   │       ├── reports.routes.js  # Reports routes
│   │       ├── reports.controller.js # Reports controllers
│   │       ├── reports.service.js # Reports business logic
│   │       └── reports.test.js    # Reports tests
│   │
│   ├── middleware/                # Middleware functions
│   │   ├── auth.middleware.js     # Authentication middleware
│   │   ├── error.middleware.js    # Error handling middleware
│   │   ├── logger.middleware.js   # Request logging middleware
│   │   ├── validation.middleware.js # Input validation middleware
│   │   ├── roles.middleware.js    # Role-based authorization middleware
│   │   └── rateLimiter.middleware.js # Rate limiting middleware
│   │
│   ├── models/                    # Database models (if not using Prisma)
│   │   └── index.js               # Models export
│   │
│   ├── prisma/                    # Prisma ORM
│   │   ├── schema.prisma          # Prisma schema
│   │   └── migrations/            # Prisma migrations
│   │
│   ├── utils/                     # Utility functions
│   │   ├── logger.js              # Logging utility
│   │   ├── errors.js              # Error classes
│   │   ├── pagination.js          # Pagination utility
│   │   ├── date.js                # Date manipulation utility
│   │   ├── currency.js            # Currency conversion utility
│   │   ├── validation.js          # Validation helpers
│   │   ├── email.js               # Email sending utility
│   │   ├── pdf.js                 # PDF generation utility
│   │   └── security.js            # Security utilities
│   │
│   ├── services/                  # Shared services
│   │   ├── email.service.js       # Email service
│   │   ├── pdf.service.js         # PDF generation service
│   │   ├── storage.service.js     # File storage service
│   │   ├── notification.service.js # Notification service
│   │   └── external/              # External service integrations
│   │       ├── fiscalApi.service.js # Fiscal API integration
│   │       └── paymentGateway.service.js # Payment gateway integration
│   │
│   └── routes/                    # Route aggregation
│       ├── v1.routes.js           # v1 API routes aggregation
│       └── index.js               # Routes export
│
├── tests/                         # Integration and E2E tests
│   ├── integration/               # Integration tests
│   ├── e2e/                       # End-to-end tests
│   └── fixtures/                  # Test fixtures and data
│
├── docs/                          # Documentation
│   ├── api/                       # API documentation
│   ├── architecture/              # Architecture documentation
│   └── development/               # Development guides
│
├── scripts/                       # Scripts
│   ├── seed.js                    # Database seed script
│   ├── generateApiDocs.js         # API docs generation script
│   └── deploy.js                  # Deployment script
│
└── logs/                          # Log files
    ├── access.log                 # Access logs
    └── error.log                  # Error logs


Key Files Explained
Entry Points

server.js: Application entry point that starts the server
app.js: Express application setup with middleware configurations

Config Files

database.js: Database connection configuration
auth.js: Authentication parameters (JWT secret, expiry, etc.)
cors.js: CORS settings for API access
logger.js: Logging configuration
constants.js: Application-wide constants

API Structure (per module)
Each module follows a consistent structure:

routes.js: Defines API endpoints
controller.js: Handles HTTP requests/responses
service.js: Contains business logic
validator.js: Input validation schema and rules
test.js: Unit tests for the module

Middleware

auth.middleware.js: JWT authentication verification
error.middleware.js: Global error handling
logger.middleware.js: HTTP request logging
validation.middleware.js: Request validation based on schemas
roles.middleware.js: Role-based access control
rateLimiter.middleware.js: API rate limiting to prevent abuse

Utilities

logger.js: Centralized logging utility
errors.js: Custom error classes
pagination.js: Pagination handling for list endpoints
date.js: Date/time manipulation functions
currency.js: Currency conversion functions
validation.js: Common validation helpers
email.js: Email formatting and sending
pdf.js: PDF generation for invoices, etc.
security.js: Password hashing and security utilities

Services

email.service.js: Email sending service
pdf.service.js: PDF generation service
storage.service.js: File storage service (for photos, etc.)
notification.service.js: Push/SMS notification service
fiscalApi.service.js: Integration with tax/fiscal authorities
paymentGateway.service.js: Payment processor integration
