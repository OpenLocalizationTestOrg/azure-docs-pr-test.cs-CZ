---
title: aaaAutoscaling a v1 App Service Environment
description: "Automatické škálování a služby App Service Environment"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 1a03cf494309e80596b64471d1a067b2f64a9fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a>Automatické škálování a služby App Service Environment v1

> [!NOTE]
> Tento článek je o hello App Service Environment v1.  Není k dispozici novější verze hello služby App Service Environment, je snazší toouse a běží na výkonnější infrastruktury. Další informace o nové verzi hello začínat hello toolearn [toohello Úvod App Service Environment](../app-service/app-service-environment/intro.md).
> 

Podpora prostředí Azure App Service *automatické škálování*. Můžete fondy škálování jednotlivých pracovních procesů na základě metriky nebo plán.

![Možnosti škálování fondu pracovních procesů.][intro]

Automatické škálování optimalizuje vaše využití prostředků pomocí automatické zvětšování a zmenšování toofit prostředí App Service váš profil a rozpočet nebo zatížení.

## <a name="configure-worker-pool-autoscale"></a>Konfigurace pracovního procesu škálování fondu
Funkce škálování hello můžete přistupovat z hello **nastavení** kartě hello fondu pracovních procesů.

![Karta nastavení fondu pracovních procesů hello.][settings-scale]

Z tohoto místa hello rozhraní by měl být poměrně obeznámeni, protože je hello plánování stejné prostředí, které vidíte při škálování App Service. 

![Nastavení ruční škálování.][scale-manual]

Můžete také nakonfigurovat profilem škálování.

![Nastavení automatického škálování.][scale-profile]

Profilů automatického škálování jsou užitečné tooset omezení pro vaše škálování. Tímto způsobem může mít konzistentní výkon prostředí nastavením hodnotu dolní mez rozsahu (1) a předvídatelný výdaji cap nastavením horní mez (2).

![Nastavení škálování v profilu.][scale-profile2]

Po definování profilu, můžete přidat tooscale pravidel škálování nahoru nebo dolů hello počet instancí ve fondu pracovních procesů hello v rámci hranice hello definované hello profilem. Pravidla automatického škálování jsou založené na metriky.

![Pravidlo škálování.][scale-rule]

 Všechny fondu pracovních procesů nebo front-end metriky můžou být použité toodefine škálování pravidla. Tyto metriky jsou stejné metriky můžete sledovat v hello prostředků okno grafů nebo nastavit výstrahy pro hello.

## <a name="autoscale-example"></a>Příklad škálování
Škálování služby App Service environment lze ukázat nejlépe ve scénáři s návodem.

Tento článek vysvětluje všechny požadavky nutné hello při nastavení automatického škálování. Hello článek vás provede procesem hello interakce, které začalo přehrát při zohlednit v automatické škálování služby App Service Environment, které jsou hostované v App Service Environment.

### <a name="scenario-introduction"></a>Scénář Úvod
František se správce systému pro organizace, který migroval část hello úlohy, že spravuje tooan služby App Service environment.

Hello App Service environment není nakonfigurováno toomanual škálování takto:

* **Front-zakončení:** 3
* **Fond pracovních procesů 1**: 10
* **Fond pracovních procesů 2**: 5
* **Fond pracovních procesů 3**: 5

Fond pracovních procesů 1 se používá pro úlohy v produkčním prostředí při fondu pracovních procesů 2 a fondu pracovních procesů 3 se používají pro zajištění kvality (QA) a vývoj úlohy.

plánů pro kontrolu kvality a vývojářů jsou zprostředkovatele Hello služby App Service nakonfigurované toomanual škálování. produkční Hello plán služby App Service se nastavuje tooautoscale toodeal s rozdíly v zatížení a provozu.

Jan je velmi dobře známé s aplikací hello. Ví, že hello hodiny špičky pro zatížení jsou mezi 9:00:00 a 18:00:00, protože to je – obchodní (LOB) aplikace, která zaměstnanci používají, když jsou ve hello office. Využití zahodí po, když se uživatelé provádějí pro daný den. Mimo dobu ve špičce přetrvává některé zatížení vzhledem k tomu, že uživatelé mají přístup ke hello aplikaci vzdáleně pomocí jejich mobilní zařízení nebo domácích počítačů. produkční Hello, plán služby App Service je již nakonfigurován tooautoscale podle využití procesoru s hello následující pravidla:

![Konkrétní nastavení pro obchodní aplikace.][asp-scale]

