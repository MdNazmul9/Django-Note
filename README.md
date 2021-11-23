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
   # Custom Permissions
   step 1:
   ```
   # models.py
   class Book(models.Model):
      id = models.UUIDField(
      primary_key=True,
      default=uuid.uuid4,
      editable=False)
      title = models.CharField(max_length=200)
      author = models.CharField(max_length=200)
      price = models.DecimalField(max_digits=6, decimal_places=2)
      cover = models.ImageField(upload_to='covers/', blank=True)
      
        class Meta: # new
          permissions = [
          ('special_status', 'Can read all books'),
          ]

   ```
   step 2:
   ```
   from django.contrib.auth.mixins import (
      LoginRequiredMixin,
      PermissionRequiredMixin # new
      )
    from django.views.generic import ListView, DetailView
    from .models import Book
    
    
    class BookDetailView(LoginRequiredMixin,PermissionRequiredMixin,DetailView):
      model = Book
      context_object_name = 'book'
      template_name = 'books/book_detail.html'
      login_url = 'account_login'
      permission_required = 'books.special_status' # new
    
   ```
