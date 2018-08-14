# Setup the backend:

Create a database and user. Copy and edit the `.env` file and put the db details there:

```
$ cd bad-puns-tracker-server
$ cp .env.dist .env
```

Install the project, run the migrations and run the server:

```
$ composer install
$ php bin/console doctrine:migrations:migrate
$ php -S 127.0.0.1:8000 -t public
```

Loading [127.0.0.1:8000](127.0.0.1:8000) now should show the default Symfony 4 page, and [127.0.0.1:8000/movies](127.0.0.1:8000/movies) should show you a 'Not authorized' message. NOTE: if using a virtual machine and NAT, you might need to run the server as ` php -S 0.0.0.0:8000 -t public` instead.