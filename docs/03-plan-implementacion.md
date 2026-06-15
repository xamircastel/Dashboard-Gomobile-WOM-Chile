# Dashboard Gomobile WOM Chile - Plan de Implementación

## 1. Resumen Ejecutivo

Desarrollo de una plataforma web tipo dashboard para Gomobile que centraliza la visualización y gestión de resultados de monitoreo QA de servicios VAS para la operadora WOM Chile, reemplazando el modelo actual basado en archivos Excel.

---

## 2. Stack Tecnológico Propuesto

| Capa | Tecnología | Justificación |
|------|-----------|---------------|
| **Frontend** | Next.js 14+ (App Router) | SSR, sistema de rutas, buen DX, soporte TypeScript |
| **UI Components** | shadcn/ui + Tailwind CSS | Componentes accesibles, customizables, diseño moderno |
| **Gráficas** | Recharts o Chart.js | Flexibilidad para gráficos de barras, líneas, pie charts |
| **Backend API** | Next.js API Routes / Route Handlers | Unificado con el frontend, serverless-ready |
| **ORM** | Prisma | Type-safe, migraciones, compatible con PostgreSQL |
| **Base de Datos** | PostgreSQL | Robusta, soporte JSON, funciones de fecha avanzadas |
| **Autenticación** | NextAuth.js (Auth.js v5) | Gestión de sesiones, roles, invitaciones por email |
| **Email** | Resend / Nodemailer | Envío de invitaciones y activaciones |
| **Parsing Excel** | SheetJS (xlsx) / ExcelJS | Parsing de archivos Excel en el servidor |
| **Deploy** | Vercel + Supabase (o Railway para PostgreSQL) | Escalable, bajo costo inicial |
| **Storage temporal** | Memoria / tmp (no persistente) | Los archivos Excel se procesan y descartan |

---

## 3. Fases de Implementación

### FASE 1: Fundación (Semanas 1-2)

#### 1.1 Setup del Proyecto
- [ ] Inicializar proyecto Next.js con TypeScript
- [ ] Configurar Tailwind CSS + shadcn/ui
- [ ] Configurar ESLint + Prettier
- [ ] Configurar estructura de carpetas
- [ ] Inicializar repositorio Git con estructura de ramas

#### 1.2 Base de Datos
- [ ] Provisionar PostgreSQL (Supabase / Railway / Neon)
- [ ] Configurar Prisma con schema inicial
- [ ] Crear migraciones para todas las tablas
- [ ] Ejecutar seeds con datos de referencia:
  - Roles (admin, carrier, provider)
  - Proveedores (18 CPs)
  - Clubs (54 clubs)
  - Canales (7 canales)
  - Categorías de issues (7 categorías)
  - Catálogo de issues (63 códigos)
  - Calendario laboral de Chile 2025-2027
- [ ] Validar integridad referencial

#### 1.3 Autenticación Base
- [ ] Configurar NextAuth.js con credentials provider
- [ ] Implementar modelo de usuario con roles
- [ ] Crear middleware de protección de rutas
- [ ] Implementar lógica de filtrado por rol (provider → solo su data)

**Entregable Fase 1:** Proyecto base funcional con BD poblada y autenticación operativa.

---

### FASE 2: Sistema de Usuarios y Autenticación (Semanas 3-4)

#### 2.1 Gestión de Usuarios (Panel Admin)
- [ ] Formulario de creación de usuario (nombre, email, empresa, rol)
- [ ] Validación de campos y duplicados
- [ ] Asignación de proveedor para rol "provider"
- [ ] Listado de usuarios con filtros y paginación
- [ ] Activar/Desactivar usuarios

#### 2.2 Flujo de Invitación
- [ ] Generación de token de activación único
- [ ] Envío de email de invitación con link
- [ ] Página pública de establecimiento de contraseña
- [ ] Validación y expiración del token
- [ ] Confirmación de activación exitosa

