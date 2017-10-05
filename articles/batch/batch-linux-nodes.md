---
title: "Na virtuálním počítači spusťte Linuxových výpočetních uzlů - Azure Batch | Microsoft Docs"
description: "Zjistěte, jak zpracovat paralelní výpočty úlohy na fondech virtuálních počítačích s Linuxem v Azure Batch."
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
ms.openlocfilehash: 9b2257917e2368478beb75957677de23d4157865
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a><span data-ttu-id="c35fd-103">Zřídit Linux výpočetních uzlů ve fondech Batch</span><span class="sxs-lookup"><span data-stu-id="c35fd-103">Provision Linux compute nodes in Batch pools</span></span>

<span data-ttu-id="c35fd-104">Azure Batch můžete spouštět úlohy paralelní výpočty u virtuálních počítačů Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="c35fd-104">You can use Azure Batch to run parallel compute workloads on both Linux and Windows virtual machines.</span></span> <span data-ttu-id="c35fd-105">Tento článek popisuje, jak vytvořit fondy Linuxových výpočetních uzlů ve službě Batch pomocí obou [Batch Python] [ py_batch_package] a [Batch .NET] [ api_net] knihovny klienta.</span><span class="sxs-lookup"><span data-stu-id="c35fd-105">This article details how to create pools of Linux compute nodes in the Batch service by using both the [Batch Python][py_batch_package] and [Batch .NET][api_net] client libraries.</span></span>

> [!NOTE]
> <span data-ttu-id="c35fd-106">Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017.</span><span class="sxs-lookup"><span data-stu-id="c35fd-106">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="c35fd-107">Ve fondech služby Batch vytvořených mezi 10. březnem 2016 a 5. červencem 2017 jsou podporované, pouze pokud byl fond vytvořen pomocí konfigurace cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c35fd-107">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if the pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="c35fd-108">Fondy služby Batch vytvořené před 10. březnem 2016 nepodporují balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="c35fd-108">Batch pools created prior to 10 March 2016 do not support application packages.</span></span> <span data-ttu-id="c35fd-109">Další informace o používání balíčků aplikací k nasazení aplikací do uzlů služby Batch najdete v tématu [Nasazení aplikací do výpočetních uzlů pomocí balíčků aplikací služby Batch](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="c35fd-109">For more information about using application packages to deploy your applications to your Batch nodes, see [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md).</span></span>
>
>

## <a name="virtual-machine-configuration"></a><span data-ttu-id="c35fd-110">Konfigurace virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c35fd-110">Virtual machine configuration</span></span>
<span data-ttu-id="c35fd-111">Když vytvoříte fond výpočetních uzlů ve službě Batch, máte dvě možnosti, ze kterého můžete vybrat velikost uzlu a operační systém: Konfigurace cloudových služeb a konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c35fd-111">When you create a pool of compute nodes in Batch, you have two options from which to select the node size and operating system: Cloud Services Configuration and Virtual Machine Configuration.</span></span>

