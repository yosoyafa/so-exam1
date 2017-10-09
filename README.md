# Parcial 1

**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Docente:** Daniel Barragán C.  
**Tema:** Comandos de Linux, Virtualización  
**Correo:** daniel.barragan at correo.icesi.edu.co

**Fecha de entrega:** Domingo, 08 de octubre de 2017  
**Estudiante:** Carlos Andrés Afanador Cabal  
**Código:** A00054652  
**URL:** https://github.com/yosoyafa/so-exam1  

### SOLUCIÓN

**3.** 
* **sum_all_numbers:** En este reto se deben sumar todos los numeros presentes en un archivo de texto plano, y para logralo se debe instalar, de manera previa, jq de JSON (jq -s), el cual es capaz de sumar (add) los valores del arreglo dado (sum-me.txt).  
  
 ![][1]   
 
* **replace_spaces_in_filenames:** En este reto se deben reemplazar los espacios que hay presentes en los nombres de diferentes ficheros, por puntos. Para llevar a cabo el reto, primero se crearon 3 ficheros con un espacio en su nombre, ahora bien, el comando tr reemplaza los caracteres dedos en el primer parametro, por los datos en el segundo, pero de un listsdo, el cual es dado por el comando ls.  
  
 ![][2]   
 
 * **reverse_readme:** En este reto se pide leer un archivo de texto plano de manera inversa, para lo cual se uso el comando inverso a cat, el cual es tac, que hace especificamente lo que pide el reto!  
   
 ![][3]   
 
 * **remove_duplicated_lines:** En este reto se pide eliminar, de un archivo de texto plano, las lineas que esten repetidas, y para lograrlo se usa el comando awk el cual lee el archivo que uno le pase por parametro y despliega unicamente las líneas que coincidan con el patron pasado por parámetro (awk 'patrón' 'archivo'). Aquí se muestra el archivo antes y despues de aplicar el filtro:  
   
 ![][4]   
 ![][5]   
   
 
* **disp_table:** En este reto se pide que se muestren ciertos datos contenidos en un archivo .csv, en formato de tabla. Para lograrlo, se usó el comando 'column' el cual, acompañado de '-t' muestra la información dada de manera tabular, y acompañado de '-s,' toma la coma (',') como separador de cada columna.  
  
 ![][6]   
  
  
**4.**  El script para descargar los libros del proyecto Gutenberg consiste basicamente en hacer un 'wget' de la pagina que contiene el libro. La url de cada libro se compone de dos partes, una constante y una variable; la constante es: 'http://www.gutenberg.org/cache/epub/', seguido del número identificador del libro, la cadena '/pg', nuevamente el número identificador del libro, y la cadena '.txt' para finalizar; ejemplo: http://www.gutenberg.org/cache/epub/345/pg345.txt, aquí el identificador es 345. Ahora bien, al comando wget se le debe especificar la url, y la ruta donde se guardará el contenido descargado, en este caso: el libro. Para obtener libros diferentes en cada descarga, se usa un númeor aleatorio entre 300 y 744.  
  
 ![][7]   
 ![][8]   
   
 Para que el script sea ejecutado cada 5 minutos, se debe hacer uso del crontab (crontab -e, para editarlo), archivo en el cual podemos especificar que un script sea ejecutado cada tanto que sea necesario. Ahí solicitamos que el script bajarlibros.sh sea ejecutado cada dia de la semana (* * *), cada mes del año (* * *), cada dia del mes (* * *), cada hora del dia (* * *), y cada 5 minutos de la hora (5).  
   
 ![][9]   
      
  La ejecución directa del script:  
 ![][10]   
    
  Despliegue del libro descargado, y posterirormete un nuevo libro:  
 ![][11]   
 
   
     
**5.** En general, lo que hace este código es engañar al usuario del sistema haciendo que cuando quiera reproducir cualquier archivo con extension .mp3, se haga un Rickroll (reproducir Never Gonna Give You Up- Rick Astley). Teniendo en cuenta que el código realizará una intervención directa en los llamados al sistema, se necesita incluir ciertas librerías de linux: init.h, module.h, string.h, syscalls.h, kernel.h.  
  
Primeramente, se define la ruta en la que está almacenada la canción sorpresa y se definen también los parámetros del módulo y los punteros que determinan los permisos de edición para la parte de la memoria que vamos a modificar.  
  
 ![][12] 
 ![][13]
  
  
Luego se ejecuta el rickroll_init: primero se verifica que la ruta de la canción sea valida y así no generar errores peligrosos que pueden ocasionar daños al sistema; luego se pide al sistema la tabla de llamadas al sistema y también se verifica que esta se haya obtenido de buena manera. Ahora se modifica el registro cr0 para poder modificar los llamados al sistema, que es lo que se quiere hacer, con la protección desahabilitada, se guarda la llamada al sistema original -‘__NR_open’ (para que exista la posibilidad de volver el sistema a la normalidad), se cambia el valor que tiene la tabla en __NR_open, por el rickroll_open, y por último se vuelven habilitar la protección de los llamados. Si todo va bien, retorna ‘0’, sino retornará el error que se generó.  
  
  ![][14]   

El nuevo llamado al sistema, rickroll_open, debe verificar si el archivo que se quiere abrir es una canción (.mp3) y si es pertinente desviar la apertura; si el archivo no tiene extension mp3, se ejecuta la apertura normal, sino, se procede a la alteración, la cual cambia la ruta original a abrir, por la del rickrolling.  
  
 ![][15]   
  
Ahora bien, para volver el sistema a la normalidad, se tiene el método rickroll_cleanup, el cual se resigna la llamada al sistema original (guardada previamente en el rickroll_init) a su posición el la tabla de llamadas al sistema, no sin antes haber desactivado la protección, y sin olvidar activarla posteriormente.  
  
 ![][16]   
  
    
### Referencias
* https://cmdchallenge.com  
* https://www.gutenberg.org  
* https://github.com/jvns/kernel-module-fun/blob/master/rickroll.c
* https://www.youtube.com/watch?v=efEZZZf_nTc
* https://stackoverflow.com/questions/44044449/facing-issue-while-installing-jq-in-centos
* http://richardking.github.io/devops/unix/2014/10/20/ops-tools-awk-sed-grep/
* https://www.lifewire.com/file-contents-in-column-format-linux-4018107
* https://www.gnu.org/software/wget/manual/wget.html#Option-Syntax
* https://stackoverflow.com/questions/4181703/how-to-concatenate-string-variables-in-bash
* https://askubuntu.com/questions/609850/what-is-the-correct-way-to-edit-a-crontab-file
* http://www.tldp.org/LDP/lkmpg/2.6/html/x323.html
* http://www.makelinux.net/ldd3/chp-2-sect-8
* http://elixir.free-electrons.com/linux/v3.6/ident/KERNEL_DS
  
[1]: images/sum-me.png
[2]: images/replace.png
[3]: images/reverse.png
[4]: images/facesnolisto.png
[5]: images/faceslisto.png
[6]: images/table.png
[7]: images/script.png
[8]: images/pathbooks.png
[9]: images/crontab1.png
[10]: images/descargalibros.png
[11]: images/libro1.png
[12]: images/inc.png
[13]: images/prot.png
[14]: images/init.png
[15]: images/open.png
[16]: images/cleanup.png
