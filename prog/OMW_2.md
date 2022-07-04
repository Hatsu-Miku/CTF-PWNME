# CTF-PWNME
## On My Way 2/3

Pour la deuxième partie on doit maintenant donner le chemin le plus court entre E et S donc x+, x- etc...
![spinning_pot](https://user-images.githubusercontent.com/108684684/177193217-d321c5d8-8dd7-4d0a-aff8-8a981a7eae6a.PNG)

Bon on est des flemmard on va juste reprendre le code de OMW 1 et l'adapter un peu

Et on obtient (magie de la programmation):
```python
import pwn

def coor(L,k,sd):
   i = L.index(sd)
   return (i%k,(k-1)-(i//k)%k,i//(k**2))

def liste(conn):
   M = ""
   for c in conn.decode():
      if c == "0" or c == "E" or c == "S" or c == "X":
         M += c
   return list(M)

def ES_path(x,y,z):
   path = ""

   if x < 0:
      path += "x-;"*(-x)
   else:
      path += "x+;"*x

   if y < 0:
      path += "y-;"*(-y)
   else:
      path += "y+;"*y

   if z < 0:
      path += "z-;"*(-z)
   else:
      path += "z+;"*z

   return path

k =2
C = pwn.remote("prog.pwnme.fr",7001)

while True:
   try:
      txt = C.recvuntil(b'>>', drop=True)
   except:
      print(C.interactive())
      exit(0)

   M = liste(txt)

   if M == []:
      exit(-1)

   E = coor(M,k,"E")
   S = coor(M,k,"S")
   k += 1

   answer = ES_path(S[0]-E[0],S[1]-E[1],S[2]-E[2])[:-1]
   C.send(str(answer).encode()+b"\r\n")
```

(pour les calculs de coordonées lisez https://github.com/Hatsu-Miku/CTF-PWNME/blob/main/prog/OMW_1.md)

et encore une fois flag ez : PWNME{43zy_to0_r1Gh7}
