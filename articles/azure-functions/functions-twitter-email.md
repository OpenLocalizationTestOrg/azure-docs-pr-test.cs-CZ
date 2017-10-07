---
title: "aaaCreate funkci, která se integruje se službou Azure Logic Apps | Microsoft Docs"
description: "Vytvoří funkci, která se integruje se službou Azure Logic Apps a kognitivní služby Azure postojích toocategorize tweet a odesílání oznámení, když postojích je nízký."
services: functions, logic-apps, cognitive-services
keywords: "pracovní postup, cloudové aplikace, cloudové služby, firemní procesy, systémová integrace, integrace podnikových aplikací, enterprise application integration, EAI"
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e176bd946af9a3684b3ad0e4b1bed1c3ee344019
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Vytvoří funkci, která se integruje se službou Azure Logic Apps

Azure Functions se integruje s Azure Logic Apps v hello návrhář aplikace logiky. Tato integrace umožňuje použít hello výpočetní výkon funkcí v orchestrations s jinými Azure a služby třetích stran. 

Tento kurz ukazuje, jak funguje toouse s Logic Apps a kognitivní služby Azure postojích tooanalyze z příspěvky služby Twitter. Funkce protokolu HTTP aktivované rozděluje tweetů jako zelené, žluté nebo červené podle skóre postojích hello. E-mail je odeslán, když je zjištěna nízký postojích. 

