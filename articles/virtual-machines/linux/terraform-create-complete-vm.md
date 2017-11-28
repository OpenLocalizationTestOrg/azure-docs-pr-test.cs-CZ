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
ms.openlocfilehash: 52591009ee7cb906402b8bca2ce63794ac7afcc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="d353a-103">Vytvořte základní infrastrukturu v Azure pomocí Terraform</span><span class="sxs-lookup"><span data-stu-id="d353a-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="d353a-104">Tento článek popisuje kroky hello tootake tooprovision virtuálního počítače, společně s podpůrné infrastruktuře, je nutné do Azure.</span><span class="sxs-lookup"><span data-stu-id="d353a-104">This article describes hello steps you need tootake tooprovision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="d353a-105">Se dozvíte, jak toowrite Terraform skripty a jak toovisualize hello změní před dosažení ve vaší infrastruktuře cloudu.</span><span class="sxs-lookup"><span data-stu-id="d353a-105">You will learn how toowrite Terraform scripts and how toovisualize hello changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="d353a-106">Taky se dozvíte, jak toocreate infrastrukturu v Azure pomocí Terraform.</span><span class="sxs-lookup"><span data-stu-id="d353a-106">You also will learn how toocreate infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="d353a-107">tooget spustili, vytvořte soubor s názvem \terraform_azure101.tf v textovém editoru výběru (Visual Studio Code nebo Sublime/Vim/atd.).</span><span class="sxs-lookup"><span data-stu-id="d353a-107">tooget started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="d353a-108">Hello přesný název hello souboru není důležité, protože Terraform přijímá název složky hello jako parametr: získat spouštět všechny skripty ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="d353a-108">hello exact name of hello file isn't important because Terraform accepts hello folder name as a parameter: all scripts in hello folder get executed.</span></span> <span data-ttu-id="d353a-109">Vložte následující kód do nového souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="d353a-109">Paste hello following code in hello new file:</span></span>

