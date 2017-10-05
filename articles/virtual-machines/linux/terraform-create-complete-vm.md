---
title: "Vytvořte základní infrastrukturu v Azure pomocí Terraform | Microsoft Docs"
description: "Informace o vytváření prostředků Azure pomocí Terraform"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: aa0926de32dd85f4460bbfa84b7a6007e2199d51
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a>Vytvořte základní infrastrukturu v Azure pomocí Terraform
Tento článek popisuje kroky, které je třeba provést pro zřízení virtuálního počítače, společně s podpůrné infrastruktuře, do Azure. Se dozvíte, jak psát skripty Terraform a jak k vizualizaci změny před prováděním ve vaší infrastruktuře cloudu. Můžete taky se dozvíte, jak vytvořit infrastrukturu v Azure pomocí Terraform.

Abyste mohli začít, vytvořte soubor s názvem \terraform_azure101.tf v textovém editoru výběru (Visual Studio Code nebo Sublime/Vim/atd.). Přesný název souboru není důležité, protože Terraform přijímá jako parametr název složky: získání spouštět všechny skripty ve složce. V novém souboru vložte následující kód:

~~~~
# Configure the Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have to include this block
provider "azurerm" {
  subscription_id = "your_subscription_id_from_script_execution"
  client_id       = "your_appId_from_script_execution"
  client_secret   = "your_password_from_script_execution"
  tenant_id       = "your_tenant_id_from_script_execution"
}

# create a resource group if it doesn't exist
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}
~~~~
V `provider` část skriptu, řekněte Terraform použít poskytovatele Azure k prostředkům zřídit ve skriptu. Chcete-li získat hodnoty pro ID_ODBĚRU, appId, heslo a tenant_id, přečtěte si téma [nainstalovat a nakonfigurovat Terraform](terraform-install-configure.md) průvodce. Pokud jste vytvořili proměnných prostředí pro hodnoty v tomto bloku, nemusíte ho zahrňte. 

`azurerm_resource_group` Prostředků dá pokyn Terraform vytvořit novou skupinu prostředků. Můžete zobrazit více typů prostředků, které jsou k dispozici v Terraform později v tomto článku.

## <a name="execute-the-script"></a>Spustit skript
Po uložení skriptu, ukončete na konzole nebo příkazový řádek a zadejte následující příkaz:
```
terraform init
```
 Inicializace zprostředkovatele Terraform pro Azure. Potom změňte adresář na **terraformscripts**a vydejte následující příkaz:
```
terraform plan
```
Použili jsme `plan` Terraform příkaz, který vypadá na prostředky, které jsou definované ve skriptech. Se porovná je se uloží prostřednictvím Terraform informace o stavu a poté uloží plánované provádění _bez_ ve skutečnosti vytváření prostředků v Azure. 

Po provedení předchozí příkaz byste měli vidět něco podobného jako následující obrazovka:

![Plán Terraform](./media/terraform/tf_plan2.png)

Pokud se vše vypadá v pořádku, zřídit tuto novou skupinu prostředků v Azure tak, že spustíte následující: 
```
terraform apply
```
Na portálu Azure, měli byste vidět novou skupinu prázdný prostředků s názvem `terraformtest`. V následující části přidejte virtuální počítač a všechny podpůrné infrastruktury pro tento virtuální počítač do skupiny prostředků.

## <a name="provision-an-ubuntu-vm-with-terraform"></a>Zřízení virtuálního počítače s Ubuntu s Terraform
Umožňuje rozšířit Terraform skript, který vytvořili jsme s podrobnostmi, které jsou nezbytné ke zřízení virtuálního počítače s Ubuntu. Prostředky, které můžete zřídit v následujících částech jsou:

* Síť s jedinou podsítí
* Karty síťového rozhraní 
* Účet úložiště pro diagnostiku virtuálního počítače
* Veřejnou IP adresu
* Virtuální počítač, který využívá všechny předchozí prostředky 

Důkladné dokumentaci pro jednotlivé prostředky Azure Terraform najdete v tématu [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).

