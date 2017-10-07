---
title: "Rychlý start aaaAzure prostředí cloudu (Preview) | Microsoft Docs"
description: "Rychlý start pro hello prostředí cloudu Azure"
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
ms.openlocfilehash: e60700b92c10c331910dd8bb3c627fe1a024091c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-for-using-hello-azure-cloud-shell"></a><span data-ttu-id="87982-103">Rychlý úvodní kurz pro používání hello prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="87982-103">Quickstart for using hello Azure Cloud Shell</span></span>

<span data-ttu-id="87982-104">Tento dokument podrobně popisuje, jak toouse hello prostředí cloudu Azure v hello [portál Azure](https://ms.portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="87982-104">This document details how toouse hello Azure Cloud Shell in hello [Azure portal](https://ms.portal.azure.com/).</span></span>

## <a name="start-cloud-shell"></a><span data-ttu-id="87982-105">Spusťte prostředí cloudu</span><span class="sxs-lookup"><span data-stu-id="87982-105">Start Cloud Shell</span></span>
1. <span data-ttu-id="87982-106">Spusťte **cloudové prostředí** z hello horním navigačním panelu hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="87982-106">Launch **Cloud Shell** from hello top navigation of hello Azure portal</span></span> <br>
![](media/shell-icon.png)
2. <span data-ttu-id="87982-107">Vyberte předplatné toocreate účet úložiště a sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="87982-107">Select a subscription toocreate a storage account and Azure file share</span></span>
3. <span data-ttu-id="87982-108">Vyberte "Vytvoření úložiště"</span><span class="sxs-lookup"><span data-stu-id="87982-108">Select "Create storage"</span></span>

> [!TIP]
> <span data-ttu-id="87982-109">Pro Azure CLI 2.0 se automaticky ověřeni v každé relaci..</span><span class="sxs-lookup"><span data-stu-id="87982-109">You are automatically authenticated for Azure CLI 2.0 in every sesssion.</span></span>

### <a name="set-your-subscription"></a><span data-ttu-id="87982-110">Nastavení předplatného</span><span class="sxs-lookup"><span data-stu-id="87982-110">Set your subscription</span></span>
1. <span data-ttu-id="87982-111">Seznam odběrů, ke kterým máte přístup k:</span><span class="sxs-lookup"><span data-stu-id="87982-111">List subscriptions you have access to:</span></span> <br>
`az account list`
2. <span data-ttu-id="87982-112">Nastavte upřednostňované předplatného:</span><span class="sxs-lookup"><span data-stu-id="87982-112">Set your preferred subscription:</span></span> <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> <span data-ttu-id="87982-113">Vaše předplatné se uloží pro budoucí relace pomocí `/home/<user>/.azure/azureProfile.json`.</span><span class="sxs-lookup"><span data-stu-id="87982-113">Your subscription will be remembered for future sessions using `/home/<user>/.azure/azureProfile.json`.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="87982-114">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="87982-114">Create a resource group</span></span>
<span data-ttu-id="87982-115">Vytvořte novou skupinu prostředků v WestUS s názvem "MyRG":</span><span class="sxs-lookup"><span data-stu-id="87982-115">Create a new resource group in WestUS named "MyRG":</span></span> <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a><span data-ttu-id="87982-116">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="87982-116">Create a Linux VM</span></span>
<span data-ttu-id="87982-117">Vytvoření virtuálního počítače s Ubuntu v nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="87982-117">Create an Ubuntu VM in your new resource group.</span></span> <span data-ttu-id="87982-118">Hello 2.0 rozhraní příkazového řádku Azure vytvoří klíčů SSH a instalační program hello virtuálních počítačů s nimi.</span><span class="sxs-lookup"><span data-stu-id="87982-118">hello Azure CLI 2.0 will create SSH keys and setup hello VM with them.</span></span> <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> <span data-ttu-id="87982-119">Hello veřejné a soukromé klíče používá tooauthenticate virtuálního počítače jsou umístěny v `/User/.ssh/id_rsa` a `/User/.ssh/id_rsa.pub` pomocí Azure CLI 2.0 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="87982-119">hello public and private keys used tooauthenticate your VM are placed in `/User/.ssh/id_rsa` and `/User/.ssh/id_rsa.pub` by Azure CLI 2.0 by default.</span></span> <span data-ttu-id="87982-120">Vaše složky .ssh je trvalé připojené Azure sdílené složky na obrázku 5 GB.</span><span class="sxs-lookup"><span data-stu-id="87982-120">Your .ssh folder is persisted in your attached Azure file share's 5-GB image.</span></span>

<span data-ttu-id="87982-121">Vaše uživatelské jméno pro tento virtuální počítač bude vaše uživatelské jméno použité v prostředí cloudu ($User@Azure:).</span><span class="sxs-lookup"><span data-stu-id="87982-121">Your username on this VM will be your username used in Cloud Shell ($User@Azure:).</span></span>

### <a name="ssh-into-your-linux-vm"></a><span data-ttu-id="87982-122">SSH do virtuálním počítačům s Linuxem</span><span class="sxs-lookup"><span data-stu-id="87982-122">SSH into your Linux VM</span></span>
1. <span data-ttu-id="87982-123">Vyhledejte název virtuálního počítače v panelu vyhledávání v portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="87982-123">Search for your VM name in hello Azure portal search bar</span></span>
2. <span data-ttu-id="87982-124">Klikněte na tlačítko "Připojit" a spusťte:`ssh username@ipaddress`</span><span class="sxs-lookup"><span data-stu-id="87982-124">Click "Connect" and run: `ssh username@ipaddress`</span></span>

![](media/sshcmd-copy.png)

<span data-ttu-id="87982-125">Při navazování připojení SSH hello, měli byste vidět hello Ubuntu úvodní řádku.</span><span class="sxs-lookup"><span data-stu-id="87982-125">Upon establishing hello SSH connection, you should see hello Ubuntu welcome prompt.</span></span>
![](media/ubuntu-welcome.png)

## <a name="cleaning-up"></a><span data-ttu-id="87982-126">Čištění</span><span class="sxs-lookup"><span data-stu-id="87982-126">Cleaning up</span></span> 
<span data-ttu-id="87982-127">Odstranění skupiny prostředků a všechny prostředky v něm:</span><span class="sxs-lookup"><span data-stu-id="87982-127">Delete your resource group and any resources within it:</span></span> <br>
<span data-ttu-id="87982-128">Spusťte `az group delete -n MyRG`.</span><span class="sxs-lookup"><span data-stu-id="87982-128">Run `az group delete -n MyRG`</span></span>

## <a name="next-steps"></a><span data-ttu-id="87982-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87982-129">Next Steps</span></span>
[<span data-ttu-id="87982-130">Další informace o zachování úložiště v prostředí cloudu</span><span class="sxs-lookup"><span data-stu-id="87982-130">Learn about persisting storage on Cloud Shell</span></span>](persisting-shell-storage.md) <br><span data-ttu-id="87982-131">
[Další informace o rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/)</span><span class="sxs-lookup"><span data-stu-id="87982-131">
[Learn about Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span></span> <br><span data-ttu-id="87982-132">
[Další informace o Azure File storage](../storage/files/storage-files-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="87982-132">
[Learn about Azure File storage](../storage/files/storage-files-introduction.md)</span></span> <br>