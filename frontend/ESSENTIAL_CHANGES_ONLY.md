# Essential Changes to Make This YOUR Project (ToolMandi)

## 🎯 MUST DO - Critical Changes (2-3 hours)

### 1. CREATE .env FILE (10 minutes)
```bash
# Copy the example file
copy .env.example .env
```

**Edit .env and add:**
```env
# Backend API - Use existing or deploy your own
REACT_APP_API_URL=https://krishi-sadhan-app.herokuapp.com

# Create NEW Firebase project (MUST DO - current keys are exposed publicly)
REACT_APP_FIREBASE_API_KEY=your_new_key
REACT_APP_FIREBASE_AUTH_DOMAIN=toolmandi-xxxxx.firebaseapp.com
REACT_APP_FIREBASE_PROJECT_ID=toolmandi-xxxxx
REACT_APP_FIREBASE_STORAGE_BUCKET=toolmandi-xxxxx.appspot.com
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
REACT_APP_FIREBASE_APP_ID=your_app_id
```

**How to get Firebase credentials:**
1. Go to https://console.firebase.google.com/
2. Click "Create Project" → Name it "ToolMandi"
3. Enable Firestore Database
4. Go to Project Settings → Scroll down → Copy config values

---

### 2. UPDATE FIREBASE CONFIG (5 minutes)

**File: `src/firebase.js`**

Replace entire file with:
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

---

### 3. UPDATE API CONFIGURATION (10 minutes)

**File: `src/api/config.js`**
```javascript
import axios from "axios";
import Cookies from "js-cookie";
import { renewAccessToken } from "./authAPI";

const instance = axios.create({
  baseURL: process.env.REACT_APP_API_URL || "https://krishi-sadhan-app.herokuapp.com",
});

// ... rest of the file stays the same
```

**File: `src/api/authAPI.js`**
```javascript
import instance from "./config";
import Cookies from "js-cookie";

// Remove this line:
// const url = "https://krishi-sadhan-app.herokuapp.com";

// Use this instead:
const url = process.env.REACT_APP_API_URL;

// ... rest of the file stays the same
```

**File: `src/api/bookingAPI.js`**
```javascript
import instance from "./config";
import Cookies from "js-cookie";
import axios from "axios";

// Remove this line:
// const url = "https://krishi-sadhan-app.herokuapp.com";

// Use this instead:
const url = process.env.REACT_APP_API_URL;

// ... rest of the file stays the same

// Line 91: Change this:
// await axios.patch(`https://krishi-sadhan-app.herokuapp.com/api/booking/update/${id}/`

