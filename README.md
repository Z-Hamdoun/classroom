# Classroom
 
A desktop app (built with [Kivy](https://kivy.org)) for tracking a class of 28 students across two semesters: per-student grades, a profile page per student, and class-wide statistics (averages, rankings) rendered with matplotlib.
 
## Features
 
- Student profile pages with grades for 5 exams per semester, editable in-app
- Automatic semester averages per student
- Class-wide statistics screen:
  - Pass/fail split and class average per semester
  - Bar chart of average mark per exam (Semester I vs II)
  - Top 10 / bottom 10 student rankings per semester
- SQLite-backed storage (`data.db`)
## Requirements
 
- Python 3.10+
- [Kivy](https://kivy.org) 2.x
- matplotlib
- numpy
## Installation
 
```bash
python -m venv venv
source venv/bin/activate
 
pip install kivy matplotlib numpy
```
 
## Setup
 
If `data.db` doesn't already contain your class data, populate it first:
 
```bash
python fill_db.py
```
 
This creates placeholder rows for 28 students in the `sem2` table. You'll also need to add rows to the `students` and `sem1` tables (not currently handled by `fill_db.py`) before the profile pages and Semester I stats will show real data.
 
## Usage
 
```bash
python classroom.py
```
 
- Click a column on the main screen to open a row of students (`rangee 1`–`3`)
- Click a student to open their profile page and edit grades
- Use the stats screen to view class-wide averages and rankings
- Press `Esc` to go back to the main screen from any sub-screen
## Project structure
 
| File | Purpose |
|---|---|
| `classroom.py` | App entry point, screen navigation, profile editing logic |
| `database.py` | SQLite access layer (`StudentsDB`) |
| `my_stats.py` | Statistics screen logic and chart rendering |
| `fill_db.py` | One-off script to seed `data.db` with blank rows |
| `*.kv` | Kivy layout files (`main`, `rangees`, `student`, `classroom`, `my_stats`) |
| `data.db` | SQLite database (students, sem1, sem2 tables) |
 
## Notes
- This project dates back to 2021, it was not coded with customization in mind so many aspects are hardcoded.
- Chart rendering uses matplotlib's `Agg` backend, with figures converted to PNG and displayed as Kivy `Image` widgets — this avoids the old, unmaintained `kivy.garden.matplotlib` backend, which is incompatible with Python 3.12+ (relies on the removed `distutils` module).
- `compute_moyennes()` in `database.py` references a `moyennes` table that isn't created by `StudentsDB.start()` — add that table before relying on saved edits to recompute class-wide exam averages.
