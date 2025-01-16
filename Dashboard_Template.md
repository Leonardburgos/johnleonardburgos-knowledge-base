# Dashboard Template with MUI and React.js

By: John Leonard Burgos

## Step-by-Step Instructions

### 1. Create Your Project

Run the following command to start your project:

```bash
npm create vite@latest dashboard-temp --template
```

- Choose `React` and then `JS` or `Javascript`.

Wait for the project to load, then run:

```bash
npm install
```

Follow the tutorial to set up routing and a login form.

### 2. Set Up the Project Structure

In your `src` folder, create the following folder:

- `pages`

In `pages` create the following subfolders:

- `components`
- `registration`
- `screens`
- `segments`

### 3. Install Required Packages

Run the following commands to install the required dependencies:

```bash
npm install @mui/material @emotion/react @emotion/styled
npm install @mui/icons-material
```

Add a file Router.tsx in the src folder.

```jsx
import React from "react";
import { Routes, Route } from "react-router-dom";
{Import your Page in here}

function Router() {
  return (
    <Routes>
      <Route path="/" element={<LoginForm />} />
      <Route path="/dashboard" element={<Dashboard />} />
    </Routes>
  );
}

export default Router;
```

Install React Router DOM:

```bash
npm install react-router-dom
```

Modify `App.jsx`:

```jsx
import { useState } from "react";
import RouterComponent from "./Router";
import { HashRouter as Router } from "react-router-dom";

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

Configure Tailwind CSS

1. Install Tailwind CSS:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```

2. Update `tailwind.config.js`:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

Add Tailwind directives to `src/index.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
Add `postcss.config.js`
```jsx
export default {
    plugins: {
      tailwindcss: {},
      autoprefixer: {},
    },
  }
  
```
Create `LoginPage.jsx` on `registration` folder:

```jsx
import React, { useState } from "react";

const LoginForm = () => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log("Email:", email);
    console.log("Password:", password);
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-blue-600 to-black">
      <div className="bg-white rounded-lg shadow-lg p-8 w-96">
        <h1 className="text-2xl font-bold text-center text-gray-800 mb-6">
          Welcome!
        </h1>
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
              className="w-full mt-1 p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-600"
            />
          </div>
          <div>
            <label
              htmlFor="password"
              className="block text-gray-700 font-medium"
            >
              Password:
            </label>
            <input
              type="password"
              id="password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              className="w-full mt-1 p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-600"
            />
            <a
              href="#"
              className="text-sm text-blue-600 hover:underline mt-1 block"
            >
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
            className="w-full bg-gradient-to-r from-blue-600 to-black text-white font-medium py-2 rounded-md shadow-md hover:opacity-90 focus:outline-none focus:ring-2 focus:ring-blue-600"
          >
            Sign in
          </button>
        </form>
        <p className="text-center text-sm text-gray-600 mt-4">
          New on our platform?{" "}
          <a href="#" className="text-red-600 hover:underline">
            Create an account
          </a>
        </p>
        <div className="flex justify-center space-x-3 mt-6">
        </div>
      </div>
    </div>
  );
};

export default LoginForm;
```

### 4. Create the Components

#### `Header.jsx` in `components` folder

```jsx
import React from 'react';
import AppBar from '@mui/material/AppBar';
import Toolbar from '@mui/material/Toolbar';
import IconButton from '@mui/material/IconButton';
import Typography from '@mui/material/Typography';
import MenuIcon from '@mui/icons-material/Menu';
import ArrowBackIcon from '@mui/icons-material/ArrowBackIosNewRounded';
import { blue, blueGrey } from '@mui/material/colors';

