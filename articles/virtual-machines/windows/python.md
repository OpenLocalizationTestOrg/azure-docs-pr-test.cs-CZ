---
title: "aaaCreate a spravovat virtuální počítač s Windows v Azure pomocí Python | Microsoft Docs"
description: "Další toouse Python toocreate a spravovat virtuální počítač s Windows v Azure."
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
ms.openlocfilehash: c5553e4e7361e6b9a7183cd935be382f967160cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-python"></a><span data-ttu-id="26437-103">Vytvářet a spravovat virtuální počítače Windows v Azure pomocí Python</span><span class="sxs-lookup"><span data-stu-id="26437-103">Create and manage Windows VMs in Azure using Python</span></span>

<span data-ttu-id="26437-104">[Virtuální počítač Azure](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) vyžaduje několik podpůrných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="26437-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="26437-105">Tento článek popisuje vytváření, správu a odstraňování prostředky virtuálních počítačů pomocí Pythonu.</span><span class="sxs-lookup"><span data-stu-id="26437-105">This article covers creating, managing, and deleting VM resources using Python.</span></span> <span data-ttu-id="26437-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="26437-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="26437-107">Vytvoření projektu ve Visual Studiu</span><span class="sxs-lookup"><span data-stu-id="26437-107">Create a Visual Studio project</span></span>
> * <span data-ttu-id="26437-108">Instalovat balíčky</span><span class="sxs-lookup"><span data-stu-id="26437-108">Install packages</span></span>
> * <span data-ttu-id="26437-109">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="26437-109">Create credentials</span></span>
> * <span data-ttu-id="26437-110">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="26437-110">Create resources</span></span>
> * <span data-ttu-id="26437-111">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="26437-111">Perform management tasks</span></span>
> * <span data-ttu-id="26437-112">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="26437-112">Delete resources</span></span>
> * <span data-ttu-id="26437-113">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="26437-113">Run hello application</span></span>

<span data-ttu-id="26437-114">Trvá přibližně 20 minut toodo tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="26437-114">It takes about 20 minutes toodo these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="26437-115">Vytvoření projektu ve Visual Studiu</span><span class="sxs-lookup"><span data-stu-id="26437-115">Create a Visual Studio project</span></span>

