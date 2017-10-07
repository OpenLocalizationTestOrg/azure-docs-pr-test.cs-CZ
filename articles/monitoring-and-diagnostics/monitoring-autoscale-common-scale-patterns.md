---
title: "aaaOverview běžných vzorů škálování | Microsoft Docs"
description: "Podívejte se, že některé z běžných vzorů tooauto hello škálování prostředku v Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: fc5bd97852e0af01aa32940c99721ab8e21033ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-common-autoscale-patterns"></a>Přehled běžných vzorů škálování
Tento článek popisuje některé běžné vzory tooscale hello prostředku v Azure.

Azure monitorování automatického škálování se vztahují pouze tooVirtual počítač škálování sady (VMSS), cloudové služby, plány služby app a prostředí app service. 

# <a name="lets-get-started"></a>Umožňuje Začínáme

Tento článek předpokládá, že jste obeznámeni s automatické škálování. Můžete [získat Začínáme sem tooscale prostředku][1]. Hello Toto jsou některé z běžných vzorů škálování hello.

## <a name="scale-based-on-cpu"></a>Škálování podle využití procesoru

Máte webovou aplikaci (/ VMSS/cloudové služby role) a 

- Chcete-li tooscale na více systémů nebo měřítka v závislosti na procesor.
- Kromě toho chcete tooensure je minimální počet instancí. 
- Navíc chcete tooensure, která můžete nastavit maximální limit toohello, počet instancí, které je možné škálovat na.

![Škálování podle využití procesoru][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Škálování jinak o víkendech vs dny v týdnu

Máte webovou aplikaci (/ VMSS/cloudové služby role) a

- Chcete 3 instance ve výchozím nastavení (ve všední dny)
- O víkendech nepředpokládáte přenosy a proto chcete tooscale dolů too1 instance o víkendech.

![Škálování jinak o víkendech vs dny v týdnu][3]

## <a name="scale-differently-during-holidays"></a>Jinak škálování během svátků

Máte webovou aplikaci (/ VMSS/cloudové služby role) a 

- Chcete-li tooscale nahoru/dolů podle využití procesoru ve výchozím nastavení
- Během sváteční sezóny (nebo konkrétní dny, které jsou důležité pro vaši firmu) však má výchozí hello toooverride a mít k dispozici větší kapacitu.

![Jinak na svátků škálování][4]

## <a name="scale-based-on-custom-metric"></a>Škálování podle vlastní metriku

Máte front-end webové a vrstvu rozhraní API, který komunikuje s back-end hello. 

- Chcete tooscale hello rozhraní API vrstvy na základě vlastních událostí v hello front end (Příklad: Chcete tooscale procesu odbavení podle hello počet položek v hello nákupní košík)

![Škálování podle vlastní metriku][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png