const Header = ({ isSidebarOpen, toggleSidebar }) => (
  <AppBar
    position="fixed"
    sx={{
      zIndex: (theme) => theme.zIndex.drawer + 1,
      backgroundColor: blueGrey, // Gradient with percentage stops
    }}
  >
    <Toolbar>
      <IconButton
        color="inherit"
        edge="start"
        onClick={toggleSidebar}
        sx={{ marginRight: 2 }}
      >
        {isSidebarOpen ? <ArrowBackIcon /> : <MenuIcon />}
      </IconButton>
      <Typography variant="h6" noWrap>
        Dashboard
      </Typography>
    </Toolbar>
  </AppBar>
);

export default Header;

```

#### `Sidebar.jsx` in `components` folder

```jsx
import React from 'react';
import Drawer from '@mui/material/Drawer';
import List from '@mui/material/List';
import ListItem from '@mui/material/ListItem';
import ListItemIcon from '@mui/material/ListItemIcon';
import ListItemText from '@mui/material/ListItemText';
import Typography from '@mui/material/Typography';
import Toolbar from '@mui/material/Toolbar';
import { Link } from 'react-router-dom';

const drawerWidth = 240;
const drawerWidthCollapsed = 72;

const Sidebar = ({ open, navigation, onSelectSegment }) => (
  <Drawer
    variant="permanent"
    sx={{
      width: open ? drawerWidth : drawerWidthCollapsed,
      flexShrink: 0,
      transition: 'width 0.3s',
      [`& .MuiDrawer-paper`]: {
        width: open ? drawerWidth : drawerWidthCollapsed,
        overflowX: 'hidden',
        transition: 'width 0.3s',
        backgroundColor: '#f5f5f5', // Set background color to gray
      },
    }}
  >
    <Toolbar />
    <List>
      {navigation.map((item, index) =>
        item.kind === 'header' && open ? (
          <Typography
            key={index}
            sx={{
              margin: '16px 16px 8px',
              fontWeight: 'bold',
              color: '#888',
              textTransform: 'uppercase',
              fontSize: '0.75rem',
            }}
          >
            {item.title}
          </Typography>
        ) : (
          item.segment && (
            <ListItem
              button
              key={item.segment}
              sx={{ justifyContent: open ? 'flex-start' : 'center' }}
              onClick={() => onSelectSegment(item.segment)} // Trigger content change
            >
              <ListItemIcon sx={{ minWidth: open ? '40px' : 'unset', justifyContent: 'center', color: 'black', }}>
                {item.icon}
              </ListItemIcon>
              {open && <ListItemText primary={item.title} />}
            </ListItem>
          )
        )
      )}
    </List>
  </Drawer>
);

export default Sidebar;

```

### 5. Create Other Components

#### `NotFound.jsx` in `registration` folder

```jsx
import React from "react";

const NotFoundPage = () => {
  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-blue-600 to-black">
      <div
        className="bg-white rounded-lg shadow-lg p-8 w-96"
        style={{
          backgroundColor: "rgba(199, 199, 199, 0.9)", // White with slight transparency
        }}
      >
        <div className="text-center mb-4">
          <h2 className="text-3xl font-semibold text-gray-900">404</h2>
          <p className="text-xl text-gray-700">Page Not Found</p>
        </div>

        <div className="text-center">
          <p className="text-sm text-gray-600"></p>
        </div>
      </div>
    </div>
  );
};

export default NotFoundPage;
```

### 6. Set Up Routing

In your `Router.tsx` or routing file, add the following route:

```jsx
<Route path="*" element={<NotFoundPage />} />
```
This `NotFoundPage` will catch all wrong route and redirect them into this page 

Fix the Error on `NotFoundPage` delete `{Import your Page in here}` and then import your LoginForm
```jsx
import LoginForm from "./pages/registration/LoginPage";
```

### 7. Create the Dashboard Structure

#### `Dashboard.jsx` in `screens` folder

```jsx
import React, { useState } from 'react';
import { createTheme, ThemeProvider, styled } from '@mui/material/styles';
import Grid from '@mui/material/Grid';
import CssBaseline from '@mui/material/CssBaseline';
import Box from '@mui/material/Box';
import Header from '../components/Header';
import Sidebar from '../components/Sidebar';
import DashboardIcon from '@mui/icons-material/Edit';
import ShoppingCartIcon from '@mui/icons-material/ShoppingCart';
import BarChartIcon from '@mui/icons-material/BarChart';
import LayersIcon from '@mui/icons-material/Layers';
import Toolbar from '@mui/material/Toolbar';

