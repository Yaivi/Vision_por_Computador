# Práctica 3
## Tarea 1
TAREA: Los ejemplos ilustrativos anteriores permiten saber el número de monedas presentes en la imagen. ¿Cómo saber la cantidad de dinero presente en ella? Sugerimos identificar de forma interactiva (por ejemplo haciendo clic en la imagen) una moneda de un valor determinado en la imagen (por ejemplo de 1€). Tras obtener esa información y las dimensiones en milímetros de las distintas monedas, realiza una propuesta para estimar la cantidad de dinero en la imagen. Muestra la cuenta de monedas y dinero sobre la imagen. No hay restricciones sobre utilizar medidas geométricas o de color.

Una vez resuelto el reto con la imagen ideal proporcionada, captura una o varias imágenes con monedas. Aplica el mismo esquema, tras identificar la moneda del valor determinado, calcula el dinero presente en la imagen. ¿Funciona correctamente? ¿Se observan problemas?

Nota: Para establecer la correspondencia entre píxeles y milímetros, comentar que la moneda de un euro tiene un diámetro de 23.25 mm. la de 50 céntimos de 24.35, la de 20 céntimos de 22.25, etc.

Extras: Considerar que la imagen pueda contener objetos que no son monedas y/o haya solape entre las monedas. Demo en vivo.

### Respuesta Tarea 1

Empezamos pasandole 2 listas y un diccionario, siendo los tipos de moneda, sus proporciones cogiendo un euro como base y en el diccionario va la relación entre la moneda con su valor.

Pasamos a realizar algunas funciones que nos ayudaran con los calculos. Ente ellas esta la función de **lista_proporcional(tipo)**, la cual sirve para normalizar las proporciones al tomar una moneda de referencia. La siguiente funcion es **hallar_valor(diametro_ref, diametro_nuevo, proporciones)** para determinar el tipo de moneda más cercano según la proporción de diámetros. La última de las funciones de apoyo es **calcular_diametro(cnt)** y devuelve el diámetro del círculo mínimo que contiene el contorno.

Procedemos a cargar la imagen y limpiarla. Usamos desenfoque gaussiano en la imagen en escala de grises y la pasamos a umbralizar usando Otsu. Pasamos a una limpieza morfológica para reducir ruido y unificar border usando funciones de cv2 como **getStructuringElement** y **morphologyEx**.
Ahora introducimos la detección de contornos y las filtramos las que tengan un contorno de un área mínimo y máximo. También si hay contornos muy cercanos los agrupamos para evitar duplicados de monedas.

Añadimos una copia de la imagen original para que el usuario interactue, establecemos un orden de las monedas y las enumeramos.
![Monedas enumeradas](image.png)
Al poner una imagen para que el usuario actue necesitamos hacer un evento que lea el clic del usuario. Creamos una función en la cual si lee un clic en una de las monedas aparece una pestaña en la que el usuario indica el tipo de moneda segun la lista inicial: ["2e", "1e", "50c", "20c", "10c", "5c", "2c", "1c"]. Una vez introducimos el valor correcto, se quitan las numeraciones y se escriben los valores de las monedas.
Es en este momento donde se calcula el valor total de las monedas en la imagen.
![Monedas con su valor](image-1.png)

Al terminar la ejecución se indica las monedas identificadas por separado.
![Output](image-2.png)


## Tarea 2
TAREA: La tarea consiste en extraer características (geométricas y/o visuales) de las tres imágenes completas de partida, y aprender patrones que permitan identificar las partículas en nuevas imágenes. Para ello se proporciona como imagen de test MPs_test.jpg y sus correpondientes anotaciones MPs_test_bbs.csv con la que deben obtener las métricas para su propuesta de clasificación de microplásticos, además de la matriz de confusión. La matriz de confusión permitirá mostrar para cada clase el número de muestras que se clasifican correctamente de dicha clase, y el número de muestras que se clasifican incorrectamente como perteneciente a una de las otras dos clases

### Respuesta Tarea 2

