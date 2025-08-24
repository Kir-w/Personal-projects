> Just a simple modulo challenge
> 
> Attachments
> chall.py

```
#!/usr/local/bin/python3
import os
secret = int(os.urandom(32).hex(),16)
print("Welcome to Mod!")
num=int(input("Provide a number: "))
print(num % secret)
guess = int(input("Guess: "))
if guess==secret:
    print(open('flag.txt').read())
else:
    print("Incorrect!")
```
> Launching Instance 

[explications en cours d'écriture]




```
import socket

HOST = 'play.scriptsorcerers.xyz'
PORT = 10466

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect((HOST, PORT))
    while True:
        data = s.recv(1024)
        if not data:
            break
        print(data.decode(), end='')
        user_input = input()
        s.sendall((user_input + '\n').encode())
```
Cela donne : 

> Welcome to Mod!
> Provide a number: -1
> 18747144985059669630001379704117149010572936734387761931477202718601709440726
> Guess: 18747144985059669630001379704117149010572936734387761931477202718601709440727

<details>
<summary>Flag</summary>

`scriptCTF{-1_f0r_7h3_w1n_4a3f7db1_adbb20d6e757}`

</details>