// To this:
await axios.patch(`${process.env.REACT_APP_API_URL}/api/booking/update/${id}/`
```

**File: `src/api/profileAPI.js`**
```javascript
import instance from "./config";

// Remove this line:
// const url = "https://krishi-sadhan-app.herokuapp.com";

// Use this instead:
const url = process.env.REACT_APP_API_URL;

// ... rest of the file stays the same
```

**File: `src/pages/bookingRequest/BookingRequest.jsx`**

Find line 54 and change:
```javascript
// FROM:
await axios.patch(`https://krishi-sadhan-app.herokuapp.com/api/booking/update/${id}/`

// TO:
await axios.patch(`${process.env.REACT_APP_API_URL}/api/booking/update/${id}/`
```

---

### 4. BRANDING CHANGES (30 minutes)

#### A. Update package.json
```json
{
  "name": "toolmandi",
  "version": "1.0.0",
  ...
}
```

#### B. Update public/index.html
Change these lines:
- Line 8: `<meta name="description" content="ToolMandi - Your Tool Rental Marketplace" />`
- Line 9: `<meta name="author" content="@toolmandi" />`
- Line 10: `<meta itemprop="name" content="ToolMandi" />`
- Line 11: `<meta itemprop="description" content="ToolMandi - Rent Tools Easily" />`
- Line 13: `<meta name="twitter:site" content="@toolmandi" />`
- Line 14: `<meta name="twitter:title" content="ToolMandi" />`
- Line 15: `<meta name="twitter:description" content="ToolMandi - Your Tool Rental Marketplace" />`
- Line 16: `<meta name="twitter:creator" content="@toolmandi" />`
- Line 17: `<meta property="og:title" content="ToolMandi" />`
- Line 19: `<meta property="og:url" content="https://toolmandi.netlify.app/" />`
- Line 20: `<meta property="og:description" content="ToolMandi - Your Tool Rental Marketplace" />`
- Line 21: `<meta property="og:site_name" content="ToolMandi" />`
- Line 27: `<title>ToolMandi - Rent Tools Easily</title>`

#### C. Update public/manifest.json
```json
{
    "short_name": "ToolMandi",
    "name": "ToolMandi - Tool Rental Platform",
    ...
}
```

#### D. Update Component Text (Find & Replace)

**Use your code editor's Find & Replace (Ctrl+Shift+H):**

1. Find: `Krishi Sadhan` → Replace: `ToolMandi` (in all files)
2. Find: `krishisadhan` → Replace: `toolmandi` (in all files)
3. Find: `Krishi <br /> Sadhan` → Replace: `Tool<br />Mandi` (in Header.jsx and Footer.jsx)

**Specific files to check:**
- `src/components/header/Header.jsx` - Line 36
- `src/components/footer/Footer.jsx` - Line 23
- `src/components/homeComponent/banner/Banner.jsx` - Line 83
- `src/components/homeComponent/workflow/Workflow.jsx` - Line 10
- `src/components/homeComponent/support/Support.jsx` - Line 14
- `src/components/homeComponent/services/Services.jsx` - Line 11
- `src/pages/ContactUs/ContactUs.jsx` - Line 35
- `src/pages/FAQ.js` - Multiple instances
- `src/pages/Help.js` - Multiple instances

#### E. Update Contact Email
**File: `src/pages/ContactUs/ContactUs.jsx`**
Line 35: Change email from `krishisadhan7@gmail.com` to your email

---

### 5. FIX CHAT REDIRECT (2 minutes)

**File: `src/pages/product/Product.jsx`**

Line 95-97, change:
```javascript
// FROM:
const redirect = () => {
    window.location.href = 'http://localhost:3001/login';
    return null;
}

// TO:
const redirect = () => {
    navigate('/chat');
    return null;
}
```

---

### 6. REPLACE LOGO (15 minutes)

**Create or download a ToolMandi logo, then replace:**
- `public/logo.png`
- `src/img/logo.png`
- `src/img/logo_svg.svg`

**Quick logo options:**
1. Use Canva (free): https://www.canva.com/
2. Use LogoMakr: https://logomakr.com/
3. Use AI: "Create a logo for ToolMandi, a tool rental platform"

---

## 🎨 OPTIONAL - Nice to Have (1-2 hours)

### 7. Update Welcome Message

**File: `src/components/homeComponent/banner/Banner.jsx`**
```javascript
// Line 83-86: Change from:
<p className="text-2xl font-normal text-white">
  Namaste, welcome to Krishi Sadhan.
</p>
<h1 className="text-4xl font-bold text-white">
  <span className="text-[#219653]">Farmer's Eqipments</span> at
  reasonable <br /> and affordable prices.
</h1>

// To:
<p className="text-2xl font-normal text-white">
  Welcome to ToolMandi
</p>
<h1 className="text-4xl font-bold text-white">
  <span className="text-[#219653]">Tools & Equipment</span> at
  reasonable <br /> and affordable prices.
</h1>
```

### 8. Update Categories (Optional)

**File: `src/pages/dashboard/Dashboard.jsx`**

The categories come from backend API, but you can update the brands:
```javascript
// Around line 180-182, change:
<Dropdown title="Mahindra" />
<Dropdown title="John Deere" />
<Dropdown title="CLAAS India" />

// To:
<Dropdown title="Bosch" />
<Dropdown title="DeWalt" />
<Dropdown title="Makita" />
```

---

## ✅ TESTING CHECKLIST

After making changes, test:

```bash
# 1. Install dependencies (if not done)
npm install

# 2. Start the app
npm start

# 3. Check these:
```

- [ ] App loads without errors
- [ ] No Firebase errors in console
- [ ] "ToolMandi" appears in header/footer
- [ ] Logo displays correctly
- [ ] Can view equipment list
- [ ] Search works
- [ ] Can view equipment details
- [ ] Chat page loads (even if not functional)

---

## 🚀 DEPLOYMENT (30 minutes)

### Deploy to Netlify (Recommended)

1. **Build the app:**
```bash
npm run build
```

2. **Deploy to Netlify:**
   - Go to https://app.netlify.com/
   - Drag & drop the `build` folder
   - OR connect GitHub repo

3. **Add Environment Variables in Netlify:**
   - Site Settings → Environment Variables
   - Add all variables from your .env file

4. **Custom Domain (Optional):**
   - Buy domain: toolmandi.com
   - Add to Netlify settings

---

## 📋 FINAL CHECKLIST

Before submitting/presenting:

- [ ] .env file created with Firebase credentials
- [ ] All API URLs use environment variables
- [ ] Firebase config uses environment variables
- [ ] All "Krishi Sadhan" changed to "ToolMandi"
- [ ] Logo replaced
- [ ] Contact email updated
- [ ] Chat redirect fixed
- [ ] App runs: `npm start`
- [ ] No console errors
- [ ] Tested on mobile view
- [ ] README.md updated with your info
- [ ] Deployed to Netlify/Vercel

---

## 🎯 TIME BREAKDOWN

| Task | Time | Priority |
|------|------|----------|
| Create .env file | 10 min | CRITICAL |
| Update Firebase config | 5 min | CRITICAL |
| Update API configs | 10 min | CRITICAL |
| Branding changes | 30 min | HIGH |
| Fix chat redirect | 2 min | MEDIUM |
| Replace logo | 15 min | HIGH |
| Testing | 20 min | HIGH |
| Deployment | 30 min | HIGH |
| **TOTAL** | **~2 hours** | |

---

## 🆘 QUICK HELP

### If app won't start:
```bash
rm -rf node_modules package-lock.json
npm install
npm start
```

### If Firebase errors:
- Check .env file exists
- Check all variables start with `REACT_APP_`
- Restart dev server after .env changes

### If API errors:
- Check REACT_APP_API_URL in .env
- For now, use: `https://krishi-sadhan-app.herokuapp.com`
- Later, deploy your own backend

---

## 📞 READY TO START?

1. Create .env file
2. Get Firebase credentials
3. Update firebase.js
4. Find & Replace "Krishi Sadhan" → "ToolMandi"
5. Replace logo
6. Test: `npm start`
7. Deploy!

**That's it! You now have YOUR project: ToolMandi! 🎉**
