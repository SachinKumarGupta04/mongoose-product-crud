# CRUD Operations for Product Database Using Mongoose

## Objective

Learn how to implement basic Create, Read, Update, and Delete (CRUD) operations on a MongoDB collection using Mongoose in Node.js. This task helps you understand schema design, database connectivity, and handling data in a structured, real-world manner.

## Task Description

Create a Node.js application that connects to a MongoDB database using Mongoose. Define a Product model with properties such as name, price, and category. Implement routes or functions to perform CRUD operations: add a new product, retrieve all products, update a product by its ID, and delete a product by its ID. Use appropriate Mongoose methods for each operation and ensure that all data validations and error handling are included.

## Features

- **Create Product**: Add new products to the database with name, price, and category
- **Read Products**: Retrieve all products or fetch a specific product by ID
- **Update Product**: Modify existing product details by ID
- **Delete Product**: Remove a product from the database by ID
- **Data Validation**: Ensure all product data meets schema requirements
- **Error Handling**: Comprehensive error handling for all operations
- **RESTful API**: Follow REST principles with proper HTTP methods and status codes
- **MongoDB Integration**: Persistent data storage using MongoDB

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v14 or higher) - [Download Node.js](https://nodejs.org/)
- **MongoDB** - [Download MongoDB](https://www.mongodb.com/try/download/community) or use [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) (free cloud database)
- **npm** (comes with Node.js) or **yarn**
- A code editor (VS Code, Sublime Text, etc.)
- An API testing tool (Postman, Thunder Client, curl, or Insomnia)

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/SachinKumarGupta04/mongoose-product-crud.git
cd mongoose-product-crud
```

### 2. Initialize Node.js Project

If starting from scratch:

```bash
npm init -y
```

### 3. Install Dependencies

Install Mongoose and Express:

```bash
npm install express mongoose
npm install --save-dev nodemon dotenv
```

### 4. Project Structure

Create the following file structure:

```
mongoose-product-crud/
├── node_modules/
├── src/
│   ├── server.js
│   ├── config/
│   │   └── database.js
│   ├── models/
│   │   └── Product.js
│   ├── routes/
│   │   └── productRoutes.js
│   └── controllers/
│       └── productController.js
├── .env
├── .gitignore
├── package.json
├── package-lock.json
└── README.md
```

### 5. Configure Environment Variables

Create a `.env` file in the root directory:

```env
PORT=3000
MONGODB_URI=mongodb://localhost:27017/productdb
# For MongoDB Atlas, use:
# MONGODB_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/productdb
```

### 6. Configure package.json Scripts

Add the following scripts to your `package.json`:

```json
"scripts": {
  "start": "node src/server.js",
  "dev": "nodemon src/server.js"
}
```

### 7. Run the Application

For development (with auto-reload):

```bash
npm run dev
```

For production:

```bash
npm start
```

The server will start on `http://localhost:3000` (or the port you configure).

## API Endpoint Overview

### Base URL

```
http://localhost:3000/api
```

### Endpoints

#### 1. Get All Products

```http
GET /api/products
```

**Description**: Retrieves all products from the database.

**Response**:

```json
{
  "success": true,
  "count": 2,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "name": "Laptop",
      "price": 999.99,
      "category": "Electronics",
      "createdAt": "2025-10-30T08:00:00.000Z",
      "updatedAt": "2025-10-30T08:00:00.000Z"
    },
    {
      "_id": "507f1f77bcf86cd799439012",
      "name": "Chair",
      "price": 149.99,
      "category": "Furniture",
      "createdAt": "2025-10-30T08:05:00.000Z",
      "updatedAt": "2025-10-30T08:05:00.000Z"
    }
  ]
}
```

#### 2. Get Product by ID

```http
GET /api/products/:id
```

**Description**: Retrieves a specific product by its ID.

**Example**: `GET /api/products/507f1f77bcf86cd799439011`

**Response**:

```json
{
  "success": true,
  "data": {
    "_id": "507f1f77bcf86cd799439011",
    "name": "Laptop",
    "price": 999.99,
    "category": "Electronics",
    "createdAt": "2025-10-30T08:00:00.000Z",
    "updatedAt": "2025-10-30T08:00:00.000Z"
  }
}
```

#### 3. Create a New Product

```http
POST /api/products
Content-Type: application/json

{
  "name": "Smartphone",
  "price": 699.99,
  "category": "Electronics"
}
```

**Description**: Adds a new product to the database.

**Request Body**:

- `name` (string, required): Product name
- `price` (number, required): Product price (must be positive)
- `category` (string, required): Product category

**Response**:

```json
{
  "success": true,
  "message": "Product created successfully",
  "data": {
    "_id": "507f1f77bcf86cd799439013",
    "name": "Smartphone",
    "price": 699.99,
    "category": "Electronics",
    "createdAt": "2025-10-30T08:10:00.000Z",
    "updatedAt": "2025-10-30T08:10:00.000Z"
  }
}
```

#### 4. Update Product by ID

```http
PUT /api/products/:id
Content-Type: application/json

{
  "name": "Gaming Laptop",
  "price": 1299.99
}
```

**Description**: Updates an existing product's details.

**Example**: `PUT /api/products/507f1f77bcf86cd799439011`

**Response**:

```json
{
  "success": true,
  "message": "Product updated successfully",
  "data": {
    "_id": "507f1f77bcf86cd799439011",
    "name": "Gaming Laptop",
    "price": 1299.99,
    "category": "Electronics",
    "createdAt": "2025-10-30T08:00:00.000Z",
    "updatedAt": "2025-10-30T08:15:00.000Z"
  }
}
```

#### 5. Delete Product by ID

```http
DELETE /api/products/:id
```

**Description**: Removes a product from the database.

**Example**: `DELETE /api/products/507f1f77bcf86cd799439011`

**Response**:

```json
{
  "success": true,
  "message": "Product deleted successfully"
}
```

### Error Responses

All error responses follow this format:

```json
{
  "success": false,
  "error": "Error message describing what went wrong"
}
```

**Common HTTP Status Codes**:

- `200 OK`: Successful GET, PUT, DELETE request
- `201 Created`: Successful POST request
- `400 Bad Request`: Invalid data provided or validation error
- `404 Not Found`: Product ID not found
- `500 Internal Server Error`: Database or server error

## Data Model

### Product Schema

```javascript
const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Product name is required'],
    trim: true,
    maxlength: [100, 'Product name cannot exceed 100 characters']
  },
  price: {
    type: Number,
    required: [true, 'Product price is required'],
    min: [0, 'Price cannot be negative']
  },
  category: {
    type: String,
    required: [true, 'Product category is required'],
    trim: true
  }
}, {
  timestamps: true  // Automatically adds createdAt and updatedAt fields
});
```

## Testing the API

### Using cURL

**Get all products**:

```bash
curl http://localhost:3000/api/products
```

**Create a new product**:

```bash
curl -X POST http://localhost:3000/api/products \
  -H "Content-Type: application/json" \
  -d '{"name":"Tablet","price":399.99,"category":"Electronics"}'
```

**Get product by ID**:

```bash
curl http://localhost:3000/api/products/507f1f77bcf86cd799439011
```

**Update product**:

```bash
curl -X PUT http://localhost:3000/api/products/507f1f77bcf86cd799439011 \
  -H "Content-Type: application/json" \
  -d '{"price":899.99}'
```

**Delete product**:

```bash
curl -X DELETE http://localhost:3000/api/products/507f1f77bcf86cd799439011
```

### Using Postman

1. Import the collection or create requests manually
2. Set base URL to `http://localhost:3000/api`
3. Test each endpoint with appropriate HTTP methods
4. Verify responses match expected JSON format

## Implementation Guidelines

### Validation Rules

- **Name**: Required, string, max 100 characters, trimmed
- **Price**: Required, number, must be non-negative
- **Category**: Required, string, trimmed
- **ID validation**: Must be valid MongoDB ObjectId format

### Best Practices

- Use async/await for asynchronous operations
- Implement proper error handling with try-catch blocks
- Use Mongoose schema validation
- Return consistent JSON response format
- Use appropriate HTTP status codes
- Separate concerns (routes, controllers, models)
- Use environment variables for configuration
- Add timestamps to track creation and updates

## Technologies Used

- **Node.js**: JavaScript runtime environment
- **Express.js**: Fast, minimalist web framework
- **Mongoose**: MongoDB object modeling tool
- **MongoDB**: NoSQL database for data persistence
- **dotenv**: Environment variable management
- **Nodemon**: Development tool for auto-restarting server

## Learning Outcomes

By completing this project, you will learn:

- How to set up and connect to MongoDB using Mongoose
- Defining Mongoose schemas and models
- Implementing CRUD operations with Mongoose methods
- RESTful API design principles
- Data validation and error handling
- Database query operations
- Asynchronous programming in Node.js
- Environment configuration management
- MVC (Model-View-Controller) pattern basics

## Contributing

Feel free to fork this repository and submit pull requests for improvements or additional features such as:

- Search and filtering functionality
- Pagination support
- Advanced validation rules
- Product image upload
- Authentication and authorization
- Product inventory tracking
- Category management

## License

This project is open source and available for educational purposes.

## Support

If you encounter any issues or have questions, please open an issue in this repository.

## Acknowledgments

This project is designed as a learning exercise to understand fundamental concepts of CRUD operations with Mongoose and MongoDB in a Node.js environment.
