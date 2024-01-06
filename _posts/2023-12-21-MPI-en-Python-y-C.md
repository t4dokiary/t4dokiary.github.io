---
title: MPI en Python y C 
author: t4dokiary
date: 2023-12-21 23:15:00 +0800
categories: [Apuntes, Sistemas Operativos 2, Programación Paralela, MPI, Python, C, Clase]
tags: [c, python, mpi]
render_with_liquid: false
image:
  path: /assets/img/2023-12-21-MPI-en-python-y-c/mpi-logo.png
---

MPI es un estándar de comunicación de mensajes para la programación paralela en sistemas de computación distribuida. Es ampliamente utilizado en la computación de alto rendimiento (HPC) para permitir que los procesos de un programa paralelo comuniquen y sincronicen sus operaciones a través de diferentes nodos de procesamiento.

La principal ventaja de MPI es su capacidad para permitir la escalabilidad y la eficiencia en sistemas con una gran cantidad de procesadores. MPI define un conjunto de operaciones y tipos de datos para el intercambio de mensajes, lo que facilita la escritura de programas que pueden ejecutarse en paralelo en diferentes nodos de un clúster o una red.

MPI es independiente del lenguaje de programación, aunque sus implementaciones más comunes están disponibles para lenguajes como C, C++ y Fortran. Ofrece diversas funciones para el envío y recepción de mensajes, la gestión de grupos de procesos y la comunicación colectiva entre múltiples procesos.

En resumen, MPI es un estándar esencial para la computación paralela y distribuida, proporcionando las herramientas necesarias para desarrollar aplicaciones que pueden aprovechar múltiples procesadores simultáneamente para resolver problemas complejos de manera más eficiente.

## Tema 1: Introducción a la Programación Paralela y MPI

### ¿Qué es la programación paralela?

La programación paralela es un enfoque de programación en el que se utilizan múltiples recursos de cómputo (como CPU o nodos de un clúster) para resolver un problema de manera simultánea. En lugar de ejecutar instrucciones de manera secuencial, las tareas se dividen en subprocesos o procesos independientes que pueden ejecutarse en paralelo. Esto puede resultar en un rendimiento significativamente mejorado para ciertos tipos de aplicaciones, como cálculos intensivos o tareas que se pueden descomponer en partes independientes.

### Introducción a MPI y su importancia

MPI (Message Passing Interface) es una biblioteca ampliamente utilizada para la programación paralela y distribuida. Fue diseñada para permitir la comunicación entre procesos en entornos paralelos y distribuidos. MPI es especialmente relevante en entornos de cómputo de alto rendimiento, como supercomputadoras y clústeres, donde la ejecución paralela de tareas puede aprovechar al máximo los recursos disponibles.

Algunas de las características importantes de MPI son:

- **Comunicación**: MPI proporciona mecanismos para que los procesos se comuniquen entre sí, lo que permite la coordinación y el intercambio de datos en aplicaciones paralelas.

- **Portabilidad**: MPI es un estándar ampliamente aceptado y está disponible en múltiples plataformas y lenguajes de programación.

- **Escalabilidad**: MPI es escalable y puede utilizarse en sistemas con un gran número de procesadores o nodos.

- **Flexibilidad**: MPI permite una amplia gama de patrones de comunicación, desde comunicaciones punto a punto hasta comunicaciones colectivas en grupos de procesos.

### Configuración del entorno de desarrollo

Para comenzar a trabajar con MPI, necesitas configurar un entorno de desarrollo. Aquí están los pasos básicos:

1. **Instala MPI**: Asegúrate de que MPI esté instalado en tu sistema. Puedes utilizar implementaciones populares como Open MPI o MPICH.

2. **Selecciona un lenguaje de programación**: Decide si deseas programar en Python o C, ya que MPI está disponible en ambos lenguajes.

3. **Configura tu entorno de desarrollo**: Configura tu IDE (Entorno de Desarrollo Integrado) o editor de texto preferido para trabajar con MPI en el lenguaje elegido.

4. **Aprende los conceptos básicos**: Familiarízate con los conceptos básicos de MPI, como la inicialización, la finalización y las operaciones de comunicación.

5. **Ejemplos simples**: Comienza con ejemplos simples para comprender cómo funciona MPI en la práctica. Puedes encontrar ejemplos en la documentación de MPI y tutoriales en línea.

6. **Explora recursos educativos**: Busca tutoriales, libros y cursos en línea que te ayuden a profundizar en la programación paralela con MPI.

Si tienes alguna pregunta específica o si deseas continuar con algún aspecto particular de este tema, no dudes en preguntar. ¡Estoy aquí para ayudarte a aprender!


## Tema 2: Conceptos Básicos de MPI

### Comunicación Punto a Punto en MPI

En MPI, la comunicación punto a punto se refiere a la transferencia de datos entre procesos individuales. Aquí tienes algunos conceptos clave:

- **Rank**: Cada proceso en MPI tiene un identificador único llamado "rank". Los procesos se comunican entre sí utilizando los rangos para especificar el destino de los datos.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Proceso 0 envía un mensaje al proceso 1 utilizando los rangos
if rank == 0:
    message = "Hola, proceso 1"
    dest_rank = 1
    comm.send(message, dest=dest_rank)
    print(f"Proceso {rank} envía un mensaje a proceso {dest_rank}: {message}")
    
# Proceso 1 recibe el mensaje del proceso 0
if rank == 1:
    source_rank = 0
    message = comm.recv(source=source_rank)
    print(f"Proceso {rank} recibe un mensaje de proceso {source_rank}: {message}")
```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Proceso 0 envía un mensaje al proceso 1 utilizando los rangos
    if (rank == 0) {
        char message[] = "Hola, proceso 1";
        int dest_rank = 1;
        MPI_Send(message, sizeof(message), MPI_CHAR, dest_rank, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un mensaje a proceso %d: %s\n", rank, dest_rank, message);
    }
    
    // Proceso 1 recibe el mensaje del proceso 0
    if (rank == 1) {
        int source_rank = 0;
        char message[15];
        MPI_Recv(message, sizeof(message), MPI_CHAR, source_rank, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un mensaje de proceso %d: %s\n", rank, source_rank, message);
    }

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

- **Envío y Recepción**: Los procesos pueden enviar datos a otros procesos utilizando funciones como `MPI_Send` y recibir datos utilizando funciones como `MPI_Recv`. Estas funciones especifican el buffer de datos, el tamaño, el tipo de dato y el destino del envío o la fuente de la recepción.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Proceso 0 envía un número al proceso 1
if rank == 0:
    data = 42
    comm.send(data, dest=1)
    print(f"Proceso {rank}: Envía {data} al proceso 1")
    
# Proceso 1 recibe el número del proceso 0
if rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank}: Recibe {data} del proceso 0")
```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank, data;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Status status;

    // Proceso 0 envía un número al proceso 1
    if (rank == 0) {
        data = 42;
        MPI_Send(&data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d: Envía %d al proceso 1\n", rank, data);
    }
    
    // Proceso 1 recibe el número del proceso 0
    if (rank == 1) {
        MPI_Recv(&data, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, &status);
        printf("Proceso %d: Recibe %d del proceso 0\n", rank, data);
    }

    MPI_Finalize();
    return 0;
}

