## Webapps with Flask
Today I started creating a web app on PythonAnywhere.com (= PA in the sequel).

With the free account you can create one web app, which will be located at <username>.pythonanywhere.com.

You can choose different frameworks, the tutorial suggested to start with Flask.

An important difference with streamlit is that the module is read & executed only once,
then depending on the URL path a.k.a. route, it will call chosen functions.
Thus, global data will remain available during (possibly accross) sessions.
```
# This is ~/website/flask_app.py :
from flask import Flask, render_template, session, request, redirect, url_for
from werkzeug.security import generate_password_hash, check_password_hash
import json

app = Flask(__name__)
app.secret_key = "my_PA_webapp_random_secret" # for signing the session(cookies?)
cache = {} # this will be "persistent", possibly cross-user
# Flask.session will be a dict specific to the user/visitor's session
#
@app.route("/") # this decorator tells Flask that visiting this (URL)"PATH" should call the home() function
def home(): return render_template("home.html")

@app.route("/logout")
def logout(): session.pop("username", None); return redirect("/")

@app.route("/user") # it's possible to specify args to the function that must be part of the path
def user():
    if request.method == "POST":
        vars().update((v,request.form.get(v))for v in('username','password'))
        if username and password:
            if check_credentials(username, password): # for_url() yields the route for a given function
                session["username"] = username ; return redirect(url_for('user'))
            return render_template(user.html, error_msg="Wrong username or password.")

# The following function has no URL path to it. If a function expects arguments,
# they must be part of the 'route', e.g.: @app.route("/user/<string:username>/<int:password>")
def check_credentials(username, password):
    users = load_users() # try: json.load(f := open(USERFILE)), except FileNotFoundError: ...)
    if username in users: return check_password_hash(users[username], password)

def set_password(username, password):
    if not session.get("username"): return "You must be logged in!"
    users = load_users()
    users[username] = generate_password_hash(password)
    try:
        with open(USERFILE, "w") as f: json.dump(users, f)
    except Exception as E: return f"Error when writing file: {E}"
```
The functions perform calculations, they can access HTTP info using Flask.request as shown above, 
but they should use the templates for rendering the pages. For example, you may have the following **./templates/base.html**:
```
<!DOCTYPE html>
<html><head><title>{% block title %}MFH's Web Apps on PythonAnywhere{% endblock %}</title></head>
<body>
    <nav>
        <a href="/">Home</a>
        <a href="/something">Something</a>
    <span style="float:right;">
        <a href="/user">[{{ username or "login" }}]</a>
    </span>
    </nav>
    <div id="content">
        {% block content %}{% endblock %}
    </div>
</body></html>
```
And this might be **~/website/templates/user.html**:
```
{% extends "base.html" %}
{% block title %}User account and settings{% endblock %}
{% block content %}
{% if username %}
    <h1>Hello {{ username }} !</h1>
    To log out, <a href="{{ url_for('logout') }}">click here: logout</a>.
{% else %}
    <h1>Login</h1>
    {% if error %}
        <p style="color:red;">{{ error }}</p>
    {% endif %}
  <form method="post">
    <label>Username: <input name="username" value="{{ uname|default('') }}"></label>
    <label>Password: <input name="password" type="password"></label>
    <button type="submit">Login</button>
  </form>
{% endif %}
```
In `{% ... %}` you can use several control structures like `for x in cache.things`
(with dict.key being a Jinja shortcut for dict['key'],), `if-else-endif` as seen earlier, etc.
and in `{{ ... }}` you can use most valid Python expressions plus other stuff, *e.g.*,
filters appended to expressions with `expr|filter`.

There are quite a few more things to say, but I'll stop here for now.

Check out; *e.g.*, [this website for docs on Jinja templates and other stuff](https://tedboy.github.io/jinja2/templ13.html).
