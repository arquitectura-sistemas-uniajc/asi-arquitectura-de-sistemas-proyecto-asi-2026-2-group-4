## 1. ¿Qué duele hoy? (Contexto y Problema)

El complejo deportivo alquila canchas por franjas horarias, operando dieciséis horas al día (6:00 a.m. a 10:00 p.m.). Actualmente no existe un mecanismo que garantice que una franja solo pueda ser tomada por un cliente a la vez, ni reglas claras sobre qué pasa cuando alguien cancela.

Esto genera los siguientes problemas:

- Dos clientes pueden reservar la misma cancha en la misma franja al mismo tiempo, sin que el sistema lo detecte (condición de carrera).
- No hay política de reembolso definida, por lo que cada cancelación se negocia caso por caso y genera disputas.
- El negocio paga infraestructura de alta disponibilidad pensada para operar 24/7, cuando en realidad solo usa el sistema 16 de las 24 horas del día.
- No existe un criterio claro de cuánta disponibilidad (y por tanto cuánto presupuesto) necesita realmente la operación.

---

## 2. ¿Quién sufre ese dolor? (Actores afectados)

Los problemas anteriores no golpean a todos los actores por igual: cada uno percibe una consecuencia distinta de la falta de control de concurrencia y de políticas claras.

Esto afecta directamente a:

- **Cliente**: pierde la cancha que creía reservada cuando otro usuario tomó la misma franja, o espera un reembolso que no llega a tiempo tras una cancelación.
- **Administrador de sede**: debe resolver manualmente los conflictos de reservas duplicadas y atender reclamos de cancelación sin un proceso estandarizado.
- **Negocio**: asume un sobrecosto de infraestructura al pagar disponibilidad para las 24 horas, cuando solo opera 16.

---

## 3. ¿Qué pasa si el sistema se cae una hora? (Impacto según el horario)

El impacto de una caída del sistema no es el mismo en cualquier momento del día: depende directamente de si el complejo deportivo está operando o no en ese instante.

Esto se traduce en dos escenarios distintos:

- **Dentro del horario operativo (6:00 a.m. – 10:00 p.m.):** sí importa. Los clientes no pueden consultar disponibilidad ni reservar, y el administrador no puede registrar llegadas, afectando ingresos y experiencia de usuario en tiempo real.
- **Fuera del horario operativo (10:00 p.m. – 6:00 a.m.):** no importa casi nada. No hay usuarios activos intentando reservar o llegar a una cancha a esa hora; una caída a las 3:00 a.m. es perfectamente tolerable.

> **Conclusión:** no se justifica pagar por alta disponibilidad 24/7. El negocio puede permitirse una ventana de mantenimiento o degradación controlada durante la madrugada, concentrando la inversión en resiliencia solo durante las 16 horas en que realmente opera.