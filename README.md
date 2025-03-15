##Comming soon

 # Hotel PMS Schema Diagram

```
+----------------+          +---------------+          +----------------+
| Property       |<---------| Room          |<---------| RoomType       |
+----------------+          +---------------+          +----------------+
| id             |          | id            |          | id             |
| name           |          | roomNumber    |          | name           |
| code           |          | floor         |          | description    |
| address        |          | status        |          | basePrice      |
| description    |          | condition     |          | capacity       |
| isActive       |          | notes         |          | maxAdults      |
| sortOrder      |          | isActive      |          | maxChildren    |
| createdAt      |          | roomTypeId    |          | extraAdultPrice|
| updatedAt      |          | propertyId    |          | extraChildPrice|
+----------------+          | createdAt     |          | propertyId     |
      ^                     | updatedAt     |          | createdAt      |
      |                     +---------------+          | updatedAt      |
      |                            |                   +----------------+
      |                            |                          |
      |                            |                          |
      |                     +------+------+                   |
      |                     |             |                   |
+-----+--------+    +-------+--------+    |           +------+---------+
| RateCalendar |    | RoomReservation|    |           | MealPlanPrice  |
+-------------+     +----------------+    |           +----------------+
| id          |     | id             |    |           | id             |
| roomTypeId  |<----| roomId         |    |           | roomTypeId     |
| propertyId  |     | reservationId  |    |           | mealPlan       |
| date        |     | mealPlan       |    |           | adultPrice     |
| price       |     | adultCount     |    |           | childPrice     |
| bbSupplement|     | childCount     |    |           +----------------+
| hbSupplement|     | basePrice      |    |
| fbSupplement|     | finalPrice     |    |
| aiSupplement|     | priceModified  |    |
| availableRooms|   | ...            |    |
| ...         |     +----------------+    |
+-------------+            |              |
                           |              |
                           |              |
                    +------+-------+      |
                    | MealService  |      |
                    +--------------+      |
                    | id           |      |
                    | roomReservId |      |
                    | date         |      |
                    | breakfastCount|     |
                    | lunchCount   |      |
                    | dinnerCount  |      |
                    | isModified   |      |
                    | ...          |      |
                    +--------------+      |
                                          |
                                          |
              +---------------------------+---------------------------+
              |                           |                           |
              v                           v                           v
      +----------------+        +------------------+       +------------------+
      | RoomAmenity    |        | MaintenanceIssue |       | RoomItemInstance |
      +----------------+        +------------------+       +------------------+
      | id             |        | id               |       | id               |
      | roomId         |        | roomId           |       | roomId           |
      | amenityId      |        | title            |       | itemId           |
      | status         |        | description      |       | serialNumber     |
      | notes          |        | reportedById     |       | status           |
      +----------------+        | assignedToId     |       | ...              |
              |                 | priority         |       +------------------+
              |                 | status           |                |
              |                 | ...              |                |
              |                 +------------------+                |
              |                                                     |
              v                                                     v
      +----------------+                                   +------------------+
      | Amenity        |                                   | RoomItemIssue    |
      +----------------+                                   +------------------+
      | id             |                                   | id               |
      | name           |                                   | roomItemId       |
      | description    |                                   | title            |
      | icon           |                                   | description      |
      | category       |                                   | reportedById     |
      | ...            |                                   | assignedToId     |
      +----------------+                                   | status           |
                                                          | ...              |
                                                          +------------------+


+----------------+          +---------------+          +----------------+
| Guest          |<---------| Reservation   |          | Staff          |
+----------------+          +---------------+          +----------------+
| id             |          | id            |          | id             |
| email          |          | reservationCode|         | email          |
| password       |          | guestId       |          | password       |
| firstName      |          | status        |          | firstName      |
| lastName       |          | checkInDate   |          | lastName       |
| phone          |          | checkOutDate  |          | phone          |
| address        |          | totalAmount   |          | role           |
| idNumber       |          | balanceAmount |          | employeeId     |
| ...            |          | adultCount    |          | ...            |
+----------------+          | childCount    |          +----------------+
      |                     | source        |                 ^
      |                     | isActive      |                 |
      v                     | ...           |                 |
+----------------+          +---------------+                 |
| GuestVehicle   |                |                          |
+----------------+                |                          |
| id             |                v                          |
| guestId        |        +-------+--------+                 |
| licensePlate   |        | ActiveBooking  |<----------------+
| make           |        +----------------+                 |
| model          |        | id             |                 |
| color          |        | reservationId  |                 |
| ...            |        | roomIds        |                 |
+----------------+        | checkInDate    |        +--------+--------+
                          | checkOutDate   |        | Shift           |
                          | status         |        +-----------------+
                          +----------------+        | id              |
                                                   | receptionistId  |
                                                   | startTime       |
                                                   | endTime         |
                                                   | cashBalance     |
                                                   | ...             |
                                                   +-----------------+
                                                           |
                                                           v
                                                  +------------------+
                                                  | CashReconciliation|
                                                  +------------------+
                                                  | id               |
                                                  | shiftId          |
                                                  | currencyCode     |
                                                  | expectedAmount   |
                                                  | actualAmount     |
                                                  | difference       |
                                                  | ...              |
                                                  +------------------+


+----------------+          +---------------+          +----------------+
| Payment        |<---------| PaymentTrans  |--------->| Reservation    |
+----------------+          +---------------+          +----------------+
| id             |          | id            |
| cashierId      |          | paymentId     |
| createdAt      |          | reservationId |
| updatedAt      |          | amount        |
+----------------+          | originalAmount|
                           | originalCurrency|
                           | exchangeRate   |
                           | method         |
                           | status         |
                           | ...            |
                           +---------------+


+----------------+          +---------------+          +----------------+
| Invoice        |<---------| InvoiceLineItem|         | InvoicePayment |
+----------------+          +---------------+          +----------------+
| id             |          | id            |          | id             |
| invoiceNumber  |          | invoiceId     |          | invoiceId      |
| fiscalNumber   |          | description   |          | amount         |
| reservationId  |          | quantity      |          | originalAmount |
| guestId        |          | unitPrice     |          | currency       |
| staffId        |          | taxRate       |          | exchangeRate   |
| issueDate      |          | taxAmount     |          | method         |
| dueDate        |          | totalAmount   |          | reference      |
| subtotal       |          | taxCode       |          | ...            |
| taxAmount      |          | productCode   |          +----------------+
| totalAmount    |          | ...           |
| currency       |          +---------------+
| status         |
| isFiscal       |
| ...            |
+----------------+


+----------------+          +---------------+          +----------------+
| InventoryItem  |<---------| InventoryTrans |         | PurchaseOrder  |
+----------------+          +---------------+          +----------------+
| id             |          | id            |          | id             |
| name           |          | itemId        |          | orderNumber    |
| description    |          | quantity      |          | supplierId     |
| categoryId     |          | type          |          | staffId        |
| barcode        |          | referenceId   |          | orderDate      |
| sku            |          | referenceType |          | expectedDelivery|
| unit           |          | notes         |          | status         |
| minStock       |          | staffId       |          | totalAmount    |
| currentStock   |          | ...           |          | ...            |
| reorderPoint   |          +---------------+          +----------------+
| costPrice      |                                              |
| ...            |                                              |
+----------------+                                              |
       |                                                        |
       |                                                        v
       |                                              +------------------+
       |                                              | PurchaseOrderItem|
       |                                              +------------------+
       |                                              | id               |
       |                                              | purchaseOrderId  |
       |                                              | itemId           |
       |                                              | quantity         |
       |                                              | unitPrice        |
       |                                              | totalPrice       |
       |                                              | receivedQuantity |
       |                                              | ...              |
       |                                              +------------------+
       |
       v
+----------------+
| InvItemSupplier|
+----------------+
| id             |
| itemId         |
| supplierId     |
| preferredOrder |
| supplierSku    |
| defaultPrice   |
| ...            |
+----------------+
```

