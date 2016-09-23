# Laravel 5.3 Login by username

This is a basic setup of Laravel 5.3 using the "username" to log into a laravel system after running the:

```
php artisan make:auth
```


## Edit Files

Before editing the controllers and views we will edit the user migration file first the will create the structure of of our user table located at:

```
database>migrations>"DATE"_create_users_table.php
```

We will add this line into the up function:

```php
$table->string('username')->unique();
```

To look like this:

```php
/**
 * Run the migrations.
 *
 * @return void
 */
public function up()
{
	Schema::create('users', function (Blueprint $table) {
		$table->increments('id');
		$table->string('name');
		$table->string('email')->unique();
		$table->string('username')->unique();
		$table->string('password');
		$table->rememberToken();
		$table->timestamps();
	});
}
```

Now we run:

```
php artisan migrate
```

To create a table for our user.

### Controllers

* [Login Controller] (#login-controller)
* [Register Controller] (#register-controller)

### Views

* [Login Blade] (#login-blade)
* [Register Blade] (#register-blade)

---

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

---

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

---

### Login Blade

The Login Blade is the html page that shows form where the user will log into the system it is located at:

```
resources>views>auth>login.blade.php
```

Find the code that look like this:

```php
<div class="form-group{{ $errors->has('email') ? ' has-error' : '' }}">
	<label for="email" class="col-md-4 control-label">E-Mail Address</label>

	<div class="col-md-6">
		<input id="email" type="email" class="form-control" name="email" value="{{ old('email') }}" required>

		@if ($errors->has('email'))
			<span class="help-block">
				<strong>{{ $errors->first('email') }}</strong>
			</span>
		@endif
	</div>
</div>
```

And replace it to this:

```php
<div class="form-group{{ $errors->has('username') ? ' has-error' : '' }}">
	<label for="name" class="col-md-4 control-label">Username</label>

	<div class="col-md-6">
		<input id="username" type="text" class="form-control" name="username" value="{{ old('username') }}" required autofocus>

		@if ($errors->has('username'))
			<span class="help-block">
				<strong>{{ $errors->first('username') }}</strong>
			</span>
		@endif
	</div>
</div>
```

And it will look like this:

<img src="https://hectordolo.github.io/img/Login.png" alt="Login Page">

---

### Register Blade

This is the where a new user can register it is located at 

```
resources>views>auth>register.blade.php
```

Add this block of code:

```php
<div class="form-group{{ $errors->has('username') ? ' has-error' : '' }}">
	<label for="username" class="col-md-4 control-label">Username</label>

	<div class="col-md-6">
		<input id="username" type="text" class="form-control" name="username" value="{{ old('username') }}" required autofocus>

		@if ($errors->has('username'))
			<span class="help-block">
				<strong>{{ $errors->first('username') }}</strong>
			</span>
		@endif
	</div>
</div>
```

Just Below this code:

```php
<div class="form-group{{ $errors->has('name') ? ' has-error' : '' }}">
	<label for="name" class="col-md-4 control-label">Name</label>

	<div class="col-md-6">
		<input id="name" type="text" class="form-control" name="name" value="{{ old('name') }}" required autofocus>

		@if ($errors->has('name'))
			<span class="help-block">
				<strong>{{ $errors->first('name') }}</strong>
			</span>
		@endif
	</div>
</div>
```

To add a username field into the form.

It will then look like this:

<img src="https://hectordolo.github.io/img/Register.png" alt="Register Page">
