---
title: "Řídicí panel Power BI pro vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti Cortana Intelligence získat přehledy v reálném čase a prediktivní na vehicle stavu a řídí zvyklosti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: f880aceb1657ffdfe909b73f175b9673d9ab02cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a><span data-ttu-id="7f2e4-103">Vehicle telemetrie analytics řešení šablony řídicí panel Power BI pokyny pro instalaci</span><span class="sxs-lookup"><span data-stu-id="7f2e4-103">Vehicle telemetry analytics solution template Power BI Dashboard setup instructions</span></span>
<span data-ttu-id="7f2e4-104">To **nabídky** odkazy na kapitoly v této scénářem.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-104">This **menu** links to the chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="7f2e4-105">Jak dealerům car, automobilů výrobců a pojištění společností můžete využít možnosti Cortana Intelligence a získáte přehled o v reálném čase a prediktivní na vehicle stavu a řízení zvyklosti na jednotce vylepšení v oblasti zákazníka zkušenosti, výzkumu a vývoje a marketingových kampaní, umožňující prezentovat řešení Vehicle Telemetrie analýzy.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-105">The Vehicle Telemetry Analytics solution showcases how car dealerships, automobile manufacturers and insurance companies can leverage the capabilities of Cortana Intelligence to gain real-time and predictive insights on vehicle health and driving habits to drive improvements in the area of customer experience, R&D and marketing campaigns.</span></span> <span data-ttu-id="7f2e4-106">Tento dokument obsahuje podrobné pokyny, jak můžete nakonfigurovat Power BI sestavy a řídicí panel po je řešení nasazeno v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-106">This document contains step by step instructions on how you can configure the Power BI reports and dashboard once the solution is deployed in your subscription.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7f2e4-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f2e4-107">Prerequisites</span></span>
1. <span data-ttu-id="7f2e4-108">Nasazení [Telemetrie Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) řešení</span><span class="sxs-lookup"><span data-stu-id="7f2e4-108">Deploy the [Telemetry Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) solution</span></span>  
2. [<span data-ttu-id="7f2e4-109">Nainstalujte Microsoft Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="7f2e4-109">Install Microsoft Power BI Desktop</span></span>](http://www.microsoft.com/download/details.aspx?id=45331)
3. <span data-ttu-id="7f2e4-110">[Předplatné](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7f2e4-110">An [Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="7f2e4-111">Pokud nemáte předplatné Azure, Začínáme s Azure bezplatné předplatné</span><span class="sxs-lookup"><span data-stu-id="7f2e4-111">If you don't have an Azure subscription, get started with Azure free subscription</span></span>
4. <span data-ttu-id="7f2e4-112">Účet Microsoft Power BI</span><span class="sxs-lookup"><span data-stu-id="7f2e4-112">Microsoft Power BI account</span></span>

## <a name="cortana-intelligence-suite-components"></a><span data-ttu-id="7f2e4-113">Cortana Intelligence Suite součásti</span><span class="sxs-lookup"><span data-stu-id="7f2e4-113">Cortana Intelligence Suite Components</span></span>
<span data-ttu-id="7f2e4-114">Jako součást šablona řešení Analytics Telemetrie Vehicle jsou tyto služby Cortana Intelligence nasazené v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-114">As part of the Vehicle Telemetry Analytics solution template, the following Cortana Intelligence services are deployed in your subscription.</span></span>

* <span data-ttu-id="7f2e4-115">**Centra událostí** pro příjem miliony událostí vehicle telemetrická data do Azure.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-115">**Event Hub** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="7f2e4-116">**Stream Analytics** pro získání přehledy v reálném čase na vehicle stavu a přetrvává tato data do dlouhodobého úložiště pro širší batch analýzu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-116">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="7f2e4-117">**Strojového učení** pro zjišťování anomálií v reálném čase a dávkové zpracování a získáte přehled o prediktivní.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-117">**Machine Learning** for anomaly detection in real-time and batch processing to gain predictive insights.</span></span>
* <span data-ttu-id="7f2e4-118">**HDInsight** je využít k transformaci dat ve velkém měřítku</span><span class="sxs-lookup"><span data-stu-id="7f2e4-118">**HDInsight** is leveraged to transform data at scale</span></span>
* <span data-ttu-id="7f2e4-119">**Objekt pro vytváření dat** zpracovává orchestration, plánování, správy prostředků a monitorování dávkové zpracování kanálu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-119">**Data Factory** handles orchestration, scheduling, resource management and monitoring of the batch processing pipeline.</span></span>

<span data-ttu-id="7f2e4-120">**Power BI** toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-120">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="7f2e4-121">Toto řešení využívá dvou různých zdrojů dat.: **Simulated vehicle signály a diagnostiky datovou sadu** a **vehicle katalogu**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-121">The solution uses two different data sources: **Simulated vehicle signals and diagnostic dataset** and **vehicle catalog**.</span></span>

<span data-ttu-id="7f2e4-122">Vehicle telematika jsme je součástí tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-122">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="7f2e4-123">Vysílá diagnostické informace a signály odpovídající stav nástroj a řídí vzor k danému bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-123">It emits diagnostic information and signals corresponding to the state of the vehicle and driving pattern at a given point in time.</span></span> 

<span data-ttu-id="7f2e4-124">Vehicle katalog je obsahující VIN odkaz na datovou sadu pro mapování modelu</span><span class="sxs-lookup"><span data-stu-id="7f2e4-124">The Vehicle Catalog is a reference dataset containing VIN to model mapping</span></span>

## <a name="power-bi-dashboard-preparation"></a><span data-ttu-id="7f2e4-125">Příprava Power BI řídicí panel</span><span class="sxs-lookup"><span data-stu-id="7f2e4-125">Power BI Dashboard Preparation</span></span>
### <a name="setup-power-bi-real-time-dashboard"></a><span data-ttu-id="7f2e4-126">Instalační program v reálném čase řídicí panel Power BI</span><span class="sxs-lookup"><span data-stu-id="7f2e4-126">Setup Power BI Real-Time Dashboard</span></span>

<span data-ttu-id="7f2e4-127">**Spusťte aplikaci v reálném čase řídicí panel** po dokončení nasazení, postupujte podle pokynů operaci ruční</span><span class="sxs-lookup"><span data-stu-id="7f2e4-127">**Start the real-time dashboard application** Once the deployment is completed, you should follow the Manual Operation Instructions</span></span>

* <span data-ttu-id="7f2e4-128">Stažení aplikace řídicího panelu v reálném čase RealtimeDashboardApp.zip a rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-128">Download real-time dashboard application RealtimeDashboardApp.zip, and unzip it.</span></span>
*  <span data-ttu-id="7f2e4-129">V rozbalené složce otevřete konfigurační soubor aplikace RealtimeDashboardApp.exe.config, nahraďte appSettings pro Eventhub, úložiště objektů Blob a ML připojení služby s hodnotami v pokynech operaci ruční a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-129">In the unzipped folder, open app config file 'RealtimeDashboardApp.exe.config', replace appSettings for Eventhub, Blob Storage, and ML service connections with the values in the Manual Operation Instructions, and save your changes.</span></span>
* <span data-ttu-id="7f2e4-130">Spusťte aplikaci RealtimeDashboardApp.exe.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-130">Run application RealtimeDashboardApp.exe.</span></span> <span data-ttu-id="7f2e4-131">Okno přihlášení bude překryvné, zadejte svoje platné přihlašovací údaje PowerBI a klikněte na **přijmout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-131">A login window will pop up, provide your valid PowerBI credentials and click the **Accept** button.</span></span> <span data-ttu-id="7f2e4-132">Aplikace se pak spustí ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-132">Then the app will start to run.</span></span>

   ![Přihlaste se do Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI Dashboard oprávnění](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* <span data-ttu-id="7f2e4-135">Přihlašovací údaje pro web PowerBI a vytvořit řídicí panel v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-135">Login to PowerBI website, and create real-time dashboard.</span></span>

<span data-ttu-id="7f2e4-136">Nyní jste připraveni ke konfiguraci řídicí panel Power BI s bohatých vizualizací k získání v reálném čase a zvyklosti prediktivní Statistika na vehicle stavu a řídí.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-136">Now, you are ready to configure the Power BI dashboard with rich visualizations to gain real-time and predictive insights on vehicle health and driving habits.</span></span> <span data-ttu-id="7f2e4-137">Trvá přibližně 45 minut za hodinu vytváření všech sestav a konfiguraci řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-137">It takes about 45 minutes to an hour to create all the reports and configure the dashboard.</span></span> 

### <a name="configure-power-bi-reports"></a><span data-ttu-id="7f2e4-138">Konfigurace sestav Power BI</span><span class="sxs-lookup"><span data-stu-id="7f2e4-138">Configure Power BI reports</span></span>
<span data-ttu-id="7f2e4-139">V reálném čase sestavy a řídicí panel trvat asi 30 – 45 minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-139">The real-time reports and the dashboard take about 30-45 minutes to complete.</span></span> <span data-ttu-id="7f2e4-140">Přejděte do [http://powerbi.com](http://powerbi.com) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-140">Browse to [http://powerbi.com](http://powerbi.com) and login.</span></span>

![Přihlaste se do Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

<span data-ttu-id="7f2e4-142">Nová datová sada se generuje ve službě Power BI.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-142">A new dataset is generated in Power BI.</span></span> <span data-ttu-id="7f2e4-143">Klikněte **ConnectedCarsRealtime** datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-143">Click the **ConnectedCarsRealtime** dataset.</span></span>

![Vybráno připojené v reálném čase datové sady automobilů](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

<span data-ttu-id="7f2e4-145">Uložit pomocí prázdné sestavy **Ctrl + s**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-145">Save the blank report using **Ctrl + s**.</span></span>

![Uložit prázdné sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

<span data-ttu-id="7f2e4-147">Zadejte název sestavy *Vehicle Telemetrie analýzy v reálném čase - sestavy*.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-147">Provide report name *Vehicle Telemetry Analytics Real-time - Reports*.</span></span>

![Zadejte název sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a><span data-ttu-id="7f2e4-149">Sestavy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="7f2e4-149">Real-time reports</span></span>
<span data-ttu-id="7f2e4-150">V tomto řešení jsou tři sestavy v reálném čase:</span><span class="sxs-lookup"><span data-stu-id="7f2e4-150">There are three real-time reports in this solution:</span></span>

1. <span data-ttu-id="7f2e4-151">Vozidel v operaci</span><span class="sxs-lookup"><span data-stu-id="7f2e4-151">Vehicles in operation</span></span>
2. <span data-ttu-id="7f2e4-152">Vozidel údržby</span><span class="sxs-lookup"><span data-stu-id="7f2e4-152">Vehicles Requiring Maintenance</span></span>
3. <span data-ttu-id="7f2e4-153">Statistiky vozidel stavu</span><span class="sxs-lookup"><span data-stu-id="7f2e4-153">Vehicles Health Statistics</span></span>

<span data-ttu-id="7f2e4-154">Můžete nakonfigurovat všechny tři v reálném čase sestavy nebo zastavení po jakékoli fázi a pokračujte v další části Konfigurace batch sestavy.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-154">You can choose to configure all the three real-time reports or stop after any stage and proceed to the next section of configuring the batch reports.</span></span> <span data-ttu-id="7f2e4-155">Doporučujeme, abyste vytvořili všechny tři sestav můžete znázorňovat úplné statistiky v reálném čase cesty řešení.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-155">We recommend you to create all the three reports to visualize the full insights of the real-time path of the solution.</span></span>  

### <a name="1-vehicles-in-operation"></a><span data-ttu-id="7f2e4-156">1. Vozidel v operaci</span><span class="sxs-lookup"><span data-stu-id="7f2e4-156">1. Vehicles in operation</span></span>
<span data-ttu-id="7f2e4-157">Klikněte dvakrát na **stránka 1** a přejmenujte jej na "Vozidel v operaci"</span><span class="sxs-lookup"><span data-stu-id="7f2e4-157">Double-click **Page 1** and rename it to “Vehicles in operation”</span></span>  
    <span data-ttu-id="7f2e4-158">![Připojených vozidel v operaci aut-](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-158">![Connected Cars - Vehicles in operation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span></span>  

<span data-ttu-id="7f2e4-159">Vyberte **vin** pole z **pole** a vyberte typ vizualizace jako **"Kartou"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-159">Select **vin** field from **Fields** and choose visualization type as **“Card”**.</span></span>  

<span data-ttu-id="7f2e4-160">Karta vizualizace se vytvoří, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-160">Card visualization is created as shown in figure.</span></span>  
    ![Připojené aut - vyberte vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

<span data-ttu-id="7f2e4-162">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-162">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="7f2e4-163">Vyberte **města** a **vin** z pole.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-163">Select **City** and **vin** from fields.</span></span> <span data-ttu-id="7f2e4-164">Změnit vizualizace k **"Map"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-164">Change visualization to **“Map”**.</span></span> <span data-ttu-id="7f2e4-165">Přetáhněte **vin** v oblasti hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-165">Drag **vin** in values area.</span></span> <span data-ttu-id="7f2e4-166">Přetáhněte **města** z pole **legendy** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-166">Drag **city** from fields to **Legend** area.</span></span>   
    <span data-ttu-id="7f2e4-167">![Připojené aut – karta vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-167">![Connected Cars - Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span></span>

<span data-ttu-id="7f2e4-168">Vyberte **formátu** část z **vizualizace**, klikněte na tlačítko **název** a změňte **Text** k **"Vozidel v operaci podle města"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-168">Select **format** section from **Visualizations**, click **Title** and change the **Text** to **“Vehicles in operation by city”**.</span></span>  
    <span data-ttu-id="7f2e4-169">![Připojených vozidel v operaci podle města aut-](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-169">![Connected Cars - Vehicles in operation by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span></span>   

<span data-ttu-id="7f2e4-170">Poslední vizualizace vypadá, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-170">Final visualization looks as shown in figure.</span></span>    
    ![Připojené aut - poslední vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

<span data-ttu-id="7f2e4-172">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-172">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="7f2e4-173">Vyberte **města** a **vin**, změnit typ vizualizace na **skupinový sloupcový graf**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-173">Select **City** and **vin**, change visualization type to **Clustered Column Chart**.</span></span> <span data-ttu-id="7f2e4-174">Ujistěte se, **města** pole **osy oblasti** a **vin** v **hodnotu oblasti**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-174">Ensure **City** field in **Axis area** and **vin** in **Value area**</span></span>  

<span data-ttu-id="7f2e4-175">Graf řazení podle **"Počet vin"**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-175">Sort chart by **“Count of vin”**</span></span>  
    <span data-ttu-id="7f2e4-176">![Připojené aut - počet vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-176">![Connected Cars - Count of vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span></span>  

<span data-ttu-id="7f2e4-177">Změna grafu **název** k **"Vozidel v operaci podle města"**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-177">Change chart **Title** to **“Vehicles in operation by city”**</span></span>  

<span data-ttu-id="7f2e4-178">Klikněte na tlačítko **formátu** části a pak vyberte **Data barvy**, klikněte na tlačítko **"Na"** k **Zobrazit vše**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-178">Click the **Format** section, then select **Data Colors**,  Click the **“On”** to **Show All**</span></span>  
    <span data-ttu-id="7f2e4-179">![Připojené aut – zobrazit všechny Data barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-179">![Connected Cars - Show all Data Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span></span>  

<span data-ttu-id="7f2e4-180">Změníte barvu jednotlivých města kliknutím na ikonu barev.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-180">Change the color of individual city by clicking on color icon.</span></span>  
    ![Připojené aut - Změna barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

<span data-ttu-id="7f2e4-182">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-182">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="7f2e4-183">Vyberte **skupinový sloupcový graf** vizualizace z vizualizace, přetáhněte **města** pole **osy** oblasti **modelu** v **legendy** oblasti a **vin** v **hodnotu** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-183">Select **Clustered Column Chart** visualization from visualizations, drag **city** field in **Axis** area, **Model** in **Legend** area and **vin** in **Value** area.</span></span>  
    <span data-ttu-id="7f2e4-184">![Připojené aut - skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-184">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span></span>  
    <span data-ttu-id="7f2e4-185">![Připojené aut - vykreslování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-185">![Connected Cars - Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span></span>

<span data-ttu-id="7f2e4-186">Změna uspořádání všechny vizualizace na této stránce, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-186">Rearrange all visualization on this page as shown in figure.</span></span>  
    ![Připojené aut - vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

<span data-ttu-id="7f2e4-188">Úspěšně jste nakonfigurovali v reálném čase sestavy "Vozidel v operaci".</span><span class="sxs-lookup"><span data-stu-id="7f2e4-188">You have successfully configured the “Vehicles in operation” real-time report.</span></span> <span data-ttu-id="7f2e4-189">Můžete přejít k vytvoření další sestavy v reálném čase nebo zde zastavte a konfigurujte řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-189">You can proceed to create the next real-time report or stop here and configure the dashboard.</span></span> 

### <a name="2-vehicles-requiring-maintenance"></a><span data-ttu-id="7f2e4-190">2. Vozidel údržby</span><span class="sxs-lookup"><span data-stu-id="7f2e4-190">2. Vehicles Requiring Maintenance</span></span>
<span data-ttu-id="7f2e4-191">Klikněte na tlačítko ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) přidat nové sestavy, přejmenujte ji na **"Vozidel nutnosti Údržba"**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-191">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) to add a new report, rename it to **“Vehicles Requiring Maintenance”**</span></span>

![Připojené aut - vozidel údržby](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

<span data-ttu-id="7f2e4-193">Vyberte **vin** pole a změnit typ vizualizace na **karty**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-193">Select **vin** field and change visualization type to **Card**.</span></span>  
    <span data-ttu-id="7f2e4-194">![Připojené aut – karta Vin vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-194">![Connected Cars - Vin Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span></span>  

<span data-ttu-id="7f2e4-195">Máme pole s názvem "MaintenanceLabel" v datové sadě.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-195">We have a field named “MaintenanceLabel” In the dataset.</span></span> <span data-ttu-id="7f2e4-196">Toto pole může mít hodnotu "0" nebo "1"."</span><span class="sxs-lookup"><span data-stu-id="7f2e4-196">This field can have a value of “0” or “1”.”</span></span> <span data-ttu-id="7f2e4-197">Je nastavena podle modelu Azure Machine Learning zřízené v rámci řešení a integraci s cestou v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-197">It is set by the Azure Machine Learning model provisioned as part of solution and integrated with the real-time path.</span></span> <span data-ttu-id="7f2e4-198">Hodnotu "1" označuje, že že vehicle vyžaduje údržbu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-198">The value “1” indicates a vehicle requires maintenance.</span></span> 

<span data-ttu-id="7f2e4-199">Chcete-li přidat **úrovně stránky** filtr zobrazující vozidel data, která jsou nutnosti údržby:</span><span class="sxs-lookup"><span data-stu-id="7f2e4-199">To add a **Page Level** filter for showing vehicles data, which are requiring maintenance:</span></span> 

1. <span data-ttu-id="7f2e4-200">Přetáhněte **"MaintenanceLabel"** pole do **stránky úroveň filtry**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-200">Drag the **“MaintenanceLabel”** field into **Page Level Filters**.</span></span>  
   <span data-ttu-id="7f2e4-201">![Připojené aut - filtry úrovně stránky](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-201">![Connected Cars - Page Level Filters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span></span>  
2. <span data-ttu-id="7f2e4-202">Klikněte na tlačítko **základní filtrování** nabídky nachází v dolní části MaintenanceLabel stránky úroveň filtru.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-202">Click **Basic Filtering** menu present at bottom of MaintenanceLabel Page Level Filter.</span></span>  
   <span data-ttu-id="7f2e4-203">![Připojené aut – základní filtrování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-203">![Connected Cars - Basic Filtering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span></span>  
3. <span data-ttu-id="7f2e4-204">Nastavte ho na hodnotu filtru **"1"**  </span><span class="sxs-lookup"><span data-stu-id="7f2e4-204">Set its filter value to **“1”**  </span></span>  
   <span data-ttu-id="7f2e4-205">![Připojené aut - hodnota filtru](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-205">![Connected Cars - Filter Value](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span></span>  

<span data-ttu-id="7f2e4-206">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-206">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="7f2e4-207">Vyberte **skupinový sloupcový graf** z vizualizace</span><span class="sxs-lookup"><span data-stu-id="7f2e4-207">Select **Clustered Column Chart** from visualizations</span></span>  
<span data-ttu-id="7f2e4-208">![Připojené aut – karta Vind vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-208">![Connected Cars - Vind Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span></span>  
<span data-ttu-id="7f2e4-209">![Připojené aut - skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-209">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span></span>

<span data-ttu-id="7f2e4-210">Přetáhněte pole **modelu** do **osy** oblasti **Vin** k **hodnotu** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-210">Drag field **Model** into **Axis** area, **Vin** to **Value** area.</span></span> <span data-ttu-id="7f2e4-211">Seřaďte vizualizace podle **počet vin**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-211">Then sort visualization by **Count of vin**.</span></span>  <span data-ttu-id="7f2e4-212">Změna grafu **název** k **"Vozidel nutnosti údržby modelem"**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-212">Change chart **Title** to **“Vehicles requiring maintenance by model”**</span></span>  

<span data-ttu-id="7f2e4-213">Přetáhněte **vin** polí do **sytost barev** u **pole** ![pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) části **vizualizace** karta</span><span class="sxs-lookup"><span data-stu-id="7f2e4-213">Drag **vin** fields into **Color Saturation** present at **Fields** ![Fields](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) section of **Visualization** tab</span></span>  
<span data-ttu-id="7f2e4-214">![Připojené aut - sytost barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-214">![Connected Cars - Color Saturation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span></span>  

<span data-ttu-id="7f2e4-215">Změna **Data barvy** v vizualizace z **formát** části</span><span class="sxs-lookup"><span data-stu-id="7f2e4-215">Change **Data Colors** in visualizations from **Format** section</span></span>  
<span data-ttu-id="7f2e4-216">Změnit barvu minimální: **F2C812**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-216">Change Minimum color to: **F2C812**</span></span>  
<span data-ttu-id="7f2e4-217">Změnit barvu maximální: **FF6300**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-217">Change Maximum color to: **FF6300**</span></span>  
<span data-ttu-id="7f2e4-218">![Připojené aut - změny barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-218">![Connected Cars - Color Changes](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span></span>  
<span data-ttu-id="7f2e4-219">![Připojené aut - nové Vizualizační barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-219">![Connected Cars - New Visualization Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span></span>  

<span data-ttu-id="7f2e4-220">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-220">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="7f2e4-221">Vyberte **Clustered sloupcový graf** z vizualizace, přetáhněte **vin** pole do **hodnotu** oblasti, přetáhněte **města** pole do **osy** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-221">Select **Clustered column chart** from visualizations, drag **vin** field into **Value** area, drag **City** field into **Axis** area.</span></span> <span data-ttu-id="7f2e4-222">Graf řazení podle **"Počet vin"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-222">Sort chart by **“Count of vin”**.</span></span> <span data-ttu-id="7f2e4-223">Změna grafu **název** k **"Vozidel údržby podle města"** </span><span class="sxs-lookup"><span data-stu-id="7f2e4-223">Change chart **Title** to **“Vehicles requiring maintenance by city”** </span></span>  
<span data-ttu-id="7f2e4-224">![Připojené aut - vozidel údržby podle města](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-224">![Connected Cars - Vehicles requiring maintenance by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span></span>  

<span data-ttu-id="7f2e4-225">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-225">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="7f2e4-226">Vyberte **více řádků karty** vizualizace z vizualizace, přetáhněte **modelu** a **vin** do **pole** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-226">Select **Multi-Row Card** visualization from visualizations, drag **Model** and **vin** into the **Fields** area.</span></span>  
<span data-ttu-id="7f2e4-227">![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-227">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span></span>    

<span data-ttu-id="7f2e4-228">Změna uspořádání všechny vizualizaci, poslední sestava vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="7f2e4-228">Rearranging all of the visualization, the final report looks as follows:</span></span>  
![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

<span data-ttu-id="7f2e4-230">Úspěšně jste nakonfigurovali v reálném čase sestavy "Vozidel nutnosti údržby".</span><span class="sxs-lookup"><span data-stu-id="7f2e4-230">You have successfully configured the “Vehicles Requiring Maintenance” real-time report.</span></span> <span data-ttu-id="7f2e4-231">Můžete přejít k vytvoření další sestavy v reálném čase nebo zde zastavte a konfigurujte řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-231">You can proceed to create the next real-time report or stop here and configure the dashboard.</span></span> 

### <a name="3-vehicles-health-statistics"></a><span data-ttu-id="7f2e4-232">3. Statistiky vozidel stavu</span><span class="sxs-lookup"><span data-stu-id="7f2e4-232">3. Vehicles Health Statistics</span></span>
<span data-ttu-id="7f2e4-233">Klikněte na tlačítko ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) Pokud chcete přidat novou sestavu, přejmenujte ji na **"Vozidel stavu statistika"**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-233">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) to add new report, rename it to **“Vehicles Health Statistics”**</span></span>  

<span data-ttu-id="7f2e4-234">Vyberte **měřidla** vizualizace z vizualizace, přetáhněte **rychlost** pole do **hodnotu, hodnotu minimální, maximální hodnota** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-234">Select **Gauge** visualization from visualizations, then drag the **Speed** field into **Value, Minimum Value, Maximum Value** areas.</span></span>  
<span data-ttu-id="7f2e4-235">![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span><span class="sxs-lookup"><span data-stu-id="7f2e4-235">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span></span>  

<span data-ttu-id="7f2e4-236">Změňte výchozí agregace **rychlost** v **hodnotu oblasti** k **průměrná**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-236">Change the default aggregation of **speed** in **Value area** to **Average**</span></span> 

<span data-ttu-id="7f2e4-237">Změňte výchozí agregace **rychlost** v **minimální oblasti** k **minimální**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-237">Change the default aggregation of **speed** in **Minimum area** to **Minimum**</span></span>

<span data-ttu-id="7f2e4-238">Změňte výchozí agregace **rychlost** v **maximální oblasti** k **maximální**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-238">Change the default aggregation of **speed** in **Maximum area** to **Maximum**</span></span>

![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

<span data-ttu-id="7f2e4-240">Přejmenujte **název měřidla** k **"Průměrná rychlost"**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-240">Rename the **Gauge Title** to **“Average speed”**</span></span> 

![Připojené aut - měřidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

<span data-ttu-id="7f2e4-242">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-242">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="7f2e4-243">Podobně přidat **měřidla** pro **průměrná těžba ropy modul**, **průměrná paliva**, a **průměrná modul mírného**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-243">Similarly add a **Gauge** for **average engine oil**, **average fuel**, and **average engine temperate**.</span></span>  

<span data-ttu-id="7f2e4-244">Změňte výchozí agregace polí v každé měřidla dle výše uvedených kroků v **"Průměrná rychlost"** měřidla.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-244">Change the default aggregation of fields in each gauge as per above steps in **“Average speed”** gauge.</span></span>

![Připojené aut - měřidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

<span data-ttu-id="7f2e4-246">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-246">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="7f2e4-247">Vyberte **spojnicový a skupinový sloupcový graf** z vizualizace, přetáhněte **města** pole do **sdílené osy**, přetáhněte **rychlost**, **pole tirepressure a engineoil** do **hodnoty ve sloupcích** oblasti, změnit jejich typ agregace na **průměrná**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-247">Select **Line and Clustered Column Chart** from visualizations, then drag **City** field into **Shared Axis**, drag **speed**, **tirepressure and engineoil fields** into **Column Values** area, change their aggregation type to **Average**.</span></span> 

<span data-ttu-id="7f2e4-248">Přetáhněte **engineTemperature** pole do **hodnoty řádku** oblasti změnit typ agregace na **průměrná**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-248">Drag the **engineTemperature** field into **Line Values** area, change the  aggregation type to **Average**.</span></span> 

![Připojené aut - vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

<span data-ttu-id="7f2e4-250">Změnit grafu **název** k **"Průměrná rychlost, můžete zadat naléhavost, modul těžba ropy a teploty modul"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-250">Change the chart **Title** to **“Average speed, tire pressure, engine oil and engine temperature”**.</span></span>  

![Připojené aut - vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

<span data-ttu-id="7f2e4-252">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-252">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="7f2e4-253">Vyberte **vlastnosti Treemap** vizualizace z vizualizace, přetáhněte **modelu** pole do **skupiny** oblasti a přetáhněte pole **MaintenanceProbability** do **hodnoty** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-253">Select **Treemap** visualization from visualizations, drag the **Model** field into the **Group** area, and drag the field **MaintenanceProbability** into the **Values** area.</span></span>

<span data-ttu-id="7f2e4-254">Změnit grafu **název** k **"Vehicle modely údržby"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-254">Change the chart **Title** to **“Vehicle models requiring maintenance”**.</span></span>

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

<span data-ttu-id="7f2e4-256">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-256">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="7f2e4-257">Vyberte **100 % skládaný sloupcový graf** z vizualizace, přetáhněte **města** pole do **osy** oblasti a přetáhněte **MaintenanceProbability**, **RecallProbability** polí do **hodnotu** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-257">Select **100% Stacked Bar Chart** from visualization, drag the **city** field into the **Axis** area, and drag the **MaintenanceProbability**, **RecallProbability** fields into the **Value** area.</span></span>

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

<span data-ttu-id="7f2e4-259">Klikněte na tlačítko **formátu**, vyberte **Data barvy**a nastavte **MaintenanceProbability** barvy, která má hodnotu **"F2C80F"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-259">Click **Format**, select **Data Colors**, and set the **MaintenanceProbability** color to the value **“F2C80F”**.</span></span>

<span data-ttu-id="7f2e4-260">Změna **název** grafu můžete **"pravděpodobnosti z Vehicle údržby a odvolat pomocí Město,**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-260">Change the **Title** of the chart to **“Probability of Vehicle Maintenance & Recall by City”**.</span></span>

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

<span data-ttu-id="7f2e4-262">Klikněte na prázdnou oblast přidat nové vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-262">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="7f2e4-263">Vyberte **plošný graf** z vizualizace z vizualizace, přetáhněte **modelu** pole do **osy** oblasti a přetáhněte **engineOil, tirepressure, rychlosti a MaintenanceProbability** polí do **hodnoty** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-263">Select **Area Chart** from visualization from visualizations, drag the **Model** field into the **Axis** area, and drag the **engineOil, tirepressure, speed and MaintenanceProbability** fields into the **Values** area.</span></span> <span data-ttu-id="7f2e4-264">Změnit jejich typ agregace na **"Průměrné"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-264">Change their aggregation type to **“Average”**.</span></span> 

![Připojené aut - změnit typ agregace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

<span data-ttu-id="7f2e4-266">Změnit název graf **"Průměrná těžba ropy modul, přestanou zatížení, rychlosti a údržba pravděpodobnosti modelem"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-266">Change the title of the chart to **“Average engine oil, tire pressure, speed and maintenance probability by model”**.</span></span>

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

<span data-ttu-id="7f2e4-268">Klikněte na prázdnou oblast přidat nové vizualizace:</span><span class="sxs-lookup"><span data-stu-id="7f2e4-268">Click the blank area to add new visualization:</span></span>

1. <span data-ttu-id="7f2e4-269">Vyberte **bodový graf** vizualizace z vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-269">Select **Scatter Chart** visualization from visualizations.</span></span>
2. <span data-ttu-id="7f2e4-270">Přetáhněte **modelu** pole do **podrobnosti** a **legendy** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-270">Drag the **Model** field into the **Details** and **Legend** area.</span></span>
3. <span data-ttu-id="7f2e4-271">Přetáhněte **paliva** pole do **osy x** oblasti změnit agregace na **průměrná**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-271">Drag the **fuel** field into the **X-Axis** area, change the aggregation to **Average**.</span></span>
4. <span data-ttu-id="7f2e4-272">Přetáhněte **engineTemparature** do **osy y oblasti**, změňte agregace na **průměrná**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-272">Drag **engineTemparature** into **Y-Axis area**, change the aggregation to **Average**</span></span>
5. <span data-ttu-id="7f2e4-273">Přetáhněte **vin** pole do **velikost** oblasti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-273">Drag the **vin** field into the **Size** area.</span></span>

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

<span data-ttu-id="7f2e4-275">Změnit grafu **název** k **"Průměry paliva, modul teploty modelem"**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-275">Change the chart **Title** to **“Averages of Fuel, Engine Temperature by Model”**.</span></span>

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

<span data-ttu-id="7f2e4-277">Závěrečnou zprávu bude vypadat, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-277">The final report will look like as shown below.</span></span>

![Připojené aut konečné sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a><span data-ttu-id="7f2e4-279">Vizualizace kódu PIN ze sestav na řídicím panelu v reálném čase</span><span class="sxs-lookup"><span data-stu-id="7f2e4-279">Pin visualizations from the reports to the real-time dashboard</span></span>
<span data-ttu-id="7f2e4-280">Vytvořte prázdný řídicí panel kliknutím na ikonu plus vedle řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-280">Create a blank dashboard by clicking on the plus icon next to Dashboards.</span></span> <span data-ttu-id="7f2e4-281">Pojmenujte ji "Panelu analýzy Telemetrie Vehicle"</span><span class="sxs-lookup"><span data-stu-id="7f2e4-281">You can name it “Vehicle Telemetry Analytics Dashboard”</span></span>

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

<span data-ttu-id="7f2e4-283">Připnete na řídicí panel vizualizaci z výše uvedených sestav.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-283">Pin the visualization from the above reports to the dashboard.</span></span> 

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

<span data-ttu-id="7f2e4-285">Po vytvoření všechny tři sestavy a odpovídající vizualizacemi jsou připnuli k řídicímu panelu, řídicí panel by měla vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-285">The dashboard should look as follows when all the three reports are created and the corresponding visualizations are pinned to the dashboard.</span></span> <span data-ttu-id="7f2e4-286">Pokud jste dosud nevytvořili všechny sestavy, může vypadat jinak řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-286">If you have not created all the reports, your dashboard could look different.</span></span> 

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

<span data-ttu-id="7f2e4-288">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="7f2e4-288">Congratulations!</span></span> <span data-ttu-id="7f2e4-289">Úspěšně jste vytvořili řídicím panelu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-289">You have successfully created the real-time dashboard.</span></span> <span data-ttu-id="7f2e4-290">Při dalším spuštění CarEventGenerator.exe a RealtimeDashboardApp.exe, měli byste vidět živé aktualizace na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-290">As you continue to execute CarEventGenerator.exe and RealtimeDashboardApp.exe, you should see live updates on the dashboard.</span></span> <span data-ttu-id="7f2e4-291">Pomocí následujících kroků má trvat asi 10 až 15 minut.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-291">It should take about 10 to 15 minutes to complete the following steps.</span></span>

## <a name="setup-power-bi-batch-processing-dashboard"></a><span data-ttu-id="7f2e4-292">Instalační program řídicí panel Power BI dávkové zpracování</span><span class="sxs-lookup"><span data-stu-id="7f2e4-292">Setup Power BI batch processing dashboard</span></span>
> [!NOTE]
> <span data-ttu-id="7f2e4-293">Pro kanál koncová dávkové zpracování dokončí provádění a zpracovat v roce vhodné generované datové trvá o dvou hodin (z úspěšné dokončení nasazení).</span><span class="sxs-lookup"><span data-stu-id="7f2e4-293">It takes about two hours (from the successful completion of the deployment) for the end to end batch processing pipeline to finish execution and process a year worth of generated data.</span></span> <span data-ttu-id="7f2e4-294">Proto čekající na zpracování ukončíte než budete pokračovat v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-294">So wait for the processing to finish before proceeding with the next steps.</span></span> 
> 
> 

<span data-ttu-id="7f2e4-295">**Stáhněte si soubor návrháře Power BI**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-295">**Download the Power BI designer file**</span></span>

* <span data-ttu-id="7f2e4-296">Je součástí nasazení pokyny pro ruční provoz předem nakonfigurovaný soubor návrháře Power BI</span><span class="sxs-lookup"><span data-stu-id="7f2e4-296">A pre-configured Power BI designer file is included as part of the deployment Manual Operation Instructions</span></span>
* <span data-ttu-id="7f2e4-297">Podívejte se na 2.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-297">Look for 2.</span></span> <span data-ttu-id="7f2e4-298">Instalační program PowerBI dávkové zpracování řídicí panel si můžete stáhnout šablony PowerBI pro dávkové zpracování řídicí panel zde názvem **ConnectedCarsPbiReport.pbix**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-298">Setup PowerBI batch processing dashboard You can download the PowerBI template for batch processing dashboard here called **ConnectedCarsPbiReport.pbix**.</span></span>
* <span data-ttu-id="7f2e4-299">Uložit místně</span><span class="sxs-lookup"><span data-stu-id="7f2e4-299">Save locally</span></span>

<span data-ttu-id="7f2e4-300">**Konfigurace sestav Power BI**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-300">**Configure Power BI reports**</span></span>

* <span data-ttu-id="7f2e4-301">Otevřete soubor návrháře '**ConnectedCarsPbiReport.pbix**' pomocí Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-301">Open the designer file ‘**ConnectedCarsPbiReport.pbix**’ using Power BI Desktop.</span></span> <span data-ttu-id="7f2e4-302">Pokud jste ještě není, nainstalujte Power BI Desktop z [instalace Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331).</span><span class="sxs-lookup"><span data-stu-id="7f2e4-302">If you do not already have, install the Power BI Desktop from [Power BI Desktop install](http://www.microsoft.com/download/details.aspx?id=45331).</span></span> 
* <span data-ttu-id="7f2e4-303">Klikněte **upravit dotazy**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-303">Click the **Edit Queries**.</span></span>

![Upravit dotaz Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* <span data-ttu-id="7f2e4-305">Dvakrát klikněte **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-305">Double-click the **Source**.</span></span>

![Zdroj sady Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* <span data-ttu-id="7f2e4-307">Aktualizujte připojovací řetězec serveru se serverem Azure SQL, které byly zřízeny jako součást nasazení.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-307">Update Server connection string with the Azure SQL server that got provisioned as part of the deployment.</span></span>  <span data-ttu-id="7f2e4-308">Podívejte se v pokynech ruční operace v části</span><span class="sxs-lookup"><span data-stu-id="7f2e4-308">Look in the Manual Operation Instructions under</span></span> 

    4. <span data-ttu-id="7f2e4-309">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="7f2e4-309">Azure SQL Database</span></span>
    
    * <span data-ttu-id="7f2e4-310">Server: somethingsrv.database.windows.net</span><span class="sxs-lookup"><span data-stu-id="7f2e4-310">Server: somethingsrv.database.windows.net</span></span>
    * <span data-ttu-id="7f2e4-311">Databáze: connectedcar</span><span class="sxs-lookup"><span data-stu-id="7f2e4-311">Database: connectedcar</span></span>
    * <span data-ttu-id="7f2e4-312">Uživatelské jméno: uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="7f2e4-312">Username: username</span></span>
    * <span data-ttu-id="7f2e4-313">Heslo: Heslo SQL serveru můžete spravovat z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7f2e4-313">Password: You can manage your SQL server password from Azure portal</span></span>

* <span data-ttu-id="7f2e4-314">Nechte **databáze** jako *connectedcar*.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-314">Leave **Database** as *connectedcar*.</span></span>

![Nastavte Power BI databázi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* <span data-ttu-id="7f2e4-316">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-316">Click **OK**.</span></span>
* <span data-ttu-id="7f2e4-317">Zobrazí se **pověření systému Windows** vybraná karta ve výchozím nastavení, změňte ho na **databáze pověření** kliknutím na **databáze** kartě vpravo.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-317">You will see **Windows credential** tab selected by default, change it to **Database credentials** by clicking on **Database** tab at right.</span></span>
* <span data-ttu-id="7f2e4-318">Zadejte **uživatelské jméno** a **heslo** vaší databáze SQL Azure, který byl zadán během instalace jeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-318">Provide the **Username** and **Password** of your Azure SQL Database that was specified during its deployment setup.</span></span>

![Zadejte přihlašovací údaje databáze](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* <span data-ttu-id="7f2e4-320">Klikněte na tlačítko **připojení**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-320">Click **Connect**</span></span>
* <span data-ttu-id="7f2e4-321">Zopakujte výše uvedené kroky pro každý tři zbývající dotaz, který je přítomen v pravém podokně a pak aktualizujte podrobnosti připojení zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-321">Repeat the above steps for each of the three remaining queries present at right pane, and then update the data source connection details.</span></span>
* <span data-ttu-id="7f2e4-322">Klikněte na tlačítko **zavřete a načíst**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-322">Click **Close and Load**.</span></span> <span data-ttu-id="7f2e4-323">Power BI Desktop souboru datové sady jsou připojené k tabulky databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-323">Power BI Desktop file datasets are connected to SQL Azure Database tables.</span></span>
* <span data-ttu-id="7f2e4-324">**Zavřít** souboru Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-324">**Close** Power BI Desktop file.</span></span>

![Zavřít Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* <span data-ttu-id="7f2e4-326">Klikněte na tlačítko **Uložit** tlačítko a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-326">Click **Save** button to save the changes.</span></span> 

<span data-ttu-id="7f2e4-327">Nyní máte nakonfigurovaný všechny sestavy odpovídající cesta zpracování dávky v řešení.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-327">You have now configured all the reports corresponding to the batch processing path in the solution.</span></span> 

## <a name="upload-to-powerbicom"></a><span data-ttu-id="7f2e4-328">Nahrajte do *powerbi.com*</span><span class="sxs-lookup"><span data-stu-id="7f2e4-328">Upload to *powerbi.com*</span></span>
1. <span data-ttu-id="7f2e4-329">Přejděte na webový portál Power BI na http://powerbi.com a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-329">Navigate to the Power BI web portal at http://powerbi.com and login.</span></span>
2. <span data-ttu-id="7f2e4-330">Klikněte na tlačítko **získat Data**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-330">Click **Get Data**</span></span>  
3. <span data-ttu-id="7f2e4-331">Nahrajte soubor Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-331">Upload the Power BI Desktop File.</span></span>  
4. <span data-ttu-id="7f2e4-332">Chcete-li nahrát, klikněte na tlačítko **načíst Data -> získat soubory -> místního souboru**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-332">To upload, click **Get Data -> Files Get -> Local file**</span></span>  
5. <span data-ttu-id="7f2e4-333">Přejděte na **"**ConnectedCarsPbiReport.pbix**"**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-333">Navigate to the **“**ConnectedCarsPbiReport.pbix**”**</span></span>  
6. <span data-ttu-id="7f2e4-334">Po nahrání souboru přejde zpět na váš pracovní prostor Power BI.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-334">Once the file is uploaded, you will be navigated back to your Power BI work space.</span></span>  

<span data-ttu-id="7f2e4-335">Datové sady, sestavy a řídicí panel prázdné, bude vytvořena pro vás.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-335">A dataset, report and a blank dashboard will be created for you.</span></span>  

<span data-ttu-id="7f2e4-336">Grafy kód PIN na nové řídicí panel názvem **panelu analýzy Telemetrie Vehicle** v **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-336">Pin charts to a new dashboard called **Vehicle Telemetry Analytics Dashboard** in **Power BI**.</span></span> <span data-ttu-id="7f2e4-337">Klikněte na prázdný řídicí panel vytvořili výše a potom přejděte na **sestavy** části klikněte na nově nahraném sestavu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-337">Click the blank dashboard created above and then navigate to the **Reports** section click the newly uploaded report.</span></span>  

![Vehicle Telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

<span data-ttu-id="7f2e4-339">**Poznámka: Sestava má šest stránky:**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-339">**Note the report has six pages:**</span></span>  
<span data-ttu-id="7f2e4-340">Stránka 1: Hustotu Vehicle</span><span class="sxs-lookup"><span data-stu-id="7f2e4-340">Page 1: Vehicle density</span></span>  
<span data-ttu-id="7f2e4-341">Stránka 2: Stav v reálném čase vehicle</span><span class="sxs-lookup"><span data-stu-id="7f2e4-341">Page 2: Real-time vehicle health</span></span>  
<span data-ttu-id="7f2e4-342">Stránka 3: Intenzivně řízené vozidel</span><span class="sxs-lookup"><span data-stu-id="7f2e4-342">Page 3: Aggressively Driven Vehicles</span></span>   
<span data-ttu-id="7f2e4-343">Stránka 4: Připomenout vozidel</span><span class="sxs-lookup"><span data-stu-id="7f2e4-343">Page 4: Recalled vehicles</span></span>  
<span data-ttu-id="7f2e4-344">5 stránky: Efektivní řízené vozidel paliva</span><span class="sxs-lookup"><span data-stu-id="7f2e4-344">Page 5: Fuel Efficiently Driven Vehicles</span></span>  
<span data-ttu-id="7f2e4-345">6 stránky: Logo společnosti Contoso</span><span class="sxs-lookup"><span data-stu-id="7f2e4-345">Page 6: Contoso Logo</span></span>  

![Připojené aut Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

<span data-ttu-id="7f2e4-347">**Ze stránky 3**, připnout následující:</span><span class="sxs-lookup"><span data-stu-id="7f2e4-347">**From Page 3**, pin the following:</span></span>  

1. <span data-ttu-id="7f2e4-348">Počet VIN</span><span class="sxs-lookup"><span data-stu-id="7f2e4-348">Count of VIN</span></span>  
   ![Připojené aut Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. <span data-ttu-id="7f2e4-350">Intenzivně řídí vozidel modelu – vodopádu grafu</span><span class="sxs-lookup"><span data-stu-id="7f2e4-350">Aggressively driven vehicles by model – Waterfall chart</span></span>  
   ![Vehicle Telemetrie – Pin grafy 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

<span data-ttu-id="7f2e4-352">**Ze stránky 5**, připnout následující:</span><span class="sxs-lookup"><span data-stu-id="7f2e4-352">**From Page 5**, pin the following:</span></span> 

1. <span data-ttu-id="7f2e4-353">Počet vin</span><span class="sxs-lookup"><span data-stu-id="7f2e4-353">Count of vin</span></span>    
   ![Vehicle Telemetrie – Pin grafy 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. <span data-ttu-id="7f2e4-355">Paliva efektivní vozidel modelem: Skupinový sloupcový graf</span><span class="sxs-lookup"><span data-stu-id="7f2e4-355">Fuel efficient vehicles by model: Clustered column chart</span></span>  
   ![Vehicle Telemetrie – Pin grafy 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

<span data-ttu-id="7f2e4-357">**Stránka 4**, připnout následující:</span><span class="sxs-lookup"><span data-stu-id="7f2e4-357">**From Page 4**, pin the following:</span></span>  

1. <span data-ttu-id="7f2e4-358">Počet vin</span><span class="sxs-lookup"><span data-stu-id="7f2e4-358">Count of vin</span></span>  
   ![Vehicle Telemetrie – Pin grafy 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. <span data-ttu-id="7f2e4-360">Odvolané vozidel podle města, model: vlastnosti Treemap</span><span class="sxs-lookup"><span data-stu-id="7f2e4-360">Recalled vehicles by city, model: Treemap</span></span>  
   ![Vehicle Telemetrie – Pin grafy 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

<span data-ttu-id="7f2e4-362">**Ze stránky 6**, připnout následující:</span><span class="sxs-lookup"><span data-stu-id="7f2e4-362">**From Page 6**, pin the following:</span></span>  

1. <span data-ttu-id="7f2e4-363">Logo společnosti Contoso motory</span><span class="sxs-lookup"><span data-stu-id="7f2e4-363">Contoso Motors logo</span></span>  
   ![Vehicle Telemetrie – Pin grafy 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

<span data-ttu-id="7f2e4-365">**Uspořádání řídicí panel**</span><span class="sxs-lookup"><span data-stu-id="7f2e4-365">**Organize the dashboard**</span></span>  

1. <span data-ttu-id="7f2e4-366">Přejděte na řídicí panel</span><span class="sxs-lookup"><span data-stu-id="7f2e4-366">Navigate to the dashboard</span></span>
2. <span data-ttu-id="7f2e4-367">Podržte ukazatel nad každý graf a přejmenujte ji v závislosti na pojmenování uvedené v následující obrázek kompletní řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-367">Hover over each chart and rename it based on the naming provided in the complete dashboard image below.</span></span> <span data-ttu-id="7f2e4-368">Také pohyb grafy, aby vypadala jako na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-368">Also move the charts around to look like the dashboard below.</span></span>  
   ![Vehicle Telemetrie – uspořádání řídicí panel 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle Telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. <span data-ttu-id="7f2e4-371">Pokud jste vytvořili všechny sestavy, jak je uvedeno v tomto dokumentu, poslední dokončené řídicí panel by měl vypadat jako na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-371">If you have created all the reports as mentioned in this document, the final completed dashboard should look like the following figure.</span></span> 

![Vehicle Telemetrie – uspořádání řídicí panel 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

<span data-ttu-id="7f2e4-373">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="7f2e4-373">Congratulations!</span></span> <span data-ttu-id="7f2e4-374">Úspěšně jste vytvořili sestavy a řídicí panel můžete získat v reálném čase, prediktivní a dávky o vehicle stavu a řídí zvyklosti.</span><span class="sxs-lookup"><span data-stu-id="7f2e4-374">You have successfully created the reports and the dashboard to gain real-time, predictive and batch insights on vehicle health and driving habits.</span></span>  
