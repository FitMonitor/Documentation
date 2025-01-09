# API Documentation

## Gym Endpoints

### **GET** `/api/gyms`
Retrieves a gym by its ID.

**Parameters**:  
- **id**: ID of the gym to retrieve (passed as a query parameter).

**Responses**:  
- **200 OK**: Gym found and returned successfully.  
- **404 Not Found**: No gym found for the provided ID.

---

### **GET** `/api/gyms/occupancy`
Retrieves the current occupancy of a gym by its ID.

**Parameters**:  
- **id**: ID of the gym to retrieve occupancy information for (passed as a query parameter).

**Responses**:  
- **200 OK**: Gym occupancy retrieved successfully.  
- **404 Not Found**: No gym found for the provided ID.

---

## QR Code Endpoints

### **POST** `/api/qrcode/validate`
Validates a QR Code token.

**Request Body**:  
- **qrToken**: The QR Code token to validate (as a JSON key-value pair).

**Responses**:  
- **200 OK**: QR Code token is valid, returns user details (`username`, `roles`, `expiration`).  
- **401 Unauthorized**: Invalid or expired QR Code token.

---

### **POST** `/api/qrcode/generate`
Generates a QR Code for the provided token.

**Request Body**:  
- **token**: The token to encode in the QR Code (as a JSON key-value pair).

**Responses**:  
- **200 OK**: QR Code generated and returned as a PNG image.  
- **401 Unauthorized**: Invalid or expired token.

---

### **POST** `/api/qrcode/generate-machine`
Generates a QR Code for a machine by its ID.

**Request Body**:  
- **machineId**: The ID of the machine to generate a QR Code for (as a JSON key-value pair).

**Responses**:  
- **200 OK**: Machine QR Code generated and returned as a PNG image.  
- **400 Bad Request**: Invalid request (e.g., `machineId` is null or empty).  
- **500 Internal Server Error**: Failed to generate QR Code.

---

### **POST** `/api/qr/machine`
Changes the state of a machine.

**Request Body**:  
- **machineId**: The ID of the machine to operate on.  
- **intention**: The desired state change for the machine.

**Authentication**:  
Requires the authenticated user's `usersub`.

**Responses**:  
- **200 OK**:  
  - "True": The machine state changed successfully.  
  - "User not in gym": The user is not currently in the gym.  
  - "User already using a machine": The user is already using another machine.  
  - "False": The state change was unsuccessful.

---

### **POST** `/api/qr/gym_entrance`
Processes a gym entrance request.

**Request Body**:  
- **token**: The token to validate and process the gym entrance.

**Authentication**:  
Requires the authenticated user's `usersub`.

**Responses**:  
- **200 OK**:  
  - "Left": User successfully left the gym.  
  - "Full": The gym is at maximum capacity.  
  - "Entered": User successfully entered the gym.

---

## Payment Endpoints

### **POST** `/api/payment/create-checkout-session`
Creates a checkout session.

**Request Body**:  
- **amount**: The amount to be paid (in smallest currency units).  
- **currency**: The currency for the payment (e.g., `usd`).  
- **successUrl**: The URL to redirect the user upon successful payment.  
- **cancelUrl**: The URL to redirect the user upon payment cancellation.

**Authentication**:  
Requires the authenticated user's `usersub`.

**Responses**:  
- **200 OK**: Checkout session created successfully, returns the session URL.  
- **400 Bad Request**: Error creating the checkout session.

---

### **GET** `/api/payment/user/subscriptiondate`
Retrieves the subscription date of the user.

**Authentication**:  
Requires the authenticated user's `usersub`.

**Responses**:  
- **200 OK**: Subscription date retrieved successfully.  
- **404 Not Found**: No subscription found for the user.

---

## Webhook Endpoints

### **POST** `/api/webhooks`
Handles Stripe webhook events.

**Request Body**:  
- **payload**: The payload of the Stripe event.  
- **Stripe-Signature**: The Stripe signature header.

**Responses**:  
- **200 OK**: Webhook event handled successfully.  
- **400 Bad Request**: Error handling the webhook event.

**Event Types Handled**:  
- **checkout.session.completed**: Handles successful payment sessions.

---

## Service Methods

### GymService

#### **createGym(String gymName, int capacity)**
Creates a new gym.

**Parameters**:  
- **gymName**: The name of the gym to create.  
- **capacity**: The maximum capacity of the gym.

---

#### **getGymByID(Long id)**
Retrieves a gym by its ID.

**Parameters**:  
- **id**: The ID of the gym to retrieve.

---

#### **getOccupancy(Long gymId)**
Retrieves the current occupancy of a gym.

**Parameters**:  
- **gymId**: The ID of the gym.

---

### QRCodeService

#### **generateQRCode(String content, int width, int height)**
Generates a QR Code image for the provided content.

**Parameters**:  
- **content**: The content to encode in the QR Code.  
- **width**: The width of the QR Code image.  
- **height**: The height of the QR Code image.

---

### PaymentsService

#### **getPayment(String userSub)**
Retrieves the payment record for a user.

**Parameters**:  
- **userSub**: The unique identifier of the user.

---

#### **savePayment(Payments payment)**
Saves a new payment record.

**Parameters**:  
- **payment**: The payment object to save.

---

#### **updatePayment(String userSub, Date subscriptionDate)**
Updates a payment record with a new subscription date.

**Parameters**:  
- **userSub**: The unique identifier of the user.  
- **subscriptionDate**: The new subscription date.

---

### StripeService

#### **createCheckoutSession(Long amount, String currency, String successUrl, String cancelUrl, String userSub)**
Creates a Stripe checkout session.

**Parameters**:  
- **amount**: The payment amount.  
- **currency**: The payment currency.  
- **successUrl**: The success redirect URL.  
- **cancelUrl**: The cancel redirect URL.  
- **userSub**: The user's identifier.

---

### StripeWebhookService

#### **handlePaymentSucceededEvent(String event)**
Processes a successful payment event.

**Parameters**:  
- **event**: The JSON string representing the Stripe event.

---

#### **increaseSubscription(String userSub, int days)**
Increases the user's subscription by a number of days.

**Parameters**:  
- **userSub**: The user's identifier.  
- **days**: The number of days to add.

---

#### **getSubscriptionDaysByAmount(String amount)**
Determines the number of subscription days based on the payment amount.

**Parameters**:  
- **amount**: The payment amount.


