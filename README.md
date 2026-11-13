# Library Management System

A beginner-friendly Python command-line project for managing a small library.

> This was my first-ever programming project. It includes a basic interactive frontend in the terminal and JSON-based data storage.

## Features

- View available books
- Lend a book to a user
- Return borrowed books
- Add and remove books
- See all currently issued books
- Check which books a particular user has borrowed
- Save book and lending data in `library_data.json`

## Project files

- `library_project.py` — the interactive application
- `library_data.json` — the saved library catalogue and lending records

## Getting started

1. Clone the repository:

   ```bash
   git clone https://github.com/jugaadpreet/library.py.git
   cd library.py
   ```

2. Run the application with Python 3:

   ```bash
   python library_project.py
   ```

3. Choose an option from the menu to manage the library.

## Data storage

The application automatically reads from and writes to `library_data.json`. Your changes to the books and lending records are kept between runs.

## Built with

- Python
- JSON
