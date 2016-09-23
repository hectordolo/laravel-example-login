# Laravel 5.3 Login by username

This is a basic setup of Laravel 5.3 using the "username" to log into a laravel system after running the:

```
	php artisan make:auth
```

## Edited Files

### Controllers

* [Login Controller] (#login-controller)
* [Register Controller] (#register-controller)

### Views


### Login Controller

The Login Controller handles the logging in of users into the system.

The file is located:
```
app>Http>Controllers>Auth>LoginController.php
```

Add this block of code into the file:

```php

	/**
     * Override the username method used to validate login
     *
     * @return string
     */
    public function username()
    {
        return 'username';
    }

```

### Register Controller

The Register Controller handles the creation of new users.

The file is located:
```
app>Http>Controllers>Auth>RegisterController.php
```

Add this line of code into the validator function:

```
	'username' => 'required|max:255|unique:users',
```

To look like this:

```php
	protected function validator(array $data)
    {
        return Validator::make($data, [
            'name' => 'required|max:255',
            'username' => 'required|max:255|unique:users',
            'email' => 'required|email|max:255|unique:users',
            'password' => 'required|min:6|confirmed',
        ]);
    }
```

And also add this line of code into the create function:

```
	'username' => $data['username'],
```

To look like this:

```php
	protected function create(array $data)
    {
        return User::create([
            'name' => $data['name'],
            'username' => $data['username'],
            'email' => $data['email'],
            'password' => bcrypt($data['password']),
        ]);
    }
```


