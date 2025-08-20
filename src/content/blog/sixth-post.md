---
title: 'Patrón Strategy'
description: 'Flexibilidad en la elección de algoritmos'
pubDate: 'Aug 16 2025'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

#### Introducción
Sabemos que habitualmente un mismo proceso puede ejecutarse de diferentes maneras dependiendo de contexto, condiciones o incluso preferencias de usuario. En estos casos el patrón Strategy nos permite definir un grupo de algoritmos, encapsularlos y hacerlos intercambiables sin modificar el otras partes del código.

Su objetivo principal es separar la lógica que decide que algoritmo usar y la lógica que ejecuta la operación ofreciendo así flexibilidad y facilidad de mantenimiento.

#### El problema a resolver
- Intercambiar algoritmos sin modificar la lógica principal.
- Anidamiento de lógica vinculada a instrucciones if/else o switch.
- Probar los algoritmos de manera independiente.

#### Implementación
1. Definir la interfaz de la estrategia.
```csharp
public interface IPaymentStrategy
{
    void Pay(decimal amount);
}
```
2. Crear la implementaciones concretas.
```csharp
public class CreditCardPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Pago de {amount:C} con tarjeta de crédito procesado.");
    }
}

public class PayPalPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Pago de {amount:C} con PayPal procesado.");
    }
}

public class BankTransferPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Pago de {amount:C} mediante transferencia bancaria procesado.");
    }
}
```
3. Contexto que usa la estrategia
```csharp
public class PaymentContext
{
    private IPaymentStrategy _paymentStrategy;

    public PaymentContext(IPaymentStrategy paymentStrategy)
    {
        _paymentStrategy = paymentStrategy;
    }

    public void SetStrategy(IPaymentStrategy paymentStrategy)
    {
        _paymentStrategy = paymentStrategy;
    }

    public void ExecutePayment(decimal amount)
    {
        _paymentStrategy.Pay(amount);
    }
}
```
4. Usamos el patrón Factory para seleccionar la estrategia
```csharp
public class PaymentFactory
{
    public static IPaymentStrategy GetPaymentStrategy(string paymentType)
    {
        return paymentType switch
        {
            "CreditCard" => new CreditCardPayment(),
            "PayPal" => new PayPalPayment(),
            "BankTransfer" => new BankTransferPayment(),
            _ => throw new NotImplementedException()
        };
    }
}
```

5. Y lo usamos así:
```csharp
class Program
{
    static void Main()
    {
        // Supongamos que viene de la entrada del usuario
        string selectedMethod = "PayPal";
        decimal amount = 99.99m;

        var strategy = PaymentFactory.GetPaymentStrategy(selectedMethod);
        var paymentContext = new PaymentContext(strategy);
        paymentContext.ExecutePayment(amount);
    }
}
```
#### Ventajas y desventajas
##### Ventajas
- Se puede agregar nuevas estrategias sin modificar el código existente.
- Reemplaza lógica dentro sentencias if/else o switch.
- Los algoritmos bien encapsulados y reutilizables.

##### Desventajas
- Cada estrategia requiere su propia clase de implementación.
- Posible sobre-ingeniería si solo hay una o dos variantes.

#### Conclusión
El patrón Strategy es la solución cuando necesitas funcionalidades intercambiables sin acoplarlas al código, sin embargo, como todo patrón debe aplicarse con criterio pues no tiene sentido crear estrategias cuando las opciones son mínimas o poco probables.

Cuando el objetivo es tener un código flexible, mantenible y extensible el patrón Strategy es un aliado que vale la pena tener en nuestra caja de herramientas.