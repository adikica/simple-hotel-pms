
## Implementation Notes

### Date Handling

Proper date handling is critical for the reservation system. The system consistently enforces check-in at 12:00 noon and check-out at 11:00 AM to ensure proper room turnover:

```javascript
// Set time to 12:00 noon for check-in
function setTimeToNoon(date) {
  const result = new Date(date);
  result.setHours(12, 0, 0, 0);
  return result;
}

// Set time to 11:00 AM for check-out
function setTimeToElevenAM(date) {
  const result = new Date(date);
  result.setHours(11, 0, 0, 0);
  return result;
}

// Calculate number of nights between check-in and check-out
function calculateNights(checkInDate, checkOutDate) {
  const millisecondsPerDay = 24 * 60 * 60 * 1000;
  const diffTime = checkOutDate.getTime() - checkInDate.getTime();
  return Math.ceil(diffTime / millisecondsPerDay);
}

// Check if a date falls within a reservation period
function isDateWithinRange(date, checkInDate, checkOutDate) {
  const compareDate = new Date(date);
  compareDate.setHours(12, 0, 0, 0);
  
  return compareDate >= checkInDate && compareDate < checkOutDate;
}
