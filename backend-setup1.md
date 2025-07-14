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
  server/
    models/        (database schemas)
    routes/        (API endpoints)
    middleware/    (authentication helpers)
    server.js      (main server file)
    package.json   (project information)
    .env          (secret configuration)
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
mkdir server
cd server
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

### Step 4: Install Required Packages

We need to install several "packages" (pre-written code that helps us):

```bash
npm install express mongoose socket.io cors dotenv bcryptjs jsonwebtoken
```

**What each package does:**
- **express** - Framework for building web servers
- **mongoose** - Makes working with MongoDB database easier
- **socket.io** - Enables real-time features for chat
- **cors** - Allows frontend and backend to communicate
- **dotenv** - Manages environment variables (secrets)
- **bcryptjs** - Encrypts passwords for security
- **jsonwebtoken** - Creates secure login tokens

Now install development tools:

```bash
npm install -D nodemon
```

**What nodemon does:**
- Automatically restarts your server when you make code changes
- The `-D` means it's only needed during development

### Step 5: Set Up Development Scripts

Open `package.json` in your text editor and find the "scripts" section. Replace it with this:

```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

**What these scripts do:**
- `npm start` - Runs your server normally
- `npm run dev` - Runs your server with auto-restart when files change

---

## Part 3: Creating Your First Server

### Step 1: Create the Main Server File

In your server folder, create a new file called `server.js` and add this code:

```javascript
// Import required packages
const express = require('express');

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

**Line 2**: `require('express')` imports the Express package
**Line 5**: `express()` creates a new web application
**Line 8**: `PORT = 5000` sets our server to run on port 5000
**Line 11-13**: Creates a route that responds when someone visits our server
**Line 16-18**: Starts the server and tells us it's running

### Step 2: Test Your Server

In your terminal, run:

```bash
npm run dev
```

You should see: `Server is running on http://localhost:5000`

Open your web browser and go to `http://localhost:5000`

You should see: "Welcome to our Chat App Server!"

**Congratulations! Your server is working!**

### Step 3: Add Middleware for JSON Handling

Middleware is code that runs before your route handlers. Add this to your `server.js` after creating the app:

```javascript
// Import required packages
const express = require('express');
const cors = require('cors');

// Create Express application
const app = express();

// Middleware - code that runs before our routes
app.use(express.json()); // Allows server to understand JSON data
app.use(cors()); // Allows frontend to communicate with backend

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
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
require('dotenv').config(); // Load environment variables

// Create Express application
const app = express();

// Middleware
app.use(express.json());
app.use(cors());

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
- `require('dotenv').config()` - Loads our environment variables
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
const mongoose = require('mongoose');

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
module.exports = mongoose.model('User', userSchema);
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

## Part 6: Building User Registration (Sign-up)

### Step 1: Create Routes Folder

```bash
mkdir routes
```

### Step 2: Create Authentication Routes File

Create `routes/auth.js`:

```javascript
// Import required packages
const express = require('express');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const User = require('../models/User');

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
    
    // Create JWT token for authentication
    const token = jwt.sign(
      { userId: savedUser._id },           // Payload (user ID)
      process.env.JWT_SECRET,              // Secret key
      { expiresIn: '24h' }                 // Token expires in 24 hours
    );
    
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

// Export the router
module.exports = router;
```

**Understanding this code:**

1. **Input validation** - We check if all required fields are provided
2. **Duplicate check** - We make sure username and email aren't already taken
3. **Password hashing** - We encrypt the password using bcrypt (never store plain passwords!)
4. **Database save** - We save the new user to MongoDB
5. **JWT token** - We create a token that proves the user is authenticated
6. **Response** - We send back user info and token (but never the password)

### Step 3: Connect Routes to Main Server

Update your `server.js` to use the auth routes:

```javascript
// Import required packages
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
require('dotenv').config();

// Import routes
const authRoutes = require('./routes/auth');

// Create Express application
const app = express();

// Middleware
app.use(express.json());
app.use(cors());

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
- Imported our auth routes
- Used `app.use('/api/auth', authRoutes)` to make signup available at `/api/auth/signup`

### Step 4: Test User Registration

You can test this using a tool like Postman, Thunder Client (VS Code extension), or even curl:

**Using curl (in terminal):**
```bash
curl -X POST http://localhost:5000/api/auth/signup \
  -H "Content-Type: application/json" \
  -d '{"username":"johndoe","email":"john@example.com","password":"password123"}'
```

