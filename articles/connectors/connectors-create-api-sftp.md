---
title: "Další informace o použití konektoru SFTP ve vašich logic apps | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Připojte k rozhraní API pomocí protokolu SFTP odesílat a přijímat soubory. Můžete provádět různé operace, jako je vytváření, aktualizace, získání nebo odstraňte některé soubory."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 31253d8daee1581167a96a20ba8ad529a04b3e92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sftp-connector"></a><span data-ttu-id="4cb33-105">Začínáme s konektorem SFTP</span><span class="sxs-lookup"><span data-stu-id="4cb33-105">Get started with the SFTP connector</span></span>
<span data-ttu-id="4cb33-106">Pomocí protokolu SFTP konektoru pro přístup k protokolu SFTP účet, který chcete odesílat a přijímat soubory.</span><span class="sxs-lookup"><span data-stu-id="4cb33-106">Use the SFTP connector to access an SFTP account to send and receive files.</span></span> <span data-ttu-id="4cb33-107">Můžete provádět různé operace, jako je vytváření, aktualizace, získání nebo odstraňte některé soubory.</span><span class="sxs-lookup"><span data-stu-id="4cb33-107">You can perform various operations such as create, update, get or delete files.</span></span>  

<span data-ttu-id="4cb33-108">Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="4cb33-108">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="4cb33-109">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="4cb33-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-sftp"></a><span data-ttu-id="4cb33-110">Připojit k protokolu SFTP</span><span class="sxs-lookup"><span data-stu-id="4cb33-110">Connect to SFTP</span></span>
<span data-ttu-id="4cb33-111">Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="4cb33-111">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="4cb33-112">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="4cb33-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-sftp"></a><span data-ttu-id="4cb33-113">Umožňuje vytvořit připojení k protokolu SFTP</span><span class="sxs-lookup"><span data-stu-id="4cb33-113">Create a connection to SFTP</span></span>
> [!INCLUDE [Steps to create a connection to SFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a><span data-ttu-id="4cb33-114">Použít aktivační procedury protokolu SFTP</span><span class="sxs-lookup"><span data-stu-id="4cb33-114">Use an SFTP trigger</span></span>
<span data-ttu-id="4cb33-115">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="4cb33-115">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="4cb33-116">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="4cb33-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="4cb33-117">V tomto příkladu **SFTP – Pokud je soubor přidat ani upravit** aktivační událost se použilo k zahájení pracovní postup aplikace logiky, když je soubor přidán do, nebo úpravě na serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="4cb33-117">In this example, the **SFTP - When a file is added or modified** trigger is used to initiate a logic app workflow when a file is added to, or modified on, an SFTP server.</span></span> <span data-ttu-id="4cb33-118">Můžete také přidat podmínku, která kontroluje obsah souboru nové nebo upravené a provádí rozhodnutí extrahujte soubor, pokud jeho obsah označuje, že by měla být rozbalena před použitím obsah.</span><span class="sxs-lookup"><span data-stu-id="4cb33-118">You also add a condition that checks the contents of the new or modified file, and makes a decision to extract the file if its contents indicate that it should be extracted before using the contents.</span></span> <span data-ttu-id="4cb33-119">Nakonec přidejte akci se extrahovat obsah souboru a umístit extrahované obsah do složky na serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="4cb33-119">Finally, add an action to extract the contents of a file, and place the extracted contents in a folder on the SFTP server.</span></span> 

<span data-ttu-id="4cb33-120">V příklad enterprise můžete použít této aktivační události k monitorování složky aplikace pomocí protokolu SFTP pro nové soubory, které představují objednávek zákazníků.</span><span class="sxs-lookup"><span data-stu-id="4cb33-120">In an enterprise example, you could use this trigger to monitor an SFTP folder for new files that represent customer orders.</span></span>  <span data-ttu-id="4cb33-121">Poté můžete použít konektor akce SFTP, jako například **získat obsah souboru**, chcete-li získat obsah pořadí pro další zpracování a úložiště v databázi objednávky.</span><span class="sxs-lookup"><span data-stu-id="4cb33-121">You could then use an SFTP connector action, such as **Get file content**, to get the contents of the order for further processing and storage in an orders database.</span></span>

> [!INCLUDE [Steps to create an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="4cb33-122">Přidat podmínku</span><span class="sxs-lookup"><span data-stu-id="4cb33-122">Add a condition</span></span>
> [!INCLUDE [Steps to add a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a><span data-ttu-id="4cb33-123">Použít akci protokolu SFTP</span><span class="sxs-lookup"><span data-stu-id="4cb33-123">Use an SFTP action</span></span>
<span data-ttu-id="4cb33-124">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="4cb33-124">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="4cb33-125">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="4cb33-125">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="4cb33-126">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="4cb33-126">Connector-specific details</span></span>

<span data-ttu-id="4cb33-127">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/sftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="4cb33-127">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sftpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="4cb33-128">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="4cb33-128">More connectors</span></span>
<span data-ttu-id="4cb33-129">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="4cb33-129">Go back to the [APIs list](apis-list.md).</span></span>