# Тестовое задание для KazanExpress

Ниже описано исследование для тестового задания маркетплейса KazanExpress на позицию стажера DataScience.

Исследование записано в самом ноутбуке. Здесь я кратко опишу его ход и дам некоторые комментарии по делу и не очень.

## Задача

Задачей исследования являлась категоризация товаров по различным параметрам (описанию, картинке, рейтингу и т.д.).

Вещь, безусловно, необходимая и важная, т.к. маркетплейс получает меньше прибыли если продавец ленится выбирать категорию товара, а пользователь не может найти товар в нужной ему категории.


## Что было сделано

1. Проанализированы данные, на удивление чистые и без пропусков, но с высоким дисбалансом классов(всего 874 категории товаров, с 6590 объектами в мажоритарном классе и минимум 1 объектов в минорном классе)

2. Фич немного, для отбора пользовался только логикой. По итогу было отброшено все кроме описания товара, из самого описания взято только самое важное (название)

3. Для борьбы с дисбалансом классов был использован апсемплинг `SMOTE` до 300 объектов (хотя метрику он улучшил незначительно, валидация без семплирования в исследовании не отражена)

Для подбора гиперпараметров испольована кроссвалидация `GridsearchCV` с `Pipeline` на урезанном и семплированном датасете, что является ошибкой, на которую господин-исследователь пошел сознательно, иначе ему просто не хватило бы оперативной памяти (или терпения для обучения модели). Далее с подобранными гиперпараметрами обучены и протестированы 2 модели с валидацией.

- Для перевода текста в числовой формат использовалась мера `TF-IDF`
- Для оценки использовался `взвешенный f1_score`:

$$
F_{weighted} = 2\cdot {P_{weighted} \cdot R_{weighted} \over P_{weighted} + R_{weighted}}
$$

где: 

$P_{weighted}$ - взвешенный $Precision$

$R_{weighted}$  - взвешенный $Recall$

4. Протестированные модели - Логистическая регрессия и Метод опорных векторов, валидация на 15% исходного датасета

Полученная метрика f1_weighted - 0.858 для логистической регрессии на тестовой выборке. 

Результат 1го места приватного лидерборда 0.9042 

Результат данной модели на приватном лидерборде 0.8725 (7 место из 82)

5. Проведен анализ результатов работы модели:
- Модель недостаточно хорошо отделяет друг от друга женскую и мужскую одежду.
- Модель допускает ошибки в смежных категориях: `Рукоделие->Шитье->Инструменты` и `Рукоделие->Инструменты для рукоделия`


## Что не было сделано

Никаким образом не задействовались картинки.

## Стек
pandas, sklearn, imblearn
