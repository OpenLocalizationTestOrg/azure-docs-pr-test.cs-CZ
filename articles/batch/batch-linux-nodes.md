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
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Zřídit Linux výpočetních uzlů ve fondech Batch

Azure Batch toorun paralelní výpočetních úloh můžete použít u virtuálních počítačů Linux a Windows. Tento článek popisuje, jak toocreate fondy Linux výpočetní uzly ve službě Batch hello pomocí obou hello [Batch Python] [ py_batch_package] a [Batch .NET] [ api_net] knihovny klienta.

> [!NOTE]
> Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017. Že jsou podporované v fondy Batch vytvořit až 10. března 2016 5 2017 července pouze v případě, že hello fondu byla vytvořena pomocí konfigurace cloudové služby. Fondy batch vytvořen předchozí too10. března 2016 nepodporují balíčky aplikací. Další informace o použití aplikace balíčky toodeploy uzly Batch tooyour aplikace, najdete v části [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md).
>
>

## <a name="virtual-machine-configuration"></a>Konfigurace virtuálního počítače
Když vytvoříte fond výpočetních uzlů ve službě Batch, máte dvě možnosti, ze které velikost uzlu hello tooselect a operační systém: Konfigurace cloudových služeb a konfigurace virtuálního počítače.

**Konfigurace služby Cloud Services** poskytuje *pouze* výpočetní uzly Windows. K dispozici výpočetní uzel velikosti jsou uvedeny v [velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md), a dostupných operačních systémů, jsou uvedeny v hello [verze hostovaného operačního systému Azure a kompatibilních sad SDK](../cloud-services/cloud-services-guestos-update-matrix.md). Když vytvoříte fond, který obsahuje uzly Azure Cloud Services, zadejte velikost uzlu hello a hello řada operačního systému, která jsou popsaná v hello výše články. Pro fondy Windows výpočetní uzly, se nejčastěji používá cloudové služby.

**Konfigurace virtuálního počítače** poskytuje, Linux a Windows Image pro výpočetní uzly. K dispozici výpočetní uzel velikosti jsou uvedeny v [velikosti virtuálních počítačů v Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) a [velikosti virtuálních počítačů v Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows). Když vytvoříte fond, který obsahuje uzly konfigurace virtuálního počítače, je nutné zadat velikost hello hello uzly, odkaz na obrázek hello virtuálního počítače a hello Batch uzlu agenta SKU toobe nainstalované na uzlech hello.

