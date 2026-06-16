# Manual de LINQ en C# — Programación Avanzada

> **Autor:** AngeloChimborazo  
> **Repositorio:** [github.com/AngeloChimborazo/Manual-LINQ](https://github.com/AngeloChimborazo/Manual-LINQ)  
> **Descripción:** Tutorial educativo de LINQ en C# para Programación Avanzada.

---

## Tabla de Contenidos

1. [¿Qué es LINQ?](#1-qué-es-linq)
2. [Sintaxis de LINQ](#2-sintaxis-de-linq)
3. [Operadores de Filtrado — `Where`](#3-operadores-de-filtrado--where)
4. [Operadores de Proyección — `Select`](#4-operadores-de-proyección--select)
5. [Ordenamiento — `OrderBy` / `OrderByDescending`](#5-ordenamiento--orderby--orderbydescending)
6. [Agrupamiento — `GroupBy`](#6-agrupamiento--groupby)
7. [Operadores de Agregación](#7-operadores-de-agregación)
8. [Join entre colecciones](#8-join-entre-colecciones)
9. [LINQ con Listas de Objetos](#9-linq-con-listas-de-objetos)
10. [Ejercicios Prácticos con Soluciones](#10-ejercicios-prácticos-con-soluciones)

---

## 1. ¿Qué es LINQ?

**LINQ** (Language Integrated Query) es una característica de C# que permite realizar consultas directamente sobre colecciones de datos (listas, arreglos, bases de datos, XML, etc.) usando una sintaxis similar a SQL, pero integrada en el propio lenguaje.

### ¿Para qué sirve?

- Filtrar, ordenar y agrupar datos de forma sencilla y legible.
- Trabajar con cualquier colección que implemente `IEnumerable<T>`.
- Reemplazar bucles `foreach` complejos con expresiones concisas.

### Namespaces necesarios

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
```

---

## 2. Sintaxis de LINQ

LINQ tiene **dos sintaxis equivalentes**: la sintaxis de consulta (estilo SQL) y la sintaxis de métodos (fluent/lambda). Ambas producen el mismo resultado.

### Sintaxis de consulta (Query Syntax)

```csharp
int[] numeros = { 5, 3, 8, 1, 9, 2, 7 };

var resultado = from n in numeros
                where n > 4
                orderby n
                select n;

foreach (var n in resultado)
    Console.Write(n + " ");  // Salida: 5 7 8 9
```

### Sintaxis de métodos (Method Syntax)

```csharp
int[] numeros = { 5, 3, 8, 1, 9, 2, 7 };

var resultado = numeros
    .Where(n => n > 4)
    .OrderBy(n => n);

foreach (var n in resultado)
    Console.Write(n + " ");  // Salida: 5 7 8 9
```

> 💡 **Recomendación:** La sintaxis de métodos es más usada en el mundo profesional ya que es más flexible y compatible con todos los operadores LINQ.

---

## 3. Operadores de Filtrado — `Where`

El operador `Where` filtra los elementos de una colección según una condición booleana.

### Ejemplo básico

```csharp
List<int> numeros = new List<int> { 10, 25, 3, 47, 8, 60, 15 };

// Filtrar números mayores a 20
var mayoresA20 = numeros.Where(n => n > 20);

Console.WriteLine("Números mayores a 20:");
foreach (var n in mayoresA20)
    Console.Write(n + " ");
// Salida: 25 47 60
```

### Filtrar con múltiples condiciones

```csharp
List<int> numeros = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// Números pares mayores a 4
var resultado = numeros.Where(n => n % 2 == 0 && n > 4);

foreach (var n in resultado)
    Console.Write(n + " ");
// Salida: 6 8 10
```

### Filtrar cadenas de texto

```csharp
List<string> nombres = new List<string>
    { "Ana", "Carlos", "Alberto", "Beatriz", "Andrés" };

// Nombres que empiezan con "A"
var conA = nombres.Where(n => n.StartsWith("A"));

foreach (var nombre in conA)
    Console.WriteLine(nombre);
// Salida: Ana  Alberto  Andrés
```

---

## 4. Operadores de Proyección — `Select`

El operador `Select` transforma cada elemento de una colección, similar al `SELECT` de SQL.

### Ejemplo: transformar tipos

```csharp
List<string> nombres = new List<string> { "ana", "carlos", "beatriz" };

// Convertir a mayúsculas
var enMayusculas = nombres.Select(n => n.ToUpper());

foreach (var n in enMayusculas)
    Console.WriteLine(n);
// Salida: ANA  CARLOS  BEATRIZ
```

### Ejemplo: proyectar propiedades de un objeto

```csharp
class Producto
{
    public string Nombre { get; set; }
    public double Precio { get; set; }
}

List<Producto> productos = new List<Producto>
{
    new Producto { Nombre = "Laptop", Precio = 1200.00 },
    new Producto { Nombre = "Mouse",  Precio = 25.50  },
    new Producto { Nombre = "Teclado",Precio = 45.00  }
};

// Obtener solo los nombres
var nombres = productos.Select(p => p.Nombre);

foreach (var n in nombres)
    Console.WriteLine(n);
// Salida: Laptop  Mouse  Teclado
```

### Proyección a tipo anónimo

```csharp
var resumen = productos.Select(p => new
{
    p.Nombre,
    PrecioConIVA = p.Precio * 1.12
});

foreach (var item in resumen)
    Console.WriteLine($"{item.Nombre}: ${item.PrecioConIVA:F2}");
// Salida:
// Laptop: $1344.00
// Mouse: $28.56
// Teclado: $50.40
```

---

## 5. Ordenamiento — `OrderBy` / `OrderByDescending`

Permiten ordenar colecciones de forma ascendente o descendente.

### Orden ascendente

```csharp
List<int> numeros = new List<int> { 42, 7, 19, 3, 88, 25 };

var ordenados = numeros.OrderBy(n => n);

foreach (var n in ordenados)
    Console.Write(n + " ");
// Salida: 3 7 19 25 42 88
```

### Orden descendente

```csharp
var descendente = numeros.OrderByDescending(n => n);

foreach (var n in descendente)
    Console.Write(n + " ");
// Salida: 88 42 25 19 7 3
```

### Ordenar objetos por propiedad

```csharp
List<Producto> productos = new List<Producto>
{
    new Producto { Nombre = "Laptop", Precio = 1200.00 },
    new Producto { Nombre = "Mouse",  Precio = 25.50  },
    new Producto { Nombre = "Teclado",Precio = 45.00  }
};

// Ordenar por precio (menor a mayor)
var porPrecio = productos.OrderBy(p => p.Precio);

foreach (var p in porPrecio)
    Console.WriteLine($"{p.Nombre} - ${p.Precio}");
// Salida:
// Mouse - $25.5
// Teclado - $45
// Laptop - $1200
```

### Ordenar por múltiples criterios (`ThenBy`)

```csharp
var multiOrden = productos
    .OrderBy(p => p.Nombre)
    .ThenByDescending(p => p.Precio);
```

---

## 6. Agrupamiento — `GroupBy`

Agrupa los elementos según una clave, similar al `GROUP BY` de SQL.

### Ejemplo: agrupar números pares e impares

```csharp
List<int> numeros = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

var grupos = numeros.GroupBy(n => n % 2 == 0 ? "Par" : "Impar");

foreach (var grupo in grupos)
{
    Console.Write($"{grupo.Key}: ");
    foreach (var n in grupo)
        Console.Write(n + " ");
    Console.WriteLine();
}
// Salida:
// Impar: 1 3 5 7 9
// Par:   2 4 6 8 10
```

### Agrupar objetos por categoría

```csharp
class Empleado
{
    public string Nombre       { get; set; }
    public string Departamento { get; set; }
    public double Salario      { get; set; }
}

List<Empleado> empleados = new List<Empleado>
{
    new Empleado { Nombre = "Luis",    Departamento = "TI",  Salario = 2000 },
    new Empleado { Nombre = "María",   Departamento = "RRHH",Salario = 1800 },
    new Empleado { Nombre = "Carlos",  Departamento = "TI",  Salario = 2200 },
    new Empleado { Nombre = "Sofía",   Departamento = "RRHH",Salario = 1900 },
    new Empleado { Nombre = "Andrés",  Departamento = "TI",  Salario = 2500 }
};

var porDepartamento = empleados.GroupBy(e => e.Departamento);

foreach (var grupo in porDepartamento)
{
    Console.WriteLine($"\nDepartamento: {grupo.Key}");
    foreach (var emp in grupo)
        Console.WriteLine($"  - {emp.Nombre} (${emp.Salario})");
}
```

---

## 7. Operadores de Agregación

Estos operadores calculan un valor único a partir de una colección.

| Operador | Descripción                        |
|----------|------------------------------------|
| `Count()` | Cuenta los elementos              |
| `Sum()`   | Suma los valores                  |
| `Min()`   | Valor mínimo                      |
| `Max()`   | Valor máximo                      |
| `Average()` | Promedio de los valores         |
| `First()` | Primer elemento (lanza excepción si vacío) |
| `FirstOrDefault()` | Primer elemento o valor por defecto |
| `Any()`   | `true` si existe algún elemento que cumpla la condición |
| `All()`   | `true` si todos cumplen la condición |

### Ejemplo completo

```csharp
List<int> notas = new List<int> { 85, 92, 78, 95, 88, 70, 61 };

Console.WriteLine($"Total de estudiantes: {notas.Count()}");
Console.WriteLine($"Nota máxima:          {notas.Max()}");
Console.WriteLine($"Nota mínima:          {notas.Min()}");
Console.WriteLine($"Promedio:             {notas.Average():F2}");
Console.WriteLine($"Suma total:           {notas.Sum()}");
Console.WriteLine($"¿Alguno reprobó (<70)? {notas.Any(n => n < 70)}");
Console.WriteLine($"¿Todos aprobaron?      {notas.All(n => n >= 60)}");

// Salida:
// Total de estudiantes: 7
// Nota máxima:          95
// Nota mínima:          61
// Promedio:             81.29
// Suma total:           569
// ¿Alguno reprobó (<70)? False
// ¿Todos aprobaron?      True
```

### Agregación sobre propiedades de objetos

```csharp
double totalSalarios = empleados.Sum(e => e.Salario);
double promedioSalario = empleados.Average(e => e.Salario);
double salarioMax = empleados.Max(e => e.Salario);

Console.WriteLine($"Total salarios:  ${totalSalarios}");
Console.WriteLine($"Salario promedio:${promedioSalario}");
Console.WriteLine($"Salario más alto:${salarioMax}");
```

---

## 8. Join entre colecciones

El operador `Join` combina dos colecciones basándose en una clave común, similar al `INNER JOIN` de SQL.

### Ejemplo: Clientes y Pedidos

```csharp
class Cliente
{
    public int    Id     { get; set; }
    public string Nombre { get; set; }
}

class Pedido
{
    public int    ClienteId   { get; set; }
    public string Descripcion { get; set; }
    public double Total       { get; set; }
}

List<Cliente> clientes = new List<Cliente>
{
    new Cliente { Id = 1, Nombre = "Ana García" },
    new Cliente { Id = 2, Nombre = "Pedro López" },
    new Cliente { Id = 3, Nombre = "Lucía Torres" }
};

List<Pedido> pedidos = new List<Pedido>
{
    new Pedido { ClienteId = 1, Descripcion = "Laptop",  Total = 1200 },
    new Pedido { ClienteId = 1, Descripcion = "Mouse",   Total = 25   },
    new Pedido { ClienteId = 2, Descripcion = "Teclado", Total = 45   },
    new Pedido { ClienteId = 3, Descripcion = "Monitor", Total = 350  }
};

var resultado = clientes.Join(
    pedidos,
    c => c.Id,
    p => p.ClienteId,
    (c, p) => new
    {
        Cliente = c.Nombre,
        Producto = p.Descripcion,
        p.Total
    }
);

foreach (var item in resultado)
    Console.WriteLine($"{item.Cliente} compró: {item.Producto} por ${item.Total}");

// Salida:
// Ana García compró: Laptop por $1200
// Ana García compró: Mouse por $25
// Pedro López compró: Teclado por $45
// Lucía Torres compró: Monitor por $350
```

---

## 9. LINQ con Listas de Objetos

Este es el uso más frecuente de LINQ en aplicaciones reales. Combinando múltiples operadores se pueden hacer consultas muy potentes.

### Ejemplo integrador: Sistema de Estudiantes

```csharp
class Estudiante
{
    public string Nombre   { get; set; }
    public string Carrera  { get; set; }
    public double Promedio { get; set; }
    public int    Semestre { get; set; }
}

List<Estudiante> estudiantes = new List<Estudiante>
{
    new Estudiante { Nombre="Ana",    Carrera="Software", Promedio=9.2, Semestre=5 },
    new Estudiante { Nombre="Carlos", Carrera="Redes",    Promedio=7.8, Semestre=3 },
    new Estudiante { Nombre="María",  Carrera="Software", Promedio=8.5, Semestre=5 },
    new Estudiante { Nombre="Luis",   Carrera="Software", Promedio=6.1, Semestre=2 },
    new Estudiante { Nombre="Sofía",  Carrera="Redes",    Promedio=9.5, Semestre=7 },
    new Estudiante { Nombre="Pedro",  Carrera="Redes",    Promedio=7.0, Semestre=3 }
};

// 1. Estudiantes de Software con promedio mayor a 8
Console.WriteLine("=== Software con promedio > 8 ===");
var softwareDestacados = estudiantes
    .Where(e => e.Carrera == "Software" && e.Promedio > 8)
    .OrderByDescending(e => e.Promedio);

foreach (var e in softwareDestacados)
    Console.WriteLine($"  {e.Nombre} - Promedio: {e.Promedio}");

// 2. Promedio general por carrera
Console.WriteLine("\n=== Promedio por carrera ===");
var promedioPorCarrera = estudiantes
    .GroupBy(e => e.Carrera)
    .Select(g => new
    {
        Carrera  = g.Key,
        Promedio = g.Average(e => e.Promedio)
    });

foreach (var item in promedioPorCarrera)
    Console.WriteLine($"  {item.Carrera}: {item.Promedio:F2}");

// 3. El mejor estudiante de toda la lista
var mejorEstudiante = estudiantes.OrderByDescending(e => e.Promedio).First();
Console.WriteLine($"\n=== Mejor estudiante: {mejorEstudiante.Nombre} ({mejorEstudiante.Promedio}) ===");
```

---

## 10. Ejercicios Prácticos con Soluciones

### Ejercicio 1 — Filtrado básico

**Enunciado:** Dada una lista de temperaturas en grados Celsius `{ 35, 18, 42, 7, 28, 15, 50, 22 }`, obtén las temperaturas que se consideran "calurosas" (mayores a 30°C) y muéstralas en orden descendente.

**Solución:**

```csharp
List<int> temperaturas = new List<int> { 35, 18, 42, 7, 28, 15, 50, 22 };

var calurosas = temperaturas
    .Where(t => t > 30)
    .OrderByDescending(t => t);

Console.WriteLine("Temperaturas calurosas (desc):");
foreach (var t in calurosas)
    Console.WriteLine($"  {t}°C");
// Salida: 50°C  42°C  35°C
```

---

### Ejercicio 2 — Agregación

**Enunciado:** Tienes una lista de ventas diarias `{ 1500, 2300, 800, 4100, 950, 3200, 1750 }`. Calcula: total de ventas, venta máxima, venta mínima y promedio.

**Solución:**

```csharp
List<double> ventas = new List<double> { 1500, 2300, 800, 4100, 950, 3200, 1750 };

Console.WriteLine($"Total de ventas:  ${ventas.Sum():F2}");
Console.WriteLine($"Venta más alta:   ${ventas.Max():F2}");
Console.WriteLine($"Venta más baja:   ${ventas.Min():F2}");
Console.WriteLine($"Venta promedio:   ${ventas.Average():F2}");
// Salida:
// Total de ventas:  $16600.00
// Venta más alta:   $4100.00
// Venta más baja:   $800.00
// Venta promedio:   $2371.43
```

---

### Ejercicio 3 — LINQ con objetos

**Enunciado:** Crea una lista de 5 libros con propiedades `Titulo`, `Autor`, `Año` y `Precio`. Luego:
- Muestra los libros publicados después del año 2015.
- Muestra el libro más barato.
- Muestra todos los libros ordenados por precio (menor a mayor).

**Solución:**

```csharp
class Libro
{
    public string Titulo { get; set; }
    public string Autor  { get; set; }
    public int    Anio   { get; set; }
    public double Precio { get; set; }
}

List<Libro> libros = new List<Libro>
{
    new Libro { Titulo="Clean Code",       Autor="Robert C. Martin", Anio=2008, Precio=45.00 },
    new Libro { Titulo="The Pragmatic Programmer", Autor="Hunt & Thomas", Anio=2019, Precio=52.00 },
    new Libro { Titulo="C# in Depth",      Autor="Jon Skeet",         Anio=2019, Precio=48.00 },
    new Libro { Titulo="Design Patterns",  Autor="GoF",               Anio=1994, Precio=39.00 },
    new Libro { Titulo="You Don't Know JS",Autor="Kyle Simpson",      Anio=2018, Precio=30.00 }
};

Console.WriteLine("=== Libros desde 2015 ===");
var recientes = libros.Where(l => l.Anio > 2015).OrderBy(l => l.Anio);
foreach (var l in recientes)
    Console.WriteLine($"  [{l.Anio}] {l.Titulo} — ${l.Precio}");

Console.WriteLine("\n=== Libro más barato ===");
var masBarato = libros.OrderBy(l => l.Precio).First();
Console.WriteLine($"  {masBarato.Titulo} — ${masBarato.Precio}");

Console.WriteLine("\n=== Todos ordenados por precio ===");
foreach (var l in libros.OrderBy(l => l.Precio))
    Console.WriteLine($"  ${l.Precio:F2} — {l.Titulo}");
```

---

### Ejercicio 4 — GroupBy y agregación combinados

**Enunciado:** Tienes una lista de productos con su categoría y precio. Agrupa los productos por categoría y muestra cuántos productos hay en cada una y el precio promedio.

**Solución:**

```csharp
class ProductoTienda
{
    public string Categoria { get; set; }
    public string Nombre    { get; set; }
    public double Precio    { get; set; }
}

List<ProductoTienda> inventario = new List<ProductoTienda>
{
    new ProductoTienda { Categoria="Electrónico", Nombre="Laptop",   Precio=1200 },
    new ProductoTienda { Categoria="Electrónico", Nombre="Tablet",   Precio=350  },
    new ProductoTienda { Categoria="Ropa",        Nombre="Camiseta", Precio=25   },
    new ProductoTienda { Categoria="Ropa",        Nombre="Pantalón", Precio=55   },
    new ProductoTienda { Categoria="Ropa",        Nombre="Chaqueta", Precio=120  },
    new ProductoTienda { Categoria="Electrónico", Nombre="Mouse",    Precio=30   },
    new ProductoTienda { Categoria="Alimento",    Nombre="Arroz",    Precio=2.5  },
    new ProductoTienda { Categoria="Alimento",    Nombre="Leche",    Precio=1.8  }
};

var resumenCategorias = inventario
    .GroupBy(p => p.Categoria)
    .Select(g => new
    {
        Categoria  = g.Key,
        Cantidad   = g.Count(),
        PrecioMedio = g.Average(p => p.Precio)
    })
    .OrderBy(r => r.Categoria);

Console.WriteLine($"{"Categoría",-15} {"Cantidad",8} {"Precio Medio",12}");
Console.WriteLine(new string('-', 38));
foreach (var cat in resumenCategorias)
    Console.WriteLine($"{cat.Categoria,-15} {cat.Cantidad,8} {cat.PrecioMedio,12:F2}");

// Salida:
// Categoría       Cantidad  Precio Medio
// --------------------------------------
// Alimento               2         2.15
// Electrónico            3       526.67
// Ropa                   3        66.67
```

---

## Recursos adicionales

- 📖 [Documentación oficial de LINQ — Microsoft](https://learn.microsoft.com/es-es/dotnet/csharp/linq/)
- 📖 [101 ejemplos de LINQ — Microsoft](https://learn.microsoft.com/es-es/samples/dotnet/try-samples/101-linq-samples/)
- 🛠️ [.NET Fiddle — Editor C# online](https://dotnetfiddle.net/)

---

*Manual desarrollado como parte del curso de Programación Avanzada — AngeloChimborazo*
