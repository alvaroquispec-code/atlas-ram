# Atlas RAM

**Plataforma de antibiogramas acumulados para una terapia antibiótica empírica informada**

Proyecto 2 de CS 2031 Desarrollo Basado en Plataformas · UTEC · 2026-2

Este es el repositorio principal del equipo. Reúne los tres componentes del proyecto como submódulos de git: cada carpeta apunta a su propio repositorio, en la versión exacta que se entrega.

| Carpeta | Componente | Repositorio | En producción |
|---|---|---|---|
| [`backend/`](https://github.com/alvaroquispec-code/atlas-ram-backend) | API REST · Spring Boot 3.5 · Java 21 · PostgreSQL 16 | [atlas-ram-backend](https://github.com/alvaroquispec-code/atlas-ram-backend) | https://3-85-201-152.sslip.io/api/v1 (AWS EC2 + RDS, HTTPS) |
| [`web/`](https://github.com/alvaroquispec-code/atlas-ram-web) | Frontend web · React 19 · TypeScript · Tailwind 4 | [atlas-ram-web](https://github.com/alvaroquispec-code/atlas-ram-web) | https://atlas-ram-web.vercel.app (Vercel) |
| [`mobile/`](https://github.com/alvaroquispec-code/atlas-ram-mobile) | App móvil · React Native · Expo SDK 58 | [atlas-ram-mobile](https://github.com/alvaroquispec-code/atlas-ram-mobile) | [APK Android](https://github.com/alvaroquispec-code/atlas-ram-mobile/releases/latest/download/atlas-ram.apk) (EAS Build) |

**Usuario de prueba:** `admin@atlasram.test` / `AtlasRam2026!` (datos sintéticos de demostración). También hay técnicos de laboratorio, por ejemplo `lab.hnch@atlasram.test`, con la misma contraseña.

## Qué resuelve

Cuando llega un paciente con una infección grave, el cultivo tarda entre 48 y 72 horas, pero el médico debe elegir un antibiótico de inmediato (terapia empírica). Atlas RAM reúne los resultados que los laboratorios ya generan y calcula, por distrito, qué porcentaje de cada bacteria resiste a cada antibiótico (antibiograma acumulado, CLSI M39), para que esa primera elección se haga con datos locales.

## Arquitectura

```
 Web (Vercel, HTTPS) ──┐
                       ├──► Caddy :443 (HTTPS) ──► Spring Boot :8080 ──► RDS PostgreSQL 16
 Móvil (APK, HTTPS) ───┘        AWS EC2                  │                (migraciones Flyway)
                                                         └──► Nominatim (OpenStreetMap), desde el móvil
```

El detalle de cada componente (endpoints, modelo de datos, seguridad, manejo de errores, vistas, sensores y despliegue) está en el README de cada repositorio.

## Integrantes y participación

El porcentaje indica cuánto cumplió cada integrante de sus responsabilidades en cada fase (100 % = cumplimiento completo).

| Integrante | Principales aportes | Fase 1 (API) | Fase 2 (web, móvil y despliegue) |
|---|---|:---:|:---:|
| Álvaro Felipe Quispe Carrillo | Modelo de dominio y lógica de la API; despliegue completo (AWS EC2, RDS, HTTPS con Caddy, Vercel, EAS); web: panel de resistencia, aislamientos, usuarios y catálogo; base de la app móvil | 100 % | 100 % |
| Joaquim Alexander Carrión Díaz | Fase 1: autenticación JWT con refresh tokens y autorización por rol (#5). Fase 2: web, con login y registro, cliente API con interceptores y rutas protegidas | 100 % | 100 % |
| Adriano Alonso Cosme Salazar | Fase 1: ingesta asíncrona de CSV, eventos de dominio y notificaciones (#8). Fase 2: app móvil, con escáner QR, registro de aislamientos y publicación del APK con EAS | 100 % | 100 % |
| Fabian Isaias Sanchez Zedano | Fase 1: excepciones propias, manejo global de errores y endpoints de la API (#6). No participó en la fase 2 | 100 % | 0 % |

## Clonar todo

```bash
git clone --recurse-submodules https://github.com/alvaroquispec-code/atlas-ram.git
```

Si ya se clonó sin submódulos: `git submodule update --init`. Para traer la última versión de los tres: `git submodule update --remote`.
