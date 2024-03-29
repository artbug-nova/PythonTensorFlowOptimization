<h1>Diploma Project</h2>

Оптимизация траектории движения механизма последовательной структруры методом градиентного спуска.

Сложность решения обратной задачи заключается в том, чтобы найти
положение центра схвата в начальной точке. Значения обобщенных координат
манипулятора были подобраны для начального положения
Для решения обратной задачи сформируем критериальную функцию.
Так как целью решения является поиск таких значений обобщенных
координат, при которых разность между заданным положением схвата и его
фактическим положением равнялась нулю, то в качестве критерия следует
принять параметр, отражающий эту разность.

Вначале алгоритма определяются классы Python:
KinematicPart – представляющий подвижное звено, и Robot – представляющий
манипулятор. Параметры подвижных звеньев заданы в мм:
Z1 = KinematicPart (300, 0, np.pi/2, bmin=-150*r, bmax=150*r)
Z2 = KinematicPart (0, 250, 0, bmin=-30*r, bmax=150*r)
Z3 = KinematicPart (0, 160, 0, bmin=-150*r, bmax=80*r)
Z4 = KinematicPart (0, 0, np.pi/2, bmin=-150*r, bmax=80*r)
Z5 = KinematicPart (0, 196.17, 0, bmin=0*r, bmax=0*r)
В приведенном выше фрагментах задаются пять подвижных звеньев
(соответствующих объектов Python), с параметрами s, a и альфа
соответственно.
Параметры bmin и bmax задают ограничения для каждой обобщенной
координаты, которые затем используются при вычислении штрафной
функции. Объект манипулятора задается набором подвижных звеньев: parts =
[Z1, Z2, Z3, Z4, Z5], RV = Robot (parts). Метод getXYZ класса Robot позволяет
получить пространственные координаты по обобщенным: xyz = RV.getXYZ
(Q) [:3]. Метод penalty того же класса позволяет получить значение штрафной
функции, принимая на вход обобщенные координаты и штрафные веса penalty
= RV. penalty (Q, W1, W2).
Константа r служит для перевода градусов в радианы. Следующие
блоки определяют набор подвижных звеньев parts, данный робот имеет пять
подвижных звеньев и задается опорные точки траектории движения центра
схвата для которых и будет выполняться расчет.
Далее в блок-схеме определяется последовательность вычисления
данной программы, содержащая критериальная функцию, которая
представляет собой евклидово расстояние между целевой точкой и текущей.
Для оптимизации критериальной функции воспользуемся двумя
современными методами, реализации которых имеются в TensorFlow:
AdagradOptimizer и AdamOptimizer. Требуемую точность оптимизации 
определим равной 0.04 мм. Коэффициент скорости обучения для всех
оптимизаторов определим экспоненциально затухающим.
Затухающий коэффициент позволяет адаптироваться к целевой функции и делать более короткие шаги там, где с
более длинным шагом есть риск пересечь локальный минимум.

<h3>Граф решения обратной задачи кинематики:</h3>

![graph](https://github.com/bishop777-sys/PythonTensorFlowOptimization/blob/master/graph.png)


