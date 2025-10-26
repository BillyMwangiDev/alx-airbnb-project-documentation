# Flowcharts - AirBnB Clone Backend Processes

---

## 📋 Overview

This directory contains flowcharts that visualize the workflow and processes of key backend features in the AirBnB Clone system. Each flowchart maps the step-by-step execution flow, decision points, and outcomes for critical operations.

---

## 🎯 What is a Flowchart?

A flowchart is a diagram that represents a process, showing the steps as boxes of various kinds, and their order by connecting them with arrows. Flowcharts are used to:

- Visualize process steps
- Identify decision points
- Show alternative paths
- Document error handling
- Clarify complex workflows

---

## 📊 Flowchart Symbols

### Standard Symbols Used:

| Symbol | Shape | Usage |
|--------|-------|-------|
| **Oval** | ⬭ | Start/End points |
| **Rectangle** | ▭ | Process/Action steps |
| **Diamond** | ◇ | Decision points (Yes/No) |
| **Parallelogram** | ▱ | Input/Output operations |
| **Arrow** | → | Flow direction |
| **Rounded Rectangle** | ▢ | Predefined process/subroutine |
| **Document** | 📄 | Document/Report output |
| **Database** | 🗄️ | Database operations |

---

## 📁 Flowcharts in This Directory

| Flowchart | File | Description | Status |
|-----------|------|-------------|--------|
| **User Registration** | user-registration.png | Complete user signup process | 🔲 Pending |
| **Property Booking** | property-booking.png | End-to-end booking workflow | 🔲 Pending |
| **Payment Processing** | payment-processing.png | Payment and transaction flow | 🔲 Pending |
| **Property Listing** | property-listing.png | Host property creation flow | 🔲 Pending |

---

## 🔄 Flowchart 1: User Registration Process

### Process Overview
Complete user registration workflow from initial signup to account activation.

### Flowchart Steps:

```
START
  ↓
[User clicks "Sign Up"]
  ↓
[Display registration form]
  ↓
[User enters: First Name, Last Name, Email, Password, Role]
  ↓
<Validate input data>
  ↓ NO → [Display error message] → [Return to form]
  ↓ YES
<Check if email already exists>
  ↓ YES → [Display "Email already registered"] → [Return to form]
  ↓ NO
[Hash password using bcrypt]
  ↓
[Generate user ID (UUID)]
  ↓
[Create user record in database]
  ↓
[Set account status: "Unverified"]
  ↓
[Generate verification token]
  ↓
[Send verification email]
  ↓
[Display "Check your email" message]
  ↓
<User clicks verification link?>
  ↓ NO → [Account remains unverified] → [Send reminder after 24h]
  ↓ YES
[Verify token validity]
  ↓
<Token valid and not expired?>
  ↓ NO → [Display "Invalid or expired token"] → [Resend verification option]
  ↓ YES
[Update account status: "Active"]
  ↓
[Redirect to login page]
  ↓
[Display "Account activated successfully"]
  ↓
END
```

### Decision Points:
1. **Validate input data**: Check all required fields, email format, password strength
2. **Email exists**: Query database for existing email
3. **Token valid**: Verify token and expiration (24-hour window)

### Error Handling:
- Invalid email format → Show format error
- Weak password → Show password requirements
- Email exists → Offer login option
- Token expired → Offer resend option
- Database error → Show system error message

---

## 🏠 Flowchart 2: Property Booking Process

### Process Overview
Complete booking workflow from property selection to booking confirmation.

### Flowchart Steps:

