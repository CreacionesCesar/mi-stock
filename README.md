# mi-stock
📦 EscStock

EscStock es un sistema de gestión y control de stock diseñado para trabajar desde dispositivos móviles y PC, con información centralizada y sincronizada mediante una API y una base de datos en la nube.

El proyecto está compuesto por un backend desarrollado con FastAPI, una base de datos PostgreSQL y una aplicación/interfaz desarrollada con FlutterFlow.

---

🚀 Características

- 📦 Consulta de productos y stock.
- 🔢 Visualización de código, descripción y cantidad.
- 🔄 Actualización de cantidades.
- 📥 Importación de productos mediante archivos CSV.
- 📤 Exportación del stock a CSV.
- 🌐 API accesible desde Internet.
- 🗄️ Base de datos PostgreSQL.
- 📱 Interfaz preparada para dispositivos Android.
- 💻 Posibilidad de utilizar el sistema desde PC.
- 🔗 Comunicación entre la aplicación y el backend mediante API REST.
- 📚 Documentación automática de la API mediante Swagger/OpenAPI.

---

🏗️ Arquitectura del proyecto

La estructura general de EscStock es:

                         ┌─────────────────────┐
                         │      ESCStock       │
                         │ Aplicación Flutter  │
                         │     / FlutterFlow  │
                         └──────────┬──────────┘
                                    │
                                    │ API REST
                                    ▼
                         ┌─────────────────────┐
                         │       Render        │
                         │   FastAPI Backend   │
                         └──────────┬──────────┘
                                    │
                                    │ SQLAlchemy
                                    ▼
                         ┌─────────────────────┐
                         │     PostgreSQL      │
                         │     escstock-db     │
                         └─────────────────────┘

La aplicación no necesita acceder directamente a PostgreSQL.

La comunicación se realiza mediante el backend, que funciona como intermediario entre la aplicación y la base de datos.

---

🌐 Backend

El backend está desarrollado utilizando:

- Python
- FastAPI
- SQLAlchemy
- PostgreSQL
- Uvicorn

El backend se encuentra alojado en Render.

URL del backend

https://escstock-backend-1.onrender.com

Estado del servidor

La ruta principal:

GET /

devuelve:

{
  "status": "ESCStock Online"
}

Esto permite comprobar rápidamente que el backend está funcionando.

---

📚 Documentación de la API

FastAPI genera automáticamente documentación interactiva.

La documentación se encuentra en:

/docs

Por lo tanto:

https://escstock-backend-1.onrender.com/docs

Desde Swagger se pueden consultar y probar las diferentes operaciones disponibles en la API.

---

🔌 Endpoints

GET "/"

Comprueba el estado del backend.

Respuesta

{
  "status": "ESCStock Online"
}

---

GET "/productos"

Obtiene los productos almacenados en el sistema.

La información incluye:

Codigo
Descripción
Cantidad

Ejemplo

[
  {
    "Codigo": 365,
    "Descripcion": "...",
    "Cantidad": 0
  }
]

---

POST "/subir_csv"

Permite importar información de productos mediante un archivo CSV.

Esta función está destinada a facilitar la carga inicial o actualización masiva del stock.

---

POST "/actualizar"

Permite actualizar información del stock mediante la API.

La aplicación puede utilizar este endpoint para enviar modificaciones realizadas por el usuario.

---

GET "/exportar_csv"

Permite obtener/exportar la información almacenada en el sistema en formato CSV.

---

🗄️ Base de datos

EscStock utiliza PostgreSQL como base de datos.

La base de datos se encuentra alojada en Render.

Base de datos

escstock-db

La conexión se realiza mediante las variables de entorno configuradas en el servidor.

Las credenciales y URLs privadas de conexión nunca deben almacenarse directamente en este README ni subirse al repositorio.

---

🐍 Estructura del Backend

La estructura principal del backend está organizada de la siguiente manera:

escstock-backend/
│
├── main.py
├── modelos.py
├── rutas.py
├── requisitos.txt
├── Procfile
└── README.md

"main.py"

Es el punto de entrada principal de la aplicación FastAPI.

Entre sus responsabilidades se encuentran:

- Crear la aplicación FastAPI.
- Configurar CORS.
- Crear las tablas de la base de datos.
- Importar las rutas.
- Iniciar la aplicación.

---

"modelos.py"

