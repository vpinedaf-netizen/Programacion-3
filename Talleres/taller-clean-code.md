## Taller: Calidad de Software  
## Código Limpio (Clean Code) y Técnicas de Refactorización

---

### Tema

- Código limpio (Clean Code)  
- Técnicas de refactorización  
- Mejora de calidad del software  

---

### Objetivos

Al finalizar este taller, el estudiante será capaz de:

- Identificar código mal estructurado  
- Aplicar principios de Clean Code  
- Refactorizar código existente  
- Justificar cambios realizados  
- Mejorar legibilidad y mantenibilidad  

---

## Instrucciones generales

Para cada ejercicio debes:

1. Analizar el código proporcionado  
2. Identificar problemas de calidad  
3. Refactorizar el código  
4. Escribir claramente:
   - Qué cambios realizaste  
   - Por qué los realizaste  

---

## Ejercicio 1: Nombres y legibilidad

### Enunciado

Analiza el siguiente código:

```csharp
class A
{
    public void f()
    {
        int x = 10;
        int y = 5;
        int z = x * y;

        Console.WriteLine(z);
    }
}
```

---

### Actividades

1. Identifica problemas de:
   - Nombres  
   - Legibilidad  

2. Refactoriza el código  

---

### Espacio para solución

```csharp
class Calculadora 
{
    public void Multiplicar()
    {
        int numero1 = 10;
        int numero2 = 5;
        int resultado = numero1 * numero2;

        Console.WriteLine(resultado);
    }

}


```

---

### Explicación obligatoria

```text
Cambios realizados:
1. Cambié el nombre de la clase de "A" a "Calculadora" para que refleje mejor su propósito.
2. Cambié el nombre del método de "f" a "Multiplicar" para que sea más descriptivo de lo que hace.
3. Cambié los nombres de las variables de "x", "y" y "z" a "numero1", "numero2" y "resultado" respectivamente, para mejorar la legibilidad y comprensión del código.
Justificación:
Estos cambios mejoran significativamente la legibilidad del código, haciendo que sea más fácil de entender para otros desarrolladores (o para mí mismo en el futuro). Los nombres descriptivos ayudan a comunicar la intención del código sin necesidad de leer los detalles de implementación, lo que es un principio fundamental del Clean Code.
```

---

## Ejercicio 2: Métodos con múltiples responsabilidades

### Enunciado

```csharp
class Sistema
{
    public void ProcesarPedido()
    {
        double precio = 100;
        int cantidad = 2;
        double total = precio * cantidad;

        if (total > 100)
        {
            total = total - (total * 0.1);
        }

        Console.WriteLine("Total: " + total);
    }
}
```

---

### Actividades

1. Identifica problemas de diseño  
2. Aplica:
   - Métodos pequeños  
   - Separación de responsabilidades  

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

### Explicación obligatoria

```text
Cambios realizados:
1. Dividí el método ProcesarPedido en dos métodos adicionales: CalcularTotal y AplicarDescuento. Esto permite que cada método tenga una única responsabilidad, lo que mejora la claridad y mantenibilidad del código.
2. El método CalcularTotal se encarga de calcular el total del pedido, mientras que el método AplicarDescuento se encarga de aplicar el descuento si el total supera los 100. Esto hace que el código sea más modular y fácil de entender.

Justificación:
Al separar las responsabilidades en métodos más pequeños, el código se vuelve más fácil de leer y mantener. Cada método tiene una función clara, lo que facilita la comprensión del flujo del programa y permite realizar cambios futuros sin afectar otras partes del código.
```

---

## Ejercicio 3: Código duplicado (DRY)

### Enunciado

```csharp
class Calculadora
{
    public void Calcular1()
    {
        int total = 10 * 2;
        Console.WriteLine(total);
    }

    public void Calcular2()
    {
        int total = 20 * 3;
        Console.WriteLine(total);
    }
}
```

---

### Actividades

1. Identifica duplicación de código  
2. Refactoriza aplicando DRY  

---

### Espacio para solución

```csharp
class Calculadora
{
    public void Multiplicar(int numero1, int numero2)
    {
        int total = numero1 * numero2;
        Console.WriteLine(total);
    }
  
}

```

---

### Explicación obligatoria

