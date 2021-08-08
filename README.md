# Django-Note

# How to reset django admin password?
```
# You may try through console:

$ python manage.py shell

# then use following script in shell

from django.contrib.auth.models import User
User.objects.filter(is_superuser=True)

# will list you all super users on the system. if you recognize yur username from the list:

usr = User.objects.get(username='your username')
usr.set_password('raw password')
usr.save()
# and you set a new password (:
```
