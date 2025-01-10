
# How to Create a Flask App

## Step 1: Install Python

### For Windows:
1. **Download Python**:
   - Go to the official [Python website](https://www.python.org/downloads/).
   - Download the latest version of Python for Windows.

2. **Run the Installer**:
   - Locate the downloaded `.exe` file and double-click it to start the installation.
   - **Important**: Make sure to check the box **"Add Python to PATH"** before proceeding.
   - Click on **Install Now** or choose **Customize Installation** if you want to select specific features or directories.

3. **Verify the Installation**:
   - Open **Command Prompt** and type:
     ```bash
     python --version
     ```
     or
     ```bash
     python3 --version
     ```
   - You should see the installed Python version.

---

## Step 2: Create a New Project Folder

1. Create a new project folder called `my-flask-app`.

2. Inside this folder, create a new virtual environment by running:
   ```bash
   python -m venv <.venvname>
   ```

3. Activate the virtual environment:
   - On Windows, run:
     ```bash
     <folder_directory>\<venv_name>\Scripts\activate
     ```

---

## Step 3: Install Flask

Install Flask within the virtual environment:
```bash
pip install flask
```

---

## Step 4: Create the Folder Structure

Create the following directory structure:

```
my-flask-app/
├── run.py
├── app/
│   ├── __init__.py
│   ├── routes/
│   │   └── main_routes.py
│   ├── controller/
│   │   └── example_controller.py
│   ├── config/
│   │   └── config.py
│   ├── assets/
│   └── templates/
```

---

## Step 5: Add `__init__.py` File

Inside `app/__init__.py`, add the following code to create the Flask app:

```python
from flask import Flask

def create_app():
    app = Flask(__name__)

    # Import and register blueprints
    from app.routes.main_routes import main_routes
    app.register_blueprint(main_routes)

    @app.route("/")
    def home():
        return "Welcome to Flask!"
    
    return app
```

---

## Step 6: Create `run.py`

Create `run.py` in the root of your project. This will be used to start the Flask app and configure its settings:

```python
from app import create_app

app = create_app()

if __name__ == "__main__":
    app.run(debug=True)
```

---

## Step 7: Create `main_routes.py` in the Routes Folder

Go to the `routes/` folder and create `main_routes.py`. This file will handle your routes:

```python
from flask import Blueprint, jsonify
from app.controller.example_controller import example_function

main_routes = Blueprint('main', __name__)

@main_routes.route("/", methods=['GET'])
def exampleroute():
    result = example_function()
    return jsonify(result)
```

---

## Step 8: Create `example_controller.py` in the Controller Folder

Go to the `controller/` folder and create `example_controller.py`. This file contains the logic for your routes:

```python
def example_function():
    return "This is an example function from the controller."
```

---

## Step 9: Add Configuration in `config.py`

Go to the `config/` folder and add `config.py`. This file will contain the configuration settings for your Flask app, such as the secret key and debug mode:

```python
class Config:
    DEBUG = True
    SECRET_KEY = "your_secret_key"
```

---

## Step 10: Run the Flask Application

Finally, run the Flask application by executing the following command in the terminal:

```bash
python run.py
```

Alternatively, you can use:

```bash
flask run
```

This will start the Flask app, and you can access it in your browser at `http://127.0.0.1:5000`.

---

This completes the setup for a basic Flask application. From here, you can expand your app by adding more routes, templates, assets, and controllers.
