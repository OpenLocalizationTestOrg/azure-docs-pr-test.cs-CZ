---
title: "Vytvářet a spravovat virtuální počítač s Windows v Azure pomocí Python | Microsoft Docs"
description: "Naučte se používat jazyk Python vytvářet a spravovat virtuální počítač s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: davidmu
ms.openlocfilehash: bb777d41570d7b1dc97402d532519488912948e3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-python"></a><span data-ttu-id="19b05-103">Vytvářet a spravovat virtuální počítače Windows v Azure pomocí Python</span><span class="sxs-lookup"><span data-stu-id="19b05-103">Create and manage Windows VMs in Azure using Python</span></span>

<span data-ttu-id="19b05-104">[Virtuální počítač Azure](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) vyžaduje několik podpůrných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="19b05-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="19b05-105">Tento článek popisuje vytváření, správu a odstraňování prostředky virtuálních počítačů pomocí Pythonu.</span><span class="sxs-lookup"><span data-stu-id="19b05-105">This article covers creating, managing, and deleting VM resources using Python.</span></span> <span data-ttu-id="19b05-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="19b05-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="19b05-107">Vytvoření projektu ve Visual Studiu</span><span class="sxs-lookup"><span data-stu-id="19b05-107">Create a Visual Studio project</span></span>
> * <span data-ttu-id="19b05-108">Instalovat balíčky</span><span class="sxs-lookup"><span data-stu-id="19b05-108">Install packages</span></span>
> * <span data-ttu-id="19b05-109">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="19b05-109">Create credentials</span></span>
> * <span data-ttu-id="19b05-110">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="19b05-110">Create resources</span></span>
> * <span data-ttu-id="19b05-111">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="19b05-111">Perform management tasks</span></span>
> * <span data-ttu-id="19b05-112">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="19b05-112">Delete resources</span></span>
> * <span data-ttu-id="19b05-113">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="19b05-113">Run the application</span></span>

<span data-ttu-id="19b05-114">Proveďte tyto kroky trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="19b05-114">It takes about 20 minutes to do these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="19b05-115">Vytvoření projektu ve Visual Studiu</span><span class="sxs-lookup"><span data-stu-id="19b05-115">Create a Visual Studio project</span></span>

