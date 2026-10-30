# Guion del orador — NumPy y pandas

Oct 29, 2026 · Santiago

## Cómo usar este guion

La clase completa son unos 75 minutos para un grupo con algo de base y unas tres sesiones de 60 a 70 minutos para principiantes completos (ver la sección siguiente) y alterna entre dos pantallas: la presentación (25 diapositivas) para ver la idea y el notebook `clase-numpy-pandas/intro_pandas_numpy.ipynb` para ejecutarla. Cada bloque de diálogo dice qué decir, y las señales indican cuándo cambiar de pantalla.

- **\[→ NOTEBOOK\]**: deja la presentación y ve a la sección del notebook indicada; ejecuta la celda con `Shift + Enter`.
- **\[← DIAPOSITIVAS\]**: vuelve a la presentación y avanza a la diapositiva indicada.
- **Pregunta**: detente y espera respuesta del grupo antes de continuar.
- El texto en cursiva es lo que se dice en voz alta; lo demás son instrucciones para el orador.

**Antes de empezar (10 minutos)**

1. Abre el notebook en Jupyter y ejecuta *Kernel → Restart & Run All* para comprobar que todo corre; luego *Clear All Outputs* si quieres que la audiencia vea cada resultado aparecer.
2. Ten ambas pantallas listas: presentación en una ventana, notebook en otra (o dos monitores).
3. Ejecuta de antemano la celda de importaciones de la sección 0 (`import numpy as np`, `import pandas as pd`).
4. No ejecutes las celdas de «Solución» de los ejercicios hasta el momento indicado.

## Para principiantes completos

Con un grupo que nunca ha programado, la clase funciona mejor en tres sesiones de 60 a 70 minutos que en una de 75, y con un calentamiento de Python al inicio. Los minutos del mapa de transiciones valen para un grupo con alguna base; aquí va cómo adaptar el guion.

### Tres sesiones en vez de una

| Sesión | Contenido | Diapositivas | Ejercicios |
| --- | --- | --- | --- |
| 1 · NumPy | Calentamiento de Python (10 min) y NumPy | 1 a 11 | 1 |
| 2 · pandas y limpieza | Tablas, selección, filtros, orden y datos sucios | 12 a 17 | 2 y 3 |
| 3 · Agrupar y unir | groupby, pivot, merge, proyecto y cierre | 18 a 25 | 4 |

Al empezar las sesiones 2 y 3, dedica 5 minutos a repasar con preguntas lo anterior (por ejemplo: «¿qué es un arreglo?», «¿qué hace una máscara?») y ejecuta *Restart & Run All*.

### Calentamiento de Python (10 minutos, con las diapositivas 4 y 5)

Abre una celda nueva al final del notebook y ve escribiendo en vivo. *«No hace falta saber programar. Solo necesitan tres ideas: guardar un dato con un nombre, tener una lista de datos y repetir una acción.»*

```python
x = 5
print(x)
notas = [3.5, 4.0, 2.8]
print(len(notas))
for n in notas:
    print(n * 2)
```

Resultado: `5`, luego `3`, y después `7.0`, `8.0` y `5.6`.

- *«x = 5 guarda el número 5 en una caja con la etiqueta x. El signo igual significa «guárdalo», no «es igual a».»*
- *«Una lista son varios datos entre corchetes. len cuenta cuántos hay.»*
- *«El for repite una acción con cada elemento, uno por uno. Dentro de un rato verán cómo NumPy hace esto sin repetir nada a mano.»*
- *«Para ejecutar una celda: Shift y Enter. Las celdas se ejecutan en orden; si salto una, la siguiente puede fallar.»*
- *«import numpy as np significa: traigo la caja de herramientas NumPy y la llamo np para escribir menos.»*

### Cómo leer una línea de código en voz alta

Lee cada línea como una frase, de dentro hacia fuera, antes de ejecutarla.

| Código | Lectura en voz alta |
| --- | --- |
| `a = np.array([10, 20, 30])` | Creo una variable llamada a y guardo en ella un arreglo de NumPy hecho con la lista 10, 20, 30 |
| `a[a > 25]` | De a, quédate solo con los valores mayores que 25 |
| `ventas[ventas["precio_unitario"] > 50000]` | De la tabla ventas, quédate con las filas donde el precio unitario es mayor que 50.000 |
| `ventas.groupby("ciudad")["total"].sum()` | De la tabla ventas, agrúpa por ciudad, mira la columna total y súmala |

### Vocabulario sin jerga

