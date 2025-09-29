# E-commerce Backend API

A comprehensive e-commerce backend built with Node.js, Express.js, and MongoDB. This project includes user authentication, product management, shopping cart functionality, order processing with Stripe payments, and an admin dashboard.

## 🚀 Features

### User Features

- **Authentication & Authorization**

  - User registration and login
  - OAuth integration (Google, GitHub)
  - JWT-based authentication
  - Password reset functionality
  - Role-based access control (Customer, Admin)

- **Product Browsing**

  - Browse products by categories
  - Search and filter products
  - View product details and images
  - Product reviews and ratings

- **Shopping Cart**

  - Add/remove items from cart
  - Update item quantities
  - Persistent cart (saved to database)
  - Cart total calculations

- **Order Management**
  - Place orders with Stripe payment integration
  - Order history and tracking
  - Order status updates
  - Email notifications

### Admin Features

- **Dashboard Analytics**

  - Total orders overview
  - Pending orders management
  - Cancelled orders tracking
  - Fulfilled orders summary
  - Revenue analytics

- **Product Management**

  - Add, edit, delete products
  - Manage product categories
  - Inventory management
  - Bulk product operations

- **Order Management**
  - View all orders
  - Update order status
  - Process refunds
  - Generate reports

## 🛠 Tech Stack

- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** MongoDB with Mongoose ODM
- **Authentication:** JWT, Passport.js (OAuth)
- **Payments:** Stripe API
- **File Upload:** Multer (for product images)
- **Email:** Nodemailer
- **Validation:** Joi or express-validator
- **Security:** bcrypt, helmet, cors, rate-limiting

## 📁 Project Structure

```
ecommerce-backend/
├── src/
│   ├── config/
│   │   ├── database.js
│   │   ├── stripe.js
│   │   ├── oauth.js
│   │   └── email.js
│   ├── models/
│   │   ├── User.js
│   │   ├── Product.js
│   │   ├── Category.js
│   │   ├── Cart.js
│   │   ├── Order.js
│   │   └── Review.js
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── userController.js
│   │   ├── productController.js
│   │   ├── categoryController.js
│   │   ├── cartController.js
│   │   ├── orderController.js
│   │   ├── paymentController.js
│   │   ├── reviewController.js
│   │   └── adminController.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── users.js
│   │   ├── products.js
│   │   ├── categories.js
│   │   ├── cart.js
│   │   ├── orders.js
│   │   ├── payments.js
│   │   ├── reviews.js
│   │   └── admin.js
│   ├── middleware/
│   │   ├── auth.js
│   │   ├── admin.js
│   │   ├── validation.js
│   │   ├── upload.js
│   │   └── errorHandler.js
│   ├── utils/
│   │   ├── sendEmail.js
│   │   ├── generateToken.js
│   │   ├── uploadImage.js
│   │   └── helpers.js
│   └── app.js
├── uploads/
├── tests/
├── .env.example
├── .gitignore
├── package.json
└── server.js
```

## 🗄️ Database Models

### User Model

```javascript
{
  firstName: String,
  lastName: String,
  email: String (unique),
  password: String,
  role: Enum ['customer', 'admin'],
  avatar: String,
  phone: String,
  addresses: [AddressSchema],
  oauthProvider: String,
  oauthId: String,
  isEmailVerified: Boolean,
  resetPasswordToken: String,
  resetPasswordExpires: Date,
  createdAt: Date,
  updatedAt: Date
}
```

### Product Model

```javascript
{
  name: String,
  description: String,
  price: Number,
  comparePrice: Number,
  category: ObjectId (ref: Category),
  images: [String],
  inventory: {
    quantity: Number,
    lowStockThreshold: Number
  },
  specifications: Object,
  isActive: Boolean,
  rating: {
    average: Number,
    count: Number
  },
  tags: [String],
  createdAt: Date,
  updatedAt: Date
}
```

### Category Model

