
Node.js (v14 or higher)
npm (Node Package Manager)
Step 1: Clone the Repository
git clone <repository-url>
cd <project-directory>
Step 2: Install Dependencies
Install all required dependencies by running:
npm install

Step 3: Run the Application
Start the server using:
npm start
The application will run on port 3000 or a custom port defined in the PORT environment variable

Key Features:
Authentication: Login via JWT token
Transaction Management:
Fetch all transactions
Filter transactions by school
Get transaction status
Update transaction status (manual or via webhook)
Payment Initiation: Create payments and generate payment URLs.
API Documentation
Authentication
Login
Endpoint: POST /api/auth/login
Request Body:
json
{
  "username": "admin",
  "password": "password"
}
Response:
json
{
  "token": "<JWT_TOKEN>"
}
Use the token returned for authenticated routes.
Transaction Routes
Get All Transactions
Endpoint: GET /api/transactions
Authorization: Requires JWT token in the Authorization header.
Response: A list of all transactions with details like collect_id, school_id, gateway, order_amount, etc.
Get Transactions by School
Endpoint: GET /api/transactions/school/:schoolId
Authorization: Requires JWT token in the Authorization header.
Response: List of transactions filtered by the school ID.
Get Transaction Status
Endpoint: GET /api/transactions/status/:customOrderId
Authorization: Requires JWT token in the Authorization header.
Response: Status of the transaction with details like status, transaction_amount, gateway, bank_reference.
Webhook for Status Updates
Endpoint: POST /api/transactions/webhook
Request Body:
json
{
  "status": "SUCCESS",
  "order_info": {
    "order_id": "COLL-123456",
    "order_amount": 100,
    "transaction_amount": 100,
    "gateway": "Stripe",
    "bank_reference": "REF12345"
  }
}
Response: Confirms the status has been updated in the database.
Manual Status Update
Endpoint: POST /api/transactions/status/update
Authorization: Requires JWT token in the Authorization header.
Request Body:
json
{
  "collect_id": "COLL-123456",
  "status": "SUCCESS",
  "transaction_amount": 100,
  "gateway": "Stripe",
  "bank_reference": "REF12345"
}
Response: Confirms the status has been manually updated.
Create Payment
Endpoint: POST /api/transactions/create-payment
Authorization: Requires JWT token in the Authorization header.
Request Body:
json
{
  "school_id": "SCHOOL123",
  "amount": 100,
  "gateway": "Stripe"
}
Response:
Generates a new payment, creates a collect_request and transaction_status record, and returns a payment URL.
json

{
  "message": "Payment initiated",
  "custom_order_id": "PAY-1672550900000",
  "payment_url": "https://payment-gateway.example.com/PAY-1672550900000"
}

npm start
The server will run on port 3000 by default.

project frontend 
READM

Features
Transactions Overview: Displays a list of all transactions.
School Transactions: Displays transactions filtered by school.
Transaction Status Check: Allows users to check the status of a transaction using a custom order ID.
Manual Status Update: Allows users to manually update the status of a transaction.
Webhook Logs: Displays logs for webhook status updates.
Dark Mode: Users can toggle between dark and light modes for the interface.
Prerequisites
Before you begin, ensure you have the following installed:

Node.js (v14 or higher)
npm (Node Package Manager)
React Router for routing
Project Setup
1. Clone the Repository

git clone <repository-url>
cd <project-directory>
2. Install Dependencies
Run the following command to install the necessary dependencies:
npm install
3. Run the Application
Once dependencies are installed, you can run the app locally using:
npm start
The application will be available at http://localhost:3000.

File Structure
css
Components
1. TransactionsOverview
Displays an overview of all transactions.
Users can view transaction details like school_id, gateway, and order_amount.
2. SchoolTransactions
Displays transactions filtered by the school ID.
Users can select a school to view transactions related to that specific school.
3. TransactionStatusCheck
Allows users to check the status of a transaction using a custom order ID.
Displays information such as status, transaction_amount, and bank_reference.
4. ManualStatusUpdate
Provides a form to manually update the status of a transaction.
Users can update the status of a transaction and save it.
5. WebhookHandler
Displays logs for webhook updates, which are used for automatic transaction status updates.
Provides insights into the status of the webhook operations.
Navigation
The app uses React Router for page navigation. The following routes are available:

Transactions Overview: / (Main Dashboard)
School Transactions: /school-transactions
Transaction Status Check: /status-check
Manual Status Update: /manual-update
Webhook Logs: /webhook-logs
Dark Mode
To toggle dark mode, click on the 🌙 (for dark mode) or ☀️ (for light mode) button in the navbar.



