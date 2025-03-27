# Django E-commerce Store/Cart

A basic e-commerce website built with Django, featuring product listings, shopping cart functionality, and basic checkout process.  Deployed on PythonAnywhere.

## Table of Contents

*   [Features](#features)
*   [Demo](#demo)
*   [Getting Started](#getting-started)
    *   [Prerequisites](#prerequisites)
    *   [Installation](#installation)
*   [Deployment (PythonAnywhere)](#deployment-pythonanywhere)
    *   [Setting up your PythonAnywhere Account](#setting-up-your-pythonanywhere-account)
    *   [Cloning the Repository](#cloning-the-repository)
    *   [Creating a Virtual Environment](#creating-a-virtual-environment)
    *   [Installing Dependencies](#installing-dependencies)
    *   [Configuring the Database](#configuring-the-database)
    *   [Setting up Static Files](#setting-up-static-files)
    *   [Configuring the WSGI file](#configuring-the-wsgi-file)
    *   [Running the Application](#running-the-application)
*   [Usage](#usage)
*   [Project Structure](#project-structure)
*   [Contributing](#contributing)
*   [License](#license)
*   [Acknowledgments](#acknowledgments)
*   [Future Enhancements](#future-enhancements)
*   [Troubleshooting](#troubleshooting)

## Features

*   **Product Listing:** Browse available products with descriptions and prices.
*   **Shopping Cart:** Add and remove products from a shopping cart.
*   **Basic Checkout:** Simple checkout process (may be simulated or placeholder in this basic version).
*   **User Authentication:** (Optional - If you have user registration/login) User accounts to manage orders.
*   **Responsive Design:** (Optional) Website is responsive and accessible on different devices.

## Demo

[Link](http://scav.pythonanywhere.com)

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

*   **Python:**  3.7 or higher (Check with `python --version` or `python3 --version`)
*   **pip:** Python package installer (usually comes with Python)
*   **Git:**  For cloning the repository (`git --version`)
*   **Django:** The core framework (will be installed as a dependency).
*   (Optional) **Virtualenv/venv:** Strongly recommended for creating isolated Python environments.

### Installation

1.  **Clone the repository:**

    ```bash
    git clone [[your repository URL]](https://github.com/Scavix/my_ecom.git)
    cd my_ecom
    ```

2.  **Create a virtual environment (recommended):**

    ```bash
    python -m venv venv  #or virtualenv venv
    source venv/bin/activate  # On Linux/macOS
    .\venv\Scripts\activate  # On Windows
    ```

3.  **Install the dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

4.  **Configure the database:**

    *   **Option 1 (SQLite - Default):**  Django's default SQLite database is fine for development.  No extra setup needed.
    *   **Option 2 (PostgreSQL/MySQL/etc.):**
        *   Install the necessary database adapter (e.g., `pip install psycopg2` for PostgreSQL).
        *   Update the `DATABASES` setting in your `settings.py` file with your database credentials.  Example (PostgreSQL):

            ```python
            DATABASES = {
                'default': {
                    'ENGINE': 'django.db.backends.postgresql',
                    'NAME': 'your_database_name',
                    'USER': 'your_username',
                    'PASSWORD': 'your_password',
                    'HOST': 'localhost',  # Or your database server address
                    'PORT': '5432',
                }
            }
            ```

5.  **Run database migrations:**

    ```bash
    python manage.py migrate
    ```

6.  **Create a superuser (admin account):**

    ```bash
    python manage.py createsuperuser
    ```
    *Follow the prompts to create your admin username, email, and password.*

7.  **Collect static files:**

    ```bash
    python manage.py collectstatic
    ```
    *This will copy static files (CSS, JavaScript, images) to a designated directory.*

8.  **Start the development server:**

    ```bash
    python manage.py runserver
    ```

    *Open your web browser and go to `http://127.0.0.1:8000/` to view the application.*

## Deployment (PythonAnywhere)

These instructions guide you through deploying your Django e-commerce application to PythonAnywhere.

### Setting up your PythonAnywhere Account

1.  **Create an account:**  Sign up for a beginner account at [https://www.pythonanywhere.com/](https://www.pythonanywhere.com/).  The free account has limitations, but it's sufficient for basic deployments.

### Cloning the Repository

1.  **Open a Bash console:**  Log in to PythonAnywhere and start a new Bash console.

2.  **Clone your Git repository:**

    ```bash
    git clone https://github.com/Scavix/my_ecom.git
    cd my_ecom
    ```

### Creating a Virtual Environment

1.  **Create a virtual environment:**

    ```bash
    python3.x -m venv venv  # Replace 3.x with your Python version (e.g., 3.9, 3.10)
    source venv/bin/activate
    ```

### Installing Dependencies

1.  **Install the required packages:**

    ```bash
    pip install -r requirements.txt
    ```

### Configuring the Database

*   **PythonAnywhere uses MySQL by default for free accounts.**

1.  **Set up your MySQL database:**
    * Go to the "Databases" tab in PythonAnywhere.
    * Create a new MySQL database.  Note the username, password, and database name.

2.  **Update `settings.py`:**  Modify the `DATABASES` setting in your Django project's `settings.py` file:

    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'your_pythonanywhere_username$your_database_name',  #Important format
            'USER': 'your_pythonanywhere_username',
            'PASSWORD': 'your_database_password',
            'HOST': 'your_pythonanywhere_username.mysql.pythonanywhere-services.com', #or 127.0.0.1
        }
    }
    ```

3.  **Run migrations:**

    ```bash
    python manage.py migrate
    ```

    *If you get an error about missing tables, try running `python manage.py migrate --run-syncdb`*

### Setting up Static Files

1.  **Configure `STATIC_ROOT` in `settings.py`:**  Add the following to your `settings.py` file:

    ```python
    import os

    STATIC_URL = '/static/'
    STATIC_ROOT = os.path.join(BASE_DIR, 'static')
    ```

2.  **Collect static files:**

    ```bash
    python manage.py collectstatic
    ```

    *When prompted to create the directory, type `yes`.*

### Configuring the WSGI file

1.  **Find your WSGI file:**  Go to the "Web" tab in PythonAnywhere.  There should be a default WSGI file path shown.  It's usually something like `/var/www/yourusername_pythonanywhere_com_wsgi.py`.

2.  **Edit the WSGI file:**  Click the link to edit the WSGI file.  Replace its contents with the following, adjusting the paths as needed:

    ```python
    import os
    import sys

    # Add your project directory to the Python path
    path = '/home/yourusername/yourproject'  # Replace with your project's directory
    if path not in sys.path:
        sys.path.append(path)

    os.environ['DJANGO_SETTINGS_MODULE'] = 'yourproject.settings'  # Replace with your project's settings module

    from django.core.wsgi import get_wsgi_application
    application = get_wsgi_application()
    ```

    *   **Important:** Replace `yourusername` and `yourproject` with your actual PythonAnywhere username and project directory name.  Also, replace `yourproject.settings` with the actual path to your settings file if it's different.

### Running the Application

1.  **Reload your web app:**  Go back to the "Web" tab in PythonAnywhere and click the "Reload" button next to your web app.

2.  **Visit your website:**  Click the link to your website (e.g., `yourusername.pythonanywhere.com`) to see your deployed application.

## Usage

*   **Browse Products:** Navigate to the homepage or product listing page to view available items.
*   **Add to Cart:** Click the "Add to Cart" button on a product page to add it to your shopping cart.
*   **View Cart:** Access your shopping cart to review selected items, adjust quantities, and proceed to checkout.
*   **Checkout:** (If implemented) Follow the checkout process to finalize your order.
*   **Admin Panel:** Access the Django admin panel at `/admin/` to manage products, categories, and other data.

## Project Structure

```
yourproject/
├── manage.py
├── yourproject/        # Main project directory
│   ├── __init__.py
│   ├── settings.py     # Django settings
│   ├── urls.py         # Project-level URL configuration
│   ├── wsgi.py         # WSGI configuration for deployment
│   └── asgi.py         # ASGI configuration for asynchronous tasks (if used)
├── store/             # Example: Your app containing store logic
│   ├── __init__.py
│   ├── models.py       # Database models (products, categories, etc.)
│   ├── views.py        # Views (logic for handling requests)
│   ├── urls.py         # App-level URL configuration
│   ├── forms.py        # Django forms (if used)
│   ├── templates/      # HTML templates
│   │   └── store/      # Template files for the store app
│   ├── static/         # Static files (CSS, JavaScript, images)
│   │   └── store/      # Static files for the store app
│   └── admin.py        # Admin panel configuration
├── cart/              # Example: Your app containing cart logic
│   ├── __init__.py
│   └── admin.py        # Admin panel configuration
│   ├── apps.py
│   ├── cart.py
│   ├── context_processor.py
│   ├── forms.py        # Django forms (if used)
│   ├── models.py       # Database models (products, categories, etc.)
│   └── templates/
│   │   └── cart/      # Template files for the cart app
│   ├── urls.py         # App-level URL configuration
│   └── views.py        # Views (logic for handling requests)
├── requirements.txt   # List of Python dependencies
├── venv/             # Virtual environment directory (usually not committed to Git)
└── static/           # Collected static files (from collectstatic)
```


## Contributing

Contributions are welcome!  Please follow these steps:

1.  Fork the repository.
2.  Create a new branch for your feature or bug fix.
3.  Make your changes and commit them with descriptive messages.
4.  Push your changes to your forked repository.
5.  Submit a pull request to the main repository.

## License

This project is licensed under the [MIT License](LICENSE) - see the `LICENSE` file for details.  *(Replace with the actual license you're using.)*

## Acknowledgments

*   Django Project: [https://www.djangoproject.com/](https://www.djangoproject.com/) (helpful)
*   PythonAnywhere: [https://www.pythonanywhere.com/](https://www.pythonanywhere.com/)

## Future Enhancements

*   **Payment Integration:** Integrate a real payment gateway (e.g., Stripe, PayPal).
*   **Order Management:** Implement order tracking and management features.
*   **User Profiles:** Allow users to manage their profiles, addresses, and order history.
*   **Search Functionality:** Add a search bar to easily find products.
*   **Improved UI/UX:** Enhance the user interface and user experience with better design and interactions.
*   **Testing:** Add unit tests and integration tests to improve code quality.
*   **More advanced cart features**: Add features like coupon codes, shipping cost calculations.
*   **Product Reviews**: Allow users to leave reviews on products.

## Troubleshooting

*   **Problem:** "ModuleNotFoundError: No module named 'yourproject'"
    *   **Solution:** Double-check that the `path` variable in your WSGI file is correct and that the project directory is actually in the Python path. Also, make sure the virtual environment is activated.

*   **Problem:** "Static files not loading"
    *   **Solution:** Ensure that `STATIC_ROOT` is correctly configured in `settings.py`, you have run `python manage.py collectstatic`, and your PythonAnywhere web app is configured to serve static files from the `STATIC_ROOT` directory.  Double check that the URL you're using to access the static files matches `STATIC_URL`

*   **Problem:** "Database connection errors"
    *   **Solution:** Verify that your database settings in `settings.py` are correct (hostname, username, password, database name).  Also, ensure that your database server is running and accessible from PythonAnywhere.

*   **Problem**: "My website shows a Django error page"
    *   **Solution**: In `settings.py`, set `DEBUG = True` to see more detailed error messages.  **Important:**  Remember to set `DEBUG = False` in production to prevent sensitive information from being exposed.  Check your PythonAnywhere server logs for more information

*   **Problem:** "My site looks messed up after `collectstatic`"
    *   **Solution:** Make sure that you have the correct `STATIC_URL` and `STATIC_ROOT` settings. The `STATIC_URL` specifies the URL prefix that the static files will be served from, while the `STATIC_ROOT` specifies the directory where the static files will be collected. When you run the `collectstatic` command, Django will collect all the static files from your project and copy them to the `STATIC_ROOT` directory.  Also, clear your browser cache.

---
