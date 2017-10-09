---
title: "Analýza postojích Twitter aaaReal běhu pomocí služby Azure Stream Analytics | Microsoft Docs"
description: "Zjistěte, jak toouse Stream Analytics k analýze postojích v reálném čase služby Twitter. Podrobné pokyny z toodata generování událostí na řídicím panelu za provozu."
keywords: "Analýza trendu v reálném čase twitter, postojích analýzy, analýzy sociálních médií, příklad analýza trendu"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42068691-074b-4c3b-a527-acafa484fda2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: jeffstok
ms.openlocfilehash: 157790caa7ea6f5570dd9c9d3bd9694d437eb4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Analýzy v reálném čase postojích Twitter v Azure Stream Analytics

Zjistěte, jak toobuild postojích analýzu řešení pro analýzy sociálních médií tak, že převedou v reálném čase Twitter události do centra událostí Azure. Poté můžete napsat data hello tooanalyze dotaz Azure Stream Analytics a buď úložiště hello výsledky pro pozdější použití nebo použít řídicí panel a [Power BI](https://powerbi.com/) tooprovide statistiky v reálném čase.

Analýza nástrojů sociálních médiích pomůžou organizacím pochopit trendů témata. Trendů témata jsou témata a postojích, které mají k velkému počtu příspěvcích v sociálních sítích. Postojích analýzy, která se označuje taky jako *stanovisko dolování*, používá sociálních médií analytics nástroje toodetermine postoje směrem k produktu, je vhodné a tak dále. 

Analýza trendu v reálném čase Twitter je skvělé příklad nástroj pro analýzu, protože hello hashtag předplatné modelu vám umožní toospecific toolisten (hashtags) – klíčová slova a vyvíjet postojích analýzu hello informačního kanálu.

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a>Scénář: Sociálních médií postojích analýzy v reálném čase

Společnost, která má web zprávy média má zájem o získání několik výhod před konkurencí podle s funkcí lokality obsah, který je okamžitě relevantní tooits čtečky. Společnost Hello používá analýzy sociálních médiích na témata, která jsou relevantní tooreaders díky v reálném čase postojích analýza dat Twitteru.

témata trendů tooidentify v reálném čase v síti Twitter, hello společnosti potřeby analýzy v reálném čase o hello tweet svazek a postojích klíče témata. Jinými slovy potřeba hello je modul postojích analysis analytics je založena na této sociálních médií informačního kanálu.

## <a name="prerequisites"></a>Požadavky
V tomto kurzu použijete klientská aplikace, která se připojuje tooTwitter a hledá tweetů, které mají určité hashtags (kterou můžete nastavit). V pořadí toorun hello aplikace a analyzovat hello tweetů pomocí Azure streamování Analytics, musíte mít následující hello:

* Předplatné Azure
* Účet na Twitteru 
* Aplikace služby Twitter a hello [přístupového tokenu OAuth](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) pro příslušnou aplikaci. Poskytujeme vysoké úrovně pokyny, jak toocreate aplikace Twitter později.
* Hello TwitterWPFClient aplikace, který čte hello Twitter informačního kanálu. tooget pro tuto aplikaci, stahování hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) souborů z webu GitHub a potom rozbalte balíček hello do složky v počítači. Pokud chcete, aby toosee hello zdrojového kódu a spustit aplikaci hello v ladicí program, můžete získat hello zdrojového kódu z [Githubu](https://aka.ms/azure-stream-analytics-telcogenerator). 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a>Vytvoření centra událostí pro vstup analýzy datových proudů

Ukázková aplikace Hello vygeneruje události a nabízených oznámení, je centra událostí Azure tooan. Azure event hubs je hello upřednostňovaný způsob přijímání událostí pro Stream Analytics. Další informace najdete v tématu hello [dokumentace Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).


### <a name="create-an-event-hub-namespace-and-event-hub"></a>Vytvoření centra událostí obor názvů a centra událostí
V tomto postupu vytvoříte na obor názvů centra událostí a poté přidejte na obor názvů toothat centra událostí. Obory názvů centra událostí jsou používány toologically seskupení souvisejících událostí sběrnice instancí. 

1. Přihlaste se toohello portál Azure a klikněte na tlačítko **nový** > **Internet věcí** > **centra událostí**. 

2. V hello **vytvoření oboru názvů** okno, zadejte název oboru názvů, jako `<yourname>-socialtwitter-eh-ns`. Můžete použít libovolný název pro obor názvů hello, ale název hello musí být platné adresy URL a musí být jedinečný v Azure. 
    
3. Vyberte předplatné a vytvořit nebo vybrat skupinu prostředků a potom klikněte na tlačítko **vytvořit**. 

    ![Vytvoření oboru názvů události rozbočovače](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. Po dokončení nasazení obor názvů hello najdete hello události rozbočovače oboru názvů v seznamu prostředků Azure. 

5. Klikněte na tlačítko Nový obor názvů hello a v okně hello obor názvů, klikněte na tlačítko  **+ &nbsp;centra událostí**. 

    ![tlačítko Přidat centra událostí Hello pro vytvoření nového centra událostí ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. Název hello nového centra událostí `socialtwitter-eh`. Můžete použít jiný název. Pokud tak učiníte, poznamenejte si ho, protože potřebujete název hello později. Nepotřebujete tooset další možnosti pro centra událostí hello.

    ![Okno pro vytvoření nového centra událostí](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. Klikněte na možnost **Vytvořit**.


### <a name="grant-access-toohello-event-hub"></a>Centra událostí toohello udělení přístupu

Předtím, než se proces může odesílat data centra událostí tooan, musí mít centra událostí hello zásadu, která umožňuje odpovídající přístup. zásady přístupu Hello vytvoří připojovací řetězec, který obsahuje informace o ověření.

1.  V okně oboru názvů událostí hello, klikněte na tlačítko **Event Hubs** a pak klikněte na název hello nového centra událostí.

2.  V okně Centrum událostí hello, klikněte na tlačítko **zásady sdíleného přístupu** a pak klikněte na  **+ &nbsp;přidat**.

    >[!NOTE]
    >Ujistěte se, že pracujete s hello centra událostí, není hello názvů centra událostí.

3.  Přidat zásadu s názvem `socialtwitter-access` a **deklarace identity**, vyberte **spravovat**.

    ![Okno pro vytvoření nové zásady přístupu centra událostí](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  Klikněte na možnost **Vytvořit**.

5.  Po hello byla zásada nasazená, klikněte na něj v seznamu hello zásady sdíleného přístupu.

6.  Najít hello políčko s názvem **PŘIPOJOVACÍ řetězec primární klíč** a klikněte na tlačítko hello kopie tlačítko Další toohello připojovací řetězec. 
    
    ![Kopírování hello primární připojovací řetězec klíč ze zásad přístupu hello](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  Vložte připojovací řetězec hello do textového editoru. Tento připojovací řetězec pro další části hello, musíte po provedení některé tooit malé úpravy.

    Hello připojovací řetězec vypadá takto:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    Všimněte si, že hello připojovací řetězec obsahuje více párů klíč hodnota oddělené středníky: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, a `EntityPath`.  

    > [!NOTE]
    > Pro zabezpečení se odebraly částí hello připojovací řetězec v příkladu hello.

8.  V textovém editoru hello odebrat hello `EntityPath` pár z hello připojovací řetězec (Nezapomeňte tooremove hello středník, předcházejícího). Když jste hotovi, hello připojovací řetězec vypadá takto:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-hello-twitter-client-application"></a>Nakonfigurujte a spusťte hello Twitter klientské aplikace
klientská aplikace Hello získá tweet události přímo ze služby Twitter. V pořadí toodo, tak, potřebuje oprávnění toocall hello Twitter streamování rozhraní API. tooconfigure, oprávnění, vytvoříte aplikaci v Twitter, který generuje jedinečné přihlašovací údaje (například tokenu OAuth). Potom můžete nakonfigurovat hello klienta aplikace toouse tyto přihlašovací údaje při volání rozhraní API. 

### <a name="create-a-twitter-application"></a>Vytvořit aplikaci služby Twitter.
Pokud již nemáte Twitter aplikace, která můžete použít pro tento kurz, můžete vytvořit jeden. Již musí mít účet služby Twitter.

> [!NOTE]
> přesný postup Hello v Twitter pro vytváření aplikací a získávání klíčů hello, tajných klíčů a token může změnit. Pokud tyto pokyny neodpovídají, co vidíte v lokalitě hello Twitter, naleznete v dokumentaci pro vývojáře toohello Twitter.

1. Přejděte toohello [stránku Správa aplikací Twitter](https://apps.twitter.com/). 

2. Vytvoření nové aplikace. 

    * Adresa URL webu hello zadejte platnou adresu URL. Nemá toobe živý web. (Není možné určit pouze `localhost`.)
    * Pole zpětného volání hello zůstat prázdné. Hello klientská aplikace, kterou použijete pro tento kurz nevyžaduje zpětných volání.

    ![Vytváření aplikací v Twitter](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. Volitelně můžete změňte oprávnění aplikace hello jen tooread.

4. Při vytváření aplikace hello přejděte toohello **klíče a přístupové tokeny** stránky.

5. Klikněte na tlačítko toogenerate hello přístupový token a tajný klíč přístupového tokenu.

Ponechat tyto informace užitečné, protože je budete potřebovat v dalším postupu hello.

>[!NOTE]
>Hello klíče a tajné klíče pro aplikaci služby Twitter hello zadejte účet pro přístup k tooyour Twitter. S takovými informacemi nakládat jako velká a malá písmena, hello stejné jako heslo služby Twitter. Nevnořujte například v aplikaci tyto informace, že poskytnete tooothers. 


### <a name="configure-hello-client-application"></a>Konfigurace hello klientské aplikace
Vytvořili jsme klientskou aplikaci, která se připojuje tooTwitter dat pomocí [rozhraní API pro Streaming na Twitteru](https://dev.twitter.com/streaming/overview) toocollect tweet události týkající se konkrétní sadu témat. aplikace Hello používá hello [Sentiment140](http://help.sentiment140.com/) nástroj s otevřeným zdrojem, který přiřazuje hello následující postojích hodnotu tooeach tweet:

* 0 = záporná
* 2 = neutral
* 4 = kladné

Po události tweet hello byly přiřazeny hodnotu postojích, odesílají se toohello centra událostí, který jste vytvořili dříve.

Před spuštěním aplikace hello, vyžaduje určité informace z vašeho počítače, jako je hello Twitter klíče a hello event hub připojovací řetězec. Můžete zadat informace o konfiguraci hello těmito způsoby:

* Spuštění aplikace hello a potom pomocí aplikace hello uživatelského rozhraní tooenter hello klíčů, tajné klíče a připojovací řetězec. Pokud to uděláte, informace o konfiguraci hello se používá pro aktuální relaci, ale nebyla uložena.
* Upravte soubor .config hello aplikace a hodnoty sady hello. Tento přístup udržuje informace o konfiguraci hello, ale je také znamená, že tento potenciálně citlivé informace je uložena ve formátu prostého textu ve vašem počítači.

Hello následující postup dokumenty obou přístupů. 

1. Zkontrolujte, že jste stažené a rozbalené hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) aplikaci, jak je uvedeno v hello požadavky.

2. tooset hello hodnoty v době běhu (a pouze pro aktuální relaci hello), spusťte hello `TwitterWPFClient.exe` aplikace. Pokud aplikace hello vás vyzve, zadejte hello následující hodnoty:

    * Hello Twitter uživatelský klíč (klíč rozhraní API).
    * Hello Twitter uživatelský tajný klíč (tajný klíč rozhraní API).
    * Hello Twitter tokenu přístupu.
    * Hello Twitter tajný klíč přístupového tokenu.
    * Hello připojovací řetězec informace, které jste předtím uložili. Ujistěte se, že používáte hello připojovací řetězec, že jste odebrali hello `EntityPath` dvojice klíč hodnota z.
    * Hello Twitter klíčová slova, která chcete toodetermine postojích pro.

   ![TwitterWpfClient aplikace spuštěna, zobrazující zakryt nastavení](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. hodnoty hello tooset trvalé, pomocí textového editoru tooopen hello TwitterWpfClient.exe.config souboru. Potom v hello `<appSettings>` elementu, to udělat:

    * Nastavit `oauth_consumer_key` toohello Twitter uživatelský klíč (klíč rozhraní API). 
    * Nastavit `oauth_consumer_secret` toohello Twitter uživatelský tajný klíč (tajný klíč rozhraní API).
    * Nastavit `oauth_token` toohello Twitter tokenu přístupu.
    * Nastavit `oauth_token_secret` toohello Twitter tajný klíč přístupového tokenu.

    Dále v hello `<appSettings>` elementu, tyto změny:

    * Nastavit `EventHubName` toohello název centra událostí (tedy toohello hodnota hello entity cesty).
    * Nastavit `EventHubNameConnectionString` toohello připojovací řetězec. Ujistěte se, že používáte hello připojovací řetězec, že jste odebrali hello `EntityPath` dvojice klíč hodnota z.

    Hello `<appSettings>` části vypadá hello následující ukázka. (Pro přehlednost a zabezpečení, jsme zabalená některé řádky a odebrat některé znaky.)

    ![Konfigurační soubor aplikace TwitterWpfClient v textovém editoru, zobrazující hello Twitter klíče a tajné klíče a informace o připojovacím řetězci hello události rozbočovače](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. Pokud již jste nezahájili hello aplikace, spusťte nyní TwitterWpfClient.exe. 

5. Klikněte na tlačítko hello zelená počáteční tlačítko toocollect sociálních postojích. Zobrazí události Tweet s hello **CreatedAt**, **tématu**, a **SentimentScore** hodnoty odesílány tooyour centra událostí.

    ![Spuštění aplikace TwitterWpfClient zobrazující seznam tweetů](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    >Pokud se zobrazí chyby a proud tweetů zobrazí v dolní části okna hello hello nevidíte, zkontrolujte hello klíče a tajné klíče. Také zkontrolujte připojovací řetězec hello (ujistěte se, že neobsahuje hello `EntityPath` klíče a hodnoty.)


## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics

Teď, když jsou události tweet streamování v reálném čase z Twitteru, můžete nastavit až tooanalyze úlohy Stream Analytics tyto události v reálném čase.

1. V hello portálu Azure, klikněte na **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.

2. Název úlohy hello `socialtwitter-sa-job` a určete předplatné, skupinu prostředků a umístění.

    Je vhodné tooplace hello úlohy a hello událostí rozbočovač v hello stejné oblasti pro nejlepší výkon a tak, že nemáte platíte tootransfer dat mezi oblastmi.

    ![Vytvoření nové úlohy Stream Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. Klikněte na možnost **Vytvořit**.

    bude vytvořena úloha Hello a hello portálu se zobrazí podrobnosti úlohy.


## <a name="specify-hello-job-input"></a>Zadejte vstup úlohy hello

1. V úloze Stream Analytics v části **úlohy topologie** uprostřed hello hello okno úlohy, klikněte na tlačítko **vstupy**. 

2. V hello **vstupy** okně klikněte na tlačítko  **+ &nbsp;přidat** a potom vyplňte hello okno s těmito hodnotami:

    * **Vstupní alias**: použití hello název `TwitterStream`. Pokud použijete jiný název, poznamenejte si jeho protože ji budete potřebovat později.
    * **Typ zdroje**: vyberte **datový proud**.
    * **Zdroj**: vyberte **centra událostí**.
    * **Import možnost**: vyberte **použijte Centrum událostí z aktuálního předplatného**. 
    * **Obor názvů sběrnice**: Vyberte hello události rozbočovače názvů, který jste vytvořili dříve (`<yourname>-socialtwitter-eh-ns`).
    * **Centra událostí**: Vyberte hello centra událostí, který jste vytvořili dříve (`socialtwitter-eh`).
    * **Název zásady centra událostí**: Vyberte hello zásady přístupu, kterou jste vytvořili dříve (`socialtwitter-access`).

    ![Vytvořit nový vstupní úlohy streamování Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. Klikněte na možnost **Vytvořit**.


## <a name="specify-hello-job-query"></a>Zadejte dotaz úlohy hello

Stream Analytics podporuje jednoduchý a deklarativní dotazu model, který popisuje transformace. toolearn Další informace o jazyce hello, najdete v části hello [referenční příručka Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Tento kurz vám pomůže vytvořit a vyzkoušet několik dotazů přes Twitter data.

toocompare hello počet zmínkami mezi témata, můžete použít [Přeskakující okno](https://msdn.microsoft.com/library/azure/dn835055.aspx) tooget hello počet zmínkami podle tématu každých pět sekund.

1. Zavřít hello **vstupy** okno, pokud jste tak ještě neučinili.

2. V okně úlohy hello, klikněte na tlačítko hello **dotazu** pole. Azure obsahuje hello vstupy a výstupy, které jsou nakonfigurované pro hello úlohy a umožňuje vám vytvořit dotaz, který umožňuje transformovat vstupního datového proudu hello při přenosu toohello výstup.

3. Ujistěte se, že tento hello TwitterWpfClient aplikace běží. 

3. V hello **dotazu** okně klikněte na tlačítko Další toohello hello tečky `TwitterStream` vstup a pak vyberte **vzorová data ze vstupu**.

    ![Nabídka Možnosti toouse ukázková data pro hello položka se "Ukázkových dat ze vstupu" vybrané úlohy streamování Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    Otevře se okno, které umožňuje určit, kolik tooget ukázková data, definována v tom, jak dlouho tooread hello vstupní datový proud.

4. Nastavit **minut** too3 a pak klikněte na **OK**. 
    
    ![Možnosti vzorkování hello vstupního datového proudu, s vybrané "3 minut".](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    Azure ukázky data z vstupního datového proudu hello za 3 minuty a vás upozorní, jakmile je připraven hello ukázková data. (To trvá nějakou dobu.) 

    Hello ukázková data jsou ukládána dočasně a je k dispozici, když máte otevřete okno dotazu hello. Pokud zavřete okno dotazu hello hello ukázková data zahozena a máte toocreate novou sadu ukázková data. 

5. Změna dotazu hello v hello kód editor toohello následující:

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    Pokud nepoužili `TwitterStream` jako hello alias pro vstup hello, nahraďte váš alias pro `TwitterStream` v dotazu hello.  

    Tento dotaz používá hello **TIMESTAMP BY** – klíčové slovo toospecify pole časového razítka v toobe datové části hello používá v dočasné výpočty hello. Pokud toto pole nezadáte, hello oddílová operace pomocí hello dobu, po kterou všechny události byly přijaty hello centra událostí. Další informace najdete v části hello "čas doručení vs doba aplikace" [referenční příručka Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Tento dotaz také přistoupí časové razítko pro hello konec každé okno pomocí hello **System.Timestamp** vlastnost.

5. Klikněte na tlačítko **Test**. Spustí se dotaz Hello hello data, která jste vzorků.
    
6. Klikněte na **Uložit**. To umožňuje ušetřit hello dotaz jako součást úlohy streamování Analytics hello. (Ho nebude uložen hello ukázková data.)


## <a name="experiment-using-different-fields-from-hello-stream"></a>Experiment pomocí různých polí z datového proudu hello 

Hello následující tabulka uvádí hello pole, které jsou součástí hello streamování data JSON. Zaregistrované volné tooexperiment v editoru dotazů hello.

|Vlastnost JSON | Definice|
|--- | ---|
|CreatedAt | čas Hello byl vytvořen tento tweet hello|
|Téma | Hello téma, které odpovídá hello zadané – klíčové slovo|
|SentimentScore | Hello postojích skóre z Sentiment140|
|Autor | Popisovač Hello Twitter, odeslaný hello tweet|
|Text | úplné tělo Hello hello tweet|


## <a name="create-an-output-sink"></a>Vytvoření výstupní jímku

Nyní jste definovali datového proudu událostí, události vstupní tooingest centra událostí a dotaz tooperform transformace přes hello datového proudu. posledním krokem Hello je toodefine výstupní jímku pro úlohu hello.  

V tomto kurzu zapsat hello agregovat tweet události z hello úlohy dotazu tooAzure úložiště objektů Blob.  Můžete také nabízené vaše výsledky tooAzure databáze SQL Azure Table storage, Event Hubs, nebo Power BI, v závislosti na vaší aplikace potřebuje.

## <a name="specify-hello-job-output"></a>Zadejte hello výstup úlohy

1. V hello **úlohy topologie** klikněte na tlačítko hello **výstup** pole. 

2. V hello **výstupy** okně klikněte na tlačítko  **+ &nbsp;přidat** a potom vyplňte hello okno s těmito hodnotami:

    * **Alias pro výstup**: použití hello název `TwitterStream-Output`. 
    * **Jímky**: vyberte **úložiště objektů Blob**.
    * **Možnosti importu**: vyberte **pomocí úložiště objektů blob z aktuálního předplatného**.
    * **Účet úložiště**. Vyberte **vytvořit nový účet úložiště.**
    * **Účet úložiště** (druhé pole). Zadejte `YOURNAMEsa`, kde `YOURNAME` je název vaší nebo jiné jedinečné řetězce. v názvu Hello lze použít pouze malá písmena a čísla a musí být jedinečný v Azure. 
    * **Kontejner**. Zadejte `socialtwitter`.
    název účtu úložiště Hello a název kontejneru jsou používané společně tooprovide identifikátor URI pro úložiště objektů blob hello, například takto: 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    !["Nové výstup" okno úlohy Stream Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. Klikněte na možnost **Vytvořit**. 

    Azure vytvoří účet úložiště hello a automaticky vygeneruje klíč. 

5. Zavřít hello **výstupy** okno. 


## <a name="start-hello-job"></a>Spuštění úlohy hello

Úloha vstup, dotaz a výstup nejsou zadány. Jste úlohy služby Stream Analytics připravené toostart hello.

1. Ujistěte se, že tento hello TwitterWpfClient aplikace běží. 

2. V okně úlohy hello, klikněte na tlačítko **spustit**.

    ![Spuštění úlohy Stream Analytics hello](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. V hello **spuštění úlohy** okně pro **výstup úlohy počáteční čas**, vyberte **nyní** a pak klikněte na **spustit**. 

    !["Spuštění úlohy" okno úlohy Stream Analytics hello](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    Azure vás upozorní, jakmile byla spuštěna úloha hello a v okně úlohy hello hello stav se zobrazí jako **systémem**.

    ![Spuštěná úloha](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a>Zobrazit výstup pro analýzu postojích

Po zahájení spuštěná vaše úloha zpracovává datový proud v reálném čase pro Twitter hello, můžete zobrazit výstup hello postojích analýzy.

Můžete použít nástroje, jako je [Azure Storage Explorer](https://http://storageexplorer.com/) nebo [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) tooview úlohu výstup v reálném čase. Tady můžete použít [Power BI](https://powerbi.com/) tooextend vaší aplikace tooinclude přizpůsobený řídicí panel, jako ukazuje následující snímek obrazovky hello hello:

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-tooidentify-trending-topics"></a>Vytvořte další dotaz tooidentify trendů témata

Další dotaz můžete použít sentimentu Twitter toounderstand vychází [posuvné okno](https://msdn.microsoft.com/library/azure/dn835051.aspx). témata tooidentify trendů, můžete hledat témata, které zasahují prahová hodnota pro zmínkami ve stanoveném čase.

Pro účely tohoto kurzu hello vyhledejte témata, která jsou uveden více než 20 výskyty v hello posledních 5 sekund.

1. V okně úlohy hello, klikněte na tlačítko **Zastavit** toostop hello úlohy. 

2. V hello **úlohy topologie** klikněte na tlačítko hello **dotazu** pole. 

3. Změna hello dotazu toohello následující:

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. Klikněte na **Uložit**.

5. Ujistěte se, že tento hello TwitterWpfClient aplikace běží. 

6. Klikněte na tlačítko **spustit** toorestart hello úlohy pomocí hello nový dotaz.


## <a name="get-support"></a>Získat podporu
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
