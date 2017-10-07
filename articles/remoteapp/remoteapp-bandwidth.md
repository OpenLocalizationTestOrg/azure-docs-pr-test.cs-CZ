---
title: "aaaEstimate využití šířky pásma sítě Azure Remoteappu | Microsoft Docs"
description: "Další informace o hello nároky na šířku pásma sítě pro vaše kolekce vzdálené aplikace RemoteApp Azure a aplikace."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 3127f4c7-f532-46c3-ba9b-649f647abec1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 675ee82f26ddb46a3bb3e0ee95ed397e4064e45f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Odhadnout využití šířky pásma sítě Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp používá protokol RDP (Remote Desktop) toocommunicate hello mezi aplikace běžící v hello cloudu Azure a uživatele. Tento článek obsahuje některé základní pokyny můžete tooestimate, které využívání sítě a potenciálně vyhodnotit využití šířky pásma sítě na uživatele Azure RemoteApp.

Odhadnout využití šířky pásma na uživatele je velmi složitý a vyžaduje několik aplikací současně spuštěnou ve scénářích multitasking kde aplikace může mít vliv na druhé strany výkonu na základě jejich poptávky pro šířku pásma sítě. Typ i hello klienta vzdálené plochy (například klienta se systémem Mac a HTML5 klienta) může způsobit výsledky toodifferent šířky pásma. toohelp práce přes tyto komplikace, jsme budete rozdělit scénáře použití hello na řadu hello běžné kategorie tooreplicate reálných scénářů. (Kde scénářem z reálného prostředí hello je samozřejmě směs kategorií a liší se podle uživatele.)

Předtím, než jsme přejít další - Všimněte si, že předpokládáme RDP nabízí dobrý tooexcellent prostředí většina scénářů použití v sítích s latencí menší než 120 ms a šířky pásma více než 5 MB – to je založené na protokolu RDP na možnost toodynamically upravit pomocí sítě k dispozici hello Šířka pásma a hello odhadované potřebám aplikace šířky pásma. Tento článek přejde nad rámec těch, toolook "Většina scénářů použití" hello hraniční síti, které scénáře začíná toounwind a začne toodegrade činnost koncového uživatele.

Nyní podívejte se na následující články hello podrobnosti, včetně tooconsider faktory, základní doporučení a co jsme neobsahovala v našem odhady hello.

* [Jak šířku pásma sítě a kvalitu prostředí pracovní společně?](remoteapp-bandwidthexperience.md)
* [Testování s některé běžné scénáře použití šířky pásma sítě](remoteapp-bandwidthtests.md)
* [Rychlé pokyny, pokud nemáte čas nebo možnost tootest hello](remoteapp-bandwidthguidelines.md)

## <a name="what-are-we-not-including"></a>Co nejsou jsme včetně?
Při kontrole hello navrhované testy a naše doporučení celkové (a admittedly obecná), mějte na paměti, že jsou několika různými faktory, které jsme nebere v úvahu. Například hello uživatelské prostředí komplikace poskytované hello asymetrické povaha nahrávání oproti stažení šířky pásma. Hello asymetrické povaha většinu sítí Wi-Fi kromě ovlivní výkon hello a dojem činnost koncového uživatele hello. Pro scénáře interaktivního může podřízené provoz hello nastavovat nižší, než proti proudu, hello, což může zvýšit počet hello ztraceny rámce video nebo zvuk a proto vliv dojem uživatele hello hello streamování prostředí. Můžete spustit toosee vlastní experimenty, co je vhodné pro případ použití konkrétní a sítě.

I když probereme přesměrování zařízení, jsme není vzít v úvahu hello šířky pásma dopad hello síťový provoz způsobené připojená zařízení, jako je úložiště, tiskárny, skenery, web kamery a dalších zařízení USB. efekt Hello těchto zařízení obvykle dočasně špičky potřebám hello šířky pásma a vyčkat, až se dokončí úloha hello. Ale pokud se to udělá často, že může být poměrně znatelné vyžádání šířky pásma.

Můžeme také se nezabývají jak jeden uživatel může mít vliv na ostatní uživatelé v rámci hello stejné síti. Jeden uživatel využívání video 4 kB na 100 MB/s sítě může například významně ovlivnit jiných uživatelů na stejné sítě při toodo hello stejnou úlohu. Bohužel získá progresivně těžší dopad toodetermine hello souběžné používání toogive běžné nebo zahrnující všechna doporučení o jak hello systém provede na agregaci. Všechny budeme moct je, že hello základní technologie protokolu se využívají hello nejlepší hello dostupnou šířku pásma sítě, ale nemá její omezení.

