---
title: 'Patrón Singleton'
description: 'Una clase ~ una instancia'
pubDate: 'Aug 2 2025'
heroImage: '../../assets/blog-placeholder-5.jpg'
---

#### Introducción
Es un patrón de diseño creacional que garantiza que una clase tenga una única instancia en toda la aplicación y que exista un punto global de acceso a ella.

#### Problema a resolver
- Cuando se requiere *una sola instancia* que coordine acciones del sistema, como un gestor de configuración, conexión con la base de datos o un logger.

#### Aplicando el patrón Singleton
- **Objetivo:** La clase se encarga de crear y devolver siempre la misma instancia.
- **Objetivo:** Se accede mediante un método estático o propiedad.
- **Objetivo:** Evitar duplicados no deseados.

```csharp
public sealed class Logger
{
    private static readonly Logger _instance = new Logger();

    // Constructor privado para evitar instanciación externa
    private Logger() { }

    public static Logger Instance => _instance;

    public void Log(string message)
    {
        Console.WriteLine($"[LOG] {message}");
    }
}
```

De esta forma podemos usarlo así:
```csharp
Logger.Instance.Log("Aplicación iniciada");
```

#### Ventajas y desventajas
##### Ventajas
- Control total sobre la instancia.
- Útil para recursos compartidos.

##### Desventajas
- Puede dificultar las pruebas unitarias.
- Introduce acoplamiento global si se abusa de él.

#### Conclusión
El patrón Factory Method es una herramienta valiosa cuando necesitamos manejar una única instancia de una clase aunque se debe tener cuidado de no convertir todas las clases en Singleton, Úsala solo cuando haya un caso real de necesidad de una instanciación única.