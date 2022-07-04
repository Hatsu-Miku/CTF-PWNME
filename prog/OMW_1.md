# CTF-PWNME
## On My Way 1/3

Le but de cette première partie du challenge est de determiner la distance du plus court chemin entre l'entrée (E) et la sortie (S) dans un graphe en 3D

![sex](https://user-images.githubusercontent.com/108684684/177189590-cd93288c-5ff6-4af3-9907-553e2480a319.PNG)

Pour cela on va simplement calculer les coordonées de E et S

Puis calculer chacunes des distances sur les axes x,y,z

Et sommer le tout...
on dirait une rectte de cuisine

Le code suivant fait largement l'affaire:

```python
import pwn

def coor(L,k,point):
	i = L.index(point)
	return (i%k,(k-1)-(i//k)%k,i//(k**2))

def liste(conn):
	M = ""
	for c in conn.decode():
		if c == "0" or c == "E" or c == "S" or c == "X":
			M += c
	return list(M)

k = 2
C = pwn.remote("prog.pwnme.fr",7000)

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


	answer = abs(E[0]-S[0])+abs(E[1]-S[1])+abs(E[2]-S[2])
	C.send(str(answer).encode()+b"\r\n")
```

Alors quelques petites explications sur le calcul des coordonées (parce que vous devez voir des attrocités plus haut)

Reprenons donc l'exemple du graphe 2x2x2 donné dans l'image qu'on passe sous forme de liste ainsi on a:
### 0 E 0 0 0 0 S 0
En écrivant les coordonées x,y,z du graphe dans la liste on a donc:
### x : 0 1 0 1 0 1 0 1
### y : 1 1 0 0 1 1 0 0
### z : 0 0 0 0 1 1 1 1
On peut donc calculer aisément les coordonées comme ce sont des motifs

On détermine que :

        x = i%k avec i l'indice dans la liste et k la longueur max d'un axe (dans ce cas 2)
        
        y = (k-1) - i//k%k le (k-1) est présent car le motif de y commence avec un 1 et pas un 0 et i//k%k permet la répétition du motif
        
        z = i//(k**2) même logique que y sans le (k-1)
        
Et ça c'est pour tout k >= 2

Enfin on obtient le flag ez : PWNME{n07_To0_d1ffiCul7}
