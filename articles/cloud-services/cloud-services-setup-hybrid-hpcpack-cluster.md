---
title: "aaaSet až hybridní HPC Pack clusteru v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Microsoft HPC Pack a Azure tooset až malé, hybridní vysoký výkon výpočetní (prostředí HPC) clusteru"
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: 5ad30d78dcd0c6a95d2a61f25015232edab3563c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a><span data-ttu-id="76e65-103">Nastavení hybridní vysokovýkonného výpočetního prostředí (HPC) clusteru pomocí sady Microsoft HPC Pack a na vyžádání Azure výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="76e65-103">Set up a hybrid high performance computing (HPC) cluster with Microsoft HPC Pack and on-demand Azure compute nodes</span></span>
<span data-ttu-id="76e65-104">Používejte Microsoft HPC Pack 2012 R2 a Azure tooset až malé, hybridní vysokovýkonného výpočetního prostředí (HPC) clusteru.</span><span class="sxs-lookup"><span data-stu-id="76e65-104">Use Microsoft HPC Pack 2012 R2 and Azure tooset up a small, hybrid high performance computing (HPC) cluster.</span></span> <span data-ttu-id="76e65-105">Hello clusteru uvedené v tomto článku se skládá z hlavního uzlu místními HPC Pack a některé výpočetní uzly, které nasazujete na vyžádání v Azure cloud service.</span><span class="sxs-lookup"><span data-stu-id="76e65-105">hello cluster shown in this article consists of an on-premises HPC Pack head node and some compute nodes you deploy on-demand in an Azure cloud service.</span></span> <span data-ttu-id="76e65-106">Když pak spustíte výpočetní úlohy v clusteru hybridní hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-106">You can then run compute jobs on hello hybrid cluster.</span></span>

![Hybridní clusteru HPC][Overview] 

<span data-ttu-id="76e65-108">Tento kurz ukazuje jeden ze způsobů, se někdy označuje clusteru "shluků toohello cloud," toouse, škálovatelnou a na vyžádání prostředků Azure toorun náročné aplikace.</span><span class="sxs-lookup"><span data-stu-id="76e65-108">This tutorial shows one approach, sometimes called cluster "burst toohello cloud," toouse scalable, on-demand Azure resources toorun compute-intensive applications.</span></span>

