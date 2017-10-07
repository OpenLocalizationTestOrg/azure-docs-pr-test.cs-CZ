---
title: "Přehled vytváření připojení aaaAzure IoT Suite | Microsoft Docs"
description: "Popis hello Azure IoT Suite připojen objekt pro vytváření předkonfigurovaného řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 929b5ed41ef7d82c9b4480d02aa3f0db38931919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-connected-factory-preconfigured-solution"></a>Začínáme s hello připojen objekt pro vytváření předkonfigurovaného řešení

Azure IoT Suite [předkonfigurovaných řešení] [ lnk-preconfigured-solutions] kombinovat více Azure IoT služby toodeliver začátku do konce řešení, implementující běžné obchodní scénáře IoT. Hello *připojené factory* předkonfigurované řešení připojí tooand monitorování průmyslových zařízení. Můžete použít hello řešení tooanalyze hello datový proud z vašeho zařízení a toodrive provozní produktivitu a ziskovost.

Tento kurz ukazuje, jak tooprovision hello připojen objekt pro vytváření předkonfigurovaného řešení. Je také vás provede procesem hello základní funkce hello předkonfigurované řešení. Mnoho z těchto funkcí můžete přistupovat z řešení hello *řídicí panel* které nasazuje v rámci hello předkonfigurované řešení:

![Řídicí panel předkonfigurovaného řešení propojené továrny][img-cf-home]

toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.

> [!NOTE]
> Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].
> 
> 

## <a name="provision-hello-solution"></a>Zřízení hello řešení

1. Přihlaste se pomocí svých přihlašovacích údajů účtu Azure tooazureiotsuite.com a klikněte na tlačítko "**+**" toocreate řešení.
2. Klikněte na tlačítko **vyberte** na hello **připojené factory** dlaždici.
3. Zadejte **Název řešení** pro předkonfigurované řešení připojené továrny.
4. Vyberte hello **předplatné** a **oblast** chcete toouse tooprovision hello řešení.
5. Klikněte na tlačítko **vytvořit řešení** toobegin hello procesu zřizování. Tento proces obvykle trvá několik minut toorun.

### <a name="while-you-wait-for-hello-provisioning-process-toocomplete"></a>A počkat, hello zřizování toocomplete procesu

1. Klikněte na dlaždici s řešením hello **zřizování** stavu.
2. Všimněte si hello **stavy zřizování** při nasazování služby Azure ve vašem předplatném Azure.
3. Jakmile bude zřizování dokončeno, hello změny stavu příliš**připraven**.
4. Klikněte na tlačítko hello dlaždice toosee hello informace o řešení v pravém podokně hello.

> [!NOTE]
> Pokud narazíte na problémy nasazení hello předkonfigurované řešení, přečtěte si [oprávnění na webu azureiotsuite.com hello] [ lnk-permissions] a hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md). Pokud hello problémy přetrvávají, vytvořte lístek služby na hello [portál][lnk-portal].

