En esta actividad práctica del módulo 3 puede verse el despliegue de un sistema de pedidos implementado sobre una arquitectura orientada a eventos y completamente serverless en AWS. Esta solución abarca de principio a fin la gestión del ciclo de vida de un pedido de forma escalable y tolerante a fallos. Al desacoplar la recepción de la solicitud del procesamiento interno, el sistema garantiza respuestas inmediatas al cliente mientras orquesta de manera controlada diferentes aspectos del proceso, tales como las validaciones, cobros y auditoría del pedido. 

• API Gateway: puerta de entrada del sistema para recibir las solicitudes de creación de pedidos de los clientes.
• Lambda Inicial: recibe la petición desde API Gateway, envía el evento de negocio a EventBridge, y luego devuelve una respuesta rápida al usuario.
• Amazon EventBridge: filtra el evento recibido y activa el flujo de procesamiento de forma asíncrona, desacoplando la recepción del pedido de su ejecución.
• AWS Step Function: orquesta el flujo de trabajo completo del pedido. Gestiona el estado de la ejecución, controla el paso de datos entre funciones y maneja errores o reintentos de forma automática. Está compuesta por 5 Lambdas:

1. validate-order: verifica que los datos del pedido cumplan con las reglas de negocio establecidas (formato, montos, campos obligatorios) antes de iniciar cualquier operación crítica.
2. check-inventory: consulta la disponibilidad de los productos solicitados para asegurar que exista stock suficiente antes de realizar el cobro.
3. process-payment: gestiona la transacción financiera y valida que el pago del pedido sea aprobado exitosamente.
4. confirm-order: formaliza la aprobación del pedido una vez validado el inventario y completado el pago, dejando la orden lista para su despacho.
5. register-audit: registra el resultado final de toda la operación para fines de control, monitoreo y auditoría del sistema. 