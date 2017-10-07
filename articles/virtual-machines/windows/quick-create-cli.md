---
title: "Rychlý Start - aaaAzure vytvořit CLI virtuálních počítačů Windows | Microsoft Docs"
description: "Naučte se rychle toocreate virtuální počítače s Windows s hello rozhraní příkazového řádku Azure."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 029bdecec219b12b80b958ceeedda214f1b13149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="1f552-103">Vytvoření virtuálního počítače s Windows pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1f552-103">Create a Windows virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="1f552-104">Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="1f552-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="1f552-105">Tento průvodce údaje, pomocí rozhraní příkazového řádku Azure toodeploy hello virtuálního počítače se systémem Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="1f552-105">This guide details using hello Azure CLI toodeploy a virtual machine running Windows Server 2016.</span></span> <span data-ttu-id="1f552-106">Po dokončení nasazení jsme připojení toohello serveru a instalace služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1f552-106">Once deployment is complete, we connect toohello server and install IIS.</span></span>

<span data-ttu-id="1f552-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="1f552-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1f552-108">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1f552-108">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="1f552-109">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="1f552-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1f552-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1f552-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="create-a-resource-group"></a><span data-ttu-id="1f552-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1f552-111">Create a resource group</span></span>

<span data-ttu-id="1f552-112">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="1f552-112">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="1f552-113">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="1f552-113">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="1f552-114">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="1f552-114">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="1f552-115">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1f552-115">Create virtual machine</span></span>

<span data-ttu-id="1f552-116">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="1f552-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> 

<span data-ttu-id="1f552-117">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="1f552-117">hello following example creates a VM named *myVM*.</span></span> <span data-ttu-id="1f552-118">Tento příklad používá *azureuser* pro název správce a *myPassword12* jako hello heslo.</span><span class="sxs-lookup"><span data-stu-id="1f552-118">This example uses *azureuser* for an administrative user name and *myPassword12* as hello password.</span></span> <span data-ttu-id="1f552-119">Aktualizujte tyto hodnoty toosomething odpovídající tooyour prostředí.</span><span class="sxs-lookup"><span data-stu-id="1f552-119">Update these values toosomething appropriate tooyour environment.</span></span> <span data-ttu-id="1f552-120">Tyto hodnoty jsou potřeba při vytváření připojení pomocí hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1f552-120">These values are needed when creating a connection with hello virtual machine.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

<span data-ttu-id="1f552-121">Po vytvoření hello virtuálních počítačů, hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace.</span><span class="sxs-lookup"><span data-stu-id="1f552-121">When hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="1f552-122">Poznamenejte si hello `publicIpAaddress`.</span><span class="sxs-lookup"><span data-stu-id="1f552-122">Take note of hello `publicIpAaddress`.</span></span> <span data-ttu-id="1f552-123">Tato adresa je použité tooaccess hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1f552-123">This address is used tooaccess hello VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="1f552-124">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="1f552-124">Open port 80 for web traffic</span></span> 

<span data-ttu-id="1f552-125">Ve výchozím nastavení jsou povoleny pouze připojení RDP v tooWindows virtuálních počítačů nasazených v Azure.</span><span class="sxs-lookup"><span data-stu-id="1f552-125">By default only RDP connections are allowed in tooWindows virtual machines deployed in Azure.</span></span> <span data-ttu-id="1f552-126">Pokud tento virtuální počítač bude toobe webovém serveru, je třeba tooopen port 80 z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="1f552-126">If this VM is going toobe a webserver, you need tooopen port 80 from hello Internet.</span></span> <span data-ttu-id="1f552-127">Použití hello [az virtuálních počítačů open-port](/cli/azure/vm#open-port) příkaz tooopen hello požadovaného portu.</span><span class="sxs-lookup"><span data-stu-id="1f552-127">Use hello [az vm open-port](/cli/azure/vm#open-port) command tooopen hello desired port.</span></span>  
 
 ```azurecli-interactive  
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```


## <a name="connect-toovirtual-machine"></a><span data-ttu-id="1f552-128">Připojte počítač toovirtual</span><span class="sxs-lookup"><span data-stu-id="1f552-128">Connect toovirtual machine</span></span>

<span data-ttu-id="1f552-129">Použití hello následující příkaz toocreate a relace vzdálené plochy s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1f552-129">Use hello following command toocreate a remote desktop session with hello virtual machine.</span></span> <span data-ttu-id="1f552-130">Nahraďte IP adresu hello hello veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1f552-130">Replace hello IP address with hello public IP address of your virtual machine.</span></span> <span data-ttu-id="1f552-131">Po zobrazení výzvy zadejte přihlašovací údaje hello použité při vytváření hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1f552-131">When prompted, enter hello credentials used when creating hello virtual machine.</span></span>

```bash 
mstsc /v:<Public IP Address>
```

## <a name="install-iis-using-powershell"></a><span data-ttu-id="1f552-132">Instalace služby IIS pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="1f552-132">Install IIS using PowerShell</span></span>

<span data-ttu-id="1f552-133">Teď, když jste byli přihlášeni toohello virtuálního počítače Azure, můžete použít jeden řádek tooinstall prostředí PowerShell služby IIS a povolit hello místní brány firewall pravidla tooallow webový provoz.</span><span class="sxs-lookup"><span data-stu-id="1f552-133">Now that you have logged in toohello Azure VM, you can use a single line of PowerShell tooinstall IIS and enable hello local firewall rule tooallow web traffic.</span></span> <span data-ttu-id="1f552-134">Otevřete příkazovém řádku prostředí PowerShell a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1f552-134">Open a PowerShell prompt and run hello following command:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="1f552-135">Zobrazení hello úvodní stránka služby IIS</span><span class="sxs-lookup"><span data-stu-id="1f552-135">View hello IIS welcome page</span></span>

<span data-ttu-id="1f552-136">S nainstalovanou službu IIS a port 80 nyní otevřete na vašem virtuálním počítači z hello Internetu můžete použít webový prohlížeč choice tooview hello výchozí IIS uvítací stránky.</span><span class="sxs-lookup"><span data-stu-id="1f552-136">With IIS installed and port 80 now open on your VM from hello Internet, you can use a web browser of your choice tooview hello default IIS welcome page.</span></span> <span data-ttu-id="1f552-137">Být, že toouse hello veřejnou IP adresu, kterou popsané výše toovisit hello výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="1f552-137">Be sure toouse hello public IP address you documented above toovisit hello default page.</span></span> 

![Výchozí web služby IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="1f552-139">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="1f552-139">Clean up resources</span></span>

<span data-ttu-id="1f552-140">Pokud již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, virtuálních počítačů a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="1f552-140">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="1f552-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f552-141">Next steps</span></span>

<span data-ttu-id="1f552-142">V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server.</span><span class="sxs-lookup"><span data-stu-id="1f552-142">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="1f552-143">toolearn Další informace o virtuálních počítačích Azure, pokračovat v kurzu toohello pro virtuální počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="1f552-143">toolearn more about Azure virtual machines, continue toohello tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1f552-144">Kurzy pro virtuální počítače Azure s Windows</span><span class="sxs-lookup"><span data-stu-id="1f552-144">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
