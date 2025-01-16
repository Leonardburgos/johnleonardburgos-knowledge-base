
# Common Setup: Using HTTP Methods in React, Python, and Django

This guide provides a common setup for making HTTP requests and handling them in React, Python, and Django. It covers basic use cases like `GET`, `POST`, `PUT`, and `DELETE` methods.

## React: Making HTTP Requests
In React, you can use the `fetch` API or libraries like `axios` to make HTTP requests.

### Installing Axios (optional)
```bash
npm install axios
```

### Using Fetch API
```javascript
// Example: Making a GET request
fetch('http://example.com/api/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));

// Example: Making a POST request
fetch('http://example.com/api/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({ key: 'value' }),
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### Using Axios
```javascript
import axios from 'axios';

// Example: Making a GET request
axios.get('http://example.com/api/data')
  .then(response => console.log(response.data))
  .catch(error => console.error('Error:', error));

// Example: Making a POST request
axios.post('http://example.com/api/data', { key: 'value' })
  .then(response => console.log(response.data))
  .catch(error => console.error('Error:', error));
```

## Python: Making HTTP Requests
In Python, you can use the `requests` library to make HTTP requests.

### Installing Requests
```bash
pip install requests
```

### Example Usage
```python
import requests

# Example: Making a GET request
response = requests.get('http://example.com/api/data')
if response.status_code == 200:
    print(response.json())

# Example: Making a POST request
payload = {"key": "value"}
response = requests.post('http://example.com/api/data', json=payload)
if response.status_code == 200:
    print(response.json())
```

## Django: Handling HTTP Requests
In Django, you can use views to handle HTTP methods.

### Example Views
```python
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt
import json

# Example: Handling GET requests
def get_data(request):
    if request.method == 'GET':
        data = {"message": "This is a GET response."}
        return JsonResponse(data)

# Example: Handling POST requests
@csrf_exempt
def post_data(request):
    if request.method == 'POST':
        body = json.loads(request.body)
        response_data = {"received": body}
        return JsonResponse(response_data)
```

### URLs Configuration
Add the views to your `urls.py`:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('api/get-data/', views.get_data, name='get_data'),
    path('api/post-data/', views.post_data, name='post_data'),
]
```

## Integration Example: React Frontend with Django Backend

### Django Backend
- Start the Django development server:
  ```bash
  python manage.py runserver
  ```
- Assume the server is running at `http://127.0.0.1:8000`.

### React Frontend
```javascript
// Making a GET request to Django backend
fetch('http://127.0.0.1:8000/api/get-data/')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));

// Making a POST request to Django backend
fetch('http://127.0.0.1:8000/api/post-data/', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({ key: 'value' }),
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

## Notes
1. **CORS Issues**: If you encounter CORS issues, use Django's `django-cors-headers` library.
   ```bash
   pip install django-cors-headers
   ```
   Add it to `INSTALLED_APPS` and configure middleware in `settings.py`.

2. **CSRF Tokens**: For POST requests in Django, ensure CSRF tokens are handled correctly. Use `@csrf_exempt` for testing or configure your frontend to send CSRF tokens.

# React Example: HTTP Functions for POST and GET Requests

```javascript
// POST Request Function: Save Data
export const saveData = async (data_scheduled_messages) => {
  const apiEndpoint = `${baseURL}/schedule-message`;  // Your API endpoint
  const token = localStorage.getItem("token");  // JWT token for authentication

  try {
    const response = await fetch(apiEndpoint, {
      method: "POST",  // HTTP method: POST
      headers: {
        "Content-Type": "application/json",  // Content type: JSON
        "ngrok-skip-browser-warning": "true",  // Skipping browser warning for ngrok
        Authorization: `Bearer ${token}`,  // Bearer token for authorization
      },
      body: JSON.stringify(data_scheduled_messages),  // Data to be sent in the request body
    });

    const responseData = await response.json();  // Parse the response JSON

    if (!response.ok) {
      // Handle errors if the response is not ok (e.g., status code is not 200)
      throw { status: response.status, message: responseData.message || "An error occurred" };
    }

    // Return status and data upon success
    return { status: response.status, data: responseData };
  } catch (error) {
    // Handle errors (e.g., network issues, JSON parsing errors)
    notify(error.message, 'error');
    throw error;
  }
};

// GET Request Function: Fetch Data
export const fetchData = async () => {
  const apiEndpoint = `${baseURL}/get-schedule-message`;  // Your API endpoint
  const token = localStorage.getItem("token");  // JWT token for authentication

  try {
    const response = await fetch(apiEndpoint, {
      method: "GET",  // HTTP method: GET
      headers: {
        "Content-Type": "application/json",  // Content type: JSON
        Authorization: `Bearer ${token}`,  // Bearer token for authorization
        "ngrok-skip-browser-warning": "true",  // Skipping browser warning for ngrok
      },
    });

    if (!response.ok) {
      // Handle errors if the response is not ok
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();  // Parse the response JSON
    return data;  // Return the fetched data
  } catch (error) {
    // Handle errors (e.g., network issues, JSON parsing errors)
    console.error("Error fetching data:", error);
    throw error;
  }
};
```

### Key Features:
1. **Authorization**: Both functions use JWT tokens stored in `localStorage` for authentication. You should ensure the token is added in the header for every API request requiring authentication.
   
2. **Error Handling**: Errors are caught and logged using `try-catch` blocks, making it easy to debug and handle errors gracefully. Errors also trigger a notification using `notify()`.

3. **Authorization Header**: The `Authorization` header uses the Bearer token (`Authorization: 'Bearer ' + token`) for secure requests.

4. **Response Handling**: After receiving the response, the functions check if the response is successful (`response.ok`) and parse the JSON accordingly.

### Usage Example in a Component

```javascript
import React, { useEffect, useState } from 'react';
import { saveData, fetchData } from './api';  // Assuming the functions are in an 'api.js' file

const MyComponent = () => {
  const [scheduledMessages, setScheduledMessages] = useState([]);
  const [messageData, setMessageData] = useState({ key: 'value' });

  useEffect(() => {
    // Fetching data from the backend on component mount
    const loadData = async () => {
      try {
        const data = await fetchData();  // Fetch data from the API
        setScheduledMessages(data);  // Update state with fetched data
      } catch (error) {
        console.error("Failed to fetch data", error);
      }
    };

    loadData();
  }, []);

  const handleSave = async () => {
    try {
      const result = await saveData(messageData);  // Save new data
      console.log('Data saved successfully:', result);
    } catch (error) {
      console.error('Failed to save data', error);
    }
  };

  return (
    <div>
      <h1>Scheduled Messages</h1>
      <ul>
        {scheduledMessages.map((msg, index) => (
          <li key={index}>{msg.message}</li>
        ))}
      </ul>
      <button onClick={handleSave}>Save Data</button>
    </div>
  );
};

export default MyComponent;
```

### Explanation:
- **useEffect**: This React hook is used to load the scheduled messages when the component mounts.
- **handleSave**: This function calls `saveData` to send a POST request when the user clicks the "Save Data" button.
- **State Management**: React state (`useState`) is used to store the messages and the data to be saved.

This structure is reusable and can be easily integrated with various parts of a React app while keeping it clean and maintainable.