![první dva kroky Image aplikace v návrháři aplikace logiky](media/functions-twitter-email/designer1.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření účtu kognitivní Services.
> * Vytvoří funkci, která rozděluje tweet postojích.
> * Vytvoření aplikace logiky, která se připojuje tooTwitter.
> * Přidáte aplikaci logiky toohello postojích detekce. 
> * Připojte hello logiku aplikace toohello funkce.
> * Odešlete e-mail podle hello odpověď z funkce hello.

## <a name="prerequisites"></a>Požadavky

+ Aktivní [Twitter](https://twitter.com/) účtu. 
+ [Outlook.com](https://outlook.com/) účet (pro posílání oznámení).
+ Toto téma se používá jako jeho výchozí prostředky hello bodu vytvořené v [vytvořit svoji první funkci z portálu Azure hello](functions-create-first-azure-function.md).  
Pokud jste tak již neučinili, dokončete tyto kroky nyní toocreate funkce aplikace.

## <a name="create-a-cognitive-services-account"></a>Vytvoření účtu kognitivní Services

Účet kognitivní Services je postojích hello požadované toodetect z tweetů monitorovány.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).

2. Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.

3. Klikněte na tlačítko **Data + analýzy** > **kognitivní služby**. Pak použít nastavení hello jako zadaný v tabulce hello, přijměte podmínky hello a zkontrolujte **Pin toodashboard**.

    ![Vytvoření okna kognitivní účtu](media/functions-twitter-email/cog_svcs_account.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                                        |
    | --- | --- | --- |
    | **Název** | MyCognitiveServicesAccnt | Zvolte název jedinečný účet. |
    | **Typ rozhraní API** | Rozhraní Text Analytics API | Rozhraní API používá tooanalyze text.  |
    | **Umístění** | Západní USA | V současné době pouze **západní USA** je k dispozici pro Analýza textu. |
    | **Cenová úroveň** | F0 | Začněte s nejnižší úroveň hello. Pokud spustíte z volání, škálovat tooa vyšší úroveň.|
    | **Skupina prostředků** | myResourceGroup | Použití hello stejnou skupinu prostředků pro všechny služby v tomto kurzu.|

4. Klikněte na tlačítko **vytvořit** toocreate váš účet. Po vytvoření účtu hello, klikněte na tlačítko nový řídicí panel kognitivní služby definovaného toohello účtu. 

5. V hello účet, klikněte na **klíče**a poté zkopírujte hodnotu hello **klíč 1** a uložte ho. Můžete použít tento klíč tooconnect hello logiku aplikace tooyour účtu kognitivní Services. 
 
    ![Klíče](media/functions-twitter-email/keys.png)

## <a name="create-hello-function"></a>Vytvoření funkce hello

Funkce nabízí skvělý způsob toooffload úloh zpracování v pracovním postupu logiku aplikace. Tento kurz používá protokolu HTTP aktivované funkce tooprocess tweet postojích skóre z kognitivní služeb a vrátí hodnotu kategorie.  

1. Rozbalte funkce aplikace, klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**, klikněte na tlačítko hello **HTTPTrigger** šablony. Typ `CategorizeSentiment` pro funkci hello **název** a klikněte na tlačítko **vytvořit**.

    ![Okno aplikace funkce, funkce +](media/functions-twitter-email/add_fun.png)

2. Nahraďte hello obsah souboru run.csx hello hello následující kód a potom klikněte na **Uložit**:

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // hello sentiment category defaults too'GREEN'. 
        string category = "GREEN";
    
        // Get hello sentiment score from hello request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("hello sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set hello category based on hello sentiment score.
        if (score < .3)
        {
            category = "RED";
        }
        else if (score < .6)
        {
            category = "YELLOW";
        }
        return req.CreateResponse(HttpStatusCode.OK, category);
    }
    ```
    Tento kód funkce vrátí barevné kategorie podle skóre postojích hello dostali hello požadavku. 

3. Funkce hello tootest, klikněte na tlačítko **Test** v karta hello nejvíce vpravo tooexpand hello testu. Zadejte hodnotu `0.2` pro hello **text žádosti**a potom klikněte na **spustit**. Hodnota **RED** je vrácený v textu hello hello odpovědi. 

    ![Testování funkce hello v hello portálu Azure](./media/functions-twitter-email/test.png)

Nyní máte funkci, která rozděluje postojích skóre. Dále vytvoříte aplikaci logiky, která integruje funkce s vaše účty služby Twitter a kognitivní služby. 

## <a name="create-a-logic-app"></a>Vytvoření aplikace logiky   

1. V hello portálu Azure, klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.

2. Klikněte na tlačítko **Enterprise integrace** > **aplikace logiky**. Poté použití hello nastavení zadané v tabulce hello, zkontrolujte **Pin toodashboard**a klikněte na tlačítko **vytvořit**.
 
4. Poté zadejte **název** jako `TweetSentiment`, použijte nastavení hello uvedeného v tabulce hello, přijměte podmínky hello a zkontrolujte **Pin toodashboard**.

    ![Vytvoření aplikace logiky v hello portálu Azure](./media/functions-twitter-email/new_logicApp.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                                        |
    | ----------------- | ------------ | ------------- |
    | **Název** | TweetSentiment | Vyberte vhodný název pro vaši aplikaci. |
    | **Skupina prostředků** | myResourceGroup | Rozhraní API používá tooanalyze text.  |
    | **Umístění** | Východ USA | Zvolte tooyou zavřít umístění. |
    | **Skupina prostředků** | myResourceGroup | Zvolte hello stejné existující skupině prostředků jako předtím.|

4. Klikněte na tlačítko **vytvořit** toocreate svou aplikaci logiky. Po vytvoření aplikace hello klikněte na tlačítko nový řídicí panel definovaného toohello aplikace logiky. Pak v hello návrhář aplikace logiky, posuňte se dolů a klikněte na hello **prázdné aplikace logiky** šablony. 

    ![Prázdná šablona aplikace logiky](media/functions-twitter-email/blank.png)

Teď můžete použít hello logiku aplikace Návrhář tooadd služby a aktivuje tooyour aplikace.

## <a name="connect-tootwitter"></a>Připojit tooTwitter

Nejprve vytvořte připojení tooyour účtu sítě Twitter. aplikace logiky Hello se dotazuje na tweetů, které aktivují toorun aplikace hello.

1. V Návrháři hello, klikněte na tlačítko hello **Twitter** služby a klikněte na tlačítko hello **při odeslání nové tweet** aktivační události. Přihlaste se tooyour účtu sítě Twitter a autorizovat Logic Apps toouse váš účet.

2. Použijte nastavení aktivační události služby Twitter hello jako tabulka hello. 

    ![Nastavení konektoru služby Twitter.](media/functions-twitter-email/azure_tweet.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                                        |
    | ----------------- | ------------ | ------------- |
    | **Hledaný text** | #Azure | Použijte hashtagu, který je dostatečně oblíbených pro nové tweetů toogenerate hello vybrali intervalu. Při použití úroveň Free hello a vaše hashtag je příliš oblíbených, můžete rychle použít hello transakce ve vašem účtu kognitivní služby. |
    | **Frekvence** | Minuta | jednotka frekvence Hello sloužící pro cyklické dotazování služby Twitter.  |
    | **Interval** | 15 | Uplynulý čas Hello mezi požadavků služby Twitter, v jednotkách frekvence. |

3.  Klikněte na tlačítko **Uložit** tooconnect tooyour účtu sítě Twitter. 

Nyní je vaše aplikace připojená tooTwitter. V dalším kroku připojíte tootext analytics toodetect hello představu o shromážděných tweetů.

## <a name="add-sentiment-detection"></a>Přidat postojích detekce

1. Klikněte na tlačítko **nový krok**a potom **přidat akci**.

    ![Nový krok a potom přidat akci.](media/functions-twitter-email/new_step.png)

2. V **vybrat akci**, klikněte na tlačítko **Analýza textu**a potom klikněte na hello **zjistit postojích** akce.

    ![Zjištění postojích](media/functions-twitter-email/detect_sent.png)

3. Zadejte například název připojení `MyCognitiveServicesConnection`, vložte hello klíč pro vaše kognitivní služby účtu, který jste uložili a klikněte na tlačítko **vytvořit**.  

4. Klikněte na tlačítko **Text tooanalyze** > **Tweet text**a potom klikněte na **Uložit**.  

    ![Zjištění postojích](media/functions-twitter-email/ds_tta.png)

Teď, když je nakonfigurovaná postojích detekce, můžete přidat funkci tooyour připojení, která využívá hello postojích skóre výstup.

## <a name="connect-sentiment-output-tooyour-function"></a>Připojit postojích výstup tooyour funkce

1. V hello návrhář aplikace logiky, klikněte na **nový krok** > **přidat akci**a potom klikněte na **Azure Functions**. 

2. Klikněte na tlačítko **zvolte Azure funkce**, vyberte hello **CategorizeSentiment** funkce, které jste vytvořili dříve.  

    ![Azure funkce pole zobrazující zvolte Azure funkce](media/functions-twitter-email/choose_fun.png)

3. V **text žádosti**, klikněte na tlačítko **skóre** a potom **Uložit**.

    ![Hodnocení](media/functions-twitter-email/trigger_score.png)

Nyní funkce se aktivuje, když postojích skóre se odesílá z aplikace logiky hello. Barevně kategorie je aplikace logiky toohello vrácené funkcí hello. Dál přidejte e-mailové oznámení, odeslaný při hodnotu postojích **RED** je vrácen z funkce hello. 

## <a name="add-email-notifications"></a>Přidání e-mailových oznámení

poslední část Hello hello pracovního postupu je tootrigger e-mailu, když hello postojích skóre pro magnitudu jako _RED_. Toto téma používá konektor Outlook.com. Můžete provést podobné kroky toouse konektor z Gmailu nebo Office 365 Outlook.   

1. V hello návrhář aplikace logiky, klikněte na **nový krok** > **přidat podmínku**. 

2. Klikněte na tlačítko **zvolte hodnotu**, pak klikněte na tlačítko **textu**. Vyberte **rovná**, klikněte na tlačítko **zvolte hodnotu** a typ `RED`a klikněte na tlačítko **Uložit**. 

    ![Přidáte aplikaci logiky toohello podmínku.](media/functions-twitter-email/condition.png)

3. V **Pokud ano, NEDĚLAT nic**, klikněte na tlačítko **přidat akci**, vyhledejte `outlook.com`, klikněte na tlačítko **e-mailovou zprávu**a přihlaste se tooyour účet Outlook.com.
    
    ![Vyberte akci pro podmínku hello.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > Pokud nemáte účet Outlook.com, můžete vybrat jiný konektor, třeba z Gmailu nebo Office 365 Outlook

4. V hello **e-mailovou zprávu** akci, zadat nastavení e-mailu hello použijte jako v tabulce hello. 

    ![Nakonfigurujte hello e-mailu pro odeslání hello akce e-mailu.](media/functions-twitter-email/sendemail.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis  |
    | ----------------- | ------------ | ------------- |
    | **K** | Zadejte e-mailovou adresu | Hello e-mailovou adresu, která obdrží oznámení hello. |
    | **Předmět** | Záporné tweet postojích zjistil  | Hello předmět hello e-mailové oznámení.  |
    | **Text** | Text tweet, umístění | Klikněte na tlačítko hello **Tweet text** a **umístění** parametry. |

5.  Klikněte na **Uložit**.

Teď, když pracovní postup hello je dokončen, můžete povolit aplikaci logiky hello a najdete v části funkce hello v práci.

## <a name="test-hello-workflow"></a>Pracovní postup testovacího hello

1. V hello návrhář aplikace logiky, klikněte na **spustit** toostart hello aplikace.

2. V levém sloupci hello, klikněte na **přehled** toosee hello stav hello logiku aplikace. 
 
    ![Stav spuštění aplikace logiky](media/functions-twitter-email/over1.png)

3. (Volitelné) Klikněte na jednu z hello spustí toosee podrobnosti o provádění hello.

4. Přejděte tooyour funkce, hello protokoly a ověřte, že postojích hodnoty byly přijme a zpracuje.
 
    ![Zobrazit protokoly – funkce](media/functions-twitter-email/sent.png)

5. Když se zjistí potenciálně záporné postojích, obdržíte e-mail. Pokud jste neobdrželi e-mailu, můžete změnit hello funkce kód tooreturn RED pokaždé, když:

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    Po ověření e-mailová oznámení, změňte back toohello původní kód:

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > Po dokončení tohoto kurzu, měli byste zakázat aplikaci logiky hello. Zakázáním aplikace hello se vyhnout se účtovat pro spuštění a pomocí hello transakce ve vašem účtu kognitivní služby.

Nyní jste viděli, jak je snadné toointegrate funkce do pracovního postupu Logic Apps.

## <a name="disable-hello-logic-app"></a>Vypnout aplikaci logiky hello

aplikace logiky hello toodisable, klikněte na tlačítko **přehled** a pak klikněte na **zakázat** hello horní části úvodní obrazovka. To zastaví hello aplikace logiky spuštěná a nabíhání poplatků za bez odstranění aplikace hello. 

![Protokoly – funkce](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření účtu kognitivní Services.
> * Vytvoří funkci, která rozděluje tweet postojích.
> * Vytvoření aplikace logiky, která se připojuje tooTwitter.
> * Přidáte aplikaci logiky toohello postojích detekce. 
> * Připojte hello logiku aplikace toohello funkce.
> * Odešlete e-mail podle hello odpověď z funkce hello.

Jak zálohy další kurz toolearn toohello toocreate bez serveru rozhraní API pro funkce.

> [!div class="nextstepaction"] 
> [Vytvoření rozhraní API bez serveru pomocí služby Azure Functions](functions-create-serverless-api.md)

toolearn Další informace o Logic Apps, najdete v části [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).

