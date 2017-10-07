---
title: "Aplikační služba Azure: Škálování aplikací služby App Service"
description: "Přečtěte si hello in a výpisů nastavení velikosti aplikace ve službě App Service."
keywords: "app service, azure app service, škálování, škálovatelné, plán služby App Service, náklady služby App Service"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: f403c971-4450-432b-8cea-3eeb426c0147
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2016
ms.author: byvinyal
ms.openlocfilehash: d8cd12f73915a916a75d46da2f751a40d567b189
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a>Aplikační služba Azure: Škálování aplikací služby App Service
Aplikace hostované v Azure App Service můžete [dosáhnout masivním měřítku](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Škálování aplikace je však složitý problém, který nemá řešení "velikosti, která vyhoví všechny". toocorrectly škálování aplikace jsou 3 klíčové oblasti, které přispějí úspěch tooyour aplikace:

1. Princip architektuře aplikace a její slabá místa.
   * Je vaše aplikace Stateful? Bezstavové?
   * Co jsou všechny součásti hello aplikace?
     * Kde jsou hello kritická místa v aplikaci hello?
   * Když je zatížení použité tooyour aplikace, co by došlo k přerušení první?
2. Principy hello očekává požadavky na zatížení a výkon.
   * Potřebuje aplikace hello tooserve tisíc uživatelů? nebo jeden milión?
   * Vrátí se provoz z jednoho geografické umístění nebo globálně?
   * Existují sezónních odchylek? provoz vrcholů?
   * Jak rychle by měl odpovídat hello aplikace? 1 sekunda? 1 milisekundu?
3. Pochopení a správně platformu využívají hello hostování vaší aplikace.
   * Co funkce měli I využít tooachieve Moje cíle škálování?

Tato část vám pomůže porozumět všechny hello faktory a navrhnout strategii, která využívá výhod hello nezbytné služby App Service funkce tooachieve cílů škálovatelnost nápovědy.

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