| **Škálování profil – dny v týdnu – plán služby App Service** | **Škálování profil – víkendů – plán služby App Service** |
| --- | --- |
| **Název:** profil den v týdnu |**Název:** víkendu profilu |
| **Škálování podle:** pravidla plánu a výkonu |**Škálování podle:** pravidla plánu a výkonu |
| **Profil:** dny v týdnu |**Profil:** víkendu |
| **Typ:** opakování |**Typ:** opakování |
| **Cílový rozsah:** 5 too20 instancí |**Cílový rozsah:** 3 too10 instancí |
| **Počet dnů:** pondělí, úterý, středu, čtvrtek a pátek |**Počet dnů:** sobota, neděle |
| **Čas spuštění:** 9:00:00 |**Čas spuštění:** 9:00:00 |
| **Časové pásmo:** UTC-08 |**Časové pásmo:** UTC-08 |
|  | |
| **Pravidlo automatického škálování (vertikálně navýšit kapacitu)** |**Pravidlo automatického škálování (vertikálně navýšit kapacitu)** |
| **Prostředek:** produkční (služby App Service Environment) |**Prostředek:** produkční (služby App Service Environment) |
| **Metrika:** % využití procesoru |**Metrika:** % využití procesoru |
| **Operace:** větší než 60 % |**Operace:** větší než 80 % |
| **Doba trvání:** 5 minut |**Doba trvání:** 10 minut |
| **Čas agregace:** průměrná |**Čas agregace:** průměrná |
| **Akce:** zvýšit počet 2 |**Akce:** zvýšit počet 1 |
| **Nástrojů dolů (minuty):** 15 |**Nástrojů dolů (minuty):** 20 |
|  | |
| **Pravidlo automatického škálování (škálování dolů)** |**Pravidlo automatického škálování (škálování dolů)** |
| **Prostředek:** produkční (služby App Service Environment) |**Prostředek:** produkční (služby App Service Environment) |
| **Metrika:** % využití procesoru |**Metrika:** % využití procesoru |
| **Operace:** méně než 30 % |**Operace:** méně než 20 % |
| **Doba trvání:** 10 minut |**Doba trvání:** 15 minut |
| **Čas agregace:** průměrná |**Čas agregace:** průměrná |
| **Akce:** snižte počet 1 |**Akce:** snižte počet 1 |
| **Nástrojů dolů (minuty):** 20 |**Nástrojů dolů (minuty):** 10 |

### <a name="app-service-plan-inflation-rate"></a>Míry inflace plán služby App Service
Plány služby App Service, které jsou nakonfigurované tooautoscale učinit s maximální rychlostí za hodinu. Tento kurz můžete vypočítáváno na hello hodnoty zadané v pravidle automatického škálování hello.

Princip fungování a způsob výpočtu hello *míry inflace plán služby App Service* je důležité pro škálování prostředí služby App Service, protože nejsou okamžitou fondu pracovních procesů tooa změny měřítka.

Hello míry inflace plán služby App Service se vypočítává takto:

![Výpočet míry inflace plán služby App Service.][ASP-Inflation]

Podle hello škálování – pravidlo vertikálně navýšit kapacitu pro profil den v týdnu hello hello provozních plán služby App Service:

![Míra inflace plán služby App Service pro dny v týdnu podle škálování – pravidlo vertikálně navýšit kapacitu.][Equation1]

V případě hello hello škálování – pravidlo vertikálně navýšit kapacitu pro profil víkendu hello hello provozních plán služby App Service, by hello vzorec odkazující na:

![Míry inflace plán služby App Service pro víkendů podle škálování – pravidlo vertikálně navýšit kapacitu.][Equation2]

Tato hodnota může také vypočítá operacím vertikální snížení kapacity.

Podle hello škálování – pravidlo škálování dolů profilu hello den v týdnu hello provozních plán služby App Service, to vypadat takto:

![Míra inflace plán služby App Service pro dny v týdnu podle škálování – pravidlo škálování dolů.][Equation3]

V případě hello hello škálování – pravidlo škálování dolů profilu hello víkendu hello provozních plán služby App Service, by hello vzorec odkazující na:  

![Rychlost inflace plán služby App Service pro víkendů podle škálování – pravidlo škálování dolů.][Equation4]

produkční Hello plán služby App Service můžou růst v maximálně osm instancí za hodinu během hello týden a čtyři instance za hodinu během víkendu hello. Můžete ho verzi instance s maximální rychlostí čtyři instancí za hodinu během hello týden a šesti instancí za hodinu během víkendu.

Pokud víc plány služby App Service se hostovaným ve fondu pracovních procesů, máte toocalculate hello *celkovou rychlost inflace* jako součet hello hello inflace rychlost pro všechny hello plánů služby App Service, která bude hostování v tomto fondu pracovních procesů.

![Celková rychlost inflace výpočet pro víc plány služby App Service hostované ve fondu pracovních procesů.][ASP-Total-Inflation]

### <a name="use-hello-app-service-plan-inflation-rate-toodefine-worker-pool-autoscale-rules"></a>Použití hello služby App Service plánování míry inflace toodefine pracovní fond škálování pravidla
Pracovní fondu, které hostují plány služby App Service, které jsou nakonfigurované tooautoscale nutné přidělit vyrovnávací paměť kapacity. vyrovnávací paměť Hello umožňuje toogrow operace škálování hello a podle potřeby zmenšit plán služby App Service. Minimální vyrovnávací paměti Hello můžou být hello vypočítat celkové aplikace služby plánování inflace Rate.

Protože operací škálování služby App Service environment trvat některé tooapply čas, všechny změny by měl účet pro vyžádání další změny, které může nastat, když probíhá operace škálování. tooaccommodate tato čekací doba, doporučujeme vám použít hello počítá celkovou aplikace služby plánování inflace rychlost jako hello minimální počet instancí, které jsou přidány pro každou operaci škálování.

