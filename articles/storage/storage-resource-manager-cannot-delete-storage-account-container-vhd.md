---
title: "Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky | Microsoft Docs"
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
ms.openlocfilehash: 318d7146ea53a806baf813ff7de2fe91f18becc8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a><span data-ttu-id="dd84b-103">Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky</span><span class="sxs-lookup"><span data-stu-id="dd84b-103">Troubleshoot errors when you delete Azure storage accounts, containers, or VHDs</span></span>

<span data-ttu-id="dd84b-104">Taky může docházet k chybám při pokusu o odstranění účtu úložiště Azure, kontejneru nebo virtuální pevný disk (VHD) v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dd84b-104">You might receive errors when you try to delete an Azure storage account, container, or virtual hard disk (VHD) in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="dd84b-105">Tento článek obsahuje pokyny k odstraňování problémů pomáhající při řešení problému v nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="dd84b-105">This article provides troubleshooting guidance to help resolve the problem in an Azure Resource Manager deployment.</span></span>

<span data-ttu-id="dd84b-106">Pokud v tomto článku není vaší Azure potíže vyřešit, navštivte fórech Azure na [MSDN a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="dd84b-106">If this article doesn't address your Azure problem, visit the Azure forums on [MSDN and Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="dd84b-107">Problém můžete účtovat na tyto fóra nebo na @AzureSupport na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="dd84b-107">You can post your problem on these forums or to @AzureSupport on Twitter.</span></span> <span data-ttu-id="dd84b-108">Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na [podporu Azure](https://azure.microsoft.com/support/options/) lokality.</span><span class="sxs-lookup"><span data-stu-id="dd84b-108">Also, you can file an Azure support request by selecting **Get support** on the [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="dd84b-109">Příznaky</span><span class="sxs-lookup"><span data-stu-id="dd84b-109">Symptoms</span></span>
### <a name="scenario-1"></a><span data-ttu-id="dd84b-110">Scénář 1</span><span class="sxs-lookup"><span data-stu-id="dd84b-110">Scenario 1</span></span>
<span data-ttu-id="dd84b-111">Když se pokusíte odstranit virtuální pevný disk v účtu úložiště v nasazení Resource Manager, zobrazí se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="dd84b-111">When you try to delete a VHD in a storage account in a Resource Manager deployment, you receive the following error message:</span></span>

<span data-ttu-id="dd84b-112">**Nepodařilo se odstranit objekt blob, vhds/BlobName.vhd'. Chyba: Není aktuálně zapůjčení u objektu blob a žádné ID zapůjčení zadaná v žádosti.**</span><span class="sxs-lookup"><span data-stu-id="dd84b-112">**Failed to delete blob 'vhds/BlobName.vhd'. Error: There is currently a lease on the blob and no lease ID was specified in the request.**</span></span>

<span data-ttu-id="dd84b-113">Tento problém může vzniknout, protože virtuální počítač (VM) má zapůjčení na virtuální pevný disk, který se pokoušíte odstranit.</span><span class="sxs-lookup"><span data-stu-id="dd84b-113">This problem can occur because a virtual machine (VM) has a lease on the VHD that you are trying to delete.</span></span>

### <a name="scenario-2"></a><span data-ttu-id="dd84b-114">Scénář 2</span><span class="sxs-lookup"><span data-stu-id="dd84b-114">Scenario 2</span></span>
<span data-ttu-id="dd84b-115">Když se pokusíte odstranit kontejner v účtu úložiště v nasazení Resource Manager, zobrazí se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="dd84b-115">When you try to delete a container in a storage account in a Resource Manager deployment, you receive the following error message:</span></span>

<span data-ttu-id="dd84b-116">**Nepodařilo se odstranit virtuální pevné disky, úložiště kontejneru'. Chyba: Není aktuálně k zapůjčení adresy v kontejneru a žádné ID zapůjčení zadaná v žádosti.**</span><span class="sxs-lookup"><span data-stu-id="dd84b-116">**Failed to delete storage container 'vhds'. Error: There is currently a lease on the container and no lease ID was specified in the request.**</span></span>

<span data-ttu-id="dd84b-117">Tento problém může vzniknout, protože má kontejneru VHD, který je uzamčen ve stavu zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="dd84b-117">This problem can occur because the container has a VHD that is locked in the lease state.</span></span>

### <a name="scenario-3"></a><span data-ttu-id="dd84b-118">Scénář 3</span><span class="sxs-lookup"><span data-stu-id="dd84b-118">Scenario 3</span></span>
<span data-ttu-id="dd84b-119">Když se pokusíte odstranit účet úložiště v nasazení Resource Manager, zobrazí se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="dd84b-119">When you try to delete a storage account in a Resource Manager deployment, you receive the following error message:</span></span>

<span data-ttu-id="dd84b-120">**Odstranění účtu úložiště 'StorageAccountName' se nezdařilo. Chyba: Účet úložiště nelze odstranit, protože některé jeho artefakty se používají.**</span><span class="sxs-lookup"><span data-stu-id="dd84b-120">**Failed to delete storage account 'StorageAccountName'. Error: The storage account cannot be deleted due to its artifacts being in use.**</span></span>

<span data-ttu-id="dd84b-121">Tento problém může vzniknout, protože účet úložiště obsahuje virtuální pevný disk, který je ve stavu zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="dd84b-121">This problem can occur because the storage account contains a VHD that is in the lease state.</span></span>

## <a name="solution"></a><span data-ttu-id="dd84b-122">Řešení</span><span class="sxs-lookup"><span data-stu-id="dd84b-122">Solution</span></span> 
<span data-ttu-id="dd84b-123">Chcete-li tyto problémy vyřešit, musíte určit virtuální pevný disk, který je příčinou chyby a přidružený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="dd84b-123">To resolve these problems, you must identify the VHD that is causing the error and the associated VM.</span></span> <span data-ttu-id="dd84b-124">Potom odpojte virtuální pevný disk z virtuálního počítače (pro datové disky) nebo odstranění virtuálního počítače, který je pomocí virtuálního pevného disku (pro disky operačního systému).</span><span class="sxs-lookup"><span data-stu-id="dd84b-124">Then, detach the VHD from the VM (for data disks) or delete the VM that is using the VHD (for OS disks).</span></span> <span data-ttu-id="dd84b-125">Tato zapůjčení odebere z virtuálního pevného disku a umožňuje pro odstranění.</span><span class="sxs-lookup"><span data-stu-id="dd84b-125">This removes the lease from the VHD and allows it to be deleted.</span></span> 

<span data-ttu-id="dd84b-126">K tomuto účelu použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="dd84b-126">To do this, use one of the following methods:</span></span>

### <a name="method-1---use-azure-storage-explorer"></a><span data-ttu-id="dd84b-127">Metoda 1 - použijte Azure storage Exploreru</span><span class="sxs-lookup"><span data-stu-id="dd84b-127">Method 1 - Use Azure storage explorer</span></span>

### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a><span data-ttu-id="dd84b-128">Krok 1 identifikace virtuálního pevného disku, které zabrání odstranění účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="dd84b-128">Step 1 Identify the VHD that prevent deletion of the storage account</span></span>

1. <span data-ttu-id="dd84b-129">Pokud odstraníte účet úložiště, zobrazí se dialogové okno zprávy například následující:</span><span class="sxs-lookup"><span data-stu-id="dd84b-129">When you delete the storage account, you will receive a message dialog such as the following:</span></span> 

    ![Při odstranění účtu úložiště se zobrazí zpráva](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="dd84b-131">Zkontrolujte **disku URL** k identifikaci úložiště k účtu a virtuální pevný disk, který brání odstranění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd84b-131">Check the **Disk URL** to identify the storage account and the VHD that prevents you delete the storage account.</span></span> <span data-ttu-id="dd84b-132">V následujícím příkladu, řetězec před ". blob.core.windows.net" je název účtu úložiště a "SCCM2012-2015-08-28.vhd" je název disku VHD.</span><span class="sxs-lookup"><span data-stu-id="dd84b-132">In the following example, the string before “.blob.core.windows.net “ is the storage account name, and "SCCM2012-2015-08-28.vhd" is the VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-the-vhd-by-using-azure-storage-explorer"></a><span data-ttu-id="dd84b-133">Krok 2 odstranit virtuální pevný disk pomocí Průzkumníka úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="dd84b-133">Step 2 Delete the VHD by using Azure Storage Explorer</span></span>

1. <span data-ttu-id="dd84b-134">Stáhněte a nainstalujte nejnovější verzi [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="dd84b-134">Download and Install the latest version of [Azure Storage Explorer](http://storageexplorer.com/).</span></span> <span data-ttu-id="dd84b-135">Tento nástroj je samostatná aplikace od společnosti Microsoft, která umožňuje snadno pracovat s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="dd84b-135">This tool is a standalone app from Microsoft that allows you to easily work with Azure Storage data on Windows, macOS and Linux.</span></span>
2. <span data-ttu-id="dd84b-136">Otevřete Azure Storage Explorer, vyberte</span><span class="sxs-lookup"><span data-stu-id="dd84b-136">Open Azure Storage Explorer, select</span></span> ![Ikona účtu](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) <span data-ttu-id="dd84b-138">v levém panelu vyberte prostředí Azure a pak se přihlaste.</span><span class="sxs-lookup"><span data-stu-id="dd84b-138">on the left bar, select your Azure environment, and then sign in.</span></span>

3. <span data-ttu-id="dd84b-139">Vyberte všechny odběry nebo odběr, který obsahuje účet úložiště, které chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="dd84b-139">Select all subscriptions or the subscription that contains the storage account you want to delete.</span></span>

    ![Přidat předplatné](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. <span data-ttu-id="dd84b-141">Přejděte na účet úložiště, který jsme získali z adresy URL disku starší, vyberte **kontejnery objektů Blob** > **virtuální pevné disky** a vyhledávání pro virtuální pevný disk, který brání odstranění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd84b-141">Go to the storage account that we obtained from the disk URL earlier, select the **Blob Containers** > **vhds** and search for the VHD that prevents you delete the storage account.</span></span>
5. <span data-ttu-id="dd84b-142">Pokud se najde virtuální pevný disk, zkontrolujte **název virtuálního počítače** sloupec, který se najít virtuální počítač, který používá tento virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="dd84b-142">If the VHD is found,  check the **VM Name** column to find the VM that is using this VHD.</span></span>

    ![Zkontrolujte virtuálních počítačů](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. <span data-ttu-id="dd84b-144">Odebrání zapůjčení virtuální pevný disk pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dd84b-144">Remove the lease from the VHD by using Azure portal.</span></span> <span data-ttu-id="dd84b-145">Další informace najdete v tématu [zapůjčení odebrání virtuálního pevného disku](#remove-the-lease-from-the-vhd).</span><span class="sxs-lookup"><span data-stu-id="dd84b-145">For more information, see [Remove the lease from the VHD](#remove-the-lease-from-the-vhd).</span></span> 

7. <span data-ttu-id="dd84b-146">Přejděte do Azure Storage Explorer, klikněte pravým tlačítkem na virtuální pevný disk a potom vyberte možnost odstranit.</span><span class="sxs-lookup"><span data-stu-id="dd84b-146">Go to the Azure Storage Explorer, right-click the VHD and then select delete.</span></span>

8. <span data-ttu-id="dd84b-147">Umožňuje odstranit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd84b-147">Delete the storage account.</span></span>

### <a name="method-2---use-azure-portal"></a><span data-ttu-id="dd84b-148">Metoda 2 - používat portál Azure</span><span class="sxs-lookup"><span data-stu-id="dd84b-148">Method 2 - Use Azure portal</span></span> 

#### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a><span data-ttu-id="dd84b-149">Krok 1: Identifikace virtuálního pevného disku, které zabrání odstranění účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="dd84b-149">Step 1: Identify the VHD that prevent deletion of the storage account</span></span>

1. <span data-ttu-id="dd84b-150">Pokud odstraníte účet úložiště, zobrazí se dialogové okno zprávy například následující:</span><span class="sxs-lookup"><span data-stu-id="dd84b-150">When you delete the storage account, you will receive a message dialog such as the following:</span></span> 

    ![Při odstranění účtu úložiště se zobrazí zpráva](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="dd84b-152">Zkontrolujte **disku URL** k identifikaci úložiště k účtu a virtuální pevný disk, který brání odstranění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd84b-152">Check the **Disk URL** to identify the storage account and the VHD that prevents you delete the storage account.</span></span> <span data-ttu-id="dd84b-153">V následujícím příkladu, řetězec před ". blob.core.windows.net" je název účtu úložiště a "SCCM2012-2015-08-28.vhd" je název disku VHD.</span><span class="sxs-lookup"><span data-stu-id="dd84b-153">In the following example, the string before “.blob.core.windows.net “ is the storage account name, and "SCCM2012-2015-08-28.vhd" is the VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. <span data-ttu-id="dd84b-154">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dd84b-154">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
3. <span data-ttu-id="dd84b-155">V nabídce centra vyberte **všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="dd84b-155">On the Hub menu, select **All resources**.</span></span> <span data-ttu-id="dd84b-156">Přejděte k účtu úložiště a pak vyberte **objekty BLOB** > **virtuální pevné disky**.</span><span class="sxs-lookup"><span data-stu-id="dd84b-156">Go to the storage account, and then select **Blobs** > **vhds**.</span></span>

    ![Snímek obrazovky portálu, se účet úložiště a kontejneru "VHD" zvýrazněná](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. <span data-ttu-id="dd84b-158">Najděte virtuální pevný disk, který jsme získali dříve z adresy URL disku.</span><span class="sxs-lookup"><span data-stu-id="dd84b-158">Locate the VHD that we obtained from the disk URL earlier.</span></span> <span data-ttu-id="dd84b-159">Pak určit, které je virtuální počítač pomocí virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="dd84b-159">Then, determine which VM is using the VHD.</span></span> <span data-ttu-id="dd84b-160">Obvykle můžete určit, která virtuální počítač obsahuje virtuální pevný disk kontrolou název virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="dd84b-160">Usually, you can determine which VM holds the VHD by checking name of the VHD:</span></span>

<span data-ttu-id="dd84b-161">Virtuální počítač v modelu Resource Manager vývoj</span><span class="sxs-lookup"><span data-stu-id="dd84b-161">VM in Resource Manager development  model</span></span>

   * <span data-ttu-id="dd84b-162">Disky operačního systému postupujte podle obecně tyto zásady vytváření názvů: VMName-rrrr-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="dd84b-162">OS disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="dd84b-163">Datové disky obecně postupujte podle zásad vytváření názvů: VMName-rrrr-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="dd84b-163">Data disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

<span data-ttu-id="dd84b-164">Virtuální počítač v klasickém modelu vývoj</span><span class="sxs-lookup"><span data-stu-id="dd84b-164">VM in Classic development model</span></span>

   * <span data-ttu-id="dd84b-165">Disky operačního systému postupujte podle obecně tyto zásady vytváření názvů: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="dd84b-165">OS disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="dd84b-166">Datové disky obecně postupujte podle zásad vytváření názvů: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="dd84b-166">Data disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

#### <a name="step-2-remove-the-lease-from-the-vhd"></a><span data-ttu-id="dd84b-167">Krok 2: Odeberte zapůjčení z virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="dd84b-167">Step 2: Remove the lease from the VHD</span></span>

<span data-ttu-id="dd84b-168">[Odebrání virtuálního pevného disku zapůjčení](#remove-the-lease-from-the-vhd)a potom odstraňte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd84b-168">[Remove the lease from the VHD](#remove-the-lease-from-the-vhd), and then delete the storage account.</span></span>

## <a name="what-is-a-lease"></a><span data-ttu-id="dd84b-169">Co je zapůjčení?</span><span class="sxs-lookup"><span data-stu-id="dd84b-169">What is a lease?</span></span>
<span data-ttu-id="dd84b-170">Zapůjčení je uzamčen, který můžete použít k řízení přístupu do objektu blob (například VHD).</span><span class="sxs-lookup"><span data-stu-id="dd84b-170">A lease is a lock that can be used to control access to a blob (for example, a VHD).</span></span> <span data-ttu-id="dd84b-171">Když je pronajatý objekt blob, jenom vlastníci zapůjčení přistupovat k objektu blob.</span><span class="sxs-lookup"><span data-stu-id="dd84b-171">When a blob is leased, only the owners of the lease can access the blob.</span></span> <span data-ttu-id="dd84b-172">Zapůjčení je důležité z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="dd84b-172">A lease is important for the following reasons:</span></span>

* <span data-ttu-id="dd84b-173">Zabrání poškození dat pokud více vlastníky zkuste k zápisu do stejnou část objektu blob ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="dd84b-173">It prevents data corruption if multiple owners try to write to the same portion of the blob at the same time.</span></span>
* <span data-ttu-id="dd84b-174">Objekt blob zabrání odstraňuje Pokud něco aktivně používá ho (například počítač).</span><span class="sxs-lookup"><span data-stu-id="dd84b-174">It prevents the blob from being deleted if something is actively using it (for example, a VM).</span></span>
* <span data-ttu-id="dd84b-175">Účet úložiště zabrání odstraňuje Pokud něco aktivně používá ho (například počítač).</span><span class="sxs-lookup"><span data-stu-id="dd84b-175">It prevents the storage account from being deleted if something is actively using it (for example, a VM).</span></span>

### <a name="remove-the-lease-from-the-vhd"></a><span data-ttu-id="dd84b-176">Odeberte zapůjčení z virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="dd84b-176">Remove the lease from the VHD</span></span>
<span data-ttu-id="dd84b-177">Pokud virtuální pevný disk je disk s operačním systémem, je nutné odstranit virtuální počítač odebrat zapůjčení:</span><span class="sxs-lookup"><span data-stu-id="dd84b-177">If the VHD is an OS disk, you must delete the VM to remove the lease:</span></span>

1. <span data-ttu-id="dd84b-178">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dd84b-178">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="dd84b-179">Na **rozbočovače** nabídce vyberte možnost **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="dd84b-179">On the **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="dd84b-180">Vyberte virtuální počítač, který obsahuje zapůjčení na virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="dd84b-180">Select the VM that holds a lease on the VHD.</span></span>
4. <span data-ttu-id="dd84b-181">Ujistěte se, že nic aktivně používá virtuální počítač a virtuální počítač už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="dd84b-181">Make sure that nothing is actively using the virtual machine, and that you no longer need the virtual machine.</span></span>
5. <span data-ttu-id="dd84b-182">V horní části **podrobnosti virtuálního počítače** vyberte **odstranit**a potom klikněte na **Ano** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="dd84b-182">At the top of the **VM details** blade, select **Delete**, and then click **Yes** to confirm.</span></span>
6. <span data-ttu-id="dd84b-183">Virtuální počítač by měl být odstraněn, ale uchovávání může být virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="dd84b-183">The VM should be deleted, but the VHD can be retained.</span></span> <span data-ttu-id="dd84b-184">Ale VHD už měli zapůjčení na něm.</span><span class="sxs-lookup"><span data-stu-id="dd84b-184">However, the VHD should no longer have a lease on it.</span></span> <span data-ttu-id="dd84b-185">To může trvat několik minut, než zapůjčení k uvolnění.</span><span class="sxs-lookup"><span data-stu-id="dd84b-185">It may take a few minutes for the lease to be released.</span></span> <span data-ttu-id="dd84b-186">Chcete-li ověřit, že zapůjčení vydání, přejděte na **všechny prostředky** > **název účtu úložiště** > **objekty BLOB** > **virtuální pevné disky**.</span><span class="sxs-lookup"><span data-stu-id="dd84b-186">To verify that the lease is released, go to **All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="dd84b-187">V **Blob vlastnosti** podokně **zapůjčení stav** hodnota by měla být **Odemknutý**.</span><span class="sxs-lookup"><span data-stu-id="dd84b-187">In the **Blob properties** pane, the **Lease Status** value should be **Unlocked**.</span></span>

<span data-ttu-id="dd84b-188">Pokud virtuální pevný disk je datový disk, odpojte virtuální pevný disk z virtuálního počítače odebrat zapůjčení:</span><span class="sxs-lookup"><span data-stu-id="dd84b-188">If the VHD is a data disk, detach the VHD from the VM to remove the lease:</span></span>

1. <span data-ttu-id="dd84b-189">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dd84b-189">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="dd84b-190">Na **rozbočovače** nabídce vyberte možnost **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="dd84b-190">On the **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="dd84b-191">Vyberte virtuální počítač, který obsahuje zapůjčení na virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="dd84b-191">Select the VM that holds a lease on the VHD.</span></span>
4. <span data-ttu-id="dd84b-192">Vyberte **disky** na **podrobnosti virtuálního počítače** okno.</span><span class="sxs-lookup"><span data-stu-id="dd84b-192">Select **Disks** on the **VM details** blade.</span></span>
5. <span data-ttu-id="dd84b-193">Vyberte datový disk, který obsahuje zapůjčení na virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="dd84b-193">Select the data disk that holds a lease on the VHD.</span></span> <span data-ttu-id="dd84b-194">Můžete určit, která je připojena virtuálního pevného disku v disku kontrolou adresu URL virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="dd84b-194">You can determine which VHD is attached in the disk by checking the URL of the VHD.</span></span>
6. <span data-ttu-id="dd84b-195">Určení s jistotou určit, že nic aktivně používá datový disk.</span><span class="sxs-lookup"><span data-stu-id="dd84b-195">Determine with certainty that nothing is actively using the data disk.</span></span>
7. <span data-ttu-id="dd84b-196">Klikněte na tlačítko **odpojení** na **disku podrobnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="dd84b-196">Click **Detach** on the **Disk details** blade.</span></span>
8. <span data-ttu-id="dd84b-197">Z virtuálního počítače, by měl nyní odpojit disk a virtuální pevný disk na něm už měli zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="dd84b-197">The disk should now be detached from the VM, and the VHD should no longer have a lease on it.</span></span> <span data-ttu-id="dd84b-198">To může trvat několik minut, než zapůjčení k uvolnění.</span><span class="sxs-lookup"><span data-stu-id="dd84b-198">It may take a few minutes for the lease to be released.</span></span> <span data-ttu-id="dd84b-199">Chcete-li ověřit, že byla vydána zapůjčení, přejděte na **všechny prostředky** > **název účtu úložiště** > **objekty BLOB** > **virtuální pevné disky**.</span><span class="sxs-lookup"><span data-stu-id="dd84b-199">To verify that the lease has been released, go to **All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="dd84b-200">V **Blob vlastnosti** podokně **zapůjčení stav** hodnota by měla být **Odemknutý**.</span><span class="sxs-lookup"><span data-stu-id="dd84b-200">In the **Blob properties** pane, the **Lease Status** value should be **Unlocked**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd84b-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd84b-201">Next steps</span></span>
* [<span data-ttu-id="dd84b-202">Odstranit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="dd84b-202">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
* [<span data-ttu-id="dd84b-203">K ukončení uzamčení zapůjčení úložiště objektů blob ve službě Microsoft Azure (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="dd84b-203">How to break the locked lease of blob storage in Microsoft Azure (PowerShell)</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
