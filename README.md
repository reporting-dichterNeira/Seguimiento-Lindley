# Seguimiento Lindley

Portal: https://reporting-dichterneira.github.io/Seguimiento-Lindley/

## Operación con MT

Dos fuentes operativas: export Databricks y Excel de universo con forecast integrado. En Administración selecciona el mes y carga el universo; se detecta la hoja por sus columnas, con preferencia por BD y encabezados dentro de las primeras 20 filas. El orden de las columnas no afecta la carga. Se reconocen Código / CODIGO, USUARIOS / MT, T/S / SELECCIÓN, TIPOLOGIA / Programa de Valor, Día de campo / DIA, RUTAS / RUT.COM y CDA2 / DES LOC_COM. Las columnas adicionales se ignoran. MT es obligatorio en todos los puntos; día de campo es obligatorio solo para titulares y opcional para suplentes. CODIGO cruza con ID_de_PDV. MT asigna los puntos, incluidas las visitas pendientes; en el plan integrado no se infieren responsables por ruta.

El forecast cuenta puntos titulares (SELECCION=T) por MT y DIA. Programa de Valor identifica Titanes. La primera fecha con auditorías de la ola es día de campo 1; las siguientes fechas distintas son días 2, 3… para toda la ola, incluso con saltos entre fechas. Todos los estados identifican fechas; los estados efectivos suman al avance. El corte limita producción y metas a los días observados. Los días futuros mantienen el forecast sin inventar fechas calendario.

Carga un Excel de homologación con MT, Usuario y Nombre opcional. Usuario debe coincidir con nombre_usuario del export. Se rechaza repetir la misma pareja MT y Usuario. Se permiten MT compartidos y usuarios con varios MT. Las equivalencias se guardan por mes; el acceso MT usa la homologación más reciente. El servidor habilita las cuentas faltantes. Sin equivalencia, el MT queda pendiente. Cada auditor ve sus puntos asignados y sus visitas; el administrador ve el estudio completo.

## Databricks

Reporting Cluster: 1115-192254-jqnbpmi. Job automático export 448396700809372: 06:00, 12:00 y 17:00 America/Bogota. Consulta solo la Ola del mes actual y conserva otros meses. Actualizar histórico del export usa job 577854098883515 y consulta todo 2026/2027. Cada carga reemplaza la versión activa de la ola y conserva versiones privadas.

El job de forecast 873798021992995 queda PAUSADO y el servidor rechaza sus sincronizaciones. El forecast nuevo proviene del universo MT/DIA. Los meses con universo anterior conservan su información histórica.

## Supabase y código

GitHub contiene solo interfaz y fuente. Los datos, perfiles y homologaciones están en Supabase con autorización del servidor y bucket lindley-private protegido. Las tablas no tienen acceso directo desde el cliente. Aplica supabase/mt-plan.sql y despliega supabase/index.ts. El acceso MT conserva el límite de intentos; el servidor valida el rol y filtra por usuario del export. Las credenciales administrativas permanecen sin cambios.

fuente-lindley.zip contiene el proyecto editable. Instala npm ci y compila npm run build. Configura las claves públicas VITE_SUPABASE_ANON_KEY y VITE_SUPABASE_LOGIN_JWT. Nunca publiques claves de servidor, bases ni códigos de acceso. Databricks verifica la identidad temporal de la ejecución; el inicio manual utiliza una credencial leída únicamente en el servidor desde integrations/databricks.json.

Un usuario puede conservar varios MT. Un MT compartido admite varias filas de homologación y divide cuota, forecast y metas Titanes por partes iguales (50 % para dos personas), manteniendo el avance efectivo real por usuario del export. Al ingresar un MT compartido se selecciona el usuario del export. Las metas impares pueden expresarse con 0,5 para conservar el total exacto.

## Auditores por mes

En Administración, «Equivalencias y auditores del mes» permite añadir un auditor con Nombre, Usuario del export y uno o varios MT separados por comas. Las cuentas existentes se conservan; las nuevas se habilitan automáticamente. No es necesario volver a subir el Excel y pueden añadirse MT antes de actualizar el universo.

La casilla «Activo en el mes» controla la participación del auditor en ese periodo. El filtro muestra Todos, Activos, Inactivos y Retirados. «Eliminar del mes» requiere confirmar el retiro y conserva un registro recuperable; «Restaurar al mes» recupera sus equivalencias. Ninguna acción borra auditorías ni modifica otros meses. El estado se guarda en la versión de homologación del mes y queda en el historial de cargas. Subir otro Excel reemplaza estas equivalencias.

