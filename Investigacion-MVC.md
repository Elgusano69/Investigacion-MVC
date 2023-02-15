
## Modelo Vista Controlador
En líneas generales, MVC es una propuesta de arquitectura del software utilizada para separar el código por sus distintas responsabilidades, manteniendo distintas capas que se encargan de hacer una tarea muy concreta, lo que ofrece beneficios diversos.
MVC se usa inicialmente en sistemas donde se requiere el uso de interfaces de usuario, aunque en la práctica el mismo patrón de arquitectura se puede utilizar para distintos tipos de aplicaciones. Surge de la necesidad de crear software más robusto con un ciclo de vida más adecuado, donde se potencie la facilidad de mantenimiento, reutilización del código y la separación de conceptos.
## Modelo de Representación de Datos
Son tecnicas para poder mostrar las diferentes representaciones graficas u otras metodo que existan para poder transmitir los diversos datos e informacion que tengamos. Unos Ejemplos serian:

- Diagrama Causa-efecto
- Diagrama de Flujo
- Histogaramas
- Diagrama de Pareto
- Diagrama de Controlador
- Diarama de dispersion
## Funcionalidades de Aplicaciones
Algunas de las Funcionalidades de ua aplicacion serian:

1. Usabiliad y versatilidad
2. Geolocalizacion
3. Analitica y centro de estadisticas
4. Persoalizacion
5. Integracion
6. Sincronizacion on y off-line
7. Autonomia
8. Adaptacion
9. Omnicanalidad
10. Seguridad
## Infraestructura para el almacenamiento y recuperación de datos
La infraestructura de almacenamiento de datos y recuperacion de datos existe demasiada informacion, por lo que es necesario que sea almacenada, protegida, optimzada y bien gestionada. Y para poder cumplir con este objetivo, la infraestructura se convierte en uno de los elementos esenciales de la arquitectura del almacenamiento. Y su estructura se podria representarse asi:
- Recopilacion de Datos
- Backuo automatiazo
- Recuperacion de informacion
- Datos ante desastres
## Ejemplo MVC
Aqui veremos ejemplo de un modelo MVC 
- El controlador
```
<?php
    require_once("../models/modelo.php");
    $services = new Service();
    $datos = $services->getServicios();
    require_once("../views/vista.php");
?>

```
-   Modelo
```
<?php

class Service {
    
    private $servicio;
    private $db;

    public function __construct() {
        $this->servicio = array();
        $this->db = new PDO('mysql:host=localhost;dbname=ejemplo_mvc', "root", "");
    }

    private function setNames() {
        return $this->db->query("SET NAMES 'utf8'");
    }

    public function getServicios() {

        self::setNames();
        $sql = "SELECT id, nombre, precio FROM servicio";
        foreach ($this->db->query($sql) as $res) {
            $this->servicio[] = $res;
        }
        return $this->servicio;
    }

    public function setServicio($nombre, $precio) {

        self::setNames();
        $sql = "INSERT INTO servicio(nombre, precio) VALUES ('" . $nombre . "', '" . $precio . "')";
        $result = $this->db->query($sql);

        if ($result) {
            return true;
        } else {
            return false;
        }
    }
}
?>
```
- Vista
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Ejemplo MVC con PHP</title>
        <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet" type="text/css" />
        <link href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet" >
        <script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
        <script type="text/javascript" src="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
    </head>
    <body>
        <div class="container">
            <header class="text-center">
                <h1>Ejemplo MVC con PHP</h1>
                <hr/>
                <p class="lead">Creamos una base de datos de los servicios <br/>
                    que podría realizar un taller y <br/>
                    operamos con ella utilizando el paradigma MVC</p>
            </header>
            <div class="col-lg-6 text-center">
                <hr/>
                <h3>Listado de servicios</h3>
                <table class="table">
                    <tr>
                        <td><strong>SERVICIO</strong></td>
                        <td><strong>PRECIO</strong></td>
                    </tr>
                    <?php
                    for ($i = 0; $i < count($datos); $i++) {
                        ?>
                        <tr>
                            <td><?php echo $datos[$i]["nombre"]; ?></td>
                            <td><?php echo $datos[$i]["precio"]; ?> €</td>
                        </tr>
                        <?php
                    }
                    ?>
                </table>
                <a href="../index.php"> <i class="fa fa-arrow-circle-left"></i> Volver a la página principal</a>
                <hr/>
            </div> 
            <footer class="col-lg-12 text-center">
                Adaweb - <?php echo date("Y"); ?>
            </footer>
        </div>
    </body>
</html>
```
- Index
```
<!DOCTYPE html>
<?php
if ((isset($_POST['nombre'])) && ($_POST['nombre'] != '') && (isset($_POST['precio'])) && ($_POST['precio'] != '')) {

    include "models/modelo.php";
    $nuevo = new Service();
    $asd = $nuevo->setServicio($_POST['nombre'], $_POST['precio']);
}
?>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Ejemplo MVC con PHP</title>
        <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet" type="text/css" />
        <link href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet" >
        <script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
        <script type="text/javascript" src="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
    </head>
    <body>
        <div class="container">
            <header class="text-center">
                <h1>Ejemplo MVC con PHP</h1>
                <hr/>
                <p class="lead">Creamos una base de datos de los servicios <br/>
                    que podría realizar un taller y <br/>
                    operamos con ella utilizando el paradigma MVC</p>
            </header>
            <div class="row">
                <div class="col-lg-6">

                    <form action="#" method="post" class="col-lg-5">
                        <h3>Nuevo servicio</h3>                
                        Nombre: <input type="text" name="nombre" class="form-control"/>    
                        Precio: <input type="text" name="precio" class="form-control"/>    
                        <br/>
                        <input type="submit" value="Crear" class="btn btn-success"/>
                    </form>
                </div>
                <div class="col-lg-6 text-center">
                    <hr/>
                    <h3>Listado de servicios</h3>
                    <a href="controllers/controlador.php"><i class="fa fa-align-justify"></i> Acceder al listado de servicios</a>
                    <hr/>
                </div> 
            </div>
            <footer class="col-lg-12 text-center">
                Adaweb - <?php echo date("Y"); ?>
            </footer>
        </div>
    </body>
</html>

```

## Link GetHub
Aqui esta el link de mi perfil de GetHub donde esta la investigacion:

https://github.com/Elgusano69
## Link GetHub
Aqui esta el link de mi perfil de GetHub donde esta la investigacion:

https://github.com/Elgusano69