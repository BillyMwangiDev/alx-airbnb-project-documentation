# Data Flow Diagram (DFD) - AirBnB Clone Backend

---

## 📋 Overview

This directory contains the Data Flow Diagram (DFD) for the AirBnB Clone backend system, illustrating how data moves through the system from inputs to processes to outputs. The DFD maps the flow of data for core operations including user management, property listings, bookings, and payments.

---

## 🎯 What is a Data Flow Diagram?

A Data Flow Diagram (DFD) is a graphical representation of data flow through a system. It shows:

- **External Entities**: Sources and destinations of data (users, external systems)
- **Processes**: Operations that transform data
- **Data Stores**: Where data is stored (databases)
- **Data Flows**: Movement of data between entities, processes, and stores

---

## 📊 DFD Components

### External Entities (Rectangles)
1. **Guest** - User who books properties
2. **Host** - User who lists properties
3. **Admin** - System administrator
4. **Payment Gateway** - External payment service (Stripe, PayPal)
5. **Email Service** - External notification service

### Processes (Circles/Rounded Rectangles)
1. **P1: User Authentication** - Handle registration and login
2. **P2: Property Management** - Manage property listings
3. **P3: Property Search** - Search and filter properties
4. **P4: Booking Management** - Handle reservations
5. **P5: Payment Processing** - Process payments and payouts
6. **P6: Review Management** - Handle reviews and ratings
7. **P7: Messaging System** - Manage user communications
8. **P8: Admin Operations** - Platform administration

### Data Stores (Parallel Lines)
1. **D1: User Database** - User accounts and profiles
2. **D2: Property Database** - Property listings
3. **D3: Booking Database** - Reservations and bookings
4. **D4: Payment Database** - Transactions and payouts
5. **D5: Review Database** - Reviews and ratings
6. **D6: Message Database** - User messages

### Data Flows (Arrows)
Labeled arrows showing data movement between components

---

## 🗺️ DFD Level 0 (Context Diagram)

High-level overview showing the system as a single process with external entities.

```
┌─────────┐                                           ┌─────────────────┐
│  Guest  │────── User Data ────────────────────────>│                 │
│         │<───── Property Info ─────────────────────│                 │
│         │<───── Booking Confirmation ──────────────│                 │
└─────────┘                                           │                 │
                                                      │   AirBnB Clone  │
┌─────────┐                                           │  Backend System │
│  Host   │────── Property Data ─────────────────────>│                 │
│         │<───── Booking Notifications ──────────────│                 │
│         │<───── Payment Info ───────────────────────│                 │
└─────────┘                                           │                 │
                                                      │                 │
┌─────────┐                                           │                 │
│  Admin  │────── Admin Commands ─────────────────────>│                 │
│         │<───── Reports & Analytics ────────────────│                 │
└─────────┘                                           └─────────────────┘
                                                              │    ^
                                                              │    │
                                            Payment Request   │    │  Payment Status
                                                              v    │
                                                      ┌─────────────────┐
                                                      │ Payment Gateway │
                                                      └─────────────────┘
                                                              │    ^
                                                              │    │
                                              Email Request   │    │  Delivery Status
                                                              v    │
                                                      ┌─────────────────┐
                                                      │  Email Service  │
                                                      └─────────────────┘
```

---

## 🔍 DFD Level 1 (Detailed Diagram)

Detailed diagram showing all major processes and data flows.

### Process Breakdown:

#### P1: User Authentication
**Inputs:**
- Registration data (Guest/Host)
- Login credentials

**Outputs:**
- User account
- JWT token
- Verification email

**Data Stores:**
- D1: User Database (read/write)

**Data Flows:**
- Guest/Host → Registration Data → P1
- P1 → User Record → D1
- P1 → Verification Email → Email Service
- Guest/Host → Login Credentials → P1
- P1 → JWT Token → Guest/Host

---

#### P2: Property Management
**Inputs:**
- Property details (from Host)
- Property updates

**Outputs:**
- Property listing
- Listing confirmation

**Data Stores:**
- D2: Property Database (read/write)
- D1: User Database (read - verify host)

**Data Flows:**
- Host → Property Data → P2
- P2 → Query Host → D1
- P2 → Property Record → D2
- P2 → Listing Confirmation → Host
- P2 → Approval Request → Admin

---

#### P3: Property Search
**Inputs:**
- Search criteria (from Guest)
- Filter parameters

**Outputs:**
- Property list
- Property details

**Data Stores:**
- D2: Property Database (read)

**Data Flows:**
- Guest → Search Query → P3
- P3 → Read Properties → D2
- D2 → Property Records → P3
- P3 → Search Results → Guest

---

#### P4: Booking Management
**Inputs:**
- Booking request (from Guest)
- Booking confirmation/cancellation

**Outputs:**
- Booking record
- Availability update
- Notifications

