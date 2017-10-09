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
# <a name="connect-tooan-ibm-mq-server-from-logic-apps-using-hello-mq-connector"></a><span data-ttu-id="41f2d-103">Připojit tooan IBM MQ server z aplikace logic apps pomocí konektoru MQ hello</span><span class="sxs-lookup"><span data-stu-id="41f2d-103">Connect tooan IBM MQ server from logic apps using hello MQ connector</span></span> 

<span data-ttu-id="41f2d-104">Microsoft Connector pro MQ odešle a načítá zprávy o uložená v MQ Server místní nebo v Azure.</span><span class="sxs-lookup"><span data-stu-id="41f2d-104">Microsoft Connector for MQ sends and retrieves messages stored in an MQ Server on-premises, or in Azure.</span></span> <span data-ttu-id="41f2d-105">Tento konektor zahrnuje Microsoft MQ klienta, který komunikuje se vzdáleným serverem IBM MQ přes síť TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="41f2d-105">This connector includes a Microsoft MQ client that communicates with a remote IBM MQ server across a TCP/IP network.</span></span> <span data-ttu-id="41f2d-106">Tento dokument je konektor úvodní příručka toouse hello MQ.</span><span class="sxs-lookup"><span data-stu-id="41f2d-106">This document is a starter guide toouse hello MQ connector.</span></span> <span data-ttu-id="41f2d-107">Doporučujeme začínat procházením do jedné zprávy ve frontě, a potom zkusit hello dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="41f2d-107">We recommended you begin by browsing a single message on a queue, and then trying hello other actions.</span></span>    

<span data-ttu-id="41f2d-108">konektor MQ Hello zahrnuje hello následující akce.</span><span class="sxs-lookup"><span data-stu-id="41f2d-108">hello MQ connector includes hello following actions.</span></span> <span data-ttu-id="41f2d-109">Neexistují žádné aktivační události.</span><span class="sxs-lookup"><span data-stu-id="41f2d-109">There are no triggers.</span></span>

-   <span data-ttu-id="41f2d-110">Přejděte do jedné zprávy bez odstranění uvítací zprávu z hello IBM MQ Server</span><span class="sxs-lookup"><span data-stu-id="41f2d-110">Browse a single message without deleting hello message from hello IBM MQ Server</span></span>
-   <span data-ttu-id="41f2d-111">Procházet dávku zpráv bez odstranění hello zprávy z hello IBM MQ Server</span><span class="sxs-lookup"><span data-stu-id="41f2d-111">Browse a batch of messages without deleting hello messages from hello IBM MQ Server</span></span>
-   <span data-ttu-id="41f2d-112">Přijímat do jedné zprávy a odstranit uvítací zprávu z hello IBM MQ Server</span><span class="sxs-lookup"><span data-stu-id="41f2d-112">Receive a single message and delete hello message from hello IBM MQ Server</span></span>
-   <span data-ttu-id="41f2d-113">Přijímat dávku zpráv a odstranit hello zpráv z hello IBM MQ Server</span><span class="sxs-lookup"><span data-stu-id="41f2d-113">Receive a batch of messages and delete hello messages from hello IBM MQ Server</span></span>
-   <span data-ttu-id="41f2d-114">Odeslat jedné zprávy toohello IBM MQ Server</span><span class="sxs-lookup"><span data-stu-id="41f2d-114">Send a single message toohello IBM MQ Server</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="41f2d-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="41f2d-115">Prerequisites</span></span>

* <span data-ttu-id="41f2d-116">Pokud používáte MQ místnímu serveru [nainstalovat bránu dat místní hello](../logic-apps/logic-apps-gateway-install.md) na serveru v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="41f2d-116">If using an on-premises MQ server, [install hello on-premises data gateway](../logic-apps/logic-apps-gateway-install.md) on a server within your network.</span></span> <span data-ttu-id="41f2d-117">Pokud hello MQ Server veřejně, nebo dostupné v rámci Azure, není brána dat hello používá nebo vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="41f2d-117">If hello MQ Server is publicly available, or available within Azure, then hello data gateway is not used or required.</span></span>

    > [!NOTE]
    > <span data-ttu-id="41f2d-118">server Hello kde hello místní Data brána je nainstalovaná musí také mít rozhraní .net Framework 4.6 pro konektor MQ toofunction hello nainstalována.</span><span class="sxs-lookup"><span data-stu-id="41f2d-118">hello server where hello On-Premises Data Gateway is installed must also have .Net Framework 4.6 installed for hello MQ Connector toofunction.</span></span>

