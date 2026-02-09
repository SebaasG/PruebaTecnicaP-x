

---

# Prueba Técnica - RPA con PIX RPA

## Descripción

Este proyecto automatiza un proceso RPA usando la plantilla universal de **PIX RPA**, realizando:

* Consumo de API pública (Fake Store API)
* Respaldo en JSON
* Inserción en base de datos PostgreSQL (evitando duplicados)
* Generación de reporte Excel con resumen
* Subida automática a OneDrive con Microsoft Graph API
* Envío del reporte mediante formulario web
* Evidencia de confirmación

---

## API usada

* Endpoint: `https://fakestoreapi.com/products`

Campos extraídos:

* `id`, `title`, `price`, `category`, `description`

---

## Base de Datos (PostgreSQL)

Tabla utilizada: `productos`

```sql
CREATE TABLE productos (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    category TEXT NOT NULL,
    description TEXT,
    fecha_insercion TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

📌 Se valida que el `id` no exista antes de insertar para evitar duplicados.

---

## Reporte Excel generado

Nombre:

* `Reporte_YYYY-MM-DD.xlsx`

Contenido:

* Hoja 1: Productos
* Hoja 2: Resumen (totales y promedios por categoría)

---

## OneDrive (Microsoft Graph API)

Se suben automáticamente:

* JSON de respaldo
* Excel del reporte

Autenticación usada:

* Sin interacción del usuario (client credentials)

---

## Formulario Web usado

Formulario para envío automático del reporte:

[https://docs.google.com/forms/d/e/1FAIpQLSeBX8GWCe2O40vjc6yohpueWmMOlOxn54mflgznZMgvY8XaFw/viewform](https://docs.google.com/forms/d/e/1FAIpQLSeBX8GWCe2O40vjc6yohpueWmMOlOxn54mflgznZMgvY8XaFw/viewform)

El robot:

* Completa los campos requeridos
* Sube el Excel generado
* Envía el formulario
* Guarda evidencia de confirmación

---

## Configuración Importante (Rutas)

Antes de ejecutar el robot, es necesario configurar las rutas dentro del **config del proyecto**, para que el proceso encuentre correctamente los archivos.

Se deben configurar estas keys:

* `RutaExcel`
* `rutaScript`

---

## Evidencias

Se genera una captura del envío exitoso del formulario en:

* `/Evidencias/formulario_confirmacion.png`

---

## Ejecución

1. Configurar las rutas en el config del proyecto (`RutaExcel` y `rutaScript`).
2. Ejecutar el flujo principal desde PIX RPA.
3. El robot realiza el proceso completo automáticamente (API → BD → Excel → OneDrive → Formulario → Evidencia).

---


