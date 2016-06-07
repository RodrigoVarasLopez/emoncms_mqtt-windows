# emoncms_mqtt-windows
Emoncms using package phpmqtt_input.php configuration

###1. instalamos todos los componentes necesarios siguiendo la instalación emoncms:
https://github.com/emoncms/emoncms/blob/master/readme.md

En nuestro servidor apache(www o httdocs) en vez de utilizar el descrgable de emoncms nos bajamos el que he configurado:
https://github.com/RodrigoVarasLopez/emoncms_mqtt-windows/blob/master/produccion/emoncms.zip


**WARNING: ESTA PARTE YA ESTA ECHA DENTRO DE MI FICHERO DESCARGABLE, SOLAMENTE ES INFORMATIVA!!:**

Los cambios realizados dentro de la carpeta **emoncms/settings.php** son los sigueintes muy parecedios a los de la guia salvo MQTT:

**emoncms/settings.php**
```
//1 #### Mysql database settings 
    $server   = "localhost";
    $database = "emoncms";
    $username = "rov";
    $password = "123456";
    $port     = "3306";
```   

```
//3 #### MQTT
    $mqtt_enabled = true;          // Activate MQTT by changing to true
    $mqtt_server = array( 'host'     => 'localhost',
                          'port'     => 1883,
                          'user'     => '',
                          'password' => '',
                          'basetopic'=> 'emon'
                          );
```

```
        'phpfiwa'=>array(
            'datadir' => "C:\\Users\\RODRIGO\\emoncmsdata\\phpfiwa\\"
        ),
        'phpfina'=>array(
            'datadir' => "C:\\Users\\RODRIGO\\emoncmsdata\\phpfina\\"
        ),
        'phptimeseries'=>array(
            'datadir' => "C:\\Users\\RODRIGO\\emoncmsdata\\phptimeseries\\"
        )
    );
```

```
$log_filename = "C:\\Users\\RODRIGO\\emoncmsdata\\emoncms.log";
```

Cambios en **emoncms/scripts/phpmqttsettings.php**:

**emoncms/scripts/phpmqttsettings.php**
```
//basetopic es el topic donde se va subscribir el que quiera publicar 
 $mqttsettings = array(
        'userid' => 1,
        'basetopic' => "nodes"
    );
```

```
//Cambiamos el fichero phpmqtt_imput.lock a una ruta de windows con permisos de escritura
linea 43: $fp = fopen("C:\Users\RODRIGO\\emoncmsdata\phpmqtt_input.lock", "w");
```

**END WARING**

### 2. Instalar y Ejecutar mosquitto también en el lado de emoncms
**http://mosquitto.org/download/**

### 3.Ejecutamos el script **emoncms/scripts/phpmqttsettings.php** que va a hacer de subscriptor 
php phpmqttsettings.php

### 4. Nos conectamos con mosquitto_pub 
mosquitto_pub -t "nodes/emontx/power" -m "10" 

* nodes es el basetopic
* emontx el nodo 
* power el key 
* 10 el value

