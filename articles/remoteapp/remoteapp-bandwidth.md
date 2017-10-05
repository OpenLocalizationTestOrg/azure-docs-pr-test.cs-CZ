---
title: "Odhadnout využití šířky pásma sítě Azure Remoteappu | Microsoft Docs"
description: "Další informace o požadavcích na šířku pásma sítě pro vaše kolekce vzdálené aplikace RemoteApp Azure a aplikace."
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
ms.openlocfilehash: 16b4ba974742d004ea02e3f83e522b9c43f2ef40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Odhadnout využití šířky pásma sítě Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp používá ke komunikaci mezi aplikací, které běží v cloudu Azure a uživatele protokolu RDP (Remote Desktop). Tento článek obsahuje některé základní pokyny, které můžete použít k určení této využití sítě a potenciálně vyhodnotit využití šířky pásma sítě na uživatele Azure RemoteApp.

Odhadnout využití šířky pásma na uživatele je velmi složitý a vyžaduje několik aplikací současně spuštěnou ve scénářích multitasking kde aplikace může mít vliv na druhé strany výkonu na základě jejich poptávky pro šířku pásma sítě. I typ klienta vzdálené plochy (například klienta se systémem Mac a HTML5 klienta) může vést k výsledky různých šířky pásma. K usnadnění práce přes tyto komplikace, jsme budete scénáře použití rozdělit na některé běžné kategorie k replikaci reálných scénářů. (Kde scénářem z reálného prostředí je samozřejmě směs kategorií a liší se podle uživatele.)

Předtím, než jsme přejít další - Všimněte si, že předpokládáme RDP poskytuje dobrou vynikající prostředí pro většinu scénářů použití v sítích s latencí menší než 120 ms a šířky pásma více než 5 MB – to je založené na protokolu RDP na schopnost dynamicky upravit pomocí dostupnou šířku pásma sítě a šířku pásma odhadované aplikace potřebuje. Tento článek přejde nad rámec těch, "Většina scénářů použití" podívat se na hranici, kde začít unwind scénáře a činnost koncového uživatele začne snižovat.

Základní doporučení a co jsme neobsahovala v našem odhady v následujících článcích podrobnosti, včetně faktory vzít v úvahu, teď rezervovat.

* [Jak šířku pásma sítě a kvalitu prostředí pracovní společně?](remoteapp-bandwidthexperience.md)
* [Testování s některé běžné scénáře použití šířky pásma sítě](remoteapp-bandwidthtests.md)
* [Rychlé pokyny, pokud nemáte čas nebo možnosti testování](remoteapp-bandwidthguidelines.md)

## <a name="what-are-we-not-including"></a>Co nejsou jsme včetně?
Při kontrole navrhované testy a naše doporučení celkové (a admittedly obecná), mějte na paměti, že jsou několika různými faktory, které jsme nebere v úvahu. Například komplikace činnost koncového uživatele, poskytované asymetrické povaha nahrávání oproti stáhne šířky pásma. Asymetrické povaha většinu sítí Wi-Fi kromě ovlivní výkon a dojem činnost koncového uživatele. Pro interaktivní scénáře může být nižší, než nadřazený, což může zvýšit počet ztraceny video nebo zvuk rámců a proto vliv dojem uživatele streamování prostředí nastavovat podřízené provoz. Můžete spustit vlastní experimenty a zjistěte, co je vhodné pro případ použití konkrétní a sítě.

I když probereme přesměrování zařízení, jsme není vzít v úvahu dopad šířky pásma síťového provozu způsobené připojená zařízení, jako je úložiště, tiskárny, skenery, web kamery a dalších zařízení USB. Platnost těchto zařízení obvykle dočasně špičky potřebám šířky pásma a vyčkat po dokončení úlohy. Ale pokud se to udělá často, že může být poměrně znatelné vyžádání šířky pásma.

Můžeme také se nezabývají jak jeden uživatel může mít vliv na ostatní uživatelé ve stejné síti. Například jeden uživatel využívání video 4 kB na 100 MB/s sítě může významně ovlivnit jiných uživatelů na stejné sítě snaží udělat pro stejnou úlohu. Bohužel získá progresivně těžší k určení dopad souběžné používání umožnit společné nebo zahrnující všechna doporučení o jak systému provádí na agregaci. Všechny budeme moct je, že základní technologii protokolu bude co nejlépe využít dostupnou šířku pásma sítě, ale nemá její omezení.

