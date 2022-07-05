# CTF-PWNME
## Depycted

Source code : https://github.com/Hatsu-Miku/CTF-PWNME/raw/main/rev/depycted/chall.pyc

Output :

      2195160159893668717327286059367551976012130689570892075754234400430874403925069147738764347075321
  
Le fichier est un script python compilé (l'extension est .pyc et pas .py)

Donc pour avoir le script originel on va utiliser uncompyle6 
![ChAlL py](https://user-images.githubusercontent.com/108684684/177321632-250381ac-5ccd-40c0-96ff-ab3081a2c726.PNG)

On va simplement reverse les deux fonctions a et b pour pouvoir décoder le flag chiffré
```python
from Crypto.Util.number import long_to_bytes, bytes_to_long
from pwn import xor

def a(first,second):
    return long_to_bytes(first ^ 13) + long_to_bytes(second ^ 37)


def b(value1,booleen):
   c = value1[::-1]

   if booleen:
      c = c[len(cipher)//2+10:]
   else:
      c = c[:len(cipher)//2+10]

   return int(c)

def decrypt(cipher):
    global a
    global b
    first, second = b(cipher,1),b(cipher,0)
    print(a(first,second))

cipher = "2195160159893668717327286059367551976012130689570892075754234400430874403925069147738764347075321"
decrypt(cipher)
```

Flag : PWNME{84ec5fec3e2ec91291bb74648d35dcbc4}
