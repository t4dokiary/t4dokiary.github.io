---
title: MPI en Python y C 
author: t4dokiary
date: 2023-12-21 23:15:00 +0800
categories: [Apuntes, Sistemas Operativos 2, Programación Paralela, MPI, Python, C, Clase]
tags: [c, python, mpi]
render_with_liquid: false
pin: true
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
- Inicialización de MPI en Python y C.
- Finalización de MPI y liberación de recursos.

## Tema 4: Desarrollo de Programas MPI en Python
- Creación de un programa MPI simple en Python.
- Compilación y ejecución de programas MPI en Python.
- Ejemplos prácticos.

## Tema 5: Desarrollo de Programas MPI en C
- Creación de un programa MPI simple en C.
- Compilación y ejecución de programas MPI en C.
- Ejemplos prácticos.

## Tema 6: Comunicación Punto a Punto Avanzada
- Uso de comunicadores intercomunicadores.
- Comunicación remota en MPI.
- Ejemplos de comunicación punto a punto avanzada.

## Tema 7: Programación Paralela y Tareas Distribuidas
- División de tareas en paralelo.
- Sincronización y barreras en MPI.
- Ejemplos de programación paralela.

## Tema 8: Rendimiento y Optimización
- Estrategias para optimizar el rendimiento en MPI.
- Herramientas de análisis de rendimiento.
- Ejemplos de optimización.

## Tema 9: Aplicaciones Prácticas
- Casos de estudio y aplicaciones del mundo real de MPI.
- Desarrollo de aplicaciones distribuidas.

## Tema 10: MPI en Clústeres y Supercomputadoras
- Configuración y ejecución de programas MPI en clústeres.
- Ejemplos de uso en supercomputadoras.

## Tema 11: MPI y Bibliotecas Externas
- Integración de MPI con otras bibliotecas paralelas.
- Ejemplos de uso con bibliotecas como OpenMP y CUDA (si es aplicable).

## Tema 12: MPI en la Nube
- Uso de MPI en entornos de computación en la nube.
- Ejemplos de aplicaciones en la nube.
