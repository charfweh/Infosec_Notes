# drive.htb
upon visiting the site we get the following, so its basically a website about storing files like a google drive

![[Pasted image 20231129022626.png]]
upon registering we get the upload file and dashboard button, lets check it out

after registering
![[Pasted image 20231129062519.png]]

lets visit dashboard

dashboard /home
the dashboard brings us to what files are there and its information
![[Pasted image 20231129062556.png]]

files
![[Pasted image 20231129062623.png]]
## upload file
![[Pasted image 20231130083156.png]]


Reserve files 
It lets you reserve files under your name, with this you can essentially edit the content of unreserved files and view it. this will help later

Unreserve files
it lets you unreserve files, meaning its out there for anyone to view it and own it under their name


view file
![[Pasted image 20231129062658.png]]
we see that our file number is `112` lets try other files
## idor fuzzing
lets fuzz file
![[Pasted image 20231129064739.png]]
99 returns unauth, so we know the file exists

![[Pasted image 20231129064819.png]]
999 returns server error so we can conclude the file doesnt exist
python script
```python
f = open("numbers.txt",'w')
for i in range(0,999):
    f.write(str(i)+"\n")
f.close()
```

lets create a wordlist 0-999 and send it to wfuzz to check for other files
cmd:
```bash
wfuzz -u http://drive.htb/FUZZ/getFileDetail/ -w numbers.txt --follow --hw 11 -p localhost:8080 -b "sessionid=wxi6mpbfs1dqhj5b4id8oik2o8fze7po"
-w for wordlist
--follow follows redirect
-p ran it through burp
-b our sessionid
```


o/p:
![[Pasted image 20231129064957.png]]
we get some files, out of which 100 is the admin file on homepage and 112,115 is our files. lets see if we can reserve the other files under our name.



# exploitation

so the vulnerability lies in the `reserve` functionality, where you can pass in any file and it will reserve the file for you meaning only you can read it so we can take advantage of this 

![[Pasted image 20231129065501.png]]
after intercepting the `Reserve` request, we change the file id to `79` and send the request to receive the following request. it appears to be password for martin
![[Pasted image 20231129065552.png]]
The funny thing is the above `Reserve` is the only attack vector vulnerable, when you go to `Files > ReserveFiles` it sends a POST request to `/blockFile` in contrast to when you go from `Dashboard > Reserve` files it sends a GET request to `/[id]/block` so those two are different functionality keep in mind. 

After retrieving the other files we find they're also taking about databases file.
![[Pasted image 20231201080741.png]]
again this will help us later, therefore you must have your enumeration game tight!


[[10 - Hard/drive/initial foothold|initial foothold]]

# source code analysis
## file object structure
```python
class File(models.Model):
    name = models.CharField(max_length=50 , unique=True)
    file = models.FileField(upload_to=get_upload_path )
    owner  = models.ForeignKey(
        CustomUser,
        on_delete=models.CASCADE,
    )

    block  = models.ForeignKey(CustomUser, null=True,on_delete=models.SET_NULL  ,blank=True , related_name='block')
    

    content = models.TextField(default="none")
    createdDate  = models.DateTimeField(auto_now_add=True)
    groups = models.ManyToManyField(G) 

    def __str__(self):
        return self.name 
```


## block_one_file
```python
def block_one_file(request, id):
    user = request.user
    file = get_object_or_404(File, id = id)
    file.block = user
    file.save()
    groups = file.groups.all()
    userGroups = user.g_set.all()
    level = 0 
    for group in groups:
        if userGroups.filter(id = group.id ).exists(): #l.g_set.all().filter(id = f.id).exists()
            level = 1 
            break
    #log action
    log_record = {'user': request.user.username  , 'method' : 'blockFile' , 'file_name' : file.name , 'time_stamp': str(datetime.datetime.now())}
    log(log_record, 'files-log.json', 2)
    file_obj = open((SITE_ROOT+'/media/'+ str(file.file) ))
    data = file_obj.read()
    file.content = data.splitlines()
    return render(request , 'getFileDetail.html' , {'file': file , 'user':user  , 'level':level})

```
as we can see theres no check if we own the file or not and it directly blocks the files then log it 
so the functionality of block_one_f
![[Pasted image 20231130022329.png]]
when you hover over Reserver button you see `http://drive.htb/113/block/`
113 being the id of the file and after fuzzing for available files we get the idea on how to exploit it


## blockFile
```python
def blockFile(request):
    user = request.user 
    if request.method == 'POST':
        files = File.objects.all().filter(name__in = request.POST.getlist('files'))
        user_Bloackable_File = File.objects.all().filter(groups__in = user.g_set.all()).distinct()
        try:
            with transaction.atomic():
                for file in files:
                    if file.block is None and file in user_Bloackable_File:
                        file.block = user
                        file.save()
                        #log action
                        log_record = {'user': request.user.username  , 'method' : 'blockFile' , 'file_name' : file.name , 'time_stamp': str(datetime.datetime.now())}
                        log(log_record, 'files-log.json', 2)
                    else:
                        1/0
        except:
            return JsonResponse({'status':'fail','message':'operation failed'})     
        return JsonResponse({'status':'success','message':'files Reserved successfully'})
    
    mygroups = user.g_set.all()
    i = 0
    result = G.objects.none()
    while i< len(mygroups):
        result = list(chain(mygroups[i].file_set.all() , result))
        i = i+1 
    result = set(result)
    return render( request, 'blockFile.html' , {'files': result})

```

here we see that theres a check `user_Bloackable_File` which retrieves the file thats owned by us to block it
and theres also another conditional `if file.block is None and file in user_Bloackable_File` so the first check is true the file is not blocked but the second check is false because we dont own the file and therefore it gets out of the conditional if and directly sends the response that operation is successfull