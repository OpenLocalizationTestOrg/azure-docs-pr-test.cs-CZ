---
title: "aaaHow tooscale prostředí Azure časové řady Insights | Microsoft Docs"
description: "Tento kurz se zaměřuje na tom, jak tooscale prostředí Statistika Azure časové řady"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: 55eda388997589185bd34228762b95e182b228ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-your-time-series-insights-environment"></a>Jak tooscale prostředí Statistika časové řady

Tento kurz se zaměřuje na tom, jak tooscale prostředí Statistika časové řady.

> [!NOTE]
> Rozšiřování škálování využívajících typů sku není povoleno. Prostředí s S1 Sku nelze převést do prostředí S2.

## <a name="s1-sku-ingress-rates-and-capacities"></a>Příjem příchozích dat sazby S1 SKU a kapacity

| S1 Kapacita SKU | Příchozí míra | Maximální kapacita úložiště
| --- | --- | --- |
| 1 | 1 GB (1 milion událostí) | (30 milionů událostí) 30 GB za měsíc |
| 10 | 10 GB (10 milionů událostí) | 300 GB za měsíc (300 milión událostí) |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU příjem příchozích dat rychlostí a kapacity

| S2 Kapacita SKU | Příchozí míra | Maximální kapacita úložiště
| --- | --- | --- |
| 1 | 10 GB (10 milionů událostí) | 300 GB za měsíc (300 milión událostí) |
| 10 | 100 GB (100 miliónů událostí) | 3 TB (3 miliardy událostí) za měsíc |

Kapacity lineárně, takže S1 sku s kapacitou 2 podporuje 2 GB (2 miliony) událostí za den příchozího rychlost a 60 GB (60 milión událostí) za měsíc.

## <a name="changing-hello-capacity-of-your-environment"></a>Změna hello kapacitu vašeho prostředí

1. V hello portálu Azure, vyberte text hello prostředí jejichž kapacity chcete toochange.
1. V části nastavení klikněte na tlačítko Konfigurovat.
1. Použijte hello kapacity posuvníku tooselect hello kapacitu, který splňuje požadavky hello sazby příjem příchozích dat a kapacitu úložiště.

## <a name="next-steps"></a>Další kroky

* Ověřte, zda je dostatek nové kapacity hello tooprevent omezení. Další podrobnosti najdete v tématu hello *prostředí může získávání omezeny* části [zde](time-series-insights-diagnose-and-solve-problems.md).
