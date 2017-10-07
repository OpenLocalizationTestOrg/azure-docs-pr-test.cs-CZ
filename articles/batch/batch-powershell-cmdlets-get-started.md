---
title: "aaaGet spuštění pomocí prostředí PowerShell pro Azure Batch | Microsoft Docs"
description: "Rychlý úvod toohello toomanage prostředky Batch můžete použít rutiny prostředí Azure PowerShell."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: f9ad62c5-27bf-4e6b-a5bf-c5f5914e6199
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e4d12e9c1e52a5b2db2dd44346edda93b7ef92b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a><span data-ttu-id="ae91c-103">Správa prostředků služby Batch pomocí rutin PowerShellu</span><span class="sxs-lookup"><span data-stu-id="ae91c-103">Manage Batch resources with PowerShell cmdlets</span></span>

<span data-ttu-id="ae91c-104">S hello rutiny prostředí PowerShell Azure Batch, můžete provést a skriptu řadu hello stejné úlohy, které se provádějí pomocí rozhraní API služby Batch, hello hello portál Azure a hello rozhraní příkazového řádku Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="ae91c-104">With hello Azure Batch PowerShell cmdlets, you can perform and script many of hello same tasks you carry out with hello Batch APIs, hello Azure portal, and hello Azure Command-Line Interface (CLI).</span></span> <span data-ttu-id="ae91c-105">Toto je toohello rutiny rychlý úvod, můžete použít toomanage účty Batch a pracovat s prostředky služby Batch, například fondy, úlohy a úkoly.</span><span class="sxs-lookup"><span data-stu-id="ae91c-105">This is a quick introduction toohello cmdlets you can use toomanage your Batch accounts and work with your Batch resources such as pools, jobs, and tasks.</span></span>

