# Auto Repair Shop Database

A fully normalized relational database system I built for managing auto repair shop operations. 
Handles customers, vehicles, employees, repair orders, parts inventory, invoicing, and payments.

![ERD](https://github.com/user-attachments/assets/f2175c16-dae8-4bb6-b3d7-a2cbad249a3c)

## Built With
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

## Features
- 13 normalized tables with primary and foreign key relationships
- 2 views for common data lookups without repeated joins
- 1 trigger that automatically recalculates the invoice total when a new part is added to a repair line item, keeping billing accurate without manual updates
- 8 business-use queries for real shop operations

## Business Rules
- Each repair order must be linked to one vehicle
- Each vehicle must belong to one customer
- A repair order must be assigned to one employee
- An invoice can only exist if a repair order is linked to it
- Each part used on a repair line item must be recorded in `partUsed` and reference the `part` table

## Queries
- Open repair orders by assigned mechanic
- Full repair history lookup by VIN
- Monthly revenue reporting
- Top 10 customers by total spend
- Most performed services
- Employee labor revenue by technician
- Parts usage and inventory tracking
- Unpaid invoice tracking

## Schema Overview
| Table | Description |
|---|---|
| `customer` | Customer contact information |
| `vehicle` | Vehicles linked to customers |
| `make` | Vehicle manufacturer |
| `model` | Vehicle model linked to make |
| `employee` | Employee details and job role |
| `job` | Job titles for employees |
| `serviceType` | Types of services the shop performs |
| `repairOrder` | Repair orders linked to vehicles and employees |
| `repairLineItem` | Individual service lines within a repair order |
| `part` | Parts inventory |
| `partUsed` | Parts used on specific repair line items |
| `invoice` | Invoices linked to repair orders |
| `payment` | Payment records linked to invoices |



## How to Run
1. Open MySQL Workbench or any MySQL client
2. Run `kkailay_3_db.sql` first to create and populate the database
3. Run `module15.sql` to create the views, trigger, and queries

## Sample Data

**customer**
| customerID | custFirstName | custLastName | phone | email |
|---|---|---|---|---|
| 1 | Aiden | Jones | 317-123-456 | ajones@gmail.com |
| 2 | John | Doe | 317-987-6543 | jdoe@gmail.com |

**vehicle**
| vehicleID | customerID | modelID | vin | year |
|---|---|---|---|---|
| 1 | 1 | 1 | 4T1BF1FK7HU123456 | 2022 |
| 2 | 2 | 2 | 1FTEW1E50JFA65432 | 2019 |

**make**
| makeID | makeName |
|---|---|
| 1 | Honda |
| 2 | Mercedes |

**model**
| modelID | makeID | modelName |
|---|---|---|
| 1 | 1 | Accord |
| 2 | 2 | G-Wagon |

**employee**
| employeeID | empFirstName | empLastName | phone | email | jobID |
|---|---|---|---|---|---|
| 1 | John | Cena | 317-456-7613 | jcena@gmail.com | 1 |
| 2 | Sidhu | Moosewala | 317-467-1278 | smoosewala@gmail.com | 2 |

**job**
| jobID | jobTitle |
|---|---|
| 1 | Mechanic |
| 2 | Service Manager |

**serviceType**
| serviceTypeID | serviceName | standardLaborHours |
|---|---|---|
| 1 | Oil Change | 1.0 |
| 2 | Brake Pad Replacement | 2.5 |

**repairOrder**
| repairOrderID | vehicleID | employeeID | dateIn | status | notes |
|---|---|---|---|---|---|
| 1 | 2 | 2 | 2025-12-10 | open | Customer needed oil change |
| 2 | 2 | 1 | 2025-12-11 | closed | Replaced front brake pads |

**repairLineItem**
| repairLineItemID | repairOrderID | serviceTypeID | laborHours | laborRate | lineNotes |
|---|---|---|---|---|---|
| 1 | 1 | 1 | 1.0 | 95 | Change oil + filter |
| 2 | 1 | 2 | 2.5 | 95 | Also wanted to change rear brake pads |
| 3 | 2 | 2 | 2.5 | 95 | Front brake pads replacement |

**part**
| partID | partName | partDescription | unitPrice |
|---|---|---|---|
| 1 | Oil Filter | Standard Oil Filter | 19.99 |
| 2 | Brake Pad Set | Ceramic Pads | 159.99 |

**partUsed**
| partUsedID | repairLineItemID | partID | quantityUsed | unitPriceAtUse |
|---|---|---|---|---|
| 1 | 1 | 1 | 1 | 19.99 |
| 2 | 2 | 2 | 1 | 159.99 |
| 3 | 3 | 2 | 1 | 159.99 |

**invoice**
| invoiceID | repairOrderID | invoiceDate | totalAmount |
|---|---|---|---|
| 1 | 1 | 2025-12-10 | 512.48 |
| 2 | 2 | 2025-12-11 | 397.49 |

**payment**
| paymentID | invoiceID | paymentDate | amount | paymentMethod |
|---|---|---|---|---|
| 1 | 1 | 2025-12-10 | 512.48 | Card |
| 2 | 2 | 2025-12-11 | 397.49 | Cash |

