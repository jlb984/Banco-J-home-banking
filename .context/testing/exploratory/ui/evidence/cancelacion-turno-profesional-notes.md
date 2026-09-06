# Evidencia textual: Cancelación de un turno por el profesional

**Fecha:** 06/09/2026
**Entorno:** Producción
**Modo:** Playwright MCP
**Historia:** BJHB-24

## Observaciones ejecutadas

1. El acceso autenticado llegó a `/dashboard` y mostró tres turnos sintéticos futuros. Cada tarjeta ofrecía `Contactar` y `Cancelar`.
2. Al seleccionar `Cancelar` en el primer turno, el navegador abrió un diálogo nativo `confirm` con el texto literal: `¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer.`
3. El diálogo no ofreció ningún control para ingresar el motivo opcional de hasta 250 caracteres definido para BJHB-24.
4. Se rechazó el diálogo. Los tres turnos continuaron visibles y no se observó una solicitud de cancelación.
5. La apertura y el rechazo se repitieron dos veces consecutivas. En ambos intentos apareció un solo diálogo con el mismo texto y la lista quedó sin cambios.
6. Se navegó desde el dashboard a `/dashboard/availability` y se volvió mediante Back Button. La pantalla regresó a `/dashboard` y mantuvo las tres acciones `Cancelar`.
7. Con viewport de 390 × 844, las tres tarjetas conservaron visibles los datos sintéticos y la acción `Cancelar`, sin solapamientos dentro de la sección.

## Limitaciones de evidencia

* Playwright no permite tomar una captura mientras un diálogo nativo está abierto. Por eso el texto y el tipo `confirm` se conservan como evidencia textual y las capturas muestran los estados anterior y posterior al rechazo.
* No se aceptó el diálogo, para evitar una escritura sobre Producción y un posible correo real.
* No se copió a los artefactos el email de la cuenta ni ningún secreto. Las capturas se limitaron a la sección que contiene datos sintéticos.

## Resultado de heurísticas

| Heurística | Resultado |
| :--- | :--- |
| Super User | Dos aperturas y rechazos consecutivos mantuvieron un único diálogo por acción y no alteraron la lista. |
| Back Button | La navegación Disponibilidad -> Atrás devolvió al dashboard con las acciones disponibles. |
| Goldilocks | Bloqueada: no existe un campo de motivo sobre el cual probar límites de 0, 250 y 251 caracteres. |
