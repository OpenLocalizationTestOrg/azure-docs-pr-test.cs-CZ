---
title: "aaaCollect vlastní protokoly v OMS Log Analytics | Microsoft Docs"
description: "Analýzy protokolů můžete shromažďovat události z textových souborů v počítačích Windows a Linux.  Tento článek popisuje, jak podrobnosti hello záznamů a toodefine nový vlastní protokol vytvoří v úložišti OMS hello."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: aca7f6bb-6f53-4fd4-a45c-93f12ead4ae1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2017
ms.author: bwren
ms.openlocfilehash: a75d058c371fecf7c43690a797d4e650c1570317
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-logs-in-log-analytics"></a>Vlastní protokoly v analýzy protokolů
zdroj dat vlastní protokoly Hello v analýzy protokolů umožňuje toocollect události z textových souborů v počítačích Windows a Linux. Mnoho aplikací informace tootext soubory místo standardní protokolování služby, jako je například protokol událostí systému Windows nebo Syslog protokolu.  Jakmile získány, můžete analyzovat každý záznam v protokolu hello v polích tooindividual pomocí hello [vlastní pole](log-analytics-custom-fields.md) funkce analýzy protokolů.

![Vlastní protokol kolekce](media/log-analytics-data-sources-custom-logs/overview.png)

toobe soubory protokolu Hello shromážděných musí odpovídat hello následující kritéria.

- Hello protokolu musí mít jednu položku na každý řádek nebo použít časové razítko odpovídající, které jedna z následujících hello formáty při spuštění hello každé položky.

    RRRR MM-DD HH: MM:<br>M/D/RRRR HH: MM: SS DOP. / ODP <br>MON DD, rrrr hh: mm:

- soubor protokolu Hello nesmí povolit cyklické aktualizací, kde hello k přepsání souboru nové položky.
- soubor protokolu Hello musí používat kódování ASCII nebo UTF-8.  Ostatní formáty například UTF-16 nejsou podporovány.

>[!NOTE]
>Pokud existují duplicitní položky v souboru protokolu hello, analýzy protokolů je sloučit.  Výsledky hledání hello však bude být nekonzistentní kde výsledky filtru hello zobrazit další události než počet výsledků hello.  Je důležité, abyste ověření toodetermine protokolu hello, pokud hello aplikace, která ji vytvoří, způsobí toto chování a pokud je to možné adres před vytvořením definice kolekce hello vlastního protokolu.  
>
  
## <a name="defining-a-custom-log"></a>Definování vlastní protokol
Použijte následující postup toodefine vlastní protokol hello.  Posuňte se toohello konci tohoto článku podrobný ukázkové přidání vlastního protokolu.

### <a name="step-1-open-hello-custom-log-wizard"></a>Krok 1. Otevřete hello vlastní protokolu Průvodce
Hello vlastního průvodce protokolu běží na portálu OMS hello a můžete toodefine nové toocollect vlastního protokolu.

1. Na portálu OMS hello, přejděte příliš**nastavení**.
2. Klikněte na **Data** a potom **vlastní protokoly**.
3. Standardně jsou všechny změny konfigurace automaticky instaluje tooall agenty.  Pro agenty Linux je odeslán soubor konfigurace toohello Fluentd data kolekce.  Pokud chcete tento soubor na každého agenta Linux ručně toomodify, poté zrušte zaškrtnutí políčka hello *použít následující konfigurace počítačů se systémem Linux toomy*.
4. Klikněte na tlačítko **přidat +** tooopen hello Průvodce vlastního protokolu.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Krok 2. Nahrání a analyzovat ukázkový protokol
Můžete začít odesílání vzorku hello vlastního protokolu.  Průvodce Hello analyzovat a zobrazit hello položky v tomto souboru, pro které toovalidate.  Analýzy protokolů použije hello oddělovač zadejte tooidentify každý záznam.

**Nový řádek** hello výchozí oddělovač a slouží pro soubory protokolů, které mají jednu položku na každý řádek.  Pokud hello řádek začíná datum a čas v jednom z formátů hello k dispozici, pak můžete zadat **časové razítko** oddělovač, který podporuje položky, které jsou rozmístěny více než jeden řádek.

Pokud se používá oddělovač časové razítko, bude vlastnost TimeGenerated hello každý záznam uložený v OMS vyplňovat hello datum a čas zadaný pro tuto položku v souboru protokolu hello.  Pokud se používá nový řádek oddělovač, pak TimeGenerated je naplněna datum a čas, analýzy protokolů shromážděných hello položku.

> [!NOTE]
> Analýzy protokolů aktuálně zpracovává hello datum a čas shromážděných z protokolu pomocí časového razítka oddělovač ve formátu UTC.  Brzy bude změněné toouse hello časové pásmo na hello agenta.
>
>

