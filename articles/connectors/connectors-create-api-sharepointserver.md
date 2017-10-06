---
title: "aaaUse hello serveru konektoru služby SharePoint ve vašich Logic Apps | Microsoft Docs"
description: "Začněte používat hello hello konektor Server služby SharePoint ve vašich Logic apps"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 3b814f42611e4971ff5c94ae3b021829217911dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sharepoint-connector"></a>Začínáme s konektorem služby SharePoint hello
Hello konektor služby SharePoint poskytuje toowork způsob, jak se seznamy služby SharePoint.

Začněte vytvořením aplikace logiky; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-toosharepoint"></a>Vytvoření připojení tooSharePoint
toouse hello konektor služby SharePoint, je nejprve vytvořit **připojení** pak zadejte hello podrobnosti pro tyto vlastnosti: 

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| Token |Ano |Zadejte pověření serveru SharePoint |

tooconnect příliš**SharePoint**, zadejte tooSharePoint vaší identity (uživatelské jméno a heslo, pověření čipové karty, atd.). Po jste si ověřit, můžete v aplikaci logiky toouse hello SharePoint konektor. 

Při na hello návrhář aplikace logiky, postupujte podle těchto kroků toosign do služby SharePoint toocreate hello připojení **připojení** pro použití v aplikaci logiky:

1. Hello vyhledávacího pole zadejte SharePoint a počkejte tooreturn hello vyhledávání všech položek s služby SharePoint v názvu hello:   
   ![Konfigurace služby SharePoint][1]  
2. Vyberte **SharePoint – když dojde k vytvoření souboru**   
3. Vyberte **přihlášení tooSharePoint**:   
   ![Konfigurace služby SharePoint][2]    
4. Vaše přihlašovací údaje toosign služby SharePoint v tooauthenticate poskytnout služby SharePoint   
   ![Konfigurace služby SharePoint][3]     
5. Po dokončení ověřování hello budete přesměrovaného tooyour logiku aplikace toocomplete ho konfigurací Sharepointu **vytvoření souboru** dialogové okno.          
   ![Konfigurace služby SharePoint][4]  
6. Poté můžete přidat další triggery a akce, je nutné toocomplete svou aplikaci logiky.   
7. Uložte si práci, výběrem **Uložit** v řádku nabídek hello výše.  

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/sharepoint/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
