# Animal-AI WebGL Platform

**Animal-AI WebGL Platform** is an integrated environment that combines a Django backend with a Unity WebGL frontend to facilitate running and managing behavioral experiments directly in a web browser. Researchers can create experiments, upload custom Unity builds along with YAML configurations, and share these experiments with participants who interact through their browsers. The platform handles participant declarations, data collection, and provides researchers with tools to analyze and manage experiment data.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Key Components](#key-components)
- [Technologies Used](#technologies-used)
- [Architecture and Data Flow](#architecture-and-data-flow)
- [Installation and Setup](#installation-and-setup)
  - [Prerequisites](#prerequisites)
  - [Repository Structure](#repository-structure)
  - [Configuration](#configuration)
  - [Database Setup](#database-setup)
  - [Static Files and Collectstatic](#static-files-and-collectstatic)
  - [Running the Development Server](#running-the-development-server)
  - [Django Backend Explained](#django-backend-explained)
    - [Apps and Modules](#apps-and-modules)
    - [Models and Data Entities](#models-and-data-entities)

---

## Project Overview

The platform serves as a bridge between **Django** and **Unity WebGL**:

- **Django Backend:** Manages user accounts (researchers, participants), experiments, participant declarations, and data storage.
- **Unity Frontend:** Embedded on the homepage and experiment pages, providing a real-time simulation environment for the Animal-AI experiments. It is limited in functionality and has limitations.

---

## Key Components

- **Homepage:** Hosts the Unity WebGL build, allowing casual play and direct participant engagement.
- **Login/Registration:** Researchers can create accounts, log in, and access the researcher dashboard.
- **Researcher Dashboard:** Manage experiments, upload Unity builds, associate YAML configurations, and export data.
- **Experiment Creation:** Researchers can create new experiments, provide descriptions, and associate Unity builds and YAML configurations.
- **Participant Declaration:** Participants fill out a declaration form before accessing the Unity environment.
- **Researcher Dashboard:** Accessible after login, enabling experiment creation, Unity build upload, YAML config association, and data export.
- **Participant Experience:** Participants receive a shareable link, fill out a declaration form, and then access the Unity environment.
- **Data Collection:** Participant interactions are logged and stored in the database for later analysis and export.

---

## Tech Stack

- **Backend:** Python 3.x, Django 4.x
- **Frontend (Base):** HTML5, CSS3, JavaScript
- **Game/Simulation:** Unity (WebGL build)
- **Data Storage:** PostgreSQL (recommended for AWS deployment) or SQLite (development)
- **Deployment:** Gunicorn or uWSGI, Nginx, Whitenoise for static files

---

## Architecture and Data Flow

1. **Frontend (Unity + HTML):**
   - `home.html` loads the Unity WebGL game files (AAI.data, AAI.framework.js, AAI.wasm) and the Unity loader for casual play.

2. **Backend (Django):**
   - Handles user authentication, experiment CRUD operations, participant forms, and data retrieval.
   - Serves static files (Unity builds, YAML files) and templates.
   - Manages the database (PostgreSQL/SQLite) for storing experiments, participants, and data.

3. **YAML Integration:**
   - Unity retrieves YAML files from `StreamingAssets/` at runtime via HTTP requests.
   - YAML defines experiment parameters, which Unity then uses to instantiate arenas and challenges.

4. **Researcher Workflow:** 
   - Researchers create experiments, upload Unity builds, associate YAML configurations, and manage participant data.
   - Researchers can export participant data for analysis and research purposes.
   - Researchers can access the admin panel to manage experiments and participants.
   - Researchers can create multiple experiments and manage them through the dashboard.
   - Researchers can upload custom Unity builds and associate them with experiments.

5. **Participant Workflow:**
   - Participants receive a shareable link to the experiment (multiple experiments can be run simultaneously and multipme participants can participate in the same experiment).
   - Participants access the Unity environment embedded in the browser.
   - Participants interact with the Unity environment and complete the experiment.
   - Participants are required to fill out a declaration form before accessing the Unity environment.
   - Participant data is logged and stored in the database for analysis.

---

## Technologies Used

- **Backend:**
  - Python 3.12
  - Django 4.1
- **Frontend:**
  - HTML5, CSS3, JavaScript
  - Unity WebGL
- **Database:**
  - PostgreSQL (recommended for AWS deployment)
  - SQLite (for development)
- **Deployment:**
  - Nginx or Apache (for serving Django app)
  - Whitenoise (for serving static files)
- **Others:**
  - UUID for unique identifiers
  - YamlDotNet (or similar) for YAML parsing in Unity

### Repository Structure

Current project structure:

```plaintext
animal-ai-webgl-platform/
├── gameapp/
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py
│   ├── migrations/
│   ├── models.py
│   ├── static/
│   │   ├── game_build/
│   │   │   ├── Build/
│   │   │   │   ├── AAI.data
│   │   │   │   ├── AAI.framework.js
│   │   │   │   ├── AAI.loader.js
│   │   │   │   ├── AAI.wasm
│   │   │   ├── StreamingAssets/
│   │   │       ├── your_arena_config.yaml
│   │   ├── css/
│   │   ├── js/
│   │   └── images/
│   ├── templates/
│   │   ├── base.html
│   │   ├── home.html
│   │   ├── instructions.html
│   │   ├── registration/
│   │   │   ├── login.html
│   │   │   └── register.html
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── animal_ai_webgl_platform/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
├── requirements.txt
└── README.md
```

### Configuration

1. Clone the repository and navigate to the project root.

2. Create a virtual environment and activate it:
   ```bash
   python -m venv venv
   source venv/bin/activate
   ```

3. Install the required Python packages:

   ```bash
    pip install -r requirements.txt
    ```

4. Create a `.env` file in the project root and add the following environment variables:
5. Generate a new Django secret key and add it to the `.env` file:

   ```bash
   python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
   ```

6. Set up the database connection in the `.env` file:

   ```plaintext
    DB_NAME=your_db_name
    DB_USER=your_db_user
    DB_PASSWORD=your_db_password
    DB_HOST=your_db_host
    DB_PORT=your_db_port
    SECRET_KEY=your_secret_key
    DEBUG=True
    ```
7. Run the Django migrations to create the database schema:
8. Create a superuser account to access the admin panel:

   ```bash
   python manage.py createsuperuser
   ```
9. Start the Django development server:

   ```bash
    python manage.py runserver
    ```
10. Access the Django admin panel at `http://localhost:8000/admin/` and log in with the superuser credentials.
11. Access the homepage at `http://localhost:8000/` and the admin panel at `http://localhost:8000/admin/`.

### Database Setup

1. Create a new PostgreSQL database and user in pgAdmin.
2. Grant the user all privileges on the database.
3. Update the `.env` file with the database connection details.
4. Run the Django migrations to create the database schema:

   ```bash
   python manage.py migrate
   ```


### Static Files and Collectstatic

1. Collect the static files to the `staticfiles/` directory:

   ```bash
   python manage.py collectstatic
   ```
2. This command will copy all static files (CSS, JS, Unity builds) to the `staticfiles/` directory.
3. Ensure that the `STATIC_ROOT` setting in `settings.py` points to the correct directory.
4. The `staticfiles/` directory can be served by a web server like Nginx in production.
5. In development, Django's built-in development server will serve the static files.
6. The Unity WebGL build files should be placed in the `static/game_build/` directory.
7. The `game_build/` directory should contain the `Build/` folder generated by Unity.
8. The `Build/` folder should contain the `index.html`, `AAI.data`, `AAI.framework.js`, and `AAI.wasm` files.
9. The `StreamingAssets/` directory should be placed in the `static/game_build/` directory.
10. The `StreamingAssets/` directory should contain the YAML configuration files.
11. The `StreamingAssets/` directory is accessed by the Unity app at runtime to load YAML configurations.
12. The `StreamingAssets/` directory should be accessible via HTTP requests.

### Running the Development Server

1. Start the Django development server:

   ```bash
   python manage.py runserver
   ```
2. Access the homepage at `http://localhost:8000/` and the admin panel at `http://localhost:8000/admin/`.
3. The Unity WebGL build should load on the homepage, and the admin panel should be accessible with the superuser credentials.
4. Create experiments, upload Unity builds, associate YAML configurations, and manage participant data through the admin panel.


## Django Backend Explained

The Django backend consists of multiple apps and modules that handle different aspects of the platform:

### Apps and Modules

1. **`gameapp` App:**
   - Contains the core functionality of the platform.
   - Models for experiments, participants, declarations, and data.
   - Views for rendering templates, handling form submissions, and managing data.
   - URLs for routing requests to the appropriate views.
   - Templates for rendering HTML pages.
   - Static files for CSS, JS, and Unity builds.
   - Admin configuration for managing data through the Django admin panel.
   - Middleware for handling CORS headers and other request/response modifications.
   - Forms for handling user input and data validation.
   - Signals for handling model events (e.g., post-save signals).
   - Utilities for common functions and helper methods.
   - Tests for testing the app's functionality (needs to be implemented).
   - Migrations for database schema changes.

### Models and Data Entities

1. **`Experiment` Model:**
   - Represents an experiment created by a researcher.
   - Fields include the experiment name, description, associated Unity build, and YAML configuration.
   - Has a many-to-many relationship with participants.
   - Can be created, updated, and deleted through the admin panel.
   - Can be associated with Unity builds and YAML configurations.
   - Can be used to track participant interactions and data.
  
2. **`Participant` Model:**
    - Represents a participant who interacts with the Unity environment.
    - Fields include the participant's name, email, and declaration form data.
    - Has a many-to-many relationship with experiments.
    - Can be created, updated, and deleted through the admin panel.
    - Can be associated with experiments and declaration forms.
    - Can be used to track participant interactions and data.
    - Can be used to export participant data for analysis.