Contiene los modelos utilizados por SQLAlchemy para representar las estructuras de la base de datos.

---

"rutas.py"

Contiene las rutas/endpoints de la API.

Entre ellas:

/productos
/subir_csv
/actualizar
/exportar_csv

---

"requisitos.txt"

Contiene las dependencias necesarias para ejecutar el backend.

---

"Procfile"

Se utiliza para indicar a la plataforma de alojamiento cómo iniciar el servidor.

---

🔐 CORS

El backend utiliza configuración CORS para permitir la comunicación con la aplicación.

Actualmente se configuró:

allow_origins=["*"]

Esto permite solicitudes desde diferentes orígenes.

«Para una versión definitiva orientada a producción, se recomienda restringir los orígenes permitidos a los dominios y aplicaciones autorizadas.»

---

📱 Aplicación EscStock

La aplicación cliente está siendo desarrollada utilizando FlutterFlow.

Proyecto

ESCStock

La aplicación consume directamente la API de EscStock.

---

🔗 Comunicación FlutterFlow → API

La aplicación utiliza una llamada API denominada:

GetProductos

La llamada utiliza:

GET /productos

y obtiene los productos desde el backend.

Los datos recibidos se utilizan para mostrar la información dentro de una lista.

---

📋 Visualización de productos

La interfaz utiliza una lista de productos.

Los principales campos mostrados son:

Código
Descripción
Cantidad

La información proviene directamente del backend.

Ejemplo conceptual:

┌──────────────────────────────────────┐
│ Código    Descripción       Cantidad │
├──────────────────────────────────────┤
│ 365       Producto A             0   │
│ 100116    Coca Cola 500          25  │
│ 109101    Coca Cola 350          18  │
└──────────────────────────────────────┘

---

🔄 Flujo de información

El funcionamiento general es:

Usuario
   │
   ▼
Aplicación EscStock
   │
   │ GET /productos
   ▼
FastAPI
   │
   ▼
PostgreSQL
   │
   ▼
FastAPI
   │
   ▼
Aplicación
   │
   ▼
Usuario

Para actualizar información:

Usuario
   │
   ▼
Aplicación
   │
   │ POST /actualizar
   ▼
FastAPI
   │
   ▼
PostgreSQL

---

📥 Importación de stock

EscStock contempla la posibilidad de importar información mediante CSV.

Flujo:

Archivo CSV
     │
     ▼
/subir_csv
     │
     ▼
FastAPI
     │
     ▼
PostgreSQL

Esto permite cargar grandes cantidades de productos sin tener que introducirlos manualmente uno por uno.

---

📤 Exportación

El sistema también permite exportar la información mediante:

GET /exportar_csv

Flujo:

PostgreSQL
     │
     ▼
FastAPI
     │
     ▼
Archivo CSV

---

☁️ Infraestructura

Actualmente la infraestructura está planteada de la siguiente manera:

Componente| Tecnología
Aplicación| FlutterFlow / Flutter
Backend| Python + FastAPI
Servidor| Render
Base de datos| PostgreSQL
API| REST
Documentación API| Swagger / OpenAPI
Importación| CSV
Exportación| CSV

---

⚙️ Variables de entorno

Las configuraciones sensibles deben almacenarse mediante variables de entorno.

Por ejemplo:

DATABASE_URL

Nunca se deben subir al repositorio:

Contraseñas
Tokens
Claves privadas
Credenciales de PostgreSQL
API Keys

Si una credencial aparece accidentalmente en GitHub, debe considerarse comprometida y ser reemplazada.

---

🛠️ Instalación y desarrollo local

Para ejecutar el backend localmente se necesita:

- Python 3.x
- PostgreSQL
- Git
- Dependencias indicadas en "requisitos.txt"

Clonar el repositorio:

git clone https://github.com/cesaremanueldfrcocacola-glitch/escstock-backend.git

Entrar al proyecto:

cd escstock-backend

Instalar las dependencias:

pip install -r requisitos.txt

Configurar las variables de entorno necesarias.

Luego iniciar FastAPI con Uvicorn.

Ejemplo:

uvicorn main:app --reload

Una vez iniciado, la API estará disponible normalmente en:

http://127.0.0.1:8000

Y la documentación en:

http://127.0.0.1:8000/docs

---

🚀 Despliegue en Render

El backend está preparado para ejecutarse en Render.

El servicio utilizado es:

