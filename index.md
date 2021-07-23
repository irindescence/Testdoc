---
layout: default
title: Home
nav_order: 1
permalink: /
---

# SpaceHosting
{: .fs-9 }

Сервис для поиска k-ближайших соседей.
{: .fs-6 .fw-300 }

[View it on GitHub](https://github.com/kontur-model-ops/space-hosting){: .btn .fs-5 .mb-4 .mb-md-0 }

---

# Get started

SpaceHosting - это сервис для поиска k-ближайших соседей (далее kNN), использующий .NET библиотеку [SpaceHosting.Index](https://github.com/kontur-model-ops/space-hosting-index#spacehostingindex). 

Библиотека SpaceHosting.Index поддерживает dense и sparse вектора. Для работы с dense векторами используется библиотека [Faiss](https://github.com/facebookresearch/faiss), для sparse векторов мы реализовали свою версию библиотеки [PySparNN](https://github.com/facebookresearch/pysparnn) на C#. 

## Why SpaceHosting? 

Преимущества SpaceHosting: 
* Поддерживает работу с dense и sparse векторами.
* Использует поисковые алгоритмы:
  * Faiss.
  * Собственная реализация PySparNN.
* Поддерживает Approximate kNN (AkNN).

## Library SpaceHosting.Index 

SpaceHosting.Index умеет хранить внутри себя маппинг между векторами и набором ключей. При поиске kNN/AkNN вернутся не номера векторов, а ключи, которые им соответствуют. Ключ – это произвольный набор параметров, который характеризует вектор. 

Также библиотека позволяет хранить произвольные метаданные вместе с соответствующими векторами. В результате поиска kNN/AkNN метаданные возвращаются вместе с векторами. 

Подробнее про библиотеку SpaceHosting.Index читайте [здесь](https://github.com/kontur-model-ops/space-hosting-index#spacehostingindex).

## Local installation 
```
git clone https://github.com/kontur-model-ops/space-hosting.git 
cd space-hosting 
./docker-compose-up.sh
```
После этого по ссылке <http://localhost:8080> будет доступна Swagger-спецификация SpaceHosting API.

## Next Step 

[See How to use SpaceHosting]().

---

## Support

Если у вас возникли вопросы или проблемы с сервисом SpaceHosting, пишите нам в Slack в канал … .

## License

SpaceHosting is distributed by an [Apache License 2.0](https://github.com/kontur-model-ops/space-hosting/blob/master/LICENSE).