1. Klikněte na tlačítko **Procházet** a procházet tooa ukázkový soubor.  Všimněte si, že to může tlačítko může být označeno jako **zvolit soubor** v některé prohlížeče.
2. Klikněte na **Další**.
3. Hello vlastní protokolu Průvodce odešlete hello soubor a seznamu hello záznamy, které ji identifikuje.
4. Změnit hello oddělovač, který je použité tooidentify nový záznam a vyberte hello oddělovač, který nejlépe identifikuje hello záznamy v souboru protokolu.
5. Klikněte na **Další**.

### <a name="step-3-add-log-collection-paths"></a>Krok 3. Přidat cesty ke kolekcím protokolů
Je nutné zadat jednu nebo více cest v hello agentovi, kde najdou hello vlastního protokolu.  Můžete buď zadat konkrétní cestu a název souboru protokolu hello nebo se zástupnými znaky pro hello název můžete zadat cestu.  To podporuje aplikace, které každý den nebo pokud jeden soubor dosáhne určité velikosti, vytvořte nový soubor.  Můžete zadat také více cest pro jeden soubor protokolu.

Například aplikace může vytvořit soubor protokolu každý den s datem hello součástí hello název jako log20100316.txt. Vzor takové protokolu může být *protokolu\*.txt* které bude platit tooany soubor protokolu aplikace hello je pojmenování schématu.

Hello následující tabulka obsahuje příklady platných vzorů toospecify různých protokolových souborech.

| Popis | Cesta |
|:--- |:--- |
| Všechny soubory v *C:\Logs* s příponou .txt pro agenta služby Windows |C:\Logs\\\*.txt |
| Všechny soubory v *C:\Logs* s názvem počínaje protokolu a příponu .txt pro agenta služby Windows |C:\Logs\log\*.txt |
| Všechny soubory v */var/log/audit* s příponou .txt na agenta systému Linux |/var/log/audit/*.txt |
| Všechny soubory v */var/log/audit* s názvem počínaje protokolu a příponu .txt na agenta systému Linux |/var/log/audit/log\*.txt |

1. Vyberte systému Windows nebo Linux toospecify formát cesty, které přidáváte.
2. Zadejte do pole Cesta hello a klikněte na tlačítko hello  **+**  tlačítko.
3. Hello postup opakujte pro všechny další cesty.

### <a name="step-4-provide-a-name-and-description-for-hello-log"></a>Krok 4. Zadejte název a popis pro protokol hello
Hello název, který zadáte se použije pro typ protokolu hello jak bylo popsáno výše.  Ho bude vždy končit _CL toodistinguish jej jako vlastní protokol.

1. Zadejte název pro protokol hello.  Hello  **\_CL** automaticky zajištěna příponu.
2. Přidejte volitelné **popis**.
3. Klikněte na tlačítko **Další** toosave hello vlastní protokol definice.

### <a name="step-5-validate-that-hello-custom-logs-are-being-collected"></a>Krok 5. Ověřte, že hello vlastní protokoly jsou shromažďovány
To může trvat až hodinu tooan hello počáteční data z nové tooappear vlastního protokolu v analýzy protokolů.  Spustí shromažďování položky z hello protokoly najít v cestě hello jste zadali z bodu hello je definovaný hello vlastní protokol.  Nezachovají hello položky, které jste odeslali během vytváření hello vlastního protokolu, ale shromáždí již existující položky v hello soubory protokolů, které je možné vyhledat.

Jakmile analýzy protokolů spustí shromažďování z vlastního protokolu hello, bude k dispozici s hledání protokolů svoje záznamy.  Použití hello název, který jste zadali vlastní protokol hello jako hello **typ** v dotazu.

> [!NOTE]
> Pokud chybí hello RawData vlastnost z hello hledání, může potřebovat tooclose a znovu otevřete prohlížeč.
>
>

### <a name="step-6-parse-hello-custom-log-entries"></a>Krok 6. Analyzovat položky vlastní protokolu hello
Položka Hello celý protokolu bude uložen v jedné vlastnost s názvem **RawData**.  Pravděpodobně budete chtít tooseparate hello různé části informací v každé položky do jednotlivé vlastnosti, které jsou uložené v záznamu hello.  To provedete pomocí hello [vlastní pole](log-analytics-custom-fields.md) funkce analýzy protokolů.

Podrobné kroky k analýze položka hello vlastní protokolu nejsou uvedeny zde.  Podrobnosti najdete toohello [vlastní pole](log-analytics-custom-fields.md) dokumentaci pro tyto informace.

## <a name="disabling-a-custom-log"></a>Zakázání vlastního protokolu
Definice vlastního protokolu nelze odebrat, jakmile je byla vytvořena, ale můžete ji zakázat odebráním všech jeho cesty ke kolekcím.

1. Na portálu OMS hello, přejděte příliš**nastavení**.
2. Klikněte na **Data** a potom **vlastní protokoly**.
3. Klikněte na tlačítko **podrobnosti** další toodisable definice toohello vlastního protokolu.
4. Odeberte všechny hello cesty ke kolekcím pro definice hello vlastního protokolu.

