
# React Project Setup with Routing and Tailwind CSS

## Prerequisites
1. Install **Node.js**.

## Step 1: Create a React Project
Run the following command:
```bash
npm create vite@latest <name-of-project> --template
```

Use the arrow keys to select:
- **Framework**: React
- **Variant**: JavaScript

Navigate to the project folder and install dependencies:
```bash
cd <name-of-project>
npm install
```

Start the development server:
```bash
npm run dev
```
Visit the provided localhost URL to view your app.

## Step 2: Setup Routing
1. Add a file `Router.tsx` in the `src` folder.
2. Create a `pages` folder inside `src` and add the following files:

**Page1.jsx**
```jsx
import { useState } from 'react';

function Page1() {
  return (
    <>
      <div>this is page 1</div>
    </>
  );
}

export default Page1;
```

**Page2.jsx**
```jsx
import { useState } from 'react';

function Page2() {
  return (
    <>
      <div>this is page 2</div>
    </>
  );
}

export default Page2;
```

3. Add routing logic to `Router.tsx`:
```jsx
import React from "react";
import { Routes, Route } from "react-router-dom";
import Page1 from "./pages/Page1";
import Page2 from "./pages/Page2";

function Router() {
  return (
    <Routes>
      <Route path="/" element={<Page1 />} />
      <Route path="/page2" element={<Page2 />} />
    </Routes>
  );
}

export default Router;
```

4. Install React Router DOM:
```bash
npm install react-router-dom
```

5. Modify `App.jsx`:
```jsx
import { useState } from "react";
import RouterComponent from "./Router";
import { HashRouter as Router } from 'react-router-dom';

function App() {
  return (
    <>
      <div>
        <Router>
          <RouterComponent />
        </Router>
      </div>
    </>
  );
}

export default App;
```

## Step 3: Configure Tailwind CSS
1. Install Tailwind CSS:
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```

2. Update `tailwind.config.js`:
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

3. Add Tailwind directives to `src/index.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

4. Verify Tailwind setup:
Modify `App.jsx`:
```jsx
function App() {
  return (
    <div className="h-screen flex items-center justify-center bg-gray-100">
      <h1 className="text-3xl font-bold text-blue-500">
        Hello, Tailwind CSS with Vite!
      </h1>
    </div>
  );
}

export default App;
```

## Step 4: Create a Login Form
1. Update `Router.tsx`:
```jsx
import React from "react";
import { Routes, Route } from "react-router-dom";
import LoginForm from "./pages/registration/LoginPage";

function Router() {
  return (
    <Routes>
      <Route path="/" element={<LoginForm />} />
    </Routes>
  );
}

export default Router;
```

2. Create `LoginPage.jsx`:
```jsx
import React, { useState } from 'react';

const LoginForm = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log('Email:', email);
    console.log('Password:', password);
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-red-600 to-black">
      <div className="bg-white rounded-lg shadow-lg p-8 w-96">
        <h1 className="text-2xl font-bold text-center text-gray-800 mb-6">Welcome!</h1>
        <form onSubmit={handleSubmit} className="space-y-4">
          <div>
            <label htmlFor="email" className="block text-gray-700 font-medium">
              Email or Username:
            </label>
            <input
              type="text"
              id="email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              className="w-full mt-1 p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-red-600"
            />
          </div>
          <div>
            <label htmlFor="password" className="block text-gray-700 font-medium">
              Password:
            </label>
            <input
              type="password"
              id="password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              className="w-full mt-1 p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-red-600"
            />
            <a href="#" className="text-sm text-red-600 hover:underline mt-1 block">
              Forgot Password?
            </a>
          </div>
          <div className="flex items-center">
            <input
              type="checkbox"
              id="rememberMe"
              className="h-4 w-4 text-red-600 border-gray-300 rounded"
            />
            <label htmlFor="rememberMe" className="ml-2 text-gray-700">
              Remember Me
            </label>
          </div>
          <button
            type="submit"
            className="w-full bg-gradient-to-r from-red-600 to-black text-white font-medium py-2 rounded-md shadow-md hover:opacity-90 focus:outline-none focus:ring-2 focus:ring-red-600"
          >
            Sign in
          </button>
        </form>
        <p className="text-center text-sm text-gray-600 mt-4">
          New on our platform?{' '}
          <a href="#" className="text-red-600 hover:underline">
            Create an account
          </a>
        </p>
        <div className="flex justify-center space-x-3 mt-6">
          <button className="p-2 rounded-full bg-blue-600 text-white hover:opacity-90 shadow-md">
            <i className="fab fa-facebook-f"></i>
          </button>
          <button className="p-2 rounded-full bg-red-600 text-white hover:opacity-90 shadow-md">
            <i className="fab fa-google-plus-g"></i>
          </button>
          <button className="p-2 rounded-full bg-blue-400 text-white hover:opacity-90 shadow-md">
            <i className="fab fa-twitter"></i>
          </button>
        </div>
      </div>
    </div>
  );
};

export default LoginForm;
```

Run your project to see the Login Form in action.
