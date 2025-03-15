/hotel-pms-frontend
│
├── .env                           # Environment variables
├── .env.local                     # Local environment variables (not committed)
├── .gitignore                     # Git ignore file
├── package.json                   # Node.js dependencies
├── tsconfig.json                  # TypeScript configuration
├── next.config.js                 # Next.js configuration
├── postcss.config.js              # PostCSS configuration
├── tailwind.config.js             # Tailwind CSS configuration
├── README.md                      # Project documentation
│
├── public/                        # Static files
│   ├── favicon.ico                # Favicon
│   ├── logo.svg                   # Hotel logo
│   ├── images/                    # Static images
│   │   ├── amenities/             # Amenity icons
│   │   ├── rooms/                 # Room images
│   │   └── backgrounds/           # Background images
│   └── locales/                   # Translation files
│
├── src/                           # Source code
│   │
│   ├── app/                       # App Router directory
│   │   │
│   │   ├── layout.tsx             # Root layout
│   │   ├── page.tsx               # Home page (landing page)
│   │   ├── error.tsx              # Error handling component
│   │   ├── loading.tsx            # Loading component
│   │   ├── not-found.tsx          # 404 component
│   │   │
│   │   ├── (website)/             # Public website
│   │   │   │
│   │   │   ├── layout.tsx         # Website layout
│   │   │   ├── about/             # About page
│   │   │   │   └── page.tsx
│   │   │   ├── contact/           # Contact page
│   │   │   │   └── page.tsx
│   │   │   ├── rooms/             # Room browsing
│   │   │   │   ├── page.tsx       # Room listing page
│   │   │   │   └── [slug]/        # Room detail
│   │   │   │       └── page.tsx
│   │   │   ├── book/              # Booking flow
│   │   │   │   ├── page.tsx       # Booking page
│   │   │   │   ├── search/        # Search availability
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── room-selection/ # Room selection
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── guest-details/ # Guest details
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── payment/       # Payment
│   │   │   │   │   └── page.tsx
│   │   │   │   └── confirmation/  # Booking confirmation
│   │   │   │       └── page.tsx
│   │   │   └── faqs/              # FAQs page
│   │   │       └── page.tsx
│   │   │
│   │   ├── auth/                  # Authentication pages
│   │   │   ├── login/             # Login page
│   │   │   │   └── page.tsx
│   │   │   ├── register/          # Registration page
│   │   │   │   └── page.tsx
│   │   │   ├── forgot-password/   # Forgot password
│   │   │   │   └── page.tsx
│   │   │   └── reset-password/    # Reset password
│   │   │       ├── page.tsx
│   │   │       └── [token]/
│   │   │           └── page.tsx
│   │   │
│   │   ├── guest-portal/          # Guest portal (authenticated)
│   │   │   │
│   │   │   ├── layout.tsx         # Guest layout with auth protection
│   │   │   ├── dashboard/         # Guest dashboard
│   │   │   │   └── page.tsx
│   │   │   ├── reservations/      # Guest reservations
│   │   │   │   ├── page.tsx       # List all reservations
│   │   │   │   └── [id]/          # Reservation details
│   │   │   │       └── page.tsx
│   │   │   ├── profile/           # Guest profile
│   │   │   │   ├── page.tsx       # Profile view
│   │   │   │   └── edit/          # Edit profile
│   │   │   │       └── page.tsx
│   │   │   ├── invoices/          # Guest invoices
│   │   │   │   ├── page.tsx       # List all invoices
│   │   │   │   └── [id]/          # Invoice details
│   │   │   │       └── page.tsx
│   │   │   └── vehicles/          # Guest vehicles
│   │   │       ├── page.tsx       # List vehicles
│   │   │       ├── new/           # Add vehicle
│   │   │       │   └── page.tsx
│   │   │       └── [id]/          # Edit vehicle
│   │   │           └── page.tsx
│   │   │
│   │   ├── reception/             # Front Desk Portal (authenticated)
│   │   │   │
│   │   │   ├── layout.tsx         # Reception layout with auth protection
│   │   │   ├── dashboard/         # Reception dashboard
│   │   │   │   └── page.tsx
│   │   │   │
│   │   │   ├── reservations/      # Reservation management
│   │   │   │   ├── page.tsx       # List all reservations
│   │   │   │   ├── new/           # Create reservation
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Reservation details
│   │   │   │       ├── page.tsx   # View reservation
│   │   │   │       ├── edit/      # Edit reservation
│   │   │   │       │   └── page.tsx
│   │   │   │       └── move/      # Move guest to different room
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── check-in/          # Check-in management
│   │   │   │   ├── page.tsx       # Today's check-ins
│   │   │   │   └── [id]/          # Check-in process
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── check-out/         # Check-out management
│   │   │   │   ├── page.tsx       # Today's check-outs
│   │   │   │   └── [id]/          # Check-out process
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── guests/            # Guest management
│   │   │   │   ├── page.tsx       # List all guests
│   │   │   │   ├── new/           # Create guest
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Guest details
│   │   │   │       ├── page.tsx   # View guest
│   │   │   │       ├── edit/      # Edit guest
│   │   │   │       │   └── page.tsx
│   │   │   │       └── vehicles/  # Guest vehicles
│   │   │   │           ├── page.tsx
│   │   │   │           └── new/
│   │   │   │               └── page.tsx
│   │   │   │
│   │   │   ├── rooms/             # Room status
│   │   │   │   ├── page.tsx       # Room status dashboard
│   │   │   │   ├── availability/  # Room availability
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Room details
│   │   │   │       ├── page.tsx   # View room
│   │   │   │       └── issues/    # Report issues
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── payments/          # Payment processing
│   │   │   │   ├── page.tsx       # Payment list
│   │   │   │   ├── new/           # New payment
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Payment details
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── invoices/          # Invoice management
│   │   │   │   ├── page.tsx       # List all invoices
│   │   │   │   ├── new/           # Create invoice
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Invoice details
│   │   │   │       ├── page.tsx   # View invoice
│   │   │   │       ├── edit/      # Edit invoice
│   │   │   │       │   └── page.tsx
│   │   │   │       └── print/     # Print invoice
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── housekeeping/      # Housekeeping management
│   │   │   │   ├── page.tsx       # Today's tasks
│   │   │   │   └── [date]/        # Tasks by date
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── maintenance/       # Maintenance management
│   │   │   │   ├── page.tsx       # Maintenance issues list
│   │   │   │   ├── new/           # Report new issue
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Issue details
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── reports/           # Reports for reception
│   │   │   │   ├── page.tsx       # Report dashboard
│   │   │   │   ├── daily/         # Daily reports
│   │   │   │   │   └── page.tsx
│   │   │   │   └── occupancy/     # Occupancy reports
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   └── shift/             # Shift management
│   │   │       ├── page.tsx       # Current shift
│   │   │       ├── start/         # Start shift
│   │   │       │   └── page.tsx
│   │   │       └── end/           # End shift
│   │   │           └── page.tsx
│   │   │
│   │   ├── admin/                 # Admin portal (authenticated)
│   │   │   │
│   │   │   ├── layout.tsx         # Admin layout with auth protection
│   │   │   ├── dashboard/         # Admin dashboard
│   │   │   │   └── page.tsx
│   │   │   │
│   │   │   ├── properties/        # Property management
│   │   │   │   ├── page.tsx       # List all properties
│   │   │   │   ├── new/           # Create property
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Property details
│   │   │   │       ├── page.tsx   # View property
│   │   │   │       └── edit/      # Edit property
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── room-types/        # Room type management
│   │   │   │   ├── page.tsx       # List all room types
│   │   │   │   ├── new/           # Create room type
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Room type details
│   │   │   │       ├── page.tsx   # View room type
│   │   │   │       ├── edit/      # Edit room type
│   │   │   │       │   └── page.tsx
│   │   │   │       └── amenities/ # Manage amenities
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── rooms/             # Room management
│   │   │   │   ├── page.tsx       # List all rooms
│   │   │   │   ├── new/           # Create room
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Room details
│   │   │   │       ├── page.tsx   # View room
│   │   │   │       ├── edit/      # Edit room
│   │   │   │       │   └── page.tsx
│   │   │   │       └── items/     # Manage room items
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── users/             # User management
│   │   │   │   ├── page.tsx       # List all users
│   │   │   │   ├── staff/         # Staff management
│   │   │   │   │   ├── page.tsx   # List all staff
│   │   │   │   │   ├── new/       # Create staff
│   │   │   │   │   │   └── page.tsx
│   │   │   │   │   └── [id]/      # Staff details
│   │   │   │   │       ├── page.tsx # View staff
│   │   │   │   │       └── edit/  # Edit staff
│   │   │   │   │           └── page.tsx
│   │   │   │   └── guests/        # Guest management
│   │   │   │       ├── page.tsx   # List all guests
│   │   │   │       └── [id]/      # Guest details
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── rates/             # Rate management
│   │   │   │   ├── page.tsx       # Rate calendar
│   │   │   │   ├── bulk-edit/     # Bulk edit rates
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [roomTypeId]/  # Room type rates
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── meal-plans/        # Meal plan management
│   │   │   │   ├── page.tsx       # Meal plan list
│   │   │   │   └── prices/        # Meal plan prices
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── amenities/         # Amenity management
│   │   │   │   ├── page.tsx       # List all amenities
│   │   │   │   ├── new/           # Create amenity
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Amenity details
│   │   │   │       ├── page.tsx   # View amenity
│   │   │   │       └── edit/      # Edit amenity
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── room-items/        # Room item management
│   │   │   │   ├── page.tsx       # List all room items
│   │   │   │   ├── categories/    # Item categories
│   │   │   │   │   ├── page.tsx   # List categories
│   │   │   │   │   ├── new/       # Create category
│   │   │   │   │   │   └── page.tsx
│   │   │   │   │   └── [id]/      # Category details
│   │   │   │   │       └── page.tsx
│   │   │   │   ├── new/           # Create room item
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Item details
│   │   │   │       ├── page.tsx   # View item
│   │   │   │       └── edit/      # Edit item
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── taxes/             # Tax settings
│   │   │   │   ├── page.tsx       # Tax settings list
│   │   │   │   ├── new/           # Create tax setting
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Tax setting details
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── currencies/        # Currency settings
│   │   │   │   ├── page.tsx       # Currency list
│   │   │   │   └── [code]/        # Currency details
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── inventory/         # Inventory management
│   │   │   │   ├── page.tsx       # Inventory dashboard
│   │   │   │   │
│   │   │   │   ├── items/         # Inventory items
│   │   │   │   │   ├── page.tsx   # List items
│   │   │   │   │   ├── new/       # Create item
│   │   │   │   │   │   └── page.tsx
│   │   │   │   │   └── [id]/      # Item details
│   │   │   │   │       ├── page.tsx # View item
│   │   │   │   │       └── edit/  # Edit item
│   │   │   │   │           └── page.tsx
│   │   │   │   │
│   │   │   │   ├── categories/    # Inventory categories
│   │   │   │   │   ├── page.tsx   # List categories
│   │   │   │   │   ├── new/       # Create category
│   │   │   │   │   │   └── page.tsx
│   │   │   │   │   └── [id]/      # Category details
│   │   │   │   │       └── page.tsx
│   │   │   │   │
│   │   │   │   ├── suppliers/     # Suppliers
│   │   │   │   │   ├── page.tsx   # List suppliers
│   │   │   │   │   ├── new/       # Create supplier
│   │   │   │   │   │   └── page.tsx
│   │   │   │   │   └── [id]/      # Supplier details
│   │   │   │   │       ├── page.tsx # View supplier
│   │   │   │   │       └── edit/  # Edit supplier
│   │   │   │   │           └── page.tsx
│   │   │   │   │
│   │   │   │   ├── transactions/  # Inventory transactions
│   │   │   │   │   ├── page.tsx   # List transactions
│   │   │   │   │   └── new/       # Create transaction
│   │   │   │   │       └── page.tsx
│   │   │   │   │
│   │   │   │   └── purchase-orders/ # Purchase orders
│   │   │   │       ├── page.tsx   # List purchase orders
│   │   │   │       ├── new/       # Create purchase order
│   │   │   │       │   └── page.tsx
│   │   │   │       └── [id]/      # Purchase order details
│   │   │   │           ├── page.tsx # View order
│   │   │   │           ├── edit/  # Edit order
│   │   │   │           │   └── page.tsx
│   │   │   │           └── receive/ # Receive order
│   │   │   │               └── page.tsx
│   │   │   │
│   │   │   ├── reports/           # Reports
│   │   │   │   ├── page.tsx       # Report dashboard
│   │   │   │   ├── occupancy/     # Occupancy reports
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── revenue/       # Revenue reports
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── financial/     # Financial reports
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── staff/         # Staff reports
│   │   │   │   │   └── page.tsx
│   │   │   │   └── inventory/     # Inventory reports
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   └── settings/          # System settings
│   │   │       ├── page.tsx       # General settings
│   │   │       ├── hotel/         # Hotel information
│   │   │       │   └── page.tsx
│   │   │       ├── fiscal/        # Fiscal settings
│   │   │       │   └── page.tsx
│   │   │       └── backup/        # Backup & restore
│   │   │           └── page.tsx
│   │   │
│   │   ├── housekeeping/          # Housekeeping portal
│   │   │   │
│   │   │   ├── layout.tsx         # Housekeeping layout
│   │   │   ├── dashboard/         # Housekeeping dashboard
│   │   │   │   └── page.tsx
│   │   │   │
│   │   │   ├── tasks/             # Cleaning tasks
│   │   │   │   ├── page.tsx       # Today's tasks
│   │   │   │   ├── completed/     # Completed tasks
│   │   │   │   │   └── page.tsx
│   │   │   │   └── [id]/          # Task details
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── rooms/             # Room status
│   │   │   │   ├── page.tsx       # Room list
│   │   │   │   └── [id]/          # Room details
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   └── reports/           # Report issues
│   │   │       ├── page.tsx       # Report list
│   │   │       └── new/           # New report
│   │   │           └── page.tsx
│   │   │
│   │   └── maintenance/           # Maintenance portal
│   │       │
│   │       ├── layout.tsx         # Maintenance layout
│   │       ├── dashboard/         # Maintenance dashboard
│   │       │   └── page.tsx
│   │       │
│   │       ├── tasks/             # Maintenance tasks
│   │       │   ├── page.tsx       # Task list
│   │       │   └── [id]/          # Task details
│   │       │       ├── page.tsx   # View task
│   │       │       └── update/    # Update task status
│   │       │           └── page.tsx
│   │       │
│   │       ├── room-items/        # Room item issues
│   │       │   ├── page.tsx       # Item issues list
│   │       │   └── [id]/          # Item issue details
│   │       │       └── page.tsx
│   │       │
│   │       └── reports/           # Maintenance reports
│   │           └── page.tsx
│   │
│   ├── components/                # Reusable components
│   │   │
│   │   ├── ui/                    # UI components
│   │   │   ├── button.tsx         # Button component
│   │   │   ├── card.tsx           # Card component
│   │   │   ├── dialog.tsx         # Dialog/modal component
│   │   │   ├── dropdown.tsx       # Dropdown component
│   │   │   ├── input.tsx          # Input component
│   │   │   ├── select.tsx         # Select component
│   │   │   ├── table.tsx          # Table component
│   │   │   ├── tabs.tsx           # Tabs component
│   │   │   ├── calendar.tsx       # Calendar component
│   │   │   ├── toast.tsx          # Toast notification component
│   │   │   ├── pagination.tsx     # Pagination component
│   │   │   ├── data-table.tsx     # Data table with sorting/filtering
│   │   │   └── skeleton.tsx       # Loading skeleton
│   │   │
│   │   ├── layout/                # Layout components
│   │   │   ├── navbar.tsx         # Navigation bar
│   │   │   ├── sidebar.tsx        # Sidebar navigation
│   │   │   ├── footer.tsx         # Footer
│   │   │   ├── header.tsx         # Header
│   │   │   ├── breadcrumbs.tsx    # Breadcrumb navigation
│   │   │   └── page-header.tsx    # Page header with actions
│   │   │
│   │   ├── forms/                 # Form components
│   │   │   ├── form.tsx           # Form wrapper
│   │   │   ├── form-field.tsx     # Form field
│   │   │   ├── reservation-form.tsx # Reservation form
│   │   │   ├── guest-form.tsx     # Guest form
│   │   │   ├── room-form.tsx      # Room form
│   │   │   ├── payment-form.tsx   # Payment form
│   │   │   ├── invoice-form.tsx   # Invoice form
│   │   │   ├── search-form.tsx    # Search form
│   │   │   └── vehicle-form.tsx   # Vehicle form
│   │   │
│   │   ├── crud/                  # CRUD operation components
│   │   │   ├── data-list.tsx      # Generic data listing component
│   │   │   ├── data-form.tsx      # Generic data form component
│   │   │   ├── data-detail.tsx    # Generic data detail component
│   │   │   ├── confirm-delete.tsx # Confirm delete dialog
│   │   │   └── action-buttons.tsx # CRUD action buttons
│   │   │
│   │   ├── dashboard/             # Dashboard components
│   │   │   ├── stats-card.tsx     # Statistics card
│   │   │   ├── recent-bookings.tsx # Recent bookings widget
│   │   │   ├── occupancy-chart.tsx # Occupancy chart
│   │   │   ├── revenue-chart.tsx  # Revenue chart
│   │   │   └── todo-list.tsx      # To-do list
│   │   │
│   │   ├── reservation/           # Reservation components
│   │   │   ├── reservation-card.tsx # Reservation card
│   │   │   ├── reservation-list.tsx # Reservation list
│   │   │   ├── reservation-detail.tsx # Reservation details
│   │   │   ├── create-reservation.tsx # Create reservation form
│   │   │   ├── edit-reservation.tsx # Edit reservation form
│   │   │   ├── room-selector.tsx  # Room selection component
│   │   │   ├── meal-plan-selector.tsx # Meal plan selection
│   │   │   ├── date-range-picker.tsx # Date range picker
│   │   │   ├── guest-selector.tsx # Guest selection component
│   │   │   ├── price-breakdown.tsx # Price breakdown
│   │   │   ├── check-in-form.tsx  # Check-in form
│   │   │   ├── check-out-form.tsx # Check-out form
│   │   │   └── room-move-form.tsx # Room move form
│   │   │
│   │   ├── guest/                 # Guest components
│   │   │   ├── guest-card.tsx     # Guest card
│   │   │   ├── guest-list.tsx     # Guest list
│   │   │   ├── guest-detail.tsx   # Guest details
│   │   │   ├── create-guest.tsx   # Create guest form
│   │   │   ├── edit-guest.tsx     # Edit guest form
│   │   │   ├── vehicle-form.tsx   # Vehicle form
│   │   │   └── vehicle-list.tsx   # Vehicle list
│   │   │
│   │   ├── room/                  # Room components
│   │   │   ├── room-card.tsx      # Room card
│   │   │   ├── room-grid.tsx      # Room grid view
│   │   │   ├── room-list.tsx      # Room list view
│   │   │   ├── room-detail.tsx    # Room details
│   │   │   ├── create-room.tsx    # Create room form
│   │   │   ├── edit-room.tsx      # Edit room form
│   │   │   ├── room-status.tsx    # Room status indicator
│   │   │   ├── room-type-card.tsx # Room type card
│   │   │   ├── room-type-list.tsx # Room type list
│   │   │   ├── create-room-type.tsx # Create room type form
│   │   │   └── edit-room-type.tsx # Edit room type form
│   │   │
│   │   ├── housekeeping/          # Housekeeping components
│   │   │   ├── cleaning-list.tsx  # Cleaning list
│   │   │   ├── cleaning-card.tsx  # Cleaning task card
│   │   │   ├── cleaning-detail.tsx # Cleaning task details
│   │   │   └── room-status-board.tsx # Room status board
│   │   │
│   │   ├── maintenance/           # Maintenance components
│   │   │   ├── issue-card.tsx     # Issue card
│   │   │   ├── issue-list.tsx     # Issue list
│   │   │   ├── issue-detail.tsx   # Issue details
│   │   │   ├── create-issue.tsx   # Create issue form
│   │   │   ├── edit-issue.tsx     # Edit issue form
│   │   │   └── update-status.tsx  # Update status form
│   │   │
│   │   ├── payment/               # Payment components
│   │   │   ├── payment-card.tsx   # Payment card
│   │   │   ├── payment-list.tsx   # Payment list
│   │   │   ├── payment-detail.tsx # Payment details
│   │   │   ├── create-payment.tsx # Create payment form
│   │   │   ├── currency-selector.tsx # Currency selector
│   │   │   └── payment-method-selector.tsx # Payment method selector
│   │   │
│   │   ├── invoice/               # Invoice components
│   │   │   ├── invoice-card.tsx   # Invoice card
│   │   │   ├── invoice-list.tsx   # Invoice list
│   │   │   ├── invoice-detail.tsx # Invoice details
│   │   │   ├── create-invoice.tsx # Create invoice form
│   │   │   ├── edit-invoice.tsx   # Edit invoice form
│   │   │   ├── invoice-line-items.tsx # Invoice line items
│   │   │   ├── invoice-preview.tsx # Invoice preview for printing
│   │   │   └── fiscal-badge.tsx   # Fiscal invoice indicator
│   │   │
│   │   ├── inventory/             # Inventory components
│   │   │   ├── item-card.tsx      # Inventory item card
│   │   │   ├── item-list.tsx      # Inventory item list
│   │   │   ├── item-detail.tsx    # Item details
│   │   │   ├── create-item.tsx    # Create item form
│   │   │   ├── edit-item.tsx      # Edit item form
│   │   │   ├── stock-level.tsx    # Stock level indicator
│   │   │   ├── supplier-card.tsx  # Supplier card
│   │   │   ├── supplier-list.tsx  # Supplier list
│   │   │   ├── purchase-order-form.tsx # Purchase order form
│   │   │   └── receive-items.tsx  # Receive items form
│   │   │
│   │   ├── reports/               # Report components
│   │   │   ├── report-card.tsx    # Report card
│   │   │   ├── date-range-selector.tsx # Date range selector
│   │   │   ├── chart-container.tsx # Chart container
│   │   │   ├── occupancy-report.tsx # Occupancy report
│   │   │   ├── revenue-report.tsx # Revenue report
│   │   │   ├── staff-report.tsx   # Staff performance report
│   │   │   └── export-button.tsx  # Export button for reports
│   │   │
│   │   ├── settings/              # Settings components
│   │   │   ├── setting-card.tsx   # Setting card
│   │   │   ├── setting-form.tsx   # Setting form
│   │   │   ├── tax-settings.tsx   # Tax settings form
│   │   │   ├── currency-settings.tsx # Currency settings form
│   │   │   └── hotel-info.tsx     # Hotel information form
│   │   │
│   │   └── auth/                  # Auth components
│   │       ├── login-form.tsx     # Login form
│   │       ├── register-form.tsx  # Registration form
│   │       ├── password-reset-form.tsx # Password reset form
│   │       └── user-menu.tsx      # User dropdown menu
│   │
│   ├── hooks/                     # Custom hooks
│   │   │
│   │   ├── data/                  # Data fetching hooks
│   │   │   ├── use-reservations.ts # Reservations data hook
│   │   │   ├── use-rooms.ts       # Rooms data hook
│   │   │   ├── use-guests.ts      # Guests data hook
│   │   │   ├── use-payments.ts    # Payments data hook
│   │   │   ├── use-invoices.ts    # Invoices data hook
│   │   │   ├── use-inventory.ts   # Inventory data hook
│   │   │   ├── use-maintenance.ts # Maintenance data hook
│   │   │   └── use-reports.ts     # Reports data hook
│   │   │
│   │   ├── form/                  # Form handling hooks
│   │   │   ├── use-form.ts        # Form state management hook
│   │   │   ├── use-field.ts       # Form field hook
│   │   │   └── use-validation.ts  # Form validation hook
│   │   │
│   │   ├── auth/                  # Authentication hooks
│   │   │   ├── use-auth.ts        # Authentication state hook
│   │   │   ├── use-login.ts       # Login hook
│   │   │   ├── use-register.ts    # Registration hook
│   │   │   └── use-permissions.ts # Permissions check hook
│   │   │
│   │   ├── ui/                    # UI hooks
│   │   │   ├── use-toast.ts       # Toast notification hook
│   │   │   ├── use-dialog.ts      # Dialog/modal hook
│   │   │   ├── use-media-query.ts # Media query hook
│   │   │   └── use-theme.ts       # Theme hook
│   │   │
│   │   └── misc/                  # Miscellaneous hooks
│   │       ├── use-debounce.ts    # Debounce hook
│   │       ├── use-pagination.ts  # Pagination hook
│   │       ├── use-sort.ts        # Sorting hook
│   │       └── use-filter.ts      # Filtering hook
│   │
│   ├── lib/                       # Library code
│   │   │
│   │   ├── api/                   # API client
│   │   │   ├── client.ts          # Base API client
│   │   │   ├── endpoints.ts       # API endpoints
│   │   │   ├── types.ts           # API types
│   │   │   └── error-handler.ts   # API error handler
│   │   │
│   │   ├── auth/                  # Authentication utilities
│   │   │   ├── auth-provider.tsx  # Auth context provider
│   │   │   ├── auth-service.ts    # Auth service functions
│   │   │   └── auth-utils.ts      # Auth utility functions
│   │   │
│   │   ├── utils/                 # Utility functions
│   │   │   ├── date-utils.ts      # Date utilities
│   │   │   ├── currency-utils.ts  # Currency utilities
│   │   │   ├── string-utils.ts    # String utilities
│   │   │   ├── array-utils.ts     # Array utilities
│   │   │   ├── form-utils.ts      # Form utilities
│   │   │   ├── validation-utils.ts # Validation utilities
│   │   │   └── file-utils.ts      # File utilities
│   │   │
│   │   ├── pdf/                   # PDF generation
│   │   │   ├── pdf-generator.ts   # PDF generation utility
│   │   │   ├── invoice-template.ts # Invoice PDF template
│   │   │   └── report-template.ts # Report PDF template
│   │   │
│   │   └── config/                # Configuration
│   │       ├── routes.ts          # Route definitions
│   │       ├── permissions.ts     # Permission definitions
│   │       └── constants.ts       # Application constants
│   │
│   ├── types/                     # TypeScript type definitions
│   │   ├── api.types.ts           # API response types
│   │   ├── auth.types.ts          # Authentication types
│   │   ├── user.types.ts          # User and staff types
│   │   ├── guest.types.ts         # Guest types
│   │   ├── reservation.types.ts   # Reservation types
│   │   ├── room.types.ts          # Room and room type types
│   │   ├── payment.types.ts       # Payment types
│   │   ├── invoice.types.ts       # Invoice types
│   │   ├── housekeeping.types.ts  # Housekeeping types
│   │   ├── maintenance.types.ts   # Maintenance types
│   │   ├── inventory.types.ts     # Inventory types
│   │   ├── report.types.ts        # Report types
│   │   └── common.types.ts        # Common shared types
│   │
│   ├── contexts/                  # React contexts
│   │   ├── auth-context.tsx       # Authentication context
│   │   ├── toast-context.tsx      # Toast notification context
│   │   ├── dialog-context.tsx     # Dialog/modal context
│   │   ├── theme-context.tsx      # Theme context
│   │   ├── settings-context.tsx   # Application settings context
│   │   └── permission-context.tsx # User permissions context
│   │
│   └── styles/                    # Styles
│       ├── globals.css            # Global styles
│       ├── components.css         # Component-specific styles
│       └── themes/                # Theme files
│           ├── light.css          # Light theme
│           └── dark.css           # Dark theme
│
├── middleware.ts                  # Next.js middleware for auth protection
│
└── tests/                         # Tests
    ├── components/                # Component tests
    ├── hooks/                     # Hook tests
    ├── pages/                     # Page tests
    └── utils/                     # Utility tests
