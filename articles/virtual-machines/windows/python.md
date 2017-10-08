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
# <a name="create-and-manage-windows-vms-in-azure-using-python"></a>Vytvářet a spravovat virtuální počítače Windows v Azure pomocí Python

[Virtuální počítač Azure](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) vyžaduje několik podpůrných prostředků Azure. Tento článek popisuje vytváření, správu a odstraňování prostředky virtuálních počítačů pomocí Pythonu. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvoření projektu ve Visual Studiu
> * Instalovat balíčky
> * Vytvořit přihlašovací údaje
> * Vytvoření prostředků
> * Provádění úloh správy
> * Odstraňte prostředky
> * Spuštění aplikace hello

Trvá přibližně 20 minut toodo tyto kroky.

## <a name="create-a-visual-studio-project"></a>Vytvoření projektu ve Visual Studiu

1. Pokud jste to ještě neudělali, nainstalujte [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Vyberte **vývoj Python** na hello zatížení stránky a potom klikněte na **nainstalovat**. V hello souhrn, můžete uvidíte, že **Python 3 64-bit (3.6.0)** je automaticky vybrána pro vás. Pokud jste již nainstalovali Visual Studio, můžete přidat hello Python zatížení pomocí hello Spouštěče Visual Studio.
2. Po instalaci a spuštění sady Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.
3. Klikněte na tlačítko **šablony** > **Python** > **aplikace Python**, zadejte *myPythonProject* pro název hello hello projektu, vyberte umístění hello hello projektu a pak klikněte na tlačítko **OK**.

## <a name="install-packages"></a>Instalovat balíčky

1. V Průzkumníku řešení klikněte v části *myPythonProject*, klikněte pravým tlačítkem na **prostředí Python**a potom vyberte **přidat virtuální prostředí**.
2. Na úvodní obrazovka Přidání virtuálního prostředí, přijměte výchozí název hello *env*, ujistěte se, že *3.6 Python (64 bitů)* pro základní překladač hello je vybrána a pak klikněte na tlačítko **vytvořit**.
3. Klikněte pravým tlačítkem na hello *env* prostředí, které jste vytvořili, klikněte na tlačítko **instalovat balíček Python**, zadejte *azure* v hello vyhledávacího pole a stiskněte klávesu Enter.

Měli byste vidět v oknech výstupu hello balíčky hello azure byly úspěšně nainstalovány. 

## <a name="create-credentials"></a>Vytvořit přihlašovací údaje

Než začnete tento krok, ujistěte se, že máte [objektu služby Active Directory](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Také byste měli zaznamenat ID aplikace hello hello ověřovací klíč a hello ID klienta, který budete potřebovat později.

1. Otevřete *myPythonProject.py* soubor, který byl vytvořen a poté přidejte tento kód tooenable toorun vaší aplikace:

    ```python
    if __name__ == "__main__":
    ```

2. tooimport hello kód, který je potřeba, přidejte tyto příkazy toohello horní části souboru .py hello:

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. Dále v souboru .py hello, přidejte proměnné po příkazy pro import hello toospecify běžné hodnoty použít v hello kódu:
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    Nahraďte **id předplatného** s ID vašeho předplatného.

4. pověření služby Active Directory hello toocreate, je nutné, aby toomake požadavků, přidejte tuto funkci po hello proměnné v souboru .py hello:

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    Nahraďte **id aplikace**, **ověřovací klíč**, a **id klienta** hello hodnotami, které jste dříve shromážděna při vytváření služby Azure Active Directory instanční objekt.

5. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a>Vytvoření prostředků
 
### <a name="initialize-management-clients"></a>Inicializace klientů pro správu

Klienti pro správu jsou potřebné toocreate a spravovat prostředky pomocí hello Python SDK v Azure. toocreate hello správu klientů, přidejte tento kód pod hello **Pokud** údajů na adrese pak konec souboru .py hello:

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

### <a name="create-hello-vm-and-supporting-resources"></a>Vytvoření hello virtuálních počítačů a podpůrné prostředky

Musí být všechny prostředky obsažené v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).

1. toocreate skupinu prostředků, přidejte tuto funkci po hello proměnné v souboru .py hello:

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter toocontinue...')
    ```

[Skupiny dostupnosti](tutorial-availability-sets.md) bylo snazší pro vás toomaintain hello virtuální počítače používané vaší aplikace.

1. toocreate dostupnosti nastaven, přidejte tuto funkci po hello proměnné v souboru .py hello:
   
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

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter toocontinue...')
    ```

A [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) je potřebné toocommunicate s hello virtuálního počítače.

1. toocreate veřejnou IP adresu pro virtuální počítač hello, přidejte tuto funkci po hello proměnné v souboru .py hello:

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

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

Virtuální počítač musí být v podsíti [virtuální síť](../../virtual-network/virtual-networks-overview.md).

1. toocreate virtuální sítě, přidejte tuto funkci po hello proměnné v souboru .py hello:

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

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

3. tooadd toohello podsíť virtuální sítě, přidejte tuto funkci po hello proměnné v souboru .py hello:
    
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
        
4. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

Virtuální počítač vyžaduje toocommunicate rozhraní sítě ve virtuální síti hello.

1. toocreate síťového rozhraní, přidejte tuto funkci po hello proměnné v souboru .py hello:

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

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

Teď, když jste vytvořili všechny hello Podpora prostředků, můžete vytvořit virtuální počítač.

1. toocreate hello virtuálního počítače, přidejte tuto funkci po hello proměnné v souboru .py hello:
   
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
    > V tomto kurzu vytvoří virtuální počítač s verzí operačního systému Windows Server hello. toolearn Další informace o výběru ostatní Image, najdete v části [vyhledání a výběr imagí virtuálních počítačů Azure pomocí prostředí Windows PowerShell a rozhraní příkazového řádku Azure hello](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

## <a name="perform-management-tasks"></a>Provádění úloh správy

Během životního cyklu hello virtuálního počítače můžete úlohy správy toorun například spuštění, zastavení nebo odstranění virtuálního počítače. Kromě toho můžete toocreate kód tooautomate opakovaných nebo komplexní úlohy.

### <a name="get-information-about-hello-vm"></a>Získání informací o hello virtuálních počítačů

1. tooget informace o virtuálním počítači hello, přidejte tuto funkci po hello proměnné v souboru .py hello:

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
2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter toocontinue...')
    ```

### <a name="stop-hello-vm"></a>Zastavit hello virtuálních počítačů

Můžete zastavit virtuální počítač a ponechat jeho nastavení, ale pokračovat toobe účtovat pro něj nebo můžete zastavit virtuální počítač a jeho navrácení. Při zrušení jeho přidělení virtuálního počítače jsou končí deallocated a fakturace pro ni také všechny prostředky, které jsou s ním spojená.

1. toostop hello virtuální počítač bez rušení přidělení, přidejte tuto funkci po hello proměnné v souboru .py hello:

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    Pokud chcete toodeallocate hello virtuální počítač, změňte hello power_off volání toothis kódu:

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    stop_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="start-hello-vm"></a>Hello spuštění virtuálního počítače

1. toostart hello virtuálního počítače, přidejte tuto funkci po hello proměnné v souboru .py hello:

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    start_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="resize-hello-vm"></a>Změnit velikost hello virtuálních počítačů

Mnoho aspektů nasazení měli zvážit při rozhodování o velikosti pro virtuální počítač. Další informace najdete v tématu [velikosti virtuálních počítačů](sizes.md).

1. velikost hello toochange hello virtuálního počítače, přidejte tuto funkci po hello proměnné v souboru .py hello:

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

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter toocontinue...')
    ```

### <a name="add-a-data-disk-toohello-vm"></a>Přidání disku toohello data virtuálního počítače

Virtuální počítače může mít jeden nebo více [datových disků](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , jsou uloženy jako soubory VHD.

1. tooadd datového disku toohello virtuálního počítače, přidejte tuto funkci po hello proměnné v souboru .py hello: 

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

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter toocontinue...')
    ```

## <a name="delete-resources"></a>Odstraňte prostředky

Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, je vždy prostředky toodelete osvědčených postupů, které už nejsou potřeba. Pokud chcete, aby toodelete hello virtuální počítače a všechny hello podpora prostředky, všechny máte toodo je odstranit skupinu prostředků hello.

1. Skupina prostředků hello toodelete a všechny prostředky, přidejte tuto funkci po hello proměnné v souboru .py hello:
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. Funkce hello toocall, kterou jste dříve přidali, přidejte tento kód pod hello **Pokud** příkaz na konci hello soubor .py hello:
   
    ```python
    delete_resources(resource_group_client)
    ```

3. Uložit *myPythonProject.py*.

## <a name="run-hello-application"></a>Spuštění aplikace hello

1. toorun hello konzolovou aplikaci, klikněte na tlačítko **spustit** v sadě Visual Studio.

2. Stiskněte klávesu **Enter** po je vrácen hello stav každého prostředku. Informace o stavu hello byste měli vidět **úspěšné** Stav zřizování. Po vytvoření virtuálního počítače hello máte možnost toodelete hello všechny hello prostředky, které vytvoříte. Před stisknutím klávesy **Enter** toostart odstraňování prostředky, vám může trvat několik minut tooverify jejich vytvoření v hello portálu Azure. Pokud máte hello portálu Azure otevřete, může být toorefresh hello okno toosee nové prostředky.  

    Má trvat přibližně pět minut, než tato toorun aplikace konzoly zcela od počáteční toofinish. Může trvat několik minut, po dokončení aplikace hello před všechny prostředky hello a hello skupiny prostředků se odstraní.


## <a name="next-steps"></a>Další kroky

- Pokud byly nějaké problémy s hello nasazení, je dalším krokem bude toolook v [řešení potíží s nasazením skupin prostředků pomocí portálu Azure](../../resource-manager-troubleshoot-deployments-portal.md)
- Další informace o hello [knihovna Python Azure](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)

