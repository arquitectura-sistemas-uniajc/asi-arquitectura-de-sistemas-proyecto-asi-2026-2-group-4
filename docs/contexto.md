# ReservaYa — Decisiones Arquitectónicas

## ¿Qué duele hoy (Contexto y Problema)

- **Reservas duplicadas**: dos clientes pueden reservar la misma cancha en la misma franja horaria al mismo tiempo (condición de carrera).
- **Cancelaciones sin política clara**: ausencia de reglas definidas de reembolso genera disputas entre clientes y administración.
- **Sobrecosto de disponibilidad**: pagar infraestructura de alta disponibilidad 24/7 para un negocio que solo opera 16 horas al día (6:00 a.m. – 10:00 p.m.).

## ¿Quién sufre ese dolor?

| Actor | Impacto |
|---|---|
| **Cliente** | Pierde la cancha que creía reservada; espera reembolsos que no llegan a tiempo |
| **Administrador de sede** | Resuelve manualmente conflictos de reservas duplicadas y reclamos de cancelación |
| **Negocio** | Paga de más por disponibilidad que no se usa (8 horas muertas fuera del horario operativo) |

## ¿Qué pasa si el sistema se cae una hora?

- **Dentro del horario operativo (6:00 a.m. – 10:00 p.m.):** **Sí importa.** Clientes no pueden consultar disponibilidad ni reservar; el administrador no puede registrar llegadas. Impacto directo en ingresos y experiencia de usuario.
- **Fuera del horario operativo (10:00 p.m. – 6:00 a.m.):** **No importa casi nada.** No hay usuarios activos intentando reservar o llegar a una cancha a esa hora. Una caída a las 3:00 a.m. es tolerable.

> **Conclusión:** no vale la pena pagar por alta disponibilidad 24/7. El negocio puede permitirse una ventana de mantenimiento o degradación nocturna sin afectar al usuario real, concentrando la inversión en resiliencia durante el horario operativo.