```
START
  ↓
[Guest views property details]
  ↓
[Guest selects check-in and check-out dates]
  ↓
[Guest enters number of guests]
  ↓
<User authenticated?>
  ↓ NO → [Redirect to login] → [Return to property page after login]
  ↓ YES
[Calculate number of nights]
  ↓
[Query property availability for dates]
  ↓
<Property available?>
  ↓ NO → [Display "Property not available"] → [Suggest alternative dates] → END
  ↓ YES
[Retrieve property pricing]
  ↓
[Calculate base price (nights × nightly rate)]
  ↓
[Add cleaning fee]
  ↓
[Add service fee (10% of subtotal)]
  ↓
[Calculate taxes (varies by location)]
  ↓
[Calculate total price]
  ↓
[Display price breakdown to guest]
  ↓
<Apply promo code?>
  ↓ YES → [Validate promo code]
        ↓ Valid → [Apply discount] → [Recalculate total]
        ↓ Invalid → [Display "Invalid code"] → [Continue without discount]
  ↓ NO
[Display final total price]
  ↓
[Show cancellation policy]
  ↓
<Guest clicks "Book Now"?>
  ↓ NO → END
  ↓ YES
[Create booking record with status: "Pending"]
  ↓
[Lock dates in availability calendar (temporary hold - 15 min)]
  ↓
[Redirect to payment page]
  ↓
[Display payment form]
  ↓
[Guest enters payment details]
  ↓
<Payment method valid?>
  ↓ NO → [Display payment error] → [Allow retry] 
        ↓ Max retries exceeded? → [Cancel booking] → [Release dates] → END
  ↓ YES
[Send payment request to Payment Gateway]
  ↓
<Payment authorized?>
  ↓ NO → [Display payment failure]
        ↓
        [Log failed transaction]
        ↓
        <Retry payment?>
          ↓ YES → [Return to payment page]
          ↓ NO → [Cancel booking] → [Release dates] → END
  ↓ YES
[Capture payment]
  ↓
[Update booking status: "Confirmed"]
  ↓
[Permanently block dates in calendar]
  ↓
[Create payment record]
  ↓
[Generate booking confirmation number]
  ↓
[Send confirmation email to guest]
  ↓
[Send booking notification to host]
  ↓
[Display confirmation page with booking details]
  ↓
[Trigger post-booking processes:]
  - Add booking to guest's history
  - Add booking to host's calendar
  - Schedule reminder emails
  - Update property statistics
  ↓
END
```

### Decision Points:
1. **User authenticated**: Check if guest is logged in
2. **Property available**: Query availability for selected dates
3. **Apply promo code**: Validate and apply discount if provided
4. **Guest confirms booking**: User explicitly confirms reservation
5. **Payment valid**: Validate payment method details
6. **Payment authorized**: Payment gateway authorization success
7. **Retry payment**: Guest chooses to retry failed payment

### Error Handling:
- Property not available → Suggest alternatives
- User not authenticated → Redirect to login
- Invalid promo code → Continue without discount
- Payment validation fails → Show specific error
- Payment authorization fails → Allow retry with different method
- Database error → Show system error, release temporary hold
- Network timeout → Retry payment capture, manual reconciliation

### Time Constraints:
- Temporary date lock: 15 minutes
- Payment authorization timeout: 3 minutes
- Maximum payment retries: 3 attempts

---

## 💳 Flowchart 3: Payment Processing

### Process Overview
Payment authorization and capture workflow.

### Flowchart Steps:

```
START
  ↓
[Receive payment request from booking system]
  ↓
[Retrieve booking details and amount]
  ↓
<Payment method type?>
  ├─ Credit Card → [Validate card format]
  ├─ PayPal → [Initiate PayPal OAuth]
  └─ Stripe → [Validate Stripe token]
  ↓
[Prepare payment request payload]
  ↓
[Log payment attempt]
  ↓
[Send authorization request to Payment Gateway]
  ↓
<Gateway response received?>
  ↓ NO (Timeout) → [Retry 3 times]
        ↓ Still failing → [Mark as "Payment Timeout"] → [Notify admin] → END
  ↓ YES
<Payment authorized?>
  ↓ NO → [Parse error code]
        ↓
        <Error type?>
          ├─ Insufficient funds → [Notify user: Insufficient funds]
          ├─ Invalid card → [Notify user: Invalid card details]
          ├─ Card declined → [Notify user: Card declined by bank]
          ├─ Fraud detected → [Flag transaction] → [Block payment] → [Notify security team]
          └─ Other → [Generic error message]
        ↓
        [Create failed transaction record]
        ↓
        [Return error to booking system]
        ↓
        END
  ↓ YES (Authorized)
[Hold authorized amount]
  ↓
[Create authorization record]
  ↓
<Capture immediately or later?>
  ├─ Immediate (Direct booking) → [Proceed to capture]
  └─ Later (Request booking) → [Wait for host approval] → [Capture on approval]
  ↓
[Send capture request to Payment Gateway]
  ↓
<Capture successful?>
  ↓ NO → [Retry capture 3 times]
        ↓ Still failing → [Manual reconciliation required] → [Alert finance team] → END
  ↓ YES
[Deduct platform service fee (10-15%)]
  ↓
[Calculate host payout amount]
  ↓
[Create transaction record with status: "Completed"]
  ↓
[Update booking payment status: "Paid"]
  ↓
[Queue host payout (released after check-in)]
  ↓
[Generate receipt]
  ↓
[Send receipt to guest via email]
  ↓
[Send payment confirmation to host]
  ↓
[Update payment analytics]
  ↓
[Log successful transaction]
  ↓
[Return success to booking system]
  ↓
END
```

