---
title: aaaLearn jak toouse hello MQ konektor v Azure Logic Apps | Microsoft Docs
description: "Připojit tooan na pracovišti nebo v Azure MQ server z vaší toobrowse pracovní postup aplikace logiky, příjem a odesílání zpráv tooWebSphere MQ"
services: logic-apps
author: valthom
manager: anneta
documentationcenter: 
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 06/01/2017
ms.author: valthom; ladocs
ms.openlocfilehash: 8b36d53b457ced1a7461c229aecfcf8e4ae668a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-ibm-mq-server-from-logic-apps-using-hello-mq-connector"></a>Připojit tooan IBM MQ server z aplikace logic apps pomocí konektoru MQ hello 

Microsoft Connector pro MQ odešle a načítá zprávy o uložená v MQ Server místní nebo v Azure. Tento konektor zahrnuje Microsoft MQ klienta, který komunikuje se vzdáleným serverem IBM MQ přes síť TCP/IP. Tento dokument je konektor úvodní příručka toouse hello MQ. Doporučujeme začínat procházením do jedné zprávy ve frontě, a potom zkusit hello dalších akcí.    

konektor MQ Hello zahrnuje hello následující akce. Neexistují žádné aktivační události.

-   Přejděte do jedné zprávy bez odstranění uvítací zprávu z hello IBM MQ Server
-   Procházet dávku zpráv bez odstranění hello zprávy z hello IBM MQ Server
-   Přijímat do jedné zprávy a odstranit uvítací zprávu z hello IBM MQ Server
-   Přijímat dávku zpráv a odstranit hello zpráv z hello IBM MQ Server
-   Odeslat jedné zprávy toohello IBM MQ Server 

## <a name="prerequisites"></a>Požadavky

* Pokud používáte MQ místnímu serveru [nainstalovat bránu dat místní hello](../logic-apps/logic-apps-gateway-install.md) na serveru v rámci vaší sítě. Pokud hello MQ Server veřejně, nebo dostupné v rámci Azure, není brána dat hello používá nebo vyžaduje.

    > [!NOTE]
    > server Hello kde hello místní Data brána je nainstalovaná musí také mít rozhraní .net Framework 4.6 pro konektor MQ toofunction hello nainstalována.

* Vytvoření hello prostředků Azure pro bránu dat místní hello - [nastavit připojení k bráně data hello](../logic-apps/logic-apps-gateway-connection.md).

* Oficiálně podporované IBM WebSphere MQ verze:
   * MQ 7.5
   * MQ 8.0

## <a name="create-a-logic-app"></a>Vytvoření aplikace logiky

1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**. 
2. Zadejte hello **název**, jako je například MQTestApp, **předplatné**, **skupiny prostředků**, a **umístění** (použijte umístění hello kde hello místní brána dat je nakonfigurováno připojení). Vyberte **Pin toodashboard**a vyberte **vytvořit**.  
![Vytvoření aplikace logiky](media/connectors-create-api-mq/Create_Logic_App.png)

## <a name="add-a-trigger"></a>Přidat aktivační událost

> [!NOTE]
> Hello MQ konektor nemá žádné aktivační události. Ano, použít jiný toostart aktivační událost svou aplikaci logiky, jako je například hello **opakování** aktivační události. 

1. Hello **logiku aplikace Návrhář** otevře, vyberte **opakování** v hello seznam běžných aktivační události.
2. Vyberte **upravit** v rámci hello opakování aktivační události. 
3. Sada hello **frekvence** příliš**den**a sadu hello **Interval** příliš**7**. 

## <a name="browse-a-single-message"></a>Přejděte do jedné zprávy
1. Vyberte **+ nový krok**a vyberte **přidat akci**.
2. Hello vyhledávacího pole zadejte `mq`a potom vyberte **MQ - procházet zpráva**.  
![Procházet zpráv](media/connectors-create-api-mq/Browse_message.png)

