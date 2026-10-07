## 1. ¿Cómo manejan dos personas reservando la misma franja al mismo tiempo?
 
El complejo deportivo necesita evitar reservas duplicadas: si dos clientes intentan tomar la misma cancha en la misma franja horaria de forma simultánea, el sistema debe garantizar que solo uno de los dos se quede con ella.
 
La solución pasa por controlar la concurrencia a nivel de datos, no solo de interfaz:
 
- Se define una restricción única en base de datos sobre la combinación (cancha, fecha, franja horaria), de forma que la segunda escritura simultánea sea rechazada automáticamente por el motor de BD.
- El flujo de reserva usa un bloqueo optimista: la API verifica disponibilidad y escribe la reserva en una sola transacción, evitando la ventana de tiempo entre "consultar" y "reservar" donde ocurriría la condición de carrera.
- Si la escritura falla por duplicado, el cliente recibe de inmediato un mensaje de "franja no disponible" en vez de una reserva fantasma.
> **Conclusión:** la concurrencia no se resuelve validando en el frontend, sino con una restricción a nivel de base de datos que actúa como última línea de defensa, sin importar cuántas peticiones lleguen al mismo tiempo.
 
---
 
## 2. Si el sistema se cae a las tres de la mañana, ¿importa?
 
El complejo opera dieciséis horas al día (6:00 a.m. – 10:00 p.m.). A las 3:00 a.m. no hay clientes consultando disponibilidad, reservando canchas ni administradores registrando llegadas, porque el negocio está cerrado.
 
Esto se traduce en dos escenarios con impacto muy distinto:
 
- **Dentro del horario operativo (6:00 a.m. – 10:00 p.m.):** sí importa. Una caída aquí bloquea reservas, cancelaciones y registro de llegadas, con impacto directo en ingresos y experiencia de usuario.
- **Fuera del horario operativo, incluida la madrugada:** no importa casi nada. No hay usuarios activos intentando usar el sistema a esa hora, así que una caída pasa prácticamente desapercibida.
> **Conclusión:** una caída a las 3:00 a.m. es tolerable porque no hay nadie usando el sistema en ese momento; el riesgo real está concentrado en las 16 horas operativas, no en las 24.
 
---
 
## 3. ¿Cuánta disponibilidad compraron y por qué?
 
Comprar infraestructura de alta disponibilidad para las veinticuatro horas implicaría pagar por redundancia que nunca se usa, ya que el negocio solo opera 16 de las 24 horas del día.
 
La decisión de cuánta disponibilidad comprar se basa en el uso real del sistema:
 
- Se compra alta disponibilidad solo para las 16 horas operativas (6:00 a.m. – 10:00 p.m.), que es cuando el sistema realmente importa.
- Fuera de ese horario se acepta una ventana de mantenimiento o degradación controlada, sin pagar por redundancia adicional.
- Esto reduce el costo de infraestructura sin aumentar el riesgo real del negocio, ya que la pregunta 2 ya demostró que una caída nocturna no tiene impacto.
> **Conclusión:** el presupuesto de disponibilidad debe dimensionarse al horario real de operación (16 horas), no a un estándar genérico de 24/7 que el negocio no necesita ni puede justificar económicamente.
 
---
 
## 4. ¿Qué cambiaría si el negocio pasara a operar veinticuatro horas?
 
Si el complejo deportivo decidiera operar sin cierre nocturno, el supuesto que justifica ahorrar en disponibilidad durante la madrugada ya no sería válido, y la arquitectura tendría que adaptarse.
 
Los cambios principales serían:
 
- Pasar de alta disponibilidad "por horario" a alta disponibilidad real 24/7, lo que implica redundancia de servidores y base de datos en todo momento, no solo durante el día.
- Eliminar o reducir cualquier ventana de mantenimiento nocturna, ya que cualquier caída —a cualquier hora— afectaría ahora a usuarios activos.
- Revisar la capacidad del Worker de Notificaciones y de la API para sostener carga constante, en vez de picos concentrados en 16 horas.
- El costo de infraestructura subiría, por lo que esta decisión debe evaluarse frente al aumento real de ingresos que traería operar 24 horas.
> **Conclusión:** operar 24 horas no es solo una decisión de negocio, sino que obliga a rediseñar la estrategia de disponibilidad de toda la arquitectura, pasando de "alta disponibilidad parcial" a "alta disponibilidad total".