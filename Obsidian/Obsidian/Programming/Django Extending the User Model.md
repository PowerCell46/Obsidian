
### Use **AUTH_USER_MODEL** in your project settings to specify your custom User model
ensures that all references to the User model within Django and third-party apps point to your extended model

## 1. One-To-One Relationship (Easiest to implement)

```python
from django.contrib.auth import get_user_model 
from django.db import models 

UserModel = get_user_model() 


class Profile(models.Model): 
	user = models.OneToOneField(UserModel, on_delete=models.CASCADE, primary_key=True) 
	date_of_birth = models.DateField(null=True, blank=True) 
	profile_picture = models.ImageField(upload_to='profile_pics/', null=True, blank=True) 
	# Add any additional fields related to the user profile 
	
	def __str__(self): 
		return self.user.username
```
#### Accessing the Data

```python
def example_view(request): 
	current_user = request.user # Access the related profile 
	current_user_profile = current_user.profile 
	...
```

# 2. Inheriting from AbstractUser
#### This approach allows you to add extra fields to the user model without creating a separate table in the database.
#### Use When: You need more control over the user model compared to the built-in User model

You need to update the AUTH_USER_MODEL property in your project's settings.py file to point to your new model

Remember to update the AUTH_USER_MODEL setting in your project's settings.py file before running migrations

```python
from django.contrib.auth import models as auth_models

class CustomUser(auth_models.AbstractUser): 
	# Add extra fields
	email = models.EmailField(unique=True) 
	date_of_birth = models.DateField(null=True, blank=True)
	profile_picture = models.ImageField(upload_to='profile_pics/', null=True, blank=True) 

	USERNAME_FIELD = 'email'
	...
```

```python
from django.contrib.auth import models as auth_models 
from django.db import models 


class AppUserManager(auth_models.BaseUserManager): 
	def create_user(self, email, password=None, **extra_fields): 
		if not email: 
			raise ValueError('The Email field must be set!') 
		email = self.normalize_email(email) 
		user = self.model(email=email, **extra_fields) 
		user.set_password(password) 
		user.save(using=self._db) 
		return user 
	
	def create_superuser(self, email, password=None, **extra_fields):
		extra_fields.setdefault('is_staff', True)
		extra_fields.setdefault('is_superuser', True) 
		return self.create_user(email, password, **extra_fields)
```

# 3. Extending the AbstractBaseUser

#### When subclassing `AbstractBaseUser`, you need to define the fields and methods that are necessary for your application. This offers maximum flexibility but requires more implementation effort compared to `AbstractUser`.
#### A way to completely replace Django's existing user model with a custom user model of your design.

Remember to update the AUTH_USER_MODEL setting in your project's settings.py file before running migrations

```python
class AppUser(auth_models.AbstractBaseUser, auth_models.PermissionsMixin): 
	email = models.EmailField(null=False, blank=False, unique=True) 
	# You can add additional fields, related to user authentication 
	... 
	is_active = models.BooleanField(default=True) 
	is_staff = models.BooleanField(default=False) 
	
	objects = AppUserManager() 
	
	USERNAME_FIELD = 'email'
	REQUIRED_FIELDS = [] 
	
	def __str__(self): 
		return self.email
```