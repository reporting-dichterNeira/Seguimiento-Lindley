# Seguimiento Lindley

Portal: https://reporting-dichterneira.github.io/Seguimiento-Lindley/

## Operación con MT

Dos fuentes operativas: export Databricks y Excel de universo con forecast integrado. En Administración selecciona el mes y carga el universo; la hoja BD se detecta automáticamente. Requiere CODIGO, CLIENTE, RUT.COM, Programa de Valor, SELECCION, MT y DIA. CODIGO cruza con ID_de_PDV. MT asigna los puntos, incluidas las visitas pendientes; en el plan integrado no se infieren responsables por ruta.

El forecast cuenta puntos titulares (SELECCION=T) por MT y DIA. Programa de Valor identifica Titanes. La primera fecha con auditorías de la ola es día de campo 1; las siguientes fechas distintas son días 2, 3… para toda la ola, incluso con saltos entre fechas. Todos los estados identifican fechas; solo Aprobada suma al avance. El corte limita producción y metas a los días observados. Los días futuros mantienen el forecast sin inventar fechas calendario.

Carga un Excel de homologación con MT, Usuario y Nombre opcional. Usuario debe coincidir con nombre_usuario del export. Se rechaza repetir la misma pareja MT y Usuario. Se permiten MT compartidos y usuarios con varios MT. Las equivalencias se guardan por mes; el acceso MT usa la homologación más reciente. El servidor habilita las cuentas faltantes. Sin equivalencia, el MT queda pendiente. Cada auditor ve sus puntos asignados y sus visitas; el administrador ve el estudio completo.

## Databricks

Reporting Cluster: 1115-192254-jqnbpmi. Job automático export 448396700809372: 06:00, 12:00 y 17:00 America/Bogota. Consulta solo la Ola del mes actual y conserva otros meses. Actualizar histórico del export usa job 577854098883515 y consulta todo 2026/2027. Cada carga reemplaza la versión activa de la ola y conserva versiones privadas.

El job de forecast 873798021992995 queda PAUSADO y el servidor rechaza sus sincronizaciones. El forecast nuevo proviene del universo MT/DIA. Los meses con universo anterior conservan su información histórica.

## Supabase y código

GitHub contiene solo interfaz y fuente. Los datos, perfiles y homologaciones están en Supabase con autorización del servidor y bucket lindley-private protegido. Las tablas no tienen acceso directo desde el cliente. Aplica supabase/mt-plan.sql y despliega supabase/index.ts. El acceso MT conserva el límite de intentos; el servidor valida el rol y filtra por usuario del export. Las credenciales administrativas permanecen sin cambios.

fuente-lindley.zip contiene el proyecto editable. Instala npm ci y compila npm run build. Configura las claves públicas VITE_SUPABASE_ANON_KEY y VITE_SUPABASE_LOGIN_JWT. Nunca publiques claves de servidor, bases ni códigos de acceso. Databricks verifica la identidad temporal de la ejecución; el inicio manual utiliza una credencial leída únicamente en el servidor desde integrations/databricks.json.

Un usuario puede conservar varios MT. Un MT compartido admite varias filas de homologación y divide cuota, forecast y metas Titanes por partes iguales (50 % para dos personas), manteniendo el avance real por usuario del export. Al ingresar un MT compartido se selecciona el nombre del auditor. Las metas impares pueden expresarse con 0,5 para conservar el total exacto.
