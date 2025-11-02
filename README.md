# Proyecto de Reconocimiento de Señas en Tiempo Real

Este es un proyecto de Visión por Computadora capaz de reconocer las letras del abecedario (Lengua de Señas) en tiempo real utilizando una cámara web.

El sistema funciona extrayendo los 21 puntos clave (landmarks) de la mano detectada usando la biblioteca **Mediapipe** de Google. Estos puntos se utilizan luego para entrenar un modelo de Machine Learning (`RandomForestClassifier` de Scikit-learn) que clasifica la seña correspondiente.

## Tecnologías Utilizadas

* **OpenCV (`opencv-python`)**: Para la captura y procesamiento de video en tiempo real.
* **Mediapipe**: Para la detección y seguimiento de alta fidelidad de los puntos clave de la mano.
* **Scikit-learn (`sklearn`)**: Para entrenar el modelo clasificador de Machine Learning.
* **Numpy**: Para el manejo eficiente de los arreglos de datos.
* **Pickle**: Para guardar y cargar el modelo entrenado y el dataset procesado.

## Instalación

1.  Clona este repositorio (o descarga los archivos).
2.  Asegúrate de tener Python 3.x instalado.
3.  Instala las dependencias necesarias:

```bash
pip install opencv-python mediapipe scikit-learn numpy
```

## Modo de Uso

El proyecto está dividido en 4 pasos principales. Debes ejecutarlos en orden.

### Paso 1: Recolectar Imágenes

Este paso te permite grabar tus propias imágenes de señas para crear el dataset.

1.  Modifica el script `collect_imgs.py` y ajusta la variable `number_of_classes` al número de señas que deseas grabar (ej: `26` para el abecedario).
2.  Ejecuta el script:

    ```bash
    python collect_imgs.py
    ```
3.  El script creará carpetas `data/0`, `data/1`, `data/2`, etc., una para cada clase.
4.  Cuando estés listo, presiona 'Q' para comenzar a grabar las `100` muestras para esa clase. Repite el proceso para todas las clases.

**Nota:** Deberás llevar un registro de qué número de carpeta corresponde a qué letra (ej: `0 = 'A'`, `1 = 'B'`, etc.).

### Paso 2: Crear el Dataset Procesado

Este script toma todas las imágenes de la carpeta `data/`, extrae los puntos clave de la mano de cada una usando Mediapipe y los guarda en un único archivo.

```bash
python create_dataset.py
```

Esto generará un archivo llamado `data.pickle` que contiene todos los datos de entrenamiento listos para el modelo.

### Paso 3: Entrenar el Modelo

Este script carga el archivo `data.pickle` y entrena un modelo `RandomForestClassifier` con él.

```bash
python train_classifier.py
```

Al finalizar, imprimirá la precisión (accuracy) del modelo en una porción de los datos de prueba y guardará el modelo entrenado como `model.p`.

### Paso 4: ¡Ejecutar el Reconocimiento!

Este es el script principal que ejecuta la aplicación en tiempo real.

1.  Asegúrate de modificar el diccionario `labels_dict` en `inference_classifier.py` para que coincida con las clases que grabaste (ej: `{0: 'A', 1: 'B', ...}`).
2.  Ejecuta el script:

    ```bash
    python inference_classifier.py
    ```
3.  ¡Muestra tus señas a la cámara! El programa dibujará los puntos en tu mano y predecirá la letra correspondiente.
4.  Presiona la tecla **'q'** para salir del programa.

## Archivos del Proyecto

* `collect_imgs.py`: Script para capturar y guardar las imágenes de entrenamiento.
* `create_dataset.py`: Script para procesar las imágenes y crear el archivo `data.pickle`.
* `train_classifier.py`: Script para entrenar el modelo y crear el archivo `model.p`.
* `inference_classifier.py`: Script principal que ejecuta el reconocimiento en tiempo real.
* `data/`: Carpeta (creada por `collect_imgs.py`) que contiene las imágenes de entrenamiento.
* `data.pickle`: Archivo de datos procesados (generado por `create_dataset.py`).
* `model.p`: Modelo de clasificación entrenado (generado por `train_classifier.py`).
