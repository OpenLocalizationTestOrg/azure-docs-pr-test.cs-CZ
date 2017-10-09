---
title: aaaHPC Pack clusteru pro aplikaci Excel a SOA | Microsoft Docs
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
ms.openlocfilehash: 55b4b2c25fe65d06b75025cc23c3c13b8b764238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="65f14-103">Začít a spustit úlohy aplikace Excel a SOA v clusteru HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="65f14-103">Get started running Excel and SOA workloads on an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="65f14-104">Tento článek ukazuje, jak toodeploy Microsoft HPC Pack 2012 R2 clusteru na virtuálních počítačích Azure pomocí šablony Azure rychlý start, nebo volitelně skript nasazení Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="65f14-104">This article shows you how toodeploy a Microsoft HPC Pack 2012 R2 cluster on Azure virtual machines by using an Azure quickstart template, or optionally an Azure PowerShell deployment script.</span></span> <span data-ttu-id="65f14-105">Hello clusteru používá virtuální počítač Azure Marketplace bitové kopie určené toorun Microsoft Excel nebo orientované na služby architektura (SOA) úlohy s HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="65f14-105">hello cluster uses Azure Marketplace VM images designed toorun Microsoft Excel or service-oriented architecture (SOA) workloads with HPC Pack.</span></span> <span data-ttu-id="65f14-106">Můžete použít toorun hello clusteru HPC aplikace Excel a SOA služby z klientského počítače k místní.</span><span class="sxs-lookup"><span data-stu-id="65f14-106">You can use hello cluster toorun Excel HPC and SOA services from an on-premises client computer.</span></span> <span data-ttu-id="65f14-107">služby HPC pro Excel Hello zahrnují snižování zátěže sešitu aplikace Excel a uživatelem definované funkce aplikace Excel nebo UDF.</span><span class="sxs-lookup"><span data-stu-id="65f14-107">hello Excel HPC services include Excel workbook offloading and Excel user-defined functions, or UDFs.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="65f14-108">Tento článek vychází z funkce, šablony a skripty pro HPC Pack 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="65f14-108">This article is based on features, templates, and scripts for HPC Pack 2012 R2.</span></span> <span data-ttu-id="65f14-109">Tento scénář není aktuálně podporován v prostředí HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="65f14-109">This scenario is not currently supported in HPC Pack 2016.</span></span>
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="65f14-110">Na vysoké úrovni, hello následující diagram znázorňuje hello HPC Pack clusteru, že který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="65f14-110">At a high level, hello following diagram shows hello HPC Pack cluster you create.</span></span>

![Cluster HPC s uzly, které jsou spuštěné úlohy aplikace Excel][scenario]

