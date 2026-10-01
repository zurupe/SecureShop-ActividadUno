
### Trabajo en Clase

---

* **MATERIA:** Desarollo de Software Seguro
* **NRC:** 36900
* **VERSIÓN.:** V2.0
* **CARRERA:** Ingeniería de Software
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
| 15 | Copias de seguridad (backups) | Datos |
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

### 3. Amenazas de cada activo

| Activo | Tipo | Amenaza 1 | Amenaza 2 | Amenaza 3 |
|---|---|---|---|---|
| Datos de usuarios | Información | Inyección SQL que permite extraer datos personales. | Abuso de privilegios por personal interno con acceso excesivo. | Phishing e ingeniería social dirigidos a clientes y empleados. |
| Credenciales | Información (sensible) | Ataques de fuerza bruta y credential stuffing sobre el inicio de sesión. | Phishing para robar usuario y contraseña. | Almacenamiento débil de contraseñas (hash inseguro) que facilita su descifrado si se roba la base de datos. |
| Catálogo de productos | Datos | Modificación no autorizada de precios mediante la API. | Scraping masivo del catálogo por parte de la competencia. | Cuenta de administrador comprometida que carga o cambia productos. |
| Pedidos y transacciones | Datos | Manipulación de parámetros (montos, estados) en las solicitudes. | Acceso a pedidos de otros clientes por falta de control de acceso (IDOR). | Fraude con tarjetas robadas o repudio de transacciones por falta de trazabilidad. |
| Código fuente (repositorio GitHub) | Software | Robo de cuentas de GitHub de los desarrolladores. | Dependencias vulnerables o maliciosas (cadena de suministro). | Subida accidental de secretos y contraseñas al repositorio. |
| Aplicación web (frontend) | Software | Cross-Site Scripting (XSS) que roba sesiones o datos. | Defacement o secuestro del dominio/DNS. | Scripts de terceros comprometidos que capturan datos de pago (skimming). |
| Microservicio de usuarios | Servicio | Escalada de privilegios por control de acceso deficiente. | Fuerza bruta sobre los endpoints de autenticación. | Inyección de código en los endpoints de registro y perfil. |
| Microservicio de productos | Servicio | Inyección SQL en filtros y búsquedas. | Abuso de la API sin límites de solicitudes. | Alteración no autorizada de inventario o precios. |
| Microservicio de pedidos | Servicio | Acceso a pedidos ajenos por manipulación de identificadores (IDOR). | Repetición de solicitudes (replay) que duplica pedidos o pagos. | Denegación de servicio por saturación en la creación de pedidos. |
| API Gateway | Servicio | Ataques DDoS contra el punto de entrada. | Omisión de la autenticación por mala configuración de rutas. | Intercepción de tráfico (man-in-the-middle) si no se usa TLS. |
| Bases de datos (usuarios, productos, pedidos) | Infraestructura | Inyección SQL desde los servicios. | Ransomware que cifra o borra la información. | Credenciales de base de datos expuestas o configuración por defecto. |
| Servidores, contenedores y red | Infraestructura | Ataques DDoS que saturan la red. | Imágenes de contenedores vulnerables o con malware. | Puertos y servicios expuestos por mala configuración en la nube. |
| Secretos y claves (JWT, API keys, variables de entorno) | Información (sensible) | Exposición en el repositorio o en archivos de configuración. | Robo desde un servidor o contenedor comprometido. | Falta de rotación de claves, que permite su uso por personas que ya no deberían tener acceso. |
| Registros de actividad (logs) | Datos | Borrado o alteración de logs por un atacante para ocultar rastros. | Registro de datos sensibles que luego se filtran. | Saturación del almacenamiento por exceso de registros (log flooding). |
| Copias de seguridad (backups) | Datos | Robo de respaldos sin cifrar. | Cifrado o borrado de los respaldos por ransomware. | Fallos o corrupción de las copias que no se detectan a tiempo. |
| Copias de seguridad (backups) | Datos | Acceso a una copia completa de toda la información del negocio (usuarios, pedidos, credenciales); fuga masiva sin tocar los sistemas en producción. | Respaldos alterados o corruptos que, al restaurarse, reintroducen datos falsos o código malicioso. | Sin posibilidad de recuperarse ante un ataque, borrado o falla; pérdida permanente de información. |

## 4. Mecanismo de control por amenaza


