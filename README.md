# sojith
Here’s a **5-page detailed README file** (in plain text/Markdown format) for your **Node.js backend contact form project** — explaining setup, architecture, technologies, and deployment in depth.

---

# 📘 README: Node.js Backend Contact Form

## 🧩 Page 1 — Project Overview

### Project Title

**Node.js Contact Form Backend**

### Introduction

The **Node.js Contact Form Backend** is a simple yet powerful web application backend that handles user messages submitted through a contact form on a website. It processes form data (name, email, message) and sends it to an administrator via email or stores it in a database.
This project demonstrates backend development concepts such as:

* HTTP request handling
* Form validation
* RESTful API creation
* Integration with frontend (HTML/JS)
* Email sending using **Nodemailer**
* Data storage using **MongoDB** (optional)

### Objective

To build a lightweight and efficient backend service using **Node.js** and **Express.js** that can:

1. Accept POST requests from a frontend form.
2. Validate incoming data.
3. Send email notifications.
4. Optionally save form data to a database.

### Key Features

* Contact form API endpoint (`/contact`)
* Email integration (via Nodemailer)
* Input validation with **Express Validator**
* Cross-Origin Resource Sharing (CORS) support
* Error handling and response management
* Scalable and secure code structure

---

## 🧱 Page 2 — Project Structure & Technologies

### Folder Structure

```
contact-form-backend/
│
├── package.json
├── server.js
├── .env
├── /routes
│   └── contactRoutes.js
├── /controllers
│   └── contactController.js
├── /models
│   └── contactModel.js
└── /config
    └── db.js
```

### Technologies Used

| Technology             | Purpose                                  |
| ---------------------- | ---------------------------------------- |
| **Node.js**            | Backend runtime environment              |
| **Express.js**         | Web framework for routing and middleware |
| **Nodemailer**         | Send emails using SMTP                   |
| **MongoDB / Mongoose** | Database for storing form submissions    |
| **dotenv**             | Environment variable management          |
| **body-parser**        | Parse incoming form data                 |
| **CORS**               | Enable cross-origin requests             |

### Environment Variables (`.env`)

```
PORT=5000
EMAIL_USER=your_email@example.com
EMAIL_PASS=your_password
RECEIVER_EMAIL=admin@example.com
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/contact
```

### API Endpoint

**POST /contact**

* Accepts: `{ name, email, message }`
* Returns: `{ success: true, msg: "Message sent successfully" }`

---

## ⚙️ Page 3 — Setup and Installation

### Prerequisites

Ensure you have installed:

* Node.js (v14 or above)
* npm or yarn
* MongoDB (optional, for data storage)
* A working email account (Gmail, Outlook, etc.)

### Step-by-Step Setup

#### Step 1 — Clone the repository

```bash
git clone https://github.com/yourusername/contact-form-backend.git
cd contact-form-backend
```

#### Step 2 — Install dependencies

```bash
npm install
```

#### Step 3 — Create `.env` file

Create a `.env` file in the root directory and fill in:

```bash
PORT=5000
EMAIL_USER=your_email@example.com
EMAIL_PASS=your_password
RECEIVER_EMAIL=admin@example.com
MONGO_URI=your_mongodb_connection_string
```

#### Step 4 — Run the server

```bash
npm start
```

You should see:

```
Server running on http://localhost:5000
Connected to MongoDB
```

#### Step 5 — Connect Frontend

In your frontend contact form (HTML or React app), send a `POST` request:

```js
fetch('http://localhost:5000/contact', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    name: 'John Doe',
    email: 'john@example.com',
    message: 'Hello, this is a test!'
  })
});
```

---

## 🧠 Page 4 — Code Explanation & Functionality

### 1. **server.js**

The entry point of the application.

```js
const express = require('express');
const dotenv = require('dotenv');
const cors = require('cors');
const contactRoutes = require('./routes/contactRoutes');
const connectDB = require('./config/db');

dotenv.config();
const app = express();
app.use(cors());
app.use(express.json());

connectDB();
app.use('/contact', contactRoutes);

app.listen(process.env.PORT, () => {
  console.log(`Server running on port ${process.env.PORT}`);
});
```

### 2. **contactRoutes.js**

Handles routing for `/contact`.

```js
const express = require('express');
const { sendMessage } = require('../controllers/contactController');
const router = express.Router();

router.post('/', sendMessage);
module.exports = router;
```

### 3. **contactController.js**

Manages form submission logic — sending emails and saving data.

```js
const nodemailer = require('nodemailer');
const Contact = require('../models/contactModel');

exports.sendMessage = async (req, res) => {
  const { name, email, message } = req.body;
  try {
    const transporter = nodemailer.createTransport({
      service: 'gmail',
      auth: {
        user: process.env.EMAIL_USER,
        pass: process.env.EMAIL_PASS,
      },
    });

    await transporter.sendMail({
      from: email,
      to: process.env.RECEIVER_EMAIL,
      subject: `New Contact Message from ${name}`,
      text: message,
    });

    const newMessage = new Contact({ name, email, message });
    await newMessage.save();

    res.status(200).json({ success: true, msg: 'Message sent successfully!' });
  } catch (err) {
    res.status(500).json({ success: false, msg: 'Server error', error: err.message });
  }
};
```

### 4. **contactModel.js**

Defines the schema for storing contact form submissions.

```js
const mongoose = require('mongoose');

const contactSchema = new mongoose.Schema({
  name: String,
  email: String,
  message: String,
  date: { type: Date, default: Date.now },
});

module.exports = mongoose.model('Contact', contactSchema);
```

---

## 🚀 Page 5 — Deployment & Maintenance

### Deployment Options

You can deploy the Node.js backend using:

* **Render** – easy for small projects
* **Vercel (Serverless Functions)**
* **Railway.app**
* **Heroku (legacy)**
* **AWS EC2 / DigitalOcean** – for full control

### Deployment Steps (Example: Render)

1. Push your project to GitHub.
2. Go to [https://render.com](https://render.com).
3. Create a new Web Service.
4. Link your GitHub repo.
5. Add environment variables (from `.env`).
6. Click **Deploy**.

### Security Tips

* Never commit `.env` files.
* Use strong passwords for your email credentials.
* Implement rate limiting to avoid spam.
* Validate inputs to prevent injection attacks.

### Future Enhancements

* Add **Google reCAPTCHA** to reduce spam.
* Add **file upload** support for resumes or attachments.
* Create an **admin dashboard** to view submissions.
* Use **JWT authentication** for secure routes.

### Conclusion

The Node.js Contact Form backend is a robust, reusable backend system ideal for personal or business portfolio websites. It demonstrates how to handle form submissions securely, validate data, send notifications, and connect frontend and backend seamlessly.

---

Would you like me to **format this README as a downloadable Word document (5 pages)** for you? I can generate a `.docx` file with headings, code blocks, and clear formatting.
