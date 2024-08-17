# Django Import CLI Command

## Installation

1. Install Django ```python -m pip install Django```


## Set-up New Project

1. Create new Project ```django-admin startproject <name>```


## Operations


1. Run Web Server ```python manage.py runserver```

2. Create App(Module) in Project ```python manage.py startapp <name>```


## Database

1. Creates migration files from models ```python manage.py makemigrations```

2. Migrate to DB ```python manage.py migrate```


## Loading Data into DB

1. Create the ```fixtures``` folder into the app folder.

2. Create a json file within fixtures folder. It should look like this

   
    ```json
     <!-- books.json -->
    [
    {
        "model": "book_store.book",
        "pk":1,
        "fields":{
            "title":"Django : The practical guide",
            "rating":10,
            "author":"Max Schwarz",
            "is_bestselling":true,
            "slug":"django-the-practical-guide"
        }
    },
    ]
    ```

3. Command ```django-admin loaddata <fixture label> ``` example ```django-admin loaddata books```

## Django Shell & Operations

Access the shell ```python manage.py shell``` 


### Access the model and carry out save

 ```py
from book_store.models import Book
#from <app_name>.models import <model_name>
Book.objects.get(pk=1).save()
#<model_name>.objects.get(pk=<primary_key_num>)
Book.objects.get(pk=2).save()
Book.objects.get(pk=3).save()
Book.objects.get(pk=4).save()
Book.objects.get(pk=5).save()
```

### Create model and attach to another model

```py
#import
from book_store.models import Book,Author

#create 'master' model
jkrowling = Book(first_name="J.K" , last_name="Rowling")
#save
jkrowling.save()

#create 'slave' model and link to 'master' model
hp1 = Book(title="Harry Potter1", rating=9, author=jkrowling, is_bestselling=True)
hp1.save()

# Query new results for checking

Author.objects.all()
# <QuerySet [<Author: first_name: Max , last_name:Schwarz>, <Author: first_name: Jose , last_name:Portilla>, <Author: first_name: Taylor , last_name:Otwell>, <Author: first_name: Tim , last_name:Bulchalka>, <Author: first_name: Chad , last_name:Darby>, <Author: first_name: J.K> , last_name:Rowling>]>

Book.objects.all()
# <QuerySet [<Book: Title: Django : The practical guide,  Author: first_name: Max , last_name:Schwarz Bestseller: True>, <Book: Title: Flask : Create API,  Author: first_name: Jose , last_name:Portilla Bestseller: False>, <Book: Title: Laravel : Django, but in PHP,  Author: first_name: Taylor , last_name:Otwell Bestseller: False>, <Book: Title: Java : Java17 Masterclass,  Author: first_name: Tim , last_name:Bulchalka Bestseller: True>, <Book: Title: SpringBoot : Spring Masterclass,  Author: first_name: Chad , last_name:Darby Bestseller: False>, <Book: Title: Harry Potter1,  Author: first_name: J.K> , last_name:Rowling Bestseller: True>]>


```

### Using model relationship to query

```py
#import
from book_store.models import Book,Author
author1 =Book.objects.get(id=1).author
#<Author: first_name: Max , last_name:Schwarz>

Book.objects.get(id=1).author.first_name
#Max
```

### Query using model relationship 2

```py
#import
from book_store.models import Book,Author

#<model1>.objects.filter(<field_relationship>__<model_attribute>=value)
Book.objects.filter(author__last_name="Rowling")
# <QuerySet [<Book: Title: Harry Potter1,  Author: first_name: J.K> , last_name:Rowling Bestseller: True>]>

Book.objects.filter(author__last_name__contains="ling")
# <QuerySet [<Book: Title: Harry Potter1,  Author: first_name: J.K> , last_name:Rowling Bestseller: True>]>

jkr =Author.objects.get(last_name="Rowling")
jkr_book_set=jkr.book_set.all()

# <QuerySet [<Book: Title: Harry Potter1,  Author: first_name: J.K> , last_name:Rowling Bestseller: True>, <Book: Title: Harry Potter2,  Author: first_name: J.K> , last_name:Rowling Bestseller: False>]>

```

### Creating a Related Name in Model for establishing the relationship

Add related_name in the field

```py
author=models.ForeignKey(Author,on_delete=models.CASCADE,null=True,related_name="books")
```

Query in the shell

```py
#import
from book_store.models import Book,Author

Author.objects.get(last_name="Rowling").books.all()
# <QuerySet [<Book: Title: Harry Potter1,  Author: first_name: J.K> , last_name:Rowling Bestseller: True>, <Book: Title: Harry Potter2,  Author: first_name: J.K> , last_name:Rowling Bestseller: False>, <Book: Title: Harry Potter3,  Author: first_name: J.K> , last_name:Rowling Bestseller: True>]>

# chaining model relationship

angela = Author(first_name="Angela",last_name="Yu")
p1 = Book(title="100 days of Python",rating=9,author=angela,is_bestselling=True)
Book.save(p1)
Address.save(street="Random street 6",postal_code="Random postal code 6",city="London",author =angela)
p1.author.address
#<Address: ('Random street 6', 'Random postal code 6', 'London')>


```

## Many To Many Relationship Association

```py
from book_store.models import Book,Author,Address,Country
usa = Country(name="United States of America",code="USA")
usa.save()
springboot = Book.objects.get(title="SpringBoot : Spring Masterclass")
springboot.published_countries.add(usa)
#<QuerySet [<Country: United Kingdom(UK)>, <Country: United States of America(USA)>]>


usa.book_set.all()
#<QuerySet [<Book: Title: SpringBoot : Spring Masterclass,  Author: first_name: Chad , last_name:Darby Bestseller: False>]>
Country.objects.get(code="GER").book_set.all()
# <QuerySet [<Book: Title: Django : The practical guide,  Author: first_name: Max , last_name:Schwarz Bestseller: True>, <Book: Title: Flask : Create API,  Author: first_name: Jose , last_name:Portilla Bestseller: False>, <Book: Title: Java : Java17 Masterclass,  Author: first_name: Tim , last_name:Bulchalka Bestseller: True>]>

```

