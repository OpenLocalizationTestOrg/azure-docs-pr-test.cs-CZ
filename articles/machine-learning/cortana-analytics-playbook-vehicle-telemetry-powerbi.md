---
title: "aaaPower řídicí panel BI vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti hello Cortana Intelligence toogain v reálném čase a prediktivní Statistika na vehicle stavu a řídí zvyklosti."
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
ms.openlocfilehash: 0bd054d943387ecad7301236eebae22458173aba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a><span data-ttu-id="5f04b-103">Vehicle telemetrie analytics řešení šablony řídicí panel Power BI pokyny pro instalaci</span><span class="sxs-lookup"><span data-stu-id="5f04b-103">Vehicle telemetry analytics solution template Power BI Dashboard setup instructions</span></span>
<span data-ttu-id="5f04b-104">To **nabídky** odkazy kapitolám toohello v této scénářem.</span><span class="sxs-lookup"><span data-stu-id="5f04b-104">This **menu** links toohello chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="5f04b-105">Hello řešení Vehicle Telemetrie analýzy umožňující prezentovat jak dealerům car, automobilů výrobců a pojištění společností můžete využít možnosti hello přehledy v reálném čase a prediktivní toogain Cortana Intelligence na vehicle stavu a řízení zvyklosti toodrive vylepšení v oblasti hello zákazníka prostředí R & D a marketingových kampaní.</span><span class="sxs-lookup"><span data-stu-id="5f04b-105">hello Vehicle Telemetry Analytics solution showcases how car dealerships, automobile manufacturers and insurance companies can leverage hello capabilities of Cortana Intelligence toogain real-time and predictive insights on vehicle health and driving habits toodrive improvements in hello area of customer experience, R&D and marketing campaigns.</span></span> <span data-ttu-id="5f04b-106">Tento dokument obsahuje pokyny krok za krokem, jak můžete nakonfigurovat hello Power BI sestavy a řídicí panel po nasazení řešení hello ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="5f04b-106">This document contains step by step instructions on how you can configure hello Power BI reports and dashboard once hello solution is deployed in your subscription.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="5f04b-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f04b-107">Prerequisites</span></span>
1. <span data-ttu-id="5f04b-108">Nasazení hello [Telemetrie Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) řešení</span><span class="sxs-lookup"><span data-stu-id="5f04b-108">Deploy hello [Telemetry Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) solution</span></span>  
2. [<span data-ttu-id="5f04b-109">Nainstalujte Microsoft Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="5f04b-109">Install Microsoft Power BI Desktop</span></span>](http://www.microsoft.com/download/details.aspx?id=45331)
3. <span data-ttu-id="5f04b-110">[Předplatné](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5f04b-110">An [Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="5f04b-111">Pokud nemáte předplatné Azure, Začínáme s Azure bezplatné předplatné</span><span class="sxs-lookup"><span data-stu-id="5f04b-111">If you don't have an Azure subscription, get started with Azure free subscription</span></span>
4. <span data-ttu-id="5f04b-112">Účet Microsoft Power BI</span><span class="sxs-lookup"><span data-stu-id="5f04b-112">Microsoft Power BI account</span></span>

## <a name="cortana-intelligence-suite-components"></a><span data-ttu-id="5f04b-113">Cortana Intelligence Suite součásti</span><span class="sxs-lookup"><span data-stu-id="5f04b-113">Cortana Intelligence Suite Components</span></span>
<span data-ttu-id="5f04b-114">V rámci šablony řešení Analytics Telemetrie Vehicle hello hello následující služby Cortana Intelligence nasazených v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="5f04b-114">As part of hello Vehicle Telemetry Analytics solution template, hello following Cortana Intelligence services are deployed in your subscription.</span></span>

* <span data-ttu-id="5f04b-115">**Centra událostí** pro příjem miliony událostí vehicle telemetrická data do Azure.</span><span class="sxs-lookup"><span data-stu-id="5f04b-115">**Event Hub** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="5f04b-116">**Stream Analytics** pro získání přehledy v reálném čase na vehicle stavu a přetrvává tato data do dlouhodobého úložiště pro širší batch analýzu.</span><span class="sxs-lookup"><span data-stu-id="5f04b-116">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="5f04b-117">**Strojového učení** pro zjišťování anomálií v reálném čase a dávkového zpracování toogain prediktivní statistiky.</span><span class="sxs-lookup"><span data-stu-id="5f04b-117">**Machine Learning** for anomaly detection in real-time and batch processing toogain predictive insights.</span></span>
* <span data-ttu-id="5f04b-118">**HDInsight** data tootransform využít ve velkém měřítku</span><span class="sxs-lookup"><span data-stu-id="5f04b-118">**HDInsight** is leveraged tootransform data at scale</span></span>
* <span data-ttu-id="5f04b-119">**Objekt pro vytváření dat** zpracovává orchestration, plánování, správy prostředků a monitorování hello dávkové zpracování kanálu.</span><span class="sxs-lookup"><span data-stu-id="5f04b-119">**Data Factory** handles orchestration, scheduling, resource management and monitoring of hello batch processing pipeline.</span></span>

<span data-ttu-id="5f04b-120">**Power BI** toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="5f04b-120">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="5f04b-121">řešení Hello používá dvou různých zdrojů dat.: **Simulated vehicle signály a diagnostiky datovou sadu** a **vehicle katalogu**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-121">hello solution uses two different data sources: **Simulated vehicle signals and diagnostic dataset** and **vehicle catalog**.</span></span>

<span data-ttu-id="5f04b-122">Vehicle telematika jsme je součástí tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="5f04b-122">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="5f04b-123">Vysílá diagnostické informace a signály odpovídající toohello stav hello vehicle a podporovat jeho vzor k danému bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="5f04b-123">It emits diagnostic information and signals corresponding toohello state of hello vehicle and driving pattern at a given point in time.</span></span> 

<span data-ttu-id="5f04b-124">Hello Vehicle katalogu je referenční datová sada obsahující VIN toomodel mapování</span><span class="sxs-lookup"><span data-stu-id="5f04b-124">hello Vehicle Catalog is a reference dataset containing VIN toomodel mapping</span></span>

## <a name="power-bi-dashboard-preparation"></a><span data-ttu-id="5f04b-125">Příprava Power BI řídicí panel</span><span class="sxs-lookup"><span data-stu-id="5f04b-125">Power BI Dashboard Preparation</span></span>
### <a name="setup-power-bi-real-time-dashboard"></a><span data-ttu-id="5f04b-126">Instalační program v reálném čase řídicí panel Power BI</span><span class="sxs-lookup"><span data-stu-id="5f04b-126">Setup Power BI Real-Time Dashboard</span></span>

<span data-ttu-id="5f04b-127">**Spuštění aplikace v reálném čase řídicí panel hello** po dokončení nasazení hello, postupujte podle pokynů operaci ruční hello</span><span class="sxs-lookup"><span data-stu-id="5f04b-127">**Start hello real-time dashboard application** Once hello deployment is completed, you should follow hello Manual Operation Instructions</span></span>

* <span data-ttu-id="5f04b-128">Stažení aplikace řídicího panelu v reálném čase RealtimeDashboardApp.zip a rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="5f04b-128">Download real-time dashboard application RealtimeDashboardApp.zip, and unzip it.</span></span>
*  <span data-ttu-id="5f04b-129">V rozbalené složce hello otevřete aplikaci konfiguračního souboru 'RealtimeDashboardApp.exe.config', nahraďte appSettings pro Eventhub, úložiště objektů Blob a ML připojení služby s hodnotami hello v hello pokyny pro ruční provoz a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="5f04b-129">In hello unzipped folder, open app config file 'RealtimeDashboardApp.exe.config', replace appSettings for Eventhub, Blob Storage, and ML service connections with hello values in hello Manual Operation Instructions, and save your changes.</span></span>
* <span data-ttu-id="5f04b-130">Spusťte aplikaci RealtimeDashboardApp.exe.</span><span class="sxs-lookup"><span data-stu-id="5f04b-130">Run application RealtimeDashboardApp.exe.</span></span> <span data-ttu-id="5f04b-131">Okno přihlášení bude překryvné, zadejte svoje platné přihlašovací údaje PowerBI a klikněte na hello **přijmout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f04b-131">A login window will pop up, provide your valid PowerBI credentials and click hello **Accept** button.</span></span> <span data-ttu-id="5f04b-132">Pak spustí aplikaci hello toorun.</span><span class="sxs-lookup"><span data-stu-id="5f04b-132">Then hello app will start toorun.</span></span>

   ![Přihlášení tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI Dashboard oprávnění](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* <span data-ttu-id="5f04b-135">Web tooPowerBI přihlášení a vytvořit řídicí panel v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="5f04b-135">Login tooPowerBI website, and create real-time dashboard.</span></span>

<span data-ttu-id="5f04b-136">Nyní jsou připravené tooconfigure řídicí panel Power BI hello s bohatých vizualizací toogain v reálném čase a zvyklosti prediktivní Statistika na vehicle stavu a řídí.</span><span class="sxs-lookup"><span data-stu-id="5f04b-136">Now, you are ready tooconfigure hello Power BI dashboard with rich visualizations toogain real-time and predictive insights on vehicle health and driving habits.</span></span> <span data-ttu-id="5f04b-137">Jak dlouho trvá přibližně 45 minut tooan hodinu toocreate všechny hello sestavy a řídicí panel hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5f04b-137">It takes about 45 minutes tooan hour toocreate all hello reports and configure hello dashboard.</span></span> 

### <a name="configure-power-bi-reports"></a><span data-ttu-id="5f04b-138">Konfigurace sestav Power BI</span><span class="sxs-lookup"><span data-stu-id="5f04b-138">Configure Power BI reports</span></span>
<span data-ttu-id="5f04b-139">Hello v reálném čase sestavy a řídicí panel hello trvat o toocomplete 30 – 45 minut.</span><span class="sxs-lookup"><span data-stu-id="5f04b-139">hello real-time reports and hello dashboard take about 30-45 minutes toocomplete.</span></span> <span data-ttu-id="5f04b-140">Procházet příliš[http://powerbi.com](http://powerbi.com) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="5f04b-140">Browse too[http://powerbi.com](http://powerbi.com) and login.</span></span>

![Přihlášení tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

<span data-ttu-id="5f04b-142">Nová datová sada se generuje ve službě Power BI.</span><span class="sxs-lookup"><span data-stu-id="5f04b-142">A new dataset is generated in Power BI.</span></span> <span data-ttu-id="5f04b-143">Klikněte na tlačítko hello **ConnectedCarsRealtime** datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5f04b-143">Click hello **ConnectedCarsRealtime** dataset.</span></span>

![Vybráno připojené v reálném čase datové sady automobilů](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

<span data-ttu-id="5f04b-145">Uložit pomocí prázdné sestavy hello **Ctrl + s**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-145">Save hello blank report using **Ctrl + s**.</span></span>

![Uložit prázdné sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

<span data-ttu-id="5f04b-147">Zadejte název sestavy *Vehicle Telemetrie analýzy v reálném čase - sestavy*.</span><span class="sxs-lookup"><span data-stu-id="5f04b-147">Provide report name *Vehicle Telemetry Analytics Real-time - Reports*.</span></span>

![Zadejte název sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a><span data-ttu-id="5f04b-149">Sestavy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="5f04b-149">Real-time reports</span></span>
<span data-ttu-id="5f04b-150">V tomto řešení jsou tři sestavy v reálném čase:</span><span class="sxs-lookup"><span data-stu-id="5f04b-150">There are three real-time reports in this solution:</span></span>

1. <span data-ttu-id="5f04b-151">Vozidel v operaci</span><span class="sxs-lookup"><span data-stu-id="5f04b-151">Vehicles in operation</span></span>
2. <span data-ttu-id="5f04b-152">Vozidel údržby</span><span class="sxs-lookup"><span data-stu-id="5f04b-152">Vehicles Requiring Maintenance</span></span>
3. <span data-ttu-id="5f04b-153">Statistiky vozidel stavu</span><span class="sxs-lookup"><span data-stu-id="5f04b-153">Vehicles Health Statistics</span></span>

<span data-ttu-id="5f04b-154">Můžete zvolit tooconfigure všechny hello tři v reálném čase sestavy nebo zastavena po jakékoli fázi a pokračovat další části toohello konfigurace hello batch sestavy.</span><span class="sxs-lookup"><span data-stu-id="5f04b-154">You can choose tooconfigure all hello three real-time reports or stop after any stage and proceed toohello next section of configuring hello batch reports.</span></span> <span data-ttu-id="5f04b-155">Doporučujeme toocreate všechny hello tři sestavy toovisualize hello úplné statistiky v reálném čase cesty hello hello řešení.</span><span class="sxs-lookup"><span data-stu-id="5f04b-155">We recommend you toocreate all hello three reports toovisualize hello full insights of hello real-time path of hello solution.</span></span>  

### <a name="1-vehicles-in-operation"></a><span data-ttu-id="5f04b-156">1. Vozidel v operaci</span><span class="sxs-lookup"><span data-stu-id="5f04b-156">1. Vehicles in operation</span></span>
<span data-ttu-id="5f04b-157">Klikněte dvakrát na **stránka 1** a přejmenujte ji příliš "Vozidel v operaci"</span><span class="sxs-lookup"><span data-stu-id="5f04b-157">Double-click **Page 1** and rename it too“Vehicles in operation”</span></span>  
    <span data-ttu-id="5f04b-158">![Připojených vozidel v operaci aut-](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-158">![Connected Cars - Vehicles in operation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span></span>  

<span data-ttu-id="5f04b-159">Vyberte **vin** pole z **pole** a vyberte typ vizualizace jako **"Kartou"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-159">Select **vin** field from **Fields** and choose visualization type as **“Card”**.</span></span>  

<span data-ttu-id="5f04b-160">Karta vizualizace se vytvoří, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="5f04b-160">Card visualization is created as shown in figure.</span></span>  
    ![Připojené aut - vyberte vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

<span data-ttu-id="5f04b-162">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-162">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="5f04b-163">Vyberte **města** a **vin** z pole.</span><span class="sxs-lookup"><span data-stu-id="5f04b-163">Select **City** and **vin** from fields.</span></span> <span data-ttu-id="5f04b-164">Změnit vizualizace příliš**"Map"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-164">Change visualization too**“Map”**.</span></span> <span data-ttu-id="5f04b-165">Přetáhněte **vin** v oblasti hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5f04b-165">Drag **vin** in values area.</span></span> <span data-ttu-id="5f04b-166">Přetáhněte **města** z pole příliš**legendy** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-166">Drag **city** from fields too**Legend** area.</span></span>   
    <span data-ttu-id="5f04b-167">![Připojené aut – karta vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-167">![Connected Cars - Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span></span>

<span data-ttu-id="5f04b-168">Vyberte **formátu** část z **vizualizace**, klikněte na tlačítko **název** a změňte hello **Text** příliš**"vozidel v operace ve městě"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-168">Select **format** section from **Visualizations**, click **Title** and change hello **Text** too**“Vehicles in operation by city”**.</span></span>  
    <span data-ttu-id="5f04b-169">![Připojených vozidel v operaci podle města aut-](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-169">![Connected Cars - Vehicles in operation by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span></span>   

<span data-ttu-id="5f04b-170">Poslední vizualizace vypadá, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="5f04b-170">Final visualization looks as shown in figure.</span></span>    
    ![Připojené aut - poslední vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

<span data-ttu-id="5f04b-172">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-172">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="5f04b-173">Vyberte **města** a **vin**, změnit typ vizualizace příliš**skupinový sloupcový graf**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-173">Select **City** and **vin**, change visualization type too**Clustered Column Chart**.</span></span> <span data-ttu-id="5f04b-174">Ujistěte se, **města** pole **osy oblasti** a **vin** v **hodnotu oblasti**</span><span class="sxs-lookup"><span data-stu-id="5f04b-174">Ensure **City** field in **Axis area** and **vin** in **Value area**</span></span>  

<span data-ttu-id="5f04b-175">Graf řazení podle **"Počet vin"**</span><span class="sxs-lookup"><span data-stu-id="5f04b-175">Sort chart by **“Count of vin”**</span></span>  
    <span data-ttu-id="5f04b-176">![Připojené aut - počet vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-176">![Connected Cars - Count of vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span></span>  

<span data-ttu-id="5f04b-177">Změna grafu **název** příliš**"Vozidel v operaci podle města"**</span><span class="sxs-lookup"><span data-stu-id="5f04b-177">Change chart **Title** too**“Vehicles in operation by city”**</span></span>  

<span data-ttu-id="5f04b-178">Klikněte na tlačítko hello **formátu** části a pak vyberte **Data barvy**, klikněte na tlačítko hello **"Na"** příliš**Zobrazit vše**</span><span class="sxs-lookup"><span data-stu-id="5f04b-178">Click hello **Format** section, then select **Data Colors**,  Click hello **“On”** too**Show All**</span></span>  
    <span data-ttu-id="5f04b-179">![Připojené aut – zobrazit všechny Data barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-179">![Connected Cars - Show all Data Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span></span>  

<span data-ttu-id="5f04b-180">Změnit barvu hello jednotlivých města kliknutím na ikonu barev.</span><span class="sxs-lookup"><span data-stu-id="5f04b-180">Change hello color of individual city by clicking on color icon.</span></span>  
    ![Připojené aut - Změna barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

<span data-ttu-id="5f04b-182">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-182">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="5f04b-183">Vyberte **skupinový sloupcový graf** vizualizace z vizualizace, přetáhněte **města** pole **osy** oblasti **modelu** v **legendy** oblasti a **vin** v **hodnotu** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-183">Select **Clustered Column Chart** visualization from visualizations, drag **city** field in **Axis** area, **Model** in **Legend** area and **vin** in **Value** area.</span></span>  
    <span data-ttu-id="5f04b-184">![Připojené aut - skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-184">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span></span>  
    <span data-ttu-id="5f04b-185">![Připojené aut - vykreslování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-185">![Connected Cars - Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span></span>

<span data-ttu-id="5f04b-186">Změna uspořádání všechny vizualizace na této stránce, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="5f04b-186">Rearrange all visualization on this page as shown in figure.</span></span>  
    ![Připojené aut - vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

<span data-ttu-id="5f04b-188">Úspěšně jste nakonfigurovali v reálném čase sestavy "Vozidel v operaci" hello.</span><span class="sxs-lookup"><span data-stu-id="5f04b-188">You have successfully configured hello “Vehicles in operation” real-time report.</span></span> <span data-ttu-id="5f04b-189">Můžete pokračovat v reálném čase sestavy toocreate hello další nebo zde zastavte a konfigurujte hello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="5f04b-189">You can proceed toocreate hello next real-time report or stop here and configure hello dashboard.</span></span> 

### <a name="2-vehicles-requiring-maintenance"></a><span data-ttu-id="5f04b-190">2. Vozidel údržby</span><span class="sxs-lookup"><span data-stu-id="5f04b-190">2. Vehicles Requiring Maintenance</span></span>
<span data-ttu-id="5f04b-191">Klikněte na tlačítko ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd nové sestavy, přejmenujte ji příliš**"Vozidel nutnosti Údržba"**</span><span class="sxs-lookup"><span data-stu-id="5f04b-191">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd a new report, rename it too**“Vehicles Requiring Maintenance”**</span></span>

![Připojené aut - vozidel údržby](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

<span data-ttu-id="5f04b-193">Vyberte **vin** pole a změnit typ vizualizace příliš**karty**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-193">Select **vin** field and change visualization type too**Card**.</span></span>  
    <span data-ttu-id="5f04b-194">![Připojené aut – karta Vin vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-194">![Connected Cars - Vin Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span></span>  

<span data-ttu-id="5f04b-195">Máme pole s názvem "MaintenanceLabel" v datové sadě hello.</span><span class="sxs-lookup"><span data-stu-id="5f04b-195">We have a field named “MaintenanceLabel” In hello dataset.</span></span> <span data-ttu-id="5f04b-196">Toto pole může mít hodnotu "0" nebo "1"."</span><span class="sxs-lookup"><span data-stu-id="5f04b-196">This field can have a value of “0” or “1”.”</span></span> <span data-ttu-id="5f04b-197">Je nastavena podle model Azure Machine Learning hello zřízené v rámci řešení a integraci s cestou hello v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="5f04b-197">It is set by hello Azure Machine Learning model provisioned as part of solution and integrated with hello real-time path.</span></span> <span data-ttu-id="5f04b-198">Hello hodnotu "1" označuje, že že vehicle vyžaduje údržbu.</span><span class="sxs-lookup"><span data-stu-id="5f04b-198">hello value “1” indicates a vehicle requires maintenance.</span></span> 

<span data-ttu-id="5f04b-199">tooadd **úrovně stránky** filtr zobrazující vozidel data, která jsou nutnosti údržby:</span><span class="sxs-lookup"><span data-stu-id="5f04b-199">tooadd a **Page Level** filter for showing vehicles data, which are requiring maintenance:</span></span> 

1. <span data-ttu-id="5f04b-200">Přetáhněte hello **"MaintenanceLabel"** pole do **stránky úroveň filtry**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-200">Drag hello **“MaintenanceLabel”** field into **Page Level Filters**.</span></span>  
   <span data-ttu-id="5f04b-201">![Připojené aut - filtry úrovně stránky](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-201">![Connected Cars - Page Level Filters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span></span>  
2. <span data-ttu-id="5f04b-202">Klikněte na tlačítko **základní filtrování** nabídky nachází v dolní části MaintenanceLabel stránky úroveň filtru.</span><span class="sxs-lookup"><span data-stu-id="5f04b-202">Click **Basic Filtering** menu present at bottom of MaintenanceLabel Page Level Filter.</span></span>  
   <span data-ttu-id="5f04b-203">![Připojené aut – základní filtrování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-203">![Connected Cars - Basic Filtering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span></span>  
3. <span data-ttu-id="5f04b-204">Nastavení jeho hodnoty filtru příliš**"1"**  </span><span class="sxs-lookup"><span data-stu-id="5f04b-204">Set its filter value too**“1”**  </span></span>  
   <span data-ttu-id="5f04b-205">![Připojené aut - hodnota filtru](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-205">![Connected Cars - Filter Value](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span></span>  

<span data-ttu-id="5f04b-206">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-206">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="5f04b-207">Vyberte **skupinový sloupcový graf** z vizualizace</span><span class="sxs-lookup"><span data-stu-id="5f04b-207">Select **Clustered Column Chart** from visualizations</span></span>  
<span data-ttu-id="5f04b-208">![Připojené aut – karta Vind vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-208">![Connected Cars - Vind Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span></span>  
<span data-ttu-id="5f04b-209">![Připojené aut - skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-209">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span></span>

<span data-ttu-id="5f04b-210">Přetáhněte pole **modelu** do **osy** oblasti **Vin** příliš**hodnotu** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-210">Drag field **Model** into **Axis** area, **Vin** too**Value** area.</span></span> <span data-ttu-id="5f04b-211">Seřaďte vizualizace podle **počet vin**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-211">Then sort visualization by **Count of vin**.</span></span>  <span data-ttu-id="5f04b-212">Změna grafu **název** příliš**"Vozidel nutnosti údržby modelem"**</span><span class="sxs-lookup"><span data-stu-id="5f04b-212">Change chart **Title** too**“Vehicles requiring maintenance by model”**</span></span>  

<span data-ttu-id="5f04b-213">Přetáhněte **vin** polí do **sytost barev** u **pole** ![pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) části **vizualizace** karta</span><span class="sxs-lookup"><span data-stu-id="5f04b-213">Drag **vin** fields into **Color Saturation** present at **Fields** ![Fields](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) section of **Visualization** tab</span></span>  
<span data-ttu-id="5f04b-214">![Připojené aut - sytost barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-214">![Connected Cars - Color Saturation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span></span>  

<span data-ttu-id="5f04b-215">Změna **Data barvy** v vizualizace z **formát** části</span><span class="sxs-lookup"><span data-stu-id="5f04b-215">Change **Data Colors** in visualizations from **Format** section</span></span>  
<span data-ttu-id="5f04b-216">Změnit barvu minimální: **F2C812**</span><span class="sxs-lookup"><span data-stu-id="5f04b-216">Change Minimum color to: **F2C812**</span></span>  
<span data-ttu-id="5f04b-217">Změnit barvu maximální: **FF6300**</span><span class="sxs-lookup"><span data-stu-id="5f04b-217">Change Maximum color to: **FF6300**</span></span>  
<span data-ttu-id="5f04b-218">![Připojené aut - změny barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-218">![Connected Cars - Color Changes](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span></span>  
<span data-ttu-id="5f04b-219">![Připojené aut - nové Vizualizační barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-219">![Connected Cars - New Visualization Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span></span>  

<span data-ttu-id="5f04b-220">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-220">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="5f04b-221">Vyberte **Clustered sloupcový graf** z vizualizace, přetáhněte **vin** pole do **hodnotu** oblasti, přetáhněte **města** pole do **osy** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-221">Select **Clustered column chart** from visualizations, drag **vin** field into **Value** area, drag **City** field into **Axis** area.</span></span> <span data-ttu-id="5f04b-222">Graf řazení podle **"Počet vin"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-222">Sort chart by **“Count of vin”**.</span></span> <span data-ttu-id="5f04b-223">Změna grafu **název** příliš**"Vozidel údržby podle města"** </span><span class="sxs-lookup"><span data-stu-id="5f04b-223">Change chart **Title** too**“Vehicles requiring maintenance by city”** </span></span>  
<span data-ttu-id="5f04b-224">![Připojené aut - vozidel údržby podle města](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-224">![Connected Cars - Vehicles requiring maintenance by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span></span>  

<span data-ttu-id="5f04b-225">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-225">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="5f04b-226">Vyberte **více řádků karty** vizualizace z vizualizace, přetáhněte **modelu** a **vin** do hello **pole** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-226">Select **Multi-Row Card** visualization from visualizations, drag **Model** and **vin** into hello **Fields** area.</span></span>  
<span data-ttu-id="5f04b-227">![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-227">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span></span>    

<span data-ttu-id="5f04b-228">Změna uspořádání všechny hello vizualizace, závěrečnou zprávu hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="5f04b-228">Rearranging all of hello visualization, hello final report looks as follows:</span></span>  
![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

<span data-ttu-id="5f04b-230">Úspěšně jste nakonfigurovali v reálném čase sestavy "Vozidel nutnosti Údržba" hello.</span><span class="sxs-lookup"><span data-stu-id="5f04b-230">You have successfully configured hello “Vehicles Requiring Maintenance” real-time report.</span></span> <span data-ttu-id="5f04b-231">Můžete pokračovat v reálném čase sestavy toocreate hello další nebo zde zastavte a konfigurujte hello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="5f04b-231">You can proceed toocreate hello next real-time report or stop here and configure hello dashboard.</span></span> 

### <a name="3-vehicles-health-statistics"></a><span data-ttu-id="5f04b-232">3. Statistiky vozidel stavu</span><span class="sxs-lookup"><span data-stu-id="5f04b-232">3. Vehicles Health Statistics</span></span>
<span data-ttu-id="5f04b-233">Klikněte na tlačítko ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd nové sestavy, přejmenujte ji příliš**"Vozidel stavu statistika"**</span><span class="sxs-lookup"><span data-stu-id="5f04b-233">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd new report, rename it too**“Vehicles Health Statistics”**</span></span>  

<span data-ttu-id="5f04b-234">Vyberte **měřidla** vizualizace z vizualizace, přetáhněte hello **rychlost** pole do **hodnotu, hodnotu minimální, maximální hodnota** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-234">Select **Gauge** visualization from visualizations, then drag hello **Speed** field into **Value, Minimum Value, Maximum Value** areas.</span></span>  
<span data-ttu-id="5f04b-235">![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span><span class="sxs-lookup"><span data-stu-id="5f04b-235">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span></span>  

<span data-ttu-id="5f04b-236">Změňte výchozí agregace hello **rychlost** v **hodnotu oblasti** příliš**průměrná**</span><span class="sxs-lookup"><span data-stu-id="5f04b-236">Change hello default aggregation of **speed** in **Value area** too**Average**</span></span> 

<span data-ttu-id="5f04b-237">Změňte výchozí agregace hello **rychlost** v **minimální oblasti** příliš**minimální**</span><span class="sxs-lookup"><span data-stu-id="5f04b-237">Change hello default aggregation of **speed** in **Minimum area** too**Minimum**</span></span>

<span data-ttu-id="5f04b-238">Změňte výchozí agregace hello **rychlost** v **maximální oblasti** příliš**maximální**</span><span class="sxs-lookup"><span data-stu-id="5f04b-238">Change hello default aggregation of **speed** in **Maximum area** too**Maximum**</span></span>

![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

<span data-ttu-id="5f04b-240">Přejmenujte hello **název měřidla** příliš**"Průměrná rychlost"**</span><span class="sxs-lookup"><span data-stu-id="5f04b-240">Rename hello **Gauge Title** too**“Average speed”**</span></span> 

![Připojené aut - měřidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

<span data-ttu-id="5f04b-242">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-242">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="5f04b-243">Podobně přidat **měřidla** pro **průměrná těžba ropy modul**, **průměrná paliva**, a **průměrná modul mírného**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-243">Similarly add a **Gauge** for **average engine oil**, **average fuel**, and **average engine temperate**.</span></span>  

<span data-ttu-id="5f04b-244">Změňte výchozí agregace hello polí v každé měřidla dle výše uvedených kroků v **"Průměrná rychlost"** měřidla.</span><span class="sxs-lookup"><span data-stu-id="5f04b-244">Change hello default aggregation of fields in each gauge as per above steps in **“Average speed”** gauge.</span></span>

![Připojené aut - měřidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

<span data-ttu-id="5f04b-246">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-246">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="5f04b-247">Vyberte **spojnicový a skupinový sloupcový graf** z vizualizace, přetáhněte **města** pole do **sdílené osy**, přetáhněte **rychlost**, **pole tirepressure a engineoil** do **hodnoty ve sloupcích** oblasti, změnit jejich typ agregace příliš**průměrná**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-247">Select **Line and Clustered Column Chart** from visualizations, then drag **City** field into **Shared Axis**, drag **speed**, **tirepressure and engineoil fields** into **Column Values** area, change their aggregation type too**Average**.</span></span> 

<span data-ttu-id="5f04b-248">Přetáhněte hello **engineTemperature** pole do **hodnoty řádku** oblasti změnit typ agregace hello příliš**průměrná**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-248">Drag hello **engineTemperature** field into **Line Values** area, change hello  aggregation type too**Average**.</span></span> 

![Připojené aut - vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

<span data-ttu-id="5f04b-250">Změna hello grafu **název** příliš**"Průměrná rychlost, můžete zadat naléhavost, modul těžba ropy a teploty modul"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-250">Change hello chart **Title** too**“Average speed, tire pressure, engine oil and engine temperature”**.</span></span>  

![Připojené aut - vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

<span data-ttu-id="5f04b-252">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-252">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="5f04b-253">Vyberte **vlastnosti Treemap** vizualizace z vizualizace, přetáhněte hello **modelu** pole do hello **skupiny** oblasti a přetáhněte pole hello  **MaintenanceProbability** do hello **hodnoty** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-253">Select **Treemap** visualization from visualizations, drag hello **Model** field into hello **Group** area, and drag hello field **MaintenanceProbability** into hello **Values** area.</span></span>

<span data-ttu-id="5f04b-254">Změna hello grafu **název** příliš**"Vehicle modely údržby"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-254">Change hello chart **Title** too**“Vehicle models requiring maintenance”**.</span></span>

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

<span data-ttu-id="5f04b-256">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-256">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="5f04b-257">Vyberte **100 % skládaný sloupcový graf** z vizualizace, přetáhněte hello **města** pole do hello **osy** oblasti a přetáhněte hello **MaintenanceProbability**, **RecallProbability** pole do hello **hodnotu** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-257">Select **100% Stacked Bar Chart** from visualization, drag hello **city** field into hello **Axis** area, and drag hello **MaintenanceProbability**, **RecallProbability** fields into hello **Value** area.</span></span>

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

<span data-ttu-id="5f04b-259">Klikněte na tlačítko **formátu**, vyberte **Data barvy**a sadu hello **MaintenanceProbability** barvu, která hodnota toohello **"F2C80F"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-259">Click **Format**, select **Data Colors**, and set hello **MaintenanceProbability** color toohello value **“F2C80F”**.</span></span>

<span data-ttu-id="5f04b-260">Změna hello **název** z hello grafu příliš**"pravděpodobnosti z Vehicle údržby a odvolat pomocí Město,**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-260">Change hello **Title** of hello chart too**“Probability of Vehicle Maintenance & Recall by City”**.</span></span>

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

<span data-ttu-id="5f04b-262">Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f04b-262">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="5f04b-263">Vyberte **plošný graf** z vizualizace z vizualizace, přetáhněte hello **modelu** pole do hello **osy** oblasti a přetáhněte hello **engineOil, tirepressure, rychlosti a MaintenanceProbability** pole do hello **hodnoty** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-263">Select **Area Chart** from visualization from visualizations, drag hello **Model** field into hello **Axis** area, and drag hello **engineOil, tirepressure, speed and MaintenanceProbability** fields into hello **Values** area.</span></span> <span data-ttu-id="5f04b-264">Změnit jejich typ agregace příliš**"Průměrné"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-264">Change their aggregation type too**“Average”**.</span></span> 

![Připojené aut - změnit typ agregace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

<span data-ttu-id="5f04b-266">Změňte název hello hello grafu příliš**"Průměrná těžba ropy modul, přestanou zatížení, rychlosti a údržba pravděpodobnosti modelem"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-266">Change hello title of hello chart too**“Average engine oil, tire pressure, speed and maintenance probability by model”**.</span></span>

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

<span data-ttu-id="5f04b-268">Klikněte na novou vizualizaci tooadd hello prázdnou oblast:</span><span class="sxs-lookup"><span data-stu-id="5f04b-268">Click hello blank area tooadd new visualization:</span></span>

1. <span data-ttu-id="5f04b-269">Vyberte **bodový graf** vizualizace z vizualizace.</span><span class="sxs-lookup"><span data-stu-id="5f04b-269">Select **Scatter Chart** visualization from visualizations.</span></span>
2. <span data-ttu-id="5f04b-270">Přetáhněte hello **modelu** pole do hello **podrobnosti** a **legendy** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-270">Drag hello **Model** field into hello **Details** and **Legend** area.</span></span>
3. <span data-ttu-id="5f04b-271">Přetáhněte hello **paliva** pole do hello **osy x** oblasti změnit hello agregace příliš**průměrná**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-271">Drag hello **fuel** field into hello **X-Axis** area, change hello aggregation too**Average**.</span></span>
4. <span data-ttu-id="5f04b-272">Přetáhněte **engineTemparature** do **osy y oblasti**, změňte hello agregace příliš**průměrná**</span><span class="sxs-lookup"><span data-stu-id="5f04b-272">Drag **engineTemparature** into **Y-Axis area**, change hello aggregation too**Average**</span></span>
5. <span data-ttu-id="5f04b-273">Přetáhněte hello **vin** pole do hello **velikost** oblasti.</span><span class="sxs-lookup"><span data-stu-id="5f04b-273">Drag hello **vin** field into hello **Size** area.</span></span>

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

<span data-ttu-id="5f04b-275">Změna hello grafu **název** příliš**"Průměry paliva, modul teploty modelem"**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-275">Change hello chart **Title** too**“Averages of Fuel, Engine Temperature by Model”**.</span></span>

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

<span data-ttu-id="5f04b-277">závěrečnou zprávu Hello bude vypadat, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="5f04b-277">hello final report will look like as shown below.</span></span>

![Připojené aut konečné sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-hello-reports-toohello-real-time-dashboard"></a><span data-ttu-id="5f04b-279">Vizualizace kódu PIN z řídicího panelu hello sestavy toohello v reálném čase</span><span class="sxs-lookup"><span data-stu-id="5f04b-279">Pin visualizations from hello reports toohello real-time dashboard</span></span>
<span data-ttu-id="5f04b-280">Kliknutím na další tooDashboards hello plus ikona vytvořte prázdný řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="5f04b-280">Create a blank dashboard by clicking on hello plus icon next tooDashboards.</span></span> <span data-ttu-id="5f04b-281">Pojmenujte ji "Panelu analýzy Telemetrie Vehicle"</span><span class="sxs-lookup"><span data-stu-id="5f04b-281">You can name it “Vehicle Telemetry Analytics Dashboard”</span></span>

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

<span data-ttu-id="5f04b-283">Režim PIN kódu hello vizualizace z hello výše sestavy toohello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="5f04b-283">Pin hello visualization from hello above reports toohello dashboard.</span></span> 

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

<span data-ttu-id="5f04b-285">řídicí panel Hello by měla vypadat takto definovaného toohello řídicí panel po všech hello, které jsou vytvořeny tři sestavy a hello odpovídající vizualizace.</span><span class="sxs-lookup"><span data-stu-id="5f04b-285">hello dashboard should look as follows when all hello three reports are created and hello corresponding visualizations are pinned toohello dashboard.</span></span> <span data-ttu-id="5f04b-286">Pokud jste dosud nevytvořili všechny sestavy hello, může vypadat jinak řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="5f04b-286">If you have not created all hello reports, your dashboard could look different.</span></span> 

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

<span data-ttu-id="5f04b-288">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="5f04b-288">Congratulations!</span></span> <span data-ttu-id="5f04b-289">Úspěšně jste vytvořili v reálném čase řídicí panel hello.</span><span class="sxs-lookup"><span data-stu-id="5f04b-289">You have successfully created hello real-time dashboard.</span></span> <span data-ttu-id="5f04b-290">Jak budete pokračovat, tooexecute CarEventGenerator.exe a RealtimeDashboardApp.exe, měli byste vidět živé aktualizace na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="5f04b-290">As you continue tooexecute CarEventGenerator.exe and RealtimeDashboardApp.exe, you should see live updates on hello dashboard.</span></span> <span data-ttu-id="5f04b-291">Má trvat asi 10 too15 minut toocomplete hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="5f04b-291">It should take about 10 too15 minutes toocomplete hello following steps.</span></span>

## <a name="setup-power-bi-batch-processing-dashboard"></a><span data-ttu-id="5f04b-292">Instalační program řídicí panel Power BI dávkové zpracování</span><span class="sxs-lookup"><span data-stu-id="5f04b-292">Setup Power BI batch processing dashboard</span></span>
> [!NOTE]
> <span data-ttu-id="5f04b-293">Trvá přibližně 2 hodiny (z hello úspěšné dokončení nasazení hello) pro hello end tooend dávkového zpracování kanálu toofinish provádění a zpracovat vygenerovaný data za rok.</span><span class="sxs-lookup"><span data-stu-id="5f04b-293">It takes about two hours (from hello successful completion of hello deployment) for hello end tooend batch processing pipeline toofinish execution and process a year worth of generated data.</span></span> <span data-ttu-id="5f04b-294">Proto počkejte hello zpracování toofinish než budete pokračovat v dalších krocích hello.</span><span class="sxs-lookup"><span data-stu-id="5f04b-294">So wait for hello processing toofinish before proceeding with hello next steps.</span></span> 
> 
> 

<span data-ttu-id="5f04b-295">**Stáhněte si soubor návrháře Power BI hello**</span><span class="sxs-lookup"><span data-stu-id="5f04b-295">**Download hello Power BI designer file**</span></span>

* <span data-ttu-id="5f04b-296">Je součástí nasazení hello pokyny pro ruční provoz předem nakonfigurovaný soubor návrháře Power BI</span><span class="sxs-lookup"><span data-stu-id="5f04b-296">A pre-configured Power BI designer file is included as part of hello deployment Manual Operation Instructions</span></span>
* <span data-ttu-id="5f04b-297">Podívejte se na 2.</span><span class="sxs-lookup"><span data-stu-id="5f04b-297">Look for 2.</span></span> <span data-ttu-id="5f04b-298">Instalační program PowerBI dávkové zpracování řídicí panel si můžete stáhnout hello PowerBI šablonu pro dávkové zpracování řídicí panel zde názvem **ConnectedCarsPbiReport.pbix**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-298">Setup PowerBI batch processing dashboard You can download hello PowerBI template for batch processing dashboard here called **ConnectedCarsPbiReport.pbix**.</span></span>
* <span data-ttu-id="5f04b-299">Uložit místně</span><span class="sxs-lookup"><span data-stu-id="5f04b-299">Save locally</span></span>

<span data-ttu-id="5f04b-300">**Konfigurace sestav Power BI**</span><span class="sxs-lookup"><span data-stu-id="5f04b-300">**Configure Power BI reports**</span></span>

* <span data-ttu-id="5f04b-301">Otevřete hello návrháře souboru '**ConnectedCarsPbiReport.pbix**' pomocí Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="5f04b-301">Open hello designer file ‘**ConnectedCarsPbiReport.pbix**’ using Power BI Desktop.</span></span> <span data-ttu-id="5f04b-302">Pokud již máte, nenainstalujete hello Power BI Desktop z [instalace Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331).</span><span class="sxs-lookup"><span data-stu-id="5f04b-302">If you do not already have, install hello Power BI Desktop from [Power BI Desktop install](http://www.microsoft.com/download/details.aspx?id=45331).</span></span> 
* <span data-ttu-id="5f04b-303">Klikněte na tlačítko hello **upravit dotazy**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-303">Click hello **Edit Queries**.</span></span>

![Upravit dotaz Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* <span data-ttu-id="5f04b-305">Klikněte dvakrát na hello **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-305">Double-click hello **Source**.</span></span>

![Zdroj sady Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* <span data-ttu-id="5f04b-307">Aktualizujte připojovací řetězec serveru hello server Azure SQL, které byly zřízeny jako součást nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="5f04b-307">Update Server connection string with hello Azure SQL server that got provisioned as part of hello deployment.</span></span>  <span data-ttu-id="5f04b-308">Hledat v hello ruční operace pokyny v části</span><span class="sxs-lookup"><span data-stu-id="5f04b-308">Look in hello Manual Operation Instructions under</span></span> 

    4. <span data-ttu-id="5f04b-309">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5f04b-309">Azure SQL Database</span></span>
    
    * <span data-ttu-id="5f04b-310">Server: somethingsrv.database.windows.net</span><span class="sxs-lookup"><span data-stu-id="5f04b-310">Server: somethingsrv.database.windows.net</span></span>
    * <span data-ttu-id="5f04b-311">Databáze: connectedcar</span><span class="sxs-lookup"><span data-stu-id="5f04b-311">Database: connectedcar</span></span>
    * <span data-ttu-id="5f04b-312">Uživatelské jméno: uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5f04b-312">Username: username</span></span>
    * <span data-ttu-id="5f04b-313">Heslo: Heslo SQL serveru můžete spravovat z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5f04b-313">Password: You can manage your SQL server password from Azure portal</span></span>

* <span data-ttu-id="5f04b-314">Nechte **databáze** jako *connectedcar*.</span><span class="sxs-lookup"><span data-stu-id="5f04b-314">Leave **Database** as *connectedcar*.</span></span>

![Nastavte Power BI databázi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* <span data-ttu-id="5f04b-316">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-316">Click **OK**.</span></span>
* <span data-ttu-id="5f04b-317">Zobrazí se **pověření systému Windows** vybraná karta ve výchozím nastavení, změňte jej příliš**databáze pověření** kliknutím na **databáze** kartě vpravo.</span><span class="sxs-lookup"><span data-stu-id="5f04b-317">You will see **Windows credential** tab selected by default, change it too**Database credentials** by clicking on **Database** tab at right.</span></span>
* <span data-ttu-id="5f04b-318">Zadejte hello **uživatelské jméno** a **heslo** vaší databáze SQL Azure, který byl zadán během instalace jeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="5f04b-318">Provide hello **Username** and **Password** of your Azure SQL Database that was specified during its deployment setup.</span></span>

![Zadejte přihlašovací údaje databáze](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* <span data-ttu-id="5f04b-320">Klikněte na tlačítko **připojení**</span><span class="sxs-lookup"><span data-stu-id="5f04b-320">Click **Connect**</span></span>
* <span data-ttu-id="5f04b-321">Hello výše kroky opakujte pro každý hello tři zbývající dotazů nachází v pravém podokně a pak aktualizujte podrobnosti připojení zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="5f04b-321">Repeat hello above steps for each of hello three remaining queries present at right pane, and then update hello data source connection details.</span></span>
* <span data-ttu-id="5f04b-322">Klikněte na tlačítko **zavřete a načíst**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-322">Click **Close and Load**.</span></span> <span data-ttu-id="5f04b-323">Power BI Desktop souboru datové sady jsou připojené tooSQL Azure databázových tabulek.</span><span class="sxs-lookup"><span data-stu-id="5f04b-323">Power BI Desktop file datasets are connected tooSQL Azure Database tables.</span></span>
* <span data-ttu-id="5f04b-324">**Zavřít** souboru Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="5f04b-324">**Close** Power BI Desktop file.</span></span>

![Zavřít Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* <span data-ttu-id="5f04b-326">Klikněte na tlačítko **Uložit** tlačítko toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="5f04b-326">Click **Save** button toosave hello changes.</span></span> 

<span data-ttu-id="5f04b-327">Nyní máte nakonfigurovaný všechny sestavy hello odpovídající toohello dávkové zpracování cestu hello řešení.</span><span class="sxs-lookup"><span data-stu-id="5f04b-327">You have now configured all hello reports corresponding toohello batch processing path in hello solution.</span></span> 

## <a name="upload-toopowerbicom"></a><span data-ttu-id="5f04b-328">Nahrát příliš*powerbi.com*</span><span class="sxs-lookup"><span data-stu-id="5f04b-328">Upload too*powerbi.com*</span></span>
1. <span data-ttu-id="5f04b-329">Přejděte toohello Power BI webový portál na http://powerbi.com a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5f04b-329">Navigate toohello Power BI web portal at http://powerbi.com and login.</span></span>
2. <span data-ttu-id="5f04b-330">Klikněte na tlačítko **získat Data**</span><span class="sxs-lookup"><span data-stu-id="5f04b-330">Click **Get Data**</span></span>  
3. <span data-ttu-id="5f04b-331">Nahrajte hello soubor Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="5f04b-331">Upload hello Power BI Desktop File.</span></span>  
4. <span data-ttu-id="5f04b-332">tooupload, klikněte na tlačítko **načíst Data -> získat soubory -> místního souboru**</span><span class="sxs-lookup"><span data-stu-id="5f04b-332">tooupload, click **Get Data -> Files Get -> Local file**</span></span>  
5. <span data-ttu-id="5f04b-333">Přejděte toohello **"**ConnectedCarsPbiReport.pbix**"**</span><span class="sxs-lookup"><span data-stu-id="5f04b-333">Navigate toohello **“**ConnectedCarsPbiReport.pbix**”**</span></span>  
6. <span data-ttu-id="5f04b-334">Po nahrání souboru hello bude navigaci back tooyour Power BI pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="5f04b-334">Once hello file is uploaded, you will be navigated back tooyour Power BI work space.</span></span>  

<span data-ttu-id="5f04b-335">Datové sady, sestavy a řídicí panel prázdné, bude vytvořena pro vás.</span><span class="sxs-lookup"><span data-stu-id="5f04b-335">A dataset, report and a blank dashboard will be created for you.</span></span>  

<span data-ttu-id="5f04b-336">PIN kód grafy tooa novým řídicím panelem názvem **panelu analýzy Telemetrie Vehicle** v **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="5f04b-336">Pin charts tooa new dashboard called **Vehicle Telemetry Analytics Dashboard** in **Power BI**.</span></span> <span data-ttu-id="5f04b-337">Klikněte na hello prázdný řídicí panel, vytvořili výše a potom přejděte toohello **sestavy** klikněte na část hello nově nahrát sestavu.</span><span class="sxs-lookup"><span data-stu-id="5f04b-337">Click hello blank dashboard created above and then navigate toohello **Reports** section click hello newly uploaded report.</span></span>  

![Vehicle Telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

<span data-ttu-id="5f04b-339">**Poznámka: hello sestava obsahuje šest stránky:**</span><span class="sxs-lookup"><span data-stu-id="5f04b-339">**Note hello report has six pages:**</span></span>  
<span data-ttu-id="5f04b-340">Stránka 1: Hustotu Vehicle</span><span class="sxs-lookup"><span data-stu-id="5f04b-340">Page 1: Vehicle density</span></span>  
<span data-ttu-id="5f04b-341">Stránka 2: Stav v reálném čase vehicle</span><span class="sxs-lookup"><span data-stu-id="5f04b-341">Page 2: Real-time vehicle health</span></span>  
<span data-ttu-id="5f04b-342">Stránka 3: Intenzivně řízené vozidel</span><span class="sxs-lookup"><span data-stu-id="5f04b-342">Page 3: Aggressively Driven Vehicles</span></span>   
<span data-ttu-id="5f04b-343">Stránka 4: Připomenout vozidel</span><span class="sxs-lookup"><span data-stu-id="5f04b-343">Page 4: Recalled vehicles</span></span>  
<span data-ttu-id="5f04b-344">5 stránky: Efektivní řízené vozidel paliva</span><span class="sxs-lookup"><span data-stu-id="5f04b-344">Page 5: Fuel Efficiently Driven Vehicles</span></span>  
<span data-ttu-id="5f04b-345">6 stránky: Logo společnosti Contoso</span><span class="sxs-lookup"><span data-stu-id="5f04b-345">Page 6: Contoso Logo</span></span>  

![Připojené aut Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

<span data-ttu-id="5f04b-347">**Ze stránky 3**, připnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="5f04b-347">**From Page 3**, pin hello following:</span></span>  

1. <span data-ttu-id="5f04b-348">Počet VIN</span><span class="sxs-lookup"><span data-stu-id="5f04b-348">Count of VIN</span></span>  
   ![Připojené aut Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. <span data-ttu-id="5f04b-350">Intenzivně řídí vozidel modelu – vodopádu grafu</span><span class="sxs-lookup"><span data-stu-id="5f04b-350">Aggressively driven vehicles by model – Waterfall chart</span></span>  
   ![Vehicle Telemetrie – Pin grafy 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

<span data-ttu-id="5f04b-352">**Ze stránky 5**, připnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="5f04b-352">**From Page 5**, pin hello following:</span></span> 

1. <span data-ttu-id="5f04b-353">Počet vin</span><span class="sxs-lookup"><span data-stu-id="5f04b-353">Count of vin</span></span>    
   ![Vehicle Telemetrie – Pin grafy 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. <span data-ttu-id="5f04b-355">Paliva efektivní vozidel modelem: Skupinový sloupcový graf</span><span class="sxs-lookup"><span data-stu-id="5f04b-355">Fuel efficient vehicles by model: Clustered column chart</span></span>  
   ![Vehicle Telemetrie – Pin grafy 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

<span data-ttu-id="5f04b-357">**Stránka 4**, připnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="5f04b-357">**From Page 4**, pin hello following:</span></span>  

1. <span data-ttu-id="5f04b-358">Počet vin</span><span class="sxs-lookup"><span data-stu-id="5f04b-358">Count of vin</span></span>  
   ![Vehicle Telemetrie – Pin grafy 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. <span data-ttu-id="5f04b-360">Odvolané vozidel podle města, model: vlastnosti Treemap</span><span class="sxs-lookup"><span data-stu-id="5f04b-360">Recalled vehicles by city, model: Treemap</span></span>  
   ![Vehicle Telemetrie – Pin grafy 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

<span data-ttu-id="5f04b-362">**Ze stránky 6**, připnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="5f04b-362">**From Page 6**, pin hello following:</span></span>  

1. <span data-ttu-id="5f04b-363">Logo společnosti Contoso motory</span><span class="sxs-lookup"><span data-stu-id="5f04b-363">Contoso Motors logo</span></span>  
   ![Vehicle Telemetrie – Pin grafy 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

<span data-ttu-id="5f04b-365">**Uspořádání hello řídicí panel**</span><span class="sxs-lookup"><span data-stu-id="5f04b-365">**Organize hello dashboard**</span></span>  

1. <span data-ttu-id="5f04b-366">Přejděte toohello řídicí panel</span><span class="sxs-lookup"><span data-stu-id="5f04b-366">Navigate toohello dashboard</span></span>
2. <span data-ttu-id="5f04b-367">Pozastavte ukazatel myši nad každým grafem a přejmenujte ji podle hello pojmenování uvedené v následující obrázek hello kompletní řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="5f04b-367">Hover over each chart and rename it based on hello naming provided in hello complete dashboard image below.</span></span> <span data-ttu-id="5f04b-368">Také přesunete hello grafy kolem toolook jako řídicí panel hello níže.</span><span class="sxs-lookup"><span data-stu-id="5f04b-368">Also move hello charts around toolook like hello dashboard below.</span></span>  
   ![Vehicle Telemetrie – uspořádání řídicí panel 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle Telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. <span data-ttu-id="5f04b-371">Pokud jste vytvořili všechny hello sestavy, jak je uvedeno v tomto dokumentu, hello poslední dokončené řídicí panel by měl vypadat jako hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="5f04b-371">If you have created all hello reports as mentioned in this document, hello final completed dashboard should look like hello following figure.</span></span> 

![Vehicle Telemetrie – uspořádání řídicí panel 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

<span data-ttu-id="5f04b-373">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="5f04b-373">Congratulations!</span></span> <span data-ttu-id="5f04b-374">Úspěšně jste vytvořili hello sestavy a řídicí panel toogain v reálném čase, prediktivní hello a zvyklosti batch Statistika na vehicle stavu a řídí.</span><span class="sxs-lookup"><span data-stu-id="5f04b-374">You have successfully created hello reports and hello dashboard toogain real-time, predictive and batch insights on vehicle health and driving habits.</span></span>  
