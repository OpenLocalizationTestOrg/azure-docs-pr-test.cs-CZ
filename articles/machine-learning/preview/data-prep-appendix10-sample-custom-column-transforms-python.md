---
title: "Ukázkové Python pro odvození nové sloupce při přípravě dat Azure Machine Learning | Microsoft Docs"
description: "Tento dokument obsahuje příklady kódu pro jazyk Python pro vytvoření nových sloupců v Azure Machine Learning data přípravy."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: e576d44a854159054d4f7886fe7859ae34875c8f
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="sample-of-custom-column-transforms-python"></a>Ukázka vlastního sloupce transformací (Python) 
Je název Tato transformace v nabídce **přidat sloupec (skript)**.

Před čtením této příloha, přečtěte si [Python rozšiřitelnost přehled](data-prep-python-extensibility-overview.md).

## <a name="test-equivalence-and-replace-values"></a>Testování ekvivalenční a nahraďte hodnoty 
Pokud je hodnota ve sloupci Sloupec1 menší než 4, nový sloupec by měl mít hodnotu 1. Pokud je hodnota ve sloupci Sloupec1 více než 4, nový sloupec má hodnotu 2. 

```python
    1 if row["Col1"] < 4 else 2
```
## <a name="current-date-and-time"></a>Aktuální datum a čas 

```python
    datetime.datetime.now()
```
## <a name="typecasting"></a>Změna typu 
```python
    float(row["Col1"]) / float(row["Col2"] - 1)
```
## <a name="evaluate-for-nullness"></a>Vyhodnocení pro nullness 
Pokud Sloupec1 obsahuje hodnotu null, označte nového sloupce jako **chybný**. Pokud ne, označte ji jako **funkční.** 

```python
    'Bad' if pd.isnull(row["Col1"]) else 'Good'
```
## <a name="new-computed-column"></a>Nový počítaný sloupec 
```python
    np.log(row["Col1"])
```
## <a name="epoch-computation"></a>Výpočet epoch 
Počet sekund od Epoch systému Unix (v případě Sloupec1 je již datum): 
```python
    row["Col1"] - datetime.datetime.utcfromtimestamp(0)).total_seconds()
```

## <a name="hash-a-column-value-into-a-new-column"></a>Hash hodnota sloupce do nového sloupce
```python
    import hashlib
    hash(row["MyColumnToHashCol1"])

```




