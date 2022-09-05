# Proyecto de balanceo de carga y failover
Descripción: El proyecto consiste en simular alguna aplicación web brindada, esta debe ser replicada en varios nodos del clúster brindando un acceso optimo a cada
usuario que se conecte, posteriormente se simulará una falla en alguno de los servidores/nodos para lo cual el orquestador
debe levantar un nuevo servidor, manual o automáticamente.

## Integrantes :frowning_man:
- José Luis Colcha
- Wendy Palomo
- Roberth Pincha
- Luis Jacome

## Arquitectura

[![3fbfb852-0bf8-434a-a9cd-875a55de2a70.jpg](https://i.postimg.cc/wTF6r56c/3fbfb852-0bf8-434a-a9cd-875a55de2a70.jpg)](https://postimg.cc/rK0X0rRK)

## Funcionamiento 📌
1. Como primer paso se procede a crear una carpeta que contiene el sistema web y el archivo Dockerfile. Estos archivos son necesarios para generar una imagen dentro de docker, lo cual ayudara para realizar el levantamiento de los servidores con minikube.

[![Whats-App-Image-2022-09-05-at-3-12-39-AM.jpg](https://i.postimg.cc/xd68Zg7L/Whats-App-Image-2022-09-05-at-3-12-39-AM.jpg)](https://postimg.cc/QBWj9gjM)

2. Para el despliegue del sistema web se configura el archivo Dockerfile y posteriomente se realiza la ejecucion del mismo.

[![Whats-App-Image-2022-09-05-at-3-13-59-AM.jpg](https://i.postimg.cc/NjfxSGfV/Whats-App-Image-2022-09-05-at-3-13-59-AM.jpg)](https://postimg.cc/SJ38zh97)

3. Paso siguiente se confirma que los procesos se esten ejecutando mediante el comando minikube status.

[![Whats-App-Image-2022-09-05-at-3-15-44-AM.jpg](https://i.postimg.cc/jjN0wh0y/Whats-App-Image-2022-09-05-at-3-15-44-AM.jpg)](https://postimg.cc/qtkZWy77)

4. Nosotros generaremos un archivo YAML para realizar el deploy de los contenedores que queremos ejecutar junto a nuestra imagen de docker previamente generada.

[![Whats-App-Image-2022-09-05-at-3-16-22-AM.jpg](https://i.postimg.cc/8546GM5V/Whats-App-Image-2022-09-05-at-3-16-22-AM.jpg)](https://postimg.cc/VrS6BSn7)

5. Al realizar el deploy, los pods no son generados correctamente puesto que minikube trabaja con imagenes alojadas en la nube. Por ello, se debe trabajar con el servicio de Docker Hub para enviar la imagen creada de manera local, al servicio en la web.

[![88379f22-1fd1-4670-b0b4-21632c6d6f67.jpg](https://i.postimg.cc/j5xwdcLS/88379f22-1fd1-4670-b0b4-21632c6d6f67.jpg)](https://postimg.cc/sBLDTpYk)

6. Creamos una cuenta en Docker Hub y definimos un repositorio el cual es donde se alojara nuestra imagen de docker que coniene nuestra pagina estatica.
[![Whats-App-Image-2022-09-05-at-3-18-17-AM.jpg](https://i.postimg.cc/RFGxTVPR/Whats-App-Image-2022-09-05-at-3-18-17-AM.jpg)](https://postimg.cc/r0dHVc20)

7. Despues de establecer nuestro repositorio, lo que sigue es realizar la autenticacion con nuestro usuario y establecer la conexion con el repositorio para luego enviar la imagen.

[![Whats-App-Image-2022-09-05-at-3-20-06-AM.jpg](https://i.postimg.cc/QdLNCCv9/Whats-App-Image-2022-09-05-at-3-20-06-AM.jpg)](https://postimg.cc/svcC0jjs)

8. Una vez actualizado el repositorio con la imagen de la pagina estatica, lo que sigue es nuevamente establecer el deploy de los pods.
[![Whats-App-Image-2022-09-05-at-3-21-20-AM.jpg](https://i.postimg.cc/SKPm0KLx/Whats-App-Image-2022-09-05-at-3-21-20-AM.jpg)](https://postimg.cc/gw3C8mBC)

9. Tambien se debe modificar el archivo YAML que se genero en minikube, porque el archivo no especifica la ruta de la imagen que se ha establecido. Por ello, debemos definir la ruta completa en el campo de Image.
[![Whats-App-Image-2022-09-05-at-3-23-31-AM.jpg](https://i.postimg.cc/MHNP0vQg/Whats-App-Image-2022-09-05-at-3-23-31-AM.jpg)](https://postimg.cc/Mf0mqZtY)

10. De esta forma, se ha podido levantar los pods sin problemas, lo que ayudaria a poder establecer diferentes conexiones del sitio web. 

[![Whats-App-Image-2022-09-05-at-3-23-56-AM.jpg](https://i.postimg.cc/yNGLCDfx/Whats-App-Image-2022-09-05-at-3-23-56-AM.jpg)](https://postimg.cc/PLmWm5nn)

11. Despues establecido el grupo de contenedores, lo que sigue es levantar el servicio de balanceador de carga para poder realizar n conexiones junto a los pods generados.

[![Whats-App-Image-2022-09-05-at-3-24-47-AM.jpg](https://i.postimg.cc/GhMY5Zgh/Whats-App-Image-2022-09-05-at-3-24-47-AM.jpg)](https://postimg.cc/crnrK5NV)

12. A continuacion, se presenta el servicio establecido con los pods.

[![Whats-App-Image-2022-09-05-at-3-25-38-AM.jpg](https://i.postimg.cc/jq3S0tT9/Whats-App-Image-2022-09-05-at-3-25-38-AM.jpg)](https://postimg.cc/K4L2tXRr)

13. Al establecer el servcio de balanceador de carga, se puede realizar despues el levantamiento del mismo, lo cual puede generar diferentes paginas web estaticas en distintos servidores.

[![Whats-App-Image-2022-09-05-at-3-29-34-AM.jpg](https://i.postimg.cc/Ghc3ytQH/Whats-App-Image-2022-09-05-at-3-29-34-AM.jpg)](https://postimg.cc/0rBqg8hs)

## Failover  📌

* En la implementacion de un Failover, se debe hacer seguimiento al estado de los servidores puesto que si un servidor falla es necesario levantar otro para que asi el   numero de peticiones no sea sobrecargado para los otros servidores. En esta ocasion, a traves de minikube si observamos el dashboard podemos encontrar los servidores   activos.
![image](https://user-images.githubusercontent.com/58041699/188496714-397d6e8b-b954-43f2-97ac-fb3e02408078.png)

* En la siguiente imagen si un servidor se cae, existen otros dos que estaran ecuchando las peticiones de los usuarios. Puesto que el balanceador de carga, administra las peticiones, dandole al usuario los servicios que esten funcionando.

![image](https://user-images.githubusercontent.com/58041699/188496944-e760d7bd-d924-4619-87b3-d10ad0373edd.png)

* A continuacion, se puede observar los eventos sucedidos tras la caida de un servidor.

![image](https://user-images.githubusercontent.com/58041699/188497240-446917cc-edce-4e50-b881-29f39c72a284.png)

 * A traves de los comandos que ofrece la herramienta kubectl, podemos observar mensajes que describen el problema del servidor.

![image](https://user-images.githubusercontent.com/58041699/188497315-be5f2c1b-4b0c-4059-b66d-51192c51cf43.png)

* Aun con la caida del servidor, el servicio implementado en minikube puede seguir mostrandose ante los usuarios.

![image](https://user-images.githubusercontent.com/58041699/188497900-96deea7a-465e-42c3-b05b-f72be4adf636.png)

* Los servidores del sistema, si tratan con muchas peticiones pueden sobrecargarse los servidores y asi dañar los que restan. Para ello, se levantara un servidor de manera manual, lo que ayudara a distribuir las peticiones en ese servidor y asi facilitar el ingreso de usuarios.

![image](https://user-images.githubusercontent.com/58041699/188499473-830f4fcc-b3fa-4d60-ac15-85a44d98aa1f.png)

* A continuacion, se puede observar en el dashboard que se ha implementado un nuevo servidor y que esta activo.

![image](https://user-images.githubusercontent.com/58041699/188499610-fb933340-9f6d-4111-8075-7f84b24d2951.png)

* A su vez podemos comprobar que el servicio Balanceador de Carga esta haciendo uso de los 3 servidores.
![image](https://user-images.githubusercontent.com/58041699/188499764-4090576d-3103-4008-b135-3b84c3103faf.png)

## Conexiones activas  📌
Para la simulación de usuarios que ingresan al sistema se utilizo el gestor de pruebas JMeter.

* Lo primero es crear un plan de pruebas y colocarle un nombre.
* 
[![Captura-de-pantalla-2022-09-05-124756.png](https://i.postimg.cc/MKnNYx7V/Captura-de-pantalla-2022-09-05-124756.png)](https://postimg.cc/4mTWJDFx)

* Paso siguiente se genera un grupo de hilos el cual servirá para simular los usuarios que ingresan al sistema, se coloca el número de usuarios en este caso 100 y a la vez se coloca el intervalo de tiempo en el cual cada usuario se conecta en este caso 20 segundos

[![Captura-de-pantalla-2022-09-05-125134.png](https://i.postimg.cc/x8rFdNZR/Captura-de-pantalla-2022-09-05-125134.png)](https://postimg.cc/0MfCWjPM)

* Luego se configura el protocolo , el servidor central y el puerto donde se encuentra el servicio. 

[![Captura-de-pantalla-2022-09-05-125617.png](https://i.postimg.cc/Y2fJSbpL/Captura-de-pantalla-2022-09-05-125617.png)](https://postimg.cc/bdr6C0Sq)

* Luego se crea un arbol de de resultados donde se visualiza las peticiones que se estan realizando los usuarios.

[![Captura-de-pantalla-2022-09-05-132443.png](https://i.postimg.cc/13PZ9XVP/Captura-de-pantalla-2022-09-05-132443.png)](https://postimg.cc/jL81cshk)

* De igual forma se genera un gráfico donde se visualiza la subida y bajada del tiempo cuando han ingresado los usuarios al sistema.

[![Captura-de-pantalla-2022-09-05-132628.png](https://i.postimg.cc/7Z6bxK2q/Captura-de-pantalla-2022-09-05-132628.png)](https://postimg.cc/SJBmd7dP)

* Al detener un servidor se visualiza que la conexión continúa ejecutándose, gracias al balanceador de carga que redirecciona la petición a los otros servidores.

[![Captura-de-pantalla-2022-09-05-134528.png](https://i.postimg.cc/nzT6cG7t/Captura-de-pantalla-2022-09-05-134528.png)](https://postimg.cc/fttH8XBH)