**Data Stores:**
- D3: Booking Database (read/write)
- D2: Property Database (read/update)
- D1: User Database (read)

**Data Flows:**
- Guest → Booking Request → P4
- P4 → Check Availability → D2
- D2 → Availability Data → P4
- P4 → Booking Record → D3
- P4 → Update Availability → D2
- P4 → Booking Details → P5 (Payment)
- P4 → Notification → Host
- P4 → Confirmation → Email Service

---

#### P5: Payment Processing
**Inputs:**
- Payment details (from Guest)
- Payout request (from Host)

**Outputs:**
- Payment confirmation
- Transaction record
- Payout

**Data Stores:**
- D4: Payment Database (read/write)
- D3: Booking Database (read/update)

**Data Flows:**
- Guest → Payment Info → P5
- P5 → Payment Request → Payment Gateway
- Payment Gateway → Payment Status → P5
- P5 → Transaction Record → D4
- P5 → Update Booking Status → D3
- P5 → Receipt → Guest
- P5 → Receipt Email → Email Service
- Host → Payout Request → P5
- P5 → Payout → Payment Gateway
- P5 → Payout Confirmation → Host

---

#### P6: Review Management
**Inputs:**
- Review data (from Guest)
- Review response (from Host)

**Outputs:**
- Review record
- Average rating update

**Data Stores:**
- D5: Review Database (read/write)
- D2: Property Database (update - rating)
- D3: Booking Database (read - verify)

**Data Flows:**
- Guest → Review Data → P6
- P6 → Verify Booking → D3
- D3 → Booking Verification → P6
- P6 → Review Record → D5
- P6 → Update Rating → D2
- P6 → Review Notification → Host
- Host → Response → P6
- P6 → Updated Review → D5

---

#### P7: Messaging System
**Inputs:**
- Message content (from Guest/Host)

**Outputs:**
- Message record
- Message notification

**Data Stores:**
- D6: Message Database (read/write)
- D1: User Database (read)

**Data Flows:**
- Guest/Host → Message → P7
- P7 → Verify Users → D1
- P7 → Message Record → D6
- D6 → Message History → P7
- P7 → Message → Recipient (Guest/Host)
- P7 → Notification → Email Service

---

#### P8: Admin Operations
**Inputs:**
- Admin commands
- Report requests

**Outputs:**
- System reports
- Analytics data
- Moderation actions

**Data Stores:**
- D1: User Database (read/write)
- D2: Property Database (read/write)
- D3: Booking Database (read/write)
- D4: Payment Database (read)
- D5: Review Database (read/write)

**Data Flows:**
- Admin → Admin Command → P8
- P8 → Query All Stores → D1, D2, D3, D4, D5
- D1-D5 → Data → P8
- P8 → Reports → Admin
- P8 → Moderation Actions → D1, D2, D5
- P8 → Analytics → Admin

---

## 🎨 Draw.io Creation Guide

### Step 1: Setup
1. Open https://app.diagrams.net/
2. Create new diagram
3. Name: `airbnb-data-flow-diagram`
4. Select blank diagram

### Step 2: Add External Entities (Rectangles)
Use **Rectangle** shapes for external entities:
- Guest (top left)
- Host (middle left)
- Admin (bottom left)
- Payment Gateway (top right)
- Email Service (bottom right)

