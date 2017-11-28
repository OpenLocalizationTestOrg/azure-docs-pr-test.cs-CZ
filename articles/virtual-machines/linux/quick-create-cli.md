---
title: "Rychlý Start - aaaAzure vytvořit virtuální počítač CLI | Microsoft Docs"
description: "Naučte se rychle toocreate virtuálních počítačů s hello rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0d9c686e2f4c7339b29b8c43cd1dd9ee56d7dc6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="781bb-103">Vytvořit virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="781bb-103">Create a Linux virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="781bb-104">Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="781bb-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="781bb-105">Tento průvodce údaje, pomocí rozhraní příkazového řádku Azure toodeploy hello virtuálního počítače s Ubuntu server.</span><span class="sxs-lookup"><span data-stu-id="781bb-105">This guide details using hello Azure CLI toodeploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="781bb-106">Po nasazení hello serveru se vytvoří připojení SSH a je nainstalován webovém serveru NGINX.</span><span class="sxs-lookup"><span data-stu-id="781bb-106">Once hello server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="781bb-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="781bb-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="781bb-108">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="781bb-108">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="781bb-109">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="781bb-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="781bb-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="781bb-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="781bb-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="781bb-111">Create a resource group</span></span>

<span data-ttu-id="781bb-112">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="781bb-112">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="781bb-113">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="781bb-113">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="781bb-114">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="781bb-114">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="781bb-115">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="781bb-115">Create virtual machine</span></span>

<span data-ttu-id="781bb-116">Vytvoření virtuálního počítače s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="781bb-116">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="781bb-117">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* a vytvoří klíče SSH, pokud už neexistují ve výchozím umístění klíče.</span><span class="sxs-lookup"><span data-stu-id="781bb-117">hello following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="781bb-118">toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.</span><span class="sxs-lookup"><span data-stu-id="781bb-118">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="781bb-119">Po vytvoření hello virtuálních počítačů, hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace.</span><span class="sxs-lookup"><span data-stu-id="781bb-119">When hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="781bb-120">Poznamenejte si hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="781bb-120">Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="781bb-121">Tato adresa je použité tooaccess hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="781bb-121">This address is used tooaccess hello VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="781bb-122">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="781bb-122">Open port 80 for web traffic</span></span> 

<span data-ttu-id="781bb-123">Ve výchozím nastavení jsou na virtuální počítače s Linuxem, které jsou nasazené v Azure, povolená pouze připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="781bb-123">By default only SSH connections are allowed into Linux virtual machines deployed in Azure.</span></span> <span data-ttu-id="781bb-124">Pokud tento virtuální počítač bude toobe webovém serveru, je třeba tooopen port 80 z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="781bb-124">If this VM is going toobe a webserver, you need tooopen port 80 from hello Internet.</span></span> <span data-ttu-id="781bb-125">Použití hello [az virtuálních počítačů open-port](/cli/azure/vm#open-port) příkaz tooopen hello požadovaného portu.</span><span class="sxs-lookup"><span data-stu-id="781bb-125">Use hello [az vm open-port](/cli/azure/vm#open-port) command tooopen hello desired port.</span></span>  
 
 ```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a><span data-ttu-id="781bb-126">Připojení SSH k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="781bb-126">SSH into your VM</span></span>

<span data-ttu-id="781bb-127">Použití hello následující příkaz toocreate na relace SSH s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="781bb-127">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="781bb-128">Ujistěte se, že tooreplace  *<publicIpAddress>*  s hello opravte veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="781bb-128">Make sure tooreplace *<publicIpAddress>* with hello correct public IP address of your virtual machine.</span></span>  <span data-ttu-id="781bb-129">V našem příkladu byla IP adresa *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="781bb-129">In our example above our IP address was *40.68.254.142*.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a><span data-ttu-id="781bb-130">Instalace serveru NGINX</span><span class="sxs-lookup"><span data-stu-id="781bb-130">Install NGINX</span></span>

<span data-ttu-id="781bb-131">Použití hello následující zdroje balíčků tooupdate skriptů bash a instalovat nejnovější balíček NGINX hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-131">Use hello following bash script tooupdate package sources and install hello latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-nginx-welcome-page"></a><span data-ttu-id="781bb-132">Zobrazení hello NGINX úvodní stránka</span><span class="sxs-lookup"><span data-stu-id="781bb-132">View hello NGINX welcome page</span></span>

<span data-ttu-id="781bb-133">NGINX nainstalován a port 80 nyní otevřete na vašem virtuálním počítači z hello Internetu – můžete použít webový prohlížeč volba tooview hello výchozí NGINX uvítací stránky.</span><span class="sxs-lookup"><span data-stu-id="781bb-133">With NGINX installed and port 80 now open on your VM from hello Internet - you can use a web browser of your choice tooview hello default NGINX welcome page.</span></span> <span data-ttu-id="781bb-134">Se, zda text hello toouse *publicIpAddress* popsané výše toovisit hello výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="781bb-134">Be sure toouse hello *publicIpAddress* you documented above toovisit hello default page.</span></span> 

![Výchozí web NGINX](./media/quick-create-cli/nginx.png) 


## <a name="clean-up-resources"></a><span data-ttu-id="781bb-136">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="781bb-136">Clean up resources</span></span>

<span data-ttu-id="781bb-137">Pokud již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, virtuálních počítačů a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="781bb-137">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="781bb-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="781bb-138">Next steps</span></span>

<span data-ttu-id="781bb-139">V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server.</span><span class="sxs-lookup"><span data-stu-id="781bb-139">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="781bb-140">toolearn Další informace o virtuálních počítačích Azure, pokračovat v kurzu toohello pro virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="781bb-140">toolearn more about Azure virtual machines, continue toohello tutorial for Linux VMs.</span></span>


> [!div class="nextstepaction"]
> [<span data-ttu-id="781bb-141">Kurzy pro virtuální počítače Azure s Linuxem</span><span class="sxs-lookup"><span data-stu-id="781bb-141">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