Existují by uživatel očekával toosee podrobnosti, které nejsou uvedené pro vaše řešení? Sdělte nám návrhy na funkce na webu [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## <a name="scenario-overview"></a>Přehled scénáře

Při nasazování hello připojen objekt pro vytváření předkonfigurovaného řešení, je naplněna prostředky, které umožňují toostep prostřednictvím průmyslových běžný scénář. V tomto scénáři připojené několik objektů pro vytváření sestav řešení toohello hello data hodnoty požadované toocompute celkovou efektivitu zařízení (OEE) a klíčových ukazatelů výkonu (KPI). Hello následující části ukazují, jak na:

* Monitorovat továrnu, výrobní linky, celkovou efektivitu zařízení stanic a hodnoty klíčových ukazatelů výkonu
* Analyzovat telemetrická data hello generované z těchto zařízení pomocí Azure časové řady statistiky
* Fungovat na problémy toofix výstrahy

Klíčové funkce tento scénář je, že můžete provádět tyto akce vzdáleně z řídicí panel řešení hello. Není nutné zařízení toohello fyzický přístup.

## <a name="view-hello-solution-dashboard"></a>Zobrazení řídicí panel řešení hello

řídicí panel řešení Hello umožňuje toomanage hello nasazené řešení. Je to hierarchická reprezentace globální konfigurace továrny. Můžete například zobrazit celkovou efektivitu zařízení a klíčové ukazatele výkonu nebo publikovat nové uzly pro výstrahy akcí a telemetrie.

1. Když je hello zřizování dokončeno a dlaždice hello pro předkonfigurované řešení označuje **připraven**, zvolte **spusťte** tooopen portálu připojené vytváření řešení na nové záložce.

    ![Spusťte hello předkonfigurované řešení][img-launch-solution]

1. Ve výchozím nastavení, hello portálu řešení zobrazuje hello *řídicí panel*. oblasti tooother toonavigate hello portál, pomocí hello nabídky na levé straně stránky hello hello.

    ![Řídicí panel předkonfigurovaného řešení propojené továrny][cf-img-menu]

řídicí panel Hello zobrazuje hello následující informace:

* A **objekt pro vytváření seznamu** panel, který ukazuje hello stav, umístění a aktuální konfiguraci produkční v řešení hello. Při prvním spuštění hello řešení, existuje několik simulovaných zařízení. Hello výrobní simulace se skládá ze tří skutečné OPC UA servery na každý řádek produkční simulované úkoly, které sdílet data. Další informace o OPC UA najdete v tématu hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md).
* A **mapy** , zobrazí hello umístění každého zařízení připojené toohello řešení. řešení Hello hello rozhraní API map Bing tooplot informace můžete použít na mapě hello. Pokud je ve vašem předplatném povolené podnikové rozhraní API pro Mapy Bing, tato funkce se použije automaticky. Pokud ne, najdete v části hello [– nejčastější dotazy] [ lnk-faq] toolearn jak toomake hello dynamické mapy.
* Panel **Výstrahy**, na kterém se zobrazují výstrahy generované při překročení konkrétních mezních hodnot pro telemetrii nebo hodnoty celkové efektivity zařízení nebo klíčových ukazatelů výkonu.
* **Celkovou efektivitu vybavení** panel, který ukazuje hello OEE hodnot pro celý podnik hello nebo hello výroby řádku/stanici můžete prohlížíte. Tato hodnota se shromažďují z hello stanice zobrazení toohello podnikové úrovně. Hello OEE obrázku a jeho základní prvky mohou být další analyzovány.
* **Klíčové ukazatele výkonu** panely, které zobrazí počet hello jednotky, které vytváří a energie používané hello celý podnik nebo hello objekt pro vytváření nebo produkční řádku stanici můžete prohlížíte. Tyto hodnoty se shromažďují z úrovně stanice zobrazení toohello enterprise.

## <a name="view-factories"></a>Zobrazení továren

Hello *objekty Factory* panelu ukazuje hello zeměpisné umístění všech objektů Factory hello hello řešení, jejich stav a aktuální konfiguraci výroby. Ze seznamu hello umístění můžete přejít toohello úrovně v hierarchii řešení hello. hypertextové odkazy, které odkazují podrobnosti hello produkční řádků v tomto umístění nejsou Hello řádky v seznamu hello. Je pak možné toodrill do hello výrobní podrobnosti a dolů toohello stanice úrovně zobrazení. Můžete také použít toohello seznam filtrů.

![Továrny v předkonfigurovaném řešení propojené továrny][cf-img-factories] 

1. Hello **objekt pro vytváření panelů** ukazuje hello objekt pro vytváření seznamu pro toto řešení.

2. seznam objekt pro vytváření Hello původně obsahuje šest objektů Factory vytvořené hello procesu zřizování. Můžete přidat další Simulovaná a fyzické zařízení toohello řešení.

3. Podrobnosti o hello tooview objektu pro vytváření, klikněte na tlačítko hello řádek v objektu pro vytváření seznamu hello.

4. Podrobnosti hello tooview produkční řádku, klikněte na řádek hello v seznamu hello.

5. tooview hello publikovaná OPC UA uzly stanice na řádku výrobní hello, klikněte na řádek hello v seznamu hello.

6. Podrobnosti tooview v konkrétním uzlu v hello stanice, klikněte na řádek hello v seznamu hello. Tato akce spustí hello kontextu panel s vizualizacemi Statistika časové řady. Klikněte na tlačítko tyto grafy toodo další analýzu v prostředí Průzkumníka hello Statistika časové řady.

## <a name="view-map"></a>Zobrazení mapy

Pokud vaše předplatné má toohello přístup k rozhraní API map Bing, hello *objekty Factory* mapy ukazuje hello zeměpisné umístění a stav všech objektů Factory hello v řešení hello. toodrill do hello podrobnosti umístění, klikněte na tlačítko Zobrazit na mapě hello umístění hello.

