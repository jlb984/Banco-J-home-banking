# Handoff UI -> API/DB: Cancelación de un turno por el profesional

La sesión UI se ejecutó sobre Producción sin confirmar ninguna cancelación. Los endpoints y efectos persistentes indicados como esperados provienen de `.context/architecture/system-design.md` y de BJHB-24; no se presentan como tráfico observado en esta ejecución.

```json
{
  "feature": "Cancelación de un turno por el profesional",
  "story": "BJHB-24",
  "environment": "Producción",
  "ui_actions": [
    {
      "step": "Clic en Cancelar y rechazo del diálogo nativo",
      "execution": "Ejecutado",
      "expected_api": {
        "method": "Ninguna",
        "endpoint": "Ninguno",
        "expected_status": "Sin solicitud"
      },
      "expected_db": {
        "table": "appointments",
        "operation": "Ninguna",
        "where": {
          "appointment_id": "No expuesto"
        }
      }
    },
    {
      "step": "Aceptar la cancelación de un turno futuro propio",
      "execution": "No ejecutado: escritura productiva y posible correo real",
      "expected_api": {
        "method": "POST",
        "endpoint": "/api/appointments/<appointment_id>/cancel",
        "expected_status": "2xx pendiente de contrato explícito"
      },
      "expected_db": {
        "table": "appointments",
        "operation": "UPDATE atómico",
        "where": {
          "id": "<appointment_id>",
          "professional_id": "<authenticated_professional_id>",
          "status_before": "confirmed",
          "status_after": "cancelled"
        }
      }
    },
    {
      "step": "Recargar el dashboard después de cancelar",
      "execution": "No ejecutado",
      "expected_api": {
        "method": "GET",
        "endpoint": "/api/appointments",
        "expected_status": 200
      },
      "expected_db": {
        "table": "appointments",
        "operation": "SELECT",
        "where": {
          "id": "<appointment_id>",
          "expected_status": "cancelled"
        }
      }
    },
    {
      "step": "Consultar la disponibilidad pública del día del turno cancelado",
      "execution": "No ejecutado",
      "expected_api": {
        "method": "GET",
        "endpoint": "/api/public/availability",
        "expected_status": 200
      },
      "expected_db": {
        "table": "appointments",
        "operation": "SELECT para cálculo de disponibilidad",
        "where": {
          "id": "<appointment_id>",
          "status": "cancelled",
          "slot_expected": "available"
        }
      }
    }
  ],
  "test_data": {
    "professional": "Cuenta de prueba almacenada en el perfil del navegador",
    "appointment_label": "Prueba QA 1",
    "appointment_id": "No expuesto",
    "reason": "No ingresado: control ausente"
  },
  "trace_ids": {
    "request_id": "No disponible",
    "correlation_id": "No disponible",
    "appointment_id": "No disponible"
  },
  "findings": [
    {
      "id": "BJHB-28",
      "status": "Sincronizado con Jira",
      "summary": "La UI no permite informar el motivo opcional de hasta 250 caracteres"
    },
    {
      "id": "BJHB-24-D1",
      "status": "Conocido, no reejecutado",
      "summary": "Falso éxito de cancelación, estado confirmed y slot no liberado"
    }
  ]
}
```

## Consultas sugeridas para las capas siguientes

* API: verificar autorización por `professional_id`, ventana de dos horas, transición idempotente `confirmed -> cancelled`, status HTTP y cuerpo de error.
* DB: comprobar una única actualización de `appointments`, conservación de relaciones y ausencia del turno cancelado en el cálculo de disponibilidad.
* Correo: confirmar que el evento se genera solo después del commit, con idempotencia y sin revertir la cancelación ante una falla de entrega.
