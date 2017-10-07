---
title: "Základní infrastruktura aaaCreate v Azure pomocí Terraform | Microsoft Docs"
description: "Zjistěte, jak toocreate Azure prostředky pomocí Terraform"
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
ms.openlocfilehash: 916a838c118f28b3fbd373188e0acb2afc655081
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a>Vytvořte základní infrastrukturu v Azure pomocí Terraform
Tento článek popisuje kroky hello tootake tooprovision virtuálního počítače, společně s podpůrné infrastruktuře, je nutné do Azure. Se dozvíte, jak toowrite Terraform skripty a jak toovisualize hello změní před dosažení ve vaší infrastruktuře cloudu. Taky se dozvíte, jak toocreate infrastrukturu v Azure pomocí Terraform.

tooget spustili, vytvořte soubor s názvem \terraform_azure101.tf v textovém editoru výběru (Visual Studio Code nebo Sublime/Vim/atd.). Hello přesný název hello souboru není důležité, protože Terraform přijímá název složky hello jako parametr: získat spouštět všechny skripty ve složce hello. Vložte následující kód do nového souboru hello hello:

~~~~
# Configure hello Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have tooinclude this block
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
V hello `provider` části hello skriptu, se Terraform toouse zjistit prostředky služby Azure poskytovatele tooprovision ve skriptu hello. hodnoty tooget ID_ODBĚRU, appId, heslo a tenant_id, najdete v části hello [nainstalovat a nakonfigurovat Terraform](terraform-install-configure.md) průvodce. Pokud jste vytvořili proměnných prostředí pro hello hodnoty v tomto bloku, nepotřebujete tooinclude ho. 

Hello `azurerm_resource_group` prostředků dá pokyn Terraform toocreate novou skupinu prostředků. Můžete zobrazit více typů prostředků, které jsou k dispozici v Terraform později v tomto článku.

## <a name="execute-hello-script"></a>Spuštění skriptu hello
Po uložení hello skriptu, ukončete toohello konzole nebo příkazového řádku a zadejte následující hello:
```
terraform init
```
Zprostředkovatel Terraform tooinitialize pro Azure. Poté zadejte hello následující:
```
terraform plan terraformscripts
```
Budeme předpokládat, že `terraformscripts` je hello složku, kam byla uložena hello skriptu. Použili jsme hello `plan` Terraform příkaz, který zjistí hello prostředky definované ve skriptech hello. Porovná je ukládaná Terraform toohello informace o stavu a pak výstupy hello plánované provádění _bez_ ve skutečnosti vytváření prostředků v Azure. 

Po provedení předchozí příkaz hello byste měli vidět něco podobného jako hello následující obrazovka:

![Plán Terraform](linux/media/terraform/tf_plan2.png)

Pokud se vše vypadá v pořádku, zřídit tuto novou skupinu prostředků v Azure spuštěním hello následující: 
```
terraform apply terraformscripts
```
V hello portálu Azure, měli byste vidět hello novou prázdnou skupinu prostředků s názvem `terraformtest`. V následující části hello můžete přidat že virtuální počítač a všechny podpůrné infrastruktury pro tuto skupinu prostředků pro virtuální počítač toohello hello.

## <a name="provision-an-ubuntu-vm-with-terraform"></a>Zřízení virtuálního počítače s Ubuntu s Terraform
Umožňuje rozšířit hello Terraform skript, který vytvořili jsme s hello podrobností, které jsou nezbytné tooprovision virtuálního počítače s Ubuntu. Hello prostředky, které můžete zřídit v hello následující části jsou:

* Síť s jedinou podsítí
* Karty síťového rozhraní 
* Účet úložiště se kontejner úložiště
* Veřejnou IP adresu
* Virtuální počítač, který využívá všechny prostředky předchozí hello 

Důkladné dokumentaci pro každou hello Azure Terraform prostředků najdete v tématu hello [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).

Hello plnou verzi hello [skriptu](#complete-terraform-script) jsou tu taky ke zvýšení pohodlí.

### <a name="extend-hello-terraform-script"></a>Rozšíření hello Terraform skriptu
Rozšíření hello skript, který byl vytvořen s hello následující prostředky: 
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
předchozí skript Hello vytvoří virtuální síť a podsíť v rámci této virtuální sítě. Všimněte si hello odkaz toohello prostředků skupiny, které jste již vytvořili prostřednictvím "${azurerm_resource_group.helloterraform.name}" hello virtuální sítě a definice podsítě hello.

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
Hello předchozí skript fragmenty vytvoření veřejné IP adresy a síťové rozhraní, které využívá hello veřejnou IP adresu vytvořit. Poznámka: hello odkazuje toosubnet_id a public_ip_address_id. Terraform má vestavěné inteligentní toounderstand této hello síťové rozhraní má závislost na prostředky hello této toobe třeba vytvořit před vytvořením hello hello síťového rozhraní.

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
Zde vytvoříte účet úložiště a definovaný kontejner úložiště v rámci tohoto účtu úložiště. Tento účet úložiště se pro virtuální počítač hello o toobe vytvořit kam můžete ukládat virtuální pevné disky (VHD).

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
Nakonec hello předchozí fragment kódu vytvoří virtuální počítač, který využívá už zřízené všechny prostředky hello. Jsou účtu úložiště a kontejner pro virtuální pevný disk, je síťové rozhraní s veřejné IP adresy a podsítě zadán, a skupina prostředků hello jste již vytvořili. Poznámka: vlastnost vm_size hello, kde hello skriptu určuje A0 SKU služby Azure.

### <a name="execute-hello-script"></a>Spuštění skriptu hello
Pomocí skriptu úplné hello uložit ukončete toohello konzole nebo příkazového řádku a zadejte následující hello:
```
terraform apply terraformscripts
```
Po určité době hello zdroje, včetně virtuální počítač, se zobrazí v hello `terraformtest` skupiny prostředků v hello portálu Azure.

## <a name="complete-terraform-script"></a>Dokončení Terraform skriptu

Pro usnadnění vaší práce je pod dokončení skriptu Terraform hello této zřizuje všechny hello infrastruktury popsané v tomto článku.

```
variable "resourcesname" {
  default = "helloterraform"
}

# Configure hello Microsoft Azure Provider
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
Základní infrastruktura jste vytvořili v Azure pomocí Terraform. Složitější scénáře, včetně příklady, které pomocí služby Vyrovnávání zatížení a virtuální počítač škálování sad, najdete v tématu množství [Terraform příklady Azure](https://github.com/hashicorp/terraform/tree/master/examples). Aktuální seznam podporovaných zprostředkovatelů Azure, najdete v části hello [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).