#### 2.3 Login y Sesiones
- [ ] Página de login con email y contraseña
- [ ] Manejo de sesiones con JWT
- [ ] Redirección según rol post-login
- [ ] Logout y expiración de sesión
- [ ] Página de recuperación de contraseña

**Entregable Fase 2:** Sistema completo de gestión de usuarios con invitaciones por email.

---

### FASE 3: Ingesta de Datos (Semanas 5-7)

#### 3.1 Selección de Fechas
- [ ] Calendario interactivo con días laborales de Chile
- [ ] Selección de rango de fechas o fechas individuales
- [ ] Validación: no permitir fechas futuras
- [ ] Indicador visual de días ya cargados vs pendientes
- [ ] Distinción de fines de semana y feriados (no seleccionables)

#### 3.2 Carga de Archivos Excel/CSV
- [ ] Interfaz de upload (drag & drop + selector de archivos)
- [ ] Soporte multi-archivo (uno por día)
- [ ] Validación de nombre de archivo (formato esperado)
- [ ] Parsing del Excel en el servidor
- [ ] Validación de estructura del archivo (columnas esperadas)
- [ ] Mapeo de datos a esquema de BD
- [ ] Validaciones de integridad:
  - Canal, Proveedor, Club existen en catálogos
  - Código de issue existe
  - Fecha corresponde al archivo
- [ ] Proceso de inserción en BD con transacciones
- [ ] Reporte de resultado de carga (éxitos, errores, detalles)
- [ ] Registro en tabla data_loads

#### 3.3 Formularios de Ingesta Manual
- [ ] Formulario que replica el modelo del Excel
- [ ] Selector de Canal, Proveedor, Club (dropdowns encadenados)
- [ ] Selector de código de Issue (con búsqueda)
- [ ] Auto-completado de Categoría, Descripción, Nivel y Resultado
- [ ] Validaciones en tiempo real
- [ ] Guardado individual y por lote
- [ ] Carga secuencial día por día

#### 3.4 Edición de Datos
- [ ] Búsqueda de registros cargados por fecha y proveedor
- [ ] Tabla editable con los registros del día
- [ ] Modificación puntual de campos sin recargar todo
- [ ] Historial de ediciones (quién modificó qué y cuándo)
- [ ] Confirmación antes de guardar cambios

#### 3.5 Vista de Estado de Carga
- [ ] Calendario/tabla con estado por día:
  - ✅ Cargado correctamente
  - ⚠️ Carga parcial / con errores
  - ❌ Pendiente de carga
  - ⬜ No aplica (fin de semana / feriado)
- [ ] Filtro por mes/período
- [ ] Acceso rápido a editar o cargar un día específico
- [ ] Resumen estadístico (% de días cargados en el período)

#### 3.6 Carga Histórica Inicial
- [ ] Soporte para carga masiva de múltiples archivos
- [ ] Proceso batch de ingesta (últimos 12 meses)
- [ ] Barra de progreso para cargas extensas
- [ ] Manejo de errores sin detener toda la carga
- [ ] Reporte consolidado de carga histórica

**Entregable Fase 3:** Sistema completo de ingesta de datos funcional con ambos métodos.

---

### FASE 4: Dashboard - Visualizaciones (Semanas 8-10)

#### 4.1 Home / Vista General
- [ ] Card con cantidad total de proveedores activos
- [ ] Card con cantidad total de clubs/productos
- [ ] Tabla/gráfico resumen de clubs por proveedor
- [ ] Indicador de período de datos disponibles
- [ ] Gráfico circular: proporción OK vs Issue (últimos 4 meses)

#### 4.2 Sección: Fallas a Nivel General
- [ ] Gráfico de barras: Total de fallas por criticidad (Critico, Medio, Bajo)
- [ ] Gráfico de líneas: Tendencia mensual de fallas
- [ ] Gráfico de barras agrupadas: Fallas por categoría
- [ ] Tabla detallada: Top fallas más frecuentes
- [ ] Filtros:
  - Rango de fechas
  - Mes específico
  - Canal
