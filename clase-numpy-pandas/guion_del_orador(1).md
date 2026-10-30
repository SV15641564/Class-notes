# Guion del orador — NumPy y pandas

Oct 29, 2026 · Santiago

*Versión con explicaciones conceptuales y de código ampliadas. Todas las cifras citadas se comprobaron ejecutando el notebook completo (NumPy 2.4, pandas 3.0).*

## Cómo usar este guion

La clase completa son unos 75 minutos para un grupo con algo de base y unas tres sesiones de 60 a 70 minutos para principiantes completos (ver «Para principiantes completos») y alterna entre dos pantallas: la presentación (25 diapositivas) para ver la idea y el notebook `clase-numpy-pandas/intro_pandas_numpy.ipynb` para ejecutarla. Cada bloque de diálogo dice qué decir, y las señales indican cuándo cambiar de pantalla.

- **\[→ NOTEBOOK\]**: deja la presentación y ve a la sección del notebook indicada; ejecuta la celda con `Shift + Enter`.
- **\[← DIAPOSITIVAS\]**: vuelve a la presentación y avanza a la diapositiva indicada.
- **Pregunta**: detente y espera respuesta del grupo antes de continuar. La respuesta esperada va entre paréntesis.
- El texto en cursiva es lo que se dice en voz alta; lo demás son instrucciones para el orador.

Cada bloque está construido en capas, para que puedas ajustar la profundidad según el grupo:

- **Idea clave**: lo que el grupo debe poder repetir al terminar el bloque. Se dice siempre.
- **Código, pieza por pieza**: cómo leer en voz alta las líneas del notebook. Con principiantes, hazlo siempre; con un grupo con base, limítate a las líneas que traen algo nuevo.
- **Malentendido típico**: el error de concepto más frecuente y cómo corregirlo antes de que se instale.
- **Si preguntan**: respuestas breves a dudas previsibles. No las digas si nadie pregunta.

**Antes de empezar (10 minutos)**

1. Abre el notebook en Jupyter y ejecuta *Kernel → Restart & Run All* para comprobar que todo corre; luego *Clear All Outputs* si quieres que la audiencia vea cada resultado aparecer.
2. Ten ambas pantallas listas: presentación en una ventana, notebook en otra (o dos monitores).
3. Ejecuta de antemano la celda de importaciones de la sección 0 (`import numpy as np`, `import pandas as pd`).
4. No ejecutes las celdas de «Solución» de los ejercicios hasta el momento indicado.
5. Mira la salida de la celda de versiones: algunas pantallas de `info()` y `dtypes` cambian según la versión de pandas (ver «Plan B»).
6. Repasa la tabla de dependencias de abajo: el notebook es un hilo, y saltarse una celda rompe las siguientes.

**Dependencias entre celdas**

El kernel recuerda todo lo que se ha ejecutado, en el orden en que se ejecutó, y no en el orden en que está escrito. Por eso estas celdas son críticas:

| Si no ejecutas… | Falla… | Porque… |
| --- | --- | --- |
| La celda de importaciones | Todo | `np` y `pd` no existen |
| La celda que crea `ventas` (sección 2) | Secciones 2.x, 4, 6 y ejercicios 2 y 4 | Todas leen la tabla `ventas` |
| La celda de columnas nuevas (2.4) | Secciones 4 y 6 y ejercicio 4 | Crea `total`, `mes`, `tipo_venta` y `region` |
| La celda de 1.5 | La celda de 1.6 | El z-score usa `notas` como arreglo |
| `limpiar` (3.5) | Solución del ejercicio 3 | La solución llama a `limpiar(sucio)` |

Cuidado con un nombre repetido: `notas` es una lista de tres números en el calentamiento de Python y un arreglo de 4 × 3 en la sección 1.5. Si alguien vuelve a ejecutar el calentamiento después de la sección 1.5, la celda del z-score falla con `AttributeError: 'list' object has no attribute 'mean'`. La solución es volver a ejecutar la celda de 1.5.

## El hilo conductor

Toda la clase es una sola idea con siete variaciones: operar sobre columnas completas en vez de celda por celda. Repítela al inicio de cada parada y señala en qué variación estás.

| Variación | La pregunta que responde | Dónde aparece |
| --- | --- | --- |
| Vectorización | ¿Cómo calculo sobre todos los datos sin escribir un bucle? | Diapositiva 7 |
| Máscara booleana | ¿Cómo me quedo solo con una parte? | Diapositivas 8 y 15 |
| Broadcasting | ¿Qué pasa si las formas no coinciden? | Diapositivas 9 y 11 |
| `axis` | ¿En qué dirección resumo? | Diapositiva 10 |
| Etiquetas e índice | ¿Cómo nombro filas y columnas? | Diapositivas 12 a 14 |
| `groupby` | ¿Cómo resumo por categorías? | Diapositivas 18 a 20 |
| `merge` | ¿Cómo junto datos que están en tablas distintas? | Diapositiva 21 |

Tres hábitos acompañan a las siete ideas: **predecir antes de ejecutar**, **mirar antes de tocar** y **validar antes de concluir**.

## Para principiantes completos

Con un grupo que nunca ha programado, la clase funciona mejor en tres sesiones de 60 a 70 minutos que en una de 75, y con un calentamiento de Python al inicio. Los minutos del mapa de transiciones valen para un grupo con alguna base; aquí va cómo adaptar el guion.

### Tres sesiones en vez de una

| Sesión | Contenido | Diapositivas | Ejercicios |
| --- | --- | --- | --- |
| 1 · NumPy | Calentamiento de Python (10 min) y NumPy | 1 a 11 | 1 |
| 2 · pandas y limpieza | Tablas, selección, filtros, orden y datos sucios | 12 a 17 | 2 y 3 |
| 3 · Agrupar y unir | groupby, pivot, merge, proyecto y cierre | 18 a 25 | 4 |

Al empezar las sesiones 2 y 3, dedica 5 minutos a repasar con preguntas lo anterior (por ejemplo: «¿qué es un arreglo?», «¿qué hace una máscara?», «¿qué significa axis=0?») y ejecuta *Restart & Run All*.

### Calentamiento de Python (10 minutos, con las diapositivas 4 y 5)

La forma más segura es ejecutar las celdas que ya existen en la sección «Python en 10 minutos» del notebook. Si quieres improvisar variantes, hazlo en una celda nueva al final y con nombres distintos (por ejemplo `mis_notas` en vez de `notas`), para no pisar las variables que usarán las secciones siguientes.

*«No hace falta saber programar. Solo necesitan tres ideas: guardar un dato con un nombre, tener una lista de datos y repetir una acción.»*

```python
x = 5
print(x)
notas = [3.5, 4.0, 2.8]
print(len(notas))
for n in notas:
    print(n * 2)
```

Resultado: `5`, luego `3`, y después `7.0`, `8.0` y `5.6`.

**Código, pieza por pieza**

- `x = 5`: *«Python lee de derecha a izquierda: toma el 5 y lo guarda en una caja con la etiqueta x. El signo igual significa «guárdalo», no «es igual a». Y la caja queda guardada: mientras el cuaderno esté abierto, Python se acuerda de x.»*
- `print(x)`: *«print muestra en pantalla lo que hay dentro de la caja. Sin print, la celda guarda el dato pero no lo enseña.»*
- `notas = [3.5, 4.0, 2.8]`: *«Una lista son varios datos entre corchetes y separados por comas, guardados bajo una sola etiqueta. Cada uno tiene un número de posición, y el primero es el 0, no el 1.»*
- `print(len(notas))`: *«len cuenta cuántos hay: tres. Fíjense en el orden: primero se calcula len(notas), que da 3, y ese 3 es lo que recibe print.»*
- `for n in notas:`: *«Significa: para cada elemento de la lista, llámalo n y haz lo que viene debajo. Los dos puntos del final y la sangría de la línea siguiente son obligatorios: lo que está hacia adentro es lo que se repite.»*
- `print(n * 2)`: *«En cada vuelta, n vale un número distinto.»* Dibuja o proyecta la traza:

| Vuelta | `n` vale | `n * 2` | Se imprime |
| --- | --- | --- | --- |
| 1 | 3.5 | 7.0 | `7.0` |
| 2 | 4.0 | 8.0 | `8.0` |
| 3 | 2.8 | 5.6 | `5.6` |

- *«El for repite una acción con cada elemento, uno por uno. Dentro de un rato verán cómo NumPy hace esto sin repetir nada a mano.»*

**Más ideas para el calentamiento**

