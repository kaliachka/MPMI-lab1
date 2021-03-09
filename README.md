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

* Полносвязный выходной слой Dense, используя функцию активации softmax, определяет вероятность попадания каждого из изображений в нужный, из 20-ти возможных, класс.

```outputs = tf.keras.layers.Dense(NUM_CLASSES, activation=tf.keras.activations.softmax)(x)```

2.График обучения для задачи клонирования репозитория с использованием команды git clone:
---------
*синяя ломаная линия - при валидации*

*оранжевая ломаная линия - при обучении*

**График метрики точности:**

![1](https://user-images.githubusercontent.com/59210216/110445765-81d90080-80cf-11eb-8aa6-aad20e6b97af.jpg)

**График функции потерь:**

![2](https://user-images.githubusercontent.com/59210216/110445885-a46b1980-80cf-11eb-87ad-00d3c02b43c2.jpg)

3.Анализ полученных результатов:
-------
Анализируя полученные диаграммы можно заметить, что, на графике функции потерь, ломаная при валидации начала линейно возрастать, по отношению к графику метрики точности. Однако, если рассмотреть ломаную при обучении, то на графике функции потерь данная линия сильно убавила свои значения, по отношению к графику метрики точности. Все это говорит о переобучение тестируемой модели, что может быть вызвано тем, что наш датасет слишком велик для начальных случайных приближений.

4.Графики обучения нейронной сети с количеством сверточных слоев > 3:
------
* Для решения данной задачи мною были добавлены 3 дополнительных сверточных слоя Conv2D и 3 дополнительных слоя подвыборки MaxPool2D.
* В каждом из сверточных слоев Conv2D было изменено количество шагов(strides = 2).
* В каждом из слоев подвыборки MaxPool2D было изменено количество шагов(strides = 1).
```php
inputs = tf.keras.Input(shape=(RESIZE_TO, RESIZE_TO, 3))
  x = tf.keras.layers.Conv2D(filters=8, kernel_size=3, strides=2)(inputs)
  x = tf.keras.layers.MaxPool2D(strides=1)(x)
  x = tf.keras.layers.Conv2D(filters=8, kernel_size=3, strides=2)(x)
  x = tf.keras.layers.MaxPool2D(strides=1)(x)
  x = tf.keras.layers.Conv2D(filters=8, kernel_size=3, strides=2)(x)
  x = tf.keras.layers.MaxPool2D(strides=1)(x)
  x = tf.keras.layers.Conv2D(filters=8, kernel_size=3, strides=2)(x)
  x = tf.keras.layers.MaxPool2D(strides=1)(x)
  x = tf.keras.layers.Flatten()(x)
  outputs = tf.keras.layers.Dense(NUM_CLASSES, activation=tf.keras.activations.softmax)(x)
  ```
*синяя ломаная линия - при валидации*

*оранжевая ломаная линия - при обучении*

**График метрики точности:**

  ![3](https://user-images.githubusercontent.com/59210216/110455036-011f0200-80d9-11eb-8e63-43791fa5209a.jpg)
  
**График функции потерь:**

  ![4](https://user-images.githubusercontent.com/59210216/110455050-05e3b600-80d9-11eb-9302-72c286e11847.jpg)



  
