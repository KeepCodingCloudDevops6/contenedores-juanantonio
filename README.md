Contedores Dcoker y Kubernetes Juan Antonio

En está practica vamos a implementar una aplicación que consiste en un microservicio que es capaz de leer y escribir en una base de datos

Para está practica es necesario tener instalado el sistema operativo linux en cualquiera de su variantes o bien tener una maquina virtual de las mismas caracteristicas. Para ello lo primero es instalar virtualbox para despues poder instalarte una maquina virtual.

Necesario tener instalado git para poder clanar el repositorio con el comando gitclone https://github.com/KeepCodingCloudDevops6/Contendores-JuanAntonio.git

Primera parte:

1- Instalar docker y docker-compose. Dichas apliaciones se pueden instalar en la pagina oficina de docker: https://docs.docker.com/. Para instalar docker compose tambien desde la pagina oficina de docker donde apareceran todas las intrucciones https://docs.docker.com/compose/install/.

2- Una vez instalado, lo primero será crear una imagen, que se llamara dockerfile, utilizamos multistage para que nos instale todas las aplicaciones necesarias en este caso vamos a desplegar dos bases de datos, la primera se ha hecho con redis y la segunda con mongo y sea menos pesada, por lo que irá más fluida. En nuestro caso la imagen ocupa tan solo ocupa 52.9 megas, si la hubiera hecho sin multistage, El nos hubiera ocupado el doble. Para generar la imagen lo hacemos con el comando docker build -t (el nombre de la imagen que queramos poner) y despues un punto. Para ver si se ha creado la imagen docker image ls
El siguiente paso seŕa preparar la imagen para poder subirla a docker hud, para ello, lo primero será registrarnos en dockerhud, logarnos con docker login en nuestro terminal, introducir usuario y contraseña utilizado en el registro y despues poner docker tag nombredeusuario/nombredelrepositio/etiqueta. Despues pondremos docker push y nombre de la imagen y la subiremos a docker hud.
Despues y ya terminariamos con la parte de docker lo que tendremos que hacer es generar el contenedor para que instale nuestra aplicación. El archivo se tendrá que llamar docker-compose.yml. Para lanzarlo una vez creado, tendremos que lanzarlo con el comando docker-compose up y ya podremos acceder a localhost:8000 por cualquier navegador donde aparecerá un testo, según puedes comprobar en la captura.

Segunda parte:

1- Instalar kubernetes tambien se utiliza para gestionar contenedores. Lo primero de todo es instalar minikube para poder trabajar con kubernetes en entorno local https://minikube.sigs.k8s.io/docs/start/. Una vez instalado tendrémos que instala kubectl, para ello https://kubernetes.io/es/docs/tasks/tools/included/install-kubectl-linux/

2- Creamos un fichero mongo-config-yaml, este archivo permite almacenar la configuración para que la usen otros objetos de manera encriptado.

3. Cramos el archivo mongo.yaml. Este archivo va a ser un archivo de configuración donde vamos a crear el deployment y service para mongo.db
El apartado template en este archivo, es una configuración de la parte dentro de la configuración del deployment, en la que template tiene su porpia metadata y especificaciones.

El apartado environment, pasamos estas variables de entorno a la aplicación monogo. Se crea una lista de variables de entorno con nombres y valores


3- Creamos el archivo mongo-service.yaml: Creamos este tipo de fichero por que la aplicación, necesita diferentes servicios para que se pueda ejecutar en kubernetes

4. Creamos un archivo webapp.yaml. Este fichero consiste crear una imagen para nuestra aplicación y configura los deployment y los service.

5. Para finalizar vamos a generar todos estos archivos con los siguientes comandos:
kubectl get pod         
		kubectl apply -f mongo-config.yaml         
		kubectl apply -f mongo-secret.yaml		  
		kubectl apply -f mongo.yaml					 
		kubectl apply -f webapp.yaml        			

Comprobamos que se ha generado correctamente con el comando get config map, kubectl get secret y para terminal con el comando kubectl get pod.
Para añadir la opción de autoescalado. Está opción lo que hace es si nuestro contenedor supera 0% de su capacidad, como en este caso, se crearía una réplica, para evitar que colapse, asegurando su funcionamiento. Esto se conoce como alta disponibilidad(high availability). Esto lo hace hasta un máximo de 10 veces, como se especifica en el fichero:![autoscaling](https://user-images.githubusercontent.com/86802349/201473887-39c4c38a-233e-43aa-bbf9-9b4afc2a78c0.JPG)

Una vez viendo que funciona, se crea el fichero ingress.yaml


En Host se puede ver el FQDN, para resolver hacia la aplicación. Para que nuestro ordenador lo entienda, hay que modificar el fichero /etc/hosts, de esta manera, va a resolver la url hacia nuestra aplicación, consiguiendo el mmismo efecto que el port-forward, pero ya directamente.
