# Flask-Basic


Sure! Here are some notes for getting started with Flask, a lightweight web framework for Python:

### Flask Basics

#### 1. **Introduction to Flask**
- **Flask** is a micro web framework for Python based on Werkzeug and Jinja2.
- It is lightweight and designed with simplicity in mind, making it easy to start with web development in Python.

#### 2. **Installation**
- Install Flask using pip:
  ```sh
  pip install Flask
  ```

#### 3. **Hello World Example**
- Create a simple Flask application:
  ```python
  from flask import Flask

  app = Flask(__name__)

  @app.route('/')
  def hello_world():
      return 'Hello, World!'

  if __name__ == '__main__':
      app.run(debug=True)
  ```
- Run the application:
  ```sh
  python app.py
  ```

#### 4. **Routing**
- Define routes to handle different URLs:
  ```python
  @app.route('/home')
  def home():
      return 'Welcome to the Home Page'

  @app.route('/about')
  def about():
      return 'About Page'
  ```

#### 5. **Dynamic Routing**
- Use dynamic routing to capture parts of the URL:
  ```python
  @app.route('/user/<username>')
  def show_user_profile(username):
      return f'User: {username}'

  @app.route('/post/<int:post_id>')
  def show_post(post_id):
      return f'Post ID: {post_id}'
  ```

### Templates

#### 1. **Using Templates**
- Templates are used to render HTML pages. Flask uses Jinja2 as its template engine.
- Create a `templates` folder and add HTML files.
  ```html
  <!-- templates/index.html -->
  <!doctype html>
  <html>
    <head><title>Hello</title></head>
    <body>
      <h1>Hello, {{ name }}!</h1>
    </body>
  </html>
  ```

#### 2. **Rendering Templates**
- Render templates using the `render_template` function:
  ```python
  from flask import render_template

  @app.route('/hello/<name>')
  def hello(name):
      return render_template('index.html', name=name)
  ```

### Forms and Request Handling

#### 1. **Handling Forms**
- Use the `request` object to handle form data:
  ```html
  <!-- templates/form.html -->
  <form action="/submit" method="post">
    <input type="text" name="name">
    <input type="submit">
  </form>
  ```

- Handle form submission in the view:
  ```python
  from flask import request

  @app.route('/submit', methods=['POST'])
  def submit():
      name = request.form['name']
      return f'Hello, {name}!'
  ```

### Static Files

#### 1. **Serving Static Files**
- Create a `static` folder for static files like CSS, JavaScript, and images.
- Reference static files in templates:
  ```html
  <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
  ```

### Database Integration

#### 1. **Using SQLite with Flask**
- Use SQLite with Flask through the `sqlite3` module or Flask extensions like `Flask-SQLAlchemy`.
- Simple SQLite integration:
  ```python
  import sqlite3
  from flask import g

  DATABASE = 'mydatabase.db'

  def get_db():
      db = getattr(g, '_database', None)
      if db is None:
          db = g._database = sqlite3.connect(DATABASE)
      return db

  @app.teardown_appcontext
  def close_connection(exception):
      db = getattr(g, '_database', None)
      if db is not None:
          db.close()
  ```

#### 2. **Using Flask-SQLAlchemy**
- Install Flask-SQLAlchemy:
  ```sh
  pip install Flask-SQLAlchemy
  ```

- Configure and use SQLAlchemy:
  ```python
  from flask_sqlalchemy import SQLAlchemy

  app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///mydatabase.db'
  db = SQLAlchemy(app)

  class User(db.Model):
      id = db.Column(db.Integer, primary_key=True)
      username = db.Column(db.String(80), unique=True, nullable=False)

  db.create_all()
  ```

### Flask Extensions

- Flask has many extensions for adding functionality, such as authentication, forms, and more.
- Some popular Flask extensions:
  - Flask-WTF for forms
  - Flask-Login for user authentication
  - Flask-Migrate for database migrations

### Example Project Structure

```
my_flask_app/
├── app.py
├── templates/
│   ├── index.html
│   └── form.html
├── static/
│   └── style.css
├── models.py
└── config.py
```

### Running the Flask Application

- Set the `FLASK_APP` environment variable and run the application:
  ```sh
  export FLASK_APP=app.py
  flask run
  ```

This should give you a good starting point for working with Flask. As you progress, you can explore more advanced features and best practices for building scalable and secure Flask applications.
