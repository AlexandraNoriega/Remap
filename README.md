# Remap

REMAP

==============

En este tutorial se mostrará como se modificó el repositorio de RBX1 para dirigir la Tutlebot dentro de rviz con el teclado dentro del terminal (parámetros de velocidad lineal y angular). Lo primero que haremos es descargar el repositorio RBX1 para ello dentro del src de nuestro workspace ejecutamos


```
git clone https://github.com/pirobot/rbx1.git 
```

Descargamos el paquete de esta forma con el fin de saber la ubicación de los archivos que contiene el paquete y así poder editarlos. Ahora en caso de no tener las dependencias de ubuntu las descargamos

```
sudo apt-get install python-rosinstall
```

Una vez tenemos la copia del repositorio en nuestro workspace, lo compilamos para luego poder ejecutarlo desde nuestro terminal

```
cd <workspace>
catkin_make
```

**Cuando escribamos el remap debemos saber que topic se enlazaran** para ello corremos 

```
roscore
roslaunch rbx1_bringup fake_turtlebot.launch
rostopic list
```

Ahora corremos el nodo de teleop para revisar que topic debemos enlazar entre los comandos que recibe el terminal y el que recibe el paquete

```
rosrun turtlesim turtle_teleop_key
rostopic list
```

Una vez confirmado nos dirigimos al launch que ejecutamos, *workspace/src/rbx1//rbx1_bringup/launch/fake_turtlebot.launch* y lo abrimos como gedit, allí nos ubicamos en la zona de arbotix y antes de cerrar el node ponemos el comando remap del topic que queremos enlazar con el que controlamos con el terminal

```
  <node name="arbotix" pkg="arbotix_python" type="arbotix_driver" output="screen" clear_params="true">
      <rosparam file="$(find rbx1_bringup)/config/fake_turtlebot_arbotix.yaml" command="load" />
      <param name="sim" value="true"/>
      <remap from="/cmd_vel" to="/turtle1/cmd_vel"/>
  </node>
```

Ahora como se modifico el launch debemos matar el roslaunch ya ejecutado y volver a correrlo para que reciba los cambios. Podremos observar que se comunica el terminal con el rviz.

**Personalización**
Si quiero modificar la velocidad que envía el teleop, mato el teleop y ejecuto

```
rosparam set teleop_turtle/scale_linear 0.5
```

Vulevo a ejecutar y observo los cambios.
