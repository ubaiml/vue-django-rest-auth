# server development

``` bash
# install dependencies
pipenv install --dev
pipenv shell

# setup development database
./manage.py makemigrations api --noinput
./manage.py migrate --noinput

# load data
./manage.py createsuperuser --username=root --email=root@example.com --noinput
./manage.py create_fixtures

# run development server
./manage.py runserver --settings=server.settings
```

**Note:** You should install [pipenv](https://docs.pipenv.org/) before installing any python dependencies.


Starting The Worker Process
Open a new terminal tab, and run the following command:

celery -A mysite worker -l info
Change mysite to the name of your project.

Starting the worker process¶
In a production environment you’ll want to run the worker in the background as a daemon - see Daemonization - but for testing and development it is useful to be able to start a worker instance by using the celery worker manage command, much as you’d use Django’s manage.py runserver:

$ celery -A proj worker -l info
For a complete listing of the command-line options available, use the help command:

$ celery help
