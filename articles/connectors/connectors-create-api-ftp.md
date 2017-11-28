---
title: aaaLearn jak toouse hello konektor FTP v aplikace logiky | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. Připojte tooFTP server toomanage vaše soubory. Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v serveru FTP."
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
ms.openlocfilehash: a7020df2005ebb34fc569627ae0516b8528cc7a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-ftp-connector"></a><span data-ttu-id="63c8d-105">Začínáme s konektor FTP hello</span><span class="sxs-lookup"><span data-stu-id="63c8d-105">Get started with hello FTP connector</span></span>
<span data-ttu-id="63c8d-106">Pomocí konektoru toomonitor hello FTP, spravovat a vytvoření souborů na serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="63c8d-106">Use hello FTP connector toomonitor, manage and create files on an  FTP server.</span></span> 

<span data-ttu-id="63c8d-107">toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="63c8d-107">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="63c8d-108">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="63c8d-108">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooftp"></a><span data-ttu-id="63c8d-109">Připojit tooFTP</span><span class="sxs-lookup"><span data-stu-id="63c8d-109">Connect tooFTP</span></span>
<span data-ttu-id="63c8d-110">Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="63c8d-110">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="63c8d-111">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="63c8d-111">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-tooftp"></a><span data-ttu-id="63c8d-112">Vytvoření připojení tooFTP</span><span class="sxs-lookup"><span data-stu-id="63c8d-112">Create a connection tooFTP</span></span>
> [!INCLUDE [Steps toocreate a connection tooFTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a><span data-ttu-id="63c8d-113">Použít aktivační událost FTP</span><span class="sxs-lookup"><span data-stu-id="63c8d-113">Use a FTP trigger</span></span>
<span data-ttu-id="63c8d-114">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="63c8d-114">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="63c8d-115">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="63c8d-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="63c8d-116">konektor FTP Hello vyžaduje serveru FTP, který je dostupný z Internetu hello a je nakonfigurované toooperate pasivní režim.</span><span class="sxs-lookup"><span data-stu-id="63c8d-116">hello FTP connector requires an FTP server that  is accessible from hello Internet and is configured toooperate with PASSIVE mode.</span></span> <span data-ttu-id="63c8d-117">Je taky hello FTP konektor **není kompatibilní s implicitní FTPS (FTP přes protokol SSL)**.</span><span class="sxs-lookup"><span data-stu-id="63c8d-117">Also, hello FTP connector is **not compatible with implicit FTPS (FTP over SSL)**.</span></span> <span data-ttu-id="63c8d-118">konektor FTP Hello podporuje pouze explicitní FTPS (FTP přes protokol SSL).</span><span class="sxs-lookup"><span data-stu-id="63c8d-118">hello FTP connector only supports explicit FTPS (FTP over SSL).</span></span>  
> 
> 

<span data-ttu-id="63c8d-119">V tomto příkladu I vám ukáže, jak toouse hello **FTP – Pokud je soubor přidat ani upravit** aktivují tooinitiate pracovní postup aplikace logiky, když je přidán do, nebo změny souboru na serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="63c8d-119">In this example, I will show you how toouse hello **FTP - When a file is added or modified** trigger tooinitiate a logic app workflow when a file is added to, or modified on, an FTP server.</span></span> <span data-ttu-id="63c8d-120">V příklad enterprise můžete použít tento aktivační událost toomonitor složky FTP pro nové soubory, které představují objednávek zákazníků.</span><span class="sxs-lookup"><span data-stu-id="63c8d-120">In an enterprise example, you could use this trigger toomonitor an FTP folder for new files that represent orders from customers.</span></span>  <span data-ttu-id="63c8d-121">Poté můžete použít akci konektor FTP, jako **získat obsah souboru** tooget hello obsah hello pořadí pro další zpracování a úložiště v databázi objednávky.</span><span class="sxs-lookup"><span data-stu-id="63c8d-121">You could then use an FTP connector action such as **Get file content** tooget hello contents of hello order for further processing and storage in your orders database.</span></span>

1. <span data-ttu-id="63c8d-122">Zadejte *ftp* hello vyhledávacího pole v designeru aplikace logiky hello zvolte hello **FTP – Pokud je soubor přidat ani upravit** aktivační události</span><span class="sxs-lookup"><span data-stu-id="63c8d-122">Enter *ftp* in hello search box on hello logic apps designer then select hello **FTP - When a file is added or modified**  trigger</span></span>   
   <span data-ttu-id="63c8d-123">![Obrázek aktivační událost FTP 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="63c8d-123">![FTP trigger image 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span></span>  
   <span data-ttu-id="63c8d-124">Hello **je přidána nebo upravena souboru** otevře se ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="63c8d-124">hello **When a file is added or modified** control opens up</span></span>  
   <span data-ttu-id="63c8d-125">![Obrázek aktivační událost FTP 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="63c8d-125">![FTP trigger image 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span></span>  
2. <span data-ttu-id="63c8d-126">Vyberte hello **...**  nachází na pravé straně hello hello řízení.</span><span class="sxs-lookup"><span data-stu-id="63c8d-126">Select hello **...** located on hello right side of hello control.</span></span> <span data-ttu-id="63c8d-127">Otevře se ovládací prvek pro výběr složky hello</span><span class="sxs-lookup"><span data-stu-id="63c8d-127">This opens hello folder picker control</span></span>  
   <span data-ttu-id="63c8d-128">![Obrázek aktivační událost FTP 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span><span class="sxs-lookup"><span data-stu-id="63c8d-128">![FTP trigger image 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span></span>  
3. <span data-ttu-id="63c8d-129">Vyberte hello  **>**  (šipka doprava) a vyhledejte složku hello toofind má toomonitor pro nové nebo upravení soubory.</span><span class="sxs-lookup"><span data-stu-id="63c8d-129">Select hello **>** (right arrow) and browse toofind hello folder that you want toomonitor for new or modified files.</span></span> <span data-ttu-id="63c8d-130">Vyberte složku hello a Všimněte si hello složky se nyní zobrazí v hello **složky** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="63c8d-130">Select hello folder and notice hello folder is now displayed in hello **Folder** control.</span></span>  
   <span data-ttu-id="63c8d-131">![FTP aktivační událost obrázek 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span><span class="sxs-lookup"><span data-stu-id="63c8d-131">![FTP trigger image 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span></span>   

<span data-ttu-id="63c8d-132">Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která bude zahájena spuštění hello jiných triggery a akce v pracovním postupu hello po upravit nebo vytvořit v konkrétní složce FTP hello souboru.</span><span class="sxs-lookup"><span data-stu-id="63c8d-132">At this point, your logic app has been configured with a trigger that will begin a run of hello other triggers and actions in hello workflow when a file is either modified or created in hello specific FTP folder.</span></span> 

> [!NOTE]
> <span data-ttu-id="63c8d-133">Pro logiku aplikace toobe funkční musí obsahovat nejméně jedna aktivační událost a jedna akce.</span><span class="sxs-lookup"><span data-stu-id="63c8d-133">For a logic app toobe functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="63c8d-134">Postupujte podle kroků hello v další části tooadd hello akce.</span><span class="sxs-lookup"><span data-stu-id="63c8d-134">Follow hello steps in hello next section tooadd an action.</span></span>  
> 
> 

## <a name="use-a-ftp-action"></a><span data-ttu-id="63c8d-135">Pomocí akce FTP</span><span class="sxs-lookup"><span data-stu-id="63c8d-135">Use a FTP action</span></span>
<span data-ttu-id="63c8d-136">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="63c8d-136">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="63c8d-137">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="63c8d-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="63c8d-138">Teď, když jste přidali aktivační událost, postupujte podle těchto kroků tooadd akci, která bude načíst obsah hello hello nové nebo upravené souboru nalezen hello aktivační procedura.</span><span class="sxs-lookup"><span data-stu-id="63c8d-138">Now that you have added a trigger, follow these steps tooadd an action that will get hello contents of hello new or modified file found by hello trigger.</span></span>    

1. <span data-ttu-id="63c8d-139">Vyberte **+ nový krok** tooadd hello hello akce tooget hello obsah souboru hello na serveru hello FTP</span><span class="sxs-lookup"><span data-stu-id="63c8d-139">Select **+ New step** tooadd hello hello action tooget hello contents of hello file on hello FTP server</span></span>  
2. <span data-ttu-id="63c8d-140">Vyberte hello **přidat akci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="63c8d-140">Select hello **Add an action** link.</span></span>  
   <span data-ttu-id="63c8d-141">![Obrázek akce FTP 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span><span class="sxs-lookup"><span data-stu-id="63c8d-141">![FTP action image 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span></span>  
3. <span data-ttu-id="63c8d-142">Zadejte *FTP* toosearch pro všechny akce související s tooFTP.</span><span class="sxs-lookup"><span data-stu-id="63c8d-142">Enter *FTP* toosearch for all actions related tooFTP.</span></span>
4. <span data-ttu-id="63c8d-143">Vyberte **FTP – získat obsah souboru** jako hello tootake akce při nové nebo upravené soubor naleznete ve složce hello FTP.</span><span class="sxs-lookup"><span data-stu-id="63c8d-143">Select **FTP - Get file content**  as hello action tootake when a new or modified file is found in hello FTP folder.</span></span>      
   <span data-ttu-id="63c8d-144">![Obrázek akce FTP 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span><span class="sxs-lookup"><span data-stu-id="63c8d-144">![FTP action image 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span></span>  
   <span data-ttu-id="63c8d-145">Hello **získat obsah souboru** řízení otevře.</span><span class="sxs-lookup"><span data-stu-id="63c8d-145">hello **Get file content** control opens.</span></span> <span data-ttu-id="63c8d-146">**Poznámka:**: vám bude výzvami tooauthorize vaše tooaccess aplikace logiky váš server FTP účtu, pokud jste tak dosud neučinili dříve.</span><span class="sxs-lookup"><span data-stu-id="63c8d-146">**Note**: you will be prompted tooauthorize your logic app tooaccess your FTP server account if you have not done so previously.</span></span>  
   <span data-ttu-id="63c8d-147">![Obrázek akce FTP 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span><span class="sxs-lookup"><span data-stu-id="63c8d-147">![FTP action image 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span></span>   
5. <span data-ttu-id="63c8d-148">Vyberte hello **soubor** ovládací prvek (hello mezer nacházel pod **souboru***).</span><span class="sxs-lookup"><span data-stu-id="63c8d-148">Select hello **File** control (hello white space located below **FILE***).</span></span> <span data-ttu-id="63c8d-149">Tady můžete použít všechny hello různé vlastnosti z hello nový nebo změněný soubor nalezen na serveru FTP hello.</span><span class="sxs-lookup"><span data-stu-id="63c8d-149">Here, you can use any of hello various properties from hello new or modified file found on hello FTP server.</span></span>  
6. <span data-ttu-id="63c8d-150">Vyberte hello **souboru obsahu** možnost.</span><span class="sxs-lookup"><span data-stu-id="63c8d-150">Select hello **File content** option.</span></span>  
   <span data-ttu-id="63c8d-151">![Obrázek akce FTP 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span><span class="sxs-lookup"><span data-stu-id="63c8d-151">![FTP action image 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span></span>   
7. <span data-ttu-id="63c8d-152">ovládací prvek Hello aktualizován, která určuje, že hello **FTP – získat obsah souboru** akce získají hello *souboru obsahu* hello nové nebo upravené souboru na serveru hello FTP.</span><span class="sxs-lookup"><span data-stu-id="63c8d-152">hello control is updated, indicating that hello **FTP - Get file content** action will get hello *file content* of hello new or modified file on hello FTP server.</span></span>      
   <span data-ttu-id="63c8d-153">![Obrázek akce FTP 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span><span class="sxs-lookup"><span data-stu-id="63c8d-153">![FTP action image 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span></span>     
8. <span data-ttu-id="63c8d-154">Uložte si práci, pak přidejte souboru toohello FTP složky tootest pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="63c8d-154">Save your work then add a file toohello FTP folder tootest your workflow.</span></span>    

<span data-ttu-id="63c8d-155">V tomto okamžiku aplikace logiky hello bylo nakonfigurované toomonitor aktivační události do složky na serveru FTP a pracovní postup inicializace hello Pokud najde soubor nové nebo upravené soubor na serveru hello FTP.</span><span class="sxs-lookup"><span data-stu-id="63c8d-155">At this point, hello logic app has been configured with a trigger toomonitor a folder on an FTP server and initiate hello workflow when it finds either a new file or a modified file on hello FTP server.</span></span> 

<span data-ttu-id="63c8d-156">aplikace logiky Hello také má nakonfigurované akci tooget hello obsah souboru hello nové nebo upravené.</span><span class="sxs-lookup"><span data-stu-id="63c8d-156">hello logic app also has been configured with an action tooget hello contents of hello new or modified file.</span></span>

<span data-ttu-id="63c8d-157">Nyní můžete přidat další akci, jako je například hello [SQL Server – vložit řádek](connectors-create-api-sqlazure.md) akce tooinsert hello obsah hello nové nebo upravené souboru do tabulky databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="63c8d-157">You can now add another action such as hello [SQL Server - insert row](connectors-create-api-sqlazure.md) action tooinsert hello contents of hello new or modified file into a SQL database table.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="63c8d-158">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="63c8d-158">Connector-specific details</span></span>

<span data-ttu-id="63c8d-159">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/ftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="63c8d-159">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/ftpconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="63c8d-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63c8d-160">Next Steps</span></span>
[<span data-ttu-id="63c8d-161">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="63c8d-161">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