![Mapa v předkonfigurovaném řešení propojené továrny][cf-img-map]

## <a name="view-alerts"></a>Zobrazení výstrah

Hello **výstrahy** panelu ukazuje výstrahy generované z důvodu tooa hlášené hodnoty nebo počítané hodnoty OEE/klíčového ukazatele výkonu vyšší než její nakonfigurovanou prahovou hodnotu. Tento panel zobrazuje výstrahy na každé úrovni hello hierarchii, postupně od hello stanice úrovně zobrazení toohello globální zobrazení. Hello výstrahy obsahují popis hello výstraha, datum, čas, umístění a počet výskytů. Můžete získat přehledy toohello data, která způsobila výstrahu hello pomocí hello datům přehledů časové řady. Hello čas řady statistiky, které data jsou vizualizována ve hello výstrahy, kde je to možné. Pokud jste správce, můžete provést výchozích akcí na hello výstrahy jako:

* Výstraha zavřít hello.
* Potvrdit hello výstrahy.

Volitelně můžete provést složitější akce. Například pro hello přetížení OPC UA uzlu hello sestavení, které může:

* Zobrazit v novém okně prohlížeče webovou stránku s podpůrnými informacemi.
* Zmírnění hello příčinu výstrahy hello voláním metody OPC UA hello zařízení.
* Potlačíte hello dostupnost hello výchozích akcí.

    ![Výstrahy v předkonfigurovaném řešení propojené továrny][cf-img-alerts]

> [!NOTE]
> Tyto výstrahy jsou generovány podle pravidel, které jsou určené v konfiguračním souboru v hello předkonfigurované řešení. Tato pravidla mohou generovat výstrahy, když hello OEE nebo obrázky klíčového ukazatele výkonu nebo OPC UA uzlu hodnoty jsou překročení jejich nakonfigurovanou prahovou hodnotu.

1. Hello **výstrahy panely** ukazuje hello výstrahy generované v tomto řešení.

2. tooview hello podrobnosti výstrahy, klikněte na výstrahu hello panelu hello výstrahy.

3. toofurther analýza dat hello výstrah, klikněte na graf hello v hello výstrahy panely tooopen hello časové řady Statistika explorer prostředí.

4. tooaddress hello výstrahy, jsou k dispozici výstrah panelu hello několik akce. Vyberte příslušnou možnost hello pro vás a klikněte na hello provést akce příkazového tlačítka.

## <a name="view-overall-equipment-efficiency"></a>Zobrazení celkové efektivity zařízení

Sazby OEE hello efektivitu hello výrobní proces pomocí klíče související s provozním provozních parametrů. OEE je oborový standard měr vypočtena vynásobením hello míra dostupnosti, výkonu rychlost a míra kvality: OEE = dostupnosti x výkonu x kvality.

![Celková efektivita zařízení v předkonfigurovaném řešení propojené továrny][cf-img-oee]

1. tooview OEE pro všechny úrovně v hierarchii hello přejděte toohello konkrétní zobrazení, které vyžadujete. Hello OEE pro toto zobrazení zobrazí panelu hello společně s každou hello elementů, které tvoří hello OEE procento.

2. toofurther analyzovat hello OEE pro všechny úrovně v hierarchii dat hello, klikněte na hello OEE, dostupnosti, výkonu nebo procento kvality. Kontext panelu se zobrazí se statistickými časové řady napájený vizualizací, které zobrazuje data z hello poslední hodinu, posledních 24 hodin a posledních 7 dnů.

    ![Vizualizace TSI v předkonfigurovaném řešení propojené továrny][cf-img-tsi-visualization]