```
{: .nolineno}

- **Tipos de Datos**: MPI permite enviar y recibir datos de diferentes tipos, incluidos enteros, flotantes, arreglos y estructuras personalizadas. Debes especificar el tipo de dato correctamente para garantizar una comunicación exitosa.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un número entero
if rank == 0:
    data = 42
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un entero: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un entero: {data}")

# Enviar y recibir un número de punto flotante
if rank == 1:
    data = 3.14159
    comm.send(data, dest=0)
    print(f"Proceso {rank} envía un flotante: {data}")
elif rank == 0:
    data = comm.recv(source=1)
    print(f"Proceso {rank} recibe un flotante: {data}")
```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un número entero
    if (rank == 0) {
        int int_data = 42;
        MPI_Send(&int_data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un entero: %d\n", rank, int_data);
    } else if (rank == 1) {
        int int_data;
        MPI_Recv(&int_data, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un entero: %d\n", rank, int_data);
    }

    // Enviar y recibir un número de punto flotante
    if (rank == 1) {
        float float_data = 3.14159;
        MPI_Send(&float_data, 1, MPI_FLOAT, 0, 1, MPI_COMM_WORLD);
        printf("Proceso %d envía un flotante: %f\n", rank, float_data);
    } else if (rank == 0) {
        float float_data;
        MPI_Recv(&float_data, 1, MPI_FLOAT, 1, 1, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un flotante: %f\n", rank, float_data);
    }

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

### Comunicación Colectiva en MPI

La comunicación colectiva en MPI involucra a grupos de procesos en lugar de operaciones individuales de envío y recepción. Algunos ejemplos de operaciones de comunicación colectiva incluyen:

- **Broadcast**: Un proceso envía un dato a todos los demás procesos en el grupo.



``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()
data = None

if rank == 0:
    data = 42  # El proceso raíz tiene el valor a enviar

# Broadcast del valor desde el proceso 0 a todos los demás procesos
data = comm.bcast(data, root=0)

print(f"Proceso {rank}: El valor recibido es {data}")
```
{: .nolineno}



``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank, data;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    data = 0;  // Inicializamos data en todos los procesos

    if (rank == 0) {
        data = 42;  // El proceso raíz tiene el valor a enviar
    }

    // Broadcast del valor desde el proceso 0 a todos los demás procesos
    MPI_Bcast(&data, 1, MPI_INT, 0, MPI_COMM_WORLD);

    printf("Proceso %d: El valor recibido es %d\n", rank, data);

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

- **Scatter**: Un proceso distribuye una matriz de datos a todos los procesos del grupo.

``` python
from mpi4py import MPI
import numpy as np

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Datos que se enviarán desde el proceso raíz (proceso 0)
if rank == 0:
    data = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], dtype='i')
else:
    data = np.empty(10, dtype='i')

# Distribuir la matriz entre todos los procesos utilizando Scatter
comm.Scatter(data, data, root=0)

# Cada proceso tiene su porción de la matriz
print(f"Proceso {rank}: Recibió datos {data}")

```
{: .nolineno}



``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank, data[10];
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Datos que se enviarán desde el proceso raíz (proceso 0)
    if (rank == 0) {
        int original_data[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        for (int i = 0; i < 10; i++) {
            data[i] = original_data[i];
        }
    }

    // Distribuir la matriz entre todos los procesos utilizando Scatter
    MPI_Scatter(data, 1, MPI_INT, data, 1, MPI_INT, 0, MPI_COMM_WORLD);

    // Cada proceso tiene su porción de la matriz
    printf("Proceso %d: Recibió datos [%d]\n", rank, data[0]);

    MPI_Finalize();
    return 0;
}

```
{: .nolineno}


- **Gather**: Los procesos recopilan datos de todos los demás procesos en un solo proceso.

``` python
from mpi4py import MPI
import numpy as np

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Datos que se enviarán desde cada proceso
data = rank + 1  # Cada proceso envía su rango + 1

# Inicializa una matriz vacía para recopilar los datos
gathered_data = np.empty(comm.Get_size(), dtype='i')

# Recopila los datos de todos los procesos en el proceso raíz (proceso 0)
comm.Gather(data, gathered_data, root=0)

# El proceso raíz imprime los datos recopilados
if rank == 0:
    print("Proceso raíz (0) ha recopilado los datos:", gathered_data)

```
{: .nolineno}



``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank, data;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Datos que se enviarán desde cada proceso
    data = rank + 1;  // Cada proceso envía su rango + 1

    // Inicializa un arreglo vacío para recopilar los datos
    int gathered_data[4];  // Supongamos que hay 4 procesos

    // Recopila los datos de todos los procesos en el proceso raíz (proceso 0)
    MPI_Gather(&data, 1, MPI_INT, gathered_data, 1, MPI_INT, 0, MPI_COMM_WORLD);

    // El proceso raíz imprime los datos recopilados
    if (rank == 0) {
        printf("Proceso raíz (0) ha recopilado los datos: ");
        for (int i = 0; i < 4; i++) {
            printf("%d ", gathered_data[i]);
        }
        printf("\n");
    }

    MPI_Finalize();
    return 0;
}

```
{: .nolineno}

- **Reduce**: Los procesos combinan datos de todos los demás procesos en un solo resultado.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Datos que se enviarán desde cada proceso
data = rank + 1  # Cada proceso tiene un valor diferente

# Reducción de datos: se sumarán todos los valores
result = comm.reduce(data, op=MPI.SUM, root=0)

# El proceso raíz (0) imprime el resultado final
if rank == 0:
    print("Resultado final de la reducción:", result)

```
{: .nolineno}



``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank, data, result;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Datos que se enviarán desde cada proceso
    data = rank + 1;  // Cada proceso tiene un valor diferente

    // Reducción de datos: se sumarán todos los valores
    MPI_Reduce(&data, &result, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    // El proceso raíz (0) imprime el resultado final
    if (rank == 0) {
        printf("Resultado final de la reducción: %d\n", result);
    }

    MPI_Finalize();
    return 0;
}

```
{: .nolineno}

- **All-to-All**: Cada proceso envía datos a todos los demás procesos y recibe datos de todos los demás.

``` python
from mpi4py import MPI
import numpy as np

comm = MPI.COMM_WORLD
rank = comm.Get_rank()
size = comm.Get_size()

# Datos que se enviarán desde cada proceso (números del 1 al 4 en este caso)
send_data = np.array([rank + 1] * size, dtype='i')

# Crear un arreglo vacío para recibir datos
recv_data = np.empty(size, dtype='i')

# Realizar la operación All-to-All
comm.Alltoall(send_data, recv_data)

# Imprimir los datos recibidos por cada proceso
print(f"Proceso {rank} ha recibido datos: {recv_data}")

```
{: .nolineno}



``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank, data[4];  // Supongamos que hay 4 procesos
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Status status;

    // Datos que se enviarán desde cada proceso (números del 1 al 4 en este caso)
    for (int i = 0; i < 4; i++) {
        data[i] = rank + 1;
    }

    // Crear un arreglo vacío para recibir datos
    int recv_data[4];

    // Realizar la operación All-to-All
    MPI_Alltoall(data, 1, MPI_INT, recv_data, 1, MPI_INT, MPI_COMM_WORLD);

    // Imprimir los datos recibidos por cada proceso
    printf("Proceso %d ha recibido datos: [%d, %d, %d, %d]\n", rank, recv_data[0], recv_data[1], recv_data[2], recv_data[3]);

    MPI_Finalize();
    return 0;
}

```
{: .nolineno}

### Tipos de Datos en MPI

MPI admite una variedad de tipos de datos predefinidos y permite definir tipos de datos personalizados. Algunos de los tipos de datos más comunes en MPI son `MPI_INT`, `MPI_FLOAT`, `MPI_DOUBLE`, etc. También puedes crear tipos de datos personalizados para estructuras de datos más complejas.

