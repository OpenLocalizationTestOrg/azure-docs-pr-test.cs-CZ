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
ms.openlocfilehash: 9660a95b440c2e4311829979e270d9f10099f624
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
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

# create a resource group 
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
Inicializace zprostředkovatele Terraform pro Azure. Potom zadejte následující příkaz:
```
terraform plan terraformscripts
```
Budeme předpokládat, že `terraformscripts` je složka, kam byl uložen skript. Použili jsme `plan` Terraform příkaz, který vypadá na prostředky, které jsou definované ve skriptech. Se porovná je se uloží prostřednictvím Terraform informace o stavu a poté uloží plánované provádění _bez_ ve skutečnosti vytváření prostředků v Azure. 

Po provedení předchozí příkaz byste měli vidět něco podobného jako následující obrazovka:

![Plán Terraform](linux/media/terraform/tf_plan2.png)

Pokud se vše vypadá v pořádku, zřídit tuto novou skupinu prostředků v Azure tak, že spustíte následující: 
```
terraform apply terraformscripts
```
Na portálu Azure, měli byste vidět novou skupinu prázdný prostředků s názvem `terraformtest`. V následující části přidejte virtuální počítač a všechny podpůrné infrastruktury pro tento virtuální počítač do skupiny prostředků.

## <a name="provision-an-ubuntu-vm-with-terraform"></a>Zřízení virtuálního počítače s Ubuntu s Terraform
Umožňuje rozšířit Terraform skript, který vytvořili jsme s podrobnostmi, které jsou nezbytné ke zřízení virtuálního počítače s Ubuntu. Prostředky, které můžete zřídit v následujících částech jsou:

* Síť s jedinou podsítí
* Karty síťového rozhraní 
* Účet úložiště se kontejner úložiště
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
        environment = "TerraformDemo"
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
}
~~~~
Předchozí fragmenty skriptu vytvoření veřejné IP adresy a síťové rozhraní, které využívá veřejnou IP adresu vytvořit. Všimněte si, odkazy na subnet_id a public_ip_address_id. Terraform má vestavěné inteligentní si uvědomit, že síťové rozhraní má závislost na prostředcích, které je třeba vytvořit před vytvořením síťového rozhraní.

~~~~
# create a random id
resource "random_id" "randomId" {
  byte_length = 4
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "tfstorage${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "westus"
    account_type = "Standard_LRS"

    tags {
        environment = "staging"
    }
}

# create storage container
resource "azurerm_storage_container" "helloterraformstoragestoragecontainer" {
    name = "vhd"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    storage_account_name = "${azurerm_storage_account.helloterraformstorage.name}"
    container_access_type = "private"
    depends_on = ["azurerm_storage_account.helloterraformstorage"]
}
~~~~
Zde vytvoříte účet úložiště a definovaný kontejner úložiště v rámci tohoto účtu úložiště. Tento účet úložiště se, kam můžete ukládat virtuální pevné disky (VHD) pro virtuální počítač, o který se má vytvořit.

~~~~
# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_A0"

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "14.04.2-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        vhd_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}${azurerm_storage_container.helloterraformstoragestoragecontainer.name}/myosdisk.vhd"
        caching = "ReadWrite"
        create_option = "FromImage"
    }

    os_profile {
        computer_name = "hostname"
        admin_username = "testadmin"
        admin_password = "Password1234!"
    }

    os_profile_linux_config {
        disable_password_authentication = false
    }

    tags {
        environment = "staging"
    }
}
~~~~
Nakonec fragmentu předchozí vytvoří virtuální počítač, který využívá všechny prostředky, které jsou už zřízené. Jsou účtu úložiště a kontejner pro virtuální pevný disk, je síťové rozhraní s veřejné IP adresy a podsítě zadán, a skupině prostředků jste už vytvořili. Poznámka: vlastnost vm_size, kde skript určuje A0 SKU služby Azure.

### <a name="execute-the-script"></a>Spustit skript
Pomocí úplné skriptu uložit ukončete na konzole nebo příkazový řádek a zadejte následující příkaz:
```
terraform apply terraformscripts
```
Po určité době zdroje, včetně virtuální počítač, se zobrazí v `terraformtest` skupinu prostředků na portálu Azure.

## <a name="complete-terraform-script"></a>Dokončení Terraform skriptu

Pro usnadnění vaší práce je nižší než dokončení skriptu Terraform zřizuje, že všechny infrastruktury popsané v tomto článku.

```
variable "resourcesname" {
  default = "helloterraform"
}

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

# create virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "tfvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "tfsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}


# create public IPs
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "TerraformDemo"
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
}

# create a random id
resource "random_id" "randomId" {
  byte_length = 4
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "tfstorage${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "westus"
    account_type = "Standard_LRS"

    tags {
        environment = "staging"
    }
}

# create storage container
resource "azurerm_storage_container" "helloterraformstoragestoragecontainer" {
    name = "vhd"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    storage_account_name = "${azurerm_storage_account.helloterraformstorage.name}"
    container_access_type = "private"
    depends_on = ["azurerm_storage_account.helloterraformstorage"]
}

# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_A0"

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "14.04.2-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        vhd_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}${azurerm_storage_container.helloterraformstoragestoragecontainer.name}/myosdisk.vhd"
        caching = "ReadWrite"
        create_option = "FromImage"
    }

    os_profile {
        computer_name = "hostname"
        admin_username = "testadmin"
        admin_password = "Password1234!"
    }

    os_profile_linux_config {
        disable_password_authentication = false
    }

    tags {
        environment = "staging"
    }
}
```

## <a name="next-steps"></a>Další kroky
Základní infrastruktura jste vytvořili v Azure pomocí Terraform. Složitější scénáře, včetně příklady, které pomocí služby Vyrovnávání zatížení a virtuální počítač škálování sad, najdete v tématu množství [Terraform příklady Azure](https://github.com/hashicorp/terraform/tree/master/examples). Aktuální seznam podporovaných zprostředkovatelů Azure, najdete v článku [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).
