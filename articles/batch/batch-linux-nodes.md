---
title: "aaaRun Linux na virtuálním počítači výpočetní uzly - Azure Batch | Microsoft Docs"
description: "Zjistěte, jak tooprocess vaše paralelní výpočetní úlohy na fondy virtuálních počítačích s Linuxem v Azure Batch."
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3daabd5c577baaafd0544f9f7913cb7b116d74d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a><span data-ttu-id="911f0-103">Zřídit Linux výpočetních uzlů ve fondech Batch</span><span class="sxs-lookup"><span data-stu-id="911f0-103">Provision Linux compute nodes in Batch pools</span></span>

<span data-ttu-id="911f0-104">Azure Batch toorun paralelní výpočetních úloh můžete použít u virtuálních počítačů Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="911f0-104">You can use Azure Batch toorun parallel compute workloads on both Linux and Windows virtual machines.</span></span> <span data-ttu-id="911f0-105">Tento článek popisuje, jak toocreate fondy Linux výpočetní uzly ve službě Batch hello pomocí obou hello [Batch Python] [ py_batch_package] a [Batch .NET] [ api_net] knihovny klienta.</span><span class="sxs-lookup"><span data-stu-id="911f0-105">This article details how toocreate pools of Linux compute nodes in hello Batch service by using both hello [Batch Python][py_batch_package] and [Batch .NET][api_net] client libraries.</span></span>

