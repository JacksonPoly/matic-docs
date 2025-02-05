---
id: optimisticoracle
title: Оракул UMA Optimistic Oracle
sidebar_label: UMA
description: Оптимистический Oracle UMA, позволяет контрактам быстро запрашивать и получать любые данные
keywords:   
  - wiki
  - polygon
  - oracle
  - UMA
  - Optimistic Oracle
image: https://wiki.polygon.technology/img/polygon-wiki.png
---

Оптимистический Oracle UMA, позволяет контрактам быстро запрашивать и получать любые данные. Система оракула UMA, состоящая из двух основных компонентов:

1. Оракул Optimistic Oracle
2. Механизм проверки данных (DVM)

## Оракул Optimistic Oracle {#optimistic-oracle}

**Оптимистический Oracle** UMA, позволяет контрактам быстро запрашивать и получать информацию о цене. Optimistic Oracle выступает в качестве общей игры для эскалации между контрактами, that запрос на цену, и системой разрешения споров UMA, известной как Механизм проверки данных (DVM).

Цены, предлагаемые оракулом Optimistic Oracle, не отправляются в DVM, если они не оспариваются. Это позволяет контрактам получать информацию о цене в течение любого заранее определенного периода времени без указания цены на актив в цепочке.

## Механизм проверки данных (DVM) {#data-verification-mechanism-dvm}

При возникновении спора в DVM отправляется запрос. Все контракты на базе UMA используют DVM как центр разрешения споров. Отправленные в DVM споры разрешаются через 48 часов после того, как держатели токенов UMA проголосуют за цену актива в определенное время. Контрактам UMA не требуется использовать оракул Optimistic Oracle, если им не требуется получить цену актива быстрее, чем за 48 часов.

Механизм проверки данных (DVM) — это сервис разрешения споров для контрактов на базе протокола UMA. DVM — это мощный ресурс, включающий элемент суждения для обеспечения безопасного и правильного управления контрактами при возникновении проблем на волатильных (и иногда подверженных манипуляции) рынках.

## Интерфейс Optimistic Oracle {#optimistic-oracle-interface}

Оракул Optimistic Oracle используется финансовыми контрактами или любыми третьими сторонами для получения цен. После запроса цены кто угодно сможет предложить цену в ответ. После предложения цена проходит жизненный цикл, когда кто угодно может оспорить предлагаемую цену и отправить оспариваемую цену в DVM UMA для урегулирования.

:::info

