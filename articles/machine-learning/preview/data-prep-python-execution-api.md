---
title: "Podrobný průvodce o tom, jak používat Azure Machine Learning Data přípravy spuštění rozhraní API | Microsoft Docs"
description: "Tento dokument obsahuje informace o provádění dříve určený zdroje dat a dat přípravy balíčků"
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
ms.openlocfilehash: 488fe5697992bf21f9b748d272a6bda50eb1eb90
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="execute-data-sources-and-data-preparations-packages-from-python"></a>Spustit zdroje dat a dat přípravy balíčky z Pythonu

Použití balíček Azure Machine Learning Data zdroje nebo Azure Machine Learning Data přípravy v rámci Python:

1. Přejděte na **Data** kartě projektu.

2. Klikněte pravým tlačítkem na příslušné zdroje.

3. Zvolte **generovat soubor dat přístupový kód.**

Tato akce vytvoří krátký skript v jazyce Python, která se spustí balíček a vrátí dataframe.

## <a name="data-sources"></a>Zdroje dat

`azureml.dataprep.datasource` Modul obsahuje jedinou funkci provést zdroj dat a vrátíte se dataframe: `load_datasource(path, secrets=None, spark=None)`.
- `path`je cesta ke zdroji dat (soubor .dsource).
- `secrets`je volitelné slovník, který mapuje klíče tajných klíčů.
- `spark`je volitelné bool, která určuje, jestli se mají vracet Spark dataframe nebo Pandas dataframe. Ve výchozím nastavení určuje Azure Machine Learning Workbench, jaký druh dataframe vrátit za běhu na základě kontextu.

## <a name="data-preparations-packages"></a>Balíčky přípravy dat

`azureml.dataprep.package` Modul obsahuje tři funkce, které provedení datový tok od přípravy Data balíčku.

### <a name="execution-functions"></a>Spuštění funkce

- `submit(package_path, dataflow_idx=0, secrets=None, spark=None)`odešle tok zadaná data pro zpracování, ale nevrací dataframe.
- `run(package_path, dataflow_idx=0, secrets=None, spark=None)`Spustí zadaný datový tok a vrátí výsledky jako dataframe.
- `run_on_data(user_config, package_path, dataflow_idx=0, secrets=None, spark=None)`Spustí zadaný datový tok na základě zdroje dat v paměti a vrátí výsledky jako dataframe. `user_config` Argument je slovník, který mapuje absolutní cestu ke zdroji dat (soubor .dsource) a zdroji dat v paměti vyjádřené seznam seznamů.

### <a name="common-arguments"></a>Běžné argumenty

- `package_path`je cesta k přípravě Data balíčku (.dprep).
- `dataflow_idx`je index založený na nule které toku dat v balíčku k provedení. Pokud zadaný datový tok odkazuje na jiné datové toky nebo zdroje dat, jsou spouštěny také.
- `secrets`je volitelné slovník, který mapuje klíče tajných klíčů.
- `spark`je volitelné bool, která určuje, jestli se mají vracet Spark dataframe nebo Pandas dataframe. Ve výchozím nastavení určuje Workbench, jaký druh dataframe vrátit za běhu na základě kontextu.
