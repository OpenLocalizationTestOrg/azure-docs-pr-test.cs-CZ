---
title: "Stav hello aaaMonitor prostředků Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toomonitor hello stav svých prostředků Azure CDN pomocí Azure Resource Health."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0a77e56d2fecae4bde6c83730c05375853a6638a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-hello-health-of-azure-cdn-resources"></a>Sledování stavu hello prostředků Azure CDN
  
Stav Azure CDN prostředku je podmnožinou [stavu prostředků Azure](../resource-health/resource-health-overview.md).  Můžete použít Azure resource health toomonitor hello stav CDN prostředků a přijímat řešitelné pokyny tootroubleshoot problémy.

>[!IMPORTANT] 
>Stav prostředku Azure CDN aktuálně pouze účty pro hello stavu doručování globální a funkcí rozhraní API.  Stav prostředku Azure CDN neověřuje jednotlivých koncových bodů CDN.
>
>Hello signály, které kanálu stav prostředku Azure CDN může být až too15 minut zpožděna.

## <a name="how-toofind-azure-cdn-resource-health"></a>Jak toofind stav prostředku Azure CDN

1. V hello [portál Azure](https://portal.azure.com), procházet tooyour profil CDN.

2. Klikněte na tlačítko hello **nastavení** tlačítko.

    ![Tlačítko Nastavení](./media/cdn-resource-health/cdn-profile-settings.png)

3. V části *podpory a řešení potíží s*, klikněte na tlačítko **stav prostředku**.

    ![Stav prostředku CDN](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
>Můžete také vyhledat CDN prostředky uvedené v hello *stav prostředku* dlaždici v hello *Nápověda a podpora* okno.  Můžete rychle získat příliš*Nápověda a podpora* kliknutím hello v kroužku **?** v hello pravého horního rohu portálu hello.
>
> ![Nápověda a podpora](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a>Azure CDN konkrétní zprávy

Stav prostředku CDN související tooAzure stavy naleznete níže.

|Zpráva | Doporučená akce |
|---|---|
|Vám může mít zastavena, odebráno nebo špatně nakonfigurovaný minimálně jeden koncové body CDN | Vám může mít zastavena, odebráno nebo špatně nakonfigurovaný minimálně jeden koncové body CDN.|
|Je nám líto, hello CDN management service není momentálně k dispozici | Pravidelně kontrolujte stav aktualizací; Pokud problém přetrvává i po hello očekává doba řešení, obraťte se na podporu.|
|Je nám líto, že koncové body CDN může být ovlivněno probíhající problémy s některé z našich poskytovatelů CDN | Pravidelně kontrolujte stav aktualizací; Jak používat hello Poradce při potížích nástroj toolearn tootest počátečního a koncového bodu CDN; Pokud problém přetrvává i po hello očekává doba řešení, obraťte se na podporu. |
|Je nám líto, změny konfigurace koncového bodu CDN pozorují zpoždění šíření | Pravidelně kontrolujte stav aktualizací; Pokud vaše změny konfigurace nebudou rozšířeny plně v čase hello očekávání, obraťte se na podporu.|
|Je nám líto, že nemůžeme mají problémy načítání hello doplňkovém portálu | Pravidelně kontrolujte stav aktualizací; Pokud problém přetrvává i po hello očekává doba řešení, obraťte se na podporu.|
Je nám líto, že nemůžeme mají problémy se některé z našich poskytovatelů CDN | Pravidelně kontrolujte stav aktualizací; Pokud problém přetrvává i po hello očekává doba řešení, obraťte se na podporu. |

## <a name="next-steps"></a>Další kroky

- [Přečtěte si přehled o stavu prostředků Azure.](../resource-health/resource-health-overview.md)
- [Řešení problémů s kompresí CDN](./cdn-troubleshoot-compression.md)
- [Řešení problémů s chyb 404](./cdn-troubleshoot-endpoint.md)