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

