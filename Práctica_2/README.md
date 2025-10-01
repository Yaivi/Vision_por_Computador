# Práctica 2
## Contenido
### Apartado 1
Para realizar la cuenta de pixeles por filas, proponemos dos maneras de realizarlo, la primera es rotando la imagen y contar columnas. La otra manera es contandolo por filas.
Imprimimos el valor máximo y una lista con los valores de las filas y sus posiciones que sean mayor o igual que el 90% del valor máximo de la fila.
### Apartado 2
Esta tarea consistía en realizar lo mismo que la anterior pero esta vez con Sobel en vez de con Canny, asi que de la misma forma, aplicamos el filtro Sobel sobre la imagen del mandril y convertimos la salida a 8 bits. Luego, realizamos un umbralizado y revisamos los píxeles nulos por filas y columnas, calculando el valor máximo y determinando cuáles superan el 0.9 del máximo. Tras esto, en la imagen con Sobel aplicado le marcamos las filas y columnas en las que se superaba el 0.9 del máximo para que fuesen visible.

Sobre los resultados, lo que podemos observar al comparar los valores de los píxeles, es que para Canny el valor del pixel es superior, pero que igualmente el número de filas que superan el 0.9 del valor máximo es el mismo para ambos filtros de detección de bordes. Aunque si nos fijamos en las filas que forman parte de cada lista comprobaremos que no son las mismas.

### Apartado 3
Para esta parte lo que hemos hecho ha sido crear diferentes funciones que aplican filtros a las imágenes captadas por la cámara. Cuando se inicia el programa lo siguiente y se comienza la captura el usuario puede cambiar el filtro aplicado a a la imagen pulsando las teclas del teclado. La lista de filtros y sus teclas de selección son las siguientes:

- Modo pixelado (tecla A): este modo captura la imagen reduce su tamaño por tanto se baja la resolución y luego esa imagen de menor resolución se vuelve a ensanchar, pero usando una interpolación por el vecino más cercano lo que da como resultado el efecto de pixelado.
- Modo color (tecla B): este modo cambia los colores de la imagen alterando los canales de la imagen para que al rearmar la imagen esta tengo los colores combinados de forma diferente.
- Modo bordes coloreados (tecla K): con este efecto se cogen los bordes y se colorean.
- Modo detector de movimiento (tecla D): se capturan 2 frames y se comparan para ver lo que se mueve, si se detecta movimiento se señala la zona en la que se detectó movimiento con un rectángulo de color.
- Modo base (tecla Q): para volver a la vista de cámara normal


### Apartado 4
Hemos decidido hacer una reinterpretación de la obra de My Little Piece of Privacy de Niklas Roy. En este caso tapamos el contorno de las personas con una "cortina". Esto se hizo separando el fondo, aplicando una capa por encima al contorno descubierto, sobre eta silueta se superpone una rejilla translucida para dar apariencia como la cortina original.


### Controles
Los controles son usados para el apartado 3:
- Modo pixelado (tecla A)
- Modo color (tecla B)
- Modo bordes coloreados (tecla K)
- Modo detector de movimiento (tecla )

### Autores
@Yaivi    (Javier Sánchez Godoy)
@AlfreVR  (Alfredo Villarroya Roldós)