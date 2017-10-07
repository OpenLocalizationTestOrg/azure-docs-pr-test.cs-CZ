---
title: "konektor Office 365 Outlook hello aaaAdd ve vašich Logic Apps | Microsoft Docs"
description: "Vytvoření aplikace logiky s Office 365 konektor tooenable interakci se službami Office 365. Příklad: vytváření, úpravy a aktualizaci kontakty a položky kalendáře."
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 86a573c9c54701de3d3f0500d19eaf545e0710ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-office-365-outlook-connector"></a>Začínáme s konektor Office 365 Outlook hello
konektor Office 365 Outlook Hello umožňuje interakci s aplikací Outlook v Office 365. Použít tento konektor toocreate, upravit a aktualizace kontakty a položky kalendáře a také získat, odeslání a odpovědi tooemail.

S aplikací Outlook Office 365 můžete:

* Vytvořte pracovní postup pomocí funkce e-mailu a kalendář hello v rámci služeb Office 365. 
* Použití aktivuje toostart pracovní postup, když dojde nové e-mailu, při aktualizaci položky kalendáře a další.
* Pomocí akce toosend e-mailu, vytvořte novou událost kalendáře a další. Například když je v Salesforce (aktivační událost) nový objekt, odeslat e-mailu tooyour Outlook Office 365 (akce). 

Toto téma ukazuje, jak toouse hello konektor Office 365 Outlook v aplikaci logiky, a také seznamy hello triggery a akce.

> [!NOTE]
> Tato verze článku hello vztahuje tooLogic aplikace obecné dostupnosti (GA).
> 
> 

toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toooffice-365"></a>Připojit tooOffice 365
Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* toohello služby. Připojení poskytuje připojení mezi aplikace logiky a jiné služby. Například tooconnect tooOffice 365 Outlook, musíte nejprve Office 365 *připojení*. toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello službu, kterou chcete tooconnect k. Proto s Office 365 Outlook, zadejte přihlašovací údaje tooyour hello připojení hello toocreate účet Office 365.

## <a name="create-hello-connection"></a>Vytvoření připojení hello
> [!INCLUDE [Steps toocreate a connection tooOffice 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Použít aktivační událost
Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky. Aktivační události "dotazování" hello služby intervalu a frekvenci, který chcete. [Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. V aplikaci logiky hello zadejte "office 365" tooget seznam hello aktivační události:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Vyberte **Office 365 Outlook - během spouštění brzy nadcházející události**. Pokud připojení již existuje, vyberte z rozevíracího seznamu hello kalendáře.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Pokud jste výzvami toosign v, zadejte přihlašovací hello podrobnosti toocreate hello připojení. [Vytvoření připojení hello](connectors-create-api-office365-outlook.md#create-the-connection) v tomto tématu jsou uvedeny kroky hello. 
   
   > [!NOTE]
   > V tomto příkladu se spouští aplikace logiky hello při aktualizaci události kalendáře. toosee hello výsledky této aktivační události, přidejte další akci, která vám pošle textovou zprávu. Například přidejte hello Twilio *odeslat zprávu* akci, která spouští texty případě hello kalendář události za 15 minut. 
   > 
   > 
3. Vyberte hello **upravit** tlačítko a nastavte hello **frekvence** a **Interval** hodnoty. Například pokud chcete hello aktivační událost toopoll každých 15 minut, pak nastavte hello **frekvence** příliš**minutu**a sadu hello **Interval** příliš**15**. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. **Uložit** změny (levém horním rohu hello panelu nástrojů). Aplikace logiky je uloženo a automaticky povolí.

## <a name="use-an-action"></a>Použít akci.
Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky. [Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Vyberte znaménko plus hello. Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z hello **Další** možnosti.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. Zvolte **přidat akci**.
3. Hello textového pole zadejte "office 365" tooget seznam všechny dostupné akce hello.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. V našem příkladu zvolte **Office 365 Outlook - vytvoření kontaktu**. Pokud připojení již existuje, pak zvolte hello **ID složku**, **křestní jméno**a další vlastnosti:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Pokud budete vyzváni k hello informace o připojení, zadejte hello podrobnosti toocreate hello připojení. [Vytvoření připojení hello](connectors-create-api-office365-outlook.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti. 
   
   > [!NOTE]
   > V tomto příkladu vytvoříme nový kontakt v aplikaci Outlook Office 365. Můžete použít výstup z jiného kontaktu hello toocreate aktivační události. Například přidejte hello SalesForce *když je vytvořen objekt* aktivační události. Pak přidejte hello Office 365 Outlook *vytvoření kontaktu* akci, která používá hello SalesForce polí toocreate hello nové nového kontaktu v Office 365. 
   > 
   > 
5. **Uložit** změny (levém horním rohu hello panelu nástrojů). Aplikace logiky je uloženo a automaticky povolí.

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/office365connector/). 

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).