## Key Entity Relationships

### Property Management
- Property → RoomType → Room
- Property → RateCalendar (seasonal pricing)

### Reservation System
- Guest → Reservation → RoomReservation → Room
- Reservation → ActiveBooking (performance optimization)
- RoomReservation → MealService (daily meal tracking)

### Room Features & Maintenance
- Room → RoomAmenity → Amenity
- Room → MaintenanceIssue
- Room → RoomItemInstance → RoomItemIssue

### Financial Management
- Reservation → Payment → PaymentTransaction
- Guest → Invoice → InvoiceLineItem
- Invoice → InvoicePayment

### Staff Management
- Staff → Shift → CashReconciliation
- Staff → StaffTimeRecord → StaffMonthlyHours

### Inventory Management
- InventoryItem → InventoryTransaction
- InventorySupplier → PurchaseOrder → PurchaseOrderItem
- InventoryItem ↔ InventorySupplier (many-to-many)

## Meal Plan Implementation
- RoomType → MealPlanPrice (pricing for each meal plan)
- RoomReservation → mealPlan (enum: RO, BB, HB, FB, AI)
- RoomReservation → MealService (daily meal counts)
- DailyMealSummary (for kitchen reports)

## Multi-Property Support
- Property has many Rooms
- Property has many RoomTypes
- RateCalendar linked to Property
- RoomCleaning linked to Property

## Room Item Issue Tracking
- Room → RoomItemInstance → RoomItem → RoomItemCategory
- RoomItemInstance → RoomItemIssue (tracking broken items)