<span data-ttu-id="76e65-109">Tento kurz předpokládá žádné předchozí zkušenosti s výpočetní clustery nebo HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="76e65-109">This tutorial assumes no prior experience with compute clusters or HPC Pack.</span></span> <span data-ttu-id="76e65-110">Je určený jenom toohelp nasazení hybridního výpočetního clusteru rychle pro demonstrační účely.</span><span class="sxs-lookup"><span data-stu-id="76e65-110">It is intended only toohelp you deploy a hybrid compute cluster quickly for demonstration purposes.</span></span> <span data-ttu-id="76e65-111">Požadavky a kroky toodeploy hybridní HPC Pack clusteru větší škálované v provozním prostředí, nebo toouse HPC Pack 2016, najdete v tématu hello [podrobné pokyny](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="76e65-111">For considerations and steps toodeploy a hybrid HPC Pack cluster at greater scale in a production environment, or toouse HPC Pack 2016, see hello [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span> <span data-ttu-id="76e65-112">Pro scénáře s HPC Pack, včetně automatizované nasazení clusteru ve virtuálních počítačích Azure, najdete v části [možnosti clusteru HPC pomocí sady Microsoft HPC Pack v Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="76e65-112">For other scenarios with HPC Pack, including automated cluster deployment in Azure virtual machines, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76e65-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="76e65-113">Prerequisites</span></span>
* <span data-ttu-id="76e65-114">**Předplatné Azure** – Pokud nemáte předplatné Azure, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="76e65-114">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>
* <span data-ttu-id="76e65-115">**Místní počítač se systémem Windows Server 2012 R2 nebo Windows Server 2012** -použít tento počítač jako hello hlavního uzlu v clusteru HPC hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-115">**An on-premises computer running Windows Server 2012 R2 or Windows Server 2012** - Use this computer as hello head node of hello HPC cluster.</span></span> <span data-ttu-id="76e65-116">Pokud už používáte Windows Server, můžete stáhnout a nainstalovat [zkušební verze](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span><span class="sxs-lookup"><span data-stu-id="76e65-116">If you aren't already running Windows Server, you can download and install an [evaluation version](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span></span>
  
  * <span data-ttu-id="76e65-117">Hello počítač musí být tooan připojené k doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="76e65-117">hello computer must be joined tooan Active Directory domain.</span></span> <span data-ttu-id="76e65-118">Pro účely testování můžete nakonfigurovat hello hlavního uzlu počítač jako řadič domény.</span><span class="sxs-lookup"><span data-stu-id="76e65-118">For test purposes, you can configure hello head node computer as a domain controller.</span></span> <span data-ttu-id="76e65-119">tooadd hello role serveru služby Active Directory Domain Services a hello hlavního uzlu počítač jako řadič domény povýšit, naleznete v dokumentaci k systému Windows Server hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-119">tooadd hello Active Directory Domain Services server role and promote hello head node computer as a domain controller, see hello documentation for Windows Server.</span></span>
  * <span data-ttu-id="76e65-120">toosupport HPC Pack hello operačního systému musí být nainstalován v jednom z těchto jazycích: angličtina, japonština nebo čínština (zjednodušená).</span><span class="sxs-lookup"><span data-stu-id="76e65-120">toosupport HPC Pack, hello operating system must be installed in one of these languages: English, Japanese, or Chinese (Simplified).</span></span>
  * <span data-ttu-id="76e65-121">Ověřte, že se nainstalují důležité a kritické aktualizace.</span><span class="sxs-lookup"><span data-stu-id="76e65-121">Verify that important and critical updates are installed.</span></span>
* <span data-ttu-id="76e65-122">**HPC Pack 2012 R2** - [Stáhnout](http://go.microsoft.com/fwlink/p/?linkid=328024) hello instalační balíček pro nejnovější verzi hello volné zdarma a zkopírujte hello soubory toohello hlavního uzlu počítače.</span><span class="sxs-lookup"><span data-stu-id="76e65-122">**HPC Pack 2012 R2** - [Download](http://go.microsoft.com/fwlink/p/?linkid=328024) hello installation package for hello latest version free of charge and copy hello files toohello head node computer.</span></span> <span data-ttu-id="76e65-123">Zvolte instalační soubory v hello stejný jazyk jako vaše instalace systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="76e65-123">Choose installation files in hello same language as your installation of Windows Server.</span></span>

    >[!NOTE]
    > <span data-ttu-id="76e65-124">Pokud chcete toouse HPC Pack 2016 místo HPC Pack 2012 R2, je potřeba další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="76e65-124">If you want toouse HPC Pack 2016 instead of HPC Pack 2012 R2, additional configuration is needed.</span></span> <span data-ttu-id="76e65-125">V tématu hello [podrobné pokyny](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="76e65-125">See hello [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
    > 
* <span data-ttu-id="76e65-126">**Účet domény** -tento účet musí být nakonfigurované hello hlavního uzlu tooinstall HPC Pack oprávnění místního správce.</span><span class="sxs-lookup"><span data-stu-id="76e65-126">**Domain account** - This account must be configured with local Administrator permissions on hello head node tooinstall HPC Pack.</span></span>
* <span data-ttu-id="76e65-127">**Připojení TCP na portu 443** z hlavního uzlu tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-127">**TCP connectivity on port 443** from hello head node tooAzure.</span></span>

## <a name="install-hpc-pack-on-hello-head-node"></a><span data-ttu-id="76e65-128">Instalace sady HPC Pack hello hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="76e65-128">Install HPC Pack on hello head node</span></span>
<span data-ttu-id="76e65-129">První instalaci sady Microsoft HPC Pack v místní počítače se spuštěným systémem Windows Server.</span><span class="sxs-lookup"><span data-stu-id="76e65-129">You first install Microsoft HPC Pack on your on-premises computer running Windows Server.</span></span> <span data-ttu-id="76e65-130">Tento počítač se změní na hello hlavního uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-130">This computer becomes hello head node of hello cluster.</span></span>

1. <span data-ttu-id="76e65-131">Toohello hlavního uzlu se přihlaste pomocí účtu domény, který má oprávnění místního správce.</span><span class="sxs-lookup"><span data-stu-id="76e65-131">Log on toohello head node by using a domain account that has local Administrator permissions.</span></span>

2. <span data-ttu-id="76e65-132">Spusťte Průvodce instalací HPC Pack hello spuštěním Setup.exe z hello HPC Pack instalačních souborů.</span><span class="sxs-lookup"><span data-stu-id="76e65-132">Start hello HPC Pack Installation Wizard by running Setup.exe from hello HPC Pack installation files.</span></span>

3. <span data-ttu-id="76e65-133">Na hello **instalace HPC Pack 2012 R2** obrazovky, klikněte na tlačítko **novou instalaci, nebo přidejte nové funkce tooan existující instalaci**.</span><span class="sxs-lookup"><span data-stu-id="76e65-133">On hello **HPC Pack 2012 R2 Setup** screen, click **New installation or add new features tooan existing installation**.</span></span>

    ![Instalace sady HPC Pack 2012][install_hpc1]

4. <span data-ttu-id="76e65-135">Na hello **stránku smlouvy uživatele softwaru Microsoft**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="76e65-135">On hello **Microsoft Software User Agreement page**, click **Next**.</span></span>

5. <span data-ttu-id="76e65-136">Na hello **vybrat typ instalace** klikněte na tlačítko **vytvoření nového clusteru HPC vytvořením hlavního uzlu**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76e65-136">On hello **Select Installation Type** page, click **Create a new HPC cluster by creating a head node**, and then click **Next**.</span></span>

6. <span data-ttu-id="76e65-137">Spustí Průvodce Hello několik testů před instalací.</span><span class="sxs-lookup"><span data-stu-id="76e65-137">hello wizard runs several pre-installation tests.</span></span> <span data-ttu-id="76e65-138">Klikněte na tlačítko **Další** na hello **pravidla instalace** Pokud všechny testy byly úspěšné.</span><span class="sxs-lookup"><span data-stu-id="76e65-138">Click **Next** on hello **Installation Rules** page if all tests pass.</span></span> <span data-ttu-id="76e65-139">Jinak Zkontrolujte zadané informace hello a proveďte potřebné změny ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="76e65-139">Otherwise, review hello information provided and make any necessary changes in your environment.</span></span> <span data-ttu-id="76e65-140">Pak spusťte znovu hello testů nebo pokud nezbytné spuštění hello Průvodce instalací znovu.</span><span class="sxs-lookup"><span data-stu-id="76e65-140">Then run hello tests again or if necessary start hello Installation Wizard again.</span></span>
7. <span data-ttu-id="76e65-141">Na hello **HPC DB konfigurace** se přesvědčte, že **hlavní uzel** pro všechny databáze HPC je vybrána a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76e65-141">On hello **HPC DB Configuration** page, make sure **Head Node** is selected for all HPC databases, and then click **Next**.</span></span> 

    ![Konfigurace databáze][install_hpc4]

8. <span data-ttu-id="76e65-143">Přijměte výchozí nastavení na dalších stránkách průvodce hello hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-143">Accept default selections on hello remaining pages of hello wizard.</span></span> <span data-ttu-id="76e65-144">Na hello **nainstalovat požadované součásti** klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="76e65-144">On hello **Install Required Components** page, click **Install**.</span></span>
   
    ![Instalace][install_hpc6]

9. <span data-ttu-id="76e65-146">Po dokončení instalace hello, zrušte zaškrtnutí políčka **Start Správce clusteru HPC** a pak klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="76e65-146">After hello installation completes, uncheck **Start HPC Cluster Manager** and then click **Finish**.</span></span> <span data-ttu-id="76e65-147">(V pozdější fázi spusťte Správce clusteru HPC.)</span><span class="sxs-lookup"><span data-stu-id="76e65-147">(You start HPC Cluster Manager in a later step.)</span></span>
   
    ![Dokončit][install_hpc7]

## <a name="prepare-hello-azure-subscription"></a><span data-ttu-id="76e65-149">Příprava hello předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="76e65-149">Prepare hello Azure subscription</span></span>
<span data-ttu-id="76e65-150">Proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com) s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="76e65-150">Perform hello following steps in hello [Azure portal](https://portal.azure.com) with your Azure subscription.</span></span> <span data-ttu-id="76e65-151">Po dokončení těchto kroků, můžete nasadit Azure uzly z hlavního uzlu místní hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-151">After completing these steps, you can deploy Azure nodes from hello on-premises head node.</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="76e65-152">Také si poznamenejte ID vašeho předplatného Azure, které budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="76e65-152">Also make a note of your Azure subscription ID, which you need later.</span></span> <span data-ttu-id="76e65-153">Najít ID hello v **odběry** hello portálu.</span><span class="sxs-lookup"><span data-stu-id="76e65-153">Find hello ID in **Subscriptions** in hello portal.</span></span>
  > 

### <a name="upload-hello-default-management-certificate"></a><span data-ttu-id="76e65-154">Nahrát hello výchozího certifikátu pro správu</span><span class="sxs-lookup"><span data-stu-id="76e65-154">Upload hello default management certificate</span></span>
<span data-ttu-id="76e65-155">HPC Pack nainstaluje certifikát podepsaný svým držitelem hello hlavního uzlu, nazývá hello výchozí Microsoft HPC Azure Management certifikát, který nahrajete jako certifikát pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="76e65-155">HPC Pack installs a self-signed certificate on hello head node, called hello Default Microsoft HPC Azure Management certificate, that you can upload as an Azure management certificate.</span></span> <span data-ttu-id="76e65-156">Tento certifikát je k dispozici pro testování a nasazení testování konceptu toosecure hello připojení mezi hello hlavního uzlu a Azure.</span><span class="sxs-lookup"><span data-stu-id="76e65-156">This certificate is provided for testing and proof-of-concept deployments toosecure hello connection between hello head node and Azure.</span></span>

1. <span data-ttu-id="76e65-157">Z počítače hello hlavního uzlu, přihlášení toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="76e65-157">From hello head node computer, sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="76e65-158">Klikněte na tlačítko **odběry** > *your_subscription_name*.</span><span class="sxs-lookup"><span data-stu-id="76e65-158">Click **Subscriptions** > *your_subscription_name*.</span></span>

3. <span data-ttu-id="76e65-159">Klikněte na tlačítko **certifikáty pro správu** > **nahrát**.4.</span><span class="sxs-lookup"><span data-stu-id="76e65-159">Click **Management certificates** > **Upload**.4.</span></span> <span data-ttu-id="76e65-160">Procházejte hello hlavního uzlu pro 2012\Bin\hpccert.cer C:\Program Files\Microsoft HPC Pack souboru hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-160">Browse on hello head node for hello file C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer.</span></span> <span data-ttu-id="76e65-161">Potom klikněte na **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="76e65-161">Then, click **Upload**.</span></span>

   
<span data-ttu-id="76e65-162">Hello **výchozí HPC Azure Management** certifikát se zobrazí v seznamu hello certifikáty pro správu.</span><span class="sxs-lookup"><span data-stu-id="76e65-162">hello **Default HPC Azure Management** certificate appears in hello list of management certificates.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="76e65-163">Vytvoření cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="76e65-163">Create an Azure cloud service</span></span>
> [!NOTE]
> <span data-ttu-id="76e65-164">Pro nejlepší výkon, vytvořit hello cloudové služby a účet úložiště hello (v pozdější fázi) v hello stejné zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="76e65-164">For best performance, create hello cloud service and hello storage account (in a later step) in hello same geographic region.</span></span>
> 
> 

1. <span data-ttu-id="76e65-165">Hello portálu, klikněte na tlačítko **cloudových služeb (klasické)** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="76e65-165">In hello portal, click **Cloud services (classic)** > **+Add**.</span></span>

2.  <span data-ttu-id="76e65-166">Zadejte název DNS pro službu hello, vyberte skupinu prostředků a umístění a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="76e65-166">Type a DNS name for hello service, choose a resource group and a location, and then click **Create**.</span></span>


### <a name="create-an-azure-storage-account"></a><span data-ttu-id="76e65-167">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="76e65-167">Create an Azure storage account</span></span>
1. <span data-ttu-id="76e65-168">Hello portálu, klikněte na tlačítko **účty úložiště (klasické)** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="76e65-168">In hello portal, click **Storage accounts (classic)** > **+Add**.</span></span>

2. <span data-ttu-id="76e65-169">Zadejte název pro účet hello a vyberte hello **Classic** modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="76e65-169">Type a name for hello account, and select hello **Classic** deployment model.</span></span>

3. <span data-ttu-id="76e65-170">Vyberte skupinu prostředků a umístění a nechte ostatní nastavení na výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="76e65-170">Choose a resource group and a location, and leave other settings at default values.</span></span> <span data-ttu-id="76e65-171">Poté klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="76e65-171">Then click **Create**.</span></span>

## <a name="configure-hello-head-node"></a><span data-ttu-id="76e65-172">Konfigurace hlavního uzlu hello</span><span class="sxs-lookup"><span data-stu-id="76e65-172">Configure hello head node</span></span>
<span data-ttu-id="76e65-173">toouse toodeploy Správce clusteru HPC Azure uzly a úlohy toosubmit nejprve provést několik kroků konfigurace požadovaných clusterových.</span><span class="sxs-lookup"><span data-stu-id="76e65-173">toouse HPC Cluster Manager toodeploy Azure nodes and toosubmit jobs, first perform some required cluster configuration steps.</span></span>

1. <span data-ttu-id="76e65-174">Hello hlavního uzlu spusťte Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="76e65-174">On hello head node, start HPC Cluster Manager.</span></span> <span data-ttu-id="76e65-175">Pokud hello **vyberte hlavní uzel** se zobrazí dialogové okno, klikněte na tlačítko **místního počítače**.</span><span class="sxs-lookup"><span data-stu-id="76e65-175">If hello **Select Head Node** dialog box appears, click **Local Computer**.</span></span> <span data-ttu-id="76e65-176">Hello **nasazení na seznam úkolů** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="76e65-176">hello **Deployment To-do List** appears.</span></span>

2. <span data-ttu-id="76e65-177">V části **požadované úlohy nasazení**, klikněte na tlačítko **konfigurace sítě**.</span><span class="sxs-lookup"><span data-stu-id="76e65-177">Under **Required deployment tasks**, click **Configure your network**.</span></span>
   
    ![Konfigurace sítě][config_hpc2]

3. <span data-ttu-id="76e65-179">V hello Průvodce konfigurací sítě, vyberte **všechny uzly pouze v podnikové síti** (topologie 5).</span><span class="sxs-lookup"><span data-stu-id="76e65-179">In hello Network Configuration Wizard, select **All nodes only on an enterprise network** (Topology 5).</span></span> <span data-ttu-id="76e65-180">Tato konfigurace sítě je nejjednodušší pro demonstrační účely hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-180">This network configuration is hello simplest for demonstration purposes.</span></span>
   
    ![Topologie 5][config_hpc3]
   
4. <span data-ttu-id="76e65-182">Klikněte na tlačítko **Další** tooaccept výchozí hodnoty na hello zbývající stránky průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-182">Click **Next** tooaccept default values on hello remaining pages of hello wizard.</span></span> <span data-ttu-id="76e65-183">Potom na hello **zkontrolujte** , klikněte na **konfigurace** toocomplete hello síťové konfigurace.</span><span class="sxs-lookup"><span data-stu-id="76e65-183">Then, on hello **Review** tab, click **Configure** toocomplete hello network configuration.</span></span>

5. <span data-ttu-id="76e65-184">V hello **nasazení na seznam úkolů**, klikněte na tlačítko **zadejte pověření pro instalaci**.</span><span class="sxs-lookup"><span data-stu-id="76e65-184">In hello **Deployment To-do List**, click **Provide installation credentials**.</span></span>

6. <span data-ttu-id="76e65-185">V hello **pověření pro instalaci** dialogové okno, zadejte přihlašovací údaje hello účtu domény hello, kterou jste použili tooinstall HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="76e65-185">In hello **Installation Credentials** dialog box, type hello credentials of hello domain account that you used tooinstall HPC Pack.</span></span> <span data-ttu-id="76e65-186">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="76e65-186">Then click **OK**.</span></span> 
   
    ![Pověření pro instalaci][config_hpc6]
   
7. <span data-ttu-id="76e65-188">V hello **nasazení na seznam úkolů**, klikněte na tlačítko **konfigurace hello pojmenování nové uzly**.</span><span class="sxs-lookup"><span data-stu-id="76e65-188">In hello **Deployment To-do List**, click **Configure hello naming of new nodes**.</span></span>

8. <span data-ttu-id="76e65-189">V hello **zadejte řady pojmenování uzlu** dialogové okno pole, přijměte výchozí hello pojmenování řady a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="76e65-189">In hello **Specify Node Naming Series** dialog box, accept hello default naming series and click **OK**.</span></span> <span data-ttu-id="76e65-190">Dokončení tohoto kroku, i když hello uzlů Azure zadaná v tomto kurzu jsou pojmenované automaticky.</span><span class="sxs-lookup"><span data-stu-id="76e65-190">Complete this step even though hello Azure nodes you add in this tutorial are named automatically.</span></span>
   
    ![Pojmenování uzlu][config_hpc8]
   
9. <span data-ttu-id="76e65-192">V hello **nasazení na seznam úkolů**, klikněte na tlačítko **vytvoření šablony uzlu**.</span><span class="sxs-lookup"><span data-stu-id="76e65-192">In hello **Deployment To-do List**, click **Create a node template**.</span></span> <span data-ttu-id="76e65-193">Později v kurzu hello použijete hello uzel šablony tooadd uzlů Azure toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="76e65-193">Later in hello tutorial, you use hello node template tooadd Azure nodes toohello cluster.</span></span>

10. <span data-ttu-id="76e65-194">V hello Průvodce vytvořením šablony uzlu, proveďte následující hello:</span><span class="sxs-lookup"><span data-stu-id="76e65-194">In hello Create Node Template Wizard, do hello following:</span></span>
    
    <span data-ttu-id="76e65-195">a.</span><span class="sxs-lookup"><span data-stu-id="76e65-195">a.</span></span> <span data-ttu-id="76e65-196">Na hello **výběr typu šablony uzlu** klikněte na tlačítko **šablony uzlu služby Windows Azure**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76e65-196">On hello **Choose Node Template Type** page, click **Windows Azure node template**, and then click **Next**.</span></span>
    
    ![Šablony uzlu][config_hpc10]
    
    <span data-ttu-id="76e65-198">b.</span><span class="sxs-lookup"><span data-stu-id="76e65-198">b.</span></span> <span data-ttu-id="76e65-199">Klikněte na tlačítko **Další** tooaccept hello výchozí název šablony.</span><span class="sxs-lookup"><span data-stu-id="76e65-199">Click **Next** tooaccept hello default template name.</span></span>
    
    <span data-ttu-id="76e65-200">c.</span><span class="sxs-lookup"><span data-stu-id="76e65-200">c.</span></span> <span data-ttu-id="76e65-201">Na hello **poskytují informace o předplatném** zadejte svoje ID předplatného Azure (k dispozici v informací o vašem účtu Azure).</span><span class="sxs-lookup"><span data-stu-id="76e65-201">On hello **Provide Subscription Information** page, enter your Azure subscription ID (available in your Azure account information).</span></span> <span data-ttu-id="76e65-202">Potom v **certifikát pro správu**, vyhledejte **výchozí Microsoft HPC Azure Management.**</span><span class="sxs-lookup"><span data-stu-id="76e65-202">Then, in **Management certificate**, browse for **Default Microsoft HPC Azure Management.**</span></span> <span data-ttu-id="76e65-203">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="76e65-203">Then click **Next**.</span></span>
    
    ![Šablony uzlu][config_hpc12]
    
    <span data-ttu-id="76e65-205">d.</span><span class="sxs-lookup"><span data-stu-id="76e65-205">d.</span></span> <span data-ttu-id="76e65-206">Na hello **poskytují informace o službě** stránky, vyberte hello Cloudová služba a hello účet úložiště, který jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="76e65-206">On hello **Provide Service Information** page, select hello cloud service and hello storage account that you created in a previous step.</span></span> <span data-ttu-id="76e65-207">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="76e65-207">Then click **Next**.</span></span>
    
    ![Šablony uzlu][config_hpc13]
    
    <span data-ttu-id="76e65-209">e.</span><span class="sxs-lookup"><span data-stu-id="76e65-209">e.</span></span> <span data-ttu-id="76e65-210">Klikněte na tlačítko **Další** tooaccept výchozí hodnoty na hello zbývající stránky průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-210">Click **Next** tooaccept default values on hello remaining pages of hello wizard.</span></span> <span data-ttu-id="76e65-211">Potom na hello **zkontrolujte** , klikněte na **vytvořit** toocreate hello uzel šablony.</span><span class="sxs-lookup"><span data-stu-id="76e65-211">Then, on hello **Review** tab, click **Create** toocreate hello node template.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="76e65-212">Ve výchozím nastavení, hello Azure uzlu šablona obsahuje nastavení pro toostart (zřídit) a zastavení hello uzly ručně, pomocí Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="76e65-212">By default, hello Azure node template includes settings for you toostart (provision) and stop hello nodes manually, using HPC Cluster Manager.</span></span> <span data-ttu-id="76e65-213">Volitelně můžete nakonfigurovat plán toostart a ukončení hello uzlů Azure automaticky.</span><span class="sxs-lookup"><span data-stu-id="76e65-213">You can optionally configure a schedule toostart and stop hello Azure nodes automatically.</span></span>
    > 
    > 

## <a name="add-azure-nodes-toohello-cluster"></a><span data-ttu-id="76e65-214">Přidání uzlů Azure toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="76e65-214">Add Azure nodes toohello cluster</span></span>
<span data-ttu-id="76e65-215">Teď použijte hello uzel šablony tooadd uzlů Azure toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="76e65-215">Now use hello node template tooadd Azure nodes toohello cluster.</span></span> <span data-ttu-id="76e65-216">Přidání clusteru toohello uzly hello ukládá informace o jejich konfiguraci tak, aby bylo možné spustit (zřídit) je kdykoli v hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="76e65-216">Adding hello nodes toohello cluster stores their configuration information so that you can start (provision) them at any time in hello cloud service.</span></span> <span data-ttu-id="76e65-217">Vaše předplatné pouze získá účtovat uzlů Azure po hello instance běží v cloudové službě hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-217">Your subscription only gets charged for Azure nodes after hello instances are running in hello cloud service.</span></span>

<span data-ttu-id="76e65-218">Postupujte podle těchto kroků tooadd dva malé uzly.</span><span class="sxs-lookup"><span data-stu-id="76e65-218">Follow these steps tooadd two Small nodes.</span></span>

1. <span data-ttu-id="76e65-219">Ve Správci clusteru HPC, klikněte na tlačítko **uzlu správy** (nazývá **Správa prostředků** v aktuálních verzích HPC Pack) > **přidat uzel**.</span><span class="sxs-lookup"><span data-stu-id="76e65-219">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack) > **Add Node**.</span></span>
   
    ![Přidání uzlu][add_node1]

2. <span data-ttu-id="76e65-221">V hello Průvodce přidáním uzlu na hello **vybrat způsob nasazení** klikněte na tlačítko **systému Windows Azure přidat uzly**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76e65-221">In hello Add Node Wizard, on hello **Select Deployment Method** page, click **Add Windows Azure nodes**, and then click **Next**.</span></span>
   
    ![Přidání uzlu Azure][add_node1_1]

3. <span data-ttu-id="76e65-223">Na hello **zadejte nové uzly** stránky, dříve vytvořené šablony Azure uzlu vyberte hello (nazývá ve výchozím nastavení **výchozí šablonu AzureNode**).</span><span class="sxs-lookup"><span data-stu-id="76e65-223">On hello **Specify New Nodes** page, select hello Azure node template you created previously (called by default **Default AzureNode Template**).</span></span> <span data-ttu-id="76e65-224">Pak zadejte **2** uzly velikosti **malé**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76e65-224">Then specify **2** nodes of size **Small**, and then click **Next**.</span></span>
   
    ![Určit uzlů][add_node2]
   
4. <span data-ttu-id="76e65-226">Na hello **hello dokončení Průvodce přidáním uzlu** klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="76e65-226">On hello **Completing hello Add Node Wizard** page, click **Finish**.</span></span>
    
     <span data-ttu-id="76e65-227">Dva uzly Azure s názvem **AzureCN 0001** a **AzureCN 0002**, se nyní zobrazí ve Správci clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="76e65-227">Two Azure nodes, named **AzureCN-0001** and **AzureCN-0002**, now appear in HPC Cluster Manager.</span></span> <span data-ttu-id="76e65-228">Obě jsou v hello **není nasazena** stavu.</span><span class="sxs-lookup"><span data-stu-id="76e65-228">Both are in hello **Not-Deployed** state.</span></span>
   
    ![Přidání uzlů][add_node3]

## <a name="start-hello-azure-nodes"></a><span data-ttu-id="76e65-230">Spustit hello uzlů Azure</span><span class="sxs-lookup"><span data-stu-id="76e65-230">Start hello Azure nodes</span></span>
<span data-ttu-id="76e65-231">Pokud chcete prostředky clusteru hello toouse v Azure, použijte Správce clusteru HPC toostart (zřídit) hello uzlů Azure a jejich převedení do online režimu.</span><span class="sxs-lookup"><span data-stu-id="76e65-231">When you want toouse hello cluster resources in Azure, use HPC Cluster Manager toostart (provision) hello Azure nodes and bring them online.</span></span>

1. <span data-ttu-id="76e65-232">Ve Správci clusteru HPC, klikněte na tlačítko **uzlu správy** (nazývá **Správa prostředků** v aktuálních verzích HPC Pack), a vyberte hello uzlů Azure.</span><span class="sxs-lookup"><span data-stu-id="76e65-232">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack), and select hello Azure nodes.</span></span>

2. <span data-ttu-id="76e65-233">Klikněte na tlačítko **spustit**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="76e65-233">Click **Start**, and then click **OK**.</span></span>
   
   ![Počáteční uzly][add_node4]
   
    <span data-ttu-id="76e65-235">uzly Hello přechod toohello **zřizování** stavu.</span><span class="sxs-lookup"><span data-stu-id="76e65-235">hello nodes transition toohello **Provisioning** state.</span></span> <span data-ttu-id="76e65-236">Zobrazení hello zřizování hello tootrack protokolu průběh zřizování.</span><span class="sxs-lookup"><span data-stu-id="76e65-236">View hello provisioning log tootrack hello provisioning progress.</span></span>
   
    ![Zřízení uzly][add_node6]

3. <span data-ttu-id="76e65-238">Po několika minutách hello uzlů Azure zřídit a jsou v hello **Offline** stavu.</span><span class="sxs-lookup"><span data-stu-id="76e65-238">After a few minutes, hello Azure nodes finish provisioning and are in hello **Offline** state.</span></span> <span data-ttu-id="76e65-239">V tomto stavu instance rolí hello běží, ale ještě nepovoluje úlohy clusteru.</span><span class="sxs-lookup"><span data-stu-id="76e65-239">In this state, hello role instances are running but cannot yet accept cluster jobs.</span></span>

4. <span data-ttu-id="76e65-240">tooconfirm, který hello instance role běží v hello portálu Azure, klikněte na tlačítko **cloudové služby (klasické)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="76e65-240">tooconfirm that hello role instances are running, in hello Azure portal, click **Cloud Services (classic)** > *your_cloud_service_name*.</span></span>
   
   <span data-ttu-id="76e65-241">Měli byste vidět dvě **HpcWorkerRole** instancí (uzlů) spuštěných ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-241">You should see two **HpcWorkerRole** instances (nodes) running in hello service.</span></span> <span data-ttu-id="76e65-242">HPC Pack také automaticky nasadí dvě **HpcProxy** instance (velikost střední) toohandle komunikace mezi hello hlavního uzlu a Azure.</span><span class="sxs-lookup"><span data-stu-id="76e65-242">HPC Pack also automatically deploys two **HpcProxy** instances (size Medium) toohandle communication between hello head node and Azure.</span></span>

   ![Spuštěné instance][view_instances1]

5. <span data-ttu-id="76e65-244">toobring hello Azure uzly online toorun clusteru úlohy, vyberte hello uzly, klikněte pravým tlačítkem a pak klikněte na tlačítko **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="76e65-244">toobring hello Azure nodes online toorun cluster jobs, select hello nodes, right-click, and then click **Bring Online**.</span></span>
   
    ![Offline uzly][add_node7]
   
    <span data-ttu-id="76e65-246">Správce clusteru HPC označuje, že hello uzly jsou v hello **Online** stavu.</span><span class="sxs-lookup"><span data-stu-id="76e65-246">HPC Cluster Manager indicates that hello nodes are in hello **Online** state.</span></span>

## <a name="run-a-command-across-hello-cluster"></a><span data-ttu-id="76e65-247">Spuštění příkazu v clusteru hello</span><span class="sxs-lookup"><span data-stu-id="76e65-247">Run a command across hello cluster</span></span>
<span data-ttu-id="76e65-248">toocheck hello instalace, použití hello HPC Pack **clusrun** příkaz toorun příkaz nebo aplikace na jeden nebo více uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="76e65-248">toocheck hello installation, use hello HPC Pack **clusrun** command toorun a command or application on one or more cluster nodes.</span></span> <span data-ttu-id="76e65-249">Jako jednoduchý příklad, použít **clusrun** konfiguraci IP hello tooget hello uzlů Azure.</span><span class="sxs-lookup"><span data-stu-id="76e65-249">As a simple example, use **clusrun** tooget hello IP configuration of hello Azure nodes.</span></span>

1. <span data-ttu-id="76e65-250">Hello hlavního uzlu otevřete příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="76e65-250">On hello head node, open a command prompt as an administrator.</span></span>

2. <span data-ttu-id="76e65-251">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="76e65-251">Type hello following command:</span></span>
   
    `clusrun /nodes:azurecn* ipconfig`

3. <span data-ttu-id="76e65-252">Pokud se zobrazí výzva, zadejte heslo správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="76e65-252">If prompted, enter your cluster administrator password.</span></span> <span data-ttu-id="76e65-253">Měli byste vidět, že výstup podobný toohello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="76e65-253">You should see command output similar toohello following.</span></span>
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a><span data-ttu-id="76e65-255">Spustit úlohu testu</span><span class="sxs-lookup"><span data-stu-id="76e65-255">Run a test job</span></span>
<span data-ttu-id="76e65-256">Teď odešlete testovací úlohu, která běží na clusteru hybridní hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-256">Now submit a test job that runs on hello hybrid cluster.</span></span> <span data-ttu-id="76e65-257">V tomto příkladu je úloha jednoduché čištění parametrů (typ vnitřně paralelní výpočty).</span><span class="sxs-lookup"><span data-stu-id="76e65-257">This example is a simple parametric sweep job (a type of intrinsically parallel computation).</span></span> <span data-ttu-id="76e65-258">Tento příklad spustí dílčí úkoly, které přidat celé číslo tooitself pomocí hello **nastavit /a** příkaz.</span><span class="sxs-lookup"><span data-stu-id="76e65-258">This example runs subtasks that add an integer tooitself by using hello **set /a** command.</span></span> <span data-ttu-id="76e65-259">Všechny hello uzly v clusteru hello přispívat toofinishing hello dílčí úkoly pro celá čísla od 1 too100.</span><span class="sxs-lookup"><span data-stu-id="76e65-259">All hello nodes in hello cluster contribute toofinishing hello subtasks for integers from 1 too100.</span></span>

1. <span data-ttu-id="76e65-260">Ve Správci clusteru HPC, klikněte na tlačítko **úlohy správy** > **novou čištění úlohu oblouku**.</span><span class="sxs-lookup"><span data-stu-id="76e65-260">In HPC Cluster Manager, click **Job Management** > **New Parametric Sweep Job**.</span></span>
   
    ![Nová úloha][test_job1]

2. <span data-ttu-id="76e65-262">V hello **novou čištění úlohu oblouku** dialogu **příkazového řádku**, typ `set /a *+*` (přepsal hello výchozího příkazového řádku, který se zobrazí).</span><span class="sxs-lookup"><span data-stu-id="76e65-262">In hello **New Parametric Sweep Job** dialog box, in **Command line**, type `set /a *+*` (overwriting hello default command line that appears).</span></span> <span data-ttu-id="76e65-263">Ponechte výchozí hodnoty pro hello zbývající nastavení a potom klikněte na **odeslání** toosubmit hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="76e65-263">Leave default values for hello remaining settings, and then click **Submit** toosubmit hello job.</span></span>
   
    ![Čištění parametrů][param_sweep1]

3. <span data-ttu-id="76e65-265">Po dokončení úlohy hello, dvakrát klikněte na hello **Moje oblouku úloh** úlohy.</span><span class="sxs-lookup"><span data-stu-id="76e65-265">When hello job is finished, double-click hello **My Sweep Task** job.</span></span>

4. <span data-ttu-id="76e65-266">Klikněte na tlačítko **úlohy v zobrazení**a klikněte na výstup hello vypočítat tooview dílčí úlohy na stejné úrovni.</span><span class="sxs-lookup"><span data-stu-id="76e65-266">Click **View Tasks**, and then click a subtask tooview hello calculated output of that subtask.</span></span>
   
    ![Výsledky úlohy][view_job361]

5. <span data-ttu-id="76e65-268">Klikněte na tlačítko pro tento dílčí úlohy, který uzel provést hello výpočtu toosee **přidělené uzly**.</span><span class="sxs-lookup"><span data-stu-id="76e65-268">toosee which node performed hello calculation for that subtask, click **Allocated Nodes**.</span></span> <span data-ttu-id="76e65-269">(Váš cluster může zobrazovat název jiného uzlu.)</span><span class="sxs-lookup"><span data-stu-id="76e65-269">(Your cluster might show a different node name.)</span></span>
   
    ![Výsledky úlohy][view_job362]

## <a name="stop-hello-azure-nodes"></a><span data-ttu-id="76e65-271">Zastavit hello uzlů Azure</span><span class="sxs-lookup"><span data-stu-id="76e65-271">Stop hello Azure nodes</span></span>
<span data-ttu-id="76e65-272">Po vyzkoušení hello clusteru, zastavte hello uzlů Azure tooavoid nepotřebné poplatky tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="76e65-272">After you try out hello cluster, stop hello Azure nodes tooavoid unnecessary charges tooyour account.</span></span> <span data-ttu-id="76e65-273">Tento krok zastaví hello cloudové služby a odebere hello Azure role instance.</span><span class="sxs-lookup"><span data-stu-id="76e65-273">This step stops hello cloud service and removes hello Azure role instances.</span></span>

1. <span data-ttu-id="76e65-274">V modulu Správce clusteru HPC v **uzlu správy** (nazývá **Správa prostředků** v předchozích verzích HPC Pack), vyberte oba uzly Azure.</span><span class="sxs-lookup"><span data-stu-id="76e65-274">In HPC Cluster Manager, in **Node Management** (called **Resource Management** in previous versions of HPC Pack), select both Azure nodes.</span></span> <span data-ttu-id="76e65-275">Potom klikněte na **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="76e65-275">Then, click **Stop**.</span></span>
   
    ![Zastavit uzly][stop_node1]

2. <span data-ttu-id="76e65-277">V hello **zastavit systému Windows Azure uzly** dialogové okno, klikněte na tlačítko **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="76e65-277">In hello **Stop Windows Azure nodes** dialog box, click **Stop**.</span></span> 
   
3. <span data-ttu-id="76e65-278">uzly Hello přechod toohello **zastavení** stavu.</span><span class="sxs-lookup"><span data-stu-id="76e65-278">hello nodes transition toohello **Stopping** state.</span></span> <span data-ttu-id="76e65-279">Po několika minutách, Správce clusteru HPC uvádí, že jsou uzly hello **nasazení není**.</span><span class="sxs-lookup"><span data-stu-id="76e65-279">After a few minutes, HPC Cluster Manager shows that hello nodes are **Not-Deployed**.</span></span>
   
    ![Nenasazeno uzly][stop_node4]

4. <span data-ttu-id="76e65-281">hello tooconfirm, které instance rolí hello už běží v Azure, v portálu Azure klikněte na **cloudových služeb (klasické)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="76e65-281">tooconfirm that hello role instances are no longer running in Azure, in hello Azure portal, click **Cloud services (classic)** > *your_cloud_service_name*.</span></span> <span data-ttu-id="76e65-282">Žádné instance jsou nasazeny v provozním prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-282">No instances are deployed in hello production environment.</span></span> 
   
    <span data-ttu-id="76e65-283">Dokončení kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="76e65-283">This completes hello tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="76e65-284">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76e65-284">Next steps</span></span>
* <span data-ttu-id="76e65-285">Prozkoumejte hello dokumentaci [HPC Pack](https://technet.microsoft.com/library/cc514029).</span><span class="sxs-lookup"><span data-stu-id="76e65-285">Explore hello documentation for [HPC Pack](https://technet.microsoft.com/library/cc514029).</span></span>
* <span data-ttu-id="76e65-286">najdete v části tooset až hybridní nasazení clusteru HPC Pack větší škálované [Burst tooAzure instance rolí pracovního procesu pomocí sady Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="76e65-286">tooset up a hybrid HPC Pack cluster deployment at greater scale, see [Burst tooAzure Worker Role Instances with Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
* <span data-ttu-id="76e65-287">Jiné způsoby toocreate clusteru služby HPC Pack v Azure, včetně pomocí šablon Azure Resource Manageru, najdete v tématu [možnosti clusteru HPC pomocí sady Microsoft HPC Pack v Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="76e65-287">For other ways toocreate an HPC Pack cluster in Azure, including using Azure Resource Manager templates, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
