---
title: pole aaaCustom v Log Analytics | Microsoft Docs
description: "Funkce vlastní pole Hello analýzy protokolů vám umožní toocreate vlastní prohledávatelné pole z OMS data, která přidávat vlastnosti toohello shromážděných záznamu.  Tento článek popisuje proces toocreate hello vlastních polí a poskytuje podrobný návod s událost vzorku."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 31572b51-6b57-4945-8208-ecfc3b5304fc
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 1aa0e497d5b1d7898b0da6a5ef40f568e63bc589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-fields-in-log-analytics"></a>Vlastní pole v analýzy protokolů
Hello **vlastní pole** funkce analýzy protokolů umožňuje tooextend existující záznamy v úložišti OMS hello přidávat i vlastní prohledávatelné pole.  Vlastní pole se vyplní automaticky ze dat extrahovaných z dalších vlastností v hello stejné záznamu.

![Vlastní pole – přehled](media/log-analytics-custom-fields/overview.png)

Například záznam ukázka hello níže obsahuje užitečné data schovaný v popisu události hello.  Extrahování tato data do samostatné vlastností, tak bude dostupný pro činnosti, jako je řazení a filtrování.

![Tlačítko vyhledat protokolu](media/log-analytics-custom-fields/sample-extract.png)

> [!NOTE]
> V hello Preview jsou omezené too100 vlastní pole v pracovním prostoru.  Tento limit bude rozšířena, když tato funkce dosáhne obecné dostupnosti.
> 
> 

## <a name="creating-a-custom-field"></a>Vytváření vlastních polí
Když vytvoříte vlastní pole, analýzy protokolů, musí porozumět které toopopulate toouse dat jeho hodnotu.  Používá technologii z Microsoft Research názvem FlashExtract tooquickly identifikovat tato data.  Místo nutnosti tooprovide explicitní pokyny, analýzy protokolů dozví o hello data chcete tooextract z příklady, které zadáte.

Hello následující oddíly poskytují hello postup pro vytvoření vlastního pole.  V hello dolní části tohoto článku je návod, ukázka extrakce.

> [!NOTE]
> jako záznamy odpovídající hello zadaná kritéria se přidají toohello OMS úložišti, takže se zobrazí pouze na záznamy shromážděných po vytvoření vlastních polí hello se vyplní Hello vlastní pole.  vlastní pole Hello nepřidají toorecords, které jsou již v úložišti dat hello při jeho vytvoření.
> 
> 

### <a name="step-1--identify-records-that-will-have-hello-custom-field"></a>Krok 1 – identifikace záznamy, které bude mít vlastní pole hello
prvním krokem Hello je tooidentify hello záznamy, které získají hello vlastních polí.  Spustit s [standardní protokol hledání](log-analytics-log-searches.md) a pak vyberte záznam tooact jako hello model, který analýzy protokolů se dozvíte z.  Pokud určíte, že budete tooextract dat do vlastních polí, hello **Průvodce extrakce pole** je otevřen, kde můžete ověřit a upřesněte kritéria hello.

1. Přejděte příliš**hledání protokolů** a použít [dotazování záznamů hello tooretrieve](log-analytics-log-searches.md) , bude mít vlastní pole hello.
2. Vyberte záznam, analýzy protokolů použijete tooact jako model pro extrahování dat toopopulate hello vlastních polí.  Identifikujete hello data, která chcete tooextract z tento záznam, a analýzy protokolů použije toto informace toodetermine hello logiku toopopulate hello vlastní pole pro všechny podobné záznamy.
3. Klikněte na tlačítko hello tlačítko toohello nalevo od všechny vlastnosti textu hello záznamů a vyberte možnost **rozbalte pole z**.
4. Hello **pole extrakce průvodce otevřete**, a jste vybrali záznam hello se zobrazí v hello **hlavní příklad** sloupce.  vlastní pole Hello bude určena pro tyto záznamy s hello stejné hodnoty v hello vlastnosti, které jsou vybrány.  
5. Pokud hello výběr přesně co chcete použít, vyberte kritéria hello toonarrow další pole.  Pořadí toochange hello hodnoty v polích hello kritéria musíte zrušit a vyberte jiný záznam odpovídající hello kritéria, která chcete.

