## Taller: Arquitectura, SOLID y Buenas Prácticas en C#

---

### Tema

- Principios SOLID
- Buenas prácticas de desarrollo
- Diseño orientado a objetos

---

### Objetivos

Al finalizar este taller, el estudiante será capaz de:

- Identificar problemas de diseño en código
- Aplicar principios SOLID
- Mejorar la estructura de un sistema
- Reducir el acoplamiento
- Diseñar código limpio y mantenible

---

## Ejercicio 1: Análisis de código (Detección de errores)

### Enunciado

Analiza el siguiente código:

```csharp
class Notificador
{
    public void Enviar(string tipo, string mensaje)
    {
        if (tipo == "email")
        {
            Console.WriteLine("Email: " + mensaje);
        }
        else if (tipo == "sms")
        {
            Console.WriteLine("SMS: " + mensaje);
        }
        else if (tipo == "whatsapp")
        {
            Console.WriteLine("WhatsApp: " + mensaje);
        }
    }
}
```

---

### Actividades

1. Identifica al menos 3 problemas de diseño  
    * Violación de Open/Closed Principle
    * Acoplamiento fuerte 
    * Dificultad para agregar nuevos tipos de notificación
2. Indica qué principios SOLID se están violando 
    * Open/Closed Principle
    * Single Responsibility Principle 
    * Dependency Inversion Principle
3. Explica por qué este código es difícil de mantener  
    * Cada vez que se quiera agregar un nuevo tipo de notificación, se debe modificar el método `Enviar`, lo que puede introducir errores y afecta la estabilidad del código. Además, el método tiene múltiples responsabilidades (determinar el tipo de notificación y enviar el mensaje), lo que dificulta su comprensión y mantenimiento.

---

### Pistas

- ¿Qué pasa si agregamos otro tipo?
- ¿Se está modificando código existente?
- ¿Hay acoplamiento?

---


## Ejercicio 2: Refactorización con SOLID

### Enunciado

Reestructura el código anterior aplicando:

- Open/Closed Principle  
- Dependency Inversion Principle  

---

### Requisitos

- Crear una interfaz  
- Crear al menos 2 implementaciones  
- Modificar la clase principal  
- Evitar uso de `if` o `switch`  

---
### Espacio para solución

```csharp
interface INotificador
{
    void Enviar(string mensaje);
}

class NotificadorEmail : INotificador
{
    public void Enviar(string mensaje)
    {
        Console.WriteLine("Email: " + mensaje);
    }
}

class NotificadorSMS : INotificador
{
    public void Enviar(string mensaje)
    {
        Console.WriteLine("SMS: " + mensaje);
    }
}

class NotificadorWhatsApp : INotificador
{
    public void Enviar(string mensaje)
    {
        Console.WriteLine("WhatsApp: " + mensaje);
    }
}

class Notificador
{
    private INotificador _notificador;

    public Notificador(INotificador notificador)
    {
        _notificador = notificador;
    }

    public void Enviar(string mensaje)
    {
        _notificador.Enviar(mensaje);
    }
} 
   // Uso:
   var notificadorEmail = new Notificador(new NotificadorEmail());
    notificadorEmail.Enviar("Hola por email");
    var notificadorSMS = new Notificador(new NotificadorSMS());
    notificadorSMS.Enviar("Hola por SMS");
    var notificadorWhatsApp = new Notificador(new NotificadorWhatsApp());
    notificadorWhatsApp.Enviar("Hola por WhatsApp"); 
```

---


## Ejercicio 3: Buenas prácticas

### Enunciado

Analiza el siguiente código:

```csharp
class Sistema
{
    public void Ejecutar()
    {
        double x = 100;
        double y = 2;
        double z = x * y;

        if (z > 100)
        {
            z = z - (z * 0.1);
        }

        Console.WriteLine(z);
    }
}
```

---

### Actividades

1. Identifica problemas de buenas prácticas  
2. Mejora:
   - Nombres  
   - Estructura  
   - Responsabilidades  
3. Divide el código en métodos  

---
### Espacio para solución

```csharp
class Sistema
{
    public void ProcesarPedido()
    {
        double precio = 100;
        int cantidad = 2;
        double total = CalcularTotal(precio, cantidad);

        
        Console.WriteLine("Total: " + total);
    }

    private double CalcularTotal(double precio, int cantidad)
    {
        var total = precio * cantidad;

        total = AplicarDescuento(total);

        return total;
    }

    private double AplicarDescuento(double total)
    {
        if (total > 100)
        {
            return total - (total * 0.1);
        }

        return total;
    }
}
```

---

## Ejercicio 4: Diseño (Nivel ingeniería)

