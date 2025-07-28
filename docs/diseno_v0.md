# Diseño de Pantallas - Versión 0

Este documento resume las pantallas y flujos mínimos necesarios para construir la versión funcional inicial del sistema **TUP-COINS** en Angular 19.

## Lista de Pantallas
1. Login / Autenticación
2. Dashboard Principal
3. Historial de Movimientos
4. Lista de Subastas
5. Detalle de Subasta (Tiempo Real)
6. Panel Profesor
7. Pantalla de Error / Acceso Denegado

---

## 1. Login / Autenticación
**Objetivo:** Autenticarse mediante email y contraseña, con opción a Google OAuth2.

### Estructura y Componentes
- Logo o imagen representativa del sistema.
- Campos para **Email** y **Contraseña**.
- Botón **"Ingresar"** y, opcionalmente, **"Ingresar con Google"**.
- Indicador de carga al enviar las credenciales.

### Flujos de Navegación
1. El usuario ingresa sus datos y presiona *Ingresar* (o elige *Ingresar con Google*).
2. Se validan las credenciales o se abre el flujo de Google OAuth2.
3. Si la autenticación es correcta, se navega al **Dashboard**.
4. Si la autenticación falla, se muestra el estado de error.

### Estados de UI
- **Normal:** Botón activo y disponible.
- **Cargando:** Botón deshabilitado y spinner visible.
- **Error:** Banner rojo con el texto *"Error de autenticación"*.

---

## 2. Dashboard Principal
**Objetivo:** Brindar al usuario una vista general de su información y accesos rápidos.

### Componentes y Secciones
- **Header**: nombre de usuario y botón de *logout*.
- **Tarjeta de Saldo**: tokens disponibles.
- **Botones de acceso rápido**:
  - *Ver Movimientos*: navega al historial.
  - *Ir a Subastas*: abre la lista de subastas.
- **Sección Últimas Actividades**: listado de los últimos 5 movimientos.

### Estados de UI
- Si el saldo es inferior a 10 tokens, mostrar mensaje de advertencia.

### Flujos
- Desde el Dashboard se puede ir a **Historial**, **Subastas** o al **Panel Profesor** (según rol).

---

## 3. Historial de Movimientos
**Objetivo:** Listar todas las operaciones realizadas por el usuario.

### Componentes
- **Tabla** con columnas: Fecha, Tipo (ASIGNACIÓN, PUJA, CONSUMO), Monto, Saldo resultante y Detalle.
- **Filtros** por tipo de movimiento y rango de fechas.
- **Paginación** de 10 ítems.

### Estados de UI
- **Sin datos**: mensaje *"Aún no tienes movimientos"*.
- **Cargando**: spinner y filtros deshabilitados.

---

## 4. Lista de Subastas
**Objetivo:** Mostrar las subastas disponibles y su estado.

### Componentes
- **Cards** con título, estado (ABIERTA o CERRADA) y fecha de finalización.
- Botón **"Entrar"** para abrir el detalle.
- Botón **"Volver"** para regresar al Dashboard.

### Flujos
- Al presionar en una subasta se abre la pantalla **Detalle de Subasta**.

---

## 5. Detalle de Subasta (Tiempo Real)
**Objetivo:** Permitir pujar y visualizar en tiempo real las ofertas de otros usuarios.

### Componentes
- Encabezado con título y contador de tiempo restante.
- Saldo del usuario.
- Lista de pujas en tiempo real (WebSocket). Muestra Usuario, Monto y Hora.
- Input y botón **"Pujar"** (validar monto > puja actual y que el usuario tenga saldo suficiente).

### Estados de UI
- **ABIERTA**: campo de puja activo.
- **CERRADA**: botón deshabilitado y mensaje con el ganador.

### Mensajes por WebSocket
- `NEW_BID`: Actualiza la lista de pujas y muestra aviso *"Nueva puja de [usuario]"*.
- `AUCTION_CLOSED`: Indica cierre, muestra modal con ganador y monto final.

### Validaciones
- Si el saldo no alcanza, el botón para pujar se muestra deshabilitado.

---

## 6. Panel Profesor
**Objetivo:** Administrar subastas y asignar tokens a los alumnos.

### Secciones Principales
1. **Gestión de Subastas**
   - Botón *Crear Subasta*: abre modal para ingresar título, descripción y fecha de fin.
   - Listado con subastas existentes: estado, total de pujas y botón *Cerrar*.
2. **Asignación de Tokens**
   - Inputs para indicar email/ID de alumno y cantidad.
   - Botón *Asignar*.

### Validaciones y Flujos
- Confirmar antes de cerrar una subasta.
- No superar el saldo máximo del alumno al asignar tokens.

---

## 7. Pantalla de Error / Acceso Denegado
**Casos de Uso**
- Usuario sin rol adecuado.
- Error de carga de datos u otra falla inesperada.

**Contenido Básico**
- Mensaje de error.
- Botón *"Volver al Dashboard"*.

---

## Mapa de Navegación
```
Login → Dashboard → (Historial | Subastas)
Subastas → Detalle de Subasta
Dashboard → Panel Profesor (sólo rol PROFESOR)
```

## Componentes Reutilizables
- **Header** con nombre de usuario y logout.
- **Modal** para confirmaciones y alta de subasta.
- **Tabla** reutilizable en movimientos y subastas.
- **Contador de tiempo** para subastas.

## Estados WebSocket en la UI
- Al recibir `NEW_BID`, actualizar la lista con animación y mostrar mensaje temporal.
- Al recibir `AUCTION_CLOSED`, desplegar modal informando ganador y valor final, con opción a volver a la lista de subastas.

---

Este esquema proporciona la base para implementar la versión mínima funcional del proyecto en Angular 19. Cada pantalla describe sus principales componentes y estados para que el equipo de frontend pueda desarrollarlas de forma consistente.

