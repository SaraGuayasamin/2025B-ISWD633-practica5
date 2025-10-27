# COMPLETAR  
Durante la configuración del archivo compose.yaml para entornos distribuidos, se presentó un problema común en la práctica de Docker Compose: la configuración inicial de la dependencia de servicios.

Problema (Inicial):

Al intentar conectar servicios en versiones de Docker Compose que son estrictas con la sintaxis de dependencia o que tienen problemas de timing, el servicio de la aplicación (Wordpress/SonarQube) intenta conectarse a la base de datos (MySQL/PostgreSQL) antes de que esta última haya terminado de inicializarse completamente.

Síntoma: El contenedor de la aplicación fallaba repetidamente o salía porque no podía conectar a la base de datos.

Solución Implementada:

Para asegurar una secuencia de inicio robusta, se combinó la definición de salud (healthcheck) en la base de datos con la directiva depends_on de la aplicación:

Definición de Salud (healthcheck en db): Se definió un healthcheck explícito (pg_isready -U sonarqube) en el servicio de la base de datos para que Docker sepa cuándo la base de datos está realmente lista para aceptar conexiones, no solo cuando el contenedor ha iniciado.

Condición de Dependencia (depends_on en sonarqube): Se utilizó la condición condition: service_healthy en la sección depends_on del servicio sonarqube.
