# 🛠️🤖 TaskBoard — Semana 16

![Laravel](https://img.shields.io/badge/Laravel-12-red?logo=laravel)
![PHP](https://img.shields.io/badge/PHP-8.2-blue?logo=php)
![MySQL](https://img.shields.io/badge/MySQL-MariaDB-orange?logo=mysql)
![Bruno](https://img.shields.io/badge/Bruno-API_Testing-orange)
![Sanctum](https://img.shields.io/badge/Laravel-Sanctum-red)
![UPED](https://img.shields.io/badge/UPED-Integración_de_Sistemas-0B2E59)

Proyecto desarrollado para la asignatura **Integración de Sistemas** de la **Universidad Pedagógica de El Salvador (UPED)**.

Durante la **Semana 16** se trabajó el uso crítico de IA, mantenimiento correctivo y evolutivo, pruebas de regresión y documentación de cambios sobre la API de TaskBoard.

**Estudiante:** Josthyn Stanley Cruz Vásquez  
**Carrera:** Ingeniería en Sistemas y Computación  
**Universidad:** Universidad Pedagógica de El Salvador  
**Ciclo:** 02-2026  

---

# 🎯 Objetivo general

Revisar críticamente código asistido por IA, integrar un filtro seguro de transacciones y aplicar mantenimiento correctivo y evolutivo sin romper el comportamiento existente de la API.

---

# 📅 Jueves — Uso crítico de IA

Se revisaron respuestas generadas por IA utilizando un método de seis pasos:

```text
1. Leer
2. Contrastar
3. Seguridad
4. Ejecutar
5. Probar
6. Justificar
```

Se identificaron problemas como:

- rutas sin protección
- orden incorrecto de rutas
- falta de filtro por usuario
- SQL crudo
- fechas sin validar
- ausencia de Resource

También se comprobó mediante Tinker el riesgo de concatenar datos directamente en una consulta SQL.

---

# 🔎 Filtro de transacciones

Se agregó el endpoint:

```text
GET /api/transacciones/filtrar
```

Permite filtrar transacciones por:

```text
estado
desde
hasta
monto_min
monto_max
```

La ruta está protegida con Sanctum y se registra antes de la ruta dinámica `{transaccion}`.

---

# 🔧 Viernes — Mantenimiento correctivo MT-013

Problema detectado:

```text
GET /api/transacciones?estado=Pagada
```

Antes respondía:

```text
200 OK
data: []
```

Se agregó validación para aceptar únicamente:

```text
Iniciada
Completada
Fallida
```

Ahora un estado inválido responde:

```text
422 Unprocessable Entity
```

---

# ➕ Mantenimiento evolutivo MT-014

Se agregaron los parámetros opcionales:

```text
monto_min
monto_max
```

Reglas principales:

```text
monto_min >= 0
monto_max >= 0
monto_max >= monto_min cuando ambos están presentes
```

La validación de `monto_max` se aplica de forma condicional mediante `Rule::when()`.

---

# 🧪 Pruebas en Bruno

Se comprobaron:

- autenticación
- aislamiento por usuario
- validación de estados
- validación de fechas
- intento de inyección
- filtros por monto
- compatibilidad con pruebas anteriores

Resultado de regresión:

```text
Passed: 28
Failed: 0
```

---

# 🔄 Compatibilidad del contrato

Se realizó un experimento renombrando temporalmente:

```text
cliente
```

por:

```text
nombre_cliente
```

La prueba demostró que eliminar un campo existente puede romper a los clientes de la API.

La forma compatible consiste en mantener el campo anterior durante un periodo de transición antes de retirarlo.

---

# 📝 Registro de cambios

Se creó:

```text
CAMBIOS.md
```

con:

```text
MT-013 → Corregido
MT-014 → Agregado
```

---

# 📡 API · Filtro de transacciones

## Endpoint

```text
GET /api/transacciones/filtrar
```

## Autenticación

Requiere:

```text
Authorization: Bearer <token>
```

## Parámetros opcionales

| Parámetro | Tipo | Regla |
|---|---|---|
| `estado` | texto | `Iniciada`, `Completada` o `Fallida` |
| `desde` | fecha | Fecha válida |
| `hasta` | fecha | Igual o posterior a `desde` |
| `monto_min` | número | Mayor o igual a `0` |
| `monto_max` | número | Mayor o igual a `0` y a `monto_min` cuando ambos existen |

El endpoint devuelve únicamente las transacciones pertenecientes al usuario autenticado.

## Códigos principales

```text
200 → Consulta correcta
401 → Usuario no autenticado
422 → Parámetros inválidos
```

## Ejemplo

```text
GET /api/transacciones/filtrar?estado=Completada&monto_min=20&monto_max=100
```

---

# ✅ Resultado final

```text
Filtro por estado y fechas      → OK
Filtro por rango de monto       → OK
MT-013                          → Corregido
MT-014                          → Implementado
Regresión Bruno                 → 28/28
CAMBIOS.md                      → Creado
Documentación del endpoint      → Completada
```

---

# 🛠️ Tecnologías utilizadas

- Laravel 12
- PHP 8.2
- MySQL / MariaDB
- Laravel Sanctum
- Eloquent ORM
- REST
- JSON
- Bruno
- Tinker
- Git
- GitHub
- XAMPP

---

# ▶️ Ejecución del proyecto

```bash
git clone https://github.com/StanleyJv/taskboard-semana16.git
cd taskboard-semana16
composer install
php artisan key:generate
php artisan migrate --seed
php artisan serve
```

API:

```text
http://127.0.0.1:8000/api
```

---

# 🔗 Repositorio

https://github.com/StanleyJv/taskboard-semana16

---

**Integración de Sistemas · Ciclo 02-2026 · UPED**
