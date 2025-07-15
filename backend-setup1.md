# Chapter 1: MERN Chat App Backend Setup

## Welcome to Building Your First Chat App Backend!

Hello! In this tutorial, you will learn how to build the backend (server) for a real-time chat application. Don't worry if you're completely new to this - we'll explain everything step by step.

### What You'll Build
By the end of this tutorial, you'll have a working backend server that can:
- Register new users
- Log users in and out
- Update user profiles
- Store all data in a database
- Handle user authentication securely

### What You Need Before Starting
- A computer with internet connection
- Node.js installed (if not, we'll help you install it)
- A text editor (VS Code is recommended and free)
- Basic knowledge of JavaScript (variables, functions, objects)

---

## Part 1: Understanding What We're Building

### What is a Backend?
Think of a chat app like a restaurant:
- **Frontend** = The dining area where customers sit and order food
- **Backend** = The kitchen where food is prepared and stored

When you send a message in WhatsApp:
1. Your phone (frontend) sends the message to WhatsApp's servers (backend)
2. The server stores the message in a database
3. The server sends the message to your friend's phone

### Our Technology Stack
We'll use these tools (don't worry, we'll explain each one):

**Node.js** - Allows us to write server code in JavaScript
**Express.js** - Makes creating web servers easier
**MongoDB** - Our database to store user data and messages
**Mongoose** - Helps us work with MongoDB more easily

### Project Structure
We'll organize our code like this:
```
mern-chat-app/
  backend/
    src/
      models/        (database schemas)
      routes/        (API endpoints)
      middleware/    (authentication helpers)
      utils/         (helper functions)
      server.js      (main server file)
    package.json      (project information)
    .env             (secret configuration)
```

---

## Part 2: Setting Up Your Development Environment

### Step 1: Check if Node.js is Installed

Open your terminal (Command Prompt on Windows, Terminal on Mac/Linux) and type:

```bash
node --version
```

If you see a version number like `v18.17.0`, you're good to go!

If you see "command not found" or similar error:
1. Go to [nodejs.org](https://nodejs.org/)
2. Download the LTS (Long Term Support) version
3. Install it following the setup wizard
4. Restart your terminal and try the command again

### Step 2: Create Your Project Folder

In your terminal, navigate to where you want to create your project (like Desktop or Documents):

```bash
# Create main project folder
mkdir mern-chat-app
cd mern-chat-app

# Create server folder
mkdir backend
cd backend
```

**What these commands do:**
- `mkdir` = "make directory" (creates a new folder)
- `cd` = "change directory" (moves into a folder)

### Step 3: Initialize Your Node.js Project
```bash
npm init -y
```
**What this does:**
- Creates a `package.json` file
- This file contains information about your project
- The `-y` flag automatically says "yes" to all default options
You should see a new file called `package.json` in your server folder.

### Step 4: Configure ES Modules
Open `package.json` and add `"type": "module"` to enable ES modules:
```json
{
  "name": "server",
  "version": "1.0.0",
  "type": "module",
  "description": "",
  "main": "src/server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

### Step 5: Install Required Packages
We need to install several "packages" (pre-written code that helps us):
```bash
npm install express mongoose socket.io cors dotenv bcryptjs jsonwebtoken cookie-parser
```
**What each package does:**
- **express** - Framework for building web servers
- **mongoose** - Makes working with MongoDB database easier
- **socket.io** - Enables real-time features for chat
- **cors** - Allows frontend and backend to communicate
- **dotenv** - Manages environment variables (secrets)
- **bcryptjs** - Encrypts passwords for security
- **jsonwebtoken** - Creates secure login tokens
- **cookie-parser** - Handles cookies for authentication

Now install development tools:
```bash
npm install -D nodemon
```
**What nodemon does:**
- Automatically restarts your server when you make code changes
- The `-D` means it's only needed during development

### Step 6: Set Up Development Scripts
Open `package.json` in your text editor and update the "scripts" section:
```json
{
  "name": "server",
  "version": "1.0.0",
  "type": "module",
  "description": "",
  "main": "src/server.js",
  "scripts": {
    "start": "node src/server.js",
    "dev": "nodemon src/server.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
**What these scripts do:**
- `npm start` - Runs your server normally (src/server.js)
- `npm run dev` - Runs your server with auto-restart when files change (src/server.js)

### Step 7: Create Project Structure
Now create the `src/` directory and initialize the basic project structure:
```bash
mkdir src
mkdir src/models
mkdir src/routes
mkdir src/middleware
mkdir src/controllers
mkdir src/utils
```

**What each directory does:**
- **src/** - Contains all your source code
- **src/models/** - Database schemas and models
- **src/routes/** - API endpoints and routing logic
- **src/middleware/** - Authentication and request processing
- **src/controllers/** - Business logic for handling requests
- **src/utils/** - Helper functions and utilities

### Step 8: Create Environment Configuration
Create a `.env` file in your backend root directory (not in src):
```bash
touch .env
```

Add the following configuration to your `.env` file:
```env
NODE_ENV=development
PORT=5000
MONGODB_URI=mongodb://localhost:27017/chatapp
JWT_SECRET=your-super-secret-jwt-key-here
JWT_EXPIRES_IN=7d
```

**What these variables do:**
- **NODE_ENV** - Sets the environment (development/production)
- **PORT** - Port number for your server
- **MONGODB_URI** - Database connection string
- **JWT_SECRET** - Secret key for generating tokens
- **JWT_EXPIRES_IN** - How long tokens remain valid

---

## Part 3: Creating Your First Server

### Step 1: Create the Main Server File

In your `src/` folder, create a new file called `server.js` and add this code:

```javascript
// Import required packages
import express from 'express';

// Create Express application
const app = express();

// Define which port our server will run on
const PORT = 5000;

// Create a simple route that responds to web requests
app.get('/', (req, res) => {
  res.send('Welcome to our Chat App Server!');
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

**Understanding this code:**

**Line 2**: `import express from 'express'` imports the Express package using ES modules
**Line 5**: `express()` creates a new web application
**Line 8**: `PORT = 5000` sets our server to run on port 5000
**Line 11-13**: Creates a route that responds when someone visits our server
**Line 16-18**: Starts the server and tells us it's running

### Step 2: Test Your Server

In your terminal (make sure you're in the backend folder), run:

```bash
npm run dev
```

You should see: `Server is running on http://localhost:5000`

Open your web browser and go to `http://localhost:5000`

You should see: "Welcome to our Chat App Server!"

**Congratulations! Your server is working!**

### Step 3: Add Middleware for JSON Handling

Middleware is code that runs before your route handlers. Update your `src/server.js` file:

```javascript
// Import required packages
import express from 'express';
import cors from 'cors';
import cookieParser from 'cookie-parser';

// Create Express application
const app = express();

// Middleware - code that runs before our routes
app.use(express.json()); // Allows server to understand JSON data
app.use(cors()); // Allows frontend to communicate with backend
app.use(cookieParser()); // Allows server to parse cookies

// Define which port our server will run on
const PORT = 5000;

// Create a simple route that responds to web requests
app.get('/', (req, res) => {
  res.send('Welcome to our Chat App Server!');
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

**What we added:**
- `app.use(express.json())` - Lets our server understand JSON data from requests
- `app.use(cors())` - Allows our future frontend to communicate with this backend
- `app.use(cookieParser())` - Allows our server to work with cookies

Save the file and your server should automatically restart (thanks to nodemon).

---

## Part 4: Setting Up the Database

### Understanding Databases

A database is like a digital filing cabinet where we store information permanently. For our chat app, we need to store:
- User accounts (username, email, password)
- Chat messages
- User profiles

We'll use MongoDB, which stores data in a flexible format called JSON.

### Step 1: Create a MongoDB Database (Using MongoDB Atlas - Free Cloud Database)

1. **Go to** [mongodb.com/atlas](https://www.mongodb.com/atlas)

2. **Click "Try Free"** and create an account

3. **Create a new cluster:**
   - Choose "FREE" tier
   - Select a region close to you
   - Click "Create Cluster"

4. **Create a database user:**
   - Go to "Database Access" in the left menu
   - Click "Add New Database User"
   - Choose "Password" authentication
   - Create a username and strong password
   - Write these down - you'll need them!
   - Give the user "Read and write to any database" permission
   - Click "Add User"

5. **Allow network access:**
   - Go to "Network Access" in the left menu
   - Click "Add IP Address"
   - Click "Allow Access from Anywhere" (for development only)
   - Click "Confirm"

6. **Get your connection string:**
   - Go to "Clusters" in the left menu
   - Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string (it looks like: `mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/`)

### Step 2: Create Environment Variables File

Create a new file called `.env` in your server folder:

```env
MONGODB_URI=mongodb+srv://your-username:your-password@cluster0.xxxxx.mongodb.net/chatapp
JWT_SECRET=your-super-secret-key-here-make-it-long-and-random
PORT=5000
```

**Replace:**
- `your-username` with your MongoDB username
- `your-password` with your MongoDB password
- `cluster0.xxxxx` with your actual cluster address
- `your-super-secret-key-here-make-it-long-and-random` with a long random string

**Important:** Never share your `.env` file with anyone!

### Step 3: Connect Your Server to the Database

Update your `server.js` file:

```javascript
// Import required packages
import express from 'express';
import mongoose from 'mongoose';
import cors from 'cors';
import cookieParser from 'cookie-parser';
import dotenv from 'dotenv';

// Load environment variables
dotenv.config();

// Create Express application
const app = express();

// Middleware
app.use(express.json());
app.use(cors());
app.use(cookieParser());

// Get port from environment or use 5000
const PORT = process.env.PORT || 5000;

// Connect to MongoDB database
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
.then(() => {
  console.log('✅ Connected to MongoDB database');
})
.catch((error) => {
  console.error('❌ Database connection error:', error);
});

// Routes
app.get('/', (req, res) => {
  res.send('Chat Server is Running and Connected to Database!');
});

// Start the server
app.listen(PORT, () => {
  console.log(`🚀 Server running on http://localhost:${PORT}`);
});
```

**What we added:**
- `import dotenv from 'dotenv'` and `dotenv.config()` - Loads our environment variables
- `mongoose.connect()` - Connects to our MongoDB database
- Error handling with `.then()` and `.catch()`

Save the file. You should see both messages:
- `✅ Connected to MongoDB database`
- `🚀 Server running on http://localhost:5000`

If you see an error, check that your `.env` file has the correct connection string.

---

## Part 5: Creating User Data Models

### Understanding Data Models

A model defines the structure of data in our database. It's like a blueprint that says "every user must have a username, email, and password."

### Step 1: Create the Models Folder

```bash
mkdir models
```

### Step 2: Create the User Model

Create a new file `models/User.js`:

```javascript
// Import mongoose
import mongoose from 'mongoose';

// Define the structure for user data
const userSchema = new mongoose.Schema({
  username: {
    type: String,        // Must be text
    required: true,      // Must be provided
    unique: true,        // No two users can have same username
    trim: true,          // Removes extra spaces
    minlength: 3,        // At least 3 characters
    maxlength: 20        // At most 20 characters
  },
  email: {
    type: String,
    required: true,
    unique: true,        // No two users can have same email
    lowercase: true      // Converts to lowercase
  },
  password: {
    type: String,
    required: true,
    minlength: 6         // At least 6 characters
  },
  avatar: {
    type: String,
    default: 'https://via.placeholder.com/150/4A90E2/FFFFFF?text=User'
  },
  isOnline: {
    type: Boolean,       // true or false
    default: false       // New users start offline
  }
}, {
  timestamps: true       // Automatically adds createdAt and updatedAt
});

// Create and export the User model
export default mongoose.model('User', userSchema);
```

**Understanding this schema:**

Each field has rules:
- **type** - What kind of data (String, Boolean, Number, etc.)
- **required** - Must be provided when creating a user
- **unique** - No duplicates allowed in the database
- **default** - Value to use if not provided
- **minlength/maxlength** - Validation rules

**timestamps: true** automatically adds:
- `createdAt` - When the user was created
- `updatedAt` - When the user was last modified

---

## Part 6: Creating JWT Utility Function

### Step 1: Create Utils Folder

```bash
mkdir utils
```

### Step 2: Create JWT Generation Utility

Create `utils/generateJWT.js`:

```javascript
import jwt from 'jsonwebtoken';

const generateJWT = (userId, res) => {
  // Create JWT token
  const token = jwt.sign(
    { userId },                    // Payload (user ID)
    process.env.JWT_SECRET,        // Secret key
    { expiresIn: '24h' }          // Token expires in 24 hours
  );
  
  // Set token as HTTP-only cookie
  res.cookie('jwt', token, {
    maxAge: 24 * 60 * 60 * 1000,  // 24 hours in milliseconds
    httpOnly: true,               // Prevents XSS attacks
    sameSite: 'strict',           // Prevents CSRF attacks
    secure: process.env.NODE_ENV === 'production' // Use HTTPS in production
  });
  
  return token;
};

export default generateJWT;
```

**Understanding this utility:**
- **JWT creation** - Creates a token with user ID and expiration
- **Cookie setting** - Stores token in an HTTP-only cookie for security
- **Security options** - Protects against XSS and CSRF attacks
- **Environment-aware** - Uses secure cookies in production

---

## Part 7: Creating Authentication Middleware

Authentication middleware is code that checks if a user is logged in before allowing them to access certain routes.

### Step 1: Create Middleware Folder

```bash
mkdir middleware
```

### Step 2: Create Authentication Middleware

Create `middleware/auth.js`:

```javascript
// Import required packages
import jwt from 'jsonwebtoken';
import User from '../models/User.js';

// Authentication middleware function
const auth = async (req, res, next) => {
  try {
    // Get token from cookie or Authorization header
    const token = req.cookies.jwt || req.header('Authorization')?.replace('Bearer ', '');
    
    // Check if token exists
    if (!token) {
      return res.status(401).json({
        success: false,
        message: 'No token provided, access denied'
      });
    }
    
    // Verify token
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    
    // Find user by ID from token
    const user = await User.findById(decoded.userId).select('-password');
    
    // Check if user still exists
    if (!user) {
      return res.status(401).json({
        success: false,
        message: 'Token is valid but user no longer exists'
      });
    }
    
    // Add user to request object so routes can access it
    req.user = user;
    
    // Continue to the next middleware/route
    next();
    
  } catch (error) {
    // Token is invalid
    console.error('Auth middleware error:', error);
    res.status(401).json({
      success: false,
      message: 'Token is not valid'
    });
  }
};

// Export the middleware
export default auth;
```

**Understanding this middleware:**

1. **Get token** - We extract the JWT token from cookie or Authorization header
2. **Verify token** - We check if the token is valid and not expired
3. **Find user** - We get the user data based on the user ID in the token
4. **Add to request** - We attach the user data to the request so routes can use it
5. **Continue** - We call `next()` to proceed to the actual route handler

The `.select('-password')` means "get all user fields except the password."

---

## Part 8: Building Authentication Routes

### Step 1: Create Routes Folder

```bash
mkdir routes
```

### Step 2: Create Authentication Routes File

Create `routes/auth.js`:

```javascript
// Import required packages
import express from 'express';
import bcrypt from 'bcryptjs';
import User from '../models/User.js';
import generateJWT from '../utils/generateJWT.js';
import auth from '../middleware/auth.js';

// Create router
const router = express.Router();

// Sign-up route - POST /api/auth/signup
router.post('/signup', async (req, res) => {
  try {
    // Get user data from request body
    const { username, email, password } = req.body;
    
    // Check if all required fields are provided
    if (!username || !email || !password) {
      return res.status(400).json({
        success: false,
        message: 'Please provide username, email, and password'
      });
    }
    
    // Check if username or email already exists
    const existingUser = await User.findOne({
      $or: [{ email: email }, { username: username }]
    });
    
    if (existingUser) {
      return res.status(400).json({
        success: false,
        message: 'User with this email or username already exists'
      });
    }
    
    // Hash (encrypt) the password for security
    const saltRounds = 12;
    const hashedPassword = await bcrypt.hash(password, saltRounds);
    
    // Create new user object
    const newUser = new User({
      username: username,
      email: email,
      password: hashedPassword
    });
    
    // Save user to database
    const savedUser = await newUser.save();
    
    // Generate JWT token and set cookie
    const token = generateJWT(savedUser._id, res);
    
    // Send success response (don't send password back)
    res.status(201).json({
      success: true,
      message: 'User created successfully',
      token: token,
      user: {
        id: savedUser._id,
        username: savedUser.username,
        email: savedUser.email,
        avatar: savedUser.avatar,
        isOnline: savedUser.isOnline
      }
    });
    
  } catch (error) {
    // Handle any errors
    console.error('Signup error:', error);
    res.status(500).json({
      success: false,
      message: 'Server error during registration',
      error: error.message
    });
  }
});

// Login route - POST /api/auth/login
router.post('/login', async (req, res) => {
  try {
    // Get login data from request
    const { email, password } = req.body;
    
    // Check if email and password are provided
    if (!email || !password) {
      return res.status(400).json({
        success: false,
        message: 'Please provide email and password'
      });
    }
    
    // Find user by email
    const user = await User.findOne({ email: email });
    
    // If user doesn't exist
    if (!user) {
      return res.status(400).json({
        success: false,
        message: 'Invalid email or password'
      });
    }
    
    // Check if password is correct
    const isPasswordCorrect = await bcrypt.compare(password, user.password);
    
    if (!isPasswordCorrect) {
      return res.status(400).json({
        success: false,
        message: 'Invalid email or password'
      });
    }
    
    // Update user's online status
    user.isOnline = true;
    await user.save();
    
    // Generate JWT token and set cookie
    const token = generateJWT(user._id, res);
    
    // Send success response
    res.status(200).json({
      success: true,
      message: 'Login successful',
      token: token,
      user: {
        id: user._id,
        username: user.username,
        email: user.email,
        avatar: user.avatar,
        isOnline: user.isOnline
      }
    });
    
  } catch (error) {
    console.error('Login error:', error);
    res.status(500).json({
      success: false,
      message: 'Server error during login',
      error: error.message
    });
  }
});

// Logout route - POST /api/auth/logout
router.post('/logout', auth, async (req, res) => {
  try {
    // Update user's online status to false
    await User.findByIdAndUpdate(req.user._id, {
      isOnline: false
    });
    
    // Clear the JWT cookie
    res.cookie('jwt', '', {
      maxAge: 0,
      httpOnly: true,
      sameSite: 'strict',
      secure: process.env.NODE_ENV === 'production'
    });
    
    res.status(200).json({
      success: true,
      message: 'Logged out successfully'
    });
    
  } catch (error) {
    console.error('Logout error:', error);
    res.status(500).json({
      success: false,
      message: 'Server error during logout',
      error: error.message
    });
  }
});

// Get current user route - GET /api/auth/me
router.get('/me', auth, async (req, res) => {
  try {
    // req.user is set by the auth middleware
    res.status(200).json({
      success: true,
      user: {
        id: req.user._id,
        username: req.user.username,
        email: req.user.email,
        avatar: req.user.avatar,
        isOnline: req.user.isOnline,
        createdAt: req.user.createdAt
      }
    });
    
  } catch (error) {
    console.error('Get user error:', error);
    res.status(500).json({
      success: false,
      message: 'Server error getting user information',
      error: error.message
    });
  }
});

// Update profile route - PUT /api/auth/profile
router.put('/profile', auth, async (req, res) => {
  try {
    // Get update data from request
    const { username, email, avatar } = req.body;
    
    // Prepare update object (only include provided fields)
    const updateData = {};
    if (username) updateData.username = username;
    if (email) updateData.email = email;
    if (avatar) updateData.avatar = avatar;
    
    // Check if username or email is being changed to one that already exists
    if (username || email) {
      const existingUser = await User.findOne({
        $and: [
          { _id: { $ne: req.user._id } },  // Not the current user
          { $or: [
            username ? { username: username } : {},
            email ? { email: email } : {}
          ].filter(obj => Object.keys(obj).length > 0) }
        ]
      });
      
      if (existingUser) {
        return res.status(400).json({
          success: false,
          message: 'Username or email already taken by another user'
        });
      }
    }
    
    // Update user in database
    const updatedUser = await User.findByIdAndUpdate(
      req.user._id,
      updateData,
      { 
        new: true,           // Return updated document
        runValidators: true  // Run schema validations
      }
    ).select('-password');  // Don't return password
    
    res.status(200).json({
      success: true,
      message: 'Profile updated successfully',
      user: {
        id: updatedUser._id,
        username: updatedUser.username,
        email: updatedUser.email,
        avatar: updatedUser.avatar,
        isOnline: updatedUser.isOnline
      }
    });
    
  } catch (error) {
    console.error('Profile update error:', error);
    
    // Handle validation errors
    if (error.name === 'ValidationError') {
      return res.status(400).json({
        success: false,
        message: 'Validation error',
        errors: Object.values(error.errors).map(err => err.message)
      });
    }
    
    // Handle duplicate key errors
    if (error.code === 11000) {
      return res.status(400).json({
        success: false,
        message: 'Username or email already exists'
      });
    }
    
    res.status(500).json({
      success: false,
      message: 'Server error during profile update',
      error: error.message
    });
  }
});

// Export the router
export default router;
```

**Understanding this code:**

1. **Input validation** - We check if all required fields are provided
2. **Duplicate check** - We make sure username and email aren't already taken
3. **Password hashing** - We encrypt the password using bcrypt (never store plain passwords!)
4. **Database save** - We save the new user to MongoDB
5. **JWT token** - We use our utility function to create a token and set cookie
6. **Response** - We send back user info and token (but never the password)

---

## Part 9: Connect Routes to Main Server

### Update your server.js to use the auth routes:

```javascript
// Import required packages
import express from 'express';
import mongoose from 'mongoose';
import cors from 'cors';
import cookieParser from 'cookie-parser';
import dotenv from 'dotenv';

// Import routes
import authRoutes from './routes/auth.js';

// Load environment variables
dotenv.config();

// Create Express application
const app = express();

// Middleware
app.use(express.json());
app.use(cors());
app.use(cookieParser());

// Get port from environment or use 5000
const PORT = process.env.PORT || 5000;

// Connect to MongoDB database
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
.then(() => {
  console.log('✅ Connected to MongoDB database');
})
.catch((error) => {
  console.error('❌ Database connection error:', error);
});

// Routes
app.get('/', (req, res) => {
  res.send('Chat Server is Running and Connected to Database!');
});

// Use auth routes - all auth routes will start with /api/auth
app.use('/api/auth', authRoutes);

// Start the server
app.listen(PORT, () => {
  console.log(`🚀 Server running on http://localhost:${PORT}`);
});
```

**What we added:**
- Imported our auth routes using ES module syntax
- Used `app.use('/api/auth', authRoutes)` to make all auth routes available

---

## Part 10: Testing Your Complete Backend

### Using Thunder Client for Testing

**Thunder Client** is a VS Code extension that makes API testing super easy:

1. **Install Thunder Client:**
   - Open VS Code
   - Go to Extensions (Ctrl+Shift+X)
   - Search for "Thunder Client"
   - Install it

2. **Create a New Request:**
   - Click the Thunder Client icon in the sidebar
   - Click "New Request"
   - Choose your HTTP method (GET, POST, PUT, etc.)
   - Enter your URL

### Test All Routes

**1. Test Signup** (POST /api/auth/signup)
- Method: POST
- URL: `http://localhost:5000/api/auth/signup`
- Headers: `Content-Type: application/json`
- Body (JSON):
```json
{
  "username": "testuser",
  "email": "test@example.com",
  "password": "password123"
}
```

Expected Response:
```json
{
  "success": true,
  "message": "User created successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "61f123456789abcdef123456",
    "username": "testuser",
    "email": "test@example.com",
    "avatar": "https://via.placeholder.com/150/4A90E2/FFFFFF?text=User",
    "isOnline": false
  }
}
```

**2. Test Login** (POST /api/auth/login)
- Method: POST
- URL: `http://localhost:5000/api/auth/login`
- Headers: `Content-Type: application/json`
- Body (JSON):
```json
{
  "email": "test@example.com",
  "password": "password123"
}
```

Expected Response:
```json
{
  "success": true,
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "61f123456789abcdef123456",
    "username": "testuser",
    "email": "test@example.com",
    "avatar": "https://via.placeholder.com/150/4A90E2/FFFFFF?text=User",
    "isOnline": true
  }
}
```

**3. Test Get Current User** (GET /api/auth/me)
- Method: GET
- URL: `http://localhost:5000/api/auth/me`
- Headers: `Authorization: Bearer YOUR_JWT_TOKEN`

Expected Response:
```json
{
  "success": true,
  "user": {
    "id": "61f123456789abcdef123456",
    "username": "testuser",
    "email": "test@example.com",
    "avatar": "https://via.placeholder.com/150/4A90E2/FFFFFF?text=User",
    "isOnline": true,
    "createdAt": "2024-01-15T10:30:00.000Z"
  }
}
```

**4. Test Update Profile** (PUT /api/auth/profile)
- Method: PUT
- URL: `http://localhost:5000/api/auth/profile`
- Headers: `Authorization: Bearer YOUR_JWT_TOKEN` and `Content-Type: application/json`
- Body (JSON):
```json
{
  "username": "newusername",
  "avatar": "https://example.com/new-avatar.jpg"
}
```

Expected Response:
```json
{
  "success": true,
  "message": "Profile updated successfully",
  "user": {
    "id": "61f123456789abcdef123456",
    "username": "newusername",
    "email": "test@example.com",
    "avatar": "https://example.com/new-avatar.jpg",
    "isOnline": true
  }
}
```

**5. Test Logout** (POST /api/auth/logout)
- Method: POST
- URL: `http://localhost:5000/api/auth/logout`
- Headers: `Authorization: Bearer YOUR_JWT_TOKEN`

Expected Response:
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

### Common Testing Issues and Solutions

**Issue: "Cannot POST /api/auth/signup"**
- Solution: Make sure your server is running with `npm run dev`
- Check that the route is properly imported in `server.js`

**Issue: "ValidationError: username is required"**
- Solution: Make sure you're sending all required fields in the request body
- Check that `Content-Type: application/json` header is set

**Issue: "MongoServerError: E11000 duplicate key error"**
- Solution: You're trying to create a user with a username or email that already exists
- Try with different credentials

**Issue: "JsonWebTokenError: invalid signature"**
- Solution: Check that your JWT_SECRET in `.env` file is set correctly
- Make sure the token you're using is valid and not expired

**Issue: "Cannot connect to database"**
- Solution: Check your MongoDB connection string in `.env`
- Make sure your IP address is whitelisted in MongoDB Atlas
- Verify your database credentials are correct

---

## Part 11: File Structure Overview

By now, your project should look like this:

```
mern-chat-app/
  server/
    ├── middleware/
    │   └── auth.js
    ├── models/
    │   └── User.js
    ├── routes/
    │   └── auth.js
    ├── utils/
    │   └── generateJWT.js
    ├── .env
    ├── package.json
    └── server.js
```

### What Each File Does:

**server.js** - Main server file that starts the application and connects everything together

**models/User.js** - Database schema that defines how user data is structured

**routes/auth.js** - Contains all authentication-related routes (signup, login, logout, profile)

**middleware/auth.js** - Checks if users are authenticated before accessing protected routes

**utils/generateJWT.js** - Utility function to create JWT tokens and set cookies

**.env** - Contains sensitive environment variables (database URL, JWT secret)

**package.json** - Project configuration and dependencies

---

## Part 12: Security Best Practices Implemented

### What We've Done Right:

1. **Password Hashing**: We use bcrypt to hash passwords before storing them
2. **JWT Tokens**: We use JSON Web Tokens for stateless authentication
3. **HTTP-Only Cookies**: Tokens are stored in secure, HTTP-only cookies
4. **Input Validation**: We validate all user inputs before processing
5. **Error Handling**: We handle errors gracefully without exposing sensitive information
6. **Environment Variables**: Sensitive data like database URLs and secrets are stored in .env files

### Additional Security Measures You Can Add Later:

1. **Rate Limiting**: Prevent brute force attacks
2. **CORS Configuration**: Restrict which domains can access your API
3. **Input Sanitization**: Clean user input to prevent injection attacks
4. **HTTPS**: Use SSL certificates in production
5. **Password Strength Requirements**: Enforce stronger password policies

---

## Part 13: Understanding the Code Flow

### When a User Signs Up:

1. **Request arrives** at POST /api/auth/signup
2. **Validate input** - Check if username, email, and password are provided
3. **Check duplicates** - Make sure username and email aren't already taken
4. **Hash password** - Use bcrypt to encrypt the password
5. **Save to database** - Create new user in MongoDB
6. **Generate JWT** - Create token and set HTTP-only cookie
7. **Send response** - Return user info and token (no password)

### When a User Logs In:

1. **Request arrives** at POST /api/auth/login
2. **Validate input** - Check if email and password are provided
3. **Find user** - Search for user by email in database
4. **Verify password** - Compare provided password with stored hash
5. **Update status** - Mark user as online
6. **Generate JWT** - Create token and set HTTP-only cookie
7. **Send response** - Return user info and token

### When Accessing Protected Routes:

1. **Request arrives** at protected route (like GET /api/auth/me)
2. **Auth middleware runs** - Extract token from cookie or header
3. **Verify token** - Check if token is valid and not expired
4. **Find user** - Get user data from database using token's user ID
5. **Attach user** - Add user data to request object
6. **Continue** - Proceed to the actual route handler

---

## Part 14: Next Steps

### What You've Accomplished:

✅ Created a complete authentication system
✅ Set up a MongoDB database
✅ Implemented user registration and login
✅ Built secure password hashing
✅ Created JWT-based authentication with cookies
✅ Added profile management features
✅ Implemented proper error handling
✅ Used modern ES6+ JavaScript features
✅ Separated concerns with utility functions and middleware

### What's Next:

1. **Add Message Models**: Create schemas for chat messages
2. **Build Chat Routes**: APIs for sending and receiving messages
3. **Implement Real-time Features**: Use Socket.IO for live chat
4. **Add File Upload**: Allow users to share images
5. **Create Chat Rooms**: Support for group conversations
6. **Build the Frontend**: Create a React frontend to interact with this backend

### Testing Your Knowledge:

Try these challenges to make sure you understand what you've built:

1. **Add a "forgot password" feature**
2. **Create a route to get all users**
3. **Add user status messages (like "away", "busy")**
4. **Implement user search functionality**
5. **Add user profile pictures upload**

---

## Part 15: Debugging Tips

### Common Errors and Solutions:

**1. "Cannot find module" errors:**
- Make sure you've installed all dependencies with `npm install`
- Check that your import statements use the correct file paths
- Remember to add `.js` extensions to your imports

**2. "TypeError: Cannot read property of undefined":**
- Check that you're accessing the correct properties on objects
- Make sure your request body contains the expected fields
- Verify that your middleware is running in the correct order

**3. "ValidationError" from MongoDB:**
- Check that you're sending all required fields
- Verify that your data types match the schema
- Make sure unique fields (username, email) aren't duplicated

**4. "JsonWebTokenError":**
- Verify your JWT_SECRET is set in the .env file
- Check that tokens aren't expired
- Make sure you're sending the token in the correct format

**5. Database connection issues:**
- Double-check your MongoDB connection string
- Verify your database credentials
- Make sure your IP is whitelisted in MongoDB Atlas

### Debugging Workflow:

1. **Check the console** for error messages
2. **Use console.log()** to debug variables and data flow
3. **Test each route individually** using Thunder Client
4. **Verify your database data** using MongoDB Compass
5. **Check environment variables** are loaded correctly

---

## Part 16: Advanced Features You Can Add

### 1. Password Reset Feature

```javascript
// In routes/auth.js
router.post('/forgot-password', async (req, res) => {
  // Generate reset token
  // Send email with reset link
  // Store reset token in database
});

router.post('/reset-password', async (req, res) => {
  // Verify reset token
  // Update password
  // Clear reset token
});
```

### 2. Email Verification

```javascript
// In routes/auth.js
router.post('/verify-email', async (req, res) => {
  // Verify email token
  // Update user's email verification status
});
```

### 3. User Search

```javascript
// In routes/auth.js
router.get('/search', auth, async (req, res) => {
  const { query } = req.query;
  const users = await User.find({
    $or: [
      { username: { $regex: query, $options: 'i' } },
      { email: { $regex: query, $options: 'i' } }
    ]
  }).select('-password');
  res.json({ users });
});
```

### 4. Rate Limiting

```javascript
// Install: npm install express-rate-limit
import rateLimit from 'express-rate-limit';

const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5, // limit each IP to 5 requests per windowMs
  message: 'Too many authentication attempts, please try again later.'
});

// Use in routes
router.post('/login', authLimiter, async (req, res) => {
  // login logic
});
```

---

## Congratulations! 🎉

You've successfully built a complete, production-ready backend authentication system for a chat application! This is a solid foundation that you can build upon to create a full-featured chat app.

### Key Skills You've Learned:

- **API Development**: Building RESTful APIs with Express.js
- **Database Management**: Working with MongoDB and Mongoose
- **Authentication**: Implementing JWT-based authentication
- **Security**: Password hashing, HTTP-only cookies, input validation
- **Error Handling**: Proper error responses and debugging
- **Modern JavaScript**: ES6+ modules, async/await, destructuring
- **Project Structure**: Organizing code with models, routes, middleware, and utils

### Your Backend Can Handle:

✅ User registration and login
✅ Secure password storage
✅ JWT token authentication
✅ Profile management
✅ Session management with cookies
✅ Input validation and error handling
✅ Database operations with MongoDB

The skills you've learned here are fundamental to backend development and will serve you well in future projects. You now have the knowledge to build secure, scalable backend systems!

Keep practicing, keep building, and most importantly, keep learning! 🚀

### Resources for Further Learning:

- **MongoDB Documentation**: [docs.mongodb.com](https://docs.mongodb.com)
- **Express.js Guide**: [expressjs.com](https://expressjs.com)
- **JWT Best Practices**: [auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp](https://auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp)
- **Node.js Security**: [nodejs.org/en/docs/guides/security](https://nodejs.org/en/docs/guides/security)
- **Thunder Client Documentation**: [thunderclient.com](https://thunderclient.com)

Happy coding! 💻✨