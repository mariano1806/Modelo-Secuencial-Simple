# Proyecto: TensorFlow\.js&#x20;

Este proyecto muestra cómo crear una página HTML que entrena un modelo secuencial con TensorFlow\.js para la fórmula \(y = 2x + 6\), y luego permite al usuario introducir un valor de \(x\) para predecir \(y\).

---

## Contenido

- [Instalación](#instalación)
- [Uso](#uso)
- [Cómo funciona](#cómo-funciona)
  - [Definición del modelo](#definición-del-modelo)
  - [Datos y tensores](#datos-y-tensores)
  - [Entrenamiento y épocas](#entrenamiento-y-épocas)
  - [Predicción](#predicción)

---

## Instalación

1. Clona o descarga este repositorio.
2. Abre `index.html` en tu navegador.

## Uso

1. Haz clic en **Entrenar Modelo**.
2. Espera el mensaje "Entrenamiento finalizado. El modelo está listo para ser usado.".
3. Ingresa un número en el campo de texto (valor de X).
4. Pulsa **Predecir** y observa el resultado.

## Cómo funciona

### Definición del modelo

- Se crea un modelo secuencial de una sola capa densa:
  ```js
  modelo = tf.sequential();
  modelo.add(tf.layers.dense({ units: 1, inputShape: [1] }));
  ```
- `units: 1`: una neurona de salida.
- `inputShape: [1]`: cada entrada es un solo valor (el \(x\)).

### Datos y tensores

- Los datos de entrenamiento son pares \((x, y)\) donde \(y = 2x + 6\).
- Se usan 9 muestras desde \(x = -6\) hasta \(x = 2\).
- `xs = tf.tensor1d([-6, -5, ..., 2])` tiene forma `[9]`.
- `ys = tf.tensor1d([-6, -4, ..., 10])` tiene forma `[9]`.
- Para predecir, se crea un tensor2D con forma `[1, 1]`:
  ```js
  tf.tensor2d([valorX], [1, 1])
  ```

### Entrenamiento y épocas

- Se compila el modelo con:
  ```js
  modelo.compile({ loss: 'meanSquaredError', optimizer: 'sgd' });
  ```
- **Época (epoch)**: una pasada completa por todo el set de datos.
- Entrenamos **350 épocas**, es decir, el modelo ve las 9 muestras 350 veces.
- Al final, el callback `onTrainEnd` muestra que el modelo está listo.

### Predicción

- Verifica que el modelo haya sido entrenado.
- Toma el valor de \(x\) del formulario.
- Llama a `modelo.predict()` y convierte el tensor resultante a un arreglo para mostrar el número.

---