> [!NOTE]
> <span data-ttu-id="911f0-106">Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017.</span><span class="sxs-lookup"><span data-stu-id="911f0-106">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="911f0-107">Že jsou podporované v fondy Batch vytvořit až 10. března 2016 5 2017 července pouze v případě, že hello fondu byla vytvořena pomocí konfigurace cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="911f0-107">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if hello pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="911f0-108">Fondy batch vytvořen předchozí too10. března 2016 nepodporují balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="911f0-108">Batch pools created prior too10 March 2016 do not support application packages.</span></span> <span data-ttu-id="911f0-109">Další informace o použití aplikace balíčky toodeploy uzly Batch tooyour aplikace, najdete v části [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="911f0-109">For more information about using application packages toodeploy your applications tooyour Batch nodes, see [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md).</span></span>
>
>

## <a name="virtual-machine-configuration"></a><span data-ttu-id="911f0-110">Konfigurace virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="911f0-110">Virtual machine configuration</span></span>
<span data-ttu-id="911f0-111">Když vytvoříte fond výpočetních uzlů ve službě Batch, máte dvě možnosti, ze které velikost uzlu hello tooselect a operační systém: Konfigurace cloudových služeb a konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="911f0-111">When you create a pool of compute nodes in Batch, you have two options from which tooselect hello node size and operating system: Cloud Services Configuration and Virtual Machine Configuration.</span></span>

<span data-ttu-id="911f0-112">**Konfigurace služby Cloud Services** poskytuje *pouze* výpočetní uzly Windows.</span><span class="sxs-lookup"><span data-stu-id="911f0-112">**Cloud Services Configuration** provides Windows compute nodes *only*.</span></span> <span data-ttu-id="911f0-113">K dispozici výpočetní uzel velikosti jsou uvedeny v [velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md), a dostupných operačních systémů, jsou uvedeny v hello [verze hostovaného operačního systému Azure a kompatibilních sad SDK](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="911f0-113">Available compute node sizes are listed in [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md), and available operating systems are listed in hello [Azure Guest OS releases and SDK compatibility matrix](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span> <span data-ttu-id="911f0-114">Když vytvoříte fond, který obsahuje uzly Azure Cloud Services, zadejte velikost uzlu hello a hello řada operačního systému, která jsou popsaná v hello výše články.</span><span class="sxs-lookup"><span data-stu-id="911f0-114">When you create a pool that contains Azure Cloud Services nodes, you specify hello node size and hello OS family, which are described in hello previously mentioned articles.</span></span> <span data-ttu-id="911f0-115">Pro fondy Windows výpočetní uzly, se nejčastěji používá cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="911f0-115">For pools of Windows compute nodes, Cloud Services is most commonly used.</span></span>

<span data-ttu-id="911f0-116">**Konfigurace virtuálního počítače** poskytuje, Linux a Windows Image pro výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="911f0-116">**Virtual Machine Configuration** provides both Linux and Windows images for compute nodes.</span></span> <span data-ttu-id="911f0-117">K dispozici výpočetní uzel velikosti jsou uvedeny v [velikosti virtuálních počítačů v Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) a [velikosti virtuálních počítačů v Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span><span class="sxs-lookup"><span data-stu-id="911f0-117">Available compute node sizes are listed in [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) and [Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span></span> <span data-ttu-id="911f0-118">Když vytvoříte fond, který obsahuje uzly konfigurace virtuálního počítače, je nutné zadat velikost hello hello uzly, odkaz na obrázek hello virtuálního počítače a hello Batch uzlu agenta SKU toobe nainstalované na uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="911f0-118">When you create a pool that contains Virtual Machine Configuration nodes, you must specify hello size of hello nodes, hello virtual machine image reference, and hello Batch node agent SKU toobe installed on hello nodes.</span></span>

### <a name="virtual-machine-image-reference"></a><span data-ttu-id="911f0-119">Odkaz na obrázek virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="911f0-119">Virtual machine image reference</span></span>
<span data-ttu-id="911f0-120">Hello používá služba Batch [sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tooprovide Linux výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="911f0-120">hello Batch service uses [virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tooprovide Linux compute nodes.</span></span> <span data-ttu-id="911f0-121">Můžete zadat bitové kopie z hello [Azure Marketplace][vm_marketplace], nebo zadejte vlastní obrázek, který jste připravili.</span><span class="sxs-lookup"><span data-stu-id="911f0-121">You can specify an image from hello [Azure Marketplace][vm_marketplace], or provide a custom image that you have prepared.</span></span> <span data-ttu-id="911f0-122">Další informace o vlastních imagích najdete v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md#pool).</span><span class="sxs-lookup"><span data-stu-id="911f0-122">For more details about custom images, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#pool).</span></span>

<span data-ttu-id="911f0-123">Když konfigurujete odkaz bitové kopie virtuálního počítače, je třeba zadat hello vlastnosti bitové kopie virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="911f0-123">When you configure a virtual machine image reference, you specify hello properties of hello virtual machine image.</span></span> <span data-ttu-id="911f0-124">Hello následující vlastnosti jsou požadovány při vytvoření odkaz na obrázek virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="911f0-124">hello following properties are required when you create a virtual machine image reference:</span></span>

| <span data-ttu-id="911f0-125">**Vlastnosti referenční bitové kopie**</span><span class="sxs-lookup"><span data-stu-id="911f0-125">**Image reference properties**</span></span> | <span data-ttu-id="911f0-126">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="911f0-126">**Example**</span></span> |
| --- | --- |
| <span data-ttu-id="911f0-127">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="911f0-127">Publisher</span></span> |<span data-ttu-id="911f0-128">Canonical</span><span class="sxs-lookup"><span data-stu-id="911f0-128">Canonical</span></span> |
| <span data-ttu-id="911f0-129">Nabídka</span><span class="sxs-lookup"><span data-stu-id="911f0-129">Offer</span></span> |<span data-ttu-id="911f0-130">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="911f0-130">UbuntuServer</span></span> |
| <span data-ttu-id="911f0-131">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="911f0-131">SKU</span></span> |<span data-ttu-id="911f0-132">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="911f0-132">14.04.4-LTS</span></span> |
| <span data-ttu-id="911f0-133">Verze</span><span class="sxs-lookup"><span data-stu-id="911f0-133">Version</span></span> |<span data-ttu-id="911f0-134">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-134">latest</span></span> |

> [!TIP]
> <span data-ttu-id="911f0-135">Další informace o těchto vlastností a jak toolist Marketplace Image v nástroji [vyhledání a vyberte Image virtuálních počítačů Linux v Azure pomocí rozhraní CLI nebo Powershellu](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="911f0-135">You can learn more about these properties and how toolist Marketplace images in [Navigate and select Linux virtual machine images in Azure with CLI or PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="911f0-136">Upozorňujeme, že ne všechny bitové kopie Marketplace jsou momentálně kompatibilní s Batch.</span><span class="sxs-lookup"><span data-stu-id="911f0-136">Note that not all Marketplace images are currently compatible with Batch.</span></span> <span data-ttu-id="911f0-137">Další informace najdete v tématu [uzlu agenta SKU](#node-agent-sku).</span><span class="sxs-lookup"><span data-stu-id="911f0-137">For more information, see [Node agent SKU](#node-agent-sku).</span></span>
>
>

### <a name="node-agent-sku"></a><span data-ttu-id="911f0-138">Uzel agenta SKU</span><span class="sxs-lookup"><span data-stu-id="911f0-138">Node agent SKU</span></span>
<span data-ttu-id="911f0-139">agent uzlu Hello Batch je program, který běží na každém uzlu ve fondu hello a poskytuje rozhraní příkazu a řízení hello mezi hello uzlu a služba Batch hello.</span><span class="sxs-lookup"><span data-stu-id="911f0-139">hello Batch node agent is a program that runs on each node in hello pool and provides hello command-and-control interface between hello node and hello Batch service.</span></span> <span data-ttu-id="911f0-140">Existují různé implementace hello uzlu agenta, označuje jako SKU, pro různé operační systémy.</span><span class="sxs-lookup"><span data-stu-id="911f0-140">There are different implementations of hello node agent, known as SKUs, for different operating systems.</span></span> <span data-ttu-id="911f0-141">V podstatě při vytváření konfigurace virtuálního počítače, nejprve zadat odkaz na obrázek hello virtuálního počítače a pak zadejte hello uzlu agenta tooinstall na bitovou kopii hello.</span><span class="sxs-lookup"><span data-stu-id="911f0-141">Essentially, when you create a Virtual Machine Configuration, you first specify hello virtual machine image reference, and then you specify hello node agent tooinstall on hello image.</span></span> <span data-ttu-id="911f0-142">Obvykle každého uzlu agenta SKU je kompatibilní s více bitových kopií virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="911f0-142">Typically, each node agent SKU is compatible with multiple virtual machine images.</span></span> <span data-ttu-id="911f0-143">Tady je několik příkladů uzlu agenta SKU:</span><span class="sxs-lookup"><span data-stu-id="911f0-143">Here are a few examples of node agent SKUs:</span></span>

* <span data-ttu-id="911f0-144">batch.Node.Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="911f0-144">batch.node.ubuntu 14.04</span></span>
* <span data-ttu-id="911f0-145">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="911f0-145">batch.node.centos 7</span></span>
* <span data-ttu-id="911f0-146">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="911f0-146">batch.node.windows amd64</span></span>

> [!IMPORTANT]
> <span data-ttu-id="911f0-147">Ne všechny bitové kopie virtuálních počítačů, které jsou k dispozici v hello Marketplace jsou kompatibilní s agenty uzlu Batch aktuálně k dispozici hello.</span><span class="sxs-lookup"><span data-stu-id="911f0-147">Not all virtual machine images that are available in hello Marketplace are compatible with hello currently available Batch node agents.</span></span> <span data-ttu-id="911f0-148">Použijte hello SDK služby Batch toolist hello k dispozici uzel agenta SKU a hello bitové kopie virtuálních počítačů, ke kterým jsou kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="911f0-148">Use hello Batch SDKs toolist hello available node agent SKUs and hello virtual machine images with which they are compatible.</span></span> <span data-ttu-id="911f0-149">V tématu hello [bitové kopie seznamu virtuálních počítačů](#list-of-virtual-machine-images) dále v tomto článku pro další informace a příklady, jak tooretrieve seznam platných obrázků za běhu.</span><span class="sxs-lookup"><span data-stu-id="911f0-149">See hello [List of Virtual Machine images](#list-of-virtual-machine-images) later in this article for more information and examples of how tooretrieve a list of valid images at runtime.</span></span>
>
>

## <a name="create-a-linux-pool-batch-python"></a><span data-ttu-id="911f0-150">Vytvoření fondu Linux: Batch Python</span><span class="sxs-lookup"><span data-stu-id="911f0-150">Create a Linux pool: Batch Python</span></span>
<span data-ttu-id="911f0-151">Hello následující fragment kódu ukazuje příklad toouse hello [Microsoft Azure Batch klientské knihovny pro jazyk Python] [ py_batch_package] toocreate Ubuntu Server fond výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="911f0-151">hello following code snippet shows an example of how toouse hello [Microsoft Azure Batch Client Library for Python][py_batch_package] toocreate a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="911f0-152">Referenční dokumentace pro hello modulu Batch Python najdete na [azure.batch balíček] [ py_batch_docs] na hello dokumentace pro čtení.</span><span class="sxs-lookup"><span data-stu-id="911f0-152">Reference documentation for hello Batch Python module can be found at [azure.batch package][py_batch_docs] on Read hello Docs.</span></span>

<span data-ttu-id="911f0-153">Vytvoří tento fragment kódu [elementu ImageReference] [ py_imagereference] explicitně a určí, každý z jeho vlastnosti (vydavatel, nabídku, SKU a verzi).</span><span class="sxs-lookup"><span data-stu-id="911f0-153">This snippet creates an [ImageReference][py_imagereference] explicitly and specifies each of its properties (publisher, offer, SKU, version).</span></span> <span data-ttu-id="911f0-154">V produkčním kódu, ale doporučujeme vám použít hello [list_node_agent_skus] [ py_list_skus] toodetermine metoda a vyberte z hello k dispozici bitovou kopii a uzel agenta SKU kombinace za běhu.</span><span class="sxs-lookup"><span data-stu-id="911f0-154">In production code, however, we recommend that you use hello [list_node_agent_skus][py_list_skus] method toodetermine and select from hello available image and node agent SKU combinations at runtime.</span></span>

```python
# Import hello required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize hello Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create hello unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure hello start task for hello pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies hello Marketplace
# virtual machine image tooinstall on hello nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create hello VirtualMachineConfiguration, specifying
# hello VM image reference and hello Batch node agent to
# be installed on hello node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign hello virtual machine configuration toohello pool
new_pool.virtual_machine_configuration = vmc

# Create pool in hello Batch service
client.pool.add(new_pool)
```

<span data-ttu-id="911f0-155">Jak je uvedeno nahoře, doporučujeme místo vytvoření hello [elementu ImageReference] [ py_imagereference] explicitně, použijete hello [list_node_agent_skus] [ py_list_skus] metoda toodynamically vybrat z hello aktuálně podporované kombinace bitovou kopii agenta nebo Marketplace uzlu.</span><span class="sxs-lookup"><span data-stu-id="911f0-155">As mentioned previously, we recommend that instead of creating hello [ImageReference][py_imagereference] explicitly, you use hello [list_node_agent_skus][py_list_skus] method toodynamically select from hello currently supported node agent/Marketplace image combinations.</span></span> <span data-ttu-id="911f0-156">Následující fragment kódu ukazuje Python jak Hello toouse tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="911f0-156">hello following Python snippet shows how toouse this method.</span></span>

```python
# Get hello list of node agents from hello Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain hello desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick hello first image reference from hello list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create hello VirtualMachineConfiguration, specifying hello VM image
# reference and hello Batch node agent toobe installed on hello node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a><span data-ttu-id="911f0-157">Vytvoření fondu Linux: Batch .NET</span><span class="sxs-lookup"><span data-stu-id="911f0-157">Create a Linux pool: Batch .NET</span></span>
<span data-ttu-id="911f0-158">Hello následující fragment kódu ukazuje příklad toouse hello [Batch .NET] [ nuget_batch_net] klienta knihovny toocreate Ubuntu Server fond výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="911f0-158">hello following code snippet shows an example of how toouse hello [Batch .NET][nuget_batch_net] client library toocreate a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="911f0-159">Můžete najít hello [Batch .NET referenční dokumentaci k nástroji] [ api_net] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="911f0-159">You can find hello [Batch .NET reference documentation][api_net] on MSDN.</span></span>

<span data-ttu-id="911f0-160">Hello následující fragment kódu používá hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] tooselect metoda z hello seznam aktuálně podporovaných Marketplace bitové kopie a uzlu agenta SKU kombinací.</span><span class="sxs-lookup"><span data-stu-id="911f0-160">hello following code snippet uses hello [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method tooselect from hello list of currently supported Marketplace image and node agent SKU combinations.</span></span> <span data-ttu-id="911f0-161">Tento postup je žádoucí, protože hello seznam podporovaných kombinací může změnit z tootime čas.</span><span class="sxs-lookup"><span data-stu-id="911f0-161">This technique is desirable because hello list of supported combinations may change from time tootime.</span></span> <span data-ttu-id="911f0-162">Nejčastěji jsou přidány podporovaných kombinací.</span><span class="sxs-lookup"><span data-stu-id="911f0-162">Most commonly, supported combinations are added.</span></span>

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us tooselect from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of hello VM image
// that we wish toouse.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain hello first node agent SKU in hello collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create hello VirtualMachineConfiguration for use when actually
// creating hello pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

// Create hello unbound pool object using hello VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit hello pool toohello Batch service
await pool.CommitAsync();
```

<span data-ttu-id="911f0-163">I když předchozí fragment hello používá hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metoda toodynamically seznam a vyberte jednu z podporované bitové kopie a uzlu agenta SKU kombinace (doporučeno), můžete také nakonfigurovat [elementu ImageReference] [ net_imagereference] explicitně:</span><span class="sxs-lookup"><span data-stu-id="911f0-163">Although hello previous snippet uses hello [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method toodynamically list and select from supported image and node agent SKU combinations (recommended), you can also configure an [ImageReference][net_imagereference] explicitly:</span></span>

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a><span data-ttu-id="911f0-164">Seznam bitové kopie virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="911f0-164">List of virtual machine images</span></span>
<span data-ttu-id="911f0-165">Hello následující tabulka uvádí Image hello Marketplace virtuálních počítačů, které jsou kompatibilní s agenty hello k dispozici Batch uzlu v době poslední aktualizace v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="911f0-165">hello following table lists hello Marketplace virtual machine images that are compatible with hello available Batch node agents when this article was last updated.</span></span> <span data-ttu-id="911f0-166">Je důležité toonote, že tento seznam není spolehlivý, protože bitové kopie a agenty uzlu může přidat nebo odebrat kdykoli.</span><span class="sxs-lookup"><span data-stu-id="911f0-166">It is important toonote that this list is not definitive because images and node agents may be added or removed at any time.</span></span> <span data-ttu-id="911f0-167">Doporučujeme vaší aplikací a služeb Batch vždy použít [list_node_agent_skus] [ py_list_skus] (Python) a [ListNodeAgentSkus] [ net_list_skus] Toodetermine (batch .NET) a vyberte z hello aktuálně dostupné edice.</span><span class="sxs-lookup"><span data-stu-id="911f0-167">We recommend that your Batch applications and services always use [list_node_agent_skus][py_list_skus] (Python) and [ListNodeAgentSkus][net_list_skus] (Batch .NET) toodetermine and select from hello currently available SKUs.</span></span>

> [!WARNING]
> <span data-ttu-id="911f0-168">Následující seznam Hello může kdykoli změnit.</span><span class="sxs-lookup"><span data-stu-id="911f0-168">hello following list may change at any time.</span></span> <span data-ttu-id="911f0-169">Vždy používat hello **agent uzlu seznamu SKU** metody, které jsou k dispozici v toolist rozhraní API služby Batch hello hello kompatibilní virtuální počítač a uzlu agenta SKU při spuštění úlohy Batch.</span><span class="sxs-lookup"><span data-stu-id="911f0-169">Always use hello **list node agent SKU** methods available in hello Batch APIs toolist hello compatible virtual machine and node agent SKUs when you run your Batch jobs.</span></span>
>
>

| <span data-ttu-id="911f0-170">**Vydavatele**</span><span class="sxs-lookup"><span data-stu-id="911f0-170">**Publisher**</span></span> | <span data-ttu-id="911f0-171">**Nabídka**</span><span class="sxs-lookup"><span data-stu-id="911f0-171">**Offer**</span></span> | <span data-ttu-id="911f0-172">**Obrázek SKU**</span><span class="sxs-lookup"><span data-stu-id="911f0-172">**Image SKU**</span></span> | <span data-ttu-id="911f0-173">**Verze**</span><span class="sxs-lookup"><span data-stu-id="911f0-173">**Version**</span></span> | <span data-ttu-id="911f0-174">**Uzel agenta SKU ID**</span><span class="sxs-lookup"><span data-stu-id="911f0-174">**Node agent SKU ID**</span></span> |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| <span data-ttu-id="911f0-175">Canonical</span><span class="sxs-lookup"><span data-stu-id="911f0-175">Canonical</span></span> | <span data-ttu-id="911f0-176">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="911f0-176">UbuntuServer</span></span> | <span data-ttu-id="911f0-177">14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="911f0-177">14.04.5-LTS</span></span> | <span data-ttu-id="911f0-178">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-178">latest</span></span> | <span data-ttu-id="911f0-179">batch.Node.Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="911f0-179">batch.node.ubuntu 14.04</span></span> |
| <span data-ttu-id="911f0-180">Canonical</span><span class="sxs-lookup"><span data-stu-id="911f0-180">Canonical</span></span> | <span data-ttu-id="911f0-181">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="911f0-181">UbuntuServer</span></span> | <span data-ttu-id="911f0-182">16.04.0-LTS</span><span class="sxs-lookup"><span data-stu-id="911f0-182">16.04.0-LTS</span></span> | <span data-ttu-id="911f0-183">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-183">latest</span></span> | <span data-ttu-id="911f0-184">batch.Node.Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="911f0-184">batch.node.ubuntu 16.04</span></span> |
| <span data-ttu-id="911f0-185">Credativ</span><span class="sxs-lookup"><span data-stu-id="911f0-185">Credativ</span></span> | <span data-ttu-id="911f0-186">Debian</span><span class="sxs-lookup"><span data-stu-id="911f0-186">Debian</span></span> | <span data-ttu-id="911f0-187">8</span><span class="sxs-lookup"><span data-stu-id="911f0-187">8</span></span> | <span data-ttu-id="911f0-188">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-188">latest</span></span> | <span data-ttu-id="911f0-189">batch.Node.debian 8</span><span class="sxs-lookup"><span data-stu-id="911f0-189">batch.node.debian 8</span></span> |
| <span data-ttu-id="911f0-190">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="911f0-190">OpenLogic</span></span> | <span data-ttu-id="911f0-191">CentOS</span><span class="sxs-lookup"><span data-stu-id="911f0-191">CentOS</span></span> | <span data-ttu-id="911f0-192">7.0</span><span class="sxs-lookup"><span data-stu-id="911f0-192">7.0</span></span> | <span data-ttu-id="911f0-193">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-193">latest</span></span> | <span data-ttu-id="911f0-194">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="911f0-194">batch.node.centos 7</span></span> |
| <span data-ttu-id="911f0-195">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="911f0-195">OpenLogic</span></span> | <span data-ttu-id="911f0-196">CentOS</span><span class="sxs-lookup"><span data-stu-id="911f0-196">CentOS</span></span> | <span data-ttu-id="911f0-197">7.1</span><span class="sxs-lookup"><span data-stu-id="911f0-197">7.1</span></span> | <span data-ttu-id="911f0-198">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-198">latest</span></span> | <span data-ttu-id="911f0-199">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="911f0-199">batch.node.centos 7</span></span> |
| <span data-ttu-id="911f0-200">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="911f0-200">OpenLogic</span></span> | <span data-ttu-id="911f0-201">CentOS HPC</span><span class="sxs-lookup"><span data-stu-id="911f0-201">CentOS-HPC</span></span> | <span data-ttu-id="911f0-202">7.1</span><span class="sxs-lookup"><span data-stu-id="911f0-202">7.1</span></span> | <span data-ttu-id="911f0-203">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-203">latest</span></span> | <span data-ttu-id="911f0-204">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="911f0-204">batch.node.centos 7</span></span> |
| <span data-ttu-id="911f0-205">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="911f0-205">OpenLogic</span></span> | <span data-ttu-id="911f0-206">CentOS</span><span class="sxs-lookup"><span data-stu-id="911f0-206">CentOS</span></span> | <span data-ttu-id="911f0-207">7.2</span><span class="sxs-lookup"><span data-stu-id="911f0-207">7.2</span></span> | <span data-ttu-id="911f0-208">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-208">latest</span></span> | <span data-ttu-id="911f0-209">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="911f0-209">batch.node.centos 7</span></span> |
| <span data-ttu-id="911f0-210">Oracle</span><span class="sxs-lookup"><span data-stu-id="911f0-210">Oracle</span></span> | <span data-ttu-id="911f0-211">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="911f0-211">Oracle-Linux</span></span> | <span data-ttu-id="911f0-212">7.0</span><span class="sxs-lookup"><span data-stu-id="911f0-212">7.0</span></span> | <span data-ttu-id="911f0-213">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-213">latest</span></span> | <span data-ttu-id="911f0-214">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="911f0-214">batch.node.centos 7</span></span> |
| <span data-ttu-id="911f0-215">Oracle</span><span class="sxs-lookup"><span data-stu-id="911f0-215">Oracle</span></span> | <span data-ttu-id="911f0-216">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="911f0-216">Oracle-Linux</span></span> | <span data-ttu-id="911f0-217">7.2</span><span class="sxs-lookup"><span data-stu-id="911f0-217">7.2</span></span> | <span data-ttu-id="911f0-218">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-218">latest</span></span> | <span data-ttu-id="911f0-219">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="911f0-219">batch.node.centos 7</span></span> |
| <span data-ttu-id="911f0-220">SUSE</span><span class="sxs-lookup"><span data-stu-id="911f0-220">SUSE</span></span> | <span data-ttu-id="911f0-221">openSUSE</span><span class="sxs-lookup"><span data-stu-id="911f0-221">openSUSE</span></span> | <span data-ttu-id="911f0-222">13.2</span><span class="sxs-lookup"><span data-stu-id="911f0-222">13.2</span></span> | <span data-ttu-id="911f0-223">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-223">latest</span></span> | <span data-ttu-id="911f0-224">batch.Node.opensuse 13.2</span><span class="sxs-lookup"><span data-stu-id="911f0-224">batch.node.opensuse 13.2</span></span> |
| <span data-ttu-id="911f0-225">SUSE</span><span class="sxs-lookup"><span data-stu-id="911f0-225">SUSE</span></span> | <span data-ttu-id="911f0-226">openSUSE přestupného</span><span class="sxs-lookup"><span data-stu-id="911f0-226">openSUSE-Leap</span></span> | <span data-ttu-id="911f0-227">42.1</span><span class="sxs-lookup"><span data-stu-id="911f0-227">42.1</span></span> | <span data-ttu-id="911f0-228">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-228">latest</span></span> | <span data-ttu-id="911f0-229">batch.Node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="911f0-229">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="911f0-230">SUSE</span><span class="sxs-lookup"><span data-stu-id="911f0-230">SUSE</span></span> | <span data-ttu-id="911f0-231">SLES</span><span class="sxs-lookup"><span data-stu-id="911f0-231">SLES</span></span> | <span data-ttu-id="911f0-232">12-SP1</span><span class="sxs-lookup"><span data-stu-id="911f0-232">12-SP1</span></span> | <span data-ttu-id="911f0-233">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-233">latest</span></span> | <span data-ttu-id="911f0-234">batch.Node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="911f0-234">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="911f0-235">SUSE</span><span class="sxs-lookup"><span data-stu-id="911f0-235">SUSE</span></span> | <span data-ttu-id="911f0-236">SLES HPC</span><span class="sxs-lookup"><span data-stu-id="911f0-236">SLES-HPC</span></span> | <span data-ttu-id="911f0-237">12-SP1</span><span class="sxs-lookup"><span data-stu-id="911f0-237">12-SP1</span></span> | <span data-ttu-id="911f0-238">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-238">latest</span></span> | <span data-ttu-id="911f0-239">batch.Node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="911f0-239">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="911f0-240">Microsoft služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="911f0-240">microsoft-ads</span></span> | <span data-ttu-id="911f0-241">data-vědecké účely-virtuálního počítače s linuxem</span><span class="sxs-lookup"><span data-stu-id="911f0-241">linux-data-science-vm</span></span> | <span data-ttu-id="911f0-242">linuxdsvm</span><span class="sxs-lookup"><span data-stu-id="911f0-242">linuxdsvm</span></span> | <span data-ttu-id="911f0-243">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-243">latest</span></span> | <span data-ttu-id="911f0-244">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="911f0-244">batch.node.centos 7</span></span> |
| <span data-ttu-id="911f0-245">Microsoft služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="911f0-245">microsoft-ads</span></span> | <span data-ttu-id="911f0-246">Standard-data-vědecké účely vm</span><span class="sxs-lookup"><span data-stu-id="911f0-246">standard-data-science-vm</span></span> | <span data-ttu-id="911f0-247">Standard-data-vědecké účely vm</span><span class="sxs-lookup"><span data-stu-id="911f0-247">standard-data-science-vm</span></span> | <span data-ttu-id="911f0-248">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-248">latest</span></span> | <span data-ttu-id="911f0-249">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="911f0-249">batch.node.windows amd64</span></span> |
| <span data-ttu-id="911f0-250">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-250">MicrosoftWindowsServer</span></span> | <span data-ttu-id="911f0-251">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-251">WindowsServer</span></span> | <span data-ttu-id="911f0-252">AKTUALIZACE SP1 2008 R2</span><span class="sxs-lookup"><span data-stu-id="911f0-252">2008-R2-SP1</span></span> | <span data-ttu-id="911f0-253">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-253">latest</span></span> | <span data-ttu-id="911f0-254">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="911f0-254">batch.node.windows amd64</span></span> |
| <span data-ttu-id="911f0-255">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-255">MicrosoftWindowsServer</span></span> | <span data-ttu-id="911f0-256">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-256">WindowsServer</span></span> | <span data-ttu-id="911f0-257">2012-Datacenter</span><span class="sxs-lookup"><span data-stu-id="911f0-257">2012-Datacenter</span></span> | <span data-ttu-id="911f0-258">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-258">latest</span></span> | <span data-ttu-id="911f0-259">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="911f0-259">batch.node.windows amd64</span></span> |
| <span data-ttu-id="911f0-260">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-260">MicrosoftWindowsServer</span></span> | <span data-ttu-id="911f0-261">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-261">WindowsServer</span></span> | <span data-ttu-id="911f0-262">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="911f0-262">2012-R2-Datacenter</span></span> | <span data-ttu-id="911f0-263">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-263">latest</span></span> | <span data-ttu-id="911f0-264">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="911f0-264">batch.node.windows amd64</span></span> |
| <span data-ttu-id="911f0-265">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-265">MicrosoftWindowsServer</span></span> | <span data-ttu-id="911f0-266">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-266">WindowsServer</span></span> | <span data-ttu-id="911f0-267">2016 Datacenter</span><span class="sxs-lookup"><span data-stu-id="911f0-267">2016-Datacenter</span></span> | <span data-ttu-id="911f0-268">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-268">latest</span></span> | <span data-ttu-id="911f0-269">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="911f0-269">batch.node.windows amd64</span></span> |
| <span data-ttu-id="911f0-270">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-270">MicrosoftWindowsServer</span></span> | <span data-ttu-id="911f0-271">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="911f0-271">WindowsServer</span></span> | <span data-ttu-id="911f0-272">2016 datového centra s kontejnery</span><span class="sxs-lookup"><span data-stu-id="911f0-272">2016-Datacenter-with-Containers</span></span> | <span data-ttu-id="911f0-273">nejnovější</span><span class="sxs-lookup"><span data-stu-id="911f0-273">latest</span></span> | <span data-ttu-id="911f0-274">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="911f0-274">batch.node.windows amd64</span></span> |

## <a name="connect-toolinux-nodes-using-ssh"></a><span data-ttu-id="911f0-275">Připojit tooLinux uzly pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="911f0-275">Connect tooLinux nodes using SSH</span></span>
<span data-ttu-id="911f0-276">Během vývoje nebo při řešení potíží možná bude nutné toosign v toohello uzly ve fondu.</span><span class="sxs-lookup"><span data-stu-id="911f0-276">During development or while troubleshooting, you may find it necessary toosign in toohello nodes in your pool.</span></span> <span data-ttu-id="911f0-277">Na rozdíl od Windows výpočetní uzly nemůžete použít protokol RDP (Remote Desktop) tooconnect tooLinux uzlů.</span><span class="sxs-lookup"><span data-stu-id="911f0-277">Unlike Windows compute nodes, you cannot use Remote Desktop Protocol (RDP) tooconnect tooLinux nodes.</span></span> <span data-ttu-id="911f0-278">Místo toho hello služba Batch umožňuje přístup k SSH na každém uzlu vzdáleného připojení.</span><span class="sxs-lookup"><span data-stu-id="911f0-278">Instead, hello Batch service enables SSH access on each node for remote connection.</span></span>

<span data-ttu-id="911f0-279">Hello následující fragment kódu Python vytvoří uživatele na každém uzlu ve fondu, který je vyžadován pro připojení ke vzdálené.</span><span class="sxs-lookup"><span data-stu-id="911f0-279">hello following Python code snippet creates a user on each node in a pool, which is required for remote connection.</span></span> <span data-ttu-id="911f0-280">Potom zobrazí informace o připojení hello zabezpečené shell (SSH) pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="911f0-280">It then prints hello secure shell (SSH) connection information for each node.</span></span>

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify hello ID of an existing pool containing Linux nodes
# currently in hello 'idle' state
pool_id = ''

# Specify hello username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create hello user that will be added tooeach node in hello pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get hello list of nodes in hello pool
nodes = batch_client.compute_node.list(pool_id)

# Add hello user tooeach node in hello pool and print
# hello connection information for hello node
for node in nodes:
    # Add hello user toohello node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for hello node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print hello connection info for hello node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

<span data-ttu-id="911f0-281">Toto je ukázkový výstup hello předchozí kód pro fond, který obsahuje čtyři uzly Linux:</span><span class="sxs-lookup"><span data-stu-id="911f0-281">Here is sample output for hello previous code for a pool that contains four Linux nodes:</span></span>

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

<span data-ttu-id="911f0-282">Namísto hesla můžete zadat veřejný klíč SSH při vytváření uživatele na uzlu.</span><span class="sxs-lookup"><span data-stu-id="911f0-282">Instead of a password, you can specify an SSH public key when you create a user on a node.</span></span> <span data-ttu-id="911f0-283">V hello Python SDK, pomocí hello **ssh_public_key** parametr na [ComputeNodeUser][py_computenodeuser].</span><span class="sxs-lookup"><span data-stu-id="911f0-283">In hello Python SDK, use hello **ssh_public_key** parameter on [ComputeNodeUser][py_computenodeuser].</span></span> <span data-ttu-id="911f0-284">V rozhraní .NET, použijte hello [ComputeNodeUser][net_computenodeuser].[ Parametru SshPublicKey] [ net_ssh_key] vlastnost.</span><span class="sxs-lookup"><span data-stu-id="911f0-284">In .NET, use hello [ComputeNodeUser][net_computenodeuser].[SshPublicKey][net_ssh_key] property.</span></span>

## <a name="pricing"></a><span data-ttu-id="911f0-285">Ceny</span><span class="sxs-lookup"><span data-stu-id="911f0-285">Pricing</span></span>
<span data-ttu-id="911f0-286">Azure Batch je založený na technologii cloudových služeb Azure a virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="911f0-286">Azure Batch is built on Azure Cloud Services and Azure Virtual Machines technology.</span></span> <span data-ttu-id="911f0-287">Hello samotnou službu Batch se nabízí zadarmo, což znamená, budou se vám účtovat pouze pro hello výpočetní prostředky, které využívají řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="911f0-287">hello Batch service itself is offered at no cost, which means you are charged only for hello compute resources that your Batch solutions consume.</span></span> <span data-ttu-id="911f0-288">Pokud vyberete **konfigurace cloudových služeb**, budou se vám účtovat podle hello [ceny cloudové služby] [ cloud_services_pricing] struktury.</span><span class="sxs-lookup"><span data-stu-id="911f0-288">When you choose **Cloud Services Configuration**, you are charged based on hello [Cloud Services pricing][cloud_services_pricing] structure.</span></span> <span data-ttu-id="911f0-289">Pokud vyberete **konfigurace virtuálního počítače**, budou se vám účtovat podle hello [ceny virtuálních počítačů] [ vm_pricing] struktury.</span><span class="sxs-lookup"><span data-stu-id="911f0-289">When you choose **Virtual Machine Configuration**, you are charged based on hello [Virtual Machines pricing][vm_pricing] structure.</span></span> 

<span data-ttu-id="911f0-290">Pokud nasazujete aplikace tooyour Batch uzlů pomocí [balíčky aplikací](batch-application-packages.md), se účtují poplatky pro prostředky úložiště Azure hello, balíčky aplikací používat.</span><span class="sxs-lookup"><span data-stu-id="911f0-290">If you deploy applications tooyour Batch nodes using [application packages](batch-application-packages.md), you are also charged for hello Azure Storage resources that your application packages consume.</span></span> <span data-ttu-id="911f0-291">Obecně platí náklady na úložiště Azure hello jsou minimální.</span><span class="sxs-lookup"><span data-stu-id="911f0-291">In general, hello Azure Storage costs are minimal.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="911f0-292">Další kroky</span><span class="sxs-lookup"><span data-stu-id="911f0-292">Next steps</span></span>
### <a name="batch-python-tutorial"></a><span data-ttu-id="911f0-293">Kurz k Batch Pythonu</span><span class="sxs-lookup"><span data-stu-id="911f0-293">Batch Python tutorial</span></span>
<span data-ttu-id="911f0-294">Pro více podrobný kurz o toowork službou Batch pomocí Python, podívejte se na [Začínáme s klientem Azure Batch Python hello](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="911f0-294">For a more in-depth tutorial about how toowork with Batch by using Python, check out [Get started with hello Azure Batch Python client](batch-python-tutorial.md).</span></span> <span data-ttu-id="911f0-295">Jeho doprovodné [ukázka kódu] [ github_samples_pyclient] zahrnuje podpůrná funkce `get_vm_config_for_distro`, který ukazuje další tooobtain technika konfiguraci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="911f0-295">Its companion [code sample][github_samples_pyclient] includes a helper function, `get_vm_config_for_distro`, that shows another technique tooobtain a virtual machine configuration.</span></span>

### <a name="batch-python-code-samples"></a><span data-ttu-id="911f0-296">Ukázek kódu služby batch Python</span><span class="sxs-lookup"><span data-stu-id="911f0-296">Batch Python code samples</span></span>
<span data-ttu-id="911f0-297">Hello [ukázky kódu jsou Python] [ github_samples_py] v hello [azure-batch-samples] [ github_samples] úložišti na Githubu obsahovat skripty, které ukazují, jak tooperform běžné operace Batch, třeba fond, úlohy a vytváření úlohy.</span><span class="sxs-lookup"><span data-stu-id="911f0-297">hello [Python code samples][github_samples_py] in hello [azure-batch-samples][github_samples] repository on GitHub contain scripts that show you how tooperform common Batch operations, such as pool, job, and task creation.</span></span> <span data-ttu-id="911f0-298">Hello [README] [ github_py_readme] doprovodný hello Python ukázky obsahuje podrobnosti o tom, jak tooinstall hello požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="911f0-298">hello [README][github_py_readme] that accompanies hello Python samples has details about how tooinstall hello required packages.</span></span>

### <a name="batch-forum"></a><span data-ttu-id="911f0-299">Fórum k službě Batch</span><span class="sxs-lookup"><span data-stu-id="911f0-299">Batch forum</span></span>
<span data-ttu-id="911f0-300">Hello [fóru služby Azure Batch] [ forum] na webu MSDN je skvělá umístit toodiscuss Batch a klást otázky týkající se služby hello.</span><span class="sxs-lookup"><span data-stu-id="911f0-300">hello [Azure Batch Forum][forum] on MSDN is a great place toodiscuss Batch and ask questions about hello service.</span></span> <span data-ttu-id="911f0-301">Užitečné pro čtení "připnutý" účtuje a zveřejněte svoje otázky, kterým dochází při sestavování řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="911f0-301">Read helpful "pinned" posts, and post your questions as they arise while you build your Batch solutions.</span></span>

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
