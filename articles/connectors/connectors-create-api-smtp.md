---
title: konektor aaaSMTP v Azure Logic Apps | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. Připojte tooSMTP toosend e-mailu."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 36bb836851014d24f2e069fda8376ad7a08c943b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-smtp-connector"></a>Začínáme s konektor SMTP hello
Připojte tooSMTP toosend e-mailu.

toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky. Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toosmtp"></a>Připojit tooSMTP
Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby. A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby. Například tooconnect tooSMTP, musíte nejdřív serveru SMTP *připojení*. toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello služby, ke kterým se připojujete. Ano v příkladu hello SMTP, zadejte název připojení tooyour hello přihlašovací údaje, adresy serveru SMTP a uživatelské přihlašovací informace toocreate hello připojení tooSMTP.  

### <a name="create-a-connection-toosmtp"></a>Vytvoření připojení tooSMTP
> [!INCLUDE [Steps toocreate a connection tooSMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a>Aktivační událost pomocí serveru SMTP
Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky. [Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

V tomto příkladu, protože SMTP nemá aktivační událost své vlastní, použijeme hello **Salesforce – když je vytvořen objekt** aktivační události. Tento aktivační událost se aktivuje, když je vytvořen nový objekt v Salesforce. Pro náš příklad nastavíme ho tak, aby nové zájemce pokaždé, když je vytvořen v Salesforce, *odeslání e-mailu* akci dojde prostřednictvím konektoru hello SMTP s upozornění při vytváření nové zájemce hello.

1. Zadejte *salesforce* hello vyhledávacího pole v designeru aplikace logiky hello zvolte hello **Salesforce – když je vytvořen objekt** aktivační události.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. Hello **když je vytvořen objekt** ovládací prvek je zobrazen.
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. Vyberte hello **typ objektu** vyberte *vést* hello seznamu objektů. V tomto kroku jsou indikující, že vytváříte aktivační událost, která vás upozorní aplikace logiky, vždy, když se vytvoří nové zájemce v Salesforce.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. Hello aktivační událost byla vytvořena.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Použít akci SMTP
Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky. [Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Teď, když hello aktivační událost byla přidána, postupujte podle těchto kroků tooadd serveru SMTP akci, která se stane, když se vytvoří nové zájemce v Salesforce.

1. Vyberte **+ nový krok** tooadd hello akce chcete tootake při vytváření nové zájemce.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. Vyberte **přidat akci**. Pole pro vyhledávání otevře hello kde můžete vyhledat všechny akce jste chtěli tootake.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. Zadejte *smtp* toosearch pro akce související tooSMTP.  
4. Vyberte **SMTP - odeslat E-mail** jako hello tootake akce při vytváření nové zájemce hello. Otevře se řídicí blok Hello akce. Budete mít tooestablish připojení smtp v bloku návrháře hello Pokud jste tak dosud neučinili dříve.  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. Zadejte příslušné informace požadované e-mailu na hello **SMTP - odeslat E-mail** bloku.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. Uložte práci v pořadí tooactivate pracovního postupu.  

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/smtpconnector/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).
