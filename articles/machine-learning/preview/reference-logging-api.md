---
title: "Azure ML protokolování API – referenční informace | Microsoft Docs"
description: "Protokolování referenční dokumentace rozhraní API."
services: machine-learning
author: akshaya-a
ms.author: akannava
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/25/2017
ms.openlocfilehash: 1906425c6657fb6232a9dc306b05f9171c9c7bef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="logging-api-reference"></a>Protokolování referenční dokumentace rozhraní API

Knihovna Azure ML protokolování umožňuje programu pro vydávání metriky a soubory, které jsou sledovány službou historie pro pozdější analýzu. V současné době jsou podporovány pár základních typů souborů a metriky a sadu podporované typy bude zvýší s budoucími verzemi balíčku Python.

## <a name="uploading-metrics"></a>Odesílání metriky

```python
# import logging API package
from azureml.logging import get_azureml_logger

# initialize a logger object
logger = get_azureml_logger()

# log "scalar" metrics
logger.log("simple integer value", 7)
logger.log("simple float value", 3.141592)
logger.log("simple string value", "this is a string metric")

# log a list of numerical values. 
# this automatically creates a chart in the Run History details page
logger.log("chart data points", [1, 3, 5, 10, 6, 4])
```

Standardně jsou všechny metriky odeslán asynchronně tak, aby odesílání nebude mít dopad na spuštění programu. Při odesílání více metriky v hraniční případech to může způsobit řazení problémy. Příkladem může být dva metriky protokolována ve stejnou dobu, ale z nějakého důvodu, kterou uživatel by raději přesný řazení zachovaná. Jiné případem je při metriku musí být sledována před spuštění některých kódu, který je znám selhat rychlé. V obou případech řešení je _počkejte_ dokud metrika je plně protokolována než budete pokračovat:

```python
# blocking call
logger.log("my metric 1", 1).wait()
logger.log("my metric 2", 2).wait()
```

## <a name="consuming-metrics"></a>Využívání metriky

Metriky jsou uložené ve službě historie a vázané na spuštění, která je vytvořena. Karta Historie spustit i rozhraní příkazového řádku následující příkaz povolí vám umožňuje načíst jejich (a artefakty níže), po dokončení spuštění.

```azurecli
# show the last run
$ az ml history last

# list all past runs
$ az ml history list 

# show a paritcular run
$ az ml history info -r <runid>
```

## <a name="artifacts-files"></a>Artefakty (soubory)

Kromě metriky AzureML umožňuje uživateli sledovat i soubory. Ve výchozím nastavení jsou všechny soubory, které jsou zapsány do `outputs` složku relativně k programu pracovní adresář (složce projektu v kontextu výpočetní) se odešlou do historie služby a sledovat pro pozdější analýzu. Přímý přístup do paměti je, že velikost jednotlivých souborů musí být menší než 512 MB.


```Python
# Log content as an artifact
logger.upload("artifact/path", "This should be the contents of artifact/path in the service")
```

## <a name="consuming-artifacts"></a>Využívání artefaktů

Tisknout obsah artefakt, které je sledováno, uživatel může použít na kartě Historie spusťte pro danou spustit a **Stáhnout** nebo **povýšit** artefaktů, nebo použijte níže rozhraní příkazového řádku k dosažení stejného efektu.

```azurecli
# show all artifacts generated by a run
$ az ml history info -r <runid> -a <artifact/path>

# promote a particular artifact
$ az ml history promote -r <runid> -ap <artifact/prefix> -n <name of asset to create>
```
## <a name="next-steps"></a>Další kroky
- Provede [klasifikace iris tutoria, část 2](tutorial-classifying-iris-part-2.md) zobrazíte protokolování rozhraní API v akci.
- Zkontrolujte [historii běhů použití a metrik Model v Azure Machine Learning Workbench](how-to-use-run-history-model-metrics.md) pochopit podrobnější protokolování rozhraní API pro použití v historii spustit.