```javascript
{
  name: String,
  description: String,
  slug: String,
  image: String,
  parentCategory: ObjectId (ref: Category),
  isActive: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

### Cart Model

```javascript
{
  user: ObjectId (ref: User),
  items: [{
    product: ObjectId (ref: Product),
    quantity: Number,
    price: Number
  }],
  totalAmount: Number,
  createdAt: Date,
  updatedAt: Date
}
```

### Order Model

```javascript
{
  orderNumber: String,
  user: ObjectId (ref: User),
  items: [{
    product: ObjectId (ref: Product),
    quantity: Number,
    price: Number,
    name: String
  }],
  shippingAddress: AddressSchema,
  billingAddress: AddressSchema,
  paymentInfo: {
    method: String,
    stripePaymentIntentId: String,
    status: String
  },
  orderStatus: Enum ['pending', 'processing', 'shipped', 'delivered', 'cancelled'],
  totalAmount: Number,
  shippingCost: Number,
  taxAmount: Number,
  notes: String,
  createdAt: Date,
  updatedAt: Date
}
```

### Review Model

```javascript
{
  user: ObjectId (ref: User),
  product: ObjectId (ref: Product),
  rating: Number (1-5),
  comment: String,
  isVerifiedPurchase: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

## 🔗 API Endpoints

### Authentication Routes

- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/google` - Google OAuth
- `GET /api/auth/github` - GitHub OAuth
- `POST /api/auth/forgot-password` - Forgot password
- `POST /api/auth/reset-password` - Reset password

### Product Routes

- `GET /api/products` - Get all products (with pagination, filtering)
- `GET /api/products/:id` - Get single product
- `POST /api/products` - Create product (Admin only)
- `PUT /api/products/:id` - Update product (Admin only)
- `DELETE /api/products/:id` - Delete product (Admin only)

### Category Routes

- `GET /api/categories` - Get all categories
- `POST /api/categories` - Create category (Admin only)
- `PUT /api/categories/:id` - Update category (Admin only)
- `DELETE /api/categories/:id` - Delete category (Admin only)

### Cart Routes

- `GET /api/cart` - Get user's cart
- `POST /api/cart/add` - Add item to cart
- `PUT /api/cart/update/:itemId` - Update cart item
- `DELETE /api/cart/remove/:itemId` - Remove item from cart
- `DELETE /api/cart/clear` - Clear entire cart

### Order Routes

- `GET /api/orders` - Get user's orders
- `GET /api/orders/:id` - Get single order
- `POST /api/orders` - Create new order
- `PUT /api/orders/:id/cancel` - Cancel order

### Payment Routes

- `POST /api/payments/create-intent` - Create Stripe payment intent
- `POST /api/payments/confirm` - Confirm payment
- `POST /api/payments/webhook` - Stripe webhook

### Admin Routes

- `GET /api/admin/dashboard` - Dashboard analytics
- `GET /api/admin/orders` - All orders with filtering
- `PUT /api/admin/orders/:id/status` - Update order status
- `GET /api/admin/users` - All users
- `GET /api/admin/analytics` - Revenue and sales analytics

## 🚀 Getting Started

### Prerequisites

- Node.js (v14 or higher)
- MongoDB
- Stripe Account
- Google/GitHub OAuth Apps (optional)

### Installation

1. Clone the repository

```bash
git clone <repository-url>
cd ecommerce-backend
```

2. Install dependencies

```bash
npm install
```

3. Create environment file

```bash
cp .env.example .env
```

4. Configure environment variables

```env
# Server
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/ecommerce

# JWT
JWT_SECRET=your-jwt-secret
JWT_EXPIRE=30d

# Stripe
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# OAuth
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GITHUB_CLIENT_ID=your-github-client-id
GITHUB_CLIENT_SECRET=your-github-client-secret

# Email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password
```

5. Start the development server

```bash
npm run dev
```

## 📚 Learning Objectives

By completing this project, students will learn:

1. **Backend Architecture**

   - RESTful API design
   - MVC pattern implementation
   - Database modeling and relationships

2. **Authentication & Security**

   - JWT token management
   - OAuth integration
   - Password hashing and security best practices

3. **Payment Processing**

   - Stripe API integration
   - Webhook handling
   - Secure payment flows

4. **Database Operations**

   - MongoDB with Mongoose
   - Complex queries and aggregations
   - Data validation and sanitization

5. **Real-world Skills**
   - Error handling and logging
   - API documentation
   - Testing strategies
   - Deployment considerations

## 🧪 Testing

```bash
# Run all tests
npm test

# Run tests with coverage
npm run test:coverage

# Run specific test file
npm test -- controllers/authController.test.js
```

## 📝 Development Guidelines

1. **Code Structure**

   - Use async/await for asynchronous operations
   - Implement proper error handling
   - Follow RESTful conventions
   - Write clean, documented code

2. **Security**

   - Validate all inputs
   - Sanitize data before database operations
   - Implement rate limiting
   - Use HTTPS in production

3. **Performance**
   - Implement pagination for large datasets
   - Use database indexes appropriately
   - Optimize queries and avoid N+1 problems
   - Implement caching where appropriate

## 🚀 Deployment

Ready for deployment on platforms like:

- Heroku
- DigitalOcean
- AWS
- Vercel (for serverless)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## 📄 License

This project is licensed under the MIT License.

---

## 📖 Additional Resources

- [Express.js Documentation](https://expressjs.com/)
- [MongoDB/Mongoose Documentation](https://mongoosejs.com/)
- [Stripe API Documentation](https://stripe.com/docs/api)
- [JWT Authentication Guide](https://jwt.io/)
- [OAuth 2.0 Guide](https://oauth.net/2/)

Happy coding!
