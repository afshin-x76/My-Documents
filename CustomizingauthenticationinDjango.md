# Customizing authentication in Django

You can give your models [custom permissions]() that can be checked through Django’s authorization system.

You can [extend]() the default User model, or [substitute]() a completely customized model.

## Other authentication sources

you can plug in other authentication source, You can override Django’s default database-based scheme, or you can use the default system in tandem with other systems.

See the [authentication backend]() reference for information on the authentication backends included with Django.

## Specifying authentication backends

 Django maintains a list of “authentication backends” that it checks for authentication. When somebody calls django.contrib.auth.authenticate() Django tries authenticating across all of its authentication backends.
 
list of authentication backends is in AUTHENTICATION_BACKENDS setting.

By default is set to:
```
['django.contrib.auth.backends.ModelBackend']
```

checks the Django users database and queries the built-in permissions.

The order of AUTHENTICATION_BACKENDS matters, so if the same username and password is valid in multiple backends, Django will stop processing at the first positive match.

if you change AUTHENTICATION_BACKENDS, you’ll need to clear out session data.
```
Session.objects.all().delete()
```

## [Writing an authentication backend](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#writing-an-authentication-backend)

authentication backend is a class that implements two required methods: **get_user(user_id)** and **authenticate(request, `**`credentials)**.

The get_user method takes a PrimaryKey.

## [Handling authorization in custom backends](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#handling-authorization-in-custom-backends)

Custom auth backends can provide their own permissions.

The user model and its manager will delegate permission lookup functions **(get_user_permissions()**, **get_group_permissions()**, **get_all_permissions()**, **has_perm()**, **has_module_perms()**, and **with_perm()**) to any authentication backend that implements these functions.

The permissions given to the user will be the superset of all permissions returned by all backends.

### [Handling object permissions](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#handling-object-permissions)

django checks for object permissions will always return False or an empty list. authentication backend receive the parameters obj and user_obj for each authorization method and can return the object level permission as appropriate.


## [Custom permissions](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#custom-permissions)

To create custom permissions for a given model object, use the permissions model Meta attribute.

## [Extending the existing User model](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#extending-the-existing-user-model)

We know This well :)

## [Substituting a custom User model](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#substituting-a-custom-user-model)

Django allows you to override the default user model by providing AUTH_USER_MODEL setting that references a custom model:

`AUTH_USER_MODEL = 'myapp.MyUser'`

### [Using a custom user model when starting a project](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project)

You shouldn't migrate in your project until you specified your custom user model with `makemigrations`.

### [Reusable apps and AUTH_USER_MODEL](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#reusable-apps-and-auth-user-model)

If you need to store per user information in your app, use a ForeignKey or OneToOneField to settings.AUTH_USER_MODEL as described below.

### [Referencing the User model](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#referencing-the-user-model)

If you reference User directly, your code will not work in projects where the AUTH_USER_MODEL setting has been changed to a different user model.

Instead of referring to User directly, you should reference the user model using django.contrib.auth.get_user_model(). This method will return the currently active user model.

### [Specifying a custom user model](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#specifying-a-custom-user-model)

The easiest way is to inherit from AbstractBaseUser. You must then provide some key implementation details:

- USERNAME_FIELD
- EMAIL_FIELD
- REQUIRED_FIELDS
- is_active
- get_full_name()
- get_short_name()

The following attributes and methods are available on any subclass of AbstractBaseUser:

- get_username()
- clean()
- classmethod get_email_field_name()
- classmethod normalize_username(username)
- is_authenticated
- is_anonymous
- set_password(raw_password)
- check_password(raw_password)
- set_unusable_password()
- has_usable_password()
- get_session_auth_hash()

### [Writing a manager for a custom user model](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#writing-a-manager-for-a-custom-user-model)

You should also define a custom manager for your user model. you’ll need to define a custom manager that extends BaseUserManager providing two additional methods:

- create_user(username_field, password=None, `**`other_fields)
- create_superuser(username_field, password=None, `**`other_fields)

BaseUserManager provides the following utility methods:

- classmethod normalize_email(email)
- get_by_natural_key(username)
- make_random_password(length=10, allowed_chars='abcdefghjkmnpqrstuvwxyzABCDEFGHJKLMNPQRSTUVWXYZ23456789')

### [Extending Django’s default User](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#extending-django-s-default-user)

If you’re entirely happy with Django’s User model, but you want to add some additional profile information, you could subclass
**django.contrib.auth.models.AbstractUser** and add your custom profile fields

### [Custom users and the built-in auth forms](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#custom-users-and-the-built-in-auth-forms)

The following forms are compatible with any subclass of AbstractBaseUser:

- AuthenticationForm: Uses the username field specified by USERNAME_FIELD.
- SetPasswordForm
- PasswordChangeForm
- AdminPasswordChangeForm
- PasswordResetForm: Assumes that the user model has a field that stores the user’s email address with the name returned by **get_email_field_name()** that can be used to identify the user and a boolean field named **is_active** to prevent password resets for inactive users.

Finally, the following forms are tied to User and need to be rewritten or extended to work with a custom user model:

- UserCreationForm
- UserChangeForm

### [Custom users and permissions](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#custom-users-and-permissions)

Django provides PermissionsMixin. This is an abstract model you can inherit from it.

PermissionsMixin provides the following methods and attributes:

- is_superuser
- get_user_permissions(obj=None)
- get_group_permissions(obj=None)
- get_all_permissions(obj=None)
- has_perm(perm, obj=None)
- has_perms(perm_list, obj=None)
- has_module_perms(package_name)


### References

- [Full example for Creating Custom User Model. testdriven.io](https://testdriven.io/blog/django-custom-user-model/)
- [Customizing authentication in django](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/)
