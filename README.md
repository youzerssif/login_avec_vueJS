# login_avec_vueJS

### views.py
```python
from django.shortcuts import render, redirect
from django.http import JsonResponse
import json
from appmail.models import Utilisateur

from django.contrib.auth import authenticate, login, logout
from django.contrib.auth.decorators import login_required



def deconnexion(request):
    logout(request)
    return redirect('login')

def islogin(request,):
    
    postdata = json.loads(request.body.decode('utf-8'))
    
    # name = postdata['name']

    username = postdata['username']
    password = postdata['password']
    isSuccess=False

    user = authenticate(username=username, password=password)
    
    if user is not None and user.is_active:
        print("user is login")
        isSuccess = True
        login(request, user)
        datas = {
        'success':True,
        'username':username
    }
        return JsonResponse(datas,safe=False) # page si connect
        
    else:
        data = {
        'success':False,
        }
        return JsonResponse(data,safe=False)
    
    
    
    return JsonResponse(datas, safe=False)

```
