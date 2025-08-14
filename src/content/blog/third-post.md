---
title: 'Patrón Factory Method'
description: 'Creando objetos sin acoplar tu código'
pubDate: 'Jul 26 2025'
heroImage: '../../assets/blog-placeholder-4.jpg'
---

#### Introducción
En algunos proyectos puede pasar que el código para crear un objeto termine disperso o mezclado con lógica de negocio. Lo anterior no solo dificulta su mantenimiento sino que obliga a modificar múltiples partes del sistema cuando se cambian los detalles de la creación de dichos objetos. El patrón Factory Method nos ayuda a centrar esa responsabilidad favoreciendo el desacoplamiento y la extensibilidad.

#### El problema a resolver
- Cuando se crean instancias en distintos lugares obligando a cambiarlas cada que se agregan nuevas variantes.

```csharp
public void GenerateReport(string type)
{
  if(type == "PDF")
    report = new PdfReport();
  else if (type == "Excel")
    report = new ExcelReport();
}
```
- Lógica de creación dispersa
- Violación del principio abierto/cerrado (OCP de SOLID)

##### Aplicando Factory Method
- **Objetivo** Delegar la responsabilidad de instanciar objetos a una clase o método específico en lugar de directamente *new*.
- **Objetivo** Permitir que las subclases decidan que clase instanciar.

**Interfaz base**
```csharp
public interface IReport
{
  void Generate();
}
```
**Clases concretas**
```csharp
public class PdfReport : IReport
{
    public void Generate() => Console.WriteLine("Generando reporte en PDF");
}

public class ExcelReport : IReport
{
    public void Generate() => Console.WriteLine("Generando reporte en Excel");
}
```
**Creador base**
```csharp
public abstract class ReportCreator
{
    public abstract IReport CreateReport();

    public void GenerateReport()
    {
        var report = CreateReport();
        report.Generate();
    }
}
```
**Creadores concretos**
```csharp
public class PdfReportCreator : ReportCreator
{
    public override IReport CreateReport() => new PdfReport();
}

public class ExcelReportCreator : ReportCreator
{
    public override IReport CreateReport() => new ExcelReport();
}
```

De esta forma podemos usarlo así:
```csharp
ReportCreator creator = new PdfReportCreator();
creator.GenerateReport();

creator = new ExcelReportCreator();
creator.GenerateReport();
```

#### Ventajas y desventajas
##### Ventajas
- Desacopla la creación de la lógica de uso
- Facilita agregar nuevas variantes
- Favorece el cumplimiento de los principios SOLID

##### Desventajas
- Puede añadir complejidad cuando se usa con objetos simples.

#### Conclusión
El patrón Factory Method es una herramienta valiosa cuando necesitamos manejar múltiples tipos de objetos de forma flexible. Su uso evita la proliferación de condicionales y facilita el mantenimiento a largo plazo.


