### Overview
This is a demo Django app with postgres database connection designed to be easily deployed to cloud.gov

# Local setup
### Set up virtualenv, install requirements
```
pyenv virtualenv 3.11 django-demo
pyenv activate django-demo
pip install -r requirements.txt
```

### Set up postgres
```
brew install postgresql@15
```
https://formulae.brew.sh/formula/postgresql@15
...or use your installation method of choice

### Set env var
```
export DATABASE_URL=postgresql://:@/postgres
```

### Run migrations
```
python manage.py migrate
```

### Make a super user locally
```
python manage.py createsuperuser
```


### Run the site
```
gunicorn --bind 0.0.0.0:8080 mysite.wsgi -w 9 -t 200
```

### Test app locally
http://0.0.0.0:8080/simpleapp

### Test logging in with superuser

http://0.0.0.0:8080/admin
go to
http://0.0.0.0:8080/simpleapp

# cloud.gov

Deploy the app with `cf push -f manifest-dev.yml`

### Create database service (if it's not already there)
```
cf create-service aws-rds small-psql-redundant login-demo-rds
```

### Deploy to cloud.gov
```
cf login --sso
```
target fec-fecfile, dev space
```
cf push -f manifest-dev.yml
```

### Test on cloud.gov
https://django-demo.app.cloud.gov/simpleapp

### Create super user
```
cf ssh django-demo
/tmp/lifecycle/shell
./manage.py createsuperuser
```

### Log in, test database connection
http://0.0.0.0:8080/admin
go to
http://0.0.0.0:8080/simpleapp

### Log in, test database connection
```
cf connect-to-service --no-client django-demo django-demo-rds
```
in a new tab:
```
pgcli --dbname <> --host <> --port <> --username <>
```
