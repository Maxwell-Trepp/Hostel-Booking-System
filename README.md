# Hostel-Booking-System
Maxwell-Trepp-patch-1
Group project
#  Off-Campus Accommodation System

A Django web application that connects university students with hostel owners near campus. Students can search and book hostels; owners can manage listings and respond to booking requests.

---

##  Table of Contents

1. [Project Overview](#project-overview)
2. [Team Members & Responsibilities](#team-members--responsibilities)
3. [Tech Stack](#tech-stack)
4. [Getting Started (Every Member Must Do This)](#getting-started-every-member-must-do-this)
5. [Project Structure](#project-structure)
6. [Environment Variables](#environment-variables)
7. [Running the Application](#running-the-application)
8. [Database & Migrations](#database--migrations)
9. [User Roles & Permissions](#user-roles--permissions)
10. [Features Overview](#features-overview)
11. [GitHub Workflow](#github-workflow)
12. [Branch Strategy](#branch-strategy)
13. [Commit Message Convention](#commit-message-convention)
14. [Pull Request Process](#pull-request-process)
15. [Coding Standards](#coding-standards)
16. [Common Commands](#common-commands)
17. [Troubleshooting](#troubleshooting)
18. [Testing Checklist](#testing-checklist)

---

## Project Overview

The Off-Campus Accommodation System solves the problem of students manually searching for hostels near campus. It provides a centralised platform where:

- **Students** can register, search for available hostels, view details, and submit booking requests.
- **Hostel Owners** can list their properties, view incoming booking requests, and approve or decline them.
- **The System** automatically enforces booking rules — a student cannot hold two active requests, and pending requests expire after 24 hours with no owner response.

---

## Team Members & Responsibilities

| Member | Role | Feature Branch |
|--------|------|----------------|
| maxwell | Team Lead — Project Setup, Settings, GitHub Admin | `feature/project-setup` |
| Packson| Authentication — Register, Login, Logout, Decorators | `feature/authentication` |
| Justice | Hostel Management — Models, Owner Dashboard, CRUD | `feature/hostel-management` |
| Shupie| Booking System — Requests, Approve/Decline, Expiry | `feature/booking-system` |
| Lauritta | Frontend & UI — Templates, Search, Pagination, Styling | `feature/frontend-ui` |

Each member works exclusively on their branch and opens a Pull Request into `develop` when their feature is ready for review.

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | Python 3.10+, Django 4.x |
| Frontend | HTML5, Bootstrap 5, Django Templates |
| Database | SQLite (development) |
| Forms | django-crispy-forms + crispy-bootstrap5 |
| Images | Pillow |
| Config | python-decouple |
| Version Control | Git + GitHub |

---

## Getting Started (Everyone  Must Do This)

Follow every step in order. Do not skip any step.

### Step 1 — Clone the Repository

```bash
git clone https://github.com/your-team/off-campus-accommodation.git
cd off-campus-accommodation
```

### Step 2 — Create and Activate a Virtual Environment

```bash
# Create the virtual environment
python -m venv venv

# Activate it
# On Windows:
venv\Scripts\activate

# On Mac or Linux:
source venv/bin/activate
```

You will know the virtual environment is active when you see `(venv)` at the start of your terminal line.

> **Important:** Never commit the `venv/` folder. It is already listed in `.gitignore`.

### Step 3 — Install All Dependencies

```bash
pip install -r requirements.txt
```

If you install any new package during development, update the file immediately:

```bash
pip freeze > requirements.txt
git add requirements.txt
git commit -m "chore: add <package-name> to requirements"
```

### Step 4 — Set Up Your Environment Variables

The project uses a `.env` file to store secret settings. This file is **never committed to GitHub**.

```bash
# Copy the example file
cp .env.example .env
```

Then open `.env` in any text editor and fill in the values:

```
SECRET_KEY=any-long-random-string-you-make-up-for-local-use
DEBUG=True
ALLOWED_HOSTS=127.0.0.1,localhost
```

Ask your team lead if you are unsure what values to use.

### Step 5 — Apply Database Migrations

```bash
python manage.py migrate
```

This creates the `db.sqlite3` database file with all the required tables.

### Step 6 — Create a Superuser (Admin Account)

```bash
python manage.py createsuperuser
```

You will be prompted to enter a username, email, and password. This account gives you access to the Django Admin panel at `http://127.0.0.1:8000/admin/`.

### Step 7 — Run the Development Server

```bash
python manage.py runserver
```

Open your browser and go to `http://127.0.0.1:8000/`. You should see the home page.

---

## Project Structure

```
off_campus_accommodation/
│
├── core/                        ← Django project settings
│   ├── settings.py              ← Main configuration
│   ├── urls.py                  ← Root URL dispatcher
│   └── wsgi.py
│
├── accounts/                    ← User authentication app
│   ├── models.py                ← CustomUser model
│   ├── views.py                 ← Register, login, logout
│   ├── forms.py                 ← Registration form
│   ├── decorators.py            ← student_required, owner_required
│   ├── urls.py
│   ├── admin.py
│   └── context_processors.py   ← Global template variables
│
├── hostels/                     ← Hostel management app
│   ├── models.py                ← Hostel model
│   ├── views.py                 ← CRUD + search + owner dashboard
│   ├── forms.py                 ← HostelForm, HostelSearchForm
│   ├── urls.py
│   └── admin.py
│
├── bookings/                    ← Booking system app
│   ├── models.py                ← BookingRequest model
│   ├── views.py                 ← Request, respond, my bookings
│   ├── forms.py                 ← BookingRequestForm, BookingResponseForm
│   ├── urls.py
│   ├── admin.py
│   └── management/
│       └── commands/
│           └── expire_bookings.py   ← Auto-expire command
│
├── templates/                   ← All HTML templates
│   ├── base.html                ← Master layout (navbar, messages)
│   ├── accounts/
│   │   ├── register.html
│   │   └── login.html
│   ├── hostels/
│   │   ├── home.html            ← Hostel listing + search
│   │   ├── detail.html          ← Single hostel page
│   │   ├── hostel_form.html     ← Add/Edit form
│   │   ├── delete_confirm.html
│   │   └── owner_dashboard.html
│   └── bookings/
│       ├── request_form.html
│       ├── my_bookings.html
│       ├── owner_requests.html
│       └── respond.html
│
├── static/
│   ├── css/style.css
│   └── js/main.js
│
├── media/                       ← Uploaded hostel images (not committed)
├── .env                         ← Secret config (not committed)
├── .env.example                 ← Template for .env (committed)
├── .gitignore
├── requirements.txt
└── manage.py
```

---

## Environment Variables

| Variable | Description | Example Value |
|----------|-------------|---------------|
| `SECRET_KEY` | Django secret key — keep this private | `django-insecure-abc123xyz...` |
| `DEBUG` | Show detailed errors (True in dev only) | `True` |
| `ALLOWED_HOSTS` | Comma-separated list of allowed domains | `127.0.0.1,localhost` |

These are read from the `.env` file by `python-decouple`. Never hardcode these values in `settings.py`.

---

## Running the Application

```bash
# Make sure your virtual environment is active first
source venv/bin/activate        # Mac/Linux
venv\Scripts\activate           # Windows

# Start the server
python manage.py runserver
```

| URL | Page |
|-----|------|
| `http://127.0.0.1:8000/` | Home — hostel listing and search |
| `http://127.0.0.1:8000/accounts/register/` | Create a new account |
| `http://127.0.0.1:8000/accounts/login/` | Login |
| `http://127.0.0.1:8000/accounts/logout/` | Logout |
| `http://127.0.0.1:8000/dashboard/` | Owner dashboard (owners only) |
| `http://127.0.0.1:8000/bookings/my-bookings/` | Student booking history |
| `http://127.0.0.1:8000/bookings/requests/` | Owner booking requests |
| `http://127.0.0.1:8000/admin/` | Django admin panel |

---

## Database & Migrations

Whenever you change a model (add a field, remove a field, change a field), you must create and apply a new migration:

```bash
# Step 1: Generate the migration file
python manage.py makemigrations

# Step 2: Apply it to the database
python manage.py migrate
```

Always commit migration files to GitHub. Other team members will run `migrate` after pulling your changes.

```bash
git add accounts/migrations/ hostels/migrations/ bookings/migrations/
git commit -m "feat(hostels): add contact_phone field to Hostel model"
```

> **If you get migration conflicts:** Tell your team lead immediately. Do not delete migration files without team discussion.

---

## User Roles & Permissions

The system has two user types controlled by the `user_type` field on `CustomUser`.

### Student

- Can register with a Student ID
- Can browse and search all available hostels
- Can view hostel details
- Can submit one booking request at a time
- Cannot book again while a request is pending (unless 24 hours pass with no owner response)
- Cannot book again if already approved
- Can book again if their request was declined or expired
- Can view the status of all their past requests

### Hostel Owner

- Can register without a Student ID
- Can add, edit, and delete their own hostels
- Can view all booking requests made to their hostels
- Can approve or decline any pending request
- Can see the current occupancy and available slots for each hostel
- Cannot make booking requests themselves

### Booking Rules Enforced by the System

1. Only students can submit booking requests.
2. A student cannot have more than one active (pending) request at a time.
3. A student with an approved booking cannot request another hostel.
4. A pending request automatically expires after 24 hours if the owner does not respond.
5. After expiry or a decline, the student is free to book again.

---

## Features Overview

### For Students

- Register and log in with a student account
- Search hostels by name, location, or price
- View hostel details including amenities, capacity, and available slots
- Submit a booking request with an optional message to the owner
- Track the status of their booking request (Pending / Approved / Declined / Expired)

### For Hostel Owners

- Register and log in with an owner account
- Add new hostels with photos, price, capacity, amenities, and contact details
- Edit or delete their hostel listings
- View a dashboard showing all their hostels with a capacity progress bar
- View all incoming booking requests from students
- Approve or decline requests with an optional response note
- See a notification badge in the navbar showing the count of unread pending requests

---

## GitHub Workflow

### Every Member — Daily Routine

Follow this routine every time you sit down to work on the project.

**Morning — before you start coding:**

```bash
# 1. Switch to develop and pull the latest changes
git checkout develop
git pull origin develop

# 2. Switch back to your feature branch and bring in the new changes
git checkout feature/your-feature-name
git rebase develop
```

**During work — commit regularly:**

```bash
# Save your progress with a meaningful commit message
git add .
git commit -m "feat(bookings): add booking expiry check to my_bookings view"
```

**End of session — push your work:**

```bash
git push origin feature/your-feature-name
```

**When your feature is complete — open a Pull Request:**

Go to GitHub → Your Repository → Pull Requests → New Pull Request.
Set the base branch to `develop` and the compare branch to your feature branch.
Fill in the PR template and request a review from at least one teammate.

---

## Branch Strategy

```
main
  └── develop
        ├── feature/project-setup
        ├── feature/authentication
        ├── feature/hostel-management
        ├── feature/booking-system
        └── feature/frontend-ui
```

| Branch | Purpose | Who pushes here |
|--------|---------|-----------------|
| `main` | Final, stable, submission-ready code | Team Lead only, via PR from develop |
| `develop` | Integration branch — all features merge here | All members, via PR |
| `feature/*` | Individual feature development | The assigned member |

**Rules:**
- Never push directly to `main` or `develop`. Always use a Pull Request.
- Never work directly on `develop`. Always create a feature branch.
- Delete your feature branch after it is merged.

---

## Commit Message Convention

Every commit message must follow this format:

```
type(scope): short description in present tense
```

**Types:**

| Type | When to use |
|------|-------------|
| `feat` | Adding a new feature |
| `fix` | Fixing a bug |
| `style` | CSS or template changes only — no logic changes |
| `refactor` | Restructuring code without changing behaviour |
| `docs` | README, comments, or docstrings |
| `chore` | Setup, configuration, or dependency updates |
| `test` | Adding or fixing tests |

**Scopes** — use the app name: `accounts`, `hostels`, `bookings`, `templates`, `settings`

**Good examples:**

```
feat(bookings): enforce 24-hour expiry rule on pending requests
fix(accounts): require student_id only when user_type is student
style(templates): improve hostel card hover shadow on home page
feat(hostels): add capacity progress bar to owner dashboard
chore: update requirements.txt with crispy-bootstrap5
docs: update README with environment variable instructions
refactor(bookings): extract active booking check into helper function
```

**Bad examples (do not do this):**

```
update stuff
fixed bug
working now
changes
wip
```

---

## Pull Request Process

When your feature is complete and all your code is pushed, open a Pull Request on GitHub.

### Pull Request Template

Copy and paste this into the PR description:

```
## What does this PR do?
<!-- Describe what you built or fixed in 2-3 sentences -->

## Changes made
<!-- List the main files you changed -->
- 
- 
- 

## Testing done
- [ ] Tested locally with python manage.py runserver
- [ ] Migrations created and applied successfully
- [ ] No existing pages are broken

## Security checklist (for booking-related PRs)
- [ ] Only students can submit booking requests
- [ ] Double-booking prevention is working
- [ ] 24-hour expiry logic is correct

## General checklist
- [ ] No .env file committed
- [ ] No db.sqlite3 committed
- [ ] requirements.txt updated if new packages were installed
- [ ] Followed commit message convention
- [ ] At least one teammate has been asked to review

Closes #<issue-number>
```

### Review Process

- At least one other team member must approve the PR before it is merged.
- The reviewer should check out the branch locally and test it if possible.
- If changes are requested, the author makes the fixes and pushes again — the PR updates automatically.
- Once approved, the author merges the PR themselves.

---

## Coding Standards

These rules must be followed by all team members for the codebase to stay consistent.

### Python

- Use 4 spaces for indentation — never tabs.
- Keep functions focused — one function does one thing.
- Add a one-line comment above any logic that is not immediately obvious.
- Use the custom decorators instead of manual checks inside views:

```python
# Correct — use decorators
@student_required
def request_booking(request, hostel_id):
    ...

# Wrong — do not do this
@login_required
def request_booking(request, hostel_id):
    if not request.user.is_student():
        ...
```

### HTML Templates

- Always extend `base.html` at the top of every template.
- Always include `{% csrf_token %}` inside every `<form>` tag.
- Use `{% url 'name' %}` for all links — never hardcode URLs.
- Use `{% load crispy_forms_tags %}` and `{{ form|crispy }}` for all forms.
- Use `{% for message in messages %}` block from `base.html` — do not create your own alerts.

### Django

- Never put business logic inside templates — keep it in views or models.
- Use `get_object_or_404` when fetching a single object by primary key.
- Use Django messages (`messages.success`, `messages.error`) for user feedback.
- Never use `print()` for debugging — use Django's logging if needed.

---

## Common Commands

```bash
# Activate virtual environment
source venv/bin/activate              # Mac/Linux
venv\Scripts\activate                 # Windows

# Start the development server
python manage.py runserver

# Create database migrations after changing a model
python manage.py makemigrations

# Apply migrations to the database
python manage.py migrate

# Create a superuser for the admin panel
python manage.py createsuperuser

# Manually expire old pending booking requests
python manage.py expire_bookings

# Open the Django interactive shell
python manage.py shell

# Collect static files (run before deployment)
python manage.py collectstatic

# Check for project errors without starting the server
python manage.py check
```

---

## Troubleshooting

### "ModuleNotFoundError: No module named 'django'"
Your virtual environment is not active. Run `source venv/bin/activate` (Mac/Linux) or `venv\Scripts\activate` (Windows) first.

### "No such table: accounts_customuser"
You have not run migrations. Run `python manage.py migrate`.

### "TemplateDoesNotExist"
The template file is missing or the path is wrong. Check that the file exists in the `templates/` folder and the name matches exactly what is in the view.

### "CSRF verification failed"
You forgot `{% csrf_token %}` inside your form tag. Add it immediately after the `<form>` opening tag.

### "You are trying to add a non-nullable field" (migration error)
You added a required field to a model. Either provide a default value in the model field definition or make the field nullable with `null=True, blank=True`.

### Git merge conflict
Open the conflicting file in your editor. Look for lines starting with `<<<<<<<`, `=======`, and `>>>>>>>`. Keep the correct version of the code, remove the conflict markers, then:

```bash
git add the_conflicting_file.py
git rebase --continue
```

### "Permission denied" when accessing a page
You are logged in as the wrong user type. For owner-only pages, log in as an owner. For student-only pages, log in as a student.

---

## Testing Checklist

Before opening a Pull Request, test every item relevant to your feature.

### Authentication
- [ ] A student can register with a student ID
- [ ] A hostel owner can register without a student ID
- [ ] Registering as a student without a student ID shows an error
- [ ] Login with correct credentials works
- [ ] Login with wrong password shows an error message
- [ ] Logout redirects to the login page
- [ ] Accessing a protected page while logged out redirects to login

### Hostel Management
- [ ] An owner can add a hostel with an image
- [ ] An owner can edit their own hostel
- [ ] An owner can delete their own hostel
- [ ] A student attempting to add a hostel is denied access
- [ ] The home page shows all available hostels
- [ ] The search form filters by name and location correctly
- [ ] The hostel detail page shows all information

### Booking System
- [ ] A student can submit a booking request
- [ ] An owner trying to book sees an error
- [ ] A student with a pending request cannot book again
- [ ] A student with an approved booking cannot book again
- [ ] A student with a declined request can book again
- [ ] A student whose request expired (24 hours) can book again
- [ ] A full hostel cannot be booked — button is disabled
- [ ] An owner can approve a pending request
- [ ] An owner can decline a pending request
- [ ] An owner cannot respond to another owner's bookings
- [ ] Approved hostel shows one fewer available slot
- [ ] Student can see status updates in My Bookings

### Admin Panel
- [ ] Superuser can log into `/admin/`
- [ ] All users, hostels, and bookings are visible and editable

---



---



main
