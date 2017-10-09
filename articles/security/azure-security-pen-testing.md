---
title: "aaaPen testování | Microsoft Docs"
description: "Hello článek obsahuje přehled hello průnikům testování procesu (pentest) a jak provádět pentest proti vaší aplikace běžící v Azure infrastruktury."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: 202c239f46d8693ab7aa85e237235372e743e108
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pen-testing"></a>Testování pera
Jeden z hello užitečných funkcí pomocí Microsoft Azure pro testování aplikací a nasazení je, že nepotřebujete tooput společně toodevelop místní infrastruktury, testování a nasazení aplikací. Všechny infrastruktury hello se stará podle hello služeb platformy Microsoft Azure. Nemáte tooworry o žádanek, získávání a "stáčení a překrývání" vlastní místní hardware.

Toto je skvělým – ale budete potřebovat, že, proveďte běžné zabezpečení toomake kvůli opatrností. Jeden hello věcí, musíte je toodo průnikům testovací hello aplikace nasadit v Azure.

Možná už víte, že Microsoft provádí [průnikům testování naše prostředí Azure](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). To nám pomůže vylepšovat naše platforma a provede naše akce z hlediska vylepšení kontrolních mechanismů pro zabezpečení, Představujeme nové prvky zabezpečení a zlepšení našeho procesy zabezpečení.

Jsme nemáte pera testování vaší aplikace pro vás, ale Chápeme, že se chcete a potřebovat tooperform pera testování na vaše vlastní aplikace. Dobrá vlastnost, je to způsobeno můžete zvýšit zabezpečení hello aplikací, vám pomoci lépe zabezpečit celý ekosystém Azure hello.

Pokud jste pera testování vaší aplikace, může vypadat například toous útoku. Jsme [neustále monitorovat](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) pro vzorce útoků a zahájí o proces reakcí na incidenty, pokud je potřeba. Není můžete a nemá nám pomoci Pokud se spouští reakce na incidenty kvůli tooyour vlastní kvůli testování pera opatrností.

Jaké toodo?

Až budete připraveni toopen testování aplikací hostovaných v Azure, máte možnost příliš[dejte nám vědět](https://portal.msrc.microsoft.com/en-us/engage/pentest). Jakmile víme, že budete toobe provedení specifických testů, jsme nebudou neúmyslně vypnout (třeba blokování hello IP adresu, která testujete z), tak dlouho, dokud testy v souladu s toohello Azure pera testování podmínky a ujednání, které jsou popsané v [Microsoft Cloud Unified průnikům testování pravidla zapojení](https://technet.microsoft.com/en-us/mt784683).
Standardní testy, které můžete provádět, zahrnují:

* Testy na vaše koncové body toouncover hello [aplikace otevřete webový projekt zabezpečení (OWASP) top 10 ohrožení zabezpečení](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Testování argumentu neurčité](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) vaše koncových bodů
* [Kontrola portů](https://en.wikipedia.org/wiki/Port_scanner) vaše koncových bodů

Druh jeden typ testu, který nelze provést žádné je [útok na dostupnost služby (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) útoku. To zahrnuje inicializaci útoku DoS sám sebe nebo provádění souvisejících testy, které může určit, ukazují nebo simulovat jakýkoli typ útoku DoS.

Jste připraveni začít s pera testování vaší aplikace hostované ve službě Microsoft Azure tooget? Pokud ano, head na přes toohello [přehled testovací průnikům](https://technet.microsoft.com/library/mt784683.aspx) (a klikněte na tlačítko hello testování požadavku na hello dolní části stránky hello tlačítko vytvořit. Naleznete zde také další informace o testování podmínky a ujednání a užitečné odkazy na tom, jak můžete sestavy související tooAzure nedostatky zabezpečení nebo jinou službu Microsoft pera hello.
