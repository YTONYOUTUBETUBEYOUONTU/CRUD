# CRUD_CLIENTES
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
## `Create.php`
En el apartado create.php, se gestiona la creación de nuevos clientes a través de un formulario modal en una página web creada con PHP. En este apartado se recogen los datos del formulario, como nombre, email, teléfono y dirección, y los inserta de manera segura en la base de datos al utilizar el metodo POST y consultas preparadas (`prepare`). Además, incorpora scripts (JavaScript) para mejorar la experiencia del usuario al poner el formato adecuado en el número de teléfono y validar el campo correspondiente. Este enfoque permite una entrada de datos más eficiente y segura. Al completar el formulario, redirige al usuario a la página de lectura (read.php) para visualizar los clientes, incluyendo el nuevo registro creado.
- Codigo `Create.php`:
```php
<?php
 include "connection.php";
 if($_POST){
  $nombre = (isset($_POST["nombre"])) ? $_POST["nombre"] :"";
  $email = (isset($_POST["email"])) ? $_POST["email"] :"";
  $telefono = (isset($_POST["telefono"])) ? $_POST["telefono"] :"";
  $direccion = (isset($_POST["direccion"])) ? $_POST["direccion"] :"";
  $stmt = $conn->prepare("INSERT INTO clientes(id, nombre, email, telefono, direccion) VALUES(null,:nombre,:email,:telefono,:direccion)");
  $stmt->bindValue(":nombre", $nombre);
  $stmt->bindValue(":email", $email);
  $stmt->bindValue(":telefono", $telefono);
  $stmt->bindValue(":direccion", $direccion);
  $stmt->execute();
  header("location:read.php");
}
?>
<!--Modal para crear cliente-->
<div class="modal" id="create" tabindex="-1" arial-labelledby="modallabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalLabel">Nuevo Cliente</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <form action="" method="post">
          <div class="mb-3">
            <label for="nombre" class="form-label">Nombre</label>
            <input type="text" class="form-control" name="nombre" id="inputnombre" placeholder="Nombre Example"
              required>
          </div>

          <div class="mb-3">
            <label for="Email" class="form-label">Email</label>
            <input type="email" class="form-control" name="email" id="inputemail" placeholder="Example@mail.com"
              required>
          </div>

          <div class="mb-3">
            <label for="inputTelefono" class="form-label">Teléfono</label>
            <input type="tel" class="form-control" name="telefono" id="inputTelefono" placeholder="123-456-7890"
              oninput="formatPhoneNumber(this)" onkeypress="return onlyNumbers(event)"
              pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}" title="Formato válido: 123-456-7890">
            <small id="telHelp" class="form-text text-muted">Ingrese el teléfono en el formato 123-456-7890.</small>
          </div>

          <div class="mb-3">
            <label for="direccion" class="form-label">Direccion</label>
            <input type="text" class="form-control" name="direccion" id="inputdireccion"
              placeholder="Ingresa tu dirección" required>
          </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="submit" class="btn btn-primary">Guardar</button>
      </div>
      </form>
    </div>
  </div>
</div>

<script>
  function formatPhoneNumber(input) {
    let phoneNumber = input.value.replace(/\D/g, ''); // Eliminar no números

    if (phoneNumber.length > 10) {
      phoneNumber = phoneNumber.slice(0, 10);
    }

    if (phoneNumber.length >= 3 && phoneNumber.length < 7) {
      phoneNumber = phoneNumber.slice(0, 3) + '-' + phoneNumber.slice(3);
    } else if (phoneNumber.length >= 7) {
      phoneNumber = phoneNumber.slice(0, 3) + '-' + phoneNumber.slice(3, 6) + '-' + phoneNumber.slice(6, 10);
    }

    input.value = phoneNumber;
  }

  function onlyNumbersAndHyphen(event) {
    const charCode = event.which ? event.which : event.keyCode;

    if ((charCode >= 48 && charCode <= 57) || charCode === 8 || charCode === 189) {
      return true; // Permitir números, retroceso y guiones
    } else {
      event.preventDefault();
      return false;
    }
  }
</script>
```
## `Read.php`:
El archivo read.php se encarga de mostrar la información de los clientes almacenados en la base de datos, el codigo del archivo `read.php` se encarga de mostrar una tabla con la información de todos los clientes almacenados en la base de datos. Además, proporciona botones de acción para editar y eliminar cada cliente; También incluye un formulario modal para agregar nuevos clientes (create.php)
- Codigo `Read.php`:
```php
<?php
include "header.php";
include "connection.php";

$stmt = $conn->prepare("SELECT * FROM clientes");
$stmt->execute();
$results = $stmt->fetchAll(PDO::FETCH_ASSOC);
?>
<!-- Contenido -->
<div class="row">
    <div class="col-md-8">
        <h1>Clientes</h1>
    </div>
    <div class="col-md-4 text-right">
        <h1>
            <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#create">
                <i class="bi bi-person-fill-add"></i> Nuevo
            </button>
            <a href="index.php" class="btn btn-info">
                <i class="bi bi-house-up-fill"></i> Pagina Principal
            </a>
        </h1>
    </div>
</div>

<table class="table table-bordered table-striped">
	<thead>
		<tr>
			<th width="20">id</th>
			<th>nombre</th>
			<th>email</th>
			<th>telefono</th>
			<th>direccion</th>
			<th width="100">Acciones</th>
		</tr>
	</thead>
	<tbody>
		<?php foreach ($results as $result) { ?>
			<tr>
				<td>
					<?php echo $result['id']; ?>
				</td>
				<td>
					<?php echo $result['nombre']; ?>
				</td>
				<td>
					<?php echo $result['email']; ?>
				</td>
				<td>
					<?php echo $result['telefono']; ?>
				</td>
				<td>
					<?php echo $result['direccion']; ?>
				</td>
				<td>

					<a href="update.php?id=<?php echo $result['id']; ?>" class="btn btn-warning btn-sm"><i
							class="bi bi-pencil-fill"></i></a>
					<a onclick="return confirm_delete()" href="delete.php?id=<?php echo $result['id']; ?>"
						class="btn btn-danger btn-sm"><i class="bi bi-trash"></i></a>

					<!--<a href="update.php?id=  ?php echo $result['id']; ?>" class="btn btn-warning btn-sm"><i
							class="bi bi-pencil-fill"></i></a>
							
					 Botón de editar con ícono -->
					<!--<button type="button" class="btn btn-warning btn-sm">
					<i class="bi bi-pencil-fill"></i>
					</button>-->


					<!-- Botón de eliminar con ícono 
					<button type="button" class="btn btn-danger btn-sm">
						<i class="bi bi-trash"></i>
					</button>-->
				</td>
			</tr>
		<?php } ?>
	</tbody>
</table>
<script type="text/javascript">
	function confirm_delete() {
		return confirm('Esta seguro de eliminarlo?')
	}
</script>
<?php
include "footer.php";
include "create.php";
?>
```
## `Update.php`
El codigo update.php se encarga de actualizar la información de un cliente en la base de datos. Primero, se verifica si se ha proporcionado un parámetro id a través de la URL. En caso afirmativo, se realiza una consulta para obtener los detalles del cliente correspondiente a ese ID. Luego, se muestra un formulario prellenado con la información actual del cliente.

