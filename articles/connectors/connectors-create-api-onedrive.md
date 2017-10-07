---
title: "konektor OneDrive hello aaaAdd ve vašich Logic Apps | Microsoft Docs"
description: "Přehled konektoru OneDrive hello s parametry rozhraní REST API"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 8303794bb3c2844de288f87f40639abb84c160fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-onedrive-connector"></a>Začínáme s konektorem OneDrive hello
Připojte tooOneDrive toomanage vaše soubory, včetně nahrávání, get, odstraňte soubory a další. 

S OneDrive můžete: 

* Vytvoření pracovního postupu uložením souborů na Onedrivu, nebo aktualizovat existující soubory na OneDrive. 
* Použijte toostart aktivuje pracovní postup, pokud soubor se vytvoří nebo aktualizuje v rámci OneDrive.
* Pomocí akce toocreate souboru, odstraňte soubor a další. Například pokud byl přijat nový Office 365 e-mail s přílohou (aktivační události) vytvořte nový soubor ve Onedrivu (akce).

Toto téma ukazuje, jak toouse hello konektoru aplikace logiky na OneDrive a také seznamy hello triggery a akce.

toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooonedrive"></a>Připojit tooOneDrive
Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* toohello služby. Připojení poskytuje připojení mezi aplikace logiky a jiné služby. Například tooconnect tooOneDrive, musíte nejdřív OneDrive *připojení*. toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello službu, kterou chcete tooconnect k. Ano OneDrive, zadejte hello pověření tooyour OneDrive účet toocreate hello připojení.

### <a name="create-hello-connection"></a>Vytvoření připojení hello
> [!INCLUDE [Steps toocreate a connection tooOneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Použít aktivační událost
Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky. Aktivační události "dotazování" hello služby intervalu a frekvenci, který chcete. [Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. V aplikaci logiky hello zadejte "onedrive" tooget seznam hello aktivační události:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Vyberte **změny souboru**. Pokud připojení již existuje, pak vyberte hello zobrazit výběr tlačítko tooselect složku.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Pokud jste výzvami toosign v, zadejte přihlašovací hello podrobnosti toocreate hello připojení. [Vytvoření připojení hello](connectors-create-api-onedrive.md#create-the-connection) v tomto tématu jsou uvedeny kroky hello. 
   
   > [!NOTE]
   > V tomto příkladu aplikace logiky hello spustí při otevření souboru ve složce hello, který zvolíte, se aktualizuje. toosee hello výsledky této aktivační události, přidejte další akci, která vám pošle e-mailu. Například přidejte hello Office 365 Outlook *e-mailovou zprávu* akci, která odešle e-mail, když se aktualizuje soubor. 

3. Vyberte hello **upravit** tlačítko a nastavte hello **frekvence** a **Interval** hodnoty. Například pokud chcete hello aktivační událost toopoll každých 15 minut, pak nastavte hello **frekvence** příliš**minutu**a sadu hello **Interval** příliš**15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Uložit** změny (levém horním rohu hello panelu nástrojů). Aplikace logiky je uloženo a automaticky povolí.

## <a name="use-an-action"></a>Použít akci.
Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky. [Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Vyberte znaménko plus hello. Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z hello **Další** možnosti.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Zvolte **přidat akci**.
3. Hello textového pole zadejte "onedrive" tooget seznam všechny dostupné akce hello.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. V našem příkladu zvolte **OneDrive – vytvoření souboru**. Pokud připojení již existuje, pak vyberte hello **cesta ke složce** tooput hello souboru, zadejte hello **název souboru**a zvolte hello **obsah souboru** chcete:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Pokud budete vyzváni k hello informace o připojení, zadejte hello podrobnosti toocreate hello připojení. [Vytvoření připojení hello](connectors-create-api-onedrive.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti. 
   
   > [!NOTE]
   > V tomto příkladu vytvoříme nový soubor ve složce OneDrive. Můžete použít výstup z jiného aktivační událost toocreate hello OneDrive souboru. Například přidejte hello Office 365 Outlook *při doručení nových e-mailů* aktivační události. Pak přidejte hello OneDrive *vytvořit soubor* akci, která používá hello příloh a pole Content-Type v rámci příkazu ForEach toocreate hello nový soubor ve Onedrivu. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. **Uložit** změny (levém horním rohu hello panelu nástrojů). Aplikace logiky je uloženo a automaticky povolí.


## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/onedriveconnector/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).
