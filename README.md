<div align="center">

# Vocabulary Learning App

**Team-built Django web application for creating vocabulary sets, learning with flashcards, practicing typed answers, generating quizzes and tracking study progress.**

<p>
  <img src="https://img.shields.io/badge/Python-Django-7C3AED?style=for-the-badge&logo=python&logoColor=white" alt="Python and Django">
  <img src="https://img.shields.io/badge/REST%20API-DRF-1F1733?style=for-the-badge" alt="Django REST Framework">
  <img src="https://img.shields.io/badge/Frontend-Vanilla%20JS-7C3AED?style=for-the-badge&logo=javascript&logoColor=white" alt="Vanilla JavaScript frontend">
  <img src="https://img.shields.io/badge/Database-SQLite-1F1733?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQLite database">
  <img src="https://img.shields.io/badge/Academic-Team%20Project-7C3AED?style=for-the-badge" alt="Academic team project">
</p>

<p>
  <a href="#quick-start">Quick start</a> ·
  <a href="#project-scope">Project scope</a> ·
  <a href="#my-role">My role</a> ·
  <a href="#api-overview">API</a> ·
  <a href="#verification">Verification</a> ·
  <a href="#tests">Tests</a>
</p>

</div>

## About

This repository contains an academic group project built as a full-stack vocabulary learning application. The app lets users create public or private word sets, import vocabulary from files, practice with flashcards, check typed answers, generate quizzes and monitor learning progress over time.

The project was developed by a five-person team. It is presented here as a GitHub-ready portfolio repository with the team context kept explicit. My main contribution was the frontend layer: the browser interface, page structure, responsive layout, visual styling, interactive study views and JavaScript integration with the REST API.

## Project Scope

| Area | Implementation |
| --- | --- |
| User accounts | Registration and login with Django users and DRF token authentication. |
| Vocabulary sets | Public and private sets with ownership checks and CRUD operations. |
| Word management | Manual adding, editing, deleting and importing word pairs from CSV or JSON. |
| Flashcards | Card-based learning mode with flipping, mastered/review status and difficult-word marking. |
| Typing practice | Typed English answers checked against the selected vocabulary word. |
| Quiz mode | Automatic multiple-choice quiz generation from selected sets with saved results. |
| Progress tracking | Per-user word progress, quiz scores, mastery percentage and review queue. |

## My Role

| Contributor | Main responsibility in this portfolio context |
| --- | --- |
| Robert Tworek | Frontend/UI work: Django template structure, responsive dashboard layout, CSS styling, vanilla JavaScript state handling and connecting user flows to API endpoints. |
| Project team | Backend logic, data model, API behavior, testing and feature integration were developed as part of the five-person academic project. |

## What It Shows

| Area | Details |
| --- | --- |
| Full-stack flow | Django renders the application shell while the frontend communicates with JSON API endpoints. |
| Practical UI | One-page dashboard with login, set management, flashcards, typing practice, quiz generation and statistics. |
| Access control | Private sets are visible only to their owners, while public sets are available to logged-in users. |
| Learning state | User progress is stored per word and reused across flashcards, typed answers, reviews and statistics. |
| Import support | CSV and JSON imports allow quick creation of larger vocabulary sets. |
| Test coverage | Django tests cover the main page, authentication, set creation, imports, flashcards, quiz submission and access control for private sets. |

## Tech Stack

| Layer | Tools |
| --- | --- |
| Backend | Python, Django, Django REST Framework |
| Authentication | Django Auth, DRF Token Authentication |
| Database | SQLite |
| Frontend | HTML, CSS, Vanilla JavaScript, Django Templates |
| Testing | Django TestCase, DRF APIClient |

## Verification

The project was checked locally in a clean virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python manage.py test
```

Current verification result:

```text
Ran 21 tests in 4.259s