## <a name="prerequisites"></a><span data-ttu-id="65f14-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="65f14-112">Prerequisites</span></span>
* <span data-ttu-id="65f14-113">**Klientský počítač** -je nutné klienta se systémem Windows počítače toosubmit ukázkové aplikace Excel a SOA úlohy toohello cluster.</span><span class="sxs-lookup"><span data-stu-id="65f14-113">**Client computer** - You need a Windows-based client computer toosubmit sample Excel and SOA jobs toohello cluster.</span></span> <span data-ttu-id="65f14-114">Musíte taky hello toorun počítače Windows Azure PowerShell skript nasazení clusteru (Pokud zvolíte tuto metodu nasazení).</span><span class="sxs-lookup"><span data-stu-id="65f14-114">You also need a Windows computer toorun hello Azure PowerShell cluster deployment script (if you choose that deployment method).</span></span>
* <span data-ttu-id="65f14-115">**Předplatné Azure** – Pokud nemáte předplatné Azure, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="65f14-115">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="65f14-116">**Kvóta jader** -může být nutné tooincrease hello kvóty jader, zvlášť pokud nasadíte několik uzlů clusteru s vícejádrovými velikosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="65f14-116">**Cores quota** - You might need tooincrease hello quota of cores, especially if you deploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="65f14-117">Pokud používáte šablonu Azure rychlý start, kvóta jader hello ve službě Správce prostředků je za oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="65f14-117">If you are using an Azure quickstart template, hello cores quota in Resource Manager is per Azure region.</span></span> <span data-ttu-id="65f14-118">V takovém případě můžete potřebovat tooincrease hello kvóty v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="65f14-118">In that case, you might need tooincrease hello quota in a specific region.</span></span> <span data-ttu-id="65f14-119">V tématu [limity předplatného Azure, kvóty a omezení](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="65f14-119">See [Azure subscription limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="65f14-120">tooincrease kvótu, [otevřete žádosti o podporu online zákazníka](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zdarma.</span><span class="sxs-lookup"><span data-stu-id="65f14-120">tooincrease a quota, [open an online customer support request](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) at no charge.</span></span>
* <span data-ttu-id="65f14-121">**Licenci aplikace Microsoft Office** – Pokud nasadíte výpočetní uzly bitovou kopii virtuálního počítače Marketplace HPC Pack 2012 R2 pomocí aplikace Microsoft Excel, 30denní zkušební verzi Microsoft Excelu Professional Plus 2013 je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="65f14-121">**Microsoft Office license** - If you deploy compute nodes using a Marketplace HPC Pack 2012 R2 VM image with Microsoft Excel, a 30-day evaluation version of Microsoft Excel Professional Plus 2013 is installed.</span></span> <span data-ttu-id="65f14-122">Po hello zkušební období budete potřebovat tooprovide platný Microsoft Office licence tooactivate Excel toocontinue toorun úlohy.</span><span class="sxs-lookup"><span data-stu-id="65f14-122">After hello evaluation period, you need tooprovide a valid Microsoft Office license tooactivate Excel toocontinue toorun workloads.</span></span> <span data-ttu-id="65f14-123">V tématu [aktivace v aplikaci Excel](#excel-activation) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="65f14-123">See [Excel activation](#excel-activation) later in this article.</span></span> 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="65f14-124">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="65f14-124">Step 1.</span></span> <span data-ttu-id="65f14-125">Nastavení clusteru služby HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="65f14-125">Set up an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="65f14-126">Ukážeme dvě možnosti tooset až clusteru HPC Pack 2012 R2 hello: první, použití šablony Azure rychlý start a hello portál Azure; a Zadruhé, pomocí skript nasazení Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="65f14-126">We show two options tooset up hello HPC Pack 2012 R2 cluster: first, using an Azure quickstart template and hello Azure portal; and second, using an Azure PowerShell deployment script.</span></span>

### <a name="option-1-use-a-quickstart-template"></a><span data-ttu-id="65f14-127">Možnost 1.</span><span class="sxs-lookup"><span data-stu-id="65f14-127">Option 1.</span></span> <span data-ttu-id="65f14-128">Použít šabloně pro rychlý start</span><span class="sxs-lookup"><span data-stu-id="65f14-128">Use a quickstart template</span></span>
<span data-ttu-id="65f14-129">Použití služby Azure rychlý start tooquickly šablony nasazení clusteru HPC Pack v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="65f14-129">Use an Azure quickstart template tooquickly deploy an HPC Pack cluster in hello Azure portal.</span></span> <span data-ttu-id="65f14-130">Když otevřete šablonu hello hello portálu, zobrazí jednoduchého uživatelského rozhraní, kde zadáte hello nastavení pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="65f14-130">When you open hello template in hello portal, you get a simple UI where you enter hello settings for your cluster.</span></span> <span data-ttu-id="65f14-131">Tady jsou kroky hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-131">Here are hello steps.</span></span> 

> [!TIP]
> <span data-ttu-id="65f14-132">Pokud chcete, použijte [šablony Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) vytvářející cluster podobné speciálně pro zatížení aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="65f14-132">If you want, use an [Azure Marketplace template](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) that creates a similar cluster specifically for Excel workloads.</span></span> <span data-ttu-id="65f14-133">kroky Hello mírně lišit od následující hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-133">hello steps differ slightly from hello following.</span></span>
> 
> 

1. <span data-ttu-id="65f14-134">Navštivte hello [stránku šablony vytvořit Cluster prostředí HPC na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span><span class="sxs-lookup"><span data-stu-id="65f14-134">Visit hello [Create HPC Cluster template page on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span></span> <span data-ttu-id="65f14-135">Pokud chcete, zkontrolujte informace o šabloně hello a hello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="65f14-135">If you want, review information about hello template and hello source code.</span></span>
2. <span data-ttu-id="65f14-136">Klikněte na tlačítko **nasazení tooAzure** toostart nasazení pomocí šablony hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="65f14-136">Click **Deploy tooAzure** toostart a deployment with hello template in hello Azure portal.</span></span>
   
   ![Nasazení šablony tooAzure][github]
3. <span data-ttu-id="65f14-138">Hello portálu postupujte podle těchto kroků tooenter hello parametrů šablony clusteru HPC hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-138">In hello portal, follow these steps tooenter hello parameters for hello HPC cluster template.</span></span>
   
   <span data-ttu-id="65f14-139">a.</span><span class="sxs-lookup"><span data-stu-id="65f14-139">a.</span></span> <span data-ttu-id="65f14-140">Na hello **parametry** stránky zadejte nebo upravte hodnoty pro parametry šablony hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-140">On hello **Parameters** page, enter or modify values for hello template parameters.</span></span> <span data-ttu-id="65f14-141">(Klikněte na tlačítko hello další tooeach nastavení Ikona pro informace nápovědy.) Ukázkové hodnoty jsou uvedené v následující obrazovce hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-141">(Click hello icon next tooeach setting for help information.) Sample values are shown in hello following screen.</span></span> <span data-ttu-id="65f14-142">Tento příklad vytvoří cluster s názvem *hpc01* v hello *hpc.local* domény, který se skládá z hlavního uzlu a 2 výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="65f14-142">This example creates a cluster named *hpc01* in hello *hpc.local* domain consisting of a head node and 2 compute nodes.</span></span> <span data-ttu-id="65f14-143">Hello výpočetní uzly jsou vytvořeny z virtuálních počítačů HPC Pack obrázku, který obsahuje aplikace Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="65f14-143">hello compute nodes are created from an HPC Pack VM image that includes Microsoft Excel.</span></span>
   
   ![Zadejte parametry][parameters-new-portal]
   
   > [!NOTE]
   > <span data-ttu-id="65f14-145">virtuální počítač je automaticky vytvořen z hello hlavního uzlu Hello [nejnovější image Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 na Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="65f14-145">hello head node VM is created automatically from hello [latest Marketplace image](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) of HPC Pack 2012 R2 on Windows Server 2012 R2.</span></span> <span data-ttu-id="65f14-146">Aktuálně hello image založena na HPC Pack 2012 R2 Update 3.</span><span class="sxs-lookup"><span data-stu-id="65f14-146">Currently hello image is based on HPC Pack 2012 R2 Update 3.</span></span>
   > 
   > <span data-ttu-id="65f14-147">Výpočetní uzel virtuální počítače jsou vytvořeny z nejnovější bitové kopie hello hello vybrané výpočetní uzel rodiny.</span><span class="sxs-lookup"><span data-stu-id="65f14-147">Compute node VMs are created from hello latest image of hello selected compute node family.</span></span> <span data-ttu-id="65f14-148">Vyberte hello **ComputeNodeWithExcel** možnost hello nejnovější HPC Pack výpočetního uzlu bitové kopie obsahující zkušební verzi Microsoft Excelu Professional Plus 2013.</span><span class="sxs-lookup"><span data-stu-id="65f14-148">Select hello **ComputeNodeWithExcel** option for hello latest HPC Pack compute node image that includes an evaluation version of Microsoft Excel Professional Plus 2013.</span></span> <span data-ttu-id="65f14-149">toodeploy clusteru s podporou pro obecné SOA relací nebo snižování zátěže systému souborů UDF Excelu, zvolte hello **ComputeNode** možnost (bez nainstalované aplikace Excel).</span><span class="sxs-lookup"><span data-stu-id="65f14-149">toodeploy a cluster for general SOA sessions or for Excel UDF offloading, choose hello **ComputeNode** option (without Excel installed).</span></span>
   > 
   > 
   
   <span data-ttu-id="65f14-150">b.</span><span class="sxs-lookup"><span data-stu-id="65f14-150">b.</span></span> <span data-ttu-id="65f14-151">Zvolte předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-151">Choose hello subscription.</span></span>
   
   <span data-ttu-id="65f14-152">c.</span><span class="sxs-lookup"><span data-stu-id="65f14-152">c.</span></span> <span data-ttu-id="65f14-153">Vytvořte skupinu prostředků clusteru hello, jako například *hpc01RG*.</span><span class="sxs-lookup"><span data-stu-id="65f14-153">Create a resource group for hello cluster, such as *hpc01RG*.</span></span>
   
   <span data-ttu-id="65f14-154">d.</span><span class="sxs-lookup"><span data-stu-id="65f14-154">d.</span></span> <span data-ttu-id="65f14-155">Vyberte umístění pro skupinu prostředků hello, jako je například střed USA.</span><span class="sxs-lookup"><span data-stu-id="65f14-155">Choose a location for hello resource group, such as Central US.</span></span>
   
   <span data-ttu-id="65f14-156">e.</span><span class="sxs-lookup"><span data-stu-id="65f14-156">e.</span></span> <span data-ttu-id="65f14-157">Na hello **právní podmínky** stránky, přečtěte si podmínky hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-157">On hello **Legal terms** page, review hello terms.</span></span> <span data-ttu-id="65f14-158">Pokud souhlasíte, klikněte na tlačítko **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="65f14-158">If you agree, click **Purchase**.</span></span> <span data-ttu-id="65f14-159">Potom klikněte na po skončení nastavení hello hodnoty pro šablonu hello **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="65f14-159">Then, when you are finished setting hello values for hello template, click **Create**.</span></span>
4. <span data-ttu-id="65f14-160">Po dokončení nasazení hello (obvykle trvá přibližně 30 minut), exportovat soubor certifikátu hello clusteru z hlavního uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-160">When hello deployment completes (it typically takes around 30 minutes), export hello cluster certificate file from hello cluster head node.</span></span> <span data-ttu-id="65f14-161">V pozdější fázi importujte tento veřejný certifikát hello ověřování klientského počítače tooprovide hello serverové pro zabezpečené vazby HTTP.</span><span class="sxs-lookup"><span data-stu-id="65f14-161">In a later step, you import this public certificate on hello client computer tooprovide hello server-side authentication for secure HTTP binding.</span></span>
   
   <span data-ttu-id="65f14-162">a.</span><span class="sxs-lookup"><span data-stu-id="65f14-162">a.</span></span> <span data-ttu-id="65f14-163">V hello portálu Azure, přejděte toohello řídicí panel, vyberte hello hlavního uzlu a klikněte na tlačítko **Connect** hello horní části tooconnect stránku hello pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="65f14-163">In hello Azure portal, go toohello dashboard, select hello head node, and click **Connect** at hello top of hello page tooconnect using Remote Desktop.</span></span>
   
    <!-- ![Connect toohello head node][connect] -->
   
   <span data-ttu-id="65f14-164">b.</span><span class="sxs-lookup"><span data-stu-id="65f14-164">b.</span></span> <span data-ttu-id="65f14-165">Použijte standardní postupy v certifikátu hlavního uzlu hello tooexport správce certifikátů (nachází se v části Cert: \LocalMachine\My) bez hello soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="65f14-165">Use standard procedures in Certificate Manager tooexport hello head node certificate (located under Cert:\LocalMachine\My) without hello private key.</span></span> <span data-ttu-id="65f14-166">V tomto příkladu exportovat *CN = hpc01.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="65f14-166">In this example, export *CN = hpc01.eastus.cloudapp.azure.com*.</span></span>
   
   ![Export certifikátu hello][cert]

### <a name="option-2-use-hello-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="65f14-168">Možnost 2.</span><span class="sxs-lookup"><span data-stu-id="65f14-168">Option 2.</span></span> <span data-ttu-id="65f14-169">Použít skript nasazení IaaS HPC Pack hello</span><span class="sxs-lookup"><span data-stu-id="65f14-169">Use hello HPC Pack IaaS Deployment script</span></span>
<span data-ttu-id="65f14-170">Hello skript nasazení HPC Pack IaaS poskytuje jiný způsob univerzální toodeploy clusteru služby HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="65f14-170">hello HPC Pack IaaS deployment script provides another versatile way toodeploy an HPC Pack cluster.</span></span> <span data-ttu-id="65f14-171">Vytvoří cluster v modelu nasazení classic hello, zatímco hello šablona používá hello modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="65f14-171">It creates a cluster in hello classic deployment model, whereas hello template uses hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="65f14-172">Skript hello je také kompatibilní s předplatným Azure globální hello nebo služby Azure China.</span><span class="sxs-lookup"><span data-stu-id="65f14-172">Also, hello script is compatible with a subscription in either hello Azure Global or Azure China service.</span></span>

<span data-ttu-id="65f14-173">**Další požadavky**</span><span class="sxs-lookup"><span data-stu-id="65f14-173">**Additional prerequisites**</span></span>

* <span data-ttu-id="65f14-174">**Prostředí Azure PowerShell** - [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) (verze 0.8.10 nebo novější) na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="65f14-174">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="65f14-175">**Skript nasazení HPC Pack IaaS** – stáhněte a rozbalte hello nejnovější verzi hello skript z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="65f14-175">**HPC Pack IaaS deployment script** - Download and unpack hello latest version of hello script from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="65f14-176">Zkontrolujte verzi hello hello skriptu spuštěním `New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="65f14-176">Check hello version of hello script by running `New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="65f14-177">Tento článek je založen na verzi 4.5.0 nebo později hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="65f14-177">This article is based on version 4.5.0 or later of hello script.</span></span>

<span data-ttu-id="65f14-178">**Vytvoření konfiguračního souboru hello**</span><span class="sxs-lookup"><span data-stu-id="65f14-178">**Create hello configuration file**</span></span>

 <span data-ttu-id="65f14-179">Hello skript nasazení HPC Pack IaaS používá jako vstup, který popisuje hello infrastruktura clusteru HPC hello konfigurační soubor XML.</span><span class="sxs-lookup"><span data-stu-id="65f14-179">hello HPC Pack IaaS deployment script uses an XML configuration file as input that describes hello infrastructure of hello HPC cluster.</span></span> <span data-ttu-id="65f14-180">toodeploy cluster, který se skládá z hlavního uzlu a 18 výpočetních uzlů vytvořené z hello výpočetní uzel image, která obsahuje aplikaci Microsoft Excel, nahraďte hodnoty pro vaše prostředí do hello následující vzorový konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="65f14-180">toodeploy a cluster consisting of a head node and 18 compute nodes created from hello compute node image that includes Microsoft Excel, substitute values for your environment into hello following sample configuration file.</span></span> <span data-ttu-id="65f14-181">Další informace o hello konfigurační soubor najdete v tématu hello Manual.rtf souboru ve složce hello skriptu a [vytvoření clusteru prostředí HPC s hello skript nasazení HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="65f14-181">For more information about hello configuration file, see hello Manual.rtf file in hello script folder and [Create an HPC cluster with hello HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

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

<span data-ttu-id="65f14-182">**Poznámky k hello konfiguračního souboru**</span><span class="sxs-lookup"><span data-stu-id="65f14-182">**Notes about hello configuration file**</span></span>

* <span data-ttu-id="65f14-183">Hello **VMName** hlavního uzlu hello **musí** být hello stejné jako hello **ServiceName**, nebo úlohy architektury SOA selže toorun.</span><span class="sxs-lookup"><span data-stu-id="65f14-183">hello **VMName** of hello head node **MUST** be hello same as hello **ServiceName**, or SOA jobs fail toorun.</span></span>
* <span data-ttu-id="65f14-184">Je nutné zadat **EnableWebPortal** tak, aby hello hlavního uzlu certifikátu je generována a exportovat.</span><span class="sxs-lookup"><span data-stu-id="65f14-184">Make sure you specify **EnableWebPortal** so that hello head node certificate is generated and exported.</span></span>
* <span data-ttu-id="65f14-185">soubor Hello Určuje skript prostředí PowerShell po konfiguraci PostConfig.ps1, která běží na hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="65f14-185">hello file specifies a post-configuration PowerShell script PostConfig.ps1 that runs on hello head node.</span></span> <span data-ttu-id="65f14-186">Následující ukázkový skript Hello nakonfiguruje hello úložiště Azure připojovací řetězec, odebere hello výpočetní uzel roli z hlavního uzlu hello a přináší všechny uzly online při jejich nasazení.</span><span class="sxs-lookup"><span data-stu-id="65f14-186">hello following sample script configures hello Azure storage connection string, removes hello compute node role from hello head node, and brings all nodes online when they are deployed.</span></span> 

```
    # add hello HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set hello Azure storage connection string for hello cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove hello compute node role for head node toomake sure hello Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in hello deployment including hello head node and compute nodes, which should match hello number specified in hello XML configuration file
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

<span data-ttu-id="65f14-187">**Spusťte skript hello**</span><span class="sxs-lookup"><span data-stu-id="65f14-187">**Run hello script**</span></span>

1. <span data-ttu-id="65f14-188">Otevřete konzolu prostředí PowerShell hello na klientském počítači hello jako správce.</span><span class="sxs-lookup"><span data-stu-id="65f14-188">Open hello PowerShell console on hello client computer as an administrator.</span></span>
2. <span data-ttu-id="65f14-189">Změnit složku toohello skriptu (E:\IaaSClusterScript v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="65f14-189">Change directory toohello script folder (E:\IaaSClusterScript in this example).</span></span>
   
   ```
   cd E:\IaaSClusterScript
   ```
3. <span data-ttu-id="65f14-190">toodeploy hello HPC Pack clusteru, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-190">toodeploy hello HPC Pack cluster, run hello following command.</span></span> <span data-ttu-id="65f14-191">Tento příklad předpokládá, že hello konfigurační soubor se nachází v E:\HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="65f14-191">This example assumes that hello configuration file is located in E:\HPCDemoConfig.xml.</span></span>
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

<span data-ttu-id="65f14-192">Hello HPC Pack nasazovací skript spustí nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="65f14-192">hello HPC Pack deployment script runs for some time.</span></span> <span data-ttu-id="65f14-193">Jeden skript hello věc je tooexport a stáhněte certifikát hello clusteru a uložit ho hello aktuální složku Dokumenty uživatele na klientském počítači hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-193">One thing hello script does is tooexport and download hello cluster certificate and save it in hello current user’s Documents folder on hello client computer.</span></span> <span data-ttu-id="65f14-194">Hello skript generuje následující toohello podobné zprávy.</span><span class="sxs-lookup"><span data-stu-id="65f14-194">hello script generates a message similar toohello following.</span></span> <span data-ttu-id="65f14-195">V tomto kroku importujete hello certifikátu v úložišti hello příslušný certifikát.</span><span class="sxs-lookup"><span data-stu-id="65f14-195">In a following step, you import hello certificate in hello appropriate certificate store.</span></span>    

    You have enabled REST API or web portal on HPC Pack head node. Please import hello following certificate in hello Trusted Root Certification Authorities certificate store on hello computer where you are submitting job or accessing hello HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a><span data-ttu-id="65f14-196">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="65f14-196">Step 2.</span></span> <span data-ttu-id="65f14-197">Snižování zátěže sešitů aplikace Excel a spusťte UDF z místního klienta</span><span class="sxs-lookup"><span data-stu-id="65f14-197">Offload Excel workbooks and run UDFs from an on-premises client</span></span>
### <a name="excel-activation"></a><span data-ttu-id="65f14-198">Aktivace aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="65f14-198">Excel activation</span></span>
<span data-ttu-id="65f14-199">Při použití hello image virtuálního počítače ComputeNodeWithExcel pro úlohy v produkčním prostředí, je nutné tooprovide platný Microsoft Office licenční klíče tooactivate aplikace Excel na výpočetních uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-199">When using hello ComputeNodeWithExcel VM image for production workloads, you need tooprovide a valid Microsoft Office license key tooactivate Excel on hello compute nodes.</span></span> <span data-ttu-id="65f14-200">Jinak hello zkušební verzi aplikace Excel vyprší po 30 dnů a systémem sešitů aplikace Excel se nezdaří s hello COMException (0x800AC472).</span><span class="sxs-lookup"><span data-stu-id="65f14-200">Otherwise, hello evaluation version of Excel expires after 30 days, and running Excel workbooks will fail with hello COMException (0x800AC472).</span></span> 

<span data-ttu-id="65f14-201">Obnovení aplikace Excel můžete aktivačního období pro jiné 30 dnů od doby vyhodnocení: Přihlaste toohello hlavního uzlu a clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` na všechny aplikace Excel výpočetní uzly prostřednictvím Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="65f14-201">You can rearm Excel for another 30 days of evaluation time: Log on toohello head node and clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` on all Excel compute nodes via HPC Cluster Manager.</span></span> <span data-ttu-id="65f14-202">Po obnovení aktivačního období maximálně dvakrát.</span><span class="sxs-lookup"><span data-stu-id="65f14-202">You can rearm a maximum of two times.</span></span> <span data-ttu-id="65f14-203">Potom je nutné zadat platný klíč licence Office.</span><span class="sxs-lookup"><span data-stu-id="65f14-203">After that, you must provide a valid Office license key.</span></span>

<span data-ttu-id="65f14-204">Hello Office Professional Plus 2013 nainstalovaný na hello image virtuálního počítače je multilicenční edici s obecné svazku licenční klíč (kód GVLK).</span><span class="sxs-lookup"><span data-stu-id="65f14-204">hello Office Professional Plus 2013 installed on hello VM image is a volume edition with a Generic Volume License Key (GVLK).</span></span> <span data-ttu-id="65f14-205">Aktivujte ji prostřednictvím služby správy klíčů (KMS) nebo aktivaci prostřednictvím služby (AD-BA) nebo klíč k vícenásobné aktivaci (MAK).</span><span class="sxs-lookup"><span data-stu-id="65f14-205">You can activate it via Key Management Service (KMS)/Active Directory-Based Activation (AD-BA) or Multiple Activation Key (MAK).</span></span> 

    * <span data-ttu-id="65f14-206">toouse služby správy KLÍČŮ/AD-BA použít existující server služby správy KLÍČŮ nebo nastavte novou pomocí hello Microsoft Office 2013 svazku balíků.</span><span class="sxs-lookup"><span data-stu-id="65f14-206">toouse KMS/AD-BA, use an existing KMS server or set up a new one by using hello Microsoft Office 2013 Volume License Pack.</span></span> <span data-ttu-id="65f14-207">(Pokud chcete, nastavte server hello hello hlavního uzlu.) Potom aktivujte klíč hostitele služby správy KLÍČŮ hello prostřednictvím hello Internet nebo telefon.</span><span class="sxs-lookup"><span data-stu-id="65f14-207">(If you want to, set up hello server on hello head node.) Then, activate hello KMS host key via hello Internet or telephone.</span></span> <span data-ttu-id="65f14-208">Potom clusrun `ospp.vbs` tooset hello serveru služby správy KLÍČŮ a portu a aktivovat Office na všechny hello Excel výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="65f14-208">Then clusrun `ospp.vbs` tooset hello KMS server and port and activate Office on all hello Excel compute nodes.</span></span> 

    * <span data-ttu-id="65f14-209">toouse klíč k vícenásobné aktivaci, první clusrun `ospp.vbs` tooinput hello klíč a poté znovu aktivovat všechny výpočetní uzly hello Excel prostřednictvím hello Internet nebo telefon.</span><span class="sxs-lookup"><span data-stu-id="65f14-209">toouse MAK, first clusrun `ospp.vbs` tooinput hello key and then activate all hello Excel compute nodes via hello Internet or telephone.</span></span> 

> [!NOTE]
> <span data-ttu-id="65f14-210">Prodejní kódy product key pro Office Professional Plus 2013 nelze použít s touto bitovou kopií virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="65f14-210">Retail product keys for Office Professional Plus 2013 cannot be used with this VM image.</span></span> <span data-ttu-id="65f14-211">Pokud máte platné klíče a instalační médium pro edice Office nebo aplikace Excel než tato edice Office Professional Plus 2013 svazku, můžete je používat místo.</span><span class="sxs-lookup"><span data-stu-id="65f14-211">If you have valid keys and installation media for Office or Excel editions other than this Office Professional Plus 2013 volume edition, you can use them instead.</span></span> <span data-ttu-id="65f14-212">Nejprve odinstalujte tento multilicenční edici a nainstalujte hello edici, která máte.</span><span class="sxs-lookup"><span data-stu-id="65f14-212">First uninstall this volume edition and install hello edition that you have.</span></span> <span data-ttu-id="65f14-213">Hello přeinstalovat Excel výpočetním uzlu se dají zachytit jako vlastní toouse bitové kopie virtuálních počítačů v nasazení ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="65f14-213">hello reinstalled Excel compute node can be captured as a customized VM image toouse in a deployment at scale.</span></span>
> 
> 

### <a name="offload-excel-workbooks"></a><span data-ttu-id="65f14-214">Snižování zátěže sešitů aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="65f14-214">Offload Excel workbooks</span></span>
<span data-ttu-id="65f14-215">Postupujte podle těchto kroků toooffload sešitu aplikace Excel, tak, aby běžel v clusteru HPC Pack hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="65f14-215">Follow these steps toooffload an Excel workbook so that it runs on hello HPC Pack cluster in Azure.</span></span> <span data-ttu-id="65f14-216">toodo, musí být v aplikaci Excel 2010 nebo 2013 už nainstalovaná na klientském počítači hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-216">toodo this, you must have Excel 2010 or 2013 already installed on hello client computer.</span></span>

1. <span data-ttu-id="65f14-217">Použijte jednu z možností hello v kroku 1 toodeploy clusteru služby HPC Pack s hello Excel výpočetní uzel image.</span><span class="sxs-lookup"><span data-stu-id="65f14-217">Use one of hello options in Step 1 toodeploy an HPC Pack cluster with hello Excel compute node image.</span></span> <span data-ttu-id="65f14-218">Získáte hello clusteru soubor certifikátu (.cer) a cluster uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="65f14-218">Obtain hello cluster certificate file (.cer) and cluster username and password.</span></span>
2. <span data-ttu-id="65f14-219">Na klientském počítači hello importujte certifikát hello clusteru pod Cert: \CurrentUser\Root.</span><span class="sxs-lookup"><span data-stu-id="65f14-219">On hello client computer, import hello cluster certificate under Cert:\CurrentUser\Root.</span></span>
3. <span data-ttu-id="65f14-220">Zkontrolujte, zda že je nainstalována aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="65f14-220">Make sure Excel is installed.</span></span> <span data-ttu-id="65f14-221">Vytvořte soubor Excel.exe.config s hello následující obsah v hello stejné složce jako Excel.exe na klientském počítači hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-221">Create an Excel.exe.config file with hello following contents in hello same folder as Excel.exe on hello client computer.</span></span> <span data-ttu-id="65f14-222">Tento krok zajistí, že tento hello doplněk HPC Pack 2012 R2 Excel COM úspěšně načten.</span><span class="sxs-lookup"><span data-stu-id="65f14-222">This step ensures that hello HPC Pack 2012 R2 Excel COM add-in loads successfully.</span></span>
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. <span data-ttu-id="65f14-223">Nastavení clusteru HPC Pack toohello toosubmit úlohy aplikace hello klienta.</span><span class="sxs-lookup"><span data-stu-id="65f14-223">Set up hello client toosubmit jobs toohello HPC Pack cluster.</span></span> <span data-ttu-id="65f14-224">Jednou z možností je toodownload hello úplné [HPC Pack 2012 R2 Update 3 instalace](http://www.microsoft.com/download/details.aspx?id=49922) a instalace klienta na HPC Pack hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-224">One option is toodownload hello full [HPC Pack 2012 R2 Update 3 installation](http://www.microsoft.com/download/details.aspx?id=49922) and install hello HPC Pack client.</span></span> <span data-ttu-id="65f14-225">Můžete taky stáhnout a nainstalovat hello [HPC Pack 2012 R2 Update 3 klienta nástroje](https://www.microsoft.com/download/details.aspx?id=49923) a hello odpovídající Visual C++ 2010 redistributable pro tento počítač ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).</span><span class="sxs-lookup"><span data-stu-id="65f14-225">Alternatively, download and install hello [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923) and hello appropriate Visual C++ 2010 redistributable for your computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).</span></span>
5. <span data-ttu-id="65f14-226">V tomto příkladu používáme Ukázka sešitu aplikace Excel s názvem ConvertiblePricing_Complete.xlsb.</span><span class="sxs-lookup"><span data-stu-id="65f14-226">In this example, we use a sample Excel workbook named ConvertiblePricing_Complete.xlsb.</span></span> <span data-ttu-id="65f14-227">Můžete ho stáhnout [zde](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span><span class="sxs-lookup"><span data-stu-id="65f14-227">You can download it [here](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span></span>
6. <span data-ttu-id="65f14-228">Zkopírujte hello pracovní složky tooa sešitu aplikace Excel například D:\Excel\Run.</span><span class="sxs-lookup"><span data-stu-id="65f14-228">Copy hello Excel workbook tooa working folder such as D:\Excel\Run.</span></span>
7. <span data-ttu-id="65f14-229">Otevřete sešit aplikace Excel hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-229">Open hello Excel workbook.</span></span> <span data-ttu-id="65f14-230">Na hello **vývoj** pásu karet, klikněte na tlačítko **COM Doplňky** a potvrďte, že hello doplňku HPC Pack Excel COM byla úspěšně zavedena.</span><span class="sxs-lookup"><span data-stu-id="65f14-230">On hello **Develop** ribbon, click **COM Add-Ins** and confirm that hello HPC Pack Excel COM add-in is loaded successfully.</span></span>
   
   ![Add-in pro prostředí HPC Pack v aplikaci Excel][addin]
8. <span data-ttu-id="65f14-232">Makra VBA hello HPCControlMacros v aplikaci Excel upravit tak, že změníte hello komentář řádky, jak je znázorněno v následující skript hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-232">Edit hello VBA macro HPCControlMacros in Excel by changing hello commented lines as shown in hello following script.</span></span> <span data-ttu-id="65f14-233">Nahraďte příslušnými hodnotami pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="65f14-233">Substitute appropriate values for your environment.</span></span>
   
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
9. <span data-ttu-id="65f14-235">Zkopírujte hello nahrávání adresáře tooan sešitu aplikace Excel například D:\Excel\Upload.</span><span class="sxs-lookup"><span data-stu-id="65f14-235">Copy hello Excel workbook tooan upload directory such as D:\Excel\Upload.</span></span> <span data-ttu-id="65f14-236">Tento adresář je určen v konstanta HPC_DependsFiles hello v makro VBA hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-236">This directory is specified in hello HPC_DependsFiles constant in hello VBA macro.</span></span>
10. <span data-ttu-id="65f14-237">toorun hello sešitu na clusteru hello v Azure, klikněte na tlačítko hello **clusteru** tlačítko na listu hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-237">toorun hello workbook on hello cluster in Azure, click hello **Cluster** button on hello worksheet.</span></span>

### <a name="run-excel-udfs"></a><span data-ttu-id="65f14-238">Spusťte systém souborů UDF Excelu</span><span class="sxs-lookup"><span data-stu-id="65f14-238">Run Excel UDFs</span></span>
<span data-ttu-id="65f14-239">toorun systém souborů UDF Excelu, postupujte podle předchozích kroků hello tooset 1 – 3 až hello klientský počítač.</span><span class="sxs-lookup"><span data-stu-id="65f14-239">toorun Excel UDFs, follow hello preceding steps 1 – 3 tooset up hello client computer.</span></span> <span data-ttu-id="65f14-240">Pro systém souborů UDF Excelu nepotřebujete toohave hello Excel aplikace nainstalované na výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="65f14-240">For Excel UDFs, you don't need toohave hello Excel application installed on compute nodes.</span></span> <span data-ttu-id="65f14-241">Tím, že při vytváření clusteru výpočetních uzlů, může zvolit bitovou kopii normální výpočetní uzel místo hello výpočetní uzel image pomocí aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="65f14-241">So, when creating your cluster compute nodes, you could choose a normal compute node image instead of hello compute node image with Excel.</span></span>

> [!NOTE]
> <span data-ttu-id="65f14-242">Existuje limit 34 znak v hello Excel 2010 a 2013 dialogové okno konektor clusteru.</span><span class="sxs-lookup"><span data-stu-id="65f14-242">There is a 34 character limit in hello Excel 2010 and 2013 cluster connector dialog box.</span></span> <span data-ttu-id="65f14-243">Můžete použít toto dialogové okno pole toospecify hello clusteru, který spouští hello UDF.</span><span class="sxs-lookup"><span data-stu-id="65f14-243">You use this dialog box toospecify hello cluster that runs hello UDFs.</span></span> <span data-ttu-id="65f14-244">Pokud je název clusteru úplné hello delší (například hpcexcelhn01.southeastasia.cloudapp.azure.com), v dialogovém okně hello nevejde.</span><span class="sxs-lookup"><span data-stu-id="65f14-244">If hello full cluster name is longer (for example, hpcexcelhn01.southeastasia.cloudapp.azure.com), it does not fit in hello dialog box.</span></span> <span data-ttu-id="65f14-245">Hello alternativní řešení je tooset celého systému proměnná, jako třeba *CCP_IAASHN* s hodnotou hello název hello dlouho clusteru.</span><span class="sxs-lookup"><span data-stu-id="65f14-245">hello workaround is tooset a machine-wide variable such as *CCP_IAASHN* with hello value of hello long cluster name.</span></span> <span data-ttu-id="65f14-246">Potom zadejte *CCP_IAASHN %* v dialogovém okně hello jako název hlavního uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-246">Then, enter *%CCP_IAASHN%* in hello dialog box as hello cluster head node name.</span></span> 
> 
> 

<span data-ttu-id="65f14-247">Po úspěšném nasazení clusteru hello, pokračujte hello následující kroky toorun integrované ukázka UDF Excelu.</span><span class="sxs-lookup"><span data-stu-id="65f14-247">After hello cluster is successfully deployed, continue with hello following steps toorun a sample built-in Excel UDF.</span></span> <span data-ttu-id="65f14-248">Vlastní systém souborů UDF Excelu, najdete v těchto [prostředky](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) toobuild hello XLL a nasadit je na hello IaaS clusteru.</span><span class="sxs-lookup"><span data-stu-id="65f14-248">For customized Excel UDFs, see these [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) toobuild hello XLLs and deploy them on hello IaaS cluster.</span></span>

1. <span data-ttu-id="65f14-249">Otevřete nový sešit aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="65f14-249">Open a new Excel workbook.</span></span> <span data-ttu-id="65f14-250">Na hello **vývoj** pásu karet, klikněte na tlačítko **doplňky**. Klikněte v dialogovém okně hello, **Procházet**, přejděte toohello %CCP_HOME%Bin\XLL32 složky a vyberte hello ukázka ClusterUDF32.xll.</span><span class="sxs-lookup"><span data-stu-id="65f14-250">On hello **Develop** ribbon, click **Add-Ins**. Then, in hello dialog box, click **Browse**, navigate toohello %CCP_HOME%Bin\XLL32 folder, and select hello sample ClusterUDF32.xll.</span></span> <span data-ttu-id="65f14-251">Pokud hello ClusterUDF32 neexistuje na hello klientský počítač, zkopírujte jej ze složky %CCP_HOME%Bin\XLL32 hello hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="65f14-251">If hello ClusterUDF32 doesn't exist on hello client machine, copy it from hello %CCP_HOME%Bin\XLL32 folder on hello head node.</span></span>
   
   ![Vyberte hello UDF][udf]
2. <span data-ttu-id="65f14-253">Klikněte na tlačítko **soubor** > **možnosti** > **rozšířené**.</span><span class="sxs-lookup"><span data-stu-id="65f14-253">Click **File** > **Options** > **Advanced**.</span></span> <span data-ttu-id="65f14-254">V části **vzorce**, zkontrolujte **povolit výpočetním clusteru, na uživatelem definované funkce toorun XLL**.</span><span class="sxs-lookup"><span data-stu-id="65f14-254">Under **Formulas**, check **Allow user-defined XLL functions toorun a compute cluster**.</span></span> <span data-ttu-id="65f14-255">Pak klikněte na tlačítko **možnosti** a zadejte název clusteru úplné hello v **název hlavního uzlu clusteru**.</span><span class="sxs-lookup"><span data-stu-id="65f14-255">Then click **Options** and enter hello full cluster name in **Cluster head node name**.</span></span> <span data-ttu-id="65f14-256">(Jak je uvedeno dříve toto vstupní pole je omezená too34 znaků, tak, aby název dlouho clusteru nemusí vešly.</span><span class="sxs-lookup"><span data-stu-id="65f14-256">(As noted previously this input box is limited too34 characters, so a long cluster name may not fit.</span></span> <span data-ttu-id="65f14-257">Proměnnou celého systému může použít pro název dlouho clusteru.)</span><span class="sxs-lookup"><span data-stu-id="65f14-257">You may use a machine-wide variable here for a long cluster name.)</span></span>
   
   ![Konfigurace hello UDF][options]
3. <span data-ttu-id="65f14-259">toorun hello výpočtu UDF v clusteru hello, klikněte na tlačítko hello buňky s hodnotou =XllGetComputerNameC() a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="65f14-259">toorun hello UDF calculation on hello cluster, click hello cell with value =XllGetComputerNameC() and press Enter.</span></span> <span data-ttu-id="65f14-260">Funkce Hello jednoduše načte název hello hello výpočetním uzlu, na které hello UDF běží.</span><span class="sxs-lookup"><span data-stu-id="65f14-260">hello function simply retrieves hello name of hello compute node on which hello UDF runs.</span></span> <span data-ttu-id="65f14-261">Pro hello prvním spuštění dialogové okno přihlašovací údaje vyzve k zadání hello uživatelské jméno a heslo tooconnect toohello IaaS clusteru.</span><span class="sxs-lookup"><span data-stu-id="65f14-261">For hello first run, a credentials dialog box prompts for hello username and password tooconnect toohello IaaS cluster.</span></span>
   
   ![Spustit UDF][run]
   
   <span data-ttu-id="65f14-263">Po mnoho toocalculate buněk se stisknutím Alt-Shift-Ctrl + F9 toorun hello výpočet na všechny buňky.</span><span class="sxs-lookup"><span data-stu-id="65f14-263">When there are many cells toocalculate, press Alt-Shift-Ctrl + F9 toorun hello calculation on all cells.</span></span>

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a><span data-ttu-id="65f14-264">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="65f14-264">Step 3.</span></span> <span data-ttu-id="65f14-265">Spuštění úlohy architektury SOA z místního klienta</span><span class="sxs-lookup"><span data-stu-id="65f14-265">Run a SOA workload from an on-premises client</span></span>
<span data-ttu-id="65f14-266">toorun obecné aplikace SOA v clusteru HPC Pack IaaS hello, nejprve použijte jednu z metod hello v kroku 1 toodeploy hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="65f14-266">toorun general SOA applications on hello HPC Pack IaaS cluster, first use one of hello methods in Step 1 toodeploy hello cluster.</span></span> <span data-ttu-id="65f14-267">Zadejte v tomto případě bitovou kopii obecné výpočetního uzlu, protože není nutné aplikaci Excel na výpočetních uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-267">Specify a generic compute node image in this case, because you do not need Excel on hello compute nodes.</span></span> <span data-ttu-id="65f14-268">Potom postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="65f14-268">Then follow these steps.</span></span>

1. <span data-ttu-id="65f14-269">Po načtení certifikátu clusteru hello, importujte ho na klientském počítači hello pod Cert: \CurrentUser\Root.</span><span class="sxs-lookup"><span data-stu-id="65f14-269">After retrieving hello cluster certificate, import it on hello client computer under Cert:\CurrentUser\Root.</span></span>
2. <span data-ttu-id="65f14-270">Nainstalujte hello [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) a [HPC Pack 2012 R2 Update 3 klienta nástroje](https://www.microsoft.com/download/details.aspx?id=49923).</span><span class="sxs-lookup"><span data-stu-id="65f14-270">Install hello [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) and [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923).</span></span> <span data-ttu-id="65f14-271">Tyto nástroje umožňují toodevelop a spusťte SOA klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="65f14-271">These tools enable you toodevelop and run SOA client applications.</span></span>
3. <span data-ttu-id="65f14-272">Stáhnout hello HelloWorldR2 [ukázkový kód](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="65f14-272">Download hello HelloWorldR2 [sample code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span> <span data-ttu-id="65f14-273">Otevřete hello HelloWorldR2.sln v sadě Visual Studio 2010 nebo 2012.</span><span class="sxs-lookup"><span data-stu-id="65f14-273">Open hello HelloWorldR2.sln in Visual Studio 2010 or 2012.</span></span> <span data-ttu-id="65f14-274">(Tato ukázka není momentálně kompatibilní s novější verzí sady Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="65f14-274">(This sample is not currently compatible with more recent versions of Visual Studio.)</span></span>
4. <span data-ttu-id="65f14-275">Nejprve sestavení projektu EchoService hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-275">Build hello EchoService project first.</span></span> <span data-ttu-id="65f14-276">Potom nasaďte hello služby toohello IaaS clusteru v hello stejným způsobem jako nasazení tooan místní cluster.</span><span class="sxs-lookup"><span data-stu-id="65f14-276">Then, deploy hello service toohello IaaS cluster in hello same way you deploy tooan on-premises cluster.</span></span> <span data-ttu-id="65f14-277">Podrobné pokyny najdete v části hello Readme.doc v HelloWordR2.</span><span class="sxs-lookup"><span data-stu-id="65f14-277">For detailed steps, see hello Readme.doc in HelloWordR2.</span></span> <span data-ttu-id="65f14-278">Upravit a vytvořit hello HellWorldR2 a další projekty, jak je popsáno v následující části toogenerate hello SOA klientské aplikace, které běží na clusteru služby Azure IaaS hello.</span><span class="sxs-lookup"><span data-stu-id="65f14-278">Modify and build hello HellWorldR2 and other projects as described in hello following section toogenerate hello SOA client applications that run on an Azure IaaS cluster.</span></span>

### <a name="use-http-binding-with-azure-storage-queue"></a><span data-ttu-id="65f14-279">Vazba Http pomocí fronty Azure storage</span><span class="sxs-lookup"><span data-stu-id="65f14-279">Use Http binding with Azure storage queue</span></span>
<span data-ttu-id="65f14-280">Vazba Http toouse s frontu úložiště Azure provést několik změn toohello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="65f14-280">toouse Http binding with an Azure storage queue, make a few changes toohello sample code.</span></span>

* <span data-ttu-id="65f14-281">Název clusteru hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="65f14-281">Update hello cluster name.</span></span>
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* <span data-ttu-id="65f14-282">Volitelně můžete použít výchozí hello TransportScheme v SessionStartInfo nebo explicitně nastavit tooHttp.</span><span class="sxs-lookup"><span data-stu-id="65f14-282">Optionally, use hello default TransportScheme in SessionStartInfo or explicitly set it tooHttp.</span></span>

```
    info.TransportScheme = TransportScheme.Http;
```

* <span data-ttu-id="65f14-283">Používejte výchozí vazbu pro hello BrokerClient.</span><span class="sxs-lookup"><span data-stu-id="65f14-283">Use default binding for hello BrokerClient.</span></span>
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    <span data-ttu-id="65f14-284">Nebo nastavte explicitně pomocí hello basicHttpBinding.</span><span class="sxs-lookup"><span data-stu-id="65f14-284">Or set explicitly using hello basicHttpBinding.</span></span>
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* <span data-ttu-id="65f14-285">Volitelně můžete nastavit hello UseAzureQueue příznak tootrue v SessionStartInfo.</span><span class="sxs-lookup"><span data-stu-id="65f14-285">Optionally, set hello UseAzureQueue flag tootrue in SessionStartInfo.</span></span> <span data-ttu-id="65f14-286">Pokud není sada, nastaví se tootrue ve výchozím nastavení při hello název clusteru má přípony domény Azure a hello TransportScheme Http.</span><span class="sxs-lookup"><span data-stu-id="65f14-286">If not set, it will be set tootrue by default when hello cluster name has Azure domain suffixes and hello TransportScheme is Http.</span></span>
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a><span data-ttu-id="65f14-287">Použít vazbu Http bez fronty Azure storage</span><span class="sxs-lookup"><span data-stu-id="65f14-287">Use Http binding without Azure storage queue</span></span>
<span data-ttu-id="65f14-288">Vazba Http toouse bez fronty úložiště Azure, explicitně sadu hello UseAzureQueue příznak toofalse v hello SessionStartInfo.</span><span class="sxs-lookup"><span data-stu-id="65f14-288">toouse Http binding without an Azure storage queue, explicitly set hello UseAzureQueue flag toofalse in hello SessionStartInfo.</span></span>

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a><span data-ttu-id="65f14-289">NetTcp vazbu používají</span><span class="sxs-lookup"><span data-stu-id="65f14-289">Use NetTcp binding</span></span>
<span data-ttu-id="65f14-290">toouse NetTcp vazby, konfigurace hello je podobné tooconnecting tooan místní cluster.</span><span class="sxs-lookup"><span data-stu-id="65f14-290">toouse NetTcp binding, hello configuration is similar tooconnecting tooan on-premises cluster.</span></span> <span data-ttu-id="65f14-291">Je nutné tooopen několik koncových bodů hello hlavního uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="65f14-291">You need tooopen a few endpoints on hello head node VM.</span></span> <span data-ttu-id="65f14-292">Pokud jste použili hello HPC Pack IaaS nasazení skriptu toocreate hello clusteru, například sada koncových bodů hello v hello portál Azure následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="65f14-292">If you used hello HPC Pack IaaS deployment script toocreate hello cluster, for example, set hello endpoints in hello Azure portal as follows.</span></span>

1. <span data-ttu-id="65f14-293">Zastavte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="65f14-293">Stop hello VM.</span></span>
2. <span data-ttu-id="65f14-294">Přidat porty TCP hello 9090, 9087, 9091, 9094 pro hello relace, zprostředkovatel, respektive zprostředkovatel worker a datové služby</span><span class="sxs-lookup"><span data-stu-id="65f14-294">Add hello TCP ports 9090, 9087, 9091, 9094 for hello Session, Broker, Broker worker, and Data services, respectively</span></span>
   
    ![Konfigurace koncových bodů][endpoint-new-portal]
3. <span data-ttu-id="65f14-296">Spusťte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="65f14-296">Start hello VM.</span></span>

<span data-ttu-id="65f14-297">Hello SOA klientská aplikace nevyžaduje žádné změny kromě změna hello head toohello IaaS clusteru úplný název.</span><span class="sxs-lookup"><span data-stu-id="65f14-297">hello SOA client application requires no changes except altering hello head name toohello IaaS cluster full name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="65f14-298">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65f14-298">Next steps</span></span>
* <span data-ttu-id="65f14-299">V tématu [tyto prostředky](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) pro další informace o spuštění úlohy aplikace Excel pomocí sady HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="65f14-299">See [these resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) for more information about running Excel workloads with HPC Pack.</span></span>
* <span data-ttu-id="65f14-300">V tématu [Správa služeb SOA v Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) Další informace o nasazení a správě SOA services pomocí sady HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="65f14-300">See [Managing SOA Services in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) for more about deploying and managing SOA services with HPC Pack.</span></span>

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
