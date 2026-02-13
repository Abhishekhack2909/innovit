# ToolMandi - Quick Start Guide

## 🚀 Get Started in 5 Steps

### Step 1: Clone and Install
```bash
# You already have the code, so just install dependencies
npm install
```

### Step 2: Set Up Environment Variables
```bash
# Copy the example file
copy .env.example .env

# Open .env and fill in your values
# At minimum, you need:
# - REACT_APP_API_URL (your backend URL)
# - Firebase credentials (create new project)
```

### Step 3: Create Firebase Project
1. Go to https://console.firebase.google.com/
2. Click "Add Project"
3. Name it "ToolMandi"
4. Enable Firestore Database
5. Go to Project Settings → General
6. Scroll to "Your apps" → Web app
7. Copy the config values to your .env file

### Step 4: Set Up Backend (Choose One)

#### Option A: Use Existing Backend (Recommended for Hackathon)
```bash
# Clone the backend branch
git checkout backend

# Follow backend setup instructions
# Deploy to Heroku or Railway
# Get the URL and add to .env as REACT_APP_API_URL
```

#### Option B: Mock Backend (Quick Testing)
Create a simple mock server or use json-server:
```bash
npm install -g json-server
# Create db.json with sample data
json-server --watch db.json --port 8000
```

### Step 5: Run the App
```bash
# Make sure you're on the main branch
git checkout master

# Start the development server
npm start

# App will open at http://localhost:3000
```

---

## 🔧 Essential Changes Before Demo

### 1. Update Branding (5 minutes)
Run this search and replace in your code editor:
- Find: `Krishi Sadhan` → Replace: `ToolMandi`
- Find: `krishisadhan` → Replace: `toolmandi`
- Find: `Krishi` → Replace: `Tool` (be careful with this one)

### 2. Fix Critical Files

#### Update `src/firebase.js`:
```javascript
const app = {
    apiKey: process.env.REACT_APP_FIREBASE_API_KEY,
    authDomain: process.env.REACT_APP_FIREBASE_AUTH_DOMAIN,
    projectId: process.env.REACT_APP_FIREBASE_PROJECT_ID,
    storageBucket: process.env.REACT_APP_FIREBASE_STORAGE_BUCKET,
    messagingSenderId: process.env.REACT_APP_FIREBASE_MESSAGING_SENDER_ID,
    appId: process.env.REACT_APP_FIREBASE_APP_ID
};
```

#### Update `src/api/config.js`:
```javascript
const instance = axios.create({
  baseURL: process.env.REACT_APP_API_URL || "http://localhost:8000",
});
```

### 3. Replace Logo
- Put your ToolMandi logo in `public/logo.png`
- Put it in `src/img/logo.png` too
- Update `src/img/logo_svg.svg` if you have SVG version

---

## 🎯 Minimum Viable Demo (Without Backend)

If you don't have time to set up the backend, you can demo with:

1. **Static Data**: Modify components to use hardcoded data
2. **Mock API**: Use json-server with sample data
3. **Frontend Only**: Show UI/UX, explain backend would handle data

### Quick Mock Data Setup:
```bash
# Install json-server
npm install -g json-server

# Create db.json in root
```

```json
{
  "equipment": [
    {
      "id": 1,
      "title": "Power Drill",
      "manufacturer": "Bosch",
      "daily_rental": 500,
      "hourly_rental": 100,
      "description": "Professional power drill",
      "image_1": "https://via.placeholder.com/300"
    }
  ],
  "bookings": [],
  "users": []
}
```

```bash
# Run mock server
json-server --watch db.json --port 8000
```

---

## 📋 Pre-Demo Checklist

- [ ] .env file created with Firebase credentials
- [ ] All "Krishi Sadhan" changed to "ToolMandi"
- [ ] Logo replaced
- [ ] Backend URL configured (or mock server running)
- [ ] App runs without errors: `npm start`
- [ ] Can view equipment list
- [ ] Search works
- [ ] Voice search works (optional)
- [ ] Can view equipment details
- [ ] Booking flow works (even if just UI)

---

## 🐛 Common Issues & Fixes

### Issue: "Module not found" errors
```bash
# Delete node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

### Issue: Firebase errors
- Check .env file has correct Firebase credentials
- Make sure .env variables start with `REACT_APP_`
- Restart dev server after changing .env

### Issue: API errors
- Check REACT_APP_API_URL in .env
- Make sure backend is running
- Check browser console for CORS errors

### Issue: Port 3000 already in use
```bash
# Kill the process
# Windows:
netstat -ano | findstr :3000
taskkill /PID <PID> /F

# Or use different port:
set PORT=3001 && npm start
```

---

## 🎨 Quick UI Improvements

### Change Color Scheme:
The app uses green (#219653, #68AC5D). To change:
1. Search for `#219653` and `#68AC5D` in all files
2. Replace with your brand colors
3. Update `tailwind.config.js` if needed

### Update Categories:
Edit `src/pages/dashboard/Dashboard.jsx`:
- Change equipment categories from farming to general tools
- Update filter options

---

## 📱 Demo Tips

1. **Start with Homepage**: Show the banner and features
2. **Show Dashboard**: Demonstrate search and filters
3. **Equipment Details**: Show booking flow
4. **Voice Search**: This is a unique feature - demo it!
5. **Booking History**: Show tracking features
6. **Chat**: Explain real-time communication

---

## 🚨 Emergency Fixes

If something breaks right before demo:

1. **Comment out broken features**: Better to have working partial app
2. **Use screenshots**: Prepare screenshots of working features
3. **Explain architecture**: Focus on design and potential
4. **Show code quality**: Highlight clean code and structure

---

## 📞 Last-Minute Help

### Can't get backend working?
- Use mock data in components
- Show frontend capabilities
- Explain backend architecture

### Firebase not working?
- Comment out Firebase imports
- Remove chat features temporarily
- Focus on other features

### Build failing?
- Run `npm start` instead of build
- Demo from development mode
- Fix errors one by one using console

---

## 🎯 Hackathon Presentation Points

1. **Problem Statement**: Tool rental marketplace
2. **Solution**: Web platform connecting tool owners and renters
3. **Key Features**:
   - Voice search (unique!)
   - Real-time booking
   - Chat system
   - Advanced filters
4. **Tech Stack**: React, Redux, Firebase, Django
5. **Future Scope**: Payment integration, ratings, delivery

Good luck with your hackathon! 🚀