Pomocí těchto informací můžete definovat František hello profil škálování a pravidla:

![Profil pravidel škálování například LOB.][Worker-Pool-Scale]

| **Škálování profil – dny v týdnu** | **Škálování profil – víkendů** |
| --- | --- |
| **Název:** profil den v týdnu |**Název:** víkendu profilu |
| **Škálování podle:** pravidla plánu a výkonu |**Škálování podle:** pravidla plánu a výkonu |
| **Profil:** dny v týdnu |**Profil:** víkendu |
| **Typ:** opakování |**Typ:** opakování |
| **Cílový rozsah:** 13 too25 instancí |**Cílový rozsah:** 6 too15 instancí |
| **Počet dnů:** pondělí, úterý, středu, čtvrtek a pátek |**Počet dnů:** sobota, neděle |
| **Čas spuštění:** 7:00:00 |**Čas spuštění:** 9:00:00 |
| **Časové pásmo:** UTC-08 |**Časové pásmo:** UTC-08 |
|  | |
| **Pravidlo automatického škálování (vertikálně navýšit kapacitu)** |**Pravidlo automatického škálování (vertikálně navýšit kapacitu)** |
| **Prostředek:** pracovní fond 1 |**Prostředek:** pracovní fond 1 |
| **Metrika:** WorkersAvailable |**Metrika:** WorkersAvailable |
| **Operace:** menší než 8 |**Operace:** menší než 3 |
| **Doba trvání:** 20 minut |**Doba trvání:** 30 minut |
| **Čas agregace:** průměrná |**Čas agregace:** průměrná |
| **Akce:** zvýšit počet 8 |**Akce:** zvýšení počtu 3 |
| **Nástrojů dolů (minuty):** 180 |**Nástrojů dolů (minuty):** 180 |
|  | |
| **Pravidlo automatického škálování (škálování dolů)** |**Pravidlo automatického škálování (škálování dolů)** |
| **Prostředek:** pracovní fond 1 |**Prostředek:** pracovní fond 1 |
| **Metrika:** WorkersAvailable |**Metrika:** WorkersAvailable |
| **Operace:** větší než 8 |**Operace:** větší než 3 |
| **Doba trvání:** 20 minut |**Doba trvání:** 15 minut |
| **Čas agregace:** průměrná |**Čas agregace:** průměrná |
| **Akce:** snižte počet 2 |**Akce:** snížit počet 3 |
| **Nástrojů dolů (minuty):** 120. |**Nástrojů dolů (minuty):** 120. |

minimální instancí hello definovaná v profilu pro hello plán služby App Service + vyrovnávací paměti je vypočítána Hello cílový rozsah definovaný v profilu hello.

Maximální rozsah Hello by všechny rozsahy maximální hello pro všechny plány služby App Service hostované ve fondu pracovních procesů hello hello součet.

Hello zvýšení počtu u hello rozšiřování škálování využívajících pravidla musí být sada tooat minimálně 1 X míry inflace plán služby App škálování nahoru.

Snižte počet může být upravenou toosomething mezi 1/2 X nebo 1 X hello míry inflace plán služby App pro škálování směrem dolů.

### <a name="autoscale-for-front-end-pool"></a>Škálování front-endu fondu
Pravidla pro automatické škálování front-endu jsou jednodušší než pro fondy pracovních procesů. Především se stává měli byste  
Ujistěte se, když trvání hello měření a hello cooldown časovače zvážit, že operace škálování na plán služby App Service nejsou okamžitou.

Pro tento scénář František ví, že míra chyb hello zvyšuje po dosažení front-end 80 % využití CPU a nastaví hello škálování instance tooincrease pravidel následujícím způsobem:

![Nastavení automatického škálování front-endu fondu.][Front-End-Scale]

| **Škálování profil – přední končí** |
| --- |
| **Název:** škálování – přední končí |
| **Škálování podle:** pravidla plánu a výkonu |
| **Profil:** každý den |
| **Typ:** opakování |
| **Cílový rozsah:** 3 too10 instancí |
| **Počet dnů:** každý den |
| **Čas spuštění:** 9:00:00 |
| **Časové pásmo:** UTC-08 |
|  |
| **Pravidlo automatického škálování (vertikálně navýšit kapacitu)** |
| **Prostředek:** Front-end fondu |
| **Metrika:** % využití procesoru |
| **Operace:** větší než 60 % |
| **Doba trvání:** 20 minut |
| **Čas agregace:** průměrná |
| **Akce:** zvýšení počtu 3 |
| **Nástrojů dolů (minuty):** 120. |
|  |
| **Pravidlo automatického škálování (škálování dolů)** |
| **Prostředek:** pracovní fond 1 |
| **Metrika:** % využití procesoru |
| **Operace:** méně než 30 % |
| **Doba trvání:** 20 minut |
| **Čas agregace:** průměrná |
| **Akce:** snížit počet 3 |
| **Nástrojů dolů (minuty):** 120. |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
