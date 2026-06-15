# Dashboard Gomobile WOM Chile - Documento Maestro de Contexto

## 1. Información General del Proyecto

| Campo | Detalle |
|-------|---------|
| **Nombre del Proyecto** | Dashboard Gomobile WOM Chile |
| **Proveedor Tecnológico** | Xafratech |
| **Cliente Directo** | Gomobile |
| **Cliente Final** | WOM Chile (Operadora Móvil) |
| **Objetivo** | Centralizar y visualizar las métricas de monitoreo de servicios VAS en un dashboard digital |

---

## 2. Partes Involucradas

### 2.1 WOM Chile (Operadora Móvil)
- Operadora de telefonía móvil en Chile.
- Ofrece servicios de Valor Agregado (VAS) a sus usuarios a través de proveedores de contenido.
- Los cargos de VAS se aplican a la facturación mensual (postpago) o se debitan del saldo de recarga (prepago).
- **Rol en el dashboard:** Visualización de métricas y KPIs (rol Carrier).

### 2.2 Gomobile
- Partner asignado por WOM Chile para la ejecución de pruebas QA de los servicios VAS.
- Realiza pruebas periódicas (diarias en días laborales de Chile) a los servicios de los proveedores de contenido.
- Posee equipos móviles y SIM cards con diferentes líneas y tipos de planes para las pruebas.
- Documenta resultados en archivos Excel con formato estructurado.
- **Rol en el dashboard:** Administración total del sistema (rol Admin).

### 2.3 Proveedores de Contenido (CP - Content Providers)
- Empresas poseedoras de contenidos digitales conectadas a WOM Chile para ofrecer servicios VAS.
- Cada proveedor puede tener múltiples "Clubs" (productos/servicios).
- **Rol en el dashboard:** Visualización limitada a sus propios datos (rol Proveedor).

### 2.4 Xafratech
- Proveedor tecnológico contratado por Gomobile.
- Responsable del desarrollo y soporte del dashboard.
- No tiene acceso operativo al dashboard en producción.

---

## 3. Listado de Proveedores de Contenido (18 CPs)

| # | Proveedor | Clubs |
|---|-----------|-------|
| 1 | Opratel | Club 1, Club 2, Club 3 |
| 2 | Chocolate | Club 1, Club 2, Club 3 |
| 3 | Renxo | Club 1, Club 2, Club 3 |
| 4 | Digital Virgo | Club 1, Club 2, Club 3 |
| 5 | Bennu | Club 1, Club 2, Club 3 |
| 6 | T-Mobs | Club 1, Club 2, Club 3 |
| 7 | airG | Club 1, Club 2, Club 3 |
| 8 | Zed | Club 1, Club 2, Club 3 |
| 9 | GPM | Club 1, Club 2, Club 3 |
| 10 | Celcom | Club 1, Club 2, Club 3 |
| 11 | AWG | Club 1, Club 2, Club 3 |
| 12 | Cykadas | Club 1, Club 2, Club 3 |
| 13 | FaroMobile | Club 1, Club 2, Club 3 |
| 14 | KMD | Club 1, Club 2, Club 3 |
| 15 | True Caller | Club 1, Club 2, Club 3 |
| 16 | Wasabi | Club 1, Club 2, Club 3 |
| 17 | SocialAst | Club 1, Club 2, Club 3 |
| 18 | Digevo | Club 1, Club 2, Club 3 |

**Total:** 18 proveedores × 3 clubs = 54 clubs/productos

---

## 4. Canales de Prueba

Los servicios VAS se prueban a través de los siguientes canales:

| Canal | Descripción |
|-------|-------------|
| SMS | Mensajes de texto |
| SAT | SIM Application Toolkit |
| Web | Portales web |
| Wap | Portales WAP / mobile web |
| Sim | Servicios embebidos en SIM |
| Tienda | Tiendas físicas del operador |
| Otro | Otros canales (IVR, etc.) |

---

## 5. Sistema de Roles y Permisos

