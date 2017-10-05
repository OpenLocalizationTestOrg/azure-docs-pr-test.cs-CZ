---
title: "Brána pro správu dat pro objekt pro vytváření dat | Microsoft Docs"
description: "Nastaví bránu dat pro přesun dat mezi místními a cloudem. Přesunutí dat pomocí brány pro správu dat v Azure Data Factory."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 9e40eba285aeb1cce6b77311d1b69a6b96967a0b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="data-management-gateway"></a><span data-ttu-id="1f2e1-104">Brána správy dat</span><span class="sxs-lookup"><span data-stu-id="1f2e1-104">Data Management Gateway</span></span>
<span data-ttu-id="1f2e1-105">Brána pro správu dat je klientský agent, který je nutné nainstalovat v prostředí místní ke kopírování dat mezi úložišti dat cloudu a místně.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-105">The Data management gateway is a client agent that you must install in your on-premises environment to copy data between cloud and on-premises data stores.</span></span> <span data-ttu-id="1f2e1-106">Místní data podporovaných službou Data Factory úložiště jsou uvedeny v [podporované zdroje dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) části.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-106">The on-premises data stores supported by Data Factory are listed in the [Supported data sources](data-factory-data-movement-activities.md#supported-data-stores-and-formats) section.</span></span>

<span data-ttu-id="1f2e1-107">Doplňuje podle návodu v tomto článku [přesun dat mezi místní a cloudové úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-107">This article complements the walkthrough in the [Move data between on-premises and cloud data stores](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="1f2e1-108">V tomto návodu vytvoříte kanál, který používá bránu pro přesun dat z databáze SQL serveru místně do objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-108">In the walkthrough, you create a pipeline that uses the gateway to move data from an on-premises SQL Server database to an Azure blob.</span></span> <span data-ttu-id="1f2e1-109">Tento článek obsahuje podrobné podrobné informace o Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-109">This article provides detailed in-depth information about the data management gateway.</span></span> 

<span data-ttu-id="1f2e1-110">Brána pro správu dat můžete škálovat tím, že přidružíte více místní počítače s bránou.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-110">You can scale out a data management gateway by associating multiple on-premises machines with the gateway.</span></span> <span data-ttu-id="1f2e1-111">Je možné škálovat až zvýšením počtu úloh přesun dat, které můžou běžet souběžně na uzlu.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-111">You can scale up by increasing number of data movement jobs that can run concurrently on a node.</span></span> <span data-ttu-id="1f2e1-112">Tato funkce je také k dispozici pro logické brány se jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-112">This feature is also available for a logical gateway with a single node.</span></span> <span data-ttu-id="1f2e1-113">V tématu [škálování Brána pro správu dat v Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-113">See [Scaling data management gateway in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) article for details.</span></span>

> [!NOTE]
> <span data-ttu-id="1f2e1-114">Brána v současné době podporuje pouze aktivity kopírování a aktivity uložené procedury v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-114">Currently, gateway supports only the copy activity and stored procedure activity in Data Factory.</span></span> <span data-ttu-id="1f2e1-115">Není možné používat pro přístup k místním zdrojům dat brány z vlastních aktivit.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-115">It is not possible to use the gateway from a custom activity to access on-premises data sources.</span></span>      

## <a name="overview"></a><span data-ttu-id="1f2e1-116">Přehled</span><span class="sxs-lookup"><span data-stu-id="1f2e1-116">Overview</span></span>
### <a name="capabilities-of-data-management-gateway"></a><span data-ttu-id="1f2e1-117">Možnosti brány pro správu dat</span><span class="sxs-lookup"><span data-stu-id="1f2e1-117">Capabilities of data management gateway</span></span>
<span data-ttu-id="1f2e1-118">Brána pro správu dat poskytuje následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-118">Data management gateway provides the following capabilities:</span></span>

* <span data-ttu-id="1f2e1-119">Model místní zdroje dat a cloudové zdroje dat v rámci stejné služby data factory a přesouvat data.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-119">Model on-premises data sources and cloud data sources within the same data factory and move data.</span></span>
* <span data-ttu-id="1f2e1-120">Máte jedno podokno skla pro monitorování a správu s přehledem stav brány. na stránce služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-120">Have a single pane of glass for monitoring and management with visibility into gateway status from the Data Factory page.</span></span>
* <span data-ttu-id="1f2e1-121">Spravovat přístup k místním zdrojům dat bezpečně.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-121">Manage access to on-premises data sources securely.</span></span>
  * <span data-ttu-id="1f2e1-122">Žádné změny do podnikové brány firewall.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-122">No changes required to corporate firewall.</span></span> <span data-ttu-id="1f2e1-123">Brána je pouze odchozí připojení k Internetu otevřete založené na protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-123">Gateway only makes outbound HTTP-based connections to open internet.</span></span>
  * <span data-ttu-id="1f2e1-124">Šifrování přihlašovacích údajů pro vaše místní data úložiště pomocí certifikátu.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-124">Encrypt credentials for your on-premises data stores with your certificate.</span></span>
* <span data-ttu-id="1f2e1-125">Přesun dat efektivně – data se přenáší paralelně, odolné vůči přerušované sítě problémy s automatickým logika opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-125">Move data efficiently – data is transferred in parallel, resilient to intermittent network issues with auto retry logic.</span></span>

### <a name="command-flow-and-data-flow"></a><span data-ttu-id="1f2e1-126">Příkaz a tok dat</span><span class="sxs-lookup"><span data-stu-id="1f2e1-126">Command flow and data flow</span></span>
<span data-ttu-id="1f2e1-127">Pokud použijete aktivitu kopírování ke kopírování dat mezi místními a cloudovými, aktivita používá bránu k přenosu dat z místního zdroje dat do cloudu a naopak.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-127">When you use a copy activity to copy data between on-premises and cloud, the activity uses a gateway to transfer data from on-premises data source to cloud and vice versa.</span></span>

<span data-ttu-id="1f2e1-128">Zde je souhrn kroků pro kopírování pomocí brána dat a podrobný datový tok pro: ![tok dat pomocí brány.](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)</span><span class="sxs-lookup"><span data-stu-id="1f2e1-128">Here is the high-level data flow for and summary of steps for copy with data gateway: ![Data flow using gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)</span></span>

1. <span data-ttu-id="1f2e1-129">Data developer vytvoří bránu pro Azure Data Factory pomocí [portál Azure](https://portal.azure.com) nebo [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/dn820234.aspx).</span><span class="sxs-lookup"><span data-stu-id="1f2e1-129">Data developer creates a gateway for an Azure Data Factory using either the [Azure portal](https://portal.azure.com) or [PowerShell Cmdlet](https://msdn.microsoft.com/library/dn820234.aspx).</span></span>
2. <span data-ttu-id="1f2e1-130">Data developer vytvoří propojené služby pro místnímu úložišti dat tak, že zadáte brány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-130">Data developer creates a linked service for an on-premises data store by specifying the gateway.</span></span> <span data-ttu-id="1f2e1-131">Jako součást nastavení propojené služby developer dat používá aplikace nastavení pověření k určení typů ověřování a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-131">As part of setting up the linked service, data developer uses the Setting Credentials application to specify authentication types and credentials.</span></span>  <span data-ttu-id="1f2e1-132">Dialogové okno nastavení pověření aplikace komunikuje s úložištěm dat k testování připojení a bránu a uložte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-132">The Setting Credentials application dialog communicates with the data store to test connection and the gateway to save credentials.</span></span>
3. <span data-ttu-id="1f2e1-133">Brána šifruje přihlašovací údaje s certifikát přidružený k bráně (poskytl vývojáře data), před uložením přihlašovací údaje v cloudu.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-133">Gateway encrypts the credentials with the certificate associated with the gateway (supplied by data developer), before saving the credentials in the cloud.</span></span>
4. <span data-ttu-id="1f2e1-134">Služba data Factory komunikuje s bránou pro plánování a správu úloh přes řídicí kanál, který používá sdílené služby Azure service bus fronty.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-134">Data Factory service communicates with the gateway for scheduling & management of jobs via a control channel that uses a shared Azure service bus queue.</span></span> <span data-ttu-id="1f2e1-135">Když úlohu kopírování aktivity musí být spuštěna., Data Factory zařadí do fronty požadavek spolu s přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-135">When a copy activity job needs to be kicked off, Data Factory queues the request along with credential information.</span></span> <span data-ttu-id="1f2e1-136">Brána se spustí úlohu po dotazování fronty.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-136">Gateway kicks off the job after polling the queue.</span></span>
5. <span data-ttu-id="1f2e1-137">Brána dešifruje přihlašovací údaje s stejný certifikát a pak připojí k místnímu úložišti dat typu správné ověření a přihlašovací údaje se.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-137">The gateway decrypts the credentials with the same certificate and then connects to the on-premises data store with proper authentication type and credentials.</span></span>
6. <span data-ttu-id="1f2e1-138">Brána zkopíruje data z místního úložiště do cloudového úložiště, nebo naopak v závislosti na konfiguraci aktivitě kopírování v datovém kanálu.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-138">The gateway copies data from an on-premises store to a cloud storage, or vice versa depending on how the Copy Activity is configured in the data pipeline.</span></span> <span data-ttu-id="1f2e1-139">Pro tento krok brány komunikuje přímo se služby cloudového úložiště, jako je například úložiště objektů Blob Azure přes zabezpečený kanál (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="1f2e1-139">For this step, the gateway directly communicates with cloud-based storage services such as Azure Blob Storage over a secure (HTTPS) channel.</span></span>

### <a name="considerations-for-using-gateway"></a><span data-ttu-id="1f2e1-140">Předpoklady pro použití brány</span><span class="sxs-lookup"><span data-stu-id="1f2e1-140">Considerations for using gateway</span></span>
* <span data-ttu-id="1f2e1-141">Jedna instance brány pro správu dat můžete použít pro více zdrojů dat na místě.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-141">A single instance of data management gateway can be used for multiple on-premises data sources.</span></span> <span data-ttu-id="1f2e1-142">Ale **jedna instance brány je vázaný na pouze jeden objekt pro vytváření dat Azure** a nemohou být sdíleny s jinou služby data factory.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-142">However, **a single gateway instance is tied to only one Azure data factory** and cannot be shared with another data factory.</span></span>
* <span data-ttu-id="1f2e1-143">Můžete mít **pouze jedna instance brány pro správu dat** nainstalované na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-143">You can have **only one instance of data management gateway** installed on a single machine.</span></span> <span data-ttu-id="1f2e1-144">Předpokládejme, že, máte dvě datové továrny, které potřebují přístup k místním zdrojům dat, budete muset nainstalovat brány ve dvou počítačích na místě.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-144">Suppose, you have two data factories that need to access on-premises data sources, you need to install gateways on two on-premises computers.</span></span> <span data-ttu-id="1f2e1-145">Jinými slovy brány je vázaný na konkrétní data factory</span><span class="sxs-lookup"><span data-stu-id="1f2e1-145">In other words, a gateway is tied to a specific data factory</span></span>
* <span data-ttu-id="1f2e1-146">**Brány nemusí být ve stejném počítači jako zdroj dat**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-146">The **gateway does not need to be on the same machine as the data source**.</span></span> <span data-ttu-id="1f2e1-147">S brány blíž ke zdroji dat však zkracuje čas pro bránu pro připojení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-147">However, having gateway closer to the data source reduces the time for the gateway to connect to the data source.</span></span> <span data-ttu-id="1f2e1-148">Doporučujeme nainstalovat bránu na počítači, který se liší od ten, který je hostitelem zdroje dat na místě.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-148">We recommend that you install the gateway on a machine that is different from the one that hosts on-premises data source.</span></span> <span data-ttu-id="1f2e1-149">Pokud zdroj brány a data jsou na různé počítače, brána není pokouší o prostředky se zdrojem dat.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-149">When the gateway and data source are on different machines, the gateway does not compete for resources with data source.</span></span>
* <span data-ttu-id="1f2e1-150">Můžete mít **více bran na různé počítače připojení ke zdroji dat stejnou místní**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-150">You can have **multiple gateways on different machines connecting to the same on-premises data source**.</span></span> <span data-ttu-id="1f2e1-151">Například můžete mít dvě brány obsluhující dvě datové továrny ale stejný místní zdroj dat není zaregistrována datové továrny.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-151">For example, you may have two gateways serving two data factories but the same on-premises data source is registered with both the data factories.</span></span>
* <span data-ttu-id="1f2e1-152">Pokud je již nainstalovaná na vašem počítači slouží **Power BI** scénář, nainstalovat **samostatnou bránu pro Azure Data Factory** na jiném počítači.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-152">If you already have a gateway installed on your computer serving a **Power BI** scenario, install a **separate gateway for Azure Data Factory** on another machine.</span></span>
* <span data-ttu-id="1f2e1-153">Brány musí použít i v případě, že používáte **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-153">Gateway must be used even when you use **ExpressRoute**.</span></span>
* <span data-ttu-id="1f2e1-154">Váš zdroj dat považovat za zdroj dat na místě, (který je za bránou firewall) i při použití **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-154">Treat your data source as an on-premises data source (that is behind a firewall) even when you use **ExpressRoute**.</span></span> <span data-ttu-id="1f2e1-155">Použijte bránu k navázání připojení mezi službou a zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-155">Use the gateway to establish connectivity between the service and the data source.</span></span>
* <span data-ttu-id="1f2e1-156">Je nutné **použití brány** i v případě, že je úložiště dat v cloudu **virtuálního počítače Azure IaaS**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-156">You must **use the gateway** even if the data store is in the cloud on an **Azure IaaS VM**.</span></span>

## <a name="installation"></a><span data-ttu-id="1f2e1-157">Instalace</span><span class="sxs-lookup"><span data-stu-id="1f2e1-157">Installation</span></span>
### <a name="prerequisites"></a><span data-ttu-id="1f2e1-158">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1f2e1-158">Prerequisites</span></span>
* <span data-ttu-id="1f2e1-159">U podporovaných **operačního systému** jsou verze Windows 7, Windows 8 nebo 8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-159">The supported **Operating System** versions are Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2.</span></span> <span data-ttu-id="1f2e1-160">Instalace brány pro správu dat na řadiči domény není aktuálně podporována.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-160">Installation of the data management gateway on a domain controller is currently not supported.</span></span>
* <span data-ttu-id="1f2e1-161">Rozhraní .NET framework 4.5.1 nebo novější je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-161">.NET Framework 4.5.1 or above is required.</span></span> <span data-ttu-id="1f2e1-162">Pokud instalujete brány na počítače s Windows 7, nainstalujte rozhraní .NET Framework 4.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-162">If you are installing gateway on a Windows 7 machine, install .NET Framework 4.5 or later.</span></span> <span data-ttu-id="1f2e1-163">V tématu [požadavky na systém rozhraní .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-163">See [.NET Framework System Requirements](https://msdn.microsoft.com/library/8z6watww.aspx) for details.</span></span>
* <span data-ttu-id="1f2e1-164">Doporučené **konfigurace** pro bránu počítač je alespoň 2 GHz, 4 jádra, 8 GB paměti RAM a 80-GB místa na disku.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-164">The recommended **configuration** for the gateway machine is at least 2 GHz, 4 cores, 8-GB RAM, and 80-GB disk.</span></span>
* <span data-ttu-id="1f2e1-165">Pokud hostitelský počítač přejde do režimu spánku, brána neodpovídá na požadavky na data.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-165">If the host machine hibernates, the gateway does not respond to data requests.</span></span> <span data-ttu-id="1f2e1-166">Proto konfigurovat odpovídající **schéma napájení** v počítači před instalací brány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-166">Therefore, configure an appropriate **power plan** on the computer before installing the gateway.</span></span> <span data-ttu-id="1f2e1-167">Pokud je počítač nakonfigurovaný do režimu spánku, vyzve k instalaci brány zprávu.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-167">If the machine is configured to hibernate, the gateway installation prompts a message.</span></span>
* <span data-ttu-id="1f2e1-168">Musíte být správce na počítači k instalaci a konfiguraci brány pro správu dat úspěšně.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-168">You must be an administrator on the machine to install and configure the data management gateway successfully.</span></span> <span data-ttu-id="1f2e1-169">Můžete přidat další uživatelům **Brána pro správu dat uživatele** místní skupiny Windows.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-169">You can add additional users to the **data management gateway Users** local Windows group.</span></span> <span data-ttu-id="1f2e1-170">Členové této skupiny jsou schopné používat **Správce konfigurace brány pro správu dat** nástroj pro konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-170">The members of this group are able to use the **Data Management Gateway Configuration Manager** tool to configure the gateway.</span></span>

<span data-ttu-id="1f2e1-171">Jako běh aktivit kopie dojít na konkrétní frekvenci, využití prostředků (procesor, paměť) na počítači také používá se stejný vzor s ve špičce a doby nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-171">As copy activity runs happen on a specific frequency, the resource usage (CPU, memory) on the machine also follows the same pattern with peak and idle times.</span></span> <span data-ttu-id="1f2e1-172">Využití prostředků závisí také výraznou na množství dat přesouvá.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-172">Resource utilization also depends heavily on the amount of data being moved.</span></span> <span data-ttu-id="1f2e1-173">Více úloh kopie jsou v průběhu, uvidíte zvedla během špiček využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-173">When multiple copy jobs are in progress, you see resource usage go up during peak times.</span></span>

### <a name="installation-options"></a><span data-ttu-id="1f2e1-174">Možnosti instalace</span><span class="sxs-lookup"><span data-stu-id="1f2e1-174">Installation options</span></span>
<span data-ttu-id="1f2e1-175">Brána pro správu dat lze nainstalovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-175">Data management gateway can be installed in the following ways:</span></span>

* <span data-ttu-id="1f2e1-176">Stažením balíčku MSI Instalační program z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span><span class="sxs-lookup"><span data-stu-id="1f2e1-176">By downloading an MSI setup package from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span></span>  <span data-ttu-id="1f2e1-177">Soubor MSI můžete také použít k upgradu existující brány pro správu dat na nejnovější verzi, se všemi možnými nastavení zachovaná.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-177">The MSI can also be used to upgrade existing data management gateway to the latest version, with all settings preserved.</span></span>
* <span data-ttu-id="1f2e1-178">Kliknutím na **stáhnout a nainstalovat bránu dat** odkazu na RUČNÍ instalace nebo **nainstalovat přímo na tomto počítači** pod Expresní instalace.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-178">By clicking **Download and install data gateway** link under MANUAL SETUP or **Install directly on this computer** under EXPRESS SETUP.</span></span> <span data-ttu-id="1f2e1-179">V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny k používání Expresní instalace najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-179">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on using express setup.</span></span> <span data-ttu-id="1f2e1-180">Manuální krok přejdete na webu Stažení softwaru.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-180">The manual step takes you to the download center.</span></span>  <span data-ttu-id="1f2e1-181">Pokyny ke stažení a instalaci brány ze služby Stažení softwaru jsou v další části.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-181">The instructions for downloading and installing the gateway from download center are in the next section.</span></span>

### <a name="installation-best-practices"></a><span data-ttu-id="1f2e1-182">Osvědčené postupy instalace:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-182">Installation best practices:</span></span>
1. <span data-ttu-id="1f2e1-183">Schéma napájení nakonfigurujte na hostitelském počítači pro bránu, aby počítač nepřejde do režimu spánku.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-183">Configure power plan on the host machine for the gateway so that the machine does not hibernate.</span></span> <span data-ttu-id="1f2e1-184">Pokud hostitelský počítač přejde do režimu spánku, brána neodpovídá na požadavky na data.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-184">If the host machine hibernates, the gateway does not respond to data requests.</span></span>
2. <span data-ttu-id="1f2e1-185">Zálohujte certifikát přidružený k bráně.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-185">Back up the certificate associated with the gateway.</span></span>

### <a name="install-the-gateway-from-download-center"></a><span data-ttu-id="1f2e1-186">Bránu nainstalovat ze služby Stažení softwaru</span><span class="sxs-lookup"><span data-stu-id="1f2e1-186">Install the gateway from download center</span></span>
1. <span data-ttu-id="1f2e1-187">Přejděte na [stránky pro stažení Brána pro správu dat](https://www.microsoft.com/download/details.aspx?id=39717).</span><span class="sxs-lookup"><span data-stu-id="1f2e1-187">Navigate to [Microsoft Data Management Gateway download page](https://www.microsoft.com/download/details.aspx?id=39717).</span></span>
2. <span data-ttu-id="1f2e1-188">Klikněte na tlačítko **Stáhnout**, vyberte příslušnou verzi (**32-bit** vs. **64-bit**) a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-188">Click **Download**, select the appropriate version (**32-bit** vs. **64-bit**), and click **Next**.</span></span>
3. <span data-ttu-id="1f2e1-189">Spustit **MSI** přímo nebo uložte ho na pevný disk a spustit.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-189">Run the **MSI** directly or save it to your hard disk and run.</span></span>
4. <span data-ttu-id="1f2e1-190">Na **úvodní** vyberte **jazyk** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-190">On the **Welcome** page, select a **language** click **Next**.</span></span>
5. <span data-ttu-id="1f2e1-191">**Přijměte** licenční smlouvu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-191">**Accept** the End-User License Agreement and click **Next**.</span></span>
6. <span data-ttu-id="1f2e1-192">Vyberte **složky** instalace brány a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-192">Select **folder** to install the gateway and click **Next**.</span></span>
7. <span data-ttu-id="1f2e1-193">Na **připravené k instalaci** klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-193">On the **Ready to install** page, click **Install**.</span></span>
8. <span data-ttu-id="1f2e1-194">Klikněte na tlačítko **Dokončit** k dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-194">Click **Finish** to complete installation.</span></span>
9. <span data-ttu-id="1f2e1-195">Získáte klíč z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-195">Get the key from the Azure portal.</span></span> <span data-ttu-id="1f2e1-196">Najdete v části Další podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-196">See the next section for step-by-step instructions.</span></span>
10. <span data-ttu-id="1f2e1-197">Na **registrace brány** stránka **Správce konfigurace brány pro správu dat** spuštěná na počítači, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-197">On the **Register gateway** page of **Data Management Gateway Configuration Manager** running on your machine, do the following steps:</span></span>
    1. <span data-ttu-id="1f2e1-198">Vložte klíč v textu.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-198">Paste the key in the text.</span></span>
    2. <span data-ttu-id="1f2e1-199">Volitelně klikněte na **Show klíč brány** zobrazíte text klíče.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-199">Optionally, click **Show gateway key** to see the key text.</span></span>
    3. <span data-ttu-id="1f2e1-200">Klikněte na tlačítko **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-200">Click **Register**.</span></span>

### <a name="register-gateway-using-key"></a><span data-ttu-id="1f2e1-201">Zaregistrujte bránu pomocí klíče</span><span class="sxs-lookup"><span data-stu-id="1f2e1-201">Register gateway using key</span></span>
#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a><span data-ttu-id="1f2e1-202">Pokud jste ještě nevytvořili logické brány na portálu</span><span class="sxs-lookup"><span data-stu-id="1f2e1-202">If you haven't already created a logical gateway in the portal</span></span>
<span data-ttu-id="1f2e1-203">Vytvoření brány na portálu a získání klíče z **konfigurace** stránky, postupujte podle kroků z návod v [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-203">To create a gateway in the portal and get the key from the **Configure** page, Follow steps from walkthrough in the [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a><span data-ttu-id="1f2e1-204">Pokud jste již vytvořili logické brány na portálu</span><span class="sxs-lookup"><span data-stu-id="1f2e1-204">If you have already created the logical gateway in the portal</span></span>
1. <span data-ttu-id="1f2e1-205">Na portálu Azure, přejděte na **Data Factory** a klikněte na tlačítko **propojené služby** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-205">In Azure portal, navigate to the **Data Factory** page, and click **Linked Services** tile.</span></span>

    ![Stránka objektu pro vytváření dat](media/data-factory-data-management-gateway/data-factory-blade.png)
2. <span data-ttu-id="1f2e1-207">V **propojené služby** vyberte logické **brány** vytvořený na portálu.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-207">In the **Linked Services** page, select the logical **gateway** you created in the portal.</span></span>

    ![logické brány](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. <span data-ttu-id="1f2e1-209">V **bránu Data Gateway** klikněte na tlačítko **stáhnout a nainstalovat bránu dat**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-209">In the **Data Gateway** page, click **Download and install data gateway**.</span></span>

    ![Stáhněte si odkaz na portálu](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. <span data-ttu-id="1f2e1-211">V **konfigurace** klikněte na tlačítko **znovu vytvořte klíč**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-211">In the **Configure** page, click **Recreate key**.</span></span> <span data-ttu-id="1f2e1-212">Klepněte na tlačítko Ano upozornění po přečtení ji pečlivě.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-212">Click Yes on the warning message after reading it carefully.</span></span>

    ![Znovu vytvořte klíč](media/data-factory-data-management-gateway/recreate-key-button.png)
5. <span data-ttu-id="1f2e1-214">Klikněte na tlačítko Kopírovat vedle klíč.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-214">Click Copy button next to the key.</span></span> <span data-ttu-id="1f2e1-215">Klíč je zkopírován do schránky.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-215">The key is copied to the clipboard.</span></span>

    ![Zkopírovat klíč](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a><span data-ttu-id="1f2e1-217">Systémové ikony oznamovací oblasti nebo oznámení</span><span class="sxs-lookup"><span data-stu-id="1f2e1-217">System tray icons/ notifications</span></span>
<span data-ttu-id="1f2e1-218">Následující obrázek ukazuje některé ikony oznamovací oblasti, které vidíte.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-218">The following image shows some of the tray icons that you see.</span></span>

![systémové ikony oznamovací oblasti](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

<span data-ttu-id="1f2e1-220">Pokud přesuňte ukazatel na systému panelu ikonu nebo oznámení, můžete zobrazit podrobnosti o stavu operace aktualizace pro bránu v automaticky otevíraném okně.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-220">If you move cursor over the system tray icon/notification message, you see details about the state of the gateway/update operation in a popup window.</span></span>

### <a name="ports-and-firewall"></a><span data-ttu-id="1f2e1-221">Porty a brány firewall</span><span class="sxs-lookup"><span data-stu-id="1f2e1-221">Ports and firewall</span></span>
<span data-ttu-id="1f2e1-222">Existují dvě brány firewall, je potřeba zvážit: **podniková brána firewall** systémem směrovači centrální organizace, a **brány Windows firewall** nakonfigurovaný jako démon na místním počítači, kde je brána nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-222">There are two firewalls you need to consider: **corporate firewall** running on the central router of the organization, and **Windows firewall** configured as a daemon on the local machine where the gateway is installed.</span></span>  

![Brány firewall](./media/data-factory-data-management-gateway/firewalls2.png)

<span data-ttu-id="1f2e1-224">Na úrovni podniková brána firewall je nutné nakonfigurovat následující domény a odchozí porty:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-224">At corporate firewall level, you need configure the following domains and outbound ports:</span></span>

| <span data-ttu-id="1f2e1-225">Názvy domén</span><span class="sxs-lookup"><span data-stu-id="1f2e1-225">Domain names</span></span> | <span data-ttu-id="1f2e1-226">Porty</span><span class="sxs-lookup"><span data-stu-id="1f2e1-226">Ports</span></span> | <span data-ttu-id="1f2e1-227">Popis</span><span class="sxs-lookup"><span data-stu-id="1f2e1-227">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1f2e1-228">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="1f2e1-228">*.servicebus.windows.net</span></span> |<span data-ttu-id="1f2e1-229">443, 80</span><span class="sxs-lookup"><span data-stu-id="1f2e1-229">443, 80</span></span> |<span data-ttu-id="1f2e1-230">Používá ke komunikaci s back-end služba pro přesun dat</span><span class="sxs-lookup"><span data-stu-id="1f2e1-230">Used for communication with Data Movement Service backend</span></span> |
| <span data-ttu-id="1f2e1-231">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="1f2e1-231">*.core.windows.net</span></span> |<span data-ttu-id="1f2e1-232">443</span><span class="sxs-lookup"><span data-stu-id="1f2e1-232">443</span></span> |<span data-ttu-id="1f2e1-233">Použít pro kopírování připravený pomocí objektů Blob v Azure (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="1f2e1-233">Used for Staged copy using Azure Blob (if configured)</span></span>|
| <span data-ttu-id="1f2e1-234">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="1f2e1-234">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="1f2e1-235">443</span><span class="sxs-lookup"><span data-stu-id="1f2e1-235">443</span></span> |<span data-ttu-id="1f2e1-236">Používá ke komunikaci s back-end služba pro přesun dat</span><span class="sxs-lookup"><span data-stu-id="1f2e1-236">Used for communication with Data Movement Service backend</span></span> |


<span data-ttu-id="1f2e1-237">Na úrovni brány firewall systému Windows jsou obvykle povolené tyto Odchozí porty.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-237">At Windows firewall level, these outbound ports are normally enabled.</span></span> <span data-ttu-id="1f2e1-238">Pokud není, můžete nakonfigurovat domén a porty odpovídajícím způsobem v počítači brány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-238">If not, you can configure the domains and ports accordingly on gateway machine.</span></span>

> [!NOTE]
> 1. <span data-ttu-id="1f2e1-239">Na základě vaší zdroje / jímky, možná budete muset povolených dalších domén a odchozí porty v bráně firewall podnikové a Windows.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-239">Based on your source/ sinks, you may have to whitelist additional domains and outbound ports in your corporate/Windows firewall.</span></span>
> 2. <span data-ttu-id="1f2e1-240">Pro některé databáze cloudu (například: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access)atd), budete muset seznam povolených adres IP adresu počítače brány na jejich konfiguraci brány firewall.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-240">For some Cloud Databases (For example: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access), etc.), you may need to whitelist IP address of Gateway machine on their firewall configuration.</span></span>
>
>


#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a><span data-ttu-id="1f2e1-241">Kopírování dat ze zdrojového úložiště dat do úložiště dat jímka</span><span class="sxs-lookup"><span data-stu-id="1f2e1-241">Copy data from a source data store to a sink data store</span></span>
<span data-ttu-id="1f2e1-242">Zkontrolujte, zda jsou pravidla brány firewall na podnikové brány firewall, brána Windows firewall v počítači s bránou povolené správně a data ukládat sám sebe.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-242">Ensure that the firewall rules are enabled properly on the corporate firewall, Windows firewall on the gateway machine, and the data store itself.</span></span> <span data-ttu-id="1f2e1-243">Tato pravidla povolíte bránu pro připojení k obou zdroj a jímka úspěšně.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-243">Enabling these rules allows the gateway to connect to both source and sink successfully.</span></span> <span data-ttu-id="1f2e1-244">Povolení pravidel pro každou úložiště dat, který je zahrnut v operaci kopírování.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-244">Enable rules for each data store that is involved in the copy operation.</span></span>

<span data-ttu-id="1f2e1-245">Například pro kopírování z **úložišti místní data jímky Azure SQL Database nebo jímky Azure SQL Data Warehouse**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-245">For example, to copy from **an on-premises data store to an Azure SQL Database sink or an Azure SQL Data Warehouse sink**, do the following steps:</span></span>

* <span data-ttu-id="1f2e1-246">Povolit odchozí **TCP** komunikaci na portu **1433** pro bránu Windows firewall a podniková brána firewall.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-246">Allow outbound **TCP** communication on port **1433** for both Windows firewall and corporate firewall.</span></span>
* <span data-ttu-id="1f2e1-247">Konfigurace nastavení brány firewall serveru Azure SQL na IP adresu počítače brány přidat do seznamu povolených IP adres.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-247">Configure the firewall settings of Azure SQL server to add the IP address of the gateway machine to the list of allowed IP addresses.</span></span>

> [!NOTE]
> <span data-ttu-id="1f2e1-248">Pokud brána firewall nedovoluje odchozí port 1433, brána nemá přístup k Azure SQL přímo.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-248">If your firewall does not allow outbound port 1433, Gateway can't access Azure SQL directly.</span></span> <span data-ttu-id="1f2e1-249">V takovém případě můžete použít [připravený kopie](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) do SQL Azure Database nebo datového skladu SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-249">In this case, you may use [Staged Copy](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) to SQL Azure Database/ SQL Azure DW.</span></span> <span data-ttu-id="1f2e1-250">V tomto scénáři by pro přesun dat vyžadují jenom protokolu HTTPS (port 443).</span><span class="sxs-lookup"><span data-stu-id="1f2e1-250">In this scenario, you would only require HTTPS (port 443) for the data movement.</span></span>
>
>


### <a name="proxy-server-considerations"></a><span data-ttu-id="1f2e1-251">Důležité informace o proxy serveru</span><span class="sxs-lookup"><span data-stu-id="1f2e1-251">Proxy server considerations</span></span>
<span data-ttu-id="1f2e1-252">Pokud vaše podnikové síťové prostředí používá proxy server pro přístup k Internetu, nakonfigurujte Brána pro správu dat použít příslušná nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-252">If your corporate network environment uses a proxy server to access the internet, configure data management gateway to use appropriate proxy settings.</span></span> <span data-ttu-id="1f2e1-253">Během fáze počáteční registrace můžete nastavit proxy server.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-253">You can set the proxy during the initial registration phase.</span></span>

![Sada proxy během registrace](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

<span data-ttu-id="1f2e1-255">Brána používá proxy server pro připojení ke cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-255">Gateway uses the proxy server to connect to the cloud service.</span></span> <span data-ttu-id="1f2e1-256">Klikněte na tlačítko **změnu** propojení během počáteční instalace.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-256">Click **Change** link during initial setup.</span></span> <span data-ttu-id="1f2e1-257">Zobrazí **nastavení proxy serveru** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-257">You see the **proxy setting** dialog.</span></span>

![Sada proxy serveru pomocí Správce konfigurace](media/data-factory-data-management-gateway/SetProxySettings.png)

<span data-ttu-id="1f2e1-259">Existují tři možnosti konfigurace:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-259">There are three configuration options:</span></span>

* <span data-ttu-id="1f2e1-260">**Nepoužívejte proxy**: Brána pro připojení ke cloudovým službám explicitně nepoužívá jakýkoli proxy server.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-260">**Do not use proxy**: Gateway does not explicitly use any proxy to connect to cloud services.</span></span>
* <span data-ttu-id="1f2e1-261">**Použít proxy server systému**: Brána používá proxy server, který je nastavení nakonfigurované v diahost.exe.config a diawp.exe.config.  Pokud žádný proxy server je nakonfigurován v diahost.exe.config a diawp.exe.config, brána připojí ke cloudové službě přímo bez proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-261">**Use system proxy**: Gateway uses the proxy setting that is configured in diahost.exe.config and diawp.exe.config.  If no proxy is configured in diahost.exe.config and diawp.exe.config, gateway connects to cloud service directly without going through proxy.</span></span>
* <span data-ttu-id="1f2e1-262">**Používat vlastní proxy server**: Konfigurace nastavení pro bránu, místo použití konfigurace v diahost.exe.config a diawp.exe.config proxy protokolu HTTP.  Adresa a Port jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-262">**Use custom proxy**: Configure the HTTP proxy setting to use for gateway, instead of using configurations in diahost.exe.config and diawp.exe.config.  Address and Port are required.</span></span>  <span data-ttu-id="1f2e1-263">Uživatelské jméno a heslo jsou volitelné v závislosti na nastavení vašeho proxy ověřování.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-263">User Name and Password are optional depending on your proxy’s authentication setting.</span></span>  <span data-ttu-id="1f2e1-264">Všechna nastavení jsou šifrován certifikát přihlašovacích údajů brány a jsou uložené místně na hostitelském počítači brány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-264">All settings are encrypted with the credential certificate of the gateway and stored locally on the gateway host machine.</span></span>

<span data-ttu-id="1f2e1-265">Brána pro správu dat hostitelská služba restartuje automaticky po uložení aktualizované nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-265">The data management gateway Host Service restarts automatically after you save the updated proxy settings.</span></span>

<span data-ttu-id="1f2e1-266">Po Brány byl úspěšně registrován, pokud chcete zobrazit nebo aktualizovat nastavení proxy serveru, použijte Správce konfigurace brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-266">After gateway has been successfully registered, if you want to view or update proxy settings, use Data Management Gateway Configuration Manager.</span></span>

1. <span data-ttu-id="1f2e1-267">Spusťte **Správce konfigurace brány pro správu dat**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-267">Launch **Data Management Gateway Configuration Manager**.</span></span>
2. <span data-ttu-id="1f2e1-268">Přepnout **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-268">Switch to the **Settings** tab.</span></span>
3. <span data-ttu-id="1f2e1-269">Klikněte na tlačítko **změnu** na odkaz v **server Proxy protokolu HTTP** části spustíte **nastavení proxy serveru HTTP** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-269">Click **Change** link in **HTTP Proxy** section to launch the **Set HTTP Proxy** dialog.</span></span>  
4. <span data-ttu-id="1f2e1-270">Po kliknutí **Další** tlačítko se zobrazí dialog s upozorněním žádostí o oprávnění k uložení nastavení proxy serveru a restartujte hostitelskou službu brány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-270">After you click the **Next** button, you see a warning dialog asking for your permission to save the proxy setting and restart the Gateway Host Service.</span></span>

<span data-ttu-id="1f2e1-271">Můžete zobrazit a aktualizovat server proxy protokolu HTTP pomocí nástroje Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-271">You can view and update HTTP proxy by using Configuration Manager tool.</span></span>

![Sada proxy serveru pomocí Správce konfigurace](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> <span data-ttu-id="1f2e1-273">Pokud jste nastavili proxy server pomocí ověřování NTLM, kompatibilní se hostitelská služba brány pro účet domény.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-273">If you set up a proxy server with NTLM authentication, Gateway Host Service runs under the domain account.</span></span> <span data-ttu-id="1f2e1-274">Pokud změníte heslo pro účet domény později, musíte aktualizovat nastavení konfigurace pro službu a restartujte ji odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-274">If you change the password for the domain account later, remember to update configuration settings for the service and restart it accordingly.</span></span> <span data-ttu-id="1f2e1-275">Kvůli tomuto požadavku, doporučujeme používat vyhrazený doménový účet pro přístup k proxy serveru, který nevyžaduje často aktualizovat heslo.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-275">Due to this requirement, we suggest you use a dedicated domain account to access the proxy server that does not require you to update the password frequently.</span></span>
>
>

### <a name="configure-proxy-server-settings"></a><span data-ttu-id="1f2e1-276">Nakonfigurujte nastavení serveru proxy</span><span class="sxs-lookup"><span data-stu-id="1f2e1-276">Configure proxy server settings</span></span>
<span data-ttu-id="1f2e1-277">Pokud vyberete **používat proxy server systému** nastavení pro proxy server HTTP, brána používá nastavení proxy v diahost.exe.config a diawp.exe.config.  Pokud žádný proxy server je zadané v diahost.exe.config a diawp.exe.config, brána připojí ke cloudové službě přímo bez proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-277">If you select **Use system proxy** setting for the HTTP proxy, gateway uses the proxy setting in diahost.exe.config and diawp.exe.config.  If no proxy is specified in diahost.exe.config and diawp.exe.config, gateway connects to cloud service directly without going through proxy.</span></span> <span data-ttu-id="1f2e1-278">Následující postup obsahuje pokyny pro aktualizaci souboru diahost.exe.config.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-278">The following procedure provides instructions for updating the diahost.exe.config file.</span></span>  

1. <span data-ttu-id="1f2e1-279">V Průzkumníku souborů vytvořte bezpečné kopii C:\Program Files\Microsoft Data správy Gateway\2.0\Shared\diahost.exe.config zálohování původní soubor.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-279">In File Explorer, make a safe copy of C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config to back up the original file.</span></span>
2. <span data-ttu-id="1f2e1-280">Spusťte Notepad.exe spuštění jako správce a otevřete textový soubor "C:\Program Files\Microsoft Data správy Gateway\2.0\Shared\diahost.exe.config. Najít výchozí značka pro system.net, jak je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-280">Launch Notepad.exe running as administrator, and open text file “C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. You find the default tag for system.net as shown in the following code:</span></span>

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   <span data-ttu-id="1f2e1-281">Poté můžete přidat podrobnosti o proxy serveru, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-281">You can then add proxy server details as shown in the following example:</span></span>

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   <span data-ttu-id="1f2e1-282">Další vlastnosti jsou uvnitř značky proxy povoleno zadat požadované nastavení, jako je scriptLocation.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-282">Additional properties are allowed inside the proxy tag to specify the required settings like scriptLocation.</span></span> <span data-ttu-id="1f2e1-283">Odkazovat na [proxy – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na syntaxi.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-283">Refer to [proxy Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on syntax.</span></span>

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. <span data-ttu-id="1f2e1-284">Konfigurační soubor uložte do původního umístění a potom restartujte službu hostitel brány správy dat, která vybere změny.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-284">Save the configuration file into the original location, then restart the Data Management Gateway Host service, which picks up the changes.</span></span> <span data-ttu-id="1f2e1-285">Restartování služby: pomocí apletu služby v Ovládacích panelech nebo z **Správce konfigurace brány pro správu dat** > klikněte na tlačítko **zastavit službu** tlačítko a pak klikněte na **Start Služba**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-285">To restart the service: use services applet from the control panel, or from the **Data Management Gateway Configuration Manager** > click the **Stop Service** button, then click the **Start Service**.</span></span> <span data-ttu-id="1f2e1-286">Pokud se služba nespustí, je pravděpodobné, že byl přidán nesprávnou syntaxi – značka XML do konfiguračního souboru aplikace, která byla upravena.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-286">If the service does not start, it is likely that an incorrect XML tag syntax has been added into the application configuration file that was edited.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1f2e1-287">Nezapomeňte aktualizovat **i** diahost.exe.config a diawp.exe.config.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-287">Do not forget to update **both** diahost.exe.config and diawp.exe.config.</span></span>  


<span data-ttu-id="1f2e1-288">Kromě těchto bodů musíte také zkontrolujte, zda že je v seznamu povolených IP adres vaší společnosti Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-288">In addition to these points, you also need to make sure Microsoft Azure is in your company’s whitelist.</span></span> <span data-ttu-id="1f2e1-289">Seznam platných Microsoft Azure IP adres si můžete stáhnout z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="1f2e1-289">The list of valid Microsoft Azure IP addresses can be downloaded from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a><span data-ttu-id="1f2e1-290">Možné příznaky brány firewall a proxy server s problémy</span><span class="sxs-lookup"><span data-stu-id="1f2e1-290">Possible symptoms for firewall and proxy server-related issues</span></span>
<span data-ttu-id="1f2e1-291">Pokud narazíte na chyby, které jsou podobné těm, které jsou následující, je pravděpodobně z důvodu nesprávné konfigurace brány firewall nebo proxy serveru, která blokuje brány v připojení k objektu pro vytváření dat ke svému ověření.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-291">If you encounter errors similar to the following ones, it is likely due to improper configuration of the firewall or proxy server, which blocks gateway from connecting to Data Factory to authenticate itself.</span></span> <span data-ttu-id="1f2e1-292">Podívejte se na předchozí část, aby brána firewall a proxy serveru jsou správně nakonfigurovány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-292">Refer to previous section to ensure your firewall and proxy server are properly configured.</span></span>

1. <span data-ttu-id="1f2e1-293">Při pokusu o registraci brány se zobrazí následující chyba: "se nepodařilo zaregistrovat klíč brány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-293">When you try to register the gateway, you receive the following error: "Failed to register the gateway key.</span></span> <span data-ttu-id="1f2e1-294">Než se pokusíte znovu zaregistrujte klíč brány, potvrďte, že je brána pro správu dat v připojeném stavu a je spuštěna hostitelská služba brány správy dat."</span><span class="sxs-lookup"><span data-stu-id="1f2e1-294">Before trying to register the gateway key again, confirm that the data management gateway is in a connected state and the Data Management Gateway Host Service is Started."</span></span>
2. <span data-ttu-id="1f2e1-295">Když otevřete nástroje Configuration Manager, uvidíte stav jako "Odpojeno" nebo "Připojení".</span><span class="sxs-lookup"><span data-stu-id="1f2e1-295">When you open Configuration Manager, you see status as “Disconnected” or “Connecting.”</span></span> <span data-ttu-id="1f2e1-296">Při zobrazení protokoly událostí systému Windows, v části "Prohlížeč událostí" > "Aplikace a služby Logs" > "Brána pro správu dat", se zobrazí chybové zprávy, jako je například k následující chybě:`Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`</span><span class="sxs-lookup"><span data-stu-id="1f2e1-296">When viewing Windows event logs, under “Event Viewer” > “Application and Services Logs” > “Data Management Gateway”, you see error messages such as the following error: `Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`</span></span>

### <a name="open-port-8050-for-credential-encryption"></a><span data-ttu-id="1f2e1-297">Otevřete port 8050 pro šifrování přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-297">Open port 8050 for credential encryption</span></span>
<span data-ttu-id="1f2e1-298">**Nastavení pověření** aplikace používá portu pro příchozí spojení **8050** k předávání přes přihlašovací údaje k bráně při nastavování služby místní propojené na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-298">The **Setting Credentials** application uses the inbound port **8050** to relay credentials to the gateway when you set up an on-premises linked service in the Azure portal.</span></span> <span data-ttu-id="1f2e1-299">Během instalace brány ve výchozím nastavení, instalaci brány nástroje ho otevře na počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-299">During gateway setup, by default, the gateway installation opens it on the gateway machine.</span></span>

<span data-ttu-id="1f2e1-300">Pokud používáte bránu firewall jiného výrobce, můžete ručně otevřete port 8050.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-300">If you are using a third-party firewall, you can manually open the port 8050.</span></span> <span data-ttu-id="1f2e1-301">Pokud narazíte na problém brány firewall během instalace brány, můžete zkusit pomocí následujícího příkazu se nainstalovat bránu bez konfiguraci brány firewall.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-301">If you run into firewall issue during gateway setup, you can try using the following command to install the gateway without configuring the firewall.</span></span>

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

<span data-ttu-id="1f2e1-302">Pokud se rozhodnete, že není otevřete port 8050 na počítači s bránou, použijte mechanismy pro než pomocí **nastavení pověření** aplikace nakonfigurovat přihlašovací údaje k úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-302">If you choose not to open the port 8050 on the gateway machine, use mechanisms other than using the **Setting Credentials** application to configure data store credentials.</span></span> <span data-ttu-id="1f2e1-303">Například můžete použít [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-303">For example, you could use [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="1f2e1-304">V tématu [nastavení přihlašovacích údajů a zabezpečení](#set-credentials-and-securityy) oddíl na tom, jak data ukládat přihlašovací údaje lze nastavit.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-304">See [Setting Credentials and Security](#set-credentials-and-securityy) section on how data store credentials can be set.</span></span>

## <a name="update"></a><span data-ttu-id="1f2e1-305">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="1f2e1-305">Update</span></span>
<span data-ttu-id="1f2e1-306">Ve výchozím nastavení se Brána pro správu dat automaticky aktualizuje až bude dostupná novější verze brány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-306">By default, data management gateway is automatically updated when a newer version of the gateway is available.</span></span> <span data-ttu-id="1f2e1-307">Brána není aktualizován, dokud všechny naplánované úlohy se provádějí.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-307">The gateway is not updated until all the scheduled tasks are done.</span></span> <span data-ttu-id="1f2e1-308">Žádné další úlohy jsou zpracovávány bránou, dokud se nedokončí operaci aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-308">No further tasks are processed by the gateway until the update operation is completed.</span></span> <span data-ttu-id="1f2e1-309">Pokud se aktualizace nezdaří, brány je vrácena zpět na původní verzi.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-309">If the update fails, gateway is rolled back to the old version.</span></span>

<span data-ttu-id="1f2e1-310">Zobrazí čas naplánované aktualizace na těchto místech:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-310">You see the scheduled update time in the following places:</span></span>

* <span data-ttu-id="1f2e1-311">Stránka Vlastnosti brány na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-311">The gateway properties page in the Azure portal.</span></span>
* <span data-ttu-id="1f2e1-312">Domovská stránka nástroje správu Správce konfigurace brány dat</span><span class="sxs-lookup"><span data-stu-id="1f2e1-312">Home page of the Data Management Gateway Configuration Manager</span></span>
* <span data-ttu-id="1f2e1-313">Zpráva oznámení panelu systému.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-313">System tray notification message.</span></span>

<span data-ttu-id="1f2e1-314">Kartě Domů nástroje správu Správce konfigurace brány dat zobrazí plán aktualizací a čas posledního bráně byl nainstalován aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-314">The Home tab of the Data Management Gateway Configuration Manager displays the update schedule and the last time the gateway was installed/updated.</span></span>

![Aktualizace plánu](media/data-factory-data-management-gateway/UpdateSection.png)

<span data-ttu-id="1f2e1-316">Můžete instalovat aktualizace hned, nebo počkat pro bránu, aby se automaticky aktualizovaly v naplánovaném čase.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-316">You can install the update right away or wait for the gateway to be automatically updated at the scheduled time.</span></span> <span data-ttu-id="1f2e1-317">Například následující obrázek ukazuje oznámení zobrazí v aplikaci Správce konfigurace brány společně s tlačítko aktualizace, které lze klepnout a nainstalujte ji okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-317">For example, the following image shows you the notification message shown in the Gateway Configuration Manager along with the Update button that you can click to install it immediately.</span></span>

![Aktualizace v DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

<span data-ttu-id="1f2e1-319">Oznámení na hlavním panelu systému vypadat, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-319">The notification message in the system tray would look as shown in the following image:</span></span>

![Zpráva panelu systému](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

<span data-ttu-id="1f2e1-321">Zobrazí stav operace aktualizace (ručně nebo automaticky) na hlavním panelu.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-321">You see the status of update operation (manual or automatic) in the system tray.</span></span> <span data-ttu-id="1f2e1-322">Při příštím spuštění Správce konfigurace brány, zobrazí se zpráva na panelu oznámení, že brány se aktualizovala spolu s odkazem na [co je nového tématu](data-factory-gateway-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="1f2e1-322">When you launch Gateway Configuration Manager next time, you see a message on the notification bar that the gateway has been updated along with a link to [what's new topic](data-factory-gateway-release-notes.md).</span></span>

### <a name="to-disableenable-auto-update-feature"></a><span data-ttu-id="1f2e1-323">Zapnout/vypnout automatickou aktualizaci funkce</span><span class="sxs-lookup"><span data-stu-id="1f2e1-323">To disable/enable auto-update feature</span></span>
<span data-ttu-id="1f2e1-324">Je možné zapnout/vypnout funkci Automatické aktualizace provedením následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-324">You can disable/enable the auto-update feature by doing the following steps:</span></span>

<span data-ttu-id="1f2e1-325">[Pro jeden uzel brány]</span><span class="sxs-lookup"><span data-stu-id="1f2e1-325">[For single node gateway]</span></span>
1. <span data-ttu-id="1f2e1-326">Spusťte prostředí Windows PowerShell v počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-326">Launch Windows PowerShell on the gateway machine.</span></span>
2. <span data-ttu-id="1f2e1-327">Přejděte do složky C:\Program Files\Microsoft Data správy Gateway\2.0\PowerShellScript.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-327">Switch to the C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript folder.</span></span>
3. <span data-ttu-id="1f2e1-328">Spusťte následující příkaz, který zapnout automatickou aktualizaci funkci vypnout (zakázat).</span><span class="sxs-lookup"><span data-stu-id="1f2e1-328">Run the following command to turn the auto-update feature OFF (disable).</span></span>   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. <span data-ttu-id="1f2e1-329">Chcete-li zpátky na:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-329">To turn it back on:</span></span>

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
<span data-ttu-id="1f2e1-330">[[Pro více uzly vysoce dostupnou a škálovatelnou brány (preview)](data-factory-data-management-gateway-high-availability-scalability.md)]</span><span class="sxs-lookup"><span data-stu-id="1f2e1-330">[[For multi-node highly available and scalable gateway (preview)](data-factory-data-management-gateway-high-availability-scalability.md)]</span></span>
1. <span data-ttu-id="1f2e1-331">Spusťte prostředí Windows PowerShell v počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-331">Launch Windows PowerShell on the gateway machine.</span></span>
2. <span data-ttu-id="1f2e1-332">Přejděte do složky C:\Program Files\Microsoft Data správy Gateway\2.0\PowerShellScript.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-332">Switch to the C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript folder.</span></span>
3. <span data-ttu-id="1f2e1-333">Spusťte následující příkaz, který zapnout automatickou aktualizaci funkci vypnout (zakázat).</span><span class="sxs-lookup"><span data-stu-id="1f2e1-333">Run the following command to turn the auto-update feature OFF (disable).</span></span>   

    <span data-ttu-id="1f2e1-334">Pro bránu s funkci vysoké dostupnosti (preview) se vyžaduje navíc AuthKey param.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-334">For gateway with high availability feature (preview), an extra AuthKey param is required.</span></span>
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. <span data-ttu-id="1f2e1-335">Chcete-li zpátky na:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-335">To turn it back on:</span></span>

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install the gateway, you can launch Data Management Gateway Configuration Manager in one of the following ways:

1. In the **Search** window, type **Data Management Gateway** to access this utility.
2. Run the executable **ConfigManager.exe** in the folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
The Home page allows you to do the following actions:

* View status of the gateway (connected to the cloud service etc.).
* **Register** using a key from the portal.
* **Stop** and start the **Data Management Gateway Host service** on the gateway machine.
* **Schedule updates** at a specific time of the days.
* View the date when the gateway was **last updated**.

### Settings page
The Settings page allows you to do the following actions:

* View, change, and export **certificate** used by the gateway. This certificate is used to encrypt data source credentials.
* Change **HTTPS port** for the endpoint. The gateway opens a port for setting the data source credentials.
* **Status** of the endpoint
* View **SSL certificate** is used for SSL communication between portal and the gateway to set credentials for data sources.  

### Diagnostics page
The Diagnostics page allows you to do the following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs to Microsoft if there was a failure.
* **Test connection** to a data source.  

### Help page
The Help page displays the following information:  

* Brief description of the gateway
* Version number
* Links to online help, privacy statement, and license agreement.  

## Monitor gateway in the portal
In the Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate to the home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select the **gateway** in the **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In the **Gateway** page, you can see the memory and CPU usage of the gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** to see more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

The following table provides descriptions of columns in the **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of the logical gateway and nodes associated with the gateway. Node is an on-premises Windows machine that has the gateway installed on it. For information on having more than one node (up to four nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of the logical gateway and the gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows the version of the logical gateway and each gateway node. The version of the logical gateway is determined based on version of majority of nodes in the group. If there are nodes with different versions in the logical gateway setup, only the nodes with the same version number as the logical gateway function properly. Others are in the limited mode and need to be manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies the maximum concurrent jobs for each node. This value is defined based on the machine size. You can increase the limit to scale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when the scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used to execute jobs. There is only one dispatcher node, which is used to pull tasks/jobs from cloud services and dispatch them to different worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in the gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
The following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected to Data Factory service.
Offline | Node is offline.
Upgrading | The node is being auto-updated.
Limited | Due to Connectivity issue. May be due to HTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from the configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect to other nodes. 


The following table provides possible statuses of a **logical gateway**. The gateway status depends on statuses of the gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered to this logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due to credential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure the number of **concurrent data movement jobs** that can run on a node to scale up the capability of moving data between on-premises and cloud data stores. 

When the available memory and CPU are not utilized well, but the idle capacity is 0, you should scale up by increasing the number of concurrent jobs that can run on a node. You may also want to scale up when activities are timing out because the gateway is overloaded. In the advanced settings of a gateway node, you can increase the maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using the data management gateway.  

## Move gateway from one machine to another
This section provides steps for moving gateway client from one machine to another machine.

1. In the portal, navigate to the **Data Factory home page**, and click the **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in the **DATA GATEWAYS** section of the **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In the **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In the **Configure** page, click **Download and install data gateway**, and follow instructions to install the data gateway on the machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep the **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In the **Configure** page in the portal, click **Recreate key** on the command bar, and click **Yes** for the warning message. Click **copy button** next to key text that copies the key to the clipboard. The gateway on the old machine stops functioning as soon you recreate the key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste the **key** into text box in the **Register Gateway** page of the **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box to see the key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** to register the gateway with the cloud service.
9. On the **Settings** tab, click **Change** to select the same certificate that was used with the old gateway, enter the **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from the old gateway by doing the following steps: launch Data Management Gateway Configuration Manager on the old machine, switch to the **Certificate** tab, click **Export** button and follow the instructions.
10. After successful registration of the gateway, you should see the **Registration** set to **Registered** and **Status** set to **Started** on the Home page of the Gateway Configuration Manager.

## Encrypting credentials
To encrypt credentials in the Data Factory Editor, do the following steps:

1. Launch web browser on the **gateway machine**, navigate to [Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in the **DATA FACTORY** page and then click **Author & Deploy** to launch Data Factory Editor.   
2. Click an existing **linked service** in the tree view to see its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In the JSON editor, for the **gatewayName** property, enter the name of the gateway.
4. Enter server name for the **Data Source** property in the **connectionString**.
5. Enter database name for the **Initial Catalog** property in the **connectionString**.    
6. Click **Encrypt** button on the command bar that launches the click-once **Credential Manager** application. You should see the **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In the **Setting Credentials** dialog box, do the following steps:
   1. Select **authentication** that you want the Data Factory service to use to connect to the database.
   2. Enter name of the user who has access to the database for the **USERNAME** setting.
   3. Enter password for the user for the **PASSWORD** setting.  
   4. Click **OK** to encrypt credentials and close the dialog box.
8. You should see a **encryptedCredential** property in the **connectionString** now.

    ```JSON
    {
        "name": "SqlServerLinkedService",
        "properties": {
            "type": "OnPremisesSqlServer",
            "description": "",
            "typeProperties": {
                "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                "gatewayName": "adftutorialgateway"
            }
        }
    }
    ```
<span data-ttu-id="1f2e1-336">Pokud máte přístup k portálu z počítače, který se liší od počítače brány, musí se ujistěte, že aplikace Správce přihlašovacích údajů můžete připojit k počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-336">If you access the portal from a machine that is different from the gateway machine, you must make sure that the Credentials Manager application can connect to the gateway machine.</span></span> <span data-ttu-id="1f2e1-337">Pokud aplikace nebudou moci připojit počítače brány, neumožňuje vám umožní nastavit přihlašovací údaje pro zdroj dat a otestovat připojení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-337">If the application cannot reach the gateway machine, it does not allow you to set credentials for the data source and to test connection to the data source.</span></span>  

<span data-ttu-id="1f2e1-338">Při použití **nastavení pověření** aplikace portálu šifruje přihlašovací údaje s certifikát uvedený v **certifikát** kartě **Správce konfigurace brány**  na počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-338">When you use the **Setting Credentials** application, the portal encrypts the credentials with the certificate specified in the **Certificate** tab of the **Gateway Configuration Manager** on the gateway machine.</span></span>

<span data-ttu-id="1f2e1-339">Pokud hledáte způsob založené na rozhraní API pro šifrování přihlašovacích údajů, můžete použít [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny prostředí PowerShell k šifrování přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-339">If you are looking for an API-based approach for encrypting the credentials, you can use the [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet to encrypt credentials.</span></span> <span data-ttu-id="1f2e1-340">Rutina používá certifikát tuto bránu Pokud chcete použít k šifrování přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-340">The cmdlet uses the certificate that gateway is configured to use to encrypt the credentials.</span></span> <span data-ttu-id="1f2e1-341">Přidat zašifrované přihlašovací údaje k **EncryptedCredential** element **connectionString** v kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-341">You add encrypted credentials to the **EncryptedCredential** element of the **connectionString** in the JSON.</span></span> <span data-ttu-id="1f2e1-342">Použití formátu JSON s [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) rutiny nebo v editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-342">You use the JSON with the [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) cmdlet or in the Data Factory Editor.</span></span>

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

<span data-ttu-id="1f2e1-343">Neexistuje jeden další postup pro nastavení přihlašovacích údajů pomocí editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-343">There is one more approach for setting credentials using Data Factory Editor.</span></span> <span data-ttu-id="1f2e1-344">Pokud vytvoříte službu SQL Server propojené pomocí editoru a zadejte přihlašovací údaje ve formátu prostého textu, přihlašovací údaje jsou šifrované pomocí certifikátu, který vlastní službu Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-344">If you create a SQL Server linked service by using the editor and you enter credentials in plain text, the credentials are encrypted using a certificate that the Data Factory service owns.</span></span> <span data-ttu-id="1f2e1-345">Nepoužívá certifikát této brány je nakonfigurovaný na použití.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-345">It does NOT use the certificate that gateway is configured to use.</span></span> <span data-ttu-id="1f2e1-346">Tento přístup může být v některých případech trochu rychlejší, je to méně bezpečné.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-346">While this approach might be a little faster in some cases, it is less secure.</span></span> <span data-ttu-id="1f2e1-347">Proto doporučujeme, postupujte podle tohoto přístupu pouze pro účely vývoje/testování.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-347">Therefore, we recommend that you follow this approach only for development/testing purposes.</span></span>

## <a name="powershell-cmdlets"></a><span data-ttu-id="1f2e1-348">Rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1f2e1-348">PowerShell cmdlets</span></span>
<span data-ttu-id="1f2e1-349">Tato část popisuje, jak vytvořit a registrovat bránu pomocí rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-349">This section describes how to create and register a gateway using Azure PowerShell cmdlets.</span></span>

1. <span data-ttu-id="1f2e1-350">Spusťte **prostředí Azure PowerShell** v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-350">Launch **Azure PowerShell** in administrator mode.</span></span>
2. <span data-ttu-id="1f2e1-351">Přihlaste se k účtu Azure tak, že spustíte následující příkaz a zadání přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-351">Log in to your Azure account by running the following command and entering your Azure credentials.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="1f2e1-352">Použití **New-AzureRmDataFactoryGateway** rutiny pro vytvoření logické brány následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-352">Use the **New-AzureRmDataFactoryGateway** cmdlet to create a logical gateway as follows:</span></span>

    ```PowerShell
    $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    <span data-ttu-id="1f2e1-353">**Ukázkový příkaz a výstupní**:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-353">**Example command and output**:</span></span>

    ```
    PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

    Name              : MyGateway
    Description       : gateway for walkthrough
    Version           :
    Status            : NeedRegistration
    VersionStatus     : None
    CreateTime        : 9/28/2014 10:58:22
    RegisterTime      :
    LastConnectTime   :
    ExpiryTime        :
    ProvisioningState : Succeeded
    Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI
    ```

1. <span data-ttu-id="1f2e1-354">V prostředí Azure PowerShell přejděte do složky: **C:\Program Files\Microsoft Data správy Gateway\2.0\PowerShellScript\**.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-354">In Azure PowerShell, switch to the folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**.</span></span> <span data-ttu-id="1f2e1-355">Spustit **RegisterGateway.ps1** přidružený k místní proměnné **$Key** jak je znázorněno v následujícím příkazu.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-355">Run **RegisterGateway.ps1** associated with the local variable **$Key** as shown in the following command.</span></span> <span data-ttu-id="1f2e1-356">Tento skript zaregistruje Klientský agent nainstalovaný na počítači s logické brány, kterou vytvoříte dříve.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-356">This script registers the client agent installed on your machine with the logical gateway you create earlier.</span></span>

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    <span data-ttu-id="1f2e1-357">Pomocí parametru IsRegisterOnRemoteMachine můžete registrovat bránu ve vzdáleném počítači.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-357">You can register the gateway on a remote machine by using the IsRegisterOnRemoteMachine parameter.</span></span> <span data-ttu-id="1f2e1-358">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1f2e1-358">Example:</span></span>

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. <span data-ttu-id="1f2e1-359">Můžete použít **Get-AzureRmDataFactoryGateway** rutiny získat seznam bran v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-359">You can use the **Get-AzureRmDataFactoryGateway** cmdlet to get the list of Gateways in your data factory.</span></span> <span data-ttu-id="1f2e1-360">Když **stav** ukazuje **online**, znamená to vaše brána je připravený k použití.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-360">When the **Status** shows **online**, it means your gateway is ready to use.</span></span>

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
<span data-ttu-id="1f2e1-361">Můžete odebrat brány pomocí **odebrat AzureRmDataFactoryGateway** brány pomocí rutiny a aktualizace popis **Set-AzureRmDataFactoryGateway** rutiny.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-361">You can remove a gateway using the **Remove-AzureRmDataFactoryGateway** cmdlet and update description for a gateway using the **Set-AzureRmDataFactoryGateway** cmdlets.</span></span> <span data-ttu-id="1f2e1-362">Syntaxe a další podrobnosti o těchto rutinách najdete v tématu referenční Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-362">For syntax and other details about these cmdlets, see Data Factory Cmdlet Reference.</span></span>  

### <a name="list-gateways-using-powershell"></a><span data-ttu-id="1f2e1-363">Seznam brány pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1f2e1-363">List gateways using PowerShell</span></span>

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a><span data-ttu-id="1f2e1-364">Odebrat bránu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1f2e1-364">Remove gateway using PowerShell</span></span>

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a><span data-ttu-id="1f2e1-365">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f2e1-365">Next steps</span></span>
* <span data-ttu-id="1f2e1-366">V tématu [přesun dat mezi místní a cloudové úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-366">See [Move data between on-premises and cloud data stores](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="1f2e1-367">V tomto návodu vytvoříte kanál, který používá bránu pro přesun dat z databáze SQL serveru místně do objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="1f2e1-367">In the walkthrough, you create a pipeline that uses the gateway to move data from an on-premises SQL Server database to an Azure blob.</span></span>  