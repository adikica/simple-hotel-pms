# Hotel Management System

## System Overview

The Hotel Management System is a comprehensive solution designed to manage all aspects of hotel operations, including front-desk operations, reservations, channel management, housekeeping, rate and occupancy management, and payment processing. The system follows a modern architecture with:

- **Backend**: Node.js with Express, using Prisma ORM with PostgreSQL database
- **Frontend**: Next.js 15.2.2 with React 19, featuring separate interfaces for:
  - Receptionist portal
  - Admin dashboard
  - Customer user account
  - Public website

## Database Schema

### Core Tables

#### User
```prisma
model User {
  id            String    @id @default(uuid())
  email         String    @unique
  password      String
  firstName     String
  lastName      String
  phoneNumber   String?
  role          Role      @default(CUSTOMER)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  
  // Relations
  customer      Customer?
  receptionist  Receptionist?
  admin         Admin?
  
  @@map("users")
}

### Room Availability Checking

When booking a room, the system must ensure no reservation conflicts exist:

```javascript
// Check if a room is available for booking during a specific period
async function checkRoomAvailability(roomId, checkInDate, checkOutDate) {
  // Format dates correctly for reservation system
  const formattedCheckIn = setTimeToNoon(new Date(checkInDate));
  const formattedCheckOut = setTimeToElevenAM(new Date(checkOutDate));
  
  // Validate input dates
  if (formattedCheckIn >= formattedCheckOut) {
    throw new Error('Check-out date must be after check-in date');
  }
  
  // Ensure check-in date is not in the past
  if (formattedCheckIn < setTimeToNoon(new Date())) {
    throw new Error('Check-in date cannot be in the past');
  }
  
  // Check for overlapping reservations
  const overlappingReservations = await prisma.reservation.findMany({
    where: {
      roomId: roomId,
      status: { in: ['CONFIRMED', 'CHECKED_IN'] },
      OR: [
        // Case 1: New reservation starts during an existing reservation
        {
          AND: [
            { checkInDate: { lte: formattedCheckIn } },
            { checkOutDate: { gt: formattedCheckIn } }
          ]
        },
        // Case 2: New reservation ends during an existing reservation
        {
          AND: [
            { checkInDate: { lt: formattedCheckOut } },
            { checkOutDate: { gte: formattedCheckOut } }
          ]
        },
        // Case 3: New reservation completely contains an existing reservation
        {
          AND: [
            { checkInDate: { gte: formattedCheckIn } },
            { checkOutDate: { lte: formattedCheckOut } }
          ]
        }
      ]
    }
  });
  
  return {
    isAvailable: overlappingReservations.length === 0,
    conflictingReservations: overlappingReservations
  };
}
```

### Dynamic Pricing

The system supports different pricing strategies:

```javascript
// Calculate total price based on room type, dates, and applicable rates
async function calculateTotalPrice(roomTypeId, checkInDate, checkOutDate) {
  const roomType = await prisma.roomType.findUnique({
    where: { id: roomTypeId },
    include: { rates: true }
  });
  
  if (!roomType) {
    throw new Error('Room type not found');
  }
  
  let totalPrice = 0;
  let currentDate = new Date(checkInDate);
  
  // Iterate through each night of the stay
  while (currentDate < checkOutDate) {
    const dayOfWeek = currentDate.getDay(); // 0 = Sunday, 6 = Saturday
    const currentDateOnly = new Date(currentDate);
    
    // Find applicable rate for this date
    const applicableRate = roomType.rates.find(rate => {
      // Check if rate applies to this day of week
      const dayApplies = rate.daysOfWeek.includes(dayOfWeek);
      
      // Check if rate is within seasonal date range (if applicable)
      const inSeasonRange = !rate.startDate || !rate.endDate || 
        (currentDate >= rate.startDate && currentDate <= rate.endDate);
      
      return dayApplies && inSeasonRange;
    });
    
    // Use applicable rate or default to base price
    const nightlyRate = applicableRate ? applicableRate.price : roomType.basePrice;
    totalPrice += Number(nightlyRate);
    
    // Move to next day
    currentDate.setDate(currentDate.getDate() + 1);
  }
  
  return totalPrice;
}
```

### Housekeeping Management

The system automatically schedules room cleaning:

```javascript
// Schedule housekeeping after guest checkout
async function scheduleHousekeeping(roomId) {
  // Update room status to cleaning
  await prisma.room.update({
    where: { id: roomId },
    data: { status: 'CLEANING' }
  });
  
  // Create housekeeping log entry
  return prisma.housekeepingLog.create({
    data: {
      roomId,
      status: 'PENDING',
      notes: 'Scheduled after checkout'
    }
  });
}

// Mark room as cleaned
async function completeHousekeeping(housekeepingLogId, staffId) {
  const log = await prisma.housekeepingLog.findUnique({
    where: { id: housekeepingLogId },
    include: { room: true }
  });
  
  if (!log) {
    throw new Error('Housekeeping log not found');
  }
  
  // Update housekeeping log
  await prisma.housekeepingLog.update({
    where: { id: housekeepingLogId },
    data: {
      status: 'COMPLETED',
      assignedTo: staffId,
      completedAt: new Date(),
      notes: `${log.notes ? log.notes + '; ' : ''}Completed by staff ${staffId}`
    }
  });
  
  // Update room status to available
  return prisma.room.update({
    where: { id: log.roomId },
    data: { status: 'AVAILABLE' }
  });
}
```

## Frontend Implementation Details

### Authentication and Authorization

The system implements role-based access control:

```javascript
// auth.js - Authentication middleware
function requireAuth(req, res, next) {
  const token = req.cookies.authToken || req.headers.authorization?.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ error: 'Authentication required' });
  }
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(401).json({ error: 'Invalid or expired token' });
  }
}

