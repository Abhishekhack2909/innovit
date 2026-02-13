# ToolMandi Migration Checklist

## 🔴 CRITICAL CHANGES REQUIRED

### 1. Environment Variables (CREATE .env FILE)
Currently, all sensitive data is hardcoded. You MUST create a `.env` file:

```env
# Backend API URL
REACT_APP_API_URL=https://your-new-backend.herokuapp.com
# OR for local development:
# REACT_APP_API_URL=http://localhost:8000

# Firebase Configuration (Create NEW Firebase project for ToolMandi)
REACT_APP_FIREBASE_API_KEY=your_firebase_api_key
REACT_APP_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
REACT_APP_FIREBASE_PROJECT_ID=your_project_id
REACT_APP_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
REACT_APP_FIREBASE_APP_ID=your_app_id

# EmailJS (for contact form)
REACT_APP_EMAILJS_SERVICE_ID=your_service_id
REACT_APP_EMAILJS_TEMPLATE_ID=your_template_id
REACT_APP_EMAILJS_PUBLIC_KEY=your_public_key

# Chat Engine (for chat functionality)
REACT_APP_CHAT_ENGINE_PROJECT_ID=your_chat_project_id
REACT_APP_CHAT_ENGINE_PRIVATE_KEY=your_private_key
```

---

## 📝 FILES TO MODIFY

### 2. API Configuration Files

#### `src/api/config.js`
```javascript
// CHANGE FROM:
baseURL: "https://krishi-sadhan-app.herokuapp.com"

// CHANGE TO:
baseURL: process.env.REACT_APP_API_URL || "http://localhost:8000"
```

#### `src/api/authAPI.js`
```javascript
// REMOVE hardcoded URL:
const url = "https://krishi-sadhan-app.herokuapp.com";

// USE environment variable instead:
const url = process.env.REACT_APP_API_URL;
```

#### `src/api/bookingAPI.js`
```javascript
// Line 6: REMOVE
const url = "https://krishi-sadhan-app.herokuapp.com";

// Line 91: CHANGE
await axios.patch(`https://krishi-sadhan-app.herokuapp.com/api/booking/update/${id}/`