- *«Para ejecutar una celda: Shift y Enter. Las celdas se ejecutan en orden; si salto una, la siguiente puede fallar. El cuaderno tiene memoria: si lo reinicio, olvida todo y hay que volver a ejecutar desde arriba.»*
- *«import numpy as np significa: traigo la caja de herramientas NumPy y la llamo np para escribir menos. A partir de ahí, np.algo es una herramienta de esa caja.»*
- *«Una comparación no guarda nada: pregunta. precio == 2500 responde True o False. Con un solo igual guardo; con dos pregunto.»*
- *«Un diccionario es un directorio: busco por nombre y encuentro al instante. region["Cali"] devuelve "Pacífica". Lo vamos a usar para entender cómo funcionan groupby y merge.»*
- *«Una función es una orden con nombre, como len("pandas"). Un método es una orden que pertenece a un dato y se escribe con punto: "pandas".upper(). Regla práctica: si hay un punto antes del nombre y paréntesis después, es un método.»*

**Malentendido típico:** confundir `=` con `==`. Si alguien escribe `precio = 2500` esperando una pregunta, no recibe ningún resultado y cree que no funcionó. Pídele que lea la línea en voz alta: *«guarda 2500 en precio»*, y ahí ve que no preguntó nada.

### Cómo leer una línea de código en voz alta

Hay dos reglas, según cómo esté escrita la línea:

1. **Corchetes o paréntesis anidados: de adentro hacia afuera.** Primero se resuelve lo más interno.
2. **Puntos encadenados: de izquierda a derecha.** Cada punto recibe lo que dejó el paso anterior y le aplica una orden.

Ejemplo con corchetes anidados, `ventas[ventas["precio_unitario"] > 50000]`:

1. `ventas["precio_unitario"]` → la columna del precio.
2. `... > 50000` → una columna de verdadero y falso, una por fila.
3. `ventas[ ... ]` → las filas de ventas donde sale verdadero.

Ejemplo con puntos encadenados, `ventas.groupby("ciudad")["total"].sum()`: la tabla ventas → agrúpala por ciudad → mira la columna total → súmala.

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
| atributo | Un dato que el objeto ya trae y se lee sin paréntesis, como `a.shape` |
| librería | Una caja de herramientas que se importa |
| kernel | La memoria del cuaderno: recuerda lo ejecutado; si lo reinicio, olvida todo |
| arreglo (array) | Una fila de casillas del mismo tipo, cada una con su posición desde 0 |
| `shape` | Cuántas filas y columnas tiene: (2, 3) son 2 filas y 3 columnas |
| `dtype` | El tipo de dato de las casillas: enteros, decimales, texto |
| vectorizar | Dar una sola orden para todas las casillas, sin repetir a mano |
| máscara | Una plantilla de verdadero y falso que decide qué se queda |
| índice | El nombre de cada fila |
| Series y DataFrame | Una columna con nombre de fila; una tabla de varias columnas |
| clave | La columna por la que agrupo o con la que uno dos tablas |
| `NaN` y `NaT` | «No hay dato»: una casilla vacía de número, o de fecha |
| `==` y `=` | `==` pregunta si dos cosas son iguales; `=` guarda un valor |
| hash o diccionario | Un directorio donde busco por nombre y encuentro al instante |

### Ritmo y comprobación

1. **Predecir antes de ejecutar:** *«¿Qué creen que va a salir?»* Espera tres respuestas y ejecuta. Equivocarse con una predicción es parte del aprendizaje.
2. **Cuatro pasos para cada línea nueva:** predice, ejecuta, lee la salida pieza por pieza y cambia un valor para ver qué se mueve. La salida solo enseña si se compara con la predicción.
3. **Una sola idea por diapositiva:** si el grupo se pierde, vuelve al dibujo antes de seguir con el código.
4. **Tranquiliza:** *«No necesitan memorizar la sintaxis. Hoy lo importante es entender la idea; el código se copia y se busca.»*
5. **Ejercicios en parejas:** quien no sepa por dónde empezar copia la celda de arriba y cambia un valor.
6. **Señales de que debes frenar:** silencio prolongado, preguntas sobre sintaxis básica o caras de duda. Pregunta: *«¿qué parte de esta línea no queda clara?»*
7. **Errores como oportunidad:** si una celda falla, léela con el grupo: el mensaje de error dice qué pasó en la última línea. Casi siempre es un nombre mal escrito, una celda sin ejecutar o formas que no encajan.

## Mapa de transiciones

Hay 20 pasos al notebook en 75 minutos de ritmo continuo, sin contar los 10 minutos del calentamiento (para principiantes completos, reparte la tabla en tres sesiones, como indica la sección anterior); el patrón casi siempre es el mismo: la diapositiva explica la idea, el notebook la ejecuta y se vuelve a la diapositiva para interpretar el resultado.

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

**Idea clave:** *«Toda la clase cabe en una frase: dejar de pensar en datos uno por uno y empezar a pensar en columnas completas. Si en Excel han arrastrado una fórmula por toda una columna, ya han pensado así. Hoy lo hacemos con código, y con millones de filas.»*

*«Imaginen NumPy como una caja de huevos: compartimentos iguales y pegados, todos del mismo tipo. Por eso podemos tratar la caja entera de una vez.»* (Diapositiva 3.)

*«Una lista de Python es distinta: es más bien una bolsa de papelitos, y cada papelito dice dónde está guardado el dato, que puede estar en cualquier parte de la memoria y ser de cualquier tipo: un número, un texto, otra lista. Para sumar, Python tiene que ir a cada dirección, preguntar de qué tipo es el dato y recién entonces operar. En la caja de huevos todo es del mismo tipo y está pegado: la máquina recorre la caja de un tirón, sin preguntar nada.»*

*«Y pandas toma esa caja y le pone rótulos: nombre a las columnas, nombre a las filas, y permite que cada columna tenga su propio tipo: números en una, texto en otra, fechas en otra. Es la hoja de cálculo, pero programable y repetible. Por dentro, cada columna de pandas es un arreglo de NumPy; lo comprobaremos en la diapositiva 12.»*

**Pregunta:** ¿por qué una lista de Python no sirve para diez millones de números? (Respuesta guía: guarda referencias a objetos que pueden ser de cualquier tipo y estar dispersos en la memoria, así que cada operación obliga a revisar el tipo del dato y la máquina no puede recorrerlos en bloque; además, ocupa mucha más memoria que un arreglo del mismo tamaño.)

**Si preguntan:** *«¿Por qué no usar solo pandas?»* Porque pandas se apoya en NumPy. Entender el arreglo explica por qué pandas es rápido y por qué algunas cosas fallan, como las formas que no encajan.

### Diapositivas 4 y 5 · Python y cómo leer código (solo con principiantes; 10–12 min)

*«Antes de usar las herramientas, cuatro ideas de Python: guardar un dato con un nombre, tener una lista de datos, repetir una acción y comparar. Con eso alcanza para empezar.»* (Diapositiva 4.)

**\[→ NOTEBOOK\]** Sección «Python en 10 minutos». Ejecuta las celdas en orden: `print`, variables y tipos, operaciones y comparaciones, listas, `for`, diccionario, funciones, el error provocado y el Ejercicio 0. Antes de cada una pregunta «¿qué creen que va a salir?». *«El signo igual guarda; el doble igual pregunta. Y si algo falla, lean la última línea del error: dice qué pasó.»*

**Código, pieza por pieza** (las celdas que más conviene explicar):

- `precio = 2500` y `nombre = "Cuaderno"`: *«El 2500 es un entero, así que Python lo marca como int. El texto va siempre entre comillas, y Python lo marca como str. type(precio) devuelve int y type(nombre) devuelve str. Este tipo es el mismo concepto que dtype en NumPy.»*
- `total = precio * cantidad` y `print(total > 5000)`: *«Primero Python multiplica y guarda 10.000; luego pregunta si es mayor que 5.000 y responde True.»*
- `notas[0]` y `notas[-1]`: *«El primero es el 0. Con menos uno cuento desde el final: es el último.»*
- `region["Cali"]`: *«Busco por nombre, no por posición. Esta idea es el corazón de groupby y de merge.»*
- `"pandas".upper()`: *«El punto significa «de este dato, usa esta orden». Así vamos a escribir casi todo en pandas: ventas.algo().»*
- `try` y `except NameError`: *«Aquí provoco un error a propósito para que lo vean sin miedo: pido una variable que nunca guardé. Python no se rompe: nos cuenta qué pasó, y la última línea del mensaje lo resume.»*

**Malentendido típico:** creer que el notebook ejecuta «todo el archivo» como un programa. Aclara que cada celda se ejecuta cuando tú lo ordenas y que el orden de ejecución es lo que cuenta, no el orden en que están escritas.

**\[← DIAPOSITIVAS\]** Diapositiva 5.

*«Así se lee una línea de código: como una frase. Ventas, agrupa por ciudad, mira la columna total y súmala. No necesitan memorizar la sintaxis: hoy importa entender la idea.»* Pide a una persona que lea en voz alta la tercera línea de la diapositiva.