<span data-ttu-id="c35fd-112">**Konfigurace služby Cloud Services** poskytuje *pouze* výpočetní uzly Windows.</span><span class="sxs-lookup"><span data-stu-id="c35fd-112">**Cloud Services Configuration** provides Windows compute nodes *only*.</span></span> <span data-ttu-id="c35fd-113">K dispozici výpočetní uzel velikosti jsou uvedeny v [velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md), a dostupných operačních systémů, jsou uvedeny v [verze hostovaného operačního systému Azure a kompatibilních sad SDK](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="c35fd-113">Available compute node sizes are listed in [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md), and available operating systems are listed in the [Azure Guest OS releases and SDK compatibility matrix](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span> <span data-ttu-id="c35fd-114">Když vytvoříte fond, který obsahuje uzly Azure Cloud Services, je třeba zadat velikost uzlu řada operačních systémů, které jsou popsány v výše uvedených článcích.</span><span class="sxs-lookup"><span data-stu-id="c35fd-114">When you create a pool that contains Azure Cloud Services nodes, you specify the node size and the OS family, which are described in the previously mentioned articles.</span></span> <span data-ttu-id="c35fd-115">Pro fondy Windows výpočetní uzly, se nejčastěji používá cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c35fd-115">For pools of Windows compute nodes, Cloud Services is most commonly used.</span></span>

<span data-ttu-id="c35fd-116">**Konfigurace virtuálního počítače** poskytuje, Linux a Windows Image pro výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="c35fd-116">**Virtual Machine Configuration** provides both Linux and Windows images for compute nodes.</span></span> <span data-ttu-id="c35fd-117">K dispozici výpočetní uzel velikosti jsou uvedeny v [velikosti virtuálních počítačů v Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) a [velikosti virtuálních počítačů v Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span><span class="sxs-lookup"><span data-stu-id="c35fd-117">Available compute node sizes are listed in [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) and [Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span></span> <span data-ttu-id="c35fd-118">Když vytvoříte fond, který obsahuje uzly konfigurace virtuálního počítače, je nutné zadat velikost uzlů, odkaz na obrázek virtuálního počítače a agenta uzlu Batch SKU být nainstalovány na uzlu.</span><span class="sxs-lookup"><span data-stu-id="c35fd-118">When you create a pool that contains Virtual Machine Configuration nodes, you must specify the size of the nodes, the virtual machine image reference, and the Batch node agent SKU to be installed on the nodes.</span></span>

### <a name="virtual-machine-image-reference"></a><span data-ttu-id="c35fd-119">Odkaz na obrázek virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c35fd-119">Virtual machine image reference</span></span>
<span data-ttu-id="c35fd-120">Použití služby Batch [sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) zajistit Linuxových výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="c35fd-120">The Batch service uses [virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) to provide Linux compute nodes.</span></span> <span data-ttu-id="c35fd-121">Můžete zadat bitovou kopii [Azure Marketplace][vm_marketplace], nebo zadejte vlastní obrázek, který jste připravili.</span><span class="sxs-lookup"><span data-stu-id="c35fd-121">You can specify an image from the [Azure Marketplace][vm_marketplace], or provide a custom image that you have prepared.</span></span> <span data-ttu-id="c35fd-122">Další informace o vlastních imagích najdete v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md#pool).</span><span class="sxs-lookup"><span data-stu-id="c35fd-122">For more details about custom images, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#pool).</span></span>

<span data-ttu-id="c35fd-123">Když konfigurujete odkaz bitové kopie virtuálního počítače, můžete zadat vlastnosti na bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c35fd-123">When you configure a virtual machine image reference, you specify the properties of the virtual machine image.</span></span> <span data-ttu-id="c35fd-124">Následující vlastnosti jsou požadovány při vytvoření odkaz na obrázek virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="c35fd-124">The following properties are required when you create a virtual machine image reference:</span></span>

| <span data-ttu-id="c35fd-125">**Vlastnosti referenční bitové kopie**</span><span class="sxs-lookup"><span data-stu-id="c35fd-125">**Image reference properties**</span></span> | <span data-ttu-id="c35fd-126">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="c35fd-126">**Example**</span></span> |
| --- | --- |
| <span data-ttu-id="c35fd-127">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="c35fd-127">Publisher</span></span> |<span data-ttu-id="c35fd-128">Canonical</span><span class="sxs-lookup"><span data-stu-id="c35fd-128">Canonical</span></span> |
| <span data-ttu-id="c35fd-129">Nabídka</span><span class="sxs-lookup"><span data-stu-id="c35fd-129">Offer</span></span> |<span data-ttu-id="c35fd-130">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-130">UbuntuServer</span></span> |
| <span data-ttu-id="c35fd-131">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="c35fd-131">SKU</span></span> |<span data-ttu-id="c35fd-132">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="c35fd-132">14.04.4-LTS</span></span> |
| <span data-ttu-id="c35fd-133">Verze</span><span class="sxs-lookup"><span data-stu-id="c35fd-133">Version</span></span> |<span data-ttu-id="c35fd-134">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-134">latest</span></span> |

> [!TIP]
> <span data-ttu-id="c35fd-135">Další informace o těchto vlastností a jak zobrazit Marketplace obrázků v [vyhledání a vyberte Image virtuálních počítačů Linux v Azure pomocí rozhraní CLI nebo Powershellu](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c35fd-135">You can learn more about these properties and how to list Marketplace images in [Navigate and select Linux virtual machine images in Azure with CLI or PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="c35fd-136">Upozorňujeme, že ne všechny bitové kopie Marketplace jsou momentálně kompatibilní s Batch.</span><span class="sxs-lookup"><span data-stu-id="c35fd-136">Note that not all Marketplace images are currently compatible with Batch.</span></span> <span data-ttu-id="c35fd-137">Další informace najdete v tématu [uzlu agenta SKU](#node-agent-sku).</span><span class="sxs-lookup"><span data-stu-id="c35fd-137">For more information, see [Node agent SKU](#node-agent-sku).</span></span>
>
>

### <a name="node-agent-sku"></a><span data-ttu-id="c35fd-138">Uzel agenta SKU</span><span class="sxs-lookup"><span data-stu-id="c35fd-138">Node agent SKU</span></span>
<span data-ttu-id="c35fd-139">Agent uzlu Batch je program, který běží na každém uzlu ve fondu a poskytuje rozhraní příkazu a řízení mezi uzlu a služby Batch.</span><span class="sxs-lookup"><span data-stu-id="c35fd-139">The Batch node agent is a program that runs on each node in the pool and provides the command-and-control interface between the node and the Batch service.</span></span> <span data-ttu-id="c35fd-140">Existují různé implementace uzlu agenta, označuje jako SKU, pro různé operační systémy.</span><span class="sxs-lookup"><span data-stu-id="c35fd-140">There are different implementations of the node agent, known as SKUs, for different operating systems.</span></span> <span data-ttu-id="c35fd-141">V podstatě při vytváření konfigurace virtuálního počítače, nejprve zadat odkaz na obrázek virtuálního počítače a pak zadejte uzlu agenta k instalaci na bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="c35fd-141">Essentially, when you create a Virtual Machine Configuration, you first specify the virtual machine image reference, and then you specify the node agent to install on the image.</span></span> <span data-ttu-id="c35fd-142">Obvykle každého uzlu agenta SKU je kompatibilní s více bitových kopií virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c35fd-142">Typically, each node agent SKU is compatible with multiple virtual machine images.</span></span> <span data-ttu-id="c35fd-143">Tady je několik příkladů uzlu agenta SKU:</span><span class="sxs-lookup"><span data-stu-id="c35fd-143">Here are a few examples of node agent SKUs:</span></span>

* <span data-ttu-id="c35fd-144">batch.Node.Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="c35fd-144">batch.node.ubuntu 14.04</span></span>
* <span data-ttu-id="c35fd-145">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="c35fd-145">batch.node.centos 7</span></span>
* <span data-ttu-id="c35fd-146">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="c35fd-146">batch.node.windows amd64</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c35fd-147">Ne všechny bitové kopie virtuálních počítačů, které jsou k dispozici na webu Marketplace jsou kompatibilní s aktuálně k dispozici agenty uzlu Batch.</span><span class="sxs-lookup"><span data-stu-id="c35fd-147">Not all virtual machine images that are available in the Marketplace are compatible with the currently available Batch node agents.</span></span> <span data-ttu-id="c35fd-148">Pomocí sady SDK služby Batch seznam agent k dispozici uzel SKU a bitové kopie virtuálních počítačů, ke kterým jsou kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="c35fd-148">Use the Batch SDKs to list the available node agent SKUs and the virtual machine images with which they are compatible.</span></span> <span data-ttu-id="c35fd-149">Najdete v článku [bitové kopie seznamu virtuálních počítačů](#list-of-virtual-machine-images) dále v tomto článku Další příklady a další informace o tom, jak načíst seznam platných obrázků za běhu.</span><span class="sxs-lookup"><span data-stu-id="c35fd-149">See the [List of Virtual Machine images](#list-of-virtual-machine-images) later in this article for more information and examples of how to retrieve a list of valid images at runtime.</span></span>
>
>

## <a name="create-a-linux-pool-batch-python"></a><span data-ttu-id="c35fd-150">Vytvoření fondu Linux: Batch Python</span><span class="sxs-lookup"><span data-stu-id="c35fd-150">Create a Linux pool: Batch Python</span></span>
<span data-ttu-id="c35fd-151">Následující fragment kódu ukazuje příklad použití [Microsoft Azure Batch klientské knihovny pro jazyk Python] [ py_batch_package] vytvořit fond Ubuntu Server výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="c35fd-151">The following code snippet shows an example of how to use the [Microsoft Azure Batch Client Library for Python][py_batch_package] to create a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="c35fd-152">Referenční dokumentace pro modulu Batch Python najdete na [azure.batch balíček] [ py_batch_docs] na dokumentaci pro čtení.</span><span class="sxs-lookup"><span data-stu-id="c35fd-152">Reference documentation for the Batch Python module can be found at [azure.batch package][py_batch_docs] on Read the Docs.</span></span>

<span data-ttu-id="c35fd-153">Vytvoří tento fragment kódu [elementu ImageReference] [ py_imagereference] explicitně a určí, každý z jeho vlastnosti (vydavatel, nabídku, SKU a verzi).</span><span class="sxs-lookup"><span data-stu-id="c35fd-153">This snippet creates an [ImageReference][py_imagereference] explicitly and specifies each of its properties (publisher, offer, SKU, version).</span></span> <span data-ttu-id="c35fd-154">V produkčním kódu, ale doporučujeme použít [list_node_agent_skus] [ py_list_skus] metoda, abyste zjistili a vyberte z dostupných bitové kopie a uzlu agenta SKU kombinace za běhu.</span><span class="sxs-lookup"><span data-stu-id="c35fd-154">In production code, however, we recommend that you use the [list_node_agent_skus][py_list_skus] method to determine and select from the available image and node agent SKU combinations at runtime.</span></span>

```python
# Import the required modules from the
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

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

<span data-ttu-id="c35fd-155">Jak je uvedeno nahoře, doporučujeme místo vytvoření [elementu ImageReference] [ py_imagereference] explicitně, použijte [list_node_agent_skus] [ py_list_skus] metoda dynamicky vybrat z kombinace bitovou kopii agenta nebo Marketplace aktuálně podporované uzlu.</span><span class="sxs-lookup"><span data-stu-id="c35fd-155">As mentioned previously, we recommend that instead of creating the [ImageReference][py_imagereference] explicitly, you use the [list_node_agent_skus][py_list_skus] method to dynamically select from the currently supported node agent/Marketplace image combinations.</span></span> <span data-ttu-id="c35fd-156">Následující fragment kódu Python ukazuje, jak použít tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="c35fd-156">The following Python snippet shows how to use this method.</span></span>

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a><span data-ttu-id="c35fd-157">Vytvoření fondu Linux: Batch .NET</span><span class="sxs-lookup"><span data-stu-id="c35fd-157">Create a Linux pool: Batch .NET</span></span>
<span data-ttu-id="c35fd-158">Následující fragment kódu ukazuje příklad použití [Batch .NET] [ nuget_batch_net] klientské knihovny vytvořit fond Ubuntu Server výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="c35fd-158">The following code snippet shows an example of how to use the [Batch .NET][nuget_batch_net] client library to create a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="c35fd-159">Můžete najít [Batch .NET referenční dokumentaci k nástroji] [ api_net] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="c35fd-159">You can find the [Batch .NET reference documentation][api_net] on MSDN.</span></span>

<span data-ttu-id="c35fd-160">Následující kód používá fragment kódu [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metoda vyberte ze seznamu aktuálně podporované kombinace SKU agenta Marketplace pro bitové kopie a uzlu.</span><span class="sxs-lookup"><span data-stu-id="c35fd-160">The following code snippet uses the [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method to select from the list of currently supported Marketplace image and node agent SKU combinations.</span></span> <span data-ttu-id="c35fd-161">Tento postup je žádoucí, protože seznam podporovaných kombinací může občas změnit.</span><span class="sxs-lookup"><span data-stu-id="c35fd-161">This technique is desirable because the list of supported combinations may change from time to time.</span></span> <span data-ttu-id="c35fd-162">Nejčastěji jsou přidány podporovaných kombinací.</span><span class="sxs-lookup"><span data-stu-id="c35fd-162">Most commonly, supported combinations are added.</span></span>

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit the pool to the Batch service
await pool.CommitAsync();
```

<span data-ttu-id="c35fd-163">I když fragmentu předchozí používá [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metodu pro dynamicky seznam a vyberte z podporované bitové kopie a uzlu agenta SKU kombinace (doporučeno), můžete také nakonfigurovat [elementu ImageReference] [ net_imagereference] explicitně:</span><span class="sxs-lookup"><span data-stu-id="c35fd-163">Although the previous snippet uses the [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method to dynamically list and select from supported image and node agent SKU combinations (recommended), you can also configure an [ImageReference][net_imagereference] explicitly:</span></span>

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a><span data-ttu-id="c35fd-164">Seznam bitové kopie virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c35fd-164">List of virtual machine images</span></span>
<span data-ttu-id="c35fd-165">Následující tabulka uvádí Marketplace Image virtuálních počítačů, které jsou kompatibilní s dostupných agentů uzlu Batch při poslední aktualizace v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c35fd-165">The following table lists the Marketplace virtual machine images that are compatible with the available Batch node agents when this article was last updated.</span></span> <span data-ttu-id="c35fd-166">Je důležité si uvědomit, že tento seznam není spolehlivý, protože bitové kopie a agenty uzlu může přidat nebo odebrat kdykoli.</span><span class="sxs-lookup"><span data-stu-id="c35fd-166">It is important to note that this list is not definitive because images and node agents may be added or removed at any time.</span></span> <span data-ttu-id="c35fd-167">Doporučujeme vaší aplikací a služeb Batch vždy použít [list_node_agent_skus] [ py_list_skus] (Python) a [ListNodeAgentSkus] [ net_list_skus] (Batch .NET), abyste zjistili a vyberte z aktuálně dostupné edice.</span><span class="sxs-lookup"><span data-stu-id="c35fd-167">We recommend that your Batch applications and services always use [list_node_agent_skus][py_list_skus] (Python) and [ListNodeAgentSkus][net_list_skus] (Batch .NET) to determine and select from the currently available SKUs.</span></span>

> [!WARNING]
> <span data-ttu-id="c35fd-168">V následujícím seznamu může kdykoli změnit.</span><span class="sxs-lookup"><span data-stu-id="c35fd-168">The following list may change at any time.</span></span> <span data-ttu-id="c35fd-169">Vždy nutné použít **agent uzlu seznamu SKU** metody, které jsou k dispozici v rozhraní API služby Batch k zobrazení seznamu kompatibilní virtuální počítač a uzlu agenta SKU při spuštění úlohy Batch.</span><span class="sxs-lookup"><span data-stu-id="c35fd-169">Always use the **list node agent SKU** methods available in the Batch APIs to list the compatible virtual machine and node agent SKUs when you run your Batch jobs.</span></span>
>
>

| <span data-ttu-id="c35fd-170">**Vydavatele**</span><span class="sxs-lookup"><span data-stu-id="c35fd-170">**Publisher**</span></span> | <span data-ttu-id="c35fd-171">**Nabídka**</span><span class="sxs-lookup"><span data-stu-id="c35fd-171">**Offer**</span></span> | <span data-ttu-id="c35fd-172">**Obrázek SKU**</span><span class="sxs-lookup"><span data-stu-id="c35fd-172">**Image SKU**</span></span> | <span data-ttu-id="c35fd-173">**Verze**</span><span class="sxs-lookup"><span data-stu-id="c35fd-173">**Version**</span></span> | <span data-ttu-id="c35fd-174">**Uzel agenta SKU ID**</span><span class="sxs-lookup"><span data-stu-id="c35fd-174">**Node agent SKU ID**</span></span> |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| <span data-ttu-id="c35fd-175">Canonical</span><span class="sxs-lookup"><span data-stu-id="c35fd-175">Canonical</span></span> | <span data-ttu-id="c35fd-176">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-176">UbuntuServer</span></span> | <span data-ttu-id="c35fd-177">14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="c35fd-177">14.04.5-LTS</span></span> | <span data-ttu-id="c35fd-178">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-178">latest</span></span> | <span data-ttu-id="c35fd-179">batch.Node.Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="c35fd-179">batch.node.ubuntu 14.04</span></span> |
| <span data-ttu-id="c35fd-180">Canonical</span><span class="sxs-lookup"><span data-stu-id="c35fd-180">Canonical</span></span> | <span data-ttu-id="c35fd-181">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-181">UbuntuServer</span></span> | <span data-ttu-id="c35fd-182">16.04.0-LTS</span><span class="sxs-lookup"><span data-stu-id="c35fd-182">16.04.0-LTS</span></span> | <span data-ttu-id="c35fd-183">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-183">latest</span></span> | <span data-ttu-id="c35fd-184">batch.Node.Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="c35fd-184">batch.node.ubuntu 16.04</span></span> |
| <span data-ttu-id="c35fd-185">Credativ</span><span class="sxs-lookup"><span data-stu-id="c35fd-185">Credativ</span></span> | <span data-ttu-id="c35fd-186">Debian</span><span class="sxs-lookup"><span data-stu-id="c35fd-186">Debian</span></span> | <span data-ttu-id="c35fd-187">8</span><span class="sxs-lookup"><span data-stu-id="c35fd-187">8</span></span> | <span data-ttu-id="c35fd-188">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-188">latest</span></span> | <span data-ttu-id="c35fd-189">batch.Node.debian 8</span><span class="sxs-lookup"><span data-stu-id="c35fd-189">batch.node.debian 8</span></span> |
| <span data-ttu-id="c35fd-190">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="c35fd-190">OpenLogic</span></span> | <span data-ttu-id="c35fd-191">CentOS</span><span class="sxs-lookup"><span data-stu-id="c35fd-191">CentOS</span></span> | <span data-ttu-id="c35fd-192">7.0</span><span class="sxs-lookup"><span data-stu-id="c35fd-192">7.0</span></span> | <span data-ttu-id="c35fd-193">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-193">latest</span></span> | <span data-ttu-id="c35fd-194">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="c35fd-194">batch.node.centos 7</span></span> |
| <span data-ttu-id="c35fd-195">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="c35fd-195">OpenLogic</span></span> | <span data-ttu-id="c35fd-196">CentOS</span><span class="sxs-lookup"><span data-stu-id="c35fd-196">CentOS</span></span> | <span data-ttu-id="c35fd-197">7.1</span><span class="sxs-lookup"><span data-stu-id="c35fd-197">7.1</span></span> | <span data-ttu-id="c35fd-198">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-198">latest</span></span> | <span data-ttu-id="c35fd-199">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="c35fd-199">batch.node.centos 7</span></span> |
| <span data-ttu-id="c35fd-200">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="c35fd-200">OpenLogic</span></span> | <span data-ttu-id="c35fd-201">CentOS HPC</span><span class="sxs-lookup"><span data-stu-id="c35fd-201">CentOS-HPC</span></span> | <span data-ttu-id="c35fd-202">7.1</span><span class="sxs-lookup"><span data-stu-id="c35fd-202">7.1</span></span> | <span data-ttu-id="c35fd-203">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-203">latest</span></span> | <span data-ttu-id="c35fd-204">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="c35fd-204">batch.node.centos 7</span></span> |
| <span data-ttu-id="c35fd-205">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="c35fd-205">OpenLogic</span></span> | <span data-ttu-id="c35fd-206">CentOS</span><span class="sxs-lookup"><span data-stu-id="c35fd-206">CentOS</span></span> | <span data-ttu-id="c35fd-207">7.2</span><span class="sxs-lookup"><span data-stu-id="c35fd-207">7.2</span></span> | <span data-ttu-id="c35fd-208">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-208">latest</span></span> | <span data-ttu-id="c35fd-209">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="c35fd-209">batch.node.centos 7</span></span> |
| <span data-ttu-id="c35fd-210">Oracle</span><span class="sxs-lookup"><span data-stu-id="c35fd-210">Oracle</span></span> | <span data-ttu-id="c35fd-211">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="c35fd-211">Oracle-Linux</span></span> | <span data-ttu-id="c35fd-212">7.0</span><span class="sxs-lookup"><span data-stu-id="c35fd-212">7.0</span></span> | <span data-ttu-id="c35fd-213">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-213">latest</span></span> | <span data-ttu-id="c35fd-214">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="c35fd-214">batch.node.centos 7</span></span> |
| <span data-ttu-id="c35fd-215">Oracle</span><span class="sxs-lookup"><span data-stu-id="c35fd-215">Oracle</span></span> | <span data-ttu-id="c35fd-216">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="c35fd-216">Oracle-Linux</span></span> | <span data-ttu-id="c35fd-217">7.2</span><span class="sxs-lookup"><span data-stu-id="c35fd-217">7.2</span></span> | <span data-ttu-id="c35fd-218">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-218">latest</span></span> | <span data-ttu-id="c35fd-219">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="c35fd-219">batch.node.centos 7</span></span> |
| <span data-ttu-id="c35fd-220">SUSE</span><span class="sxs-lookup"><span data-stu-id="c35fd-220">SUSE</span></span> | <span data-ttu-id="c35fd-221">openSUSE</span><span class="sxs-lookup"><span data-stu-id="c35fd-221">openSUSE</span></span> | <span data-ttu-id="c35fd-222">13.2</span><span class="sxs-lookup"><span data-stu-id="c35fd-222">13.2</span></span> | <span data-ttu-id="c35fd-223">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-223">latest</span></span> | <span data-ttu-id="c35fd-224">batch.Node.opensuse 13.2</span><span class="sxs-lookup"><span data-stu-id="c35fd-224">batch.node.opensuse 13.2</span></span> |
| <span data-ttu-id="c35fd-225">SUSE</span><span class="sxs-lookup"><span data-stu-id="c35fd-225">SUSE</span></span> | <span data-ttu-id="c35fd-226">openSUSE přestupného</span><span class="sxs-lookup"><span data-stu-id="c35fd-226">openSUSE-Leap</span></span> | <span data-ttu-id="c35fd-227">42.1</span><span class="sxs-lookup"><span data-stu-id="c35fd-227">42.1</span></span> | <span data-ttu-id="c35fd-228">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-228">latest</span></span> | <span data-ttu-id="c35fd-229">batch.Node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="c35fd-229">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="c35fd-230">SUSE</span><span class="sxs-lookup"><span data-stu-id="c35fd-230">SUSE</span></span> | <span data-ttu-id="c35fd-231">SLES</span><span class="sxs-lookup"><span data-stu-id="c35fd-231">SLES</span></span> | <span data-ttu-id="c35fd-232">12-SP1</span><span class="sxs-lookup"><span data-stu-id="c35fd-232">12-SP1</span></span> | <span data-ttu-id="c35fd-233">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-233">latest</span></span> | <span data-ttu-id="c35fd-234">batch.Node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="c35fd-234">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="c35fd-235">SUSE</span><span class="sxs-lookup"><span data-stu-id="c35fd-235">SUSE</span></span> | <span data-ttu-id="c35fd-236">SLES HPC</span><span class="sxs-lookup"><span data-stu-id="c35fd-236">SLES-HPC</span></span> | <span data-ttu-id="c35fd-237">12-SP1</span><span class="sxs-lookup"><span data-stu-id="c35fd-237">12-SP1</span></span> | <span data-ttu-id="c35fd-238">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-238">latest</span></span> | <span data-ttu-id="c35fd-239">batch.Node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="c35fd-239">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="c35fd-240">Microsoft služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="c35fd-240">microsoft-ads</span></span> | <span data-ttu-id="c35fd-241">data-vědecké účely-virtuálního počítače s linuxem</span><span class="sxs-lookup"><span data-stu-id="c35fd-241">linux-data-science-vm</span></span> | <span data-ttu-id="c35fd-242">linuxdsvm</span><span class="sxs-lookup"><span data-stu-id="c35fd-242">linuxdsvm</span></span> | <span data-ttu-id="c35fd-243">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-243">latest</span></span> | <span data-ttu-id="c35fd-244">batch.Node.centos 7</span><span class="sxs-lookup"><span data-stu-id="c35fd-244">batch.node.centos 7</span></span> |
| <span data-ttu-id="c35fd-245">Microsoft služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="c35fd-245">microsoft-ads</span></span> | <span data-ttu-id="c35fd-246">Standard-data-vědecké účely vm</span><span class="sxs-lookup"><span data-stu-id="c35fd-246">standard-data-science-vm</span></span> | <span data-ttu-id="c35fd-247">Standard-data-vědecké účely vm</span><span class="sxs-lookup"><span data-stu-id="c35fd-247">standard-data-science-vm</span></span> | <span data-ttu-id="c35fd-248">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-248">latest</span></span> | <span data-ttu-id="c35fd-249">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="c35fd-249">batch.node.windows amd64</span></span> |
| <span data-ttu-id="c35fd-250">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-250">MicrosoftWindowsServer</span></span> | <span data-ttu-id="c35fd-251">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-251">WindowsServer</span></span> | <span data-ttu-id="c35fd-252">AKTUALIZACE SP1 2008 R2</span><span class="sxs-lookup"><span data-stu-id="c35fd-252">2008-R2-SP1</span></span> | <span data-ttu-id="c35fd-253">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-253">latest</span></span> | <span data-ttu-id="c35fd-254">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="c35fd-254">batch.node.windows amd64</span></span> |
| <span data-ttu-id="c35fd-255">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-255">MicrosoftWindowsServer</span></span> | <span data-ttu-id="c35fd-256">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-256">WindowsServer</span></span> | <span data-ttu-id="c35fd-257">2012-Datacenter</span><span class="sxs-lookup"><span data-stu-id="c35fd-257">2012-Datacenter</span></span> | <span data-ttu-id="c35fd-258">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-258">latest</span></span> | <span data-ttu-id="c35fd-259">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="c35fd-259">batch.node.windows amd64</span></span> |
| <span data-ttu-id="c35fd-260">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-260">MicrosoftWindowsServer</span></span> | <span data-ttu-id="c35fd-261">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-261">WindowsServer</span></span> | <span data-ttu-id="c35fd-262">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="c35fd-262">2012-R2-Datacenter</span></span> | <span data-ttu-id="c35fd-263">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-263">latest</span></span> | <span data-ttu-id="c35fd-264">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="c35fd-264">batch.node.windows amd64</span></span> |
| <span data-ttu-id="c35fd-265">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-265">MicrosoftWindowsServer</span></span> | <span data-ttu-id="c35fd-266">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-266">WindowsServer</span></span> | <span data-ttu-id="c35fd-267">2016 Datacenter</span><span class="sxs-lookup"><span data-stu-id="c35fd-267">2016-Datacenter</span></span> | <span data-ttu-id="c35fd-268">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-268">latest</span></span> | <span data-ttu-id="c35fd-269">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="c35fd-269">batch.node.windows amd64</span></span> |
| <span data-ttu-id="c35fd-270">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-270">MicrosoftWindowsServer</span></span> | <span data-ttu-id="c35fd-271">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="c35fd-271">WindowsServer</span></span> | <span data-ttu-id="c35fd-272">2016 datového centra s kontejnery</span><span class="sxs-lookup"><span data-stu-id="c35fd-272">2016-Datacenter-with-Containers</span></span> | <span data-ttu-id="c35fd-273">nejnovější</span><span class="sxs-lookup"><span data-stu-id="c35fd-273">latest</span></span> | <span data-ttu-id="c35fd-274">batch.Node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="c35fd-274">batch.node.windows amd64</span></span> |

## <a name="connect-to-linux-nodes-using-ssh"></a><span data-ttu-id="c35fd-275">Připojení k Linuxových uzlů pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="c35fd-275">Connect to Linux nodes using SSH</span></span>
<span data-ttu-id="c35fd-276">Během vývoje nebo při řešení potíží možná bude nutné se přihlásit k uzly ve fondu.</span><span class="sxs-lookup"><span data-stu-id="c35fd-276">During development or while troubleshooting, you may find it necessary to sign in to the nodes in your pool.</span></span> <span data-ttu-id="c35fd-277">Na rozdíl od Windows výpočetní uzly nemůžete použít protokol RDP (Remote Desktop) pro připojení k Linuxových uzlů.</span><span class="sxs-lookup"><span data-stu-id="c35fd-277">Unlike Windows compute nodes, you cannot use Remote Desktop Protocol (RDP) to connect to Linux nodes.</span></span> <span data-ttu-id="c35fd-278">Místo toho služba Batch umožňuje přístup k SSH na každém uzlu vzdáleného připojení.</span><span class="sxs-lookup"><span data-stu-id="c35fd-278">Instead, the Batch service enables SSH access on each node for remote connection.</span></span>

<span data-ttu-id="c35fd-279">Následující fragment kódu Python vytvoří uživatele na každém uzlu ve fondu, který je vyžadován pro připojení ke vzdálené.</span><span class="sxs-lookup"><span data-stu-id="c35fd-279">The following Python code snippet creates a user on each node in a pool, which is required for remote connection.</span></span> <span data-ttu-id="c35fd-280">Potom zobrazí informace o připojení zabezpečené shell (SSH) pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="c35fd-280">It then prints the secure shell (SSH) connection information for each node.</span></span>

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

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
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

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

<span data-ttu-id="c35fd-281">Tady je ukázkový výstup pro předchozí kód pro fond, který obsahuje čtyři uzly Linux:</span><span class="sxs-lookup"><span data-stu-id="c35fd-281">Here is sample output for the previous code for a pool that contains four Linux nodes:</span></span>

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

<span data-ttu-id="c35fd-282">Namísto hesla můžete zadat veřejný klíč SSH při vytváření uživatele na uzlu.</span><span class="sxs-lookup"><span data-stu-id="c35fd-282">Instead of a password, you can specify an SSH public key when you create a user on a node.</span></span> <span data-ttu-id="c35fd-283">V sadě SDK pro Python, použijte **ssh_public_key** parametr na [ComputeNodeUser][py_computenodeuser].</span><span class="sxs-lookup"><span data-stu-id="c35fd-283">In the Python SDK, use the **ssh_public_key** parameter on [ComputeNodeUser][py_computenodeuser].</span></span> <span data-ttu-id="c35fd-284">V rozhraní .NET, použijte [ComputeNodeUser][net_computenodeuser].[ Parametru SshPublicKey] [ net_ssh_key] vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c35fd-284">In .NET, use the [ComputeNodeUser][net_computenodeuser].[SshPublicKey][net_ssh_key] property.</span></span>

## <a name="pricing"></a><span data-ttu-id="c35fd-285">Ceny</span><span class="sxs-lookup"><span data-stu-id="c35fd-285">Pricing</span></span>
<span data-ttu-id="c35fd-286">Azure Batch je založený na technologii cloudových služeb Azure a virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="c35fd-286">Azure Batch is built on Azure Cloud Services and Azure Virtual Machines technology.</span></span> <span data-ttu-id="c35fd-287">Služba Batch samotný se nabízí zadarmo, což znamená, že budou účtovat pouze pro výpočetní prostředky, vaše řešení Batch využívat.</span><span class="sxs-lookup"><span data-stu-id="c35fd-287">The Batch service itself is offered at no cost, which means you are charged only for the compute resources that your Batch solutions consume.</span></span> <span data-ttu-id="c35fd-288">Pokud vyberete **konfigurace cloudových služeb**, budou se vám účtovat na základě [ceny cloudové služby] [ cloud_services_pricing] struktury.</span><span class="sxs-lookup"><span data-stu-id="c35fd-288">When you choose **Cloud Services Configuration**, you are charged based on the [Cloud Services pricing][cloud_services_pricing] structure.</span></span> <span data-ttu-id="c35fd-289">Pokud vyberete **konfigurace virtuálního počítače**, budou se vám účtovat na základě [ceny virtuálních počítačů] [ vm_pricing] struktury.</span><span class="sxs-lookup"><span data-stu-id="c35fd-289">When you choose **Virtual Machine Configuration**, you are charged based on the [Virtual Machines pricing][vm_pricing] structure.</span></span> 

<span data-ttu-id="c35fd-290">Pokud nasazujete aplikace na uzly Batch pomocí [balíčky aplikací](batch-application-packages.md), se účtují poplatky za prostředky Azure Storage, aby balíčky aplikací používat.</span><span class="sxs-lookup"><span data-stu-id="c35fd-290">If you deploy applications to your Batch nodes using [application packages](batch-application-packages.md), you are also charged for the Azure Storage resources that your application packages consume.</span></span> <span data-ttu-id="c35fd-291">Obecně platí jsou minimální, náklady na úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c35fd-291">In general, the Azure Storage costs are minimal.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c35fd-292">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c35fd-292">Next steps</span></span>
### <a name="batch-python-tutorial"></a><span data-ttu-id="c35fd-293">Kurz k Batch Pythonu</span><span class="sxs-lookup"><span data-stu-id="c35fd-293">Batch Python tutorial</span></span>
<span data-ttu-id="c35fd-294">Pro více podrobný kurz o tom, jak pracovat s Batch pomocí Python, podívejte se na [Začínáme s klientem Azure Batch Python](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="c35fd-294">For a more in-depth tutorial about how to work with Batch by using Python, check out [Get started with the Azure Batch Python client](batch-python-tutorial.md).</span></span> <span data-ttu-id="c35fd-295">Jeho doprovodné [ukázka kódu] [ github_samples_pyclient] zahrnuje podpůrná funkce `get_vm_config_for_distro`, který ukazuje další technika, jak získat konfiguraci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c35fd-295">Its companion [code sample][github_samples_pyclient] includes a helper function, `get_vm_config_for_distro`, that shows another technique to obtain a virtual machine configuration.</span></span>

### <a name="batch-python-code-samples"></a><span data-ttu-id="c35fd-296">Ukázek kódu služby batch Python</span><span class="sxs-lookup"><span data-stu-id="c35fd-296">Batch Python code samples</span></span>
<span data-ttu-id="c35fd-297">[Ukázky kódu jsou Python] [ github_samples_py] v [azure-batch-samples] [ github_samples] úložišti na Githubu obsahovat skripty, které ukazují, jak provádět běžné operace Batch, například fond, úlohy a vytváření úlohy.</span><span class="sxs-lookup"><span data-stu-id="c35fd-297">The [Python code samples][github_samples_py] in the [azure-batch-samples][github_samples] repository on GitHub contain scripts that show you how to perform common Batch operations, such as pool, job, and task creation.</span></span> <span data-ttu-id="c35fd-298">[README] [ github_py_readme] doprovodný Python ukázky obsahuje podrobnosti o tom, jak nainstalovat požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="c35fd-298">The [README][github_py_readme] that accompanies the Python samples has details about how to install the required packages.</span></span>

### <a name="batch-forum"></a><span data-ttu-id="c35fd-299">Fórum k službě Batch</span><span class="sxs-lookup"><span data-stu-id="c35fd-299">Batch forum</span></span>
<span data-ttu-id="c35fd-300">[Fóru služby Azure Batch] [ forum] na webu MSDN je skvělým místem popisují Batch a klást otázky týkající se služby.</span><span class="sxs-lookup"><span data-stu-id="c35fd-300">The [Azure Batch Forum][forum] on MSDN is a great place to discuss Batch and ask questions about the service.</span></span> <span data-ttu-id="c35fd-301">Užitečné pro čtení "připnutý" účtuje a zveřejněte svoje otázky, kterým dochází při sestavování řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="c35fd-301">Read helpful "pinned" posts, and post your questions as they arise while you build your Batch solutions.</span></span>

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