3. toofurther analýza dat hello výstrah, klikněte na graf hello výstrah panelu hello. Tato akce otevře prostředí Průzkumníka hello Statistika časové řady.

    ![Průzkumník TSI v předkonfigurovaném řešení propojené továrny][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a>Zobrazení klíčových ukazatelů výkonu

Hello řešení poskytuje dvě klíčové ukazatele výkonu, *jednotky za hodinu* a *energie v kWh*.

![Klíčové ukazatele výkonu v předkonfigurovaném řešení propojené továrny][cf-img-kpi]

1. jednotky tooview za hodinu nebo energie použít pro všechny úrovně v hierarchii hello přejděte toohello konkrétní zobrazení, které vyžadujete. jednotky Hello za hodinu a energie použít zobrazení panelu hello.

2. jednotky tooanalyze za hodinu nebo energie použít pro všechny úrovně v hierarchii hello další, klikněte na tlačítko hello měřidla v hello **klíčové ukazatele výkonu** panelu. Kontext panelu se zobrazí s vizualizacemi čas řady Insights používá technologii umožňuje tooview data z hello poslední hodinu hello posledních 24 hodin a posledních 7 dnů.

## <a name="scenario-review"></a>Revize scénáře

V tomto scénáři můžete sledovat svoje objekty Factory OEE a klíčových ukazatelů výkonu hodnoty, na řídicím panelu hello. Můžete pak použít časové řady Insights tooprovide Další informace o toohelp k podrobnostem další hello telemetrická data pro OEE a klíčových ukazatelů výkonu toohelp s detekce anomálií. Také použít hello výstrahy panely tooview problémy s vaší továrny a použít hello akce k dispozici tooyou tooresolve hello výstrahy.

## <a name="other-features"></a>Další funkce

Hello následující části popisují některé další funkce, které nejsou popsané v předchozím scénáři hello hello připojen objekt pro vytváření řešení.

## <a name="apply-filters"></a>Použití filtrů

1. Klikněte na tlačítko hello **dvojitou** toodisplay seznam k dispozici tyto filtry na buď hello factory umístění panelu nebo hello výstrahy.

2. Hello filtry panelu se zobrazí za vás. 

    ![Filtry v předkonfigurovaném řešení propojené továrny][cf-img-alert-filter]

3. Zvolte hello filtr, který požadujete. Je také možné tootype libovolný text do pole filtru hello.

4. Filtr Hello se použije pro vás. Stav filtru Hello se také zobrazí v řídicí panel hello prostřednictvím trychtýřového grafu, který se zobrazí v hello továrny a výstrahy tabulky.

    ![Filtry v předkonfigurovaném řešení propojené továrny][cf-img-alert-filter-funnel]

    > [!NOTE]
    > Aktivní filtr nemá vliv na hodnoty OEE a klíčového ukazatele výkonu hello zobrazí, jenom filtruje hello zobrazovat obsah.

5. tooclear filtr, klikněte na hello trychtýřového grafu a klikněte na tlačítko filtru panelu kontextu filtru hello. Hello text **všechny** se zobrazí v hello továrny a výstrahy tabulky.

## <a name="browse-an-opc-ua-server"></a>Procházení serveru OPC UA

Když nasadíte hello předkonfigurované řešení, automaticky zřizovat simulované OPC UA servery, které můžete vyhledat pomocí prohlížeče řešení hello. Tyto servery jsou *simulované servery OPC UA*. Simulované servery usnadnění pro vás tooexperiment hello předkonfigurované řešení bez hello nutné toodeploy skutečné, fyzické servery. Pokud chcete tooconnect skutečná řešení toohello serveru OPC UA, najdete v části hello [připojit vaše OPC UA zařízení toohello připojené objekt pro vytváření předkonfigurovaného řešení] [ lnk-connect-cf] kurzu.

1. Klikněte na tlačítko hello **factory ikonu** hello řídicí panel navigačním panelu.

    ![Prohlížeč serveru v předkonfigurovaném řešení propojené továrny][cf-img-server-browser]

2. Zvolte jeden ze serverů hello hello předkonfigurované seznamu. Tento seznam obsahuje hello servery, které jsou nasazeny pro vás v hello předkonfigurované řešení.

    ![Výběr serveru v předkonfigurovaném řešení propojené továrny][cf-img-server-choice]

3. Klikněte na **Připojit**, zobrazí se dialogové okno zabezpečení. Pro simulaci hello je bezpečné tooclick **pokračovat**

4. tooexpand hello uzly ve stromu hello serveru, klikněte na něj. Vedle uzlů, které publikují telemetrii, je značka.

    ![Stromová struktura serveru v předkonfigurovaném řešení propojené továrny][cf-img-server-tree]

5. Klikněte pravým tlačítkem myši položku tooread, zápis, publikovat nebo volání tento uzel. k dispozici tooyou Hello akce závisí na oprávnění a atributy hello hello uzlu. Hello přečíst možnost toodisplays panelu kontextu zobrazující hello hodnotu hello konkrétním uzlu. Hello zápisu zobrazí možnost kontextu panel, kde můžete zadat novou hodnotu. možnost volání Hello zobrazuje uzel, kde můžete zadat parametry hello hello volání.

## <a name="publish-a-node"></a>Publikování uzlu

Když přejdete *simulované serveru OPC UA*, můžete také zvolit toopublish nové uzly. Můžete analyzovat hello telemetrie z tyto uzly v řešení hello. Tyto *simulated OPC UA servery* zkontrolujte snadno tooexperiment hello předkonfigurované řešení bez nasazení skutečné, fyzické zařízení.

1. Procházejte tooa uzel ve stromu prohlížeče server OPC UA chcete toopublish hello.

2. Klikněte pravým tlačítkem na uzel hello.

3. Zvolte **Publikovat**.

    ![Publikování uzlu v propojené továrně][cf-img-publish-node]

4. Kontext panelu se zobrazí, která sděluje, že hello publikování proběhla úspěšně. Hello uzel se objeví v zobrazení úrovně hello stanice s zaškrtnutí u ní.

    ![Úspěšné publikování v předkonfigurovaném řešení propojené továrny][cf-img-publish-success]

## <a name="command-and-control"></a>Příkazy a ovládání

vytváření připojené Hello umožňuje příkazů a řídit zařízení odvětví přímo z cloudu hello. Pomocí této funkce toorespond tooalerts generován zařízením hello. Například může odeslat příkaz toohello zařízení z cloudu hello. Můžete najít hello dostupné příkazy v hello **StationCommands** uzel v hello OPC UA servery prohlížeče stromu. V tomto scénáři otevřete ventil verze tlak na stanici sestavení hello produkční řádku v Mnichově. Příkazy a ovládání funkce hello toouse musí být v hello **správce** role pro hello předkonfigurované řešení nasazení.

1. Procházet toohello **StationCommands** uzel v hello OPC UA server prohlížeče stromu.

2. Vyberte příkaz hello chcete použít.

3. Klikněte pravým tlačítkem na uzel hello.

4. Zvolte **Volání**.

    ![Příkaz volání v předkonfigurovaném řešení propojené továrny][cf-img-call-command]

5. Kontext panelu se zobrazí informací, jakou metodu jste o toocall a všechny podrobnosti parametrů se vztahuje.

6. Zvolte **Volání**.

    ![Místní panel volání v předkonfigurovaném řešení propojené továrny][cf-img-call-context]

7. panel kontextu Hello je aktualizovaný tooinform že hello volání metody, které bylo úspěšné. Můžete ověřit hello volání úspěšné načtením hello hodnota hello přetížení uzlu, který aktualizován v důsledku volání hello.

    ![Úspěšné volání v předkonfigurovaném řešení propojené továrny][cf-img-call-success]


## <a name="behind-hello-scenes"></a>Pozadí hello

Když nasadíte předkonfigurované řešení, proces nasazení hello vytvoří několik prostředků v hello předplatné Azure, které jste vybrali. Tyto prostředky můžete zobrazit v hello Azure [portál][lnk-portal]. proces nasazení Hello vytvoří **skupiny prostředků** s názvem na základě názvu hello jste vybrali pro předkonfigurované řešení:

![Předkonfigurované řešení v hello portálu Azure][img-cf-portal]

Hello nastavení každého prostředku můžete zobrazit výběrem v seznamu prostředků ve skupině prostředků hello hello.

Můžete také zobrazit zdrojový kód hello hello předkonfigurované řešení. Hello připojené objekt pro vytváření předkonfigurovaného řešení zdrojový kód je v hello [azure-iot připojené factory] [ lnk-cfgithub] úložiště GitHub:

Až skončíte, můžete odstranit hello předkonfigurované řešení ze svého předplatného Azure na hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality. Tento web můžete tooeasily odstranit všechny prostředky, které byly zřízeny při vytváření hello předkonfigurované řešení hello.

> [!NOTE]
> tooensure odstraňte všechny položky související s toohello předkonfigurované řešení, odstraňte jej na hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality. Neodstraňujte skupinu prostředků hello hello portálu.

## <a name="next-steps"></a>Další kroky

Teď, když máte ukázku nasazenou pracovní předkonfigurované řešení, budete pokračovat, Začínáme se službou IoT Suite načtením hello následující články:

* [Průvodce předkonfigurovaným řešením propojené továrny][lnk-rm-walkthrough]
* [Připojit vaše zařízení toohello připojené objekt pro vytváření předkonfigurovaného řešení][lnk-connect-cf]
* [Oprávnění na webu azureiotsuite.com hello][lnk-permissions]

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md