### 5.1 Admin (Equipo Gomobile)
- Acceso completo a todas las secciones.
- Gestión administrativa del dashboard.
- Carga de archivos Excel / CSV para ingesta de datos.
- Creación y gestión de usuarios.
- Edición de datos históricos.
- Diligenciamiento de formularios de ingesta.

### 5.2 Carrier (Operadora WOM Chile)
- Acceso a toda la información y gráficas.
- Sin capacidades administrativas.
- No puede cargar ni modificar data.
- Visualización completa del ecosistema.

### 5.3 Proveedor (Proveedores de Contenido)
- Acceso limitado a información de su propio proveedor.
- Sin capacidades administrativas.
- Solo ve gráficos e información relacionada con su empresa.

---

## 6. Gestión de Usuarios

### Flujo de Creación de Usuario:
1. Admin crea usuario desde el dashboard especificando:
   - Nombre de usuario
   - Empresa (proveedor de contenido u operadora)
   - Correo electrónico de contacto
   - Rol asignado
2. Sistema envía email al correo especificado con link de activación.
3. Usuario accede al link y establece su contraseña.
4. El correo electrónico será siempre el usuario de acceso.

---

## 7. Modelo de Ingesta de Datos

### 7.1 Métodos de Ingesta

#### A) Carga de Archivos Excel/CSV
- El equipo de Gomobile sube los archivos con los resultados de las pruebas.
- Se permite carga de múltiples archivos (uno por día).
- Los archivos deben tener un nombre y estructura específicos para identificar la fecha.
- Los archivos NO se almacenan permanentemente; solo se extrae la información.
- La data se persiste en la base de datos.

#### B) Formularios Manuales
- Interfaz digital que replica el modelo del Excel actual.
- Se ingresan valores input que generan cálculos internos para outputs.
- Un formulario por día de pruebas.
- Se diligencian secuencialmente de la fecha más antigua a la más reciente.

### 7.2 Reglas de Ingesta
- Solo se puede ingestar información de fechas pasadas o del día actual (nunca futuras).
- Frecuencia: diaria en días laborales de Chile.
- Se debe seleccionar primero la(s) fecha(s) de ingesta.
- Luego se elige el método (Excel o formulario).
- Carga histórica: se permite subir datos de los últimos 12 meses al inicio del proyecto.

### 7.3 Edición de Datos
- Se permite edición de datos ya cargados para corregir errores.
- La edición se realiza mediante formulario (sin necesidad de recargar todo el Excel).
- Ejemplo: si se identifica un error después de cargar el archivo, se corrige puntualmente.

### 7.4 Vista de Estado de Carga
- Vista que muestra claramente qué días tienen información cargada y cuáles no.
- Permite identificar días pendientes de carga.
- Indica si la información de un día tiene errores de carga.
- Confirma cuando la información de un día está correctamente cargada.

---

## 8. Estructura de Datos del Excel (Modelo Lógico)

### 8.1 Hoja Principal "DB" (1000 registros de ejemplo)

| Columna | Tipo | Descripción |
|---------|------|-------------|
| Fecha | Date | Fecha de la prueba |
| Mes | String | Mes en texto (enero, febrero, etc.) |
| Año | Integer | Año de la prueba |
| Canal | String | Canal de prueba (SMS, SAT, Web, Wap, Sim, Tienda, Otro) |
| Proveedor | String | Nombre del proveedor de contenido |
| Club | String | Nombre del club/producto (formato: "Proveedor - Club N") |
| ISSUE | Integer | Código de falla (referencia a catálogo de Issues) |
| Categoria | String | Categoría de la prueba |
| DESCRIPCION | String | Descripción del resultado de la prueba |
| Nivel | String | Nivel de criticidad (Prueba OK, Critico, Medio, Bajo) |
| Resultado | String | Resultado binario (OK / Issue) |

### 8.2 Lógica de Resultados
- **Resultado = "OK"**: La prueba fue exitosa. El campo Nivel = "Prueba OK".
- **Resultado = "Issue"**: Se encontró una falla. El campo Nivel puede ser: Critico, Medio, o Bajo.