*«Dos reglas para leer. Si hay corchetes dentro de corchetes, empiezo por lo de adentro. Si hay puntos encadenados, leo de izquierda a derecha, y cada punto trabaja con lo que dejó el anterior.»*

**\[→ NOTEBOOK\]** Baja a «Cómo leer una línea de código» y al glosario. *«Cuando aparezca una palabra nueva, vuelvan aquí.»*

**\[← DIAPOSITIVAS\]** Diapositiva 6.

### Diapositiva 6 · Un arreglo es una fila de casillas (5–9 min)

**Idea clave:** *«Un arreglo es una fila de casillas con número de posición; la primera es la 0. Todas guardan el mismo tipo de dato. Tres datos lo describen: cuántas casillas tiene (su forma, shape), de qué tipo son (dtype) y en cuántas direcciones se extiende (ndim).»*

**\[→ NOTEBOOK\]** Sección 1.1. Ejecuta la celda de `np.array`. *«Aquí está el arreglo de 5 elementos: forma (5,), tipo int64. La matriz tiene forma (2, 3): dos filas y tres columnas.»* Señala `arange` y `linspace`.

**Código, pieza por pieza**

- `np.array([10, 20, 30, 40, 50])`: *«Le doy una lista de Python y NumPy la convierte en un arreglo. Los corchetes de adentro son la lista; np.array es la orden que la transforma.»*
- `a.shape` da `(5,)`: *«La forma es un par de números entre paréntesis; con una sola dimensión queda un número y una coma. Se lee: una fila de 5 casillas.»*
- `a.dtype` da `int64`: *«Enteros de 64 bits: cada casilla ocupa siempre el mismo espacio, y por eso se puede recorrer rápido.»*
- `np.array([[1, 2, 3], [4, 5, 6]])`: *«Dos listas dentro de otra lista forman una matriz. La forma (2, 3) se lee siempre (filas, columnas).»*
- `np.zeros(3)` y `np.ones((2, 2))`: *«Reservan casillas ya llenas con ceros o con unos. Fíjense en el doble paréntesis de ones: el interior es la forma.»*
- `np.arange(0, 10, 2)` da `[0 2 4 6 8]`: *«Empieza en 0, llega hasta antes de 10 y avanza de 2 en 2. El final nunca se incluye.»*
- `np.linspace(0, 1, 5)` da `[0. 0.25 0.5 0.75 1.]`: *«Aquí no digo cuánto avanzo, sino cuántos puntos quiero: cinco, repartidos a igual distancia, y esta vez sí incluye los dos extremos.»*

**Malentendido típico:** creer que `(5,)` y `(5, 1)` son lo mismo. El primero es una fila plana de 5 casillas; el segundo es una columna de 5 filas con 1 casilla cada una. Se verá en broadcasting.

**Demo opcional (30 segundos):** ejecuta `np.array([1, 2.5])`. Da `[1. 2.5]` con dtype `float64`: *«NumPy obliga a todas las casillas a ser del mismo tipo, así que el 1 pasó a ser 1.0. Esa uniformidad es lo que hace posible la velocidad.»*

**Pregunta:** ¿cuántas casillas tiene una matriz de forma (2, 3)? (Seis: 2 por 3.)

**\[← DIAPOSITIVAS\]** Diapositiva 7.

### Diapositiva 7 · Vectorización (9–12 min)

*«Así se calcula el doble de cada número con un bucle: uno por uno. Y así con NumPy: una sola orden para todas las casillas.»*

**Idea clave:** *«Vectorizar no significa que el bucle desaparece: significa que ya no lo escribo yo en Python. NumPy lo ejecuta por dentro, en un lenguaje compilado llamado C, sobre casillas que ya sabe que son del mismo tipo y están pegadas en memoria. Con la lista, Python revisa el tipo de cada elemento mil veces; con el arreglo, no revisa nada.»*

**\[→ NOTEBOOK\]** Sección 1.2. Ejecuta la celda de tiempos. *«Con un millón de números, en la diapositiva la lista tardó unos 0,19 segundos y NumPy unos 0,03: unas 6,5 veces más rápido en esa máquina. Y la última línea dice True: el resultado es idéntico.»* Si tus tiempos difieren, dilo: depende del equipo, y en muchos equipos la diferencia es mayor. Lo que importa es el orden de magnitud, no la cifra exacta.

**Código, pieza por pieza**

- `lista = list(range(n))` y `arr = np.arange(n)`: *«Preparo los mismos números de dos formas: como lista de Python y como arreglo de NumPy. n vale un millón; el guion bajo en 1_000_000 es solo para leerlo mejor.»*
- `[x * 2 for x in lista]`: *«Es un bucle escrito en una línea: para cada x de la lista, calcula x por 2 y arma una lista nueva. Python hace un millón de pasos, uno por uno.»*
- `arr * 2`: *«El operador por, aplicado a un arreglo, significa «multiplica cada casilla». No hay for en ninguna parte.»*
- `time.perf_counter()`: *«Es un cronómetro. Lo miro antes y después y resto: la diferencia es lo que tardó.»*
- `f"{t1 - t0:.4f} s"`: *«Una f-string pone el valor dentro del texto; el :.4f dice «con cuatro decimales».»*
- `doble_lista == doble_arr.tolist()`: *«tolist convierte el arreglo de vuelta a lista para poder compararlos. Da True: gané velocidad sin perder exactitud.»*

**Malentendido típico:** *«Si NumPy es más rápido, siempre debo usarlo.»* No: con 5 elementos la diferencia es invisible. Su ventaja aparece cuando el volumen crece.

**Demo opcional (30 segundos, muy visual):** ejecuta `[1, 2, 3] * 2` y `np.array([1, 2, 3]) * 2`. La lista da `[1, 2, 3, 1, 2, 3]` (la repite) y el arreglo da `[2 4 6]` (multiplica). *«El mismo símbolo significa cosas distintas según la caja. En NumPy, los operadores trabajan casilla por casilla.»*

**\[← DIAPOSITIVAS\]** Diapositiva 8.

### Diapositiva 8 · Máscara booleana (12–15 min)

*«Filtrar es poner una plantilla encima. Primero comparo todo el arreglo con una condición y obtengo una fila de True y False; luego me quedo con lo que cae en True.»*

**Idea clave:** *«Filtrar son dos pasos: primero una pregunta que se hace a todas las casillas a la vez y produce verdadero o falso para cada una; luego, esa fila de verdaderos y falsos se usa como plantilla para decidir quién se queda. La pregunta y el filtro son cosas separadas.»*

**Pregunta:** ¿qué valores quedan con `a > 25`? Espera respuestas.

**\[→ NOTEBOOK\]** Sección 1.3. Ejecuta la celda. *«Quedan 30, 40 y 50. Y para combinar condiciones uso &, | y \~, cada una entre paréntesis; no sirven `and` ni `or`.»*

**Código, pieza por pieza**

- `a[0]`, `a[-1]`, `a[1:4]`, `a[::2]`: *«El primero, el último, las posiciones 1, 2 y 3 (el final no se incluye) y de dos en dos.»*
- `mascara = a > 25`: *«Comparo cada casilla con 25 y guardo la respuesta. Da [False False True True True]: un arreglo nuevo del mismo tamaño. El original no cambia.»*
- `a[mascara]`: *«Pongo la plantilla encima: solo pasan las casillas donde hay True. Quedan 30, 40 y 50.»*
- `a[(a > 15) & (a < 45)]`: *«Dos preguntas, una por cada paréntesis, unidas con & que significa «y». Una casilla pasa si responde True a las dos: quedan 20, 30 y 40.»*
- `m[1, 2]`, `m[:, 0]`, `m[0, :]`: *«En una matriz se pide [fila, columna]. Los dos puntos significan «todas». Entonces m[:, 0] es toda la primera columna y m[0, :] es toda la primera fila.»*

**Malentendido típico:** escribir `a > 15 & a < 45` sin paréntesis. Python hace primero el & y falla con un error de «valor de verdad ambiguo». Con `and` el error es el mismo: Python espera un único verdadero o falso, y un arreglo tiene uno por casilla. Por eso se usan los símbolos &, | y \~, que operan casilla por casilla, y cada pregunta lleva su paréntesis.

**Si preguntan:** *«¿La máscara modifica el arreglo?»* No. Una máscara devuelve un arreglo nuevo con las casillas elegidas; `a` queda intacto.

**\[← DIAPOSITIVAS\]** Diapositiva 9.

### Diapositiva 9 · Broadcasting (15–19 min)

*«Si sumo una fila de tres números a una matriz de dos filas, NumPy fotocopia la fila hacia abajo, sin gastar memoria, hasta que encaje. La regla: se comparan las formas de derecha a izquierda y deben ser iguales o valer uno.»*

