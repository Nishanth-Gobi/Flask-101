
Now we almost never return the `<h1>Hello World!</h1>` alone. We would in most cases return a huge HTML file.

We can use a multiline string to add the whole HTML code in the _main.py_ file, but due to __better modularity__ and __cleaner code__ we go with templates

1. Make a templates directory within the given project.
2. Add HTML files that have to return inside it.

Once you are done you have to render these files inside the _main.py_ file.

```python
from flask import Flask, render_template  
  
app = Flask(__name__)  
  
  
@app.route("/")  
@app.route("/home")  
def hello():  
    return render_template('home.html')  
  
@app.route("/about")  
def about():  
    return "About page"  
  
  
if __name__ == '__main__':  
    app.run(debug=True)
```

Now that's how you make any good static webapp in Flask!

---

__But, what about webapps that are dynamic (have assosiated data)?__

Well this is how we do it,

```python

from flask import Flask, render_template  
  
app = Flask(__name__)  
  
posts = [  
    {  
        'author':'Nish',  
 'title':'I am awesome',  
 },  
 {  
        'author':'Nishanth',  
 'title':'I am really awesome',  
 }  
]  
  
@app.route("/")  
@app.route("/home")  
def hello():  
    return render_template('home.html', posts=posts)  
  
if __name__ == '__main__':  
    app.run(debug=True)

```

In the above code snippet, you can consider the `posts` list as a generalised representation of data.

Then we inject the template on to our HTML file,

```html

<!DOCTYPE html>  
<html>  
<head>  
 <meta charset="utf-8">  
 <title></title></head>  
<body>  
 {% for post in posts %}  
      <h1>{{ post.title }}</h1>  
 <p>By {{ post.author }}</p>  
 {% endfor %}  
</body>  
</html>

```

We just used a for loop to loop through data inside a HTML file! Wohooo!

Is that it? Not even close! We can have conditional statements as well!

Consider the following code snippets,

The _main.py_ file,
```python
from flask import Flask, render_template  
app = Flask(__name__)  
  
posts = [  
    {  
        'author':'Nish',  
 'title':'I am awesome',  
 },  
 {  
        'author':'Nishanth',  
 'title':'I am really awesome',  
 }  
]  
  
@app.route("/")  
@app.route("/home")  
def hello():  
    return render_template('home.html', posts=posts)  
  
@app.route("/about")  
def about():  
    return render_template('about.html', title="About")  
  
if __name__ == '__main__':  
    app.run(debug=True)
```

The _home.html_ file,
```html
<!DOCTYPE html>  
<html>  
<head>  
 <meta charset="utf-8">  
 {% if title %}  
      <title>A Blog - {{ title }}</title>  
 {% else %}  
      <title>A Blog</title>  
 {% endif %}  
</head>  
<body>  
 {% for post in posts %}  
      <h1>{{ post.title }}</h1>  
 <p>By {{ post.author }}</p>  
 {% endfor %}  
</body>  
</html>
```

The _about.html_ file,
```html
<!DOCTYPE html>  
<html>  
<head>  
 <meta charset="utf-8">  
 {% if title %}  
      <title>A Blog - {{ title }}</title>  
 {% else %}  
      <title>A Blog</title>  
 {% endif %}  
</head>  
<body>  
 <h1>About page</h1>  
</body>  
</html>
```

In the above scenario the _if condition_ fils in _home.html_ and we get the default title - 'A Blog'

However if the _if condition _ is satisfied in the _about.html_ file and we get the title as 'A Blog - About'

---

### Template Inheritance

Almost always we have  a base html file that helps us prevent rewriting the same code snippets in multiple HTML files.

Let's call this _base.html_ and it has the following code in it,

```html
<!DOCTYPE html>  
<html>  
<head>  
 <meta charset="utf-8">  
 {% if title %}  
      <title>A Blog - {{ title }}</title>  
 {% else %}  
      <title>A Blog</title>  
 {% endif %}  
</head>  
<body>  
 {% block content %}{% endblock %}  
</body>  
</html>
```

And the _about.html_ and _home.html_ changes as follows,

_home.html_
```html
{% extends "base.html" %}  
{% block content %}  
   {% for post in posts %}  
      <h1>{{ post.title }}</h1>  
 <p>By {{ post.author }}</p>  
 {% endfor %}  
{% endblock content %}
```

_about.html_
```html
{% extends "base.html" %}  
{% block content %}  
   <h1>About page</h1>  
{% endblock content %}
```

It's standard to add the _css_ and _javascript_ to the _base.html_ as then we would only have to make edits in one file in case of any changes in the future 

