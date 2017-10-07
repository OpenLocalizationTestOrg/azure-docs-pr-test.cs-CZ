---
title: aaaLearn jak toouse hello konektor Twitter v aplikace logiky | Microsoft Docs
description: "Přehled konektor Twitter s parametry rozhraní REST API"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: ead4e4dc95bf894fd72b908c5375b407ba27642d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twitter-connector"></a>Začínáme s konektor Twitter hello
Konektor Twitter hello můžete:

* Odesílání tweetů a tweetů get
* Časové osy přístup, přátele a délky
* Proveďte jednu z hello jiných akcí a aktivační události popsané dole  

toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky. Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).  

## <a name="connect-tootwitter"></a>Připojit tooTwitter
Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby. A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.  

### <a name="create-a-connection-tootwitter"></a>Vytvoření připojení tooTwitter
> [!INCLUDE [Steps toocreate a connection tooTwitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a>Použít aktivační událost služby Twitter.
Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky. [Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

V tomto příkladu I vám ukáže, jak toouse hello **při odeslání nové tweet** aktivovat toosearch pro #Seattle a pokud najde #Seattle, aktualizujte soubor v Dropbox hello text z hello tweet. V příklad enterprise může vyhledat hello název vaší společnosti a aktualizace databáze SQL z hello tweet textu hello.

1. Zadejte *twitter* hello vyhledávacího pole v designeru aplikace logiky hello zvolte hello **Twitter - při odeslání nové tweet** aktivační události   
   ![Obrázek aktivační události služby Twitter 1](./media/connectors-create-api-twitter/trigger-1.png)  
2. Zadejte *#Seattle* v hello **hledaný Text** ovládací prvek  
   ![Obrázek aktivační události služby Twitter 2](./media/connectors-create-api-twitter/trigger-2.png) 

Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která zahájí spuštění hello jiných triggery a akce v pracovním postupu hello. 

> [!NOTE]
> Pro logiku aplikace toobe funkční musí obsahovat nejméně jedna aktivační událost a jedna akce. Postupujte podle kroků hello v další části tooadd hello akce.  
> 
> 

## <a name="add-a-condition"></a>Přidat podmínku
Vzhledem k tomu, že jsme zajímá pouze tweetů od uživatelů s více než 50 uživateli, podmínku, která potvrdí hello počet délky je nejprve nutné přidat toohello aplikace logiky.  

1. Vyberte **+ nový krok** tooadd hello akce chcete tootake při #Seattle se nachází v nové tweet  
   ![Obrázek akce Twitter 1](../../includes/media/connectors-create-api-twitter/action-1.png)  
2. Vyberte hello **přidat podmínku** odkaz.  
   ![Obrázek stavu Twitter 1](../../includes/media/connectors-create-api-twitter/condition-1.png)   
   Tím se otevře hello **podmínku** řízení, kde můžete zkontrolovat podmínky, jako *rovná*, *je menší než*, *je větší než*, *obsahuje*atd.  
   ![Obrázek stavu Twitter 2](../../includes/media/connectors-create-api-twitter/condition-2.png)   
3. Vyberte hello **zvolte hodnotu** ovládacího prvku.  
   U tohoto ovládacího prvku můžete vybrat jednu nebo více vlastností hello z libovolné předchozí akce nebo aktivační události jako hello hodnotu, jehož podmínka bude vyhodnotí tootrue, nebo hodnotu NEPRAVDA.
   ![Obrázek stavu Twitter 3](../../includes/media/connectors-create-api-twitter/condition-3.png)   
4. Vyberte hello **...**  tooexpand hello seznam vlastností, aby se zobrazily všechny hello vlastnosti, které jsou k dispozici.        
   ![Obrázek stavu Twitter 4](../../includes/media/connectors-create-api-twitter/condition-4.png)   
5. Vyberte hello **délky počet** vlastnost.    
   ![Obrázek stavu Twitter 5](../../includes/media/connectors-create-api-twitter/condition-5.png)   
6. Všimněte si počtu vlastnost je nyní v ovládacím prvku hello hodnota délky hello.    
   ![Obrázek stavu Twitter 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. Vyberte **je větší než** hello operátory seznamu.    
   ![Obrázek stavu Twitter 7](../../includes/media/connectors-create-api-twitter/condition-7.png)   
8. Zadejte 50, hello operand pro hello *je větší než* operátor.  
   Podmínka Hello je nyní přidána. Uložte si práci pomocí hello **Uložit** odkaz v nabídce hello výše.    
   ![Twitter podmínku obrázek 8](../../includes/media/connectors-create-api-twitter/condition-8.png)   

## <a name="use-a-twitter-action"></a>Pomocí akce služby Twitter.
Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky. [Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Teď, když jste přidali aktivační událost, postupujte podle těchto kroků tooadd akci, která bude odeslána nové tweet s hello obsah hello tweetů nalezen hello aktivační procedura. Pro účely tohoto návodu hello budou odeslány pouze tweetů od uživatelů s více než 50 délky.  

V dalším kroku hello přidáte Twitter akci, která bude odeslána tweet pomocí některé z vlastností hello každý tweet, který byl odeslán uživatelem, který má více než 50 délky.  

1. Vyberte **přidat akci**. Otevře se ovládací prvek vyhledávání hello kde můžete vyhledat další akce a aktivační události.  
   ![Obrázek stavu Twitter 9](../../includes/media/connectors-create-api-twitter/condition-9.png)   
2. Zadejte *twitter* do vyhledávacího pole hello vyberte hello **Twitter – Post tweet** akce. Tím se otevře hello **Post tweet** řídit, kdy bude zadejte všechny podrobnosti hello tweet odeslání.      
   ![Twitter akce obrázek 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)   
3. Vyberte hello **Tweet text** ovládacího prvku. Všechny výstupy z předchozí akce a aktivačních událostí v aplikaci logiky hello jsou nyní zobrazeny. Můžete vybrat některý z těchto a použít je jako část textu hello tweet o nové tweet hello.     
   ![Obrázek akce Twitter 2](../../includes/media/connectors-create-api-twitter/action-2.png)   
4. Vyberte **uživatelské jméno**   
5. Zadejte *říká:* v ovládacím prvku text tweet hello. To lze proveďte pouze po uživatelské jméno.  
6. Vyberte *Tweet text*.       
   ![Obrázek akce Twitter 3](../../includes/media/connectors-create-api-twitter/action-3.png)   
7. Uložte si práci a odeslala tweet s hello #Seattle hashtag tooactivate pracovního postupu.  


## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/twitterconnector/). 

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)