**Idea clave:** *«Broadcasting es la respuesta a «¿qué pasa si las formas no coinciden?». Si hay un modo sensato de estirar el arreglo pequeño, NumPy lo estira solo, sin copiar los datos; si no lo hay, falla con un error que dice las dos formas.»*

**Cómo aplicar la regla, con los tres casos del notebook** (escríbelos en la pizarra, alineados a la derecha):

| Caso | Formas alineadas por la derecha | Resultado |
| --- | --- | --- |
| `m + fila` | (2, 3) con (3,): 3 y 3 coinciden; al 2 no le toca pareja y la fila se repite | Funciona: (2, 3) |
| `m + col` | (2, 3) con (2, 1): 3 con 1 se estira; 2 con 2 coinciden | Funciona: (2, 3) |
| `m + np.array([1, 2])` | (2, 3) con (2,): se comparan 3 y 2, que no son iguales y ninguno es 1 | Error |

**\[→ NOTEBOOK\]** Sección 1.4. Ejecuta toda la celda, incluido el error. *«La forma (2,3) con (2,) falla porque 3 y 2 no coinciden: es el error que deben aprender a reconocer.»*

**Código, pieza por pieza**

- `m + 10`: *«Un número suelto se estira para cubrir toda la matriz: [[11 12 13], [14 15 16]].»*
- `m + fila`: *«La fila [100 200 300] se suma a cada fila de m: [[101 202 303], [104 205 306]].»*
- `m + col` con `col = np.array([[1000], [2000]])`: *«Esta vez cada fila de m recibe un número distinto: la primera suma 1000 y la segunda 2000. Da [[1001 1002 1003], [2004 2005 2006]]. Los dobles corchetes hacen que col tenga forma (2, 1): una columna.»*
- `try` y `except ValueError`: *«Provoco el error a propósito y lo atrapo para ver el mensaje: «no se pueden hacer broadcast con las formas (2,3) y (2,)». Ahí está la regla, hecha mensaje.»*

**Malentendido típico:** *«Quiero sumar un valor por fila con un arreglo de forma (2,) y falla.»* Un arreglo plano se alinea con las columnas, no con las filas. La solución es darle forma de columna: `np.array([[1], [2]])`, o convertirlo con `v.reshape(2, 1)`.

**Pregunta:** ¿qué pasaría con una columna de forma (2, 1)? (Se estiraría a lo ancho: es el ejemplo `col`.)

**\[← DIAPOSITIVAS\]** Diapositiva 10.

### Diapositiva 10 · axis (19–22 min)

*«Para recordar axis: es el eje que desaparece. Con axis=0 desaparecen las filas y queda un valor por columna; con axis=1 desaparecen las columnas y queda un valor por fila.»*

**Idea clave:** *«Resumir es aplastar. axis dice qué dirección aplasto. La forma del resultado es la forma original sin ese número: una matriz de forma (4, 3) con axis=0 queda de forma (3,), tres medias; con axis=1 queda de forma (4,), cuatro medias.»* Pídeles que verifiquen la regla con los resultados de la diapositiva.

**\[→ NOTEBOOK\]** Sección 1.5. Ejecuta la celda. *«Media por examen: 3,75, 4,05 y 3,825. Media por alumno: el cuarto tiene 4,87 y su índice es 3.»* Señala la media y la desviación calculadas a mano: dan lo mismo que NumPy.

**Código, pieza por pieza**

- `notas.mean()` sin axis: *«Sin axis aplasto todo en un solo número: 3,875, la media de las doce notas.»*
- `notas.mean(axis=0)`: *«Una media por columna, es decir, por examen: [3.75 4.05 3.825].»*
- `notas.mean(axis=1)`: *«Una media por fila, es decir, por alumno: [3.43 4.5 2.7 4.87].»*
- `.argmax()`: *«No devuelve la nota más alta, sino la posición donde está. La mejor media es la 4,87 y está en la posición 3. Eso sirve para saber quién es, no solo cuánto sacó.»*
- Media a mano: `x = notas[:, 0]` toma la primera columna; `x.sum() / len(x)` es la definición: sumo y divido por cuántos son.
- Desviación a mano, `np.sqrt(((x - media) ** 2).sum() / len(x))`, se lee de adentro hacia afuera: *«A cada nota le resto la media (broadcasting con un solo número). Elevo al cuadrado para que los negativos no cancelen a los positivos. Sumo, divido por cuántas son y saco la raíz para volver a las unidades originales.»* Da 1,1456: *«Es la distancia típica a la media.»*
- `np.isclose(...)`: *«Compara con una pequeña tolerancia, porque los decimales de un computador tienen redondeos diminutos y comparar con == puede engañar.»*

**Si preguntan:** *«¿Por qué en pandas la desviación da distinta?»* NumPy divide por n (`ddof=0`, como si los datos fueran toda la población); pandas divide por n − 1 (`ddof=1`, como si fueran una muestra). Con las notas del primer examen, NumPy da 1,1456 y pandas daría 1,3229. Ninguna está mal: responden preguntas ligeramente distintas.

**\[← DIAPOSITIVAS\]** Diapositiva 11.

### Diapositiva 11 · z-score (22–26 min)

*«Un 4,0 no significa lo mismo en un examen fácil que en uno difícil. Restamos la media y dividimos por la desviación, y todo queda medido con la misma regla.»*

**Idea clave:** *«El z-score responde: ¿cuántas desviaciones estándar está este dato por encima o por debajo de la media? Restar la media centra los datos en cero. Dividir por la desviación cambia la unidad: ya no son «puntos de examen», son «desviaciones». Negativo es por debajo del promedio; positivo, por encima.»*

**Ejemplo a mano con una nota** (escríbelo en la pizarra): examen 1, media 3,75 y desviación 1,1456. Para el 2,0: (2,0 − 3,75) ÷ 1,1456 = −1,53. *«El 2,0 está a una desviación y media por debajo del promedio.»* Para el 5,0: (5,0 − 3,75) ÷ 1,1456 = +1,09.

**\[→ NOTEBOOK\]** Sección 1.6. Ejecuta la celda. *«La media de cada columna es 0 y la desviación 1. El 2,0 del primer examen queda en menos 1,53: el más alejado por debajo.»* Menciona `min-max` (valores entre 0 y 1).

**Código, pieza por pieza**

- `notas.mean(axis=0)`: *«Tres medias, una por examen.»*
- `notas - notas.mean(axis=0)`: *«Aquí está el broadcasting de la diapositiva 9: la matriz es (4, 3) y las medias son (3,). La fila de medias se estira sobre los cuatro alumnos y a cada nota se le resta la media de su propio examen.»*
- `/ notas.std(axis=0)`: *«Lo mismo con las tres desviaciones. En una línea normalizo doce notas sin ningún bucle.»*
- `z.round(2)`: *«Redondea para poder leer.»*
- `z.mean(axis=0).round(10)` muestra `[0. 0. -0.]`: *«Verifico: la media de cada columna es cero. Ese -0. es un cero con un error de redondeo diminuto, no un número negativo.»*
- `mm = (notas - min) / (max - min)`: *«La alternativa: el mínimo de cada examen pasa a ser 0, el máximo pasa a ser 1 y todo lo demás queda en medio. En el primer examen: 0,5, 0,83, 0 y 1.»*

**Malentendido típico:** *«¿Por qué axis=0 y no axis=1?»* Porque queremos comparar a los alumnos dentro de un mismo examen, así que cada columna se normaliza con su propia media. Con axis=1 normalizaríamos por alumno y respondería otra pregunta; además, un arreglo de forma (4,) no encaja con uno de forma (4, 3), y por eso fallaría (la regla de broadcasting de la diapositiva 9).

**Si preguntan:** *«¿Cuándo uso z-score y cuándo min-max?»* z-score cuando importa «cuánto se aleja del promedio» y hay valores extremos; min-max cuando necesito valores entre 0 y 1, sabiendo que un solo dato extremo comprime todos los demás.

### Ejercicio 1 (26–29 min) · permanece en el notebook

*«Ahora les toca a ustedes. Tienen tres minutos para las tres preguntas del Ejercicio 1.»* Camina por el salón. Al terminar, ejecuta la celda Solución (promedio 11,85; quedan 12,5, 15,2 y 20,0).

**Qué reforzar al corregir**

1. `precios.mean()` es la media de las seis casillas: 11,85.
2. `precios[precios > promedio]` es una máscara: la pregunta «¿es mayor que el promedio?» es la plantilla, y el corchete externo filtra. Quedan 12,5, 15,2 y 20,0.
3. `np.where(precios > 10, precios * 0.9, precios)` se lee: *«donde se cumpla la condición, pon el precio con 10 % de descuento; donde no, deja el precio igual.»* Da `[11.25 8. 13.68 9.9 18. 5.5]`. np.where es un `if` vectorizado: una máscara que escoge entre dos arreglos.