### Decision Points:
1. **Payment method type**: Route to appropriate gateway handler
2. **Gateway response**: Check for timeout or response
3. **Payment authorized**: Validate authorization success
4. **Error type**: Categorize specific payment failure
5. **Capture timing**: Immediate or after host approval
6. **Capture successful**: Verify funds captured

### Error Handling:
- Gateway timeout → Retry mechanism (3 attempts)
- Authorization failed → Specific error messages per reason
- Capture failed → Retry + manual reconciliation fallback
- Fraud detected → Block and flag for review
- Network errors → Queue for retry

---

## 🏡 Flowchart 4: Property Listing Creation

### Process Overview
Host creates a new property listing workflow.

### Flowchart Steps:

```
START
  ↓
<User authenticated as Host?>
  ↓ NO → [Redirect to login] → [Return after authentication]
  ↓ YES
[Display "Create Listing" form - Step 1: Basic Info]
  ↓
[Host enters: Property name, Type, Address, City, Country]
  ↓
[Host specifies: Guests capacity, Bedrooms, Beds, Bathrooms]
  ↓
<Validate basic info?>
  ↓ NO → [Highlight errors] → [Return to form]
  ↓ YES
[Save progress as draft]
  ↓
[Move to Step 2: Description & Amenities]
  ↓
[Host writes property description]
  ↓
[Host selects amenities from checklist:]
  - WiFi, Kitchen, Parking, Pool, etc.
  ↓
[Host sets house rules]
  ↓
<Description adequate? (min 50 characters)>
  ↓ NO → [Prompt to add more details]
  ↓ YES
[Save progress]
  ↓
[Move to Step 3: Photos]
  ↓
[Host uploads photos (min 3, max 20)]
  ↓
<Minimum photos uploaded?>
  ↓ NO → [Display "Please upload at least 3 photos"]
  ↓ YES
[Validate image formats and sizes]
  ↓
<All images valid?>
  ↓ NO → [Show invalid images] → [Allow re-upload]
  ↓ YES
[Upload images to cloud storage (S3/Cloudinary)]
  ↓
[Generate image URLs]
  ↓
[Host sets primary photo]
  ↓
[Save progress]
  ↓
[Move to Step 4: Pricing]
  ↓
[Host sets nightly rate]
  ↓
[Host sets cleaning fee (optional)]
  ↓
[Host sets minimum/maximum stay nights]
  ↓
<Pricing reasonable? (validate min/max)>
  ↓ NO → [Show validation error]
  ↓ YES
[Calculate platform service fee preview]
  ↓
[Display estimated earnings per booking]
  ↓
[Save progress]
  ↓
[Move to Step 5: Availability & Policies]
  ↓
[Host sets available dates on calendar]
  ↓
[Host selects cancellation policy:]
  - Flexible, Moderate, Strict, or Non-refundable
  ↓
[Host sets check-in time window]
  ↓
[Host sets check-out time]
  ↓
[Save progress]
  ↓
[Move to Step 6: Review & Submit]
  ↓
[Display complete listing preview]
  ↓
<Host reviews all information?>
  ↓ Edit needed → [Return to specific section] → [Make changes] → [Return to review]
  ↓ Looks good
[Host checks terms and conditions]
  ↓
<Terms accepted?>
  ↓ NO → END
  ↓ YES
[Generate unique property ID (UUID)]
  ↓
[Create property record in database with status: "Pending Review"]
  ↓
[Link property to host account]
  ↓
[Send listing to admin moderation queue]
  ↓
[Display "Listing submitted for review" message]
  ↓
[Send confirmation email to host]
  ↓
[Notify admin team of new listing]
  ↓
--- Admin Review Process ---
  ↓
<Admin reviews listing?>
  ↓
  <Listing compliant with policies?>
    ├─ NO → [Request modifications] → [Notify host] → [Host edits] → [Resubmit]
    └─ YES → [Approve listing]
              ↓
              [Update status: "Active"]
              ↓
              [Make listing visible in search results]
              ↓
              [Send approval email to host]
              ↓
              [Add listing to public database]
              ↓
              [Index for search optimization]
              ↓
END
```

