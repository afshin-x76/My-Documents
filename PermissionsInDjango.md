# Permissions In Django

#### auto creating custom Permissions 

```
class CustomModel(mdoels.Model):
	...
    class Meta:
    	...
    	permissions = [
        	('codename', 'explain')
        ]
        ...
```

#### creating custom permissions manually

```
from django.contrib.auth.models import Permission, User
from django.contrib.contenttypes.models import ContentType
form app.models import mymodel

content_type = ContentType.objects.get_for_model(mymodel)
permission = Permission.objects.create(
	codename='<codename>',
    name='<explain>'
	content_type=content_type,
	)
```

#### adding that permission to users:

```
from testapp.models import MyUser
from django.contrib.auth.models import Permission

user = MyUser.objects.get(...)
permission = Permission.objects.get(codename=<codename>)

user.user_permissions.add(permission)

### if you want to test it right after this
### The ModelBackend caches permissions on the user object after the first time ### they  need to be fetched for a permissions check. 
### you should re-fetch user from database.

user.has_perm('<appname>.<codename>')
False
user = MyUser.objects.get(...)
user.has_perm('<appname>.<codename>')
True

```

### enforcing permissions

##### simplest way to restrict an action:
```
from django.core.exceptions import PermissionDenied

def users_list_view(request):
    if not request.user.has_perm('<appname>.<codename>'):
        raise PermissionDenied()
```
##### make it easier to enforce permissions in functional views
```
from django.contrib.auth.decorators import permission_required

@permission_required('<appname>.<codename>')
def users_list_view(request):
    pass
```

##### make it easier to enforce permissions in class base views

```
from django.contrib.auth.mixins import PermissionRequiredMixin


class MyView(PermissionRequiredMixin, View):
    permission_required = '<appname>.<codename>'
    # Or multiple of permissions:
    permission_required = ('<appname>.<codename>', '<appname>.<codename>')
```

##### enforce permissions in templates

```
{% if perms.<appname>.<codename> %}
	...
{% endif %}
```
> perms is special template variable


#### set user permissions when user is created

you need to use [signals](https://docs.djangoproject.com/en/3.2/topics/signals/)

```
from django.dispatch import receiver
from django.db.models.signals import post_save
from django.contrib.auth.models import User, Permission

@receiver(signal=post_save, sender=User)
def add_permission(sender, instance=None, created=False, **kwargs):
    if created:
        permission = Permission.objects.get(codename='<codename>')
        instance.user_permissions.add(permission)

```
