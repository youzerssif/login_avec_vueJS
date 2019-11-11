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
### fichier html (vuejs)
```html
<div id="test">
      <form action="/static/index2.html" method="post">
        <div class="form-group has-feedback">
          <input v-model="username" type="email" class="form-control" placeholder="Username">
          <span class="glyphicon glyphicon-user form-control-feedback"></span>
        </div>
        <div class="form-group has-feedback">
          <input v-model="password" type="password" class="form-control" placeholder="Password">
          <span class="glyphicon glyphicon-lock form-control-feedback"></span>
        </div>
        <div class="row">
          
          <!-- /.col -->```
          <div class="col-xs-4">
            <div class="row">
              
            </div>
            <button v-on:click="login" type="button" class="btn btn-primary btn-block btn-flat">Sign In</button>
            <div v-if="loader" class="lds-ring"><div></div><div></div><div></div><div></div></div>
            
          </div>
          <!-- /.col -->
        </div>
      </form>
    </div> 
```

```javascript
<script>
  // Block Vue JS 
  new Vue({
        el: '#test',
        data: {
          username : '',
          password : '',
          loader : false,
          isSuccess : false,
          error : false,
        
        },
        delimiters : [ "${", "}"],
        mounted(){
            
        },
        methods: {
          login:function(){
              axios.defaults.xsrfCookieName = 'csrftoken'
              axios.defaults.xsrfHeaderName = 'X-CSRFToken'
              axios.post('http://127.0.0.1:8000/post', {
                  username: '' + this.username,
                  password: '' + this.password,
                  }).then(response => {
                      console.log(response) 
                      if(response.data.success){
                          this.isSuccess=true
                          this.error=false
                          window.location.replace("profile");
                      }
                      else{
                          this.error=true
                          this.isSuccess=false
                          window.location.replace("login");
                      }
                      
                      console.log("success variable"+this.isSuccess)
                      // console.log("success variable"+this.error)
                  })
                  .catch((err) => {
                      console.log(err);
                      
                  })
          },
        }
    });

   
</script>
```
