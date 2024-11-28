## Práctica : Git y GitHub
### Libre configuración - Grupo 3: Ángela Pedrera, Inés González y Raquel Aguilar
![Portada](https://live.staticflickr.com/65535/40666021673_fb324524ec_b.jpg)
## Objetivo de la práctica
En esta práctica, aprenderemos a usar comandos básicos de Git y GitHub, centrándonos en la creación y manejo de ramas dentro de un repositorio compartido. 

---
##### Índice
- **Creación de una rama**<br>
- **Listado y borrado de Ramas**<br>
- **Commit Inicial**<br>
- **Conflicto entre la web y local**<br>
- **Uso de Merge**<br>
- **Uso de Stash y Amend**<br>
- **Uso de Revert**<br>
- **Uso de Cherry Pick**<br>
- **Creación de Pull-Request**<br>
- **Conclusiones**
---

**Clave SSH**
~~~
ssh-keygen -t ed25519 -C raquel.aguilarcastellanos@iesvalleinclan.es
cat ~/.ssh/id_ed25519.pub
~~~

### Paso 1: Clonar el Repositorio
Empezamos por clonar el repositorio usando git clone, que crea una copia local del repositorio remoto, incluyendo el historial y las ramas.   
~~~
git clone https://github.com/Raquel1403/HLCProyecto.git
~~~
### Paso 2: Crear una Rama Nueva

Creamos una rama con nuestro nombre, donde haremos todos los cambios:
~~~
git checkout -b rama_v1
~~~

O bien:
~~~
git branch rama_v1
git checkout rama_v1
~~~

## Listado y borrado de ramas

1. En primer lugar vamos a listar las ramas del repositorio.
~~~
git branch
~~~

2. Vamos a crear otra rama
~~~
git branch rama_v2
~~~

2. A continuación, vamos a eliminar las rama v2. Para ello tenemos que estar en otra rama que no sea ninguna de ellas, como v1.
~~~
git checkout rama_v1
git branch -d rama_v2
~~~

En el caso que la rama también esté en remoto es necesario hacer el siguiente comando:
~~~
git push origin --delete rama_v2
~~~

## Commit inicial
1. Nos situamos en la rama rama_v1
~~~
git checkout rama_v1
~~~

1. Creamos un archivo command_history.txt para registrar los comandos utilizados.
~~~
echo "git clone https://github.com/Raquel1403/HLC.git" >> historialComandos.txt
echo "git checkout -b rama_v1" >> historialComandos.txt
~~~
2. Hacemos un commit de estos cambios:
~~~
git add .
git commit -m "Añadido comandos"
git push
~~~
3. Si la rama está creada en local pero no en remoto, Git nos indicará que es necesario subirla al repositorio remoto. 

~~~
git push --set-upstream origin rama_v1
~~~

## Provocando un conflicto

1. Desde la web de GitHub, en la rama `rama_v1` vamos a cambiar la primera línea del fichero `historialComandos.txt` por cualquier texto y después lo subiremos con el mensaje `Actualizado historial desde GitHub`.
   
2. Volvemos al repositorio local y **sin actualizar** la rama vamos a cambiar de nuevo la primera línea del fichero `historialComandos.txt`. Esta vez el texto debe ser diferente al del paso 1, y el mensaje del commit será `Actualizado historial desde consola`.
   
3. Desde consola, commitearemos los cambios y los subiremos.
~~~
git add .
git commit -m "Actualizado historial desde consola"
git push
~~~

4. Git mostrará un mensaje indicando que la rama no está actualizada, por lo cual realizaremos la actualización de la rama.
~~~
git pull origin rama_v1
~~~

5. Tras actualizar la rama intentaremos subir los cambios de nuevo. 
~~~
git push
~~~

6. Git nos informará de que hay un conflicto en el fichero y nos pedirá resolverlo.
Nuestra sugerencia es realizar el siguiente comando para comparar los cambios.
~~~
git config --global pull.rebase false
~~~
Con git config --global pull.rebase false, Git hará merge por defecto, lo cual es generalmente más seguro y fácil de manejar, ya que no cambia el historial de commits y te permite resolver los conflictos de forma manual y clara.

7. Abrimos fichero que tiene conflicto y resolvemos.

8. Tras hacerlo, volveremos a crear un commit con el conflicto resuelto y lo subiremos.
~~~
git add .
git commit -m "Conflictos resultos"
git push
~~~
## Merge entre ramas

1. Creamos otra rama desde `main` llamada `rama_v2`
~~~
git checkout -b rama_v2
~~~
2. Añadimos otra línea en el archivo txt existente:
~~~
echo "nueva línea desde rama_v2" >> historialComandos.txt
~~~
Nos aseguramos de que se haya escrito:
~~~
cat historialComandos.txt
~~~
IMPORTANTE: Antes de cambiar de rama, hacer commit o stash.
~~~
git add .
git commit -m "Nueva línea desde rama_v2"
~~~
3. Creamos otro archivo txt:
~~~
echo "nuevo archivo" >> nuevoHistorialComandos.txt
~~~
~~~
git add .
git commit -m "Nuevo archivo.txt desde rama_v2"
~~~
4. Nos situamos en la rama_v1
~~~
git checkout rama_v1
~~~
Comprobamos el contenido de nuestro archivo txt
~~~
cat historialComandos.txt
~~~
5. Desde esta rama, `rama_v1`, quiero obtener los datos de la rama_v2. 
~~~
git merge rama_v2 -m "Merge de rama_v2 a rama_v1"
~~~
Comprobamos que ha fusionado el contenido de nuestro archivo txt
~~~
cat historialComandos.txt
~~~
6. Hacemos push de los cambios. Observamos que no es necesario crear un nuevo commit con los cambios del merge porque Git ya lo hace automáticamente.
~~~
git push
~~~
7. Vamos a visualizar el historial de commits, y comprobamos que los commits que introducimos en la rama v1 se encuentran tal cual en la rama v2 mediante `git log`. Este comando muestra el historial de commits en orden cronológico inverso (del más reciente al más antiguo). Para salir del visualizador, basta con pulsar la tecla `q`.
~~~
git log
~~~

El comando `git log` cuenta con varias opciones:
~~~
git log -n <número>                                   # Limita la cantidad de commits mostrados
git log --author=<nombre-autor>                       # Filtra los commits por autor
git log --oneline                                     # Muestra cada commit en una línea única
git log --since=<fecha-inicial> --until=<fecha-final> # Filtra por rango de fechas
git log --graph                                       # Muestra un gráfico de ramas y merges
git log --grep="<texto-a-buscar>"                     # Busca commits por mensaje
git log -- <nombre-archivo>                           # Filtra commits por archivo
~~~

## Stash y amend
1. Volvemos a la rama v1 y actualizamos el fichero `historialComandos.txt` añadiendo en la última línea el mensaje "Haciendo stash desde rama_v1".
~~~
git checkout rama_v1
cat historialComandos.txt
echo "Haciendo stash desde rama_v1" >> historialComandos.txt
cat historialComandos.txt
~~~
2. Visualizamos el estado de los ficheros y observamos que Git nos indica que hay cambios que no han sido guardados.

~~~
git status
~~~

3. En lugar de realizar commit, vamos a guardar temporalmente el cambio en un stash. El stash es útil cuando necesitas cambiar de rama o realizar otras operaciones sin comprometer tus cambios actuales. Tras realizarlo, observamos que el cambio ha desaparecido. 

IMPORTANTE: Antes de hacer stash, hacer git add
~~~
git add .
git stash
~~~
~~~
cat historialComandos.txt
~~~
4. Nos situamos en otra rama, por ejemplo, en rama_v2, y modificamos, por ejemplo, el archivo txt.
~~~
git checkout rama_v2
cat historialComandos.txt
echo "hola desde rama_v2" >> historialComandos.txt
git add .
git commit -m "hola desde rama_v2"
~~~
5. Volvemos a la rama `rama_v1`
~~~
git checkout rama_v1
~~~
Comprobamos el archivo:
~~~
cat historialComandos.txt
~~~
6.  Vamos a visualizar la lista de stashes y a volver a aplicar el último cambio. Observamos que el cambio ha vuelto a aparecer.
~~~
git stash list
~~~
~~~
git stash apply "stash@{0}"
~~~
Comprobamos el archivo:
~~~
cat historialComandos.txt
~~~

7. Subimos el cambio al repositorio remoto con el mensaje `Stash desde rama_v2`.

~~~
git add .
git commit -m "Stash desde rama_v2"
git push 
~~~
8. Nos damos cuenta que el mensaje está equivocado porque es desde la rama_v1. Para ello,  vamos a editar el mensaje del commit que acabamos de publicar con el flag `--amend` y escribiremos como nuevo mensaje de commit el texto `Stash desde rama_v1`. Este flag se utiliza junto con el comando `git commit` para realizar modificaciones en el commit más reciente. Permite modificar el mensaje del commit, agregar cambios olvidados o combinar nuevos cambios en un commit ya realizado.
~~~
echo "prueba" >> prueba.txt
git add .
git commit --amend -m "Stash desde rama_v1"
~~~
9. Para sustituir el nuevo commit por el anterior es necesario forzar el push mediante el flag `-f`. De esta forma le indicamos a Git que sobrescriba el historial remoto con nuestro historial local, incluso si eso implica reescribir la historia de commits del repositorio. Por defecto, Git no permite actualizar el historial remoto de manera predeterminada si eso implica cambios en la historia. Hay que tener en cuenta que esto también afectaría a los ficheros si se hubiesen realizado cambios con los peligros que eso conlleva, por lo que hay que utilizar el forzado con precaución.
~~~
git push -f
~~~
Si no hubiésemos hecho push del commit anterior, habría bastado con hacer el amend en local, ya que no implicaría ningún cambio en la historia del repositorio.

## Revert
1. Vamos a revertir el último commit realizado en la rama_v1. Para ello, visualizamos el historial de commits y copiamos el **id** del último commit realizado. Es necesario conocer este id para poder indicarle al comando `revert` qué commit debe revertir. A diferencia de `git reset`, que elimina los commits y puede reescribir el historial, `git revert` no modifica el historial existente; en su lugar, agrega nuevos commits que deshacen los cambios de un commit anterior.
~~~
git log
~~~

2. Realizamos un revert al commit y lo pusheamos. Tras ejecutar el comando se abrirá un vim donde podremos editar el mensaje del commit. Para salir sin realizar ningún cambio, pulsamos `:q`. Si hemos realizado algún cambio y queremos que se guarde, pulsamos `:wq`. Observamos que no es necesario crear un nuevo commit ya que el revert lo hace automáticamente.
~~~
git revert <id>
cat .\historialComandos.txt
git push
~~~

## Cherry Pick

1. Nos movemos a la rama `rama_v2`.
~~~
git checkout rama_v2
~~~
2. Añadimos otra línea al archivo .txt y conmiteamos.
~~~
echo "hola2 desde rama_v2" >> historialComandos.txt
cat historialComandos.txt
git add .
git commit -m "hola2 desde rama_v2"
~~~

3. Utilizamos cherry-pick para aplicar un commit específico de la rama `rama_v2` en la rama `main`. Este comando permite seleccionar y aplicar commits específicos en otra rama, lo que puede ser útil cuando solo queremos aplicar cambios específicos de una rama a otra, sin necesidad de fusionar todas las ramas o realizar un merge completo.
~~~
git checkout main
git log rama_v2
git cherry-pick <id del commit que queremos recuperar en el main>
~~~

4. Tras ello, subimos los cambios al repositorio remoto.
~~~
git add .
git commit -m "Cherry-pick hecho al main de un commit de rama_v2"
git push
~~~

## Crear Pull-request
1. Desde la web de GitHub, hacemos una Pull Request para incorporar la rama v2 en la v1. Para ello, seleccionamos `Open pull request` en el desplegable `Contribute`. Dejamos la PR vacía y la publicamos.

2. Tras ello, aceptamos nosotros mismos el PR e incorporamos los cambios de rama_v2 en rama_v1 mediante el botón `Merge`.

## Conclusión

En resumen, Git y GitHub son herramientas esenciales para el desarrollo colaborativo en proyectos de software. Durante esta práctica, hemos aprendido a gestionar ramas, resolver conflictos y trabajar de forma coordinada, lo que optimiza el flujo de trabajo en equipo. Git nos permite un control detallado del historial de cambios, garantizando que cada contribución quede registrada y pueda ser revisada o revertida cuando sea necesario. GitHub complementa estas funcionalidades al facilitar la colaboración remota y la revisión de código a través de pull requests.

Además, muchos de los entornos de desarrollo integrados (IDEs) que utilizamos actualmente ya incorporan soporte para Git, lo que simplifica aún más su uso. Esto permite que los desarrolladores puedan realizar tareas como commits, pushes o resoluciones de conflictos directamente desde su entorno de trabajo, agilizando los procesos y aumentando la productividad. Estas herramientas no solo mejoran la calidad del desarrollo, sino que también promueven la organización, la transparencia y el trabajo en equipo en proyectos de cualquier escala.