// TO:
await axios.patch(`${process.env.REACT_APP_API_URL}/api/booking/update/${id}/`
```

#### `src/api/profileAPI.js`
```javascript
// Line 5: CHANGE
const url = "https://krishi-sadhan-app.herokuapp.com";

// TO:
const url = process.env.REACT_APP_API_URL;
```

#### `src/pages/bookingRequest/BookingRequest.jsx`
```javascript
// Line 39 & 54: CHANGE
`https://krishi-sadhan-app.herokuapp.com/api/booking/...`

// TO:
`${process.env.REACT_APP_API_URL}/api/booking/...`
```

---

### 3. Firebase Configuration

#### `src/firebase.js` - COMPLETE REWRITE NEEDED
```javascript
import Firebase from 'firebase/compat/app';
import 'firebase/compat/firestore';

const app = {
    apiKey: process.env.REACT_APP_FIREBASE_API_KEY,
    authDomain: process.env.REACT_APP_FIREBASE_AUTH_DOMAIN,
    projectId: process.env.REACT_APP_FIREBASE_PROJECT_ID,
    storageBucket: process.env.REACT_APP_FIREBASE_STORAGE_BUCKET,
    messagingSenderId: process.env.REACT_APP_FIREBASE_MESSAGING_SENDER_ID,
    appId: process.env.REACT_APP_FIREBASE_APP_ID
};

const FirebaseApp = Firebase.initializeApp(app);
const db = FirebaseApp.firestore();

export default db;
```

**ACTION REQUIRED:**
1. Go to https://console.firebase.google.com/
2. Create NEW project called "ToolMandi"
3. Get new credentials
4. Add to .env file

---

### 4. Branding Changes (Krishi Sadhan → ToolMandi)

#### `public/index.html`
- Line 8: Change description to "ToolMandi - Your Tool Rental Marketplace"
- Line 9: Change author to "@toolmandi"
- Line 10-11: Update meta tags
- Line 13-21: Update all social media tags
- Line 27: Change title to "ToolMandi - Rent Tools Easily"

#### `public/manifest.json`
```json
{
    "short_name": "ToolMandi",
    "name": "ToolMandi - Tool Rental Platform",
    ...
}
```

#### `package.json`
```json
{
  "name": "toolmandi",
  "version": "1.0.0",
  ...
}
```

#### Text Changes in Components:

**`src/components/homeComponent/banner/Banner.jsx`**
- Line 83: "Namaste, welcome to Krishi Sadhan" → "Welcome to ToolMandi"
- Line 86: "Farmer's Eqipments" → "Tools & Equipment"

**`src/components/homeComponent/workflow/Workflow.jsx`**
- Line 10: "How KRISHI SADHAN works?" → "How ToolMandi Works?"

**`src/components/homeComponent/support/Support.jsx`**
- Line 14: "Being a part of Krishi Sadhan" → "Being a part of ToolMandi"

**`src/components/homeComponent/services/Services.jsx`**
- Line 11: "Krishi Sadhan market provides for Farmers" → "ToolMandi provides"

**`src/components/header/Header.jsx`**
- Line 36: "Krishi Sadhan" → "ToolMandi"

**`src/components/footer/Footer.jsx`**
- Line 23: "Krishi Sadhan" → "ToolMandi"

**`src/pages/ContactUs/ContactUs.jsx`**
- Line 35: Email "krishisadhan7@gmail.com" → "contact@toolmandi.com"

**`src/pages/FAQ.js`**
- Replace all 15+ instances of "Krishi Sadhan" with "ToolMandi"

**`src/pages/Help.js`**
- Line 9: "How do I book an equipment on Krishi Sadhan" → "ToolMandi"
- Line 37: Update references

---

### 5. Chat Redirect Fix

#### `src/pages/product/Product.jsx`
```javascript
// Line 95: CHANGE
window.location.href = 'http://localhost:3001/login';

// TO:
navigate('/chat'); // or your actual chat route
```

---

### 6. Logo & Images

**REQUIRED ACTIONS:**
1. Create new ToolMandi logo
2. Replace `public/logo.png`
3. Replace `src/img/logo.png`
4. Replace `src/img/logo_svg.svg`
5. Update favicon

---

## 🔧 BACKEND SETUP REQUIRED

### 7. Backend Deployment

You need to either:

**Option A: Deploy existing backend**
1. Clone backend branch: `git checkout backend`
2. Deploy to Heroku/Railway/Render
3. Update .env with new URL

**Option B: Create new backend**
1. Set up Django REST Framework
2. Implement all API endpoints
3. Set up PostgreSQL database
4. Deploy and get URL

**Required API Endpoints:**
- `/api/auth/register/` - User registration
- `/api/auth/login/` - User login
- `/api/auth/verify-otp/` - OTP verification
- `/api/profile/` - User profile
- `/api/equipment/` - List equipment
- `/api/equipment/create/` - Add equipment
- `/api/equipment/{id}/` - Equipment details
- `/api/equipment_type/` - Equipment categories
- `/api/brand/` - Equipment brands
- `/api/booking/` - List bookings
- `/api/booking/create/` - Create booking
- `/api/booking/request/` - Booking requests
- `/api/booking/update/{id}/` - Update booking status
- `/enquiry/feedback/` - Submit feedback
- `/enquiry/report-equipment/` - Report equipment

---

## 🎨 OPTIONAL ENHANCEMENTS

### 8. Category Updates (Expand beyond farming)

#### `src/pages/dashboard/Dashboard.jsx`
Update categories from farming-specific to general:
- Crop Protection → Power Tools
- Harvesting Equipment → Construction Equipment
- Add: Gardening Tools, Home Improvement, etc.

---

## ✅ TESTING CHECKLIST

After making changes, test:

- [ ] Registration works
- [ ] Login with email works
- [ ] Login with OTP works
- [ ] Equipment listing displays
- [ ] Search functionality works
- [ ] Voice search works
- [ ] Filters work (price, distance, date)
- [ ] Equipment details page loads
- [ ] Booking creation works
- [ ] Booking history displays
- [ ] Chat functionality works
- [ ] Profile update works
- [ ] Add equipment works
- [ ] Feedback submission works
- [ ] All images load correctly
- [ ] Mobile responsiveness

---

## 🚀 DEPLOYMENT CHECKLIST

### 9. Before Deploying

- [ ] Create .env file with all variables
- [ ] Add .env to .gitignore (already done)
- [ ] Update all API URLs to use environment variables
- [ ] Create new Firebase project
- [ ] Set up backend and get URL
- [ ] Update all branding references
- [ ] Replace logo and images
- [ ] Test locally with `npm start`
- [ ] Build production: `npm run build`
- [ ] Deploy to Netlify/Vercel
- [ ] Set environment variables in hosting platform
- [ ] Test production deployment

---

## 📧 EXTERNAL SERVICES TO SET UP

### 10. Third-Party Services

**Firebase (Chat & Real-time features):**
- Create project at https://console.firebase.google.com/
- Enable Firestore Database
- Get credentials

**EmailJS (Contact form):**
- Sign up at https://www.emailjs.com/
- Create email template
- Get service ID, template ID, public key

**Chat Engine (Optional - for chat):**
- Sign up at https://chatengine.io/
- Create project
- Get project ID and private key

---

## ⚠️ SECURITY NOTES

1. **NEVER commit .env file to Git**
2. **Regenerate all API keys** (don't use the exposed Firebase keys)
3. **Set up CORS** properly on backend
4. **Use HTTPS** for production
5. **Implement rate limiting** on backend
6. **Add input validation** everywhere
7. **Sanitize user inputs** to prevent XSS

---

## 📱 CONTACT INFORMATION TO UPDATE

- Email: krishisadhan7@gmail.com → contact@toolmandi.com
- Social media handles: @krishisadhan → @toolmandi
- Website URL: krishisadhan.netlify.app → toolmandi.com

---

## 🎯 PRIORITY ORDER

1. **HIGH PRIORITY (Must do before hackathon):**
   - Create .env file
   - Update all API URLs to use environment variables
   - Create new Firebase project
   - Deploy/set up backend
   - Change all "Krishi Sadhan" to "ToolMandi"
   - Replace logo

2. **MEDIUM PRIORITY:**
   - Update categories for general tools
   - Fix chat redirect
   - Update contact information
   - Test all features

3. **LOW PRIORITY (Nice to have):**
   - Add new tool categories
   - Improve UI/UX
   - Add more features

---

## 📞 NEED HELP?

Common issues:
- **CORS errors**: Configure backend CORS settings
- **Firebase errors**: Check credentials in .env
- **API not responding**: Verify backend is running
- **Build errors**: Check all imports and environment variables