**Error frecuente:** mezclar los dos umbrales. En la pregunta 2 se compara con el promedio (11,85) y en la 3 con 10. Por eso 9,9 no recibe descuento aunque esté cerca.

**\[← DIAPOSITIVAS\]** Diapositiva 12.

## Guion, parte 2: pandas y limpieza (minutos 29 a 55)

### Diapositiva 12 · Del arreglo al DataFrame (29–32 min)

**Idea clave:** *«Una Series es una columna con nombre de fila. Un DataFrame es un montón de Series pegadas por el mismo índice. Y por dentro, cada Series es un arreglo de NumPy. Lo que pandas añade a NumPy son las etiquetas: ya no pido «la casilla 2», puedo pedir «la nota de Luis».»*

*«Esto tiene una consecuencia práctica enorme: todo lo que aprendimos en NumPy (vectorización, máscaras, axis) funciona igual sobre las columnas de pandas.»*

**\[→ NOTEBOOK\]** Sección 2. Ejecuta la celda de `pd.Series` y después la celda que crea `ventas`; las siguientes dependen de ella. *«La última línea imprime numpy.ndarray: lo que les prometí al inicio.»*

**Código, pieza por pieza**

- `pd.Series([3.5, 4.1, 2.9], index=["Ana", "Luis", "Marta"], name="nota")`: *«Los valores van en la primera lista, las etiquetas de las filas en index y el nombre de la columna en name.»*
- `s["Luis"]` da 4.1: *«Pido por etiqueta, como en un diccionario.»*
- `s.mean()` da 3.5: *«La misma media de NumPy: (3,5 + 4,1 + 2,9) ÷ 3.»*
- `type(s.values)` da `numpy.ndarray`: *«.values destapa el arreglo que hay por dentro.»*

**La celda que crea `ventas`, línea por línea** (explícala una vez, sin detenerte a calcular nada):

- `rng = np.random.default_rng(7)`: *«Un generador de números al azar con semilla 7. Con la misma semilla todos obtenemos exactamente los mismos datos, así que lo que ven en pantalla coincide con las diapositivas.»*
- `pd.DataFrame({...})`: *«Un DataFrame se arma con un diccionario: cada nombre es una columna y cada lista, sus valores.»*
- `np.arange(1, n + 1)`: *«Los números del 1 al 40, para el identificador de cada venta.»*
- `pd.date_range("2026-01-01", periods=n, freq="3D")`: *«40 fechas, una cada 3 días a partir del 1 de enero.»*
- `rng.choice([...], size=n)`: *«Sortea 40 veces una de las opciones. Así se arman la ciudad y el producto.»*
- `rng.integers(1, 10, size=n)`: *«Enteros del 1 al 9: el 10 no se incluye, como en arange.»*
- `.astype(float)`: *«Convierte los precios a decimales.»*
- `ventas.head()`: *«Las cinco primeras filas. Siempre empiecen mirando.»*

**Aviso para el orador:** el producto y el precio se sortean por separado, así que hay «lápices» de 85.000 y «mochilas» de 1.200. Los datos son sintéticos y sirven para practicar el método; si alguien lo nota, dilo con naturalidad: *«Es un dato inventado: lo que importa hoy es el método.»*

**Malentendido típico:** *«¿Una Series es una lista?»* Se parece, pero tiene dos cosas que la lista no tiene: etiquetas y un único tipo de dato. Cuando dos Series se operan, pandas las alinea por etiqueta, no por posición.

**\[← DIAPOSITIVAS\]** Diapositiva 13.

### Diapositiva 13 · Conoce tus datos (32–35 min)

**Idea clave:** *«Nunca analicen sin mirar. Cuatro herramientas: `head`, `shape`, `info` y `describe`. Cada una responde una pregunta: ¿qué aspecto tienen?, ¿cuántos son?, ¿de qué tipo y cuántos faltan?, ¿cómo se distribuyen?»*

**\[→ NOTEBOOK\]** Sección 2.1. Ejecuta `shape`, `info`, `describe` y `value_counts`. *«Son 40 ventas y 6 columnas, ningún dato nulo. Barranquilla tiene 14 ventas y Bogotá solo 5.»*

**Código, pieza por pieza**

- `ventas.shape` da `(40, 6)`: *«Sin paréntesis, porque no es una orden: es un dato que la tabla ya trae. Se lee (filas, columnas).»*
- `list(ventas.columns)`: *«Los nombres de las columnas.»*
- `ventas.info()`: *«Es una orden, lleva paréntesis. Mire la columna Non-Null Count: dice cuántos datos hay en cada columna; si algún número es menor que 40, faltan datos ahí. Y mire Dtype: el tipo de cada columna, que debe tener sentido.»*
- `ventas.describe()`: *«Resumen estadístico: cuántos, media, desviación, mínimo, cuartiles y máximo. La cantidad media es 5,1 y el precio unitario medio es 37.175.»*
- `ventas["ciudad"].value_counts()`: *«Cuenta cuántas veces aparece cada valor de una columna de categorías, ordenado de mayor a menor.»*

**Malentendido típico:** *«describe() resume todo lo que tiene sentido resumir.»* No sabe lo que significan las columnas: también calcula la media de `id_venta`, que es solo un identificador y no tiene interpretación. Eso lo decide quien analiza.

**Pregunta:** ¿qué ciudad tiene menos ventas? (Bogotá.) Guárdenlo para después: cuando sumemos ventas por ciudad, tendremos que recordar que Bogotá parte con menos filas.

**\[← DIAPOSITIVAS\]** Diapositiva 14.

### Diapositiva 14 · loc e iloc (35–38 min)

**Idea clave:** *«Dos formas de pedir filas: por nombre o por puesto. loc viene de Label, etiqueta: pido las filas por su nombre. iloc viene de Integer, posición: las pido por su puesto en la tabla. loc incluye el final del rango; iloc lo excluye.»*

*«Hoy el nombre de cada fila coincide con su puesto (0, 1, 2…), y por eso parecen lo mismo. En cuanto filtre u ordene la tabla, los nombres se quedan con su fila y los puestos se reacomodan.»*

**\[→ NOTEBOOK\]** Sección 2.2. Ejecuta la celda. *«loc de 0 a 2 devuelve tres filas; iloc de 0 a 2 devuelve dos.»*

**Código, pieza por pieza**

- `ventas["producto"]`: *«Corchetes simples con un nombre: una columna sola, que es una Series.»*
- `ventas[["producto", "cantidad"]]`: *«Corchetes dobles: el de afuera es «selecciona» y el de adentro es una lista de nombres. El resultado es una tabla.»*
- `ventas.loc[0:2, ["producto", "cantidad"]]`: *«Las filas con etiqueta 0, 1 y 2 (incluye el 2), y solo esas dos columnas, pedidas por nombre.»*
- `ventas.iloc[0:2, 0:3]`: *«Las filas de posición 0 y 1 (el 2 no entra), y las columnas de posición 0, 1 y 2: id_venta, fecha y ciudad.»*

**Demo opcional que lo aclara todo (1 minuto):** ejecuta `cali = ventas[ventas["ciudad"] == "Cali"]`, y luego `cali.iloc[0]` (devuelve la primera fila que quedó) y `cali.loc[0]` (da `KeyError: 0`). *«La etiqueta 0 era de una venta de Barranquilla y ya no está en esta tabla. iloc cuenta puestos; loc busca nombres.»*

**Pregunta rápida:** ¿cuántas filas devuelve `ventas.iloc[3:6]`? (Tres: posiciones 3, 4 y 5.)

**\[← DIAPOSITIVAS\]** Diapositiva 15.

### Diapositiva 15 · Filtros y columnas nuevas (38–42 min)

**Idea clave:** *«Es la misma plantilla de True y False de NumPy, pero con una entrada por fila.»* Y para crear columnas: *«Operar columnas completas es vectorización otra vez: multiplico dos columnas y obtengo una columna nueva, sin bucle.»*

**\[→ NOTEBOOK\]** Sección 2.3. Ejecuta la celda de filtros. *«Veinte de las cuarenta ventas son de productos de más de 50.000. Y la combinación Bogotá con Mochila devuelve vacío: no es un error, significa que no hubo coincidencias.»* Luego sección 2.4: ejecuta la celda que crea `total`, `tipo_venta` y `region`.

**Código, pieza por pieza (filtros)**