// Role-based authorization middleware
function requireRole(roles = []) {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Authentication required' });
    }
    
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    
    next();
  };
}
```

### React Components

Key React components for the receptionist interface:

```jsx
// ReservationForm.jsx - For creating/editing reservations
import React, { useState, useEffect } from 'react';
import { useForm } from 'react-hook-form';
import { DatePicker } from '../ui/date-picker';
import { Select } from '../ui/select';
import { Button } from '../ui/button';
import { Input } from '../ui/input';
import { fetchAvailableRooms, createReservation } from '@/lib/api';

const ReservationForm = ({ onSuccess }) => {
  const { register, handleSubmit, watch, setValue, formState: { errors } } = useForm();
  const [availableRooms, setAvailableRooms] = useState([]);
  const [loading, setLoading] = useState(false);
  
  // Watch check-in/check-out dates to update available rooms
  const checkInDate = watch('checkInDate');
  const checkOutDate = watch('checkOutDate');
  
  useEffect(() => {
    if (checkInDate && checkOutDate) {
      setLoading(true);
      fetchAvailableRooms(checkInDate, checkOutDate)
        .then(rooms => setAvailableRooms(rooms))
        .finally(() => setLoading(false));
    }
  }, [checkInDate, checkOutDate]);
  
  const onSubmit = async (data) => {
    setLoading(true);
    try {
      const reservation = await createReservation({
        ...data,
        checkInDate: setTimeToNoon(new Date(data.checkInDate)),
        checkOutDate: setTimeToElevenAM(new Date(data.checkOutDate))
      });
      
      onSuccess(reservation);
    } catch (error) {
      console.error('Error creating reservation:', error);
    } finally {
      setLoading(false);
    }
  };
  
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Customer details section */}
      <div className="space-y-4">
        <h3 className="text-lg font-medium">Guest Information</h3>
        <div className="grid grid-cols-2 gap-4">
          <div>
            <label htmlFor="firstName">First Name</label>
            <Input
              id="firstName"
              {...register('firstName', { required: 'First name is required' })}
              error={errors.firstName?.message}
            />
          </div>
          <div>
            <label htmlFor="lastName">Last Name</label>
            <Input
              id="lastName"
              {...register('lastName', { required: 'Last name is required' })}
              error={errors.lastName?.message}
            />
          </div>
        </div>
        <div>
          <label htmlFor="email">Email</label>
          <Input
            id="email"
            type="email"
            {...register('email', { required: 'Email is required' })}
            error={errors.email?.message}
          />
        </div>
        <div>
          <label htmlFor="phone">Phone</label>
          <Input
            id="phone"
            {...register('phone', { required: 'Phone is required' })}
            error={errors.phone?.message}
          />
        </div>
      </div>
      
      {/* Reservation details section */}
      <div className="space-y-4 mt-6">
        <h3 className="text-lg font-medium">Reservation Details</h3>
        <div className="grid grid-cols-2 gap-4">
          <div>
            <label htmlFor="checkInDate">Check-in Date</label>
            <DatePicker
              id="checkInDate"
              {...register('checkInDate', { required: 'Check-in date is required' })}
              onChange={date => setValue('checkInDate', date)}
              error={errors.checkInDate?.message}
              minDate={new Date()}
            />
            <p className="text-sm text-gray-500">Check-in time: 12:00 PM</p>
          </div>
          <div>
            <label htmlFor="checkOutDate">Check-out Date</label>
            <DatePicker
              id="checkOutDate"
              {...register('checkOutDate', { required: 'Check-out date is required' })}
              onChange={date => setValue('checkOutDate', date)}
              error={errors.checkOutDate?.message}
              minDate={checkInDate ? new Date(checkInDate) : new Date()}
            />
            <p className="text-sm text-gray-500">Check-out time: 11:00 AM</p>
          </div>
        </div>
        
        <div>
          <label htmlFor="numberOfGuests">Number of Guests</label>
          <Input
            id="numberOfGuests"
            type="number"
            min="1"
            {...register('numberOfGuests', { 
              required: 'Number of guests is required',
              min: { value: 1, message: 'At least 1 guest is required' }
            })}
            error={errors.numberOfGuests?.message}
          />
        </div>
        
        <div>
          <label htmlFor="roomId">Room</label>
          <Select
            id="roomId"
            {...register('roomId', { required: 'Room selection is required' })}
            error={errors.roomId?.message}
            disabled={loading || availableRooms.length === 0}
          >
            <option value="">Select a room</option>
            {availableRooms.map(room => (
              <option key={room.id} value={room.id}>
                {room.roomNumber} - {room.roomType.name} (${room.roomType.basePrice}/night)
              </option>
            ))}
          </Select>
          {availableRooms.length === 0 && checkInDate && checkOutDate && !loading && (
            <p className="text-sm text-red-500">No rooms available for selected dates</p>
          )}
        </div>
        
        <div>
          <label htmlFor="notes">Special Requests</label>
          <textarea
            id="notes"
            {...register('notes')}
            className="w-full p-2 border rounded"
            rows="3"
          ></textarea>
        </div>
      </div>
      
      <div className="mt-6">
        <Button type="submit" disabled={loading}>
          {loading ? 'Processing...' : 'Create Reservation'}
        </Button>
      </div>
    </form>
  );
};

export default ReservationForm;
```

```jsx
// PaymentForm.jsx - For processing payments
import React, { useState } from 'react';
import { useForm } from 'react-hook-form';
import { Button } from '../ui/button';
import { Input } from '../ui/input';
import { Select } from '../ui/select';
import { processPayment } from '@/lib/api';

