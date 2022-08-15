### Copy arrays is not that simple ...
soit une liste a, faire b=a réalise une copie de la référence à la liste a, autrement dit b et a pointe sur le meme ensemble espace memoire.  
Preuve:  
> a = [1,2,3]  
> b = a  
> b.append(4)
> a --> contient alors les valeurs [1,2,3,4]  

Comment copier juste par valeur ??
* premiere tentative: ***b=a.copy()***  
  ca marche , ca copie les valeurs, mais ca ne marche pas bien pour les imbrications  - dans le cas de nested lists  
  exemple:  
  ```
  >>> a=[[1,2],[3,4]]
  >>> b=a.copy()
  >>> b
  [[1, 2], [3, 4]]
  >>> b.append([5,6])
  >>> b
  [[1, 2], [3, 4], [5, 6]]
  >>> a
  [[1, 2], [3, 4]]
  >>> b[0][0]=10
  >>> a
  [[10, 2], [3, 4]]
  >>> 
  ```
* deuxième tentative: ***b=a[:]***
  meme phenomene que la premiere tentative  
  
* troisième tentative: *b=list(a)*
  meme phenome que la premiere tentative  
* ULTIME TENTATIVE: 
  ***import copy***  
  ***b=copy.deepcopy(a)***  
  dans ce cas, on a le résultat escompté même avec les nested lists, mais ca a un coût: deepcopy est une fonction récursive ...  
  Exemple:  
  ```
  >>> import copy
  >>> a=[[1,2],[3,4]]
  >>> b=copy.deepcopy(a)
  >>> b
  [[1, 2], [3, 4]]
  >>> b[0][0]=10
  >>> b
  [[10, 2], [3, 4]]
  >>> a
  [[1, 2], [3, 4]]
  >>> 
  ```
Pour de plus amples explications, voir --> https://stackoverflow.com/questions/2612802/how-do-i-clone-a-list-so-that-it-doesnt-change-unexpectedly-after-assignment  


  
### CHEAT SHEET  
1. le type et l'adresse d'une variable:  
   > type(int)  
   > id(int)  
2. les commentaires multiline:  
   > '''
   > Hello, this is a multi line comment  
   > Hasta Luego !  
   > '''  
   