### <a name="virtual-machine-image-reference"></a>Odkaz na obrázek virtuálního počítače
Hello používá služba Batch [sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tooprovide Linux výpočetních uzlů. Můžete zadat bitové kopie z hello [Azure Marketplace][vm_marketplace], nebo zadejte vlastní obrázek, který jste připravili. Další informace o vlastních imagích najdete v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md#pool).

Když konfigurujete odkaz bitové kopie virtuálního počítače, je třeba zadat hello vlastnosti bitové kopie virtuálního počítače hello. Hello následující vlastnosti jsou požadovány při vytvoření odkaz na obrázek virtuálního počítače:

| **Vlastnosti referenční bitové kopie** | **Příklad** |
| --- | --- |
| Vydavatel |Canonical |
| Nabídka |UbuntuServer |
| Skladová jednotka (SKU) |14.04.4-LTS |
| Verze |nejnovější |

> [!TIP]
> Další informace o těchto vlastností a jak toolist Marketplace Image v nástroji [vyhledání a vyberte Image virtuálních počítačů Linux v Azure pomocí rozhraní CLI nebo Powershellu](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Upozorňujeme, že ne všechny bitové kopie Marketplace jsou momentálně kompatibilní s Batch. Další informace najdete v tématu [uzlu agenta SKU](#node-agent-sku).
>
>

### <a name="node-agent-sku"></a>Uzel agenta SKU
agent uzlu Hello Batch je program, který běží na každém uzlu ve fondu hello a poskytuje rozhraní příkazu a řízení hello mezi hello uzlu a služba Batch hello. Existují různé implementace hello uzlu agenta, označuje jako SKU, pro různé operační systémy. V podstatě při vytváření konfigurace virtuálního počítače, nejprve zadat odkaz na obrázek hello virtuálního počítače a pak zadejte hello uzlu agenta tooinstall na bitovou kopii hello. Obvykle každého uzlu agenta SKU je kompatibilní s více bitových kopií virtuálního počítače. Tady je několik příkladů uzlu agenta SKU:

* batch.Node.Ubuntu 14.04
* batch.Node.centos 7
* batch.Node.Windows amd64

> [!IMPORTANT]
> Ne všechny bitové kopie virtuálních počítačů, které jsou k dispozici v hello Marketplace jsou kompatibilní s agenty uzlu Batch aktuálně k dispozici hello. Použijte hello SDK služby Batch toolist hello k dispozici uzel agenta SKU a hello bitové kopie virtuálních počítačů, ke kterým jsou kompatibilní. V tématu hello [bitové kopie seznamu virtuálních počítačů](#list-of-virtual-machine-images) dále v tomto článku pro další informace a příklady, jak tooretrieve seznam platných obrázků za běhu.
>
>

## <a name="create-a-linux-pool-batch-python"></a>Vytvoření fondu Linux: Batch Python
Hello následující fragment kódu ukazuje příklad toouse hello [Microsoft Azure Batch klientské knihovny pro jazyk Python] [ py_batch_package] toocreate Ubuntu Server fond výpočetních uzlů. Referenční dokumentace pro hello modulu Batch Python najdete na [azure.batch balíček] [ py_batch_docs] na hello dokumentace pro čtení.

Vytvoří tento fragment kódu [elementu ImageReference] [ py_imagereference] explicitně a určí, každý z jeho vlastnosti (vydavatel, nabídku, SKU a verzi). V produkčním kódu, ale doporučujeme vám použít hello [list_node_agent_skus] [ py_list_skus] toodetermine metoda a vyberte z hello k dispozici bitovou kopii a uzel agenta SKU kombinace za běhu.

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

Jak je uvedeno nahoře, doporučujeme místo vytvoření hello [elementu ImageReference] [ py_imagereference] explicitně, použijete hello [list_node_agent_skus] [ py_list_skus] metoda toodynamically vybrat z hello aktuálně podporované kombinace bitovou kopii agenta nebo Marketplace uzlu. Následující fragment kódu ukazuje Python jak Hello toouse tuto metodu.

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

## <a name="create-a-linux-pool-batch-net"></a>Vytvoření fondu Linux: Batch .NET
Hello následující fragment kódu ukazuje příklad toouse hello [Batch .NET] [ nuget_batch_net] klienta knihovny toocreate Ubuntu Server fond výpočetních uzlů. Můžete najít hello [Batch .NET referenční dokumentaci k nástroji] [ api_net] na webu MSDN.

Hello následující fragment kódu používá hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] tooselect metoda z hello seznam aktuálně podporovaných Marketplace bitové kopie a uzlu agenta SKU kombinací. Tento postup je žádoucí, protože hello seznam podporovaných kombinací může změnit z tootime čas. Nejčastěji jsou přidány podporovaných kombinací.

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

I když předchozí fragment hello používá hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metoda toodynamically seznam a vyberte jednu z podporované bitové kopie a uzlu agenta SKU kombinace (doporučeno), můžete také nakonfigurovat [elementu ImageReference] [ net_imagereference] explicitně:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Seznam bitové kopie virtuálních počítačů
Hello následující tabulka uvádí Image hello Marketplace virtuálních počítačů, které jsou kompatibilní s agenty hello k dispozici Batch uzlu v době poslední aktualizace v tomto článku. Je důležité toonote, že tento seznam není spolehlivý, protože bitové kopie a agenty uzlu může přidat nebo odebrat kdykoli. Doporučujeme vaší aplikací a služeb Batch vždy použít [list_node_agent_skus] [ py_list_skus] (Python) a [ListNodeAgentSkus] [ net_list_skus] Toodetermine (batch .NET) a vyberte z hello aktuálně dostupné edice.

> [!WARNING]
> Následující seznam Hello může kdykoli změnit. Vždy používat hello **agent uzlu seznamu SKU** metody, které jsou k dispozici v toolist rozhraní API služby Batch hello hello kompatibilní virtuální počítač a uzlu agenta SKU při spuštění úlohy Batch.
>
>

| **Vydavatele** | **Nabídka** | **Obrázek SKU** | **Verze** | **Uzel agenta SKU ID** |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| Canonical | UbuntuServer | 14.04.5-LTS | nejnovější | batch.Node.Ubuntu 14.04 |
| Canonical | UbuntuServer | 16.04.0-LTS | nejnovější | batch.Node.Ubuntu 16.04 |
| Credativ | Debian | 8 | nejnovější | batch.Node.debian 8 |
| OpenLogic | CentOS | 7.0 | nejnovější | batch.Node.centos 7 |
| OpenLogic | CentOS | 7.1 | nejnovější | batch.Node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | nejnovější | batch.Node.centos 7 |
| OpenLogic | CentOS | 7.2 | nejnovější | batch.Node.centos 7 |
| Oracle | Oracle Linux | 7.0 | nejnovější | batch.Node.centos 7 |
| Oracle | Oracle Linux | 7.2 | nejnovější | batch.Node.centos 7 |
| SUSE | openSUSE | 13.2 | nejnovější | batch.Node.opensuse 13.2 |
| SUSE | openSUSE přestupného | 42.1 | nejnovější | batch.Node.opensuse 42.1 |
| SUSE | SLES | 12-SP1 | nejnovější | batch.Node.opensuse 42.1 |
| SUSE | SLES HPC | 12-SP1 | nejnovější | batch.Node.opensuse 42.1 |
| Microsoft služby Active Directory | data-vědecké účely-virtuálního počítače s linuxem | linuxdsvm | nejnovější | batch.Node.centos 7 |
| Microsoft služby Active Directory | Standard-data-vědecké účely vm | Standard-data-vědecké účely vm | nejnovější | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | AKTUALIZACE SP1 2008 R2 | nejnovější | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter | nejnovější | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter | nejnovější | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016 Datacenter | nejnovější | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016 datového centra s kontejnery | nejnovější | batch.Node.Windows amd64 |

## <a name="connect-toolinux-nodes-using-ssh"></a>Připojit tooLinux uzly pomocí protokolu SSH
Během vývoje nebo při řešení potíží možná bude nutné toosign v toohello uzly ve fondu. Na rozdíl od Windows výpočetní uzly nemůžete použít protokol RDP (Remote Desktop) tooconnect tooLinux uzlů. Místo toho hello služba Batch umožňuje přístup k SSH na každém uzlu vzdáleného připojení.

Hello následující fragment kódu Python vytvoří uživatele na každém uzlu ve fondu, který je vyžadován pro připojení ke vzdálené. Potom zobrazí informace o připojení hello zabezpečené shell (SSH) pro každý uzel.

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

Toto je ukázkový výstup hello předchozí kód pro fond, který obsahuje čtyři uzly Linux:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Namísto hesla můžete zadat veřejný klíč SSH při vytváření uživatele na uzlu. V hello Python SDK, pomocí hello **ssh_public_key** parametr na [ComputeNodeUser][py_computenodeuser]. V rozhraní .NET, použijte hello [ComputeNodeUser][net_computenodeuser].[ Parametru SshPublicKey] [ net_ssh_key] vlastnost.

## <a name="pricing"></a>Ceny
Azure Batch je založený na technologii cloudových služeb Azure a virtuálních počítačích Azure. Hello samotnou službu Batch se nabízí zadarmo, což znamená, budou se vám účtovat pouze pro hello výpočetní prostředky, které využívají řešení Batch. Pokud vyberete **konfigurace cloudových služeb**, budou se vám účtovat podle hello [ceny cloudové služby] [ cloud_services_pricing] struktury. Pokud vyberete **konfigurace virtuálního počítače**, budou se vám účtovat podle hello [ceny virtuálních počítačů] [ vm_pricing] struktury. 

Pokud nasazujete aplikace tooyour Batch uzlů pomocí [balíčky aplikací](batch-application-packages.md), se účtují poplatky pro prostředky úložiště Azure hello, balíčky aplikací používat. Obecně platí náklady na úložiště Azure hello jsou minimální. 

## <a name="next-steps"></a>Další kroky
### <a name="batch-python-tutorial"></a>Kurz k Batch Pythonu
Pro více podrobný kurz o toowork službou Batch pomocí Python, podívejte se na [Začínáme s klientem Azure Batch Python hello](batch-python-tutorial.md). Jeho doprovodné [ukázka kódu] [ github_samples_pyclient] zahrnuje podpůrná funkce `get_vm_config_for_distro`, který ukazuje další tooobtain technika konfiguraci virtuálního počítače.

### <a name="batch-python-code-samples"></a>Ukázek kódu služby batch Python
Hello [ukázky kódu jsou Python] [ github_samples_py] v hello [azure-batch-samples] [ github_samples] úložišti na Githubu obsahovat skripty, které ukazují, jak tooperform běžné operace Batch, třeba fond, úlohy a vytváření úlohy. Hello [README] [ github_py_readme] doprovodný hello Python ukázky obsahuje podrobnosti o tom, jak tooinstall hello požadované balíčky.

### <a name="batch-forum"></a>Fórum k službě Batch
Hello [fóru služby Azure Batch] [ forum] na webu MSDN je skvělá umístit toodiscuss Batch a klást otázky týkající se služby hello. Užitečné pro čtení "připnutý" účtuje a zveřejněte svoje otázky, kterým dochází při sestavování řešení Batch.

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
