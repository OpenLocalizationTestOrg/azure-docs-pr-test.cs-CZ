---
title: "Použít místní SQL Server v Azure Machine Learning | Microsoft Docs"
description: "Pomocí dat z databáze SQL serveru místní provádět pokročilé analýzy pomocí Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 08e4610d-02b6-4071-aad7-a2340ad8e2ea
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: garye;krishnan
ms.openlocfilehash: ec1620be2aaf4339ba9d22540ffb2239d0678b70
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a><span data-ttu-id="79bd0-103">Provádění pokročilých analýz dat místní databáze SQL Serveru pomocí služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="79bd0-103">Perform advanced analytics with Azure Machine Learning using data from an on-premises SQL Server database</span></span>

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

<span data-ttu-id="79bd0-104">Často podnikům, které pracují s místními daty chtěli využívat měřítka a flexibility cloudu pro jejich strojového učení úlohy.</span><span class="sxs-lookup"><span data-stu-id="79bd0-104">Often enterprises that work with on-premises data would like to take advantage of the scale and agility of the cloud for their machine learning workloads.</span></span> <span data-ttu-id="79bd0-105">Ale nechcete použít narušit jejich aktuální podnikové procesy a pracovní postupy přesunutím jejich místní data do cloudu.</span><span class="sxs-lookup"><span data-stu-id="79bd0-105">But they don't want to disrupt their current business processes and workflows by moving their on-premises data to the cloud.</span></span> <span data-ttu-id="79bd0-106">Azure Machine Learning teď podporuje čtení dat z databáze SQL serveru na místě a pak školení a vyhodnocování model se tato data.</span><span class="sxs-lookup"><span data-stu-id="79bd0-106">Azure Machine Learning now supports reading your data from an on-premises SQL Server database and then training and scoring a model with this data.</span></span> <span data-ttu-id="79bd0-107">Už máte ručně zkopírujte a synchronizaci dat mezi cloudu a místní server.</span><span class="sxs-lookup"><span data-stu-id="79bd0-107">You no longer have to manually copy and sync the data between the cloud and your on-premises server.</span></span> <span data-ttu-id="79bd0-108">Místo toho **importovat Data** modulu v Azure Machine Learning Studio nyní může načíst přímo z vaší místní databáze SQL serveru pro váš školení a vyhodnocování úlohy.</span><span class="sxs-lookup"><span data-stu-id="79bd0-108">Instead, the **Import Data** module in Azure Machine Learning Studio can now read directly from your on-premises SQL Server database for your training and scoring jobs.</span></span>

<span data-ttu-id="79bd0-109">Tento článek obsahuje přehled o tom, jak příchozího místní data SQL serveru do Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="79bd0-109">This article provides an overview of how to ingress on-premises SQL server data into Azure Machine Learning.</span></span> <span data-ttu-id="79bd0-110">Předpokládá, že jste obeznámeni s koncepty Azure Machine Learning pracovních prostorů, moduly, datové sady, experimenty, *atd*.</span><span class="sxs-lookup"><span data-stu-id="79bd0-110">It assumes that you're familiar with Azure Machine Learning concepts like workspaces, modules, datasets, experiments, *etc.*.</span></span>

