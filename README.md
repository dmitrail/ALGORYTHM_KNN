# Метод К ближайших соседей

### Описание метода:
  KNN (k-Nearest Neighbors) – это алгоритм классификации, 
KNN – ленивый классификатор.

  Ленивый классификатор в процессе обучения не делает ничего, а только хранит тренировочные данные. Он начинает классификацию 
только тогда, когда появляются новые немаркированные данные.
  kNN не строит никакую классификационную модель. Вместо этого он просто сохраняет размеченные тренировочные данные.

Когда появляется новые неразмеченные данные, kNN проходит по 2 базовым шагам:
  1) Сначала он ищет k ближайших соседей.
  2) Затем, используя классы соседей, kNN решает, как лучше классифицировать новые данные.

Как KNN понимает, какие точки находятся ближе всего? Для непрерывных данных kNN использует дистанционную метрику, например, Евклидову дистанцию (метрику). Выбор метрики зависит от типа данных.

### Алгоритм на языке R:

Итак, Построим выборку ирисов Фишера: <br/>
  >colors <- c("setosa" = "red", "versicolor" = "green3",
  >"virginica" = "blue")
  >plot(iris[, 3:4], pch = 21, bg = colors[iris$Species],
  >col = colors[iris$Species])

получим следующий результат:
![alt text](https://github.com/dmitrail/ALGORYTHM_KNN/blob/master/KNN_RAW.png) <br/>
    Зададим Евклидово расстояние: <br/>
  >euclideanDistance <- function(u, v)<br/>
  >{<br/>
   >sqrt(sum((u - v)^2))<br/>
  >} <br/>
  
    Просортируем объекты согласно расстояния до объекта z: <br/>
  >sortObjectsByDist <- function(xl, z, metricFunction = euclideanDistance) <br/>
  >{ <br/>
  >l <- dim(xl)[1] <br/>
  >n <- dim(xl)[2] - 1 <br/>
  
  Создадим матрицу расстояний: <br/>
  >distances <- matrix(NA, l, 2) <br/>
  >for (i in 1:l) <br/>
  >{ <br/>
  >distances[i, ] <- c(i, metricFunction(xl[i, 1:n], z)) <br/>
  >} <br/>
  
Сортируем: <br/>
  >orderedXl <- xl[order(distances[, 2]), ]<br/>
  >return (orderedXl);<br/>
  >}<br/>
  
Применяем метод kNN: <br/>
  >kNN <- function(xl, z, k)<br/>
  >{<br/>
  
  Сортируем выборку согласно классифицируемого объекта: <br/>
  >orderedXl <- sortObjectsByDist(xl, z)<br/>
  >n <- dim(orderedXl)[2] - 1<br/>
  
Получаем классы первых k соседей:<br/>
  >classes <- orderedXl[1:k, n + 1]<br/>
  
Составляем таблицу встречаемости каждого класса:<br/>
  >counts <- table(classes)<br/>
  
Находим класс, который доминирует среди первых k соседей:<br/>
  >class <- names(which.max(counts))<br/>
  >return (class)<br/>
  >}<br/>
  
Рисуем выборку:<br/>
  >colors <- c("setosa" = "red", "versicolor" = "green3",
  >"virginica" = "blue")<br/>
  >plot(iris[, 3:4], pch = 21, bg = colors[iris$Species], col<br/>
  >= colors[iris$Species], asp = 1)<br/>
  
Классификация одного заданного объекта: <br/>
  >z <- c(2.7, 1)<br/>
  >xl <- iris[, 3:5]<br/>
  >class <- kNN(xl, z, k=6)<br/>
  >points(z[1], z[2], pch = 22, bg = colors[class], asp = 1)<br/>
### Результатом работы, будет являться следующий вывод:
![alt text](https://github.com/dmitrail/ALGORYTHM_KNN/blob/master/KNN_DONE.png) <br/>
(Исходный код закреплен в шапке статьи)