**Style:** Blue fill (#4A90E2), white text, bold

### Step 3: Add Processes (Circles)
Use **Ellipse** or **Rounded Rectangle** shapes:
- P1: User Authentication
- P2: Property Management
- P3: Property Search
- P4: Booking Management
- P5: Payment Processing
- P6: Review Management
- P7: Messaging System
- P8: Admin Operations

**Style:** Green fill (#7ED321), white text, numbered

### Step 4: Add Data Stores (Open Rectangles)
Use **Parallel Lines** or **Open Rectangle**:
- D1: User Database
- D2: Property Database
- D3: Booking Database
- D4: Payment Database
- D5: Review Database
- D6: Message Database

**Style:** Gray fill (#8B8B8B), black text, on right side

### Step 5: Add Data Flows (Arrows)
Use **Arrows** with labels:
- From entities to processes
- From processes to data stores
- Between processes
- To external systems

**Label examples:**
- "Registration Data"
- "Property List"
- "Booking Request"
- "Payment Info"

### Step 6: Layout
```
Left Side:              Center:                    Right Side:
┌──────────┐           ┌─────────┐                ║ D1: User DB    ║
│  Guest   │──────────>│   P1    │───────────────>║                ║
└──────────┘           │  Auth   │                ║ D2: Property   ║
                       └─────────┘                ║                ║
┌──────────┐           ┌─────────┐                ║ D3: Booking    ║
│   Host   │──────────>│   P2    │───────────────>║                ║
└──────────┘           │Property │                ║ D4: Payment    ║
                       └─────────┘                ║                ║
┌──────────┐           ┌─────────┐                ║ D5: Review     ║
│  Admin   │──────────>│   P8    │───────────────>║                ║
└──────────┘           │ Admin   │                ║ D6: Message    ║
                       └─────────┘                ║                ║
```

### Step 7: Color Scheme
- **External Entities:** Blue (#4A90E2)
- **Processes:** Green (#7ED321)
- **Data Stores:** Gray (#8B8B8B)
- **Data Flows:** Black arrows with labels

### Step 8: Export
1. File → Export as → PNG
2. Settings:
   - Resolution: 300 DPI
   - Border: 20px
   - Filename: `data-flow.png`
3. Also save: `data-flow.drawio`

---

## 📋 Key Data Flows Summary

### Guest User Flows
1. **Registration:** Guest → P1 → D1 → Email Service
2. **Search:** Guest → P3 → D2 → Guest
3. **Booking:** Guest → P4 → D2, D3 → P5 → Payment Gateway
4. **Payment:** Guest → P5 → D4 → Payment Gateway → Email Service
5. **Review:** Guest → P6 → D3, D5 → D2 (update rating)

### Host User Flows
1. **List Property:** Host → P2 → D2 → Host
2. **View Bookings:** Host → P4 → D3 → Host
3. **Receive Payout:** Host → P5 → D4 → Payment Gateway → Host
4. **Respond to Review:** Host → P6 → D5 → Guest

### Admin Flows
1. **User Management:** Admin → P8 → D1 → Admin
2. **Property Moderation:** Admin → P8 → D2 → Admin
3. **Analytics:** Admin → P8 → D1-D5 → Admin

### External System Flows
1. **Payment:** P5 ↔ Payment Gateway
2. **Email:** P1, P4, P5, P7 → Email Service

---

## 🔢 DFD Notation Rules

### Naming Conventions
- **Processes:** Verb + Noun (e.g., "Process Payment")
- **Data Flows:** Noun or Noun Phrase (e.g., "User Data")
- **Data Stores:** Plural Noun (e.g., "Users", "Bookings")

### Numbering
- **Processes:** P1, P2, P3...
- **Data Stores:** D1, D2, D3...

### Arrow Direction
- **→** Single direction (one-way data flow)
- **↔** Both directions (two-way data flow)

---

## ✅ DFD Quality Checklist

Before exporting, verify:

- [ ] All 5 external entities are present
- [ ] All 8 core processes are included
- [ ] All 6 data stores are shown
- [ ] All data flows are labeled
- [ ] Arrows point in correct direction
- [ ] No process without input or output
- [ ] No data store without connection to process
- [ ] Color coding is consistent
- [ ] Layout is clear and organized
- [ ] No overlapping elements
- [ ] Legend is included
- [ ] Diagram is readable at normal size

---

## 📊 Data Flow Examples

### Example 1: Complete Booking Flow
```
Guest → [Booking Request] → P4: Booking Management
P4 → [Check Availability] → D2: Property Database
D2 → [Availability Status] → P4
P4 → [Create Booking] → D3: Booking Database
P4 → [Payment Required] → P5: Payment Processing
P5 → [Payment Request] → Payment Gateway
Payment Gateway → [Payment Confirmation] → P5
P5 → [Transaction Record] → D4: Payment Database
P5 → [Confirm Booking] → P4
P4 → [Update Status] → D3
P4 → [Confirmation] → Guest
P4 → [Notification] → Email Service
P4 → [Booking Alert] → Host
```

### Example 2: Property Listing Flow
```
Host → [Property Data] → P2: Property Management
P2 → [Verify Host] → D1: User Database
D1 → [Host Info] → P2
P2 → [Create Listing] → D2: Property Database
P2 → [Approval Request] → P8: Admin Operations
P8 → [Review Listing] → D2
P8 → [Approve/Reject] → P2
P2 → [Confirmation] → Host
P2 → [Email Notification] → Email Service
```

---

## 🔗 Related Documentation

| Document | Location | Description |
|----------|----------|-------------|
| Features | [../features-and-functionalities/](../features-and-functionalities/) | Feature specifications |
| Use Case Diagram | [../use-case-diagram/](../use-case-diagram/) | System interactions |
| User Stories | [../user-stories/](../user-stories/) | User requirements |
| Database Schema | [alx-airbnb-database](https://github.com/BillyMwangiDev/alx-airbnb-database) | Data models |

---

## 📝 Notes

- This is a **logical DFD**, showing what data flows, not how it flows
- **Physical implementation** (APIs, protocols) is separate
- Focus is on **data transformation** through processes
- External systems (Payment Gateway, Email) are shown as entities
- All major CRUD operations are represented

---

**Last Updated:** October 26, 2025  
**Version:** 1.0  
**Status:** DFD specification complete - Draw.io diagram pending

