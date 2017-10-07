---
title: aaaUse hello Slack konektor v Azure logic apps | Microsoft Docs
description: "Připojit tooSlack ve vašich logic apps"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 6599d7b69d2147425c9fab978c5d0f93e5605f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-slack-connector"></a>Začínáme s konektorem Slack hello
Slack je nástroj komunikace týmu, který spojuje všechny team komunikace v jednom umístění, okamžitě vyhledávat a bez ohledu na přejdete k dispozici. 

Začněte vytvořením aplikace logiky nyní; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-tooslack"></a>Vytvoření připojení tooSlack
toouse hello Slack konektor, nejprve vytvoříte **připojení** pak zadejte hello podrobnosti pro tyto vlastnosti: 

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| Token |Ano |Zadejte přihlašovací údaje Slack |

Postupujte podle těchto kroků toosign do systému Slack a dokončení hello konfiguraci hello Slack **připojení** v aplikaci logiky:

1. Vyberte **opakování**
2. Vyberte **frekvence** a zadejte **intervalu**
3. Vyberte **přidat akci**  
   ![Konfigurace systému Slack][1]  
4. Zadejte Slack hello vyhledávacího pole a počkejte hello vyhledávání tooreturn všech položek s Slack v názvu hello
5. Vyberte **Slack - Post zpráv**
6. Vyberte **přihlášení tooSlack**:  
   ![Konfigurace systému Slack][2]
7. Zadejte vaše toosign Slack přihlašovací údaje v aplikaci tooauthorize hello    
   ![Konfigurace systému Slack][3]  
8. Budete přesměrovaného tooyour organizace přihlašovací stránky. **Autorizovat** Slack toointeract s svou aplikaci logiky:      
   ![Konfigurace systému Slack][5] 
9. Po dokončení autorizace hello budete přesměrovaného tooyour logiku aplikace toocomplete ji tak, že nakonfigurujete hello **Slack – získat všechny zprávy** části. Přidejte další aktivační události a akce, které potřebujete.  
   ![Konfigurace systému Slack][6]
10. Uložte si práci, výběrem **Uložit** v řádku nabídek hello výše.

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/slack/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