Cada término tiene una versión corta para decir la primera vez que aparece.

| Término | Qué decir |
| --- | --- |
| variable | Una caja con una etiqueta donde guardo un dato |
| función o método | Una orden con nombre; lleva paréntesis, por ejemplo `print(x)` o `a.mean()` |
| argumento | Lo que le doy a la orden dentro del paréntesis |
| librería | Una caja de herramientas que se importa |
| arreglo (array) | Una fila de casillas del mismo tipo, cada una con su posición desde 0 |
| `shape` | Cuántas filas y columnas tiene: (2, 3) son 2 filas y 3 columnas |
| `dtype` | El tipo de dato de las casillas: enteros, decimales, texto |
| vectorizar | Dar una sola orden para todas las casillas, sin repetir a mano |
| máscara | Una plantilla de verdadero y falso que decide qué se queda |
| índice | El nombre de cada fila |
| Series y DataFrame | Una columna con nombre de fila; una tabla de varias columnas |
| `NaN` | «No hay dato»: una casilla vacía |
| `==` y `=` | `==` pregunta si dos cosas son iguales; `=` guarda un valor |
| hash o diccionario | Un directorio donde busco por nombre y encuentro al instante |

### Ritmo y comprobación

1. **Predecir antes de ejecutar:** *«¿Qué creen que va a salir?»* Espera tres respuestas y ejecuta. Equivocarse con una predicción es parte del aprendizaje.
2. **Una sola idea por diapositiva:** si el grupo se pierde, vuelve al dibujo antes de seguir con el código.
3. **Tranquiliza:** *«No necesitan memorizar la sintaxis. Hoy lo importante es entender la idea; el código se copia y se busca.»*
4. **Ejercicios en parejas:** quien no sepa por dónde empezar copia la celda de arriba y cambia un valor.
5. **Señales de que debes frenar:** silencio prolongado, preguntas sobre sintaxis básica o caras de duda. Pregunta: *«¿qué parte de esta línea no queda clara?»*
6. **Errores como oportunidad:** si una celda falla, léela con el grupo: el mensaje de error dice qué pasó en la última línea.

## Mapa de transiciones

Hay 20 pasos al notebook en 75 minutos de ritmo continuo, sin contar los 10 minutos del calentamiento (para principiantes completos, reparte la tabla en tres sesiones, como indica la sección siguiente); el patrón casi siempre es el mismo: la diapositiva explica la idea, el notebook la ejecuta y se vuelve a la diapositiva para interpretar el resultado.

| Min | Diapositiva | Pasar al notebook (sección → celda) | Volver a la presentación en |
| --- | --- | --- | --- |
| 0 | 1 Portada, 2 Mapa, 3 Analogía | Sin notebook | Diapositiva 4 (o la 6 si el grupo ya programa) |
| 5 (+10) | 4 Python y 5 Cómo leer código (solo principiantes) | Python en 10 minutos → todas las celdas; luego «Cómo leer una línea de código» y el glosario | Diapositiva 6 |
| 5 | 6 Arreglo | 1.1 → celda `np.array`, `zeros`, `arange`, `linspace` | Diapositiva 7 |
| 9 | 7 Vectorización | 1.2 → celda de tiempos (1.000.000 de números) | Diapositiva 8 |
| 12 | 8 Máscara booleana | 1.3 → celda de indexación y máscaras | Diapositiva 9 |
| 15 | 9 Broadcasting | 1.4 → celda con `fila`, `col` y el error esperado | Diapositiva 10 |
| 19 | 10 axis | 1.5 → celda de estadística y «media a mano» | Diapositiva 11 |
| 22 | 11 z-score | 1.6 → celda `z` y `mm` | Diapositiva 12 tras el ejercicio |
| 26 | Ejercicio 1 (sin diapositiva) | Ejercicio 1 → celda vacía; luego la celda Solución | Diapositiva 12 |
| 29 | 12 DataFrame | Sección 2 → celda `pd.Series` y la celda que crea `ventas` | Diapositiva 13 |
| 32 | 13 Explorar | 2.1 → `shape`, `info`, `describe`, `value_counts` | Diapositiva 14 |
| 35 | 14 loc e iloc | 2.2 → celda de selección | Diapositiva 15 |
| 38 | 15 Filtros y columnas | 2.3 → celda de filtros; 2.4 → columnas nuevas | Diapositiva 16 |
| 42 | 16 Ordenar | 2.5 → las dos celdas de `sort_values` | Ejercicio 2 en el notebook |
| 44 | Ejercicio 2 | Ejercicio 2 → celda vacía y luego Solución | Diapositiva 17 |
| 47 | 17 Limpieza | 3 → `sucio`; 3.1 a 3.5 en orden hasta `limpiar(sucio)` | Ejercicio 3 en el notebook |
| 53 | Ejercicio 3 | Ejercicio 3 → celda Solución | Diapositiva 18 |
| 55 | 18 Split-apply-combine | 4 → celda del diccionario y la de `groupby` por ciudad | Diapositiva 19 |
| 59 | 19 Agregaciones y 20 Pivot | 4 → `agg`; 4.1 → `pivot_table` | Ejercicio 4 en el notebook |
| 62 | Ejercicio 4 | Ejercicio 4 → celda Solución | Diapositiva 21 |
| 65 | 21 Merge | 5.1 → `inner`, `left`, `outer` y el hash join a mano | Diapositiva 22 |
| 69 | 22 Proyecto | 6 → celda de validación y la de la gráfica | Diapositiva 23 |
| 72 | 23 Errores, 24 Resumen, 25 Cierre | Sin notebook; al final se menciona la sección 8 | Fin a los 75 min |

