# Initialize Django project

1. pip install django
2. django-admin startproject *projectname*
3. cd *projectname*
4. Optionally create an app: python manage.py startapp *appname*
5. Optionally run the server: python manage.py runserver

# Run Django DbShell

- python manage.py dbshell

# Run Django Shell

- python manage.py shell

```python
from main_app.models import Employee 

all_employees = Employee.objects.all()
```

## Null - database-related

- False by default. If True, empty values will be stored as NULL
Use for non-string fields such as integers, Booleans, and dates
## Blank - validation-related

- False by default. If True, the field is allowed to be blank
## Choices

```python
MONTHS = [  
    ("Jan", "January"),  
    ("Feb", "February"),  
    ("Mar", "March"),  
    ("Apr", "April"),  
    ("May", "May"),  
    ("Jun", "June"),  
    ("Jul", "July"),  
    ("Aug", "August"),  
    ("Sep", "September"),  
    ("Oct", "October"),  
    ("Nov", "November"),  
    ("Dec", "December"),  
]
```

# Django migrations

```python
python manage.py migrate main_app -> apply migrations to one app

# To revert a certain migration, pass the app name and the number of the migration you need to revert to
python manage.py migrate main_app 0001 -> apply specific migration

# To reverse all already applied migrations, use the app name and the name zero as parameters
python manage.py migrate main_app zero

# Sqlmigrate -> prints the SQL for the named migration
	python manage.py sqlmigrate main_app 0001_initial()
```

# Lookup Keys Django

```python
Employee.objects.filter(id__gt=2)
Employee.objects.exclude(id__gte=2)

Employee.objects.filter(id__lt=5)
employees = Employee.objects.filter(id__lte=5)

Employee.objects.filter(id__range=(2, 5))

Employee.objects.exclude(job_level__exact="Jr.")
Employee.objects.get(email_address__iexact="a@b.com")


Employee.objects.exclude(job_title__contains="Engineer") Employee.objects.filter(job_title__icontains="engineer")

Employee.objects.exclude(job_level__startswith="Sr.") Employee.objects.filter(job_title__endswith="Engineer")
```
### [More Info](https://docs.djangoproject.com/en/5.0/topics/db/queries/)

# Related Name

By default, the related name is generated automatically 
by appending _set to the lowercase name of the model that defines the foreign key
```python
author = Author.objects.create(name='J.K. Rowling')

Book.objects.create(title='Harry Potter and the Sorcerer\'s Stone', author=author)
Book.objects.create(title='Harry Potter and the Chamber of Secrets', author=author)

books = author.book_set.all()
```

# Many to many ***Through Table***

Mostly used when associating extra data with a many-to-many relationship 
- Gives more control 
- Allows adding extra fields

```python
class Employee(models.Model):
	... 

class Project(models.Model): 
	... 
	employees = models.ManyToManyField( Employee, through='ProjectAssignment' )

class ProjectAssignments(models.Model):
		employee = models.ForeighKey(Employee, on_delete=models.CASCASE)
		project = models.ForeignKey(Project, on_delete=models.CASCASE)
		start_date = models.DateField()
		role = models.CharField(max_length=30)
```

# Self-Referential Foreign key

```python
class Employee(models.Model):
	...
	manager = models.ForeignKey('self', on_delete=models.Cascade, null=True, blank=True, related_name="employees")
```

# Lazy Relationships

When resolving circular dependencies between two models 
Using strings to define model relationships without direct imports

```python
class Manager(models.Model):
	team = models.ManyToManyField('Employee')

class Employee(models.Model):
	team_leader = models.ForeignKey('Manager')
```

## Proxy models 

allow you to 
- create a new model that behaves exactly like an existing model with some customizations added 
- The proxy model uses the same database table as the original model 
- Useful when adding extra methods, managers, or custom behaviour to an existing model without modifying the original model.

__________________________________________________________________________
## Custom Validation

```python
class ZooKeeper(Employee):
	...

	def clean(self):
	super().clean()

	choices = [choice[0] for choice in SPECIALITIES]
	if self.speciality not in choices:
		raise ValidationError("Specialty must be a valid choice.")
```

## Custom methods

```python
class ZooDisplayAnimal(models.Model):
	...
	
	def display_info(self):
		return f'Meet {self.name}! It\'s a {self.species} and it\'s born {self.birth_date}. It makes a noice like "{self.sound}"! {self.__extra_info()}'	
```

## Custom Fields

```python
class BooleanChoiceField(models.BooleanField):
	def __init__(self, *args, **kwargs):
		kwargs['choices'] = ((True, 'Available'), (False, 'Not Available'))
		kwargs['default'] = True
		super().__init__(*args, **kwargs)

class Veterinarian(models.Model):
	...
	availability = BooleanChoiceField()

	def is_available(self):
		return self.availability 
```
## Custom Property

