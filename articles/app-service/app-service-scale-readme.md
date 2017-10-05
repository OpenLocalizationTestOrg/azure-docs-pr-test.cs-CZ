---
title: "Aplikační služba Azure: Škálování aplikací služby App Service"
description: "Zjistěte, výpisů a in nastavení velikosti aplikace ve službě App Service."
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
ms.openlocfilehash: 4ebaafe69fc1f91c7890611b14e8600df5c40cde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a>Aplikační služba Azure: Škálování aplikací služby App Service
Aplikace hostované v Azure App Service můžete [dosáhnout masivním měřítku](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Škálování aplikace je však složitý problém, který nemá řešení "velikosti, která vyhoví všechny". Správně škálování aplikace existuje jsou 3 klíčové oblasti, budou tyto rovněž přispívat k úspěchu aplikace:

1. Princip architektuře aplikace a její slabá místa.
   * Je vaše aplikace Stateful? Bezstavové?
   * Co jsou všechny součásti aplikace?
     * Kde jsou kritická místa v aplikaci?
   * Při načítání do vaší aplikace, co by došlo k přerušení první?
2. Seznámení s požadavky na očekávané zátěže a výkonu.
   * Aplikace musí poskytovat tisíc uživatelů? nebo jeden milión?
   * Vrátí se provoz z jednoho geografické umístění nebo globálně?
   * Existují sezónních odchylek? provoz vrcholů?
   * Jak rychle musí odpovídat aplikace? 1 sekunda? 1 milisekundu?
3. Princip fungování a způsob správně využít platformou hostování vaší aplikace.
   * Jaké funkce by je měli využít k dosažení Moje cíle škálování?

Tato část vám pomůže porozumět faktory a navrhnout strategii, která využívá potřebné funkce služby App Service, abyste dosáhli svých cílů škálovatelnost nápovědy.

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