// Import content pages
import DashboardPage from '../segments/DashboardPage';
import OrdersPage from '../segments/OrdersPage';
import ReportsPage from '../segments/ReportsPage';
import IntegrationsPage from '../segments/IntegrationPage';

const drawerWidth = 240;
const drawerWidthCollapsed = 72;

const NAVIGATION = [
  { kind: 'header', title: 'Main items' },
  { segment: 'dashboard', title: 'Dashboard', icon: <DashboardIcon /> },
  { segment: 'orders', title: 'Orders', icon: <ShoppingCartIcon /> },
  { kind: 'header', title: 'Analytics' },
  { segment: 'reports', title: 'Reports', icon: <BarChartIcon /> },
  { segment: 'integrations', title: 'Integrations', icon: <LayersIcon /> },
];

const demoTheme = createTheme({
  palette: {
    mode: 'light',
  },
});

const Skeleton = styled('div')(({ theme, height }) => ({
  backgroundColor: theme.palette.action.hover,
  borderRadius: theme.shape.borderRadius,
  height,
}));

const Dashboard = () => {
  const [isSidebarOpen, setSidebarOpen] = useState(false);
  const [selectedSegment, setSelectedSegment] = useState('dashboard'); // Track selected segment

  const toggleSidebar = () => setSidebarOpen((prev) => !prev);

  // Function to change content based on selected segment
  const getContent = () => {
    switch (selectedSegment) {
      case 'dashboard':
        return <DashboardPage />;
      case 'orders':
        return <OrdersPage />;
      case 'reports':
        return <ReportsPage />;
      case 'integrations':
        return <IntegrationsPage />;
      default:
        return <DashboardPage />;
    }
  };

  return (
    <ThemeProvider theme={demoTheme}>
      <CssBaseline />
      <Box sx={{ display: 'relative' }}>
        {/* Sidebar */}
        <Sidebar
          open={isSidebarOpen}
          navigation={NAVIGATION}
          onSelectSegment={(segment) => setSelectedSegment(segment)} // Update selected segment
        />

        {/* Main Content */}
        <Box
          component="main"
          sx={{
            flexGrow: 1,
            bgcolor: 'background.default',
            p: 3,
            ml: isSidebarOpen ? `${drawerWidth}px` : `${drawerWidthCollapsed}px`,
            transition: 'margin-left 0.3s',
          }}
        >
          <Header isSidebarOpen={isSidebarOpen} toggleSidebar={toggleSidebar} />

          <Toolbar />

          {/* Render content based on selected segment */}
          {getContent()}
        </Box>
      </Box>
    </ThemeProvider>
  );
};

export default Dashboard;

```

Duplicate `DashboardPage.jsx` to create:

- `OrdersPage.jsx`
- `ReportsPage.jsx`
- `IntegrationsPage.jsx`

```jsx
// src/pages/DashboardPage.jsx
import React from 'react';
import Box from '@mui/material/Box';

const DashboardPage = () => {
  return (
    <Box>
      <h2>Dashboard Page</h2>
      {/* Your Dashboard content */}
    </Box>
  );
};

export default DashboardPage;

```

Update their names and content accordingly.

Import the pages in Router.tsx

```jsx
import Dashboard from "./pages/screens/Dashboard";
import NotFoundPage from "./pages/registration/NotFoundPage";
```

### 8. Run the Application

Run the project using:

```bash
npm run dev
```
## Download Template
You can download the template from this [GitHub repository](https://github.com/Leonardburgos/leonardburgos-dashboardMUI-react).