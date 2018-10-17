# Purpose
The purpose of this project was to learn (and provide a template for) using:  
* the Flask-Login library.  The example use case is entering a password to gain access to a web page.
* the Flask-dotenv library to use both private (.env file - not checked into GitHub) and public (.flashenv - e.g.: FLASH_APP and FLASH_ENV are defined here) variables.
* the Flask-WTF library for handling forms in Flask.
* the Flask-Bootstrap4 library so the web pages look "professional" without me having to cruft all the fancy-schmanzy CSS
* PyCharm as a "time saver" IDE for Flask apps.
# Getting Started
```angular2html
python -m venv venv --prompt flask_login
source venv\bin\activate
(flask_login) pip -r requirements.txt
```


## Pycharm
I started using Pycharm for this project.  Previously, I had been using Atom and to a lesser extent Eclipse for my Python projects.  I currently have many reasons to favor Pycharm:  
* Excellent integration with GitHub. 
  * Easy to get started.  I found PyCharm's videos to be very helpful. 
    * [In-Depth VCS #1: Getting Started](https://www.youtube.com/watch?v=jFnYQbUZQlA)
    * [In-Depth VCS #2: Core VCS](https://www.youtube.com/watch?v=_w9XWHDSSa4)
    * [In-Depth VCS #3: Branching and Merging](https://www.youtube.com/watch?v=AHkiCKG-JhM)
  * Nice touches. For example, as I type this, the markup is shown in the pane to the right of me in WYSIWIG.
* Intelligent pip install of packages I start using within my code.  For example, when I type time.delay(20) and don't have the time module installed, PyCharm shows an indicator and asks if I wish to install it.
* There are plenty more things to discover.  For example, the Pytest integration is excellent.  I am just learning Pytest and am finding the video [_Productive pytest with PyCharm_](https://www.youtube.com/watch?v=ixqeebhUa-w&t=919s) to be a great resource.

One thing I found confusing at first was setting up the project's environment configuration.  While not as confusing as Eclipse, it took me awhile to integrate what tools I wanted to use, etc.  I ended watching several PyCharm videos to learn best techniques for handling preferences.    
## Logging In
The protagonist of this project is the Flask-Login module.  As noted in the resources, Flask-login revolves around a User class that inherits from UserMixin.  I put my User class in login_user.py.  app.py has the goo for instantiating the login_manager and setting up the callback that hands over our user object to Flask-Login.
## Environment Variables
There appear to be several add-on libraries for setting up environment variables.  I liked the thought process behind Flask-dotenv.  I can split up private variables in a .env file and not push this file to Git.  I can make my .flaskenv public.  For example, here is my .flaskenv file:
```
FLASK_APP=app.py
FLASK_ENV=development
```
Going to the run and debug configuration I'm using (Login_config)  I then set the run parameters to ```run --port=9999```.  The thing I don't like/understand is it seems I have to say what host and port I want from the run parameters part of configuration and not within .flaskenv.  A .env example might be ```SECRET_KEY='myverysecretkey'```.  I don't want to share that with anyone who uses this code, so this variable is in .env.  When I want to access it:
```angular2html
import os
my_secret_key = os.getenv('SECRET_KEY')

---or----

app.config['SECRET_KEY'] = os.getenv('SECRET_KEY')

```
An initial challenge I had with using environment variables was placement of the .env and .flaskenv files.
```angular2html
├── .env
├── .flaskenv
├── .gitignore
├── Readme.md
├── app.py
├── login_user.py
├── requirements.txt
├── templates
│   ├── dashboard.html
│   └── login.html
└── venv
    ├── bin
    ├── include
    ├── lib
```
The above tree shows the .flaskenv and .env files are at the same directory level as the flask app.
## Quality HTML
I was ecstatic to find flask-bootstrap4!  I use Bootstrap 4 to build higher quality web sites using Bootstrap's "canned" CSS.  For example:
```angular2html
{% extends "bootstrap/base.html" %}
{% import "bootstrap/wtf.html" as wtf %}

{% block title %}
    Login
{% endblock %}

{% block content %}

    <div class="container">
        {% with messages = get_flashed_messages() %}
            {% if messages %}
                {% for message in messages %}
                    <div class="alert alert-info" role="alert">{{ message }}</div>
                {% endfor %}
            {% endif %}
        {% endwith %}
        <form class="form-signin" method="POST" action="/login">
            <h2 class="form-signin-heading">Please sign in</h2>
            {{ form.hidden_tag() }}
            {{ wtf.form_field(form.password) }}
            <button class="btn btn-lg btn-primary btn-block" type="submit">Sign in</button>
        </form>

    </div> <!-- /container -->


{% endblock %}
```
The example shows integration with the flask-wtf library as well as flask's flash() function, which I use in a route within app.py.
# Resources
* Flask-Login, Flask-WTF forms
  * I subscribe to Treehouse. I found Kenneth's course, [_Build a Social Network with Flask_](https://teamtreehouse.com/library/build-a-social-network-with-flask) the best way for me to learn how to set up and use Flask-Login.
  * [ReadTheDocs](https://flask-login.readthedocs.io/en/latest/) 
  * [The Flask Mega-Tutorial Part V: User Logins](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-v-user-logins)  
  * [Build a User Login System With Flask-Login, Flask-WTForms, Flask-Bootstrap, and Flask-SQLAlchemy](https://www.youtube.com/watch?v=8aTnmsDMldY)
* pytest
  * [Productive pytest with PyCharm](https://www.youtube.com/watch?v=ixqeebhUa-w&t=919s)
  * [Python Testing with pytest](https://pragprog.com/book/bopytest/python-testing-with-pytest)
* Using Bootstrap and using flash() with Bootstrap
  * [The Flask Mega-Tutorial Part XI: Facelift](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-xi-facelift)