Regla práctica: no pases más de tres minutos seguidos en el notebook sin volver a la diapositiva; ahí está el dibujo que da sentido al resultado.

## Guion, parte 1: apertura y NumPy (minutos 0 a 29)

### Diapositivas 1 a 3 · Portada, mapa y analogía (0–5 min)

*«Buenos días. Hoy vamos a aprender a manejar datos como lo hace la gente que trabaja con ellos todos los días. ¿Quién ha usado Excel con muchas filas y se ha quedado esperando? Para eso existen NumPy y pandas.»*

*«Hoy hacemos cinco paradas: NumPy, pandas, limpieza, agrupar y unir, y un proyecto final. En cada una verán primero un dibujo, luego el código y por último el resultado real.»* (Diapositiva 2.)

*«Imaginen NumPy como una caja de huevos: compartimentos iguales y pegados, todos del mismo tipo. Por eso podemos tratar la caja entera de una vez. Y pandas es una hoja de cálculo con nombres en las columnas, construida encima de NumPy.»* (Diapositiva 3.)

**Pregunta:** ¿por qué una lista de Python no sirve para diez millones de números? (Respuesta guía: mezcla tipos y cada elemento vive en un sitio distinto de la memoria.)

### Diapositivas 4 y 5 · Python y cómo leer código (solo con principiantes; 10–12 min)

*«Antes de usar las herramientas, cuatro ideas de Python: guardar un dato con un nombre, tener una lista de datos, repetir una acción y comparar. Con eso alcanza para empezar.»* (Diapositiva 4.)

**\[→ NOTEBOOK\]** Sección «Python en 10 minutos». Ejecuta las celdas en orden: `print`, variables y tipos, operaciones y comparaciones, listas, `for`, diccionario, funciones, el error provocado y el Ejercicio 0. Antes de cada una pregunta «¿qué creen que va a salir?». *«El signo igual guarda; el doble igual pregunta. Y si algo falla, lean la última línea del error: dice qué pasó.»*

**\[← DIAPOSITIVAS\]** Diapositiva 5.

*«Así se lee una línea de código: como una frase, de dentro hacia fuera. Ventas, agrupa por ciudad, mira la columna total y súmala. No necesitan memorizar la sintaxis: hoy importa entender la idea.»* Pide a una persona que lea en voz alta la tercera línea de la diapositiva.

**\[→ NOTEBOOK\]** Baja a «Cómo leer una línea de código» y al glosario. *«Cuando aparezca una palabra nueva, vuelvan aquí.»*

**\[← DIAPOSITIVAS\]** Diapositiva 6.

### Diapositiva 6 · Un arreglo es una fila de casillas (5–9 min)

*«Un arreglo es una fila de casillas con número de posición; la primera es la 0. Todas guardan el mismo tipo de dato.»*

**\[→ NOTEBOOK\]** Sección 1.1. Ejecuta la celda de `np.array`. *«Aquí está el arreglo de 5 elementos: forma (5,), tipo int64. La matriz tiene forma (2, 3): dos filas y tres columnas.»* Señala `arange` y `linspace`.

**\[← DIAPOSITIVAS\]** Diapositiva 7.

### Diapositiva 7 · Vectorización (9–12 min)

*«Así se calcula el doble de cada número con un bucle: uno por uno. Y así con NumPy: una sola orden para todas las casillas.»*

