---
title: "aaaComparing Azure Mobile Engagement s jinými službami podobné Azure"
description: "Porovnání s jinými podobné službami Azure - HockeyApp, AppInsights, centra oznámení Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f114775-3a9a-4dd4-8d59-b10d1da9a68b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0859ae9980d0fa96ffbc0edfbd795ccc58d2c843
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Porovnání Azure Mobile Engagement s jinými službami podobné Azure
Někdy rozšiřuje Hello seznam služeb, které nabízí Microsoft Azure a v některých případech může zajímat, jak se liší od této služby, který jste právě číst nebo uslyšíme Azure Mobile Engagement. Tento článek pokusí tooclear hello přehlednosti a přesměruje je toochoose Azure Mobile Engagement, když je nejvhodnější pro vaše použití. 

Azure Mobile Engagement je služba, konkrétně cílem **pro digitální obchodníci/CMOs** , ale může podle **vlastníka mobilní aplikace nebo vydavatele** kdo chce tooincrease hello využití, uchovávání a možnostmi finančního zhodnocení z jejich mobilních aplikací. 

*Je platformu zapojení uživatelů software jako služba (SaaS), která poskytuje datové přehledy o využití aplikace, segmentaci uživatelů v reálném čase a umožňuje zasílat kontextová nabízená oznámení a zasílání zpráv v aplikaci.* 

Rozdělení tuto definici, máme hello následující klíčové vlastnosti, které se také označuje jeho nabídky jedinečnou hodnotu:

1. **Kontextová nabízená oznámení a zasílání zpráv v aplikaci**
   
   Toto je základní fokus hello produktu hello – provádí cílové a přizpůsobené nabízená oznámení. A pro tento toohappen shromažďujeme bohaté chování analytická data. 
2. **Řízené daty přehledy o využití aplikace**
   
   Poskytujeme křížové platformy sady SDK toocollect hello analýzy chování o uživatelích aplikace hello. Poznámka: hello analýzy chování pojem (vůči analýzy výkonu), protože zaměříme na tom, jak uživatelům hello aplikací pomocí aplikace hello. Shromažďovány základní výkon analytická data o chyby, dojde k chybě atd, ale který je hello fokus jádra není hello produktu. 
3. **Segmentaci uživatelů v reálném čase**
   
   Po shromáždění dat pro vypracování analýzy chování uživatelů aplikace, nám umožňují toosegment cílovou skupinu na základě různých parametrů a shromážděná data tooenable toorun je cílem nabízené kampaně. 
4. **Software jako služba (SaaS):**
   
   Jsme portálu odděleně od portál pro správu Azure hello, což je optimalizované toointeract a zobrazit bohaté analýzy chování o uživatelích aplikace hello a spustit marketingových kampaní nabízených. produkt Hello je cílem tooget, můžete přejít v žádný čas!   

S touto sadou rozdíl v dolním, zde je jak jsme porovnán zdánlivě podobné Azure nabídky - tohoto článku hello neprovádí úrovně porovnání funkcí, ale trvá více toodefine přístup scénáře na základě které produkt funguje nejlépe:

1. [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) je mobilní řešení DevOps hello Microsoft. Pomocí něho můžete distribuovat sestavení toobeta testery, shromažďovat data o chybách a získat zpětnou vazbu od uživatelů. Je integrován se sadou Visual Studio Team Services umožňující snadné nasazení sestavení a integraci pracovních položek. 
   
   je zde fokus Hello na DevOps a shromažďování výkonu analytická data o hello mobilní aplikace. Můžete skončit s integrací obou HockeyApps a Mobile Engagement v aplikaci a že nebude, protože i když některé překrývajících v datech shromážděných hello se fokus základní hello produktů hello se liší a pomáhají v dosažení jiný cíle pro vás.  
2. [Application Insights](../application-insights/app-insights-overview.md) Pokud má vaše mobilní aplikace straně serveru, pak budete používat Application Insights toomonitor hello straně webového serveru vaší aplikace, ale pro hello klientské straně mobilní aplikace, měli byste použít HockeyApp. 
3. [Centra oznámení](https://azure.microsoft.com/services/notification-hubs/) poskytuje odlehčený službu v hello smyslu, že nepotřebujete sady SDK integrovaná v mobilní aplikaci hello a může mít plnou kontrolu nad mobilní aplikace a umožňuje odesílání nabízených oznámení pomocí základních funkcí označování. Tato služba je skvělá pro všechny aplikace vlastníka, který zdroje menší o cílení a více o odesílání komunikaci informace okamžitě tootheir uživatele aplikace (všesměrového vysílání tooa velké naplnění). 
   
   Všimněte si zaměřuje hello při odesílání oznámení neuvěřitelně rychlou základní segmentace funkci. 

Podívejme se na některých scénářích:

1. TIM je součástí týmu digitální marketing hello v úložišti společnosti Adventure Works, která publikuje mobilní aplikace pro uživatele. Cílem je TIM je tooensure, hello uživatelé, kteří stáhnout své mobilní aplikace dál toouse ho a často. To nejenom zvýší brand připojit s uživatele aplikace hello ale taky zvyšuje možnostmi finančního zhodnocení, pokud uživatele aplikace hello nákupech pomocí mobilní aplikace hello. Pro tento Tim bude potřebovat toosend cílové uživatele aplikace toohello oznámení, které budou užitečné, tooencourage je tooopen hello aplikace a vraťte tooit často. Toto je, kde Tim užitečné Mobile Engagement. 
2. Joann je součástí vývojového týmu hello Dobrý den, mobilní aplikace na společnosti Adventure Works a je hledá podrobnosti o produktu toolog o havárií, výjimky a získat výkonu telemetrie z aplikace. Toto je, kde Joann užitečné HockeyApp. Joann může integrovat obou HockeyApp pro svůj vývojáře zaměřuje scénáře a Azure Mobile Engagement pro Tim v hello stejné tooget aplikace hello nejlepší z obou. 
3. Každý s každým je součástí vývojového týmu hello hello mobilních aplikací v síti Contoso zprávy a všechny, které chce je toosend si nejnovější zprávy výstrahy tooall uživatelé bez mnohem segmentace také výstrahy zprávy hello. Toto je, kde každý s každým užitečné centra oznámení. 
   V tomto scénáři je možné, ale během existence hello mobilní aplikace se požadavek toodo mnohem širší segmentace a získání podrobností o chování uživatele aplikace hello. V tomto okamžiku bude mít každý s každým tooevaluate Azure Mobile Engagement. 

toorecap, hello účel Mobile Engagement není právě toocollect analytics – není "ještě jiné Analytics produkt společnosti Microsoft". Jde o odesílání cílové nabízených oznámení a pro tuto cílovou jsme shromažďování dat pro vypracování analýzy chování ale hello fokus zůstává na odesílání nabízených oznámení, které zajistit hello většina uživatelů aplikace toohello smysl, takže není setkat za spam. 

Další podrobnosti – prohlédněte si to [rychlé video](mobile-engagement-overview.md) o Mobile Engagement v nutshell. 