> [!NOTE]
> <span data-ttu-id="79bd0-111">Tato funkce není k dispozici pro bezplatné pracovní prostory.</span><span class="sxs-lookup"><span data-stu-id="79bd0-111">This feature is not available for free workspaces.</span></span> <span data-ttu-id="79bd0-112">Další informace o cenách Machine Learning a úrovně najdete v tématu [Azure Machine Learning – ceny](https://azure.microsoft.com/pricing/details/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="79bd0-112">For more information about Machine Learning pricing and tiers, see [Azure Machine Learning Pricing](https://azure.microsoft.com/pricing/details/machine-learning/).</span></span>
>
>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="install-the-microsoft-data-management-gateway"></a><span data-ttu-id="79bd0-113">Nainstalujte bránu pro správu dat společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="79bd0-113">Install the Microsoft Data Management Gateway</span></span>
<span data-ttu-id="79bd0-114">Pro přístup místní databázi systému SQL Server v Azure Machine Learning, musíte stáhnout a nainstalovat bránu pro správu dat společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="79bd0-114">To access an on-premises SQL Server database in Azure Machine Learning, you need to download and install the Microsoft Data Management Gateway.</span></span> <span data-ttu-id="79bd0-115">Když konfigurujete připojení brány v nástroji Machine Learning Studio, máte možnost stáhnout a nainstalovat bránu **stahování a registrace brány dat** dialogové okno popsané dole.</span><span class="sxs-lookup"><span data-stu-id="79bd0-115">When you configure the gateway connection in Machine Learning Studio, you have the opportunity to download and install the gateway using the **Download and register data gateway** dialog described below.</span></span>

<span data-ttu-id="79bd0-116">Také můžete nainstalovat brána pro správu dat předem stažení a spuštění instalačního balíčku MSI z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span><span class="sxs-lookup"><span data-stu-id="79bd0-116">You also can install the Data Management Gateway ahead of time by downloading and running the MSI setup package from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span></span>
<span data-ttu-id="79bd0-117">Vyberte nejnovější verzi, výběr 32bitové nebo 64bitové vhodnou pro váš počítač.</span><span class="sxs-lookup"><span data-stu-id="79bd0-117">Choose the latest version, selecting either 32-bit or 64-bit as appropriate for your computer.</span></span> <span data-ttu-id="79bd0-118">Soubor MSI můžete také použít k upgradu existující brána správy dat na nejnovější verzi, se všemi možnými nastavení zachovaná.</span><span class="sxs-lookup"><span data-stu-id="79bd0-118">The MSI can also be used to upgrade an existing Data Management Gateway to the latest version, with all settings preserved.</span></span>

<span data-ttu-id="79bd0-119">Bránu má následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="79bd0-119">The gateway has the following prerequisites:</span></span>

* <span data-ttu-id="79bd0-120">Podporované verze operačního systému Windows jsou Windows 7, Windows 8 nebo 8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="79bd0-120">The supported Windows operating system versions are Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, and Windows Server 2012 R2.</span></span>
* <span data-ttu-id="79bd0-121">Doporučenou konfiguraci pro počítače brány je alespoň 2 GHz, 4 jádra, 8GB paměti RAM a 80 GB místa na disku.</span><span class="sxs-lookup"><span data-stu-id="79bd0-121">The recommended configuration for the gateway machine is at least 2 GHz, 4 cores, 8GB RAM, and 80GB disk.</span></span>
* <span data-ttu-id="79bd0-122">Pokud hostitelský počítač přejde do režimu spánku, brána nebude odpovídat na požadavky na data.</span><span class="sxs-lookup"><span data-stu-id="79bd0-122">If the host machine hibernates, the gateway won’t respond to data requests.</span></span> <span data-ttu-id="79bd0-123">Proto konfigurovat plán odpovídající napájení v počítači před instalací brány.</span><span class="sxs-lookup"><span data-stu-id="79bd0-123">Therefore, configure an appropriate power plan on the computer before installing the gateway.</span></span> <span data-ttu-id="79bd0-124">Pokud je počítač nakonfigurovaný do režimu spánku, brána instalace zobrazí zprávu.</span><span class="sxs-lookup"><span data-stu-id="79bd0-124">If the machine is configured to hibernate, the gateway installation displays a message.</span></span>
* <span data-ttu-id="79bd0-125">Protože aktivity kopírování dojde k určité frekvencí, využití prostředků (procesor, paměť) na počítači také používá se stejný vzor s ve špičce a doby nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="79bd0-125">Because copy activity occurs at a specific frequency, the resource usage (CPU, memory) on the machine also follows the same pattern with peak and idle times.</span></span> <span data-ttu-id="79bd0-126">Využití prostředků závisí také výraznou na množství dat přesouvá.</span><span class="sxs-lookup"><span data-stu-id="79bd0-126">Resource utilization also depends heavily on the amount of data being moved.</span></span> <span data-ttu-id="79bd0-127">Pokud více úloh kopie jsou v průběhu, budete sledovat využití prostředků během špiček přejděte.</span><span class="sxs-lookup"><span data-stu-id="79bd0-127">When multiple copy jobs are in progress, you'll observe resource usage go up during peak times.</span></span> <span data-ttu-id="79bd0-128">Minimální konfigurace uvedené výše je technicky dostatečná, můžete v konfiguraci s více prostředků než minimální konfigurace v závislosti na konkrétní zatížení pro přesun dat.</span><span class="sxs-lookup"><span data-stu-id="79bd0-128">While the minimum configuration listed above is technically sufficient, you may want to have a configuration with more resources than the minimum configuration depending on your specific load for data movement.</span></span>

<span data-ttu-id="79bd0-129">Vezměte v úvahu následující při nastavování a používání Brána pro správu dat:</span><span class="sxs-lookup"><span data-stu-id="79bd0-129">Consider the following when setting up and using a Data Management Gateway:</span></span>

* <span data-ttu-id="79bd0-130">Můžete nainstalovat pouze jedna instance brány pro správu dat v jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="79bd0-130">You can install only one instance of Data Management Gateway on a single computer.</span></span>
* <span data-ttu-id="79bd0-131">Jedna brána můžete použít pro více zdrojů dat na místě.</span><span class="sxs-lookup"><span data-stu-id="79bd0-131">You can use a single gateway for multiple on-premises data sources.</span></span>
* <span data-ttu-id="79bd0-132">Více bran na různých počítačích můžete připojit ke stejnému zdroji dat na místě.</span><span class="sxs-lookup"><span data-stu-id="79bd0-132">You can connect multiple gateways on different computers to the same on-premises data source.</span></span>
* <span data-ttu-id="79bd0-133">Nakonfigurovat bránu pouze jednoho pracovního prostoru v čase.</span><span class="sxs-lookup"><span data-stu-id="79bd0-133">You configure a gateway for only one workspace at a time.</span></span> <span data-ttu-id="79bd0-134">V současné době brány se nedají sdílet mezi pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="79bd0-134">Currently, gateways can’t be shared across workspaces.</span></span>
* <span data-ttu-id="79bd0-135">Můžete nakonfigurovat více bran pro jednoho pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="79bd0-135">You can configure multiple gateways for a single workspace.</span></span> <span data-ttu-id="79bd0-136">Můžete například použít bránu, která je připojena k testu zdroje dat během vývoje a produkční brány, až budete připraveni zprovoznit.</span><span class="sxs-lookup"><span data-stu-id="79bd0-136">For example, you may want to use a gateway that's connected to your test data sources during development and a production gateway when you're ready to operationalize.</span></span>
* <span data-ttu-id="79bd0-137">Brána nemusí být ve stejném počítači jako zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="79bd0-137">The gateway does not need to be on the same machine as the data source.</span></span> <span data-ttu-id="79bd0-138">Ale zachování co nejblíže ke zdroji dat se snižuje čas pro bránu pro připojení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="79bd0-138">But staying closer to the data source reduces the time for the gateway to connect to the data source.</span></span> <span data-ttu-id="79bd0-139">Doporučujeme nainstalovat bránu na počítači, který se liší od toho, který je hostitelem místní zdroj dat tak, aby brána a datový zdroj není pokouší pro prostředky.</span><span class="sxs-lookup"><span data-stu-id="79bd0-139">We recommend that you install the gateway on a machine that's different from the one that hosts the on-premises data source so that the gateway and data source don't compete for resources.</span></span>
* <span data-ttu-id="79bd0-140">Pokud je již nainstalovaná ve vašem počítači obsluhující Power BI nebo Azure Data Factory scénáře, nainstalujte samostatnou bránu pro Azure Machine Learning na jiném počítači.</span><span class="sxs-lookup"><span data-stu-id="79bd0-140">If you already have a gateway installed on your computer serving Power BI or Azure Data Factory scenarios, install a separate gateway for Azure Machine Learning on another computer.</span></span>

  > [!NOTE]
  > <span data-ttu-id="79bd0-141">Brána pro správu dat a Power BI Gateway nelze spustit na stejném počítači.</span><span class="sxs-lookup"><span data-stu-id="79bd0-141">You can't run Data Management Gateway and Power BI Gateway on the same computer.</span></span>
  >
  >
* <span data-ttu-id="79bd0-142">Budete muset použít Brána pro správu dat pro Azure Machine Learning, i když používáte Azure ExpressRoute pro další data.</span><span class="sxs-lookup"><span data-stu-id="79bd0-142">You need to use the Data Management Gateway for Azure Machine Learning even if you are using Azure ExpressRoute for other data.</span></span> <span data-ttu-id="79bd0-143">Zdroje dat by měly zpracovávat jako místní zdroj dat (který je za bránou firewall) i při použití ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="79bd0-143">You should treat your data source as an on-premises data source (that's behind a firewall) even when you use ExpressRoute.</span></span> <span data-ttu-id="79bd0-144">Brána pro správu dat použijte k navázání připojení mezi Machine Learning a zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="79bd0-144">Use the Data Management Gateway to establish connectivity between Machine Learning and the data source.</span></span>

<span data-ttu-id="79bd0-145">Podrobné informace o požadavky pro instalaci, kroky instalace a tipy k řešení potíží najdete v článku [Brána pro správu dat](../data-factory/data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="79bd0-145">You can find detailed information on installation prerequisites, installation steps, and troubleshooting tips in the article [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md).</span></span>

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span data-ttu-id="79bd0-146"><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Příchozí přenos dat z vaší místní databáze SQL serveru do Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="79bd0-146"><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Ingress data from your on-premises SQL Server database into Azure Machine Learning</span></span>
<span data-ttu-id="79bd0-147">V tomto návodu nastavit Brána pro správu dat v pracovním prostoru Azure Machine Learning, nakonfigurovat ji a potom číst data z databáze SQL serveru místní.</span><span class="sxs-lookup"><span data-stu-id="79bd0-147">In this walkthrough, you will set up a Data Management Gateway in an Azure Machine Learning workspace, configure it, and then read data from an on-premises SQL Server database.</span></span>

> [!TIP]
> <span data-ttu-id="79bd0-148">Než začnete, zakažte blokování automaticky otevíraných oken v prohlížeči pro `studio.azureml.net`.</span><span class="sxs-lookup"><span data-stu-id="79bd0-148">Before you start, disable your browser’s pop-up blocker for `studio.azureml.net`.</span></span> <span data-ttu-id="79bd0-149">Pokud používáte prohlížeč Google Chrome, stáhněte a nainstalujte jedním z několika moduly plug-in k dispozici na webu Google Chrome WebStore [klikněte jednou rozšíření aplikace](https://chrome.google.com/webstore/search/clickonce?_category=extensions).</span><span class="sxs-lookup"><span data-stu-id="79bd0-149">If you're using the Google Chrome browser, download and install one of the several plug-ins available at Google Chrome WebStore [Click Once App Extension](https://chrome.google.com/webstore/search/clickonce?_category=extensions).</span></span>
>
>

### <a name="step-1-create-a-gateway"></a><span data-ttu-id="79bd0-150">Krok 1: Vytvoření brány</span><span class="sxs-lookup"><span data-stu-id="79bd0-150">Step 1: Create a gateway</span></span>
<span data-ttu-id="79bd0-151">Prvním krokem je vytvoření a nastavení brány pro přístup k vaší databázi SQL na místě.</span><span class="sxs-lookup"><span data-stu-id="79bd0-151">The first step is to create and set up the gateway to access your on-premises SQL database.</span></span>

1. <span data-ttu-id="79bd0-152">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net/Home/) a vyberte pracovní prostor, který chcete, aby fungovaly v.</span><span class="sxs-lookup"><span data-stu-id="79bd0-152">Log in to [Azure Machine Learning  Studio](https://studio.azureml.net/Home/) and select the workspace  that you want to work in.</span></span>
2. <span data-ttu-id="79bd0-153">Klikněte na **nastavení** okno na levé straně a klikněte **brány DATA GATEWAYS** v horní části.</span><span class="sxs-lookup"><span data-stu-id="79bd0-153">Click the **SETTINGS** blade on the left, and then click the **DATA  GATEWAYS** tab at the top.</span></span>
3. <span data-ttu-id="79bd0-154">Klikněte na tlačítko **novou BRÁNU dat** v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="79bd0-154">Click **NEW DATA GATEWAY** at the bottom of the screen.</span></span>

    ![Nová brána dat](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. <span data-ttu-id="79bd0-156">V **novou bránu dat** dialogové okno, zadejte **název brány** a volitelně přidejte **popis**.</span><span class="sxs-lookup"><span data-stu-id="79bd0-156">In the **New data gateway** dialog, enter the **Gateway Name** and optionally add a **Description**.</span></span> <span data-ttu-id="79bd0-157">Klikněte na šipku v pravém dolním přejít k dalšímu kroku konfigurace.</span><span class="sxs-lookup"><span data-stu-id="79bd0-157">Click the arrow on the bottom right-hand corner to go to the next step of the configuration.</span></span>

    ![Zadejte název brány a popis](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. <span data-ttu-id="79bd0-159">Stažení a zaregistrujte bránu dialog datových zkopírujte do schránky registrační klíč brány.</span><span class="sxs-lookup"><span data-stu-id="79bd0-159">In the Download and register data gateway dialog, copy the GATEWAY  REGISTRATION KEY to the clipboard.</span></span>

    ![Stáhněte si a registraci brány dat](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span data-ttu-id="79bd0-161"><span id="note-1" class="anchor"></span>Pokud jste ještě stáhli a nainstalovali jste Brána pro správu dat společnosti Microsoft, klepněte na tlačítko **Brána pro správu dat stažení**.</span><span class="sxs-lookup"><span data-stu-id="79bd0-161"><span id="note-1" class="anchor"></span>If you have not yet downloaded and installed the Microsoft Data Management Gateway, then click **Download data management gateway**.</span></span> <span data-ttu-id="79bd0-162">Tím přejdete na Microsoft Download Center, kde můžete vybrat verzi brány, kterou budete potřebovat, stáhněte ho a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="79bd0-162">This takes you to the Microsoft Download Center where you can select the gateway version you need, download it, and install it.</span></span> <span data-ttu-id="79bd0-163">Podrobné informace o požadavky pro instalaci, kroky instalace a tipy k řešení potíží najdete v částech začátku článku [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) .</span><span class="sxs-lookup"><span data-stu-id="79bd0-163">You can find detailed information on installation prerequisites, installation steps, and troubleshooting tips in the beginning sections of the article [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).</span></span>
7. <span data-ttu-id="79bd0-164">Poté, co je brána nainstalovaná, otevře se Správce konfigurace brány pro správu dat a **registrace brány** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79bd0-164">After the gateway is installed, the Data Management Gateway Configuration Manager will open and the **Register gateway** dialog is displayed.</span></span> <span data-ttu-id="79bd0-165">Vložení **registrační klíč brány** jste zkopírovali do schránky a klikněte na tlačítko **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="79bd0-165">Paste the **Gateway Registration Key** that you copied to the clipboard and click **Register**.</span></span>
8. <span data-ttu-id="79bd0-166">Pokud již máte nainstalovaná, spusťte Správce konfigurace brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="79bd0-166">If you already have a gateway installed, run the Data Management Gateway Configuration Manager.</span></span> <span data-ttu-id="79bd0-167">Klikněte na tlačítko **změnit klíč**, vložte **registrační klíč brány** který jste zkopírovali do schránky v předchozím kroku a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="79bd0-167">Click **Change key**, paste the **Gateway Registration Key** that you copied to the clipboard in the previous step, and click **OK**.</span></span>
9. <span data-ttu-id="79bd0-168">Po dokončení instalace **registrace brány** zobrazí dialog pro Microsoft Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="79bd0-168">When the installation is complete, the **Register gateway** dialog for Microsoft Data Management Gateway Configuration Manager is displayed.</span></span> <span data-ttu-id="79bd0-169">Vložte registrační klíč brány, kterou jste zkopírovali do schránky v předchozím kroku a klikněte na tlačítko **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="79bd0-169">Paste the GATEWAY REGISTRATION KEY that you copied to the clipboard in a previous step and click **Register**.</span></span>

    ![Registraci brány](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. <span data-ttu-id="79bd0-171">Když následující hodnoty jsou nastavené na dokončení konfigurace brány **Domů** kartě v Microsoft Data Management Gateway Configuration Manager:</span><span class="sxs-lookup"><span data-stu-id="79bd0-171">The gateway configuration is complete when the following values are set on the **Home** tab in Microsoft Data Management Gateway Configuration Manager:</span></span>

    * <span data-ttu-id="79bd0-172">**Název brány** a **název Instance** jsou nastavena na název brány.</span><span class="sxs-lookup"><span data-stu-id="79bd0-172">**Gateway name** and **Instance name** are set to the name of   the gateway.</span></span>
    * <span data-ttu-id="79bd0-173">**Registrace** je nastaven na **registrovaná**.</span><span class="sxs-lookup"><span data-stu-id="79bd0-173">**Registration** is set to **Registered**.</span></span>
    * <span data-ttu-id="79bd0-174">**Stav** je nastaven na **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="79bd0-174">**Status** is set to **Started**.</span></span>
    * <span data-ttu-id="79bd0-175">Stavový řádek v dolní zobrazí **připojené ke cloudové službě brány správy dat** společně s zelená značka zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="79bd0-175">The status bar at the bottom displays **Connected to Data   Management Gateway Cloud Service** along with a green   check mark.</span></span>

      ![Správce brány pro správu dat](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      <span data-ttu-id="79bd0-177">Azure Machine Learning Studio získá také aktualizovat, po úspěšné registraci.</span><span class="sxs-lookup"><span data-stu-id="79bd0-177">Azure Machine Learning Studio also gets updated when the registration is successful.</span></span>

    ![Registrace brány úspěšné](media/machine-learning-use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. <span data-ttu-id="79bd0-179">V **stáhnout a bránu dat zaregistrujte** dialogové okno, kliknutím na značku zaškrtnutí dokončete nastavení.</span><span class="sxs-lookup"><span data-stu-id="79bd0-179">In the **Download and register data gateway** dialog, click the check mark to complete the setup.</span></span> <span data-ttu-id="79bd0-180">**Nastavení** stránce se zobrazuje stav brány jako "Online".</span><span class="sxs-lookup"><span data-stu-id="79bd0-180">The **Settings** page displays the gateway status as "Online".</span></span> <span data-ttu-id="79bd0-181">V pravém podokně zjistíte stav a další užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="79bd0-181">In the right-hand pane, you'll find status and other useful information.</span></span>

    ![Nastavení brány](media/machine-learning-use-data-from-an-on-premises-sql-server/gateway-status.png)
12. <span data-ttu-id="79bd0-183">V Microsoft Data Management Gateway Správce konfigurace přepnout **certifikát** kartě.</span><span class="sxs-lookup"><span data-stu-id="79bd0-183">In the Microsoft Data Management Gateway Configuration Manager switch to the **Certificate** tab.</span></span> <span data-ttu-id="79bd0-184">Certifikát uvedený na této kartě se používá k šifrování a dešifrování přihlašovací údaje pro místní datové úložiště, které zadáte na portálu.</span><span class="sxs-lookup"><span data-stu-id="79bd0-184">The certificate specified on this tab is used to encrypt/decrypt credentials for the on-premises data store that you specify in the portal.</span></span> <span data-ttu-id="79bd0-185">Tento certifikát je výchozí certifikát.</span><span class="sxs-lookup"><span data-stu-id="79bd0-185">This certificate is the default certificate.</span></span> <span data-ttu-id="79bd0-186">Společnost Microsoft doporučuje, tato změna na vlastní certifikát, který zálohování v systému správy certifikátů.</span><span class="sxs-lookup"><span data-stu-id="79bd0-186">Microsoft recommends changing this to your own certificate that you back up in your certificate management system.</span></span> <span data-ttu-id="79bd0-187">Klikněte na tlačítko **změnu** místo toho použít vlastní certifikát.</span><span class="sxs-lookup"><span data-stu-id="79bd0-187">Click **Change** to use your own certificate instead.</span></span>

    ![Certifikát brány změn](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. <span data-ttu-id="79bd0-189">(volitelné) Pokud chcete povolit podrobné protokolování při řešení problémů s bránou v Microsoft Data Management Gateway Správce konfigurace přepnout **diagnostiky** kartě a zkontrolujte **povolit podrobné protokolování pro účely odstraňování potíží** možnost.</span><span class="sxs-lookup"><span data-stu-id="79bd0-189">(optional) If you want to enable verbose logging in order to troubleshoot issues with the gateway, in the Microsoft Data Management Gateway Configuration Manager switch to the **Diagnostics** tab and check the **Enable verbose logging for troubleshooting purposes** option.</span></span> <span data-ttu-id="79bd0-190">Informace o protokolování naleznete v prohlížeči událostí systému Windows v části **protokoly aplikací a služeb**  - &gt; **Brána pro správu dat** uzlu.</span><span class="sxs-lookup"><span data-stu-id="79bd0-190">The logging information can be found in the Windows Event Viewer under the **Applications and Services Logs** -&gt; **Data Management Gateway** node.</span></span> <span data-ttu-id="79bd0-191">Můžete také **diagnostiky** kartě, abyste otestovali připojení ke zdroji dat místní používání brány.</span><span class="sxs-lookup"><span data-stu-id="79bd0-191">You can also use the **Diagnostics** tab to test the connection to an on-premises data source using the gateway.</span></span>

    ![Povolit podrobné protokolování](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

<span data-ttu-id="79bd0-193">Dokončení procesu instalace brány v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="79bd0-193">This completes the gateway setup process in Azure Machine Learning.</span></span>
<span data-ttu-id="79bd0-194">Nyní jste připraveni použít místní data.</span><span class="sxs-lookup"><span data-stu-id="79bd0-194">You're now ready to use your on-premises data.</span></span>

<span data-ttu-id="79bd0-195">Můžete vytvořit a nastavit více bran v Studio pro každý pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="79bd0-195">You can create and set up multiple gateways in Studio for each workspace.</span></span> <span data-ttu-id="79bd0-196">Například může mít bránu, která chcete připojit ke zdrojům dat vaše testovací během vývoje a jinou bránu pro zdroje dat produkční.</span><span class="sxs-lookup"><span data-stu-id="79bd0-196">For example, you may have a gateway that you want to connect to your test data sources during development, and a different gateway for your production data sources.</span></span> <span data-ttu-id="79bd0-197">Azure Machine Learning vám dává možnost nastavit více bran, v závislosti na vašem podnikovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="79bd0-197">Azure Machine Learning gives you the flexibility to set up multiple gateways depending upon your corporate environment.</span></span> <span data-ttu-id="79bd0-198">V současné době nelze sdílet brána mezi pracovními prostory a pouze jeden gateway se dá nainstalovat v jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="79bd0-198">Currently you can’t share a gateway between workspaces and only one gateway can be installed on a single computer.</span></span> <span data-ttu-id="79bd0-199">Další informace najdete v tématu [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="79bd0-199">For more information, see [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).</span></span>

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a><span data-ttu-id="79bd0-200">Krok 2: Použití brány číst data z místního zdroje dat</span><span class="sxs-lookup"><span data-stu-id="79bd0-200">Step 2: Use the gateway to read data from an on-premises data source</span></span>
<span data-ttu-id="79bd0-201">Po nastavení brány můžete přidat **importovat Data** modulu experimentu vstup data z databáze systému SQL Server na místě.</span><span class="sxs-lookup"><span data-stu-id="79bd0-201">After you set up the gateway, you can add an **Import Data** module to an experiment that inputs the data from the on-premises SQL Server database.</span></span>

1. <span data-ttu-id="79bd0-202">V nástroji Machine Learning Studio, vyberte **EXPERIMENTY** , klikněte na **+ nový** v levém dolním rohu a vyberte **prázdný Experiment** (nebo vybrat některou z několika ukázka experimentů k dispozici).</span><span class="sxs-lookup"><span data-stu-id="79bd0-202">In Machine Learning Studio, select the **EXPERIMENTS** tab, click **+NEW** in the lower-left corner, and select **Blank Experiment** (or select one of several sample experiments available).</span></span>
2. <span data-ttu-id="79bd0-203">Najděte a přetáhněte **importovat Data** modulu na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="79bd0-203">Find and drag the **Import Data** module to the experiment canvas.</span></span>
3. <span data-ttu-id="79bd0-204">Klikněte na tlačítko **uložit jako** níže na plátno.</span><span class="sxs-lookup"><span data-stu-id="79bd0-204">Click **Save as** below the canvas.</span></span> <span data-ttu-id="79bd0-205">Zadejte "Azure kurz strojového učení místní SQL Server" pro název experimentu, vyberte pracovní prostor a klikněte **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="79bd0-205">Enter "Azure Machine Learning On-Premises SQL Server Tutorial" for the experiment name, select the workspace, and click the **OK** check mark.</span></span>

   ![Uložte s novým názvem experimentu](media/machine-learning-use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. <span data-ttu-id="79bd0-207">Klikněte na tlačítko **importovat Data** modulu, pak vyberte v **vlastnosti** v podokně napravo od plátna vyberte "Místní databáze SQL" **zdroj dat** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="79bd0-207">Click the **Import Data** module to select it, then in the **Properties** pane to the right of the canvas, select "On-Premises SQL Database" in the **Data source** dropdown list.</span></span>
5. <span data-ttu-id="79bd0-208">Vyberte **bránu Data gateway** instalaci a registraci.</span><span class="sxs-lookup"><span data-stu-id="79bd0-208">Select the **Data gateway** you installed and registered.</span></span> <span data-ttu-id="79bd0-209">Jinou bránu můžete nastavit tak, že vyberete "(Přidat novou bránu dat...)".</span><span class="sxs-lookup"><span data-stu-id="79bd0-209">You can set up another gateway by selecting "(add new Data Gateway…)".</span></span>

   ![Vyberte bránu dat pro Import dat modul](media/machine-learning-use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. <span data-ttu-id="79bd0-211">Zadejte SQL **název databázového serveru** a **název databáze**, spolu s SQL **databázový dotaz** chcete provést.</span><span class="sxs-lookup"><span data-stu-id="79bd0-211">Enter the SQL **Database server name** and **Database name**, along with the SQL **Database query** you want to execute.</span></span>
7. <span data-ttu-id="79bd0-212">Klikněte na tlačítko **zadejte hodnoty** pod **uživatelské jméno a heslo** a zadejte své přihlašovací údaje databáze.</span><span class="sxs-lookup"><span data-stu-id="79bd0-212">Click **Enter values** under **User name and password** and enter your database credentials.</span></span> <span data-ttu-id="79bd0-213">Můžete použít integrované ověřování systému Windows nebo ověřování systému SQL Server v závislosti na konfiguraci vaší místní SQL Server.</span><span class="sxs-lookup"><span data-stu-id="79bd0-213">You can use Windows Integrated Authentication or SQL Server Authentication depending upon how your on-premises SQL Server is configured.</span></span>

   ![Zadejte přihlašovací údaje databáze](media/machine-learning-use-data-from-an-on-premises-sql-server/database-credentials.png)

   <span data-ttu-id="79bd0-215">Změny zpráva "hodnoty požadované" do "Sada hodnoty" Zelená značka zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="79bd0-215">The message "values required" changes to "values set" with a green check mark.</span></span> <span data-ttu-id="79bd0-216">Potřebujete jenom jednou zadejte přihlašovací údaje, dokud se informace o databázi nebo heslo nezmění.</span><span class="sxs-lookup"><span data-stu-id="79bd0-216">You only need to enter the credentials once unless the database information or password changes.</span></span> <span data-ttu-id="79bd0-217">Azure Machine Learning používá certifikát, který jste zadali při instalaci brány k šifrování přihlašovacích údajů v cloudu.</span><span class="sxs-lookup"><span data-stu-id="79bd0-217">Azure Machine Learning uses the certificate you provided when you installed the gateway to encrypt the credentials in the cloud.</span></span> <span data-ttu-id="79bd0-218">Azure nikdy ukládá místních přihlašovacích údajů bez šifrování.</span><span class="sxs-lookup"><span data-stu-id="79bd0-218">Azure never stores on-premises credentials without encryption.</span></span>

   ![Import modulu vlastnosti dat](media/machine-learning-use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. <span data-ttu-id="79bd0-220">Klikněte na tlačítko **spustit** ke spuštění experimentu.</span><span class="sxs-lookup"><span data-stu-id="79bd0-220">Click **RUN** to run the experiment.</span></span>

<span data-ttu-id="79bd0-221">Po dokončení spuštění experimentu, můžete vizualizovat data importovat z databáze kliknutím na výstupní port modulu **importovat Data** modulu a výběrem **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="79bd0-221">Once the experiment finishes running, you can visualize the data you imported from the database by clicking the output port of the **Import Data** module and selecting **Visualize**.</span></span>

<span data-ttu-id="79bd0-222">Jakmile dokončíte vývoj experimentu, můžete nasadit a zprovoznit model.</span><span class="sxs-lookup"><span data-stu-id="79bd0-222">Once you finish developing your experiment, you can deploy and operationalize your model.</span></span> <span data-ttu-id="79bd0-223">Pomocí služby Batch provádění, data z místního serveru SQL databáze nakonfigurované v **importovat Data** modul bude číst a používat pro vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="79bd0-223">Using the Batch Execution Service, data from the on-premises SQL Server database configured in the **Import Data** module will be read and used for scoring.</span></span> <span data-ttu-id="79bd0-224">I když můžete používat službu žádosti o odpovědi pro vyhodnocování místní data, společnost Microsoft doporučuje používat [doplněk aplikace Excel](machine-learning-excel-add-in-for-web-services.md) místo.</span><span class="sxs-lookup"><span data-stu-id="79bd0-224">While you can use the Request Response Service for scoring on-premises data, Microsoft recommends using the [Excel Add-in](machine-learning-excel-add-in-for-web-services.md) instead.</span></span> <span data-ttu-id="79bd0-225">V současné době zápisu do místní databáze systému SQL Server prostřednictvím **Export dat** není podporované buď v experimentů nebo publikované webové služby.</span><span class="sxs-lookup"><span data-stu-id="79bd0-225">Currently, writing to an on-premises SQL Server database through **Export Data** is not supported either in your experiments or published web services.</span></span>