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

# When username is known as username="admin"
$ ``` python manage.py changepassword admin```

# Create View reverse to DetailView
```
  #step1: models.py
  
  def get_absolute_url(self,*args,**kwargs):
        return reverse('image-view',kwargs={'pk': self.pk})
        
   #step2: models.py
        
  class ImageCreate(CreateView):
    model = Image
    template_name='image_create.html'
    fields = ['title','image']
    def get_success_url(self):
        return reverse('image-view', kwargs={'pk' : self.object.pk})

    def get_form_kwargs(self, *args, **kwargs):
        kwargs = super(ImageCreate, self).get_form_kwargs(
            *args, **kwargs)
        return kwargs
        
   ```
   