1. <span data-ttu-id="26437-116">Pokud jste to ještě neudělali, nainstalujte [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="26437-116">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="26437-117">Vyberte **vývoj Python** na hello zatížení stránky a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="26437-117">Select **Python development** on hello Workloads page, and then click **Install**.</span></span> <span data-ttu-id="26437-118">V hello souhrn, můžete uvidíte, že **Python 3 64-bit (3.6.0)** je automaticky vybrána pro vás.</span><span class="sxs-lookup"><span data-stu-id="26437-118">In hello summary, you can see that **Python 3 64-bit (3.6.0)** is automatically selected for you.</span></span> <span data-ttu-id="26437-119">Pokud jste již nainstalovali Visual Studio, můžete přidat hello Python zatížení pomocí hello Spouštěče Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26437-119">If you have already installed Visual Studio, you can add hello Python workload using hello Visual Studio Launcher.</span></span>
2. <span data-ttu-id="26437-120">Po instalaci a spuštění sady Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="26437-120">After installing and starting Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="26437-121">Klikněte na tlačítko **šablony** > **Python** > **aplikace Python**, zadejte *myPythonProject* pro název hello hello projektu, vyberte umístění hello hello projektu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="26437-121">Click **Templates** > **Python** > **Python Application**, enter *myPythonProject* for hello name of hello project, select hello location of hello project, and then click **OK**.</span></span>

## <a name="install-packages"></a><span data-ttu-id="26437-122">Instalovat balíčky</span><span class="sxs-lookup"><span data-stu-id="26437-122">Install packages</span></span>

1. <span data-ttu-id="26437-123">V Průzkumníku řešení klikněte v části *myPythonProject*, klikněte pravým tlačítkem na **prostředí Python**a potom vyberte **přidat virtuální prostředí**.</span><span class="sxs-lookup"><span data-stu-id="26437-123">In Solution Explorer, under *myPythonProject*, right-click **Python Environments**, and then select **Add virtual environment**.</span></span>
2. <span data-ttu-id="26437-124">Na úvodní obrazovka Přidání virtuálního prostředí, přijměte výchozí název hello *env*, ujistěte se, že *3.6 Python (64 bitů)* pro základní překladač hello je vybrána a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="26437-124">On hello Add Virtual Environment screen, accept hello default name of *env*, make sure that *Python 3.6 (64-bit)* is selected for hello base interpreter, and then click **Create**.</span></span>
3. <span data-ttu-id="26437-125">Klikněte pravým tlačítkem na hello *env* prostředí, které jste vytvořili, klikněte na tlačítko **instalovat balíček Python**, zadejte *azure* v hello vyhledávacího pole a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="26437-125">Right-click hello *env* environment that you created, click **Install Python Package**, enter *azure* in hello search box, and then press Enter.</span></span>

<span data-ttu-id="26437-126">Měli byste vidět v oknech výstupu hello balíčky hello azure byly úspěšně nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="26437-126">You should see in hello output windows that hello azure packages were successfully installed.</span></span> 

## <a name="create-credentials"></a><span data-ttu-id="26437-127">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="26437-127">Create credentials</span></span>

<span data-ttu-id="26437-128">Než začnete tento krok, ujistěte se, že máte [objektu služby Active Directory](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="26437-128">Before you start this step, make sure that you have an [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="26437-129">Také byste měli zaznamenat ID aplikace hello hello ověřovací klíč a hello ID klienta, který budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="26437-129">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in a later step.</span></span>

1. <span data-ttu-id="26437-130">Otevřete *myPythonProject.py* soubor, který byl vytvořen a poté přidejte tento kód tooenable toorun vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="26437-130">Open *myPythonProject.py* file that was created, and then add this code tooenable your application toorun:</span></span>

    ```python
    if __name__ == "__main__":
    ```

2. <span data-ttu-id="26437-131">tooimport hello kód, který je potřeba, přidejte tyto příkazy toohello horní části souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-131">tooimport hello code that is needed, add these statements toohello top of hello .py file:</span></span>

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. <span data-ttu-id="26437-132">Dále v souboru .py hello, přidejte proměnné po příkazy pro import hello toospecify běžné hodnoty použít v hello kódu:</span><span class="sxs-lookup"><span data-stu-id="26437-132">Next in hello .py file, add variables after hello import statements toospecify common values used in hello code:</span></span>
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    <span data-ttu-id="26437-133">Nahraďte **id předplatného** s ID vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="26437-133">Replace **subscription-id** with your subscription identifier.</span></span>

4. <span data-ttu-id="26437-134">pověření služby Active Directory hello toocreate, je nutné, aby toomake požadavků, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-134">toocreate hello Active Directory credentials that you need toomake requests, add this function after hello variables in hello .py file:</span></span>

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    <span data-ttu-id="26437-135">Nahraďte **id aplikace**, **ověřovací klíč**, a **id klienta** hello hodnotami, které jste dříve shromážděna při vytváření služby Azure Active Directory instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="26437-135">Replace **application-id**, **authentication-key**, and **tenant-id** with hello values that you previously collected when you created your Azure Active Directory service principal.</span></span>

5. <span data-ttu-id="26437-136">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-136">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a><span data-ttu-id="26437-137">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="26437-137">Create resources</span></span>
 
### <a name="initialize-management-clients"></a><span data-ttu-id="26437-138">Inicializace klientů pro správu</span><span class="sxs-lookup"><span data-stu-id="26437-138">Initialize management clients</span></span>

<span data-ttu-id="26437-139">Klienti pro správu jsou potřebné toocreate a spravovat prostředky pomocí hello Python SDK v Azure.</span><span class="sxs-lookup"><span data-stu-id="26437-139">Management clients are needed toocreate and manage resources using hello Python SDK in Azure.</span></span> <span data-ttu-id="26437-140">toocreate hello správu klientů, přidejte tento kód pod hello **Pokud** údajů na adrese pak konec souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-140">toocreate hello management clients, add this code under hello **if** statement at then end of hello .py file:</span></span>

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

### <a name="create-hello-vm-and-supporting-resources"></a><span data-ttu-id="26437-141">Vytvoření hello virtuálních počítačů a podpůrné prostředky</span><span class="sxs-lookup"><span data-stu-id="26437-141">Create hello VM and supporting resources</span></span>

<span data-ttu-id="26437-142">Musí být všechny prostředky obsažené v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="26437-142">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

1. <span data-ttu-id="26437-143">toocreate skupinu prostředků, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-143">toocreate a resource group, add this function after hello variables in hello .py file:</span></span>

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. <span data-ttu-id="26437-144">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-144">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter toocontinue...')
    ```

<span data-ttu-id="26437-145">[Skupiny dostupnosti](tutorial-availability-sets.md) bylo snazší pro vás toomaintain hello virtuální počítače používané vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="26437-145">[Availability sets](tutorial-availability-sets.md) make it easier for you toomaintain hello virtual machines used by your application.</span></span>

1. <span data-ttu-id="26437-146">toocreate dostupnosti nastaven, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-146">toocreate an availability set, add this function after hello variables in hello .py file:</span></span>
   
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

2. <span data-ttu-id="26437-147">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-147">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter toocontinue...')
    ```

<span data-ttu-id="26437-148">A [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) je potřebné toocommunicate s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="26437-148">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed toocommunicate with hello virtual machine.</span></span>

1. <span data-ttu-id="26437-149">toocreate veřejnou IP adresu pro virtuální počítač hello, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-149">toocreate a public IP address for hello virtual machine, add this function after hello variables in hello .py file:</span></span>

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

2. <span data-ttu-id="26437-150">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-150">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

<span data-ttu-id="26437-151">Virtuální počítač musí být v podsíti [virtuální síť](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="26437-151">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="26437-152">toocreate virtuální sítě, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-152">toocreate a virtual network, add this function after hello variables in hello .py file:</span></span>

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

2. <span data-ttu-id="26437-153">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-153">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

3. <span data-ttu-id="26437-154">tooadd toohello podsíť virtuální sítě, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-154">tooadd a subnet toohello virtual network, add this function after hello variables in hello .py file:</span></span>
    
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
        
4. <span data-ttu-id="26437-155">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-155">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

<span data-ttu-id="26437-156">Virtuální počítač vyžaduje toocommunicate rozhraní sítě ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="26437-156">A virtual machine needs a network interface toocommunicate on hello virtual network.</span></span>

1. <span data-ttu-id="26437-157">toocreate síťového rozhraní, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-157">toocreate a network interface, add this function after hello variables in hello .py file:</span></span>

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

2. <span data-ttu-id="26437-158">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-158">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

<span data-ttu-id="26437-159">Teď, když jste vytvořili všechny hello Podpora prostředků, můžete vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="26437-159">Now that you created all hello supporting resources, you can create a virtual machine.</span></span>

1. <span data-ttu-id="26437-160">toocreate hello virtuálního počítače, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-160">toocreate hello virtual machine, add this function after hello variables in hello .py file:</span></span>
   
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
    > <span data-ttu-id="26437-161">V tomto kurzu vytvoří virtuální počítač s verzí operačního systému Windows Server hello.</span><span class="sxs-lookup"><span data-stu-id="26437-161">This tutorial creates a virtual machine running a version of hello Windows Server operating system.</span></span> <span data-ttu-id="26437-162">toolearn Další informace o výběru ostatní Image, najdete v části [vyhledání a výběr imagí virtuálních počítačů Azure pomocí prostředí Windows PowerShell a rozhraní příkazového řádku Azure hello](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="26437-162">toolearn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

2. <span data-ttu-id="26437-163">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-163">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

## <a name="perform-management-tasks"></a><span data-ttu-id="26437-164">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="26437-164">Perform management tasks</span></span>

<span data-ttu-id="26437-165">Během životního cyklu hello virtuálního počítače můžete úlohy správy toorun například spuštění, zastavení nebo odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="26437-165">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="26437-166">Kromě toho můžete toocreate kód tooautomate opakovaných nebo komplexní úlohy.</span><span class="sxs-lookup"><span data-stu-id="26437-166">Additionally, you may want toocreate code tooautomate repetitive or complex tasks.</span></span>

### <a name="get-information-about-hello-vm"></a><span data-ttu-id="26437-167">Získání informací o hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="26437-167">Get information about hello VM</span></span>

1. <span data-ttu-id="26437-168">tooget informace o virtuálním počítači hello, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-168">tooget information about hello virtual machine, add this function after hello variables in hello .py file:</span></span>

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
2. <span data-ttu-id="26437-169">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-169">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter toocontinue...')
    ```

### <a name="stop-hello-vm"></a><span data-ttu-id="26437-170">Zastavit hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="26437-170">Stop hello VM</span></span>

<span data-ttu-id="26437-171">Můžete zastavit virtuální počítač a ponechat jeho nastavení, ale pokračovat toobe účtovat pro něj nebo můžete zastavit virtuální počítač a jeho navrácení.</span><span class="sxs-lookup"><span data-stu-id="26437-171">You can stop a virtual machine and keep all its settings, but continue toobe charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="26437-172">Při zrušení jeho přidělení virtuálního počítače jsou končí deallocated a fakturace pro ni také všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="26437-172">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

1. <span data-ttu-id="26437-173">toostop hello virtuální počítač bez rušení přidělení, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-173">toostop hello virtual machine without deallocating it, add this function after hello variables in hello .py file:</span></span>

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    <span data-ttu-id="26437-174">Pokud chcete toodeallocate hello virtuální počítač, změňte hello power_off volání toothis kódu:</span><span class="sxs-lookup"><span data-stu-id="26437-174">If you want toodeallocate hello virtual machine, change hello power_off call toothis code:</span></span>

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. <span data-ttu-id="26437-175">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-175">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    stop_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="start-hello-vm"></a><span data-ttu-id="26437-176">Hello spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="26437-176">Start hello VM</span></span>

1. <span data-ttu-id="26437-177">toostart hello virtuálního počítače, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-177">toostart hello virtual machine, add this function after hello variables in hello .py file:</span></span>

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. <span data-ttu-id="26437-178">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-178">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    start_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="resize-hello-vm"></a><span data-ttu-id="26437-179">Změnit velikost hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="26437-179">Resize hello VM</span></span>

<span data-ttu-id="26437-180">Mnoho aspektů nasazení měli zvážit při rozhodování o velikosti pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="26437-180">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="26437-181">Další informace najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="26437-181">For more information, see [VM sizes](sizes.md).</span></span>

1. <span data-ttu-id="26437-182">velikost hello toochange hello virtuálního počítače, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-182">toochange hello size of hello virtual machine, add this function after hello variables in hello .py file:</span></span>

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

2. <span data-ttu-id="26437-183">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-183">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter toocontinue...')
    ```

### <a name="add-a-data-disk-toohello-vm"></a><span data-ttu-id="26437-184">Přidání disku toohello data virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="26437-184">Add a data disk toohello VM</span></span>

<span data-ttu-id="26437-185">Virtuální počítače může mít jeden nebo více [datových disků](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , jsou uloženy jako soubory VHD.</span><span class="sxs-lookup"><span data-stu-id="26437-185">Virtual machines can have one or more [data disks](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) that are stored as VHDs.</span></span>

1. <span data-ttu-id="26437-186">tooadd datového disku toohello virtuálního počítače, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-186">tooadd a data disk toohello virtual machine, add this function after hello variables in hello .py file:</span></span> 

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

2. <span data-ttu-id="26437-187">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-187">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter toocontinue...')
    ```

## <a name="delete-resources"></a><span data-ttu-id="26437-188">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="26437-188">Delete resources</span></span>

<span data-ttu-id="26437-189">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, je vždy prostředky toodelete osvědčených postupů, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="26437-189">Because you are charged for resources used in Azure, it's always a good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="26437-190">Pokud chcete, aby toodelete hello virtuální počítače a všechny hello podpora prostředky, všechny máte toodo je odstranit skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="26437-190">If you want toodelete hello virtual machines and all hello supporting resources, all you have toodo is delete hello resource group.</span></span>

1. <span data-ttu-id="26437-191">Skupina prostředků hello toodelete a všechny prostředky, přidejte tuto funkci po hello proměnné v souboru .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-191">toodelete hello resource group and all resources, add this function after hello variables in hello .py file:</span></span>
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. <span data-ttu-id="26437-192">Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:</span><span class="sxs-lookup"><span data-stu-id="26437-192">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>
   
    ```python
    delete_resources(resource_group_client)
    ```

3. <span data-ttu-id="26437-193">Uložit *myPythonProject.py*.</span><span class="sxs-lookup"><span data-stu-id="26437-193">Save *myPythonProject.py*.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="26437-194">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="26437-194">Run hello application</span></span>

1. <span data-ttu-id="26437-195">toorun hello konzolovou aplikaci, klikněte na tlačítko **spustit** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26437-195">toorun hello console application, click **Start** in Visual Studio.</span></span>

2. <span data-ttu-id="26437-196">Stiskněte klávesu **Enter** po je vrácen hello stav každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="26437-196">Press **Enter** after hello status of each resource is returned.</span></span> <span data-ttu-id="26437-197">Informace o stavu hello byste měli vidět **úspěšné** Stav zřizování.</span><span class="sxs-lookup"><span data-stu-id="26437-197">In hello status information, you should see a **Succeeded** provisioning state.</span></span> <span data-ttu-id="26437-198">Po vytvoření virtuálního počítače hello máte možnost toodelete hello všechny hello prostředky, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="26437-198">After hello virtual machine is created, you have hello opportunity toodelete all hello resources that you create.</span></span> <span data-ttu-id="26437-199">Před stisknutím klávesy **Enter** toostart odstraňování prostředky, vám může trvat několik minut tooverify jejich vytvoření v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="26437-199">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify their creation in hello Azure portal.</span></span> <span data-ttu-id="26437-200">Pokud máte hello portálu Azure otevřete, může být toorefresh hello okno toosee nové prostředky.</span><span class="sxs-lookup"><span data-stu-id="26437-200">If you have hello Azure portal open, you might have toorefresh hello blade toosee new resources.</span></span>  

    <span data-ttu-id="26437-201">Má trvat přibližně pět minut, než tato toorun aplikace konzoly zcela od počáteční toofinish.</span><span class="sxs-lookup"><span data-stu-id="26437-201">It should take about five minutes for this console application toorun completely from start toofinish.</span></span> <span data-ttu-id="26437-202">Může trvat několik minut, po dokončení aplikace hello před všechny prostředky hello a hello skupiny prostředků se odstraní.</span><span class="sxs-lookup"><span data-stu-id="26437-202">It may take several minutes after hello application has finished before all hello resources and hello resource group are deleted.</span></span>


## <a name="next-steps"></a><span data-ttu-id="26437-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="26437-203">Next steps</span></span>

- <span data-ttu-id="26437-204">Pokud byly nějaké problémy s hello nasazení, je dalším krokem bude toolook v [řešení potíží s nasazením skupin prostředků pomocí portálu Azure](../../resource-manager-troubleshoot-deployments-portal.md)</span><span class="sxs-lookup"><span data-stu-id="26437-204">If there were issues with hello deployment, a next step would be toolook at [Troubleshooting resource group deployments with Azure portal](../../resource-manager-troubleshoot-deployments-portal.md)</span></span>
- <span data-ttu-id="26437-205">Další informace o hello [knihovna Python Azure](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)</span><span class="sxs-lookup"><span data-stu-id="26437-205">Learn more about hello [Azure Python Library](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)</span></span>