3. Pokud není k dispozici připojení k existující MQ, vytvořte hello připojení:  

    1. Vyberte **připojit prostřednictvím místní brána dat**a zadejte vlastnosti hello serveru MQ.  
    Pro **Server**, můžete zadat název serveru MQ hello nebo zadejte IP adresu hello následovaný dvojtečkou a hello číslo portu. 
    2. Hello **brány** rozevírací seznam obsahuje seznam všech existujících připojení brány, které byly nakonfigurovány. Vyberte bránu.
    3. Vyberte **vytvořit** po dokončení. Připojení vypadá podobně jako toohello následující:   
    ![Vlastnosti připojení](media/connectors-create-api-mq/Connection_Properties.png)

4. Ve vlastnostech hello akce můžete:  

    * Použití hello **fronty** tooaccess vlastnost název fronty jiný než co je definována v hello připojení
    * Použití hello **MessageId**, **CorrelationId**, **GroupId**a další vlastnosti toobrowse zprávy podle vlastnosti zprávy MQ různých hello
    * Nastavit **IncludeInfo** příliš**True** informace o dalších zpráv tooinclude ve výstupu hello. Nebo ji nastavte příliš**False** toonot zahrnují informace o dalších zpráv ve výstupu hello.
    * Zadejte **časový limit** hodnotu toodetermine jak dlouho toowait pro zprávu tooarrive v prázdné frontě. Pokud je zadán nic, jsou načtena hello první zprávu ve frontě hello a neexistuje žádný doby čekání na tooappear zprávy.  
    ![Procházet vlastnosti zprávy](media/connectors-create-api-mq/Browse_message_Props.png)

5. **Uložit** změny a potom **spustit** svou aplikaci logiky:  
![Uložte a spusťte](media/connectors-create-api-mq/Save_Run.png)

6. Za několik sekund jsou uvedeny kroky hello hello spustit, a můžete se podívat na výstup hello. Vyberte podrobnosti toosee hello zeleného zaškrtnutí jednotlivých kroků. Vyberte **najdete v části nezpracovaná výstupy** toosee další podrobnosti o hello výstupní data.  
![Procházet výstup zpráv](media/connectors-create-api-mq/Browse_message_output.png)  

    Nezpracovaná výstup:  
    ![Procházet nezpracovaná výstup zpráv](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. Když hello **IncludeInfo** je možnost nastavena tootrue, zobrazí se následující výstup hello:  
![Procházet zpráva obsahovat informace o](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Procházet více zpráv
Hello **procházet zprávy** zahrnuje akce **BatchSize** možnost tooindicate má být vrácen počet zpráv z fronty hello.  Pokud **BatchSize** nemá žádný záznam, jsou vráceny všechny zprávy. Hello vrátit výstupem je pole zpráv.

1. Při přidávání hello **procházet zprávy** je standardně vybraná akce, hello prvního připojení, který je nakonfigurovaný. Vyberte **změnit připojení** toocreate nové připojení, nebo vyberte jiné připojení.

2. výstup Hello procházet zprávy zobrazí:  
![Procházet výstupní zprávy](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a>Přijímat do jedné zprávy
Hello **příjmu zpráv** akce má hello stejné vstupy a výstupy jako hello **zpráva Procházet** akce. Při použití **příjmu zpráv**, odstraní hello zprávu z fronty hello.

## <a name="receive-multiple-messages"></a>Zobrazí více zpráv
Hello **přijímat zprávy** akce má hello stejné vstupy a výstupy jako hello **procházet zprávy** akce. Při použití **přijímat zprávy**, jsou odstraněny hello zprávy z fronty hello.

Pokud nejsou žádné zprávy ve frontě hello, když procházet nebo příjmu, krok hello selže s touto hello následující výstup:  
![Ne MQ zprávy chyby](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a>Odeslat zprávu
1. Při přidávání hello **odesílání zpráv** ve výchozím nastavení je vybrané akce, hello prvního připojení, který je nakonfigurovaný. Vyberte **změnit připojení** toocreate nové připojení, nebo vyberte jiné připojení. Hello platný **typy zpráv** jsou **Datagram**, **odpověď**, nebo **požadavku**.  
![Odeslat Msg Props](media/connectors-create-api-mq/Send_Msg_Props.png)

2. Hello výstup odeslat zpráva vypadá hello následující:  
![Odeslání výstupu Msg](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/mq/).

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).
