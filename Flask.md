## Flask 101
---
### First Flask app

1. Create a virtual env
2. `pip install flask`
3. Make a new directory
4. Make a python (*main.py* here) file with the following code,
	```python
	from flask import Flask  
	app = Flask(__name__) 		 
	
	@app.route("/")  
	def hello():  
    	return "<h1>Hello World!<h1>"
	```
5. Open terminal here and export the environment variable,
	```shell
	export FLASK_APP=main.py
	```
6. Run the app by running, `flask run` (or ) `python main.py`
	

8. Click on the link shown in the terminal and tada, you made your first flask app!

---

### Auto-reload

Under the default configuration, you would have to re-run flask everytime you make a change! We can overcome this by running,
```shell
export FLASK_DEBUG=1
flask run
```
in the terminal.

(or)

In your _main.py_,

Add the below line,

```python
if __name__ == '__main__':
	app.run(debug=True)
```

---

### Routes

```python
	from flask import Flask  
	app = Flask(__name__) 		 
	
	@app.route("/")  
	def hello():  
    	return "<h1>Hello World!<h1>"
```

In the above code snippet the `@app.route("/")` signifies the URL route for the application.

By default the function under it (`hello`) will be executed.

To add more routes copy paste the whole segment and change the route and function name.

```python
	from flask import Flask  
	app = Flask(__name__) 		 
	
	@app.route("/")  
	def hello():  
    	return "<h1>Hello World!<h1>"
		
	@app.route("/about")  
	def about():  
    	return "<h1>About page<h1>"
```

If you want multiple routes to direct to the same page, then you can do it as follows,

```python
	from flask import Flask  
	app = Flask(__name__) 		 
	
	@app.route("/")  
	@app.route("/home")
	def hello():  
    	return "<h1>Hello World!<h1>"
```

---

### ![[Templates]]

> Flask uses the jinja2 templating engine

---

### CSS & JS

You can add the [bootstrap starter template](https://getbootstrap.com/docs/4.3/getting-started/introduction/) to the _base.html_ file for a prettier look.

Static files in Flask has to be stored in a separate directory named `static`

1. You can add in your _main.css_ into it.
2. Then in _base.html_ include the _main.css_ file using ___url_for___ (it's a flask function to find the exact location of routes for us)

```python
from flask import url_for
```

```
<link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='main.css' }}">
```

### Forms and user input

Don't reinvent the wheel!

Use __wtforms__: `pip install flask-wtf`

Create a _forms.py_ file under the main project directory

```python
from flask_wtf import Flaskform
from wtforms import StringField, PasswordField, SubmitField
from wtforms.validators import DataRequired, Length, Email, EqualTo

class RegistrationForm(FlaskForm):
	
	username = StringField('Username', validators=[DataRequired(), Length(min=2, max=20)])
	
	email = StringField('Email', validators=[DataRequired(), Email()])
	
	password = PasswordField('Password', validators=[DataRequired()])
		
	confirm_password = PasswordField('Confirm Password', validators=[DataRequired(), EqualTo('password')])
	
	submit = SubmitField('Sign up')
	
	
```