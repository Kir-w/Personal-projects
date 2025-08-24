> Un stagiaire un peu trop enthousiaste de la salle d’arcade a proposé un nouveau protocole d’authentification par mot de passe (Password Authentication Key Agreement) pour « sécuriser l’avenir d’internet ». Il s’appelle Marty. C’est à toi qu’incombe la mission de renvoyer ce mec dans sa ligne temporelle d’origine en cassant son protocole. 
>
> Flag format : AMSI{mot de passe} 
>
> Exemple : AMSI{canard}
>
> starter_pake.py
```
import random
import hashlib
from Crypto.Util.number import getPrime, inverse
from math import gcd
from flag import pwd

# Public
n = getPrime(512)
g = 2

def H(pwd):
    return int(hashlib.sha256(pwd.encode()).hexdigest(), 16) % (n - 1)

def invertible_Q(pwd):
    Q = H(pwd)
    while gcd(Q, n - 1) != 1:
        Q = (Q + 1) % (n - 1)
    return Q

def alice_send_X1(Q, a):
    print("[Client ➜ Server] X1")
    X1 = pow(g, a * Q, n)
    return X1

def bob_send_Y1_and_compute_X(Q, b, X1):
    print("[Server ➜ Client] Y1")
    Y1 = pow(g, b * Q, n)
    Qinv = inverse(Q, n - 1)
    X = pow(X1, Qinv, n) 
    return Y1, X

def alice_compute_Y(Y1, Q):
    Qinv = inverse(Q, n - 1)
    Y = pow(Y1, Qinv, n)
    return Y

def derive_key(base, exponent):
    return pow(base, exponent, n)


if __name__ == "__main__":
    print(f"n = {n}\ng = {g}\n")

    Q = invertible_Q(pwd)

    a = random.randint(2, n-2)
    b = random.randint(2, n-2)

    X1 = alice_send_X1(Q, a)

    Y1, X = bob_send_Y1_and_compute_X(Q, b, X1)

    Y = alice_compute_Y(Y1, Q)

    Key_client = derive_key(Y, a)

    Key_server = derive_key(X, b)

    print(f"X1 = {X1}")
    print(f"Y1 = {Y1}")
    print(f"Y  = {Y}")
    print(f"{'Let\'s start the session bro' if Key_client == Key_server else 'nah'}")
```

C'est un protocole PAKE (Password Authenticated Key Exchange) custom. On veut casser ce protocole pour retrouver le mot de passe.

On a :
- un grand nombre premier $n$ (512 bits)
- base $g = 2$
- un mot de passe secret pwd inconnu
- une fonction de hachage H(pwd) qui renvoie un entier modulo n-1
- une fonction invertible_Q(pwd) qui ajuste Q = H(pwd) pour que gcd(Q, n-1) = 1.
- Ensuite, Alice et Bob font des échanges:
  - Alice envoie $X_1 = g^{aQ} \bmod n$
  - Bob envoie $Y_1 = g^{bQ} \bmod n$ et calcule $X = X_1^{Q^{-1}} \bmod n = g^a$
  - Alice calcule $Y = Y_1^{Q^{-1}} \bmod n = g^b$
- La clé partagée est : $Key = g^{ab} \bmod n$

Donc $n, g, X_1, Y_1, X, Y$.

Or, $Q = \mathrm{invertible\_Q}(\mathrm{pwd})$ est secret (invertible_Q(pwd) = fonction qui s'assure que Q a un inverse mod n−1).
Mais :
$X_1 = g^{aQ} \bmod n$ et $Y_1 = g^{bQ} \bmod n$

Donc pour retrouver $Q$, il faut résoudre :
$Q ≡ log_X(X_1) \pmod{n-1}$

C’est un logarithme discret : impossible à casser directement pour $n$ sur 512 bits.

L’espace des mots de passe étant petit, on peut tester tous les candidats pwd,
càd calculer $Q = \mathrm{invertible\_Q}(\mathrm{pwd})$.
Puis vérifier si : $X^Q \bmod n = X_1$
Si l’égalité est vraie, on a trouvé le bon pwd.

$Q$ dépend de pwd, mais reste dans $\mathbb{Z}_{n-1}^*$.
Comme $n$ est premier, le groupe multiplicatif est cyclique d’ordre $n-1$.
On peut utiliser un dictionnaire ou brute-force.

Script pour brute-force sur un petit dictionnaire de mots (rockyou.txt) : 
```
from hashlib import sha256
from Crypto.Util.number import inverse

n = 13347009883618949844569534852918652790232045744561482858019385778321631987247529407341249651614011174400891057015201013063998724379417939791817413629090891
Y1 = 12909301433184182492219411534508555097299720925451896359396839402089681964790202969801412434403112475388757955113132851489073699006876294862598281870375303
Y = 5928304613146686176852942049762364955536998015292608677439575403896660328870338053776078606193789421466895566645992533623355867050456144423017536052765185

def H(pwd):
    return int(sha256(pwd.encode()).hexdigest(), 16) % (n - 1)

def gcd(a,b):
    while b:
        a,b = b,a%b
    return a

def invertible_Q(pwd):
    Q = H(pwd)
    while gcd(Q, n-1) != 1:
        Q = (Q + 1) % (n - 1)
    return Q

def try_password(pwd):
    Q = invertible_Q(pwd)
    try:
        Qinv = inverse(Q, n - 1)
    except ValueError:
        return False
    return pow(Y1, Qinv, n) == Y

with open("rockyou.txt", "r", encoding="latin-1") as f:
    for line in f:
        pwd = line.strip()
        if try_password(pwd):
            print(f"Mot de passe trouvé : {pwd}")
            print(f"Flag : AMSI{{{pwd}}}")
            break
```