Plnou verzi [skriptu](#complete-terraform-script) jsou tu taky ke zvýšení pohodlí.

### <a name="extend-the-terraform-script"></a>Rozšíření Terraform skriptu
Rozšíření skript, který byl vytvořen v následujících zdrojích informací: 
~~~~
# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}
~~~~
Předchozí skript vytvoří virtuální síť a podsíť v rámci této virtuální sítě. Poznámka: odkaz na skupinu prostředků, které jste již vytvořili prostřednictvím "${azurerm_resource_group.helloterraform.name}" virtuální sítě a definice podsítě.

~~~~
# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# create network interface
resource "azurerm_network_interface" "helloterraformnic" {
    name = "tfni"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    ip_configuration {
        name = "testconfiguration1"
        subnet_id = "${azurerm_subnet.helloterraformsubnet.id}"
        private_ip_address_allocation = "static"
        private_ip_address = "10.0.2.5"
        public_ip_address_id = "${azurerm_public_ip.helloterraformips.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
Předchozí fragmenty skriptu vytvoření veřejné IP adresy a síťové rozhraní, které využívá veřejnou IP adresu vytvořit. Všimněte si, odkazy na subnet_id a public_ip_address_id. Terraform má vestavěné inteligentní si uvědomit, že síťové rozhraní má závislost na prostředcích, které je třeba vytvořit před vytvořením síťového rozhraní.

~~~~
# create a random id
resource "random_id" "randomId" {
  keepers = {
    # Generate a new id only when a new resource group is defined
    resource_group = "${azurerm_resource_group.helloterraform.name}"
  }

  byte_length = 8
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "West US"
    account_type = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
Zde můžete vytvořit účet úložiště. Tento účet úložiště je, kde virtuální počítač ukládat její podrobnosti diagnostiky.

~~~~
# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_DS1_v2"

    os_profile {
        computer_name = "hostname"
        admin_username = "azureuser"
        admin_password = ""
    }

    os_profile_linux_config {
        disable_password_authentication = true

        ssh_keys {
            path = "/home/azureuser/.ssh/authorized_keys"
            key_data = "... INSERT OPENSSH PUBLIC KEY HERE ..."
        }
    }

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "16.04.0-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        managed_disk_type = "Premium_LRS"
        create_option = "FromImage"
    } 

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
Nakonec fragmentu předchozí vytvoří virtuální počítač, který využívá všechny prostředky, které jsou už zřízené. Jsou k účtu úložiště pro diagnostiku virtuálního počítače, je síťové rozhraní s veřejnou IP adresu a Zadaná podsíť a skupině prostředků jste už vytvořili. Poznámka: vlastnost vm_size, kde skript Určuje standardní DS1v2 SKU Azure.

Je nutné zadat veřejný klíč SSH. Umístěte hodnota veřejného klíče do části **... VLOŽTE VEŘEJNÝ KLÍČ OPENSSH ZDE...**  výše. Můžete použít existující ssh klíč spárujte nebo postupujte podle [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) nebo [systému Linux nebo macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) dokumentaci ke generování páru klíčů.

### <a name="execute-the-script"></a>Spustit skript
Pomocí úplné skriptu uložit ukončete na konzole nebo příkazový řádek a zadejte následující příkaz:
```
terraform apply
```
Po určité době zdroje, včetně virtuální počítač, se zobrazí v `terraformtest` skupinu prostředků na portálu Azure.

## <a name="complete-terraform-script"></a>Dokončení Terraform skriptu

Pro usnadnění vaší práce je nižší než dokončení skriptu Terraform zřizuje, že všechny infrastruktury popsané v tomto článku.

```
# Configure the Microsoft Azure Provider
provider "azurerm" {
  subscription_id = "XXX"
  client_id       = "XXX"
  client_secret   = "XXX"
  tenant_id       = "XXX"
}

# create a resource group if it doesn't exist
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}

# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}

# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# create network interface
resource "azurerm_network_interface" "helloterraformnic" {
    name = "tfni"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    ip_configuration {
        name = "testconfiguration1"
        subnet_id = "${azurerm_subnet.helloterraformsubnet.id}"
        private_ip_address_allocation = "static"
        private_ip_address = "10.0.2.5"
        public_ip_address_id = "${azurerm_public_ip.helloterraformips.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}

# create a random id
resource "random_id" "randomId" {
  keepers = {
    # Generate a new id only when a new resource group is defined
    resource_group = "${azurerm_resource_group.helloterraform.name}"
  }

  byte_length = 8
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "West US"
    account_type = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}

# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_DS1_v2"

    os_profile {
        computer_name = "hostname"
        admin_username = "azureuser"
        admin_password = ""
    }

    os_profile_linux_config {
        disable_password_authentication = true

        ssh_keys {
            path = "/home/azureuser/.ssh/authorized_keys"
            key_data = "... INSERT OPENSSH PUBLIC KEY HERE ..."
        }
    }

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "16.04.0-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        managed_disk_type = "Premium_LRS"
        create_option = "FromImage"
    } 

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```

## <a name="next-steps"></a>Další kroky
Základní infrastruktura jste vytvořili v Azure pomocí Terraform. Složitější scénáře, včetně příklady, které pomocí služby Vyrovnávání zatížení a virtuální počítač škálování sad, najdete v tématu množství [Terraform příklady Azure](https://github.com/hashicorp/terraform/tree/master/examples). Aktuální seznam podporovaných zprostředkovatelů Azure, najdete v článku [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).
