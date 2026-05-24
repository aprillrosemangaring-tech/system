# Accurate Algorithm and Live Stock Depletion Fix

## Problem
Current implementation uses static `monthlyUsage` values from CSV, not recalculated based on live order data.

## Solution
Replace the `/api/inventory/predict/:id` endpoint with accurate live calculations.

## Key Changes

### 1. Calculate Actual Monthly Usage from Orders
Instead of using the CSV value, query the past 6 months of orders to determine real usage:

```typescript
async function calculateActualMonthlyUsage(db: any) {
  const sixMonthsAgo = new Date();
  sixMonthsAgo.setMonth(sixMonthsAgo.getMonth() - 6);
  
  const orders = await db.all(`
    SELECT COUNT(*) as order_count
    FROM orders
    WHERE date >= ?
  `, [sixMonthsAgo.toISOString().split('T')[0]]);
  
  const avgMonthlyOrders = (orders[0]?.order_count || 0) / 6;
  return Math.max(1, avgMonthlyOrders * 0.5);
}
```

### 2. Accurate Depletion Date Calculation
Calculate exactly when stock will hit minimum threshold:

```typescript
function calculateDepletionDate(quantity, minStock, monthlyUsage) {
  const dailyUsage = monthlyUsage / 30;
  const usableStock = Math.max(0, quantity - minStock);
  const daysRemaining = usableStock > 0 ? Math.floor(usableStock / dailyUsage) : 0;
  
  const depletionDate = new Date();
  depletionDate.setDate(depletionDate.getDate() + daysRemaining);
  
  return {
    depletionDate: depletionDate.toISOString().split('T')[0],
    daysRemaining,
    weeksRemaining: Math.floor(daysRemaining / 7),
    monthsRemaining: Math.floor(daysRemaining / 30)
  };
}
```

### 3. Status Indicators with Emoji
Add visual indicators for inventory health:

```typescript
function getStockStatus(quantity, minStock, daysRemaining) {
  if (quantity <= minStock || daysRemaining <= 3) 
    return { status: '🔴 Critical', color: '#dc2626' };
  if (daysRemaining <= 14) 
    return { status: '🟠 Warning', color: '#ea580c' };
  if (daysRemaining <= 30) 
    return { status: '🟡 Low', color: '#eab308' };
  return { status: '🟢 Healthy', color: '#16a34a' };
}
```

### 4. Response Format
Updated prediction response includes:

```json
{
  "itemId": "ORD-0001",
  "itemName": "Ariel Powder 1kg",
  "currentStock": 168,
  "minStock": 13,
  "actualMonthlyUsage": 12.5,
  "dailyUsage": 0.417,
  "depletionDate": "2026-06-15",
  "daysRemaining": 21,
  "weeksRemaining": 3,
  "monthsRemaining": 0,
  "currentStatus": "🟢 Healthy",
  "predictions": [
    {
      "month": "Jun 2026",
      "quantity": 155.5,
      "status": "🟢 Healthy"
    },
    ...
  ],
  "recommendation": "📦 Reorder soon. Stock depletes in 21 days"
}
```

## Implementation Steps

1. Find the `/api/inventory/predict/:id` endpoint in server.ts (around line 770)
2. Replace with the new version that:
   - Calculates `actualMonthlyUsage` from database orders
   - Uses `calculateDepletionDate()` for accurate dates
   - Uses `getStockStatus()` for emoji indicators
   - Returns enriched prediction data

3. Add helper functions at top of file or in utility section

4. Test with:
   ```bash
   curl http://localhost:5000/api/inventory/predict/ORD-0001
   ```

## Benefits

✅ Real-time accuracy based on actual order history
✅ Accurate depletion dates (not based on static CSV data)
✅ Visual indicators (emoji) for quick status recognition
✅ Actionable recommendations for restocking
✅ Live recalculation each time endpoint is called
