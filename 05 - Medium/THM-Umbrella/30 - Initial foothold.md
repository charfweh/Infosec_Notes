as claire-r
![[Pasted image 20240121084823.png]]

# app.js
```js
app.post('/time', function(request, response) {

            if (request.session.loggedin && request.session.username) {

                let timeCalc = parseInt(eval(request.body.time));
                let time = isNaN(timeCalc) ? 0 : timeCalc;
                let username = request.session.username;

                connection.query("UPDATE users SET time = time + ? WHERE user = ?", [time, username], function(error, results, fields) {
                    if (error) {
                        log(error, "error")
                    };

                    log(`${username} added ${time} minutes.`, "info")
                    response.redirect('/');
                });
                } else {
        response.redirect('/');;
    }

});
```

unsanitized eval

payload
![[Pasted image 20240121091254.png]]

docker container
![[Pasted image 20240121091313.png]]
we copy /bin/bash to /logs
![[Pasted image 20240121091357.png]]

setuid on copied /bin/bash
![[Pasted image 20240121091421.png]]
and we get root
![[Pasted image 20240121091454.png]]