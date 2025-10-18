# -rbol_decisi-n


# Taller Árbol de Decisión — Clasificación de Vinos

## 1. Introducción

En este taller se utiliza el Wine Dataset del repositorio UCI, disponible en `sklearn.datasets`.
El objetivo es predecir la clase de vino (0, 1 o 2) a partir de variables químicas como alcohol, ácido málico, flavonoides, entre otras.

### Preguntas y Respuestas

**¿Qué diferencia hay entre un problema de regresión y uno de clasificación?**

* Regresión: la variable objetivo es numérica continua (por ejemplo, predecir precio o temperatura).
* Clasificación: la variable objetivo es categórica (por ejemplo, tipo de vino o clase de flor).

**¿Por qué un árbol de decisión es adecuado para clasificación?**
Porque divide los datos en ramas jerárquicas según reglas lógicas ("si... entonces...") y permite predecir etiquetas discretas. Además, es interpretable y visualizable fácilmente.

---

## 2. Carga y exploración de datos

### Librerías principales

* `pandas` y `numpy`: manejo y análisis de datos tabulares.
* `matplotlib` y `seaborn`: visualización gráfica.
* `sklearn.datasets`, `sklearn.tree`, `sklearn.model_selection`, `sklearn.metrics`: librerías de scikit-learn para cargar datos, crear modelos, dividir conjuntos y evaluar resultados.

### Balance de clases

El dataset de vinos tiene tres clases balanceadas, aproximadamente con el mismo número de ejemplos por clase.
Esto es importante porque:

* Si una clase domina, el modelo puede aprender un sesgo hacia ella.
* El balance permite una evaluación más justa y un modelo más generalizable.

---

## 3. Visualización de datos

Se analizaron distribuciones con `sns.histplot()` y una matriz de correlación con `sns.heatmap()`.

**¿Qué correlación existe entre la variable x y el hue (clase)?**
Las variables como flavonoides y color_intensity muestran diferencias notables entre clases, por lo que tienen alta correlación con la etiqueta objetivo.

**¿Qué variables parecen más útiles para diferenciar clases?**

* alcohol
* flavonoides
* proline
* color_intensity

Estas tienen mayor separación visual entre las clases.

---

## 4. Segmentación de datos

El dataset se divide en:

* 70% entrenamiento (train)
* 15% validación (validation)
* 15% prueba (test)

Esto se logra dividiendo primero en 70% y 30%, y luego dividiendo el 30% restante por la mitad.

**Roles de cada conjunto:**

* Entrenamiento: para ajustar los parámetros del modelo.
* Validación: para ajustar hiperparámetros y evitar sobreajuste.
* Prueba: para medir el rendimiento final del modelo con datos nuevos.

**¿Qué implica evaluar primero en prueba y luego en validación?**
Se estaría usando información del futuro, contaminando el proceso de validación. Siempre se valida antes de probar.

---

## 5. Entrenamiento del modelo

Modelo base:

```python
model = DecisionTreeClassifier(criterion="gini", max_depth=3, random_state=0)
```

**Preguntas:**

**¿Qué significa criterion="gini"?**
Es una métrica de impureza que mide qué tan mezcladas están las clases en un nodo. Cuanto menor el Gini, más puro el nodo.

**¿Qué pasa si cambiamos max_depth?**
A mayor profundidad, el árbol aprende más detalles, pero puede sobreajustarse (overfitting).
A menor profundidad, el modelo puede subajustarse (underfitting).

**¿Qué variable aparece en la raíz del árbol?**
Generalmente flavonoides o proline, ya que son las de mayor poder discriminante.

**¿Por qué esa variable fue elegida?**
Porque ofrece la mayor ganancia de información al dividir el dataset en esa característica.

**¿Cómo se interpretan las reglas del árbol?**
Cada nodo representa una decisión condicional, y cada rama lleva a una predicción final.
Por ejemplo: Si flavonoides > 2.0 → clase 0, si no → seguir otra condición.

---

## 6. Evaluación en validación

Se predicen las etiquetas del conjunto de validación.
Se calcula el accuracy y la matriz de confusión.

**Preguntas:**

**¿Qué relación tiene el accuracy de entrenamiento y validación con el riesgo de overfitting?**
Si el accuracy de entrenamiento es mucho mayor que el de validación, el modelo memorizó los datos (overfitting).
Si son similares, el modelo generaliza bien.

**¿Cómo se interpretan los resultados en la matriz de confusión?**
Muestra cuántos ejemplos de cada clase fueron correctamente o incorrectamente clasificados.
Los valores fuera de la diagonal indican errores.

---

## 7. Evaluación en prueba

Se repite el proceso con el conjunto de prueba para obtener el rendimiento final.

**Preguntas:**

**¿Qué aporta la matriz de confusión frente al accuracy?**
El accuracy solo da un número global, mientras la matriz de confusión muestra qué clases se confunden más.

**¿Qué se puede concluir?**
El modelo logra alta precisión (alrededor de 97 a 99 por ciento), por lo que separa bien las clases de vino según sus características químicas.

---

## 8. Reto y mejoras aplicadas

Se implementó una búsqueda de hiperparámetros (GridSearchCV) para optimizar:

* max_depth
* criterion (gini, entropy)
* min_samples_split

Mejoras observadas:

* El accuracy aumentó hasta aproximadamente 97–99 por ciento.
* Se redujo la profundidad del árbol y se evitó el sobreajuste.
* Se guardaron los resultados (reporte de clasificación y matriz de confusión) en un archivo Excel:
  `/mnt/data/resultados_arbol_vinos.xlsx`

---

## 9. Conclusiones finales

* Los árboles de decisión son modelos intuitivos, explicables y potentes para clasificación supervisada.
* El dataset de vinos permite ver claramente la relación entre variables químicas y el tipo de vino.
* Con una correcta selección de hiperparámetros, el modelo logra alta precisión y generalización.
* La interpretación de reglas facilita la explicabilidad del modelo, un punto clave en la analítica de datos.

