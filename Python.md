### Copy arrays is not that simple ...
soit une liste a, faire b=a réalise une copie de la référence à la liste a, autrement dit b et a pointe sur le meme ensemble espace memoire.  
Preuve:  
> a = [1,2,3]  
> b = a  
> b.append(4)
> a --> contient alors les valeurs [1,2,3,4]  

Comment copier juste par valeur ??
* premiere tentative: *b=a.copy()*  
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
  
* deuxième tentative: *b=a[:]*
* troisième tentative: *b=list(a)*
* 
