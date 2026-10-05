# Seguimiento Lindley

Portal publicado en https://reporting-dichterneira.github.io/Seguimiento-Lindley/

GitHub Pages aloja la interfaz. Supabase aloja las bases en un bucket privado y valida cada consulta con Supabase Auth. Los roles y la relación con nombre_usuario se guardan en lindley_profiles; el cliente no puede elegir su rol ni consultar tablas directamente.

## Operación

1. El auditor ingresa con su código personal. El botón Inicia administrador abre usuario y contraseña.
2. En Administración selecciona el mes y carga universo y forecast. El export se sincroniza desde Databricks a las 06:00, 12:00 y 17:00, America/Bogota, en Reporting Cluster.
3. Valida y revisa la muestra de filas antes de guardar.
4. Confirma responsables de rutas ambiguas desde la página.
5. En Auditores y códigos de acceso puedes añadir usuarios y copiar el código de cada auditor.
6. Actualizar ahora inicia una nueva ejecución en Databricks y la página muestra cuándo termina.

## Databricks

databricks/sync_lindley.py ejecuta la consulta Lindley de 2026 y 2027, valida todos los registros y publica cada ola por separado. El job 577854098883515 corre a las 06:00, 12:00 y 17:00 de Bogotá y se puede ejecutar desde Actualizar ahora. Usa Reporting Cluster (1115-192254-jqnbpmi). La publicación del export usa la identidad temporal nativa de la ejecución, verificada por el servidor con la API de Databricks. No se guardan contraseñas ni claves privadas en los notebooks. La identidad autorizada es masanchez@dichter-neira.com; cambiar el propietario exige actualizar esa autorización en el servidor. El inicio manual usa una credencial privada de integración leída exclusivamente por el servidor desde el bucket protegido, con vigencia hasta octubre de 2027. Nunca se envía al navegador ni se incluye en el código. Si el cluster está detenido, el arranque puede añadir tiempo a la actualización.

Aplica supabase/databricks-sync.sql antes de desplegar supabase/index.ts. Las solicitudes solo se crean con una sesión administrativa. Los auditores no pueden iniciar sincronizaciones y las tablas de solicitudes no tienen acceso directo desde el navegador. Los archivos export se pueden subir manualmente como respaldo. Universo, forecast y responsables de ruta se conservan.

El universo cruza CODIGO con ID PDV. Programa de Valor define Titanes y SELECCIÓN define titulares/suplentes. El forecast cruza usuario y fecha; solo Aprobada suma al avance y el corte incluye la fecha seleccionada. Los exports reemplazan la base activa del mes. El historial conserva versiones.

## Código

index.html contiene la interfaz compilada; xlsx.full.min.js es el lector local de Excel. fuente-lindley.zip contiene el proyecto editable, esquema SQL y servidor Supabase (supabase/index.ts). Instala con npm ci y compila con npm run build. Configura VITE_SUPABASE_ANON_KEY con una clave pública de Supabase. Nunca incluyas claves de servidor ni bases en GitHub.

Las contraseñas y códigos se entregan por separado y no forman parte del repositorio. Configura también VITE_SUPABASE_LOGIN_JWT con la clave pública anon JWT heredada: solo permite llegar al endpoint inicial; el servidor exige el código correcto o las credenciales administrativas antes de emitir una sesión. Los datos y el listado de códigos exigen una sesión y autorización de rol.