1. <span data-ttu-id="19b05-116">Pokud jste to ještě neudělali, nainstalujte [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="19b05-116">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="19b05-117">Vyberte **vývoj Python** na stránce úlohy a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="19b05-117">Select **Python development** on the Workloads page, and then click **Install**.</span></span> <span data-ttu-id="19b05-118">V souhrnu, můžete uvidíte, že **Python 3 64-bit (3.6.0)** je automaticky vybrána pro vás.</span><span class="sxs-lookup"><span data-stu-id="19b05-118">In the summary, you can see that **Python 3 64-bit (3.6.0)** is automatically selected for you.</span></span> <span data-ttu-id="19b05-119">Pokud jste již nainstalovali Visual Studio, můžete přidat Python zatížení pomocí Spouštěče sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19b05-119">If you have already installed Visual Studio, you can add the Python workload using the Visual Studio Launcher.</span></span>
2. <span data-ttu-id="19b05-120">Po instalaci a spuštění sady Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="19b05-120">After installing and starting Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="19b05-121">Klikněte na tlačítko **šablony** > **Python** > **aplikace Python**, zadejte *myPythonProject* pro název projekt, vyberte umístění projektu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="19b05-121">Click **Templates** > **Python** > **Python Application**, enter *myPythonProject* for the name of the project, select the location of the project, and then click **OK**.</span></span>

## <a name="install-packages"></a><span data-ttu-id="19b05-122">Instalovat balíčky</span><span class="sxs-lookup"><span data-stu-id="19b05-122">Install packages</span></span>

1. <span data-ttu-id="19b05-123">V Průzkumníku řešení klikněte v části *myPythonProject*, klikněte pravým tlačítkem na **prostředí Python**a potom vyberte **přidat virtuální prostředí**.</span><span class="sxs-lookup"><span data-stu-id="19b05-123">In Solution Explorer, under *myPythonProject*, right-click **Python Environments**, and then select **Add virtual environment**.</span></span>
2. <span data-ttu-id="19b05-124">Na obrazovce Přidat virtuální prostředí, přijměte výchozí název *env*, ujistěte se, že *3.6 Python (64 bitů)* pro základní překladač je vybrána a potom klikněte na **vytvořit** .</span><span class="sxs-lookup"><span data-stu-id="19b05-124">On the Add Virtual Environment screen, accept the default name of *env*, make sure that *Python 3.6 (64-bit)* is selected for the base interpreter, and then click **Create**.</span></span>
3. <span data-ttu-id="19b05-125">Klikněte pravým tlačítkem myši *env* prostředí, které jste vytvořili, klikněte na tlačítko **instalovat balíček Python**, zadejte *azure* do pole hledání a potom stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="19b05-125">Right-click the *env* environment that you created, click **Install Python Package**, enter *azure* in the search box, and then press Enter.</span></span>

<span data-ttu-id="19b05-126">Měli byste vidět v oknech výstupu azure balíčky byly úspěšně nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="19b05-126">You should see in the output windows that the azure packages were successfully installed.</span></span> 

## <a name="create-credentials"></a><span data-ttu-id="19b05-127">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="19b05-127">Create credentials</span></span>

<span data-ttu-id="19b05-128">Než začnete tento krok, ujistěte se, že máte [objektu služby Active Directory](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="19b05-128">Before you start this step, make sure that you have an [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="19b05-129">Také byste měli zaznamenat ID aplikace, ověřovací klíč a ID klienta, který budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="19b05-129">You should also record the application ID, the authentication key, and the tenant ID that you need in a later step.</span></span>

1. <span data-ttu-id="19b05-130">Otevřete *myPythonProject.py* soubor, který byl vytvořen a poté přidejte tento kód k povolení spuštění vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="19b05-130">Open *myPythonProject.py* file that was created, and then add this code to enable your application to run:</span></span>

    ```python
    if __name__ == "__main__":
    ```

2. <span data-ttu-id="19b05-131">Pokud chcete importovat kód, který je potřeba, přidejte do horní části souboru .py tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="19b05-131">To import the code that is needed, add these statements to the top of the .py file:</span></span>

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. <span data-ttu-id="19b05-132">Dále v souboru .py přidejte proměnné po importu příkazy k určení běžné hodnoty, které se používá v kódu:</span><span class="sxs-lookup"><span data-stu-id="19b05-132">Next in the .py file, add variables after the import statements to specify common values used in the code:</span></span>
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    <span data-ttu-id="19b05-133">Nahraďte **id předplatného** s ID vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="19b05-133">Replace **subscription-id** with your subscription identifier.</span></span>

4. <span data-ttu-id="19b05-134">Pokud chcete vytvořit přihlašovací údaje služby Active Directory, které je třeba, aby žádosti, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-134">To create the Active Directory credentials that you need to make requests, add this function after the variables in the .py file:</span></span>

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    <span data-ttu-id="19b05-135">Nahraďte **id aplikace**, **ověřovací klíč**, a **id klienta** hodnotami, které jste dříve shromážděna při vytváření služby Azure Active Directory objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="19b05-135">Replace **application-id**, **authentication-key**, and **tenant-id** with the values that you previously collected when you created your Azure Active Directory service principal.</span></span>

5. <span data-ttu-id="19b05-136">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-136">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a><span data-ttu-id="19b05-137">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="19b05-137">Create resources</span></span>
 
### <a name="initialize-management-clients"></a><span data-ttu-id="19b05-138">Inicializace klientů pro správu</span><span class="sxs-lookup"><span data-stu-id="19b05-138">Initialize management clients</span></span>

<span data-ttu-id="19b05-139">Klienti pro správu jsou potřebné k vytváření a správě prostředků v Azure pomocí sady SDK pro Python.</span><span class="sxs-lookup"><span data-stu-id="19b05-139">Management clients are needed to create and manage resources using the Python SDK in Azure.</span></span> <span data-ttu-id="19b05-140">Chcete-li vytvořit klientů pro správu, přidejte tento kód v části **Pokud** údajů na adrese pak konec souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-140">To create the management clients, add this code under the **if** statement at then end of the .py file:</span></span>

```python
resource_group_client = ResourceManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
network_client = NetworkManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
compute_client = ComputeManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
```

### <a name="create-the-vm-and-supporting-resources"></a><span data-ttu-id="19b05-141">Vytvoření virtuálního počítače a podpůrné prostředky</span><span class="sxs-lookup"><span data-stu-id="19b05-141">Create the VM and supporting resources</span></span>

<span data-ttu-id="19b05-142">Musí být všechny prostředky obsažené v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="19b05-142">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

1. <span data-ttu-id="19b05-143">Pokud chcete vytvořit skupinu prostředků, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-143">To create a resource group, add this function after the variables in the .py file:</span></span>

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. <span data-ttu-id="19b05-144">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-144">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter to continue...')
    ```

<span data-ttu-id="19b05-145">[Skupiny dostupnosti](tutorial-availability-sets.md) umožňují snadnější zachování virtuálních počítačů, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="19b05-145">[Availability sets](tutorial-availability-sets.md) make it easier for you to maintain the virtual machines used by your application.</span></span>

1. <span data-ttu-id="19b05-146">Pokud chcete vytvořit skupinu dostupnosti, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-146">To create an availability set, add this function after the variables in the .py file:</span></span>
   
    ```python
    def create_availability_set(compute_client):
        avset_params = {
            'location': LOCATION,
            'sku': { 'name': 'Aligned' },
            'platform_fault_domain_count': 3
        }
        availability_set_result = compute_client.availability_sets.create_or_update(
            GROUP_NAME,
            'myAVSet',
            avset_params
        )
    ```

2. <span data-ttu-id="19b05-147">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-147">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter to continue...')
    ```

<span data-ttu-id="19b05-148">A [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) je potřeba ke komunikaci s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="19b05-148">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed to communicate with the virtual machine.</span></span>

1. <span data-ttu-id="19b05-149">Pokud chcete vytvořit veřejnou IP adresu pro virtuální počítač, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-149">To create a public IP address for the virtual machine, add this function after the variables in the .py file:</span></span>

    ```python
    def create_public_ip_address(network_client):
        public_ip_addess_params = {
            'location': LOCATION,
            'public_ip_allocation_method': 'Dynamic'
        }
        creation_result = network_client.public_ip_addresses.create_or_update(
            GROUP_NAME,
            'myIPAddress',
            public_ip_addess_params
        )

        return creation_result.result()
    ```

2. <span data-ttu-id="19b05-150">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-150">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

<span data-ttu-id="19b05-151">Virtuální počítač musí být v podsíti [virtuální síť](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="19b05-151">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="19b05-152">Pokud chcete vytvořit virtuální síť, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-152">To create a virtual network, add this function after the variables in the .py file:</span></span>

    ```python
    def create_vnet(network_client):
        vnet_params = {
            'location': LOCATION,
            'address_space': {
                'address_prefixes': ['10.0.0.0/16']
            }
        }
        creation_result = network_client.virtual_networks.create_or_update(
            GROUP_NAME,
            'myVNet',
            vnet_params
        )
        return creation_result.result()
    ```

2. <span data-ttu-id="19b05-153">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-153">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

3. <span data-ttu-id="19b05-154">Chcete-li přidat podsíť virtuální sítě, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-154">To add a subnet to the virtual network, add this function after the variables in the .py file:</span></span>
    
    ```python
    def create_subnet(network_client):
        subnet_params = {
            'address_prefix': '10.0.0.0/24'
        }
        creation_result = network_client.subnets.create_or_update(
            GROUP_NAME,
            'myVNet',
            'mySubnet',
            subnet_params
        )

        return creation_result.result()
    ```
        
4. <span data-ttu-id="19b05-155">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-155">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

<span data-ttu-id="19b05-156">Virtuální počítač vyžaduje síťové rozhraní pro komunikaci ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="19b05-156">A virtual machine needs a network interface to communicate on the virtual network.</span></span>

1. <span data-ttu-id="19b05-157">Pokud chcete vytvořit síťové rozhraní, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-157">To create a network interface, add this function after the variables in the .py file:</span></span>

    ```python
    def create_nic(network_client):
        subnet_info = network_client.subnets.get(
            GROUP_NAME, 
            'myVNet', 
            'mySubnet'
        )
        publicIPAddress = network_client.public_ip_addresses.get(
            GROUP_NAME,
            'myIPAddress'
        )
        nic_params = {
            'location': LOCATION,
            'ip_configurations': [{
                'name': 'myIPConfig',
                'public_ip_address': publicIPAddress,
                'subnet': {
                    'id': subnet_info.id
                }
            }]
        }
        creation_result = network_client.network_interfaces.create_or_update(
            GROUP_NAME,
            'myNic',
            nic_params
        )

        return creation_result.result()
    ```

2. <span data-ttu-id="19b05-158">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-158">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

<span data-ttu-id="19b05-159">Teď, když jste vytvořili doprovodné materiály, můžete vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="19b05-159">Now that you created all the supporting resources, you can create a virtual machine.</span></span>

1. <span data-ttu-id="19b05-160">Pokud chcete vytvořit virtuální počítač, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-160">To create the virtual machine, add this function after the variables in the .py file:</span></span>
   
    ```python
    def create_vm(network_client, compute_client):  
        nic = network_client.network_interfaces.get(
            GROUP_NAME, 
            'myNic'
        )
        avset = compute_client.availability_sets.get(
            GROUP_NAME,
            'myAVSet'
        )
        vm_parameters = {
            'location': LOCATION,
            'os_profile': {
                'computer_name': VM_NAME,
                'admin_username': 'azureuser',
                'admin_password': 'Azure12345678'
            },
            'hardware_profile': {
                'vm_size': 'Standard_DS1'
            },
            'storage_profile': {
                'image_reference': {
                    'publisher': 'MicrosoftWindowsServer',
                    'offer': 'WindowsServer',
                    'sku': '2012-R2-Datacenter',
                    'version': 'latest'
                }
            },
            'network_profile': {
                'network_interfaces': [{
                    'id': nic.id
                }]
            },
            'availability_set': {
                'id': avset.id
            }
        }
        creation_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm_parameters
        )
    
        return creation_result.result()
    ```

    > [!NOTE]
    > <span data-ttu-id="19b05-161">V tomto kurzu vytvoří virtuální počítač s verzí operačního systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="19b05-161">This tutorial creates a virtual machine running a version of the Windows Server operating system.</span></span> <span data-ttu-id="19b05-162">Další informace o výběru ostatní Image, najdete v tématu [vyhledání a výběr imagí virtuálních počítačů Azure pomocí prostředí Windows PowerShell a rozhraní příkazového řádku Azure](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="19b05-162">To learn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and the Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

2. <span data-ttu-id="19b05-163">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-163">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

## <a name="perform-management-tasks"></a><span data-ttu-id="19b05-164">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="19b05-164">Perform management tasks</span></span>

<span data-ttu-id="19b05-165">Během životního cyklu virtuálního počítače můžete spustit úlohy správy, jako je například spuštění, zastavení nebo odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="19b05-165">During the lifecycle of a virtual machine, you may want to run management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="19b05-166">Kromě toho můžete vytvořit kód pro automatizaci úloh opakovaných nebo komplexní.</span><span class="sxs-lookup"><span data-stu-id="19b05-166">Additionally, you may want to create code to automate repetitive or complex tasks.</span></span>

### <a name="get-information-about-the-vm"></a><span data-ttu-id="19b05-167">Získat informace o virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="19b05-167">Get information about the VM</span></span>

1. <span data-ttu-id="19b05-168">Chcete-li získat informace o virtuálním počítači, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-168">To get information about the virtual machine, add this function after the variables in the .py file:</span></span>

    ```python
    def get_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME, expand='instanceView')
        print("hardwareProfile")
        print("   vmSize: ", vm.hardware_profile.vm_size)
        print("\nstorageProfile")
        print("  imageReference")
        print("    publisher: ", vm.storage_profile.image_reference.publisher)
        print("    offer: ", vm.storage_profile.image_reference.offer)
        print("    sku: ", vm.storage_profile.image_reference.sku)
        print("    version: ", vm.storage_profile.image_reference.version)
        print("  osDisk")
        print("    osType: ", vm.storage_profile.os_disk.os_type.value)
        print("    name: ", vm.storage_profile.os_disk.name)
        print("    createOption: ", vm.storage_profile.os_disk.create_option.value)
        print("    caching: ", vm.storage_profile.os_disk.caching.value)
        print("\nosProfile")
        print("  computerName: ", vm.os_profile.computer_name)
        print("  adminUsername: ", vm.os_profile.admin_username)
        print("  provisionVMAgent: {0}".format(vm.os_profile.windows_configuration.provision_vm_agent))
        print("  enableAutomaticUpdates: {0}".format(vm.os_profile.windows_configuration.enable_automatic_updates))
        print("\nnetworkProfile")
        for nic in vm.network_profile.network_interfaces:
            print("  networkInterface id: ", nic.id)
        print("\nvmAgent")
        print("  vmAgentVersion", vm.instance_view.vm_agent.vm_agent_version)
        print("    statuses")
        for stat in vm_result.instance_view.vm_agent.statuses:
            print("    code: ", stat.code)
            print("    displayStatus: ", stat.display_status)
            print("    message: ", stat.message)
            print("    time: ", stat.time)
        print("\ndisks");
        for disk in vm.instance_view.disks:
            print("  name: ", disk.name)
            print("  statuses")
            for stat in disk.statuses:
                print("    code: ", stat.code)
                print("    displayStatus: ", stat.display_status)
                print("    time: ", stat.time)
        print("\nVM general status")
        print("  provisioningStatus: ", vm.provisioning_state)
        print("  id: ", vm.id)
        print("  name: ", vm.name)
        print("  type: ", vm.type)
        print("  location: ", vm.location)
        print("\nVM instance status")
        for stat in vm.instance_view.statuses:
            print("  code: ", stat.code)
            print("  displayStatus: ", stat.display_status)
    ```
2. <span data-ttu-id="19b05-169">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-169">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter to continue...')
    ```

### <a name="stop-the-vm"></a><span data-ttu-id="19b05-170">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="19b05-170">Stop the VM</span></span>

<span data-ttu-id="19b05-171">Můžete zastavit virtuální počítač a ponechat jeho nastavení, ale nadále účtovat poplatek za ho nebo můžete zastavit virtuální počítač a jeho navrácení.</span><span class="sxs-lookup"><span data-stu-id="19b05-171">You can stop a virtual machine and keep all its settings, but continue to be charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="19b05-172">Při zrušení jeho přidělení virtuálního počítače jsou končí deallocated a fakturace pro ni také všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="19b05-172">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

1. <span data-ttu-id="19b05-173">Zastavte virtuální počítač bez rušení přidělení ho, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-173">To stop the virtual machine without deallocating it, add this function after the variables in the .py file:</span></span>

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    <span data-ttu-id="19b05-174">Pokud chcete zrušit přidělení virtuálního počítače, změna volání power_off tento kód:</span><span class="sxs-lookup"><span data-stu-id="19b05-174">If you want to deallocate the virtual machine, change the power_off call to this code:</span></span>

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. <span data-ttu-id="19b05-175">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-175">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    stop_vm(compute_client)
    input('Press enter to continue...')
    ```

### <a name="start-the-vm"></a><span data-ttu-id="19b05-176">Spusťte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="19b05-176">Start the VM</span></span>

1. <span data-ttu-id="19b05-177">Pokud chcete spustit virtuální počítač, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-177">To start the virtual machine, add this function after the variables in the .py file:</span></span>

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. <span data-ttu-id="19b05-178">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-178">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    start_vm(compute_client)
    input('Press enter to continue...')
    ```

### <a name="resize-the-vm"></a><span data-ttu-id="19b05-179">Změnit velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="19b05-179">Resize the VM</span></span>

<span data-ttu-id="19b05-180">Mnoho aspektů nasazení měli zvážit při rozhodování o velikosti pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="19b05-180">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="19b05-181">Další informace najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="19b05-181">For more information, see [VM sizes](sizes.md).</span></span>

1. <span data-ttu-id="19b05-182">Chcete-li změnit velikost virtuálního počítače, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-182">To change the size of the virtual machine, add this function after the variables in the .py file:</span></span>

    ```python
    def update_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        vm.hardware_profile.vm_size = 'Standard_DS3'
        update_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm
        )

    return update_result.result()
    ```

2. <span data-ttu-id="19b05-183">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-183">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter to continue...')
    ```

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="19b05-184">Přidat datový disk k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="19b05-184">Add a data disk to the VM</span></span>

<span data-ttu-id="19b05-185">Virtuální počítače může mít jeden nebo více [datových disků](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , jsou uloženy jako soubory VHD.</span><span class="sxs-lookup"><span data-stu-id="19b05-185">Virtual machines can have one or more [data disks](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) that are stored as VHDs.</span></span>

1. <span data-ttu-id="19b05-186">Chcete-li přidat datový disk k virtuálnímu počítači, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-186">To add a data disk to the virtual machine, add this function after the variables in the .py file:</span></span> 

    ```python
    def add_datadisk(compute_client):
        disk_creation = compute_client.disks.create_or_update(
            GROUP_NAME,
            'myDataDisk1',
            {
                'location': LOCATION,
                'disk_size_gb': 1,
                'creation_data': {
                    'create_option': DiskCreateOption.empty
                }
            }
        )
        data_disk = disk_creation.result()
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        add_result = vm.storage_profile.data_disks.append({
            'lun': 1,
            'name': 'myDataDisk1',
            'create_option': DiskCreateOption.attach,
            'managed_disk': {
                'id': data_disk.id
            }
        })
        add_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME,
            VM_NAME,
            vm)

        return add_result.result()
    ```

2. <span data-ttu-id="19b05-187">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-187">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter to continue...')
    ```

## <a name="delete-resources"></a><span data-ttu-id="19b05-188">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="19b05-188">Delete resources</span></span>

<span data-ttu-id="19b05-189">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem odstranit prostředky, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="19b05-189">Because you are charged for resources used in Azure, it's always a good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="19b05-190">Pokud chcete odstranit virtuální počítače a všechny podpůrné prostředky, je vše, co musíte udělat, odstraňte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="19b05-190">If you want to delete the virtual machines and all the supporting resources, all you have to do is delete the resource group.</span></span>

1. <span data-ttu-id="19b05-191">Pokud chcete odstranit skupinu prostředků a všechny prostředky, přidejte tuto funkci po proměnné v souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-191">To delete the resource group and all resources, add this function after the variables in the .py file:</span></span>
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. <span data-ttu-id="19b05-192">Chcete-li zavolat funkci, kterou jste dříve přidali, přidejte tento kód v části **Pokud** příkaz na konci souboru .py:</span><span class="sxs-lookup"><span data-stu-id="19b05-192">To call the function that you previously added, add this code under the **if** statement at the end of the .py file:</span></span>
   
    ```python
    delete_resources(resource_group_client)
    ```

3. <span data-ttu-id="19b05-193">Uložit *myPythonProject.py*.</span><span class="sxs-lookup"><span data-stu-id="19b05-193">Save *myPythonProject.py*.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="19b05-194">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="19b05-194">Run the application</span></span>

1. <span data-ttu-id="19b05-195">Chcete-li spustit aplikaci konzoly, klikněte na tlačítko **spustit** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19b05-195">To run the console application, click **Start** in Visual Studio.</span></span>

2. <span data-ttu-id="19b05-196">Stiskněte klávesu **Enter** po je vrácen stav každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="19b05-196">Press **Enter** after the status of each resource is returned.</span></span> <span data-ttu-id="19b05-197">Informace o stavu, byste měli vidět **úspěšné** Stav zřizování.</span><span class="sxs-lookup"><span data-stu-id="19b05-197">In the status information, you should see a **Succeeded** provisioning state.</span></span> <span data-ttu-id="19b05-198">Po vytvoření virtuálního počítače, máte možnost odstranit všechny prostředky, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="19b05-198">After the virtual machine is created, you have the opportunity to delete all the resources that you create.</span></span> <span data-ttu-id="19b05-199">Před stisknutím klávesy **Enter** zahájíte odstranění prostředků, může trvat několik minut na ověření jejich vytvoření na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="19b05-199">Before you press **Enter** to start deleting resources, you could take a few minutes to verify their creation in the Azure portal.</span></span> <span data-ttu-id="19b05-200">Pokud máte na portálu Azure otevřete, můžete chtít aktualizovat v okně zobrazíte nové prostředky.</span><span class="sxs-lookup"><span data-stu-id="19b05-200">If you have the Azure portal open, you might have to refresh the blade to see new resources.</span></span>  

    <span data-ttu-id="19b05-201">Dokončit má trvat přibližně pět minut, než tato Konzolová aplikace spustit úplně od začátku.</span><span class="sxs-lookup"><span data-stu-id="19b05-201">It should take about five minutes for this console application to run completely from start to finish.</span></span> <span data-ttu-id="19b05-202">Může trvat několik minut, po dokončení aplikace před všechny prostředky a skupině prostředků se odstraní.</span><span class="sxs-lookup"><span data-stu-id="19b05-202">It may take several minutes after the application has finished before all the resources and the resource group are deleted.</span></span>


## <a name="next-steps"></a><span data-ttu-id="19b05-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19b05-203">Next steps</span></span>

- <span data-ttu-id="19b05-204">Pokud byly nějaké problémy s nasazením, je dalším krokem projít si téma [Řešení potíží s nasazením skupin prostředků pomocí webu Azure Portal](../../resource-manager-troubleshoot-deployments-portal.md).</span><span class="sxs-lookup"><span data-stu-id="19b05-204">If there were issues with the deployment, a next step would be to look at [Troubleshooting resource group deployments with Azure portal](../../resource-manager-troubleshoot-deployments-portal.md)</span></span>
- <span data-ttu-id="19b05-205">Další informace o [knihovna Python Azure](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)</span><span class="sxs-lookup"><span data-stu-id="19b05-205">Learn more about the [Azure Python Library](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)</span></span>