- `ventas[ventas["precio_unitario"] > 50000]`: *«Lo de adentro es la pregunta: una columna de True y False, una por fila. Lo de afuera es el filtro: me quedo con las filas donde sale True.»* Puedes ejecutar solo la parte interna para que vean la plantilla.
- `(ventas["ciudad"] == "Bogotá") & (ventas["producto"] == "Mochila")`: *«Dos preguntas, cada una entre paréntesis, unidas con &. Dos iguales porque pregunto, no guardo. Da una tabla vacía: ninguna venta cumple las dos a la vez.»*
- `ventas["ciudad"].isin(["Cali", "Medellín"])`: *«¿Está el valor dentro de esta lista? Evita escribir dos preguntas unidas con |. Salen 21 filas.»*
- `ventas.query("cantidad >= 8 and ciudad == 'Bogotá'")`: *«Otra forma de escribir el filtro: como un texto. Aquí sí puedo usar and, porque query lo interpreta él mismo.»* Devuelve 1 fila.

**Código, pieza por pieza (columnas nuevas)**

- `ventas["total"] = ventas["cantidad"] * ventas["precio_unitario"]`: *«Multiplica las dos columnas fila a fila y guarda el resultado en una columna nueva. Si el nombre de la columna no existe, la crea.»*
- `ventas["fecha"].dt.month`: *«El prefijo .dt abre las herramientas de fechas, igual que .str abre las de texto. Saca el mes de cada fecha.»*
- `np.where(ventas["total"] >= 100000, "grande", "pequeña")`: *«Un if para toda la columna: donde el total llegue a 100.000 pongo «grande»; donde no, «pequeña».»*
- `ventas["ciudad"].map(region)`: *«Traduce cada ciudad con un diccionario. Es como el directorio de la diapositiva 4: busco la ciudad y encuentro su región.»* Aclara que si una ciudad no estuviera en el diccionario, el resultado sería `NaN`.

**Malentendido típico:** *«Una tabla vacía significa que me equivoqué.»* No: significa que la pregunta se hizo bien y no hubo coincidencias. El error es otra cosa.

**\[← DIAPOSITIVAS\]** Diapositiva 16.

### Diapositiva 16 · Ordenar (42–44 min)

**Idea clave:** *«Ordenar es reubicar filas completas según una o varias claves. Con dos claves, la segunda solo desempata a la primera.»*

*«Las barras están ordenadas de mayor a menor total. Con dos claves, primero ciudad y dentro de cada ciudad el total.»*

**\[→ NOTEBOOK\]** Sección 2.5. Ejecuta las dos celdas de `sort_values`. *«La venta más grande es la 38, una calculadora de 765.000.»*

**Código, pieza por pieza**

- `ventas.sort_values("total", ascending=False)`: *«Ordena por la columna total; ascending=False pide de mayor a menor. Devuelve una tabla nueva: la original no se toca.»*
- `.head(5)[["id_venta", "producto", "total"]]`: *«Es una cadena que se lee de izquierda a derecha: ordeno, tomo las cinco primeras y me quedo con tres columnas.»*
- `sort_values(["ciudad", "total"], ascending=[True, False])`: *«Dos claves y dos direcciones, en el mismo orden. Ciudad de la A a la Z y, dentro de cada ciudad, el total de mayor a menor. Barranquilla abre con 680.000, 540.000 y 480.000.»*

**Por qué n log n** (para quien pregunte por el costo): *«Un algoritmo eficiente parte la lista a la mitad una y otra vez. Con un millón de filas, eso son unos 20 niveles, y en cada nivel se compara cada fila una vez: unos 20 millones de comparaciones. Comparar cada fila con todas las demás serían un billón.»*

**Malentendido típico:** olvidar que `sort_values` devuelve una tabla nueva. Si escribes `ventas.sort_values("total")` en una celda y luego miras `ventas`, la tabla sigue en el orden original. Para conservar el resultado, guárdalo: `ordenada = ventas.sort_values("total")`.

### Ejercicio 2 (44–47 min) · permanece en el notebook

*«Tres preguntas: cuántas calculadoras se vendieron en Cali, cuál es el total más alto y creen la columna de IVA.»* Resultado esperado: 5 ventas; 765.000 de una calculadora; primer IVA 1.140.

**Pistas, de menos a más:**

1. Es un filtro con dos condiciones unidas por &; `len(...)` cuenta las filas que quedaron.
2. `ventas["total"].max()` da el valor, pero no dice a qué fila pertenece. `idxmax()` devuelve la etiqueta de esa fila, y con `loc` se leen su total y su producto.
3. Una columna nueva se crea igual que `total`: `ventas["iva"] = ventas["total"] * 0.19`.

**\[← DIAPOSITIVAS\]** Diapositiva 17.

### Diapositiva 17 · Limpieza (47–53 min)

**Idea clave:** *«Los datos reales casi nunca llegan limpios. Limpiar no es borrar: cada paso es una decisión, con una consecuencia sobre los resultados. Quien limpia deja registro de lo que decidió.»*

*«Miren la tabla de la izquierda y díganme qué está mal.»* Deja que el grupo encuentre: espacios sobrantes, mayúsculas mezcladas, un nombre faltante, dos ingresos vacíos, una edad escrita como texto y una fila repetida.

**\[→ NOTEBOOK\]** Sección 3. Ejecuta `sucio`; luego 3.1 a 3.5 en orden, comentando cada paso: faltantes con la mediana, duplicados, texto con `.str`, tipos con `to_numeric` y `to_datetime`. Termina ejecutando `limpiar(sucio)`.

**Cada problema, con su decisión**

| Problema | Herramienta | Qué decir |
| --- | --- | --- |
| Valores faltantes | `isna().sum()`, `dropna`, `fillna` | *«Primero cuento cuántos faltan. Después decido: o elimino la fila, o relleno con un valor razonable. Cada opción tiene un costo: eliminar pierde información; rellenar inventa un dato.»* |
| Mediana en vez de media | `fillna(x.median())` | *«Con un ingreso extremo, la media se distorsiona; la mediana, el valor del medio, no. Por eso es más segura para rellenar.»* |
| Duplicados | `duplicated()`, `drop_duplicates()` | *«duplicated marca las repeticiones a partir de la segunda aparición; drop_duplicates las elimina y deja una.»* |
| Texto desordenado | `.str.strip().str.title()` | *«.str aplica una orden de texto a toda la columna. strip quita los espacios de los extremos y title pone mayúscula inicial en cada palabra. Así «bogotá» y «Bogotá» pasan a ser la misma ciudad.»* |
| Número escrito como texto | `pd.to_numeric(errors="coerce")` | *«Convierte a número. Donde no puede, como con «abc», deja NaN en vez de detener el programa, y el problema queda a la vista. Por eso la edad pasa a mostrar 23.0: con un NaN en la columna, todos los valores pasan a decimal.»* |
| Fechas en formatos mezclados | `pd.to_datetime(format=..., errors="coerce")` | *«Le digo en qué formato espero la fecha: año-mes-día. La fecha «05/02/2026» no encaja y se convierte en NaT, «no hay fecha». No adivino porque podría ser el 5 de febrero o el 2 de mayo; es mejor tratar ese caso aparte.»* |

**Código, pieza por pieza (la función `limpiar`)**

- `def limpiar(df):`: *«Definir una función es ponerle nombre a una receta de varios pasos. La flecha y los tipos son solo documentación para el lector.»*
- `out = df.copy()`: *«Trabajo sobre una copia para no modificar la tabla original: una función que altera sus datos de entrada da sorpresas.»*
- Los pasos siguen el orden de la tabla anterior: cada línea reemplaza una columna por su versión limpia.
- `.drop_duplicates().reset_index(drop=True)`: *«Después de borrar filas, las etiquetas quedan con huecos (0, 1, 2, 4…). reset_index las renumera de 0 en adelante y drop=True descarta las etiquetas viejas.»*
- `limpiar(sucio)`: *«Así quedó: cuatro filas, con el ingreso de Luis imputado en 2.950.000.»* Apunta lo que sigue pendiente a propósito: la edad de Pedro es NaN y su fecha es NaT. *«Dejarlos a la vista es mejor que inventar un dato.»*

**Para un grupo avanzado:** *«¿De dónde sale el 2.950.000?»* Es la mediana de los ingresos que quedaban cuando se rellenó: 2.500.000, 3.100.000, 3.100.000 y 2.800.000. Como el duplicado de Marta todavía estaba en la tabla, su ingreso contó dos veces. Si se eliminaran los duplicados antes de rellenar, la mediana sería 2.800.000. *«El orden de los pasos cambia el resultado: es una decisión que hay que documentar.»*

### Ejercicio 3 (53–55 min) · permanece en el notebook

*«Aún queda una edad en NaN. Rellénenla con la mediana.»* Resultado esperado: 27,0.

**Qué reforzar al corregir:** la mediana de las edades conocidas (23, 31 y 27) es 27, el valor del medio una vez ordenadas. `fillna` reemplaza el hueco con ese valor, y `median()` ignora los NaN al calcular.

**\[← DIAPOSITIVAS\]** Diapositiva 18.

