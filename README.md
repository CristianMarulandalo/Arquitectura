Sistema de Carrito de compras
README.md 
1. Introducción y Metas
Nombre del proyecto: Sistema de Pedidos (tienda_flask)
Problema / necesidad que resuelve:
El sistema surge de la necesidad de digitalizar y centralizar el proceso de compra de productos electrónicos, permitiendo a los usuarios registrarse, autenticarse y generar pedidos de forma segura, sin depender de canales manuales (WhatsApp, formularios físicos, etc.). Automatiza el registro de usuarios, la autenticación y el historial de pedidos en una única plataforma web.
2. Stakeholders
Integrantes, roles y responsabilidades del equipo:
Cristian Marulanda: Backend
Erick Gregorio Caicedo: Backend
Neiver Dario Fernandez: Frontend
Veronica Parra: Tester
3. Tecnologías (Environment)
Stack tecnológico
•	Backend: Flask (Python)
•	Base de Datos: MySQL
•	Frontend: HTML5, CSS3, JavaScript
•	Framework CSS: Bootstrap 5.3.2
•	Seguridad: Werkzeug (hashing de contraseñas)
Requisitos de instalación / despliegue
•	Python 3.8+
•	MySQL 8.0+
•	pip (gestor de paquetes de Python)
pip install flask
pip install mysql-connector-python
pip install werkzeug
Configuración de base de datos:
CREATE DATABASE tienda_flask;
CREATE USER 'flask_user'@'localhost' IDENTIFIED BY 'engranaje1';
GRANT ALL PRIVILEGES ON tienda_flask.* TO 'flask_user'@'localhost';
FLUSH PRIVILEGES;
Ejecución:
python app.py
# Disponible en http://127.0.0.1:5000
4. Mapeo Arquitectónico
Estructura de carpetas
proyecto/
├── app.py               # Aplicación principal Flask
├── db_config.py         # Configuración de conexión a BD
├── templates/           # Plantillas HTML
│   ├── login.html
│   ├── register.html
│   ├── pedido.html
│   └── pedidos_lista.html
└── static/              # Archivos estáticos
    ├── css/
    │   ├── main.css
    │   └── pedido.css
    ├── js/
    │   └── pedido.js
    └── img/
    
Enlaces a diagramas (docs/)
•	[Diagrama de flujo de navegación → docs/flujo_navegacion.png]
•	[Diagrama entidad-relación de la base de datos → docs/diagrama_er.png]
•	[Diagrama de arquitectura general → docs/arquitectura.png]
5. Estado y Límites
Limitaciones actuales
•	El catálogo de productos está codificado directamente en el backend (no proviene de una tabla de base de datos).
•	La clave secreta de sesión (secret key) está definida como texto plano en el código.
•	No se implementan validaciones robustas de formularios (longitud, formato de correo, fortaleza de contraseña).
•	No hay protección explícita contra CSRF ni límite de intentos (rate limiting) en el login.
•	El proyecto está pensado para entorno de desarrollo (debug=True), no para producción.
Trabajo futuro
•	Migrar la secret key y credenciales de BD a variables de entorno.
•	Implementar HTTPS en producción.
•	Agregar tokens CSRF y rate limiting en el login.
•	Mover el catálogo de productos a la base de datos.
•	Agregar validación de datos de formularios y requisitos de contraseña más fuertes.

Información adicional de referencia 
Base de datos — Tabla usuarios:
CREATE TABLE usuarios (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(100) NOT NULL,
  correo VARCHAR(100) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
Base de datos — Tabla pedidos:
CREATE TABLE pedidos (
  id INT PRIMARY KEY AUTO_INCREMENT,
  usuario_id INT NOT NULL,
  producto VARCHAR(100) NOT NULL,
  cantidad INT NOT NULL,
  fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
);
Funcionalidades principales:
•	Registro de usuarios (/register) con hash de contraseña.
•	Inicio de sesión (/login) con verificación de contraseña.
•	Cierre de sesión (/logout).
•	Creación de pedidos (/pedido), protegida por sesión activa.
•	Historial de pedidos (/pedidos), filtrado por usuario.
