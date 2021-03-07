Лабораторная работа №1
====================
Подготовка окружения для решения задачи классификации изображений из набора данных Oregon Wildlife с использованием нейронных сетей глубокого обучения
---------
### При выполнении лабораторной работы №1, по предмету "Современные методы обработки информации", ставилась задача классифицировать набор данных Oregon-Wildlife, состоящий из 14.000 изображений, по 20 различным категориям.
### Для выполнения поставленной задачи мы обучали сверточные нейронные сети, прокрутив весь набор данных 50 раз, так как количество эпох = 50. При этом каждая эпоха делится на равные части, по размеру batch_size, в моем случае batch_size=64.

1.Описание архитектуры нейронной сети:
-----------
* Параметр для задания размерности входного изображения (224х224х3)

```inputs = tf.keras.Input(shape=(RESIZE_TO, RESIZE_TO, 3))```
*  Convolutional layer - сверточный 2D-слой, размера 222х222х8, создающий ядро свертки. В качестве параметров задаются количество выходных фильтров = 8 и размерность матрицы ядра 3х3. 
```x = tf.keras.layers.Conv2D(filters=8, kernel_size=3)(inputs)```
