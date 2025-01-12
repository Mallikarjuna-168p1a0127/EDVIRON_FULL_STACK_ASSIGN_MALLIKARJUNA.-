Project Setup Instructions
Prerequisites:
Before you begin, ensure you have the following installed:

Node.js (v14 or higher)
npm (Node Package Manager)
Step 1: Clone the Repository
bash
Copy code
git clone <repository-url>
cd <project-directory>
Step 2: Install Dependencies
Install all required dependencies by running:

bash
Copy code
npm install
Step 3: Run the Application
Start the server using:

bash
Copy code
npm start
The application will run on port 3000 or a custom port defined in the PORT environment variable.

Application Overview
This backend application provides an API for managing and processing transaction data. It includes authentication via JWT, functionality for managing collect requests, transaction statuses, and webhooks for status updates.

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
Copy code
{
  "username": "admin",
  "password": "password"
}
Response:
json
Copy code
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
Copy code
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
Copy code
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
Copy code
{
  "school_id": "SCHOOL123",
  "amount": 100,
  "gateway": "Stripe"
}
Response:
Generates a new payment, creates a collect_request and transaction_status record, and returns a payment URL.
json
Copy code
{
  "message": "Payment initiated",
  "custom_order_id": "PAY-1672550900000",
  "payment_url": "https://payment-gateway.example.com/PAY-1672550900000"
}
Database Schema
collect_requests:

_id: Unique identifier for the collect request.
school_id: The ID of the school associated with the transaction.
trustee_id: The ID of the trustee responsible for the transaction.
gateway: The payment gateway used (e.g., Stripe, PayPal).
order_amount: The original order amount.
custom_order_id: Unique identifier for the payment/order.
transaction_status:

_id: Unique identifier for the transaction status.
collect_id: Reference to the collect_requests table.
status: Transaction status (PENDING, SUCCESS, FAILURE).
payment_method: Payment method used (e.g., Credit Card, Bank Transfer).
gateway: Payment gateway used for the transaction.
transaction_amount: The actual transaction amount.
bank_reference: Reference from the bank/payment gateway.
Data Import
The application imports sample data from CSV files:

test.collect_request.csv: Contains sample collect request data.
test.collect_request_status.csv: Contains sample transaction status data.
Ensure these CSV files are available in the project directory, or update the import paths accordingly.

Error Handling
The application has robust error handling. If any unexpected error occurs, the server will return a 500 status with the following structure:

json
Copy code
{
  "error": "Something went wrong!"
}
Running the Application Locally
Set Up the Environment: If you need a specific environment configuration, create a .env file in the root of the project and define your variables (e.g., PORT, JWT_SECRET).

Start the Server: Use the following command to start the server:

bash
Copy code
npm start
The server will run on port 3000 by default.

project frontend 
README
Project Overview
This project is a School Payment Dashboard built using React. It provides an interface for viewing transactions, checking transaction statuses, manually updating transaction statuses, and viewing webhook logs. Additionally, it supports a dark mode feature for better user experience.

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
bash
Copy code
git clone <repository-url>
cd <project-directory>
2. Install Dependencies
Run the following command to install the necessary dependencies:

bash
Copy code
npm install
3. Run the Application
Once dependencies are installed, you can run the app locally using:

bash
Copy code
npm start
The application will be available at http://localhost:3000.

File Structure
css
Copy code
src/
├── components/
│   ├── TransactionsOverview.js
│   ├── SchoolTransactions.js
│   ├── TransactionStatusCheck.js
│   ├── ManualStatusUpdate.js
│   └── WebhookHandler.js
├── App.js
├── App.css
└── index.js
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
The app includes a dark mode toggle that can be accessed from the navbar. When enabled, it applies a dark theme to the UI.

To toggle dark mode, click on the 🌙 (for dark mode) or ☀️ (for light mode) button in the navbar.

CSS Styling
App.css handles the main styling of the application, including dark mode and light mode themes.
A class .dark-mode is dynamically applied to the root div when the dark mode is toggled.
Troubleshooting
1. App Not Displaying Properly
If the app is not displaying as expected, try the following:

Ensure that all dependencies are installed (npm install).
Check the browser console for errors.
2. Dark Mode Not Working
If the dark mode toggle is not working, make sure you are using a modern browser that supports CSS variables and dark mode themes.

Contributing
If you'd like to contribute to this project, follow these steps:

Fork the repository
Create a new branch (git checkout -b feature-name)
Make your changes
Commit your changes (git commit -am 'Add new feature')
Push to the branch (git push origin feature-name)
Open a Pull Request
