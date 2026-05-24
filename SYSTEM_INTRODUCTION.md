# Laundry Management System

## System Introduction

This is a comprehensive **Laundry Management System** built with modern web technologies to streamline laundry business operations. The system is designed to manage orders, inventory, customer relationships, and business analytics in a single integrated platform. It features a Philippine Blue & Gold themed interface for a professional, country-specific user experience.

### Technology Stack

**Frontend:**
- React with TypeScript
- Vite (Build tool)
- Tailwind CSS (Styling)
- React Hooks for state management

**Backend:**
- Express.js (Node.js server)
- SQLite3 (Database)
- CSV data import/export
- RESTful API architecture

---

## Core Functionalities

### 1. **Authentication System**
- Email and password-based login
- Session-based authentication
- Logout functionality with data cleanup
- Demo credentials support for testing

### 2. **Dashboard & Analytics**
- **Order Statistics**: Total number of orders tracked
- **Inventory Overview**: Total inventory items count
- **Revenue Tracking**: Calculate and display total revenue from all orders
- **Stock Health**: Monitor inventory items above minimum stock levels
- **Real-time Data**: Loading states for asynchronous data fetching

### 3. **Order Management**
- **Order Tracking**: Display recent orders in a formatted table
- **Order Details**: 
  - Order ID, Customer Name, Service Type
  - Weight (kg), Amount (Philippine Peso)
  - Status (Pending, Completed, In Progress, etc.)
  - Order Date
- **Customer Information**: Linked to customer profiles
- **Payment Tracking**: Monitor payment status (Paid, Pending, etc.)
- **Service Types**: Multiple laundry service categories

### 4. **Inventory Management**
- **Stock Level Tracking**: Monitor current inventory quantities
- **Minimum Stock Alerts**: Flag items below minimum stock thresholds
- **Monthly Usage Analytics**: Track consumption patterns
- **Units Management**: Support various unit types (kg, liters, pieces, etc.)
- **Last Updated Tracking**: Know when inventory was last modified

### 5. **Customer Management**
- **Customer Profiles**:
  - Customer ID, Name, Contact Number
  - Email Address, Physical Address
  - Order History Count
  - Total Amount Spent
  - Last Visit Date
  - Customer Status (Active, Inactive, VIP, etc.)
- **Customer Notes**: Store special instructions or preferences
- **Contact Information**: Phone number validation and storage

### 6. **Activity Logging**
- **User Activity Tracking**: Log all business transactions
- **Action Records**: What happened, when, and by whom
- **Activity Types**: Different categories for various operations
- **Audit Trail**: Complete history for accountability

### 7. **Business Settings**
- **Business Configuration**:
  - Business Name
  - Contact Number
  - Business Address
  - Logo/Branding
- **Service Management**:
  - Service names and pricing
  - Unit pricing (per kg, per piece, etc.)
  - Service categories
  - Custom pricing models

### 8. **Data Import & Export**
- **CSV Data Loading**: Bulk import of orders and inventory data
- **Initial Data Setup**: Automatic data population on first load
- **Data Validation**: Phone number validation using libphonenumber-js
- **Flexible Data Format**: Support for multiple data sources

### 9. **Responsive User Interface**
- **Professional Dashboard Layout**: Organized stat cards and tables
- **Color-Coded Status**: Visual indicators for order status
- **Philippine Blue & Gold Theme**: Professional branding with national colors
- **Data Tables**: Paginated and sortable order lists
- **Loading States**: User feedback during data operations

### 10. **API Endpoints** (Backend)
- `GET /api/orders` - Fetch all orders
- `GET /api/inventory` - Fetch inventory data
- `GET /api/customers` - Retrieve customer information
- `GET /api/activities` - View activity logs
- `GET /api/settings` - Business settings
- `POST /api/orders` - Create new order
- `POST /api/inventory` - Add/update inventory

---

## Data Models

### Order Interface
```typescript
{
  id: string;
  customerId: string;
  customerName: string;
  weight: number;
  status: string;
  date: string;
  time: string;
  serviceType: string;
  amount: number;
  paymentStatus: string;
}
```

### Inventory Interface
```typescript
{
  id: string;
  name: string;
  quantity: number;
  unit: string;
  minStock: number;
  monthlyUsage: number;
}
```

---

## Key Features

✅ **Real-time Dashboard** - See all business metrics at a glance  
✅ **Multi-table Database** - Relational data structure for data integrity  
✅ **Customer Tracking** - Complete customer relationship management  
✅ **Inventory Optimization** - Smart stock level alerts  
✅ **Revenue Analytics** - Financial performance tracking  
✅ **Activity Auditing** - Complete transaction history  
✅ **CSV Import** - Bulk data loading capabilities  
✅ **Responsive Design** - Works on desktop and tablets  
✅ **Professional Theme** - Philippine-inspired color scheme  
✅ **Type-Safe** - Full TypeScript support for reliability  

---

## Getting Started

1. **Install Dependencies**:
   ```bash
   npm install
   ```

2. **Start Development Server**:
   ```bash
   npm run dev
   ```

3. **Login**: Use any email and password combination (demo mode)

4. **Access Dashboard**: View orders, inventory, and business analytics

---

## File Structure

- `src/app.tsx` - Main React application component
- `src/app.css` - Application styling
- `server.ts` - Express server and API endpoints
- `database/` - Database initialization and schema
- `laundry_orders.csv` - Sample order data
- `laundry_stock_inventory.csv` - Sample inventory data
- `vite.config.ts` - Vite build configuration
- `tsconfig.json` - TypeScript configuration

---

## Future Enhancements

- User role-based access control
- Advanced reporting and exports
- SMS/Email notifications
- Mobile app integration
- Payment gateway integration
- Automated billing system
- Machine learning for demand forecasting
- Real-time order tracking for customers
