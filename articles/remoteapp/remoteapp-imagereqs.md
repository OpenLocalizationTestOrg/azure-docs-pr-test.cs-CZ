---
title: "Požadavky image Azure Remoteappu | Microsoft Docs"
description: "Další informace o požadavcích pro vytvoření bitové kopie, který se má použít s Azure Remoteappem"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7cbb90f4-6dc9-462c-a429-088cdb57414e
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 75b0f8d6b25a80f11002b683152cfb294cbb68bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="requirements-for-azure-remoteapp-images"></a><span data-ttu-id="b3513-103">Požadavky pro Azure RemoteApp bitové kopie</span><span class="sxs-lookup"><span data-stu-id="b3513-103">Requirements for Azure RemoteApp images</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b3513-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="b3513-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="b3513-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="b3513-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="b3513-106">Azure RemoteApp používá bitovou kopii systému Windows Server 2012 R2 k hostování všechny programy, které chcete sdílet s uživateli.</span><span class="sxs-lookup"><span data-stu-id="b3513-106">Azure RemoteApp uses a Windows Server 2012 R2 image to host all the programs that you want to share with your users.</span></span> <span data-ttu-id="b3513-107">Pokud chcete vytvořit vlastní image, můžete spustit pomocí stávající image nebo [vytvořte novou](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="b3513-107">To create a custom image, you can start with an existing image or [create a new one](remoteapp-create-custom-image.md).</span></span>

> [!TIP]
> <span data-ttu-id="b3513-108">Věděli jste, že vaše předplatné Azure Remoteappu umožňuje přístup k systému Windows Server 2012 R2 obrázek v galerii virtuálních počítačů Azure, který můžete použít k vytvoření vlastního image šablony?</span><span class="sxs-lookup"><span data-stu-id="b3513-108">Did you know that your Azure RemoteApp subscription gives you access to a Windows Server 2012 R2 image in the Azure VM gallery that you can use to create your own template image?</span></span> <span data-ttu-id="b3513-109">[Projděte si](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="b3513-109">[Check it out](remoteapp-image-on-azurevm.md).</span></span>  
> 
> 

<span data-ttu-id="b3513-110">Požadavky na bitovou kopii, která mohou být nahrány pro použití s Azure Remoteappem jsou:</span><span class="sxs-lookup"><span data-stu-id="b3513-110">The requirements for the image that can be uploaded for use with Azure RemoteApp are:</span></span>

* <span data-ttu-id="b3513-111">Vlastní aplikace neukládejte data místně na bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="b3513-111">Custom applications don’t store data locally on the image.</span></span> <span data-ttu-id="b3513-112">Tyto Image jsou bezstavové a smí obsahovat pouze aplikace.</span><span class="sxs-lookup"><span data-stu-id="b3513-112">These images are stateless and should only contain applications.</span></span>
* <span data-ttu-id="b3513-113">Obrázek neobsahuje data, která mohou být ztraceny.</span><span class="sxs-lookup"><span data-stu-id="b3513-113">The image does not contain data that can be lost.</span></span>
* <span data-ttu-id="b3513-114">Velikost obrázku musí být násobkem MB.</span><span class="sxs-lookup"><span data-stu-id="b3513-114">The image size should be a multiple of MBs.</span></span> <span data-ttu-id="b3513-115">Pokud se pokusíte odeslat obrázek, který není přesný více, nahrávání se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="b3513-115">If you try to upload an image that is not an exact multiple, the upload will fail.</span></span>
* <span data-ttu-id="b3513-116">Velikost obrázku musí být 127 GB nebo menší.</span><span class="sxs-lookup"><span data-stu-id="b3513-116">The image size must be 127 GB or smaller.</span></span>
* <span data-ttu-id="b3513-117">Musí být na soubor virtuálního pevného disku (VHDX soubory nejsou aktuálně podporovány.).</span><span class="sxs-lookup"><span data-stu-id="b3513-117">It must be on a VHD file (VHDX files are not currently supported).</span></span>
* <span data-ttu-id="b3513-118">Virtuální pevný disk nesmí být virtuální počítač generace 2.</span><span class="sxs-lookup"><span data-stu-id="b3513-118">The VHD must not be a generation 2 virtual machine.</span></span>
* <span data-ttu-id="b3513-119">Virtuální pevný disk může být buď pevné velikosti, nebo dynamicky se zvětšující.</span><span class="sxs-lookup"><span data-stu-id="b3513-119">The VHD can be either fixed-size or dynamically expanding.</span></span> <span data-ttu-id="b3513-120">Dynamicky se zvětšující virtuální pevný disk je doporučená, protože trvá méně času k nahrání do Azure, než soubor virtuálního pevného disku pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="b3513-120">A dynamically expanding VHD is recommended because it takes less time to upload to Azure than a fixed-size VHD file.</span></span>
* <span data-ttu-id="b3513-121">Disk musí být inicializován pomocí hlavní spouštěcí záznam (MBR) oddílů.</span><span class="sxs-lookup"><span data-stu-id="b3513-121">The disk must be initialized using the Master Boot Record (MBR) partitioning style.</span></span> <span data-ttu-id="b3513-122">Stylem oddílů GUID (GPT) není podporována.</span><span class="sxs-lookup"><span data-stu-id="b3513-122">The GUID partition table (GPT) partition style is not supported.</span></span>
* <span data-ttu-id="b3513-123">Virtuální pevný disk musí obsahovat jeden instalace systému Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="b3513-123">The VHD must contain a single installation of Windows Server 2012 R2.</span></span> <span data-ttu-id="b3513-124">Může obsahovat několik svazků, ale jenom jeden s obsahuje instalace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="b3513-124">It can contain multiple volumes, but only one that contains an installation of Windows.</span></span>
* <span data-ttu-id="b3513-125">Musí být nainstalována role Hostitel relace vzdálené plochy (RDSH) a funkci Možnosti práce s počítačem.</span><span class="sxs-lookup"><span data-stu-id="b3513-125">The Remote Desktop Session Host (RDSH) role and the Desktop Experience feature must be installed.</span></span>
* <span data-ttu-id="b3513-126">Role Zprostředkovatel připojení ke vzdálené ploše musí *není* nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="b3513-126">The Remote Desktop Connection Broker role must *not* be installed.</span></span>
* <span data-ttu-id="b3513-127">Musí se zakázat systému souborů EFS (ENCRYPTING File System).</span><span class="sxs-lookup"><span data-stu-id="b3513-127">The Encrypting File System (EFS) must be disabled.</span></span>
* <span data-ttu-id="b3513-128">Obrázek musí být SYSPREPed pomocí parametrů **/oobe / generalize/shutdown** (se nepoužije **/mode:vm** parametr).</span><span class="sxs-lookup"><span data-stu-id="b3513-128">The image must be SYSPREPed using the parameters **/oobe /generalize /shutdown** (DO NOT use the **/mode:vm** parameter).</span></span>
* <span data-ttu-id="b3513-129">Odesílání svůj disk VHD z řetěz snímku není podporována.</span><span class="sxs-lookup"><span data-stu-id="b3513-129">Uploading your VHD from a snapshot chain is not supported.</span></span>

<span data-ttu-id="b3513-130">V tématu [vytvoření image Azure Remoteappu](remoteapp-imageoptions.md) Další informace o vytváření bitových kopií pro Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b3513-130">See [Create an Azure RemoteApp image](remoteapp-imageoptions.md) for more information about creating images for Azure RemoteApp.</span></span>

