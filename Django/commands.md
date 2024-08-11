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

1. Access the shell ```python manage.py shell``` 

2. Access the model and carry out save

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