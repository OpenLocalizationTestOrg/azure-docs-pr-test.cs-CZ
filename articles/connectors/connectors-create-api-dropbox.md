---
title: konektor aaaDropbox v Azure Logic Apps | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. Připojte tooDropbox toomanage vaše soubory. Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v Dropbox."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cb0ae033-aba7-4ac9-beaa-be561a0f0cac
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1f307477836104c0bc0008341604a1400860987f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-dropbox-connector"></a>Začínáme s konektorem Dropbox hello
Připojte tooDropbox toomanage vaše soubory. Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v Dropbox.

toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky. Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toodropbox"></a>Připojit tooDropbox
Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby. Připojení poskytuje připojení mezi aplikace logiky a jiné služby. Například v pořadí tooconnect tooDropbox, musíte nejdřív Dropbox *připojení*. toocreate připojení, bude třeba tooprovide hello přihlašovací údaje, obvykle použijete tooaccess hello službu, kterou chcete tooconnect k. Ano v příkladu hello Dropbox, bude třeba tooyour hello přihlašovací údaje účtu Dropboxu v pořadí toocreate hello připojení tooDropbox. [Další informace o připojení]()

### <a name="create-a-connection-toodropbox"></a>Vytvoření připojení tooDropbox
> [!INCLUDE [Steps toocreate a connection tooDropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Použít aktivační událost Dropbox
Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky. [Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

V tomto příkladu použijeme hello **vytvoření souboru** aktivační události. Když dojde k této aktivační události, říkáme hello **získat obsah souboru pomocí cesty** Dropbox akce. 

1. Zadejte *dropbox* hello vyhledávacího pole v designeru hello Logic Apps, pak vyberte hello **Dropboxu - Pokud dojde k vytvoření souboru** aktivační události.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Vyberte hello složku, ve kterém chcete tootrack vytváření souborů. Vyberte... (označená červenou hello pole) a procházet toohello složku, vstup tooselect hello aktivační události.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Pomocí akce Dropbox
Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky. [Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Teď, když hello aktivační událost byla přidána, postupujte podle těchto kroků tooadd akci, která bude získat obsah hello nový soubor.

1. Vyberte **+ nový krok** tooadd hello akce chcete tootake při vytvoření nového souboru.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Vyberte **přidat akci**. Pole pro vyhledávání otevře hello kde můžete vyhledat všechny akce jste chtěli tootake.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Zadejte *dropbox* toosearch pro akce související tooDropbox.  
4. Vyberte **Dropbox - Get obsah souboru pomocí cesty** jako hello tootake akce při vytvoření nového souboru v hello vybrané složce Dropbox. Otevře se řídicí blok Hello akce. Můžete se výzvami tooauthorize vaše tooaccess aplikace logiky vaší schránky účtu, pokud jste tak dosud neučinili dříve.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Vyberte... (nachází na pravé straně hello hello **cesta k souboru** ovládací prvek) a cesta k souboru toohello toouse chcete procházet. Nebo použijte hello **cesta k souboru** tokenu toospeed si vaše vytváření aplikace logiky.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Uložte si práci a vytvořte nový soubor v Dropbox tooactivate pracovního postupu.  

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/dropbox/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).
