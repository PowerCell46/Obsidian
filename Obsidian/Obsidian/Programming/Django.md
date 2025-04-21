## Django SQL queries

```python
models_list = Model.objects.raw("SELECT * FROM web_model1")

param1 = 1

# Possible SQL injection
selected_model = Model.objects.raw(f"SELECT * FROM web_model1 WHERE id = {param1}")
```

# CTRL . -> suggestions
## reverse()

- URL reversing is a process in Django that allows you
- to generate a URL for a specific view
- based on its name and any arguments it may require

```python
from django.urls import reverse

url = reverse('department-by-id', kwargs={'department_id': 1})
```
- Helps you to decouple URLs from view functions
- Allows you to generate URLs based on the current state of your application and the data available

```python
from django.shortcuts import reverse, redirect

def show_department_by_name(request, department_name):
	# find the id of the department by its name
	url = reverse('department-by-id', kwargs={'department_id': found_department_id})
	return redirect(url)

def show_department_by_id(request, department_id)
```

```python
from django.http import HttpResponse, Http404

return HttpResponseNotFound('Department was not found')

return HttpResponse(status=404)

raise Http404

```

__________________________________________________________________________
# Django Template filters

use the pipe symbol (|) followed by the filter's name \
**{{ variable_name|filter_name }}**

Filters can also be chained 
The output of one filter is passed as input to the next

Filters can also be chained 
The output of one filter is passed as input to the next

Certain filters may require arguments 
Use a colon (:) followed by the argument values 
**{{ variable_name|filter_name:argument }}**

```python
{{ list|join:", " }}

{{ my_date|date:"D d M Y" }}

{{ value|default:"nothing" }}

{{ value|floatformat:N }}
```


```python
{% for employee in employees %}- {{ employee.first_name }}
{% empty %}- No employees in this list.
{% endfor %}
```

## Django custom filters

1. Create a package named `templatetags` inside the python application
2. Create file e.g. ``number_filters.py`

```python
from django import template  
  
register = template.Library()  
  
  
@register.filter  
def only_odd(numbers):  
    return only_meet_condition(numbers, lambda n: n % 2 != 0)  
  
  
@register.filter  
def only_even(numbers):  
    return only_meet_condition(numbers, lambda n: n % 2 == 0)  
  
  
def only_meet_condition(numbers, condition_func):  
    return [n for n in numbers if condition_func(n)]
```

3. load the library in the template

```python
{% load number_filters %}
```

__________________________________________________________________________
## Nesting templates

Template snippets provide a way to include templates within another template

```python
{% include "nav-bar.html" %}
```

## Adding a parameter to a template

```python
{% include 'partials/header.html' with title='Cats app' %}
```

## Django Custom Tags

1. Create a package named `templatetags` inside the python application
2. Create file `article_tag.py`

```python
register = template.Library()

@register.inclusion_tag('articles.html')
def show_articles():
	articles = Article.objects.all()
	return {'articles': articles}
```

4. Create file e.g. ``articles.html`

```html
<ul>
	{% for article in articles %}
		<li>{{article.title}}</li>
	{% endfor %}
</ul>
```

5. Load the library

```python
{% show_articles %}
```


```python
class NameForm(forms.Form):
	name = forms.CharField(error_messages={"required": "Please, enter your name"})


class UserName(models.Model):
	username = models.CharField(max_length=50, unique=True, error_messages={"unique": "The name is already taken."})
```

## Cleaning a certain field

```python
def clean_email(self):
	email = self.cleaned_data.get('email')

	if email and not email.endswith('@gmail.com'):
		raise ValidationError('WrongDomain')

	return email
```

__________________________________________________________________________
# Authentication

### Importing the user model

```python
from django.contrib.auth import get_user_model 

UserModel = get_user_model()
```

### Creating a new user

```python
from django.contrib.auth import get_user_model 

UserModel = get_user_model() 

new_user = UserModel.objects.create_user('peter', 'peter@gmail.com', 'peterpass')
```

### Authenticate Users
If the credentials are correct and match an existing user, `authenticate()` returns the corresponding `User` object. If not, it returns `None`.

**You typically use this when you need to verify user credentials (e.g., during login).**

```python
from django.contrib.auth import authenticate 

user = authenticate(username='peter', password='peterpass') 

if user: 
	# Credentials are valid 
else: 
	# Credentials are not valid
```
### Login User
**This is used typically after successful authentication to establish a session for the user.**

```python
from django.contrib.auth import login, get_user_model 

def index(request): 
	UserModel = get_user_model() 
	some_user = UserModel.objects.get(username='Peter') print(request.user.__class__.__name__) # AnonymousUser login(request, some_user) print(request.user.__class__.__name__) # User return render(request, 'home_page.html')
```

### Logout User

```python
from django.contrib.auth import logout 

def logout_page(request): 
	print(request.user.__class__.__name__) # User 
	logout(request) print(request.user.__class__.__name__) # AnonymousUser 
	return render(request, 'logout_page.html')
```

### Login Required
- **LoginRequiredMixin** is a Django class-based view mixin that enforces login requirements for views 
- It ensures that only authenticated users can access certain view

```python
rom django.contrib.auth.mixins import LoginRequiredMixin 
from django.views.generic import ListView 


class MyProtectedListView(LoginRequiredMixin, ListView): 
	# Your view logic here
```

## Groups
### Custom Access Endpoint Decorator

```python
from django.http import HttpResponse
from django.shortcuts import render


def allowed_groups(allowed_roles=[]):
	def decorator(view_func):
		def wrapper(request, *args, **kwargs):
			group = None
			if request.user.groups.exists():
				group = request.user.groups.all()[0].name
			if group in allowed_roles:
				return view_func(request, *args, **kwargs)
			else:
				return HttpResponse('You are not allowed to view the articles')
		return wrapper
	return decorator


...

from .decorators import allowed_groups


@allowed_groups(['Users'])
def index(req):
	articles = Article.objects.all()
	return render(req, 'index.html', {'articles': articles})
```

# [[Django Extending the User Model]]

# Signals
???
