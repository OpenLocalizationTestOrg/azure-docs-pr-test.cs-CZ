---
title: "aaaSet až monitorování událostí s Azure Event Hubs pro Azure Logic Apps | Microsoft Docs"
description: "Sledování událostí tooreceive datových proudů dat a odesílání událostí pro Azure Logic Apps s Azure Event Hubs"
services: logic-apps
keywords: "datový proud, monitorování událostí, služba event hubs"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.openlocfilehash: 4aad2c2ac1134b4d4d440019b4773559e49be122
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-receive-and-send-events-with-hello-event-hubs-connector"></a>Monitorování, příjem a odesílání událostí s konektorem služby Event Hubs hello

tooset nahoru monitorování událostí tak, aby aplikace logiky můžete zjišťovat události, přijímat události a odesílat události, připojit tooan [centra událostí Azure](https://azure.microsoft.com/services/event-hubs) z aplikace logiky. Další informace o [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

## <a name="requirements"></a>Požadavky

* Máte toohave [Event Hubs obor názvů a centra událostí](../event-hubs/event-hubs-create.md) v Azure. Další informace [jak toocreate služby Event Hubs obor názvů a centra událostí](../event-hubs/event-hubs-create.md). 

* toouse [všechny konektory](https://docs.microsoft.com/azure/connectors/apis-list) v aplikaci logiky máte toocreate aplikace logiky poprvé. Další informace [jak toocreate aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-hello-connection-string"></a>Zkontrolujte oprávnění k oboru názvů služby Event Hubs a najít hello připojovací řetězec

Pro vaše aplikace tooaccess logiku žádné služby, máte toocreate [ *připojení* ](./connectors-overview.md) mezi logiku aplikace a hello služby, pokud jste tak ještě neučinili. Toto připojení autorizuje data tooaccess logiku aplikace.
Vaše tooaccess aplikace logiky Centrum událostí, je nutné toohave **spravovat** oprávnění a hello připojovací řetězec pro obor názvů služby Event Hubs.

toocheck oprávnění a get hello připojovací řetězec, postupujte podle těchto kroků.

1.  Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure"). 

2.  Přejděte tooyour Event Hubs *obor názvů*, není hello konkrétní Centrum událostí. V okně hello obor názvů v části **nastavení**, zvolte **zásady sdíleného přístupu**. V části **deklarace identity**, zkontrolujte, zda máte **spravovat** oprávnění pro tento obor názvů.

    ![Správa oprávnění pro obor názvů centra událostí](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  Zvolte toocopy hello připojovací řetězec pro obor názvů hello Event Hubs, **RootManageSharedAccessKey**. Další tooyour primární klíče připojovací řetězec, zvolte tlačítko Kopírovat hello.

    ![Zkopírujte Event Hubs obor názvů připojovacího řetězce](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > tooconfirm, zda je připojovací řetězec přidružené vašeho oboru názvů služby Event Hubs nebo s konkrétní Centrum událostí, zkontrolujte hello připojovací řetězec pro hello `EntityPath` parametr. Pokud zjistíte, tento parametr, hello připojovací řetězec je pro konkrétní Centrum událostí "entita" a není správný řetězec toouse hello s svou aplikaci logiky.

4.  Nyní když se zobrazí výzva k zadání pověření po přidání služby Event Hubs aktivační události nebo akce tooyour logiku aplikace, můžete připojit tooyour Event Hubs oboru názvů. Zadejte název připojení, zadejte hello připojovací řetězec, který jste zkopírovali a zvolte **vytvořit**.

    ![Zadejte připojovací řetězec pro obor názvů služby Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Po vytvoření připojení, název připojení hello objevit v aktivační události Event Hubs hello nebo akce. 
    Poté můžete dál s hello další kroky v aplikaci logiky.

    ![Připojení obor názvů centra událostí vytvořit](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a>Spustit pracovní postup v případě, že Centrum událostí obdrží nové události

A [ *aktivační událost* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) je událost, která se spouští v aplikaci logiky pracovního postupu. Spuštění pracovního postupu odeslání nové události jsou tooyour centra událostí, postupujte podle těchto kroků pro přidání hello aktivační událost, která zjistí tuto událost.

1.  V hello [portál Azure](https://portal.azure.com "portál Azure"), přejděte existující aplikace logiky tooyour nebo vytvoření aplikace logiky prázdné.

2.  Hello vyhledávacího pole pro hello návrhář aplikace logiky, zadejte `event hubs` filtru. Vyberte této aktivační události: **když jsou události dostupné v Centru událostí**

    ![Vyberte aktivační událost pro Jakmile Centrum událostí obdrží nové události](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    Pokud ještě nemáte obor názvů služby Event Hubs tooyour připojení, se zobrazí výzva toocreate nyní toto připojení. Zadejte název připojení a zadejte hello připojovací řetězec pro obor názvů služby Event Hubs. 
    V případě potřeby další [jak toofind připojovací řetězec](#permissions-connection-string).

    ![Zadejte připojovací řetězec pro obor názvů služby Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Po vytvoření připojení hello hello nastavení pro hello **při událost v dostupné v Centru událostí** zobrazí aktivační události.

    ![Aktivační událost nastavení po Centrum událostí obdrží nové události](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  Zadejte nebo vyberte název hello hello centra událostí, které chcete toomonitor. Vyberte hello frekvence a intervalu pro požadovanou četnost toocheck hello centra událostí.

    > [!TIP]
    > toooptionally vyberte skupinu uživatelů pro čtení událostí, zvolte **zobrazit rozšířené možnosti**. 

    ![Zadejte centra událostí nebo skupiny uživatelů](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    Jste nyní nastavili toostart aktivační událost pracovního postupu pro svou aplikaci logiky. 
    Aplikace logiky jsou kontrolovány hello zadaný centra událostí podle nastaveného plánu hello. 
    Pokud vaše aplikace najde nové události v hello centra událostí, aktivační události hello běží další akce nebo aktivační události v aplikaci logiky.

## <a name="send-events-tooyour-event-hub-from-your-logic-app"></a>Odesílání událostí tooyour centra událostí z aplikace logiky

[*Akce*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) je úloha, kterou provádí pracovní postup vaší aplikace logiky. Po přidání aplikace logiky tooyour aktivační událost, můžete přidat akci tooperform operace s daty generovanými této aktivační události. toosend událostí tooyour centra událostí z vaší aplikace logiky, postupujte podle těchto kroků.

1.  V návrháři aplikace logiky, vyberte v části vaší aktivační událostí aplikace logiky **nový krok** > **přidat akci**.

    ![Pak zvolte "Nový krok", "Přidat akci."](./media/connectors-create-api-azure-event-hubs/add-action.png)

    Nyní můžete najít a vybrat tooperform akce. 
    I když můžete vybrat všechny akce, například chceme hello Event Hubs akce toosend události.

2.  Hello vyhledávacího pole zadejte `event hubs` filtru.
Vyberte tuto akci: **odesílání událostí**

    ![Vyberte "Event Hubs - odesílání událostí" akce](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  Zadejte podrobnosti hello požadované hello události, jako je například název hello hello centra událostí, kde chcete toosend hello událost. Zadejte další volitelné podrobnosti o hello události, jako je třeba obsah pro tuto událost.

    > [!TIP]
    > toooptionally zadejte oddílu centra událostí hello kde toosend hello událostí, zvolte **zobrazit rozšířené možnosti**. 

    ![Zadejte název centra událostí a podrobnosti volitelné události](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  Uložte provedené změny.

    ![Uložení aplikace logiky](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    Jste nyní nastavili události toosend akce z vaší aplikace logiky. 

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/eventhubs/). 

## <a name="get-help"></a>Podpora

tooask otázky, odpovědi na dotazy a zjistit, jaké jiných uživatelů Azure Logic Apps dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp zlepšit konektory a aplikace logiky, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

*  [Najít další konektory pro Azure Logic apps](./apis-list.md)