```python
class Animal(models.Model):
	...

	@property
	def age(self):
		today = self.today()
		age = today.year - self.birth_date.year - ((today.month, today.day) < (self.birth_date.month, self.birth_date.day))
		return age
```

# Table Index

```python
class MyModel(models.Model): 
	title = models.CharField(max_length=200, db_index=True) 
	author = models.CharField(max_length=100)

	class Meta: indexes=[ 
	# Composite Index
	models.Index(fields=["title", "author"]), 
	# Single-field Index
	models.Index(fields=["publication_date"])
```

# Mixins

```python
class TimestampMixin(models.Model): 
	created_at = models.DateTimeField(auto_now_add=True) 
	updated_at = models.DateTimeField(auto_now=True) 
	
	class Meta: 
		abstract = True


class MyModel(TimestampMixin): 
	name = models.CharField(max_length=100) 
	...
```

__________________________________________________________________________
# Advanced Queries

## Custom Managers

```python
class EmployeeManager(models.Manager):
	def by_job_title(self, job_title):
		return self.filter(job_title=job_title)


class Employee(models.Model):
	first_name = models.CharField(max_length=100)
	...

	objects = EmployeeManager


def get_software_eng():
	software_engineers = Employee.objects.by_job_title('Software Engineer')
	print(software_engineers)
```


## Annotations
allows you to add calculated fields to your query results
- #### GROUP BY in SQL

```python
class Employee(models.Model): 
	first_name = models.CharField(max_length=100) 
	last_name = models.CharField(max_length=100) 
	job_title = models.CharField(max_length=100) 
	...


def count_per_job_title():
	# SELECT job_title, COUNT(id) FROM Employee group by job_title;
	employee_counts = Employee.objects.values('job_title')
	.annotate(num_employees=Count('id'))

	for entry in employee_counts:
		print(f'Job Title: {entry['job_title']}, Number of Employees: {entry['num_employees']}')
```

## Select_related

- #### JOIN in SQL

```python
from django.db import models

class Author(models.Model):
    name = models.CharField(max_length=100)

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)


# SELECT * FROM books JOIN author ON books.author_id = author.id
books = Book.objects.select_related('author').all()
for book in books:
    print(book.title, book.author.name)

```

## Prefetch_related

- #### Joining Many-To-Many Relationship in SQL

```python
from django.db import models

class Author(models.Model):
    name = models.CharField(max_length=100)

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.ForeignKey(Author, related_name='books', on_delete=models.CASCADE)

class Publisher(models.Model):
    name = models.CharField(max_length=100)
    books = models.ManyToManyField(Book, related_name='publishers')


# Fetching all of the books for every author
authors = Author.objects.prefetch_related('books').all()
for author in authors:
    print(author.name)
    for book in author.books.all():
        print(f" - {book.title}")

```

__________________________________________________________________________

# Query-related Tools

## Q object

using logical operators
AND (&), OR (I), NOT (~), and XOR (^)

```python
def filter_employees_q_obj(): 
	query = Q(department=1) | Q(job_title='Dev') 
	filtered_employees = Employee.objects.filter(query) 
	
	for employee in filtered_employees: 
		print(f"{employee.first_name} {employee.last_name}")


def filter_products(): 
	query = Q(is_available=True) & Q(price__gt=3.00) 
	products = Product.objects.filter(query).order_by('-price', 'name') 
	result = [] 
	
	for product in products: 
		result.append(f"{product.name}: {product.price}lv.") 
		
	return "\n".join(result)
```

## F object
reference a field's value in a query expression
You can compare and manipulate field values directly in the database query 
Comparing the values of two fields ▪ updating fields with other fields' values

```python
class Employee: 
	salary = models.FloatField(default=1.00) 
	...

def update_salary_f_obj(): 
	Employee.objects.update(salary=F('salary') * 1.1)
```

```python
def above_average_salary():
	employees_above_avg_salary = Employee.objects.annotate(
	avg_department_salary=Avg('department__employees__salary'))
	.filter(salary__gt=F('avg_department_salary'))

def give_discount():
	DISCOUNT = 0.7
	reduction = F('price') * DISCOUNT

	query = Q(is_available=True) & Q(price__gt=3)

	procuts = Product.objects.filter(query).order_by('-price', 'name')
	products.update(price=reduction)

	result = []
	for product in Product.objects.available_products().order_by('-price', 'name'):
		result.append(f'{product.name}: {product.price}lv.')

	"\n".join(result)
```


## `python manage.py inspect_db`

generates Django model code by introspecting an existing database.

### `python manage.py inspectdb > models.py`