| Activo | Amenaza | Mecanismo de control |
|---|---|---|
| Datos de usuarios | Inyección SQL que permite extraer datos personales. | Consultas parametrizadas (prepared statements) o un ORM, validación de entradas en el servidor y una cuenta de base de datos con privilegios mínimos. |
| | Abuso de privilegios por personal interno con acceso excesivo. | Control de acceso basado en roles (RBAC) con principio de mínimo privilegio, revisión periódica de permisos y auditoría de los accesos a datos personales. |
| | Phishing e ingeniería social dirigidos a clientes y empleados. | Autenticación multifactor (MFA), capacitación de concientización al personal y configuración de SPF, DKIM y DMARC en el correo de la tienda. |
| Credenciales | Ataques de fuerza bruta y credential stuffing sobre el inicio de sesión. | Límite de intentos con bloqueo temporal, rate limiting en el API Gateway, CAPTCHA tras intentos fallidos y MFA. |
| | Phishing para robar usuario y contraseña. | MFA, avisos por correo ante inicios de sesión desde dispositivos nuevos y campañas de concientización a los usuarios. |
| | Almacenamiento débil de contraseñas (hash inseguro) que facilita su descifrado si se roba la base de datos. | Hash con algoritmos diseñados para contraseñas (bcrypt o Argon2) con salt, y política de contraseñas mínimas; nunca guardar en texto plano ni con MD5/SHA1. |
| Catálogo de productos | Modificación no autorizada de precios mediante la API. | Autenticación y autorización por rol en los endpoints de escritura, validación de precios en el servidor y registro de auditoría de cada cambio. |
| | Scraping masivo del catálogo por parte de la competencia. | Rate limiting por IP y por usuario, detección de comportamiento automatizado (bots) y paginación con límites en la API. |
| | Cuenta de administrador comprometida que carga o cambia productos. | MFA obligatorio para administradores, sesiones con expiración corta y alertas ante cambios masivos o fuera de horario. |
| Pedidos y transacciones | Manipulación de parámetros (montos, estados) en las solicitudes. | Calcular montos y estados siempre en el servidor (nunca confiar en el cliente), validación estricta del esquema de las solicitudes y verificación de integridad. |
| | Acceso a pedidos de otros clientes por falta de control de acceso (IDOR). | Verificar en cada solicitud que el pedido pertenezca al usuario autenticado, usar identificadores no predecibles (UUID) y pruebas de autorización. |
| | Fraude con tarjetas robadas o repudio de transacciones por falta de trazabilidad. | Pasarela de pago certificada (PCI DSS) con 3D Secure, reglas antifraude y registro de cada transacción con fecha, usuario e IP para trazabilidad. |
| Código fuente (repositorio GitHub) | Robo de cuentas de GitHub de los desarrolladores. | MFA obligatorio en la organización, uso de llaves SSH o tokens con permisos limitados y revisión periódica de miembros y accesos. |
| | Dependencias vulnerables o maliciosas (cadena de suministro). | Escaneo de dependencias con Dependabot o npm audit en el pipeline, archivos lock con versiones fijas y revisión de nuevas librerías antes de añadirlas. |
| | Subida accidental de secretos y contraseñas al repositorio. | Archivo .gitignore para los .env, herramientas de detección de secretos (gitleaks, GitHub secret scanning) como pre-commit hook y rotación inmediata de cualquier secreto expuesto. |
| Aplicación web (frontend) | Cross-Site Scripting (XSS) que roba sesiones o datos. | Escape y sanitización de la salida, Content Security Policy (CSP) estricta y cookies de sesión con las banderas HttpOnly, Secure y SameSite. |
| | Defacement o secuestro del dominio/DNS. | Bloqueo del registro del dominio (registrar lock), DNSSEC, MFA en la cuenta del proveedor de dominio y monitoreo de cambios en los registros DNS. |
| | Scripts de terceros comprometidos que capturan datos de pago (skimming). | Subresource Integrity (SRI) en los scripts externos, CSP que limite los orígenes permitidos, reducir al mínimo los scripts de terceros y usar campos de pago alojados por la pasarela (iframe). |
| Microservicio de usuarios | Escalada de privilegios por control de acceso deficiente. | Autorización por roles verificada en cada endpoint, los roles se leen del token firmado (nunca del cuerpo de la solicitud) y pruebas de autorización en el pipeline. |
| | Fuerza bruta sobre los endpoints de autenticación. | Rate limiting y bloqueo temporal por cuenta e IP, retraso progresivo entre intentos y alertas ante picos de inicios de sesión fallidos. |
| | Inyección de código en los endpoints de registro y perfil. | Validación de esquema y sanitización de todos los campos, consultas parametrizadas y rechazo de entradas con formato inesperado. |
| Microservicio de productos | Inyección SQL en filtros y búsquedas. | Consultas parametrizadas, lista blanca de campos permitidos para ordenar y filtrar, y usuario de base de datos de solo lectura para las consultas del catálogo. |
| | Abuso de la API sin límites de solicitudes. | Rate limiting y cuotas por cliente en el API Gateway, paginación obligatoria y límite en el tamaño de las respuestas. |
| | Alteración no autorizada de inventario o precios. | Endpoints de escritura solo para rol administrador, validación de rangos (por ejemplo, precio mayor a 0) y bitácora de auditoría de cada cambio. |
| Microservicio de pedidos | Acceso a pedidos ajenos por manipulación de identificadores (IDOR). | Comprobar que el pedido pertenece al usuario del token en cada consulta, identificadores UUID y pruebas automatizadas de autorización. |
| | Repetición de solicitudes (replay) que duplica pedidos o pagos. | Claves de idempotencia por pedido, tokens con expiración corta y nonce o marca de tiempo validada en el servidor. |
| | Denegación de servicio por saturación en la creación de pedidos. | Rate limiting por usuario, colas de mensajes para procesar pedidos de forma asíncrona, límites de tamaño de solicitud y autoescalado. |
| API Gateway | Ataques DDoS contra el punto de entrada. | Servicio anti-DDoS y WAF (por ejemplo, Cloudflare), rate limiting global, balanceo de carga y varias instancias del gateway. |
| | Omisión de la autenticación por mala configuración de rutas. | Política de denegar por defecto, validación del JWT en el gateway para todas las rutas protegidas, revisión de la configuración y pruebas de acceso a rutas sin token. |
| | Intercepción de tráfico (man-in-the-middle) si no se usa TLS. | TLS 1.2 o superior en todas las conexiones, certificados válidos con renovación automática, HSTS y redirección forzada de HTTP a HTTPS. |
| Bases de datos (usuarios, productos, pedidos) | Inyección SQL desde los servicios. | Consultas parametrizadas en todos los servicios, usuarios de base de datos distintos por servicio con privilegios mínimos y WAF frente a la API. |
| | Ransomware que cifra o borra la información. | Respaldos automáticos fuera de línea o inmutables, segmentación de red, base de datos sin acceso público y antimalware con actualizaciones al día en los servidores. |
| | Credenciales de base de datos expuestas o configuración por defecto. | Cambio de usuarios y contraseñas por defecto, secretos en un gestor (Vault o similar) y no en el código, y acceso solo desde la red interna de los servicios. |
| Servidores, contenedores y red | Ataques DDoS que saturan la red. | Protección anti-DDoS del proveedor o CDN, límites de tráfico en el firewall y balanceo de carga con autoescalado. |
| | Imágenes de contenedores vulnerables o con malware. | Imágenes base oficiales y mínimas, escaneo con Trivy en el pipeline, versiones fijas (sin latest) y contenedores ejecutados sin usuario root. |
| | Puertos y servicios expuestos por mala configuración en la nube. | Reglas de firewall o security groups que cierran todo lo que no sea necesario, revisión periódica de puertos abiertos (por ejemplo, con nmap) y configuración como código revisada. |
| Secretos y claves (JWT, API keys, variables de entorno) | Exposición en el repositorio o en archivos de configuración. | Gestor de secretos o variables de entorno inyectadas en el despliegue, .env fuera del repositorio y detección de secretos con gitleaks en cada commit. |
| | Robo desde un servidor o contenedor comprometido. | Secretos con permisos mínimos y de corta duración, cifrado en reposo, contenedores sin root y acceso al servidor solo con MFA. |
| | Falta de rotación de claves que permite su uso por personas que ya no deberían tener acceso. | Rotación periódica y automática de claves y tokens, revocación inmediata al salir un integrante del equipo y JWT con expiración corta. |
| Registros de actividad (logs) | Borrado o alteración de logs por un atacante para ocultar rastros. | Envío de los logs a un almacenamiento centralizado de solo escritura (append-only), permisos restringidos y firma o hash para detectar alteraciones. |
| | Registro de datos sensibles que luego se filtran. | No registrar contraseñas, tokens ni datos de tarjeta, enmascarar los datos personales en los logs y restringir quién puede leerlos. |
| | Saturación del almacenamiento por exceso de registros (log flooding). | Rotación y retención definida de logs, cuotas de almacenamiento, niveles de log adecuados en producción y alertas por uso de disco. |
| Copias de seguridad (backups) | Robo de respaldos sin cifrar. | Cifrado de los respaldos (AES-256) tanto en tránsito como en reposo, claves guardadas aparte y acceso restringido con MFA. |
| | Cifrado o borrado de los respaldos por ransomware. | Regla 3-2-1, copias inmutables o fuera de línea (air-gapped) y cuentas de respaldo separadas de las de producción. |
| | Fallos o corrupción de las copias que no se detectan a tiempo. | Verificación de integridad con checksums en cada respaldo, pruebas periódicas de restauración y alertas cuando un respaldo falla. |
