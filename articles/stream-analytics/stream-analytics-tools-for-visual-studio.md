---
title: aaaUse Azure Stream Analytics Tools pro sadu Visual Studio | Microsoft Docs
description: "Úvodní kurz pro hello Azure Stream Analytics Tools pro sadu Visual Studio"
keywords: Visual studio
documentationcenter: 
services: stream-analytics
author: 
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 
ms.author: 
ms.openlocfilehash: bda8e548040509a6f29f1b713268bc38f73228fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a><span data-ttu-id="296df-104">Použití Azure Stream Analytics Tools pro sadu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="296df-104">Use Azure Stream Analytics Tools for Visual Studio</span></span>
## <a name="introduction"></a><span data-ttu-id="296df-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="296df-105">Introduction</span></span>
<span data-ttu-id="296df-106">V tomto kurzu zjistíte, jak vytvářet toouse Azure Stream Analytics Tools pro Visual Studio toocreate, lokálně otestovat, spravovat a ladit vaše úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="296df-106">In this tutorial, you learn how toouse Azure Stream Analytics Tools for Visual Studio toocreate, author, test locally, manage, and debug your Stream Analytics jobs.</span></span> 

<span data-ttu-id="296df-107">Po dokončení tohoto kurzu, budete moci:</span><span class="sxs-lookup"><span data-stu-id="296df-107">After completing this tutorial, you will be able to:</span></span>
* <span data-ttu-id="296df-108">Seznamte se s Stream Analytics Tools pro sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="296df-108">Familiarize yourself with Stream Analytics Tools for Visual Studio.</span></span>
* <span data-ttu-id="296df-109">Nakonfigurujte a nasaďte úloha Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="296df-109">Configure and deploy a Stream Analytics job.</span></span>
* <span data-ttu-id="296df-110">Otestujte úlohu místně s místní ukázková data.</span><span class="sxs-lookup"><span data-stu-id="296df-110">Test your job locally with local sample data.</span></span>
* <span data-ttu-id="296df-111">Pomocí monitorování tootroubleshoot problémy.</span><span class="sxs-lookup"><span data-stu-id="296df-111">Use monitoring tootroubleshoot issues.</span></span>
* <span data-ttu-id="296df-112">Exportujte stávající tooprojects úlohy.</span><span class="sxs-lookup"><span data-stu-id="296df-112">Export existing jobs tooprojects.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="296df-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="296df-113">Prerequisites</span></span>
<span data-ttu-id="296df-114">toocomplete tohoto kurzu budete potřebovat hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="296df-114">toocomplete this tutorial, you need hello following prerequisites:</span></span>
* <span data-ttu-id="296df-115">Dokončit hello kroky, které předcházet "Vytvořit úlohu služby Stream Analytics" v hello [sestavení řešení IoT pomocí služby Stream Analytics kurzu](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="296df-115">Finish hello steps that precede "Create a Stream Analytics job" in hello [Build an IoT solution by using Stream Analytics tutorial](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span></span> 
* <span data-ttu-id="296df-116">Pomocí sady Visual Studio 2015, Visual Studio 2013 update 4 nebo Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="296df-116">Use Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012.</span></span> <span data-ttu-id="296df-117">Enterprise (Ultimate nebo Premium), Professional a Community jsou podporované tyto edice.</span><span class="sxs-lookup"><span data-stu-id="296df-117">Enterprise (Ultimate/Premium), Professional, and Community editions are supported.</span></span> <span data-ttu-id="296df-118">Express edition není podporován.</span><span class="sxs-lookup"><span data-stu-id="296df-118">Express edition is not supported.</span></span> <span data-ttu-id="296df-119">Visual Studio 2017 není podporována.</span><span class="sxs-lookup"><span data-stu-id="296df-119">Visual Studio 2017 is not supported.</span></span> 
* <span data-ttu-id="296df-120">Použití hello Azure SDK pro .NET verze 2.7.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="296df-120">Use hello Azure SDK for .NET version 2.7.1 or later.</span></span> <span data-ttu-id="296df-121">Nainstalujte ji pomocí hello [instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="296df-121">Install it by using hello [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="296df-122">Nainstalujte hello [Stream Analytics Tools pro sadu Visual Studio](http://aka.ms/asatoolsvs).</span><span class="sxs-lookup"><span data-stu-id="296df-122">Install hello [Stream Analytics Tools for Visual Studio](http://aka.ms/asatoolsvs).</span></span>

## <a name="create-a-stream-analytics-project"></a><span data-ttu-id="296df-123">Vytvoření projektu Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="296df-123">Create a Stream Analytics project</span></span>
1. <span data-ttu-id="296df-124">V sadě Visual Studio, klikněte na tlačítko hello **soubor** nabídku a vyberte **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="296df-124">In Visual Studio, click hello **File** menu and select **New Project**.</span></span> 

2. <span data-ttu-id="296df-125">V seznamu šablon hello na levé straně hello, vyberte **Stream Analytics** a pak klikněte na **Azure Stream Analytics aplikace**.</span><span class="sxs-lookup"><span data-stu-id="296df-125">In hello templates list on hello left, select **Stream Analytics** and then click **Azure Stream Analytics Application**.</span></span>

3. <span data-ttu-id="296df-126">Zadejte hello projektu **název**, **umístění**, a **název řešení** jako u jiných projektů.</span><span class="sxs-lookup"><span data-stu-id="296df-126">Enter hello project **Name**, **Location**, and **Solution name** as you do for other projects.</span></span>

    ![Okno Nový projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    <span data-ttu-id="296df-128">A **Projedou** projektu se generuje ve **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="296df-128">A **Toll** project is generated in **Solution Explorer**.</span></span>

    ![Projedou projektu vygenerované v Průzkumníku řešení](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-hello-correct-subscription"></a><span data-ttu-id="296df-130">Vyberte správné předplatné hello</span><span class="sxs-lookup"><span data-stu-id="296df-130">Choose hello correct subscription</span></span>
1. <span data-ttu-id="296df-131">V sadě Visual Studio, klikněte na tlačítko hello **zobrazení** nabídky a otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="296df-131">In Visual Studio, click hello **View** menu and open **Server Explorer**.</span></span>

2. <span data-ttu-id="296df-132">Přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="296df-132">Sign in with your Azure Account.</span></span> 

## <a name="define-hello-input-sources"></a><span data-ttu-id="296df-133">Definování vstupních zdrojů hello</span><span class="sxs-lookup"><span data-stu-id="296df-133">Define hello input sources</span></span>
1.  <span data-ttu-id="296df-134">V **Průzkumníku řešení**, rozbalte položku hello **vstupy** uzlu a přejmenujte **Input.json** příliš**EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="296df-134">In **Solution Explorer**, expand hello **Inputs** node and rename **Input.json** too**EntryStream.json**.</span></span> <span data-ttu-id="296df-135">Klikněte dvakrát na **EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="296df-135">Double-click **EntryStream.json**.</span></span>
2.  <span data-ttu-id="296df-136">Hello **vstupní Alias** je nyní **EntryStream**.</span><span class="sxs-lookup"><span data-stu-id="296df-136">hello **Input Alias** is now **EntryStream**.</span></span> <span data-ttu-id="296df-137">Hello vstupní alias se používá ve skriptu hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="296df-137">hello input alias is used in hello query script.</span></span> 
3.  <span data-ttu-id="296df-138">V **typ zdroje**, vyberte **datový proud**.</span><span class="sxs-lookup"><span data-stu-id="296df-138">In **Source Type**, select **Data Stream**.</span></span>
4.  <span data-ttu-id="296df-139">V **zdroj**, vyberte **centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="296df-139">In **Source**, select **Event Hub**.</span></span>
5.  <span data-ttu-id="296df-140">V **Service Bus Namespace**, vyberte hello **TollData** možnost.</span><span class="sxs-lookup"><span data-stu-id="296df-140">In **Service Bus Namespace**, select hello **TollData** option.</span></span>
6.  <span data-ttu-id="296df-141">V **název centra událostí**, vyberte **položka**.</span><span class="sxs-lookup"><span data-stu-id="296df-141">In **Event Hub Name**, select **entry**.</span></span>
7.  <span data-ttu-id="296df-142">V **název zásady centra událostí**, vyberte **RootManageSharedAccessKey** (hello výchozí hodnota).</span><span class="sxs-lookup"><span data-stu-id="296df-142">In **Event Hub Policy Name**, select **RootManageSharedAccessKey** (hello default value).</span></span>
8.  <span data-ttu-id="296df-143">V **formát serializace událostí**, vyberte **Json**.</span><span class="sxs-lookup"><span data-stu-id="296df-143">In **Event Serialization Format**, select **Json**.</span></span> 
9.  <span data-ttu-id="296df-144">V **kódování**, vyberte **UTF8**.</span><span class="sxs-lookup"><span data-stu-id="296df-144">In **Encoding**, select **UTF8**.</span></span> <span data-ttu-id="296df-145">Vaše nastavení by mělo vypadat jako hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="296df-145">Your settings should look like hello following screenshot:</span></span>

    ![Vstupní okna](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. <span data-ttu-id="296df-147">toofinish hello průvodce, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="296df-147">toofinish hello wizard, click **Save**.</span></span> <span data-ttu-id="296df-148">Teď můžete přidat další vstupní zdroj toocreate hello konec datového proudu.</span><span class="sxs-lookup"><span data-stu-id="296df-148">Now you can add another input source toocreate hello exit stream.</span></span> <span data-ttu-id="296df-149">Klikněte pravým tlačítkem na hello **vstupy** uzel a vyberte možnost **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="296df-149">Right-click hello **Inputs** node, and select **New Item**.</span></span>

    ![Nová položka](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. <span data-ttu-id="296df-151">V okně hello vyberte **Azure Stream Analytics vstup**a změňte hello **název** příliš**ExitStream.json**.</span><span class="sxs-lookup"><span data-stu-id="296df-151">In hello window, select **Azure Stream Analytics Input**, and change hello **Name** too**ExitStream.json**.</span></span> <span data-ttu-id="296df-152">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="296df-152">Click **Add**.</span></span>

    ![Přidat novou položku – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. <span data-ttu-id="296df-154">Klikněte dvakrát na **ExitStream.json** v projektu hello a postupujte podle hello stejné kroky jako jste to udělali pro hello vstupní datový proud.</span><span class="sxs-lookup"><span data-stu-id="296df-154">Double-click **ExitStream.json** in hello project, and follow hello same steps as you did for hello entry stream.</span></span> <span data-ttu-id="296df-155">Být jisti tooenter **ukončete** pro hello **název centra událostí** jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="296df-155">Be sure tooenter **exit** for hello **Event Hub Name** as shown in hello following screenshot:</span></span>

    ![Okno ExitStream](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    <span data-ttu-id="296df-157">Nyní jste definovali dva vstupní datové proudy:</span><span class="sxs-lookup"><span data-stu-id="296df-157">Now you have defined two input streams:</span></span>

    ![Vstupní a výstupní vstupní datové proudy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    <span data-ttu-id="296df-159">Dál přidejte referenčního datového vstupu pro soubor hello objektů blob, který obsahuje car registrační data.</span><span class="sxs-lookup"><span data-stu-id="296df-159">Next, add reference data input for hello blob file that contains car registration data.</span></span>

13. <span data-ttu-id="296df-160">Klikněte pravým tlačítkem na hello **vstupy** uzlu hello projektu a potom postupujte podle hello stejné kroky jako jste to udělali pro vstupy hello datového proudu.</span><span class="sxs-lookup"><span data-stu-id="296df-160">Right-click hello **Inputs** node in hello project, and then follow hello same steps as you did for hello stream inputs.</span></span> <span data-ttu-id="296df-161">V **vstupní Alias**, zadejte **registrace**a v **typ zdroje**, vyberte **referenční data**.</span><span class="sxs-lookup"><span data-stu-id="296df-161">In **Input Alias**, enter **Registration**, and in **Source Type**, select **Reference data**.</span></span>

    ![Okno registrace](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. <span data-ttu-id="296df-163">V **účet úložiště**, vyberte hello **tolldata** možnost.</span><span class="sxs-lookup"><span data-stu-id="296df-163">In **Storage Account**, select hello **tolldata** option.</span></span> <span data-ttu-id="296df-164">V **kontejneru**, vyberte **tolldata**a v **vzorek cesty**, zadejte **registration.json**.</span><span class="sxs-lookup"><span data-stu-id="296df-164">In **Container**, select **tolldata**, and in **Path Pattern**, enter **registration.json**.</span></span> <span data-ttu-id="296df-165">Tento název souboru je velká a malá písmena a musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="296df-165">This file name is case sensitive and should be lowercase.</span></span>
15. <span data-ttu-id="296df-166">toofinish hello průvodce, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="296df-166">toofinish hello wizard, click **Save**.</span></span>

<span data-ttu-id="296df-167">Nyní jsou definovány všechny vstupy hello.</span><span class="sxs-lookup"><span data-stu-id="296df-167">Now all hello inputs are defined.</span></span>

## <a name="define-hello-output"></a><span data-ttu-id="296df-168">Definování výstup hello</span><span class="sxs-lookup"><span data-stu-id="296df-168">Define hello output</span></span>
1.  <span data-ttu-id="296df-169">V **Průzkumníku řešení**, rozbalte položku hello **vstupy** uzel a poklikejte na soubor **Output.json**.</span><span class="sxs-lookup"><span data-stu-id="296df-169">In **Solution Explorer**, expand hello **Inputs** node and double-click **Output.json**.</span></span>

2.  <span data-ttu-id="296df-170">V **Alias pro výstup**, zadejte **výstup**.</span><span class="sxs-lookup"><span data-stu-id="296df-170">In **Output Alias**, enter **output**.</span></span> 
3.  <span data-ttu-id="296df-171">V **jímky**, vyberte **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="296df-171">In **Sink**, select **SQL Database**.</span></span>
4.  <span data-ttu-id="296df-172">V **databáze**, vyberte **TollDataDB**.</span><span class="sxs-lookup"><span data-stu-id="296df-172">In **Database**, select **TollDataDB**.</span></span>
5.  <span data-ttu-id="296df-173">V **uživatelské jméno**, zadejte **tolladmin**.</span><span class="sxs-lookup"><span data-stu-id="296df-173">In **User Name**, enter **tolladmin**.</span></span> 
6.  <span data-ttu-id="296df-174">V **heslo**, zadejte **123toll!**.</span><span class="sxs-lookup"><span data-stu-id="296df-174">In **Password**, enter **123toll!**.</span></span>
7.  <span data-ttu-id="296df-175">V **tabulky**, zadejte **TollDataRefJoin**.</span><span class="sxs-lookup"><span data-stu-id="296df-175">In **Table**, enter **TollDataRefJoin**.</span></span>
8.  <span data-ttu-id="296df-176">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="296df-176">Click **Save**.</span></span>

    ![Výstup – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a><span data-ttu-id="296df-178">Vytvořte dotaz služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="296df-178">Create a Stream Analytics query</span></span>
<span data-ttu-id="296df-179">V tomto kurzu pokusí tooanswer několik obchodní otázky, které jsou tootoll související data.</span><span class="sxs-lookup"><span data-stu-id="296df-179">This tutorial attempts tooanswer several business questions that are related tootoll data.</span></span> <span data-ttu-id="296df-180">Vytvoří také Stream Analytics dotazy, které lze použít v odpovědi relevantní tooprovide Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="296df-180">It also constructs Stream Analytics queries that can be used in Stream Analytics tooprovide relevant answers.</span></span>
<span data-ttu-id="296df-181">Než začnete vaše první práce Stream Analytics, podíváme se na jednoduchý scénář a hello syntaxe dotazu.</span><span class="sxs-lookup"><span data-stu-id="296df-181">Before you start your first Stream Analytics job, let’s explore a simple scenario and hello query syntax.</span></span>

### <a name="introduction-toohello-stream-analytics-query-language"></a><span data-ttu-id="296df-182">Úvod toohello Stream Analytics query language</span><span class="sxs-lookup"><span data-stu-id="296df-182">Introduction toohello Stream Analytics query language</span></span>
<span data-ttu-id="296df-183">Řekněme, že je potřeba toocount hello počet vozidel, které projedou stánek zadejte.</span><span class="sxs-lookup"><span data-stu-id="296df-183">Let’s say that you need toocount hello number of vehicles that enter a toll booth.</span></span> <span data-ttu-id="296df-184">V tomto příkladu je nepřetržitý proud událostí, a proto máte toodefine a časové období.</span><span class="sxs-lookup"><span data-stu-id="296df-184">Because this example is a continuous stream of events, you have toodefine a period of time.</span></span> <span data-ttu-id="296df-185">Upravit toobe otázku hello "kolik vozidel zadejte projedou stánek každé tři minuty?"</span><span class="sxs-lookup"><span data-stu-id="296df-185">Modify hello question toobe “How many vehicles enter a toll booth every three minutes?”</span></span> <span data-ttu-id="296df-186">Tento způsob toocount, data se běžně označuje počet přeskakující tooas hello.</span><span class="sxs-lookup"><span data-stu-id="296df-186">This way toocount data is commonly referred tooas hello tumbling count.</span></span>

<span data-ttu-id="296df-187">Podívejte se na hello Stream Analytics dotaz, který odpoví na tuto otázku:</span><span class="sxs-lookup"><span data-stu-id="296df-187">Look at hello Stream Analytics query that answers this question:</span></span>

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

<span data-ttu-id="296df-188">Stream Analytics používá dotazovací jazyk, který je například SQL a přidává několik rozšíření toospecify souvisejících s časem aspektů hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="296df-188">Stream Analytics uses a query language that's like SQL and adds a few extensions toospecify time-related aspects of hello query.</span></span>

<span data-ttu-id="296df-189">Další informace najdete v tématu [Správa času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [Oddílová](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstrukce používaných v dotazu hello z webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="296df-189">For more information, see [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) and [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructs used in hello query from MSDN.</span></span>

<span data-ttu-id="296df-190">Teď, když jste napsali svůj první dotaz služby Stream Analytics, je čas tootest ho.</span><span class="sxs-lookup"><span data-stu-id="296df-190">Now that you have written your first Stream Analytics query, it's time tootest it.</span></span> <span data-ttu-id="296df-191">Použijte hello ukázkových datových souborů umístěné ve složce TollApp aplikace hello následující cesty:</span><span class="sxs-lookup"><span data-stu-id="296df-191">Use hello sample data files located in your TollApp folder in hello following path:</span></span>

<span data-ttu-id="296df-192">.. \TollApp\TollApp\Data</span><span class="sxs-lookup"><span data-stu-id="296df-192">..\TollApp\TollApp\Data</span></span>

<span data-ttu-id="296df-193">Tato složka obsahuje hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="296df-193">This folder contains hello following files:</span></span>
*   <span data-ttu-id="296df-194">Entry.JSON</span><span class="sxs-lookup"><span data-stu-id="296df-194">Entry.json</span></span>
*   <span data-ttu-id="296df-195">Exit.JSON</span><span class="sxs-lookup"><span data-stu-id="296df-195">Exit.json</span></span>
*   <span data-ttu-id="296df-196">Registration.JSON</span><span class="sxs-lookup"><span data-stu-id="296df-196">Registration.json</span></span>

## <a name="count-hello-number-of-vehicles-entering-a-toll-booth"></a><span data-ttu-id="296df-197">Počet hello vozidel zadávání stánek projedou</span><span class="sxs-lookup"><span data-stu-id="296df-197">Count hello number of vehicles entering a toll booth</span></span>
<span data-ttu-id="296df-198">V projektu hello, klikněte dvakrát na **Script.asaql** tooopen hello skript v hello **Editor dotazů**.</span><span class="sxs-lookup"><span data-stu-id="296df-198">In hello project, double-click **Script.asaql** tooopen hello script in hello **Query Editor**.</span></span> <span data-ttu-id="296df-199">Zkopírujte a vložte hello skript v předchozí části hello do editoru hello.</span><span class="sxs-lookup"><span data-stu-id="296df-199">Copy and paste hello script in hello previous section into hello editor.</span></span> <span data-ttu-id="296df-200">Hello Editor dotazů podporuje technologii IntelliSense, barevné zvýrazňování syntaxe a hello chyba značky.</span><span class="sxs-lookup"><span data-stu-id="296df-200">hello Query Editor supports IntelliSense, syntax coloring, and hello error marker.</span></span>

![Editor dotazů](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a><span data-ttu-id="296df-202">Testování dotazů Stream Analytics místně</span><span class="sxs-lookup"><span data-stu-id="296df-202">Test Stream Analytics queries locally</span></span>

1. <span data-ttu-id="296df-203">Klikněte pravým tlačítkem na projekt hello toocompile hello dotazu toosee Pokud je chyba syntaxe a vyberte **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="296df-203">toocompile hello query toosee if there is a syntax error, right-click hello project and select **Build**.</span></span> 

2. <span data-ttu-id="296df-204">toovalidate tento dotaz proti ukázková data, můžete použít místní ukázková data.</span><span class="sxs-lookup"><span data-stu-id="296df-204">toovalidate this query against sample data, you can use local sample data.</span></span> <span data-ttu-id="296df-205">Klikněte pravým tlačítkem na vstup hello a vyberte **přidat místní vstup**.</span><span class="sxs-lookup"><span data-stu-id="296df-205">Right-click hello input, and select **Add local input**.</span></span>

    ![Přidat místní vstup](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. <span data-ttu-id="296df-207">V místním okně hello vyberte hello ukázková data z vaší místní cesta.</span><span class="sxs-lookup"><span data-stu-id="296df-207">In hello pop-up window, select hello sample data from your local path.</span></span> <span data-ttu-id="296df-208">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="296df-208">Click **Save**.</span></span>

    ![Přidejte místní vstupní okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    <span data-ttu-id="296df-210">Soubor s názvem **local_EntryStream.json** se automaticky přidá tooyour vstupy složky.</span><span class="sxs-lookup"><span data-stu-id="296df-210">A file named **local_EntryStream.json** is automatically added tooyour inputs folder.</span></span>

    ![Soubor přidané tooinputs složku](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. <span data-ttu-id="296df-212">V hello **Editor dotazů**, klikněte na tlačítko **spustit místně**.</span><span class="sxs-lookup"><span data-stu-id="296df-212">In hello **Query Editor**, click **Run Locally**.</span></span> <span data-ttu-id="296df-213">Nebo můžete stisknutím klávesy F5 hello.</span><span class="sxs-lookup"><span data-stu-id="296df-213">Or you can press hello F5 key.</span></span>

    ![Spusťte místně](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Místní spuštění výstupu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    <span data-ttu-id="296df-216">Stiskněte klávesu žádný výstup hello klíče tooview v hello **ASA místní spuštění výsledek** oken v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="296df-216">Press any key tooview hello output in hello **ASA Local Run Result** window in Visual Studio.</span></span> 

    ![Okno výsledků ASA místní spuštění](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. <span data-ttu-id="296df-218">Klikněte na tlačítko **otevřít složku výsledek** toocheck hello výstupní soubory oba ve formátu CSV a JSON.</span><span class="sxs-lookup"><span data-stu-id="296df-218">Click **Open Result Folder** toocheck hello output files both in CSV and JSON format.</span></span>

    ![Otevřete složku výsledků výstupu](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-hello-input-data"></a><span data-ttu-id="296df-220">Ukázka hello vstupních dat</span><span class="sxs-lookup"><span data-stu-id="296df-220">Sample hello input data</span></span>
<span data-ttu-id="296df-221">Můžete také ukázková vstupní data z místního souboru tooa vstupního zdroje.</span><span class="sxs-lookup"><span data-stu-id="296df-221">You can also sample input data from input sources tooa local file.</span></span> 
1. <span data-ttu-id="296df-222">Klikněte pravým tlačítkem na hello vstupní konfigurační soubor a vyberte **ukázková Data**.</span><span class="sxs-lookup"><span data-stu-id="296df-222">Right-click hello input config file, and select **Sample Data**.</span></span> 

   ![Ukázková data](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    <span data-ttu-id="296df-224">Teď můžete ukázkové pouze centra událostí nebo služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="296df-224">You can sample only event hub or IoT hub for now.</span></span> <span data-ttu-id="296df-225">Jiných vstupních zdrojů nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="296df-225">Other input sources are not supported.</span></span>

2. <span data-ttu-id="296df-226">V místním okně hello zadejte hello místní cestu použitou toosave hello ukázková data.</span><span class="sxs-lookup"><span data-stu-id="296df-226">In hello pop-up window, enter hello local path used toosave hello sample data.</span></span> <span data-ttu-id="296df-227">Klikněte na tlačítko **ukázka**.</span><span class="sxs-lookup"><span data-stu-id="296df-227">Click **Sample**.</span></span>

    ![Okno ukázkových dat](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    <span data-ttu-id="296df-229">Zobrazí se průběh hello hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="296df-229">You can see hello progress in hello **Output** window.</span></span> 

    ![Výstup – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-tooazure"></a><span data-ttu-id="296df-231">Odeslání tooAzure dotazů Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="296df-231">Submit a Stream Analytics query tooAzure</span></span>
1. <span data-ttu-id="296df-232">V hello **Editor dotazů**, klikněte na tlačítko **odeslání tooAzure** v editor skriptů hello.</span><span class="sxs-lookup"><span data-stu-id="296df-232">In hello **Query Editor**, click **Submit tooAzure** in hello script editor.</span></span>

    ![Odeslání tooAzure](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. <span data-ttu-id="296df-234">Vyberte **vytvoření nové úlohy Azure Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="296df-234">Select **Create a New Azure Stream Analytics Job**.</span></span> <span data-ttu-id="296df-235">Zadejte hello **název úlohy**a vyberte správnou hello **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="296df-235">Enter hello **Job Name**, and select hello correct **Subscription**.</span></span> <span data-ttu-id="296df-236">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="296df-236">Click **Submit**.</span></span>

    ![Odeslání úlohy okna](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a><span data-ttu-id="296df-238">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="296df-238">Start a job</span></span>
<span data-ttu-id="296df-239">Teď, když se vytvoří úlohu, se automaticky otevře zobrazení úloh hello.</span><span class="sxs-lookup"><span data-stu-id="296df-239">Now that your job is created, hello job view is automatically opened.</span></span> 
1. <span data-ttu-id="296df-240">toostart hello úlohy, klikněte na tlačítko hello **zelenou šipku** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="296df-240">toostart hello job, click hello **green arrow** button.</span></span>

    ![Spustit úlohu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. <span data-ttu-id="296df-242">Vyberte hello výchozí nastavení a klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="296df-242">Select hello default setting, and click **Start**.</span></span>
 
    ![Spusťte okno úlohy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    <span data-ttu-id="296df-244">Úloha Hello **stav** změní příliš**systémem**, a **vstup události** a **výstupních událostech** zobrazí.</span><span class="sxs-lookup"><span data-stu-id="296df-244">hello job **Status** changes too**Running**, and **Input Events** and **Output Events** appear.</span></span>

    ![Stav spuštění úlohy souhrnu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-hello-results-in-visual-studio"></a><span data-ttu-id="296df-246">Výsledky kontroly hello v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="296df-246">Check hello results in Visual Studio</span></span>
1. <span data-ttu-id="296df-247">V sadě Visual Studio otevřete **Průzkumníka serveru** a klikněte pravým tlačítkem na hello **TollDataRefJoin** tabulky.</span><span class="sxs-lookup"><span data-stu-id="296df-247">In Visual Studio, open **Server Explorer** and right-click hello **TollDataRefJoin** table.</span></span>
2. <span data-ttu-id="296df-248">Vyberte **zobrazit Data tabulky** toosee hello výstupu úlohy.</span><span class="sxs-lookup"><span data-stu-id="296df-248">Select **Show Table Data** toosee hello output of your job.</span></span>
   
    ![Výběr dat v tabulce zobrazit v Průzkumníku serveru](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-hello-job-metrics"></a><span data-ttu-id="296df-250">Zobrazit hello úlohy metriky</span><span class="sxs-lookup"><span data-stu-id="296df-250">View hello job metrics</span></span>
<span data-ttu-id="296df-251">Statistikami základní úlohy lze nalézt v **úlohy metriky**.</span><span class="sxs-lookup"><span data-stu-id="296df-251">Some basic job statistics can be found in **Job Metrics**.</span></span> 

![Okno metriky úlohy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-hello-job-in-server-explorer"></a><span data-ttu-id="296df-253">Seznam hello úlohy v Průzkumníku serveru</span><span class="sxs-lookup"><span data-stu-id="296df-253">List hello job in Server Explorer</span></span>
<span data-ttu-id="296df-254">V **Průzkumníka serveru**, klikněte na tlačítko **úlohy Stream Analytics** a pak klikněte na **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="296df-254">In **Server Explorer**, click **Stream Analytics Jobs** and then click **Refresh**.</span></span> <span data-ttu-id="296df-255">Hello úlohy se zobrazí pod **úlohy Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="296df-255">hello job appears under **Stream Analytics jobs**.</span></span>

![Úlohy Stream Analytics uvedené v Průzkumníku serveru](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-hello-job-view"></a><span data-ttu-id="296df-257">Zobrazení úloh otevřete hello</span><span class="sxs-lookup"><span data-stu-id="296df-257">Open hello job view</span></span>
<span data-ttu-id="296df-258">zobrazení úloh hello tooopen, rozbalte uzel vaší úlohy a dvakrát klikněte na hello **zobrazení úloh** uzlu.</span><span class="sxs-lookup"><span data-stu-id="296df-258">tooopen hello job view, expand your job node and double-click hello **Job View** node.</span></span>

![Zobrazení úloh uzlu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-tooa-project"></a><span data-ttu-id="296df-260">Export existujícího projektu tooa úlohy</span><span class="sxs-lookup"><span data-stu-id="296df-260">Export an existing job tooa project</span></span>
<span data-ttu-id="296df-261">Existují dva způsoby, můžete exportovat existujícího projektu tooa úlohy.</span><span class="sxs-lookup"><span data-stu-id="296df-261">There are two ways you can export an existing job tooa project.</span></span>

<span data-ttu-id="296df-262">V **Průzkumníka serveru**, klikněte pravým tlačítkem na hello úlohy uzlu v hello **úlohy Stream Analytics** uzel a vyberte možnost **exportovat tooNew Stream Analytics projektu**.</span><span class="sxs-lookup"><span data-stu-id="296df-262">In **Server Explorer**, right-click hello job node in hello **Stream Analytics Jobs** node and select **Export tooNew Stream Analytics Project**.</span></span>

![Export tooNew Stream Analytics projektu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

<span data-ttu-id="296df-264">projekt Hello se generuje ve **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="296df-264">hello project is generated in **Solution Explorer**.</span></span>

![Projektu vygenerované v Průzkumníku řešení](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
<span data-ttu-id="296df-266">Můžete také použít hello zobrazení úloh a klikněte na tlačítko **generovat projektu**.</span><span class="sxs-lookup"><span data-stu-id="296df-266">You also can use hello job view, and click **Generate Project**.</span></span>

![Generování projektu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a><span data-ttu-id="296df-268">Známé problémy a omezení</span><span class="sxs-lookup"><span data-stu-id="296df-268">Known issues and limitations</span></span>
 
- <span data-ttu-id="296df-269">Neexistuje žádná podpora pro výstup Power BI a výstup Azure data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="296df-269">There is no support for Power BI output and Azure Date Lake Store output.</span></span>
- <span data-ttu-id="296df-270">Neexistuje žádná podpora editor pro přidání nebo změna uživatelem definované funkce jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="296df-270">There is no editor support for adding or changing JavaScript user-defined functions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="296df-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="296df-271">Next steps</span></span>
* [<span data-ttu-id="296df-272">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="296df-272">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="296df-273">Začínáme s použitím Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="296df-273">Get started by using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="296df-274">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="296df-274">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="296df-275">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="296df-275">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="296df-276">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="296df-276">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