### <a name="step-2---perform-initial-extract"></a>Krok 2 – provádět počáteční extrakce.
Jakmile jste zformulovali hello záznamy, které budou mít hello vlastních polí, identifikujete hello dat, které chcete tooextract.  Analýzy protokolů použije tento vzory podobné tooidentify informace v podobných záznamů.  V kroku hello po této bude být schopný toovalidate hello výsledky a poskytnout další podrobnosti pro analýzy protokolů toouse v jeho analýzu.

1. Zvýraznění textu hello v záznamu ukázka hello má toopopulate hello vlastních polí.  Potom zobrazí s tooprovide dialogové okno pole Název hello pole a tooperform hello počáteční extrakce.  Hello znaků  **\_CR** se automaticky připojí.
2. Klikněte na tlačítko **extrahovat** tooperform analýzu shromážděných záznamů.  
3. Hello **Souhrn** a **výsledky hledání** oddíly uvádějí hello výsledky hello extrakce, si můžete prohlédnout její přesnost.  **Souhrn** zobrazí hello kritéria tooidentify záznamů a počtu pro každou z hodnot dat hello identifikovat.  **Výsledky hledání** poskytuje podrobný seznam záznamů, které odpovídají kritériím hello.

### <a name="step-3--verify-accuracy-of-hello-extract-and-create-custom-field"></a>Krok 3 – ověřit přesnost hello extrakce a vytvořit vlastní pole
Po provedení počáteční extrakce hello analýzy protokolů se zobrazí její výsledky na základě dat, které již byly zjištěny.  Pokud hello výsledky vypadají přesné můžete vytvořit vlastní pole hello se žádné další práci.  Pokud ne, pak můžete upřesnit hello výsledky tak, aby analýzy protokolů může zlepšit svou logikou.

1. Pokud všechny hodnoty v hello počáteční extrakce nejsou správné, klikněte hello **upravit** nesprávné záznam další tooan ikonu a vyberte **upravit tento zvýraznění** v pořadí toomodify hello výběru.
2. Hello položka je zkopírovaný toohello **Další příklady** pod hello **hlavní příklad**.  Můžete upravit hello zvýraznění zde toohelp analýzy protokolů pochopit by měl mít provedeného výběru hello.
3. Klikněte na tlačítko **extrahovat** toouse zaznamenává této nové informace tooevaluate všechny hello existující.  pro záznamy než hello jeden, který jste právě provedli změnu založené na tuto novou intelligence může upravit výsledky Hello.
4. Pokračujte, tooadd opravy, dokud všechny záznamy v hello extrahovat správně identifikovat hello data toopopulate hello v nové vlastní pole.
5. Klikněte na tlačítko **uložit extrahovat** až budete spokojeni s výsledky hello.  vlastní pole Hello je nyní definována, ale nebude přidáno tooany záznamy ještě.
6. Počkejte pro nové záznamy odpovídající hello zadat kritéria toobe shromážděna a poté znovu spusťte hledání protokolů hello. Nové záznamy by měl mít hello vlastních polí.
7. Vlastní pole hello jako ostatní záznamů vlastnost použijte.  Lze jej používat data tooaggregate a skupiny a používat i tooproduce nové statistiky.

## <a name="viewing-custom-fields"></a>Zobrazení vlastních polí
Můžete zobrazit seznam všech vlastních polí ve vaší skupině pro správu z hello **nastavení** dlaždici řídicího panelu, hello OMS.  Vyberte **Data** a potom **vlastní pole** seznam všech vlastních polí v pracovním prostoru.  

