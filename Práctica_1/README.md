# Práctica 1


Esta es la primera práctica, son una serie de ejercicios simples. Usamos **Python** e importamos las librerias de **OpenCV** , de **matplotlib.pyplot**  y de **NumPy** como **np**.


## Contenido
1. **Generamos una imagen con la textura de un tablero de ajedrez**. \
Un tablero de 800x800 en los cuales se recorre pintando las pares y luego desplazando con un *buffer* la salida para volver a pintarlos.

2. **Creamos una imagen al estilo Mondrian**. \
Usamos OpenCV para generar formas geométricas y lineas para realizar la imagen y las mostramos con las funciones de openCV.

3. **Procesamos el video capturado por la camara y modificamos la salida**.\
Aprovechamos para mostrar nuestras dos versiones de modificar realizando un *collage* de ambas. En una de ellas le pasamos un tinte a la camara de forma que todo esta coloreado por un color de forma aleatoria cambiando cada poco tiempo. En la otra, tenemos la capa de grises *Green*, en el cual modificamos un valor a uno distinto y se refleja en la imagen al saturarse.

4. **Detectar el pixel más claro y el más oscuro en la imagen**. \
Lanzamos la cámara y capturamos un fotograma, este lo convertimos a escala de grises y situamos los máximos y mínimos, guardando su posición y valor en una variable. Procedemos a pintarlos usando OpenCV y mostrarlo en un *plot*.

5. **Pop art con la cámara**. \
Generamos un 4x4 en los cuales, excetuando el primero, variamos sus colores para asemejar el efecto *Pop art*. Tras cerrar la cámara, se genera un *plot* con la última imagen con este efecto.

## Requisitos
- Python , versión 3.11.5
- OpenCV
- MatPlotLib
- Numpy

## Autores
@Yaivi  
@AlfreVR