### Decision Points:
1. **User authenticated as host**: Verify host role
2. **Validate basic info**: Check all required fields
3. **Description adequate**: Minimum character count met
4. **Minimum photos**: At least 3 photos uploaded
5. **All images valid**: Format (JPG/PNG) and size (< 5MB) checks
6. **Pricing reasonable**: Within platform min/max limits
7. **Terms accepted**: Host agrees to hosting terms
8. **Admin approval**: Listing meets quality standards

### Error Handling:
- Missing required fields → Highlight and prompt
- Invalid image format → Show error, allow re-upload
- Image upload fails → Retry mechanism
- Pricing out of range → Show acceptable range
- Terms not accepted → Cannot proceed
- Admin rejection → Detailed feedback provided

---

## 🎨 Draw.io Creation Guidelines

### General Setup
1. Open https://app.diagrams.net/
2. Create new blank diagram
3. Name appropriately (e.g., `property-booking-flowchart`)
4. Set page size: A4 Portrait (for most) or A3 Landscape (for complex flows)

### Color Scheme
- **Start/End (Oval):** Green (#7ED321) for Start, Red (#D0021B) for End
- **Process (Rectangle):** Blue (#4A90E2)
- **Decision (Diamond):** Orange (#F5A623)
- **Input/Output (Parallelogram):** Purple (#9013FE)
- **Database (Cylinder):** Gray (#8B8B8B)
- **Document (Document shape):** Light blue (#50E3C2)

### Best Practices
1. **Flow Direction:** Top to bottom, left to right
2. **Spacing:** Consistent spacing between elements (40px)
3. **Label Clarity:** Short, clear labels (< 5 words per box)
4. **Decision Labels:** Always label outgoing arrows (Yes/No, True/False)
5. **Alignment:** Use grid snapping for neat alignment
6. **Connectors:** Use straight or rounded connectors consistently
7. **Swim Lanes:** Use for multi-actor processes (optional)

### Size Guidelines
- **Start/End Ovals:** 100x50px
- **Process Rectangles:** 150x60px
- **Decision Diamonds:** 120x120px
- **Input/Output:** 150x60px
- **Text:** 11pt Arial or Helvetica

### Export Settings
1. File → Export as → PNG
2. Resolution: 300 DPI
3. Border: 10px
4. Transparent background: No
5. Include grid: No

---

## 📋 Flowchart Checklist

Before exporting, verify:

- [ ] All start points have oval shapes
- [ ] All end points have oval shapes
- [ ] All decision points are diamonds with Yes/No labels
- [ ] All arrows have clear direction
- [ ] All processes have descriptive labels
- [ ] Error paths are included
- [ ] Happy path is clearly visible
- [ ] No orphaned elements
- [ ] Consistent color scheme
- [ ] Proper alignment and spacing
- [ ] Legend is included (if using custom symbols)
- [ ] Flowchart fits on page without scrolling
- [ ] Text is readable at normal zoom

---

## 🔗 Process Integration

### How These Processes Connect:

1. **User Registration** → Enables access to all other features
2. **Property Listing** → Creates properties available for booking
3. **Property Booking** → Triggers payment processing
4. **Payment Processing** → Completes booking, triggers payouts

### Shared Components:
- User authentication (all processes)
- Database operations (all processes)
- Email notifications (all processes)
- Error handling patterns (consistent across all)

---

## 📚 Related Documentation

| Document | Location | Description |
|----------|----------|-------------|
| Features | [../features-and-functionalities/](../features-and-functionalities/) | Complete feature specs |
| Use Case Diagram | [../use-case-diagram/](../use-case-diagram/) | System interactions |
| User Stories | [../user-stories/](../user-stories/) | User requirements |
| Data Flow Diagram | [../data-flow-diagram/](../data-flow-diagram/) | System data flows |
| Database Schema | [alx-airbnb-database](https://github.com/BillyMwangiDev/alx-airbnb-database) | Data models |

---

## 🚀 Implementation Notes

When implementing these processes:

1. **Transaction Management:** Use database transactions for multi-step processes
2. **Async Operations:** Handle email sending and payment processing asynchronously
3. **Retry Logic:** Implement exponential backoff for external API calls
4. **Logging:** Log all decision points and state changes
5. **Monitoring:** Set up alerts for failed payments, timeouts
6. **Testing:** Test all paths including error scenarios
7. **Performance:** Optimize database queries, cache where appropriate

---

**Last Updated:** October 26, 2025  
**Version:** 1.0  
**Status:** Flowchart specifications complete - Draw.io diagrams pending

