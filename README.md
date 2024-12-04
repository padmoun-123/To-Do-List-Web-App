# To-Do-List-Web-App
Build a web-based to-do list application where users can add, view, edit, and delete tasks.
Project Overview:
Objective: Build a web-based to-do list application where users can add, view, edit, and delete tasks.
Skills: Web development with Flask, HTML, CSS, JavaScript, and using SQLite for database storage.
Technologies: Python, Flask, SQLite, HTML, CSS, JavaScript.

Kuku, here’s an intermediate-level Python project idea along with code that you can work on and share on GitHub. This project involves creating a To-Do List Web Application using Flask, a lightweight web framework for Python.

Intermediate-Level Python Project: To-Do List Web App
Project Overview:
Objective: Build a web-based to-do list application where users can add, view, edit, and delete tasks.
Skills: Web development with Flask, HTML, CSS, JavaScript, and using SQLite for database storage.
Technologies: Python, Flask, SQLite, HTML, CSS, JavaScript.
Step 1: Set Up Your Environment
Install Flask and SQLite3: Install the necessary packages using pip.

bash
Copy code
pip install flask sqlite3
Step 2: Create the Flask App
Create the project folder and inside that folder, create the following files:
app.py (Main Python file for the Flask application)
templates/ (Folder for HTML templates)
static/ (Folder for CSS and JavaScript files)
Step 3: Write the Flask Application Code
app.py
python
Copy code
from flask import Flask, render_template, request, redirect, url_for
import sqlite3

app = Flask(__name__)

# Database connection
def get_db_connection():
    conn = sqlite3.connect('todo.db')
    conn.row_factory = sqlite3.Row
    return conn

# Initialize the database
def init_db():
    conn = get_db_connection()
    conn.execute('CREATE TABLE IF NOT EXISTS tasks (id INTEGER PRIMARY KEY, task TEXT NOT NULL)')
    conn.commit()
    conn.close()

@app.route('/')
def index():
    conn = get_db_connection()
    tasks = conn.execute('SELECT * FROM tasks').fetchall()
    conn.close()
    return render_template('index.html', tasks=tasks)

@app.route('/add', methods=['POST'])
def add_task():
    task = request.form['task']
    if task:
        conn = get_db_connection()
        conn.execute('INSERT INTO tasks (task) VALUES (?)', (task,))
        conn.commit()
        conn.close()
    return redirect(url_for('index'))

@app.route('/delete/<int:task_id>')
def delete_task(task_id):
    conn = get_db_connection()
    conn.execute('DELETE FROM tasks WHERE id = ?', (task_id,))
    conn.commit()
    conn.close()
    return redirect(url_for('index'))

@app.route('/edit/<int:task_id>', methods=['GET', 'POST'])
def edit_task(task_id):
    conn = get_db_connection()
    if request.method == 'POST':
        new_task = request.form['task']
        conn.execute('UPDATE tasks SET task = ? WHERE id = ?', (new_task, task_id))
        conn.commit()
        conn.close()
        return redirect(url_for('index'))
    else:
        task = conn.execute('SELECT * FROM tasks WHERE id = ?', (task_id,)).fetchone()
        conn.close()
        return render_template('edit.html', task=task)

if __name__ == "__main__":
    init_db()  # Initialize the database when the app starts
    app.run(debug=True)
Step 4: Create HTML Templates
Create two HTML templates in the templates folder: index.html and edit.html.

templates/index.html
html
Copy code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
    <div class="container">
        <h1>My To-Do List</h1>
        <form action="/add" method="POST">
            <input type="text" name="task" placeholder="New Task" required>
            <button type="submit">Add Task</button>
        </form>
        
        <ul>
            {% for task in tasks %}
                <li>
                    {{ task['task'] }}
                    <a href="{{ url_for('edit_task', task_id=task['id']) }}">Edit</a>
                    <a href="{{ url_for('delete_task', task_id=task['id']) }}">Delete</a>
                </li>
            {% endfor %}
        </ul>
    </div>
</body>
</html>
templates/edit.html
html
Copy code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Edit Task</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
    <div class="container">
        <h1>Edit Task</h1>
        <form action="" method="POST">
            <input type="text" name="task" value="{{ task['task'] }}" required>
            <button type="submit">Update Task</button>
        </form>
    </div>
</body>
</html>
Step 5: Add Styling with CSS
Create a styles.css file in the static folder to style the web app.

static/styles.css
css
Copy code
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 0;
}

.container {
    width: 50%;
    margin: 50px auto;
    padding: 20px;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
    text-align: center;
}

form input {
    padding: 10px;
    width: 80%;
    margin-right: 10px;
}

form button {
    padding: 10px 20px;
    background-color: #28a745;
    color: white;
    border: none;
    cursor: pointer;
}

form button:hover {
    background-color: #218838;
}

ul {
    list-style-type: none;
    padding: 0;
}

ul li {
    padding: 10px 0;
    border-bottom: 1px solid #ddd;
}

ul li a {
    margin-left: 10px;
    color: #007bff;
    text-decoration: none;
}

ul li a:hover {
    text-decoration: underline;
}
Step 6: Run the Flask Application
To run the application, execute the following command in your project directory:

bash
Copy code
python app.py
Visit http://127.0.0.1:5000/ in your browser to see the To-Do List web app in action.

Step 7: Share the Project on GitHub
Create a GitHub repository (e.g., flask-todo-app).
Add your files to the repository.
Create a README.md file to explain how to use the app.
Commit your changes and push to GitHub:
bash
Copy code
git init
git add .
git commit -m "Initial commit: Add To-Do List web app"
git remote add origin https://github.com/yourusername/flask-todo-app.git
git push -u origin master