Se envía el formulario mediante el método POST, se recogen los datos actualizados y se ejecuta una consulta de actualización en la base de datos. Dependiendo del resultado de la operación, se muestra un mensaje (alert) de éxito: se actualizó el cliente o error: no se puedo actualizar el cliente.
- Codigo `Update.php`:
```php
<?php
include "connection.php";
if (isset($_GET['id'])) {
    $id = (isset($_GET['id']) ? $_GET['id'] : "");
    $stmt = $conn->prepare("SELECT * FROM clientes WHERE id = :id");
    $stmt->bindValue(":id", $id, PDO::PARAM_INT);
    $stmt->execute();
    $result = $stmt->fetch(PDO::FETCH_ASSOC);
    $nombre = $result['nombre'];
    $email = $result['email'];
    $telefono = $result['telefono'];
    $direccion = $result['direccion'];
}
if ($_POST) {
    $id = (isset($_POST['id']) ? $_POST['id'] : "");
    $nombre = (isset($_POST['nombre']) ? $_POST['nombre'] : "");
    $email = (isset($_POST['email']) ? $_POST['email'] : "");
    $telefono = (isset($_POST['telefono']) ? $_POST['telefono'] : "");
    $direccion = (isset($_POST['direccion']) ? $_POST['direccion'] : "");

    $stmt = $conn->prepare("UPDATE clientes SET nombre = :nombre, email = :email, direccion = :direccion, telefono = :telefono WHERE id = :id");
    $stmt->bindValue(':id', $id);
    $stmt->bindValue(':nombre', $nombre);
    $stmt->bindValue(':email', $email);
    $stmt->bindValue(':telefono', $telefono);
    $stmt->bindValue(':direccion', $direccion);
    $stmt->execute();
    if ($stmt) {
        ?>
        <div class="alert alert-success alert-dismissible fade show" role="alert">
            <strong>Correcto!</strong> Se actualizó el cliente
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>

        <?php
    } else {
        ?>
        <div class="alert alert-danger alert-dismissible fade show" role="alert">
            <strong>Error!</strong> No se pudo actualizar el cliente
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>
        <?php
    }
}
include "header.php";
?>
<!-- Formulario de actualizar cliente -->
<div class="row">
    <div class="col-m10">
        <h2>ACTUALIZAR CLIENTE</h2>
    </div>
    <form action="" method="post">
        <input type="hidden" id="id" name="id" value="<?php echo $id; ?>">
        <div class="mb-3">
            <label for="nombre" class="form-label">Nombre</label>
            <input type="text" class="form-control" name="nombre" id="inputnombre" required
                value="<?php echo $nombre ?>">
        </div>

        <div class="mb-3">
            <label for="Email" class="form-label">Email</label>
            <input type="email" class="form-control" name="email" id="inputemail" required value="<?php echo $email ?>">
        </div>

        <div class="mb-3">
            <label for="inputTelefono" class="form-label">Teléfono</label>
            <input type="tel" class="form-control" name="telefono" id="inputTelefono" placeholder="123-456-7890"
                oninput="formatPhoneNumber(this)" onkeypress="return onlyNumbers(event)"
                pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}" title="Formato válido: 123-456-7890" required
                value="<?php echo $telefono ?>">
            <small id="telHelp" class="form-text text-muted">Ingrese el teléfono en el formato 123-456-7890.</small>
        </div>

        <div class="mb-3">
            <label for="direccion" class="form-label">Direccion</label>
            <input type="text" class="form-control" name="direccion" id="inputdireccion"
                placeholder="Ingresa tu dirección" required value="<?php echo $direccion ?>">
        </div>
        <button type="submit" class="btn btn-primary">Guardar</button>
            <a href="read.php" class="btn btn-info">
                <i class="bi bi-house-up-fill"></i> Agregar Clientes
            </a>
    </form>
</div>
<?php
include "footer.php"
    ?>
```
## `Delete.php`
El apartado de delete.php se encarga de eliminar un cliente de la base de datos. Se tiene que hacer clic en el icono que aparece al lado derecho del cliente, se verifica si se ha proporcionado un parámetro id a través de la URL (de forma automática). Si este parámetro está presente, se prepara una declaración SQL para eliminar el cliente correspondiente a ese ID. Se utiliza la sentencia DELETE FROM clientes WHERE id =:id para eliminar la fila específica de la tabla de clientes.
- Codigo `Delete.php`:
```php
<?php
    include "connection.php";
    if (isset($_GET['id'])){
        $id = (isset($_GET['id'])?$_GET['id']:"");
        $stmt = $conn->prepare("DELETE FROM clientes WHERE id =:id");
        $stmt -> bindParam(':id', $id);
        $stmt ->execute();
        header ('location: read.php');
    }
?>
```
