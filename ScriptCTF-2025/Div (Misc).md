> I love division
> 
> Attachments
> 
> chall.py
> 
> qui contient :
```
import os
import decimal
decimal.getcontext().prec = 50

secret = int(os.urandom(16).hex(),16)
num = input('Enter a number: ')

if 'e' in num.lower():
   print("Nice try...")
   exit(0)

if len(num) >= 10:
   print('Number too long...')
   exit(0)

fl_num = decimal.Decimal(num)
div = secret / fl_num

if div == 0:
   print(open('flag.txt').read().strip())
else:
   print('Try again...')
```
> and
> Instance Info (launch an instance)

Le programme propose de deviner un nombre (num) tel que la division du nombre secret aléatoire par votre nombre donne exactement zéro :
le nombre secret est un entier aléatoire de 16 octets
On doit entrer un nombre et dont la longueur est limitée à 9 chiffres.
La division est effectuée en utilisant le module decimal avec une haute précision.
Le flag s'affiche seulement si secret / num == 0

Tests like 0, 0.0, 1, 0.000000001, 000000001, 999999999, secret etc. donnent “Try again…” or exit or “Nice try…” since they don’t respect the rules.

So now if we think mathematically…

With “inf”... 

<details>
<summary>Flag</summary>

`scriptCTF{70_1nf1n17y_4nd_b3y0nd_5665f3de701e}`

</details>
