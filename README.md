# Descripción
Desarrollo de una aplicación de pruebas para reconocimiento de usuarios mediante el uso de la cámara Intel RealSense Depth Camera D435I.
# Pasos a seguir
## Instalación de componentes necesarios para el funcionamiento del dispositivo
El proyecto hace uso de la librería dlib, para instalarla correctamente hay que seguir los pasos indicados en el siguiente enlace: https://github.com/IntelRealSense/librealsense/tree/master/wrappers/dlib

Ya que para este proyecto se ha utilizado como base algunos códigos de Intel, es necesario seguir los pasos indicados en el siguiente link (se recomienda eliminar la capeta "examples" y dentro de la carpeta "wrappers", eliminar todas las carpetas menos "dlib"): https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md

**Atención:** para que todo funcione correctamente, cuando se ha creado la carpeta build y se procede a ejecutar el programa cmak, hay que acordarse de ejecutar el programa con las referencias a la librería dlib instalada anteriormente. Utilizar el siguiente comando dentro de la carpeta build: **cmake ../ -DBUILD_EXAMPLES=false -DBUILD_GRAPHICAL_EXAMPLES=false -DBUILD_DLIB_EXAMPLES=true -DDLIB_DIR=~/work/dlib-19.22** 

## Preparación
Una vez instalado todo correctamente, copiamos los archivos de este repositorio a la siguiente dirección: **Facedetection/wrappers/dlib/face** y sustituimos los dos archivos que ya hay con el mismo nombre. Después de esto, en el directorio **/Facedetection/build/wrappers/dlib/face/**, ejecutamos el comando **sudo make install** y con esto estaremos compilando el código de la aplicación.

# Funcionamiento
Una vez que ejecutamos la aplicación, se tomarán las medidas de profundidad de nuestra cara. En mayor detalle, se toma la profundidad de la boca y de los ojos respecto a la nariz.
La aplicación es capaz de reconocer si una cara es "real" o "ficticia", por lo que solo se tomarán las medidas si la cara es real. Si se ha detectado la cara como "real", se nos pedirá si queremos registrarnos o loguearnos.
Después de hacer nuestra elección, añadimos nuestro nombre (acordarse de respetar mayúsculas y minúsculas) y con esto nos registraremos si hemos elegido esta opción, o nos loguearemos si hemos elegido esta otra.

# Posibles problemas
## No detecta la librería dlib 
Colocar el path correcto a la hora de ejecutar los archivos cmake. La versión de la librería instalada puede cambiar con respecto a la utilizada en esta aplicación por lo que habría que cambiar la versión en el comando indicado arriba.

## Tarda excesivamente en la primera compilación
Es normal que en la primera compilación de la aplciación tarde un poco más. Una vez realizado esto, el tiempo de compilación de los códigos aportados es mucho menor.
Una posible solución es aumentar el número de núcleos usados para realizar la compilación (se recomienda poner algunos menos que el total disponible en el equipo). Para esto, usamos el siguiente comando: **sudo make -jx [comandos]** donde "x" es el número de núcleos a usar.

## No detecta la cara
Puede ocurrir que en situaciones de baja luminosidad, no se detecte la cara correctamente. Por ello es aconsejable el uso de la aplicación en entornos luminosos.

## No reconoce al usuario aun habiendose registrado o reconoce a dos usuarios como el mismo
Puede darse el caso de que el rango de variación entre una medida y otra sea mayor al que se ha establecido por defecto. Para sulucionar esto, hay que modificar el valor de la variable "rangoDeteccion" en la linea 161 del código "rs-face-dlib.cpp". Un valor menor hará que la diferencia de profundidad entre una medición y otra sea más estricto, llevando al sistema a ser más restrictivo. Si aumentamos dicho valor, el sistema será más permisivo con la diferencia entre mediciones, produciendo que la aplicación sea menos segura. 
