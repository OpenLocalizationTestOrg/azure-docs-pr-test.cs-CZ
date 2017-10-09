---
title: "aaaGet začít s předkonfigurovanými řešeními | Microsoft Docs"
description: "Postupujte podle tohoto kurzu toolearn jak toodeploy Azure IoT Suite předkonfigurované řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: a7f46023d26b08de2e8ed48c34c5066a43e3fa38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-preconfigured-solutions"></a>Začínáme s hello předkonfigurované řešení

Azure IoT Suite [předkonfigurovaných řešení] [ lnk-preconfigured-solutions] kombinovat více Azure IoT služby toodeliver začátku do konce řešení, implementující běžné obchodní scénáře IoT. Hello *vzdálené monitorování* předkonfigurované řešení připojí tooand monitorování zařízení. Můžete vytvořit hello řešení tooanalyze hello datový proud ze zařízení a výstupy obchodní tooimprove tím, že procesy reagovat automaticky toothat datový proud.

Tento kurz ukazuje, jak tooprovision hello předkonfigurované řešení vzdáleného monitorování. Je také vás provede procesem hello základní funkce hello předkonfigurované řešení. Mnoho z těchto funkcí můžete přistupovat z řešení hello *řídicí panel* které nasazuje v rámci hello předkonfigurované řešení:

![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-dashboard]

toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.

> [!NOTE]
> Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a>Přehled scénáře

Když nasadíte předkonfigurované řešení vzdáleného sledování hello, je naplněna prostředky, které umožňují toostep prostřednictvím běžný scénář vzdáleného monitorování. V tomto scénáři jsou několik zařízení připojených toohello řešení reporting neočekávané teploty hodnoty. Hello následující části ukazují, jak na:

* Identifikujte zařízení hello odesílání neočekávané teploty hodnoty.
* Konfigurace těchto zařízení toosend podrobnější telemetrie.
* Hello problém vyřešte aktualizací firmwaru hello v těchto zařízeních.
* Ověřte, že vaše akce rozpoznal hello problém.

Klíčové funkce tento scénář je, že můžete provádět tyto akce vzdáleně z řídicí panel řešení hello. Není nutné zařízení toohello fyzický přístup.

## <a name="view-hello-solution-dashboard"></a>Zobrazení řídicí panel řešení hello

řídicí panel řešení Hello umožňuje toomanage hello nasazené řešení. Můžete například zobrazit telemetrická data, přidat zařízení nebo konfigurovat pravidla.

1. Když je hello zřizování dokončeno a dlaždice hello pro předkonfigurované řešení označuje **připravené**, zvolte **spusťte** tooopen vzdálené monitorování řešení portálu na nové kartě.

    ![Spusťte hello předkonfigurované řešení][img-launch-solution]

1. Ve výchozím nastavení, hello portálu řešení zobrazuje hello *řídicí panel*. Můžete procházet tooother oblasti hello portál řešení pomocí hello nabídky na levé straně stránky hello hello.

    ![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-menu]

řídicí panel Hello zobrazuje hello následující informace:

* Mapu, která zobrazuje umístění každého zařízení hello připojen toohello řešení. Při prvním spuštění hello řešení, nejsou 25 simulované zařízení. Hello Simulovaná zařízení jsou implementována jako Azure WebJobs a řešení hello používá hello rozhraní API map Bing tooplot informace na mapě hello. V tématu hello [– nejčastější dotazy] [ lnk-faq] toolearn jak toomake hello dynamické mapy.
* Panel **Historie telemetrie**, na kterém se skoro v reálném čase zobrazuje telemetrie vlhkosti a teploty z vybraného zařízení a agregovaná data (například minimální, maximální a průměrná vlhkost).
* Panel **Historie alarmů**, který zobrazuje události, kdy v poslední době hodnota telemetrie překročila stanovenou mez. V příkladech toohello přidání vytvořené hello předkonfigurované řešení můžete definovat vlastní alarmy.
* Panel **Úlohy**, na kterém se zobrazují informace o plánovaných úlohách. Vlastní úlohy můžete plánovat na stránce **Úlohy správy**.

## <a name="view-alarms"></a>Zobrazení alarmů

panel Historie alarmů Hello ukazuje, že pěti zařízení podávají zprávy vyšší než očekávaný telemetrie hodnoty.

