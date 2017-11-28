---
title: "aaaLearn jak toouse hello SFTP konektor ve vašich logic apps | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Připojte toosend tooSFTP rozhraní API a přijímat soubory. Můžete provádět různé operace, jako je vytváření, aktualizace, získání nebo odstraňte některé soubory."
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
ms.openlocfilehash: 3f50570191c9b9339fe6584b9056b2549512b789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sftp-connector"></a><span data-ttu-id="7a739-105">Začínáme s konektorem SFTP hello</span><span class="sxs-lookup"><span data-stu-id="7a739-105">Get started with hello SFTP connector</span></span>
<span data-ttu-id="7a739-106">Použijte hello SFTP konektor tooaccess toosend účet protokolu SFTP a přijímat soubory.</span><span class="sxs-lookup"><span data-stu-id="7a739-106">Use hello SFTP connector tooaccess an SFTP account toosend and receive files.</span></span> <span data-ttu-id="7a739-107">Můžete provádět různé operace, jako je vytváření, aktualizace, získání nebo odstraňte některé soubory.</span><span class="sxs-lookup"><span data-stu-id="7a739-107">You can perform various operations such as create, update, get or delete files.</span></span>  

<span data-ttu-id="7a739-108">toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="7a739-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="7a739-109">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="7a739-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosftp"></a><span data-ttu-id="7a739-110">Připojit tooSFTP</span><span class="sxs-lookup"><span data-stu-id="7a739-110">Connect tooSFTP</span></span>
<span data-ttu-id="7a739-111">Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="7a739-111">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="7a739-112">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="7a739-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-toosftp"></a><span data-ttu-id="7a739-113">Vytvoření připojení tooSFTP</span><span class="sxs-lookup"><span data-stu-id="7a739-113">Create a connection tooSFTP</span></span>
> [!INCLUDE [Steps toocreate a connection tooSFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a><span data-ttu-id="7a739-114">Použít aktivační procedury protokolu SFTP</span><span class="sxs-lookup"><span data-stu-id="7a739-114">Use an SFTP trigger</span></span>
<span data-ttu-id="7a739-115">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="7a739-115">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="7a739-116">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="7a739-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="7a739-117">V tomto příkladu hello **SFTP – Pokud je soubor přidat ani upravit** aktivační událost není použité tooinitiate pracovní postup aplikace logiky při je přidat do souboru, nebo úpravě na serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="7a739-117">In this example, hello **SFTP - When a file is added or modified** trigger is used tooinitiate a logic app workflow when a file is added to, or modified on, an SFTP server.</span></span> <span data-ttu-id="7a739-118">Můžete také přidat podmínku, která kontroluje hello obsah hello nové nebo upravené souboru a vytvoří soubor rozhodnutí tooextract hello, pokud jeho obsah označuje, že by měla být rozbalena před použitím hello obsah.</span><span class="sxs-lookup"><span data-stu-id="7a739-118">You also add a condition that checks hello contents of hello new or modified file, and makes a decision tooextract hello file if its contents indicate that it should be extracted before using hello contents.</span></span> <span data-ttu-id="7a739-119">Nakonec přidejte akci tooextract hello obsah souboru a umístit do složky na serveru pomocí protokolu SFTP hello hello extrahovat obsah.</span><span class="sxs-lookup"><span data-stu-id="7a739-119">Finally, add an action tooextract hello contents of a file, and place hello extracted contents in a folder on hello SFTP server.</span></span> 

<span data-ttu-id="7a739-120">V příklad enterprise můžete použít tuto aktivační událost toomonitor na složku SFTP pro nové soubory, které představují objednávek zákazníků.</span><span class="sxs-lookup"><span data-stu-id="7a739-120">In an enterprise example, you could use this trigger toomonitor an SFTP folder for new files that represent customer orders.</span></span>  <span data-ttu-id="7a739-121">Poté můžete použít konektor akce SFTP, jako například **získat obsah souboru**, tooget hello obsah hello pořadí pro další zpracování a úložiště v databázi objednávky.</span><span class="sxs-lookup"><span data-stu-id="7a739-121">You could then use an SFTP connector action, such as **Get file content**, tooget hello contents of hello order for further processing and storage in an orders database.</span></span>

> [!INCLUDE [Steps toocreate an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="7a739-122">Přidat podmínku</span><span class="sxs-lookup"><span data-stu-id="7a739-122">Add a condition</span></span>
> [!INCLUDE [Steps tooadd a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a><span data-ttu-id="7a739-123">Použít akci protokolu SFTP</span><span class="sxs-lookup"><span data-stu-id="7a739-123">Use an SFTP action</span></span>
<span data-ttu-id="7a739-124">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="7a739-124">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="7a739-125">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="7a739-125">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="7a739-126">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="7a739-126">Connector-specific details</span></span>

<span data-ttu-id="7a739-127">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/sftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="7a739-127">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sftpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="7a739-128">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="7a739-128">More connectors</span></span>
<span data-ttu-id="7a739-129">Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="7a739-129">Go back toohello [APIs list](apis-list.md).</span></span>
