# teachebleMachine
Estas son como las referencias que use principalmente:<br/>
Este es un repo que tiene un ejemplo sencillo y mas ordenado:<br/>
https://github.com/amitshekhariitbhu/Android-TensorFlow-Lite-Example/blob/master/app/src/main/java/com/amitshekhar/tflite/Classifier.java<br/>
Aca hay otro repo que tiene muchos ejemplos de como usar el tensorflowLite:<br/>
https://github.com/techzizou/examples/blob/master/lite/examples/image_classification/android/app/src/main/java/org/tensorflow/lite/examples/classification/ClassifierActivity.java<br/>
Este es un video mas paso a paso, solo que usan kotlin:<br/>
https://youtu.be/13TdOYgw2Fc?si=sTZAtRTyoJna8o1T<br/>
Este esta en ingles pero tambien explica bien como se puede integrar:<br/>
https://youtu.be/jhGm4KDafKU?si=7PhRMG7y4k_rwnGC<br/>
Y esta es una documentacion de firebase que tiene los codigos para interpretar las imagenes:<br/>
https://firebase.google.com/docs/ml/android/use-custom-models?hl=es-419#java_1<br/>

Ahora asi lo implemente yo:<br/>
Primero se descarga el archivo en Teacheable machine:<br/>
![image](https://github.com/user-attachments/assets/d3a414c3-45f3-483b-95ef-c153e4e4f14d)<br/>
El archivo es un .tflite, ese se copia en la carpeta assets del proyecto app/src/main/assets.<br/>
![image](https://github.com/user-attachments/assets/e923f198-8408-4a4d-9488-71fb9a52b1f9)<br/>
Luego en el build gradle a nivel de app se tiene que agragar la dependencia de tensorflow:<br/>
implementation("org.tensorflow:tensorflow-lite:2.9.0")<br/>
Hay otra forma que ya te lo agrega android studio las dependencias que en los videos explican, pero pues o lo puse manual :(.<br/>
Bueno ya en android studio toca crear una clase classifier para poder cargar el modelo:<br/>
![image](https://github.com/user-attachments/assets/080c785b-a8c7-4861-85b9-99ce23c55b20)<br/>
Luego en la clase principal se debe tener un metodo para capturar o subir las imagenes:<br/>
En mi codigo como es una version vieja esta desordenado jajaja pero los metodos estan en la clase PlantActivity, son estos:<br/>
showImageSourceDialog() este va a preguntar si se quiere subir una foto o tomarla directamente<br/>
![image](https://github.com/user-attachments/assets/85663220-498a-4735-af50-ab29a1eeac2f)<br/>
openCamera() si se elije abrir la camara se usa este metodo, este abre la cámara del dispositivo para tomar una foto, guarda la imagen en un archivo temporal y usa FileProvider para obtener la URI segura del archivo.<br/>
![image](https://github.com/user-attachments/assets/ce6f9e57-e9e6-432e-9607-76bb17542df4)<br/>
openFileChooser() si se elije subir una imagen<br/>
![image](https://github.com/user-attachments/assets/cd48cc03-2204-4db5-b80f-8885486bd5cd)<br/>
onActivityResult(int requestCode, int resultCode, Intent data) maneja el resultado de la selección de imagen o captura con la cámara, si se elige una imagen, llama a recognizePlant(imageUri) para procesarla.<br/>
![image](https://github.com/user-attachments/assets/12a1eb23-ad9a-4a09-94e4-641072f80f82)<br/>
recognizePlant(Uri imageUri) convierte la imagen en un Bitmap, redimensiona la imagen a 224x224 píxeles para el modelo, llama a preprocessImage(Bitmap bitmap) para formatear la imagen en un array, usa el Classifier para predecir la planta en base a la imagen, y muestra el resultado en un Toast, ademas agrega la planta reconocida automáticamente a la base de datos con addRecognizedPlantToDatabase(). En la parte donde esta el nombre de las plantas pues se cambia dependiendo lo que uno este clasificando. <br/>
![image](https://github.com/user-attachments/assets/e2f14aa3-bc1b-4489-8228-14f5860480b2)<br/>
preprocessImage(Bitmap bitmap) convierte la imagen en un array normalizado de píxeles en el rango [0,1], que es el necesario para el modelo de Machine Learning.<br/>
![image](https://github.com/user-attachments/assets/8aedc175-5a0c-4644-bb9b-8d1f451a7125)<br/>
createImageFile() crea un archivo temporal en el almacenamiento para guardar la imagen capturada y devuelve un archivo con un nombre basado en la fecha actual.<br/>
![image](https://github.com/user-attachments/assets/18be2c7c-1f3b-403b-9e5f-41e0d8943f5d)<br/>
addRecognizedPlantToDatabase(String name, String description) agrega una planta reconocida automáticamente a la base de datos de Firebase.<br/>
![image](https://github.com/user-attachments/assets/73c422bd-0532-4ef6-a1fb-902e35f9cf82)<br/>
Listo, esa es la manera en que yo lo integre, es basicamente añadir el modelo a la app y crear una clase classifier para poder cagar el modelo, y para subir las imagenes se pueden crear varios metodos.<br/>
<br/>