## Guion, parte 3: agrupar, unir y cierre (minutos 55 a 75)

### Diapositiva 18 · Split-apply-combine (55–59 min)

**Idea clave:** *«Esta es la idea más importante de pandas. Tengo cuatro ventas de dos ciudades. Primero las reparto en cajones por ciudad (split); luego hago un cálculo dentro de cada cajón, por ejemplo sumar (apply); al final junto los resultados en una tabla nueva (combine). Es lo que hace una tabla dinámica de Excel, o lo que haría una persona repartiendo facturas en pilas por ciudad y sumando cada pila.»*

**\[→ NOTEBOOK\]** Sección 4. Ejecuta primero la celda del diccionario. *«Así lo haría a mano: da Bogotá 150 y Cali 100. Y pandas lo hace en una línea con `groupby`, con el mismo resultado.»* Ejecuta la celda de `groupby` por ciudad con las 40 ventas: Barranquilla 3.101.000, Cali 1.874.300, Medellín 1.485.400, Bogotá 984.600.

**Código, pieza por pieza (a mano, con diccionario)**

- `acumulado = {}`: *«Un directorio vacío: todavía no hay ningún cajón.»*
- `for ciudad, total in datos:`: *«Recorro las ventas de una en una. En cada vuelta, la tupla se abre en dos: ciudad y total.»*
- `acumulado.get(ciudad, 0) + total`: *«get busca el cajón de esa ciudad; si todavía no existe, devuelve 0. Le sumo el total nuevo y lo guardo.»*

Traza en la pizarra, con las cuatro filas:

| Fila | Ciudad | Total | `acumulado` después |
| --- | --- | --- | --- |
| 1 | Bogotá | 100 | `{Bogotá: 100}` |
| 2 | Cali | 70 | `{Bogotá: 100, Cali: 70}` |
| 3 | Bogotá | 50 | `{Bogotá: 150, Cali: 70}` |
| 4 | Cali | 30 | `{Bogotá: 150, Cali: 100}` |

**Código, pieza por pieza (con pandas)**

- `mini.groupby("ciudad")`: *«Split: reparte las filas en cajones según la ciudad. Todavía no calcula nada.»*
- `["total"]`: *«Dice en qué columna voy a calcular.»*
- `.sum()`: *«Apply y combine: suma dentro de cada cajón y junta los resultados.»*
- El resultado es una Series cuyo índice son las ciudades: *«La clave por la que agrupé se convierte en el nombre de las filas.»*
- `.sort_values(ascending=False)`: *«Para ver primero la ciudad que más vende.»*

**Pregunta:** ¿qué cambia si quiero el promedio en vez de la suma? (Solo el paso Apply: `.sum()` pasa a `.mean()`; el split y el combine son iguales.)

**Malentendido típico para el cierre del bloque:** Barranquilla suma más del triple que Bogotá, pero tiene 14 ventas y Bogotá solo 5, es decir, casi tres veces más filas. Pregunta: *«¿Vende más porque vende mejor o porque vende más veces?»* Con `ventas.groupby("ciudad")["total"].agg(["count", "sum", "mean"])` se ve que Barranquilla promedia 221.500 por venta y Bogotá 196.920: casi toda la diferencia es volumen, no calidad. *«Una suma sola puede engañar; mírenla siempre junto a cuántos hay.»*

**\[← DIAPOSITIVAS\]** Diapositiva 19.

### Diapositivas 19 y 20 · Agregaciones y tabla dinámica (59–62 min)

**Idea clave:** *«Puedo pedir varias métricas a la vez, y puedo agrupar por dos claves en vez de una. Con dos claves, el resultado se puede presentar como una matriz: eso es una tabla dinámica.»*

*«Cuaderno vende más en total, pero Lápiz tiene el ticket promedio más alto. Y si agrupo por dos claves, obtengo este mapa de calor.»*

**\[→ NOTEBOOK\]** Sección 4, celda `agg`; después 4.1, celda `pivot_table`. *«Total general: 7.445.300. Bogotá depende casi por completo de Lápiz; Cali no vendió nada de Lápiz.»* Opcional si hay tiempo: 4.2, `resample` (ventas por mes).

**Código, pieza por pieza**

- `.agg(ventas=("id_venta", "count"), ...)`: *«Cada línea tiene la forma nombre_nuevo = (columna, función). Leo la primera: crea una columna llamada «ventas» que cuenta los id_venta de cada producto.»*
- `ticket_promedio=("total", "mean")`: *«Ticket promedio es el valor medio de una venta. Cuaderno tiene 13 ventas que suman 2.300.600, y de ahí sale 176.969; Lápiz tiene 9 ventas que suman 2.154.700, y de ahí 239.411.»*
- *«Fíjense en la diferencia: Cuaderno suma más porque se vende más veces; Lápiz vale más cada vez. Son dos preguntas distintas y los dos números sirven.»*
- `groupby(["ciudad", "producto"])`: *«Dos claves: un cajón por cada pareja ciudad-producto.»*
- `.unstack(fill_value=0)`: *«Toma la segunda clave, el producto, y la pone en las columnas. Donde una pareja no existe, rellena con 0 en vez de dejar un hueco.»*
- `pd.pivot_table(ventas, values="total", index="ciudad", columns="producto", aggfunc="sum", ...)`: *«Lo mismo, con todo explícito: qué calculo (values), qué va en las filas (index), qué va en las columnas (columns) y cómo lo resumo (aggfunc).»*
- `margins=True, margins_name="Total"`: *«Agrega la fila y la columna de totales.»*

**Cómo leer la tabla dinámica:** Barranquilla se apoya en Cuaderno (1.490.000), Bogotá casi solo en Lápiz (956.200 de 984.600) y Cali en Calculadora (1.127.000). El cero de Cali con Lápiz significa que esa pareja no tuvo ventas, no que falten datos.

**Opcional, `resample` (4.2):** *«Es un groupby por ventanas de tiempo: reparte las ventas por mes y suma cada mes.»* Resultado: enero 2.710.200, febrero 912.100, marzo 1.313.400 y abril 2.509.600. `"MS"` significa «inicio de mes».

### Ejercicio 4 (62–65 min) · permanece en el notebook

*«Cantidad promedio por venta en cada región, y cuál región tiene el mayor promedio.»* Resultado esperado: Andina 4,88; Caribe 6,00; Pacífica 4,20; la mayor es Caribe.

**Qué reforzar al corregir:** `groupby("region")["cantidad"].mean()` cambia solo el Apply, como en la pregunta de la diapositiva 18. `idxmax()` devuelve el nombre del grupo que tiene el máximo. La columna `region` existe solo si se ejecutó la celda 2.4. Si alguien obtiene `KeyError: 'region'`, falta ejecutarla.

**\[← DIAPOSITIVAS\]** Diapositiva 21.

### Diapositiva 21 · Merge (65–69 min)

**Idea clave:** *«Los datos suelen estar repartidos en varias tablas para no repetir información: el nombre del cliente se guarda una sola vez y cada pedido solo guarda su número. Para juntarlos necesito una llave que aparezca en ambas tablas. Aquí, clientes y pedidos comparten `id_cliente`.»*

*«El algoritmo: construyo un diccionario con la tabla de pedidos y para cada cliente busco su llave. Es casi instantáneo, en vez de comparar todos contra todos.»*

**Por qué es rápido:** *«Con mil clientes y mil pedidos, comparar todos contra todos son un millón de comparaciones. Con el diccionario son unos dos mil pasos: mil para armarlo y mil para buscar.»* (Del orden n × m al orden n + m.)

**\[→ NOTEBOOK\]** Sección 5.1. Ejecuta la celda de `merge` y muestra los tres resultados. *«Inner: 3 filas, Ana dos veces porque tiene dos pedidos y Marta. Left conserva a todos los clientes, y Luis y Pedro quedan con NaN: 5 filas. Outer añade el pedido 104, cuyo cliente no existe: 6 filas.»* Ejecuta también la celda del hash join a mano: da lo mismo que el inner. Menciona `concat` (5.2) en una frase: apila tablas, no busca llaves.

**Código, pieza por pieza**

- `clientes.merge(pedidos, on="id_cliente", how="inner")`: *«Une clientes con pedidos por la columna id_cliente. how decide qué hago con las llaves sin pareja.»*
- `inner`: *«Solo las llaves que están en las dos tablas: Ana y Marta.»*
- `left`: *«Todos los de la izquierda, tengan o no pareja: Luis y Pedro se quedan, con NaN en las columnas del pedido.»* Los números del pedido pasan a decimales (101.0) por el mismo motivo de siempre: un NaN obliga a toda la columna a ser decimal.
- `outer` con `indicator=True`: *«Todos los de ambas tablas. La columna `_merge` dice de dónde viene cada fila: both, left_only o right_only. Es una buena forma de depurar una unión.»*