![Vlastní pole](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Odebrání vlastního pole
Existují dva způsoby tooremove vlastní pole.  Hello je nejdřív hello **odebrat** možnost pro každé pole při prohlížení hello úplný seznam, jak je popsáno výše.  Hello jiné metody je tooretrieve toohello záznam a klikněte na tlačítko hello ponecháno hello pole.  Hello nabídky bude mít hello tooremove možnost pro vlastní pole.

## <a name="sample-walkthrough"></a>Ukázka návod
Hello následující části vás provede kompletní příklad, jak vytváření vlastních polí.  Tento příklad extrahuje hello název služby v systému Windows události, které označují změna stavu služby.  To závisí na události vytvořené pomocí Správce řízení služeb v protokolu systému hello na počítače se systémem Windows.  Pokud chcete v tomto příkladu toofollow, musí být [shromažďování událostí informace pro protokol systému hello](log-analytics-data-sources-windows-events.md).

Jsme zadejte hello následující dotaz tooreturn všechny události ze Správce řízení služeb s ID události z 7036, což je hello událost, která určuje, spuštění nebo zastavení služby.

![Dotaz](media/log-analytics-custom-fields/query.png)

Zvolte jsme žádný záznam se události ID 7036.

![Zdrojový záznam](media/log-analytics-custom-fields/source-record.png)

Chceme hello název služby, který se zobrazí v hello **RenderedDescription** vlastnost a vyberte hello tlačítko Další toothis vlastnost.

![Rozbalte pole](media/log-analytics-custom-fields/extract-fields.png)

Hello **pole extrakce průvodce** je otevřena a hello **protokolu událostí** a **EventID** pole jsou vybrána v hello **hlavní příklad** sloupce.  To znamená, že toto vlastní pole hello budou určené pro události z protokolu systému hello s ID události 7036.  Toto je dostatečná, takže jsme nepotřebujete tooselect další pole.

![Hlavní příklad](media/log-analytics-custom-fields/main-example.png)

Jsme zvýrazněte hello název služby hello v hello **RenderedDescription** vlastnost a použití **služby** název služby tooidentify hello.  vlastní pole Hello bude volána **Service_CF**.

![Název pole](media/log-analytics-custom-fields/field-title.png)

Vidíte, že tento název služby hello se identifikuje správně pro některé záznamy, ale nikoli pro ostatní uživatele.   Hello **výsledky hledání** zobrazit část hello název hello **adaptér výkonu služby WMI** nebyla vybrána.  Hello **Souhrn** ukazuje, že čtyři zaznamenává s **DPRMA** služby zahrnuty nesprávně navíc word a dva záznamy identifikovat **instalační program moduly** místo **Instalační služby systému Windows moduly**.  

![Výsledky hledání](media/log-analytics-custom-fields/search-results-01.png)

Začneme s hello **adaptér výkonu služby WMI** záznamu.  Kliknete na ikonu pro úpravu a potom **upravit tento zvýraznění**.  

![Upravit zvýraznění](media/log-analytics-custom-fields/modify-highlight.png)

Jsme zvýšit hello zvýraznění tooinclude hello word **WMI** a poté znovu spusťte hello extrakce.  

![Další příklad](media/log-analytics-custom-fields/additional-example-01.png)

Uvidíte, že hello položky pro **adaptér výkonu služby WMI** opravení, a analýzy protokolů také použít tento informace toocorrect hello záznamy pro **Instalační služby systému Windows modulu**.  Vidíme v hello **Souhrn** části ale který **DPMRA** není právě nadále identifikovány správně.

![Výsledky hledání](media/log-analytics-custom-fields/search-results-02.png)

Jsme posuňte tooa záznam s hello služba DPMRA a použít stejný proces toocorrect, který záznam hello.

![Další příklad](media/log-analytics-custom-fields/additional-example-02.png)

 Když jsme spustí hello extrakce, jsme můžete zobrazit, že všechny naše výsledky jsou nyní správné.

![Výsledky hledání](media/log-analytics-custom-fields/search-results-03.png)

Jsme uvidíte, že **Service_CF** je vytvořen, ale ještě není přidán tooany záznamy.

![Počáteční počet](media/log-analytics-custom-fields/initial-count.png)

Po určité době uplynutí tak nové události se mají shromažďovat, uvidíte, který hello **Service_CF** pole je nyní přidáván toorecords, který naše kritériím.

![Konečné výsledky](media/log-analytics-custom-fields/final-results.png)

Jsme teď můžete použít vlastní pole hello jako libovolné jiné vlastnosti záznamu.  tooillustrate, se nám vytvořit dotaz, které jsou seskupeny podle hello nové **Service_CF** tooinspect pole, které služby jsou nejaktivnější hello.

![Seskupit podle dotazu](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) toobuild dotazy pomocí vlastních polí pro kritéria.
* Monitorování [vlastní protokol soubory](log-analytics-data-sources-custom-logs.md) , analyzovat pomocí vlastních polí.