~~~~
# Configure hello Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have tooinclude this block
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
<span data-ttu-id="d353a-110">V hello `provider` části hello skriptu, se Terraform toouse zjistit prostředky služby Azure poskytovatele tooprovision ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="d353a-110">In hello `provider` section of hello script, you tell Terraform toouse an Azure provider tooprovision resources in hello script.</span></span> <span data-ttu-id="d353a-111">hodnoty tooget ID_ODBĚRU, appId, heslo a tenant_id, najdete v části hello [nainstalovat a nakonfigurovat Terraform](terraform-install-configure.md) průvodce.</span><span class="sxs-lookup"><span data-stu-id="d353a-111">tooget values for subscription_id, appId, password, and tenant_id, see hello [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="d353a-112">Pokud jste vytvořili proměnných prostředí pro hello hodnoty v tomto bloku, nepotřebujete tooinclude ho.</span><span class="sxs-lookup"><span data-stu-id="d353a-112">If you have created environment variables for hello values in this block, you don't need tooinclude it.</span></span> 

<span data-ttu-id="d353a-113">Hello `azurerm_resource_group` prostředků dá pokyn Terraform toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d353a-113">hello `azurerm_resource_group` resource instructs Terraform toocreate a new resource group.</span></span> <span data-ttu-id="d353a-114">Můžete zobrazit více typů prostředků, které jsou k dispozici v Terraform později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d353a-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-hello-script"></a><span data-ttu-id="d353a-115">Spuštění skriptu hello</span><span class="sxs-lookup"><span data-stu-id="d353a-115">Execute hello script</span></span>
<span data-ttu-id="d353a-116">Po uložení hello skriptu, ukončete toohello konzole nebo příkazového řádku a zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="d353a-116">After you save hello script, exit toohello console/command line, and type hello following:</span></span>
```
terraform init
```
 <span data-ttu-id="d353a-117">Zprostředkovatel Terraform hello tooinitialize pro Azure.</span><span class="sxs-lookup"><span data-stu-id="d353a-117">tooinitialize hello Terraform provider for Azure.</span></span> <span data-ttu-id="d353a-118">Potom změnit adresář, hello příliš**terraformscripts**, a problém hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d353a-118">Then change hello directory too**terraformscripts**, and issue hello following command:</span></span>
```
terraform plan
```
<span data-ttu-id="d353a-119">Použili jsme hello `plan` Terraform příkaz, který zjistí hello prostředky definované ve skriptech hello.</span><span class="sxs-lookup"><span data-stu-id="d353a-119">We used hello `plan` Terraform command, which looks at hello resources defined in hello scripts.</span></span> <span data-ttu-id="d353a-120">Porovná je ukládaná Terraform toohello informace o stavu a pak výstupy hello plánované provádění _bez_ ve skutečnosti vytváření prostředků v Azure.</span><span class="sxs-lookup"><span data-stu-id="d353a-120">It compares them toohello state information saved by Terraform and then outputs hello planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="d353a-121">Po provedení předchozí příkaz hello byste měli vidět něco podobného jako hello následující obrazovka:</span><span class="sxs-lookup"><span data-stu-id="d353a-121">After you execute hello previous command, you should see something like hello following screen:</span></span>

![Plán Terraform](./media/terraform/tf_plan2.png)

<span data-ttu-id="d353a-123">Pokud se vše vypadá v pořádku, zřídit tuto novou skupinu prostředků v Azure spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="d353a-123">If everything looks correct, provision this new resource group in Azure by executing hello following:</span></span> 
```
terraform apply
```
<span data-ttu-id="d353a-124">V hello portálu Azure, měli byste vidět hello novou prázdnou skupinu prostředků s názvem `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="d353a-124">In hello Azure portal, you should see hello new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="d353a-125">V následující části hello můžete přidat že virtuální počítač a všechny podpůrné infrastruktury pro tuto skupinu prostředků pro virtuální počítač toohello hello.</span><span class="sxs-lookup"><span data-stu-id="d353a-125">In hello following section, you add a virtual machine and all hello supporting infrastructure for that virtual machine toohello resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="d353a-126">Zřízení virtuálního počítače s Ubuntu s Terraform</span><span class="sxs-lookup"><span data-stu-id="d353a-126">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="d353a-127">Umožňuje rozšířit hello Terraform skript, který vytvořili jsme s hello podrobností, které jsou nezbytné tooprovision virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="d353a-127">Let's extend hello Terraform script we've created with hello details that are necessary tooprovision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="d353a-128">Hello prostředky, které můžete zřídit v hello následující části jsou:</span><span class="sxs-lookup"><span data-stu-id="d353a-128">hello resources that you provision in hello following sections are:</span></span>

* <span data-ttu-id="d353a-129">Síť s jedinou podsítí</span><span class="sxs-lookup"><span data-stu-id="d353a-129">A network with a single subnet</span></span>
* <span data-ttu-id="d353a-130">Karty síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="d353a-130">A network interface card</span></span> 
* <span data-ttu-id="d353a-131">Účet úložiště pro hello diagnostiky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d353a-131">A storage account for hello virtual machine diagnostics</span></span>
* <span data-ttu-id="d353a-132">Veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="d353a-132">A public IP</span></span>
* <span data-ttu-id="d353a-133">Virtuální počítač, který využívá všechny prostředky předchozí hello</span><span class="sxs-lookup"><span data-stu-id="d353a-133">A virtual machine that utilizes all hello previous resources</span></span> 

<span data-ttu-id="d353a-134">Důkladné dokumentaci pro každou hello Azure Terraform prostředků najdete v tématu hello [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="d353a-134">For thorough documentation for each of hello Azure Terraform resources, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="d353a-135">Hello plnou verzi hello [skriptu](#complete-terraform-script) jsou tu taky ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="d353a-135">hello full version of hello [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-hello-terraform-script"></a><span data-ttu-id="d353a-136">Rozšíření hello Terraform skriptu</span><span class="sxs-lookup"><span data-stu-id="d353a-136">Extend hello Terraform script</span></span>
<span data-ttu-id="d353a-137">Rozšíření hello skript, který byl vytvořen s hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="d353a-137">Extend hello script that was created with hello following resources:</span></span> 
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
<span data-ttu-id="d353a-138">předchozí skript Hello vytvoří virtuální síť a podsíť v rámci této virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d353a-138">hello previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="d353a-139">Všimněte si hello odkaz toohello prostředků skupiny, které jste již vytvořili prostřednictvím "${azurerm_resource_group.helloterraform.name}" hello virtuální sítě a definice podsítě hello.</span><span class="sxs-lookup"><span data-stu-id="d353a-139">Note hello reference toohello resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both hello virtual network and hello subnet definition.</span></span>

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
<span data-ttu-id="d353a-140">Hello předchozí skript fragmenty vytvoření veřejné IP adresy a síťové rozhraní, které využívá hello veřejnou IP adresu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="d353a-140">hello previous script snippets create a public IP and a network interface that makes use of hello public IP created.</span></span> <span data-ttu-id="d353a-141">Poznámka: hello odkazuje toosubnet_id a public_ip_address_id.</span><span class="sxs-lookup"><span data-stu-id="d353a-141">Note hello references toosubnet_id and public_ip_address_id.</span></span> <span data-ttu-id="d353a-142">Terraform má vestavěné inteligentní toounderstand této hello síťové rozhraní má závislost na prostředky hello této toobe třeba vytvořit před vytvořením hello hello síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d353a-142">Terraform has built-in intelligence toounderstand that hello network interface has a dependency on hello resources that need toobe created before hello creation of hello network interface.</span></span>

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
<span data-ttu-id="d353a-143">Zde můžete vytvořit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d353a-143">Here, you created a storage account.</span></span> <span data-ttu-id="d353a-144">Tento účet úložiště je, kde hello virtuálního počítače ukládat její podrobnosti diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="d353a-144">This storage account is where hello virtual machine will store its diagnostics details.</span></span>

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
<span data-ttu-id="d353a-145">Nakonec hello předchozí fragment kódu vytvoří virtuální počítač, který využívá už zřízené všechny prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="d353a-145">Finally, hello previous snippet creates a virtual machine that utilizes all hello resources provisioned already.</span></span> <span data-ttu-id="d353a-146">Jsou k účtu úložiště pro hello diagnostiky virtuálního počítače, je síťové rozhraní s veřejné IP adresy a Zadaná podsíť a skupině prostředků hello jste již vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d353a-146">They are a storage account for hello virtual machine diagnostics, a network interface with public IP and subnet specified, and hello resource group you already created.</span></span> <span data-ttu-id="d353a-147">Poznámka: vlastnost vm_size hello, kde hello skriptu Určuje standardní DS1v2 SKU Azure.</span><span class="sxs-lookup"><span data-stu-id="d353a-147">Note hello vm_size property, where hello script specifies an Azure Standard DS1v2 SKU.</span></span>

<span data-ttu-id="d353a-148">Jste požadované toosupply veřejný klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="d353a-148">You are required toosupply an SSH public key.</span></span> <span data-ttu-id="d353a-149">Umístěte hello hodnota veřejného klíče do části hello **... VLOŽTE VEŘEJNÝ KLÍČ OPENSSH ZDE...**  výše.</span><span class="sxs-lookup"><span data-stu-id="d353a-149">Place hello public key value into hello section **... INSERT OPENSSH PUBLIC KEY HERE ...** above.</span></span> <span data-ttu-id="d353a-150">Můžete použít existující ssh klíče dvojice nebo postupujte podle hello [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) nebo [systému Linux nebo macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) dokumentace toogenerate hello pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="d353a-150">You can use an existing ssh key pair or follow hello [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) or [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) documentation toogenerate hello key pair.</span></span>

### <a name="execute-hello-script"></a><span data-ttu-id="d353a-151">Spuštění skriptu hello</span><span class="sxs-lookup"><span data-stu-id="d353a-151">Execute hello script</span></span>
<span data-ttu-id="d353a-152">Pomocí skriptu úplné hello uložit ukončete toohello konzole nebo příkazového řádku a zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="d353a-152">With hello full script saved, exit toohello console/command line and type hello following:</span></span>
```
terraform apply
```
<span data-ttu-id="d353a-153">Po určité době hello zdroje, včetně virtuální počítač, se zobrazí v hello `terraformtest` skupiny prostředků v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d353a-153">After some time, hello resources, including a virtual machine, appear in hello `terraformtest` resource group in hello Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="d353a-154">Dokončení Terraform skriptu</span><span class="sxs-lookup"><span data-stu-id="d353a-154">Complete Terraform script</span></span>

<span data-ttu-id="d353a-155">Pro usnadnění vaší práce je pod dokončení skriptu Terraform hello této zřizuje všechny hello infrastruktury popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d353a-155">For your convenience, below is hello complete Terraform script that provisions all of hello infrastructure discussed in this article.</span></span>

```
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

## <a name="next-steps"></a><span data-ttu-id="d353a-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d353a-156">Next steps</span></span>
<span data-ttu-id="d353a-157">Základní infrastruktura jste vytvořili v Azure pomocí Terraform.</span><span class="sxs-lookup"><span data-stu-id="d353a-157">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="d353a-158">Složitější scénáře, včetně příklady, které pomocí služby Vyrovnávání zatížení a virtuální počítač škálování sad, najdete v tématu množství [Terraform příklady Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="d353a-158">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="d353a-159">Aktuální seznam podporovaných zprostředkovatelů Azure, najdete v části hello [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="d353a-159">For an up-to-date list of supported Azure providers, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
