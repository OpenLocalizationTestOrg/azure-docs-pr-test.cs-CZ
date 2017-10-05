---
title: "Další informace o použití konektoru MQ v Azure Logic Apps | Microsoft Docs"
description: "Připojení k místní nebo Azure MQ server z vaší aplikace logiky pracovního postupu procházet, příjem a odesílání zpráv do WebSphere MQ"
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
ms.openlocfilehash: 17c651585b56dae186286f5d8c68c363ae9c524d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-to-an-ibm-mq-server-from-logic-apps-using-the-mq-connector"></a><span data-ttu-id="93e10-103">Připojit k serveru IBM MQ z aplikace logic apps pomocí konektoru MQ</span><span class="sxs-lookup"><span data-stu-id="93e10-103">Connect to an IBM MQ server from logic apps using the MQ connector</span></span> 

<span data-ttu-id="93e10-104">Microsoft Connector pro MQ odešle a načítá zprávy o uložená v MQ Server místní nebo v Azure.</span><span class="sxs-lookup"><span data-stu-id="93e10-104">Microsoft Connector for MQ sends and retrieves messages stored in an MQ Server on-premises, or in Azure.</span></span> <span data-ttu-id="93e10-105">Tento konektor zahrnuje Microsoft MQ klienta, který komunikuje se vzdáleným serverem IBM MQ přes síť TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="93e10-105">This connector includes a Microsoft MQ client that communicates with a remote IBM MQ server across a TCP/IP network.</span></span> <span data-ttu-id="93e10-106">Tento dokument je úvodní příručka k používání konektoru MQ.</span><span class="sxs-lookup"><span data-stu-id="93e10-106">This document is a starter guide to use the MQ connector.</span></span> <span data-ttu-id="93e10-107">Doporučujeme že začněte tím, že procházení do jedné zprávy ve frontě a potom zkusit dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="93e10-107">We recommended you begin by browsing a single message on a queue, and then trying the other actions.</span></span>    

<span data-ttu-id="93e10-108">Konektor MQ zahrnuje následující akce.</span><span class="sxs-lookup"><span data-stu-id="93e10-108">The MQ connector includes the following actions.</span></span> <span data-ttu-id="93e10-109">Neexistují žádné aktivační události.</span><span class="sxs-lookup"><span data-stu-id="93e10-109">There are no triggers.</span></span>

-   <span data-ttu-id="93e10-110">Přejděte do jedné zprávy bez odstranění zprávy ze serveru IBM MQ</span><span class="sxs-lookup"><span data-stu-id="93e10-110">Browse a single message without deleting the message from the IBM MQ Server</span></span>
-   <span data-ttu-id="93e10-111">Procházet dávku zpráv bez odstranění zprávy ze serveru IBM MQ</span><span class="sxs-lookup"><span data-stu-id="93e10-111">Browse a batch of messages without deleting the messages from the IBM MQ Server</span></span>
-   <span data-ttu-id="93e10-112">Přijme do jedné zprávy a odstraní zprávu ze serveru IBM MQ</span><span class="sxs-lookup"><span data-stu-id="93e10-112">Receive a single message and delete the message from the IBM MQ Server</span></span>
-   <span data-ttu-id="93e10-113">Přijetí dávku zpráv a odstranění zprávy ze serveru IBM MQ</span><span class="sxs-lookup"><span data-stu-id="93e10-113">Receive a batch of messages and delete the messages from the IBM MQ Server</span></span>
-   <span data-ttu-id="93e10-114">Odesílání do jedné zprávy na IBM MQ Server</span><span class="sxs-lookup"><span data-stu-id="93e10-114">Send a single message to the IBM MQ Server</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="93e10-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="93e10-115">Prerequisites</span></span>

* <span data-ttu-id="93e10-116">Pokud používáte MQ místnímu serveru [nainstalovat bránu dat místní](../logic-apps/logic-apps-gateway-install.md) na serveru v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="93e10-116">If using an on-premises MQ server, [install the on-premises data gateway](../logic-apps/logic-apps-gateway-install.md) on a server within your network.</span></span> <span data-ttu-id="93e10-117">Pokud je Server MQ veřejně, nebo dostupné v rámci Azure, není bránu dat použít nebo vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="93e10-117">If the MQ Server is publicly available, or available within Azure, then the data gateway is not used or required.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93e10-118">Serveru s nainstalovanou bránu dat On-Premises musí také mít rozhraní .net Framework 4.6 nainstalovaný konektor MQ funkce.</span><span class="sxs-lookup"><span data-stu-id="93e10-118">The server where the On-Premises Data Gateway is installed must also have .Net Framework 4.6 installed for the MQ Connector to function.</span></span>

