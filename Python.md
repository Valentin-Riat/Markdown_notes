# Python

## basics
[Lien du cours (Openclassroom)](https://openclassrooms.com/fr/courses/235344-apprenez-a-programmer-en-python)

### Types et arithmétique

> Taper `type(variable)` pour savoir le type d'une variable.

**base**

```python
10**2        # 10 puissance 2
10/3         # division à virgule (= 3.333...)
10//3        # division euclidienne (= 3)
```

```python
max(lst_of_num) # retourne le plus grand nombre de la list
max(lst_of_lst, key=len) # retourne la sous-liste la plus grande 

from operator import attrgetter
max(lst_of_objects, key=attrgetter('capacity')) # retourne l'objet dont l'attibut 'capacity' est le plus grand 
```

### String

**Déclaration string**

```python
str1 = "abc' \" abc"    # correspond à : abc' " abc
str2 = 'l\'epfl'    #correspond à : l'epfl
str3 = str1+str2    #correspond à : abc' " abcl'epfl
```

**Misc strings**

```python
lettre in "baobab" #renvoie true si 'lettre' (char) 
                   #appartient à "baobab"
```

**méthodes de string**

> Presque toutes les méthodes de string ne modifie pas le string mais en renvoie un nouveau.

```python
str.lower()        #met tout en minuscule
str.upper()        #met tout en majuscule
str.capitalize()    #met la première lettre en majuscule
str.strip()        #enlève tous les espaces au début et à la fin d'une chaine
str.center(10)        #ajoute des espaces au début et à la fin pour obtenir la taille de chaine en arg
str.replace(" ", ",") # Remplace les espaces par des virgules 
"bla {0} bla {1}".format(str1, str2) # remplace {0} par str1 et {2} par str2
```

```python
len(str)     #renvoie la longueur de la chaine
str[0]        #renvoie le premier caractère
str[-1]        #renvoie le dernier caractère 
str[2:5]    #renvoie un string qui va du 2ème char (compris) jusqu'au 5ème (non compris)
str[2:]     #renvoie un string qui va du 2ème char (compris) jusquà la fin

list = str.split(",") #converti le string en list en utilisant le délimiteur en argument
list = "a,bc,d".split(",") # list = ['a','bc','d']

str = ''.join(list) #join une liste de string en un unique string
```

(sinon taper `help(str)`)

### ANSI codes with strings

An ANSI escape code can be inserted into a string to modify it

color codes  :

|Color  | ANSI code | 
|:-     |:-  |
|Violet |"\033[95m"|
|Blue   |"\033[94m"|
|Green  |"\033[32m"|
|Red    |"\033[91m"|
|Yellow |"\033[1;33m"|
|Gray   |"\033[90m"|
|Reset  |"\033[0m"|

### Listes

> contrairement aux string, les méthodes de list modifie les objets eux même

```python
lst = [1,10,100]
lst.append(10)    # ajoute un élément à la fin
lst.insert(1, 5)    # insert '5' à la place de l'indice 1 (indice est 1 décalé vers la fin)
lst.extend(lst2)    #insert lst2 à la fin de lst
del lst[1]        #delete l'élément 1 de la liste
lst[0], lst[4] = (lst[4], lst[0]) # swap les elements 0 et 4

[elem**2 for elem in lst if elem < 100]    # renvoie une list correspondante à lst mais avec tous ces élément inférieur à 100 au carré.

any(elem == 1 for elem in lst) # renvoie true si un élement de lst est égal à 1
```

> taper `help(list)` pour plus d'info

###Dictionnaires
> taper `help(dict)` pour plus d'info

```python
dict["clef"] = valeur
dict = {clef1: valeur1, clef2:valeur2}
del dict[clef1]
for clef,value in dict.items():
```

### Structures

```python
if == 1 and b <= 9 :
    #do something
elif a != 2 or b > 8 :
    #do something else

for lettre in str1 :
    print(lettre)

for i,elem in enumerate(list1) :
    print("indice: {}, élément : {}".format(i,elem) )    
```

### Fonctions

```python
def nom(param1, param2):
    """docstring"""
    #instructions

def nom(*params):     #met tous les params reçu en entrée dans un tuple, utile pour avoir un nb indéterminé de paramètres non nommés en entrée
def nom(**params)    #met tous les params reçu en entrée dans un dictionnaire, utile pour avoir un nb indéterminé de paramètres nommés en entrée

help(nom)    #affiche le docstring de la fonc 'nom'
f = lambda param1 : param1 * 2
f(3)     # renvoie 3*2
```

### Exceptions

[Lien pour la liste des exceptions built in](https://docs.python.org/3/library/exceptions.html)

```python
try:
    #code
except ZeroDivisionError:
    pass #rien de se passe en cas de cette erreur
except TypeError as errorTxt:
    print(errorTxt)
else:
    print("aucune des erreurs n'est survenu")
finally:
    #executé quoi qu'il arrive à la fin
```

```python
raise TypeDErreur("message de l'erreur")
```

Autre méthode pour le débuging :

- le code `assert [bool]` raise l'erreur `AssertionError`

### Fichiers

```python
import os
os.chdir("C:/...")
with open('nomFichier', 'w') as fichier: #le mode 'w' écrase le contenu, 'a' ajoute à la fin
    fichier.write("sample text")
with open('nomFichier', 'r') as fichier:
    str = fichier.read()

import pickle
with open('nomFichier', 'wb') as fichier:
    mon_pickler = pickle.Pickler(fichier)
    mon_pickler.dump(variable_à_enregistrer)
with open('nomFichier', 'rb') as fichier:
    mon_depickler = pickle.Unpickler(fichier)
    mon_depickler.load()
```

### Classes

**Exemple** :

```python
class Compteur:              
    objets_crees = 0 # Le compteur vaut 0 au départ (attribut de 
    def __init__(self):                   #constructeur
            """À chaque fois qu'on crée un objet, on incrémente le compteur"""
            Compteur.objets_crees += 1
        self.label = 'new_count'          # attribut classique
    def combien(cls):                     # méthode de classe
            """Méthode de classe affichant combien d'objets ont été créés"""
            print("Jusqu'à présent, {} objets ont été créés.".format(cls.objets_crees))
    combien = classmethod(combien)        # ligne requise pour une méthode de classe

    def afficher():                       # méthode statique
            print("On affiche la même chose, peu importe l'objet.")
    afficher = staticmethod(afficher)     # ligne requise pour une méthode statique
```

#### Les propiétés :

```python
class Personne:
    def __init__(self, nom):
        self._nom = nom
    def _get_nom(self):
        return nom
    def _set_nom(self, str):
        self._nom = str
    nom = property(_get_nom, _set_nom, _del_nom, _help_nom) # nom est un propriété de la class
```

#### Les méthodes spéciales :

| méthodes spéciales                   | effet                                                                                                               |
|:------------------------------------ |:------------------------------------------------------------------------------------------------------------------- |
| \_\_init\_\_(self, ...)              | constructeur                                                                                                        |
| \_\_del\_\_(self)                    | destructeur                                                                                                         |
| \_\_repr\_\_(self)                   | défini ce qui s'affiche (avec `return my_str`) losqu'on tape directement le nom d'un objet                          |
| \_\_str\_\_(self)                    | défini ce qui s'affiche lorsqu'on fait `print(obj)`                                                                 |
| \_\_getattr\_\_(self, nom)           | est appelée si on fait `obj.nom` mais que `nom` n'existe pas                                                        |
| \_\_setattr\_\_(self, nom, value)    | est appelé  pour modifier un attribut (**_exemple plus bas_**)                                                      |
| \_\_delattr\_\_(self, nom)           | est appelé pour supprimer un attribut (`del obj.nom`)                                                               |
| **Math**                             |                                                                                                                     |
| \_\_add\_\_(self, value)             | défini `obj + value`                                                                                                |
| \_\_radd\_\_(self, value)            | défini `value + obj`                                                                                                |
| \_\_iadd\_\_(self, value)            | défini `obj += value                                                                                                |
| même principe pour                   | `__sub__`, `__mul__`,  `__truediv__` , `__floordiv__`(div entière), `__mod__`(modulo %), `__pow__` (puissance), ... |
| **Comparaison**                      |                                                                                                                     |
| \_\_eq\_\_(self, obj)                | `==`                                                                                                                |
| \_\_ne\_\_(self, obj)                | `!=`                                                                                                                |
| \_\_gt\_\_(self, obj)                | `>`                                                                                                                 |
| \_\_ge\_\_(self, obj)                | `>=`                                                                                                                |
| \_\_lt\_\_(self, obj)                | `<`                                                                                                                 |
| \_\_le\_\_(self, obj)                | `<=`                                                                                                                |
| **Objet conteneur**                  |                                                                                                                     |
| \_\_getitem\_\_(self, index)         | appelé quand on tape `obj[index]`                                                                                   |
| \_\_setitem\_\_(self, index,  value) | appelé quand on tape `obj[index] = value`                                                                           |
| \_\_delitem\_\_(self, index)         | appelé quand on tape `del obj[index]`                                                                               |
| \_\_contains\_\_(self, value)        | appelé quand on tape le mot `in` (renvoie un bool) $\phantom{xxxxxxx}$ ex : `8 in obj`                              |
| \_\_len\_\_(self)                    | renvoie la taille de l'objet (le nb d'item)                                                                         |

> taper `dir(un_objet)` permet de connaitre tous les attributs et méthodes de cet objet

### Héritage

Si la classe B hérite de A, on déclare `class B(A):`
B hérite des méthodes de A mais il faut déclarer tous les attributes dans le constructeur de B ( on peut utiliser `A.__init__(self, ...)` )

> `issubclass( subclass, class)` vérifie si `subclass` hérite bien de `class`
> `issinstance(obj, class)`vérifie si `obj` est une instance de `class` ou d'une de ces super-classes

### Itérateurs, Générateur

[Lien OpenClassroom](https://openclassrooms.com/fr/courses/235344-apprenez-a-programmer-en-python/233261-decouvrez-la-boucle-for)

### Create your own library

1. make a file with all the functions of the lib.
1. put this file in the Lib folder of the python install files (probably at _C:\Users\Valentin\AppData\Local\Programs\Python\Python310\\_)
1. to import your lib, use `import filename`

# Modules standards Python

## Expressions régulières (regex)

| Signes      | signification                                           | regex : correspond       |
|:-----------:| ------------------------------------------------------- |:------------------------ |
| ^           | indique qu'on au début d'une chaine                     | ^cha : chat, ~~achat~~   |
| $           | on cherche à la fin                                     | at\$ : chat, ~~chatton~~ |
| *           | le char précédant peut ce trouver 0, 1, ou plus de fois | ab* : abc, abbc, ac      |
| +           | le char précédant peut ce trouver 1 ou plus de fois     | ab+ : abc, abbc, ~~ac~~  |
| ?           | le char précédant peut ce trouver 0 ou 1 fois           | ab? : abc, ~~abbc~~, ac  |
| .           | match n'import quel character                           | .b  : abc, cbc, ~~bc~~, ~~aa~~ |
| {3}         | le char précédant ce trouve 3 fois                      | e{2} : aee ~~ae~~        |
| {1,3}       | le char précédant ce trouve entre 1 et 3 fois           |                          |
| {,3} / {3,} | le char précédant ce trouve au max/min trois fois       |                          |
| [abcd]      | le char peut être soit a,b,c ou d                       | [abcd]e : ae, be         |
| [A-Za-z0-9] | le char peut être une lettre ou un chiffre              |                          |
| ()          | appliquer un des controles si dessus à plusieurs char   | (ab){2} : abab           |
|             |                                                         |                          |

Pour match in character spécial comme . ou ?, il faut écrire \. ou \?

```python
import re
re.search('[a-z]bc', chaine_a_tester) # renvoie None si la regex n'est pas dans le string
re.sub('^0([0-9]{2})', '+41\\1', '022 342 15 33') # renvoie '+4122 342 15 33'
```

> *re.sub(regex, str_remplacement, str_a_changer)* : les parenthèses dans la regex répresentent les groupes qui sont référer plus tard (par `\\1`, `\\2`, etc) dans le string de remplacement si besoin 

## OS

Pour déplacer et copier des fichier, utiliser `import shutil`

```python
import os
os.chdir(C:/Users/) # change directory to c:\users\
os.getcwd() # returns the directory from which the script is run (not always the file path)
            # the file path is contained in the variable : __file__
list = os.listdir() # renvoie une liste des noms des fichier dans le dossier courant (ici C:/Users/) sous forme de strings
os.remove("file.txt") # supprime le fichier
os.rename("old_name.txt", "new_name.txt")

os.path.abspath("c:/Users/") # returns "c:\Users\", good for cross-platfrom compatibility
os.path.dirname("c:/Users/filename.txt") # returns "c:/Users/"
file_path = os.path.dirname(os.path.realpath(__file__)) # file_path is the path to the file without the file name
```

Pour trouver tous fichier qui match une expression
```python
from glob import glob

list_of_file = glob('*.pdf') # retourne une liste de tous les fichiers du dossier qui sont des pdf
list_of_file = glob('*.pdf', recursive=True) # regarde aussi dans les sous-dossiers (peut prendre beaucoup de temps si beaucoup de sous-dossier)
```


## Time

```python
import time
time.time()      # donne les secondes depuis UNIX EPOCH (1 janv. 1970)
time.localtime() # renvoie un objet contenant des infos comme la date, l'année ...
time.sleep(10.5) # met le programme en pause pendant 10.5 secondes 
```

## Programmation système (sys)

### les flux standard

```python
import sys
sys.stdin  = ...   # redéfini l'entrée standard
sys.stdout = ...   # redéfini la sortie standard (peut être un fichier)
sys.stderr = open('erreur.txt', 'a')   # redéfini la sortie des erreurs vers un fichier (qui est créer si besoin)
sys.stdin  = sys.__stdin__   # rétabli l'entrée de base
sys.stdout = sys.__stdout__  # rétabli
sys.stderr = sys.__stderr__  # rétabli
```

### Les signaux

ils sont utilisé pour la communiquation **système <-> programme** et **programme <-> programme**.
Notre programme peut intercepter des signaux et réagir en conséquence.
Exemple avec le signal `SIGINT` (signal "il faut fermer le programme") : 

```python
import signal
import sys
def fermer(signal, frame) :    # les deux arguments sont obligatoires
    print("fermeture du programme")
    sys.exit(0)                # commande pour fermer le programme sans erreurs
signal.signal(signal.SIGINT, fermer)   # lie le signal à la fonction 'fermer' 
```

### Interpreter les arguments de la console de commande

#### La façon simple

```python
import sys

sys.argv[0]  # contient le nom du programme
sys.argv[1:] # list des argument passé au programme
```

#### La façon clean

[Documentation](https://docs.python.org/3/library/argparse.html)

`sys.argv` est une liste contenant le nom du programme puis les arguments passé.
Pour aller plus loin, il faut utiliser argparse :

```python
import os
import sys
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('file', type=str, help="nom générique des fichiers à modifier sans indice ni extention")
parser.add_argument('final_file', type=str, help ="nom générique de fichier final") 
parser.add_argument('fin', type=int, help = "indice par lequel le script va finir")
parser.add_argument('shift', type=int, help= "chiffre qui va s'aditionner à l'indice initial pour créer l'indice final")
parser.add_argument('-n','--number') # si -n 8 est specifier, args.number vaudra 8
parser.add_argument('-o','--overwrite', action='store_true') # si -o est specifier, args.overwrite sera True
args = parser.parse_args()

fname  = args.file
ffname = args.final_file
n      = args.fin
shift  = args.shift
```

La fonction `add_argument~

### Envoyer des commandes au système

`os.system('ls')` envoie une commande au système mais ne permet pas de voir le retour de la commande.
`cmd = os.popen('ls')` permet de voir le retour d'une commande en faisant `cmd.read()`.
Ces commande sont exécutée dans un environnement séparé (faire 'sleep 5' n’arrêtera pas le programme pendant 5 secondes)

# Panda

[Colab link](https://colab.research.google.com/notebooks/mlcc/intro_to_pandas.ipynb?utm_source=mlcc&utm_campaign=colab-external&utm_medium=referral&utm_content=pandas-colab&hl=en#scrollTo=aSRYu62xUi3g)

## Datatypes :

- *DataFrame :* data table (w/ rows and named columns)

- *Series :* a single column with a name (possibly in a dataFrame)
  
  > a DataFrame is constituted of Series.
  >   ###Useful syntax
  
  ```python
  import pandas as pd
  series1 = pd.Series(['lions', 'cat', 'etc'])    #create a Series
  dataFrames1 = [{}]
  ```

# Numpy

[Documentation](https://numpy.org/devdocs/user/index.html)

```python
import numpy as np
```

## Les méthodes

### Création

```python
np.array([1,2,3,4.4], dtype='float32')    #crée un tableau(matrice) en convertissant au type 'float32' (arg. facultatif)
np.zeros( (2,5) )    # crée une matrice 2 lignes 5 colonne remplie de 0
np.ones( (2,5) )     # même chose mais remplie de 1
np.full((2,5), 3.1)  # même chose mais remplie de 3.1
np.linspace(0,1,5)   # un tableau de 5 valeurs uniformément réparti entre 0 et 1
nb.arange(0,20,2)    # un tableau de 0 à 20 comprenant une valeur sur deux
np.linspace(start, stop, num)#crée une liste de start à stop(non inclus) de num éléments répartis linéairement entre start et stop
nb.random.random(9)  # un tableau de 9 valeurs aléatoire (voir aussi randint et normal)
nb.eye(3)            # matrice identité de 3x3
```

### Propriétés

Soit une array `tab` 

```python
tab.ndim     #renvoie le nb de dimension
tab.shape    #renvoie un tuple contenant la taille de chaque dimension (ex : (2,3) )
tab.size     #renvoie le nb de cases
tab.dtype    #renvoie le type les éléments dans la matrice
type(tab)    #renvoie le type de la matrice ('ndarray')
```

### Accéder aux éléments et renvoyer des sous-listes

```python
tab[start:stop]        # renvoie une liste de start à stop
tab[start:stop:step]   # si step = 2, renvoie un élément sur deux
tab[:stop]             # renvoie du tout début jusqu'à stop
tab[::2]               # renvoie un élément sur 2
tab[::-1]              # inverse
tab[-1]                # le dernier élément
```

```python
np.concatenate([tab1, tab2]) # met tab2 à la suite de tab1
np.vstack([a,b])    # vertical stack
nb.hstack([a,b])    # horizontal stack
```

### FFT
```python
sig_fft = np.fft.fft(signal, signal.size)
freq = np.fft.fftfreq(signal.size,d)
```

### Opération sur les listes

```python
tab + 2     # renvoie le tableau avec l'operation faite pour toutes les cases
tab < 10    # renvoie un tableau avec true ou false à chaque case
where(tab < 10)  # renvoie une liste contenant les indices des cases respectant la condition
```

#Mathplotlib
[cheatsheets](https://matplotlib.org/cheatsheets/)
```python
import matplotlib as plt
```
```python
plt.plot(x,y)
plt.show()
```
```python
#Plots zeros and poles (in var z and p) on a graph
t = np.linspace(0, 2*np.pi, 100)
plt.figure(figsize=(5,5))
plt.plot(np.cos(t), np.sin(t), linewidth=1)
plt.vlines(0, ymin=-1.2, ymax=1.2)
plt.hlines(0, xmin=-1.2, xmax=1.2)
plt.scatter(p.real, p.imag, marker='x', c='r', label='pôles')
plt.scatter(z.real, z.imag, marker='o', c='r', label='zéros')
plt.legend()
plt.title("Pôles et zéros")
plt.xlabel("Réel")
plt.ylabel("Imaginaire")
```

# Pikepdf
[Documentation](https://pikepdf.readthedocs.io/en/latest/tutorial.html#)

## Open a pdf and rotate some pages
```python
with pikepdf.Pdf.open("filename.pdf") as pdf: # opens the pdf

    pdf.pages[0].Rotate = 90  # rotate page 1 clockwise
    pdf.pages[1].Rotate = -90 # rotate page 2 counterclockwise
    pdf.pages[4].Rotate = 180 # rotate page 5 of 180°

    pdf.save("filename_rotated.pdf") # saves the modified file (overwriting the initial file is not possible)
```

## Modifier les pages d'un PDF

Une fois le pdf ouvert, l'objet `pdf.pages` se comport comme une liste, on peut alors les manipuler facilement :  
```python
with pikepdf.Pdf.open("filename.pdf") as pdf: # opens the pdf

    pdf.pages.reverse()  # inverse l'ordre des pages
    pdf.pages.append(10) # ajoute un élément à la fin
    pdf.pages.insert(1, other_pdf.pages[0, 2]) # insert les pages 1 et 2 de 'otherpdf' à l'indice 1 (l'indice 1 du pdf est décalé vers la fin)
    pdf.pages.extend(other_pdf.pages) #insert un autre pdf à la fin du premier
    del pdf.pages[1]     #delete la page 2
    pdf.pages[0], pdf.pages[4] = (pdf.pages[4], pdf.pages[0]) # swap les pages 1 et 5


    ###########################################
    ## Don't forget to do this before saving ##
    ##      if multiple pdf where used       ##
    ###########################################
    pdf.remove_unreferenced_resources()
    pdf.pdf_version = max(pdf.pdf_version, other_pdf.pdf_version)

    pdf.save("filename_rotated.pdf") # saves the modified file (overwriting the initial file is not possible)
```

Pour voir le numéro de la page (pour les pdf utilisant des chiffes romains par exemple) : `pdf.pages[0].label` cela renvoie un string


#PyPDF2

```python
from PyPDF2 import *
# code qui merge file1.pdf et les page 1,3 et 5 de file2.pdf et qui le sauvegarde dans result.pdf
merger = PdfFileMerger()
merger.append('file1.pdf')
merger.append('file2.pdf', pages=(0, 6, 2)) # pages 1,3, 5 (it's 0 to 6 not included every 2 pages)

merger.write("result.pdf")
merger.close()
```

see this [Tuto](https://realpython.com/pdf-python/#history-of-pypdf-pypdf2-and-pypdf4)