В этом разделе объясняется, как разные участники могут взаимодействовать с Optimistic Oracle. Чтобы посмотреть самые новые развертывания mainnet, kovan или L2 в контрактах Optimistic Oracle, используйте [адреса производства](https://docs.umaproject.org/dev-ref/addresses).

:::

Интерфейс Optimistic Oracle состоит из 12 методов.
- `requestPrice`
- `proposePrice`
- `disputePrice`
- `settle`
- `hasPrice`
- `getRequest`
- `settleAndGetPrice`
- `setBond`
- `setCustomLiveness`
- `setRefundOnDispute`
- `proposePriceFor`
- `disputePriceFor`

### requestPrice {#requestprice}

Запрашивает новую цену. Он должен выполняться для зарегистрированного идентификатора цены. Обратите внимание, что этот метод вызывается автоматически большинством финансовых контрактов, зарегистрированных в системе UMA, но также может быть вызван кем угодно для любого зарегистрированного идентификатора цены. Например, контракт Expiring Multiparty (EMP) вызывает этот метод при вызове его метода `expire`.

Параметры:
- `identifier`: запрашиваемый идентификатор цены.
- `timestamp`: временная метка запрашиваемой цены.
- `ancillaryData`: вспомогательные данные, представляющие дополнительные аргументы, передаваемые с запросом цены.
- `currency`: токен ERC20, используемый для выплаты наград и комиссий. Требуется одобрение для использования с DVM.
- `reward`: награда, предлагаемая автору успешного предложения. Выплачивается инициатором. Примечание: может иметь значение 0.

### proposePrice {#proposeprice}

Предлагает значение цены для запроса существующей цены.

Параметры:
- `requester`: отправитель начального запроса цены.
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.
- `proposedPrice`: предлагаемая цена.

### disputePrice {#disputeprice}

Оспаривает значение цены для существующего запроса цены с активным предложением.

Параметры:
- `requester`: отправитель начального запроса цены.
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.

### settle {#settle}

Пытается погасить открытый запрос цены. Он вернется, если его нельзя урегулировать.

Параметры:
- `requester`: отправитель начального запроса цены.
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.

### hasPrice {#hasprice}

Проверяет, был ли указанный запрос разрешен или погашен (т. е. есть ли цена у оракула Optimistic Oracle).

Параметры:
- `requester`: отправитель начального запроса цены.
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.

### getRequest {#getrequest}

Получает текущую структуру данных, содержащую всю информацию о запросе цены.

Параметры:
- `requester`: отправитель начального запроса цены.
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.

### settleAndGetPrice {#settleandgetprice}

Получает цену, которая ранее была запрошена вызывающим. Возвращается, если запрос не погашен или не может быть погашен. Примечание. Этот метод не является методом просмотра, так что этот вызов может фактически погасить запрос цены, если он не погашен.

Параметры:
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.

### setBond {#setbond}

Устанавливает обязательство предложения, связанное с запросом цены.

Параметры:
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.
- `bond`: задаваемое значение пользовательского обязательства.

### setCustomLiveness {#setcustomliveness}

Задает пользовательское значение жизненного цикла запроса. Жизненный цикл — это время ожидания до автоматического разрешения предложения.

Параметры:
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.
- `customLiveness`: новое пользовательское значение жизненного цикла.

### setRefundOnDispute {#setrefundondispute}

Задает возврат вознаграждения для запроса в случае оспаривания предложения. Это дает возможности «хеджирования» инициатору в случае задержек, связанных со спорами. Примечание. В случае спора победитель получает обязательство другого участника, так что прибыль возможна даже в случае возврата вознаграждения.

Параметры:
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.

### disputePriceFor {#disputepricefor}

Оспаривает запрос цены с активным предложением от имени другого адреса. Примечание. Этот адрес получит все награды, которые поступят от данного спора. Однако любые обязательства поступают от инициатора.

Параметры:
- `disputer`: заданный адрес инициатора спора.
- `requester`: отправитель начального запроса цены.
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.

### proposePriceFor {#proposepricefor}

Предлагает значение цены от имени другого адреса. Примечание. Этот адрес получит все награды, связанные с этим предложением. Однако любые обязательства поступают от инициатора.

Параметры:
- `proposer`: адрес, устанавливаемый как автор предложения.
- `requester`: отправитель начального запроса цены.
- `identifier`: идентификатор цены для идентификации существующего запроса.
- `timestamp`: временная метка для идентификации существующего запроса.
- `ancillaryData`: вспомогательные данные о запрашиваемой цене.
- `proposedPrice`: предлагаемая цена.

## Интеграция Optimistic Oracle {#integrating-the-optimistic-oracle}

Эта демонстрация настраивает контракт `OptimisticDepositBox`, берущий на ответственное хранение остаток токенов ERC-20 пользователя.

В локальном блокчейне тестовой сети пользователь вносит на депозит контракта wETH (Wrapped Ether) и выводит wETH, номинированные в долларах США. Например, если пользователь хочет вывести $10,000 USD of wETH, and the ETH/USD exchange rate is $2000, он выводит 5 wETH.

* Пользователь связывает `OptimisticDepositBox` с одним из идентификаторов цены в DVM.

* Пользователь вносит wETH в `OptimisticDepositBox` и регистрирует их с идентификатором цены `ETH/USD`.

* Теперь пользователь может вывести номинированное в долларах США количество wETH из `DepositBox` с помощью вызова смарт-контракта, а Optimistic Oracle даст возможность установить оптимистичную цену в цепочке.

В этом примере пользователь не сможет выполнить трансфер номинированного в долларах США количества wETH, не сославшись на канал цен `ETH/USD` вне цепочки. Поэтому Optimistic Oracle дает пользователю возможность получить эталонную цену.

В отличие от запросов цены в DVM, запрос цены в Optimistic Oracle может быть разрешен в течение заданного временного окна при отсутствии споров, что может занять существенно меньше времени, чем период голосования DVM. Окно жизненного цикла настраивается, но обычно оно составляет два часа в отличие от 2-3 дней при проведении операции через DVM.

Инициатор запроса цены в настоящее время не должен платить комиссию DVM. Инициатор запроса цены может предложить награду автору предложения, который ответит на запрос цены, однако в этом примере установлено значение награды `0`.

Автор предложения публикует обязательство вместе с ценой, и эта сумма возмещается, если цена не оспаривается или спор разрешается в пользу автора предложения. В ином случае это обязательство используется для оплаты окончательной комиссии DVM и награды инициатору успешного спора.

В демонстрации инициатор запроса не требует дополнительного обязательства от автора ценового предложения, поэтому общая публикуемая сумма обязательства равняется сумме окончательной комиссии wETH, которая в настоящее время составляет 0,2 wETH. Подробности реализации можно посмотреть в функции `proposePriceFor` в [контракте](https://docs-dot-uma-protocol.appspot.com/uma/contracts/OptimisticOracle.html) `OptimisticOracle`.

## Запуск демонстрации {#running-the-demo}

1. Убедитесь, что вы выполнили все предварительные шаги по настройке, описанные [здесь](https://docs.umaproject.org/developers/setup).
2. Запустите локальный экземпляр Ganache (т. е. не Kovan/Ropsten/Rinkeby/Mainnet) с помощью `yarn ganache-cli --port 9545`.
3. Выполните миграцию контрактов в другом окне, выполнив следующую команду:

  ```bash
  yarn truffle migrate --reset --network test
  ```

1. Чтобы развернуть [контракт](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/financial-templates/demo/OptimisticDepositBox.sol) `OptimisticDepositBox` и посмотреть простую процедуру действий пользователя, запустите следующий демонстрационный скрипт в корневом каталоге репозитория:

```bash
yarn truffle exec ./packages/core/scripts/demo/OptimisticDepositBox.js --network test
```

Вы увидите следующий вывод:

```
1. Deploying new OptimisticDepositBox
  - Using wETH as collateral token
  - Pricefeed identifier for ETH/USD is whitelisted
  - Collateral address for wETH is whitelisted
  - Deployed an OptimisticOracle
  - Deployed a new OptimisticDepositBox


2. Minting ERC20 to user and giving OptimisticDepositBox allowance to transfer collateral
  - Converted 10 ETH into wETH
  - User's wETH balance: 10
  - Increased OptimisticDepositBox allowance to spend wETH
  - Contract's wETH allowance: 10


3. Depositing ERC20 into the OptimisticDepositBox
  - Deposited 10 wETH into the OptimisticDepositBox
  - User's deposit balance: 10
  - Total deposit balance: 10
  - User's wETH balance: 0


4. Withdrawing ERC20 from OptimisticDepositBox
  - Submitted a withdrawal request for 10000 USD of wETH
  - Proposed a price of 2000000000000000000000 ETH/USD
  - Fast-forwarded the Optimistic Oracle and Optimistic Deposit Box to after the liveness window so we can settle.
  - New OO time is [fast-forwarded timestamp]
  - New ODB time is [fast-forwarded timestamp]
  - Executed withdrawal. This also settles and gets the resolved price within the withdrawal function.
  - User's deposit balance: 5
  - Total deposit balance: 5
  - User's wETH balance: 5
```

## Разъяснение функций контракта {#explaining-the-contract-functions}

Код `OptimisticDepositBox`[контракта](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/financial-templates/demo/OptimisticDepositBox.sol) показывает, как взаимодействовать с Oracle.

Функция `constructor` включает аргумент `_finderAddress` для контракта UMA `Finder`, который ведет реестр адреса `OptimisticOracle`, одобренного обеспечения, белых списков идентификаторов цены, а также других важных адресов контракта.

Это позволяет `constructor` проверять действительность типа обеспечения и ценового идентификатора и дает `OptimisticDepositBox` возможность находить `OptimisticOracle` и впоследствии взаимодействовать с ним.

Функция `requestWithdrawal` включает внутренний вызов `OptimisticOracle` с запросом цены `ETH/USD`. После возврата пользователь может вызвать `executeWithdrawal` для завершения вывода.

В комментариях кода гораздо больше информации, поэтому пожалуйста, посмотрите на интересующий вас вопрос.

## Дополнительные ресурсы {#additional-resources}

Вот некоторые дополнительные ресурсы по DVM UMA:

- [Техническая архитектура](https://docs.umaproject.org/oracle/tech-architecture)
- [Экономическая архитектура](https://docs.umaproject.org/oracle/econ-architecture)
- [Пост в блоге](https://medium.com/uma-project/umas-data-verification-mechanism-3c5342759eb8) о дизайне DVM UMA
- [Белая книга](https://github.com/UMAprotocol/whitepaper/blob/master/UMA-DVM-oracle-whitepaper.pdf) о дизайне DVM UMA
- [Исследовательский репозиторий](https://github.com/UMAprotocol/research) для оптимальной комиссионной политики
- [Репозиторий UMIP](https://github.com/UMAprotocol/UMIPs) для предложений по управлению
