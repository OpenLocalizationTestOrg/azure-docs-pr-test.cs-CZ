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
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="b4334-103">Vytvořte základní infrastrukturu v Azure pomocí Terraform</span><span class="sxs-lookup"><span data-stu-id="b4334-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="b4334-104">Tento článek popisuje kroky, které je třeba provést pro zřízení virtuálního počítače, společně s podpůrné infrastruktuře, do Azure.</span><span class="sxs-lookup"><span data-stu-id="b4334-104">This article describes the steps you need to take to provision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="b4334-105">Se dozvíte, jak psát skripty Terraform a jak k vizualizaci změny před prováděním ve vaší infrastruktuře cloudu.</span><span class="sxs-lookup"><span data-stu-id="b4334-105">You will learn how to write Terraform scripts and how to visualize the changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="b4334-106">Můžete taky se dozvíte, jak vytvořit infrastrukturu v Azure pomocí Terraform.</span><span class="sxs-lookup"><span data-stu-id="b4334-106">You also will learn how to create infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="b4334-107">Abyste mohli začít, vytvořte soubor s názvem \terraform_azure101.tf v textovém editoru výběru (Visual Studio Code nebo Sublime/Vim/atd.).</span><span class="sxs-lookup"><span data-stu-id="b4334-107">To get started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="b4334-108">Přesný název souboru není důležité, protože Terraform přijímá jako parametr název složky: získání spouštět všechny skripty ve složce.</span><span class="sxs-lookup"><span data-stu-id="b4334-108">The exact name of the file isn't important because Terraform accepts the folder name as a parameter: all scripts in the folder get executed.</span></span> <span data-ttu-id="b4334-109">V novém souboru vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="b4334-109">Paste the following code in the new file:</span></span>

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
<span data-ttu-id="b4334-110">V `provider` část skriptu, řekněte Terraform použít poskytovatele Azure k prostředkům zřídit ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="b4334-110">In the `provider` section of the script, you tell Terraform to use an Azure provider to provision resources in the script.</span></span> <span data-ttu-id="b4334-111">Chcete-li získat hodnoty pro ID_ODBĚRU, appId, heslo a tenant_id, přečtěte si téma [nainstalovat a nakonfigurovat Terraform](terraform-install-configure.md) průvodce.</span><span class="sxs-lookup"><span data-stu-id="b4334-111">To get values for subscription_id, appId, password, and tenant_id, see the [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="b4334-112">Pokud jste vytvořili proměnných prostředí pro hodnoty v tomto bloku, nemusíte ho zahrňte.</span><span class="sxs-lookup"><span data-stu-id="b4334-112">If you have created environment variables for the values in this block, you don't need to include it.</span></span> 

<span data-ttu-id="b4334-113">`azurerm_resource_group` Prostředků dá pokyn Terraform vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4334-113">The `azurerm_resource_group` resource instructs Terraform to create a new resource group.</span></span> <span data-ttu-id="b4334-114">Můžete zobrazit více typů prostředků, které jsou k dispozici v Terraform později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="b4334-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-the-script"></a><span data-ttu-id="b4334-115">Spustit skript</span><span class="sxs-lookup"><span data-stu-id="b4334-115">Execute the script</span></span>
<span data-ttu-id="b4334-116">Po uložení skriptu, ukončete na konzole nebo příkazový řádek a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b4334-116">After you save the script, exit to the console/command line, and type the following:</span></span>
```
terraform init
```
<span data-ttu-id="b4334-117">Inicializace zprostředkovatele Terraform pro Azure.</span><span class="sxs-lookup"><span data-stu-id="b4334-117">to initialize Terraform provider for Azure.</span></span> <span data-ttu-id="b4334-118">Potom zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b4334-118">Then type the following:</span></span>
```
terraform plan terraformscripts
```
<span data-ttu-id="b4334-119">Budeme předpokládat, že `terraformscripts` je složka, kam byl uložen skript.</span><span class="sxs-lookup"><span data-stu-id="b4334-119">We assume that `terraformscripts` is the folder where the script was saved.</span></span> <span data-ttu-id="b4334-120">Použili jsme `plan` Terraform příkaz, který vypadá na prostředky, které jsou definované ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="b4334-120">We used the `plan` Terraform command, which looks at the resources defined in the scripts.</span></span> <span data-ttu-id="b4334-121">Se porovná je se uloží prostřednictvím Terraform informace o stavu a poté uloží plánované provádění _bez_ ve skutečnosti vytváření prostředků v Azure.</span><span class="sxs-lookup"><span data-stu-id="b4334-121">It compares them to the state information saved by Terraform and then outputs the planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="b4334-122">Po provedení předchozí příkaz byste měli vidět něco podobného jako následující obrazovka:</span><span class="sxs-lookup"><span data-stu-id="b4334-122">After you execute the previous command, you should see something like the following screen:</span></span>

![Plán Terraform](linux/media/terraform/tf_plan2.png)

<span data-ttu-id="b4334-124">Pokud se vše vypadá v pořádku, zřídit tuto novou skupinu prostředků v Azure tak, že spustíte následující:</span><span class="sxs-lookup"><span data-stu-id="b4334-124">If everything looks correct, provision this new resource group in Azure by executing the following:</span></span> 
```
terraform apply terraformscripts
```
<span data-ttu-id="b4334-125">Na portálu Azure, měli byste vidět novou skupinu prázdný prostředků s názvem `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="b4334-125">In the Azure portal, you should see the new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="b4334-126">V následující části přidejte virtuální počítač a všechny podpůrné infrastruktury pro tento virtuální počítač do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4334-126">In the following section, you add a virtual machine and all the supporting infrastructure for that virtual machine to the resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="b4334-127">Zřízení virtuálního počítače s Ubuntu s Terraform</span><span class="sxs-lookup"><span data-stu-id="b4334-127">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="b4334-128">Umožňuje rozšířit Terraform skript, který vytvořili jsme s podrobnostmi, které jsou nezbytné ke zřízení virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="b4334-128">Let's extend the Terraform script we've created with the details that are necessary to provision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="b4334-129">Prostředky, které můžete zřídit v následujících částech jsou:</span><span class="sxs-lookup"><span data-stu-id="b4334-129">The resources that you provision in the following sections are:</span></span>

* <span data-ttu-id="b4334-130">Síť s jedinou podsítí</span><span class="sxs-lookup"><span data-stu-id="b4334-130">A network with a single subnet</span></span>
* <span data-ttu-id="b4334-131">Karty síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="b4334-131">A network interface card</span></span> 
* <span data-ttu-id="b4334-132">Účet úložiště se kontejner úložiště</span><span class="sxs-lookup"><span data-stu-id="b4334-132">A storage account with a storage container</span></span>
* <span data-ttu-id="b4334-133">Veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="b4334-133">A public IP</span></span>
* <span data-ttu-id="b4334-134">Virtuální počítač, který využívá všechny předchozí prostředky</span><span class="sxs-lookup"><span data-stu-id="b4334-134">A virtual machine that utilizes all the previous resources</span></span> 

<span data-ttu-id="b4334-135">Důkladné dokumentaci pro jednotlivé prostředky Azure Terraform najdete v tématu [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="b4334-135">For thorough documentation for each of the Azure Terraform resources, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="b4334-136">Plnou verzi [skriptu](#complete-terraform-script) jsou tu taky ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="b4334-136">The full version of the [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-the-terraform-script"></a><span data-ttu-id="b4334-137">Rozšíření Terraform skriptu</span><span class="sxs-lookup"><span data-stu-id="b4334-137">Extend the Terraform script</span></span>
<span data-ttu-id="b4334-138">Rozšíření skript, který byl vytvořen v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="b4334-138">Extend the script that was created with the following resources:</span></span> 
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
<span data-ttu-id="b4334-139">Předchozí skript vytvoří virtuální síť a podsíť v rámci této virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b4334-139">The previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="b4334-140">Poznámka: odkaz na skupinu prostředků, které jste již vytvořili prostřednictvím "${azurerm_resource_group.helloterraform.name}" virtuální sítě a definice podsítě.</span><span class="sxs-lookup"><span data-stu-id="b4334-140">Note the reference to the resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both the virtual network and the subnet definition.</span></span>

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
<span data-ttu-id="b4334-141">Předchozí fragmenty skriptu vytvoření veřejné IP adresy a síťové rozhraní, které využívá veřejnou IP adresu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b4334-141">The previous script snippets create a public IP and a network interface that makes use of the public IP created.</span></span> <span data-ttu-id="b4334-142">Všimněte si, odkazy na subnet_id a public_ip_address_id.</span><span class="sxs-lookup"><span data-stu-id="b4334-142">Note the references to subnet_id and public_ip_address_id.</span></span> <span data-ttu-id="b4334-143">Terraform má vestavěné inteligentní si uvědomit, že síťové rozhraní má závislost na prostředcích, které je třeba vytvořit před vytvořením síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b4334-143">Terraform has built-in intelligence to understand that the network interface has a dependency on the resources that need to be created before the creation of the network interface.</span></span>

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
<span data-ttu-id="b4334-144">Zde vytvoříte účet úložiště a definovaný kontejner úložiště v rámci tohoto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b4334-144">Here, you created a storage account and defined a storage container within that storage account.</span></span> <span data-ttu-id="b4334-145">Tento účet úložiště se, kam můžete ukládat virtuální pevné disky (VHD) pro virtuální počítač, o který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b4334-145">This storage account is where you store virtual hard disks (VHDs) for the virtual machine about to be created.</span></span>

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
<span data-ttu-id="b4334-146">Nakonec fragmentu předchozí vytvoří virtuální počítač, který využívá všechny prostředky, které jsou už zřízené.</span><span class="sxs-lookup"><span data-stu-id="b4334-146">Finally, the previous snippet creates a virtual machine that utilizes all the resources provisioned already.</span></span> <span data-ttu-id="b4334-147">Jsou účtu úložiště a kontejner pro virtuální pevný disk, je síťové rozhraní s veřejné IP adresy a podsítě zadán, a skupině prostředků jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b4334-147">They are a storage account and container for a VHD, a network interface with public IP and subnet specified, and the resource group you already created.</span></span> <span data-ttu-id="b4334-148">Poznámka: vlastnost vm_size, kde skript určuje A0 SKU služby Azure.</span><span class="sxs-lookup"><span data-stu-id="b4334-148">Note the vm_size property, where the script specifies an Azure A0 SKU.</span></span>

### <a name="execute-the-script"></a><span data-ttu-id="b4334-149">Spustit skript</span><span class="sxs-lookup"><span data-stu-id="b4334-149">Execute the script</span></span>
<span data-ttu-id="b4334-150">Pomocí úplné skriptu uložit ukončete na konzole nebo příkazový řádek a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b4334-150">With the full script saved, exit to the console/command line and type the following:</span></span>
```
terraform apply terraformscripts
```
<span data-ttu-id="b4334-151">Po určité době zdroje, včetně virtuální počítač, se zobrazí v `terraformtest` skupinu prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b4334-151">After some time, the resources, including a virtual machine, appear in the `terraformtest` resource group in the Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="b4334-152">Dokončení Terraform skriptu</span><span class="sxs-lookup"><span data-stu-id="b4334-152">Complete Terraform script</span></span>

<span data-ttu-id="b4334-153">Pro usnadnění vaší práce je nižší než dokončení skriptu Terraform zřizuje, že všechny infrastruktury popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="b4334-153">For your convenience, below is the complete Terraform script that provisions all of the infrastructure discussed in this article.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b4334-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b4334-154">Next steps</span></span>
<span data-ttu-id="b4334-155">Základní infrastruktura jste vytvořili v Azure pomocí Terraform.</span><span class="sxs-lookup"><span data-stu-id="b4334-155">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="b4334-156">Složitější scénáře, včetně příklady, které pomocí služby Vyrovnávání zatížení a virtuální počítač škálování sad, najdete v tématu množství [Terraform příklady Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="b4334-156">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="b4334-157">Aktuální seznam podporovaných zprostředkovatelů Azure, najdete v článku [Terraform dokumentaci](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="b4334-157">For an up-to-date list of supported Azure providers, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
