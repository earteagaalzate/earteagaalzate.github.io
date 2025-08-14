---
title: 'Patrón Decorator'
description: 'Añadir funcionalidades sin modificar el código original'
pubDate: 'Aug 9 2025'
heroImage: '../../assets/blog-placeholder-2.jpg'
---

#### Introducción
Suele pasar que en una aplicación existente se requiera añadir funcionalidades a una clase existente, sin embargo, modificar el código puede ser riesgoso y puede llegar a romper otras partes del sistema.

El patrón decorador es un patrón que permite:
- Envolver el objeto para añadir nuevas funcionalidades.
- No requiere modificar la clase base.
- Permite combinar varios comportamientos.

Entonces en lugar de heredar una clase para añadir funcionalidades, creamos objetos que *'decoran'* el original delegando llamadas y añadiendo lógica antes o después.

#### El problema a resolver
- Cuando se requiere extender el funcionamiento de una clase de forma dinámica y respetando el principio **Open/Closed**.

```csharp
using System;

// Componente base
public interface INotifier
{
    void Send(string message);
}

// Implementación concreta
public class BasicNotifier : INotifier
{
    public void Send(string message)
    {
        Console.WriteLine($"Notificación básica: {message}");
    }
}

// Decorador base
public abstract class NotifierDecorator : INotifier
{
    protected readonly INotifier _notifier;

    protected NotifierDecorator(INotifier notifier)
    {
        _notifier = notifier;
    }

    public virtual void Send(string message)
    {
        _notifier.Send(message);
    }
}

// Decorador concreto: Email
public class EmailNotifier : NotifierDecorator
{
    public EmailNotifier(INotifier notifier) : base(notifier) { }

    public override void Send(string message)
    {
        base.Send(message);
        Console.WriteLine($"📧 Enviando email: {message}");
    }
}

// Decorador concreto: SMS
public class SMSNotifier : NotifierDecorator
{
    public SMSNotifier(INotifier notifier) : base(notifier) { }

    public override void Send(string message)
    {
        base.Send(message);
        Console.WriteLine($"📱 Enviando SMS: {message}");
    }
}
```
Y lo usamos así:
```csharp
class Program
{
    static void Main()
    {
        INotifier notifier = new BasicNotifier();
        notifier = new EmailNotifier(notifier);
        notifier = new SMSNotifier(notifier);

        notifier.Send("Hola mundo");
    }
}
```
#### Ventajas y desventajas
##### Ventajas
- Añadir o quitar funcionalidades en tiempo de ejecución.
- Evita jerarquías de herencia grandes y difíciles de mantener.
- Facilita seguir el principio Open/Closed.

##### Desventajas
- Mas clases y objetos que manejar.
- El flujo puede ser mas difícil de seguir si hay muchos decoradores anidados.

#### Conclusión
El patrón decorador es ideal para ampliar el comportamiento de forma flexible, manteniendo el código limpio y modular. El uso correcto del patrón puede ahorrar dolores de cabeza con jerarquías complejas y facilitar el mantenimiento a largo plazo.