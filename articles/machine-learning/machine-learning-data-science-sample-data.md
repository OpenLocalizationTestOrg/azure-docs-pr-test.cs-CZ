---
title: "Ukázková data v kontejnerech objektů blob v Azure, SQL Server a tabulek Hive | Microsoft Docs"
description: "Jak chcete dynamicky prozkoumávat data uložená v různých enviromnents Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 80a9dfae-e3a6-4cfb-aecc-5701cfc7e39d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 0683be564a88ef54882e8d882d196851ecac243d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="heading"></a>Ukázková data v kontejnerech objektů blob v Azure, SQL Server a tabulek Hive
Tento dokument odkazy na témata, která vysvětluje postup ukázková data, která je uložená v jednom ze tří různých Azure umístění:

* **K datům kontejneru objektů blob v Azure** je vzorkovat stáhnout prostřednictvím kódu programu a potom ho vzorkování s ukázkový kód Python.
* **Data systému SQL Server** je shromažďovat vzorky pomocí SQL a programovací jazyk Python. 
* **Hive dat v tabulce** je vzorkovat pomocí dotazů Hive.

Následující **nabídky** odkazy na témata, které popisují, jak ukázková data ze všech těchto prostředích úložiště Azure. 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Tato úloha vzorkování je krok v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

**Proč vzorová data?**

Pokud je velké datové sady, které chcete analyzovat, je obvykle vhodné nižší ukázková data, která mají snížit velikost menší, ale reprezentativní a lepší správu bitlockeru. To usnadňuje pochopení dat, zkoumání a funkce inženýrství. Jeho role v procesu Cortana Analytics je umožnit rychlé vytváření prototypů zpracování dat funkcí a modelů strojového učení.