OK
System check identified no issues (0 silenced).
```

## Quick Start

Clone the repository and enter the project directory:

```bash
git clone <repository-url>
cd vocabulary-learning-app
```

Create and activate a virtual environment:

```bash
python -m venv .venv
```

Windows:

```powershell
.venv\Scripts\activate
```

macOS / Linux:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Apply migrations and start the development server:

```bash
python manage.py migrate
python manage.py runserver
```

Open the application:

```text
http://127.0.0.1:8000/
```

The API is available under:

```text
http://127.0.0.1:8000/api/
```

## Environment

The project uses development-friendly defaults for local work. Before deploying or sharing a public instance, set production values through environment variables:

```bash
export DJANGO_SECRET_KEY="change-me"
export DJANGO_DEBUG=False
export DJANGO_ALLOWED_HOSTS="localhost,127.0.0.1"
```

The local SQLite database file is intentionally ignored by Git.

## API Overview

Protected endpoints require the token returned during login or registration:

```http
Authorization: Token <token>
```

| Method | Endpoint | Purpose |
| --- | --- | --- |
| `POST` | `/api/register/` | Creates a user account and returns an authentication token. |
| `POST` | `/api/login/` | Authenticates a user and returns an authentication token. |
| `GET` | `/api/sets/` | Returns public sets and sets owned by the logged-in user. |
| `POST` | `/api/sets/create/` | Creates a new vocabulary set with optional starter words. |
| `GET` | `/api/sets/<set_id>/` | Returns one selected set with its words and progress. |
| `POST` / `DELETE` | `/api/sets/<set_id>/delete/` | Deletes a user-owned vocabulary set. |
| `POST` | `/api/sets/<set_id>/add_word/` | Adds a word pair to a user-owned set. |
| `POST` / `PUT` | `/api/words/<word_id>/edit/` | Updates an existing word pair. |
| `POST` / `DELETE` | `/api/words/<word_id>/delete/` | Deletes a word from a user-owned set. |
| `POST` | `/api/sets/<set_id>/import/` | Imports words from a CSV or JSON file. |
| `GET` | `/api/sets/<set_id>/flashcards/` | Returns flashcards for the selected set. |
| `POST` | `/api/words/<word_id>/check/` | Checks a typed answer and updates progress. |
| `POST` | `/api/words/<word_id>/toggle-hard/` | Marks or unmarks a word as difficult. |
| `POST` | `/api/words/<word_id>/flashcard-progress/` | Saves flashcard progress as mastered or requiring review. |
| `POST` | `/api/sets/<set_id>/quiz/` | Generates a multiple-choice quiz from a set. |
| `GET` | `/api/quizzes/<quiz_id>/` | Returns quiz questions and answer options. |
| `POST` | `/api/quizzes/<quiz_id>/submit/` | Submits quiz answers and saves the result. |
| `GET` | `/api/sets/<set_id>/review/` | Returns words selected for review. |
| `GET` | `/api/sets/<set_id>/stats/` | Returns progress and quiz statistics for a set. |

## Data Model

| Model | Responsibility |
| --- | --- |
| `WordSet` | Vocabulary set owned by a user, marked as public or private. |
| `Word` | Polish-English word pair assigned to a set. |
| `Quiz` | Generated quiz connected to a selected word set. |
| `Question` | Multiple-choice question with four options and the correct answer index. |
| `UserWordProgress` | Per-user learning progress for a single word. |
| `UserQuizResult` | Saved quiz score, number of correct answers and percentage result. |

## Import Format

CSV example:

```csv
pl,en
dom,house
pies,dog
kot,cat
```

JSON example:

```json
[
  { "pl": "dom", "en": "house" },
  { "pl": "pies", "en": "dog" },
  { "pl": "kot", "en": "cat" }
]
```

## Tests

Run the Django test suite:

```bash
python manage.py test
```

The tests cover the homepage, authentication, set creation, word import, answer checking, flashcards, word editing and quiz submission flow, plus access control: private sets stay hidden from other users, only owners can modify or delete sets and words, and unauthenticated requests are rejected.

## Repository Structure

```text
vocabulary-learning-app/
├── api/
│   ├── migrations/
│   ├── static/api/
│   │   ├── app.css
│   │   └── app.js
│   ├── templates/api/
│   │   └── index.html
│   ├── admin.py
│   ├── apps.py
│   ├── data.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── vocabapp/
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── .gitignore
├── manage.py
├── requirements.txt
├── test.csv
└── README.md
```

## Team

| Name |
| --- |
| Robert Tworek |
| Piotr Smarż |
| Kuba Kowalski |
| Maciej Gołębiowski |
| Konrad Florczak |

## GitHub Notes

This folder is suitable for publishing as a coursework or portfolio repository after initializing Git and pushing it to a remote. Runtime files are already ignored through `.gitignore`, including virtual environments, Python caches, local SQLite databases, coverage output and IDE files.

Recommended final checks before publishing:

- run `python manage.py test` after installing dependencies;
- make sure `db.sqlite3`, `.venv/`, `__pycache__/` and `.env` are not committed;
- add screenshots to the repository later if you want the GitHub page to look more visual;
- set real production environment variables before any public deployment.

## Future Improvements

- Add GitHub Actions for automated Django tests.
- Add screenshots or a short demo GIF to the README.
- Add Docker or a devcontainer for one-command local setup.
- Add password reset and stronger production deployment settings.
- Add pagination or filtering for larger public vocabulary collections.