* <span data-ttu-id="41f2d-119">Vytvoření hello prostředků Azure pro bránu dat místní hello - [nastavit připojení k bráně data hello](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="41f2d-119">Create hello Azure resource for hello on-premises data gateway - [Set up hello data gateway connection](../logic-apps/logic-apps-gateway-connection.md).</span></span>

* <span data-ttu-id="41f2d-120">Oficiálně podporované IBM WebSphere MQ verze:</span><span class="sxs-lookup"><span data-stu-id="41f2d-120">Officially supported IBM WebSphere MQ versions:</span></span>
   * <span data-ttu-id="41f2d-121">MQ 7.5</span><span class="sxs-lookup"><span data-stu-id="41f2d-121">MQ 7.5</span></span>
   * <span data-ttu-id="41f2d-122">MQ 8.0</span><span class="sxs-lookup"><span data-stu-id="41f2d-122">MQ 8.0</span></span>

## <a name="create-a-logic-app"></a><span data-ttu-id="41f2d-123">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="41f2d-123">Create a logic app</span></span>

1. <span data-ttu-id="41f2d-124">V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="41f2d-124">In hello **Azure start board**, select **+** (plus sign), **Web + Mobile**, and then **Logic App**.</span></span> 
2. <span data-ttu-id="41f2d-125">Zadejte hello **název**, jako je například MQTestApp, **předplatné**, **skupiny prostředků**, a **umístění** (použijte umístění hello kde hello místní brána dat je nakonfigurováno připojení).</span><span class="sxs-lookup"><span data-stu-id="41f2d-125">Enter hello **Name**, such as MQTestApp, **Subscription**, **Resource group**, and **Location** (use hello location where hello on-premises Data Gateway connection is configured).</span></span> <span data-ttu-id="41f2d-126">Vyberte **Pin toodashboard**a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="41f2d-126">Select **Pin toodashboard**, and select **Create**.</span></span>  
<span data-ttu-id="41f2d-127">![Vytvoření aplikace logiky](media/connectors-create-api-mq/Create_Logic_App.png)</span><span class="sxs-lookup"><span data-stu-id="41f2d-127">![Create Logic App](media/connectors-create-api-mq/Create_Logic_App.png)</span></span>

## <a name="add-a-trigger"></a><span data-ttu-id="41f2d-128">Přidat aktivační událost</span><span class="sxs-lookup"><span data-stu-id="41f2d-128">Add a trigger</span></span>

> [!NOTE]
> <span data-ttu-id="41f2d-129">Hello MQ konektor nemá žádné aktivační události.</span><span class="sxs-lookup"><span data-stu-id="41f2d-129">hello MQ Connector does not have any triggers.</span></span> <span data-ttu-id="41f2d-130">Ano, použít jiný toostart aktivační událost svou aplikaci logiky, jako je například hello **opakování** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="41f2d-130">So, use another trigger toostart your logic app, such as hello **Recurrence** trigger.</span></span> 

1. <span data-ttu-id="41f2d-131">Hello **logiku aplikace Návrhář** otevře, vyberte **opakování** v hello seznam běžných aktivační události.</span><span class="sxs-lookup"><span data-stu-id="41f2d-131">hello **Logic Apps Designer** opens, select **Recurrence** in hello list of common triggers.</span></span>
2. <span data-ttu-id="41f2d-132">Vyberte **upravit** v rámci hello opakování aktivační události.</span><span class="sxs-lookup"><span data-stu-id="41f2d-132">Select **Edit** within hello Recurrence Trigger.</span></span> 
3. <span data-ttu-id="41f2d-133">Sada hello **frekvence** příliš**den**a sadu hello **Interval** příliš**7**.</span><span class="sxs-lookup"><span data-stu-id="41f2d-133">Set hello **Frequency** too**Day**, and set hello **Interval** too**7**.</span></span> 

## <a name="browse-a-single-message"></a><span data-ttu-id="41f2d-134">Přejděte do jedné zprávy</span><span class="sxs-lookup"><span data-stu-id="41f2d-134">Browse a single message</span></span>
1. <span data-ttu-id="41f2d-135">Vyberte **+ nový krok**a vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="41f2d-135">Select **+ New step**, and select **Add an action**.</span></span>
2. <span data-ttu-id="41f2d-136">Hello vyhledávacího pole zadejte `mq`a potom vyberte **MQ - procházet zpráva**.</span><span class="sxs-lookup"><span data-stu-id="41f2d-136">In hello search box, type `mq`, and then select **MQ - Browse message**.</span></span>  
<span data-ttu-id="41f2d-137">![Procházet zpráv](media/connectors-create-api-mq/Browse_message.png)</span><span class="sxs-lookup"><span data-stu-id="41f2d-137">![Browse message](media/connectors-create-api-mq/Browse_message.png)</span></span>

3. <span data-ttu-id="41f2d-138">Pokud není k dispozici připojení k existující MQ, vytvořte hello připojení:</span><span class="sxs-lookup"><span data-stu-id="41f2d-138">If there isn't an existing MQ connection, then create hello connection:</span></span>  

    1. <span data-ttu-id="41f2d-139">Vyberte **připojit prostřednictvím místní brána dat**a zadejte vlastnosti hello serveru MQ.</span><span class="sxs-lookup"><span data-stu-id="41f2d-139">Select **Connect via on-premises data gateway**, and enter hello properties of your MQ server.</span></span>  
    <span data-ttu-id="41f2d-140">Pro **Server**, můžete zadat název serveru MQ hello nebo zadejte IP adresu hello následovaný dvojtečkou a hello číslo portu.</span><span class="sxs-lookup"><span data-stu-id="41f2d-140">For **Server**, you can enter hello MQ server name, or enter hello IP address followed by a colon and hello port number.</span></span> 
    2. <span data-ttu-id="41f2d-141">Hello **brány** rozevírací seznam obsahuje seznam všech existujících připojení brány, které byly nakonfigurovány.</span><span class="sxs-lookup"><span data-stu-id="41f2d-141">hello **gateway** dropdown lists any existing gateway connections that have been configured.</span></span> <span data-ttu-id="41f2d-142">Vyberte bránu.</span><span class="sxs-lookup"><span data-stu-id="41f2d-142">Select your gateway.</span></span>
    3. <span data-ttu-id="41f2d-143">Vyberte **vytvořit** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="41f2d-143">Select **Create** when finished.</span></span> <span data-ttu-id="41f2d-144">Připojení vypadá podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="41f2d-144">Your connection looks similar toohello following:</span></span>   
    <span data-ttu-id="41f2d-145">![Vlastnosti připojení](media/connectors-create-api-mq/Connection_Properties.png)</span><span class="sxs-lookup"><span data-stu-id="41f2d-145">![Connection Properties](media/connectors-create-api-mq/Connection_Properties.png)</span></span>

4. <span data-ttu-id="41f2d-146">Ve vlastnostech hello akce můžete:</span><span class="sxs-lookup"><span data-stu-id="41f2d-146">In hello action properties, you can:</span></span>  

    * <span data-ttu-id="41f2d-147">Použití hello **fronty** tooaccess vlastnost název fronty jiný než co je definována v hello připojení</span><span class="sxs-lookup"><span data-stu-id="41f2d-147">Use hello **Queue** property tooaccess a different queue name than what is defined in hello connection</span></span>
    * <span data-ttu-id="41f2d-148">Použití hello **MessageId**, **CorrelationId**, **GroupId**a další vlastnosti toobrowse zprávy podle vlastnosti zprávy MQ různých hello</span><span class="sxs-lookup"><span data-stu-id="41f2d-148">Use hello **MessageId**, **CorrelationId**, **GroupId**, and other properties toobrowse for a message based on hello different MQ message properties</span></span>
    * <span data-ttu-id="41f2d-149">Nastavit **IncludeInfo** příliš**True** informace o dalších zpráv tooinclude ve výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="41f2d-149">Set **IncludeInfo** too**True** tooinclude additional message information in hello output.</span></span> <span data-ttu-id="41f2d-150">Nebo ji nastavte příliš**False** toonot zahrnují informace o dalších zpráv ve výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="41f2d-150">Or, set it too**False** toonot include additional message information in hello output.</span></span>
    * <span data-ttu-id="41f2d-151">Zadejte **časový limit** hodnotu toodetermine jak dlouho toowait pro zprávu tooarrive v prázdné frontě.</span><span class="sxs-lookup"><span data-stu-id="41f2d-151">Enter a **Timeout** value toodetermine how long toowait for a message tooarrive in an empty queue.</span></span> <span data-ttu-id="41f2d-152">Pokud je zadán nic, jsou načtena hello první zprávu ve frontě hello a neexistuje žádný doby čekání na tooappear zprávy.</span><span class="sxs-lookup"><span data-stu-id="41f2d-152">If nothing is entered, hello first message in hello queue is retrieved, and there is no time spent waiting for a message tooappear.</span></span>  
    <span data-ttu-id="41f2d-153">![Procházet vlastnosti zprávy](media/connectors-create-api-mq/Browse_message_Props.png)</span><span class="sxs-lookup"><span data-stu-id="41f2d-153">![Browse Message Properties](media/connectors-create-api-mq/Browse_message_Props.png)</span></span>

5. <span data-ttu-id="41f2d-154">**Uložit** změny a potom **spustit** svou aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="41f2d-154">**Save** your changes, and then **Run** your logic app:</span></span>  
<span data-ttu-id="41f2d-155">![Uložte a spusťte](media/connectors-create-api-mq/Save_Run.png)</span><span class="sxs-lookup"><span data-stu-id="41f2d-155">![Save and run](media/connectors-create-api-mq/Save_Run.png)</span></span>

6. <span data-ttu-id="41f2d-156">Za několik sekund jsou uvedeny kroky hello hello spustit, a můžete se podívat na výstup hello.</span><span class="sxs-lookup"><span data-stu-id="41f2d-156">After a few seconds, hello steps of hello run are shown, and you can look at hello output.</span></span> <span data-ttu-id="41f2d-157">Vyberte podrobnosti toosee hello zeleného zaškrtnutí jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="41f2d-157">Select hello green checkmark toosee details of each step.</span></span> <span data-ttu-id="41f2d-158">Vyberte **najdete v části nezpracovaná výstupy** toosee další podrobnosti o hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="41f2d-158">Select **See raw outputs** toosee additional details on hello output data.</span></span>  
<span data-ttu-id="41f2d-159">![Procházet výstup zpráv](media/connectors-create-api-mq/Browse_message_output.png)</span><span class="sxs-lookup"><span data-stu-id="41f2d-159">![Browse message output](media/connectors-create-api-mq/Browse_message_output.png)</span></span>  

    <span data-ttu-id="41f2d-160">Nezpracovaná výstup:</span><span class="sxs-lookup"><span data-stu-id="41f2d-160">Raw output:</span></span>  
    ![Procházet nezpracovaná výstup zpráv](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. <span data-ttu-id="41f2d-162">Když hello **IncludeInfo** je možnost nastavena tootrue, zobrazí se následující výstup hello:</span><span class="sxs-lookup"><span data-stu-id="41f2d-162">When hello **IncludeInfo** option is set tootrue, hello following output is displayed:</span></span>  
<span data-ttu-id="41f2d-163">![Procházet zpráva obsahovat informace o](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span><span class="sxs-lookup"><span data-stu-id="41f2d-163">![Browse message include info](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span></span>

## <a name="browse-multiple-messages"></a><span data-ttu-id="41f2d-164">Procházet více zpráv</span><span class="sxs-lookup"><span data-stu-id="41f2d-164">Browse multiple messages</span></span>
<span data-ttu-id="41f2d-165">Hello **procházet zprávy** zahrnuje akce **BatchSize** možnost tooindicate má být vrácen počet zpráv z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="41f2d-165">hello **Browse messages** action includes a **BatchSize** option tooindicate how many messages should be returned from hello queue.</span></span>  <span data-ttu-id="41f2d-166">Pokud **BatchSize** nemá žádný záznam, jsou vráceny všechny zprávy.</span><span class="sxs-lookup"><span data-stu-id="41f2d-166">If **BatchSize** has no entry, all messages are returned.</span></span> <span data-ttu-id="41f2d-167">Hello vrátit výstupem je pole zpráv.</span><span class="sxs-lookup"><span data-stu-id="41f2d-167">hello returned output is an array of messages.</span></span>

1. <span data-ttu-id="41f2d-168">Při přidávání hello **procházet zprávy** je standardně vybraná akce, hello prvního připojení, který je nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="41f2d-168">When adding hello **Browse messages** action, hello first connection that is configured is selected by default.</span></span> <span data-ttu-id="41f2d-169">Vyberte **změnit připojení** toocreate nové připojení, nebo vyberte jiné připojení.</span><span class="sxs-lookup"><span data-stu-id="41f2d-169">Select **Change connection** toocreate a new connection, or select a different connection.</span></span>

2. <span data-ttu-id="41f2d-170">výstup Hello procházet zprávy zobrazí:</span><span class="sxs-lookup"><span data-stu-id="41f2d-170">hello output of Browse messages shows:</span></span>  
![Procházet výstupní zprávy](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a><span data-ttu-id="41f2d-172">Přijímat do jedné zprávy</span><span class="sxs-lookup"><span data-stu-id="41f2d-172">Receive a single message</span></span>
<span data-ttu-id="41f2d-173">Hello **příjmu zpráv** akce má hello stejné vstupy a výstupy jako hello **zpráva Procházet** akce.</span><span class="sxs-lookup"><span data-stu-id="41f2d-173">hello **Receive message** action has hello same inputs and outputs as hello **Browse message** action.</span></span> <span data-ttu-id="41f2d-174">Při použití **příjmu zpráv**, odstraní hello zprávu z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="41f2d-174">When using **Receive message**, hello message is deleted from hello queue.</span></span>

## <a name="receive-multiple-messages"></a><span data-ttu-id="41f2d-175">Zobrazí více zpráv</span><span class="sxs-lookup"><span data-stu-id="41f2d-175">Receive multiple messages</span></span>
<span data-ttu-id="41f2d-176">Hello **přijímat zprávy** akce má hello stejné vstupy a výstupy jako hello **procházet zprávy** akce.</span><span class="sxs-lookup"><span data-stu-id="41f2d-176">hello **Receive messages** action has hello same inputs and outputs as hello **Browse messages** action.</span></span> <span data-ttu-id="41f2d-177">Při použití **přijímat zprávy**, jsou odstraněny hello zprávy z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="41f2d-177">When using **Receive messages**, hello messages are deleted from hello queue.</span></span>

<span data-ttu-id="41f2d-178">Pokud nejsou žádné zprávy ve frontě hello, když procházet nebo příjmu, krok hello selže s touto hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="41f2d-178">If there are no messages in hello queue when doing a browse or a receive, hello step fails with hello following output:</span></span>  
![Ne MQ zprávy chyby](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a><span data-ttu-id="41f2d-180">Odeslat zprávu</span><span class="sxs-lookup"><span data-stu-id="41f2d-180">Send a message</span></span>
1. <span data-ttu-id="41f2d-181">Při přidávání hello **odesílání zpráv** ve výchozím nastavení je vybrané akce, hello prvního připojení, který je nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="41f2d-181">When adding hello **Send message** action, hello first connection that is configured is selected by default.</span></span> <span data-ttu-id="41f2d-182">Vyberte **změnit připojení** toocreate nové připojení, nebo vyberte jiné připojení.</span><span class="sxs-lookup"><span data-stu-id="41f2d-182">Select **Change connection** toocreate a new connection, or select a different connection.</span></span> <span data-ttu-id="41f2d-183">Hello platný **typy zpráv** jsou **Datagram**, **odpověď**, nebo **požadavku**.</span><span class="sxs-lookup"><span data-stu-id="41f2d-183">hello valid **Message Types** are **Datagram**, **Reply**, or **Request**.</span></span>  
<span data-ttu-id="41f2d-184">![Odeslat Msg Props](media/connectors-create-api-mq/Send_Msg_Props.png)</span><span class="sxs-lookup"><span data-stu-id="41f2d-184">![Send Msg Props](media/connectors-create-api-mq/Send_Msg_Props.png)</span></span>

2. <span data-ttu-id="41f2d-185">Hello výstup odeslat zpráva vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="41f2d-185">hello output of Send message looks like hello following:</span></span>  
![Odeslání výstupu Msg](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a><span data-ttu-id="41f2d-187">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="41f2d-187">Connector-specific details</span></span>

<span data-ttu-id="41f2d-188">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/mq/).</span><span class="sxs-lookup"><span data-stu-id="41f2d-188">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/mq/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="41f2d-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41f2d-189">Next steps</span></span>
<span data-ttu-id="41f2d-190">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="41f2d-190">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="41f2d-191">Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="41f2d-191">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