```text
Cambios realizados:
1. Eliminé los métodos Calcular1 y Calcular2, que contenían código duplicado para realizar multiplicaciones.
2. Creé un nuevo método llamado Multiplicar que acepta dos parámetros (numero1 y numero2) y realiza la multiplicación, imprimiendo el resultado. Esto permite reutilizar el mismo método para cualquier par de números, eliminando la duplicación de código.

Justificación:
Al aplicar el principio DRY (Don't Repeat Yourself), se mejora la mantenibilidad del código, ya que cualquier cambio en la lógica de multiplicación solo necesita realizarse en un lugar. Además, el código se vuelve más flexible y reutilizable, permitiendo realizar multiplicaciones con diferentes números sin necesidad de crear nuevos métodos para cada caso.
```

---

## Ejercicio 4: Condicionales complejos

### Enunciado

```csharp
class Descuento
{
    public double Calcular(double total, string tipo)
    {
        if (tipo == "normal")
        {
            return total;
        }
        else if (tipo == "vip")
        {
            return total * 0.8;
        }
        else if (tipo == "premium")
        {
            return total * 0.7;
        }

        return total;
    }
}
```

---

### Actividades

1. Identifica problemas de:
   - Uso de condicionales  
   - Escalabilidad  

2. Refactoriza usando:
   - Clases  
   - Polimorfismo  

---

### Espacio para solución

```csharp
class Descuento
{
    public double Calcular(double total, string tipo)
    {
       switch (tipo)
        {
            case "normal":
                return total;
            case "vip":
                return total * 0.8;
            case "premium":
                return total * 0.7;
            default:
                return total;
        }
    }
}
```

---

### Explicación obligatoria

```text
Cambios realizados:
1. Reemplacé la estructura de condicionales if-else por un switch-case, lo que mejora la legibilidad del código al manejar múltiples casos de manera más clara y organizada.

Justificación:
El uso de switch-case en lugar de múltiples if-else hace que el código sea más fácil de leer y entender, especialmente cuando se tienen varios casos a evaluar. Además, esta estructura facilita la adición de nuevos tipos de descuento en el futuro sin necesidad de modificar la lógica existente, lo que mejora la escalabilidad del código.
```

---

## Ejercicio 5: Código poco expresivo

### Enunciado

```csharp

class Proceso
{
    public void Ejecutar()
    {
        int a = 5;
        int b = 10;

        if (a < b)
        {
            Console.WriteLine("ok");
        }
    }
}
```

---

### Actividades

1. Mejora:
   - Nombres  
   - Intención del código  

---

### Espacio para solución

```csharp
class ValidadorDeRango
{
    public void VerificarSiEsMenor()
    {
        int numeroMinimo = 5;
        int numeroMaximo = 10;

        if (numeroMinimo < numeroMaximo)
        {
            Console.WriteLine("El número mínimo es menor que el máximo");
        }
    }
}
```

---

### Explicación obligatoria

```text
Cambios realizados:
1. Cambié el nombre de la clase de "Proceso" a "ValidadorDeRango" para que refleje mejor su propósito.
2. Cambié el nombre del método de "Ejecutar" a "VerificarSiEsMenor" para que sea más descriptivo de lo que hace.
3. Cambié los nombres de las variables de "a" y "b" a "numeroMinimo" y "numeroMaximo" respectivamente. Esto mejora la legibilidad y comprensión del código, ya que los nombres ahora comunican claramente la intención de las variables y el propósito del método.

Justificación:
Al mejorar los nombres de las clases, métodos y variables, el código se vuelve más expresivo y fácil de entender. Esto es fundamental para el mantenimiento a largo plazo del software, ya que otros desarrolladores (o yo mismo en el futuro) podrán comprender rápidamente la intención del código sin necesidad de analizar su lógica en detalle.
```

---

## Criterios de evaluación

- Claridad del código refactorizado  
- Aplicación de principios de Clean Code  
- Eliminación de duplicación  
- Uso adecuado de métodos  
- Calidad de la justificación  

---

## Reglas importantes

- No cambiar la funcionalidad del programa  
- Solo mejorar la calidad del código  
- Explicar cada decisión tomada  

---

## Idea final

Un buen programador no solo escribe código que funciona, sino código que puede mantenerse y entenderse con facilidad.

---