---
title: "Použití Azure Stream Analytics Tools pro sadu Visual Studio | Microsoft Docs"
description: "Úvodní kurz pro Azure Stream Analytics Tools pro sadu Visual Studio"
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
ms.openlocfilehash: 618c1055795a75e0ed71dacddba3e076f81f4946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a><span data-ttu-id="d74bb-104">Použití Azure Stream Analytics Tools pro sadu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d74bb-104">Use Azure Stream Analytics Tools for Visual Studio</span></span>
## <a name="introduction"></a><span data-ttu-id="d74bb-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="d74bb-105">Introduction</span></span>
<span data-ttu-id="d74bb-106">V tomto kurzu zjistěte, jak používat Azure Stream Analytics Tools pro sadu Visual Studio k vytvoření, vytváření, testování místně, spravovat a ladit vaše úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="d74bb-106">In this tutorial, you learn how to use Azure Stream Analytics Tools for Visual Studio to create, author, test locally, manage, and debug your Stream Analytics jobs.</span></span> 

<span data-ttu-id="d74bb-107">Po dokončení tohoto kurzu, budete moci:</span><span class="sxs-lookup"><span data-stu-id="d74bb-107">After completing this tutorial, you will be able to:</span></span>
* <span data-ttu-id="d74bb-108">Seznamte se s Stream Analytics Tools pro sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d74bb-108">Familiarize yourself with Stream Analytics Tools for Visual Studio.</span></span>
* <span data-ttu-id="d74bb-109">Nakonfigurujte a nasaďte úloha Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="d74bb-109">Configure and deploy a Stream Analytics job.</span></span>
* <span data-ttu-id="d74bb-110">Otestujte úlohu místně s místní ukázková data.</span><span class="sxs-lookup"><span data-stu-id="d74bb-110">Test your job locally with local sample data.</span></span>
* <span data-ttu-id="d74bb-111">Monitorování použít k řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="d74bb-111">Use monitoring to troubleshoot issues.</span></span>
* <span data-ttu-id="d74bb-112">Exportujte stávající úlohy na projekty.</span><span class="sxs-lookup"><span data-stu-id="d74bb-112">Export existing jobs to projects.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d74bb-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d74bb-113">Prerequisites</span></span>
<span data-ttu-id="d74bb-114">Pro absolvování tohoto kurzu musí být splněné následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="d74bb-114">To complete this tutorial, you need the following prerequisites:</span></span>
* <span data-ttu-id="d74bb-115">Dokončit kroky, které předcházet "Vytvořit úlohu služby Stream Analytics" v [sestavení řešení IoT pomocí služby Stream Analytics kurzu](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="d74bb-115">Finish the steps that precede "Create a Stream Analytics job" in the [Build an IoT solution by using Stream Analytics tutorial](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span></span> 
* <span data-ttu-id="d74bb-116">Pomocí sady Visual Studio 2015, Visual Studio 2013 update 4 nebo Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d74bb-116">Use Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012.</span></span> <span data-ttu-id="d74bb-117">Enterprise (Ultimate nebo Premium), Professional a Community jsou podporované tyto edice.</span><span class="sxs-lookup"><span data-stu-id="d74bb-117">Enterprise (Ultimate/Premium), Professional, and Community editions are supported.</span></span> <span data-ttu-id="d74bb-118">Express edition není podporován.</span><span class="sxs-lookup"><span data-stu-id="d74bb-118">Express edition is not supported.</span></span> <span data-ttu-id="d74bb-119">Visual Studio 2017 není podporována.</span><span class="sxs-lookup"><span data-stu-id="d74bb-119">Visual Studio 2017 is not supported.</span></span> 
* <span data-ttu-id="d74bb-120">Použití sady Azure SDK pro .NET verze 2.7.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d74bb-120">Use the Azure SDK for .NET version 2.7.1 or later.</span></span> <span data-ttu-id="d74bb-121">Nainstalujte ji pomocí [Instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="d74bb-121">Install it by using the [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="d74bb-122">Nainstalujte [Stream Analytics Tools pro sadu Visual Studio](http://aka.ms/asatoolsvs).</span><span class="sxs-lookup"><span data-stu-id="d74bb-122">Install the [Stream Analytics Tools for Visual Studio](http://aka.ms/asatoolsvs).</span></span>

## <a name="create-a-stream-analytics-project"></a><span data-ttu-id="d74bb-123">Vytvoření projektu Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d74bb-123">Create a Stream Analytics project</span></span>
1. <span data-ttu-id="d74bb-124">V sadě Visual Studio, klikněte **soubor** nabídku a vyberte **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-124">In Visual Studio, click the **File** menu and select **New Project**.</span></span> 

2. <span data-ttu-id="d74bb-125">V seznamu šablon na levé straně vyberte **Stream Analytics** a pak klikněte na **Azure Stream Analytics aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-125">In the templates list on the left, select **Stream Analytics** and then click **Azure Stream Analytics Application**.</span></span>

3. <span data-ttu-id="d74bb-126">Zadejte projektu **název**, **umístění**, a **název řešení** jako u jiných projektů.</span><span class="sxs-lookup"><span data-stu-id="d74bb-126">Enter the project **Name**, **Location**, and **Solution name** as you do for other projects.</span></span>

    ![Okno Nový projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    <span data-ttu-id="d74bb-128">A **Projedou** projektu se generuje ve **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-128">A **Toll** project is generated in **Solution Explorer**.</span></span>

    ![Projedou projektu vygenerované v Průzkumníku řešení](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-the-correct-subscription"></a><span data-ttu-id="d74bb-130">Vyberte správné předplatné.</span><span class="sxs-lookup"><span data-stu-id="d74bb-130">Choose the correct subscription</span></span>
1. <span data-ttu-id="d74bb-131">V sadě Visual Studio, klikněte **zobrazení** nabídky a otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-131">In Visual Studio, click the **View** menu and open **Server Explorer**.</span></span>

2. <span data-ttu-id="d74bb-132">Přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="d74bb-132">Sign in with your Azure Account.</span></span> 

## <a name="define-the-input-sources"></a><span data-ttu-id="d74bb-133">Definování vstupních zdrojů</span><span class="sxs-lookup"><span data-stu-id="d74bb-133">Define the input sources</span></span>
1.  <span data-ttu-id="d74bb-134">V **Průzkumníku řešení**, rozbalte **vstupy** uzlu a přejmenujte **Input.json** k **EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-134">In **Solution Explorer**, expand the **Inputs** node and rename **Input.json** to **EntryStream.json**.</span></span> <span data-ttu-id="d74bb-135">Klikněte dvakrát na **EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-135">Double-click **EntryStream.json**.</span></span>
2.  <span data-ttu-id="d74bb-136">**Vstupní Alias** je nyní **EntryStream**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-136">The **Input Alias** is now **EntryStream**.</span></span> <span data-ttu-id="d74bb-137">Vstupní alias se používá ve skriptu dotazu.</span><span class="sxs-lookup"><span data-stu-id="d74bb-137">The input alias is used in the query script.</span></span> 
3.  <span data-ttu-id="d74bb-138">V **typ zdroje**, vyberte **datový proud**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-138">In **Source Type**, select **Data Stream**.</span></span>
4.  <span data-ttu-id="d74bb-139">V **zdroj**, vyberte **centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-139">In **Source**, select **Event Hub**.</span></span>
5.  <span data-ttu-id="d74bb-140">V **Service Bus Namespace**, vyberte **TollData** možnost.</span><span class="sxs-lookup"><span data-stu-id="d74bb-140">In **Service Bus Namespace**, select the **TollData** option.</span></span>
6.  <span data-ttu-id="d74bb-141">V **název centra událostí**, vyberte **položka**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-141">In **Event Hub Name**, select **entry**.</span></span>
7.  <span data-ttu-id="d74bb-142">V **název zásady centra událostí**, vyberte **RootManageSharedAccessKey** (výchozí hodnota).</span><span class="sxs-lookup"><span data-stu-id="d74bb-142">In **Event Hub Policy Name**, select **RootManageSharedAccessKey** (the default value).</span></span>
8.  <span data-ttu-id="d74bb-143">V **formát serializace událostí**, vyberte **Json**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-143">In **Event Serialization Format**, select **Json**.</span></span> 
9.  <span data-ttu-id="d74bb-144">V **kódování**, vyberte **UTF8**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-144">In **Encoding**, select **UTF8**.</span></span> <span data-ttu-id="d74bb-145">Vaše nastavení by mělo vypadat jako na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="d74bb-145">Your settings should look like the following screenshot:</span></span>

    ![Vstupní okna](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. <span data-ttu-id="d74bb-147">Chcete-li dokončit průvodce, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-147">To finish the wizard, click **Save**.</span></span> <span data-ttu-id="d74bb-148">Teď můžete přidat další vstupní zdroj pro vytvoření konec datového proudu.</span><span class="sxs-lookup"><span data-stu-id="d74bb-148">Now you can add another input source to create the exit stream.</span></span> <span data-ttu-id="d74bb-149">Klikněte pravým tlačítkem myši **vstupy** uzel a vyberte možnost **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-149">Right-click the **Inputs** node, and select **New Item**.</span></span>

    ![Nová položka](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. <span data-ttu-id="d74bb-151">V okně vyberte **Azure Stream Analytics vstup**a změňte **název** k **ExitStream.json**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-151">In the window, select **Azure Stream Analytics Input**, and change the **Name** to **ExitStream.json**.</span></span> <span data-ttu-id="d74bb-152">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-152">Click **Add**.</span></span>

    ![Přidat novou položku – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. <span data-ttu-id="d74bb-154">Klikněte dvakrát na **ExitStream.json** v projektu a postupujte podle kroků stejná jako jste to udělali pro vstupní datový proud.</span><span class="sxs-lookup"><span data-stu-id="d74bb-154">Double-click **ExitStream.json** in the project, and follow the same steps as you did for the entry stream.</span></span> <span data-ttu-id="d74bb-155">Nezapomeňte zadat **ukončete** pro **název centra událostí** jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="d74bb-155">Be sure to enter **exit** for the **Event Hub Name** as shown in the following screenshot:</span></span>

    ![Okno ExitStream](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    <span data-ttu-id="d74bb-157">Nyní jste definovali dva vstupní datové proudy:</span><span class="sxs-lookup"><span data-stu-id="d74bb-157">Now you have defined two input streams:</span></span>

    ![Vstupní a výstupní vstupní datové proudy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    <span data-ttu-id="d74bb-159">Dál přidejte referenčního datového vstupu pro objekt blob, který obsahuje car registrační data.</span><span class="sxs-lookup"><span data-stu-id="d74bb-159">Next, add reference data input for the blob file that contains car registration data.</span></span>

13. <span data-ttu-id="d74bb-160">Klikněte pravým tlačítkem myši **vstupy** uzlu v projektu a pak postupujte podle stejné kroky jako jste to udělali pro vstupy datového proudu.</span><span class="sxs-lookup"><span data-stu-id="d74bb-160">Right-click the **Inputs** node in the project, and then follow the same steps as you did for the stream inputs.</span></span> <span data-ttu-id="d74bb-161">V **vstupní Alias**, zadejte **registrace**a v **typ zdroje**, vyberte **referenční data**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-161">In **Input Alias**, enter **Registration**, and in **Source Type**, select **Reference data**.</span></span>

    ![Okno registrace](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. <span data-ttu-id="d74bb-163">V **účet úložiště**, vyberte **tolldata** možnost.</span><span class="sxs-lookup"><span data-stu-id="d74bb-163">In **Storage Account**, select the **tolldata** option.</span></span> <span data-ttu-id="d74bb-164">V **kontejneru**, vyberte **tolldata**a v **vzorek cesty**, zadejte **registration.json**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-164">In **Container**, select **tolldata**, and in **Path Pattern**, enter **registration.json**.</span></span> <span data-ttu-id="d74bb-165">Tento název souboru je velká a malá písmena a musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="d74bb-165">This file name is case sensitive and should be lowercase.</span></span>
15. <span data-ttu-id="d74bb-166">Chcete-li dokončit průvodce, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-166">To finish the wizard, click **Save**.</span></span>

<span data-ttu-id="d74bb-167">Nyní jsou definovány všechny vstupy.</span><span class="sxs-lookup"><span data-stu-id="d74bb-167">Now all the inputs are defined.</span></span>

## <a name="define-the-output"></a><span data-ttu-id="d74bb-168">Definování výstup</span><span class="sxs-lookup"><span data-stu-id="d74bb-168">Define the output</span></span>
1.  <span data-ttu-id="d74bb-169">V **Průzkumníku řešení**, rozbalte **vstupy** uzel a poklikejte na soubor **Output.json**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-169">In **Solution Explorer**, expand the **Inputs** node and double-click **Output.json**.</span></span>

2.  <span data-ttu-id="d74bb-170">V **Alias pro výstup**, zadejte **výstup**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-170">In **Output Alias**, enter **output**.</span></span> 
3.  <span data-ttu-id="d74bb-171">V **jímky**, vyberte **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-171">In **Sink**, select **SQL Database**.</span></span>
4.  <span data-ttu-id="d74bb-172">V **databáze**, vyberte **TollDataDB**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-172">In **Database**, select **TollDataDB**.</span></span>
5.  <span data-ttu-id="d74bb-173">V **uživatelské jméno**, zadejte **tolladmin**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-173">In **User Name**, enter **tolladmin**.</span></span> 
6.  <span data-ttu-id="d74bb-174">V **heslo**, zadejte **123toll!**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-174">In **Password**, enter **123toll!**.</span></span>
7.  <span data-ttu-id="d74bb-175">V **tabulky**, zadejte **TollDataRefJoin**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-175">In **Table**, enter **TollDataRefJoin**.</span></span>
8.  <span data-ttu-id="d74bb-176">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-176">Click **Save**.</span></span>

    ![Výstup – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a><span data-ttu-id="d74bb-178">Vytvořte dotaz služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d74bb-178">Create a Stream Analytics query</span></span>
<span data-ttu-id="d74bb-179">V tomto kurzu se pokusí odpovězte několik obchodní otázky, které se vztahují k dat pro výběr poplatků.</span><span class="sxs-lookup"><span data-stu-id="d74bb-179">This tutorial attempts to answer several business questions that are related to toll data.</span></span> <span data-ttu-id="d74bb-180">Vytvoří také Stream Analytics dotazy, které lze použít v Stream Analytics k poskytování odpovídající odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d74bb-180">It also constructs Stream Analytics queries that can be used in Stream Analytics to provide relevant answers.</span></span>
<span data-ttu-id="d74bb-181">Než začnete vaše první práce Stream Analytics, podíváme se na syntaxi dotazu a jednoduchého scénáře.</span><span class="sxs-lookup"><span data-stu-id="d74bb-181">Before you start your first Stream Analytics job, let’s explore a simple scenario and the query syntax.</span></span>

### <a name="introduction-to-the-stream-analytics-query-language"></a><span data-ttu-id="d74bb-182">Úvod do služby Stream Analytics dotazovací jazyk</span><span class="sxs-lookup"><span data-stu-id="d74bb-182">Introduction to the Stream Analytics query language</span></span>
<span data-ttu-id="d74bb-183">Řekněme, že je potřeba počítat počet vozidel, které projedou stánek zadejte.</span><span class="sxs-lookup"><span data-stu-id="d74bb-183">Let’s say that you need to count the number of vehicles that enter a toll booth.</span></span> <span data-ttu-id="d74bb-184">V tomto příkladu je nepřetržitý proud událostí, a proto je nutné definovat v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="d74bb-184">Because this example is a continuous stream of events, you have to define a period of time.</span></span> <span data-ttu-id="d74bb-185">Upravte dotaz a "jak množství prostředků zadejte projedou stánek každé tři minuty?"</span><span class="sxs-lookup"><span data-stu-id="d74bb-185">Modify the question to be “How many vehicles enter a toll booth every three minutes?”</span></span> <span data-ttu-id="d74bb-186">Tímto způsobem počet dat se obvykle označuje jako počet přeskakující.</span><span class="sxs-lookup"><span data-stu-id="d74bb-186">This way to count data is commonly referred to as the tumbling count.</span></span>

<span data-ttu-id="d74bb-187">Podívejte se na dotaz služby Stream Analytics, na který odpoví na tuto otázku:</span><span class="sxs-lookup"><span data-stu-id="d74bb-187">Look at the Stream Analytics query that answers this question:</span></span>

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

<span data-ttu-id="d74bb-188">Stream Analytics používá dotazovací jazyk, který je například SQL a přidává několik rozšíření k určení souvisejících s časem aspekty dotazu.</span><span class="sxs-lookup"><span data-stu-id="d74bb-188">Stream Analytics uses a query language that's like SQL and adds a few extensions to specify time-related aspects of the query.</span></span>

<span data-ttu-id="d74bb-189">Další informace najdete v tématu [Správa času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [Oddílová](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstrukce použitý v dotazu z webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="d74bb-189">For more information, see [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) and [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructs used in the query from MSDN.</span></span>

<span data-ttu-id="d74bb-190">Teď, když jste napsali svůj první dotaz služby Stream Analytics, je čas to vyzkoušíte.</span><span class="sxs-lookup"><span data-stu-id="d74bb-190">Now that you have written your first Stream Analytics query, it's time to test it.</span></span> <span data-ttu-id="d74bb-191">Pomocí ukázkových datových souborů, umístěný ve složce TollApp v následující cestě:</span><span class="sxs-lookup"><span data-stu-id="d74bb-191">Use the sample data files located in your TollApp folder in the following path:</span></span>

<span data-ttu-id="d74bb-192">.. \TollApp\TollApp\Data</span><span class="sxs-lookup"><span data-stu-id="d74bb-192">..\TollApp\TollApp\Data</span></span>

<span data-ttu-id="d74bb-193">Tato složka obsahuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="d74bb-193">This folder contains the following files:</span></span>
*   <span data-ttu-id="d74bb-194">Entry.JSON</span><span class="sxs-lookup"><span data-stu-id="d74bb-194">Entry.json</span></span>
*   <span data-ttu-id="d74bb-195">Exit.JSON</span><span class="sxs-lookup"><span data-stu-id="d74bb-195">Exit.json</span></span>
*   <span data-ttu-id="d74bb-196">Registration.JSON</span><span class="sxs-lookup"><span data-stu-id="d74bb-196">Registration.json</span></span>

## <a name="count-the-number-of-vehicles-entering-a-toll-booth"></a><span data-ttu-id="d74bb-197">Počet vozidel zadávání stánek projedou</span><span class="sxs-lookup"><span data-stu-id="d74bb-197">Count the number of vehicles entering a toll booth</span></span>
<span data-ttu-id="d74bb-198">V projektu, klikněte dvakrát na **Script.asaql** otevřete skript v **Editor dotazů**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-198">In the project, double-click **Script.asaql** to open the script in the **Query Editor**.</span></span> <span data-ttu-id="d74bb-199">Zkopírujte a vložte skript v předchozí části do editoru.</span><span class="sxs-lookup"><span data-stu-id="d74bb-199">Copy and paste the script in the previous section into the editor.</span></span> <span data-ttu-id="d74bb-200">Editor dotazů podporuje technologii IntelliSense, barevné zvýrazňování syntaxe a chyba značky.</span><span class="sxs-lookup"><span data-stu-id="d74bb-200">The Query Editor supports IntelliSense, syntax coloring, and the error marker.</span></span>

![Editor dotazů](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a><span data-ttu-id="d74bb-202">Testování dotazů Stream Analytics místně</span><span class="sxs-lookup"><span data-stu-id="d74bb-202">Test Stream Analytics queries locally</span></span>

1. <span data-ttu-id="d74bb-203">Kompilace dotazu, který chcete zobrazit, pokud je chyba syntaxe, klikněte pravým tlačítkem na projekt a vyberte **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-203">To compile the query to see if there is a syntax error, right-click the project and select **Build**.</span></span> 

2. <span data-ttu-id="d74bb-204">K ověření tohoto dotazu proti ukázková data, můžete použít místní ukázková data.</span><span class="sxs-lookup"><span data-stu-id="d74bb-204">To validate this query against sample data, you can use local sample data.</span></span> <span data-ttu-id="d74bb-205">Klikněte pravým tlačítkem na vstupu a vyberte **přidat místní vstup**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-205">Right-click the input, and select **Add local input**.</span></span>

    ![Přidat místní vstup](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. <span data-ttu-id="d74bb-207">V místním okně vyberte ukázková data z vaší místní cesta.</span><span class="sxs-lookup"><span data-stu-id="d74bb-207">In the pop-up window, select the sample data from your local path.</span></span> <span data-ttu-id="d74bb-208">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-208">Click **Save**.</span></span>

    ![Přidejte místní vstupní okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    <span data-ttu-id="d74bb-210">Soubor s názvem **local_EntryStream.json** je automaticky přidán do složky vstupy.</span><span class="sxs-lookup"><span data-stu-id="d74bb-210">A file named **local_EntryStream.json** is automatically added to your inputs folder.</span></span>

    ![Soubory přidané do složky vstupy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. <span data-ttu-id="d74bb-212">V **Editor dotazů**, klikněte na tlačítko **spustit místně**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-212">In the **Query Editor**, click **Run Locally**.</span></span> <span data-ttu-id="d74bb-213">Nebo můžete stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="d74bb-213">Or you can press the F5 key.</span></span>

    ![Spusťte místně](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Místní spuštění výstupu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    <span data-ttu-id="d74bb-216">Stisknutím libovolné klávesy zobrazte výstup v **ASA místní spuštění výsledek** oken v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d74bb-216">Press any key to view the output in the **ASA Local Run Result** window in Visual Studio.</span></span> 

    ![Okno výsledků ASA místní spuštění](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. <span data-ttu-id="d74bb-218">Klikněte na tlačítko **otevřít složku výsledek** zkontrolujte výstup souborů, oba ve formátu CSV a JSON.</span><span class="sxs-lookup"><span data-stu-id="d74bb-218">Click **Open Result Folder** to check the output files both in CSV and JSON format.</span></span>

    ![Otevřete složku výsledků výstupu](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-the-input-data"></a><span data-ttu-id="d74bb-220">Ukázka vstupních dat</span><span class="sxs-lookup"><span data-stu-id="d74bb-220">Sample the input data</span></span>
<span data-ttu-id="d74bb-221">Můžete také ukázková vstupní data z vstupního zdroje do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="d74bb-221">You can also sample input data from input sources to a local file.</span></span> 
1. <span data-ttu-id="d74bb-222">Klikněte pravým tlačítkem na vstupní konfigurační soubor a vyberte **ukázková Data**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-222">Right-click the input config file, and select **Sample Data**.</span></span> 

   ![Ukázková data](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    <span data-ttu-id="d74bb-224">Teď můžete ukázkové pouze centra událostí nebo služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d74bb-224">You can sample only event hub or IoT hub for now.</span></span> <span data-ttu-id="d74bb-225">Jiných vstupních zdrojů nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="d74bb-225">Other input sources are not supported.</span></span>

2. <span data-ttu-id="d74bb-226">V místním okně zadejte místní cestu používá pro uložení ukázková data.</span><span class="sxs-lookup"><span data-stu-id="d74bb-226">In the pop-up window, enter the local path used to save the sample data.</span></span> <span data-ttu-id="d74bb-227">Klikněte na tlačítko **ukázka**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-227">Click **Sample**.</span></span>

    ![Okno ukázkových dat](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    <span data-ttu-id="d74bb-229">Můžete sledovat průběh v **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="d74bb-229">You can see the progress in the **Output** window.</span></span> 

    ![Výstup – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-to-azure"></a><span data-ttu-id="d74bb-231">Odeslat dotaz do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d74bb-231">Submit a Stream Analytics query to Azure</span></span>
1. <span data-ttu-id="d74bb-232">V **Editor dotazů**, klikněte na tlačítko **odeslat do Azure** v editoru skriptů.</span><span class="sxs-lookup"><span data-stu-id="d74bb-232">In the **Query Editor**, click **Submit To Azure** in the script editor.</span></span>

    ![Odeslat do Azure](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. <span data-ttu-id="d74bb-234">Vyberte **vytvoření nové úlohy Azure Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-234">Select **Create a New Azure Stream Analytics Job**.</span></span> <span data-ttu-id="d74bb-235">Zadejte **název úlohy**a vyberte správnou **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-235">Enter the **Job Name**, and select the correct **Subscription**.</span></span> <span data-ttu-id="d74bb-236">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-236">Click **Submit**.</span></span>

    ![Odeslání úlohy okna](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a><span data-ttu-id="d74bb-238">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="d74bb-238">Start a job</span></span>
<span data-ttu-id="d74bb-239">Teď, když se vytvoří úlohu, se automaticky otevře zobrazení úloh.</span><span class="sxs-lookup"><span data-stu-id="d74bb-239">Now that your job is created, the job view is automatically opened.</span></span> 
1. <span data-ttu-id="d74bb-240">Chcete-li spustit úlohu, klikněte na tlačítko **zelenou šipku** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d74bb-240">To start the job, click the **green arrow** button.</span></span>

    ![Spustit úlohu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. <span data-ttu-id="d74bb-242">Vyberte výchozí nastavení a klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-242">Select the default setting, and click **Start**.</span></span>
 
    ![Spusťte okno úlohy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    <span data-ttu-id="d74bb-244">Úloha **stav** změny **systémem**, a **vstup události** a **výstupních událostech** zobrazí.</span><span class="sxs-lookup"><span data-stu-id="d74bb-244">The job **Status** changes to **Running**, and **Input Events** and **Output Events** appear.</span></span>

    ![Stav spuštění úlohy souhrnu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-the-results-in-visual-studio"></a><span data-ttu-id="d74bb-246">Zkontrolujte výsledky v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d74bb-246">Check the results in Visual Studio</span></span>
1. <span data-ttu-id="d74bb-247">V sadě Visual Studio otevřete **Průzkumníka serveru** a klikněte pravým tlačítkem **TollDataRefJoin** tabulky.</span><span class="sxs-lookup"><span data-stu-id="d74bb-247">In Visual Studio, open **Server Explorer** and right-click the **TollDataRefJoin** table.</span></span>
2. <span data-ttu-id="d74bb-248">Vyberte **zobrazit Data tabulky** na zobrazení výstupu úlohy.</span><span class="sxs-lookup"><span data-stu-id="d74bb-248">Select **Show Table Data** to see the output of your job.</span></span>
   
    ![Výběr dat v tabulce zobrazit v Průzkumníku serveru](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-the-job-metrics"></a><span data-ttu-id="d74bb-250">Zobrazení metriky úlohy</span><span class="sxs-lookup"><span data-stu-id="d74bb-250">View the job metrics</span></span>
<span data-ttu-id="d74bb-251">Statistikami základní úlohy lze nalézt v **úlohy metriky**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-251">Some basic job statistics can be found in **Job Metrics**.</span></span> 

![Okno metriky úlohy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-the-job-in-server-explorer"></a><span data-ttu-id="d74bb-253">Seznam úloh v Průzkumníku serveru</span><span class="sxs-lookup"><span data-stu-id="d74bb-253">List the job in Server Explorer</span></span>
<span data-ttu-id="d74bb-254">V **Průzkumníka serveru**, klikněte na tlačítko **úlohy Stream Analytics** a pak klikněte na **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-254">In **Server Explorer**, click **Stream Analytics Jobs** and then click **Refresh**.</span></span> <span data-ttu-id="d74bb-255">Úloha se zobrazí pod **úlohy Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-255">The job appears under **Stream Analytics jobs**.</span></span>

![Úlohy Stream Analytics uvedené v Průzkumníku serveru](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-the-job-view"></a><span data-ttu-id="d74bb-257">Otevřete zobrazení úloh</span><span class="sxs-lookup"><span data-stu-id="d74bb-257">Open the job view</span></span>
<span data-ttu-id="d74bb-258">K otevření zobrazení úlohy, rozbalte uzel vaší úlohy a dvakrát klikněte **zobrazení úloh** uzlu.</span><span class="sxs-lookup"><span data-stu-id="d74bb-258">To open the job view, expand your job node and double-click the **Job View** node.</span></span>

![Zobrazení úloh uzlu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-to-a-project"></a><span data-ttu-id="d74bb-260">Exportovat stávající úloze do projektu</span><span class="sxs-lookup"><span data-stu-id="d74bb-260">Export an existing job to a project</span></span>
<span data-ttu-id="d74bb-261">Existují dva způsoby stávající úloze můžete exportovat do projektu.</span><span class="sxs-lookup"><span data-stu-id="d74bb-261">There are two ways you can export an existing job to a project.</span></span>

<span data-ttu-id="d74bb-262">V **Průzkumníka serveru**, klikněte pravým tlačítkem v uzlu úlohy **úlohy Stream Analytics** uzel a vyberte možnost **exportovat do nového projektu Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-262">In **Server Explorer**, right-click the job node in the **Stream Analytics Jobs** node and select **Export to New Stream Analytics Project**.</span></span>

![Exportovat do nového projektu Stream Analytics](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

<span data-ttu-id="d74bb-264">Projekt se generuje ve **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-264">The project is generated in **Solution Explorer**.</span></span>

![Projektu vygenerované v Průzkumníku řešení](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
<span data-ttu-id="d74bb-266">Také můžete použít zobrazení úloh a klikněte na tlačítko **generovat projektu**.</span><span class="sxs-lookup"><span data-stu-id="d74bb-266">You also can use the job view, and click **Generate Project**.</span></span>

![Generování projektu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a><span data-ttu-id="d74bb-268">Známé problémy a omezení</span><span class="sxs-lookup"><span data-stu-id="d74bb-268">Known issues and limitations</span></span>
 
- <span data-ttu-id="d74bb-269">Neexistuje žádná podpora pro výstup Power BI a výstup Azure data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="d74bb-269">There is no support for Power BI output and Azure Date Lake Store output.</span></span>
- <span data-ttu-id="d74bb-270">Neexistuje žádná podpora editor pro přidání nebo změna uživatelem definované funkce jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d74bb-270">There is no editor support for adding or changing JavaScript user-defined functions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d74bb-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d74bb-271">Next steps</span></span>
* [<span data-ttu-id="d74bb-272">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d74bb-272">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="d74bb-273">Začínáme s použitím Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d74bb-273">Get started by using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="d74bb-274">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d74bb-274">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="d74bb-275">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="d74bb-275">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="d74bb-276">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d74bb-276">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
