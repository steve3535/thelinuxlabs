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
  
* troisième tentative: ***b=list(a)***
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
   >  Hello, this is a multi line comment  
   >  Hasta Luego !  
   > '''  
3. bool('') --> False
   bool(' ') --> True
   int("4.7") --> value error 
4. Convertir en code ASCII et vice versa  
   Fonctions **ord()** et **chr()**  
5. lists  
   * remember a list in python can collect different types of data  
   * a string is immutable: impossible to do i place assignement  
   * *lst.pop()* --> will remove the last element of the list and will change the list lst  
   * *lst.remove(x)* --> remove first occurence of x  
   * *lst.index(x)* --> returns index of fisrst occurence of x  
   * check existence of element in a list:  
     *exists = x in lst*  
     *exists = lst.count(x) > 0*    
   * if x and y are lists, then x + y is a new brand list  
     x.extend(y) --> it will create a new version of x containing the elements of both lists  
 6. Strings  
    * s.count(x)  
    * s.find(x) equivalent to index() method for lists  
    * s.isdigit()  
    * s.split()  
    * s.replace('*','#')  
    * there is multiline strings, just like multiline comments !
 7. Tuples  
    * **They are immutables !!!**  
    * *x = 1,2,3,4* or *x = (1,)*  
 8. loops
    * *for i, element in enumerate(x):*  
      x being any data collection structure: list, string, tuple, ...  
      then we have access both to the element and the indice  
    * *for ... else*  
       to avoid those boolean variables within the snippet   
       you need a break inside the for  
    * *while ... else*  
  9. slices
    * it is an operator that works on any data collection  
    * syntax is **[start:stop:step]**  
    * to reverse a string, a tuple or a list: **[::-1]**  

  10. tips  
      * U wanna loop but u dont know the number of loops? use while instead of for  
      * U want to avoid using an additional boolean value to check a condition if its not met till the end of the loop? use for..else or while ..else  
      * U want to compare strings or digits representing strings, try to sort them first  
      * U want to do some kind of swap to switch variables or values, remember to use a temporary variable  
    
    
      
     
     
    
     
