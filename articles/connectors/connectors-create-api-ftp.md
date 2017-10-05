---
title: "Naučte se používat konektor FTP v aplikace logiky | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Připojte k serveru FTP spravovat soubory. Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v serveru FTP."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d83c55fe-eb59-4b7b-a5ec-afac5c772616
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/22/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 61bfbedfd4f1e84b6976099323a32f3a720634c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-ftp-connector"></a><span data-ttu-id="869b1-105">Začínáme s konektor FTP</span><span class="sxs-lookup"><span data-stu-id="869b1-105">Get started with the FTP connector</span></span>
<span data-ttu-id="869b1-106">Pomocí konektoru serveru FTP monitorovat, spravovat a vytvoření souborů na serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="869b1-106">Use the FTP connector to monitor, manage and create files on an  FTP server.</span></span> 

<span data-ttu-id="869b1-107">Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="869b1-107">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="869b1-108">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="869b1-108">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-ftp"></a><span data-ttu-id="869b1-109">Připojení k serveru FTP</span><span class="sxs-lookup"><span data-stu-id="869b1-109">Connect to FTP</span></span>
<span data-ttu-id="869b1-110">Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="869b1-110">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="869b1-111">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="869b1-111">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-ftp"></a><span data-ttu-id="869b1-112">Umožňuje vytvořit připojení k serveru FTP</span><span class="sxs-lookup"><span data-stu-id="869b1-112">Create a connection to FTP</span></span>
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a><span data-ttu-id="869b1-113">Použít aktivační událost FTP</span><span class="sxs-lookup"><span data-stu-id="869b1-113">Use a FTP trigger</span></span>
<span data-ttu-id="869b1-114">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="869b1-114">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="869b1-115">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="869b1-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="869b1-116">Konektor FTP vyžaduje serveru FTP, který je dostupný z Internetu a je nakonfigurován na provoz s pasivní režim.</span><span class="sxs-lookup"><span data-stu-id="869b1-116">The FTP connector requires an FTP server that  is accessible from the Internet and is configured to operate with PASSIVE mode.</span></span> <span data-ttu-id="869b1-117">Konektor FTP je taky **není kompatibilní s implicitní FTPS (FTP přes protokol SSL)**.</span><span class="sxs-lookup"><span data-stu-id="869b1-117">Also, the FTP connector is **not compatible with implicit FTPS (FTP over SSL)**.</span></span> <span data-ttu-id="869b1-118">Konektor FTP podporuje pouze explicitní FTPS (FTP přes protokol SSL).</span><span class="sxs-lookup"><span data-stu-id="869b1-118">The FTP connector only supports explicit FTPS (FTP over SSL).</span></span>  
> 
> 

<span data-ttu-id="869b1-119">V tomto příkladu I vám ukáže, jak používat **FTP – Pokud je soubor přidat ani upravit** aktivovat Pokud chcete spustit pracovní postup aplikace logiky, pokud je přidán do, nebo změny souboru na serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="869b1-119">In this example, I will show you how to use the **FTP - When a file is added or modified** trigger to initiate a logic app workflow when a file is added to, or modified on, an FTP server.</span></span> <span data-ttu-id="869b1-120">V příklad enterprise můžete použít této aktivační události k monitorování složky FTP pro nové soubory, které představují objednávek zákazníků.</span><span class="sxs-lookup"><span data-stu-id="869b1-120">In an enterprise example, you could use this trigger to monitor an FTP folder for new files that represent orders from customers.</span></span>  <span data-ttu-id="869b1-121">Poté můžete použít akci konektor FTP, jako **získat obsah souboru** získat obsah pořadí pro další zpracování a úložiště v databázi objednávky.</span><span class="sxs-lookup"><span data-stu-id="869b1-121">You could then use an FTP connector action such as **Get file content** to get the contents of the order for further processing and storage in your orders database.</span></span>

