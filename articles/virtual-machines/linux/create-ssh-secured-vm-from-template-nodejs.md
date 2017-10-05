---
title: "Vytvoření virtuálního počítače s Linuxem pomocí šablony Azure s Azure CLI 1.0 | Microsoft Docs"
description: "Vytvoření virtuálního počítače s Linuxem v Azure pomocí Azure CLI 1.0 a šablonu Azure Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-livech
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 33d4aaa78fcdf3bd9e2e236606f2d3049f464a8a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-linux-vm-using-the-azure-cli-10-an-azure-resource-manager-template"></a><span data-ttu-id="23ca1-103">Postup vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 1.0 šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="23ca1-103">How to create a Linux VM using the Azure CLI 1.0 an Azure Resource Manager template</span></span>
<span data-ttu-id="23ca1-104">Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem pomocí Azure CLI 1.0 a šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="23ca1-104">This article shows you how to quickly deploy a Linux Virtual Machine using the Azure CLI 1.0 and an Azure Resource Manager template.</span></span> <span data-ttu-id="23ca1-105">Tento článek vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="23ca1-105">The article requires:</span></span>

* <span data-ttu-id="23ca1-106">účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))</span><span class="sxs-lookup"><span data-stu-id="23ca1-106">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="23ca1-107">[Azure CLI 1.0](../../cli-install-nodejs.md) přihlášení `azure login`.</span><span class="sxs-lookup"><span data-stu-id="23ca1-107">the [Azure CLI 1.0](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="23ca1-108">Rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager`azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="23ca1-108">the Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

<span data-ttu-id="23ca1-109">Šablonu virtuálního počítače s Linuxem můžete rychle nasadit také pomocí webu [Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="23ca1-109">You can also quickly deploy a Linux VM template by using the [Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="23ca1-110">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="23ca1-110">CLI versions to complete the task</span></span>
<span data-ttu-id="23ca1-111">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="23ca1-111">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="23ca1-112">[Azure CLI 1.0](#quick-command-summary) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="23ca1-112">[Azure CLI 1.0](#quick-command-summary) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="23ca1-113">[Azure CLI 2.0](create-ssh-secured-vm-from-template.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="23ca1-113">[Azure CLI 2.0](create-ssh-secured-vm-from-template.md) - our next generation CLI for the resource management deployment model</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="23ca1-114">Rychlý přehled příkazu</span><span class="sxs-lookup"><span data-stu-id="23ca1-114">Quick Command Summary</span></span>
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="23ca1-115">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="23ca1-115">Detailed Walkthrough</span></span>
<span data-ttu-id="23ca1-116">Šablony umožňují vytvořit virtuální počítače na platformě Azure s nastavením, které si pak při spouštění upravíte, například uživatelská jména a názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="23ca1-116">Templates allow you to create VMs on Azure with settings that you want to customize during the launch, settings like usernames and hostnames.</span></span> <span data-ttu-id="23ca1-117">Pro tento článek spouštíme šablonu Azure využívající virtuální počítač Ubuntu společně se skupinu zabezpečení sítě (NSG) s portem 22 otevřeným pro SSH.</span><span class="sxs-lookup"><span data-stu-id="23ca1-117">For this article, we are launching an Azure template utilizing an Ubuntu VM along with a network security group (NSG) with port 22 open for SSH.</span></span>

<span data-ttu-id="23ca1-118">Šablony Azure Resource Manageru jsou soubory JSON, které jde použít k jednoduchým jednorázovým úlohám, jako je spuštění virtuálního počítače Ubuntu VM pro účely tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="23ca1-118">Azure Resource Manager templates are JSON files that can be used for simple one-off tasks like launching an Ubuntu VM as done in this article.</span></span>  <span data-ttu-id="23ca1-119">Šablony Azure jde také použít k vytvoření složitých konfigurací Azure pro celé prostředí, jako je sada pro nasazení testovacího, vývojového nebo produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="23ca1-119">Azure Templates can also be used to construct complex Azure configurations of entire environments like a testing, dev, or production deployment stack.</span></span>

## <a name="create-the-linux-vm"></a><span data-ttu-id="23ca1-120">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="23ca1-120">Create the Linux VM</span></span>
<span data-ttu-id="23ca1-121">Následující příklad kódu ukazuje, jak voláním příkazu `azure group create` vytvořit skupinu prostředků a zároveň nasadit virtuální počítač s Linuxem se zabezpečením SSH pomocí [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="23ca1-121">The following code example shows how to call `azure group create` to create a resource group and deploy an SSH-secured Linux VM at the same time using [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json).</span></span> <span data-ttu-id="23ca1-122">Je třeba myslet na to, že v příkladu musíte použít názvy, které jsou jedinečné pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="23ca1-122">Remember that in your example you need to use names that are unique to your environment.</span></span> <span data-ttu-id="23ca1-123">Tento příklad používá *myResourceGroup* jako název skupiny prostředků a *Můjvp* jako název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23ca1-123">This example uses *myResourceGroup* as the resource group name, and *myVM* as the VM name.</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

<span data-ttu-id="23ca1-124">Výstup by měl vypadat jako následující výstupní blok:</span><span class="sxs-lookup"><span data-stu-id="23ca1-124">The output should look like the following output block:</span></span>

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

<span data-ttu-id="23ca1-125">Tento příklad nasazuje virtuální počítač pomocí parametru `--template-uri`.</span><span class="sxs-lookup"><span data-stu-id="23ca1-125">That example deployed a VM using the `--template-uri` parameter.</span></span>  <span data-ttu-id="23ca1-126">Můžete taky šablonu stáhnout nebo ji vytvořit místně a předat ji pomocí parametru `--template-file`cesta k souboru šablony.</span><span class="sxs-lookup"><span data-stu-id="23ca1-126">You can also download or create a template locally and pass the template using the `--template-file` parameter with a path to the template file as an argument.</span></span> <span data-ttu-id="23ca1-127">Rozhraní Azure CLI vás požádá o parametry vyžadované šablonou.</span><span class="sxs-lookup"><span data-stu-id="23ca1-127">The Azure CLI prompts you for the parameters required by the template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23ca1-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23ca1-128">Next steps</span></span>
<span data-ttu-id="23ca1-129">V [galerii šablon](https://azure.microsoft.com/documentation/templates/) se podívejte, jaké aplikační architektury nasadit jako další.</span><span class="sxs-lookup"><span data-stu-id="23ca1-129">Search the [templates gallery](https://azure.microsoft.com/documentation/templates/) to discover what app frameworks to deploy next.</span></span>

