Proyecto TensorFlow.js
=====================

Este repositorio contiene una página HTML que entrena un modelo secuencial en TensorFlow.js para aproximar la función **y = 2x + 6**, y permite al usuario predecir el valor de y para cualquier x introducido.

Índice
------

1. Instalación
2. Uso
3. Explicación del código
   - Definición del modelo
   - Preparación de datos
   - Entrenamiento (épocas)
   - Predicción

Instalación
-----------

1. Clona o descarga este repositorio.
2. Asegúrate de que el nombre del archivo sea `README.md` para que GitHub lo renderice correctamente.
3. Abre `index.html` en un navegador moderno.

Uso
---

1. Haz clic en **Entrenar Modelo**.
2. Espera hasta que aparezca: **Entrenamiento finalizado. El modelo está listo para ser usado.**
3. Introduce un valor de **X** en el campo de texto.
4. Haz clic en **Predecir** y observa el resultado de **Y**.

Explicación del código
----------------------

Definición del modelo
~~~~~~~~~~~~~~~~~~~~~~

```js
const modelo = tf.sequential();
modelo.add(
  tf.layers.dense({
    units: 1,       // una neurona de salida
    inputShape: [1] // cada entrada es un valor x
  })
);
modelo.compile({
  loss: 'meanSquaredError',
  optimizer: 'sgd'
});
```

Preparación de datos
~~~~~~~~~~~~~~~~~~~~~

Se crean tensores con 9 muestras desde x = -6 hasta x = 2:

```js
const xs = tf.tensor1d([-6, -5, -4, -3, -2, -1, 0, 1, 2]); // forma [9]
const ys = tf.tensor1d([-6, -4, -2, 0, 2, 4, 6, 8, 10]);    // forma [9]
```

Para predecir, convertimos el valor de entrada a tensor 2D:

```js
const inputTensor = tf.tensor2d([valorX], [1, 1]); // forma [1, 1]
```

Entrenamiento (épocas)
~~~~~~~~~~~~~~~~~~~~~~

- Una **época** es una pasada completa por todas las muestras.
- Se entrenan **350 épocas** para que el modelo aprenda la relación.

```js
await modelo.fit(xs, ys, {
  epochs: 350,
  callbacks: {
    onTrainEnd: () => {
      // Mensaje de modelo listo
    }
  }
});
```

Predicción
~~~~~~~~~~

1. Verificar que el modelo esté entrenado.
2. Leer el valor de x del formulario.
3. Ejecutar:

```js
const resultadoTensor = modelo.predict(inputTensor);
resultadoTensor.array().then(array => {
  console.log(array[0][0]);
});
```

4. Mostrar el resultado en pantalla.

---
