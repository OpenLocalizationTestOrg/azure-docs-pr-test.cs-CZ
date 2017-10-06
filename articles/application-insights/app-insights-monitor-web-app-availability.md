---
title: "aaaMonitor dostupnosti a odezvy každého webový server | Microsoft Docs"
description: "Nastavení testů webu ve službě Application Insights. Zasílání upozornění, pokud web přestane být k dispozici nebo reaguje pomalu."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: bwren
ms.openlocfilehash: 4c5425c948770cc57a648ca50e217c75ac75fbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Sledování dostupnosti a odezvy libovolných webů
Poté, co nasadíte webovou aplikaci nebo webové stránky tooany serveru, můžete nastavit toomonitor testy dostupnosti a odezvy. [Azure Application Insights](app-insights-overview.md) odesílá webové žádosti tooyour aplikace v pravidelných intervalech z bodů po hello, world. Upozorní vás v případě, že vaše aplikace reaguje pomalu nebo nereaguje vůbec.

Můžete nastavit testy dostupnosti pro jakékoli HTTP nebo HTTPS koncový bod, který je přístupný z hello veřejného Internetu. Nemáte tooadd cokoli toohello webové stránky, které testujete. I nemá toobe váš web: může testovat službu REST API, na kterém závisí.

Existují dva typy testů dostupnosti:

