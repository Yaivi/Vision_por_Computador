# Parte_A

Para la realización de la mitad de este trabajo a través del entrenamiento de un modelo de YOLO con Ultralytics hemos tomado los siguientes pasos:

## Creación del dataset
Para la creación del dataset, nos pusimos de acuerdo con un grupo de compañeros en sacar fotos de las matrículas y compartirlas entre todos para tener un buen conjunto de imágenes. Una vez se tenían las fotos sacadas se obtenía anotaba el rectángulo en el que se encontraba la matrícula usando la herramienta de anotación _labelme_. Con esto obteníamos el archivo json, con la clase 'plate' asociada a estos rectángulos. Para los archivos txt que contengan los valores <object-class-id> <x> <y> <width> <height>, hicimos uso de una función que recorriese la carpeta con los json y obtuviese los datos de los rectángulos anotados y que los transformase en sus equivalentes en txt, el número de la clase, y los datos de posición con sus valores entre 0 y 1. 

## Organización de las imágenes
Una vez todo esto estuviese listo, se procede a crear la estrcutura de entrenamiento a partir de las imágenes del dataset. Para ello se pone la ubicación de la carpeta del dataset y se especifica también la carpeta de los json o label, dependiendo de con que se vaya a realizar luego el entrenamiento. A continuación se hace una lista con todas las imágenes que se encuentren en la carpeta y luego se mezclan aleatoriamente, según el número de imágenes se pone hasta que imagen va en cada parte del conjunto de datos TGCRBNW. Una vez se tienen esos valores se crean si no están las carpetas de train, val y test para poder repartir las imágenes.

Con la estructura ya creada se revisa si las imágenes tienen su txt correspondiente, para que en caso de que la imagen no tenga su archivo con los datos para entrenarlo no se añada al dataset.

## Entrenamiento del modelo
Para entrenar el modelo de YOLO se ha utilizado CUDA para poder hacer uso de la GPU, por lo que lo primero que se hace es comprobar que nuestro enviroment tiene disponible el CUDA, y se revisan las versiones de PyTorch y de la versión CUDA compilada para que en caso de no tener disponible esta última se pueda revisar si el problema viene a causa de incompatibilidades entre las versiones. Cuando _torch.cuda.is_available()_ devuelva _True_, significa que se puede hacer uso de la GPU. 

```
print("PyTorch version:", torch.__version__)
print("CUDA available:", torch.cuda.is_available())
print("CUDA version (compiled):", torch.version.cuda)
print("GPU name:", torch.cuda.get_device_name(0) if torch.cuda.is_available() else "No GPU detected")

model = YOLO('yolo11n.pt')  

model.train(
    data=r"data.yaml",  # ruta a tu YAML
    epochs=50,       # número de épocas
    imgsz=720,        # tamaño de imagen
    batch=16,         # tamaño de lote
    name="matricula_yolo11n",  # nombre de la carpeta de resultados
    workers=1,        
    device=0          # GPU (usa "cpu" si no tienes CUDA)
)
```

Para el entrenamiento del modelo hemos ido probando distintas opciones los parámetros, cambiando principalmente los epochs, el tamaño de imagen y el tamaño de lote. Finalmente nos quedamos en los valores aquí presentes, destacando el tamaño de imagen de 720 pues aumentarlo más causaba problemas en la memoria GPU, estropeando la ejecución del entrenamiento. 

El archivo data.yaml, presenta el siguiente formato:
```
# rutas del dataset
#ACTUALIZAR A RUTAS ABSOLUTAS PARA PODER USARLO SIN FALLOS EL ENTRENAMIENTO
train: C:\Users\javie\Documents\TRABAJO UNI\VC\Vision_por_Computador\Práctica_4\Parte_A\TGC_RBNW\train\images
val: C:\Users\javie\Documents\TRABAJO UNI\VC\Vision_por_Computador\Práctica_4\Parte_A\TGC_RBNW\val\images
test: C:\Users\javie\Documents\TRABAJO UNI\VC\Vision_por_Computador\Práctica_4\Parte_A\TGC_RBNW\test\images

# número de clases
nc: 1

# nombres de las clases
names: [ 'plate']
```
En este archivo se especifican con que propósito se van a usar las diferentes carpetas durante el entrenamiento. Además se añade el número de clases y el nombre de estas para que el modelo asocie lo que aprenda de las imágenes y los labels.

## Detección usando vídeo de ejemplo
Para la detección de las matrículas se cargan 2 modelos, el modelo de YOLO yolo11n.pt que es el capaz de detectar personas y vehículos como se especifíca en la lista de clases, y en cuanto se detecte un coche se hará un recorte de este y se aplicará el modelo que se ha entrenado para intentar detectar la matrícula. Tras cargar ambos modelos se hace lo mismo con el vídeo de ejemplo, y se guarda la dirección del vídeo resultante junto con los parámetros que tendrá el vídeo de salida. También se preparan las columnas que trendrá el archivo csv cuando se vaya a crear. 

Con esto ya hecho se comienza con la detección frame por frame del vídeo, pasándole el modelo yolo11n.pt para detectar las personas y coches, en caso de que en el frame se detecte algo de esto se procede a crear un rectángulo con cv2 para marcarlo, azul para coches y verde para personas, poniendo también el grado de confianza que se tiene con esta detección y la clase que se ha predicho. En caso de detectar el coche se hace el recorte antes mencionado, se comprueba que el recorte sea válido y se le pasa el modelo que se ha entrenado con el dataset de matrículas. Del resultado de la predicción del segundo modelo también se obtienen la confianza de detección y las coordenadas del bounding box, con esto creamos un rectángulo amarillo sobre el que escribimos la confianza de la detección y la clase que ha detectado.

Por cada frame se crea una línea del csv que contiene:
```
fotograma, tipo_objeto, confianza, identificador_tracking, x1, y1, x2, y2, matrícula_en_su_caso, confianza, mx1,my1,mx2,my2, texto_matricula
```

# Parte_B

## Tesseract

## EasyOCR

## PaddleOCR

## VLM