### 8.3 Niveles de Criticidad
| Nivel | Descripción |
|-------|-------------|
| Prueba OK | La prueba se completó exitosamente |
| Critico | Falla crítica que impide el funcionamiento del servicio |
| Medio | Falla de impacto medio, servicio funciona con defectos |
| Bajo | Falla menor, impacto bajo en la experiencia |

---

## 9. Catálogo de Issues (Códigos de Falla)

### Categoría (100) - Alta (Suscripción)
| Código | Descripción | Nivel |
|--------|-------------|-------|
| 100 | Alta con éxito | Prueba OK |
| 101 | No se generó el alta - no hay respuesta al kw | Critico |
| 102 | Falla en doble optin en sms | Critico |
| 103 | Inconsistencia en el welcome | Critico |
| 104 | No llega pin code | Medio |
| 105 | Inconsistencia en el pin code | Medio |
| 106 | Falla en lp de suscripción web | Critico |
| 107 | Falla en lp de suscripción wap - header enrichment | Critico |
| 108 | Inconsistencias en t&c en la lp | Critico |
| 109 | Fallas en estructura o diseño de la lp | Medio |
| 110 | Falla en alta a través de ivr | Critico |
| 111 | Falla en alta a través de sim | Critico |
| 112 | Falla en alta a través de tienda | Critico |
| 113 | Servicio suspendido | Medio |
| 114 | Falla en doble optin en sim | Critico |

### Categoría (200) - Contenidos
| Código | Descripción | Nivel |
|--------|-------------|-------|
| 200 | Contenidos recibido con éxito | Prueba OK |
| 201 | Entrega de mensaje fuera de horario establecido | Critico |
| 202 | Inconsistencias en textos/fraseología sms | Medio |
| 203 | Entrega de mensajería sms con delay | Bajo |
| 204 | Error/inconsistencia en t&c en sms | Critico |
| 205 | Llegó contenido de otro club | Critico |
| 206 | Llegó doble contenido | Medio |
| 207 | Contenido repetido | Medio |
| 208 | No cumple las políticas de estructura del operador | Critico |
| 209 | Contenido desactualizado | Medio |
| 210 | Contenido no apropiado - fuera de las políticas del operador | Critico |

### Categoría (300) - Cobros
| Código | Descripción | Nivel |
|--------|-------------|-------|
| 300 | Cobro en valor y forma correcto | Prueba OK |
| 301 | No llegó contenido (si hubo cobro) | Critico |
| 302 | No llegó contenido (no hubo cobro) | Critico |
| 303 | Llegó contenido (no hubo cobro) | Critico |
| 304 | Cobro valor diferente al autorizado | Critico |
| 305 | Cobro doble vez | Critico |
| 306 | Inconsistencias en la recurrencia del cobro | Critico |
| 307 | Inconsistencias con el cobro escalonado | Critico |
| 308 | No entrega confirmación de cobro | Critico |

### Categoría (400) - Portales / Web-App
| Código | Descripción | Nivel |
|--------|-------------|-------|
| 400 | Portal evaluado con éxito | Prueba OK |
| 401 | Problemas con las credenciales y acceso a portal / web-app | Critico |
| 402 | Portal / web-app caída | Critico |
| 403 | Portal desactualizado | Medio |
| 404 | No cumple las políticas de estructura del operador | Critico |
| 405 | Problemas de estructura o textos del portal o web-app | Medio |
| 406 | No descarga contenidos - problemas con la descarga | Critico |
| 407 | No funciona la interacción | Critico |
| 408 | Inconsistencias en los t&c del portal / web-app | Critico |
| 409 | No cumple con características de diseño responsive | Critico |

### Categoría (500) - Baja / Cancelación
| Código | Descripción | Nivel |
|--------|-------------|-------|
| 500 | Baja con éxito | Prueba OK |
| 501 | No funciona el comando de baja | Critico |
| 502 | No entrega el mt de baja | Critico |
| 503 | Dio de baja en sms pero sigue activo en la plataforma | Critico |
| 504 | No funciona la baja desde la lp | Critico |
| 505 | No funciona la baja desde tienda | Critico |
| 506 | No funciona la baja desde ivr | Critico |
| 507 | No funciona la baja desde portal / web app | Critico |
| 508 | No permite dar baja por no tener saldo | Critico |

