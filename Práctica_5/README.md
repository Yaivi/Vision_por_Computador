# Filtro entrenado con set de emociones
Se ha escogido un dataset de expresiones faciales en Kaggle, el FER-2013, que contiene expresiones faciales de 7 emociones diferentes, disgusto, miedo, enfado, alegría, tristeza, sorpresa y neutral. En total posee 28000 imagenes ya dividas en carpetas de train y test.

**Enlace al dataset:** https://www.kaggle.com/datasets/msambare/fer2013

Debido a la extrema similitud entre las expresiones de miedo y sorpresa, hemos decidio eliminar la categoría de sorpresa, y también se ha eliminado la etiqueta de disgusto, al tener muy pocas imágenes con las que contribuir al entrenamiento. Estas decisiones se han tomado con el objetivo de hacer más robusto y preciso al modelo.

## Carga de datos
Para cargar las imágenes del dataset se usado de base el código proporcionado por el prosefor, convertido a una función para poder usarlo con las carpetas de imágenes que ya vienen con el dataset. Además dentro de la función se le realiza un resize a las imágenes para ponerlas en formato 64x64. Con todo esto se consigue cargar en variables las imágenes, las etiquetas de cada una de las imágenes, todas las etiquetas que tiene el dataset y el número de etiquetas distintas que hay. También se mantiene del código original la muestra de la primera imagen de cada etiqueta que se cargue.

## Análisis y visualización preliminar
Este bloque de código realiza un análisis de componentes principales (PCA) sobre las imágenes de entrenamiento con el objetivo de extraer y visualizar los eigenfaces, que representan las direcciones de mayor variabilidad en el conjunto de caras. Esto es directamente lo mismo que viene en el código del profesor sin ningún cambio.

## Entrenamiento del modelo
Tras este análisis preliminar, se comienza con el entrenamiento real del modelo. Para ello, se aplica primero una reducción de dimensionalidad mediante PCA, utilizando 150 componentes principales y el método de descomposición randomized, que permite acelerar el cálculo en espacios de alta dimensión. El PCA se ajusta sobre las imágenes de entrenamiento y posteriormente se utiliza para transformar tanto el conjunto de entrenamiento como el de prueba al nuevo espacio reducido. Esta reducción permite conservar la mayor parte de la variabilidad relevante de los datos a la vez que disminuye significativamente la complejidad computacional del clasificador.

A continuación, las características transformadas se normalizan mediante un MinMaxScaler, que ajusta cada componente a un rango entre 0 y 1. Esta etapa es necesaria para que el clasificador SVM funcione adecuadamente, ya que su rendimiento depende de que las características estén correctamente escaladas.

Para el proceso de clasificación se emplea un SVM con kernel RBF, una de las configuraciones más eficaces para problemas de clasificación no lineal. Con el objetivo de encontrar los hiperparámetros óptimos, se utiliza una búsqueda exhaustiva mediante GridSearchCV, evaluando diferentes combinaciones de los parámetros C y gamma, que controlan respectivamente la flexibilidad del margen y la influencia de cada punto de entrenamiento. La búsqueda se realiza con una validación cruzada de 5 particiones (cv=5) y utilizando todos los núcleos disponibles de la CPU (n_jobs=-1) para acelerar el proceso.

Una vez completada la búsqueda, se entrena el modelo SVM con la mejor combinación de parámetros encontrada y se almacena junto con el PCA y el escalador mediante la librería joblib. Esto permite conservar el pipeline completo del modelo para su posterior uso en fase de inferencia sin necesidad de repetir el proceso de entrenamiento.

## Prueba del modelo
Una vez entrenado el sistema completo, se procede a evaluar su rendimiento sobre el conjunto de test. Para ello se carga el modelo previamente guardado en un único archivo mediante joblib, que contiene el PCA, el escalador y el clasificador SVM entrenado. Esto permite reproducir exactamente el mismo pipeline aplicado durante el entrenamiento sin necesidad de volver a recalcular ningún componente.

## Aplicación del modelo para detección en tiempo real y uso de filtros
El código detecta la cara en tiempo real, predice la emoción usando un modelo SVM, y coloca un filtro gráfico en diferentes zonas de la cara según la emoción, ajustando tamaño, posición y orientación automáticamente.

![Gif de uso del modelo entrenado, transicionando entre emociones](https://github.com/Yaivi/Vision_por_Computador/blob/main/Pr%C3%A1ctica_5/Emociones.gif?raw=true)


# Filtro libre elección
Para este filtro decidimos sentir la magia y hacer el truco de sacar las cartas por la boca, junto con un sombrero de copa para sentirnos que somo magos.

Definimos una función para superponer una imagen con transparencia en una posición en concreto con un valor alpha adaptable. 

También definimos otra función para rotar una imagen

Para el modelo usamos MediaPipe para detectar el rostro.

Primero tratamos de cargar el gorro y luego empezamos a cargar la cámara. 

Le decimos que solo hay una cara en los parametros de la función **mp_face_mesh.FaceMesh**, con una detección de al menos 0.5 de confianza. 

Pasamos a inicializar los valores para hacer la animacion de las cartas, le pasamos una carpeta ordenada con los ficheros en ".png".

Ahora continuamos al bucle donde miramos si se sigue capturando los frames para la cámara. Comprobamos si hay una cara y si hay entonces enganchamos el gorro a los puntos que están en la frente y en los lados de la cabeza, con esos datos establecemos como debería quedar el sombrero y lo colocamos.

En cuanto a los labios, seleccionamos tanto el labio superior y el inferior como la frente y la barbilla, para saber la altura de la cabeza. Cuando la distancia entre los labios sea mayor que el mínimo indicado empezamos con la animación, luego redimensionamos y rotamos dependiendo del ángulo de la cabeza calculada previamente. Después superponemos con la función que definimos al principio.