<span data-ttu-id="ae91c-106">Úplný seznam rutin prostředí Batch a podrobný popis syntaxe rutin najdete v části hello [rutiny Azure Batch – reference](/powershell/module/azurerm.batch/#batch).</span><span class="sxs-lookup"><span data-stu-id="ae91c-106">For a complete list of Batch cmdlets and detailed cmdlet syntax, see hello [Azure Batch cmdlet reference](/powershell/module/azurerm.batch/#batch).</span></span>

<span data-ttu-id="ae91c-107">Tento článek vychází z rutin prostředí Azure PowerShell verze 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="ae91c-107">This article is based on cmdlets in Azure PowerShell version 3.0.0.</span></span> <span data-ttu-id="ae91c-108">Doporučujeme aktualizovat prostředí Azure PowerShell často tootake výhod aktualizace a vylepšení služby.</span><span class="sxs-lookup"><span data-stu-id="ae91c-108">We recommend that you update your Azure PowerShell frequently tootake advantage of service updates and enhancements.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae91c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ae91c-109">Prerequisites</span></span>
<span data-ttu-id="ae91c-110">Proveďte následující operace toouse prostředí Azure PowerShell toomanage hello prostředky služby Batch.</span><span class="sxs-lookup"><span data-stu-id="ae91c-110">Perform hello following operations toouse Azure PowerShell toomanage your Batch resources.</span></span>

* [<span data-ttu-id="ae91c-111">Nainstalujte a nakonfigurujte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ae91c-111">Install and configure Azure PowerShell</span></span>](/powershell/azure/overview)
* <span data-ttu-id="ae91c-112">Spustit hello **Login-AzureRmAccount** rutiny tooconnect tooyour předplatné (hello Azure Batch lodě rutiny v modulu Azure Resource Manager hello):</span><span class="sxs-lookup"><span data-stu-id="ae91c-112">Run hello **Login-AzureRmAccount** cmdlet tooconnect tooyour subscription (hello Azure Batch cmdlets ship in hello Azure Resource Manager module):</span></span>
  
    `Login-AzureRmAccount`
* <span data-ttu-id="ae91c-113">**Zaregistrovat se obor názvů zprostředkovatele Batch hello**.</span><span class="sxs-lookup"><span data-stu-id="ae91c-113">**Register with hello Batch provider namespace**.</span></span> <span data-ttu-id="ae91c-114">Tuto operaci, stačí provést toobe **jednou za předplatné**.</span><span class="sxs-lookup"><span data-stu-id="ae91c-114">This operation only needs toobe performed **once per subscription**.</span></span>
  
    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a><span data-ttu-id="ae91c-115">Správa účtů a klíčů služby Batch</span><span class="sxs-lookup"><span data-stu-id="ae91c-115">Manage Batch accounts and keys</span></span>
### <a name="create-a-batch-account"></a><span data-ttu-id="ae91c-116">Vytvoření účtu Batch</span><span class="sxs-lookup"><span data-stu-id="ae91c-116">Create a Batch account</span></span>
<span data-ttu-id="ae91c-117">Rutina **New-AzureRmBatchAccount** vytvoří v zadané skupině prostředků účet služby Batch.</span><span class="sxs-lookup"><span data-stu-id="ae91c-117">**New-AzureRmBatchAccount** creates a Batch account in a specified resource group.</span></span> <span data-ttu-id="ae91c-118">Pokud ještě nemáte skupinu prostředků, vytvořte ji spuštěním hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ae91c-118">If you don't already have a resource group, create one by running hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span> <span data-ttu-id="ae91c-119">Zadejte jednu z hello Azure oblasti v hello **umístění** parametru, jako je například "Střed USA".</span><span class="sxs-lookup"><span data-stu-id="ae91c-119">Specify one of hello Azure regions in hello **Location** parameter, such as "Central US".</span></span> <span data-ttu-id="ae91c-120">Například:</span><span class="sxs-lookup"><span data-stu-id="ae91c-120">For example:</span></span>

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

<span data-ttu-id="ae91c-121">Pak vytvořte dávkový účet ve skupině prostředků hello, zadejte název pro účet hello v <*account_name*> a hello umístění a název vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="ae91c-121">Then, create a Batch account in hello resource group, specifying a name for hello account in <*account_name*> and hello location and name of your resource group.</span></span> <span data-ttu-id="ae91c-122">Vytvoření účtu Batch hello může trvat některé toocomplete čas.</span><span class="sxs-lookup"><span data-stu-id="ae91c-122">Creating hello Batch account can take some time toocomplete.</span></span> <span data-ttu-id="ae91c-123">Například:</span><span class="sxs-lookup"><span data-stu-id="ae91c-123">For example:</span></span>

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [!NOTE]
> <span data-ttu-id="ae91c-124">účet Batch Hello název musí být jedinečný toohello oblast Azure pro skupinu prostředků hello, obsahovat 3 až 24 znaků a používat jenom malá písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="ae91c-124">hello Batch account name must be unique toohello Azure region for hello resource group, contain between 3 and 24 characters, and use lowercase letters and numbers only.</span></span>
> 
> 

### <a name="get-account-access-keys"></a><span data-ttu-id="ae91c-125">Získání přístupových klíčů k účtu</span><span class="sxs-lookup"><span data-stu-id="ae91c-125">Get account access keys</span></span>
<span data-ttu-id="ae91c-126">**Get-AzureRmBatchAccountKeys** ukazuje hello přístupové klíče asociované s účtem Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="ae91c-126">**Get-AzureRmBatchAccountKeys** shows hello access keys associated with an Azure Batch account.</span></span> <span data-ttu-id="ae91c-127">Například spusťte hello následující tooget hello primární a sekundární klíče hello účtu, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ae91c-127">For example, run hello following tooget hello primary and secondary keys of hello account you created.</span></span>

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a><span data-ttu-id="ae91c-128">Vygenerování nového přístupového klíče</span><span class="sxs-lookup"><span data-stu-id="ae91c-128">Generate a new access key</span></span>
<span data-ttu-id="ae91c-129">**New-AzureRmBatchAccountKey** vygeneruje nový primární nebo sekundární přístupový klíč pro účet Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="ae91c-129">**New-AzureRmBatchAccountKey** generates a new primary or secondary account key for an Azure Batch account.</span></span> <span data-ttu-id="ae91c-130">Například toogenerate nový primární klíč pro dávkový účet, zadejte:</span><span class="sxs-lookup"><span data-stu-id="ae91c-130">For example, toogenerate a new primary key for your Batch account, type:</span></span>

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [!NOTE]
> <span data-ttu-id="ae91c-131">toogenerate nový sekundární klíč, zadejte "Sekundární" hello **KeyType** parametr.</span><span class="sxs-lookup"><span data-stu-id="ae91c-131">toogenerate a new secondary key, specify "Secondary" for hello **KeyType** parameter.</span></span> <span data-ttu-id="ae91c-132">Máte tooregenerate hello primární a sekundární klíče samostatně.</span><span class="sxs-lookup"><span data-stu-id="ae91c-132">You have tooregenerate hello primary and secondary keys separately.</span></span>
> 
> 

### <a name="delete-a-batch-account"></a><span data-ttu-id="ae91c-133">Odstranění účtu Batch</span><span class="sxs-lookup"><span data-stu-id="ae91c-133">Delete a Batch account</span></span>
<span data-ttu-id="ae91c-134">**Remove-AzureRmBatchAccount** odstraní účet Batch.</span><span class="sxs-lookup"><span data-stu-id="ae91c-134">**Remove-AzureRmBatchAccount** deletes a Batch account.</span></span> <span data-ttu-id="ae91c-135">Například:</span><span class="sxs-lookup"><span data-stu-id="ae91c-135">For example:</span></span>

    Remove-AzureRmBatchAccount -AccountName <account_name>

<span data-ttu-id="ae91c-136">Po zobrazení výzvy potvrďte, že chcete účet tooremove hello.</span><span class="sxs-lookup"><span data-stu-id="ae91c-136">When prompted, confirm you want tooremove hello account.</span></span> <span data-ttu-id="ae91c-137">Odebrání účtu může trvat některé toocomplete čas.</span><span class="sxs-lookup"><span data-stu-id="ae91c-137">Account removal can take some time toocomplete.</span></span>

## <a name="create-a-batchaccountcontext-object"></a><span data-ttu-id="ae91c-138">Vytvoření objektu BatchAccountContext</span><span class="sxs-lookup"><span data-stu-id="ae91c-138">Create a BatchAccountContext object</span></span>
<span data-ttu-id="ae91c-139">rutiny prostředí PowerShell Batch pomocí tooauthenticate hello, při vytváření a správě Batch fondy, úlohy a úlohy, a další prostředky, nejprve vytvořit toostore objekt BatchAccountContext, název účtu a klíče:</span><span class="sxs-lookup"><span data-stu-id="ae91c-139">tooauthenticate using hello Batch PowerShell cmdlets when you create and manage Batch pools, jobs, tasks, and other resources, first create a BatchAccountContext object toostore your account name and keys:</span></span>

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

<span data-ttu-id="ae91c-140">Předání objektu BatchAccountContext hello do rutin tohoto použití hello **BatchContext** parametr.</span><span class="sxs-lookup"><span data-stu-id="ae91c-140">You pass hello BatchAccountContext object into cmdlets that use hello **BatchContext** parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="ae91c-141">Ve výchozím nastavení, primární klíč účtu hello slouží k ověřování, ale můžete explicitně vybrat klíče toouse hello změnou objekt BatchAccountContext **KeyInUse** vlastnost: `$context.KeyInUse = "Secondary"`.</span><span class="sxs-lookup"><span data-stu-id="ae91c-141">By default, hello account's primary key is used for authentication, but you can explicitly select hello key toouse by changing your BatchAccountContext object’s **KeyInUse** property: `$context.KeyInUse = "Secondary"`.</span></span>
> 
> 

## <a name="create-and-modify-batch-resources"></a><span data-ttu-id="ae91c-142">Vytváření a úpravy prostředků služby Batch</span><span class="sxs-lookup"><span data-stu-id="ae91c-142">Create and modify Batch resources</span></span>
<span data-ttu-id="ae91c-143">Pomocí rutin, jako například **New-AzureBatchPool**, **New-AzureBatchJob**, a **New-AzureBatchTask** toocreate prostředky v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="ae91c-143">Use cmdlets such as **New-AzureBatchPool**, **New-AzureBatchJob**, and **New-AzureBatchTask** toocreate resources under a Batch account.</span></span> <span data-ttu-id="ae91c-144">Existují odpovídající **Get -** a **Set -** rutiny tooupdate hello vlastnosti existujících prostředků a **Remove -** rutiny tooremove prostředky v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="ae91c-144">There are corresponding **Get-** and **Set-** cmdlets tooupdate hello properties of existing resources, and  **Remove-** cmdlets tooremove resources under a Batch account.</span></span>

<span data-ttu-id="ae91c-145">Při použití řadu tyto rutiny v přidání toopassing objekt BatchContext, třeba toocreate nebo předejte objekty, které obsahují nastavení podrobné prostředků, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="ae91c-145">When using many of these cmdlets, in addition toopassing a BatchContext object, you need toocreate or pass objects that contain detailed resource settings, as shown in hello following example.</span></span> <span data-ttu-id="ae91c-146">V tématu hello podrobnou nápovědu pro všechny rutiny pro další příklady.</span><span class="sxs-lookup"><span data-stu-id="ae91c-146">See hello detailed help for each cmdlet for additional examples.</span></span>

### <a name="create-a-batch-pool"></a><span data-ttu-id="ae91c-147">Vytvoření fondu služby Batch</span><span class="sxs-lookup"><span data-stu-id="ae91c-147">Create a Batch pool</span></span>
<span data-ttu-id="ae91c-148">Při vytváření nebo aktualizaci fondu služby Batch, vyberete konfigurace hello cloudové služby nebo konfigurace virtuálního počítače hello pro hello operační systém na hello výpočetních uzlů (viz [přehled funkcí Batch](batch-api-basics.md#pool)).</span><span class="sxs-lookup"><span data-stu-id="ae91c-148">When creating or updating a Batch pool, you select either hello cloud service configuration or hello virtual machine configuration for hello operating system on hello compute nodes (see [Batch feature overview](batch-api-basics.md#pool)).</span></span> <span data-ttu-id="ae91c-149">Pokud zadáte konfigurace hello cloudové služby, výpočetní uzly se vytvoří jeho bitová kopie s jedním z hello [uvolní Azure hostovaného operačního systému](../cloud-services/cloud-services-guestos-update-matrix.md#releases).</span><span class="sxs-lookup"><span data-stu-id="ae91c-149">If you specify hello cloud service configuration, your compute nodes are imaged with one of hello [Azure Guest OS releases](../cloud-services/cloud-services-guestos-update-matrix.md#releases).</span></span> <span data-ttu-id="ae91c-150">Pokud zadáte hello konfigurace virtuálního počítače, můžete zadat jednu z hello nepodporuje Linux nebo Image virtuálního počítače s Windows uvedených v hello [Marketplace virtuálních počítačů Azure][vm_marketplace], nebo zadejte vlastní obrázek, který jste připravili.</span><span class="sxs-lookup"><span data-stu-id="ae91c-150">If you specify hello virtual machine configuration, you can either specify one of hello supported Linux or Windows VM images listed in hello [Azure Virtual Machines Marketplace][vm_marketplace], or provide a custom image that you have prepared.</span></span>

<span data-ttu-id="ae91c-151">Při spuštění **New-AzureBatchPool**, předat objekt PSCloudServiceConfiguration nebo PSVirtualMachineConfiguration hello nastavení operačního systému.</span><span class="sxs-lookup"><span data-stu-id="ae91c-151">When you run **New-AzureBatchPool**, pass hello operating system settings in a PSCloudServiceConfiguration or PSVirtualMachineConfiguration object.</span></span> <span data-ttu-id="ae91c-152">Například hello následující rutina vytvoří nový fond služby Batch s velikost malých výpočetní uzly v konfiguraci služby hello cloudu s obrázky hello nejnovější verzi operačního systému rodiny 3 (Windows Server 2012).</span><span class="sxs-lookup"><span data-stu-id="ae91c-152">For example, hello following cmdlet creates a new Batch pool with size Small compute nodes in hello cloud service configuration, imaged with hello latest operating system version of family 3 (Windows Server 2012).</span></span> <span data-ttu-id="ae91c-153">Zde hello **CloudServiceConfiguration** parametr určuje hello *$configuration* proměnné jako objekt PSCloudServiceConfiguration hello.</span><span class="sxs-lookup"><span data-stu-id="ae91c-153">Here, hello **CloudServiceConfiguration** parameter specifies hello *$configuration* variable as hello PSCloudServiceConfiguration object.</span></span> <span data-ttu-id="ae91c-154">Hello **BatchContext** parametr určuje dříve definovanou proměnnou *$context* jako objekt BatchAccountContext hello.</span><span class="sxs-lookup"><span data-stu-id="ae91c-154">hello **BatchContext** parameter specifies a previously defined variable *$context* as hello BatchAccountContext object.</span></span>

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

<span data-ttu-id="ae91c-155">Hello cílovým počtem výpočetních uzlů ve fondu nové hello je určen vzorcem pro automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="ae91c-155">hello target number of compute nodes in hello new pool is determined by an autoscaling formula.</span></span> <span data-ttu-id="ae91c-156">V takovém případě se jednoduše hello vzorec **$TargetDedicated = 4**, určující hello počet výpočetních uzlů ve fondu hello je maximálně 4.</span><span class="sxs-lookup"><span data-stu-id="ae91c-156">In this case, hello formula is simply **$TargetDedicated=4**, indicating hello number of compute nodes in hello pool is 4 at most.</span></span>

## <a name="query-for-pools-jobs-tasks-and-other-details"></a><span data-ttu-id="ae91c-157">Dotazy na fondy, úlohy, úkoly a další podrobnosti</span><span class="sxs-lookup"><span data-stu-id="ae91c-157">Query for pools, jobs, tasks, and other details</span></span>
<span data-ttu-id="ae91c-158">Pomocí rutin, jako například **Get-AzureBatchPool**, **Get-AzureBatchJob**, a **Get-AzureBatchTask** tooquery entity vytvořené v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="ae91c-158">Use cmdlets such as **Get-AzureBatchPool**, **Get-AzureBatchJob**, and **Get-AzureBatchTask** tooquery for entities created under a Batch account.</span></span>

### <a name="query-for-data"></a><span data-ttu-id="ae91c-159">Dotazy na data</span><span class="sxs-lookup"><span data-stu-id="ae91c-159">Query for data</span></span>
<span data-ttu-id="ae91c-160">Jako příklad použijte **Get-AzureBatchPools** toofind fondech.</span><span class="sxs-lookup"><span data-stu-id="ae91c-160">As an example, use **Get-AzureBatchPools** toofind your pools.</span></span> <span data-ttu-id="ae91c-161">Ve výchozím nastavení dotazuje na všechny fondy v účtu, za předpokladu, že jste již uložený objekt BatchAccountContext hello *$context*:</span><span class="sxs-lookup"><span data-stu-id="ae91c-161">By default this queries for all pools under your account, assuming you already stored hello BatchAccountContext object in *$context*:</span></span>

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a><span data-ttu-id="ae91c-162">Použití filtru OData</span><span class="sxs-lookup"><span data-stu-id="ae91c-162">Use an OData filter</span></span>
<span data-ttu-id="ae91c-163">Můžete zadat filtru OData pomocí hello **filtru** parametr toofind hello pouze objekty, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="ae91c-163">You can supply an OData filter using hello **Filter** parameter toofind only hello objects you’re interested in.</span></span> <span data-ttu-id="ae91c-164">Například můžete vyhledat všechny fondy s ID začínajícími řetězcem myPool.</span><span class="sxs-lookup"><span data-stu-id="ae91c-164">For example, you can find all pools with ids starting with “myPool”:</span></span>

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

<span data-ttu-id="ae91c-165">Tato metoda není tak účinná jako použití klauzule Where-Object v místním kanálu.</span><span class="sxs-lookup"><span data-stu-id="ae91c-165">This method is not as flexible as using “Where-Object” in a local pipeline.</span></span> <span data-ttu-id="ae91c-166">Ale hello dotazu se odešlou toohello služby Batch přímo, aby veškeré filtrování provede na straně serveru hello, ukládání šířky pásma Internetu.</span><span class="sxs-lookup"><span data-stu-id="ae91c-166">However, hello query gets sent toohello Batch service directly so that all filtering happens on hello server side, saving Internet bandwidth.</span></span>

### <a name="use-hello-id-parameter"></a><span data-ttu-id="ae91c-167">Pomocí parametru Id hello</span><span class="sxs-lookup"><span data-stu-id="ae91c-167">Use hello Id parameter</span></span>
<span data-ttu-id="ae91c-168">Alternativní tooan filtru OData je toouse hello **Id** parametr.</span><span class="sxs-lookup"><span data-stu-id="ae91c-168">An alternative tooan OData filter is toouse hello **Id** parameter.</span></span> <span data-ttu-id="ae91c-169">tooquery na konkrétní fond s id "myPool":</span><span class="sxs-lookup"><span data-stu-id="ae91c-169">tooquery for a specific pool with id "myPool":</span></span>

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

<span data-ttu-id="ae91c-170">Hello **Id** podporuje pouze vyhledávání úplných id, nepodporuje zástupné znaky nebo filtry stylu typu OData.</span><span class="sxs-lookup"><span data-stu-id="ae91c-170">hello **Id** parameter supports only full-id search, not wildcards or OData-style filters.</span></span>

### <a name="use-hello-maxcount-parameter"></a><span data-ttu-id="ae91c-171">Použití parametru MaxCount hello</span><span class="sxs-lookup"><span data-stu-id="ae91c-171">Use hello MaxCount parameter</span></span>
<span data-ttu-id="ae91c-172">Ve výchozím nastavení každá rutina vrací maximálně 1 000 objektů.</span><span class="sxs-lookup"><span data-stu-id="ae91c-172">By default, each cmdlet returns a maximum of 1000 objects.</span></span> <span data-ttu-id="ae91c-173">Pokud tento limit překročíte, buď upřesněte vaše filtru toobring vracel méně objektů, nebo explicitně nastavit maximální pomocí hello **MaxCount** parametr.</span><span class="sxs-lookup"><span data-stu-id="ae91c-173">If you reach this limit, either refine your filter toobring back fewer objects, or explicitly set a maximum using hello **MaxCount** parameter.</span></span> <span data-ttu-id="ae91c-174">Například:</span><span class="sxs-lookup"><span data-stu-id="ae91c-174">For example:</span></span>

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

<span data-ttu-id="ae91c-175">nastavit tooremove hello horní mez, **MaxCount** too0 nebo méně.</span><span class="sxs-lookup"><span data-stu-id="ae91c-175">tooremove hello upper bound, set **MaxCount** too0 or less.</span></span>

### <a name="use-hello-powershell-pipeline"></a><span data-ttu-id="ae91c-176">Použití kanálu hello prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae91c-176">Use hello PowerShell pipeline</span></span>
<span data-ttu-id="ae91c-177">Rutiny služby batch mohou využívat hello prostředí PowerShell kanálu toosend dat mezi rutinami.</span><span class="sxs-lookup"><span data-stu-id="ae91c-177">Batch cmdlets can leverage hello PowerShell pipeline toosend data between cmdlets.</span></span> <span data-ttu-id="ae91c-178">Tato akce nemá hello stejné ovlivňuje jako zadání parametru, ale díky práci s více entit.</span><span class="sxs-lookup"><span data-stu-id="ae91c-178">This has hello same effect as specifying a parameter, but makes working with multiple entities easier.</span></span>

<span data-ttu-id="ae91c-179">Když chcete například najít a zobrazit všechny úlohy ve svém účtu:</span><span class="sxs-lookup"><span data-stu-id="ae91c-179">For example, find and display all tasks under your account:</span></span>

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

<span data-ttu-id="ae91c-180">Restartování všech výpočetních uzlů ve fondu:</span><span class="sxs-lookup"><span data-stu-id="ae91c-180">Restart (reboot) every compute node in a pool:</span></span>

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a><span data-ttu-id="ae91c-181">Správa balíčků aplikací</span><span class="sxs-lookup"><span data-stu-id="ae91c-181">Application package management</span></span>
<span data-ttu-id="ae91c-182">Balíčky aplikací poskytují jednodušší způsob toodeploy aplikace toohello výpočetních uzlů ve fondech.</span><span class="sxs-lookup"><span data-stu-id="ae91c-182">Application packages provide a simplified way toodeploy applications toohello compute nodes in your pools.</span></span> <span data-ttu-id="ae91c-183">S hello rutin Powershellu ve službě Batch můžete odeslat a spravovat balíčky aplikací v účtu Batch a nasadit balíček verze toocompute uzlů.</span><span class="sxs-lookup"><span data-stu-id="ae91c-183">With hello Batch PowerShell cmdlets, you can upload and manage application packages in your Batch account, and deploy package versions toocompute nodes.</span></span>

<span data-ttu-id="ae91c-184">**Vytvoření** aplikace:</span><span class="sxs-lookup"><span data-stu-id="ae91c-184">**Create** an application:</span></span>

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

<span data-ttu-id="ae91c-185">**Přidání** balíčku aplikace:</span><span class="sxs-lookup"><span data-stu-id="ae91c-185">**Add** an application package:</span></span>

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

<span data-ttu-id="ae91c-186">Sada hello **výchozí verze** aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="ae91c-186">Set hello **default version** for hello application:</span></span>

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

<span data-ttu-id="ae91c-187">**Výčet** balíčků aplikací:</span><span class="sxs-lookup"><span data-stu-id="ae91c-187">**List** an application's packages</span></span>

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

<span data-ttu-id="ae91c-188">**Odstranění** balíčku aplikace:</span><span class="sxs-lookup"><span data-stu-id="ae91c-188">**Delete** an application package</span></span>

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

<span data-ttu-id="ae91c-189">**Odstranění** aplikace:</span><span class="sxs-lookup"><span data-stu-id="ae91c-189">**Delete** an application</span></span>

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

> [!NOTE]
> <span data-ttu-id="ae91c-190">Před odstraněním hello aplikace je nutné odstranit všechny aplikace verze balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae91c-190">You must delete all of an application's application package versions before you delete hello application.</span></span> <span data-ttu-id="ae91c-191">Zobrazí se chybové 'konfliktu, a pokud se pokusíte toodelete aplikace, která má v současné době balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="ae91c-191">You will receive a 'Conflict' error if you try toodelete an application that currently has application packages.</span></span>
> 
> 

### <a name="deploy-an-application-package"></a><span data-ttu-id="ae91c-192">Nasazení balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="ae91c-192">Deploy an application package</span></span>
<span data-ttu-id="ae91c-193">Při vytváření fondu můžete zadat jeden nebo více balíčků aplikací, které budete nasazovat.</span><span class="sxs-lookup"><span data-stu-id="ae91c-193">You can specify one or more application packages for deployment when you create a pool.</span></span> <span data-ttu-id="ae91c-194">Když zadáte balíček v okamžiku vytvoření fondu, je nasazené tooeach uzlu jako hello uzlu spojení fondu.</span><span class="sxs-lookup"><span data-stu-id="ae91c-194">When you specify a package at pool creation time, it is deployed tooeach node as hello node joins pool.</span></span> <span data-ttu-id="ae91c-195">Balíčky se také nasazují při restartování uzlu nebo jeho obnovení z image.</span><span class="sxs-lookup"><span data-stu-id="ae91c-195">Packages are also deployed when a node is rebooted or reimaged.</span></span>

<span data-ttu-id="ae91c-196">Zadejte hello `-ApplicationPackageReference` možnost při vytváření fondu toodeploy balíček toohello fondu aplikací uzly připojí hello fondu.</span><span class="sxs-lookup"><span data-stu-id="ae91c-196">Specify hello `-ApplicationPackageReference` option when creating a pool toodeploy an application package toohello pool's nodes as they join hello pool.</span></span> <span data-ttu-id="ae91c-197">Nejprve vytvořte **PSApplicationPackageReference** objektu a nakonfigurujte ho s hello aplikace Id balíčku verze a chcete, aby toodeploy toohello fondu výpočetních uzlů:</span><span class="sxs-lookup"><span data-stu-id="ae91c-197">First, create a **PSApplicationPackageReference** object, and configure it with hello application Id and package version you want toodeploy toohello pool's compute nodes:</span></span>

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

<span data-ttu-id="ae91c-198">Teď vytvořte fond hello a určit hello balíček referenční objekt jako hello argument toohello `ApplicationPackageReferences` možnost:</span><span class="sxs-lookup"><span data-stu-id="ae91c-198">Now create hello pool, and specify hello package reference object as hello argument toohello `ApplicationPackageReferences` option:</span></span>

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

<span data-ttu-id="ae91c-199">Můžete najít další informace o balíčky aplikací v [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="ae91c-199">You can find more information on application packages in [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ae91c-200">Je nutné [propojení účtu Azure Storage](#linked-storage-account-autostorage) tooyour dávkového účtu toouse balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="ae91c-200">You must [link an Azure Storage account](#linked-storage-account-autostorage) tooyour Batch account toouse application packages.</span></span>
> 
> 

### <a name="update-a-pools-application-packages"></a><span data-ttu-id="ae91c-201">Aktualizace balíčků aplikací fondu</span><span class="sxs-lookup"><span data-stu-id="ae91c-201">Update a pool's application packages</span></span>
<span data-ttu-id="ae91c-202">aplikace hello tooupdate přidělené tooan existujícího fondu, nejprve vytvořit objekt PSApplicationPackageReference s vlastnostmi hello požadovaného (Id a balíček verze aplikace):</span><span class="sxs-lookup"><span data-stu-id="ae91c-202">tooupdate hello applications assigned tooan existing pool, first create a PSApplicationPackageReference object with hello desired properties (application Id and package version):</span></span>

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

<span data-ttu-id="ae91c-203">V dalším kroku sám hello fondu Batch, vymažte všechny existující balíčky, přidat naše nový odkaz na balíček a aktualizaci služby Batch hello se nové nastavení fondu hello:</span><span class="sxs-lookup"><span data-stu-id="ae91c-203">Next, get hello pool from Batch, clear out any existing packages, add our new package reference, and update hello Batch service with hello new pool settings:</span></span>

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

<span data-ttu-id="ae91c-204">Jste nyní aktualizovat vlastnosti hello fondu v hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="ae91c-204">You've now updated hello pool's properties in hello Batch service.</span></span> <span data-ttu-id="ae91c-205">tooactually nasadit hello nové aplikace balíčku toocompute uzly ve fondu hello, ale musí restartovat nebo obnovit z Image tyto uzly.</span><span class="sxs-lookup"><span data-stu-id="ae91c-205">tooactually deploy hello new application package toocompute nodes in hello pool, however, you must restart or reimage those nodes.</span></span> <span data-ttu-id="ae91c-206">K restartování všech uzlů ve fondu můžete použít tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="ae91c-206">You can restart every node in a pool with this command:</span></span>

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

> [!TIP]
> <span data-ttu-id="ae91c-207">Můžete nasadit víc aplikací balíčky toohello výpočetních uzlů ve fondu.</span><span class="sxs-lookup"><span data-stu-id="ae91c-207">You can deploy multiple application packages toohello compute nodes in a pool.</span></span> <span data-ttu-id="ae91c-208">Pokud chcete příliš*přidat* balíček aplikace místo nahrazení hello aktuálně nasazená balíčky, vynechejte hello `$pool.ApplicationPackageReferences.Clear()` řádku výše.</span><span class="sxs-lookup"><span data-stu-id="ae91c-208">If you'd like too*add* an application package instead of replacing hello currently deployed packages, omit hello `$pool.ApplicationPackageReferences.Clear()` line above.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="ae91c-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ae91c-209">Next steps</span></span>
* <span data-ttu-id="ae91c-210">Podrobný popis syntaxe rutin najdete v článku [Rutiny služby Azure Batch – reference](/powershell/module/azurerm.batch/#batch).</span><span class="sxs-lookup"><span data-stu-id="ae91c-210">For detailed cmdlet syntax and examples, see [Azure Batch cmdlet reference](/powershell/module/azurerm.batch/#batch).</span></span>
* <span data-ttu-id="ae91c-211">Další informace o aplikacích a balíčky aplikací ve službě Batch najdete v tématu [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="ae91c-211">For more information about applications and application packages in Batch, see [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md).</span></span>

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/