**\[→ NOTEBOOK\]** Sección 1.2. Ejecuta la celda de tiempos. *«Con un millón de números, la lista tardó unos 0,19 segundos y NumPy unos 0,03: unas 6,5 veces más rápido en esta máquina. Y la última línea dice True: el resultado es idéntico.»* Si tus tiempos difieren, dilo: depende del equipo.

**\[← DIAPOSITIVAS\]** Diapositiva 8.

### Diapositiva 8 · Máscara booleana (12–15 min)

*«Filtrar es poner una plantilla encima. Primero comparo todo el arreglo con una condición y obtengo una fila de True y False; luego me quedo con lo que cae en True.»*

**Pregunta:** ¿qué valores quedan con `a > 25`? Espera respuestas.

**\[→ NOTEBOOK\]** Sección 1.3. Ejecuta la celda. *«Quedan 30, 40 y 50. Y para combinar condiciones uso &, | y \~, cada una entre paréntesis; no sirven `and` ni `or`.»*

**\[← DIAPOSITIVAS\]** Diapositiva 9.

### Diapositiva 9 · Broadcasting (15–19 min)

*«Si sumo una fila de tres números a una matriz de dos filas, NumPy fotocopia la fila hacia abajo, sin gastar memoria, hasta que encaje. La regla: se comparan las formas de derecha a izquierda y deben ser iguales o valer uno.»*

**\[→ NOTEBOOK\]** Sección 1.4. Ejecuta toda la celda, incluido el error. *«La forma (2,3) con (2,) falla porque 3 y 2 no coinciden: es el error que deben aprender a reconocer.»*

**\[← DIAPOSITIVAS\]** Diapositiva 10.

### Diapositiva 10 · axis (19–22 min)

*«Para recordar axis: es el eje que desaparece. Con axis=0 desaparecen las filas y queda un valor por columna; con axis=1 desaparecen las columnas y queda un valor por fila.»*

**\[→ NOTEBOOK\]** Sección 1.5. Ejecuta la celda. *«Media por examen: 3,75, 4,05 y 3,825. Media por alumno: el cuarto tiene 4,87 y su índice es 3.»* Señala la media y la desviación calculadas a mano: dan lo mismo que NumPy.

**\[← DIAPOSITIVAS\]** Diapositiva 11.

### Diapositiva 11 · z-score (22–26 min)

*«Un 4,0 no significa lo mismo en un examen fácil que en uno difícil. Restamos la media y dividimos por la desviación, y todo queda medido con la misma regla.»*

**\[→ NOTEBOOK\]** Sección 1.6. Ejecuta la celda. *«La media de cada columna es 0 y la desviación 1. El 2,0 del primer examen queda en menos 1,53: el más alejado por debajo.»* Menciona `min-max` (valores entre 0 y 1).

### Ejercicio 1 (26–29 min) · permanece en el notebook

*«Ahora les toca a ustedes. Tienen tres minutos para las tres preguntas del Ejercicio 1.»* Camina por el salón. Al terminar, ejecuta la celda Solución (promedio 11,85; quedan 12,5, 15,2 y 20,0).

**\[← DIAPOSITIVAS\]** Diapositiva 12.

## Guion, parte 2: pandas y limpieza (minutos 29 a 55)

### Diapositiva 12 · Del arreglo al DataFrame (29–32 min)

*«Una Series es una columna con nombre de fila. Un DataFrame es un montón de Series pegadas por el mismo índice. Y por dentro, cada Series es un arreglo de NumPy.»*

**\[→ NOTEBOOK\]** Sección 2. Ejecuta la celda de `pd.Series` y después la celda que crea `ventas`; las siguientes dependen de ella. *«La última línea imprime numpy.ndarray: lo que les prometí al inicio.»*

**\[← DIAPOSITIVAS\]** Diapositiva 13.

### Diapositiva 13 · Conoce tus datos (32–35 min)

*«Nunca analicen sin mirar. Cuatro herramientas: `head`, `shape`, `info` y `describe`.»*

**\[→ NOTEBOOK\]** Sección 2.1. Ejecuta `shape`, `info`, `describe` y `value_counts`. *«Son 40 ventas y 6 columnas, ningún dato nulo. Barranquilla tiene 14 ventas y Bogotá solo 5.»*

**Pregunta:** ¿qué ciudad tiene menos ventas? (Bogotá.) Guárdenlo para después.

**\[← DIAPOSITIVAS\]** Diapositiva 14.