![Historie alarmů úkolů na řídicí panel řešení hello][img-alarms]

> [!NOTE]
> Tyto výstrahy jsou generovány pravidlem, které je součástí hello předkonfigurované řešení. Toto pravidlo vygeneruje výstrahu, pokud hodnota teploty hello odeslaný zařízením překročí 60. Můžete definovat vlastní pravidla a akce výběrem [pravidla](#add-a-rule) a [akce](#add-an-action) v levé nabídce hello.

## <a name="view-devices"></a>Zobrazení zařízení

Hello *zařízení* seznamu jsou uvedeny všechny hello zaregistrovat zařízení v řešení hello. Ze seznamu zařízení hello můžete zobrazit a upravit metadata zařízení přidejte nebo odeberte zařízení a volat metody na zařízení. Můžete filtrovat a řadit hello seznam zařízení v seznamu zařízení hello. Můžete také upravit hello sloupce zobrazené v seznamu zařízení hello.

1. Zvolte **zařízení** tooshow hello seznam zařízení pro toto řešení.

   ![Zobrazení seznamu zařízení hello portálu řešení hello][img-devicelist]

1. seznam zařízení Hello původně ukazuje 25 Simulovaná zařízení, které jsou vytvořené hello procesu zřizování. Můžete přidat další Simulovaná a fyzické zařízení toohello řešení.

1. v seznamu zařízení hello tooview hello podrobnosti o zařízení, vyberte zařízení.

   ![Zobrazení podrobností o zařízeních hello portálu řešení hello][img-devicedetails]

Hello **podrobnosti o zařízení** panel obsahuje šest části:

* Kolekce odkazů, které jste toocustomize hello ikonu zařízení povolit, zakázat hello zařízení, přidat pravidlo, volání metody nebo odeslat příkaz. Porovnání příkazů (zpráv typu zařízení-cloud) a metod (přímých metod) najdete v [doprovodných materiálech ke komunikaci typu cloud-zařízení][lnk-c2d-guidance].
* Hello **dvojče zařízení - značky** oddílu můžete hodnoty značky tooedit hello zařízení. Můžete zobrazit v seznamu zařízení hello hodnoty značky a použít seznam zařízení hello toofilter hodnoty značky.
* Hello **dvojče zařízení - potřeby vlastnosti** oddílu můžete tooset vlastnost hodnoty toobe odeslané toohello zařízení.
* Hello **dvojče zařízení - hlášené vlastnosti** část zobrazuje hodnoty vlastností, odeslané zařízením hello.
* Hello **vlastnosti zařízení** části se zobrazují informace z registru identit hello například hello zařízení id a ověřovací klíč.
* Hello **posledních úloh** části zobrazují informace o všechny úlohy, které jste nedávno zaměřili toto zařízení.

## <a name="filter-hello-device-list"></a>Filtrovat seznam zařízení hello

Toodisplay filtru můžete použít pouze taková zařízení, která odesílají neočekávané teploty hodnoty. Hello předkonfigurovanému řešení vzdáleného monitorování zahrnuje hello **není v pořádku zařízení** filtrovat tooshow zařízení s hodnotou střední teploty větší než 60. Můžete si také [vytvořit vlastní filtry](#add-a-filter).

1. Zvolte **otevřít uložené filtru** toodisplay seznam k dispozici tyto filtry. Zvolte **není v pořádku zařízení** tooapply hello filtru:

    ![Zobrazení hello seznam filtrů][img-unhealthy-filter]

1. seznam zařízení Hello nyní zobrazuje pouze zařízení s střední teploty hodnotu větší než 60.

    ![Seznam Filtrované zařízení hello zobrazení zobrazující zařízení není v pořádku][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a>Aktualizace požadovaných vlastností

Nyní jste identifikovali sadu zařízení, která možná vyžadují opravu. Však můžete rozhodnout, že data frekvenci hello 15 sekund není dostatečná pro zrušte diagnostiku problému hello. Změna hello telemetrie frekvence toofive sekund tooprovide vám další datové body toobetter diagnostikovat problém hello. Vzdálená zařízení tuto konfiguraci změnu tooyour můžete nabízet z portálu řešení hello. Můžete provést změnu hello jednou, vyhodnotit vliv hello a potom na výsledky hello.

Postupujte podle těchto kroků toorun úlohu, která změní hello **TelemetryInterval** potřeby vlastnost hello vliv na zařízení. Když hello zařízení obdrží hello nové **TelemetryInterval** hodnotu vlastnosti, se změní, jejich konfigurace toosend telemetrie každých pět sekund místo každých 15 sekund:

1. Při hello seznam zařízení, není v pořádku zobrazené v seznamu zařízení hello, zvolte **plánovače úloh**, pak **upravit dvojče zařízení**.

1. Volání hello úlohy **interval telemetrie změny**.

1. Změnit hodnotu hello hello **požadovanou vlastnost** název **požadované. Config.TelemetryInterval** toofive sekund.

1. Zvolte **Naplánovat**.

    ![Změnit hello TelemetryInterval vlastnost toofive sekund][img-change-interval]

1. Můžete sledovat průběh hello hello úlohy na hello **úlohy správy** stránku hello portálu.

> [!NOTE]
> Pokud chcete toochange hodnotu požadované vlastnosti pro jednotlivá zařízení, použijte hello **požadované vlastnosti** část v hello **podrobnosti o zařízení** panely namísto spuštění úlohy.

Tato úloha nastaví hodnotu hello hello **TelemetryInterval** potřeby vlastnost hello dvojče zařízení pro všechny hello filtrem hello vybraná zařízení. zařízení Hello načetlo tuto hodnotu z dvojče zařízení hello a aktualizaci jejich chování. Když zařízení načte a zpracuje požadovanou vlastnost z dvojče zařízení, nastaví hello odpovídající hlášené hodnotu vlastnosti.

## <a name="invoke-methods"></a>Vyvolání metod

Při spuštění úlohy hello zjistíte v seznamu hello zařízení není v pořádku, že tato zařízení mají starý (méně než verze 1.6) firmwaru verze.

![Zobrazení hello hlášené verzi firmwaru pro zařízení není v pořádku hello][img-old-firmware]

Tato verze firmwaru může být hello hlavní příčinu hello neočekávané teploty hodnoty, protože víte, že další pořádku zařízení byly nedávno aktualizovaného tooversion 2.0. Můžete použít předdefinované hello **staré firmwaru zařízení** filtrovat tooidentify žádná zařízení s staré verze firmwaru. Z portálu hello může vzdáleně aktualizovat všechna zařízení hello stále spuštěna staré verze firmwaru:

1. Zvolte **otevřít uložené filtru** toodisplay seznam k dispozici tyto filtry. Zvolte **staré firmwaru zařízení** tooapply hello filtru:

    ![Zobrazení hello seznam filtrů][img-old-filter]

1. seznam zařízení Hello nyní zobrazuje pouze zařízení s staré verze firmwaru. Tento seznam obsahuje pět zařízení hello identifikovaný hello **není v pořádku zařízení** filtr a tři další zařízení:

    ![Seznam Filtrované zařízení hello zobrazení zobrazující staré zařízení][img-filtered-old-list]

1. Zvolte **Plánovač úloh** a potom **Vyvolat metodu**.

1. Nastavit **název úlohy** příliš**tooversion aktualizace firmwaru 2.0**.

1. Zvolte **InitiateFirmwareUpdate** jako hello **metoda**.

1. Sada hello **FwPackageUri** parametr příliš**https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.

1. Zvolte **Naplánovat**. Výchozí Hello je nyní pro úlohy toorun hello.

    ![Vytvoření úlohy tooupdate hello firmwaru hello vybrané zařízení][img-method-update]

> [!NOTE]
> Pokud chcete, aby tooinvoke metoda na jednotlivá zařízení, zvolte **metody** v hello **podrobnosti o zařízení** panely namísto spuštění úlohy.

Tato úloha vyvolá hello **InitiateFirmwareUpdate** přímá metoda na všech zařízeních hello vybraný filtr hello. Zařízení reagovat okamžitě tooIoT rozbočovače a poté asynchronně zahájit proces aktualizace firmwaru hello. zařízení Hello poskytují informace o stavu procesu aktualizace firmwaru hello prostřednictvím hodnoty hlášené vlastností, jak je znázorněno v následujícím snímky obrazovky hello. Zvolte hello **aktualizovat** ikonu tooupdate hello informace v seznamech zařízení a úlohy hello:

![Seznam úloh zobrazující hello firmware aktualizace seznamu spuštěná][img-update-1]
![seznam zařízení zobrazuje stav aktualizace firmwaru][img-update-2]
![úlohy seznamu zobrazení hello firmware aktualizace seznamu dokončení][img-update-3]

> [!NOTE]
> V produkčním prostředí můžete naplánovat úlohy toorun během časového období údržby určené.

## <a name="scenario-review"></a>Revize scénáře

V tomto scénáři identifikovat potenciální problém s některými vzdáleného zařízení pomocí hello historie alarmů na řídicím panelu hello a filtr. Můžete potom použít hello filtru a tooremotely úlohy konfigurace hello zařízení tooprovide Další informace o toohelp diagnostikovat problém hello. Nakonec použít filtr a úlohy údržby tooschedule na zařízeních hello vliv. Pokud vrátíte toohello řídicího panelu, můžete zkontrolovat, že již neexistují žádné výstrahy přicházející ze zařízení ve vašem řešení. Můžete použít filtr tooverify, který hello firmware aktuální na všech zařízeních hello ve vašem řešení a jestli je k dispozici nejsou žádné další není v pořádku zařízení:

![Filtr ukazující, že všechna zařízení mají aktuální firmware][img-updated]

![Filtr ukazující, že všechna zařízení jsou v pořádku][img-healthy]

## <a name="other-features"></a>Další funkce

Hello následující části popisují některé další funkce hello předkonfigurovanému řešení vzdáleného monitorování, které nejsou popsané v rámci předchozí scénáře hello.

### <a name="customize-columns"></a>Přizpůsobení sloupců

Můžete přizpůsobit hello informace zobrazené v seznamu zařízení hello výběrem **editor sloupců**. Můžete přidat a odebrat sloupce, které zobrazují hodnoty ohlašovaných vlastností a značek. Můžete také změnit pořadí sloupců a přejmenovat je:

   ![Seznam zařízení sloupec editor uchování hello][img-columneditor]

### <a name="customize-hello-device-icon"></a>Přizpůsobení ikonu zařízení hello

Můžete přizpůsobit ikonu hello zařízení zobrazí v seznamu zařízení hello z hello **podrobnosti o zařízení** panelu následujícím způsobem:

1. Zvolte hello tooopen ikonu tužky hello **úpravy image** panel pro zařízení:

   ![Otevřený editor obrázku zařízení][img-startimageedit]

1. Nahrát novou bitovou kopii, použijte jednu z existujících imagí hello a potom zvolte **Uložit**:

   ![Editor Upravit obrázek zařízení][img-imageedit]

1. Hello bitové kopie, které jste vybrali teď zobrazí v hello **ikonu** sloupec pro hello zařízení.

> [!NOTE]
> Hello bitová kopie uložena v úložišti objektů blob. Značku dvojče zařízení hello obsahuje bitovou kopii odkaz toohello v úložišti objektů blob.

### <a name="add-a-device"></a>Přidání zařízení

Když nasadíte hello předkonfigurované řešení, automaticky zřizovat 25 ukázka zařízení, které se zobrazí v seznamu zařízení hello. Tato zařízení jsou *simulovaná zařízení*, která běží ve webové úloze Azure. Simulovaná zařízení usnadnění pro vás tooexperiment hello předkonfigurované řešení bez hello nutné toodeploy skutečné, fyzické zařízení. Pokud chcete tooconnect řešení toohello skutečné zařízení, najdete v části hello [připojit vaše zařízení toohello předkonfigurovanému řešení vzdáleného monitorování] [ lnk-connect-rm] kurzu.

Hello následující kroky ukazují, jak tooadd řešení toohello simulované zařízení:

1. Přejděte zpět toohello seznam zařízení.

1. tooadd zařízení, zvolte **+ přidat zařízení** v levém dolním rohu hello.

   ![Přidat zařízení toohello předkonfigurované řešení][img-adddevice]

1. Zvolte **přidat nové** na hello **simulované zařízení** dlaždici.

   ![Nastavení podrobností o zařízení na řídicím panelu][img-addnew]

   V toocreating přidání nového simulovaného zařízení, můžete také přidat fyzického zařízení. Pokud se rozhodnete toocreate **vlastní zařízení**. toolearn Další informace o propojení řešení toohello fyzických zařízení, najdete v části [připojit vaše zařízení toohello předkonfigurované řešení vzdáleného sledování IoT Suite][lnk-connect-rm].

1. Vyberte možnost **Ručně definovat vlastní ID zařízení** a zadejte jedinečný název zařízení, například **mydevice_01**.

1. Zvolte **Vytvořit**.

   ![Uložení nového zařízení][img-definedevice]

1. V kroku 3 **přidání simulovaného zařízení**, zvolte **provádí** tooreturn toohello zařízení seznamu.

1. Můžete zobrazit zařízení **systémem** v seznamu zařízení hello.

    ![Zobrazení nového zařízení v seznamu zařízení][img-runningnew]

1. Můžete také zobrazit telemetrii z nového zařízení na řídicím panelu hello simulated hello:

    ![Zobrazení telemetrie z nového zařízení][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a>Zakázání a odstranění zařízení

Zařízení můžete zakázat, zakázané zařízení lze následně odebrat:

![Zakázání a odebrání zařízení][img-disable]

### <a name="add-a-rule"></a>Přidání pravidla

Neexistují žádná pravidla pro nové zařízení hello, že jste právě přidali. V této části přidáte pravidlo, které spustí alarm při hello teplota hlášená tímto hello nové, že zařízení přesáhne hodnotu 47 stupňů. Než začnete, Všimněte si, že historie telemetrie hello hello nové zařízení na řídicím panelu hello zobrazuje hello zařízení teploty nikdy nepřesáhne 45 stupňů.

1. Přejděte zpět toohello seznam zařízení.

1. tooadd pravidlo pro hello zařízení, vyberte nové zařízení v hello **seznam zařízení**a potom zvolte **přidat pravidlo**.

1. Vytvořte pravidlo, které používá **teploty** jako hello datové pole a používá **AlarmTemp** jako hello výstup, když teplota hello přesáhne hodnotu 47 stupňů:

    ![Přidání pravidla pro zařízení][img-adddevicerule]

1. Zvolte změny, toosave **uložit a zobrazit pravidla**.

1. Zvolte **příkazy** v podokně podrobností hello zařízení pro nové zařízení hello.

    ![Přidání pravidla pro zařízení][img-adddevicerule2]

1. Vyberte **ChangeSetPointTemp** ze seznamu příkaz hello a sadu **SetPointTemp** too45. Potom zvolte **Odeslat příkaz**:

    ![Přidání pravidla pro zařízení][img-adddevicerule3]

1. Přejděte zpět toohello řídicího panelu. Po krátkou dobu zobrazí jako nová položka v hello **historie alarmů** když hello teplota hlášená tímto novým zařízením překročí prahovou hodnotu 47 stupňů hello:

    ![Přidání pravidla pro zařízení][img-adddevicerule4]

1. Můžete zkontrolovat a upravit všechna pravidla v hello **pravidla** hello řídicím panelu:

    ![Zobrazení pravidel zařízení][img-rules]

1. Můžete zkontrolovat a upravit všechny hello akce, které můžete provést v pravidle tooa odpovědi na hello **akce** hello řídicím panelu:

    ![Zobrazení akcí zařízení][img-actions]

> [!NOTE]
> Je možné toodefine akce, které může odesílat e-mailovou zprávu nebo SMS v odpovědi tooa pravidla nebo integrovat-obchodní systému prostřednictvím [aplikace logiky][lnk-logic-apps]. Další informace najdete v tématu hello [tooyour připojit aplikace logiky Azure IoT Suite vzdálené monitorování předkonfigurované řešení][lnk-logicapptutorial].

### <a name="manage-filters"></a>Správa filtrů

V seznamu hello zařízení můžete vytvořit, uložit a znovu načíst filtry toodisplay přizpůsobeného seznamu zařízení připojených tooyour rozbočovače. toocreate filtru:

1. Vyberte ikonu pro úpravu filtru hello hello v seznamu zařízení:

    ![Otevřete hello filtru editoru][img-editfiltericon]

1. V hello **filtru editor**, přidat hello pole, operátory a seznam hodnot toofilter hello zařízení. Více klauzulí toorefine můžete přidat filtr. Zvolte **filtru** tooapply hello filtru:

    ![Vytvoření filtru][img-filtereditor]

1. V tomto příkladu je seznam hello filtrovaný podle výrobce a model číslo:

    ![Filtrovaný seznam][img-filterelist]

1. toosave filtr vlastní název, vyberte hello **uložit jako** ikona:

    ![Uložení filtru][img-savefilter]

1. tooreapply filtr jste uložili dřív, zvolte hello **otevřít uložené filtru** ikona:

    ![Otevření filtru][img-openfilter]

Můžete vytvořit filtry na základě ID zařízení, stavu zařízení, požadovaných vlastností, ohlášených vlastností a značek. Přidejte si vlastní zařízení tooa vlastní značky v hello **značky** části hello **podrobnosti o zařízení** panelu nebo spustit úlohu tooupdate značky na několika zařízeních.

> [!NOTE]
> V hello **filtru editor**, můžete použít hello **rozšířené zobrazení** tooedit hello text dotazu přímo.

### <a name="commands"></a>Příkazy

Z hello **podrobnosti o zařízení** panelu, můžete odeslat příkazy toohello zařízení. Při prvním spuštění zařízení, odešle se, že informace o hello příkazy, že podporuje toohello řešení. Informace o hello rozdíly mezi příkazy a metody, najdete v části [možnosti cloud zařízení Azure IoT Hub][lnk-c2d-guidance].

1. Zvolte **příkazy** v hello **podrobnosti o zařízení** panel pro vybrané zařízení hello:

   ![Příkazy zařízení na řídicím panelu][img-devicecommands]

1. Vyberte **PingDevice** hello příkaz seznamu.

1. Zvolte **Odeslat příkaz**.

1. Zobrazí se stav hello hello příkazu v historii příkazů hello.

   ![Stav příkazu na řídicím panelu][img-pingcommand]

Hello řešení sleduje stav každého příkazu, který odešle hello. Zpočátku je výsledek hello **čekající**. Když hello zařízení ohlásí, že se provedla hello příkaz, hello výsledek je nastaven příliš**úspěch**.

## <a name="behind-hello-scenes"></a>Pozadí hello

Když nasadíte předkonfigurované řešení, proces nasazení hello vytvoří několik prostředků v hello předplatné Azure, které jste vybrali. Tyto prostředky můžete zobrazit v hello Azure [portál][lnk-portal]. proces nasazení Hello vytvoří **skupiny prostředků** s názvem na základě názvu hello jste vybrali pro předkonfigurované řešení:

![Předkonfigurované řešení v hello portálu Azure][img-portal]

Hello nastavení každého prostředku můžete zobrazit výběrem v seznamu prostředků ve skupině prostředků hello hello.

Můžete také zobrazit zdrojový kód hello hello předkonfigurované řešení. Hello vzdálené monitorování zdrojový kód předkonfigurovaného řešení je v hello [azure-iot-remote-monitoring] [ lnk-rmgithub] úložiště GitHub:

* Hello **DeviceAdministration** složka obsahuje hello zdrojový kód pro řídicí panel hello.
* Hello **simulátoru** složka obsahuje hello zdrojový kód pro simulované zařízení hello.
* Hello **EventProcessor** složka obsahuje zdrojový kód hello hello back endový proces, který zpracovává příchozí telemetrii hello.

Až skončíte, můžete odstranit hello předkonfigurované řešení ze svého předplatného Azure na hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality. Tento web můžete tooeasily odstranit všechny prostředky, které byly zřízeny při vytváření hello předkonfigurované řešení hello.

> [!NOTE]
> tooensure odstraňte všechny položky související s toohello předkonfigurované řešení, odstraňte jej na hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality a neodstraňujte skupinu prostředků hello hello portálu.

## <a name="next-steps"></a>Další kroky

Teď, když máte ukázku nasazenou pracovní předkonfigurované řešení, budete pokračovat, Začínáme se službou IoT Suite načtením hello následující články:

* [Návod pro předkonfigurované řešení vzdáleného monitorování][lnk-rm-walkthrough]
* [Připojit vaše zařízení toohello předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm]
* [Oprávnění na webu azureiotsuite.com hello][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-faq.md