# Proyecto: TensorFlow.js

Este proyecto muestra cómo crear una página HTML que entrena un modelo secuencial con TensorFlow.js para la fórmula y = 2x + 6 y luego permite al usuario introducir un valor de x para predecir y.

---

## Contenido

- Instalación
- Uso
- Cómo funciona
  - Definición del modelo
  - Datos y tensores
  - Entrenamiento y épocas
  - Predicción

---

## Instalación

1. Clona o descarga este repositorio.
2. Abre `index.html` en tu navegador.

## Uso

1. Haz clic en **Entrenar Modelo**.
2. Espera el mensaje **"Entrenamiento finalizado. El modelo está listo para ser usado."**
3. Ingresa un número en el campo de texto (valor de X).
4. Pulsa **Predecir** y observa el resultado.

## Cómo funciona

### Definición del modelo

Se crea un modelo secuencial de una sola capa densa:

```js
const modelo = tf.sequential();
modelo.add(
  tf.layers.dense({ units: 1, inputShape: [1] })
);
```

- `units: 1`: una neurona de salida.
- `inputShape: [1]`: cada entrada es un solo valor (x).

### Datos y tensores

Los datos de entrenamiento son pares (x, y) donde y = 2x + 6. Se usan 9 muestras desde x = -6 hasta x = 2.

```js
const xs = tf.tensor1d([-6, -5, -4, -3, -2, -1, 0, 1, 2]);
const ys = tf.tensor1d([-6, -4, -2, 0, 2, 4, 6, 8, 10]);
```

- `xs` tiene forma [9].
- `ys` tiene forma [9].

Para predecir, se crea un tensor 2D con forma [1, 1]:

```js
const inputTensor = tf.tensor2d([valorX], [1, 1]);
```

### Entrenamiento y épocas

Se compila el modelo con minimización de error cuadrático medio y optimizador SGD:

```js
modelo.compile({
  loss: 'meanSquaredError',
  optimizer: 'sgd'
});
```

- Una **época** es una pasada completa por todas las muestras.
- Se entrenan **350 épocas**, es decir, el modelo ve las 9 muestras 350 veces.
- Al finalizar, el callback `onTrainEnd` muestra el mensaje de modelo listo.

### Predicción

1. Verifica que el modelo haya sido entrenado.
2. Lee el valor de x del formulario.
3. Llama a `modelo.predict(inputTensor)`.
4. Convierte el tensor resultante a arreglo y muestra el valor de y.

---
