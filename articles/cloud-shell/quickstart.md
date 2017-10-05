---
title: "Azure rychlý start prostředí cloudu (Preview) | Microsoft Docs"
description: "Rychlý úvodní kurz pro Azure cloudové prostředí"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: 75676eb0ab784e2adbfd27b170c1dee5599b74ac
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="quickstart-for-using-the-azure-cloud-shell"></a><span data-ttu-id="d9d8f-103">Rychlý úvodní kurz pro používání prostředí cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="d9d8f-103">Quickstart for using the Azure Cloud Shell</span></span>

<span data-ttu-id="d9d8f-104">Tento dokument podrobně popisuje postup používání prostředí cloudové služby Azure v [portál Azure](https://ms.portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d9d8f-104">This document details how to use the Azure Cloud Shell in the [Azure portal](https://ms.portal.azure.com/).</span></span>

## <a name="start-cloud-shell"></a><span data-ttu-id="d9d8f-105">Spusťte prostředí cloudu</span><span class="sxs-lookup"><span data-stu-id="d9d8f-105">Start Cloud Shell</span></span>
1. <span data-ttu-id="d9d8f-106">Spusťte **cloudové prostředí** z horním navigačním panelu portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d9d8f-106">Launch **Cloud Shell** from the top navigation of the Azure portal</span></span> <br>
![](media/shell-icon.png)
2. <span data-ttu-id="d9d8f-107">Vyberte předplatné, chcete-li vytvořit účet úložiště a sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="d9d8f-107">Select a subscription to create a storage account and Azure file share</span></span>
3. <span data-ttu-id="d9d8f-108">Vyberte "Vytvoření úložiště"</span><span class="sxs-lookup"><span data-stu-id="d9d8f-108">Select "Create storage"</span></span>

> [!TIP]
> <span data-ttu-id="d9d8f-109">Pro Azure CLI 2.0 se automaticky ověřeni v každé relaci..</span><span class="sxs-lookup"><span data-stu-id="d9d8f-109">You are automatically authenticated for Azure CLI 2.0 in every sesssion.</span></span>

### <a name="set-your-subscription"></a><span data-ttu-id="d9d8f-110">Nastavení předplatného</span><span class="sxs-lookup"><span data-stu-id="d9d8f-110">Set your subscription</span></span>
1. <span data-ttu-id="d9d8f-111">Seznam odběrů, ke kterým máte přístup k:</span><span class="sxs-lookup"><span data-stu-id="d9d8f-111">List subscriptions you have access to:</span></span> <br>
`az account list`
2. <span data-ttu-id="d9d8f-112">Nastavte upřednostňované předplatného:</span><span class="sxs-lookup"><span data-stu-id="d9d8f-112">Set your preferred subscription:</span></span> <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> <span data-ttu-id="d9d8f-113">Vaše předplatné se uloží pro budoucí relace pomocí `/home/<user>/.azure/azureProfile.json`.</span><span class="sxs-lookup"><span data-stu-id="d9d8f-113">Your subscription will be remembered for future sessions using `/home/<user>/.azure/azureProfile.json`.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="d9d8f-114">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="d9d8f-114">Create a resource group</span></span>
<span data-ttu-id="d9d8f-115">Vytvořte novou skupinu prostředků v WestUS s názvem "MyRG":</span><span class="sxs-lookup"><span data-stu-id="d9d8f-115">Create a new resource group in WestUS named "MyRG":</span></span> <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a><span data-ttu-id="d9d8f-116">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="d9d8f-116">Create a Linux VM</span></span>
<span data-ttu-id="d9d8f-117">Vytvoření virtuálního počítače s Ubuntu v nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="d9d8f-117">Create an Ubuntu VM in your new resource group.</span></span> <span data-ttu-id="d9d8f-118">Azure CLI 2.0 bude vytvoření klíčů SSH a instalace virtuálních počítačů s nimi.</span><span class="sxs-lookup"><span data-stu-id="d9d8f-118">The Azure CLI 2.0 will create SSH keys and setup the VM with them.</span></span> <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> <span data-ttu-id="d9d8f-119">Veřejné a soukromé klíče používá k ověření svého virtuálního počítače jsou umístěny v `/User/.ssh/id_rsa` a `/User/.ssh/id_rsa.pub` pomocí Azure CLI 2.0 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d9d8f-119">The public and private keys used to authenticate your VM are placed in `/User/.ssh/id_rsa` and `/User/.ssh/id_rsa.pub` by Azure CLI 2.0 by default.</span></span> <span data-ttu-id="d9d8f-120">Vaše složky .ssh je trvalé připojené Azure sdílené složky na obrázku 5 GB.</span><span class="sxs-lookup"><span data-stu-id="d9d8f-120">Your .ssh folder is persisted in your attached Azure file share's 5-GB image.</span></span>

<span data-ttu-id="d9d8f-121">Vaše uživatelské jméno pro tento virtuální počítač bude vaše uživatelské jméno použité v prostředí cloudu ($User@Azure:).</span><span class="sxs-lookup"><span data-stu-id="d9d8f-121">Your username on this VM will be your username used in Cloud Shell ($User@Azure:).</span></span>

### <a name="ssh-into-your-linux-vm"></a><span data-ttu-id="d9d8f-122">SSH do virtuálním počítačům s Linuxem</span><span class="sxs-lookup"><span data-stu-id="d9d8f-122">SSH into your Linux VM</span></span>
1. <span data-ttu-id="d9d8f-123">Vyhledejte název virtuálního počítače v panelu vyhledávání v portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d9d8f-123">Search for your VM name in the Azure portal search bar</span></span>
2. <span data-ttu-id="d9d8f-124">Klikněte na tlačítko "Připojit" a spusťte:`ssh username@ipaddress`</span><span class="sxs-lookup"><span data-stu-id="d9d8f-124">Click "Connect" and run: `ssh username@ipaddress`</span></span>

![](media/sshcmd-copy.png)

<span data-ttu-id="d9d8f-125">Při navazování připojení SSH, měli byste vidět úvodní řádku Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="d9d8f-125">Upon establishing the SSH connection, you should see the Ubuntu welcome prompt.</span></span>
![](media/ubuntu-welcome.png)

## <a name="cleaning-up"></a><span data-ttu-id="d9d8f-126">Čištění</span><span class="sxs-lookup"><span data-stu-id="d9d8f-126">Cleaning up</span></span> 
<span data-ttu-id="d9d8f-127">Odstranění skupiny prostředků a všechny prostředky v něm:</span><span class="sxs-lookup"><span data-stu-id="d9d8f-127">Delete your resource group and any resources within it:</span></span> <br>
<span data-ttu-id="d9d8f-128">Spusťte `az group delete -n MyRG`.</span><span class="sxs-lookup"><span data-stu-id="d9d8f-128">Run `az group delete -n MyRG`</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9d8f-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9d8f-129">Next Steps</span></span>
[<span data-ttu-id="d9d8f-130">Další informace o zachování úložiště v prostředí cloudu</span><span class="sxs-lookup"><span data-stu-id="d9d8f-130">Learn about persisting storage on Cloud Shell</span></span>](persisting-shell-storage.md) <br><span data-ttu-id="d9d8f-131">
[Další informace o rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/)</span><span class="sxs-lookup"><span data-stu-id="d9d8f-131">
[Learn about Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span></span> <br><span data-ttu-id="d9d8f-132">
[Další informace o Azure File storage](../storage/files/storage-files-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="d9d8f-132">
[Learn about Azure File storage](../storage/files/storage-files-introduction.md)</span></span> <br>