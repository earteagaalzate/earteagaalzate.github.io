---
title: 'Patrón Builder'
description: 'Creando objetos complejos paso a paso'
pubDate: 'Jul 19 2025'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

#### Introducción

En una aplicación no todos los objetos se crean con un simple *new*. En ocasiones puede pasar que su construcción implica varios pasos, combinaciones de parámetros o valores opcionales. Para estos casos el patrón builder nos ayuda a gestionar esta complejidad no solo mejorando la legibilidad sino también evitando tener que crear constructores interminables.

#### El problema a resolver
- Un constructor muy extenso que recibe varios parámetros.
- Dificultad para recordar el orden de los argumentos y leer el código.

```csharp
var report = new Report(
  "Ventas 2025",
  "Resumen mensual",
  "Coordinador de ventas",
  DateTime.Now,
  true,
  "PDF"
);
```
El anterior es un ejemplo claro de como la cantidad de parámetros puede llegar a confundirnos como desarrolladores.

#### Aplicando el patrón Builder
- **Objetivo:** Separar la construcción de un objeto de su representación final a la vez que permite construir el objeto de distintas formas.

```csharp
public class Report
{
    public string Title { get; private set; }
    public string Description { get; private set; }
    public string Author { get; private set; }
    public DateTime Date { get; private set; }
    public bool IncludeCharts { get; private set; }
    public string Format { get; private set; }

    private Report() { }

    public class Builder
    {
        private readonly Report _report = new Report();

        public Builder WithTitle(string title)
        {
            _report.Title = title;
            return this;
        }

        public Builder WithDescription(string description)
        {
            _report.Description = description;
            return this;
        }

        public Builder WithAuthor(string author)
        {
            _report.Author = author;
            return this;
        }

        public Builder WithDate(DateTime date)
        {
            _report.Date = date;
            return this;
        }

        public Builder IncludeCharts(bool include)
        {
            _report.IncludeCharts = include;
            return this;
        }

        public Builder WithFormat(string format)
        {
            _report.Format = format;
            return this;
        }

        public Report Build() => _report;
    }
}
```

De esta forma podemos usarlo así:
```csharp
var report = new Report.Builder()
    .WithTitle("Ventas 2025")
    .WithDescription("Resumen mensual")
    .WithAuthor("Coordinador de ventas")
    .WithDate(DateTime.Now)
    .IncludeCharts(true)
    .WithFormat("PDF")
    .Build();

```

#### Ventajas y desventajas
##### Ventajas
- Código mas legible y fácil de mantener.
- Evita errores con el orden de los argumentos.
- Facilita la construcción de objetos con parámetros opcionales.

##### Desventajas
- Puede añadir complejidad cuando se usa con objetos simples.

#### Conclusión
El patrón Builder es ideal cuando se requiere crear objetos complejos de forma controlada, clara y extensible, aunque puede ser innecesario con objetos simples, en proyectos grandes aporta estructura y facilita la colaboración entre los desarrolladores del equipo.
