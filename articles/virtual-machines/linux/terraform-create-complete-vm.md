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
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="d7506-103">Vytvořte základní infrastrukturu v Azure pomocí Terraform</span><span class="sxs-lookup"><span data-stu-id="d7506-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="d7506-104">Tento článek popisuje kroky, které je třeba provést pro zřízení virtuálního počítače, společně s podpůrné infrastruktuře, do Azure.</span><span class="sxs-lookup"><span data-stu-id="d7506-104">This article describes the steps you need to take to provision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="d7506-105">Se dozvíte, jak psát skripty Terraform a jak k vizualizaci změny před prováděním ve vaší infrastruktuře cloudu.</span><span class="sxs-lookup"><span data-stu-id="d7506-105">You will learn how to write Terraform scripts and how to visualize the changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="d7506-106">Můžete taky se dozvíte, jak vytvořit infrastrukturu v Azure pomocí Terraform.</span><span class="sxs-lookup"><span data-stu-id="d7506-106">You also will learn how to create infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="d7506-107">Abyste mohli začít, vytvořte soubor s názvem \terraform_azure101.tf v textovém editoru výběru (Visual Studio Code nebo Sublime/Vim/atd.).</span><span class="sxs-lookup"><span data-stu-id="d7506-107">To get started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="d7506-108">Přesný název souboru není důležité, protože Terraform přijímá jako parametr název složky: získání spouštět všechny skripty ve složce.</span><span class="sxs-lookup"><span data-stu-id="d7506-108">The exact name of the file isn't important because Terraform accepts the folder name as a parameter: all scripts in the folder get executed.</span></span> <span data-ttu-id="d7506-109">V novém souboru vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="d7506-109">Paste the following code in the new file:</span></span>

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
<span data-ttu-id="d7506-110">V `provider` část skriptu, řekněte Terraform použít poskytovatele Azure k prostředkům zřídit ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="d7506-110">In the `provider` section of the script, you tell Terraform to use an Azure provider to provision resources in the script.</span></span> <span data-ttu-id="d7506-111">Chcete-li získat hodnoty pro ID_ODBĚRU, appId, heslo a tenant_id, přečtěte si téma [nainstalovat a nakonfigurovat Terraform](terraform-install-configure.md) průvodce.</span><span class="sxs-lookup"><span data-stu-id="d7506-111">To get values for subscription_id, appId, password, and tenant_id, see the [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="d7506-112">Pokud jste vytvořili proměnných prostředí pro hodnoty v tomto bloku, nemusíte ho zahrňte.</span><span class="sxs-lookup"><span data-stu-id="d7506-112">If you have created environment variables for the values in this block, you don't need to include it.</span></span> 

<span data-ttu-id="d7506-113">`azurerm_resource_group` Prostředků dá pokyn Terraform vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d7506-113">The `azurerm_resource_group` resource instructs Terraform to create a new resource group.</span></span> <span data-ttu-id="d7506-114">Můžete zobrazit více typů prostředků, které jsou k dispozici v Terraform později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d7506-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-the-script"></a><span data-ttu-id="d7506-115">Spustit skript</span><span class="sxs-lookup"><span data-stu-id="d7506-115">Execute the script</span></span>
<span data-ttu-id="d7506-116">Po uložení skriptu, ukončete na konzole nebo příkazový řádek a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d7506-116">After you save the script, exit to the console/command line, and type the following:</span></span>
```
terraform init
```
 <span data-ttu-id="d7506-117">Inicializace zprostředkovatele Terraform pro Azure.</span><span class="sxs-lookup"><span data-stu-id="d7506-117">to initialize the Terraform provider for Azure.</span></span> <span data-ttu-id="d7506-118">Potom změňte adresář na **terraformscripts**a vydejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d7506-118">Then change the directory to **terraformscripts**, and issue the following command:</span></span>
```
terraform plan
```
<span data-ttu-id="d7506-119">Použili jsme `plan` Terraform příkaz, který vypadá na prostředky, které jsou definované ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="d7506-119">We used the `plan` Terraform command, which looks at the resources defined in the scripts.</span></span> <span data-ttu-id="d7506-120">Se porovná je se uloží prostřednictvím Terraform informace o stavu a poté uloží plánované provádění _bez_ ve skutečnosti vytváření prostředků v Azure.</span><span class="sxs-lookup"><span data-stu-id="d7506-120">It compares them to the state information saved by Terraform and then outputs the planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="d7506-121">Po provedení předchozí příkaz byste měli vidět něco podobného jako následující obrazovka:</span><span class="sxs-lookup"><span data-stu-id="d7506-121">After you execute the previous command, you should see something like the following screen:</span></span>

![Plán Terraform](./media/terraform/tf_plan2.png)

<span data-ttu-id="d7506-123">Pokud se vše vypadá v pořádku, zřídit tuto novou skupinu prostředků v Azure tak, že spustíte následující:</span><span class="sxs-lookup"><span data-stu-id="d7506-123">If everything looks correct, provision this new resource group in Azure by executing the following:</span></span> 
```
terraform apply
```
<span data-ttu-id="d7506-124">Na portálu Azure, měli byste vidět novou skupinu prázdný prostředků s názvem `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="d7506-124">In the Azure portal, you should see the new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="d7506-125">V následující části přidejte virtuální počítač a všechny podpůrné infrastruktury pro tento virtuální počítač do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="d7506-125">In the following section, you add a virtual machine and all the supporting infrastructure for that virtual machine to the resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="d7506-126">Zřízení virtuálního počítače s Ubuntu s Terraform</span><span class="sxs-lookup"><span data-stu-id="d7506-126">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="d7506-127">Umožňuje rozšířit Terraform skript, který vytvořili jsme s podrobnostmi, které jsou nezbytné ke zřízení virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="d7506-127">Let's extend the Terraform script we've created with the details that are necessary to provision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="d7506-128">Prostředky, které můžete zřídit v následujících částech jsou:</span><span class="sxs-lookup"><span data-stu-id="d7506-128">The resources that you provision in the following sections are:</span></span>

* <span data-ttu-id="d7506-129">Síť s jedinou podsítí</span><span class="sxs-lookup"><span data-stu-id="d7506-129">A network with a single subnet</span></span>
* <span data-ttu-id="d7506-130">Karty síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="d7506-130">A network interface card</span></span> 
* <span data-ttu-id="d7506-131">Účet úložiště pro diagnostiku virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d7506-131">A storage account for the virtual machine diagnostics</span></span>
* <span data-ttu-id="d7506-132">Veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="d7506-132">A public IP</span></span>
* <span data-ttu-id="d7506-133">Virtuální počítač, který využívá všechny předchozí prostředky</span><span class="sxs-lookup"><span data-stu-id="d7506-133">A virtual machine that utilizes all the previous resources</span></span> 

<span data-ttu-id="d7506-134">Důkladné dokumentaci pro jednotlivé prostředky Azure Terraform najdete v tématu [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="d7506-134">For thorough documentation for each of the Azure Terraform resources, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="d7506-135">Plnou verzi [skriptu](#complete-terraform-script) jsou tu taky ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="d7506-135">The full version of the [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-the-terraform-script"></a><span data-ttu-id="d7506-136">Rozšíření Terraform skriptu</span><span class="sxs-lookup"><span data-stu-id="d7506-136">Extend the Terraform script</span></span>
<span data-ttu-id="d7506-137">Rozšíření skript, který byl vytvořen v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="d7506-137">Extend the script that was created with the following resources:</span></span> 
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
<span data-ttu-id="d7506-138">Předchozí skript vytvoří virtuální síť a podsíť v rámci této virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d7506-138">The previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="d7506-139">Poznámka: odkaz na skupinu prostředků, které jste již vytvořili prostřednictvím "${azurerm_resource_group.helloterraform.name}" virtuální sítě a definice podsítě.</span><span class="sxs-lookup"><span data-stu-id="d7506-139">Note the reference to the resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both the virtual network and the subnet definition.</span></span>

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
<span data-ttu-id="d7506-140">Předchozí fragmenty skriptu vytvoření veřejné IP adresy a síťové rozhraní, které využívá veřejnou IP adresu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="d7506-140">The previous script snippets create a public IP and a network interface that makes use of the public IP created.</span></span> <span data-ttu-id="d7506-141">Všimněte si, odkazy na subnet_id a public_ip_address_id.</span><span class="sxs-lookup"><span data-stu-id="d7506-141">Note the references to subnet_id and public_ip_address_id.</span></span> <span data-ttu-id="d7506-142">Terraform má vestavěné inteligentní si uvědomit, že síťové rozhraní má závislost na prostředcích, které je třeba vytvořit před vytvořením síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d7506-142">Terraform has built-in intelligence to understand that the network interface has a dependency on the resources that need to be created before the creation of the network interface.</span></span>

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
<span data-ttu-id="d7506-143">Zde můžete vytvořit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d7506-143">Here, you created a storage account.</span></span> <span data-ttu-id="d7506-144">Tento účet úložiště je, kde virtuální počítač ukládat její podrobnosti diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="d7506-144">This storage account is where the virtual machine will store its diagnostics details.</span></span>

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
<span data-ttu-id="d7506-145">Nakonec fragmentu předchozí vytvoří virtuální počítač, který využívá všechny prostředky, které jsou už zřízené.</span><span class="sxs-lookup"><span data-stu-id="d7506-145">Finally, the previous snippet creates a virtual machine that utilizes all the resources provisioned already.</span></span> <span data-ttu-id="d7506-146">Jsou k účtu úložiště pro diagnostiku virtuálního počítače, je síťové rozhraní s veřejnou IP adresu a Zadaná podsíť a skupině prostředků jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d7506-146">They are a storage account for the virtual machine diagnostics, a network interface with public IP and subnet specified, and the resource group you already created.</span></span> <span data-ttu-id="d7506-147">Poznámka: vlastnost vm_size, kde skript Určuje standardní DS1v2 SKU Azure.</span><span class="sxs-lookup"><span data-stu-id="d7506-147">Note the vm_size property, where the script specifies an Azure Standard DS1v2 SKU.</span></span>

<span data-ttu-id="d7506-148">Je nutné zadat veřejný klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="d7506-148">You are required to supply an SSH public key.</span></span> <span data-ttu-id="d7506-149">Umístěte hodnota veřejného klíče do části **... VLOŽTE VEŘEJNÝ KLÍČ OPENSSH ZDE...**  výše.</span><span class="sxs-lookup"><span data-stu-id="d7506-149">Place the public key value into the section **... INSERT OPENSSH PUBLIC KEY HERE ...** above.</span></span> <span data-ttu-id="d7506-150">Můžete použít existující ssh klíč spárujte nebo postupujte podle [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) nebo [systému Linux nebo macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) dokumentaci ke generování páru klíčů.</span><span class="sxs-lookup"><span data-stu-id="d7506-150">You can use an existing ssh key pair or follow the [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) or [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) documentation to generate the key pair.</span></span>

### <a name="execute-the-script"></a><span data-ttu-id="d7506-151">Spustit skript</span><span class="sxs-lookup"><span data-stu-id="d7506-151">Execute the script</span></span>
<span data-ttu-id="d7506-152">Pomocí úplné skriptu uložit ukončete na konzole nebo příkazový řádek a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d7506-152">With the full script saved, exit to the console/command line and type the following:</span></span>
```
terraform apply
```
<span data-ttu-id="d7506-153">Po určité době zdroje, včetně virtuální počítač, se zobrazí v `terraformtest` skupinu prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d7506-153">After some time, the resources, including a virtual machine, appear in the `terraformtest` resource group in the Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="d7506-154">Dokončení Terraform skriptu</span><span class="sxs-lookup"><span data-stu-id="d7506-154">Complete Terraform script</span></span>

<span data-ttu-id="d7506-155">Pro usnadnění vaší práce je nižší než dokončení skriptu Terraform zřizuje, že všechny infrastruktury popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d7506-155">For your convenience, below is the complete Terraform script that provisions all of the infrastructure discussed in this article.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d7506-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7506-156">Next steps</span></span>
<span data-ttu-id="d7506-157">Základní infrastruktura jste vytvořili v Azure pomocí Terraform.</span><span class="sxs-lookup"><span data-stu-id="d7506-157">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="d7506-158">Složitější scénáře, včetně příklady, které pomocí služby Vyrovnávání zatížení a virtuální počítač škálování sad, najdete v tématu množství [Terraform příklady Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="d7506-158">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="d7506-159">Aktuální seznam podporovaných zprostředkovatelů Azure, najdete v článku [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="d7506-159">For an up-to-date list of supported Azure providers, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
