# CRUD_ARTICULOS
## `Connectio.php`:
El archivo connection.php cumple la función de establecer la conexión con la base de datos. En este caso, hemos utilizado phpMyAdmin para facilitar la conexión entre nuestra página web desarrollada en PHP y la base de datos. Para realizar esta conexión, primero especificamos el nombre del servidor, que en nuestro caso es "localhost". A continuación, proporcionamos el nombre de usuario que utilizará la conexión a la base de datos, seguido de la contraseña que valida nuestra autorización para acceder a la base de datos.
- Codigo `connection.php`:
```php
<?php
    $server = "localhost";
    $username = "root";
    $password = "";
    try {
        $conn = new PDO("mysql:host=$server;dbname=crudphp", $username, $password);
        $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        //echo "Connected successfully"; 
    } catch(PDOException $e) {
        echo "Connection failed: " . $e->getMessage();
    }
?>
```
## `Header.php`: 
El código proporciona la estructura inicial para un documento HTML. En el encabezado (head), se establecen configuraciones como el juego de caracteres y la vista móvil, junto con el título de la página. Se incluyen enlaces a las hojas de estilo de Bootstrap y Bootstrap Icons para aprovechar sus estilos y funcionalidades. En el cuerpo (body), se abre un contenedor principal con la clase container de Bootstrap, ofreciendo un diseño receptivo y ordenado para el contenido de la página. Este archivo se utiliza al principio de otras páginas para establecer una base HTML consistente, asegurando una apariencia moderna y adaptable gracias a Bootstrap.
- Codigo `Header.php`:
```php
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Bootstrap</title>
    <link rel="stylesheet" href="bootstrap-5.3.2-dist/css/bootstrap.min.css">
    <link rel="stylesheet" href="bootstrap-5.3.2-dist/bootstrap-icons-1.11.1/bootstrap-icons.min.css">
  </head>
  <body>
    <div class="container">
```
## `Footer.php`: 
Este código corresponde al cierre del contenedor principal (container) utilizado en el CRUD PHP. Además, se incluye un enlace al archivo JavaScript de Bootstrap (bootstrap.min.js) para proporcionar funcionalidades adicionales, como modales, alertas y otras características dinámicas de Bootstrap. Asegúrate de colocar este fragmento al final de tus archivos PHP del CRUD para asegurar una estructura HTML completa y la correcta inclusión de los recursos necesarios. Este footer complementa el encabezado (header.php) y juntos forman la estructura básica de los diferentes apartados en PHP.
- Codigo `Footer.php`:
```php
</div> <!--cierra la clase container-->
    <script src="bootstrap-5.3.2-dist/js/bootstrap.min.js"></script>
  </body>
</html>
```
