# Filtro entrenado con set de emociones


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

