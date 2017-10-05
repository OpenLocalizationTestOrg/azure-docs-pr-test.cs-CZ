---
title: Cluster HPC Pack pro aplikaci Excel a SOA | Microsoft Docs
description: "Začínáme s rozsáhlé úlohy aplikace Excel a SOA v clusteru HPC Pack v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: cb6a9abe-caf3-44da-b911-849a50f6cfb3
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 63babd94fdab15217cfb0757e4cd6efe458a628d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="8320e-103">Začít a spustit úlohy aplikace Excel a SOA v clusteru HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="8320e-103">Get started running Excel and SOA workloads on an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="8320e-104">Tento článek ukazuje, jak nasadit cluster Microsoft HPC Pack 2012 R2 na virtuálních počítačích Azure pomocí šablony Azure rychlý start, nebo volitelně skript nasazení Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8320e-104">This article shows you how to deploy a Microsoft HPC Pack 2012 R2 cluster on Azure virtual machines by using an Azure quickstart template, or optionally an Azure PowerShell deployment script.</span></span> <span data-ttu-id="8320e-105">Cluster využívá Image virtuálního počítače Azure Marketplace, které jsou navrženy pro spouštění úloh orientované na služby architektura (SOA) nebo Microsoft Excel pomocí sady HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="8320e-105">The cluster uses Azure Marketplace VM images designed to run Microsoft Excel or service-oriented architecture (SOA) workloads with HPC Pack.</span></span> <span data-ttu-id="8320e-106">Clusteru můžete použít ke spuštění aplikace Excel HPC a SOA služby z klientského počítače k místní.</span><span class="sxs-lookup"><span data-stu-id="8320e-106">You can use the cluster to run Excel HPC and SOA services from an on-premises client computer.</span></span> <span data-ttu-id="8320e-107">Služby HPC pro Excel obsahovat snižování zátěže sešitu aplikace Excel a uživatelem definované funkce aplikace Excel nebo UDF.</span><span class="sxs-lookup"><span data-stu-id="8320e-107">The Excel HPC services include Excel workbook offloading and Excel user-defined functions, or UDFs.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="8320e-108">Tento článek vychází z funkce, šablony a skripty pro HPC Pack 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="8320e-108">This article is based on features, templates, and scripts for HPC Pack 2012 R2.</span></span> <span data-ttu-id="8320e-109">Tento scénář není aktuálně podporován v prostředí HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="8320e-109">This scenario is not currently supported in HPC Pack 2016.</span></span>
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="8320e-110">Následující diagram znázorňuje na vysoké úrovni clusteru HPC Pack, že který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="8320e-110">At a high level, the following diagram shows the HPC Pack cluster you create.</span></span>

![Cluster HPC s uzly, které jsou spuštěné úlohy aplikace Excel][scenario]

