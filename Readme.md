# Tunomatico - Sistemas de gestion de Turnos Digitales

## 1.- Descripcion General del sistema 

**Tunomatico** es una solucion de sooftware empresarial diseñada para optimizar, automatizar y gestionar el ciclo de vida completo de la asignacion de turnos y citas digitales. el sistema aborda la problematica de la congestion en la atencion presencial, la ineficiencia en la distribucion de horarios y la falta de canales de comunicacion automatizados con los usuarios.

### Objetivos Arquitectonicos 
* **Alta disponibilidad y concurrencia:** Asegurar el acceso simultaneo de multiples usuarios garantizando la consistencia de estado de los turnos sin colisiones de reserva. 
* **Escalabilidad e Intercoperatividad:** permitir la integracion modular con proveedores externos de pasarelas de pago y servicios de mensajeria (SMS/Email) mediante un bajo acoplamiento.
* **Mantenibilidad:** estructurar el dominio aplicando patrones de diseño Gof (*Gang of Four*) para facilitar la extension del sofware sin alterar el nucleo del negocio.

------------------------------

## 2.- Modelo de Requisitos: Diagrama de casos de uso

El diagrama de casos de uso define los limites del sistema y la interaccion de los actores con las funcionalidades de la plataforma.

[Diagrama de Caso de uso](Caso_De_Uso.png)

### Descripcion y justificacion de relaciones

* **Actores de negocio (Cliente y Administrador):** Representan los agentes externos autonomos. El `Cliente` interactua principalmente con el ciclo de vida de su cita (`Registrarse`,`Solicitar Turno`), mientras que el `administrador`posee un caso de uso exclusivo (`Registrar usuario`) para el control operacional y de personal dentro de la plataforma.
* **Actores del sistema (servicio de notificacion y sistema de pagos):** Actores secundarios del tipo sistema (*system boundary*) que reaccionan de manera sincrona y asincrona ante los estimulos de bakend para procesar transacciones y despachar alertas.
* **Relaciones de inclusion (`<<include>>`):** El caso de uso `Confirmar Turno` incluye obligatoriamente a `Solicitar Turno` y `Notificar Turno`. Esto asegura que ninguna confirmacion de reserva sea consolidada en la base de datos sin una solicitud previa en el flujo y sin disparar la alerta correspondiente al cliente.
* En el subsistema financiero, `Validad pago` incluye mandatoriamente a `notificar pago`, garantizando la trazabilidad y el acuse de recibo de la transaccion de cara al usuario.
* **Relaciones de Extension (`<<extend>>`):** El caso de uso central `Solicitar Turno` es extendido de manera condicional por `Cancelar Turno` y `Consultar Disponibilidad`. Estas extenciones representan flujos alternativos que el cliente puede o no ejercutar dependiendo del estado de la sesion y la necesidad del negocio, evitando sobrecargar el flujo principal de reserva.

------------------------------

## 3.- Arquitectura logica: Diagrama de clases

El diseño logico del sistema se estructuro bajo el paradigma orientado a objetos, aplicando principios SOLID y garantizando un alto grado de cohesion.

![Diagrama de Clases UML](Diagrama_De_Clase.png)