const PaymentForm = ({ reservation, receptionistId, onSuccess }) => {
  const { register, handleSubmit, formState: { errors } } = useForm();
  const [loading, setLoading] = useState(false);
  
  // Calculate remaining balance
  const totalPaid = reservation.payments.reduce((sum, payment) => sum + payment.amount, 0);
  const remainingBalance = reservation.totalAmount - totalPaid;
  
  const onSubmit = async (data) => {
    setLoading(true);
    try {
      const payment = await processPayment({
        reservationId: reservation.id,
        receptionistId,
        amount: parseFloat(data.amount),
        paymentMethod: data.paymentMethod
      });
      
      onSuccess(payment);
    } catch (error) {
      console.error('Error processing payment:', error);
    } finally {
      setLoading(false);
    }
  };
  
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div className="space-y-4">
        <div>
          <div className="flex justify-between">
            <span>Total Amount:</span>
            <span>${reservation.totalAmount.toFixed(2)}</span>
          </div>
          <div className="flex justify-between">
            <span>Amount Paid:</span>
            <span>${totalPaid.toFixed(2)}</span>
          </div>
          <div className="flex justify-between font-bold">
            <span>Remaining Balance:</span>
            <span>${remainingBalance.toFixed(2)}</span>
          </div>
        </div>
        
        <div>
          <label htmlFor="amount">Payment Amount</label>
          <Input
            id="amount"
            type="number"
            step="0.01"
            min="0.01"
            max={remainingBalance}
            {...register('amount', {
              required: 'Payment amount is required',
              min: { value: 0.01, message: 'Amount must be greater than 0' },
              max: { value: remainingBalance, message: 'Amount cannot exceed remaining balance' }
            })}
            error={errors.amount?.message}
          />
        </div>
        
        <div>
          <label htmlFor="paymentMethod">Payment Method</label>
          <Select
            id="paymentMethod"
            {...register('paymentMethod', { required: 'Payment method is required' })}
            error={errors.paymentMethod?.message}
          >
            <option value="">Select payment method</option>
            <option value="CASH">Cash</option>
            <option value="CREDIT_CARD">Credit Card</option>
            <option value="DEBIT_CARD">Debit Card</option>
            <option value="BANK_TRANSFER">Bank Transfer</option>
            <option value="MOBILE_PAYMENT">Mobile Payment</option>
            <option value="OTHER">Other</option>
          </Select>
        </div>
        
        <div>
          <label htmlFor="notes">Notes</label>
          <textarea
            id="notes"
            {...register('notes')}
            className="w-full p-2 border rounded"
            rows="2"
          ></textarea>
        </div>
      </div>
      
      <div className="mt-6">
        <Button type="submit" disabled={loading || remainingBalance <= 0}>
          {loading ? 'Processing...' : 'Process Payment'}
        </Button>
      </div>
    </form>
  );
};

export default PaymentForm;
```

### Frontend State Management

Example of using React hooks for reservation management:

```jsx
// use-reservations.js - Custom hook for reservation data
import { useState, useEffect } from 'react';
import { 
  fetchReservations, 
  createReservation, 
  updateReservation,
  checkInReservation,
  checkOutReservation,
  cancelReservation 
} from '@/lib/api';

