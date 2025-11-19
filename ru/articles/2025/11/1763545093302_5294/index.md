---
# -------------------------------------------------------------------------------------------------------------------- #
# GENERAL
# -------------------------------------------------------------------------------------------------------------------- #

title: 'Тома'
description: ''
icon: 'far fa-file-lines'
categories:
  - 'volume'
tags:
  - 'volume'
  - 'create'
  - 'get'
  - 'set'
authors:
  - 'KaiKimera'
sources:
  - ''
license: 'CC-BY-SA-4.0'
complexity: '0'
toc: 1
comments: 1

# -------------------------------------------------------------------------------------------------------------------- #
# DATE
# -------------------------------------------------------------------------------------------------------------------- #

date: '2025-11-19T12:38:13+03:00'
publishDate: '2025-11-19T12:38:13+03:00'
lastMod: '2025-11-19T12:38:13+03:00'

# -------------------------------------------------------------------------------------------------------------------- #
# META
# -------------------------------------------------------------------------------------------------------------------- #

type: 'articles'
hash: 'a93d0c64d9fbca7ff6fd03cf9588418cc814ab89'
uuid: 'a93d0c64-d9fb-5a7f-96fd-03cf9588418c'
slug: 'a93d0c64-d9fb-5a7f-96fd-03cf9588418c'

draft: 0
---

Работа с томами.

<!--more-->

## Создание тома

- Создать том `cloud` в пуле `data`:

```bash
p='data'; v='cloud'; zfs create "${p}/${v}"
```

- Создать том `cloud` с точкой монтирования `/opt/cloud` в пуле `data`:

```bash
p='data'; v='cloud'; o=('-o' "mountpoint=/opt/${v}"); zfs create "${o[@]}" "${p}/${v}"
```

- Создать том `cloud` с алгоритмом компрессии `zstd` в пуле `data`:

```bash
p='data'; v='cloud'; o=('-o' 'compression=zstd'); zfs create "${o[@]}" "${p}/${v}"
```

- Создать том `cloud` с алгоритмом компрессии `zstd` и точкой монтирования `/opt/cloud` в пуле `data`:

```bash
p='data'; v='cloud'; o=('-o' 'compression=zstd' '-o' "mountpoint=/opt/${v}"); zfs create "${o[@]}" "${p}/${v}"
```

## Удаление тома

- Удалить том `cloud` в пуле `data`:

```bash
p='data'; v='cloud'; zfs destroy "${p}/${v}"
```

## Создание зашифрованного тома

- Создать том `secret` и зашифровать его парольной фразой в пуле `data`:

```bash
p='data'; v='secret'; o=('-o' 'encryption=on' '-o' 'keyformat=passphrase'); zfs create "${o[@]}" "${p}/${v}"
```

Где:

- `encryption=on` - включение шифрования.
- `keyformat=passphrase` - тип шифрования "парольная фраза".

{{< alert "tip" >}}
При создании тома `secret` ZFS попросит ввести парольную фразу для шифрования данных.
{{< /alert >}}

## Свойства тома

- Показать все свойства всех томов:

```bash
zfs get all
```

- Показать все свойства тома `cloud` в пуле `data`:

```bash
p='data'; v='cloud'; zfs get all "${p}/${v}"
```

- Показать свойства `compressratio`, `compression`, `mountpoint` и `atime` тома `cloud` в пуле `data`:

```bash
p='data'; v='cloud'; o=('compressratio' 'compression' 'mountpoint' 'atime'); zfs get "$( echo "${o[@]}" | tr ' ' ',' )" "${p}/${v}"
```

- Показать свойства `compressratio`, `compression`, `mountpoint` и `atime` тома `cloud` и во всех его под-томах в пуле `data`:

```bash
p='data'; v='cloud'; o=('compressratio' 'compression' 'mountpoint' 'atime'); zfs get -r "$( echo "${o[@]}" | tr ' ' ',' )" "${p}/${v}"
```

- Установить свойство `compression` в `zstd` тома `cloud` в пуле `data`:

```bash
p='data'; v='cloud'; o=('-o' 'compression=zstd'); zfs set "${o[@]}" "${p}/${v}"
```

- Вернуть свойство `compression` к стандартному наследуемому значению тома `cloud` в пуле `data`:

```bash
p='data'; v='cloud'; o=('-o' 'compression'); zfs inherit "${o[@]}" "${p}/${v}"
```

- Вернуть свойство `compression` к стандартному наследуемому значению тома `cloud` и во всех его под-томах в пуле `data`:

```bash
p='data'; v='cloud'; o=('-o' 'compression'); zfs inherit -r "${o[@]}" "${p}/${v}"
```
