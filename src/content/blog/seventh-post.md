---
title: 'Patrón Adapter'
description: 'Conectando clases con diferentes interfaces'
pubDate: 'Aug 23 2025'
heroImage: '../../assets/blog-placeholder-4.jpg'
---

#### Introducción
Puede pasar que nos encontremos librerías o sistemas legados que no se adapten de manera directa a nuestros proyectos, para estos casos contamos con el *Patrón Adapter*, una solución de diseño que podemos usar como intermediaria entre dos interfaces incompatibles, permitiendo así que trabajen juntas sin necesidad de modificar el código original.

#### El problema a resolver
Imaginemos que nuestra aplicación necesita procesar pagos en linea, ya tenemos implementada una interfaz de pagos estándar,solo que ahora nuestro cliente solicita integrar un proveedor externo cuya API expone métodos y formatos completamente distintos. En este caso reescribir la aplicación para adaptarla directamente al nuevo servicio seguro sería muy costoso, riesgoso y seguramente difícil de mantener.

Es en estos casos donde podemos usar el **Patrón Adapter** ya que podemos encapsular las diferencias en una clase intermediaria que puede traducir las llamadas de nuestro sistema hacia la interfaz del proveedor logrando la compatibilidad sin modificar la lógica original.

#### Implementación
La estructura básica de este patrón se compone de tres elementos:
1. Cliente - El código que espera una interfaz específica.
2. Adaptable - La clase existente o servicio interno que no es compatible directamente.
3. Adaptador - La clase puente que implementa la interfaz esperada por el cliente y traduce las llamadas hacia el Adaptable.
```csharp
// Interfaz esperada por el cliente
public interface IPago {
    void ProcesarPago(double monto);
}

// Clase existente con una interfaz diferente
public class ProveedorExterno {
    public void HacerTransaccion(double cantidad) {
        Console.WriteLine($"Procesando pago de {cantidad} con Proveedor Externo");
    }
}

// Adapter que conecta ambas interfaces
public class PagoAdapter : IPago {
    private readonly ProveedorExterno _proveedor;

    public PagoAdapter(ProveedorExterno proveedor) {
        _proveedor = proveedor;
    }

    public void ProcesarPago(double monto) {
        _proveedor.HacerTransaccion(monto);
    }
}

// Uso
class Program {
    static void Main() {
        IPago pago = new PagoAdapter(new ProveedorExterno());
        pago.ProcesarPago(100);
    }
}

```
De esta forma el cliente sigue trabajando con su interfaz IPago, mientras que el adaptador se encarga de hablar con el proveedor externo.

#### Ventajas y desventajas
##### Ventajas
- Permite la integración de componentes sin modificar su código original.
- Facilita la reutilización de clases existentes en nuevos contextos.
- Mejora la flexibilidad y mantenibilidad del sistema.

##### Desventajas
- Puede aumentar la complejidad del código si se abusa de el.
- Si se usan demasiados adaptadores la arquitectura puede volverse difícil de seguir.
- No resuelve problemas de incompatibilidad lógica, solo de interfaz.

#### Conclusión
El patrón Adapter es una herramienta poderosa cuando lo que se requiere es hacer que sistemas incompatibles trabajen juntos, actuando como un traductor que reduce el acoplamiento y protege la lógica de negocio de cambios externos, sin embargo, como cualquier patrón debe aplicarse con criterio, su uso es ideal en proyectos que requieren integraciones con librerías de terceros, sistemas heredados o APIs con contratos distintos a los de nuestra aplicación.
