---
title: "Rychlý start Azure – Vytvoření virtuálního počítače pomocí rozhraní příkazového řádku | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak vytvářet virtuální počítače pomocí Azure CLI."
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
ms.openlocfilehash: a7cba5b2c43704d92e36d6f808efaa9fc73fdf36
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-linux-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="ea65c-103">Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ea65c-103">Create a Linux virtual machine with the Azure CLI</span></span>

<span data-ttu-id="ea65c-104">Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="ea65c-104">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="ea65c-105">Tento průvodce podrobně popisuje nasazení virtuálního počítače se serverem Ubuntu pomocí Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ea65c-105">This guide details using the Azure CLI to deploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="ea65c-106">Po nasazení serveru se vytvoří připojení SSH a nainstaluje se webový server NGINX.</span><span class="sxs-lookup"><span data-stu-id="ea65c-106">Once the server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="ea65c-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="ea65c-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ea65c-108">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ea65c-108">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="ea65c-109">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="ea65c-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="ea65c-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ea65c-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="ea65c-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="ea65c-111">Create a resource group</span></span>

<span data-ttu-id="ea65c-112">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ea65c-112">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="ea65c-113">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="ea65c-113">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="ea65c-114">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.</span><span class="sxs-lookup"><span data-stu-id="ea65c-114">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="ea65c-115">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ea65c-115">Create virtual machine</span></span>

<span data-ttu-id="ea65c-116">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="ea65c-116">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="ea65c-117">Následující příklad vytvoří virtuální počítač *myVM*, a pokud ve výchozím umístění klíčů ještě neexistují klíče SSH, vytvoří je.</span><span class="sxs-lookup"><span data-stu-id="ea65c-117">The following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="ea65c-118">Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="ea65c-118">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="ea65c-119">Po vytvoření virtuálního počítače se v Azure CLI zobrazí podobné informace jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="ea65c-119">When the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="ea65c-120">Poznamenejte si `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="ea65c-120">Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="ea65c-121">Tato adresa se používá pro přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ea65c-121">This address is used to access the VM.</span></span>

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

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="ea65c-122">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="ea65c-122">Open port 80 for web traffic</span></span> 

<span data-ttu-id="ea65c-123">Ve výchozím nastavení jsou na virtuální počítače s Linuxem, které jsou nasazené v Azure, povolená pouze připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="ea65c-123">By default only SSH connections are allowed into Linux virtual machines deployed in Azure.</span></span> <span data-ttu-id="ea65c-124">Pokud bude tento virtuální počítač webovým serverem, budete muset otevřít port 80 z internetu.</span><span class="sxs-lookup"><span data-stu-id="ea65c-124">If this VM is going to be a webserver, you need to open port 80 from the Internet.</span></span> <span data-ttu-id="ea65c-125">Požadovaný port otevřete pomocí příkazu [az vm open-port](/cli/azure/vm#open-port).</span><span class="sxs-lookup"><span data-stu-id="ea65c-125">Use the [az vm open-port](/cli/azure/vm#open-port) command to open the desired port.</span></span>  
 
 ```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a><span data-ttu-id="ea65c-126">Připojení SSH k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="ea65c-126">SSH into your VM</span></span>

<span data-ttu-id="ea65c-127">Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH.</span><span class="sxs-lookup"><span data-stu-id="ea65c-127">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="ea65c-128">Nezapomeňte nahradit *<publicIpAddress>* správnou veřejnou IP adresou vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ea65c-128">Make sure to replace *<publicIpAddress>* with the correct public IP address of your virtual machine.</span></span>  <span data-ttu-id="ea65c-129">V našem příkladu byla IP adresa *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="ea65c-129">In our example above our IP address was *40.68.254.142*.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a><span data-ttu-id="ea65c-130">Instalace serveru NGINX</span><span class="sxs-lookup"><span data-stu-id="ea65c-130">Install NGINX</span></span>

<span data-ttu-id="ea65c-131">Pomocí následujícího skriptu bash provedete aktualizaci zdrojů balíčku a nainstalujete nejnovější balíček NGINX.</span><span class="sxs-lookup"><span data-stu-id="ea65c-131">Use the following bash script to update package sources and install the latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a><span data-ttu-id="ea65c-132">Zobrazení úvodní stránky serveru NGINX</span><span class="sxs-lookup"><span data-stu-id="ea65c-132">View the NGINX welcome page</span></span>

<span data-ttu-id="ea65c-133">S nainstalovaným serverem NGINX na virtuálním počítači a nyní otevřeným portem 80 z internetu můžete použít libovolný webový prohlížeč a zobrazit výchozí úvodní stránku serveru NGINX.</span><span class="sxs-lookup"><span data-stu-id="ea65c-133">With NGINX installed and port 80 now open on your VM from the Internet - you can use a web browser of your choice to view the default NGINX welcome page.</span></span> <span data-ttu-id="ea65c-134">Nezapomeňte pro návštěvu výchozí stránky použít veřejnou IP adresu (*publicIpAddress*) popsanou výše.</span><span class="sxs-lookup"><span data-stu-id="ea65c-134">Be sure to use the *publicIpAddress* you documented above to visit the default page.</span></span> 

![Výchozí web NGINX](./media/quick-create-cli/nginx.png) 


## <a name="clean-up-resources"></a><span data-ttu-id="ea65c-136">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="ea65c-136">Clean up resources</span></span>

<span data-ttu-id="ea65c-137">Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, virtuálního počítače a všech souvisejících prostředků použít příkaz [az group delete](/cli/azure/group#delete).</span><span class="sxs-lookup"><span data-stu-id="ea65c-137">When no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="ea65c-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ea65c-138">Next steps</span></span>

<span data-ttu-id="ea65c-139">V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server.</span><span class="sxs-lookup"><span data-stu-id="ea65c-139">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="ea65c-140">Další informace o virtuálních počítačích Azure najdete v kurzu pro virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="ea65c-140">To learn more about Azure virtual machines, continue to the tutorial for Linux VMs.</span></span>


> [!div class="nextstepaction"]
> [<span data-ttu-id="ea65c-141">Kurzy pro virtuální počítače Azure s Linuxem</span><span class="sxs-lookup"><span data-stu-id="ea65c-141">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