* [Testování ping adresy URL](#create): jednoduchý test, který můžete vytvořit v hello portálu Azure.
* [Vícekrokový test webu](#multi-step-web-tests): které vytvoříte v Visual Studio Enterprise a nahrávání toohello portálu.

Můžete vytvořit až too25 testy dostupnosti na prostředek aplikace.

## <a name="create"></a>1. Otevření prostředku pro sestavy testů dostupnosti

**Pokud jste již nakonfigurovali Application Insights** pro webovou aplikaci, otevřete jeho prostředek Application Insights v hello [portál Azure](https://portal.azure.com).

**Nebo, pokud chcete toosee si sestavy na nový prostředek,** zaregistrovat příliš[Microsoft Azure](http://azure.com), přejděte toohello [portál Azure](https://portal.azure.com)a vytvořte prostředek Application Insights.

![Nový > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Klikněte na tlačítko **všechny prostředky** okno Přehled hello tooopen pro nový prostředek hello.

## <a name="setup"></a>2. Vytvoření testu adresy URL pomocí příkazu Ping
Otevřete okno hello dostupnosti a přidat test.

![Výplně alespoň hello adresa URL vašeho webu](./media/app-insights-monitor-web-app-availability/13-availability.png)

* **Adresa URL Hello** může být žádné webové stránce chcete tootest, ale musí být viditelná z hello veřejného Internetu. Adresa URL Hello může obsahovat řetězec dotazu. To znamená, že můžete také trochu vyzkoušet svou databázi. Pokud adresa URL hello přeloží tooa přesměrování, budeme následnou akci too10 přesměrování.
* **Analyzovat závislé požadavky**: Pokud zaškrtnete toto políčko, testovací hello požadavky bitové kopie, skripty, soubory stylu a další soubory, které jsou součástí hello webové stránky v rámci testu. Hello doba zaznamenané odezvy zahrnuje hello doba tooget tyto soubory. Hello test se nezdaří, pokud tyto prostředky nelze úspěšně stáhnout v časovém limitu hello pro celý test hello. 

    Pokud není políčko hello zaškrtnuté, testovací hello pouze požádá o soubor hello na adrese URL hello jste zadali.
* **Povolit opakování**: Pokud zaškrtnete toto políčko, pokud hello test se nezdaří, zopakuje se za krátkou dobu. Selhání je nahlášeno pouze v případě tří po sobě jdoucích neúspěšných pokusů. Následné testy jsou pak provedeny frekvencí testu obvyklým hello. Opakování je dočasně pozastaveno do dalšího úspěchu hello. Toto pravidlo platí nezávisle na každém umístění testu. Doporučujeme tuto možnost. V průměru přibližně 80 % selhání při opakování zmizí.
* **Frekvence testů**: Nastaví, jak často hello test se spouští z umístění každého testu. S pětiminutovou četností a pěti testovanými místy bude váš web testován v průměru každou minutu.
* **Testovací umístění** jsou hello umístí z kterých naše servery odesílají webové požadavky tooyour adresy URL. Zvolte více než jeden, aby bylo možné rozlišit problémy ve vašem webu od problémů se sítí. Můžete vybrat až too16 umístění.
* **Kritéria úspěchu**:

    **Časový limit testu**: snížit tuto hodnotu toobe upozornění pomalé odezvy. Hello test se počítá jako selhání, pokud během tohoto období nebyly přijaty odpovědí hello z webu. Pokud jste vybrali **analyzovat závislé požadavky**, pak všechny hello bitové kopie, soubory stylů, skripty a další závislé prostředky musí být přijaty během tohoto období.

    **Odpověď HTTP**: hello vrátil stavový kód, který se počítá jako úspěšný. 200 je hello kód, který označuje, že byla vrácena normální webová stránka.

    **Shoda obsahu**: řetězec, například „Vítejte!“ U každé odpovědi testujeme výskyt přesné shody (s rozlišováním velkých a malých písmen). Musí být prostý řetězec bez zástupných znaků. Nezapomeňte, že pokud obsah vaší stránka změní může mít tooupdate ho.
* **Výstrahy** , ve výchozím nastavení, odešlou tooyou při selhání ve třech umístěních více než pět minut. Selhání v jednom umístění je pravděpodobně toobe k potížím se sítí a nejedná se o problém s webem. Ale můžete změnit hello prahová hodnota toobe více nebo méně velká a malá písmena a můžete také změnit, kdo hello e-mailů by měly být odeslány na.

    Můžete nastavit [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md), který je volán, když je vydána výstraha. (Všimněte si, že v současné době parametry dotazu neprocházejí jako vlastnosti.)

### <a name="test-more-urls"></a>Testování více adres URL
Přidat další testy Například v přidání tootesting domovské stránky, jste měli jistotu, že vaše databáze se spouští otestováním adresy URL hello hledání.


## <a name="monitor"></a>3. Zobrazení výsledků testu dostupnosti

Po několika minutách, klikněte na tlačítko **aktualizovat** toosee výsledky testů. 

![Souhrnné výsledky na domácím okně hello](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

Hello scatterplot jsou uvedena vzorová hello výsledky testů, které obsahují podrobné diagnostické test krok. Modul testu Hello ukládá diagnostické podrobnosti pro testy, s chybami. Pro úspěšné testy diagnostické informace jsou uloženy pro podmnožinu spuštěních hello. Podržte ukazatel nad žádné hello zelená nebo červené tečky toosee hello testovací časové razítko, doba trvání testu, umístění a název testu. Proklikejte se prostřednictvím jakékoli tečky v hello bodové vykreslení toosee hello podrobnosti výsledků testů hello.  

Vyberte konkrétní test, umístění, nebo snižte dobu hello období toosee více výsledků kolem hello časové období, které vás zajímají. Použijte Průzkumník služby Search toosee výsledky z všechny spuštěních, nebo použijte vlastní sestavy toorun Analytics dotazy na tato data.

Přidání toohello nezpracovaná výsledků existují dvě dostupnosti metriky v Průzkumníku metrik: 

1. Dostupnosti: Procento hello testů, které byly úspěšné, mezi jednotlivými spuštěními všechny testovací. 
2. Doba trvání testu: průměrná doba trvání u všech provedení testu.

Filtry můžete použít na hello testovací název, umístění tooanalyze trendy v konkrétní test nebo umístění.

## <a name="edit"></a> Kontrola a úprava testů

Souhrnná stránka hello vyberte konkrétní test. Zde můžete zobrazit jeho konkrétní výsledky a upravit ho nebo dočasně zakázat.

![Upravit nebo zakázat webový test](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

Můžete chtít testy dostupnosti toodisable nebo hello výstrahy pravidla s nimi spojených během provádění údržby vaší služby. 

## <a name="failures"></a>Pokud se zobrazí chyby
Klikněte na červenou tečku.

![Klikněte na červenou tečku](./media/app-insights-monitor-web-app-availability/open-instance-3.png)


Výsledek testu dostupnosti umožňuje:

* Zkontrolujte, zda text hello odpověď ze serveru.
* Otevřete hello telemetrické zprávy odesílané aplikace serveru při zpracování instance hello chybných požadavků.
* Přihlaste se problému nebo pracovních položek v Git nebo služby VSTS tootrack hello problému. Hello chyb bude obsahovat událost toothis vazba.
* Výsledek testu webové hello otevřete v sadě Visual Studio.


*Zdá se, že všechno je v pořádku, ale přesto je hlášena chyba.* Zkontrolujte všechny bitové kopie hello, skripty, šablony stylů a všechny další soubory, které jsou načteny stránku hello. Pokud některý z nich selže, hello test uvádět jako neúspěšný i v případě hello hlavní html stránka načte bez problémů.

*Žádné související položky?* Pokud máte pro aplikaci na straně serveru nastavenou službu Application Insights, může být důvodem to, že právě probíhá [vzorkování](app-insights-sampling.md). 

## <a name="multi-step-web-tests"></a>Vícekrokové webové testy
Je možné sledovat scénář, který zahrnuje posloupnost adres URL. Například pokud sledujete Prodejní web, můžete otestovat, přidání položky toohello nákupní košík funguje správně.

> [!NOTE] 
> Vícekrokové webové testy jsou zpoplatněné. [Cenové schéma](http://azure.microsoft.com/pricing/details/application-insights/).
> 

toocreate vícekrokový test zaznamenání hello scénář pomocí Visual Studio Enterprise a pak nahrajte hello zaznamenávání tooApplication statistiky. Application Insights opětovná přehrání hello scénář v intervalech a ověřuje hello odpovědi.

> [!NOTE]
> V testech nelze použít programové funkce nebo smyčky. Hello test musí být zcela obsažené ve skriptu .webtest hello. Můžete však použít standardní moduly plug-in.
>

#### <a name="1-record-a-scenario"></a>1. Záznam scénáře
Použít Visual Studio Enterprise toorecord a relace webu.

1. Vytvořte projekt testu výkonnosti webu.

    ![V aplikaci Visual Studio Enterprise edition vytvořte projekt ze šablony výkonu webu a zátěžového testu hello.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Nevidíte hello výkonu webů a zátěžového testu šablony?* – Zavřete sadu Visual Studio Enterprise. Otevřete **instalační program Visual Studio** toomodify instalaci Visual Studio Enterprise. V části **Jednotlivé komponenty** vyberte **Nástroje pro testování výkonnosti webů a zátěžové testování**.

2. Otevřete soubor .webtest hello a spusťte záznam.

    ![Otevřete soubor .webtest hello a klikněte na tlačítko záznam.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. Hello akcí uživatele chcete toosimulate v testu: Otevřete webovou stránku, přidat do košíku produkt toohello a tak dále. Pak test zastavte.

    ![Hello záznamník testování webu běží v aplikaci Internet Explorer.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Nevytvářejte příliš dlouhý scénář. Platí limit 100 kroků a 2 minut.
4. Úpravy hello test na:

   * Přidání ověření toocheck hello přijatých textu a kódů odpovědi.
   * Odeberte všechny nadbytečné interakce. Můžete také odebrat závislé požadavky pro obrázky nebo tooad nebo sledování lokalit.

     Mějte na paměti, že můžete upravit pouze hello zkušební skript – nelze přidat vlastní kód nebo volat jiné webové testy. Nevkládejte smyčky v testu hello. Můžete použít standardní zásuvné moduly webového testu.
5. Spusťte hello test v sadě Visual Studio toomake se, že funguje.

    Hello Spouštěč webových testů otevře webový prohlížeč a opakuje hello akce, které jste si poznamenali. Zkontrolujte, zda vše funguje podle očekávání.

    ![V sadě Visual Studio otevřete soubor .webtest hello a klikněte na tlačítko spustit.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-hello-web-test-tooapplication-insights"></a>2. Nahrát hello webového testu tooApplication statistiky
1. Hello portálu Application Insights vytvoření testu webu.

    ![V okně pro hello webové testy zvolte možnost přidat.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. Vyberte vícekrokový test a nahrajte soubor .webtest hello.

    ![Vyberte vícekrokový webový test.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Sada hello testovanými místy, četnost a parametry výstrah v hello stejný způsobem jako u příkazu ping testuje.

#### <a name="3-see-hello-results"></a>3. Zobrazte výsledky hello

Zobrazit svůj test výsledky a všechny chyby v hello testy stejným způsobem jako jedné adresy url.

Kromě toho si můžete stáhnout tooview výsledky testu hello je v sadě Visual Studio.

#### <a name="too-many-failures"></a>Příliš mnoho selhání?

* Běžným důvodem selhání je, že tento hello test běží příliš dlouho. Nesmí se spouštět déle než dvě minuty.

* Nezapomeňte, že musí správně načítat všechny prostředky hello stránky pro test toosucceed hello, včetně skripty, šablony stylů, obrázků a tak dále.

* Hello webový test musí být zcela obsažen ve skriptu hello .webtest: programové funkce nelze použít v testu hello.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Doba zapojení a náhodná čísla do vícekrokového testu
Předpokládejme, že testujete nástroj, který získá data závislá na čase, například akcie z externího kanálu. Při záznamu webového testu máte toouse určitých časech, ale můžete nastavit je jako parametry hello otestovat, čas spuštění a čas ukončení.

![Webový test s parametry.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

Když spustíte hello test, byste chtěli čas ukončení vždy k dispozici toobe hello čas a čas spuštění by měl 15 minut před.

Test zásuvné moduly webového poskytují způsob hello toodo Parametrizace časy.

1. Přidejte zásuvný modul webového testu pro každou hodnotu parametru proměnné, kterou chcete. V hello nástrojů webového testu zvolte **přidat modul plug-in pro testování webových**.

    ![Zvolte možnost přidat zásuvný modul pro testování webu a vyberte typ.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    V tomto příkladu používáme dvě instance hello Plug-in datum čas. Jedna instance je „před 15 minutami“ a druhá „teď“.
2. Otevřete hello vlastnosti každého zásuvného modulu. Pojmenujte ho a nastavte ji toouse hello aktuální čas. Pro jeden z nich nastavte Přidat minuty = -15.

    ![Nastavte název, použijte aktuální čas a přidejte minuty.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. V parametrech webového testu hello použijte {{plug-in name}} tooreference na název zásuvného modulu.

    ![V parametru hello testu použijte {{plug-in name}}.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Teď nahrajte portálu toohello testu. Při každé spuštění testu hello používá hello dynamické hodnoty.

## <a name="dealing-with-sign-in"></a>Vyřešení přihlášení
Pokud vaši uživatelé přihlásí tooyour aplikace, máte různé možnosti pro simulaci přihlášení, takže můžete testovat stránky za hello přihlásit. Hello přístupů, které použijete, závisí na hello typu zabezpečení poskytovaném aplikací hello.

Ve všech případech by měl vytvořit účet v aplikaci jenom pro účely hello testování. Pokud je to možné omezte hello oprávnění tohoto účtu testovací tak, že není možné hello webových testů by to ovlivnilo skutečné uživatelé.

### <a name="simple-username-and-password"></a>Jednoduché uživatelské jméno a heslo
V hello záznam webového testu obvyklým způsobem. Nejprve odstraňte soubory cookie.

### <a name="saml-authentication"></a>Ověřování SAML
Použijte zásuvný modul SAML hello, která je k dispozici pro webové testy.

### <a name="client-secret"></a>Tajný klíč klienta
Pokud vaše aplikace obsahuje trasu přihlášení, která zahrnuje tajný klíč klienta, použijte ji. Azure Active Directory (AAD) je příkladem služby, která poskytuje přihlašování pomocí tajného klíče klienta. V AAD je sdílený tajný klíč klienta hello hello klíč aplikace.

Tady je ukázkový webový test webové aplikace v Azure pomocí klíče aplikace:

![Ukázkový tajný klíč klienta](./media/app-insights-monitor-web-app-availability/110.png)

1. Získejte token ze služby AAD pomocí tajného klíče klienta (AppKey).
2. Extrahujte nosný token z odpovědi.
3. Volání rozhraní API pomocí tokenu nosiče v hlavičce autorizace hello.

Ujistěte se, že webový test hello je skutečný klienta – to znamená, má svou vlastní aplikaci v AAD - a použít jeho clientId + appkey. Služby testovaného také má svou vlastní aplikaci v AAD: hello appID URI této aplikace se odrazí v testu webu hello v poli "prostředek" hello.

### <a name="open-authentication"></a>Otevřené ověřování
Příkladem otevřeného ověřování je přihlašování pomocí účtu Microsoft nebo Google. Velký počet aplikací, použijte OAuth poskytují hello alternativní tajný klienta, takže prvním cílem by mělo být tooinvestigate tuto možnost.

Pokud váš test musí přihlásit pomocí OAuth, je obecný přístup hello:

* Pomocí nástroje, jako je Fiddler tooexamine hello provoz mezi webového prohlížeče, webem ověřování hello a aplikace.
* Proveďte dvě nebo více přihlášení pomocí různých počítačů nebo prohlížečů nebo v dlouhých intervalech (tooexpire tooallow tokeny).
* Porovnáním různých relací Identifikujte hello token předaný zpět z hello ověřování lokality, který byl následně předán tooyour aplikačního serveru po přihlášení.
* Uložte webový test pomocí sady Visual Studio.
* Parametrizace hello tokeny, nastavení hello parametr při vrácení hello tokenu z ověřovatele hello a jeho použití hello dotazu toohello lokality.
  (Visual Studio pokusí tooparameterize hello testu, ale není správně parametrizovat tokeny hello.)


## <a name="performance-tests"></a>Testy výkonnosti
Na svém webu můžete spustit zátěžový test. Jako hello test dostupnosti můžete odeslat jednoduché požadavky nebo požadavky vícekrokový z našich bodů kolem hello, world. Na rozdíl od testu dostupnosti se odesílá mnoho požadavků, které simulují několik souběžných uživatelů.

V okně Přehled hello, otevřete **nastavení**, **testy výkonnosti**. Když vytvoříte testu, se zobrazí Pozvánka tooconnect tooor vytvořit účet Visual Studio Team Services.

Po dokončení testu hello se zobrazí dobu odezvy a úspěšnost.


![Test výkonnosti](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> použít tooobserve hello důsledků testu výkonnosti [živý datový proud](app-insights-live-stream.md) a [profileru](app-insights-profiler.md).
>

## <a name="automation"></a>Automation
* [Použít tooset skripty PowerShell se test dostupnosti](app-insights-powershell.md#add-an-availability-test) automaticky.
* Nastavení [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md), který je volán při vydání výstrahy.

## <a name="qna"></a>Máte dotazy? Problémy?
* *Můžu z webového testu volat kód?*

    Ne. Hello kroky testu hello musí být v souboru .webtest hello. A nemůžete volat jiné webové testy nebo používat smyčky. Existují různé zásuvné moduly, které se vám můžou hodit.
* *Je podporovaný protokol HTTPS?*

    Podporujeme protokoly TLS 1.1 a TLS 1.2.
* *Je nějaký rozdíl mezi „webovými testy“ a „testy dostupnosti“?*

    Hello dvě podmínky může odkazovat zcela zaměnitelným významem. Testy dostupnosti je více obecný pojem, který zahrnuje jeden ping adresy URL hello testy kromě toohello vícekrokové webové testy.
* *Nastavit jako toouse testy dostupnosti na našem interním serveru, který se spouští za bránou firewall.*

    Existují dvě možná řešení:
    
    * Konfigurace vaší brány firewall toopermit příchozí požadavky z hello [IP adresy naše webové testovací agenti](app-insights-ip-addresses.md).
    * Zapsat svůj vlastní test tooperiodically kód interního serveru. Spusťte hello kód jako proces na pozadí na testovacím serveru za vaší bránou firewall. Vaše testovací proces může odesílat své výsledky tooApplication statistika pomocí [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) rozhraní API v balíčku SDK základní hello. To vyžaduje, aby vaše testovací toohave odchozí přístup toohello Application Insights přijímání koncový bod serveru, ale který je mnohem menší riziko zabezpečení než hello alternativní povolit příchozí požadavky. výsledky Hello se nebude zobrazovat na hello dostupnost webové testy oken, ale se zobrazí jako dostupnosti výsledky analýzy, vyhledávání a metrika Explorer.
* *Vícekrokový webový test se nepodařilo nahrát.*

    Maximální velikost je 300 kB.

    Smyčky nejsou podporovány.

    Odkazy na tooother webové testy nejsou podporovány.

    Zdroje dat nejsou podporovány.
* *Můj vícekrokový test se nedokončil.*

    Existuje limit 100 požadavků na test.

    Hello test je zastavena, pokud poběží déle než dvě minuty.
* *Jak spustím test s klientskými certifikáty?*

    Tuto možnost nepodporujeme, je nám líto.


## <a name="next"></a>Další kroky
[Prohledávání diagnostických protokolů][diagnostic]

[Řešení potíží][qna]

[IP adresy webových testovacích agentů](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