export function useReservations(filters = {}) {
  const [reservations, setReservations] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  // Load reservations based on filters
  const loadReservations = async () => {
    setLoading(true);
    setError(null);
    
    try {
      const data = await fetchReservations(filters);
      setReservations(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };
  
  // Load reservations on component mount or when filters change
  useEffect(() => {
    loadReservations();
  }, [JSON.stringify(filters)]);
  
  // Create a new reservation
  const addReservation = async (reservationData) => {
    try {
      const newReservation = await createReservation(reservationData);
      setReservations(prev => [...prev, newReservation]);
      return newReservation;
    } catch (err) {
      setError(err.message);
      throw err;
    }
  };
  
  // Update an existing reservation
  const modifyReservation = async (id, reservationData) => {
    try {
      const updatedReservation = await updateReservation(id, reservationData);
      setReservations(prev => 
        prev.map(res => res.id === id ? updatedReservation : res)
      );
      return updatedReservation;
    } catch (err) {
      setError(err.message);
      throw err;
    }
  };
  
  // Check in a reservation
  const checkIn = async (id) => {
    try {
      const updated = await checkInReservation(id);
      setReservations(prev => 
        prev.map(res => res.id === id ? updated : res)
      );
      return updated;
    } catch (err) {
      setError(err.message);
      throw err;
    }
  };
  
  // Check out a reservation
  const checkOut = async (id) => {
    try {
      const updated = await checkOutReservation(id);
      setReservations(prev => 
        prev.map(res => res.id === id ? updated : res)
      );
      return updated;
    } catch (err) {
      setError(err.message);
      throw err;
    }
  };
  
  // Cancel a reservation
  const cancel = async (id) => {
    try {
      const updated = await cancelReservation(id);
      setReservations(prev => 
        prev.map(res => res.id === id ? updated : res)
      );
      return updated;
    } catch (err) {
      setError(err.message);
      throw err;
    }
  };
  
  return {
    reservations,
    loading,
    error,
    reload: loadReservations,
    add: addReservation,
    update: modifyReservation,
    checkIn,
    checkOut,
    cancel
  };
}
```

## API Integration

### Frontend-to-Backend Communication

Example API client for the frontend:

```javascript
// api.js - Frontend API client
import axios from 'axios';

const api = axios.create({
  baseURL: '/api',
  headers: {
    'Content-Type': 'application/json'
  },
  withCredentials: true // For cookie-based authentication
});

// Authentication APIs
export const login = (email, password) => 
  api.post('/auth/login', { email, password }).then(res => res.data);

export const logout = () => 
  api.post('/auth/logout').then(res => res.data);

export const getCurrentUser = () => 
  api.get('/auth/me').then(res => res.data);

// Reservation APIs
export const fetchReservations = (filters = {}) => 
  api.get('/reservations', { params: filters }).then(res => res.data);

export const fetchReservation = (id) => 
  api.get(`/reservations/${id}`).then(res => res.data);

export const createReservation = (data) => 
  api.post('/reservations', data).then(res => res.data);

export const updateReservation = (id, data) => 
  api.put(`/reservations/${id}`, data).then(res => res.data);

export const checkInReservation = (id) => 
  api.post(`/reservations/${id}/check-in`).then(res => res.data);

export const checkOutReservation = (id) => 
  api.post(`/reservations/${id}/check-out`).then(res => res.data);

export const cancelReservation = (id) => 
  api.delete(`/reservations/${id}`).then(res => res.data);

// Room APIs
export const fetchRooms = (filters = {}) => 
  api.get('/rooms', { params: filters }).then(res => res.data);

export const fetchAvailableRooms = (checkInDate, checkOutDate) => 
  api.get('/rooms/available', { 
    params: { checkInDate, checkOutDate } 
  }).then(res => res.data);

// Payment APIs
export const fetchPayments = (filters = {}) => 
  api.get('/payments', { params: filters }).then(res => res.data);

export const processPayment = (data) => 
  api.post('/payments', data).then(res => res.data);

// Error handling interceptor
api.interceptors.response.use(
  response => response,
  error => {
    const message = 
      error.response?.data?.error || 
      error.message || 
      'An unknown error occurred';
    
    console.error('API Error:', message);
    return Promise.reject(new Error(message));
  }
);

export default api;
```

## Deployment Considerations

### Environment Variables

Required environment variables for deployment:

```
# Database
DATABASE_URL=postgresql://username:password@localhost:5432/hotel_db

# Authentication
JWT_SECRET=your-secret-key-here
JWT_EXPIRES_IN=24h

# Server
PORT=3000
NODE_ENV=production

# Frontend
NEXT_PUBLIC_API_URL=https://your-api-domain.com/api
```

### Database Migrations

Using Prisma for database migrations:

```bash
# Generate a migration based on schema changes
npx prisma migrate dev --name init

# Apply migrations in production
npx prisma migrate deploy
```

### Security Considerations

1. **Authentication**: JWT-based authentication with secure cookies
2. **Password Security**: Bcrypt for password hashing
3. **Input Validation**: Server-side validation for all inputs
4. **CORS**: Configured to allow only specified origins
5. **Rate Limiting**: Applied to auth routes to prevent brute force attacks
6. **Encrypted Data**: HTTPS for all communications

## Conclusion

This comprehensive hotel management system provides a complete solution for managing hotel operations, from reservations and front-desk operations to housekeeping and financial management. The system is designed with a focus on:

1. **Accurate reservation handling** with proper check-in/check-out time enforcement
2. **Payment tracking** with detailed accounting of payments by receptionist
3. **Shift management** for tracking cash handling
4. **Room availability** checking with conflict prevention
5. **Dynamic pricing** options for different seasons and days
6. **Housekeeping** and maintenance management

The architecture follows modern best practices with a clear separation of concerns:
- Backend: Node.js with Express, Prisma ORM, and PostgreSQL
- Frontend: Next.js 15.2.2 with React 19
- Clear API contracts between frontend and backend
- Role-based access control for security

Implementation should start with the database schema, followed by core backend services, API endpoints, and finally the frontend interfaces for different user roles.

enum Role {
  CUSTOMER
  RECEPTIONIST
  ADMIN
}
```

#### Customer
```prisma
model Customer {
  id                String        @id @default(uuid())
  userId            String        @unique
  address           String?
  city              String?
  country           String?
  postalCode        String?
  idNumber          String?
  idType            String?
  notes             String?
  createdAt         DateTime      @default(now())
  updatedAt         DateTime      @updatedAt
  
  // Relations
  user              User          @relation(fields: [userId], references: [id], onDelete: Cascade)
  reservations      Reservation[]
  
  @@map("customers")
}
```

#### Receptionist
```prisma
model Receptionist {
  id                String         @id @default(uuid())
  userId            String         @unique
  status            StaffStatus    @default(ACTIVE)
  hireDate          DateTime       @default(now())
  createdAt         DateTime       @default(now())
  updatedAt         DateTime       @updatedAt
  
  // Relations
  user              User           @relation(fields: [userId], references: [id], onDelete: Cascade)
  payments          Payment[]
  shifts            Shift[]
  
  @@map("receptionists")
}

enum StaffStatus {
  ACTIVE
  INACTIVE
  ON_LEAVE
}
```

#### Admin
```prisma
model Admin {
  id                String       @id @default(uuid())
  userId            String       @unique
  permissions       String[]     // Array of permission strings
  createdAt         DateTime     @default(now())
  updatedAt         DateTime     @updatedAt
  
  // Relations
  user              User         @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  @@map("admins")
}
```

#### Room
```prisma
model Room {
  id                String         @id @default(uuid())
  roomNumber        String         @unique
  roomTypeId        String
  status            RoomStatus     @default(AVAILABLE)
  floor             Int
  notes             String?
  createdAt         DateTime       @default(now())
  updatedAt         DateTime       @updatedAt
  
  // Relations
  roomType          RoomType       @relation(fields: [roomTypeId], references: [id])
  reservations      Reservation[]
  housekeepingLogs  HousekeepingLog[]
  maintenanceLogs   MaintenanceLog[]
  
  @@map("rooms")
}

enum RoomStatus {
  AVAILABLE
  OCCUPIED
  MAINTENANCE
  CLEANING
  OUT_OF_ORDER
}
```

#### RoomType
```prisma
model RoomType {
  id                String       @id @default(uuid())
  name              String       @unique
  description       String
  capacity          Int
  bedType           String
  amenities         String[]     // Array of amenity strings
  basePrice         Decimal      @db.Decimal(10, 2)
  images            String[]     // Array of image URLs
  createdAt         DateTime     @default(now())
  updatedAt         DateTime     @updatedAt
  
  // Relations
  rooms             Room[]
  rates             Rate[]
  
  @@map("room_types")
}
```

#### Reservation
```prisma
model Reservation {
  id                String           @id @default(uuid())
  customerId        String
  roomId            String
  checkInDate       DateTime         // Stored as date at 12:00 noon
  checkOutDate      DateTime         // Stored as date at 11:00 AM
  status            ReservationStatus @default(CONFIRMED)
  numberOfGuests    Int
  totalAmount       Decimal          @db.Decimal(10, 2)
  notes             String?
  source            ReservationSource @default(DIRECT)
  createdAt         DateTime         @default(now())
  updatedAt         DateTime         @updatedAt
  
  // Relations
  customer          Customer         @relation(fields: [customerId], references: [id])
  room              Room             @relation(fields: [roomId], references: [id])
  payments          Payment[]
  
  @@map("reservations")
  
  // This index ensures we can efficiently query for reservations by date range
  @@index([checkInDate, checkOutDate])
}

enum ReservationStatus {
  CONFIRMED
  CHECKED_IN
  CHECKED_OUT
  CANCELLED
  NO_SHOW
}

enum ReservationSource {
  DIRECT
  WEBSITE
  BOOKING_COM
  EXPEDIA
  AIRBNB
  PHONE
  WALK_IN
  OTHER
}
```

#### Payment
```prisma
model Payment {
  id                String         @id @default(uuid())
  reservationId     String
  receptionistId    String
  amount            Decimal        @db.Decimal(10, 2)
  paymentMethod     PaymentMethod
  paymentStatus     PaymentStatus  @default(COMPLETED)
  transactionId     String?
  paymentDate       DateTime       @default(now())
  notes             String?
  createdAt         DateTime       @default(now())
  updatedAt         DateTime       @updatedAt
  
  // Relations
  reservation       Reservation    @relation(fields: [reservationId], references: [id])
  receptionist      Receptionist   @relation(fields: [receptionistId], references: [id])
  
  @@map("payments")
}

enum PaymentMethod {
  CASH
  CREDIT_CARD
  DEBIT_CARD
  BANK_TRANSFER
  MOBILE_PAYMENT
  OTHER
}

enum PaymentStatus {
  PENDING
  COMPLETED
  FAILED
  REFUNDED
}
```

#### Rate
```prisma
model Rate {
  id                String       @id @default(uuid())
  roomTypeId        String
  name              String       // e.g., "Standard", "Weekend", "Holiday"
  price             Decimal      @db.Decimal(10, 2)
  startDate         DateTime?    // Optional: for seasonal rates
  endDate           DateTime?    // Optional: for seasonal rates
  daysOfWeek        Int[]        // 0-6 for days of week this rate applies
  minimumStay       Int          @default(1)
  createdAt         DateTime     @default(now())
  updatedAt         DateTime     @updatedAt
  
  // Relations
  roomType          RoomType     @relation(fields: [roomTypeId], references: [id])
  
  @@map("rates")
}
```

### Additional Tables

#### Shift
```prisma
model Shift {
  id                String       @id @default(uuid())
  receptionistId    String
  startTime         DateTime
  endTime           DateTime?
  status            ShiftStatus  @default(ACTIVE)
  cashBalance       Decimal      @db.Decimal(10, 2) @default(0)
  notes             String?
  createdAt         DateTime     @default(now())
  updatedAt         DateTime     @updatedAt
  
  // Relations
  receptionist      Receptionist @relation(fields: [receptionistId], references: [id])
  
  @@map("shifts")
}

enum ShiftStatus {
  ACTIVE
  COMPLETED
  RECONCILED
}
```

#### HousekeepingLog
```prisma
model HousekeepingLog {
  id                String         @id @default(uuid())
  roomId            String
  status            CleaningStatus @default(PENDING)
  notes             String?
  assignedTo        String?        // Could be expanded to relate to a housekeeping staff model
  completedAt       DateTime?
  createdAt         DateTime       @default(now())
  updatedAt         DateTime       @updatedAt
  
  // Relations
  room              Room           @relation(fields: [roomId], references: [id])
  
  @@map("housekeeping_logs")
}

enum CleaningStatus {
  PENDING
  IN_PROGRESS
  COMPLETED
  VERIFIED
}
```

#### MaintenanceLog
```prisma
model MaintenanceLog {
  id                String            @id @default(uuid())
  roomId            String
  issue             String
  priority          MaintenancePriority @default(MEDIUM)
  status            MaintenanceStatus @default(PENDING)
  assignedTo        String?
  completedAt       DateTime?
  notes             String?
  createdAt         DateTime          @default(now())
  updatedAt         DateTime          @updatedAt
  
  // Relations
  room              Room              @relation(fields: [roomId], references: [id])
  
  @@map("maintenance_logs")
}

enum MaintenancePriority {
  LOW
  MEDIUM
  HIGH
  CRITICAL
}

enum MaintenanceStatus {
  PENDING
  IN_PROGRESS
  COMPLETED
  VERIFIED
}
```

## Backend Structure

### Directory Structure

```
backend/
├── prisma/
│   ├── schema.prisma    # Prisma schema file
│   └── migrations/      # Database migrations
├── src/
│   ├── api/             # API routes
│   │   ├── routes/      # Route definitions
│   │   │   ├── auth.routes.js
│   │   │   ├── users.routes.js
│   │   │   ├── rooms.routes.js
│   │   │   ├── reservations.routes.js
│   │   │   ├── payments.routes.js
│   │   │   └── ...
│   │   └── middlewares/ # Express middlewares
│   │       ├── auth.middleware.js
│   │       ├── validation.middleware.js
│   │       └── ...
│   ├── controllers/     # Route controllers
│   │   ├── auth.controller.js
│   │   ├── users.controller.js
│   │   ├── rooms.controller.js
│   │   ├── reservations.controller.js
│   │   ├── payments.controller.js
│   │   └── ...
│   ├── services/        # Business logic
│   │   ├── auth.service.js
│   │   ├── users.service.js
│   │   ├── rooms.service.js
│   │   ├── reservations.service.js
│   │   ├── payments.service.js
│   │   └── ...
│   ├── utils/           # Utility functions
│   │   ├── errors.js
│   │   ├── validation.js
│   │   ├── dates.js
│   │   └── ...
│   ├── config/          # Configuration
│   │   ├── database.js
│   │   ├── auth.js
│   │   └── ...
│   └── app.js           # Express application setup
└── server.js            # Server entry point
```

### Key API Endpoints

#### Authentication

```
POST /api/auth/register               # Register a new user
POST /api/auth/login                  # Login user
POST /api/auth/logout                 # Logout user
GET /api/auth/me                      # Get current user info
POST /api/auth/reset-password/request # Request password reset
POST /api/auth/reset-password/confirm # Confirm password reset
```

#### Users

```
GET /api/users                    # List all users (admin only)
GET /api/users/:id                # Get user by ID
POST /api/users                   # Create new user (admin only)
PUT /api/users/:id                # Update user
DELETE /api/users/:id             # Delete user (admin only)
GET /api/users/:id/reservations   # Get user's reservations
```

#### Rooms

```
GET /api/rooms                    # List all rooms
GET /api/rooms/:id                # Get room by ID
POST /api/rooms                   # Create new room (admin only)
PUT /api/rooms/:id                # Update room (admin only)
DELETE /api/rooms/:id             # Delete room (admin only)
GET /api/rooms/:id/reservations   # Get room's reservations
GET /api/rooms/:id/housekeeping   # Get room's housekeeping logs
GET /api/rooms/:id/maintenance    # Get room's maintenance logs
GET /api/rooms/available          # Get available rooms by date range
```

#### Room Types

```
GET /api/room-types               # List all room types
GET /api/room-types/:id           # Get room type by ID
POST /api/room-types              # Create new room type (admin only)
PUT /api/room-types/:id           # Update room type (admin only)
DELETE /api/room-types/:id        # Delete room type (admin only)
GET /api/room-types/:id/rates     # Get room type's rates
```

#### Reservations

```
GET /api/reservations                 # List all reservations
GET /api/reservations/:id             # Get reservation by ID
POST /api/reservations                # Create new reservation
PUT /api/reservations/:id             # Update reservation
DELETE /api/reservations/:id          # Cancel reservation
POST /api/reservations/:id/check-in   # Check in a reservation
POST /api/reservations/:id/check-out  # Check out a reservation
GET /api/reservations/:id/payments    # Get reservation's payments
```

#### Payments

```
GET /api/payments                # List all payments
GET /api/payments/:id            # Get payment by ID
POST /api/payments               # Create new payment
PUT /api/payments/:id            # Update payment (admin only)
DELETE /api/payments/:id         # Delete payment (admin only)
```

#### Receptionist Shifts

```
GET /api/shifts                  # List all shifts
GET /api/shifts/:id              # Get shift by ID
POST /api/shifts                 # Create new shift
PUT /api/shifts/:id              # Update shift
POST /api/shifts/:id/end         # End a shift
GET /api/shifts/:id/payments     # Get payments processed during shift
```

#### Housekeeping

```
GET /api/housekeeping            # List all housekeeping logs
GET /api/housekeeping/:id        # Get housekeeping log by ID
POST /api/housekeeping           # Create new housekeeping log
PUT /api/housekeeping/:id        # Update housekeeping log
DELETE /api/housekeeping/:id     # Delete housekeeping log
```

#### Maintenance

```
GET /api/maintenance             # List all maintenance logs
GET /api/maintenance/:id         # Get maintenance log by ID
POST /api/maintenance            # Create new maintenance log
PUT /api/maintenance/:id         # Update maintenance log
DELETE /api/maintenance/:id      # Delete maintenance log
```

### Core Business Logic

#### Reservation System

The reservation system handles the booking of rooms while enforcing the hotel's check-in/check-out times:
- Check-in time: 12:00 (noon)
- Check-out time: 11:00 AM

When creating a reservation, the system must ensure:
1. Room availability during the requested period
2. No overlap with existing reservations
3. Valid check-in/check-out dates (check-in at 12:00, check-out at 11:00)

```javascript
// Example reservation availability check function (pseudocode)
async function isRoomAvailable(roomId, checkInDate, checkOutDate) {
  // Format dates to ensure proper time: check-in at 12:00, check-out at 11:00
  const formattedCheckIn = setTimeToNoon(checkInDate);
  const formattedCheckOut = setTimeToElevenAM(checkOutDate);
  
  // Query for any overlapping reservations
  const overlappingReservations = await prisma.reservation.findMany({
    where: {
      roomId: roomId,
      status: { in: ['CONFIRMED', 'CHECKED_IN'] },
      NOT: [
        // New check-in is after existing check-out
        { checkOutDate: { lte: formattedCheckIn } },
        // New check-out is before existing check-in
        { checkInDate: { gte: formattedCheckOut } }
      ]
    }
  });
  
  return overlappingReservations.length === 0;
}
```

#### Payment Processing

Payment tracking is a crucial component, especially for tracking receptionist cash handling:

1. Payments can be partial (e.g., 30% at check-in, 70% at check-out)
2. Each payment is linked to:
   - The reservation
   - The receptionist who processed it
   - The payment method
3. Shift management tracks how much cash each receptionist should have

```javascript
// Example payment creation function (pseudocode)
async function createPayment(reservationId, receptionistId, amount, paymentMethod) {
  // Find the reservation
  const reservation = await prisma.reservation.findUnique({
    where: { id: reservationId },
    include: { payments: true }
  });
  
  // Calculate total amount paid so far
  const totalPaid = reservation.payments.reduce((sum, payment) => sum + payment.amount, 0);
  
  // Ensure payment isn't exceeding total amount
  if (totalPaid + amount > reservation.totalAmount) {
    throw new Error('Payment amount exceeds remaining balance');
  }
  
  // Create the payment
  const payment = await prisma.payment.create({
    data: {
      reservationId,
      receptionistId,
      amount,
      paymentMethod,
      paymentStatus: 'COMPLETED',
      paymentDate: new Date()
    }
  });
  
  // Update receptionist's shift cash balance if payment is cash
  if (paymentMethod === 'CASH') {
    await prisma.shift.update({
      where: {
        receptionistId,
        status: 'ACTIVE'
      },
      data: {
        cashBalance: { increment: amount }
      }
    });
  }
  
  return payment;
}
```

## Frontend Structure

### Directory Structure

```
frontend/
├── public/                     # Static files
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── layout.jsx          # Root layout
│   │   ├── page.jsx            # Home page
│   │   ├── (public)/           # Public website routes
│   │   │   ├── about/
│   │   │   ├── rooms/
│   │   │   ├── contact/
│   │   │   └── ...
│   │   ├── (auth)/             # Authentication routes
│   │   │   ├── login/
│   │   │   ├── register/
│   │   │   └── ...
│   │   ├── (customer)/         # Customer portal routes
│   │   │   ├── dashboard/
│   │   │   ├── reservations/
│   │   │   ├── profile/
│   │   │   └── ...
│   │   ├── (receptionist)/     # Receptionist portal routes
│   │   │   ├── dashboard/
│   │   │   ├── check-in/
│   │   │   ├── check-out/
│   │   │   ├── reservations/
│   │   │   ├── rooms/
│   │   │   ├── payments/
│   │   │   ├── housekeeping/
│   │   │   └── ...
│   │   └── (admin)/            # Admin dashboard routes
│   │       ├── dashboard/
│   │       ├── users/
│   │       ├── rooms/
│   │       ├── room-types/
│   │       ├── rates/
│   │       ├── reports/
│   │       └── ...
│   ├── components/             # Reusable components
│   │   ├── ui/                 # UI components
│   │   ├── forms/              # Form components
│   │   ├── layouts/            # Layout components
│   │   ├── tables/             # Table components
│   │   └── ...
│   ├── lib/                    # Utility functions
│   │   ├── api.js              # API client
│   │   ├── auth.js             # Authentication utilities
│   │   ├── date-utils.js       # Date helpers
│   │   └── ...
│   ├── hooks/                  # Custom React hooks
│   │   ├── use-auth.js
│   │   ├── use-reservations.js
│   │   └── ...
│   ├── context/                # React context providers
│   │   ├── auth-context.jsx
│   │   └── ...
│   └── types/                  # TypeScript types (if using TS)
└── ...
```

### Key Frontend Features

#### 1. Public Website

The public-facing website allows potential customers to:
- View room types, amenities, and images
- Check room availability for specific dates
- Make reservations
- Learn about hotel facilities and services
- Contact the hotel

#### 2. Customer Portal

Registered customers can:
- View their current and past reservations
- Make new reservations
- Modify or cancel existing reservations
- Update their profile information
- View payment history

#### 3. Receptionist Portal

The receptionist interface focuses on daily operations:
- Dashboard with today's check-ins/check-outs
- Room status overview
- Process check-ins and check-outs
- Handle payments
- Manage reservations
- View and update housekeeping status
- Handle guest requests
- Manage shift handovers

Key receptionist features:

**Check-in Process:**
1. Search for reservation by name, confirmation number, or date
2. Verify guest information
3. Collect ID for verification
4. Process payment (full or partial)
5. Assign room key
6. Update reservation status

**Check-out Process:**
1. Review reservation details
2. Calculate any additional charges
3. Process final payment
4. Update room status for housekeeping
5. Update reservation status

**Payment Handling:**
1. Track all payments by receptionist
2. Record payment method
3. Handle partial payments
4. Generate receipts
5. Reconcile shift cash balance

#### 4. Admin Dashboard

The admin interface provides comprehensive management tools:
- User management (staff and customers)
- Room and room type management
- Rate and pricing configuration
- Reservation management
- Financial reporting
- Occupancy analytics
- System configuration

### Reservation Calendar Component

A critical component for both receptionists and admins is the reservation calendar, which visualizes room occupancy:

```jsx
// Example Room Reservation Calendar (simplified React component)
const RoomCalendar = ({ rooms, startDate, endDate }) => {
  const [reservations, setReservations] = useState([]);
  
  useEffect(() => {
    // Fetch reservations for the date range
    fetchReservations(startDate, endDate).then(setReservations);
  }, [startDate, endDate]);
  
  // Generate calendar days
  const days = generateDateRange(startDate, endDate);
  
  return (
    <div className="room-calendar">
      <div className="calendar-header">
        <div className="room-cell">Room</div>
        {days.map(day => (
          <div key={day.toISOString()} className="date-cell">
            {format(day, 'MMM d')}
          </div>
        ))}
      </div>
      
      <div className="calendar-body">
        {rooms.map(room => (
          <div key={room.id} className="room-row">
            <div className="room-cell">{room.roomNumber}</div>
            
            {days.map(day => {
              const reservation = reservations.find(r => 
                room.id === r.roomId && 
                isDateWithinRange(day, r.checkInDate, r.checkOutDate)
              );
              
              return (
                <div 
                  key={day.toISOString()} 
                  className={`date-cell ${reservation ? 'occupied' : 'available'}`}
                  title={reservation ? `${reservation.customer.firstName} ${reservation.customer.lastName}` : 'Available'}
                >
                  {reservation && <div className="reservation-indicator" />}
                </div>
              );
            })}
          </div>
        ))}
      </div>
    </div>
  );
};
```

## Reservation Business Logic

### Creating a Reservation

The system enforces the hotel's check-in/check-out policy:
- Check-in time: 12:00 (noon)
- Check-out time: 11:00 AM

This allows housekeeping one hour to prepare rooms between guests.

```javascript
// Example reservation creation (backend controller pseudocode)
async function createReservation(req, res) {
  const { customerId, roomId, checkInDate, checkOutDate, numberOfGuests } = req.body;
  
  try {
    // Validate check-in/check-out times
    const formattedCheckIn = setTimeToNoon(new Date(checkInDate));
    const formattedCheckOut = setTimeToElevenAM(new Date(checkOutDate));
    
    // Check if room is available for the requested period
    const isAvailable = await isRoomAvailable(roomId, formattedCheckIn, formattedCheckOut);
    
    if (!isAvailable) {
      return res.status(400).json({ error: 'Room is not available for the selected dates' });
    }
    
    // Calculate total price based on room type rates and length of stay
    const roomType = await getRoomType(roomId);
    const totalNights = calculateNights(formattedCheckIn, formattedCheckOut);
    const totalAmount = calculateTotalPrice(roomType, formattedCheckIn, formattedCheckOut);
    
    // Create the reservation
    const reservation = await prisma.reservation.create({
      data: {
        customerId,
        roomId,
        checkInDate: formattedCheckIn,
        checkOutDate: formattedCheckOut,
        numberOfGuests,
        totalAmount,
        status: 'CONFIRMED'
      }
    });
    
    return res.status(201).json(reservation);
  } catch (error) {
    return res.status(500).json({ error: error.message });
  }
}
```

### Payment Tracking

The system tracks all payment transactions, including:
- Which receptionist processed the payment
- Payment method used
- Partial payments
- Remaining balance

```javascript
// Example payment processing (backend service pseudocode)
async function processPayment(reservationId, receptionistId, amount, paymentMethod) {
  const reservation = await prisma.reservation.findUnique({
    where: { id: reservationId },
    include: { payments: true }
  });
  
  if (!reservation) {
    throw new Error('Reservation not found');
  }
  
  // Calculate total amount paid so far
  const totalPaid = reservation.payments.reduce((sum, payment) => sum + payment.amount, 0);
  const remainingBalance = reservation.totalAmount - totalPaid;
  
  // Validate payment amount
  if (amount > remainingBalance) {
    throw new Error('Payment amount exceeds remaining balance');
  }
  
  // Find active shift for the receptionist
  const activeShift = await prisma.shift.findFirst({
    where: {
      receptionistId,
      status: 'ACTIVE'
    }
  });
  
  if (!activeShift && paymentMethod === 'CASH') {
    throw new Error('Receptionist must have an active shift to process cash payments');
  }
  
  // Create the payment record
  const payment = await prisma.payment.create({
    data: {
      reservationId,
      receptionistId,
      amount,
      paymentMethod,
      paymentStatus: 'COMPLETED',
      paymentDate: new Date()
    }
  });
  
  // Update shift cash balance for cash payments
  if (paymentMethod === 'CASH' && activeShift) {
    await prisma.shift.update({
      where: { id: activeShift.id },
      data: {
        cashBalance: { increment: amount }
      }
    });
  }
  
  return {
    payment,
    remainingBalance: remainingBalance - amount
  };
}
```

### Receptionist Shift Management

The shift system tracks cash handled by each receptionist:

```javascript
// Start shift function (backend service pseudocode)
async function startShift(receptionistId, initialCash = 0) {
  // Check if receptionist already has an active shift
  const activeShift = await prisma.shift.findFirst({
    where: {
      receptionistId,
      status: 'ACTIVE'
    }
  });
  
  if (activeShift) {
    throw new Error('Receptionist already has an active shift');
  }
  
  // Create new shift
  return prisma.shift.create({
    data: {
      receptionistId,
      startTime: new Date(),
      cashBalance: initialCash,
      status: 'ACTIVE'
    }
  });
}

// End shift function (backend service pseudocode)
async function endShift(shiftId, finalCash) {
  const shift = await prisma.shift.findUnique({
    where: { id: shiftId }
  });
  
  if (!shift) {
    throw new Error('Shift not found');
  }
  
  if (shift.status !== 'ACTIVE') {
    throw new Error('Shift is not active');
  }
  
  // Calculate expected cash based on payments
  const cashPayments = await prisma.payment.aggregate({
    where: {
      receptionistId: shift.receptionistId,
      paymentMethod: 'CASH',
      paymentDate: {
        gte: shift.startTime,
        lte: new Date()
      }
    },
    _sum: {
      amount: true
    }
  });
  
  const expectedCash = shift.cashBalance + (cashPayments._sum.amount || 0);
  const cashDiscrepancy = finalCash - expectedCash;
  
  // Update shift as completed
  return prisma.shift.update({
    where: { id: shiftId },
    data: {
      endTime: new Date(),
      cashBalance: finalCash,
      status: 'COMPLETED',
      notes: cashDiscrepancy !== 0 
        ? `Cash discrepancy: ${cashDiscrepancy > 0 ? '+' : ''}${cashDiscrepancy}` 
        : 'Balanced'
    }
  });
}
```