1. <span data-ttu-id="869b1-122">Zadejte *ftp* do vyhledávacího pole v designeru aplikace logiky zvolte **FTP – Pokud je soubor přidat ani upravit** aktivační události</span><span class="sxs-lookup"><span data-stu-id="869b1-122">Enter *ftp* in the search box on the logic apps designer then select the **FTP - When a file is added or modified**  trigger</span></span>   
   <span data-ttu-id="869b1-123">![Obrázek aktivační událost FTP 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="869b1-123">![FTP trigger image 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span></span>  
   <span data-ttu-id="869b1-124">**Je přidána nebo upravena souboru** otevře se ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="869b1-124">The **When a file is added or modified** control opens up</span></span>  
   <span data-ttu-id="869b1-125">![Obrázek aktivační událost FTP 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="869b1-125">![FTP trigger image 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span></span>  
2. <span data-ttu-id="869b1-126">Vyberte **...**  nachází na pravé straně ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="869b1-126">Select the **...** located on the right side of the control.</span></span> <span data-ttu-id="869b1-127">Otevře se ovládací prvek pro výběr složky</span><span class="sxs-lookup"><span data-stu-id="869b1-127">This opens the folder picker control</span></span>  
   <span data-ttu-id="869b1-128">![Obrázek aktivační událost FTP 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span><span class="sxs-lookup"><span data-stu-id="869b1-128">![FTP trigger image 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span></span>  
3. <span data-ttu-id="869b1-129">Vyberte  **>**  (šipka doprava) a procházet a najít složku, kterou chcete sledovat pro nové nebo upravení soubory.</span><span class="sxs-lookup"><span data-stu-id="869b1-129">Select the **>** (right arrow) and browse to find the folder that you want to monitor for new or modified files.</span></span> <span data-ttu-id="869b1-130">Vyberte složku a složku se nyní zobrazí v oznámení **složky** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="869b1-130">Select the folder and notice the folder is now displayed in the **Folder** control.</span></span>  
   <span data-ttu-id="869b1-131">![FTP aktivační událost obrázek 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span><span class="sxs-lookup"><span data-stu-id="869b1-131">![FTP trigger image 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span></span>   

<span data-ttu-id="869b1-132">Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která zahájí spuštění ostatních triggery a akce v pracovním postupu po upravit nebo vytvořit ve složce konkrétní FTP souboru.</span><span class="sxs-lookup"><span data-stu-id="869b1-132">At this point, your logic app has been configured with a trigger that will begin a run of the other triggers and actions in the workflow when a file is either modified or created in the specific FTP folder.</span></span> 

> [!NOTE]
> <span data-ttu-id="869b1-133">Pro aplikace logiky být funkční musí obsahovat nejméně jedna aktivační událost a jedna akce.</span><span class="sxs-lookup"><span data-stu-id="869b1-133">For a logic app to be functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="869b1-134">Postupujte podle kroků v další části a přidejte akci.</span><span class="sxs-lookup"><span data-stu-id="869b1-134">Follow the steps in the next section to add an action.</span></span>  
> 
> 

## <a name="use-a-ftp-action"></a><span data-ttu-id="869b1-135">Pomocí akce FTP</span><span class="sxs-lookup"><span data-stu-id="869b1-135">Use a FTP action</span></span>
<span data-ttu-id="869b1-136">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="869b1-136">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="869b1-137">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="869b1-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="869b1-138">Teď, když jste přidali aktivační událost, použijte následující postup přidání akce, která bude získán obsah souboru nové nebo upravené nalezena. aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="869b1-138">Now that you have added a trigger, follow these steps to add an action that will get the contents of the new or modified file found by the trigger.</span></span>    

1. <span data-ttu-id="869b1-139">Vyberte **+ nový krok** přidat akce, která má být získán obsah souboru na serveru FTP</span><span class="sxs-lookup"><span data-stu-id="869b1-139">Select **+ New step** to add the the action to get the contents of the file on the FTP server</span></span>  
2. <span data-ttu-id="869b1-140">Vyberte **přidat akci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="869b1-140">Select the **Add an action** link.</span></span>  
   <span data-ttu-id="869b1-141">![Obrázek akce FTP 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span><span class="sxs-lookup"><span data-stu-id="869b1-141">![FTP action image 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span></span>  
3. <span data-ttu-id="869b1-142">Zadejte *FTP* k vyhledání všech akcí souvisejících s FTP.</span><span class="sxs-lookup"><span data-stu-id="869b1-142">Enter *FTP* to search for all actions related to FTP.</span></span>
4. <span data-ttu-id="869b1-143">Vyberte **FTP – získat obsah souboru** jako akce provést v případě nové nebo upravené soubor naleznete ve složce serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="869b1-143">Select **FTP - Get file content**  as the action to take when a new or modified file is found in the FTP folder.</span></span>      
   <span data-ttu-id="869b1-144">![Obrázek akce FTP 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span><span class="sxs-lookup"><span data-stu-id="869b1-144">![FTP action image 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span></span>  
   <span data-ttu-id="869b1-145">**Získat obsah souboru** řízení otevře.</span><span class="sxs-lookup"><span data-stu-id="869b1-145">The **Get file content** control opens.</span></span> <span data-ttu-id="869b1-146">**Poznámka:**: Zobrazí se výzva k autorizaci aplikace logiky k přístupu k účtu serveru FTP, pokud jste tak dosud neučinili dříve.</span><span class="sxs-lookup"><span data-stu-id="869b1-146">**Note**: you will be prompted to authorize your logic app to access your FTP server account if you have not done so previously.</span></span>  
   <span data-ttu-id="869b1-147">![Obrázek akce FTP 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span><span class="sxs-lookup"><span data-stu-id="869b1-147">![FTP action image 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span></span>   
5. <span data-ttu-id="869b1-148">Vyberte **soubor** ovládací prvek (mezeru nacházel pod **souboru***).</span><span class="sxs-lookup"><span data-stu-id="869b1-148">Select the **File** control (the white space located below **FILE***).</span></span> <span data-ttu-id="869b1-149">Zde můžete vytvořit různé vlastnosti ze souboru nové nebo upravené nalezena na serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="869b1-149">Here, you can use any of the various properties from the new or modified file found on the FTP server.</span></span>  
6. <span data-ttu-id="869b1-150">Vyberte **souboru obsahu** možnost.</span><span class="sxs-lookup"><span data-stu-id="869b1-150">Select the **File content** option.</span></span>  
   <span data-ttu-id="869b1-151">![Obrázek akce FTP 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span><span class="sxs-lookup"><span data-stu-id="869b1-151">![FTP action image 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span></span>   
7. <span data-ttu-id="869b1-152">Ovládací prvek se aktualizuje, což indikuje, že **FTP – získat obsah souboru** akce získáte *souboru obsahu* nové nebo upravené souboru na serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="869b1-152">The control is updated, indicating that the **FTP - Get file content** action will get the *file content* of the new or modified file on the FTP server.</span></span>      
   <span data-ttu-id="869b1-153">![Obrázek akce FTP 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span><span class="sxs-lookup"><span data-stu-id="869b1-153">![FTP action image 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span></span>     
8. <span data-ttu-id="869b1-154">Uložte si práci, pak přidá soubor do složky FTP k testování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="869b1-154">Save your work then add a file to the FTP folder to test your workflow.</span></span>    

<span data-ttu-id="869b1-155">Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační události k monitorování složky na serveru FTP a zahájit pracovního postupu, pokud najde soubor nové nebo upravené soubor na serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="869b1-155">At this point, the logic app has been configured with a trigger to monitor a folder on an FTP server and initiate the workflow when it finds either a new file or a modified file on the FTP server.</span></span> 

<span data-ttu-id="869b1-156">Aplikace logiky také nakonfigurovaný pomocí akce má být získán obsah souboru nové nebo upravené.</span><span class="sxs-lookup"><span data-stu-id="869b1-156">The logic app also has been configured with an action to get the contents of the new or modified file.</span></span>

<span data-ttu-id="869b1-157">Nyní můžete přidat další akci, jako [SQL Server – vložit řádek](connectors-create-api-sqlazure.md) akci vložení obsah souboru nové nebo upravené do tabulky databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="869b1-157">You can now add another action such as the [SQL Server - insert row](connectors-create-api-sqlazure.md) action to insert the contents of the new or modified file into a SQL database table.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="869b1-158">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="869b1-158">Connector-specific details</span></span>

<span data-ttu-id="869b1-159">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/ftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="869b1-159">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/ftpconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="869b1-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="869b1-160">Next Steps</span></span>
[<span data-ttu-id="869b1-161">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="869b1-161">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