- [ ] KPI cards: Total pruebas, % éxito, total issues

#### 4.3 Sección: Fallas por Integrador
- [ ] Selector de proveedor (o vista comparativa)
- [ ] Gráfico de líneas: Tendencia de fallas por proveedor en el tiempo
- [ ] Gráfico de barras: Fallas por criticidad por proveedor
- [ ] Tabla comparativa: Proveedores con más issues
- [ ] Desglose por club del proveedor seleccionado
- [ ] Filtros:
  - Proveedor específico
  - Rango de fechas
  - Categoría de issue
  - Nivel de criticidad

#### 4.4 Filtros Globales
- [ ] Selector de período (últimos 30 días, último mes, últimos 4 meses, rango custom)
- [ ] Selector de proveedor (solo visible para admin y carrier)
- [ ] Selector de canal
- [ ] Botón de reset de filtros
- [ ] Los filtros persisten durante la sesión de navegación

**Entregable Fase 4:** Dashboard con todas las visualizaciones funcionales.

---

### FASE 5: Refinamiento y Seguridad (Semanas 11-12)

#### 5.1 Restricciones de Rol en Vista
- [ ] Validar que rol "provider" solo ve su data en todas las vistas
- [ ] Ocultar secciones administrativas para carrier y provider
- [ ] Validar en backend (API) que las queries respetan el rol
- [ ] Pruebas de penetración básicas (acceso no autorizado)

#### 5.2 UX / UI Polish
- [ ] Diseño responsive (mobile, tablet, desktop)
- [ ] Integrar logos (Gomobile, WOM Chile) en header/sidebar
- [ ] Dark mode / Light mode
- [ ] Loading states y skeletons
- [ ] Mensajes de error amigables
- [ ] Tooltips en gráficos
- [ ] Exportación de gráficos a PDF/PNG (opcional)

#### 5.3 Performance
- [ ] Implementar vista materializada para KPIs mensuales
- [ ] Cacheo de queries frecuentes (Redis o in-memory)
- [ ] Lazy loading de gráficos
- [ ] Paginación en tablas de datos
- [ ] Optimización de bundle size

#### 5.4 Testing
- [ ] Tests unitarios para lógica de parsing Excel
- [ ] Tests unitarios para validaciones de ingesta
- [ ] Tests de integración para APIs
- [ ] Tests E2E para flujos críticos (login, carga, visualización)
- [ ] Tests de roles y permisos

**Entregable Fase 5:** Plataforma refinada, segura y optimizada.

---

### FASE 6: Deploy y Entrega (Semana 13)

#### 6.1 Despliegue
- [ ] Configurar entorno de producción
- [ ] Variables de entorno (DB, email, secrets)
- [ ] Dominio y SSL
- [ ] CI/CD pipeline (GitHub Actions)
- [ ] Monitoreo básico (logs, uptime)

#### 6.2 Carga Inicial de Datos
- [ ] Carga histórica de los últimos 12 meses de datos
- [ ] Validación de datos migrados
- [ ] Creación de usuarios iniciales (Gomobile, WOM, Proveedores)

#### 6.3 Documentación y Entrega
- [ ] Manual de usuario (Admin)
- [ ] Manual de usuario (Carrier/Provider)
- [ ] Documentación técnica (API, BD)
- [ ] Sesión de capacitación con equipo Gomobile
- [ ] Período de soporte post-entrega (2 semanas)

**Entregable Fase 6:** Plataforma en producción con datos históricos y usuarios creados.

---

## 4. Estructura de Carpetas del Proyecto

