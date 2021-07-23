---
title: "SpaceHosting"
---

SpaceHosting - это сервис для поиска k-ближайших соседей (далее kNN), использующий .NET библиотеку [SpaceHosting.Index](https://github.com/kontur-model-ops/space-hosting-index#spacehostingindex). 

Библиотека SpaceHosting.Index поддерживает dense и sparse вектора. Для работы с dense векторами используется библиотека [Faiss](https://github.com/facebookresearch/faiss), для sparse векторов мы реализовали свою версию библиотеки [PySparNN](https://github.com/facebookresearch/pysparnn) на C#. 

# Почему SpaceHosting
{:toc}
Преимущества SpaceHosting: 
* Поддерживает работу с dense и sparse векторами.
* Использует поисковые алгоритмы:
  * Faiss.
  * Собственная реализация PySparNN.
* Поддерживает Approximate kNN (AkNN).

# Библиотека SpaceHosting.Index
SpaceHosting.Index умеет хранить внутри себя маппинг между векторами и набором ключей. При поиске kNN/AkNN вернутся не номера векторов, а ключи, которые им соответствуют. Ключ – это произвольный набор параметров, который характеризует вектор. 

Также библиотека позволяет хранить произвольные метаданные вместе с соответствующими векторами. В результате поиска kNN/AkNN метаданные возвращаются вместе с векторами. 

Подробнее про библиотеку SpaceHosting.Index читайте [здесь](https://github.com/kontur-model-ops/space-hosting-index#spacehostingindex).

# Локальный запуск
```
git clone https://github.com/kontur-model-ops/space-hosting.git 
cd space-hosting 
./docker-compose-up.sh
```
После этого по ссылке <http://localhost:8080> будет доступна Swagger-спецификация SpaceHosting API.

# Помощь
Если у вас возникли вопросы или проблемы с сервисом SpaceHosting, пишите нам в Slack в канал … .

# Как использовать SpaceHosting 
1. Подготовьте файлы:
 * с [векторами](https://irindescence.github.io/github-pages-with-jekyll/#%D0%B2%D0%B5%D0%BA%D1%82%D0%BE%D1%80%D0%B0); 
 * с [метаданными](https://irindescence.github.io/github-pages-with-jekyll/#%D0%BC%D0%B5%D1%82%D0%B0%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5) (опционально). Определяется по заданной переменной окружения SH_VECTORS_METADATA_FILE_NAME из пункта 2. 
2. Запустите сервис SpaceHosting и определите следующие переменные окружения:
 * SH_VECTORS_FILE_NAME.
 * SH_VECTORS_FILE_FORMAT.
 * SH_VECTORS_METADATA_FILE_NAME (опционально). 
    Если переменная не задана, то метаданные в индекс не загружаются.
 * SH_INDEX_ALGORITHM – [тип индекса](https://github.com/irindescence/github-pages-with-jekyll/blob/main/index.md#%D0%B2%D0%BE%D0%B7%D0%BC%D0%BE%D0%B6%D0%BD%D1%8B%D0%B5-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D1%8F-sh_index_algorithm). 
3. SpaceHosting прочитает входные данные из указанных файлов и построит индекс заданного типа.
4. Отправьте запрос на поиск kNN/AkNN.
   Можете использовать batch-mode – поиск ближайших векторов сразу для нескольких входных векторов.
5. SpaceHosting вернет ответ на ваш запрос.

## Формат входных файлов
### Вектора
В файле с векторами (SH_VECTORS_FILE_NAME) должен лежать список векторов. В зависимости от значения переменной SH_VECTORS_FILE_FORMAT этот список задается по-разному:
* VectorArrayJson - список векторов сериализован в JSON. Каждый вектор - это массив чисел одинаковой длины, задающих координаты вектора. [Пример файла](https://github.com/kontur-model-ops/space-hosting/blob/master/.data-samples/vectors.json).

* PandasDataFrameCsv - список векторов задается в CSV-формате, в таком же виде, как его сериализует [pandas.DataFrame.to_csv](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_csv.html). Один вектор - это одна строка, координаты идут через запятую. [Пример файла](https://github.com/kontur-model-ops/space-hosting/blob/master/.data-samples/vectors-df.csv).

* PandasDataFrameJson - список векторов задается в JSON-формате, в таком же виде, как его сериализует pandas.DataFrame.to_json. [Пример файла](https://github.com/kontur-model-ops/space-hosting/blob/master/.data-samples/vectors-df.json). 

### Метаданные
Файл с метаданными - это набор произвольных пар ключ-значение. Формат файлов JSON. Пример файла смотрите [здесь](https://github.com/kontur-model-ops/space-hosting/blob/master/.data-samples/vectors-metadata.json). Соответствие между векторами и метаданными из входных файлов строится по индексу: первый вектор из файла с векторами соответствует первой записи в файле с метаданными.

## Возможные значения SH_INDEX_ALGORITHM
* FaissIndex.Flat.L2 – dense vectors, euclidian metric.
* FaissIndex.Flat.IP – dense vectors, inner-product metric.
* SparnnIndex.Cosine – sparse vectors, cosine metric.
* SparnnIndex.JaccardBinary – sparse binary vectors, jaccard metric.

# Примеры использования
1. [Поиск отелей по запросу пользователя](https://github.com/kontur-model-ops/space-hosting/blob/master/samples/spacehosting_hotels_example.ipynb).
Задача: сэкономить время пользователя на поиск отелей. 
В этом примере используются векторизованные отзывы пользователей для поиска отелей.

2. [Price Match Guarantee](https://github.com/kontur-model-ops/space-hosting/blob/master/samples/spacehosting_cv_example.ipynb).
Задача: помочь продавцу установить конкурентоспособную цену для своего товара на маркетплейсе.
Для этого примера используются данные из [Shopee kaggle Competiton](https://www.kaggle.com/c/shopee-product-matching/overview/description).

