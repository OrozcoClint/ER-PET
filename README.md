# ER-PET
Repositorio de trabajo de ERP para clínica veterinaria

## CARPETAS Y FICHEROS

### /config (Configuración Global)
Almacena los archivos de parametrización del entorno.

* database.php: Contiene las credenciales de acceso al servidor de base de datos MySQL (host, usuario, contraseña, nombre de la BD y puerto) y la configuración de atributos de PDO (modos de error, emulación de prepared statements y juego de caracteres UTF-8).

* app.php: Define constantes globales de la aplicación, como la URL base del proyecto, zonas horarias, modos de depuración (debug mode) y rutas absolutas del sistema.

### /database (Persistencia y Scripts SQL)
Agrupa los scripts DDL y DML para la inicialización de la base de datos.

* schema.sql: Script de definición de datos (DDL). Contiene las sentencias CREATE TABLE, definiciones de claves primarias (PRIMARY KEY), claves foráneas (FOREIGN KEY), restricciones de integridad (UNIQUE, CHECK) e índices para la construcción del modelo relacional.

* seeds.sql: Script de datos iniciales (DML). Contiene las sentencias INSERT para poblar la base de datos con información de prueba (roles predefinidos, usuarios administradores con contraseñas haseadas, clientes de prueba, catálogo inicial de medicamentos y tipos de servicios).

### /public (Punto de Entrada Público / Document Root)
Es el único directorio expuesto al servidor web Apache/Nginx. Ningún archivo fuera de esta carpeta es accesible mediante peticiones HTTP directas.

* index.php (Front Controller): Punto de entrada único a la aplicación. Se encarga de inicializar el entorno, registrar el autoloader de clases, arrancar la sesión de usuario (session_start()), instanciar el enrutador y despachar la petición HTTP al controlador adecuado.

* .htaccess: Fichero de configuración del módulo mod_rewrite de Apache. Intercepta todas las peticiones a la aplicación y las redirige hacia public/index.php, preservando la compatibilidad con archivos estáticos existentes (CSS, JS, imágenes).

* css/: Contiene los archivos de hojas de estilo (CSS3 / Frameworks) para el maquetado del panel de administración.

* js/: Contiene la lógica del cliente (JavaScript / AJAX) para la interacción dinámica sin recarga de página (validaciones en cliente, peticiones asíncronas).

* assets/: Almacena imágenes estáticas, logotipos de la clínica, fuentes tipográficas e iconos.

### /src (Código Fuente Privado de la Aplicación)
Contiene la lógica de negocio, controladores, modelos y vistas del sistema. Queda totalmente aislado del acceso web público.

#### /src/Controllers (Capa Controlador):
Clases encargadas de recibir la petición HTTP, procesar y sanitizar los parámetros de entrada ($_GET, $_POST), invocar a los modelos correspondientes y pasar los datos a la vista.

Ejemplos: AuthController.php, FacturaController.php, ClienteController.php, InventarioController.php.

#### /src/Models (Capa Modelo):
Clases orientadas a objetos encargadas de la lógica de datos, reglas de negocio y persistencia en MySQL mediante PDO con Prepared Statements.

Ejemplos: UsuarioModel.php, FacturaModel.php, MascotaModel.php, ProductoModel.php.

#### /src/Views (Capa Vista):
Plantillas PHP/HTML dedicadas exclusivamente a la presentación de la interfaz. Organizadas en subcarpetas modulares para permitir el desarrollo en paralelo sin conflictos de Git:

* Views/layouts/: Estructuras base (cabeceras, menús laterales, pie de página).

* Views/auth/: Formulario de login, recuperación de contraseña y gestión de perfil.

* Views/facturas/: Formularios de emisión de facturas, listado de cobros y plantillas de impresión.

* Views/clientes/: Fichas de propietarios, historiales clínicos y mascotas (Módulo Compañero 1).

* Views/inventario/: Catálogo de productos, alertas de stock y calendario de citas (Módulo Compañero 2).

#### /src/Core (Núcleo Framework Nativo):
Clases base e infraestructura del sistema:

* Router.php: Parsea la URL entrante, analiza el método HTTP (GET/POST) y ejecuta la acción asociada en el controlador correspondiente.

* Database.php: Implementación del patrón Singleton sobre la extensión PDO para garantizar una única conexión reutilizable a la base de datos durante el ciclo de vida de la petición.

* Controller.php: Controlador base del cual heredan todos los controladores. Ofrece métodos auxiliares para renderizar vistas (render()) y ejecutar redirecciones HTTP (redirect()).

* AuthMiddleware.php: Filtro de seguridad que intercepta las peticiones y comprueba la validez de la sesión activa y los permisos del rol (RBAC) antes de permitir la ejecución de un controlador.

### /docs
Contiene documentos técnicos y de gestión del proyecto.

* Convenciones de codificación (PSR-12), nombrado de variables, tablas y estándares de desarrollo para el equipo.

### Archivos Raíz
* .gitignore: Fichero de configuración de Git que especifica qué archivos o carpetas no deben rastrearse en el repositorio (ej. config/database.php, carpetas de subidas temporales, logs de error).
* README.md: Guía principal del repositorio y arquitectura.