* <span data-ttu-id="93e10-119">Vytvoření prostředku Azure pro bránu dat místní - [nastavení připojení brány dat](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="93e10-119">Create the Azure resource for the on-premises data gateway - [Set up the data gateway connection](../logic-apps/logic-apps-gateway-connection.md).</span></span>

* <span data-ttu-id="93e10-120">Oficiálně podporované IBM WebSphere MQ verze:</span><span class="sxs-lookup"><span data-stu-id="93e10-120">Officially supported IBM WebSphere MQ versions:</span></span>
   * <span data-ttu-id="93e10-121">MQ 7.5</span><span class="sxs-lookup"><span data-stu-id="93e10-121">MQ 7.5</span></span>
   * <span data-ttu-id="93e10-122">MQ 8.0</span><span class="sxs-lookup"><span data-stu-id="93e10-122">MQ 8.0</span></span>

## <a name="create-a-logic-app"></a><span data-ttu-id="93e10-123">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="93e10-123">Create a logic app</span></span>

1. <span data-ttu-id="93e10-124">V **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="93e10-124">In the **Azure start board**, select **+** (plus sign), **Web + Mobile**, and then **Logic App**.</span></span> 
2. <span data-ttu-id="93e10-125">Zadejte **název**, jako je například MQTestApp, **předplatné**, **skupiny prostředků**, a **umístění** (použijte umístění, kde je nakonfigurované připojení k místní brána Data Gateway).</span><span class="sxs-lookup"><span data-stu-id="93e10-125">Enter the **Name**, such as MQTestApp, **Subscription**, **Resource group**, and **Location** (use the location where the on-premises Data Gateway connection is configured).</span></span> <span data-ttu-id="93e10-126">Vyberte **připnout na řídicí panel**a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="93e10-126">Select **Pin to dashboard**, and select **Create**.</span></span>  
<span data-ttu-id="93e10-127">![Vytvoření aplikace logiky](media/connectors-create-api-mq/Create_Logic_App.png)</span><span class="sxs-lookup"><span data-stu-id="93e10-127">![Create Logic App](media/connectors-create-api-mq/Create_Logic_App.png)</span></span>

## <a name="add-a-trigger"></a><span data-ttu-id="93e10-128">Přidat aktivační událost</span><span class="sxs-lookup"><span data-stu-id="93e10-128">Add a trigger</span></span>

> [!NOTE]
> <span data-ttu-id="93e10-129">Konektor MQ nemá žádné aktivační události.</span><span class="sxs-lookup"><span data-stu-id="93e10-129">The MQ Connector does not have any triggers.</span></span> <span data-ttu-id="93e10-130">Ano, používat jiný aktivační událost spusťte aplikaci logiky, jako **opakování** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="93e10-130">So, use another trigger to start your logic app, such as the **Recurrence** trigger.</span></span> 

1. <span data-ttu-id="93e10-131">**Logiku aplikace Návrhář** otevře, vyberte **opakování** v seznamu běžných aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="93e10-131">The **Logic Apps Designer** opens, select **Recurrence** in the list of common triggers.</span></span>
2. <span data-ttu-id="93e10-132">Vyberte **upravit** v rámci opakování aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="93e10-132">Select **Edit** within the Recurrence Trigger.</span></span> 
3. <span data-ttu-id="93e10-133">Nastavte **frekvence** k **den**a nastavte **Interval** k **7**.</span><span class="sxs-lookup"><span data-stu-id="93e10-133">Set the **Frequency** to **Day**, and set the **Interval** to **7**.</span></span> 

## <a name="browse-a-single-message"></a><span data-ttu-id="93e10-134">Přejděte do jedné zprávy</span><span class="sxs-lookup"><span data-stu-id="93e10-134">Browse a single message</span></span>
1. <span data-ttu-id="93e10-135">Vyberte **+ nový krok**a vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="93e10-135">Select **+ New step**, and select **Add an action**.</span></span>
2. <span data-ttu-id="93e10-136">Do vyhledávacího pole zadejte `mq`a potom vyberte **MQ - procházet zpráva**.</span><span class="sxs-lookup"><span data-stu-id="93e10-136">In the search box, type `mq`, and then select **MQ - Browse message**.</span></span>  
<span data-ttu-id="93e10-137">![Procházet zpráv](media/connectors-create-api-mq/Browse_message.png)</span><span class="sxs-lookup"><span data-stu-id="93e10-137">![Browse message](media/connectors-create-api-mq/Browse_message.png)</span></span>

3. <span data-ttu-id="93e10-138">Pokud není k dispozici připojení k existující MQ, vytvořte připojení:</span><span class="sxs-lookup"><span data-stu-id="93e10-138">If there isn't an existing MQ connection, then create the connection:</span></span>  

    1. <span data-ttu-id="93e10-139">Vyberte **připojit prostřednictvím místní brána dat**a zadejte vlastnosti serveru MQ.</span><span class="sxs-lookup"><span data-stu-id="93e10-139">Select **Connect via on-premises data gateway**, and enter the properties of your MQ server.</span></span>  
    <span data-ttu-id="93e10-140">Pro **Server**, můžete zadat název serveru MQ nebo zadejte IP adresu následovaný dvojtečkou a číslo portu.</span><span class="sxs-lookup"><span data-stu-id="93e10-140">For **Server**, you can enter the MQ server name, or enter the IP address followed by a colon and the port number.</span></span> 
    2. <span data-ttu-id="93e10-141">**Brány** rozevírací seznam obsahuje seznam všech existujících připojení brány, které byly nakonfigurovány.</span><span class="sxs-lookup"><span data-stu-id="93e10-141">The **gateway** dropdown lists any existing gateway connections that have been configured.</span></span> <span data-ttu-id="93e10-142">Vyberte bránu.</span><span class="sxs-lookup"><span data-stu-id="93e10-142">Select your gateway.</span></span>
    3. <span data-ttu-id="93e10-143">Vyberte **vytvořit** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="93e10-143">Select **Create** when finished.</span></span> <span data-ttu-id="93e10-144">Připojení vypadá podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="93e10-144">Your connection looks similar to the following:</span></span>   
    <span data-ttu-id="93e10-145">![Vlastnosti připojení](media/connectors-create-api-mq/Connection_Properties.png)</span><span class="sxs-lookup"><span data-stu-id="93e10-145">![Connection Properties](media/connectors-create-api-mq/Connection_Properties.png)</span></span>

4. <span data-ttu-id="93e10-146">Ve vlastnostech akce můžete:</span><span class="sxs-lookup"><span data-stu-id="93e10-146">In the action properties, you can:</span></span>  

    * <span data-ttu-id="93e10-147">Použití **fronty** vlastnost, která fronty jiný název než co je definována v připojení</span><span class="sxs-lookup"><span data-stu-id="93e10-147">Use the **Queue** property to access a different queue name than what is defined in the connection</span></span>
    * <span data-ttu-id="93e10-148">Použití **MessageId**, **CorrelationId**, **GroupId**a dalších vlastností a vyhledejte zprávu založenou na jiné vlastnosti zprávy MQ</span><span class="sxs-lookup"><span data-stu-id="93e10-148">Use the **MessageId**, **CorrelationId**, **GroupId**, and other properties to browse for a message based on the different MQ message properties</span></span>
    * <span data-ttu-id="93e10-149">Nastavit **IncludeInfo** k **True** zahrnout informace o dalších zpráv ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="93e10-149">Set **IncludeInfo** to **True** to include additional message information in the output.</span></span> <span data-ttu-id="93e10-150">Nebo ji nastavte na **False** tak, aby neobsahoval informace o dalších zpráv ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="93e10-150">Or, set it to **False** to not include additional message information in the output.</span></span>
    * <span data-ttu-id="93e10-151">Zadejte **časový limit** hodnotu určete, jak dlouho čekat na zprávu, která přicházejí v prázdné frontě.</span><span class="sxs-lookup"><span data-stu-id="93e10-151">Enter a **Timeout** value to determine how long to wait for a message to arrive in an empty queue.</span></span> <span data-ttu-id="93e10-152">Pokud je zadán nic, se načte první zprávu ve frontě a neexistuje žádný doby čekání na zprávu, která se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="93e10-152">If nothing is entered, the first message in the queue is retrieved, and there is no time spent waiting for a message to appear.</span></span>  
    <span data-ttu-id="93e10-153">![Procházet vlastnosti zprávy](media/connectors-create-api-mq/Browse_message_Props.png)</span><span class="sxs-lookup"><span data-stu-id="93e10-153">![Browse Message Properties](media/connectors-create-api-mq/Browse_message_Props.png)</span></span>

5. <span data-ttu-id="93e10-154">**Uložit** změny a potom **spustit** svou aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="93e10-154">**Save** your changes, and then **Run** your logic app:</span></span>  
<span data-ttu-id="93e10-155">![Uložte a spusťte](media/connectors-create-api-mq/Save_Run.png)</span><span class="sxs-lookup"><span data-stu-id="93e10-155">![Save and run](media/connectors-create-api-mq/Save_Run.png)</span></span>

6. <span data-ttu-id="93e10-156">Za několik sekund jsou uvedeny kroky spuštění, a můžete se podívat na výstup.</span><span class="sxs-lookup"><span data-stu-id="93e10-156">After a few seconds, the steps of the run are shown, and you can look at the output.</span></span> <span data-ttu-id="93e10-157">Vyberte na zelenou značku zaškrtnutí zobrazíte podrobnosti o jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="93e10-157">Select the green checkmark to see details of each step.</span></span> <span data-ttu-id="93e10-158">Vyberte **najdete v části nezpracovaná výstupy** zobrazíte další podrobnosti na výstupní data.</span><span class="sxs-lookup"><span data-stu-id="93e10-158">Select **See raw outputs** to see additional details on the output data.</span></span>  
<span data-ttu-id="93e10-159">![Procházet výstup zpráv](media/connectors-create-api-mq/Browse_message_output.png)</span><span class="sxs-lookup"><span data-stu-id="93e10-159">![Browse message output](media/connectors-create-api-mq/Browse_message_output.png)</span></span>  

    <span data-ttu-id="93e10-160">Nezpracovaná výstup:</span><span class="sxs-lookup"><span data-stu-id="93e10-160">Raw output:</span></span>  
    ![Procházet nezpracovaná výstup zpráv](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. <span data-ttu-id="93e10-162">Když **IncludeInfo** je možnost nastavena na hodnotu true, se zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="93e10-162">When the **IncludeInfo** option is set to true, the following output is displayed:</span></span>  
<span data-ttu-id="93e10-163">![Procházet zpráva obsahovat informace o](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span><span class="sxs-lookup"><span data-stu-id="93e10-163">![Browse message include info](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span></span>

## <a name="browse-multiple-messages"></a><span data-ttu-id="93e10-164">Procházet více zpráv</span><span class="sxs-lookup"><span data-stu-id="93e10-164">Browse multiple messages</span></span>
<span data-ttu-id="93e10-165">**Procházet zprávy** zahrnuje akce **BatchSize** možnost indikující, kolik zpráv má být vrácen z fronty.</span><span class="sxs-lookup"><span data-stu-id="93e10-165">The **Browse messages** action includes a **BatchSize** option to indicate how many messages should be returned from the queue.</span></span>  <span data-ttu-id="93e10-166">Pokud **BatchSize** nemá žádný záznam, jsou vráceny všechny zprávy.</span><span class="sxs-lookup"><span data-stu-id="93e10-166">If **BatchSize** has no entry, all messages are returned.</span></span> <span data-ttu-id="93e10-167">Vrácený výstupem je pole zpráv.</span><span class="sxs-lookup"><span data-stu-id="93e10-167">The returned output is an array of messages.</span></span>

1. <span data-ttu-id="93e10-168">Při přidávání **procházet zprávy** je standardně vybraná akce, první připojení, který je nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="93e10-168">When adding the **Browse messages** action, the first connection that is configured is selected by default.</span></span> <span data-ttu-id="93e10-169">Vyberte **změnit připojení** vytvořte nové připojení, nebo vyberte jiné připojení.</span><span class="sxs-lookup"><span data-stu-id="93e10-169">Select **Change connection** to create a new connection, or select a different connection.</span></span>

2. <span data-ttu-id="93e10-170">Výstup procházet zprávy zobrazí:</span><span class="sxs-lookup"><span data-stu-id="93e10-170">The output of Browse messages shows:</span></span>  
![Procházet výstupní zprávy](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a><span data-ttu-id="93e10-172">Přijímat do jedné zprávy</span><span class="sxs-lookup"><span data-stu-id="93e10-172">Receive a single message</span></span>
<span data-ttu-id="93e10-173">**Příjmu zpráv** akce se stejnými vstupy a výstupy jako **zpráva Procházet** akce.</span><span class="sxs-lookup"><span data-stu-id="93e10-173">The **Receive message** action has the same inputs and outputs as the **Browse message** action.</span></span> <span data-ttu-id="93e10-174">Při použití **příjmu zpráv**, odstraní zprávu z fronty.</span><span class="sxs-lookup"><span data-stu-id="93e10-174">When using **Receive message**, the message is deleted from the queue.</span></span>

## <a name="receive-multiple-messages"></a><span data-ttu-id="93e10-175">Zobrazí více zpráv</span><span class="sxs-lookup"><span data-stu-id="93e10-175">Receive multiple messages</span></span>
<span data-ttu-id="93e10-176">**Přijímat zprávy** akce se stejnými vstupy a výstupy jako **procházet zprávy** akce.</span><span class="sxs-lookup"><span data-stu-id="93e10-176">The **Receive messages** action has the same inputs and outputs as the **Browse messages** action.</span></span> <span data-ttu-id="93e10-177">Při použití **přijímat zprávy**, zprávy jsou odstraněny z fronty.</span><span class="sxs-lookup"><span data-stu-id="93e10-177">When using **Receive messages**, the messages are deleted from the queue.</span></span>

<span data-ttu-id="93e10-178">Pokud nejsou žádné zprávy ve frontě, když procházet nebo příjmu, selže s následující výstup:</span><span class="sxs-lookup"><span data-stu-id="93e10-178">If there are no messages in the queue when doing a browse or a receive, the step fails with the following output:</span></span>  
![Ne MQ zprávy chyby](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a><span data-ttu-id="93e10-180">Odeslat zprávu</span><span class="sxs-lookup"><span data-stu-id="93e10-180">Send a message</span></span>
1. <span data-ttu-id="93e10-181">Při přidávání **odesílání zpráv** ve výchozím nastavení je vybrané akce, první připojení, který je nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="93e10-181">When adding the **Send message** action, the first connection that is configured is selected by default.</span></span> <span data-ttu-id="93e10-182">Vyberte **změnit připojení** vytvořte nové připojení, nebo vyberte jiné připojení.</span><span class="sxs-lookup"><span data-stu-id="93e10-182">Select **Change connection** to create a new connection, or select a different connection.</span></span> <span data-ttu-id="93e10-183">Platném **typy zpráv** jsou **Datagram**, **odpověď**, nebo **požadavku**.</span><span class="sxs-lookup"><span data-stu-id="93e10-183">The valid **Message Types** are **Datagram**, **Reply**, or **Request**.</span></span>  
<span data-ttu-id="93e10-184">![Odeslat Msg Props](media/connectors-create-api-mq/Send_Msg_Props.png)</span><span class="sxs-lookup"><span data-stu-id="93e10-184">![Send Msg Props](media/connectors-create-api-mq/Send_Msg_Props.png)</span></span>

2. <span data-ttu-id="93e10-185">Výstup odeslat zpráva vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="93e10-185">The output of Send message looks like the following:</span></span>  
![Odeslání výstupu Msg](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a><span data-ttu-id="93e10-187">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="93e10-187">Connector-specific details</span></span>

<span data-ttu-id="93e10-188">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/mq/).</span><span class="sxs-lookup"><span data-stu-id="93e10-188">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/mq/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="93e10-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93e10-189">Next steps</span></span>
<span data-ttu-id="93e10-190">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="93e10-190">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="93e10-191">Prozkoumejte dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="93e10-191">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
