# Dermis_clasification

## Descripcion
Comparación de diferentes modelos de clasificación para distinguir 23 enfermedades de la piel, se entrenaron los modelos con más de 23000 imágenes después de haber hecho alimentación de datos para equilibrar las clases.

## Fuentes
El Dataset fue tomado de kaggle `https://www.kaggle.com/datasets/shubhamgoel27/dermnet/data` cuenta en total con 19500 imagenes de 23 diferentes afecciones de la piel, estas clases no estan distribuidas de manera uniforme. El dataset esta previamente dividido en conjunto de entrenamiento y  conjunto de prueba, las imagenes son de diferentes tamaños y fueron tomadas con diferentes camaras,iluminaciones y fondos.

Tome inspiracion de notebooks que la comunidad de kaggle comparte de este dataset, principalmente de `https://www.kaggle.com/code/sarawarhossain/explore-augmentation` que utiliza aumentacion de datos para combatir el desbalance de clases, en notebook, ademas, se utilizan transformadores para la clasificacion cosa que yo no implemente, sin embargo los resultados obtenidos no difieren por mucho de los que se lograron en este proyecto con algunos modelos.

Las herramientas principales de este proyecto fueren Pytorch y tensoflow 

## Desarrollo

### 1.- preparacion de datos: 

primero se hizo el analisis de los datos para ver su distribucion, dado que habia mucha diferencia entre la clase con mas imagenes y la clase con menos imagenes opte por hacer un aumento de datos a un punto "medio".

Tambien se normalizo el tamaño de las imageenes y el rango de los pixeles.

### 2.-Modelo base line

Para tener un punto mininmo de comparacion, que no fuese el azar, decidi utilizar **AlexNet** como base-line. Dada la gran cantidad de imagenes el entrenamiento duro mucho tiempo (aproximadamente dos horas) lo cual limitó la exploracion de diferentes configuraciones de parametros , ya que, sumado a esto google colab tiene restricciones sobre el uso de sus gpus, dichas se remueven pasadas mas de 12 horas.

El modelo presento un rendimiento regular en el conjunto de dev y train, pero en el conjunto de prueba resulto deficiente. Este modelo fue hecho con tensorflow

### 3.- Transfer learning en Alex Net

Como parte de experimentar con alexnet Probe hacer transfer learning la manera mas facil era mediante pytorch, padeio de los mismos problemas de tiempo que el anterior modelo por lo que las limitaciones fueron las mismas.

El accuracy en los conjuntos train y dev fue levemente peor que en el modelo anterio, sin embargo, dio mejor desempeño en el conjunto de prueba

### 4.- Vgg16

Dado que se noto un mejor performance con transfer learning en el modelo anterior, opte por hacer transfer learning con un modelo mas profundo como lo es vgg16. Este mostro un desmepeño mejor en las primeras epocas pero se quedo estancado muy rapido, esto dio pie a que pudiera experimentar con mas parametros como el learning rate, el decaimiento de este mismo pero no era un cambio sustancial. Apesar de solo correr en pocas epocas mostro un mejor desempeño que los modelos anteriores en todos los datasets

### 5.- ResNet

Con la idea de que modelos mas profundos presentan mejores resultados, decidi utilizar resnet101 sin embargo el poder de computo me limitaba nuevamente a causa de esto utilize una version mas ligera de esta familia de arquitecturas la resnet 50.

Esta red Fue la mejor de todas en cada conjunto presentado.

| Métrica | **ResNet-50 (Transfer Learning)** | **VGG-16 (Transfer Learning)** | **AlexNet (Transfer Learning)** | **AlexNet (Desde Cero / Scratch)** |
| :--- | :--- | :--- | :--- | :--- |
| **Epochs Entrenados** | 35 | 9 | 50 | 31 |
| **Mejor Accuracy (Train)** | **97.50%** | 73.35% | 50.17% | 51.55% |
| **Mejor Accuracy (Val)** | **59.67%** | **43.80%** | **41.45%** |  **34.76%** |
| **Gap (Overfitting)** | ~38% (Extremo) | ~30% (Alto) | ~8% (Bajo) | ~17% (Medio) |
| **Mejor Loss (Train)** | 0.0649 | 1.1393 | 1.6273 | 1.3487 |
| **Velocidad de Convergencia** | Rápida | **Explosiva** | Lenta | Muy Lenta e Inestable |
| **Conclusión** | Potente pero memoriza demasiado | **Balance Ideal Tiempo/Resultados** | Estable pero limitada | **Insuficiente (Necesita pesos pre-entrenados)** |