## <a name="prerequisites"></a><span data-ttu-id="8320e-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8320e-112">Prerequisites</span></span>
* <span data-ttu-id="8320e-113">**Klientský počítač** -potřebujete počítač klienta se systémem Windows k odesílání ukázkové aplikace Excel a SOA úloh do clusteru.</span><span class="sxs-lookup"><span data-stu-id="8320e-113">**Client computer** - You need a Windows-based client computer to submit sample Excel and SOA jobs to the cluster.</span></span> <span data-ttu-id="8320e-114">Musíte taky počítač se systémem Windows pro spuštění skriptu nasazení clusteru Azure PowerShell (Pokud zvolíte tuto metodu nasazení).</span><span class="sxs-lookup"><span data-stu-id="8320e-114">You also need a Windows computer to run the Azure PowerShell cluster deployment script (if you choose that deployment method).</span></span>
* <span data-ttu-id="8320e-115">**Předplatné Azure** – Pokud nemáte předplatné Azure, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="8320e-115">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="8320e-116">**Kvóta jader** -možná budete muset zvýšit kvótu jádra, zvlášť pokud nasadíte několik uzlů clusteru s vícejádrovými velikosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8320e-116">**Cores quota** - You might need to increase the quota of cores, especially if you deploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="8320e-117">Pokud používáte šablonu Azure rychlý start, kvóta jádra ve službě Správce prostředků je za oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="8320e-117">If you are using an Azure quickstart template, the cores quota in Resource Manager is per Azure region.</span></span> <span data-ttu-id="8320e-118">V takovém případě je potřeba zvýšit kvótu v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="8320e-118">In that case, you might need to increase the quota in a specific region.</span></span> <span data-ttu-id="8320e-119">V tématu [limity předplatného Azure, kvóty a omezení](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="8320e-119">See [Azure subscription limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="8320e-120">Pokud chcete zvýšit kvótu, [otevřete žádosti o podporu online zákazníka](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zdarma.</span><span class="sxs-lookup"><span data-stu-id="8320e-120">To increase a quota, [open an online customer support request](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) at no charge.</span></span>
* <span data-ttu-id="8320e-121">**Licenci aplikace Microsoft Office** – Pokud nasadíte výpočetní uzly bitovou kopii virtuálního počítače Marketplace HPC Pack 2012 R2 pomocí aplikace Microsoft Excel, 30denní zkušební verzi Microsoft Excelu Professional Plus 2013 je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="8320e-121">**Microsoft Office license** - If you deploy compute nodes using a Marketplace HPC Pack 2012 R2 VM image with Microsoft Excel, a 30-day evaluation version of Microsoft Excel Professional Plus 2013 is installed.</span></span> <span data-ttu-id="8320e-122">Po uplynutí zkušebního období musíte zadat platnou licenci aplikace Microsoft Office Excel nadále spouštět úlohy aktivovat.</span><span class="sxs-lookup"><span data-stu-id="8320e-122">After the evaluation period, you need to provide a valid Microsoft Office license to activate Excel to continue to run workloads.</span></span> <span data-ttu-id="8320e-123">V tématu [aktivace v aplikaci Excel](#excel-activation) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="8320e-123">See [Excel activation](#excel-activation) later in this article.</span></span> 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="8320e-124">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="8320e-124">Step 1.</span></span> <span data-ttu-id="8320e-125">Nastavení clusteru služby HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="8320e-125">Set up an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="8320e-126">Ukážeme dvě možnosti pro nastavení clusteru HPC Pack 2012 R2: první, pomocí šablonu Azure rychlý start a webu Azure portal; a Zadruhé, pomocí skript nasazení Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8320e-126">We show two options to set up the HPC Pack 2012 R2 cluster: first, using an Azure quickstart template and the Azure portal; and second, using an Azure PowerShell deployment script.</span></span>

### <a name="option-1-use-a-quickstart-template"></a><span data-ttu-id="8320e-127">Možnost 1.</span><span class="sxs-lookup"><span data-stu-id="8320e-127">Option 1.</span></span> <span data-ttu-id="8320e-128">Použít šabloně pro rychlý start</span><span class="sxs-lookup"><span data-stu-id="8320e-128">Use a quickstart template</span></span>
<span data-ttu-id="8320e-129">Rychlé nasazení clusteru HPC Pack na portálu Azure pomocí šablony Azure rychlý start.</span><span class="sxs-lookup"><span data-stu-id="8320e-129">Use an Azure quickstart template to quickly deploy an HPC Pack cluster in the Azure portal.</span></span> <span data-ttu-id="8320e-130">Když otevřete šablonu na portálu, získáte jednoduchého uživatelského rozhraní, kde můžete zadat nastavení pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="8320e-130">When you open the template in the portal, you get a simple UI where you enter the settings for your cluster.</span></span> <span data-ttu-id="8320e-131">Tady jsou kroky.</span><span class="sxs-lookup"><span data-stu-id="8320e-131">Here are the steps.</span></span> 

> [!TIP]
> <span data-ttu-id="8320e-132">Pokud chcete, použijte [šablony Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) vytvářející cluster podobné speciálně pro zatížení aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-132">If you want, use an [Azure Marketplace template](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) that creates a similar cluster specifically for Excel workloads.</span></span> <span data-ttu-id="8320e-133">Kroky mírně lišit od následující.</span><span class="sxs-lookup"><span data-stu-id="8320e-133">The steps differ slightly from the following.</span></span>
> 
> 

1. <span data-ttu-id="8320e-134">Přejděte [stránku šablony vytvořit Cluster prostředí HPC na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span><span class="sxs-lookup"><span data-stu-id="8320e-134">Visit the [Create HPC Cluster template page on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span></span> <span data-ttu-id="8320e-135">Pokud chcete, přečtěte si informace o šabloně a zdrojový kód.</span><span class="sxs-lookup"><span data-stu-id="8320e-135">If you want, review information about the template and the source code.</span></span>
2. <span data-ttu-id="8320e-136">Klikněte na tlačítko **nasadit do Azure** zahájíte nasazení pomocí šablony na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8320e-136">Click **Deploy to Azure** to start a deployment with the template in the Azure portal.</span></span>
   
   ![Nasazení šablony Azure][github]
3. <span data-ttu-id="8320e-138">Na portálu použijte následující postup zadat parametry šablony clusteru prostředí HPC.</span><span class="sxs-lookup"><span data-stu-id="8320e-138">In the portal, follow these steps to enter the parameters for the HPC cluster template.</span></span>
   
   <span data-ttu-id="8320e-139">a.</span><span class="sxs-lookup"><span data-stu-id="8320e-139">a.</span></span> <span data-ttu-id="8320e-140">Na **parametry** stránky zadejte nebo upravte hodnoty pro parametry šablony.</span><span class="sxs-lookup"><span data-stu-id="8320e-140">On the **Parameters** page, enter or modify values for the template parameters.</span></span> <span data-ttu-id="8320e-141">(Klikněte na ikonu u kteréhokoli nastavení pro informace nápovědy.) Ukázkové hodnoty jsou uvedené v následující obrazovku.</span><span class="sxs-lookup"><span data-stu-id="8320e-141">(Click the icon next to each setting for help information.) Sample values are shown in the following screen.</span></span> <span data-ttu-id="8320e-142">Tento příklad vytvoří cluster s názvem *hpc01* v *hpc.local* domény, který se skládá z hlavního uzlu a 2 výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="8320e-142">This example creates a cluster named *hpc01* in the *hpc.local* domain consisting of a head node and 2 compute nodes.</span></span> <span data-ttu-id="8320e-143">Výpočetní uzly jsou vytvořené z virtuálních počítačů HPC Pack obrázek, který obsahuje aplikace Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-143">The compute nodes are created from an HPC Pack VM image that includes Microsoft Excel.</span></span>
   
   ![Zadejte parametry][parameters-new-portal]
   
   > [!NOTE]
   > <span data-ttu-id="8320e-145">Virtuální počítač je automaticky vytvořen z hlavního uzlu [nejnovější image Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 na Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="8320e-145">The head node VM is created automatically from the [latest Marketplace image](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) of HPC Pack 2012 R2 on Windows Server 2012 R2.</span></span> <span data-ttu-id="8320e-146">Aktuálně bitovou kopii je založena na HPC Pack 2012 R2 Update 3.</span><span class="sxs-lookup"><span data-stu-id="8320e-146">Currently the image is based on HPC Pack 2012 R2 Update 3.</span></span>
   > 
   > <span data-ttu-id="8320e-147">Výpočetní uzel virtuální počítače jsou vytvořeny z nejnovější bitové kopie rodiny vybrané výpočetního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8320e-147">Compute node VMs are created from the latest image of the selected compute node family.</span></span> <span data-ttu-id="8320e-148">Vyberte **ComputeNodeWithExcel** možnost pro nejnovější aktualizaci HPC Pack výpočetní uzel image, která obsahuje zkušební verzi Microsoft Excelu Professional Plus 2013.</span><span class="sxs-lookup"><span data-stu-id="8320e-148">Select the **ComputeNodeWithExcel** option for the latest HPC Pack compute node image that includes an evaluation version of Microsoft Excel Professional Plus 2013.</span></span> <span data-ttu-id="8320e-149">Chcete-li nasadit cluster s podporou pro obecné SOA relací nebo snižování zátěže systému souborů UDF Excelu, zvolte **ComputeNode** možnost (bez nainstalované aplikace Excel).</span><span class="sxs-lookup"><span data-stu-id="8320e-149">To deploy a cluster for general SOA sessions or for Excel UDF offloading, choose the **ComputeNode** option (without Excel installed).</span></span>
   > 
   > 
   
   <span data-ttu-id="8320e-150">b.</span><span class="sxs-lookup"><span data-stu-id="8320e-150">b.</span></span> <span data-ttu-id="8320e-151">Zvolte předplatné.</span><span class="sxs-lookup"><span data-stu-id="8320e-151">Choose the subscription.</span></span>
   
   <span data-ttu-id="8320e-152">c.</span><span class="sxs-lookup"><span data-stu-id="8320e-152">c.</span></span> <span data-ttu-id="8320e-153">Vytvořte skupinu prostředků clusteru, například *hpc01RG*.</span><span class="sxs-lookup"><span data-stu-id="8320e-153">Create a resource group for the cluster, such as *hpc01RG*.</span></span>
   
   <span data-ttu-id="8320e-154">d.</span><span class="sxs-lookup"><span data-stu-id="8320e-154">d.</span></span> <span data-ttu-id="8320e-155">Vyberte umístění pro skupinu prostředků, jako je například střed USA.</span><span class="sxs-lookup"><span data-stu-id="8320e-155">Choose a location for the resource group, such as Central US.</span></span>
   
   <span data-ttu-id="8320e-156">e.</span><span class="sxs-lookup"><span data-stu-id="8320e-156">e.</span></span> <span data-ttu-id="8320e-157">Na **právní podmínky** stránky, přečtěte si podmínky.</span><span class="sxs-lookup"><span data-stu-id="8320e-157">On the **Legal terms** page, review the terms.</span></span> <span data-ttu-id="8320e-158">Pokud souhlasíte, klikněte na tlačítko **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="8320e-158">If you agree, click **Purchase**.</span></span> <span data-ttu-id="8320e-159">Po dokončení nastavení hodnoty pro šablonu, klepněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8320e-159">Then, when you are finished setting the values for the template, click **Create**.</span></span>
4. <span data-ttu-id="8320e-160">Po dokončení nasazení (obvykle trvá přibližně 30 minut), exportovat soubor certifikátu clusteru z hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="8320e-160">When the deployment completes (it typically takes around 30 minutes), export the cluster certificate file from the cluster head node.</span></span> <span data-ttu-id="8320e-161">V pozdější fázi importujete tento veřejný certifikát na klientském počítači, aby poskytovala ověřování na straně serveru pro zabezpečené vazby HTTP.</span><span class="sxs-lookup"><span data-stu-id="8320e-161">In a later step, you import this public certificate on the client computer to provide the server-side authentication for secure HTTP binding.</span></span>
   
   <span data-ttu-id="8320e-162">a.</span><span class="sxs-lookup"><span data-stu-id="8320e-162">a.</span></span> <span data-ttu-id="8320e-163">V portálu Azure, přejděte na řídicí panel, vyberte z hlavního uzlu a klikněte na **Connect** v horní části stránky a připojte se pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="8320e-163">In the Azure portal, go to the dashboard, select the head node, and click **Connect** at the top of the page to connect using Remote Desktop.</span></span>
   
    <!-- ![Connect to the head node][connect] -->
   
   <span data-ttu-id="8320e-164">b.</span><span class="sxs-lookup"><span data-stu-id="8320e-164">b.</span></span> <span data-ttu-id="8320e-165">Použijte standardní postupy ve Správci certifikátů a vyexportovat si certifikát hlavního uzlu (nachází se v části Cert: \LocalMachine\My) bez soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="8320e-165">Use standard procedures in Certificate Manager to export the head node certificate (located under Cert:\LocalMachine\My) without the private key.</span></span> <span data-ttu-id="8320e-166">V tomto příkladu exportovat *CN = hpc01.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="8320e-166">In this example, export *CN = hpc01.eastus.cloudapp.azure.com*.</span></span>
   
   ![Export certifikátu][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="8320e-168">Možnost 2.</span><span class="sxs-lookup"><span data-stu-id="8320e-168">Option 2.</span></span> <span data-ttu-id="8320e-169">Pomocí tohoto skriptu nasazení IaaS HPC Pack</span><span class="sxs-lookup"><span data-stu-id="8320e-169">Use the HPC Pack IaaS Deployment script</span></span>
<span data-ttu-id="8320e-170">Skript nasazení HPC Pack IaaS poskytuje další univerzální způsob nasazení clusteru HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="8320e-170">The HPC Pack IaaS deployment script provides another versatile way to deploy an HPC Pack cluster.</span></span> <span data-ttu-id="8320e-171">Vytvoří cluster v modelu nasazení classic, zatímco šablona používá model nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8320e-171">It creates a cluster in the classic deployment model, whereas the template uses the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="8320e-172">Skript je také kompatibilní s předplatným ve službě Azure globální nebo Azure China.</span><span class="sxs-lookup"><span data-stu-id="8320e-172">Also, the script is compatible with a subscription in either the Azure Global or Azure China service.</span></span>

<span data-ttu-id="8320e-173">**Další požadavky**</span><span class="sxs-lookup"><span data-stu-id="8320e-173">**Additional prerequisites**</span></span>

* <span data-ttu-id="8320e-174">**Prostředí Azure PowerShell** - [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) (verze 0.8.10 nebo novější) na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="8320e-174">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="8320e-175">**Skript nasazení HPC Pack IaaS** – stáhněte a rozbalte nejnovější verzi skript z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="8320e-175">**HPC Pack IaaS deployment script** - Download and unpack the latest version of the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="8320e-176">Zkontrolujte verzi skript spuštěním `New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="8320e-176">Check the version of the script by running `New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="8320e-177">Tento článek je založen na verzi 4.5.0 nebo později skript.</span><span class="sxs-lookup"><span data-stu-id="8320e-177">This article is based on version 4.5.0 or later of the script.</span></span>

<span data-ttu-id="8320e-178">**Vytvoření konfiguračního souboru**</span><span class="sxs-lookup"><span data-stu-id="8320e-178">**Create the configuration file**</span></span>

 <span data-ttu-id="8320e-179">Skript nasazení HPC Pack IaaS používá jako vstup, který popisuje infrastruktura clusteru HPC konfigurační soubor XML.</span><span class="sxs-lookup"><span data-stu-id="8320e-179">The HPC Pack IaaS deployment script uses an XML configuration file as input that describes the infrastructure of the HPC cluster.</span></span> <span data-ttu-id="8320e-180">Chcete-li nasadit cluster, který se skládá z hlavního uzlu a 18 výpočetních uzlů vytvořené z image výpočetního uzlu, která obsahuje aplikaci Microsoft Excel, nahraďte hodnoty pro vaše prostředí do následující vzorový konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="8320e-180">To deploy a cluster consisting of a head node and 18 compute nodes created from the compute node image that includes Microsoft Excel, substitute values for your environment into the following sample configuration file.</span></span> <span data-ttu-id="8320e-181">Další informace o konfiguračním souboru, najdete v souboru Manual.rtf ve složce skriptu a [vytvoření clusteru prostředí HPC pomocí skriptu pro nasazení HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8320e-181">For more information about the configuration file, see the Manual.rtf file in the script folder and [Create an HPC cluster with the HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

<span data-ttu-id="8320e-182">**Poznámky k konfiguračního souboru**</span><span class="sxs-lookup"><span data-stu-id="8320e-182">**Notes about the configuration file**</span></span>

* <span data-ttu-id="8320e-183">**VMName** hlavního uzlu **musí** být stejný jako **ServiceName**, nebo úlohy architektury SOA se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="8320e-183">The **VMName** of the head node **MUST** be the same as the **ServiceName**, or SOA jobs fail to run.</span></span>
* <span data-ttu-id="8320e-184">Je nutné zadat **EnableWebPortal** tak, aby je generována a exportovat certifikát hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8320e-184">Make sure you specify **EnableWebPortal** so that the head node certificate is generated and exported.</span></span>
* <span data-ttu-id="8320e-185">Soubor Určuje skript prostředí PowerShell po konfiguraci PostConfig.ps1, která běží na hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8320e-185">The file specifies a post-configuration PowerShell script PostConfig.ps1 that runs on the head node.</span></span> <span data-ttu-id="8320e-186">Následující ukázkový skript nakonfiguruje připojovací řetězec úložiště Azure, odebere roli výpočetní uzel z hlavního uzlu a při jejich nasazení přináší všechny uzly online.</span><span class="sxs-lookup"><span data-stu-id="8320e-186">THe following sample script configures the Azure storage connection string, removes the compute node role from the head node, and brings all nodes online when they are deployed.</span></span> 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

<span data-ttu-id="8320e-187">**Spusťte skript**</span><span class="sxs-lookup"><span data-stu-id="8320e-187">**Run the script**</span></span>

1. <span data-ttu-id="8320e-188">Otevřete konzolu prostředí PowerShell v klientském počítači jako správce.</span><span class="sxs-lookup"><span data-stu-id="8320e-188">Open the PowerShell console on the client computer as an administrator.</span></span>
2. <span data-ttu-id="8320e-189">Změňte adresář na složku skriptu (E:\IaaSClusterScript v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="8320e-189">Change directory to the script folder (E:\IaaSClusterScript in this example).</span></span>
   
   ```
   cd E:\IaaSClusterScript
   ```
3. <span data-ttu-id="8320e-190">Nasazení clusteru HPC Pack, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="8320e-190">To deploy the HPC Pack cluster, run the following command.</span></span> <span data-ttu-id="8320e-191">Tento příklad předpokládá, že konfigurační soubor nachází v E:\HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="8320e-191">This example assumes that the configuration file is located in E:\HPCDemoConfig.xml.</span></span>
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

<span data-ttu-id="8320e-192">Skript nasazení HPC Pack spustí nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="8320e-192">The HPC Pack deployment script runs for some time.</span></span> <span data-ttu-id="8320e-193">Jednou z věcí, které nemá skript je exportovat a stáhněte certifikát clusteru a uložit do složky Dokumenty aktuální uživatel v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="8320e-193">One thing the script does is to export and download the cluster certificate and save it in the current user’s Documents folder on the client computer.</span></span> <span data-ttu-id="8320e-194">Tento skript generuje zprávu podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="8320e-194">The script generates a message similar to the following.</span></span> <span data-ttu-id="8320e-195">V tomto kroku importujete certifikát v úložišti příslušný certifikát.</span><span class="sxs-lookup"><span data-stu-id="8320e-195">In a following step, you import the certificate in the appropriate certificate store.</span></span>    

    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a><span data-ttu-id="8320e-196">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="8320e-196">Step 2.</span></span> <span data-ttu-id="8320e-197">Snižování zátěže sešitů aplikace Excel a spusťte UDF z místního klienta</span><span class="sxs-lookup"><span data-stu-id="8320e-197">Offload Excel workbooks and run UDFs from an on-premises client</span></span>
### <a name="excel-activation"></a><span data-ttu-id="8320e-198">Aktivace aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="8320e-198">Excel activation</span></span>
<span data-ttu-id="8320e-199">Pokud používáte image virtuálního počítače ComputeNodeWithExcel pro úlohy v produkčním prostředí, je třeba zadat platný klíč licence Microsoft Office k aktivaci aplikace Excel na výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="8320e-199">When using the ComputeNodeWithExcel VM image for production workloads, you need to provide a valid Microsoft Office license key to activate Excel on the compute nodes.</span></span> <span data-ttu-id="8320e-200">Jinak po 30 dnech vyprší platnost zkušební verzi aplikace Excel a systémem sešitů aplikace Excel se nezdaří s COMException (0x800AC472).</span><span class="sxs-lookup"><span data-stu-id="8320e-200">Otherwise, the evaluation version of Excel expires after 30 days, and running Excel workbooks will fail with the COMException (0x800AC472).</span></span> 

<span data-ttu-id="8320e-201">Obnovení aplikace Excel můžete aktivačního období pro jiné 30 dnů od doby vyhodnocení: Přihlaste se k hlavnímu uzlu a clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` na všechny aplikace Excel výpočetní uzly prostřednictvím Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="8320e-201">You can rearm Excel for another 30 days of evaluation time: Log on to the head node and clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` on all Excel compute nodes via HPC Cluster Manager.</span></span> <span data-ttu-id="8320e-202">Po obnovení aktivačního období maximálně dvakrát.</span><span class="sxs-lookup"><span data-stu-id="8320e-202">You can rearm a maximum of two times.</span></span> <span data-ttu-id="8320e-203">Potom je nutné zadat platný klíč licence Office.</span><span class="sxs-lookup"><span data-stu-id="8320e-203">After that, you must provide a valid Office license key.</span></span>

<span data-ttu-id="8320e-204">Office Professional Plus 2013 nainstalovaný na bitovou kopii virtuálního počítače je multilicenční edici s obecné svazku licenční klíč (kód GVLK).</span><span class="sxs-lookup"><span data-stu-id="8320e-204">The Office Professional Plus 2013 installed on the VM image is a volume edition with a Generic Volume License Key (GVLK).</span></span> <span data-ttu-id="8320e-205">Aktivujte ji prostřednictvím služby správy klíčů (KMS) nebo aktivaci prostřednictvím služby (AD-BA) nebo klíč k vícenásobné aktivaci (MAK).</span><span class="sxs-lookup"><span data-stu-id="8320e-205">You can activate it via Key Management Service (KMS)/Active Directory-Based Activation (AD-BA) or Multiple Activation Key (MAK).</span></span> 

    * <span data-ttu-id="8320e-206">Pomocí služby správy KLÍČŮ/AD-BA, použít existující server služby správy KLÍČŮ nebo nastavit novou pomocí sady Microsoft Office 2013 svazku licence.</span><span class="sxs-lookup"><span data-stu-id="8320e-206">To use KMS/AD-BA, use an existing KMS server or set up a new one by using the Microsoft Office 2013 Volume License Pack.</span></span> <span data-ttu-id="8320e-207">(Pokud chcete, nastavení serveru z hlavního uzlu.) Potom aktivujte klíč hostitele služby správy KLÍČŮ přes Internet nebo telefon.</span><span class="sxs-lookup"><span data-stu-id="8320e-207">(If you want to, set up the server on the head node.) Then, activate the KMS host key via the Internet or telephone.</span></span> <span data-ttu-id="8320e-208">Potom clusrun `ospp.vbs` nastavit server služby správy KLÍČŮ a port a aktivovat Office na všechny aplikace Excel výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="8320e-208">Then clusrun `ospp.vbs` to set the KMS server and port and activate Office on all the Excel compute nodes.</span></span> 

    * <span data-ttu-id="8320e-209">Použít klíč k vícenásobné aktivaci, první clusrun `ospp.vbs` k zadejte klíč a poté znovu aktivovat všechny aplikace Excel výpočetní uzly přes Internet nebo telefon.</span><span class="sxs-lookup"><span data-stu-id="8320e-209">To use MAK, first clusrun `ospp.vbs` to input the key and then activate all the Excel compute nodes via the Internet or telephone.</span></span> 

> [!NOTE]
> <span data-ttu-id="8320e-210">Prodejní kódy product key pro Office Professional Plus 2013 nelze použít s touto bitovou kopií virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8320e-210">Retail product keys for Office Professional Plus 2013 cannot be used with this VM image.</span></span> <span data-ttu-id="8320e-211">Pokud máte platné klíče a instalační médium pro edice Office nebo aplikace Excel než tato edice Office Professional Plus 2013 svazku, můžete je používat místo.</span><span class="sxs-lookup"><span data-stu-id="8320e-211">If you have valid keys and installation media for Office or Excel editions other than this Office Professional Plus 2013 volume edition, you can use them instead.</span></span> <span data-ttu-id="8320e-212">Nejprve odinstalujte tento multilicenční edici a nainstalujte na edici, ke které máte.</span><span class="sxs-lookup"><span data-stu-id="8320e-212">First uninstall this volume edition and install the edition that you have.</span></span> <span data-ttu-id="8320e-213">Jako vlastní image virtuálního počítače pro nasazení ve velkém měřítku, se dají zachytit přeinstalovaného výpočetním uzlu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-213">The reinstalled Excel compute node can be captured as a customized VM image to use in a deployment at scale.</span></span>
> 
> 

### <a name="offload-excel-workbooks"></a><span data-ttu-id="8320e-214">Snižování zátěže sešitů aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="8320e-214">Offload Excel workbooks</span></span>
<span data-ttu-id="8320e-215">Postupujte podle těchto kroků snižování zátěže sešitu aplikace Excel, tak, aby běžel v clusteru HPC Pack v Azure.</span><span class="sxs-lookup"><span data-stu-id="8320e-215">Follow these steps to offload an Excel workbook so that it runs on the HPC Pack cluster in Azure.</span></span> <span data-ttu-id="8320e-216">Chcete-li to provést, musí mít aplikaci Excel 2010 nebo 2013 v klientském počítači již nainstalována.</span><span class="sxs-lookup"><span data-stu-id="8320e-216">To do this, you must have Excel 2010 or 2013 already installed on the client computer.</span></span>

1. <span data-ttu-id="8320e-217">Použijte jednu z možností v kroku 1 k nasazení clusteru HPC Pack pomocí aplikace Excel výpočetní uzel image.</span><span class="sxs-lookup"><span data-stu-id="8320e-217">Use one of the options in Step 1 to deploy an HPC Pack cluster with the Excel compute node image.</span></span> <span data-ttu-id="8320e-218">Získejte clusteru soubor certifikátu (.cer) a cluster uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="8320e-218">Obtain the cluster certificate file (.cer) and cluster username and password.</span></span>
2. <span data-ttu-id="8320e-219">Na klientském počítači importujte certifikát clusteru pod Cert: \CurrentUser\Root.</span><span class="sxs-lookup"><span data-stu-id="8320e-219">On the client computer, import the cluster certificate under Cert:\CurrentUser\Root.</span></span>
3. <span data-ttu-id="8320e-220">Zkontrolujte, zda že je nainstalována aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-220">Make sure Excel is installed.</span></span> <span data-ttu-id="8320e-221">Vytvořte soubor Excel.exe.config s následující obsah ve stejné složce jako Excel.exe na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="8320e-221">Create an Excel.exe.config file with the following contents in the same folder as Excel.exe on the client computer.</span></span> <span data-ttu-id="8320e-222">Tento krok zajistí, že-in prostředí HPC Pack 2012 R2 Excel COM úspěšně načten.</span><span class="sxs-lookup"><span data-stu-id="8320e-222">This step ensures that the HPC Pack 2012 R2 Excel COM add-in loads successfully.</span></span>
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. <span data-ttu-id="8320e-223">Nastavení klienta k odesílání úloh do clusteru HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="8320e-223">Set up the client to submit jobs to the HPC Pack cluster.</span></span> <span data-ttu-id="8320e-224">Jednou z možností je stáhnout kompletní [HPC Pack 2012 R2 Update 3 instalace](http://www.microsoft.com/download/details.aspx?id=49922) a instalace sady HPC Pack klienta.</span><span class="sxs-lookup"><span data-stu-id="8320e-224">One option is to download the full [HPC Pack 2012 R2 Update 3 installation](http://www.microsoft.com/download/details.aspx?id=49922) and install the HPC Pack client.</span></span> <span data-ttu-id="8320e-225">Můžete taky stáhnout a nainstalovat [HPC Pack 2012 R2 Update 3 klienta nástroje](https://www.microsoft.com/download/details.aspx?id=49923) a příslušné Visual C++ 2010 redistributable pro tento počítač ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555) ).</span><span class="sxs-lookup"><span data-stu-id="8320e-225">Alternatively, download and install the [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923) and the appropriate Visual C++ 2010 redistributable for your computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).</span></span>
5. <span data-ttu-id="8320e-226">V tomto příkladu používáme Ukázka sešitu aplikace Excel s názvem ConvertiblePricing_Complete.xlsb.</span><span class="sxs-lookup"><span data-stu-id="8320e-226">In this example, we use a sample Excel workbook named ConvertiblePricing_Complete.xlsb.</span></span> <span data-ttu-id="8320e-227">Můžete ho stáhnout [zde](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span><span class="sxs-lookup"><span data-stu-id="8320e-227">You can download it [here](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span></span>
6. <span data-ttu-id="8320e-228">Pracovní složky, například D:\Excel\Run zkopírujte sešitu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-228">Copy the Excel workbook to a working folder such as D:\Excel\Run.</span></span>
7. <span data-ttu-id="8320e-229">Otevřete sešit aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-229">Open the Excel workbook.</span></span> <span data-ttu-id="8320e-230">Na **vývoj** pásu karet, klikněte na tlačítko **doplňky modelu COM** a potvrďte, že-in prostředí HPC Pack Excel COM byla úspěšně zavedena..</span><span class="sxs-lookup"><span data-stu-id="8320e-230">On the **Develop** ribbon, click **COM Add-Ins** and confirm that the HPC Pack Excel COM add-in is loaded successfully.</span></span>
   
   ![Add-in pro prostředí HPC Pack v aplikaci Excel][addin]
8. <span data-ttu-id="8320e-232">Změna řádky komentářů, jak je znázorněno v následující skript upravte makro VBA HPCControlMacros v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-232">Edit the VBA macro HPCControlMacros in Excel by changing the commented lines as shown in the following script.</span></span> <span data-ttu-id="8320e-233">Nahraďte příslušnými hodnotami pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="8320e-233">Substitute appropriate values for your environment.</span></span>
   
   ![Makro aplikace Excel pro HPC Pack][macro]
   
   ```
   'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
   Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
   
   'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
   Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.Initialize ActiveWorkbook
   HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
   
   'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
   HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
   HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
   ```
9. <span data-ttu-id="8320e-235">Zkopírujte adresář nahrávání například D:\Excel\Upload sešitu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-235">Copy the Excel workbook to an upload directory such as D:\Excel\Upload.</span></span> <span data-ttu-id="8320e-236">Tento adresář je určen v konstanta HPC_DependsFiles v makro VBA pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="8320e-236">This directory is specified in the HPC_DependsFiles constant in the VBA macro.</span></span>
10. <span data-ttu-id="8320e-237">Pokud chcete spustit sešit v clusteru v Azure, klikněte na tlačítko **clusteru** tlačítko na listu.</span><span class="sxs-lookup"><span data-stu-id="8320e-237">To run the workbook on the cluster in Azure, click the **Cluster** button on the worksheet.</span></span>

### <a name="run-excel-udfs"></a><span data-ttu-id="8320e-238">Spusťte systém souborů UDF Excelu</span><span class="sxs-lookup"><span data-stu-id="8320e-238">Run Excel UDFs</span></span>
<span data-ttu-id="8320e-239">Pokud chcete spustit systém souborů UDF Excelu, postupujte podle předchozích kroků 1 – 3 můžete nastavit v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="8320e-239">To run Excel UDFs, follow the preceding steps 1 – 3 to set up the client computer.</span></span> <span data-ttu-id="8320e-240">Pro systém souborů UDF Excelu nemusíte mít nainstalovanou na výpočetních uzlech aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-240">For Excel UDFs, you don't need to have the Excel application installed on compute nodes.</span></span> <span data-ttu-id="8320e-241">Ano při vytváření clusteru výpočetních uzlů, mohli byste normální výpočetní uzel image místo image výpočetního uzlu pomocí aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-241">So, when creating your cluster compute nodes, you could choose a normal compute node image instead of the compute node image with Excel.</span></span>

> [!NOTE]
> <span data-ttu-id="8320e-242">Existuje limit 34 znak v aplikaci Excel 2010 a 2013 dialogové okno konektor clusteru.</span><span class="sxs-lookup"><span data-stu-id="8320e-242">There is a 34 character limit in the Excel 2010 and 2013 cluster connector dialog box.</span></span> <span data-ttu-id="8320e-243">Toto dialogové okno pomůže zadejte požadovaný cluster, který běží UDF.</span><span class="sxs-lookup"><span data-stu-id="8320e-243">You use this dialog box to specify the cluster that runs the UDFs.</span></span> <span data-ttu-id="8320e-244">Pokud je název clusteru úplné delší (například hpcexcelhn01.southeastasia.cloudapp.azure.com), v dialogovém okně nevejde.</span><span class="sxs-lookup"><span data-stu-id="8320e-244">If the full cluster name is longer (for example, hpcexcelhn01.southeastasia.cloudapp.azure.com), it does not fit in the dialog box.</span></span> <span data-ttu-id="8320e-245">Řešením je třeba nastavit proměnnou celého systému *CCP_IAASHN* s hodnotou název dlouho clusteru.</span><span class="sxs-lookup"><span data-stu-id="8320e-245">The workaround is to set a machine-wide variable such as *CCP_IAASHN* with the value of the long cluster name.</span></span> <span data-ttu-id="8320e-246">Potom zadejte *CCP_IAASHN %* v dialogovém okně jako název hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="8320e-246">Then, enter *%CCP_IAASHN%* in the dialog box as the cluster head node name.</span></span> 
> 
> 

<span data-ttu-id="8320e-247">Po úspěšném nasazení clusteru, pokračujte tyto kroky a spusťte ukázku předdefinované UDF Excelu.</span><span class="sxs-lookup"><span data-stu-id="8320e-247">After the cluster is successfully deployed, continue with the following steps to run a sample built-in Excel UDF.</span></span> <span data-ttu-id="8320e-248">Vlastní systém souborů UDF Excelu, najdete v těchto [prostředky](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) sestavení XLL a nasadit je na IaaS clusteru.</span><span class="sxs-lookup"><span data-stu-id="8320e-248">For customized Excel UDFs, see these [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) to build the XLLs and deploy them on the IaaS cluster.</span></span>

1. <span data-ttu-id="8320e-249">Otevřete nový sešit aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8320e-249">Open a new Excel workbook.</span></span> <span data-ttu-id="8320e-250">Na **vývoj** pásu karet, klikněte na tlačítko **doplňky**.</span><span class="sxs-lookup"><span data-stu-id="8320e-250">On the **Develop** ribbon, click **Add-Ins**.</span></span> <span data-ttu-id="8320e-251">Klikněte v dialogovém okně **Procházet**, přejděte do složky %CCP_HOME%Bin\XLL32 a vyberte vzorek ClusterUDF32.xll.</span><span class="sxs-lookup"><span data-stu-id="8320e-251">Then, in the dialog box, click **Browse**, navigate to the %CCP_HOME%Bin\XLL32 folder, and select the sample ClusterUDF32.xll.</span></span> <span data-ttu-id="8320e-252">Pokud ClusterUDF32 neexistuje na klientský počítač, zkopírujte jej ze složky %CCP_HOME%Bin\XLL32 z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8320e-252">If the ClusterUDF32 doesn't exist on the client machine, copy it from the %CCP_HOME%Bin\XLL32 folder on the head node.</span></span>
   
   ![Vyberte UDF][udf]
2. <span data-ttu-id="8320e-254">Klikněte na tlačítko **soubor** > **možnosti** > **rozšířené**.</span><span class="sxs-lookup"><span data-stu-id="8320e-254">Click **File** > **Options** > **Advanced**.</span></span> <span data-ttu-id="8320e-255">V části **vzorce**, zkontrolujte **povolit uživatelsky definované funkce XLL ke spuštění výpočetního clusteru**.</span><span class="sxs-lookup"><span data-stu-id="8320e-255">Under **Formulas**, check **Allow user-defined XLL functions to run a compute cluster**.</span></span> <span data-ttu-id="8320e-256">Pak klikněte na tlačítko **možnosti** a zadejte název clusteru úplné v **název hlavního uzlu clusteru**.</span><span class="sxs-lookup"><span data-stu-id="8320e-256">Then click **Options** and enter the full cluster name in **Cluster head node name**.</span></span> <span data-ttu-id="8320e-257">(Jak je uvedeno dříve toto vstupní pole je omezený na 34 znaků, proto nemusí podle názvu dlouho clusteru.</span><span class="sxs-lookup"><span data-stu-id="8320e-257">(As noted previously this input box is limited to 34 characters, so a long cluster name may not fit.</span></span> <span data-ttu-id="8320e-258">Proměnnou celého systému může použít pro název dlouho clusteru.)</span><span class="sxs-lookup"><span data-stu-id="8320e-258">You may use a machine-wide variable here for a long cluster name.)</span></span>
   
   ![Konfigurace UDF][options]
3. <span data-ttu-id="8320e-260">Chcete-li spustit výpočet UDF v clusteru, klikněte na buňky s hodnotou =XllGetComputerNameC() a stiskněte Enter.</span><span class="sxs-lookup"><span data-stu-id="8320e-260">To run the UDF calculation on the cluster, click the cell with value =XllGetComputerNameC() and press Enter.</span></span> <span data-ttu-id="8320e-261">Funkce jednoduše načte název výpočetním uzlu, na kterém běží UDF.</span><span class="sxs-lookup"><span data-stu-id="8320e-261">The function simply retrieves the name of the compute node on which the UDF runs.</span></span> <span data-ttu-id="8320e-262">Pro první spustit dialogové okno přihlašovací údaje vyzve k zadání uživatelského jména a hesla se připojit ke clusteru IaaS.</span><span class="sxs-lookup"><span data-stu-id="8320e-262">For the first run, a credentials dialog box prompts for the username and password to connect to the IaaS cluster.</span></span>
   
   ![Spustit UDF][run]
   
   <span data-ttu-id="8320e-264">Když dojde k výpočtu velké množství buněk, stiskněte klávesu Alt-Shift-Ctrl + F9 ke spuštění na všechny buňky výpočtu.</span><span class="sxs-lookup"><span data-stu-id="8320e-264">When there are many cells to calculate, press Alt-Shift-Ctrl + F9 to run the calculation on all cells.</span></span>

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a><span data-ttu-id="8320e-265">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="8320e-265">Step 3.</span></span> <span data-ttu-id="8320e-266">Spuštění úlohy architektury SOA z místního klienta</span><span class="sxs-lookup"><span data-stu-id="8320e-266">Run a SOA workload from an on-premises client</span></span>
<span data-ttu-id="8320e-267">Pokud chcete spustit obecné aplikace SOA v clusteru HPC Pack IaaS, nejprve pomocí jedné z metod v kroku 1 nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="8320e-267">To run general SOA applications on the HPC Pack IaaS cluster, first use one of the methods in Step 1 to deploy the cluster.</span></span> <span data-ttu-id="8320e-268">Zadejte obecný výpočetní uzel bitové kopie v tomto případě, protože není nutné aplikaci Excel na výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="8320e-268">Specify a generic compute node image in this case, because you do not need Excel on the compute nodes.</span></span> <span data-ttu-id="8320e-269">Potom postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="8320e-269">Then follow these steps.</span></span>

1. <span data-ttu-id="8320e-270">Po načtení certifikátu clusteru, importujte ho na klientském počítači v rámci Cert: \CurrentUser\Root.</span><span class="sxs-lookup"><span data-stu-id="8320e-270">After retrieving the cluster certificate, import it on the client computer under Cert:\CurrentUser\Root.</span></span>
2. <span data-ttu-id="8320e-271">Nainstalujte [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) a [HPC Pack 2012 R2 Update 3 klienta nástroje](https://www.microsoft.com/download/details.aspx?id=49923).</span><span class="sxs-lookup"><span data-stu-id="8320e-271">Install the [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) and [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923).</span></span> <span data-ttu-id="8320e-272">Tyto nástroje vám umožňují vývoj a spusťte SOA klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="8320e-272">These tools enable you to develop and run SOA client applications.</span></span>
3. <span data-ttu-id="8320e-273">Stažení HelloWorldR2 [ukázkový kód](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="8320e-273">Download the HelloWorldR2 [sample code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span> <span data-ttu-id="8320e-274">Otevřete HelloWorldR2.sln v sadě Visual Studio 2010 nebo 2012.</span><span class="sxs-lookup"><span data-stu-id="8320e-274">Open the HelloWorldR2.sln in Visual Studio 2010 or 2012.</span></span> <span data-ttu-id="8320e-275">(Tato ukázka není momentálně kompatibilní s novější verzí sady Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="8320e-275">(This sample is not currently compatible with more recent versions of Visual Studio.)</span></span>
4. <span data-ttu-id="8320e-276">Sestavení projektu EchoService nejdřív.</span><span class="sxs-lookup"><span data-stu-id="8320e-276">Build the EchoService project first.</span></span> <span data-ttu-id="8320e-277">Potom nasaďte službu do clusteru IaaS stejným způsobem, který můžete nasadit na místní cluster.</span><span class="sxs-lookup"><span data-stu-id="8320e-277">Then, deploy the service to the IaaS cluster in the same way you deploy to an on-premises cluster.</span></span> <span data-ttu-id="8320e-278">Podrobné pokyny najdete v části Readme.doc v HelloWordR2.</span><span class="sxs-lookup"><span data-stu-id="8320e-278">For detailed steps, see the Readme.doc in HelloWordR2.</span></span> <span data-ttu-id="8320e-279">Upravit a vytvořit HellWorldR2 a další projekty, jak je popsáno v následující části ke generování SOA klientské aplikace, které běží na clusteru služby Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="8320e-279">Modify and build the HellWorldR2 and other projects as described in the following section to generate the SOA client applications that run on an Azure IaaS cluster.</span></span>

### <a name="use-http-binding-with-azure-storage-queue"></a><span data-ttu-id="8320e-280">Vazba Http pomocí fronty Azure storage</span><span class="sxs-lookup"><span data-stu-id="8320e-280">Use Http binding with Azure storage queue</span></span>
<span data-ttu-id="8320e-281">Pro použití vazby Http s frontu úložiště Azure, proveďte několik změny ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="8320e-281">To use Http binding with an Azure storage queue, make a few changes to the sample code.</span></span>

* <span data-ttu-id="8320e-282">Aktualizujte název clusteru.</span><span class="sxs-lookup"><span data-stu-id="8320e-282">Update the cluster name.</span></span>
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* <span data-ttu-id="8320e-283">V případě potřeby použijte výchozí TransportScheme v SessionStartInfo nebo explicitně nastavit na Http.</span><span class="sxs-lookup"><span data-stu-id="8320e-283">Optionally, use the default TransportScheme in SessionStartInfo or explicitly set it to Http.</span></span>

```
    info.TransportScheme = TransportScheme.Http;
```

* <span data-ttu-id="8320e-284">Používejte výchozí vazbu pro BrokerClient.</span><span class="sxs-lookup"><span data-stu-id="8320e-284">Use default binding for the BrokerClient.</span></span>
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    <span data-ttu-id="8320e-285">Nebo nastavte explicitně pomocí basicHttpBinding.</span><span class="sxs-lookup"><span data-stu-id="8320e-285">Or set explicitly using the basicHttpBinding.</span></span>
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* <span data-ttu-id="8320e-286">Volitelně můžete nastavte na hodnotu true v SessionStartInfo příznak UseAzureQueue.</span><span class="sxs-lookup"><span data-stu-id="8320e-286">Optionally, set the UseAzureQueue flag to true in SessionStartInfo.</span></span> <span data-ttu-id="8320e-287">Pokud není nastavená, nastaví se hodnotu true ve výchozím nastavení, když se název clusteru má přípony domény Azure a TransportScheme, je Http.</span><span class="sxs-lookup"><span data-stu-id="8320e-287">If not set, it will be set to true by default when the cluster name has Azure domain suffixes and the TransportScheme is Http.</span></span>
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a><span data-ttu-id="8320e-288">Použít vazbu Http bez fronty Azure storage</span><span class="sxs-lookup"><span data-stu-id="8320e-288">Use Http binding without Azure storage queue</span></span>
<span data-ttu-id="8320e-289">Pokud chcete používat vazby Http bez frontu úložiště Azure, explicitně nastavte příznak UseAzureQueue na hodnotu false v SessionStartInfo.</span><span class="sxs-lookup"><span data-stu-id="8320e-289">To use Http binding without an Azure storage queue, explicitly set the UseAzureQueue flag to false in the SessionStartInfo.</span></span>

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a><span data-ttu-id="8320e-290">NetTcp vazbu používají</span><span class="sxs-lookup"><span data-stu-id="8320e-290">Use NetTcp binding</span></span>
<span data-ttu-id="8320e-291">Pokud chcete použít NetTcp vazby, konfigurace je podobný připojení k místní cluster.</span><span class="sxs-lookup"><span data-stu-id="8320e-291">To use NetTcp binding, the configuration is similar to connecting to an on-premises cluster.</span></span> <span data-ttu-id="8320e-292">Budete muset otevřít několik koncových bodů z hlavního uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8320e-292">You need to open a few endpoints on the head node VM.</span></span> <span data-ttu-id="8320e-293">Pokud skript nasazení HPC Pack IaaS jste použili k vytvoření clusteru, například nastavte koncové body na portálu Azure následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="8320e-293">If you used the HPC Pack IaaS deployment script to create the cluster, for example, set the endpoints in the Azure portal as follows.</span></span>

1. <span data-ttu-id="8320e-294">Zastavte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8320e-294">Stop the VM.</span></span>
2. <span data-ttu-id="8320e-295">Přidat porty TCP 9090, 9087, 9091, 9094 pro relaci, zprostředkovatel, respektive zprostředkovatel worker a datové služby</span><span class="sxs-lookup"><span data-stu-id="8320e-295">Add the TCP ports 9090, 9087, 9091, 9094 for the Session, Broker, Broker worker, and Data services, respectively</span></span>
   
    ![Konfigurace koncových bodů][endpoint-new-portal]
3. <span data-ttu-id="8320e-297">Spusťte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8320e-297">Start the VM.</span></span>

<span data-ttu-id="8320e-298">Klientská aplikace SOA nevyžaduje žádné změny kromě změna head název, který má úplný název clusteru IaaS.</span><span class="sxs-lookup"><span data-stu-id="8320e-298">The SOA client application requires no changes except altering the head name to the IaaS cluster full name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8320e-299">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8320e-299">Next steps</span></span>
* <span data-ttu-id="8320e-300">V tématu [tyto prostředky](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) pro další informace o spuštění úlohy aplikace Excel pomocí sady HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="8320e-300">See [these resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) for more information about running Excel workloads with HPC Pack.</span></span>
* <span data-ttu-id="8320e-301">V tématu [Správa služeb SOA v Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) Další informace o nasazení a správě SOA services pomocí sady HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="8320e-301">See [Managing SOA Services in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) for more about deploying and managing SOA services with HPC Pack.</span></span>

<!--Image references-->
[scenario]: ./media/excel-cluster-hpcpack/scenario.png
[github]: ./media/excel-cluster-hpcpack/github.png
[template]: ./media/excel-cluster-hpcpack/template.png
[parameters]: ./media/excel-cluster-hpcpack/parameters.png
[parameters-new-portal]: ./media/excel-cluster-hpcpack/parameters-new-portal.png
[create]: ./media/excel-cluster-hpcpack/create.png
[connect]: ./media/excel-cluster-hpcpack/connect.png
[cert]: ./media/excel-cluster-hpcpack/cert.png
[addin]: ./media/excel-cluster-hpcpack/addin.png
[macro]: ./media/excel-cluster-hpcpack/macro.png
[options]: ./media/excel-cluster-hpcpack/options.png
[run]: ./media/excel-cluster-hpcpack/run.png
[endpoint]: ./media/excel-cluster-hpcpack/endpoint.png
[endpoint-new-portal]: ./media/excel-cluster-hpcpack/endpoint-new-portal.png
[udf]: ./media/excel-cluster-hpcpack/udf.png
