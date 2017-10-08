---
title: "aaaTroubleshoot chyby při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky | Microsoft Docs"
description: "Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky"
services: storage
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 33261562a2dd2614b35bc1118924513f8c624d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a><span data-ttu-id="3eec5-103">Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky</span><span class="sxs-lookup"><span data-stu-id="3eec5-103">Troubleshoot errors when you delete Azure storage accounts, containers, or VHDs</span></span>

<span data-ttu-id="3eec5-104">Taky může docházet k chybám při pokusu toodelete účtu úložiště Azure, kontejner nebo virtuální pevný disk (VHD) v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3eec5-104">You might receive errors when you try toodelete an Azure storage account, container, or virtual hard disk (VHD) in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3eec5-105">Tento článek obsahuje řešení potíží s pokyny toohelp vyřešte hello problém v nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3eec5-105">This article provides troubleshooting guidance toohelp resolve hello problem in an Azure Resource Manager deployment.</span></span>

<span data-ttu-id="3eec5-106">Pokud v tomto článku není vaší Azure potíže vyřešit, navštivte hello fóra Azure na [MSDN a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="3eec5-106">If this article doesn't address your Azure problem, visit hello Azure forums on [MSDN and Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="3eec5-107">Problém můžete zveřejnit na tyto fóra nebo too@AzureSupport na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="3eec5-107">You can post your problem on these forums or too@AzureSupport on Twitter.</span></span> <span data-ttu-id="3eec5-108">Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na hello [podporu Azure](https://azure.microsoft.com/support/options/) lokality.</span><span class="sxs-lookup"><span data-stu-id="3eec5-108">Also, you can file an Azure support request by selecting **Get support** on hello [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="3eec5-109">Příznaky</span><span class="sxs-lookup"><span data-stu-id="3eec5-109">Symptoms</span></span>
### <a name="scenario-1"></a><span data-ttu-id="3eec5-110">Scénář 1</span><span class="sxs-lookup"><span data-stu-id="3eec5-110">Scenario 1</span></span>
<span data-ttu-id="3eec5-111">Když zkusíte toodelete virtuální pevný disk v účtu úložiště v nasazení Resource Manager, zobrazí se hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="3eec5-111">When you try toodelete a VHD in a storage account in a Resource Manager deployment, you receive hello following error message:</span></span>

<span data-ttu-id="3eec5-112">**Nepodařilo toodelete blob 'vhds/BlobName.vhd'. Chyba: Není aktuálně zapůjčení u objektu blob hello a žádné ID zapůjčení byl zadaný v požadavku hello.**</span><span class="sxs-lookup"><span data-stu-id="3eec5-112">**Failed toodelete blob 'vhds/BlobName.vhd'. Error: There is currently a lease on hello blob and no lease ID was specified in hello request.**</span></span>

<span data-ttu-id="3eec5-113">Tento problém může vzniknout, protože virtuální počítač (VM) má zapůjčení na hello pokoušíte toodelete virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3eec5-113">This problem can occur because a virtual machine (VM) has a lease on hello VHD that you are trying toodelete.</span></span>

### <a name="scenario-2"></a><span data-ttu-id="3eec5-114">Scénář 2</span><span class="sxs-lookup"><span data-stu-id="3eec5-114">Scenario 2</span></span>
<span data-ttu-id="3eec5-115">Když zkusíte toodelete kontejneru v účtu úložiště v nasazení Resource Manager, zobrazí se hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="3eec5-115">When you try toodelete a container in a storage account in a Resource Manager deployment, you receive hello following error message:</span></span>

<span data-ttu-id="3eec5-116">**Nepodařilo virtuální pevné disky, toodelete úložiště kontejneru'. Chyba: Není aktuálně k zapůjčení adresy v kontejneru hello a žádné ID zapůjčení byl zadaný v požadavku hello.**</span><span class="sxs-lookup"><span data-stu-id="3eec5-116">**Failed toodelete storage container 'vhds'. Error: There is currently a lease on hello container and no lease ID was specified in hello request.**</span></span>

<span data-ttu-id="3eec5-117">Tento problém může vzniknout, protože má hello kontejneru VHD, který je v stavu zapůjčení hello uzamčena.</span><span class="sxs-lookup"><span data-stu-id="3eec5-117">This problem can occur because hello container has a VHD that is locked in hello lease state.</span></span>

### <a name="scenario-3"></a><span data-ttu-id="3eec5-118">Scénář 3</span><span class="sxs-lookup"><span data-stu-id="3eec5-118">Scenario 3</span></span>
<span data-ttu-id="3eec5-119">Když zkusíte toodelete účet úložiště v nasazení Resource Manager, zobrazí se hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="3eec5-119">When you try toodelete a storage account in a Resource Manager deployment, you receive hello following error message:</span></span>

<span data-ttu-id="3eec5-120">**Účet úložiště toodelete 'StorageAccountName' se nezdařil Chyba: účet úložiště hello nelze odstranit z důvodu tooits artefakty se používají.**</span><span class="sxs-lookup"><span data-stu-id="3eec5-120">**Failed toodelete storage account 'StorageAccountName'. Error: hello storage account cannot be deleted due tooits artifacts being in use.**</span></span>

<span data-ttu-id="3eec5-121">Tento problém může vzniknout, protože účet úložiště hello obsahuje virtuální pevný disk, který je ve stavu zapůjčení hello.</span><span class="sxs-lookup"><span data-stu-id="3eec5-121">This problem can occur because hello storage account contains a VHD that is in hello lease state.</span></span>

## <a name="solution"></a><span data-ttu-id="3eec5-122">Řešení</span><span class="sxs-lookup"><span data-stu-id="3eec5-122">Solution</span></span> 
<span data-ttu-id="3eec5-123">tooresolve tyto problémy musíte určit hello virtuálního pevného disku, který je příčinou chyby hello a hello přidružené virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3eec5-123">tooresolve these problems, you must identify hello VHD that is causing hello error and hello associated VM.</span></span> <span data-ttu-id="3eec5-124">Potom odpojit hello virtuálního pevného disku z hello virtuálního počítače (pro datové disky) nebo odstranění hello virtuální počítač, který používá hello virtuálního pevného disku (pro disky operačního systému).</span><span class="sxs-lookup"><span data-stu-id="3eec5-124">Then, detach hello VHD from hello VM (for data disks) or delete hello VM that is using hello VHD (for OS disks).</span></span> <span data-ttu-id="3eec5-125">Tato zapůjčení hello odebere hello virtuálního pevného disku a povolí jeho toobe odstranit.</span><span class="sxs-lookup"><span data-stu-id="3eec5-125">This removes hello lease from hello VHD and allows it toobe deleted.</span></span> 

<span data-ttu-id="3eec5-126">toodo, použijte některou z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="3eec5-126">toodo this, use one of hello following methods:</span></span>

### <a name="method-1---use-azure-storage-explorer"></a><span data-ttu-id="3eec5-127">Metoda 1 - použijte Azure storage Exploreru</span><span class="sxs-lookup"><span data-stu-id="3eec5-127">Method 1 - Use Azure storage explorer</span></span>

### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a><span data-ttu-id="3eec5-128">Krok 1 identifikace hello virtuálního pevného disku, které zabrání odstranění účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="3eec5-128">Step 1 Identify hello VHD that prevent deletion of hello storage account</span></span>

1. <span data-ttu-id="3eec5-129">Pokud odstraníte účet úložiště hello, zobrazí se dialogové okno zprávy například hello následující:</span><span class="sxs-lookup"><span data-stu-id="3eec5-129">When you delete hello storage account, you will receive a message dialog such as hello following:</span></span> 

    ![zpráva při odstraňování účtu úložiště hello](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="3eec5-131">Zkontrolujte hello **disku URL** účet úložiště hello tooidentify a hello virtuálního pevného disku, který brání odstranit účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="3eec5-131">Check hello **Disk URL** tooidentify hello storage account and hello VHD that prevents you delete hello storage account.</span></span> <span data-ttu-id="3eec5-132">V následujícím příkladu hello, hello řetězec před ". blob.core.windows.net" je název účtu úložiště hello a "SCCM2012-2015-08-28.vhd" je název disku VHD hello.</span><span class="sxs-lookup"><span data-stu-id="3eec5-132">In hello following example, hello string before “.blob.core.windows.net “ is hello storage account name, and "SCCM2012-2015-08-28.vhd" is hello VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-hello-vhd-by-using-azure-storage-explorer"></a><span data-ttu-id="3eec5-133">Krok 2 odstranění hello virtuálního pevného disku pomocí Průzkumníka úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="3eec5-133">Step 2 Delete hello VHD by using Azure Storage Explorer</span></span>

1. <span data-ttu-id="3eec5-134">Stažení a instalace hello nejnovější verzi [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="3eec5-134">Download and Install hello latest version of [Azure Storage Explorer](http://storageexplorer.com/).</span></span> <span data-ttu-id="3eec5-135">Tento nástroj je samostatná aplikace od společnosti Microsoft, který vám umožní tooeasily práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="3eec5-135">This tool is a standalone app from Microsoft that allows you tooeasily work with Azure Storage data on Windows, macOS and Linux.</span></span>
2. <span data-ttu-id="3eec5-136">Otevřete Azure Storage Explorer, vyberte</span><span class="sxs-lookup"><span data-stu-id="3eec5-136">Open Azure Storage Explorer, select</span></span> ![Ikona účtu](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) <span data-ttu-id="3eec5-138">v levém panelu hello vyberte prostředí Azure a pak se přihlaste.</span><span class="sxs-lookup"><span data-stu-id="3eec5-138">on hello left bar, select your Azure environment, and then sign in.</span></span>

3. <span data-ttu-id="3eec5-139">Vyberte všechny odběry nebo hello odběr, který obsahuje hello úložiště účet, že který má toodelete.</span><span class="sxs-lookup"><span data-stu-id="3eec5-139">Select all subscriptions or hello subscription that contains hello storage account you want toodelete.</span></span>

    ![Přidat předplatné](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. <span data-ttu-id="3eec5-141">Přejděte toohello účet úložiště, který jsme získali z hello starší, vyberte disk URL hello **kontejnery objektů Blob** > **virtuální pevné disky** a vyhledejte řetězec hello virtuálního pevného disku, který brání účet úložiště odstranit hello.</span><span class="sxs-lookup"><span data-stu-id="3eec5-141">Go toohello storage account that we obtained from hello disk URL earlier, select hello **Blob Containers** > **vhds** and search for hello VHD that prevents you delete hello storage account.</span></span>
5. <span data-ttu-id="3eec5-142">Pokud je nalezen hello virtuální pevný disk, zkontrolujte hello **název virtuálního počítače** sloupec toofind hello virtuální počítač, který používá tento virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="3eec5-142">If hello VHD is found,  check hello **VM Name** column toofind hello VM that is using this VHD.</span></span>

    ![Zkontrolujte virtuálních počítačů](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. <span data-ttu-id="3eec5-144">Odebrání hello zapůjčení hello virtuální pevný disk pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3eec5-144">Remove hello lease from hello VHD by using Azure portal.</span></span> <span data-ttu-id="3eec5-145">Další informace najdete v tématu [odebrat hello zapůjčení ze hello virtuálního pevného disku](#remove-the-lease-from-the-vhd).</span><span class="sxs-lookup"><span data-stu-id="3eec5-145">For more information, see [Remove hello lease from hello VHD](#remove-the-lease-from-the-vhd).</span></span> 

7. <span data-ttu-id="3eec5-146">Přejděte toohello Azure Storage Explorer, klikněte pravým tlačítkem na hello virtuální pevný disk a potom vyberte možnost odstranit.</span><span class="sxs-lookup"><span data-stu-id="3eec5-146">Go toohello Azure Storage Explorer, right-click hello VHD and then select delete.</span></span>

8. <span data-ttu-id="3eec5-147">Odstraňte účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="3eec5-147">Delete hello storage account.</span></span>

### <a name="method-2---use-azure-portal"></a><span data-ttu-id="3eec5-148">Metoda 2 - používat portál Azure</span><span class="sxs-lookup"><span data-stu-id="3eec5-148">Method 2 - Use Azure portal</span></span> 

#### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a><span data-ttu-id="3eec5-149">Krok 1: Identifikace hello virtuálního pevného disku, které zabrání odstranění účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="3eec5-149">Step 1: Identify hello VHD that prevent deletion of hello storage account</span></span>

1. <span data-ttu-id="3eec5-150">Pokud odstraníte účet úložiště hello, zobrazí se dialogové okno zprávy například hello následující:</span><span class="sxs-lookup"><span data-stu-id="3eec5-150">When you delete hello storage account, you will receive a message dialog such as hello following:</span></span> 

    ![zpráva při odstraňování účtu úložiště hello](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="3eec5-152">Zkontrolujte hello **disku URL** účet úložiště hello tooidentify a hello virtuálního pevného disku, který brání odstranit účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="3eec5-152">Check hello **Disk URL** tooidentify hello storage account and hello VHD that prevents you delete hello storage account.</span></span> <span data-ttu-id="3eec5-153">V následujícím příkladu hello, hello řetězec před ". blob.core.windows.net" je název účtu úložiště hello a "SCCM2012-2015-08-28.vhd" je název disku VHD hello.</span><span class="sxs-lookup"><span data-stu-id="3eec5-153">In hello following example, hello string before “.blob.core.windows.net “ is hello storage account name, and "SCCM2012-2015-08-28.vhd" is hello VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. <span data-ttu-id="3eec5-154">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3eec5-154">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
3. <span data-ttu-id="3eec5-155">V nabídce centra hello vyberte **všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="3eec5-155">On hello Hub menu, select **All resources**.</span></span> <span data-ttu-id="3eec5-156">Přejděte toohello účet úložiště a potom vyberte **objekty BLOB** > **virtuální pevné disky**.</span><span class="sxs-lookup"><span data-stu-id="3eec5-156">Go toohello storage account, and then select **Blobs** > **vhds**.</span></span>

    ![Snímek obrazovky portálu hello s hello účet úložiště a kontejneru "VHD" hello zvýrazněná](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. <span data-ttu-id="3eec5-158">Vyhledejte hello virtuálního pevného disku, který jsme získali dříve z adresy URL hello disku.</span><span class="sxs-lookup"><span data-stu-id="3eec5-158">Locate hello VHD that we obtained from hello disk URL earlier.</span></span> <span data-ttu-id="3eec5-159">Pak určit, které je virtuální počítač pomocí hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3eec5-159">Then, determine which VM is using hello VHD.</span></span> <span data-ttu-id="3eec5-160">Obvykle můžete určit, která obsahuje virtuální počítač hello virtuálního pevného disku kontrolou název hello virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="3eec5-160">Usually, you can determine which VM holds hello VHD by checking name of hello VHD:</span></span>

<span data-ttu-id="3eec5-161">Virtuální počítač v modelu Resource Manager vývoj</span><span class="sxs-lookup"><span data-stu-id="3eec5-161">VM in Resource Manager development  model</span></span>

   * <span data-ttu-id="3eec5-162">Disky operačního systému postupujte podle obecně tyto zásady vytváření názvů: VMName-rrrr-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="3eec5-162">OS disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="3eec5-163">Datové disky obecně postupujte podle zásad vytváření názvů: VMName-rrrr-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="3eec5-163">Data disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

<span data-ttu-id="3eec5-164">Virtuální počítač v klasickém modelu vývoj</span><span class="sxs-lookup"><span data-stu-id="3eec5-164">VM in Classic development model</span></span>

   * <span data-ttu-id="3eec5-165">Disky operačního systému postupujte podle obecně tyto zásady vytváření názvů: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="3eec5-165">OS disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="3eec5-166">Datové disky obecně postupujte podle zásad vytváření názvů: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="3eec5-166">Data disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

#### <a name="step-2-remove-hello-lease-from-hello-vhd"></a><span data-ttu-id="3eec5-167">Krok 2: Odebrání hello zapůjčení hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="3eec5-167">Step 2: Remove hello lease from hello VHD</span></span>

<span data-ttu-id="3eec5-168">[Odebrání hello zapůjčení hello virtuálního pevného disku](#remove-the-lease-from-the-vhd)a potom odstraňte účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="3eec5-168">[Remove hello lease from hello VHD](#remove-the-lease-from-the-vhd), and then delete hello storage account.</span></span>

## <a name="what-is-a-lease"></a><span data-ttu-id="3eec5-169">Co je zapůjčení?</span><span class="sxs-lookup"><span data-stu-id="3eec5-169">What is a lease?</span></span>
<span data-ttu-id="3eec5-170">Zapůjčení je uzamčen, kterou lze použít toocontrol přístup tooa blob (například VHD).</span><span class="sxs-lookup"><span data-stu-id="3eec5-170">A lease is a lock that can be used toocontrol access tooa blob (for example, a VHD).</span></span> <span data-ttu-id="3eec5-171">Když je pronajatý objekt blob, pouze hello vlastníci hello zapůjčení přistupovat k objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="3eec5-171">When a blob is leased, only hello owners of hello lease can access hello blob.</span></span> <span data-ttu-id="3eec5-172">Zapůjčení je důležité pro hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="3eec5-172">A lease is important for hello following reasons:</span></span>

* <span data-ttu-id="3eec5-173">Brání poškození dat, pokud více vlastníky zkuste toowrite toohello stejnou část hello blob v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="3eec5-173">It prevents data corruption if multiple owners try toowrite toohello same portion of hello blob at hello same time.</span></span>
* <span data-ttu-id="3eec5-174">Objekt blob hello zabrání odstraňuje Pokud něco aktivně používá ho (například počítač).</span><span class="sxs-lookup"><span data-stu-id="3eec5-174">It prevents hello blob from being deleted if something is actively using it (for example, a VM).</span></span>
* <span data-ttu-id="3eec5-175">Účet úložiště hello zabrání odstraňuje Pokud něco aktivně používá ho (například počítač).</span><span class="sxs-lookup"><span data-stu-id="3eec5-175">It prevents hello storage account from being deleted if something is actively using it (for example, a VM).</span></span>

### <a name="remove-hello-lease-from-hello-vhd"></a><span data-ttu-id="3eec5-176">Odebrání hello zapůjčení hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="3eec5-176">Remove hello lease from hello VHD</span></span>
<span data-ttu-id="3eec5-177">Pokud hello virtuálního pevného disku je disk s operačním systémem, je nutné odstranit hello virtuálních počítačů tooremove hello zapůjčení:</span><span class="sxs-lookup"><span data-stu-id="3eec5-177">If hello VHD is an OS disk, you must delete hello VM tooremove hello lease:</span></span>

1. <span data-ttu-id="3eec5-178">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3eec5-178">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3eec5-179">Na hello **rozbočovače** nabídce vyberte možnost **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="3eec5-179">On hello **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="3eec5-180">Vyberte hello virtuální počítač, který obsahuje zapůjčení na hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3eec5-180">Select hello VM that holds a lease on hello VHD.</span></span>
4. <span data-ttu-id="3eec5-181">Ujistěte se, že nic aktivně používá hello virtuálního počítače a že jste už potřebovat hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3eec5-181">Make sure that nothing is actively using hello virtual machine, and that you no longer need hello virtual machine.</span></span>
5. <span data-ttu-id="3eec5-182">Hello horní části hello **podrobnosti virtuálního počítače** vyberte **odstranit**a potom klikněte na **Ano** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="3eec5-182">At hello top of hello **VM details** blade, select **Delete**, and then click **Yes** tooconfirm.</span></span>
6. <span data-ttu-id="3eec5-183">měla by být odstraněna Hello virtuálních počítačů, ale uchovávání hello virtuální pevný disk může být.</span><span class="sxs-lookup"><span data-stu-id="3eec5-183">hello VM should be deleted, but hello VHD can be retained.</span></span> <span data-ttu-id="3eec5-184">Ale hello VHD už měli zapůjčení na něm.</span><span class="sxs-lookup"><span data-stu-id="3eec5-184">However, hello VHD should no longer have a lease on it.</span></span> <span data-ttu-id="3eec5-185">To může trvat několik minut, než toobe zapůjčení hello vydané.</span><span class="sxs-lookup"><span data-stu-id="3eec5-185">It may take a few minutes for hello lease toobe released.</span></span> <span data-ttu-id="3eec5-186">vydání tooverify, který hello zapůjčení, přejděte příliš**všechny prostředky** > **název účtu úložiště** > **objekty BLOB**  >  **virtuální pevné disky**.</span><span class="sxs-lookup"><span data-stu-id="3eec5-186">tooverify that hello lease is released, go too**All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="3eec5-187">V hello **Blob vlastnosti** podokně, hello **zapůjčení stav** hodnota by měla být **Odemknutý**.</span><span class="sxs-lookup"><span data-stu-id="3eec5-187">In hello **Blob properties** pane, hello **Lease Status** value should be **Unlocked**.</span></span>

<span data-ttu-id="3eec5-188">Pokud hello virtuálního pevného disku je datový disk, odpojte hello virtuálního pevného disku z hello virtuálních počítačů tooremove hello zapůjčení:</span><span class="sxs-lookup"><span data-stu-id="3eec5-188">If hello VHD is a data disk, detach hello VHD from hello VM tooremove hello lease:</span></span>

1. <span data-ttu-id="3eec5-189">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3eec5-189">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3eec5-190">Na hello **rozbočovače** nabídce vyberte možnost **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="3eec5-190">On hello **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="3eec5-191">Vyberte hello virtuální počítač, který obsahuje zapůjčení na hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3eec5-191">Select hello VM that holds a lease on hello VHD.</span></span>
4. <span data-ttu-id="3eec5-192">Vyberte **disky** na hello **podrobnosti virtuálního počítače** okno.</span><span class="sxs-lookup"><span data-stu-id="3eec5-192">Select **Disks** on hello **VM details** blade.</span></span>
5. <span data-ttu-id="3eec5-193">Vyberte hello datový disk, který obsahuje zapůjčení na hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3eec5-193">Select hello data disk that holds a lease on hello VHD.</span></span> <span data-ttu-id="3eec5-194">Můžete určit, která je připojena virtuálního pevného disku v hello disku kontrolou hello URL hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3eec5-194">You can determine which VHD is attached in hello disk by checking hello URL of hello VHD.</span></span>
6. <span data-ttu-id="3eec5-195">Určení s jistotou určit, že nic aktivně používá hello datový disk.</span><span class="sxs-lookup"><span data-stu-id="3eec5-195">Determine with certainty that nothing is actively using hello data disk.</span></span>
7. <span data-ttu-id="3eec5-196">Klikněte na tlačítko **odpojení** na hello **disku podrobnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="3eec5-196">Click **Detach** on hello **Disk details** blade.</span></span>
8. <span data-ttu-id="3eec5-197">Hello disku by měl nyní odpojit od hello virtuálních počítačů a hello virtuálního pevného disku na něm už měli zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="3eec5-197">hello disk should now be detached from hello VM, and hello VHD should no longer have a lease on it.</span></span> <span data-ttu-id="3eec5-198">To může trvat několik minut, než toobe zapůjčení hello vydané.</span><span class="sxs-lookup"><span data-stu-id="3eec5-198">It may take a few minutes for hello lease toobe released.</span></span> <span data-ttu-id="3eec5-199">byla vydána tooverify, který hello zapůjčení, přejděte příliš**všechny prostředky** > **název účtu úložiště** > **objekty BLOB**  >  **virtuální pevné disky**.</span><span class="sxs-lookup"><span data-stu-id="3eec5-199">tooverify that hello lease has been released, go too**All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="3eec5-200">V hello **Blob vlastnosti** podokně, hello **zapůjčení stav** hodnota by měla být **Odemknutý**.</span><span class="sxs-lookup"><span data-stu-id="3eec5-200">In hello **Blob properties** pane, hello **Lease Status** value should be **Unlocked**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3eec5-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3eec5-201">Next steps</span></span>
* [<span data-ttu-id="3eec5-202">Odstranit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="3eec5-202">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
* [<span data-ttu-id="3eec5-203">Jak toobreak hello uzamčení zapůjčení úložiště objektů blob ve službě Microsoft Azure (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="3eec5-203">How toobreak hello locked lease of blob storage in Microsoft Azure (PowerShell)</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