escstock-backend1

La aplicación debe iniciarse utilizando un servidor compatible con ASGI, como Uvicorn.

Esto es importante porque FastAPI es una aplicación ASGI.

---

🧪 Comprobación del backend

Para comprobar rápidamente que el servidor está funcionando, acceder a:

https://escstock-backend-1.onrender.com/

Debe responder con:

{
  "status": "ESCStock Online"
}

También se puede utilizar:

https://escstock-backend-1.onrender.com/docs

para abrir la documentación interactiva.

---

📌 Estado actual del proyecto

Backend

- [x] FastAPI funcionando
- [x] Backend desplegado en Render
- [x] PostgreSQL creada
- [x] Conexión con base de datos
- [x] Endpoint principal
- [x] Endpoint "/productos"
- [x] Endpoint "/subir_csv"
- [x] Endpoint "/actualizar"
- [x] Endpoint "/exportar_csv"
- [x] Documentación "/docs"
- [x] Configuración CORS

Aplicación

- [x] Proyecto creado en FlutterFlow
- [x] Conexión con API
- [x] API Call "GetProductos"
- [x] Consulta de productos
- [x] Listado de productos
- [x] Visualización de Código
- [x] Visualización de Descripción
- [x] Visualización de Cantidad

Próximos objetivos

- [ ] Completar diseño definitivo de la aplicación.
- [ ] Implementar actualización de stock desde la aplicación.
- [ ] Mejorar importación y validación de CSV.
- [ ] Mejorar exportación.
- [ ] Implementar autenticación de usuarios.
- [ ] Definir permisos por usuario.
- [ ] Mejorar seguridad de la API.
- [ ] Optimizar sincronización.
- [ ] Realizar pruebas completas.
- [ ] Generar APK Android.
- [ ] Preparar versión para PC/web.
- [ ] Implementar copias de seguridad de la base de datos.

---

🔒 Seguridad

Para una versión de producción se recomienda:

1. Utilizar autenticación de usuarios.
2. Implementar autorización y permisos.
3. Restringir CORS.
4. Validar todos los datos recibidos.
5. Proteger los endpoints de actualización.
6. Utilizar variables de entorno para información sensible.
7. Implementar copias de seguridad.
8. Registrar errores y operaciones importantes.
9. Evitar exponer información innecesaria de la base de datos.
10. Utilizar HTTPS para todas las comunicaciones.

---

🧭 Hoja de ruta

El objetivo de EscStock es evolucionar desde el sistema actual hacia una plataforma completa de gestión de stock.

Fase 1 — Backend

- API REST
- PostgreSQL
- CRUD de productos
- Importación CSV
- Exportación CSV

Fase 2 — Aplicación

- Listado de productos
- Búsqueda
- Consulta de stock
- Modificación de cantidades
- Sincronización con servidor

Fase 3 — Usuarios

- Login
- Usuarios
- Roles
- Permisos

Fase 4 — Producción

- Seguridad
- Backups
- Optimización
- Monitoreo
- APK Android
- Versión PC/Web

---

📖 Convenciones

Se recomienda mantener:

- Nombres de archivos claros.
- Código organizado por responsabilidades.
- Variables sensibles fuera del repositorio.
- Cambios importantes documentados.
- Commits descriptivos.

Ejemplo:

feat: agregar actualización de stock
fix: corregir importación CSV
docs: actualizar README
refactor: reorganizar rutas API

---

🤝 Desarrollo

EscStock es un proyecto en evolución.

Antes de realizar cambios importantes se recomienda comprobar:

1. Backend
2. Base de datos
3. API
4. FlutterFlow
5. Render
6. Compatibilidad con datos existentes

Un cambio realizado en la estructura de la base de datos o API puede afectar directamente a la aplicación.

---

📜 Licencia

La licencia del proyecto deberá definirse antes de realizar una distribución pública.

---

📞 EscStock

Proyecto: EscStock
Backend: FastAPI
Base de datos: PostgreSQL
Frontend: FlutterFlow / Flutter
Hosting: Render

---

🔗 Recursos

Backend

https://escstock-backend-1.onrender.com

Documentación de la API

https://escstock-backend-1.onrender.com/docs

Repositorio

https://github.com/cesaremanueldfrcocacola-glitch/escstock-backend

---

«EscStock — Gestión de stock simple, centralizada y sincronizada.»
