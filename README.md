Лабораторная работа №1
====================
Подготовка окружения для решения задачи классификации изображений из набора данных Oregon Wildlife с использованием нейронных сетей глубокого обучения
---------
### При выполнении лабораторной работы №1, по предмету "Современные методы обработки информации", ставилась задача классифицировать набор данных Oregon-Wildlife, состоящий из 14.000 изображений, по 20 различным категориям.
### Для выполнения поставленной задачи мы обучали сверточные нейронные сети, прокрутив весь набор данных 50 раз, так как количество эпох = 50. При этом каждая эпоха делится на равные части, по размеру batch_size, в моем случае batch_size=64.

1.Описание архитектуры нейронной сети:
-----------
* Задание размерности входного изображения (224х224х3)

```inputs = tf.keras.Input(shape=(RESIZE_TO, RESIZE_TO, 3))```
*  Convolutional layer - сверточный 2D-слой, создающий ядро свертки. В качестве параметров задаются количество выходных фильтров = 8 и размерность матрицы ядра 3х3. Размер изображения на выходе получается 222x222x8.

```x = tf.keras.layers.Conv2D(filters=8, kernel_size=3)(inputs)```
* MaxPool2D – слой подвыборки, снижающий размерность поступивших на него данных. Параметры не указаны, поэтому используются параметры, заданные по умолчанию: размер прямоугольной сетки (pool_size) = 2х2, шаг(strides) = 2. На данном шаге происходит уменьшение размера матрицы, исходя из чего, размер изображения получается в два раза меньше - 111х111х8.

```x = tf.keras.layers.MaxPool2D()(x)```
* Конволюционный слой, позволяющий преобразовать многомерную матрицу тензора в одномерную.

``` x = tf.keras.layers.Flatten()(x)```

* Данный слой, используя функцию активации softmax, определяет вероятность попадания каждого из изображений в нужный, из 20-ти возможных, класс.

```outputs = tf.keras.layers.Dense(NUM_CLASSES, activation=tf.keras.activations.softmax)(x)```