**Código, pieza por pieza (hash join a mano)**

- `pedidos.to_dict("records")`: *«Convierte la tabla en una lista de filas, y cada fila es un diccionario.»*
- `indice.setdefault(fila["id_cliente"], []).append(fila)`: *«Fase 1: para cada pedido, si su llave aún no tiene lista, créala vacía; luego agrega el pedido. Al terminar el índice queda así: 1 → pedidos 101 y 102, 3 → pedido 103, 9 → pedido 104.»*
- `indice.get(c["id_cliente"], [])`: *«Fase 2: para cada cliente, busco su llave en el índice; si no existe, devuelve una lista vacía y no pasa nada.»*
- `{**c, **p}`: *«Junta el diccionario del cliente con el del pedido en una sola fila.»*
- Un cliente sin pedidos no genera filas porque su lista es vacía: por eso este código es un inner.

**Malentendido típico:** olvidar que una llave repetida multiplica filas. Ana aparece dos veces porque tiene dos pedidos. Si un `merge` devuelve más filas de las esperadas, revisa si la llave se repite en alguna de las dos tablas.

**\[← DIAPOSITIVAS\]** Diapositiva 22.

### Diapositiva 22 · Mini-proyecto (69–72 min)

**Idea clave:** *«Unamos todo en un flujo: cargar, validar, transformar, agrupar y comunicar. La pregunta: ¿qué ciudad y producto generan más ingresos? Lo importante no es la respuesta, sino el método, que sirve igual con un archivo real.»*

**\[→ NOTEBOOK\]** Sección 6. Ejecuta la celda de validación y agrupación, y luego la de la gráfica. *«Los `assert` detendrían el programa si algo estuviera mal. El resultado: Barranquilla con Cuaderno, 1.490.000.»* Aclara que los datos son aleatorios; lo que importa es el método.

**Código, pieza por pieza**

- `df = ventas.copy()`: *«En un caso real aquí estaría pd.read_csv("archivo.csv"). Trabajo sobre una copia para no tocar el original.»*
- `assert df["id_venta"].is_unique, "id_venta duplicado"`: *«Una afirmación que el programa comprueba: «los id deben ser únicos». Si es verdad, no pasa nada; si es falsa, el programa se detiene y muestra el mensaje. Es mejor detenerse que producir un análisis falso.»*
- `df[["cantidad", "precio_unitario"]].gt(0).all().all()`: *«gt(0) pregunta, para cada casilla, si es mayor que cero. El primer all() resume cada columna: ¿todas cumplen? El segundo resume las dos columnas en un único verdadero o falso.»*
- `df["ingreso"] = df["cantidad"] * df["precio_unitario"]`: *«Transformar: una columna nueva con la cantidad por el precio. Coincide con `total` de la sección 2.4; la recalculamos aquí solo para mostrar el paso.»*
- `df.groupby(["ciudad", "producto"])["ingreso"].sum().sort_values(ascending=False).head(5)`: *«Agrupo por pareja, sumo, ordeno de mayor a menor y me quedo con las cinco primeras. Los paréntesis que envuelven la cadena permiten escribirla en varias líneas.»*
- `df.pivot_table(values="ingreso", index="mes", columns="region", ...)`: *«El ingreso de cada mes en cada región. Usa la columna `mes` de la sección 2.4.»*
- `por_mes.plot(kind="bar")`: *«pandas dibuja con una librería llamada matplotlib. Cada grupo de barras es un mes y cada color, una región.»*

**Cómo leer el resultado:** Barranquilla aparece tres veces en el top 5 (con Cuaderno, Lápiz y Mochila). Por mes, en enero domina el Caribe (1.651.000) y en abril, la región Pacífica (1.429.800) y el Caribe (1.075.000). *«Comunicar es elegir la gráfica que responde la pregunta, con título y sin adornos.»*

**\[← DIAPOSITIVAS\]** Diapositiva 23.

### Diapositivas 23 a 25 · Errores, resumen y cierre (72–75 min)

*«Cuatro tropiezos frecuentes: bucles sobre filas, `and` en vez de `&`, confundir loc con iloc y modificar un recorte sin `.copy()`.»* (Diapositiva 23.)

**Por qué ocurre cada uno**

1. **Bucle sobre filas:** *«Es lento porque cada vuelta ocurre en Python, no en C, y es largo de escribir. Casi siempre existe una operación de columna que lo reemplaza.»*
2. **`and` en vez de `&`:** *«and espera un único verdadero o falso, y una columna tiene uno por fila. pandas lo avisa con el error «The truth value of a Series is ambiguous». La solución: &, | y paréntesis.»*
3. **loc contra iloc:** *«loc busca nombres e incluye el final; iloc cuenta puestos y lo excluye. En cuanto filtro u ordeno, dejan de coincidir.»*
4. **Modificar un recorte:** *«Si extraigo una parte de una tabla y la modifico, no siempre es claro si estoy cambiando la copia o la original. Con `.copy()` dejo la intención explícita.»* Según la versión de pandas, puede aparecer una advertencia (`SettingWithCopyWarning`) o no; en pandas 3 el original nunca se altera, pero la costumbre de `.copy()` sigue siendo buena.

*«Los ocho algoritmos de hoy: vectorización, broadcasting, media y desviación, normalización, máscara booleana, ordenamiento, split-apply-combine y hash join.»* Pide que cada persona diga cuál le pareció más difícil y la explique con sus palabras. (Diapositiva 24.)

*«Si se llevan una sola idea: piensen en columnas, no en celdas. Todo lo de hoy está en el notebook, con la sección 8 como resumen. Gracias.»* (Diapositiva 25.) Abre el espacio de preguntas.

**Tarea sugerida:** repetir el recorrido con un CSV propio, con el flujo de la sección 8: *cargar → explorar → limpiar → transformar → agrupar o unir → visualizar → concluir*.

## Plan B: tiempo y fallos en vivo

Si vas justo de tiempo, recorta en este orden; cada corte libera los minutos indicados sin romper la secuencia de ideas.

| Si te faltan | Recorta | Ahorras |
| --- | --- | --- |
| 3 min | Diapositiva 16 (Ordenar) y su sección 2.5 | 2 min |
| 6 min | Diapositiva 20 (Pivot) y el Ejercicio 4: pídelo como tarea | 5 min |
| 10 min | Ejercicios 2 y 3 en clase: se dejan como tarea con su solución | 5 min |
| 15 min | Diapositiva 23 (Errores): se resume en una frase al cierre | 3 min |

No recortes la celda que crea `ventas` ni la sección 2.4: las secciones 4 y 6 dependen de ellas. Tampoco recortes los bloques «Idea clave»: son lo que el grupo se lleva. Lo que sí puedes saltar sin riesgo son los bloques «Si preguntan» y las demos opcionales.

Si vas adelantado, añade: `min-max` en la sección 1.6, `resample` en 4.2, `concat` en 5.2 y las demos opcionales (`np.array([1, 2.5])`, `[1, 2, 3] * 2`, `loc` contra `iloc` en una tabla filtrada).

**Si algo falla en vivo**

| Problema | Qué hacer |
| --- | --- |
| `NameError: name 'ventas' is not defined` | Falta ejecutar la celda que crea `ventas` (sección 2). Ejecútala y repite la celda que falló |
| `KeyError: 'total'` o `KeyError: 'region'` | Falta ejecutar la celda de columnas nuevas (2.4). Ejecútala y repite la celda que falló |
| `AttributeError: 'list' object has no attribute 'mean'` | `notas` volvió a ser una lista porque se reejecutó el calentamiento. Ejecuta de nuevo la celda de 1.5 |
| `ValueError: The truth value of ... is ambiguous` | Se usó `and` u `or` en un filtro, o faltaron los paréntesis. Cámbialos por `&` o `\|` y rodea cada condición con paréntesis |
| Los números no coinciden con las diapositivas | Ejecuta *Restart & Run All*; los datos usan semilla fija (7), así que deben coincidir. Los tiempos de la sección 1.2 sí cambian en cada equipo |
| `info()` o `dtypes` muestran `object` en lugar de `str`, o `datetime64[ns]` en lugar de `[us]` | Es normal: depende de la versión de pandas (en la 3 los textos se muestran como `str`). Los datos y los resultados son los mismos |
| Kernel colgado o sin responder | *Kernel → Restart* y vuelve a ejecutar desde la sección 0 (menos de un minuto) |
| La gráfica de la sección 6 no aparece | Ejecuta `%matplotlib inline` en una celda y vuelve a correr la gráfica |
| No hay Jupyter disponible | Sigue solo con las diapositivas: cada una ya muestra el código y el resultado real, y el diálogo de arriba sirve igual sin los pasos al notebook |

Los datos de la clase son sintéticos; no hay información real de personas en ninguno de los ejemplos.
