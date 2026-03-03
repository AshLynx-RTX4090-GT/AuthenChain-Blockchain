# Login to Dashboard Integration Guide

## Overview
The login system now seamlessly redirects users from the beautiful AuthenChain login page (`frontend.html`) to the interactive cryptocurrency dashboard after successful authentication.

## Login Flow

### 1. **User Enters Credentials** 
   - Navigate to `frontend.html` (AuthenChain Login Page)
   - User enters email and password
   - Optionally checks "Remember me"

### 2. **Validation & Authentication**
   - Form validates that all fields are filled
   - Login details are stored in `sessionStorage`
   - If "Remember me" is checked, email is saved in `localStorage`

### 3. **Welcome Screen**
   - Beautiful welcome message appears
   - Shows "Redirecting to dashboard..." message
   - Waits 1.5 seconds before navigation

### 4. **Redirect to Dashboard**
   - Automatically redirects to the React cryptocurrency dashboard
   - User data is persisted and accessible throughout the app
   - Dashboard displays user's email in the header

## File Structure

```
Frontend (HTML/JavaScript):
├── frontend.html           # Login page with authentication
└── (stores user data in sessionStorage/localStorage)

Backend (React App):
├── crypto-dashboard/
│   ├── src/
│   │   ├── context/
│   │   │   └── AuthContext.tsx        # Authentication context provider
│   │   ├── components/
│   │   │   ├── ProtectedRoute.tsx     # Route protection component
│   │   │   ├── InteractiveHomepage.tsx # Main dashboard
│   │   │   ├── CryptoAnalysis.tsx     # Analysis section
│   │   │   ├── CryptoStats.tsx        # Statistics section
│   │   │   └── PaymentSection.tsx     # Payment section
│   │   ├── App.tsx                    # Main app with auth integration
│   │   └── pages/
│   │       └── Home.tsx               # Home page wrapper
```

## Authentication Features

### Session Storage
- User authentication data is stored in `sessionStorage`
- Data persists during the browser session
- Data is cleared when the browser tab is closed

```javascript
{
  email: "user@example.com",
  timestamp: "2026-03-02T10:30:00.000Z",
  isLoggedIn: true
}
```

### Local Storage (Optional)
- Email address saved if "Remember me" is checked
- Used for form pre-fill on next visit
- Stored as `rememberEmail`

### Protected Routes
- All dashboard routes are protected by `ProtectedRoute` component
- Unauthenticated users are redirected to login
- User data is verified from `sessionStorage`

## Logout Functionality

- Users can logout by clicking the logout button in the dashboard header
- Logout clears `sessionStorage`
- User is redirected back to login page (`/login.html`)
- All user data is cleared from browser storage

## Running the Application

### Step 1: Start the React Dashboard
```bash
cd crypto-dashboard
npm install
npm run dev
```

### Step 2: Access the Login Page
- Open `frontend.html` in your browser
- Or serve it through a local server (recommended)

### Step 3: Login Process
1. Enter email address (e.g., user@example.com)
2. Enter password
3. Optionally check "Remember me"
4. Click "Login"
5. You'll be redirected to the interactive dashboard

## URLs to Access

- **Login Page**: `file:///path/to/frontend.html`
- **Dashboard**: `http://localhost:5173/` (or your configured port)

> Note: For best results, serve both files through a proper web server rather than opening as `file://` protocol

## Security Notes

⚠️ **Important**: This is a demonstration system. For production:

1. **Password Security**:
   - Never send passwords in plain text
   - Use HTTPS/TLS for all connections
   - Implement proper password hashing and verification server-side

2. **Authentication**:
   - Implement JWT tokens or similar
   - Use secure, httpOnly cookies
   - Set appropriate CORS policies

3. **Data Storage**:
   - Don't store sensitive data in localStorage
   - Use server-side sessions for authentication
   - Implement proper token refresh mechanisms

4. **API Integration**:
   - Replace mock login with actual API endpoints
   - Validate all inputs server-side
   - Implement rate limiting on login attempts

## Troubleshooting

### User is redirected back to login after page refresh
- Check if `sessionStorage` is being cleared
- Ensure AuthContext provider is wrapping the entire app
- Verify browser hasn't cleared site data

### "User is not defined" or auth context errors
- Ensure `AuthProvider` is wrapping all routes in `App.tsx`
- Verify `useAuth` hook is only used within `AuthProvider`

### Redirect loop
- Check `ProtectedRoute` logic
- Verify login form is storing data in `sessionStorage` correctly
- Check browser console for errors

### Dashboard not loading after login
- Verify React app is running on correct port
- Check that login redirect URL matches your app's base URL
- Look for CORS or routing errors in console

## Customization

### Change Redirect URL
In `frontend.html`, modify the redirect URL:
```javascript
setTimeout(() => {
  window.location.href = '/your-app-path'; // Change this
}, 1500);
```

### Modify Session Timeout
Add session timeout logic in `AuthContext.tsx`:
```typescript
const SESSION_TIMEOUT = 30 * 60 * 1000; // 30 minutes
```

### Add Remember Me Duration
In `frontend.html`:
```javascript
if (remember) {
  const expiryDate = new Date();
  expiryDate.setDate(expiryDate.getDate() + 30);
  localStorage.setItem('rememberEmail', email);
  localStorage.setItem('rememberExpiry', expiryDate.toISOString());
}
```

---

**Last Updated**: March 2, 2026
**Version**: 1.0.0