```
dashboard-gomobile-wom/
├── prisma/
│   ├── schema.prisma
│   ├── migrations/
│   └── seed.ts
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── login/
│   │   │   ├── activate/
│   │   │   └── forgot-password/
│   │   ├── (dashboard)/
│   │   │   ├── home/
│   │   │   ├── fallas-general/
│   │   │   ├── fallas-integrador/
│   │   │   └── layout.tsx
│   │   ├── (admin)/
│   │   │   ├── usuarios/
│   │   │   ├── ingesta/
│   │   │   │   ├── calendario/
│   │   │   │   ├── upload/
│   │   │   │   ├── formulario/
│   │   │   │   └── editar/
│   │   │   └── layout.tsx
│   │   ├── api/
│   │   │   ├── auth/
│   │   │   ├── users/
│   │   │   ├── data-loads/
│   │   │   ├── test-results/
│   │   │   ├── providers/
│   │   │   └── dashboard/
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/
│   │   ├── ui/ (shadcn)
│   │   ├── charts/
│   │   ├── forms/
│   │   ├── layout/
│   │   └── data-table/
│   ├── lib/
│   │   ├── auth.ts
│   │   ├── prisma.ts
│   │   ├── excel-parser.ts
│   │   ├── validations.ts
│   │   ├── permissions.ts
│   │   └── chilean-calendar.ts
│   ├── hooks/
│   ├── types/
│   └── constants/
├── public/
│   ├── logo-gomobile.png
│   └── wom-chile.png
├── docs/
├── .env.example
├── package.json
├── tsconfig.json
├── tailwind.config.ts
└── next.config.js
```

---

## 5. Cronograma Resumido

| Fase | Descripción | Duración |
|------|-------------|----------|
| 1 | Fundación (Setup + BD + Auth base) | 2 semanas |
| 2 | Sistema de Usuarios | 2 semanas |
| 3 | Ingesta de Datos | 3 semanas |
| 4 | Dashboard / Visualizaciones | 3 semanas |
| 5 | Refinamiento y Seguridad | 2 semanas |
| 6 | Deploy y Entrega | 1 semana |
| **Total** | | **13 semanas** |

---

## 6. Riesgos y Mitigaciones

| Riesgo | Impacto | Mitigación |
|--------|---------|------------|
| Formato de Excel inconsistente entre archivos históricos | Alto | Implementar parser flexible con mapeo configurable y reporte de errores detallado |
| Volumen de datos en carga histórica (12 meses) | Medio | Procesamiento batch con transacciones parciales, no todo-o-nada |
| Feriados de Chile cambian por año | Bajo | Tabla de calendario actualizable, fuente oficial SERNAC/DTT |
| Cambio en catálogo de issues | Medio | Admin panel para gestión de catálogos (fase futura) |
| Múltiples usuarios cargando datos al mismo tiempo | Bajo | Control de concurrencia por fecha + proveedor |

---

## 7. Consideraciones de Seguridad

- Hashing de contraseñas con bcrypt (mínimo 12 rounds)
- Tokens de activación con expiración (24-48 horas)
- Rate limiting en endpoints de login
- Validación de input en servidor (nunca confiar solo en frontend)
- CORS configurado correctamente
- Headers de seguridad (HSTS, CSP, X-Frame-Options)
- Sanitización de datos del Excel antes de inserción
- Queries parametrizadas (Prisma lo hace por defecto)
- Audit log de acciones administrativas

---

## 8. Métricas de Éxito

| Métrica | Objetivo |
|---------|----------|
| Tiempo de carga de página | < 3 segundos |
| Tiempo de procesamiento de Excel (1 día) | < 5 segundos |
| Tiempo de procesamiento de carga masiva (12 meses) | < 2 minutos |
| Disponibilidad | 99.5% uptime |
| Exactitud de datos | 100% match entre Excel y BD |

---

## 9. Próximos Pasos Inmediatos

1. **Validar el plan** con el equipo de Gomobile
2. **Confirmar stack tecnológico** y hosting
3. **Obtener acceso** a archivos Excel históricos reales para validar el parser
4. **Confirmar listado** de feriados Chile 2025-2026 con fuente oficial
5. **Iniciar Fase 1** - Setup del proyecto y base de datos