### Diapositiva 14 · loc e iloc (35–38 min)

*«loc viene de Label, etiqueta: pido las filas por su nombre. iloc viene de Integer, posición: las pido por su puesto en la tabla. loc incluye el final del rango; iloc lo excluye.»*

**\[→ NOTEBOOK\]** Sección 2.2. Ejecuta la celda. *«loc de 0 a 2 devuelve tres filas; iloc de 0 a 2 devuelve dos.»*

**Pregunta rápida:** ¿cuántas filas devuelve `ventas.iloc[3:6]`? (Tres.)

**\[← DIAPOSITIVAS\]** Diapositiva 15.

### Diapositiva 15 · Filtros y columnas nuevas (38–42 min)

*«Es la misma plantilla de True y False de NumPy, pero con una entrada por fila.»*

**\[→ NOTEBOOK\]** Sección 2.3. Ejecuta la celda de filtros. *«Veinte de las cuarenta ventas son de productos de más de 50.000. Y la combinación Bogotá con Mochila devuelve vacío: no es un error, significa que no hubo coincidencias.»* Luego sección 2.4: ejecuta la celda que crea `total`, `tipo_venta` y `region`.

**\[← DIAPOSITIVAS\]** Diapositiva 16.

### Diapositiva 16 · Ordenar (42–44 min)

*«Las barras están ordenadas de mayor a menor total. Con dos claves, primero ciudad y dentro de cada ciudad el total.»*

**\[→ NOTEBOOK\]** Sección 2.5. Ejecuta las dos celdas de `sort_values`. *«La venta más grande es la 38, una calculadora de 765.000.»*

### Ejercicio 2 (44–47 min) · permanece en el notebook

*«Tres preguntas: cuántas calculadoras se vendieron en Cali, cuál es el total más alto y creen la columna de IVA.»* Resultado esperado: 5 ventas; 765.000 de una calculadora; primer IVA 1.140.

**\[← DIAPOSITIVAS\]** Diapositiva 17.

### Diapositiva 17 · Limpieza (47–53 min)

*«Los datos reales casi nunca llegan limpios. Miren la tabla de la izquierda y díganme qué está mal.»* Deja que el grupo encuentre: espacios sobrantes, mayúsculas mezcladas, un nombre faltante, dos ingresos vacíos, una edad escrita como texto y una fila repetida.

**\[→ NOTEBOOK\]** Sección 3. Ejecuta `sucio`; luego 3.1 a 3.5 en orden, comentando cada paso: faltantes con la mediana, duplicados, texto con `.str`, tipos con `to_numeric` y `to_datetime`. Termina ejecutando `limpiar(sucio)`. *«Así quedó: cuatro filas limpias, con el ingreso de Luis imputado en 2.950.000.»*

### Ejercicio 3 (53–55 min) · permanece en el notebook

*«Aún queda una edad en NaN. Renénenla con la mediana.»* Resultado esperado: 27,0.

**\[← DIAPOSITIVAS\]** Diapositiva 18.

## Guion, parte 3: agrupar, unir y cierre (minutos 55 a 75)

### Diapositiva 18 · Split-apply-combine (55–59 min)

*«Esta es la idea más importante de pandas. Tengo cuatro ventas de dos ciudades. Primero las reparto en cajones por ciudad; luego sumo dentro de cada cajón; al final junto los resultados.»*

**\[→ NOTEBOOK\]** Sección 4. Ejecuta primero la celda del diccionario. *«Así lo haría a mano: da Bogotá 150 y Cali 100. Y pandas lo hace en una línea con `groupby`, con el mismo resultado.»* Ejecuta la celda de `groupby` por ciudad con las 40 ventas: Barranquilla 3.101.000, Cali 1.874.300, Medellín 1.485.400, Bogotá 984.600.

**Pregunta:** ¿qué cambia si quiero el promedio en vez de la suma? (Solo el paso Apply.)

**\[← DIAPOSITIVAS\]** Diapositiva 19.

### Diapositivas 19 y 20 · Agregaciones y tabla dinámica (59–62 min)

*«Puedo pedir varias métricas a la vez. Cuaderno vende más en total, pero Lápiz tiene el ticket promedio más alto. Y si agrupo por dos claves, obtengo este mapa de calor.»*

**\[→ NOTEBOOK\]** Sección 4, celda `agg`; después 4.1, celda `pivot_table`. *«Total general: 7.445.300. Bogotá depende casi por completo de Lápiz; Cali no vendió nada de Lápiz.»* Opcional si hay tiempo: 4.2, `resample` (ventas por mes).

