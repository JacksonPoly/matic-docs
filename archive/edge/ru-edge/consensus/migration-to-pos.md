---
id: migration-to-pos
title: Миграция из PoA в PoS
description: "Как выполнить миграцию из режима PoA в режим PoS IBFT и наоборот."
keywords:
  - docs
  - polygon
  - edge
  - migrate
  - PoA
  - PoS
---

## Обзор {#overview}

В этом разделе описывается миграция между режимами PoA и PoS IBFT в работающем кластере без необходимости перезагрузки блокчейна.

## Как выполнить миграцию в PoS {#how-to-migrate-to-pos}

Необходимо остановить все ноды, добавить конфигурацию форка в genesis.json с помощью команды `ibft switch` и перезапустить ноды.

````bash
polygon-edge ibft switch --chain ./genesis.json --type PoS --deployment 100 --from 200
````
:::caution Переключение при использовании ECDSA
При использовании ECDSA должен быть добавлен `--ibft-validator-type`флажок, который упоминает что используется ECDSA. Если не включено, Edge автоматически переключится на BLS.

````bash
polygon-edge ibft switch --chain ./genesis.json --type PoS --ibft-validator-type ecdsa --deployment 100 --from 200
````
:::Чтобы перейти в PoS, вам нужно будет указать 2 высоты блока: `deployment`и . `from``deployment`это высота для развертывания контракта стейкинга и `from`является высотой начала PoS. Контракт стейкинга будет развернут по адресу `0x0000000000000000000000000000000000001001`  в `deployment`, как и в случае с предварительно развернутым контрактом.

См. раздел [Доказательство доли владения (Proof of Stake)](/docs/edge/consensus/pos-concepts) для получения дополнительной информации о контракте стейкинга.

:::warning Валидаторы делают стейки вручную

Чтобы стать валидатором в начале PoS, каждый валидатор должен сделать стейк после развертывания контракта в `deployment` и до `from`. Каждый валидатор будет обновлять собственный набор валидаторов через контракт стейкинга в начале PoS.

Чтобы узнать больше о стейкинге посетите раздел **[Настройка и используйте Proof](/docs/edge/consensus/pos-stake-unstake)** of Stake.
:::