Solo los auditores activos participan en el reparto de las metas del MT. Si no queda ninguno activo, el MT queda sin homologar y su cuota se conserva en el total del estudio. Las efectivas históricas siguen atribuidas al usuario del export. Un auditor inactivo no puede usar ese MT para ingresar ni consultar el mes con una sesión previa. La lista manual comprueba la versión leída y pide actualizar si cambió antes de guardar.

## Universo completo

La cuota y el forecast cuentan solo titulares programados. Todos los suplentes están disponibles en Puntos de venta, incluso sin visita o sin día previsto. El filtro permite ver titulares o suplentes. SUPLENTES conserva S1, S2 y otros niveles cuando T/S indica S. Las efectivas de suplentes suman a producción sin aumentar las metas. Los estados históricos dentro del universo se ignoran: el estado actual siempre procede del export de Databricks. La pantalla Estados del export permite conciliar auditorías y puntos únicos; Última actualización corresponde a la última carga exitosa del export del mes, automática o manual.

## Visual de usuarios y estados

Las vistas de gestión muestran el usuario del export; cuando no hay equivalencia, muestran el MT. En Auditores y Administración se puede buscar por usuario o MT y filtrar MT compartidos o usuarios con varios MT. Las etiquetas muestran los códigos, y el detalle de un MT compartido permite consultar sus otros usuarios activos. Los filtros son de consulta y conservan las asignaciones. Estados del export se representa como barras con conteos, porcentaje y total de auditorías, para el mes y corte seleccionados.

En cada fila de Administración, «Editar MT» permite reemplazar todos los códigos de un usuario para el mes seleccionado. Se conservan el usuario, nombre, estado activo/retirado y otros meses. Se admiten varios MT y MT compartidos; el avance continúa por usuario del export. Guardar comprueba la versión de la lista y registra una nueva versión privada de homologación.

## Indicadores de efectivas

Efectivas cuenta puntos únicos del universo por ola y corte con Aprobada, En Proceso, Requiere Aprobación, Exenta por Validación Smart, Alerta, Cancelación Automática, Incidencia, Exenta o Cancelada. La última auditoría efectiva del punto determina el usuario y fecha de producción; varias auditorías no multiplican el avance. SubStatus identifica Exenta por Validación Smart y Cancelación Automática antes del estado principal. Otros subestados (Reconocimiento Manual, Mal Ejecutada, Incidente en PDV, etc.) conservan la categoría Estado. Los estados desconocidos y puntos sin visita no son efectivos.

Cuota, forecast y reparto de MT se conservan. Avance = efectivas / cuota; cumplimiento = efectivas / forecast; GAP = forecast − efectivas (positivo indica faltante); pendientes = máximo(0, cuota − efectivas). Titanes aplica las mismas reglas. Incidencias, canceladas y en revisión son desgloses incluidos en efectivas y no deben sumarse de nuevo. La gráfica cuenta auditorías; los indicadores cuentan puntos. Databricks trae Estado y SubStatus en las sincronizaciones del mes actual y del histórico.

## Rutas, visitas CT y exportación

En Auditores, «Ver rutas» despliega las metas y efectivas de cada ruta/CDA del usuario. El reparto de MT compartidos se conserva y las rutas concilian con el resumen del auditor. El Excel del seguimiento incluye todos los desgloses por ruta, aunque estén contraídos. Los meses anteriores sin planificación por punto no inventan un forecast por ruta.

«Cerrado temporal» reúne puntos del universo que tuvieron Incidencia / Cerrado temporal en la ola y hasta el corte. Cuenta todas sus auditorías con ID_de_audito distinto, incluso con otro estado o varias en la misma fecha. Verde OK indica dos o más visitas; rojo Falta visita indica una. Hay filtros por seguimiento y búsqueda. Para auditores se muestran puntos de su gestión y el conteo agregado del punto, sin identidades ni registros de otros usuarios.

Todas las tablas tienen «Exportar Excel». Puntos de venta exporta todos los resultados filtrados con autorización del servidor, más allá de la página visible. Programa, selección y estados se combinan con AND; varios estados se incluyen con OR. Por ejemplo, Titanes + titulares + Sin visitar. Los filtros de consulta no cambian indicadores ni fuentes.
