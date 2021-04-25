# Use Email In Django

add this variables in settings:

```
EMAIL_BACKEND ='django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_USE_TLS = True
EMAIL_PORT = 587
EMAIL_HOST_USER = 'your email addres'
EMAIL_HOST_PASSWORD = 'your email password'
```

use sendemail in for sending emails:

```
from djanog.core.mail import sendmail

send_mail(
    'Subject here',
    'Here is the message.',
    'from@example.com',
    ['to@example.com'],
    fail_silently=False,
)
```

[for more information go here](https://docs.djangoproject.com/en/3.2/topics/email/)