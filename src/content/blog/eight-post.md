---
title: 'Patrón Abstract Factory'
description: 'Creando familias de objetos sin acoplamiento de código'
pubDate: 'Aug 30 2025'
heroImage: '../../assets/blog-placeholder-5.jpg'
---

#### Introducción
En el desarrollo de software un reto común suele ser la necesidad de crear objetos relacionados que deben trabajar en conjunto, sin depender directamente de sus implementaciones concretas. Para estos casos el patrón **Abstract Factory** puede ser una solución elegante que nos permite construir familias de objetos sin acoplar nuestro código a clases específicas, facilitando la extensibilidad y la consistencia.

#### El problema a resolver
Supongamos que estamos construyendo un software que debe trabajar con documentos Pdf y Word donde:

- Para Pdf necesitamos un objecto PdfDocument y un PdfVisor.
- Para Word necesitamos un WordDocument y un WordVisor.

Si el cliente creara objetos directamente podría terminar mezclando un PdfDocument con un WordVisor, lo cual no tendría sentido.

Con el patrón **Abstract Factory** se puede evitar esta inconsistencia, creando siempre un conjunto correcto de objetos relacionados.

#### Implementación
1. Se crean Productos abstractos
```csharp
public interface IDocument {
    void Guardar(string contenido);
}

public interface IVisor {
    void Mostrar(string archivo);
}
```
2. Se crean Productos concretos
```csharp
// PDF
public class PdfDocument : IDocument {
    public void Guardar(string contenido) => 
        Console.WriteLine($"Guardando '{contenido}' en un archivo PDF");
}

public class PdfVisor : IVisor {
    public void Mostrar(string archivo) => 
        Console.WriteLine($"Mostrando el PDF: {archivo}");
}

// Word
public class WordDocument : IDocument {
    public void Guardar(string contenido) => 
        Console.WriteLine($"Guardando '{contenido}' en un archivo Word");
}

public class WordVisor : IVisor {
    public void Mostrar(string archivo) => 
        Console.WriteLine($"Mostrando el documento Word: {archivo}");
}
```
3. Implementado el patrón **Abstract Factory**
```csharp
public interface IUtilidadFactory {
    IDocument CrearDocumento();
    IVisor CrearVisor();
}

```
4. Fábricas concretas
```csharp
// PDF
public class PdfFactory : IUtilidadFactory {
    public IDocument CrearDocumento() => new PdfDocument();
    public IVisor CrearVisor() => new PdfVisor();
}

// Word
public class WordFactory : IUtilidadFactory {
    public IDocument CrearDocumento() => new WordDocument();
    public IVisor CrearVisor() => new WordVisor();
}
```
5. Usando la implementación: donde el cliente no tiene conocimiento sobre PdfDocument o WordDocument y solo conoce la fabrica seleccionada, de esta forma se asegura que siempre se usen combinaciones correctas.

```csharp
// Cliente
class Program {
    static void Main() {
        // Selección de utilidad: PDF o Word
        IUtilidadFactory factory = new PdfFactory();

        IDocument doc = factory.CrearDocumento();
        IVisor visor = factory.CrearVisor();

        doc.Guardar("Reporte financiero Q3");
        visor.Mostrar("Reporte.pdf");
    }
}
```

#### Ventajas y desventajas
##### Ventajas
- Garantiza consistencia de objetos relacionados (Un documento PDF siempre con su visor PDF)
- El cliente no depende de implementaciones concretas, solo interfaces.
- Fácil de extender: agregar un nuevo formato (ej: ODT) solo requiere crear otra fabrica y sus productos.

##### Desventajas
- Incrementa el número de clases (mas interfaces y fábricas).
- Puede sentirse demasiado complejo si el producto es pequeño.

#### Conclusión
El patrón **Abstract Factory** es ideal cuando necesitamos producir familias de objetos relacionados de manera consistente ofreciendo escalabilidad y desacoplamiento a cambio de un poco mas de estructura, lo cual es beneficioso en aplicaciones que deben soportar múltiples variantes de un mismo producto.