**Using Postman:**
1. Create a new POST request
2. URL: `http://localhost:5000/api/auth/signup`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "password123"
}
```

You should get a response like:
```json
{
  "success": true,
  "message": "User created successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "61f123456789abcdef123456",
    "username": "johndoe",
    "email": "john@example.com",
    "avatar": "https://via.placeholder.com/150/4A90E2/FFFFFF?text=User",
    "isOnline": false
  }
}
```

---

## Part 7: Building User Login

### Add Login Route to auth.js

Add this code to your `routes/auth.js` file (after the signup route):

```javascript
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
    
    // Create JWT token
    const token = jwt.sign(
      { userId: user._id },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
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
```

**Understanding this code:**

1. **Find user** - We search for a user with the provided email
2. **Password check** - We use bcrypt to compare the provided password with the stored hash
3. **Update status** - We mark the user as online
4. **Create token** - We generate a new JWT token for this login session
5. **Security note** - We return the same "Invalid email or password" message whether the email doesn't exist or the password is wrong (this prevents attackers from knowing which emails are registered)

### Test Login

**Using curl:**
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"password123"}'
```

**Using Postman:**
- POST request to `http://localhost:5000/api/auth/login`
- Body:
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

---

## Part 8: Creating Authentication Middleware

Authentication middleware is code that checks if a user is logged in before allowing them to access certain routes.

### Step 1: Create Middleware Folder

```bash
mkdir middleware
```

### Step 2: Create Authentication Middleware

Create `middleware/auth.js`:

```javascript
// Import required packages
const jwt = require('jsonwebtoken');
const User = require('../models/User');

// Authentication middleware function
const auth = async (req, res, next) => {
  try {
    // Get token from Authorization header
    const authHeader = req.header('Authorization');
    
    // Check if Authorization header exists
    if (!authHeader) {
      return res.status(401).json({
        success: false,
        message: 'No authorization header, access denied'
      });
    }
    
    // Extract token (remove "Bearer " prefix)
    const token = authHeader.replace('Bearer ', '');
    
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
module.exports = auth;
```

**Understanding this middleware:**

1. **Get token** - We extract the JWT token from the Authorization header
2. **Verify token** - We check if the token is valid and not expired
3. **Find user** - We get the user data based on the user ID in the token
4. **Add to request** - We attach the user data to the request so routes can use it
5. **Continue** - We call `next()` to proceed to the actual route handler

The `.select('-password')` means "get all user fields except the password."

---

## Part 9: Building Logout and Profile Update

### Step 1: Add Logout Route

Add this to your `routes/auth.js` file:

```javascript
// Import the auth middleware at the top of the file
const auth = require('../middleware/auth');

// Logout route - POST /api/auth/logout
router.post('/logout', auth, async (req, res) => {
  try {
    // Update user's online status to false
    await User.findByIdAndUpdate(req.user._id, {
      isOnline: false
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
```

**Understanding logout:**
- We use the `auth` middleware to ensure only logged-in users can logout
- We set the user's `isOnline` status to false
- The frontend will remove the token from storage

### Step 2: Add Profile Update Route

Add this to your `routes/auth.js` file:

```javascript
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
```

### Step 3: Add Get Current User Route

This route allows the frontend to get current user information:

```javascript
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
```

### Step 4: Complete auth.js File

Here's your complete `routes/auth.js` file:

```javascript
// Import required packages
const express = require('express');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const User = require('../models/User');
const auth = require('../middleware/auth');

// Create router
const router = express.Router();

// Sign-up route - POST /api/auth/signup
router.post('/signup', async (req, res) => {
  // ... (signup code from earlier)
});

// Login route - POST /api/auth/login
router.post('/login', async (req, res) => {
  // ... (login code from earlier)
});

// Logout route - POST /api/auth/logout
router.post('/logout', auth, async (req, res) => {
  // ... (logout code from above)
});

// Get current user route - GET /api/auth/me
router.get('/me', auth, async (req, res) => {
  // ... (get user code from above)
});

// Update profile route - PUT /api/auth/profile
router.put('/profile', auth, async (req, res) => {
  // ... (profile update code from above)
});

// Export the router
module.exports = router;
```

---

## Part 10: Testing Your Complete Backend

### Test All Routes

1. **Test Signup** (POST /api/auth/signup)
```json
{
  "username": "testuser",
  "email": "test@example.com",
  "password": "password123"
}
```

2. **Test Login** (POST /api/auth/login)
```json
{
  "email": "test@example.com",
  "password": "password123"
}
```

3. **Test Get Current User** (GET /api/auth/me)
- Add Authorization header: `Bearer YOUR_JWT_TOKEN`

4. **Test Update Profile** (PUT /api/auth/profile)
- Add Authorization header: `Bearer YOUR_JWT_TOKEN`
```json
{
  "username": "newusername",
  "avatar": "https://example.com/new-avatar.jpg"
}
```

5. **Test Logout** (POST /api/auth/logout)
- Add Authorization header: `Bearer YOUR_JWT_TOKEN`

### Common Testing Issues and Solutions

**Issue: "Cannot POST /api/auth/signup"**
- Solution: Make sure your server is running with
