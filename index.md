---
layout: default
title: Home
nav_order: 1
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
permalink: /
---

# SpaceHosting
{: .fs-9 }

Сервис для поиска k-ближайших соседей.
{: .fs-6 .fw-300 }

[View it on GitHub](https://github.com/kontur-model-ops/space-hosting){: .btn .fs-5 .mb-4 .mb-md-0 }

---

# Table of contents
* [Почему SpaceHosting](#why)
* [Библиотека SpaceHosting.Index](#liabry)
* [Локальный запуск](#lok)    
* [Помощь](#Помощь)
---

SpaceHosting - это сервис для поиска k-ближайших соседей (далее kNN), использующий .NET библиотеку [SpaceHosting.Index](https://github.com/kontur-model-ops/space-hosting-index#spacehostingindex). 

Библиотека SpaceHosting.Index поддерживает dense и sparse вектора. Для работы с dense векторами используется библиотека [Faiss](https://github.com/facebookresearch/faiss), для sparse векторов мы реализовали свою версию библиотеки [PySparNN](https://github.com/facebookresearch/pysparnn) на C#. 

# Почему SpaceHosting <a name="why"></a>
{:toc}
Преимущества SpaceHosting: 
* Поддерживает работу с dense и sparse векторами.
* Использует поисковые алгоритмы:
  * Faiss.
  * Собственная реализация PySparNN.
* Поддерживает Approximate kNN (AkNN).

# Библиотека SpaceHosting.Index <a name="liabry"></a>
SpaceHosting.Index умеет хранить внутри себя маппинг между векторами и набором ключей. При поиске kNN/AkNN вернутся не номера векторов, а ключи, которые им соответствуют. Ключ – это произвольный набор параметров, который характеризует вектор. 

Также библиотека позволяет хранить произвольные метаданные вместе с соответствующими векторами. В результате поиска kNN/AkNN метаданные возвращаются вместе с векторами. 

Подробнее про библиотеку SpaceHosting.Index читайте [здесь](https://github.com/kontur-model-ops/space-hosting-index#spacehostingindex).

# Локальный запуск <a name="lok"></a>
```
git clone https://github.com/kontur-model-ops/space-hosting.git 
cd space-hosting 
./docker-compose-up.sh
```
После этого по ссылке <http://localhost:8080> будет доступна Swagger-спецификация SpaceHosting API.

# Помощь
Если у вас возникли вопросы или проблемы с сервисом SpaceHosting, пишите нам в Slack в канал … .
