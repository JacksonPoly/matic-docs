---
id: overview
title: Обзор
description: "Введение в процедуру тестирования Polygon Edge. "
keywords:
  - docs
  - polygon
  - edge
  - performance
  - tests
  - loadbot
---
:::caution
Обратите внимание, что наш , `loadbot`использованный для выполнения этих тестов, теперь обесценивается.
:::

| Тип | Значение | Ссылка не тестирование |
| ---- | ----- | ------------ |
| Регулярные трансферы | 1428 токенов в секунду | [4 июля 2022 года](test-history/test-2022-07-04.md#results-of-eoa-to-eoa-transfers) |
| Трансферы ERC-20 | 1111 токенов в секунду | [4 июля 2022 года](test-history/test-2022-07-04.md#results-of-erc20-token-transfers) |
| Минтинг NFT | 714 токенов в секунду | [4 июля 2022 года](test-history/test-2022-07-04.md#results-of-erc721-token-minting) |

---

Наша цель состоит в том, чтобы создать высокопроизводительное, многофункциональное и простое в настройке и обслуживании программное обеспечение для блокчейна. Все испытания были проведены с помощью Polygon Edge Loadbot. Каждый отчет о производительности, который вы найдете в этом разделе, правильно датирован, среда четко описана, а метод тестирования подробно описан.

Цель проведения тестирования производительности — показать реальную производительность сети блокчейн Polygon Edge. При проведении тестирования с помощью нашего loadbot в такой же среде каждый должен получить результаты, аналогичные указанным здесь.

Все испытания производительности были проведены на платформе AWS на цепочке, которая состоит из нод-экземпляров EC2.