### Ejercicio 4 (62–65 min) · permanece en el notebook

*«Cantidad promedio por venta en cada región, y cuál región tiene el mayor promedio.»* Resultado esperado: Andina 4,88; Caribe 6,00; Pacífica 4,20; la mayor es Caribe.

**\[← DIAPOSITIVAS\]** Diapositiva 21.

### Diapositiva 21 · Merge (65–69 min)

*«Cuando los datos están en dos tablas, los uno por una llave. Aquí clientes y pedidos comparten `id_cliente`. El algoritmo: construyo un diccionario con la tabla de pedidos y para cada cliente busco su llave. Es casi instantáneo, en vez de comparar todos contra todos.»*

**\[→ NOTEBOOK\]** Sección 5.1. Ejecuta la celda de `merge` y muestra los tres resultados. *«Inner: 3 filas, Ana dos veces porque tiene dos pedidos y Marta. Left conserva a todos los clientes, y Luis y Pedro quedan con NaN: 5 filas. Outer añade el pedido 104, cuyo cliente no existe: 6 filas.»* Ejecuta también la celda del hash join a mano: da lo mismo que el inner. Menciona `concat` (5.2) en una frase: apila tablas, no busca llaves.

**\[← DIAPOSITIVAS\]** Diapositiva 22.

### Diapositiva 22 · Mini-proyecto (69–72 min)

*«Unamos todo en un flujo: cargar, validar, transformar, agrupar y comunicar. La pregunta: ¿qué ciudad y producto generan más ingresos?»*

**\[→ NOTEBOOK\]** Sección 6. Ejecuta la celda de validación y agrupación, y luego la de la gráfica. *«Los `assert` detendrían el programa si algo estuviera mal. El resultado: Barranquilla con Cuaderno, 1.490.000.»* Aclara que los datos son aleatorios; lo que importa es el método.

**\[← DIAPOSITIVAS\]** Diapositiva 23.

### Diapositivas 23 a 25 · Errores, resumen y cierre (72–75 min)

*«Cuatro tropiezos frecuentes: bucles sobre filas, `and` en vez de `&`, confundir loc con iloc y modificar un recorte sin `.copy()`.»* (Diapositiva 23.)

*«Los ocho algoritmos de hoy: vectorización, broadcasting, media y desviación, normalización, máscara booleana, ordenamiento, split-apply-combine y hash join.»* Pide que cada persona diga cuál le pareció más difícil. (Diapositiva 24.)

*«Si se llevan una sola idea: piensen en columnas, no en celdas. Todo lo de hoy está en el notebook, con la sección 8 como resumen. Gracias.»* (Diapositiva 25.) Abre el espacio de preguntas.

## Plan B: tiempo y fallos en vivo

Si vas justo de tiempo, recorta en este orden; cada corte libera los minutos indicados sin romper la secuencia de ideas.

| Si te faltan | Recorta | Ahorras |
| --- | --- | --- |
| 3 min | Diapositiva 16 (Ordenar) y su sección 2.5 | 2 min |
| 6 min | Diapositiva 20 (Pivot) y el Ejercicio 4: pídelo como tarea | 5 min |
| 10 min | Ejercicios 2 y 3 en clase: se dejan como tarea con su solución | 5 min |
| 15 min | Diapositiva 23 (Errores): se resume en una frase al cierre | 3 min |

Si vas adelantado, añade: `min-max` en la sección 1.6, `resample` en 4.2 y `concat` en 5.2.

**Si algo falla en vivo**

| Problema | Qué hacer |
| --- | --- |
| `NameError: name 'ventas' is not defined` | Falta ejecutar la celda que crea `ventas` (sección 2). Ejecútala y repite la celda que falló |
| Los números no coinciden con las diapositivas | Ejecuta *Restart & Run All*; los datos usan semilla fija (7), así que deben coincidir. Los tiempos de la sección 1.2 sí cambian en cada equipo |
| Kernel colgado o sin responder | *Kernel → Restart* y vuelve a ejecutar desde la sección 0 (menos de un minuto) |
| La gráfica de la sección 6 no aparece | Ejecuta `%matplotlib inline` en una celda y vuelve a correr la gráfica |
| No hay Jupyter disponible | Sigue solo con las diapositivas: cada una ya muestra el código y el resultado real, y el diálogo de arriba sirve igual sin los pasos al notebook |

Los datos de la clase son sintéticos; no hay información real de personas en ninguno de los ejemplos.
