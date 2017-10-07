---
title: "předkonfigurované řešení aaaPredictive údržby | Microsoft Docs"
description: "Popis prediktivní údržby Azure IoT Suite hello předkonfigurovaného řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 2d09801467d33db6b7d6333fa071aea2bf573f20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Přehled řešení předkonfigurované prediktivní údržby

Hello *prediktivní údržby* [předkonfigurované řešení] [ lnk_preconfigured_solutions] je jedním z hello [Microsoft Azure IoT Suite] [ lnk_iot_suite] předkonfigurovaných řešení. Toto řešení integruje sběr telemetrických dat ze zařízení v reálném čase s prediktivním modelem vytvořeným pomocí služby [Azure Machine Learning][lnk-machine-learning].

S Azure IoT Suite můžete rychle a snadno připojit tooand monitorování prostředky a analyzovat telemetrická data v reálném čase v řídicích panelů a vizualizací. V řešení prediktivní údržby hello hello řídicích panelů a vizualizací poskytují nové informace, které můžete zvýšit efektivitu a výnosy.

## <a name="hello-scenario"></a>Hello scénář

Fabrikam je regionální letecká společnost, která se zaměřuje na pohodlí zákazníků za konkurenční ceny. Jednou z příčin zpoždění letů jsou problémy s údržbou, protože údržba leteckých motorů je velmi náročná. Společnost Fabrikam musí vyhnout poruchám motorů během letu za každou cenu tak, aby ho své stroje pravidelně kontroluje a plány údržby podle plánu tooa. Letadla, moduly neopotřebovávají hello však stejné. Některou údržbu motorů není vždy nutné provádět. Naproti tomu se můžou vyskytnout problémy, kvůli kterým musí letadlo zůstat na zemi, dokud není opravené. Pokud je letadlo v místě kde hello vhodní technici nebo náhradní díly nejsou k dispozici, k těmto problémům může být obzvláště drahé.

Hello motory letadel společnosti Fabrikam jsou vybaveny snímači, které monitorují stav motoru během letu. Společnost Fabrikam používá hello prediktivní údržby řešení toocollect hello data shromážděná během letu hello. Po mnoho let provozu a selháních motorů společnosti Fabrikam datových vědců model, způsob toopredict hello zbývající dobu životnosti (RUL) leteckého motoru. Hello model používá korelace mezi údaji ze čtyř hello modul senzory a motoru a opotřebením motoru který vede tooeventual selhání. Když společnost Fabrikam stále tooperform pravidelné kontroly tooensure zabezpečení, ho teď můžete použít hello modely toocompute hello RUL jednotlivých motorů po každém letu. Hello model používá hello shromažďování telemetrických údajů z hello motorů během letu hello. Společnost Fabrikam teď může předpovídat budoucí selhání a s předstihem plánovat údržbu a opravy.

> [!NOTE]
> Hello model řešení využívá data o opotřebení ze skutečných motorů.

Podle predikci hello bodu čas potřebné údržby, může společnost Fabrikam optimalizovat náklady tooreduce její operace.

Koordinátoři údržby spolupracují s plánovači při:

- Plán údržby toocoincide s letadel v konkrétních místech zastavení.
- Zkontrolujte, zda dostatek času k dispozici pro hello letadla toobe mimo provoz aniž by to způsobilo harmonogramu.
- tooensure technici tooschedule že servis letadla bude probíhat efektivně bez prodlev.

Plán údržby dostávají také správci řízení zásob, kteří díky tomu mohou optimalizovat proces objednávání a skladové zásoby náhradních dílů.

Tyto aktivity povolte prostoje letadel společnosti Fabrikam toominimize a snížení provozních nákladů a zajistit bezpečnost cestujících i posádek hello.

toounderstand jak [Azure IoT Suite] [ lnk_iot_suite] poskytuje zákazníkům hello potřebovat toorealize hello potenciál prediktivní údržby, zkontrolujte tato [infografice] [lnk_infographic].

## <a name="how-hello-predictive-maintenance-solution-is-built"></a>Jak je sestaveno řešení prediktivní údržby hello

řešení Hello používá existující model Azure Machine Learning k dispozici jako šablona tooshow možnosti práce s telemetrickými údaji ze zařízení shromažďovaných prostřednictvím služeb IoT Suite. Společnost Microsoft vytvořila [regresní model] [ lnk_regression_model] leteckého motoru na základě dat o veřejně dostupné<sup>\[1\]</sup>a krok za krokem informace o tom, jak toouse hello modelu.

Hello řešení prediktivní údržby Azure IoT používá regresní model hello vytvořené z této šablony. Hello modelu je nasadit do vašeho předplatného Azure a vystavené prostřednictvím automaticky generovaného rozhraní API. Hello řešení obsahuje podmnožinu hello testování dat, která představují 4 (z celkem 100) moduly a hello 4 (z celkem 21) senzor datových proudů. Tato data jsou dostatečná tooprovide přesné výsledky z hello trained model.

*\[1\] A. Saxena and K. Goebel (2008). „Turbofan Engine Degradation Simulation Data Set“, datové úložiště NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*

## <a name="get-started-with-predictive-maintenance"></a>Začínáme s prediktivní údržbou

Tento kurz ukazuje, jak tooprovision hello řešení prediktivní údržby. Je také vás provede procesem hello základní funkce řešení prediktivní údržby hello. Mnoho z těchto funkcí můžete přistupovat prostřednictvím hello řídicí panel řešení, která nasazuje společně s hello předkonfigurované řešení.

toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.

> [!NOTE]
> Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].

1. Přihlaste se příliš[azureiotsuite.com] [ lnk-azureiotsuite] pomocí vaší Azure přihlašovací údaje účtu a klikněte na tlačítko  **+**  toocreate řešení.
1. Klikněte na tlačítko **vyberte** hello **prediktivní údržby** dlaždici.
1. Zadejte **Název řešení** pro předkonfigurované řešení prediktivní údržby.
1. Vyberte hello **oblast** a **předplatné** chcete toouse tooprovision hello řešení.
1. Klikněte na tlačítko **vytvořit řešení** toobegin hello procesu zřizování. Tento proces obvykle trvá několik minut toorun.

### <a name="wait-for-hello-provisioning-process-toocomplete"></a>Počkejte hello zřizování toocomplete procesu

1. Klikněte na dlaždici s řešením hello **zřizování** stavu.
1. Všimněte si hello **stavy zřizování** při nasazování služby Azure ve vašem předplatném Azure.
1. Jakmile bude zřizování dokončeno, hello změny stavu příliš**připraven**.
1. Klikněte na tlačítko hello dlaždice toosee hello informace o řešení v pravém podokně hello. V tomto podokně můžete spustit hello řešení řídicí panel a přístup hello pracovní prostor Machine Learning.

> [!NOTE]
> Pokud narazíte na problémy nasazení hello předkonfigurované řešení, přečtěte si [oprávnění na webu azureiotsuite.com hello] [ lnk-permissions] a hello [– nejčastější dotazy] [ lnk-faq]. Pokud hello problémy přetrvávají, vytvořte lístek služby na hello [portál][lnk-portal].

Existují by uživatel očekával toosee podrobnosti, které nejsou uvedené pro vaše řešení? Sdělte nám návrhy na funkce na webu [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## <a name="view-hello-solution"></a>Zobrazení hello řešení

Tato část vás provede hello řešení uživatelského rozhraní.

### <a name="predictive-maintenance-dashboard"></a>Řídicí panel prediktivní údržby

Tato stránka ve hello webové aplikaci používá ovládací prvky PowerBI v jazyce JavaScript (viz hello [úložiště vizuálních prvků PowerBI][lnk-powerbi]) toovisualize:

* Hello výstupní data z hello úlohy Stream Analytics v úložišti objektů blob.
* Hello počet každý motor letadla RUL a cyklů.

### <a name="observing-hello-behavior-of-hello-cloud-solution"></a>Sledování chování hello hello cloudového řešení

V hello portálu Azure, přejděte toohello skupinu prostředků s názvem řešení hello jste zvolili tooview zřízené prostředky.

![][img-resource-group]

Při zřizování hello předkonfigurovaného řešení obdržíte e-mail s pracovní prostor Machine Learning toohello odkaz. Můžete také přejít pracovní prostor Machine Learning toohello z hello [azureiotsuite.com] [ lnk-azureiotsuite] stránky zřízeného řešení. Dlaždice je k dispozici na této stránce, když hello řešení v hello **připraven** stavu.

![][img-machine-learning]

Hello portálu řešení uvidíte, že tento ukázkový hello je opatřen čtyři Simulovaná zařízení toorepresent dvě letadla se dvěma motory pro každé letadlo, každý se čtyřmi snímači. Při první návštěvě portálu řešení toohello, dojde k zastavení simulace hello.

![][img-simulation-stopped]

Klikněte na tlačítko **spustit simulaci** toobegin hello simulace. Hello senzor historie, RUL, cykly a RUL historie naplnit hello řídicího panelu.

![][img-simulation-running]

Když RUL je menší než 160 (libovolná hodnota, zvolená pro demonstrační účely), zobrazí se portál řešení hello toohello další symbol upozornění RUL zobrazení. portál řešení Hello také označuje hello leteckého motoru žlutě. Všimněte si, jak hodnoty RUL hello mají obecné klesající trend, ale mívají toobounce nahoru a dolů. Toto chování je výsledkem hello různých délek cyklu a přesnosti modelu hello.

![][img-simulation-warning]

Hello úplné simulaci trvá asi 35 minut toocomplete 148 cyklů. Hello 160 RUL dosáhla prahová hodnota pro hello poprvé v přibližně po 5 minutách a oba motory dosáhl hello prahovou hodnotu v přibližně po 8 minutách.

Hello simulace zpracuje úplnou datovou sadu hello údaji o 148 cyklech a vyrovná na konečné hodnoty RUL a cyklů.

Můžete zastavit hello simulace u libovolné bodu, ale kliknutím na tlačítko **Start simulace** opětovná přehrání hello simulace od začátku hello hello datovou sadu.

## <a name="next-steps"></a>Další kroky

Další informace o tom, jak Azure IoT umožňuje scénáře prediktivní údržby, přečtěte si toolearn [získání hodnoty z hello Internet věcí][lnk_capture_value].

Trvat [návod] [ lnk-predictive-walkthrough] řešení prediktivní údržby hello.

Můžete také prozkoumat některé hello další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:

* [Nejčastější dotazy k sadě IoT Suite][lnk-faq]
* [Zabezpečení IoT z hello pozadí][lnk-security-groundup]

[img-resource-group]: media/iot-suite-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-suite-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-suite-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-permissions]: iot-suite-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/