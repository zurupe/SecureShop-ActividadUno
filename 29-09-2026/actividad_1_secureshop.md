* **MATERIA:** Desarrollo de Software Seguro
* **FECHA:** 29 de septiembre de 2026
* **ACTIVIDAD:** Actividad 1

**ESTUDIANTE(S):**
* Pablo Zurita, Alex Cuzco, Stiven Diaz

---

## Actividad 1 – Activos de SecureShop

### 1. Identificación y clasificación de activos

Tipos de activo considerados: información, software, servicio, infraestructura y datos.

| N.º | Activo | Tipo |
|---|---|---|
| 1 | Datos de usuarios | Información |
| 2 | Credenciales | Información (sensible) |
| 3 | Catálogo de productos | Datos |
| 4 | Pedidos y transacciones | Datos |
| 5 | Código fuente (repositorio GitHub) | Software |
| 6 | Aplicación web (frontend) | Software |
| 7 | Microservicio de usuarios | Servicio |
| 8 | Microservicio de productos | Servicio |
| 9 | Microservicio de pedidos | Servicio |
| 10 | API Gateway (punto de entrada común) | Servicio |
| 11 | Bases de datos (usuarios, productos y pedidos) | Infraestructura |
| 12 | Servidores, contenedores y red de despliegue | Infraestructura |
| 13 | Secretos y claves (tokens JWT, claves de API, variables de entorno) | Información (sensible) |
| 14 | Registros de actividad (logs) | Datos |

---

### 2. Consecuencias para SecureShop

*¿Qué consecuencias tendría que el activo fuera accedido (confidencialidad), modificado (integridad) o quedara indisponible (disponibilidad)?*

| Activo | Tipo | Si es accedido sin autorización | Si es modificado | Si queda indisponible |
|---|---|---|---|---|
| Datos de usuarios | Información | Filtración de datos personales (nombre, correo, dirección, teléfono); riesgo de suplantación de identidad, sanciones por incumplir normas de protección de datos y pérdida de confianza. | Datos falsos o alterados: envíos a direcciones incorrectas, cuentas secuestradas, información poco confiable. | Los clientes no pueden registrarse ni gestionar su cuenta; se afecta la atención y la operación. |
| Credenciales | Información (sensible) | Toma de control de cuentas de clientes y administradores; fraude, compras no autorizadas y acceso a otros sistemas si se reutilizan contraseñas. | Un atacante puede cambiar contraseñas o crear cuentas privilegiadas; se bloquea a usuarios legítimos. | Nadie puede iniciar sesión; se pierden ventas y aumenta la carga de soporte. |
| Catálogo de productos | Datos | Exposición de información comercial (costos, márgenes, productos no lanzados) a la competencia. | Precios o descripciones manipulados (por ejemplo, productos a precio 0), pérdidas económicas y daño a la reputación. | No se puede mostrar ni vender productos; caen los ingresos. |
| Pedidos y transacciones | Datos | Exposición del historial de compras y direcciones de entrega; violación de privacidad y posible fraude. | Cambio de montos, estados o direcciones; pérdidas financieras, disputas con clientes y pérdida de trazabilidad. | No se pueden crear ni procesar pedidos; se detiene el negocio y hay incumplimiento de entregas. |
| Código fuente (repositorio GitHub) | Software | Robo de propiedad intelectual y exposición de vulnerabilidades o secretos incluidos en el código, lo que facilita ataques. | Inserción de código malicioso o puertas traseras (ataque a la cadena de suministro) que se despliegan en producción. | Se paraliza el desarrollo y no es posible corregir errores ni publicar actualizaciones. |
| Aplicación web (frontend) | Software | Exposición de lógica del cliente y endpoints internos, útil para preparar ataques. | Defacement, redirecciones a sitios falsos o scripts que roban datos de los clientes (XSS, skimming). | El sitio no está disponible; los clientes no pueden comprar y se daña la imagen de la marca. |
| Microservicio de usuarios | Servicio | Acceso indebido a la administración de cuentas y perfiles; facilita la filtración masiva de datos. | Cambios indebidos de roles y permisos; escalada de privilegios. | Sin registro, autenticación ni gestión de perfiles; los demás servicios que dependen de él fallan. |
| Microservicio de productos | Servicio | Acceso a funciones internas de gestión de inventario y precios. | Alteración de inventario o precios; se venden productos inexistentes o a valores errónicos. | No se consulta ni actualiza el catálogo; el sitio queda sin productos visibles. |
| Microservicio de pedidos | Servicio | Acceso a la lógica y datos de compras de todos los clientes. | Creación o modificación fraudulenta de pedidos, descuentos o estados de pago. | No se pueden realizar compras; pérdida directa de ingresos. |
| API Gateway | Servicio | Visibilidad de rutas, tokens y tráfico de todos los servicios; punto único desde el cual atacar todo el sistema. | Enrutamiento a servicios falsos, eliminación de controles de autenticación o límites de solicitudes. | Toda la plataforma queda inaccesible aunque los servicios funcionen (punto único de falla). |
| Bases de datos (usuarios, productos, pedidos) | Infraestructura | Fuga masiva y directa de toda la información del negocio. | Corrupción o manipulación de datos; pérdida de integridad difícil de detectar y revertir. | Pérdida de datos si no hay respaldos; interrupción total o parcial del servicio. |
| Servidores, contenedores y red | Infraestructura | Control del entorno: instalación de malware, robo de datos y movimiento lateral entre servicios. | Cambios de configuración que debilitan la seguridad o degradan el rendimiento. | Caída de toda la plataforma (fallas, ataques DDoS, errores de despliegue); pérdidas por inactividad. |
| Secretos y claves (JWT, API keys, variables de entorno) | Información (sensible) | Suplantación de servicios y usuarios; acceso a bases de datos y a servicios externos. | Emisión de tokens falsos; se rompe la confianza entre microservicios. | Los servicios no pueden autenticarse entre sí ni conectarse a las bases de datos; fallo generalizado. |
| Registros de actividad (logs) | Datos | Exposición de datos sensibles o técnicos (IP, errores, identificadores) útiles para un atacante. | Borrado o alteración de evidencia; no se detectan ni se investigan incidentes. | Sin visibilidad ni capacidad de auditoría o respuesta ante incidentes; dificulta el cumplimiento normativo. |

### 3. Conclusiones

| N.º | Conclusión |
|---|---|
| 1 | Los activos más críticos son las credenciales, los datos de usuarios, los pedidos y las bases de datos, porque su compromiso afecta a la vez la confidencialidad, la integridad y la disponibilidad del negocio. |
| 2 | El API Gateway es un punto único de falla: su indisponibilidad o compromiso afecta a todos los servicios. |
| 3 | En una arquitectura de microservicios los activos dependen unos de otros (por ejemplo, pedidos depende de usuarios y productos), por lo que un incidente en uno puede propagarse a los demás. |
| 4 | Estos activos servirán de base para las siguientes actividades: análisis de amenazas, vulnerabilidades, riesgos y controles de seguridad. |