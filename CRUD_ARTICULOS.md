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
## `Read.php`
El código proporcionado es para el archivo `readart.php` que muestra la lista de artículos. Este archivo incluye un encabezado (header.php), establece una conexión a la base de datos, ejecuta una consulta para obtener los artículos, y luego muestra una tabla con la información de cada artículo, incluida una opción para actualizar o eliminar cada uno. También incluye un modal (createart.php) para agregar nuevos artículos y utiliza un script JavaScript para confirmar la eliminación de un artículo. Finalmente, incluye el pie de página (footer.php).
- Código `readart.php`:
```php
<?php
include "header.php";
include "connection.php";
$stmt = $conn->prepare("SELECT * FROM articulos");
$stmt->execute();
$results = $stmt->fetchAll(PDO::FETCH_ASSOC);
?>

<div class="row">
  <div class="col-md-8">
    <h1>Articulos</h1>
  </div>
  <div class="col-md-2">
    <h1>
    <a href="index.php" class="btn btn-info">
      <i class="bi bi-house-up-fill"></i> Principal
    </a>
    </h1>
  </div>
  <div class="col-md-2">
    <h1>
      <button type="button" class="btn btn-primary data" data-bs-toggle="modal" data-bs-target="#createart">
        <i class="bi bi-database-add"></i> Nuevo Articulo
      </button>
    </h1>
  </div>
</div>
<table class="table table-bordered table-striped">
  <thead>
    <tr>
      <th width="20">ID</th>
      <th width="200">Nombre</th>
      <th width="400">Descripcion</th>
      <th width="20">Costo</th>
      <th width="20">Precio</th>
      <th width="20">Cantidad</th>
      <th width="200">Imagen</th>
      <th width="100">Acciones</th>
    </tr>
  </thead>
  <tbody>
    <?php
    foreach ($results as $result) {
      ?>
      <tr>
        <td>
          <?php echo $result['id']; ?>
        </td>
        <td>
          <?php echo $result['nombre']; ?>
        </td>
        <td>
          <?php echo $result['descripcion']; ?>
        </td>
        <td>
          <?php echo $result['costo']; ?>
        </td>
        <td>
          <?php echo $result['precio']; ?>
        </td>
        <td>
          <?php echo $result['cantidad']; ?>
        </td>
        <td>
          <?php
          if (!empty($result['imagen'])) {
            // La LINEA 48 ES LA SIGUIENTE
            echo '<img src="data:image/jpeg;base64,' . base64_encode($result['imagen']) . '" alt="Imagen del artículo" style="max-width: 200px; max-height: 200px;">';
          } else {
            echo 'No se encontró la imagen';
          }
          ?>
        </td>
        <td>
          <a href="updateart.php?id=<?php echo $result['id']; ?>" class="btn btn-warning btn-sm"><i
              class="bi bi-pencil-fill"></i></a>
          <a onclick="return confirm_delete()" href="deleteart.php?id=<?php echo $result['id']; ?>"
            class="btn btn-danger btn-sm">
            <i class="bi bi-trash-fill"></i></a>
        </td>
      </tr>
    <?php } ?>
    <script type="text/javascript">
      function confirm_delete() {
        return window.confirm('¿Estás seguro de eliminar el siguiente artículo?');
      }
    </script>
  </tbody>
</table>
<?php
include "createart.php";
include "footer.php";
?>
```
## `Update.php`
El segmento de codigo de updateart.php, que se encarga de actualizar la información de un artículo en la base de datos. El código incluye la recuperación de la información actual del artículo, la ejecución de la actualización y la presentación de mensajes de éxito o error. También contiene un formulario para ingresar la nueva información del artículo.
- Código `updateart.php`:
```php
<?php
include "connection.php";

if (isset($_GET['id'])) {
    $id = (isset($_GET['id']) ? $_GET['id'] : '');
    $stmt = $conn->prepare("SELECT * FROM articulos WHERE id=:id");
    $stmt->bindValue(":id", $id);
    $stmt->execute();
    $result = $stmt->fetch(PDO::FETCH_LAZY);
    $nombre = $result['nombre'];
    $descripcion = $result['descripcion'];
    $costo = $result['costo'];
    $precio = $result['precio'];
    $cantidad = $result['cantidad'];
    $imagen = $result['imagen'];
}

if ($_POST) {
    $id = (isset($_POST['id']) ? $_POST['id'] : '');
    $nombre = (isset($_POST['nombre']) ? $_POST['nombre'] : '');
    $descripcion = (isset($_POST['descripcion']) ? $_POST['descripcion'] : '');
    $costo = (isset($_POST['costo']) ? $_POST['costo'] : '');
    $precio = (isset($_POST['precio']) ? $_POST['precio'] : '');
    $cantidad = (isset($_POST['cantidad']) ? $_POST['cantidad'] : '');
    
    $imagen_temporal = isset($_FILES['imagen']['tmp_name']) ? $_FILES['imagen']['tmp_name'] : "";
    $imagen_contenido = file_get_contents($imagen_temporal);

    $stmt = $conn->prepare("UPDATE `articulos` SET `id`=:id, `nombre`=:nombre, `descripcion`=:descripcion, `costo`=:costo, 
        `precio`=:precio, `cantidad`=:cantidad ,`imagen`=:imagen WHERE `articulos`.`id`=:id");
    $stmt->bindValue(":id", $id);
    $stmt->bindValue(":nombre", $nombre);
    $stmt->bindValue(":descripcion", $descripcion);
    $stmt->bindValue(":costo", $costo);
    $stmt->bindValue(":precio", $precio);
    $stmt->bindValue(":cantidad", $cantidad);
    $stmt->bindValue(":imagen", $imagen_contenido, PDO::PARAM_LOB);
    $stmt->execute();
    if ($stmt) {
?>
        <div class="alert alert-succes alert-dismissible fade show" role="altert">
            <strong>Correcto!</strong> Se actualizo el cliente.

            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>
    <?php
        header('location: readart.php');
    } else {
    ?>
        <div class="alert alert-danger alert-dismissible fade show" role="alert">
            <strong>Error!</strong> No se pudo actualizar el cliente.
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>
<?php

    }
}
include "header.php";
?>

<!-- Formulario de actualizar articulos -->
<div class="row">
    <div class="col-md-10">
        <h2>Actualizar Articulos</h2>
    </div>
    <form action="" method="post" enctype="multipart/form-data">
        <input type="hidden" id="id" name="id" value="<?php echo $id; ?>">
        <div class="mb-3">
            <label for="nombre" class="form-label">Nombre</label>
            <input type="text" class="form-control" name="nombre" id="inputnombre" placeholder="Ingresa el nombre del producto" required value="<?php echo $nombre ?>">
        </div>
        <div class="mb-3">
            <label for="descripcion" class="form-label">Descripcion</label>
            <input type="descripcion" class="form-control" name="descripcion" id="inputdescripcion" placeholder="Es un producto lacteo" required value="<?php echo $descripcion ?>">
        </div>
        <div class="mb-3">
            <label for="costo" class="form-label">Costo</label>
            <input type="number" class="form-control" name="costo" id="inputcosto" placeholder="120.00" required value="<?php echo $costo ?>">
        </div>
        <div class="mb-3">
            <label for="precio" class="form-label">Precio</label>
            <input type="number" class="form-control" name="precio" id="inputprecio" placeholder="140.00" required value="<?php echo $precio ?>">
        </div>
        <div class="mb-3">
            <label for="cantidad" class="form-label">Cantidad</label>
            <input type="number" class="form-control" name="cantidad" id="inputcantidad" placeholder="50: pzs" required value="<?php echo $cantidad ?>">
        </div>
        <div class="mb-3">
            <label for="imagen" class="form-label">Imagen</label>
            <input type="file" class="form-control" name="imagen" id="inputimagen" accept="image/*" required>
        </div>
        <button type="submit" class="btn btn-primary">Modificar</button>
    </form>
</div>
<?php
include "footer.php";
?>
```
## `Delete.php`
El código deleteart.php, que se encarga de eliminar un artículo de la base de datos utilizando el ID que es proporcionado a través de la URL. El código utiliza la sentencia DELETE de SQL para eliminar el registro y luego redirige a la página de lectura de artículos (readart.php). Este proceso garantiza que los datos sean eliminados correctamente (solo un dato).
- Código `deleteart.php`:
```php
<?php
include "connection.php";
if (isset($_GET['id'])) {
    $id=(isset($_GET['id']) ? $_GET['id'] : "");
    $stmt = $conn->prepare("DELETE FROM articulos WHERE id=:id");
    $stmt->bindParam(":id",$id);
    $stmt->execute();
    header('location: readart.php');
}
?>
```
