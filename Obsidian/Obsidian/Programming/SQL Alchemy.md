
1. `pip install sqlalchemy`
2. `pip install psycopg2`

```python
# main.py 
from sqlalchemy import create_engine 
from sqlalchemy.orm import declarative_base

DATABASE_URL = 'postgresql+psycopg2://your_username:your_password@your_host /your_database' 

engine = create_engine(DATABASE_URL)


Base = declarative_base() 

class User(Base): 
	__tablename__ = 'users' 
	id = Column(Integer, primary_key=True) 
	username = Column(String) email = Column(String) 

# Create tables in the database (no migrations management) 
Base.metadata.create_all(engine)
```

### Migrations => Alembic

### Create

```python
with Session() as session: 
	new_user = User(username='john_doe', email='john@example.com') 
	session.add(new_user) 
	session.commit()
```

### Read 

```python
with Session() as session: 
	users = session.query(User).all() 
	
	for user in users: 
		print(user.username, user.email)
```

### Update 

```python
with Session() as session:  
    user_to_update = session.query(User).filter_by(username='PowerCell46').first()  
    if user_to_update:  
        user_to_update.email = 'makotsevo.fan@gmail.com'  
        session.commit()  
        print(f'User with username {user_to_update.username} has been updated!')  
    else:  
        print('No such user!')
```

### Delete

```python
with Session() as session:  
    user_to_delete = session.query(User).filter_by(username='PowerCell46').first() 
  
    if user_to_delete:  
        session.delete(user_to_delete)  
        session.commit()  
        print(f'User with username {user_to_delete.username} has been deleted!')  
    else:  
        print('No such user!')
```

### Transactions

```python
from main import Session  
from models import User  
  
session = Session()  
  
try:  
    session.begin()  
  
    session.query(User).delete()  
  
    session.commit()  
except Exception as e:  
    session.rollback()  
    print(f'Erorr: {e}')  
finally:  
    session.close()
```

### Relationships

```python
class Order(Base):  
    __tablename__ = 'orders'  
    id = Column(Integer, primary_key=True)  
    is_completed = Column(Boolean, default=False)  
    user_id = Column(Integer, ForeignKey('users.id'))  
    user = relationship('User')

def populate_order_table(): 
	with Session() as session: 
		session.add_all((Order(user_id=1), Order(user_id=2))) 
		session.commit()
```

### Database Pooling
used to efficiently manage and reuse database connections

```python
engine = create_engine(DATABASE_URL, pool_size=10, max_overflow=20)
```

## Raw SQL Queries

```python
from sqlalchemy import create_engine  
  
DATABASE_URL = 'postgresql+psycopg2://postgres:PowerCell46@localhost/sqlalchemy'  
  
  
engine = create_engine(DATABASE_URL)  
  
query = """  
SELECT   
    *  
FROM   
    users  
WHERE   
    id = 2;  
"""  
  
users_table = (pd.read_sql(query, engine))
```
