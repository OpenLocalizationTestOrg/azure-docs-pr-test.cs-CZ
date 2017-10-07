---
title: "virtuální počítač s Linuxem pomocí šablony Azure s Azure CLI 1.0 aaaCreate | Microsoft Docs"
description: "Vytvoření virtuálního počítače s Linuxem v Azure pomocí Azure CLI 1.0 hello a šablonu Azure Resource Manager."
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
ms.openlocfilehash: b694cc8247a8431b7ef4b24cc7dc2b4cdb9660ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-vm-using-hello-azure-cli-10-an-azure-resource-manager-template"></a><span data-ttu-id="cf04d-103">Jak hello toocreate a virtuální počítač s Linuxem pomocí Azure CLI 1.0 šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cf04d-103">How toocreate a Linux VM using hello Azure CLI 1.0 an Azure Resource Manager template</span></span>
<span data-ttu-id="cf04d-104">Tento článek ukazuje, jak tooquickly nasadit virtuální počítač s Linuxem pomocí hello Azure CLI 1.0 a šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cf04d-104">This article shows you how tooquickly deploy a Linux Virtual Machine using hello Azure CLI 1.0 and an Azure Resource Manager template.</span></span> <span data-ttu-id="cf04d-105">článek Hello vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="cf04d-105">hello article requires:</span></span>

* <span data-ttu-id="cf04d-106">účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))</span><span class="sxs-lookup"><span data-stu-id="cf04d-106">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="cf04d-107">Hello [Azure CLI 1.0](../../cli-install-nodejs.md) přihlášení `azure login`.</span><span class="sxs-lookup"><span data-stu-id="cf04d-107">hello [Azure CLI 1.0](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="cf04d-108">Hello rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="cf04d-108">hello Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

<span data-ttu-id="cf04d-109">Šablonu virtuálního počítače s Linuxem můžete také rychle nasadit pomocí hello [portál Azure](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cf04d-109">You can also quickly deploy a Linux VM template by using hello [Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="cf04d-110">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="cf04d-110">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="cf04d-111">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="cf04d-111">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="cf04d-112">[Azure CLI 1.0](#quick-command-summary) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="cf04d-112">[Azure CLI 1.0](#quick-command-summary) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="cf04d-113">[Azure CLI 2.0](create-ssh-secured-vm-from-template.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="cf04d-113">[Azure CLI 2.0](create-ssh-secured-vm-from-template.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="cf04d-114">Rychlý přehled příkazu</span><span class="sxs-lookup"><span data-stu-id="cf04d-114">Quick Command Summary</span></span>
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="cf04d-115">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="cf04d-115">Detailed Walkthrough</span></span>
<span data-ttu-id="cf04d-116">Šablony umožňují toocreate virtuální počítače na platformě Azure s nastavením, který má toocustomize během spuštění hello, nastavení, jako je uživatelská jména a názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="cf04d-116">Templates allow you toocreate VMs on Azure with settings that you want toocustomize during hello launch, settings like usernames and hostnames.</span></span> <span data-ttu-id="cf04d-117">Pro tento článek spouštíme šablonu Azure využívající virtuální počítač Ubuntu společně se skupinu zabezpečení sítě (NSG) s portem 22 otevřeným pro SSH.</span><span class="sxs-lookup"><span data-stu-id="cf04d-117">For this article, we are launching an Azure template utilizing an Ubuntu VM along with a network security group (NSG) with port 22 open for SSH.</span></span>

<span data-ttu-id="cf04d-118">Šablony Azure Resource Manageru jsou soubory JSON, které jde použít k jednoduchým jednorázovým úlohám, jako je spuštění virtuálního počítače Ubuntu VM pro účely tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="cf04d-118">Azure Resource Manager templates are JSON files that can be used for simple one-off tasks like launching an Ubuntu VM as done in this article.</span></span>  <span data-ttu-id="cf04d-119">Šablony Azure může být také použít tooconstruct komplexní konfigurace Azure celých prostředí jako zásobníky nasazení testování, vývojového nebo produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf04d-119">Azure Templates can also be used tooconstruct complex Azure configurations of entire environments like a testing, dev, or production deployment stack.</span></span>

## <a name="create-hello-linux-vm"></a><span data-ttu-id="cf04d-120">Vytvoření virtuálního počítače s Linuxem hello</span><span class="sxs-lookup"><span data-stu-id="cf04d-120">Create hello Linux VM</span></span>
<span data-ttu-id="cf04d-121">Následující příklad ukazuje kód jak Hello toocall `azure group create` toocreate prostředek skupiny a nasadit virtuální počítač Linux zabezpečením SSH hello současně pomocí [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="cf04d-121">hello following code example shows how toocall `azure group create` toocreate a resource group and deploy an SSH-secured Linux VM at hello same time using [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json).</span></span> <span data-ttu-id="cf04d-122">Nezapomeňte, že v příkladu potřebujete toouse názvy, které jsou jedinečné tooyour prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf04d-122">Remember that in your example you need toouse names that are unique tooyour environment.</span></span> <span data-ttu-id="cf04d-123">Tento příklad používá *myResourceGroup* jako hello název skupiny prostředků, a *Můjvp* jako hello název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cf04d-123">This example uses *myResourceGroup* as hello resource group name, and *myVM* as hello VM name.</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

<span data-ttu-id="cf04d-124">výstup Hello by měl vypadat podobně jako následující bloku výstup hello:</span><span class="sxs-lookup"><span data-stu-id="cf04d-124">hello output should look like hello following output block:</span></span>

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for hello following parameters
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

<span data-ttu-id="cf04d-125">Například nasazení virtuálního počítače pomocí hello `--template-uri` parametr.</span><span class="sxs-lookup"><span data-stu-id="cf04d-125">That example deployed a VM using hello `--template-uri` parameter.</span></span>  <span data-ttu-id="cf04d-126">Můžete také stáhnout nebo vytvořit šablonu místně a předat hello šablony pomocí hello `--template-file` parametr s cesta k souboru šablony toohello jako argument.</span><span class="sxs-lookup"><span data-stu-id="cf04d-126">You can also download or create a template locally and pass hello template using hello `--template-file` parameter with a path toohello template file as an argument.</span></span> <span data-ttu-id="cf04d-127">Hello příkazového řádku Azure CLI vás vyzve k zadání parametrů hello požadovaných šablonou hello.</span><span class="sxs-lookup"><span data-stu-id="cf04d-127">hello Azure CLI prompts you for hello parameters required by hello template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf04d-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cf04d-128">Next steps</span></span>
<span data-ttu-id="cf04d-129">Hledání hello [galerii šablon](https://azure.microsoft.com/documentation/templates/) toodiscover jaké aplikace rozhraní toodeploy Další.</span><span class="sxs-lookup"><span data-stu-id="cf04d-129">Search hello [templates gallery](https://azure.microsoft.com/documentation/templates/) toodiscover what app frameworks toodeploy next.</span></span>