## <a name="data-collection"></a>Shromažďování dat
Analýzy protokolů bude shromažďovat nové položky z každé vlastní protokolu přibližně každých 5 minut.  Hello agenta zaznamená v jednotlivých souborů protokolu, který shromáždí z jeho místo.  Pokud agenta hello přejde do režimu offline dobu, bude položky od posledního místa vypnutý, shromažďovat analýzy protokolů i v případě, že tyto položky byly vytvořeny v době, kdy hello agent offline.

celý obsah Hello hello položky protokolu se zapisují tooa jednu vlastnost s názvem **RawData**.  To můžete analyzovat na více vlastností, které mohou být analyzován a vyhledávat samostatně definováním [vlastní pole](log-analytics-custom-fields.md) po vytvoření hello vlastního protokolu.

## <a name="custom-log-record-properties"></a>Vlastnosti záznamu vlastní protokolu
Vlastní protokol záznamů mít typ s názvem hello protokolu, které poskytují a hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| TimeGenerated |Datum a čas, který hello záznam nebyla shromážděna podle analýzy protokolů.  Pokud protokol hello používá oddělovač založené na čase jde hello čas shromážděných z položky hello. |
| SourceSystem |Typ záznamu hello agenta nebyla shromážděna z. <br> Připojit OpsManager – agent systému Windows, buď přímo nebo System Center Operations Manager <br> Linux – všechny agenty Linux |
| RawData |Úplný text hello shromažďují položku. |
| ManagementGroupName |Název skupiny pro správu hello pro System Center Operations spravovat agenty.  Pro jiné agenty jde AOI -\<ID pracovního prostoru\> |

## <a name="log-searches-with-custom-log-records"></a>Protokol hledání se záznamy vlastního protokolu
Záznamy z vlastní protokoly jsou uloženy v úložišti OMS hello stejně jako záznamy z jiného zdroje dat.  Budou mít typ odpovídající hello název, který zadáte, když definujete hello protokolu, abyste je mohli používat vlastnost typu hello v záznamech tooretrieve vyhledávání shromážděných z konkrétní protokolu.

Hello následující tabulka obsahuje různé příklady vyhledávání protokolu, které načtení záznamů z vlastní protokoly.

| Dotaz | Popis |
|:--- |:--- |
| Typ = MyApp_CL |Všechny události z vlastní protokolu s názvem MyApp_CL. |
| Typ = MyApp_CL Severity_CF = chyby |Všechny události z vlastní protokolu s názvem MyApp_CL s hodnotou *chyba* do vlastní pole s názvem *Severity_CF*. |

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak změní následující toohello hello výše dotazy.

> | Dotaz | Popis |
|:--- |:--- |
| MyApp_CL |Všechny události z vlastní protokolu s názvem MyApp_CL. |
| MyApp_CL &#124; kde Severity_CF == "error. |Všechny události z vlastní protokolu s názvem MyApp_CL s hodnotou *chyba* do vlastní pole s názvem *Severity_CF*. |


## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Ukázka návod přidání vlastního protokolu
Hello následující části vás provede příklad vytvoření vlastního protokolu.  ukázkový protokol Hello shromažďovaných má jednu položku na každý řádek počínaje datum a čas a potom oddělených čárkou pole pro kód, stav a zprávy.  Níže jsou uvedeny několik položek příkladu.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect toodatabase
    2016-03-10 01:31:34 303,Error,Application lost connection toodatabase

### <a name="upload-and-parse-a-sample-log"></a>Nahrání a analyzovat ukázkový protokol
Jsme zadejte některou z hello soubory protokolu a zobrazit hello události, které bude shromažďování.  Nový řádek v tomto případě je dostatečná oddělovač.  Pokud jeden záznam v protokolu hello může zahrnovat více řádků, přestože by třeba oddělovač časové razítko toobe použít.

![Nahrání a analyzovat ukázkový protokol](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Přidat cesty ke kolekcím protokolů
soubory protokolu Hello budou umístěny v *C:\MyApp\Logs*.  Nový soubor se vytvoří každý den s názvem, který obsahuje data hello ve vzoru hello *appYYYYMMDD.log*.  Dostatečná vzor pro tento protokol by *C:\MyApp\Logs\\\*.log*.

![Cesta kolekce protokolu](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-hello-log"></a>Zadejte název a popis pro protokol hello
Používáme název *MyApp_CL* a zadejte **popis**.

![Název protokolu](media/log-analytics-data-sources-custom-logs/log-name.png)

### <a name="validate-that-hello-custom-logs-are-being-collected"></a>Ověřte, že hello vlastní protokoly jsou shromažďovány
Používáme dotaz *typ = MyApp_CL* tooreturn všechny záznamy z protokolu shromážděných hello.

![Dotaz protokolu s žádné vlastní pole](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-hello-custom-log-entries"></a>Analyzovat položky vlastní protokolu hello
Používáme vlastní pole toodefine hello *EventTime*, *kód*, *stav*, a *zpráva* pole a jsme viděli hello rozdíl hello záznamy, které jsou vrácených dotazem hello.

![Protokol dotazu s vlastními poli](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Další kroky
* Použití [vlastní pole](log-analytics-custom-fields.md) tooparse hello položky hello vlastní protokolu tooindividual polí.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.