#### MPI_INT: Entero.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un número entero
if rank == 0:
    data = 42
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un entero: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un entero: {data}")
```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un número entero
    if (rank == 0) {
        int int_data = 42;
        MPI_Send(&int_data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un entero: %d\n", rank, int_data);
    } else if (rank == 1) {
        int int_data;
        MPI_Recv(&int_data, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un entero: %d\n", rank, int_data);
    }

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

#### MPI_FLOAT: Punto flotante de precisión simple.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un número de punto flotante
if rank == 0:
    data = 3.14159
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un flotante: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un flotante: {data}")
```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un número de punto flotante
    if (rank == 0) {
        float float_data = 3.14159;
        MPI_Send(&float_data, 1, MPI_FLOAT, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un flotante: %f\n", rank, float_data);
    } else if (rank == 1) {
        float float_data;
        MPI_Recv(&float_data, 1, MPI_FLOAT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un flotante: %f\n", rank, float_data);
    }

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

#### MPI_DOUBLE: Punto flotante de doble precisión.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un número de punto flotante de doble precisión
if rank == 0:
    data = 3.141592653589793
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un double: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un double: {data}")
```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un número de punto flotante de doble precisión
    if (rank == 0) {
        double double_data = 3.141592653589793;
        MPI_Send(&double_data, 1, MPI_DOUBLE, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un double: %lf\n", rank, double_data);
    } else if (rank == 1) {
        double double_data;
        MPI_Recv(&double_data, 1, MPI_DOUBLE, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un double: %lf\n", rank, double_data);
    }

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

#### MPI_CHAR: Carácter.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un carácter (byte)
if rank == 0:
    data = 'A'
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un carácter: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un carácter: {data}")

```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un carácter (byte)
    if (rank == 0) {
        char char_data = 'A';
        MPI_Send(&char_data, 1, MPI_CHAR, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un carácter: %c\n", rank, char_data);
    } else if (rank == 1) {
        char char_data;
        MPI_Recv(&char_data, 1, MPI_CHAR, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un carácter: %c\n", rank, char_data);
    }

    MPI_Finalize();
    return 0;
}

```
{: .nolineno}

#### MPI_BYTE: Byte (utilizado para datos sin formato).

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir datos en formato MPI_BYTE
if rank == 0:
    data = b'Hello, World!'  # Datos en formato bytes
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía datos en formato MPI_BYTE: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe datos en formato MPI_BYTE: {data.decode('utf-8')}")

```
{: .nolineno}

``` c
#include <stdio.h>
#include <string.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir datos en formato MPI_BYTE
    if (rank == 0) {
        char data[] = "Hello, World!";  // Datos en formato char array
        int data_size = strlen(data) + 1;  // Tamaño de los datos incluyendo el carácter nulo
        MPI_Send(data, data_size, MPI_BYTE, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía datos en formato MPI_BYTE: %s\n", rank, data);
    } else if (rank == 1) {
        char data[20];  // Seleccionamos un tamaño suficiente para recibir
        MPI_Recv(data, sizeof(data), MPI_BYTE, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe datos en formato MPI_BYTE: %s\n", rank, data);
    }

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

#### MPI_LONG: Entero largo.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un número entero largo
if rank == 0:
    data = 1234567890123456789
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un número entero largo: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un número entero largo: {data}")

```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un número entero largo
    if (rank == 0) {
        long long_data = 1234567890123456789LL;
        MPI_Send(&long_data, 1, MPI_LONG, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un número entero largo: %lld\n", rank, long_data);
    } else if (rank == 1) {
        long long_data;
        MPI_Recv(&long_data, 1, MPI_LONG, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un número entero largo: %lld\n", rank, long_data);
    }

    MPI_Finalize();
    return 0;
}

```
{: .nolineno}

#### MPI_UNSIGNED: Entero sin signo.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un número entero sin signo
if rank == 0:
    data = 12345
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un número entero sin signo: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un número entero sin signo: {data}")
```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un número entero sin signo
    if (rank == 0) {
        unsigned int unsigned_data = 12345;
        MPI_Send(&unsigned_data, 1, MPI_UNSIGNED, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un número entero sin signo: %u\n", rank, unsigned_data);
    } else if (rank == 1) {
        unsigned int unsigned_data;
        MPI_Recv(&unsigned_data, 1, MPI_UNSIGNED, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un número entero sin signo: %u\n", rank, unsigned_data);
    }

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

#### MPI_SHORT: Entero corto.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un número entero corto
if rank == 0:
    data = 12345
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un número entero corto: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un número entero corto: {data}")
```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un número entero corto
    if (rank == 0) {
        short short_data = 12345;
        MPI_Send(&short_data, 1, MPI_SHORT, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un número entero corto: %hd\n", rank, short_data);
    } else if (rank == 1) {
        short short_data;
        MPI_Recv(&short_data, 1, MPI_SHORT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un número entero corto: %hd\n", rank, short_data);
    }

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

#### MPI_UNSIGNED_LONG representa un número entero largo sin signo.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un número entero largo sin signo
if rank == 0:
    data = 12345678901234567890
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un número entero largo sin signo: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un número entero largo sin signo: {data}")

```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un número entero largo sin signo
    if (rank == 0) {
        unsigned long unsigned_long_data = 12345678901234567890UL;
        MPI_Send(&unsigned_long_data, 1, MPI_UNSIGNED_LONG, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un número entero largo sin signo: %lu\n", rank, unsigned_long_data);
    } else if (rank == 1) {
        unsigned long unsigned_long_data;
        MPI_Recv(&unsigned_long_data, 1, MPI_UNSIGNED_LONG, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un número entero largo sin signo: %lu\n", rank, unsigned_long_data);
    }

    MPI_Finalize();
    return 0;
}

```
{: .nolineno}



#### MPI_LONG_DOUBLE: Punto flotante de precisión extendida.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Enviar y recibir un número de punto flotante de precisión extendida
if rank == 0:
    data = 3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170679
    comm.send(data, dest=1)
    print(f"Proceso {rank} envía un long double: {data}")
elif rank == 1:
    data = comm.recv(source=0)
    print(f"Proceso {rank} recibe un long double: {data}")
```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main() {
    int rank;
    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Enviar y recibir un número de punto flotante de precisión extendida
    if (rank == 0) {
        long double long_double_data = 3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170679L;
        MPI_Send(&long_double_data, 1, MPI_LONG_DOUBLE, 1, 0, MPI_COMM_WORLD);
        printf("Proceso %d envía un long double: %.15Lf\n", rank, long_double_data);
    } else if (rank == 1) {
        long double long_double_data;
        MPI_Recv(&long_double_data, 1, MPI_LONG_DOUBLE, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso %d recibe un long double: %.15Lf\n", rank, long_double_data);
    }

    MPI_Finalize();
    return 0;
}
```
{: .nolineno}

### IEEE 754 y Tipos de Datos en MPI

#### Introducción

En el mundo de la programación y la computación, es esencial comprender cómo se representan y manejan los números de punto flotante, especialmente al trabajar con operaciones matemáticas y cálculos precisos. El estándar IEEE 754 desempeña un papel crucial en esta área, ya que establece las reglas para representar números de punto flotante en sistemas informáticos. Además, cuando trabajamos con MPI (Message Passing Interface) para la programación paralela, es fundamental conocer los tipos de datos disponibles. A continuación, se explica brevemente qué es IEEE 754 y se presenta una tabla simplificada de los tipos de datos MPI más comunes.

#### IEEE 754

IEEE 754 es un estándar internacional que define la forma en que las computadoras representan y realizan operaciones con números de punto flotante. Establece reglas para representar números con decimales en formato binario y proporciona normas para redondear resultados de cálculos, lo que garantiza la consistencia en diferentes sistemas. IEEE 754 también maneja números especiales como el cero, infinito y valores "NaN" (No es un Número).

#### Tipos de Datos MPI y sus Rangos

| Tipo de Dato MPI   | Descripción                   | Valor Mínimo   | Valor Máximo   |
|--------------------|-------------------------------|-----------------|-----------------|
| `MPI_INT`          | Entero                        | -2,147,483,648  | 2,147,483,647  |
| `MPI_FLOAT`        | Punto flotante simple         | IEEE 754        | IEEE 754        |
| `MPI_DOUBLE`       | Punto flotante doble          | IEEE 754        | IEEE 754        |
| `MPI_CHAR`         | Carácter (byte)               | 1 byte          | 1 byte          |
| `MPI_BYTE`         | Byte (sin formato)            | 1 byte          | No aplicable    |
| `MPI_LONG`         | Entero largo                  | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |
| `MPI_UNSIGNED`     | Entero sin signo              | 0               | 4,294,967,295  |
| `MPI_SHORT`        | Entero corto                  | -32,768         | 32,767          |
| `MPI_UNSIGNED_LONG` | Entero largo sin signo       | 0               | 18,446,744,073,709,551,615 |
| `MPI_LONG_DOUBLE`  | Punto flotante extendido      | IEEE 754        | IEEE 754        |

#### Conclusión

Entender IEEE 754 y los tipos de datos MPI es esencial para desarrollar aplicaciones paralelas eficientes y precisas. Estos estándares y tipos de datos garantizan que los cálculos numéricos se realicen de manera consistente en diferentes plataformas y sistemas, lo que facilita la programación en paralelo y la comunicación entre procesos.



### Manejo de Comunicadores

Los comunicadores son objetos que definen grupos de procesos en los que ocurren las operaciones MPI. MPI proporciona un comunicador predeterminado llamado `MPI_COMM_WORLD`, que incluye todos los procesos. También puedes crear comunicadores personalizados para grupos específicos de procesos.

Es importante entender cómo crear y usar comunicadores adecuadamente para controlar las operaciones de comunicación en grupos específicos de procesos.

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# Crear un comunicador para procesos pares
if rank % 2 == 0:
    new_comm = comm.Split(0, rank)
else:
    new_comm = comm.Split(1, rank)

# Ahora, solo los procesos pares tienen new_comm
if new_comm != MPI.COMM_NULL:
    new_rank = new_comm.Get_rank()
    print(f"Proceso {rank} en new_comm tiene rango {new_rank}")

```
{: .nolineno}

``` c
#include <stdio.h>
#include <mpi.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);
    
    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    MPI_Comm new_comm;
    
    // Crear un comunicador para procesos pares
    if (rank % 2 == 0) {
        MPI_Comm_split(MPI_COMM_WORLD, 0, rank, &new_comm);
    } else {
        MPI_Comm_split(MPI_COMM_WORLD, 1, rank, &new_comm);
    }
    
    int new_rank, new_size;
    MPI_Comm_rank(new_comm, &new_rank);
    MPI_Comm_size(new_comm, &new_size);

    if (new_comm != MPI_COMM_NULL) {
        printf("Proceso %d en new_comm tiene rango %d de %d procesos\n", rank, new_rank, new_size);
    }
    
    MPI_Finalize();
    return 0;
}

```
{: .nolineno}


## Tema 3: Inicialización y Finalización de MPI

La inicialización de MPI en Python y C es un paso fundamental para utilizar la biblioteca MPI (Message Passing Interface) en programas que requieren cómputo paralelo y comunicación entre procesos en sistemas distribuidos o clústeres de computadoras. MPI proporciona una interfaz estándar para la comunicación y la sincronización entre procesos en paralelo.

### MPI en C:
En C, la inicialización de MPI se realiza mediante una serie de funciones proporcionadas por la biblioteca MPI. A continuación, se describen los pasos básicos para inicializar MPI en un programa C:

Incluir la biblioteca MPI: Debes incluir el encabezado `<mpi.h>` en tu programa para acceder a las funciones y constantes de MPI.

``` c
#include <mpi.h>
```
{: .nolineno}

Inicialización de MPI: Utiliza la función `MPI_Init` para inicializar el entorno MPI.

``` c
MPI_Init(&argc, &argv);
```
{: .nolineno}

`&argc` y `&argv` son los argumentos de la línea de comandos del programa principal. MPI puede requerir información adicional para inicializarse correctamente.
Obtener el rango y el tamaño del comunicador: Después de inicializar MPI, puedes usar `MPI_Comm_rank` y `MPI_Comm_size` para obtener el rango (identificador único) y el tamaño (número total de procesos) del comunicador MPI.

``` c
int rank, size;
MPI_Comm_rank(MPI_COMM_WORLD, &rank);
MPI_Comm_size(MPI_COMM_WORLD, &size);
```
{: .nolineno}

Operaciones MPI: Una vez inicializado MPI y obtenidos el rango y el tamaño, puedes usar las funciones MPI para realizar comunicación y sincronización entre los procesos.

Finalización de MPI: Finaliza el entorno MPI antes de que el programa termine utilizando la función `MPI_Finalize`.

```c
MPI_Finalize();
``` 
{: .nolineno}

### MPI en Python 3:
En Python, puedes utilizar la biblioteca `mpi4py` para trabajar con MPI. A continuación, se describen los pasos básicos para inicializar MPI en un programa Python:

Importar mpi4py: Debes importar la biblioteca `mpi4py` en tu programa para acceder a las funciones y clases relacionadas con MPI.

```python
from mpi4py import MPI
``` 
{: .nolineno}

Inicialización de MPI: Crea un objeto `MPI.COMM_WORLD` para representar el comunicador MPI predeterminado y utiliza `MPI.COMM_WORLD` para inicializar MPI.

```python
comm = MPI.COMM_WORLD
```
{: .nolineno}

Obtener el rango y el tamaño del comunicador: Utiliza `comm.Get_rank()` y `comm.Get_size()` para obtener el rango (identificador único) y el tamaño (número total de procesos) del comunicador MPI.

```python
rank = comm.Get_rank()
size = comm.Get_size()
```
{: .nolineno}

Operaciones MPI: Después de la inicialización, puedes utilizar las funciones y métodos proporcionados por `mpi4py` para realizar comunicación y sincronización entre procesos.

Finalización de MPI: Al final del programa, no es necesario finalizar explícitamente MPI en Python; `mpi4p` y lo maneja automáticamente.

La inicialización de MPI es esencial para asegurar que los procesos de tu programa puedan comunicarse de manera efectiva en un entorno paralelo. Cada proceso debe ser consciente de su rol y sus relaciones con otros procesos, y esto se logra mediante la inicialización de MPI. Una vez inicializado, puedes utilizar las funciones y métodos de MPI para realizar diversas operaciones de comunicación y cómputo paralelo en tus programas.

## Tema 4: Desarrollo de Programas MPI en Python

### Creación de un programa MPI simple en Python.

La creación de un programa MPI simple en Python implica los siguientes pasos:

  - Importar mpi4py: Debes importar la biblioteca `mpi4py` en tu programa para acceder a las funciones y clases relacionadas con MPI.
  
  - Inicialización de MPI: Crear un objeto `MPI.COMM_WORLD` para representar el comunicador MPI predeterminado y utilizar `MPI.COMM_WORLD` para inicializar MPI.

  - Obtener el rango y el tamaño del comunicador: Utilizar `comm.Get_rank()` y `comm.Get_size()` para obtener el rango (identificador único) y el tamaño (número total de procesos) del comunicador MPI.

  - Operaciones MPI: Después de la inicialización, puedes utilizar las funciones y métodos proporcionados por mpi4py para realizar comunicación y sincronización entre procesos.

  - Finalización de MPI: En Python, no es necesario finalizar explícitamente MPI; mpi4py lo maneja automáticamente.
  

### Compilación y ejecución de programas MPI en Python.

A diferencia de los programas MPI en C que requieren ser compilados antes de la ejecución, los programas MPI en Python no se compilan. Puedes ejecutar un programa MPI en Python directamente utilizando el comando mpiexec o mpirun. Por ejemplo:

``` zsh
mpiexec -n <numero_de_procesos> python mi_programa.py
```
{: .nolineno}

Reemplaza `<numero_de_procesos>` por la cantidad deseada de procesos que deseas utilizar en paralelo.

### Ejemplos prácticos.

En el contexto de MPI, los ejemplos prácticos pueden incluir:

  - *Multiplicación de Matrices*: Dividir una matriz en bloques y realizar la multiplicación de matrices en paralelo utilizando MPI.

  - *Simulaciones Monte Carlo*: Acelerar simulaciones Monte Carlo utilizando múltiples procesos MPI para calcular múltiples trayectorias en paralelo.

  - *Resolución de Ecuaciones Diferenciales Parciales (EDP)*: Dividir el dominio de una EDP en subdominios y resolverlos en paralelo con MPI.

  - *Procesamiento de Imágenes y Videos*: Procesar imágenes o videos en paralelo utilizando múltiples procesos para acelerar el procesamiento.

La clave para resolver estos problemas en paralelo con MPI radica en dividir el problema en tareas independientes que pueden ejecutarse en paralelo y utilizar las capacidades de comunicación de MPI para sincronizar y combinar los resultados. Estos ejemplos demuestran cómo MPI puede acelerar significativamente el procesamiento y la computación en paralelo en una variedad de aplicaciones científicas y de ingeniería.


## Tema 5: Desarrollo de Programas MPI en C

### Creación de un programa MPI simple en C.

  - Incluir la biblioteca MPI: Para crear un programa MPI en C, debes incluir el encabezado `<mpi.h>` en tu código para acceder a las funciones y constantes de MPI.

  - Inicialización de MPI: Utiliza la función `MPI_Init` para inicializar el entorno MPI. Esta función debe llamarse al principio de tu programa.

  - Obtener el rango y el tamaño del comunicador: Utiliza las funciones `MPI_Comm_rank` y `MPI_Comm_size` para obtener el rango (identificador único) y el tamaño (número total de procesos) del comunicador MPI. Estos valores te ayudarán a determinar el rol de cada proceso en la ejecución paralela.

  - Operaciones MPI: Después de la inicialización, puedes utilizar las funciones MPI para realizar comunicación y sincronización entre procesos. Esto puede incluir el envío y recepción de datos entre procesos.

  - Finalización de MPI: Al final de tu programa, debes finalizar el entorno MPI utilizando la función `MPI_Finalize`. Esto asegura que se liberen los recursos correctamente y se cierre el entorno MPI adecuadamente.

### Compilación y ejecución de programas MPI en C.

Compilación: Para compilar un programa MPI en C, debes asegurarte de tener un compilador de C instalado en tu sistema (como GCC) y una implementación de MPI (como MPICH o OpenMPI). Utiliza el comando mpicc proporcionado por tu implementación de MPI para compilar tu programa MPI. Por ejemplo:

``` zsh
mpicc mi_programa.c -o mi_programa
```
{: .nolineno}
Esto compilará mi_programa.c en un ejecutable llamado mi_programa.

Ejecución: Para ejecutar un programa MPI en C, utiliza el comando mpiexec o mpirun. Por ejemplo:

``` zsh
mpiexec -n <numero_de_procesos> ./mi_programa
```
{: .nolineno}

Reemplaza `<numero_de_procesos>` por la cantidad deseada de procesos que deseas utilizar en paralelo. Esto ejecutará tu programa MPI en C con el número especificado de procesos.

### Ejemplos prácticos.

Los ejemplos prácticos en C con MPI pueden incluir:

  - Multiplicación de Matrices: Dividir una matriz en bloques y realizar la multiplicación de matrices en paralelo utilizando MPI.

  - Resolución de Ecuaciones Diferenciales Parciales (EDP): Dividir el dominio de una EDP en subdominios y resolverlos en paralelo con MPI.

  - Procesamiento de Imágenes y Videos: Procesar imágenes o videos en paralelo utilizando múltiples procesos para - acelerar el procesamiento.

  - Simulaciones Numéricas: Realizar simulaciones numéricas en paralelo para resolver problemas científicos o de ingeniería.

Estos ejemplos muestran cómo MPI en C puede ser utilizado para aprovechar el poder del cómputo paralelo y resolver una variedad de problemas en el campo de la computación científica y la ingeniería.


## Tema 6: Comunicación Punto a Punto Avanzada

### Uso de comunicadores intercomunicadores.

Los comunicadores intercomunicadores son una característica avanzada de MPI que permiten la comunicación entre dos grupos de procesos, donde un grupo actúa como el grupo local y el otro como el grupo remoto. Esto es útil en situaciones donde necesitas coordinar la ejecución de procesos en diferentes grupos, como en aplicaciones cliente-servidor o en tareas que involucran múltiples procesadores o nodos de un clúster.

Creación de un Comunicador Intercomunicador
Para crear un comunicador intercomunicador en MPI, generalmente se utilizan las funciones `MPI_Comm_split` y `MPI_Intercomm_create`. A continuación, te proporciono un ejemplo simple en C y Python para ilustrar cómo crear y utilizar un comunicador intercomunicador:

#### Ejemplo en C:

```c
#include <stdio.h>
#include <mpi.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int local_rank;
    MPI_Comm local_comm; // Comunicador local para el grupo local

    // Dividir el grupo en dos: local y remoto
    int color = (rank < size / 2) ? 0 : 1;
    MPI_Comm_split(MPI_COMM_WORLD, color, rank, &local_comm);

    MPI_Comm_rank(local_comm, &local_rank);

    if (color == 0) {
        printf("Soy el proceso %d en el grupo local con rango %d.\n", rank, local_rank);
    } else {
        printf("Soy el proceso %d en el grupo remoto con rango %d.\n", rank, local_rank);
    }

    MPI_Comm_free(&local_comm);
    MPI_Finalize();

    return 0;
}
```
{: .nolineno}

#### Ejemplo en Python (con mpi4py):

```python
from mpi4py import MPI

comm = MPI.COMM_WORLD

rank = comm.Get_rank()
size = comm.Get_size()

local_rank = None
local_comm = None

# Dividir el grupo en dos: local y remoto
color = 0 if rank < size // 2 else 1
local_comm = comm.Split(color)

if color == 0:
    local_rank = local_comm.Get_rank()
    print(f"Soy el proceso {rank} en el grupo local con rango {local_rank}.")
else:
    local_rank = local_comm.Get_rank()
    print(f"Soy el proceso {rank} en el grupo remoto con rango {local_rank}.")

local_comm.Free()
```
{: .nolineno}

En estos ejemplos, dividimos el grupo de procesos en dos subgrupos: uno llamado "local" y otro llamado "remoto", según una condición simple. Luego, cada proceso imprime su rango en su respectivo grupo. Ten en cuenta que en aplicaciones reales, los grupos local y remoto podrían realizar tareas específicas y luego comunicarse entre sí utilizando el comunicador intercomunicador.

Esta es una introducción básica al uso de comunicadores intercomunicadores en MPI. Puedes adaptar estos ejemplos para situaciones más complejas donde necesites coordinar procesos en diferentes grupos de manera efectiva.


### Comunicación remota en MPI.

La comunicación remota en MPI implica la transferencia de datos entre procesos en diferentes nodos o máquinas en un sistema distribuido o clúster de computadoras. MPI proporciona mecanismos para permitir que los procesos se comuniquen entre sí de manera efectiva incluso cuando están ejecutándose en diferentes nodos.

#### Ejemplo en C
```c
#include <stdio.h>
#include <mpi.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    if (rank == 0) {
        int data_to_send = 42;
        MPI_Send(&data_to_send, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
        printf("Proceso 0 ha enviado el dato %d al proceso 1.\n", data_to_send);
    } else if (rank == 1) {
        int data_received;
        MPI_Recv(&data_received, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Proceso 1 ha recibido el dato %d del proceso 0.\n", data_received);
    }

    MPI_Finalize();

    return 0;
}
```
{: .nolineno}
#### Ejemplo en Python (con mpi4py)

```python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

if rank == 0:
    data_to_send = 42
    comm.send(data_to_send, dest=1)
    print(f"Proceso 0 ha enviado el dato {data_to_send} al proceso 1.")
elif rank == 1:
    data_received = comm.recv(source=0)
    print(f"Proceso 1 ha recibido el dato {data_received} del proceso 0.")
```
{: .nolineno}
En estos ejemplos, tenemos dos procesos, uno con rango 0 y otro con rango 1. El proceso 0 envía un valor (42) al proceso 1 utilizando `MPI_Send` en C y `comm.send` en Python, y el proceso 1 recibe el valor utilizando `MPI_Recv` en C y `comm.recv` en Python.


> Ten en cuenta que la comunicación remota en MPI puede ser más compleja y versátil que este ejemplo simple. Puedes utilizar varios tipos de datos, etiquetas y comunicadores para adaptar la comunicación a tus necesidades específicas. Además, MPI proporciona otras funciones avanzadas de comunicación, como comunicaciones no bloqueantes y comunicaciones colectivas, que pueden ser útiles en aplicaciones más complejas.
{: .prompt-info }

### Ejemplos de comunicación punto a punto avanzada.

En este ejemplo, dos procesos intercambiarán datos utilizando `MPI_Sendrecv`. El proceso con rango 0 enviará un número al proceso con rango 1 y, a su vez, recibirá un número del proceso con rango 1. Esto se hace de manera simultánea y eficiente utilizando `MPI_Sendrecv`.

Ejemplo en C

```c
#include <stdio.h>
#include <mpi.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    int send_data, recv_data;

    if (rank == 0) {
        send_data = 42;
        printf("Proceso 0 envía %d a Proceso 1 y recibe.\n", send_data);

        MPI_Sendrecv(&send_data, 1, MPI_INT, 1, 0, &recv_data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);

        printf("Proceso 0 recibió %d de Proceso 1.\n", recv_data);
    } else if (rank == 1) {
        send_data = 99;
        printf("Proceso 1 envía %d a Proceso 0 y recibe.\n", send_data);

        MPI_Sendrecv(&send_data, 1, MPI_INT, 0, 0, &recv_data, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);

        printf("Proceso 1 recibió %d de Proceso 0.\n", recv_data);
    }

    MPI_Finalize();

    return 0;
}
```
{: .nolineno}

Ejemplo en Python (con mpi4py)

```python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

send_data = None
recv_data = None

if rank == 0:
    send_data = 42
    print(f"Proceso 0 envía {send_data} a Proceso 1 y recibe.")

    recv_data = comm.sendrecv(send_data, dest=1, source=1)

    print(f"Proceso 0 recibió {recv_data} de Proceso 1.")
elif rank == 1:
    send_data = 99
    print(f"Proceso 1 envía {send_data} a Proceso 0 y recibe.")

    recv_data = comm.sendrecv(send_data, dest=0, source=0)

    print(f"Proceso 1 recibió {recv_data} de Proceso 0.")
```
{: .nolineno}

En ambos ejemplos, los procesos 0 y 1 realizan una comunicación bidireccional utilizando `MPI_Sendrecv` en C y `comm.sendrecv` en Python. Esto permite enviar datos al otro proceso y recibir datos de él en una sola llamada, lo que simplifica el código y mejora la eficiencia en muchas situaciones.

> Ten en cuenta que la comunicación punto a punto avanzada en MPI ofrece muchas más funcionalidades, como comunicaciones no bloqueantes y múltiples comunicaciones simultáneas. Estos son ejemplos simples, pero MPI proporciona la flexibilidad necesaria para manejar situaciones más complejas en aplicaciones de cómputo paralelo.
{: .prompt-info }

## Tema 7: Programación Paralela y Tareas Distribuidas

### División de tareas en paralelo.

  - Identificación de la Tarea Principal: En un programa MPI, generalmente hay un proceso (por ejemplo, el proceso con rango 0) que se considera el proceso principal o maestro. Este proceso es responsable de dividir la tarea en partes más pequeñas y distribuirlas entre los procesos secundarios (trabajadores).

  - División de la Tarea: El proceso principal divide la tarea en partes más pequeñas y define qué parte se asignará a cada proceso secundario. Esto implica determinar qué datos o cálculos serán manejados por cada proceso.

  - Comunicación y Sincronización: Una vez que se han asignado las partes de la tarea, los procesos secundarios comienzan a trabajar de manera independiente en sus respectivas partes. Durante la ejecución, es posible que necesiten comunicarse entre sí para compartir resultados parciales o sincronizar sus acciones.

  - Recolección de Resultados: Después de que los procesos secundarios hayan completado sus tareas, el proceso principal puede recopilar los resultados parciales o combinados de los procesos secundarios para obtener el resultado final.

Supongamos que deseas calcular la suma de los primeros N números naturales de manera paralela utilizando MPI. La tarea principal podría dividir los números en partes iguales y asignar cada parte a un proceso secundario. Cada proceso secundario calcularía la suma de su parte de los números y luego el proceso principal sumaría todas las sumas parciales para obtener la suma total.

#### Ejemplo en Python (Distribución de Tareas en MPI con mpi4py)

```python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()
size = comm.Get_size()

N = 100
total = 0

# Tarea principal divide la tarea y asigna partes a procesos secundarios
if rank == 0:
    chunk_size = N // size
    for i in range(1, size):
        start = (i - 1) * chunk_size + 1
        end = i * chunk_size
        comm.send((start, end), dest=i)
    
    start = (size - 1) * chunk_size + 1
    end = N
else:
    start, end = comm.recv(source=0)

# Cada proceso secundario calcula su suma parcial
local_sum = sum(range(start, end + 1))

# Tarea principal recopila sumas parciales
if rank == 0:
    for i in range(1, size):
        local_sum += comm.recv(source=i)
    total = local_sum
    print("La suma total es:", total)
else:
    comm.send(local_sum, dest=0)
```
{: .nolineno}

#### Ejemplo en C (Distribución de Tareas en MPI)

```c
#include <stdio.h>
#include <mpi.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int N = 100;
    int total = 0;
    int local_sum = 0;

    if (rank == 0) {
        int chunk_size = N / size;
        for (int i = 1; i < size; i++) {
            int start = (i - 1) * chunk_size + 1;
            int end = i * chunk_size;
            MPI_Send(&start, 1, MPI_INT, i, 0, MPI_COMM_WORLD);
            MPI_Send(&end, 1, MPI_INT, i, 0, MPI_COMM_WORLD);
        }

        int start = (size - 1) * chunk_size + 1;
        int end = N;

        for (int i = start; i <= end; i++) {
            total += i;
        }

        for (int i = 1; i < size; i++) {
            MPI_Recv(&local_sum, 1, MPI_INT, i, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
            total += local_sum;
        }

        printf("La suma total es: %d\n", total);
    } else {
        int start, end;
        MPI_Recv(&start, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        MPI_Recv(&end, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);

        for (int i = start; i <= end; i++) {
            local_sum += i;
        }

        MPI_Send(&local_sum, 1, MPI_INT, 0, 0, MPI_COMM_WORLD);
    }

    MPI_Finalize();

    return 0;
}
```
{: .nolineno}


En este ejemplo, la tarea principal divide la tarea de calcular la suma de los números naturales en partes iguales y las asigna a los procesos secundarios utilizando `MPI_Send` y `MPI_Recv`. Cada proceso secundario calcula su suma parcial y la envía de vuelta al proceso principal, que recopila todas las sumas parciales para obtener el resultado total.


> La división de tareas en paralelo es esencial para aprovechar la capacidad de procesamiento de sistemas paralelos o distribuidos y es una técnica fundamental en la programación MPI. En aplicaciones reales, la división de tareas puede ser más compleja y puede requerir una planificación cuidadosa para garantizar que los procesos se comuniquen y sincronicen correctamente.
{: .prompt-info }


### Sincronización y barreras en MPI.

La sincronización se refiere al proceso de asegurarse de que los procesos en una aplicación paralela se ejecuten en un orden específico o que se completen ciertas tareas antes de continuar. En MPI, puedes utilizar varias funciones para lograr la sincronización, entre ellas:

  - MPI_Barrier: La función MPI_Barrier se utiliza para crear una barrera que detiene la ejecución de todos los procesos hasta que todos los procesos hayan llegado a la misma barrera. Esto asegura que todos los procesos estén sincronizados antes de continuar.

  - Sincronización Implícita: MPI garantiza cierta sincronización implícita en las operaciones de comunicación. Por ejemplo, después de una llamada a MPI_Send y antes de una llamada a `MPI_Recv` correspondiente, se establece una sincronización implícita entre los procesos.

#### Barreras en MPI
Una barrera en MPI es un punto de sincronización donde todos los procesos involucrados deben llegar antes de que cualquier proceso pueda continuar más allá de la barrera. Esto es útil en situaciones donde necesitas asegurarte de que todos los procesos hayan alcanzado un cierto punto antes de proceder. La función MPI_Barrier se utiliza para crear una barrera en MPI.

Ejemplo en C (Uso de MPI_Barrier)
```c
#include <stdio.h>
#include <mpi.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    printf("Proceso %d: Antes de la barrera.\n", rank);

    // Sincronización de todos los procesos en la barrera
    MPI_Barrier(MPI_COMM_WORLD);

    printf("Proceso %d: Después de la barrera.\n", rank);

    MPI_Finalize();

    return 0;
}
```
{: .nolineno}
En este ejemplo en C, todos los procesos imprimen un mensaje antes y después de una llamada a MPI_Barrier. Los procesos esperarán en la barrera hasta que todos los procesos hayan llegado, y luego continuarán ejecutándose después de la barrera.

Ejemplo en Python (Uso de MPI_Barrier con mpi4py)
```python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

print(f"Proceso {rank}: Antes de la barrera.")

# Sincronización de todos los procesos en la barrera
comm.Barrier()

print(f"Proceso {rank}: Después de la barrera.")
```
{: .nolineno}
Este ejemplo en Python utiliza `comm.Barrier()` para lograr la misma sincronización que en el ejemplo en C. Todos los procesos esperarán en la barrera hasta que todos los procesos hayan llegado, y luego continuarán ejecutándose después de la barrera.

La sincronización y las barreras son herramientas importantes en MPI para garantizar que los procesos paralelos trabajen juntos de manera ordenada y cooperativa, lo que es fundamental para muchas aplicaciones de cómputo paralelo.

### Ejemplos de programación paralela.

En este caso, crearemos un ejemplo simple de programación paralela que calcule la suma de un arreglo de números en paralelo. La tarea se dividirá entre varios procesos, y luego se sumarán las partes parciales para obtener el resultado final.

Ejemplo de Programación Paralela en C
```c
#include <stdio.h>
#include <mpi.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int N = 100;  // Tamaño del arreglo
    int local_sum = 0;  // Suma parcial local
    int* data = NULL;

    // El proceso 0 crea y distribuye los datos
    if (rank == 0) {
        data = (int*)malloc(N * sizeof(int));
        for (int i = 0; i < N; i++) {
            data[i] = i + 1;  // Inicializar el arreglo con números naturales
        }
    }

    // Distribuir el arreglo entre todos los procesos
    int local_size = N / size;
    int* local_data = (int*)malloc(local_size * sizeof(int));
    MPI_Scatter(data, local_size, MPI_INT, local_data, local_size, MPI_INT, 0, MPI_COMM_WORLD);

    // Cada proceso calcula la suma parcial local
    for (int i = 0; i < local_size; i++) {
        local_sum += local_data[i];
    }

    // Sumar las sumas parciales locales para obtener el resultado final
    int total_sum = 0;
    MPI_Reduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    // El proceso 0 imprime el resultado final
    if (rank == 0) {
        printf("La suma total es: %d\n", total_sum);
        free(data);
    }

    free(local_data);
    MPI_Finalize();

    return 0;
}
```
{: .nolineno}
En este ejemplo en C, dividimos la tarea de calcular la suma de un arreglo de números en partes iguales entre los procesos y utilizamos MPI_Scatter para distribuir los datos entre ellos. Luego, cada proceso calcula su suma parcial local y usamos MPI_Reduce para sumar las sumas parciales locales y obtener el resultado final.

Ejemplo de Programación Paralela en Python (con mpi4py)
```python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()
size = comm.Get_size()

N = 100  # Tamaño del arreglo
local_sum = 0  # Suma parcial local
data = None

# El proceso 0 crea y distribuye los datos
if rank == 0:
    data = list(range(1, N + 1))  # Inicializar el arreglo con números naturales

# Distribuir el arreglo entre todos los procesos
local_size = N // size
local_data = [0] * local_size
comm.Scatter(data, local_data, root=0)

# Cada proceso calcula la suma parcial local
for num in local_data:
    local_sum += num

# Sumar las sumas parciales locales para obtener el resultado final
total_sum = comm.reduce(local_sum, op=MPI.SUM, root=0)

# El proceso 0 imprime el resultado final
if rank == 0:
    print("La suma total es:", total_sum)

MPI.Finalize()
```
{: .nolineno}

En este ejemplo en Python, seguimos el mismo enfoque que en C. Dividimos la tarea, distribuimos los datos, calculamos sumas parciales locales y luego reducimos las sumas locales para obtener el resultado final.

Ambos ejemplos ilustran cómo se puede realizar programación paralela en MPI para acelerar la ejecución de tareas al dividirlas entre varios procesos y luego combinar los resultados parciales. La programación paralela es útil para aprovechar la capacidad de procesamiento de sistemas paralelos o distribuidos y acelerar el procesamiento de datos en aplicaciones de cómputo intensivo.

## Tema 8: Rendimiento y Optimización

### Estrategias para optimizar el rendimiento en MPI.

Optimizar el rendimiento en MPI es esencial para aprovechar al máximo el poder de cómputo de sistemas paralelos o distribuidos. Aquí te presento algunas estrategias clave para mejorar el rendimiento en MPI:

  - Minimizar la Comunicación: La comunicación entre procesos en MPI puede ser costosa en términos de tiempo de ejecución. Para optimizar el rendimiento, trata de minimizar la cantidad de datos que se envían y reciben entre procesos. Utiliza comunicaciones colectivas en lugar de comunicaciones punto a punto siempre que sea posible.

  - División de Tareas Equitativas: Al dividir una tarea en partes para distribuirla entre procesos, asegúrate de que las partes sean aproximadamente iguales en términos de carga computacional. De esta manera, los procesos finalizan su trabajo al mismo tiempo, evitando cuellos de botella.

  - Uso Eficiente de Memoria: Gestiona eficientemente la memoria para evitar agotar los recursos del sistema. Libera la memoria que ya no se necesita y utiliza estructuras de datos eficientes.

  - Balance de Carga Dinámico: En aplicaciones donde la carga de trabajo puede variar durante la ejecución, considera implementar un mecanismo de balance de carga dinámico para redistribuir tareas entre procesos según sea necesario.

  - Comunicaciones no Bloqueantes: Utiliza comunicaciones no bloqueantes (por ejemplo, MPI_Isend y MPI_Irecv en C) para permitir que los procesos realicen cómputo mientras esperan la finalización de las operaciones de comunicación.

  - Afinación de Comunicadores: MPI permite la creación de comunicadores personalizados que agrupan un subconjunto de procesos. Al utilizar comunicadores apropiados, puedes reducir la sobrecarga de comunicación y mejorar el rendimiento.

  - Evitar Overhead de Comunicación: Minimiza la sobrecarga de comunicación evitando llamadas innecesarias a funciones de MPI. Agrupa operaciones de comunicación siempre que sea posible.

  - Utilizar Implementaciones MPI Optimizadas: Asegúrate de utilizar una implementación de MPI optimizada para tu plataforma. Algunas implementaciones, como MPICH o Open MPI, ofrecen mejoras de rendimiento específicas para hardware y sistemas operativos.

  - Probar en Diferentes Tamaños de Problemas: Realiza pruebas de rendimiento en diferentes tamaños de problemas y números de procesos para identificar el punto óptimo en términos de eficiencia.

  - Profiling y Análisis de Rendimiento: Utiliza herramientas de perfilado y análisis de rendimiento, como Intel VTune, para identificar cuellos de botella y áreas de mejora en tu código MPI.

  - Parallel I/O Eficiente: Si tu aplicación implica lectura/escritura de archivos en paralelo, utiliza las bibliotecas y técnicas de E/S paralela adecuadas para evitar cuellos de botella en las operaciones de E/S.

  - Compilación con Optimizaciones: Al compilar tu código MPI, habilita las optimizaciones del compilador para aprovechar al máximo las capacidades de tu hardware.

  - Paralelización Híbrida: En algunas aplicaciones, es beneficioso combinar MPI con paralelización en hilos (por ejemplo, utilizando OpenMP) para aprovechar al máximo los recursos de CPU y memoria.

Recuerda que la optimización en MPI puede ser un proceso iterativo y depende en gran medida de la naturaleza específica de tu aplicación y del hardware en el que se ejecute. Es importante medir y analizar el rendimiento de tu código para identificar áreas de mejora y aplicar las estrategias adecuadas para optimizarlo.

### Herramientas de análisis de rendimiento.


En MPI, existen diversas herramientas de análisis de rendimiento que pueden ayudarte a identificar cuellos de botella, problemas de rendimiento y áreas de mejora en tus aplicaciones paralelas. Estas herramientas son esenciales para optimizar tus programas MPI. A continuación, te presento algunas de las herramientas más utilizadas:

  - MPI Profilers:
    - mpip: Esta es una herramienta que proporciona información detallada sobre el uso de MPI en tu programa. Puede ayudarte a identificar llamadas MPI costosas en tiempo y recursos.
  
  - MPI Tracers:
    - mpitrace: Un trazador MPI que registra y muestra llamadas MPI, tiempos de comunicación y más información para ayudar en el análisis de rendimiento.
  
  - Generación de Perfiles:
    - gprof: Una herramienta de generación de perfiles de código que puede ayudarte a identificar las funciones más consumidoras de tiempo en tu programa MPI.
    - valgrind: Es una suite de herramientas que incluye Memcheck, un detector de errores de memoria, y Callgrind, un generador de perfiles de código.
  
  - Herramientas de Visualización:
    - Vampir: Una herramienta de visualización de rendimiento que permite analizar el comportamiento de aplicaciones MPI. Proporciona gráficos y análisis detallados.
    - Paraver: Otra herramienta de visualización de rendimiento que se utiliza comúnmente en el contexto de MPI.
  
  - Herramientas de Análisis de Comunicación:
    - Intel Trace Analyzer and Collector (ITAC): Una herramienta de Intel que puede rastrear, analizar y visualizar la comunicación MPI en tu programa.
    - Score-P: Una herramienta de análisis de rendimiento que se enfoca en la medición de eventos de comunicación MPI y generación de perfiles.
  
  - Herramientas de Monitoreo de Hardware:
    - Intel VTune Profiler: Esta herramienta permite analizar el rendimiento y el comportamiento del hardware en sistemas basados en Intel.
    - AMD CodeXL: Una herramienta similar a VTune que se utiliza para analizar el rendimiento en sistemas basados en AMD.
  
  - Herramientas de Monitoreo de Clúster:
    - Ganglia: Una herramienta de monitoreo y análisis de clúster que proporciona información sobre el rendimiento del clúster y sus nodos.
    - Nagios: Otra herramienta de monitoreo de clúster que puede ayudarte a detectar problemas de rendimiento y salud del clúster.
  
  - Herramientas de Generación de Informes:
    - hpctoolkit: Una suite de herramientas que incluye hpcviewer y hpctrace para el análisis de rendimiento y la generación de informes.
    - TAU (Tuning and Analysis Utilities): Proporciona una variedad de herramientas para la generación de informes de rendimiento.
  
  - Herramientas de Perfilado y Depuración Integradas:
    - Algunos entornos de desarrollo integrado (IDE), como Eclipse con el complemento Parallel Tools Platform (PTP) o Visual Studio con la extensión para MPI, proporcionan herramientas integradas para el perfilado y la depuración de aplicaciones MPI.

  - Herramientas de Benchmarking:
    - Las herramientas de benchmarking, como HPL (High-Performance Linpack) y HPCG (High-Performance Conjugate Gradient), pueden ayudarte a evaluar el rendimiento de tu clúster MPI en comparación con otros sistemas.


Recuerda que la elección de una herramienta de análisis de rendimiento dependerá de tus necesidades específicas y de la plataforma en la que estés trabajando. Es común utilizar varias herramientas en combinación para obtener una visión completa del rendimiento de tus aplicaciones MPI y encontrar oportunidades de optimización.

### Ejemplos de optimización.

Versión Inicial en C:

```c
#include <stdio.h>
#include <stdlib.h>
#include <mpi.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int n = 1000000;  // Tamaño de los vectores
    int* vectorA = (int*)malloc(n * sizeof(int));
    int* vectorB = (int*)malloc(n * sizeof(int));

    // Inicializar los vectores (código no mostrado)

    int local_sum = 0;
    for (int i = rank; i < n; i += size) {
        local_sum += vectorA[i] * vectorB[i];
    }

    int total_sum;
    MPI_Reduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Producto escalar: %d\n", total_sum);
    }

    free(vectorA);
    free(vectorB);
    MPI_Finalize();

    return 0;
}
```
{: .nolineno}

En esta versión inicial en C, calculamos el producto escalar de dos vectores utilizando MPI. Sin embargo, la reducción final de la suma se realiza con MPI_Reduce, lo que significa que solo el proceso 0 obtiene el resultado final y los demás procesos no contribuyen a la suma parcial.

Versión Optimizada en C:

```c
#include <stdio.h>
#include <stdlib.h>
#include <mpi.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int n = 1000000;  // Tamaño de los vectores
    int* vectorA = (int*)malloc(n * sizeof(int));
    int* vectorB = (int*)malloc(n * sizeof(int));

    // Inicializar los vectores (código no mostrado)

    int local_sum = 0;
    for (int i = rank; i < n; i += size) {
        local_sum += vectorA[i] * vectorB[i];
    }

    int total_sum;
    MPI_Allreduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Producto escalar: %d\n", total_sum);
    }

    free(vectorA);
    free(vectorB);
    MPI_Finalize();

    return 0;
}
```
{: .nolineno}

En la versión optimizada en C, hemos reemplazado MPI_Reduce con MPI_Allreduce. Esto permite que todos los procesos realicen una reducción y reciban el resultado total de manera eficiente. Todos los procesos contribuyen a la suma parcial y obtienen el resultado final, lo que reduce la sobrecarga de comunicación y mejora el rendimiento.

Ejemplo de Optimización en Python con mpi4py
A continuación, se muestra una versión similar del ejemplo de optimización en Python utilizando mpi4py:

```python
from mpi4py import MPI
import numpy as np

comm = MPI.COMM_WORLD
rank = comm.Get_rank()
size = comm.Get_size()

n = 1000000  # Tamaño de los vectores
vectorA = np.empty(n, dtype=np.int)
vectorB = np.empty(n, dtype=np.int)

# Inicializar los vectores (código no mostrado)

local_sum = 0
for i in range(rank, n, size):
    local_sum += vectorA[i] * vectorB[i]

total_sum = comm.allreduce(local_sum, op=MPI.SUM)

if rank == 0:
    print("Producto escalar:", total_sum)
```
{: .nolineno}

Al igual que en el ejemplo en C, esta versión optimizada en Python utiliza comm.allreduce para realizar una reducción y distribuir el resultado total de manera eficiente entre todos los procesos. Todos los procesos contribuyen a la suma parcial y obtienen el resultado final, lo que reduce la sobrecarga de comunicación y mejora el rendimiento.

Estos ejemplos ilustran cómo se pueden aplicar estrategias de optimización comunes en MPI para mejorar el rendimiento de tus aplicaciones paralelas.

## Tema 9: Aplicaciones Prácticas

### Casos de estudio y aplicaciones del mundo real de MPI.

Uno de los ejemplos destacados es el modelo meteorológico MC2 [Canadian Mesoscale Compressible Community Model](https://collaboration.cmc.ec.gc.ca/science/rpn/map/canada_in_map/subjects/modelling/description.html). Este modelo se ejecutó en un entorno heterogéneo de recursos de computación con dedicación parcial y/o total, utilizando MPI para paralelizar el código y mejorar su rendimiento. Se experimentó con diferentes descomposiciones de dominios para varias cantidades de CPUs. El rendimiento de la red de datos tuvo un impacto significativo en los resultados obtenidos con el modelo MC2, destacando la importancia de la comunicación en aplicaciones MPI.

Además, se realizó un estudio utilizando una infraestructura mixta de recursos computacionales, incluyendo clusters dedicados y laboratorios de enseñanza, para ejecutar este modelo meteorológico. Los resultados mostraron diferencias en los tiempos de CPU en función de la red utilizada y del tipo de recursos (dedicados y no dedicados).

Otra aplicación interesante fue el uso de MPI en un entorno Condor para obtener patrones de tráfico de red, donde se observó una reducción en el tiempo total de ejecución. También se menciona el uso de MPI en estudios paramétricos de mecánica de sólidos y en el procesamiento de aplicaciones MPI sobre recursos dedicados, parcialmente dedicados y en infraestructuras mixtas.

### Desarrollo de aplicaciones distribuidas.

  - Simulación de choques (Crash Simulation): Esta es una aplicación de memoria distribuida compleja que fue paralelizada para Ford Motor. Se utiliza en la simulación de choques vehiculares para mejorar la seguridad en la ingeniería automotriz.

  - Modelado Climático: Utilizado por organizaciones como NCAR, NASA y ECMWF, este modelo destaca la computación distribuida, compartida y vectorial. Es crucial en el estudio y la predicción del clima.

  - Predicción del clima espacial (Space Weather Prediction): Se trata de un código de malla adaptativa que se escala a más de 1000 procesadores. Es una aplicación financiada por NASA/DoD/NSF.

  - Diseño de ensayos clínicos éticos (Design of Ethical Clinical Trials): Es una aplicación intensiva en memoria financiada por la NSF. Se utiliza en el desarrollo de protocolos para ensayos clínicos que cumplen con estándares éticos.

  - Monte Carlo Simulations: Aunque no tengo un ejemplo específico, las simulaciones de Monte Carlo son un tipo de aplicación distribuida que utiliza MPI para realizar cálculos complejos en muchos campos, como la física y las finanzas.

> Cabe mencionar que estos ejemplos provienen de un tutorial de computación paralela de la Universidad de Michigan y de información general sobre MPI. La búsqueda se vio limitada por restricciones de acceso a algunos recursos en línea, lo que impidió obtener más ejemplos detallados y específicos.
{: .prompt-info }