### Enunciado

Diseña un sistema de pagos que soporte:

- Pago con tarjeta  
- Pago en efectivo  
- Pago con transferencia  

---

### Requisitos

1. Aplicar principios SOLID  
2. Usar interfaces  
3. Permitir agregar nuevos métodos de pago sin modificar código existente  
4. Implementar al menos 3 clases  

---

### Preguntas clave

- ¿Dónde usarías interfaces?  
- ¿Cómo evitarías condicionales?  
- ¿Cómo harías el sistema escalable?  

---
### Espacio para solución

```csharp
interface IPago
{
    void ProcesarPago(double monto);
}
class PagoTarjeta : IPago
{
    public void ProcesarPago(double monto)
    {
        Console.WriteLine("Procesando pago con tarjeta: " + monto);
    }
}
class pagoEfectivo : IPago
{
    public void ProcesarPago(double monto)
    {
        Console.WriteLine("Procesando pago en efectivo: " + monto);
    }
}
class pagoTransferencia : IPago 
{
    public void ProcesarPago(double monto)
    {
        Console.WriteLine("Procesando pago en transferencia: " + monto);
    }
 
}
class ProcesadorDePagos 
{
    private IPago _pago;

    public ProcesadorDePagos(IPago pago)
    {
        _pago = pago;
    }

    public void Procesar(double monto)
    {
        _pago.ProcesarPago(monto);
    }
}
//Uso: 
var ProcesadorTarjeta = new ProcesadorDePagos(new PagoTarjeta());
ProcesadorTarjeta.Procesar(100);
var ProcesadorEfectivo = new ProcesadorDePagos(new pagoEfectivo());
ProcesadorEfectivo.Procesar(50); 
var ProcesadorTransferencia = new ProcesadorDePagos(new pagoTransferencia()); 
ProcesadorTransfereancia.Procesar(200);

```

---

## Ejercicio 5: Análisis conceptual

### Enunciado

Responde:

1. ¿Qué es bajo acoplamiento?  
    * Bajo acoplamiento se refiere a un diseño de software en el que los componentes o clases tienen poca o ninguna dependencia entre sí. Esto significa que cada componente puede funcionar de manera independiente, lo que facilita la mantenibilidad, la escalabilidad y la reutilización del código. Un bajo acoplamiento permite que los cambios en una parte del sistema no afecten a otras partes, lo que reduce el riesgo de introducir errores al modificar el código.
2. ¿Qué es alta cohesión? 
    * Alta cohesión se refiere a un diseño de software en el que los elementos dentro de un módulo o clase están estrechamente relacionados y trabajan juntos para lograr una única responsabilidad o propósito. Esto significa que cada clase o módulo tiene una función clara y específica, lo que mejora la legibilidad, la mantenibilidad y la reutilización del código. La alta cohesión facilita la comprensión del código, ya que cada componente tiene una función bien definida y no realiza tareas que no le corresponden. 
3. ¿Por qué es importante programar contra interfaces?  
    * Programar contra interfaces es importante porque permite desacoplar el código, lo que facilita la mantenibilidad y la escalabilidad del sistema. Al depender de interfaces en lugar de implementaciones concretas, se puede cambiar la implementación sin afectar a las partes del código que dependen de ella. Esto también facilita la prueba de unidades, ya que se pueden usar mocks o stubs para simular el comportamiento de las dependencias. Además, programar contra interfaces fomenta un diseño más flexible y modular, lo que mejora la calidad del software.
4. ¿Qué principio SOLID consideras más importante y por qué?  
    * Personalmente, considero que el principio de Open/Closed es uno de los más importantes, ya que fomenta la extensibilidad del código sin necesidad de modificar el código existente. Esto es crucial para mantener la estabilidad del sistema a medida que evoluciona y se agregan nuevas funcionalidades. Al seguir este principio, se puede agregar nuevo comportamiento a través de la creación de nuevas clases o módulos que implementen interfaces existentes, lo que reduce el riesgo de introducir errores en el código ya probado y en producción. Además, este principio promueve un diseño más modular y flexible, lo que facilita la colaboración entre desarrolladores y la adaptación a cambios futuros.

---

## Criterios de evaluación

- Aplicación correcta de SOLID  
- Claridad en el diseño  
- Uso de buenas prácticas  
- Capacidad de análisis  
- Código limpio y estructurado  

---

## Reto adicional

Diseña un sistema de notificaciones que:

- Soporte múltiples canales  
- Sea extensible  
- No requiera modificar código existente  

---

## Idea final

El verdadero nivel de un programador no se mide por lo que escribe, sino por cómo diseña.

---