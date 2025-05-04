# Coffee Corp CFCMS Data Models Documentation

This document provides a comprehensive overview of the data models implemented in the Coffee Corp Content Management System (CFCMS). These models form the foundation of the procurement, inventory, and vendor management systems within the application.

## Table of Contents

1. [Procurement Request](#procurement-request)
2. [Quotation](#quotation)
3. [Purchase Order](#purchase-order)
4. [Vendor](#vendor)
5. [Warehouse](#warehouse)
6. [Inventory Item](#inventory-item)
7. [Stock Movement](#stock-movement)
8. [Data Flow and Relationships](#data-flow-and-relationships)

---

## Procurement Request

The Procurement Request model represents a formal request for goods or services initiated by a cooperative member.

### Key Fields

| Field | Type | Description |
|-------|------|-------------|
| requestNumber | string | Unique identifier for the procurement request |
| title | string | Brief title describing the request |
| description | string | Detailed description of the request |
| status | enum | Current status (PENDING, APPROVED, REJECTED, CANCELLED, COMPLETED) |
| requestDate | date | Date when the request was created |
| requiredByDate | date | Date by which the requested items are needed |
| itemName | string | Name of the requested item |
| itemCategory | string | Category of the requested item |
| quantity | number | Quantity of items requested |
| unit | string | Unit of measurement (e.g., kg, liters, pieces) |
| estimatedUnitPrice | number | Estimated price per unit |
| estimatedTotalPrice | number | Total estimated cost (quantity × estimatedUnitPrice) |
| justification | string | Reason for the procurement request |

### Relationships

- **Cooperative**: The cooperative that initiated the request
- **Requester**: The user who created the request
- **Approver**: The user who approved or rejected the request

### Status Flow

1. **PENDING**: Initial state when request is created
2. **APPROVED**: When authorized by an approver
3. **REJECTED**: When declined by an approver
4. **CANCELLED**: When withdrawn by the requester
5. **COMPLETED**: When the procurement process is finalized

---

## Quotation

The Quotation model represents a formal price offer from a vendor in response to a procurement request.

### Key Fields

| Field | Type | Description |
|-------|------|-------------|
| quotationNumber | string | Unique identifier for the quotation |
| title | string | Brief title describing the quotation |
| status | enum | Current status (PENDING, ACCEPTED, REJECTED, EXPIRED) |
| quotationDate | date | Date when the quotation was created |
| validUntilDate | date | Expiration date of the quotation |
| unitPrice | number | Price per unit offered by the vendor |
| quantity | number | Quantity of items quoted |
| unit | string | Unit of measurement |
| subtotal | number | Base cost before taxes and other charges |
| taxAmount | number | Amount of tax |
| shippingAmount | number | Cost of shipping |
| discountAmount | number | Applied discount |
| totalAmount | number | Final cost including all charges and discounts |
| estimatedDeliveryDate | date | Expected delivery date if accepted |
| deliveryTerms | string | Terms of delivery |
| paymentTerms | string | Terms of payment |

### Relationships

- **Procurement Request**: The request this quotation is responding to
- **Vendor**: The vendor providing the quotation
- **Cooperative**: The cooperative receiving the quotation
- **Created By**: The user who recorded the quotation
- **Approver**: The user who accepted or rejected the quotation

### Status Flow

1. **PENDING**: Initial state when quotation is received
2. **ACCEPTED**: When approved by the cooperative
3. **REJECTED**: When declined by the cooperative
4. **EXPIRED**: When the validUntilDate has passed

---

## Purchase Order

The Purchase Order model represents a formal document issued to a vendor to purchase goods or services.

### Key Fields

| Field | Type | Description |
|-------|------|-------------|
| orderNumber | string | Unique identifier for the purchase order |
| title | string | Brief title describing the order |
| status | enum | Current status (PENDING, CONFIRMED, SHIPPED, DELIVERED, CANCELLED) |
| orderDate | date | Date when the order was created |
| expectedDeliveryDate | date | Expected date of delivery |
| itemName | string | Name of the ordered item |
| itemCategory | string | Category of the ordered item |
| quantity | number | Quantity of items ordered |
| unit | string | Unit of measurement |
| unitPrice | number | Price per unit |
| subtotal | number | Base cost before taxes and other charges |
| taxAmount | number | Amount of tax |
| shippingAmount | number | Cost of shipping |
| discountAmount | number | Applied discount |
| totalAmount | number | Final cost including all charges and discounts |
| paymentStatus | enum | Current payment status (UNPAID, PARTIALLY_PAID, PAID) |
| paymentDueDate | date | Date by which payment is due |
| amountPaid | number | Amount already paid |
| deliveryAddress | string | Address for delivery |
| trackingNumber | string | Shipment tracking number |

### Relationships

- **Procurement Request**: The original request that initiated this order
- **Quotation**: The accepted quotation this order is based on
- **Vendor**: The vendor fulfilling the order
- **Cooperative**: The cooperative placing the order
- **Created By**: The user who created the purchase order

### Status Flow

1. **PENDING**: Initial state when order is created
2. **CONFIRMED**: When acknowledged by the vendor
3. **SHIPPED**: When the order has been dispatched
4. **DELIVERED**: When the order has been received
5. **CANCELLED**: When the order is terminated before completion

### Payment Status

1. **UNPAID**: No payment has been made
2. **PARTIALLY_PAID**: Some payment has been made
3. **PAID**: Full payment has been completed

---

## Vendor

The Vendor model represents individuals or businesses that supply goods or services to cooperatives.

### Key Fields

| Field | Type | Description |
|-------|------|-------------|
| name | string | Name of the vendor |
| type | enum | Type of vendor (INDIVIDUAL, BUSINESS, COOPERATIVE) |
| contactPerson | string | Primary contact person |
| email | string | Contact email address |
| phoneNumber | string | Primary phone number |
| address | string | Physical address |
| city | string | City location |
| region | string | Region/state/province |
| country | string | Country |
| businessName | string | Registered business name (if applicable) |
| taxId | string | Tax identification number |
| registrationNumber | string | Business registration number |
| businessType | string | Type of business entity |
| productCategories | string | Categories of products/services offered |
| isActive | boolean | Whether the vendor is currently active |
| isVerified | boolean | Whether the vendor's credentials have been verified |
| rating | number | Performance rating (1-5 scale) |
| paymentTerms | string | Preferred payment terms |
| bankName | string | Bank name for payments |
| bankAccountNumber | string | Bank account number |
| mobileMoney | string | Mobile money account details |

### Relationships

- **Primary Cooperative**: The main cooperative this vendor works with
- **Cooperatives**: All cooperatives this vendor is associated with

---

## Warehouse

The Warehouse model represents physical storage facilities used by cooperatives to store inventory.

### Key Fields

| Field | Type | Description |
|-------|------|-------------|
| name | string | Name of the warehouse |
| code | string | Unique code identifier |
| description | string | Detailed description |
| address | string | Physical address |
| city | string | City location |
| region | string | Region/state/province |
| country | string | Country |
| latitude | number | Geographic latitude |
| longitude | number | Geographic longitude |
| totalCapacity | number | Total storage capacity |
| usedCapacity | number | Currently used capacity |
| capacityUnit | string | Unit of capacity measurement |
| contactPerson | string | Warehouse manager or contact |
| contactPhone | string | Contact phone number |
| contactEmail | string | Contact email address |
| isActive | boolean | Whether the warehouse is currently operational |

### Relationships

- **Cooperative**: The cooperative that owns or operates the warehouse

---

## Inventory Item

The Inventory Item model represents goods or materials stored in warehouses.

### Key Fields

| Field | Type | Description |
|-------|------|-------------|
| name | string | Name of the item |
| sku | string | Stock Keeping Unit identifier |
| description | string | Detailed description |
| category | enum | Category (CROP, FERTILIZER, SEED, EQUIPMENT, PACKAGING, OTHER) |
| type | string | Specific type within the category |
| quantity | number | Current quantity in stock |
| unit | string | Unit of measurement |
| minimumStockLevel | number | Threshold for low stock warning |
| reorderPoint | number | Quantity at which reordering is recommended |
| batchNumber | string | Production batch identifier |
| expiryDate | date | Date when the item expires |
| productionDate | date | Date when the item was produced |
| receivedDate | date | Date when the item was received into inventory |
| unitCost | number | Cost per unit |
| totalCost | number | Total cost of inventory (quantity × unitCost) |
| quality | string | Quality description |
| grade | string | Quality grade |
| condition | enum | Physical condition (GOOD, DAMAGED, EXPIRED) |
| source | enum | Source of the item (FARMER_DELIVERY, PURCHASE, TRANSFER) |
| isActive | boolean | Whether the item is active in inventory |

### Relationships

- **Warehouse**: The warehouse where the item is stored
- **Cooperative**: The cooperative that owns the item

---

## Stock Movement

The Stock Movement model tracks changes in inventory levels, including additions, removals, and transfers.

### Key Fields

| Field | Type | Description |
|-------|------|-------------|
| referenceNumber | string | Unique identifier for the movement |
| type | enum | Type of movement (ADDITION, REMOVAL, TRANSFER, ADJUSTMENT) |
| quantity | number | Quantity moved |
| unit | string | Unit of measurement |
| movementDate | date | Date when the movement occurred |
| source | enum | Source of movement (FARMER_DELIVERY, PURCHASE, TRANSFER, SALE, LOSS, ADJUSTMENT) |
| sourceReference | string | Reference to source document (e.g., purchase order number) |
| unitCost | number | Cost per unit |
| totalCost | number | Total cost of moved items |
| reason | string | Reason for the movement |

### Relationships

- **Inventory Item**: The item being moved
- **Cooperative**: The cooperative performing the movement
- **Created By**: The user who recorded the movement
- **Source Warehouse**: The warehouse items are moving from (for transfers)
- **Destination Warehouse**: The warehouse items are moving to (for transfers)

---

## Data Flow and Relationships

The procurement and inventory management system follows this general workflow:

1. A **Procurement Request** is created when a cooperative needs to purchase goods
2. Vendors provide **Quotations** in response to the procurement request
3. When a quotation is accepted, a **Purchase Order** is created
4. When goods are received, they are added to the **Inventory** in a specific **Warehouse**
5. **Stock Movements** track all changes to inventory levels

### Key Relationships Diagram

```
Procurement Request → Quotation → Purchase Order → Inventory Item
                                                    ↑
                                                    |
                                  Stock Movement ───┘
                                    ↑     ↑
                                    |     |
                              Warehouse──┘
```

All entities are associated with a **Cooperative**, which represents the organization managing the procurement and inventory processes.

---

This documentation provides an overview of the data models implemented in the Coffee Corp CFCMS. These models are designed to support comprehensive procurement, inventory, and vendor management processes while maintaining data integrity and traceability throughout the system.
