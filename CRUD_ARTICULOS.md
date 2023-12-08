# CRUD_ARTICULOS
## `Connectio.php`:
El archivo connection.php cumple la función de establecer la conexión con la base de datos. En este caso, hemos utilizado phpMyAdmin para facilitar la conexión entre nuestra página web desarrollada en PHP y la base de datos. Para realizar esta conexión, primero especificamos el nombre del servidor, que en nuestro caso es "localhost". A continuación, proporcionamos el nombre de usuario que utilizará la conexión a la base de datos, seguido de la contraseña que valida nuestra autorización para acceder a la base de datos.
- Código `connection.php`:
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
- Código `header.php`:
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
- Código `footer.php`:
```php
</div> <!--cierra la clase container-->
    <script src="bootstrap-5.3.2-dist/js/bootstrap.min.js"></script>
  </body>
</html>
```
## `Create.php`
El código `createart.php` facilita la creación de nuevos artículos en la pagina web. Cuando se envía un formulario desde un modal, recopila los datos del artículo, incluida una imagen, y los inserta en la base de datos. Después de la operación, redirige al usuario a la página readart.php. El formulario incluye campos para nombre, descripción, costo, precio, cantidad e imagen del artículo.
- Código `createart.php`:
```php
<?php
include "connection.php";
if ($_POST) {
    $nombre = isset($_POST['nombre']) ? $_POST['nombre'] : "";
    $descripcion = isset($_POST['descripcion']) ? $_POST['descripcion'] : "";
    $costo = isset($_POST['costo']) ? $_POST['costo'] : "";
    $precio = isset($_POST['precio']) ? $_POST['precio'] : "";
    $cantidad = isset($_POST['cantidad']) ? $_POST['cantidad'] : "";

    $imagen_temporal = isset($_FILES['imagen']['tmp_name']) ? $_FILES['imagen']['tmp_name'] : "";
    $imagen_contenido = file_get_contents($imagen_temporal);

    $stmt = $conn->prepare("INSERT INTO articulos (nombre, descripcion, costo, precio, cantidad, imagen) VALUES (:nombre, :descripcion, :costo, :precio, :cantidad, :imagen)");
    $stmt->bindValue(":nombre", $nombre);
    $stmt->bindValue(":descripcion", $descripcion);
    $stmt->bindValue(":costo", $costo);
    $stmt->bindValue(":precio", $precio);
    $stmt->bindValue(":cantidad", $cantidad);
    $stmt->bindValue(":imagen", $imagen_contenido, PDO::PARAM_LOB);
    $stmt->execute();

    echo '<script type="text/javascript">';
    echo 'window.location.href = "readart.php";';
    echo '</script>';
}
?>

<!-- Modal para crear articulo -->
<div class="modal" id="createart" tabindex="-1" aria-labelledby="modallabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Nuevo articulo</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <form action="" method="post" enctype="multipart/form-data">
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre</label>
                        <input type="text" class="form-control" name="nombre" id="inputnombre" placeholder="Ingresa el nombre del producto" required>
                    </div>
                    <div class="mb-3">
                        <label for="descripcion" class="form-label">Descripcion</label>
                        <input type="text" class="form-control" name="descripcion" id="inputdescripcion" placeholder="Es un producto lacteo" required>
                    </div>
                    <div class="mb-3">
                        <label for="costo" class="form-label">Costo</label>
                        <input type="number" class="form-control" name="costo" id="inputcosto" placeholder="120.00" required>
                    </div>
                    <div class="mb-3">
                        <label for="precio" class="form-label">Precio</label>
                        <input type="number" class="form-control" name="precio" id="inputprecio" placeholder="140.00" required>
                    </div>
                    <div class="mb-3">
                        <label for="cantidad" class="form-label">Cantidad</label>
                        <input type="number" class="form-control" name="cantidad" id="inputcantidad" placeholder="50: piezas" required>
                    </div>
                    <div class="mb-3">
                        <label for="imagen" class="form-label">Imagen</label>
                        <input type="file" class="form-control" name="imagen" id="inputimagen" accept="image/*" required>
                    </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
                <button type="submit" class="btn btn-primary">Guardar</button>
            </div>
        </div>
        </form>
    </div>
</div>
```

  
