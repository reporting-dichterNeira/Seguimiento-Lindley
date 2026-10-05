# Seguimiento Lindley

Portal publicado en https://reporting-dichterneira.github.io/Seguimiento-Lindley/

GitHub Pages aloja la interfaz. Supabase aloja las bases en un bucket privado y valida cada consulta con Supabase Auth. Los roles y la relación con nombre_usuario se guardan en lindley_profiles; el cliente no puede elegir su rol ni consultar tablas directamente.

## Operación

1. Ingresa con una cuenta habilitada.
2. En Administración selecciona el mes y carga universo, export acumulado y forecast.
3. Valida y revisa la muestra de filas antes de guardar.
4. Confirma responsables de rutas ambiguas desde la página.

El universo cruza CODIGO con ID PDV. Programa de Valor define Titanes y SELECCIÓN define titulares/suplentes. El forecast cruza usuario y fecha; solo Aprobada suma al avance y el corte incluye la fecha seleccionada. Los exports reemplazan la base activa del mes. El historial conserva versiones.

## Código

index.html contiene la interfaz compilada; xlsx.full.min.js es el lector local de Excel. fuente-lindley.zip contiene el proyecto editable, esquema SQL y servidor Supabase (supabase/index.ts). Instala con npm ci y compila con npm run build. Configura VITE_SUPABASE_ANON_KEY con una clave pública de Supabase. Nunca incluyas claves de servidor ni bases en GitHub.

Las contraseñas se entregan por separado y no forman parte del repositorio.


## Publicación
GitHub Pages publica automáticamente los cambios de main desde la raíz del repositorio.
