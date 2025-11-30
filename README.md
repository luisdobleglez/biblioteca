# Biblioteca Virtual API — PHP + MySQL + Docker

API educativa creada para el módulo **IFCD63 — Open Source Back End Web Developer**, cuyo objetivo es mostrar cómo desarrollar y ejecutar una API Back End usando tecnologías Open Source.

La aplicación expone en formato **JSON** un listado de libros almacenados en una base de datos MySQL y permite visualizar dichos datos desde un frontend en HTML con JavaScript.

---

##  Estructura del proyecto

```
/ (carpeta raíz)
│
├── docker-compose.yml
├── Dockerfile
├── biblio.sql        ← Base de datos lista para importar 🔥
│
└── /src
    ├── api.php
    ├── index.php
    ├── conexion.inc.php
    ├── index.html
    ├── libros.json
    └── (otros archivos PHP)
```

---

##  Requisitos previos

- Docker  
- Docker Compose  
- (Opcional) phpMyAdmin para administrar la base de datos desde un navegador  

Comprobar:

```bash
docker --version
docker compose version
```

---

# Servicios definidos en Docker

Este proyecto utiliza **docker-compose** para levantar:

### ✔ 1) Servidor PHP + Apache
- PHP 8.2  
- Extensiones mysqli, pdo y pdo_mysql  
- Servidor Apache  
- Código ubicado en `/src`

### ✔ 2) MySQL 8
- Base de datos inicial: `biblio`  
- Usuario: `root`  
- Password: `root`  
- Volumen persistente `db_data`  

### ✔ 3) (Opcional pero recomendado) phpMyAdmin

Si quieres incluir phpMyAdmin, añade esto a tu `docker-compose.yml`:

```yaml
phpmyadmin:
  image: phpmyadmin/phpmyadmin
  container_name: phpmyadmin
  environment:
    PMA_HOST: db
    PMA_PORT: 3306
  ports:
    - "8081:80"
  depends_on:
    - db
```

Acceso en navegador:

```
http://localhost:8081
Usuario: root
Contraseña: root
```

---

# 🧩 Importar la base de datos (biblio.sql)

###  Opción 1 — Importar con phpMyAdmin (más fácil)

1. Abrir en navegador: `http://localhost:8081`
2. Entrar con usuario **root**, contraseña **root**
3. Crear base de datos llamada: `biblio`
4. Ir a **Importar → Seleccionar archivo**
5. Elegir `biblio.sql` (ubicado en la raíz del proyecto)
6. Pulsar **Continuar**

---

###  Opción 2 — Importar desde la terminal Docker

```bash
docker exec -i mysql_db mysql -u root -proot biblio < biblio.sql
```

---

##  Levantar el proyecto

```bash
docker compose up -d
```

Esto iniciará:

- PHP + Apache  
- MySQL  
- (Opcional) phpMyAdmin  

---

##  Acceder a la API

```
http://localhost/api.php
```

La salida será un JSON como:

```json
[
  {
    "id_libro": "1",
    "titulo": "El Capitán Tormenta II",
    "autor": "Emilio Salgari",
    "nombre_archivo": "capitan-tormenta..."
  }
]
```

---

##  Acceder al frontend

```
http://localhost/index.html
```

Este archivo consume la API con `fetch()` y muestra los libros en una tabla.

---

##  Comandos útiles

Detener:

```bash
docker compose down
```

Reconstruir todo:

```bash
docker compose down -v
docker compose up -d --build
```

Entrar a MySQL:

```bash
docker exec -it mysql_db mysql -u root -p
```

Logs:

```bash
docker compose logs php
```

---

##  Licencia educativa

Este proyecto forma parte de la formación 
**para cursos realizados por LuisGG para Smart Mind**
 Libre para uso docente.