### Categoría (700) - Interacción MO/MT (On Demand)
| Código | Descripción | Nivel |
|--------|-------------|-------|
| 700 | Flujo MO/MT con éxito | Prueba OK |
| 701 | No envía el mo | Critico |
| 702 | No se recibe mt | Critico |
| 703 | Fallas en la interacción mo/mt | Critico |
| 704 | Fallas en puntajes, trivias, créditos, etc. | Critico |
| 705 | No funcionan los kw's especiales (info, puntaje, ayuda, etc) | Medio |

### Categoría (800) - Políticas Especiales Operadores
| Código | Descripción | Nivel |
|--------|-------------|-------|
| 800 | Políticas de los operadores cumplidas | Prueba OK |
| 801 | No da respuesta a kw regulatorio | Critico |
| 802 | Falla en colilla legal con info del cp | Critico |

---

## 10. Secciones del Dashboard

### 10.1 Home (Página Principal)
- Gráficas de alto nivel.
- Cantidad total de proveedores de contenido.
- Resumen de cantidad de productos (clubs) por integrador/proveedor.

### 10.2 Fallas a Nivel General
- Cantidad total de fallas encontradas durante las pruebas.
- Agrupación por nivel de criticidad (Critico, Medio, Bajo).
- Filtros por período de tiempo.

### 10.3 Fallas por Integrador
- Tendencias de fallas por proveedor de contenido.
- Filtros por período de tiempo.
- Desglose por criticidad.
- Comparativa entre proveedores.

### 10.4 Administración (Solo Admin)
- Gestión de usuarios.
- Ingesta de datos (carga de Excel / formularios).
- Vista de estado de carga por día.
- Edición de datos históricos.

---

## 11. KPIs y Métricas a Visualizar

Basado en las tablas dinámicas del Excel:

1. **Pruebas Exitosas vs Issues** - Conteo total OK vs Issue por período.
2. **Issues por Categoría** - Distribución de fallas por categoría (Alta, Contenidos, Cobros, etc.).
3. **Issues por Descripción** - Desglose detallado por tipo específico de falla.
4. **Issues por Proveedor y Categoría** - Cruce proveedor × categoría con resultado OK/Issue.
5. **Tendencia Mensual por Criticidad** - Evolución de Critico/Medio/Bajo por mes.
6. **Issues por Canal** - Distribución de fallas por canal de prueba.

---

## 12. Vistas según la hoja "tablas dinámicas"

| Vista | Alcance | Filtros |
|-------|---------|---------|
| Home | General - Todo el ecosistema | Últimos 4 meses |
| Integradores | Todo el ecosistema por integrador | - |
| Integradores (Clubs) | Todo el ecosistema por club | - |
| Ecosistema - Fallas | General y por integrador y/o club | - |
| Ecosistema - Clubs | General y por integrador y/o club | - |

---

## 13. Assets del Proyecto

| Archivo | Descripción |
|---------|-------------|
| `assets/logo-gomobile.png` | Logo de Gomobile |
| `assets/WOM_Chile.svg.png` | Logo de WOM Chile |
| `2026 Database.xlsx` | Modelo lógico de la base de datos con datos de ejemplo |
| `2026 05 Modelo presentacion informe ejecutivo mensual.pdf` | Modelo de presentación del informe ejecutivo mensual |

---

## 14. Consideraciones Técnicas

- El dashboard debe funcionar con calendario laboral de Chile (excluir fines de semana y feriados).
- Los archivos Excel son el medio de transporte, no de almacenamiento permanente.
- La base de datos es la fuente de verdad para todas las visualizaciones.
- Se debe garantizar la integridad referencial entre proveedores, clubs, categorías y códigos de falla.
- El sistema debe manejar la carga masiva histórica (12 meses aproximadamente).
