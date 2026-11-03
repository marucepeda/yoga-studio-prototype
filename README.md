# Estudio de Yoga — MVP

Gestión digital de clases, paquetes y reservas para un estudio de yoga.

Proyecto de Fundamentos de Product Engineering con IA · Código Facilito

---

## ¿Qué es esto?

Aplicación web para la gestión de un pequeño estudio de yoga. Reemplaza el control manual de paquetes de clases (tarjetas físicas) y la gestión informal de reservas por un sistema digital sencillo, confiable y accesible desde el celular.

**Problema que resuelve:** Andrea, administradora del estudio, perdía tiempo y generaba fricciones con alumnas por desacuerdos sobre saldos de clases, clases que se llenaban sin aviso, y la dificultad de comunicar cancelaciones a todas las inscriptas.

---

## Funcionalidades del MVP

- Gestión de alumnas y sus datos
- Paquetes de clases con historial de movimientos trazable
- Agenda semanal de clases con capacidad configurable
- Reservas con verificación de cupo y saldo disponible
- Lista de espera automática cuando una clase se completa
- Comunicación de cancelaciones vía WhatsApp (mensaje prellenado, sin API externa)
- Control de asistencia por parte de las instructoras
- Tres roles diferenciados: administradora, instructoras y alumnas

---

## Stack tecnológico

| Capa | Tecnología |
| --- | --- |
| Framework | Next.js 14 (App Router) |
| Base de datos | PostgreSQL (Supabase) |
| ORM | Prisma |
| Autenticación | Supabase Auth + RLS |
| Deploy | Vercel |

---

## Estructura del proyecto

```
/
├── app/                  # Rutas y páginas (Next.js App Router)
│   ├── (admin)/          # Rutas de administración
│   ├── (instructor)/     # Rutas de instructoras
│   └── (alumna)/         # Rutas de alumnas
├── components/           # Componentes reutilizables
├── lib/                  # Lógica de negocio y utilidades
├── prisma/
│   └── schema.prisma     # Modelo de datos
└── .env.example          # Variables de entorno requeridas
```

---

## Cómo correrlo localmente

### Requisitos previos

- Node.js 18+
- Cuenta en [Supabase](https://supabase.com) (plan gratuito alcanza)

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/tu-usuario/yoga-studio-mvp
cd yoga-studio-mvp

# 2. Instalar dependencias
npm install

# 3. Configurar variables de entorno
cp .env.example .env.local
# Completar con las credenciales de tu proyecto Supabase

# 4. Ejecutar migraciones
npx prisma migrate dev

# 5. Cargar datos de prueba (opcional)
npx prisma db seed

# 6. Iniciar el servidor de desarrollo
npm run dev
```

Abrí <http://localhost:3000> en el navegador.

---

## Variables de entorno

Copiá `.env.example` a `.env.local` y completá los valores:

```env
DATABASE_URL=
DIRECT_URL=
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
```

Todos los valores se obtienen del dashboard de tu proyecto en Supabase.

---

## Usuarios de prueba

Después de correr el seed, podés ingresar con:

| Rol | Email | Contraseña |
| --- | --- | --- |
| Administradora | andrea@estudio.com | demo1234 |
| Instructora | laura@estudio.com | demo1234 |
| Alumna | maria@estudio.com | demo1234 |

---

## Decisiones de diseño

**Una única fuente de verdad.** El saldo de clases se deriva siempre del historial de transacciones, nunca de un campo editable directamente. Elimina los desacuerdos.

**Reservas atómicas.** Las reservas usan `SELECT FOR UPDATE` en PostgreSQL para evitar sobrecupos cuando dos alumnas intentan tomar el último lugar al mismo tiempo.

**WhatsApp sin API.** La comunicación de cancelaciones genera un enlace `wa.me` con el mensaje prellenado. Funciona sin costo ni integración externa.

**Row Level Security.** Cada alumna solo puede leer sus propios datos a nivel de base de datos, no solo a nivel de código.

---

## Autor

[Maru](https://github.com/marucepeda)
