---
title: "požadavky na image vzdálené aplikace RemoteApp aaaAzure | Microsoft Docs"
description: "Další informace o hello požadavky pro vytvoření bitové kopie toobe použít s Azure Remoteappem"
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
ms.openlocfilehash: 4e35203eb93a866d4e0bd591d42b34746c7ffa4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="requirements-for-azure-remoteapp-images"></a><span data-ttu-id="4a9f0-103">Požadavky pro Azure RemoteApp bitové kopie</span><span class="sxs-lookup"><span data-stu-id="4a9f0-103">Requirements for Azure RemoteApp images</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4a9f0-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="4a9f0-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="4a9f0-106">Azure RemoteApp používá všechny hello programy, které chcete tooshare s uživateli toohost bitové kopie systému Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-106">Azure RemoteApp uses a Windows Server 2012 R2 image toohost all hello programs that you want tooshare with your users.</span></span> <span data-ttu-id="4a9f0-107">toocreate vlastní image, můžete spustit pomocí stávající image nebo [vytvořte novou](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-107">toocreate a custom image, you can start with an existing image or [create a new one](remoteapp-create-custom-image.md).</span></span>

> [!TIP]
> <span data-ttu-id="4a9f0-108">Věděli jste, že vaše Azure RemoteApp předplatné vám umožňuje přístup tooa bitové kopie systému Windows Server 2012 R2 v hello Galerie virtuálního počítače Azure, které můžete použít toocreate vlastního image šablony?</span><span class="sxs-lookup"><span data-stu-id="4a9f0-108">Did you know that your Azure RemoteApp subscription gives you access tooa Windows Server 2012 R2 image in hello Azure VM gallery that you can use toocreate your own template image?</span></span> <span data-ttu-id="4a9f0-109">[Projděte si](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-109">[Check it out](remoteapp-image-on-azurevm.md).</span></span>  
> 
> 

<span data-ttu-id="4a9f0-110">požadavky na Hello hello image, která mohou být nahrány pro použití s Azure Remoteappem jsou:</span><span class="sxs-lookup"><span data-stu-id="4a9f0-110">hello requirements for hello image that can be uploaded for use with Azure RemoteApp are:</span></span>

* <span data-ttu-id="4a9f0-111">Vlastní aplikace neukládejte data místně na bitovou kopii hello.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-111">Custom applications don’t store data locally on hello image.</span></span> <span data-ttu-id="4a9f0-112">Tyto Image jsou bezstavové a smí obsahovat pouze aplikace.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-112">These images are stateless and should only contain applications.</span></span>
* <span data-ttu-id="4a9f0-113">bitová kopie Hello neobsahuje data, která mohou být ztraceny.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-113">hello image does not contain data that can be lost.</span></span>
* <span data-ttu-id="4a9f0-114">velikost bitové kopie Hello musí být násobkem MB.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-114">hello image size should be a multiple of MBs.</span></span> <span data-ttu-id="4a9f0-115">Pokud se pokusíte tooupload obrázek, který není přesný více, nahrávání hello se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-115">If you try tooupload an image that is not an exact multiple, hello upload will fail.</span></span>
* <span data-ttu-id="4a9f0-116">velikost obrazu Hello musí být 127 GB nebo menší.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-116">hello image size must be 127 GB or smaller.</span></span>
* <span data-ttu-id="4a9f0-117">Musí být na soubor virtuálního pevného disku (VHDX soubory nejsou aktuálně podporovány.).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-117">It must be on a VHD file (VHDX files are not currently supported).</span></span>
* <span data-ttu-id="4a9f0-118">Hello virtuálního pevného disku nesmí být virtuální počítač generace 2.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-118">hello VHD must not be a generation 2 virtual machine.</span></span>
* <span data-ttu-id="4a9f0-119">Hello virtuální pevný disk může být buď pevné velikosti, nebo dynamicky se zvětšující.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-119">hello VHD can be either fixed-size or dynamically expanding.</span></span> <span data-ttu-id="4a9f0-120">Dynamicky se zvětšující virtuální pevný disk je doporučená, protože trvá méně času tooupload tooAzure než soubor virtuálního pevného disku pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-120">A dynamically expanding VHD is recommended because it takes less time tooupload tooAzure than a fixed-size VHD file.</span></span>
* <span data-ttu-id="4a9f0-121">Hello disku musí být inicializován pomocí rozdělení styl hello hlavní spouštěcí záznam (MBR).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-121">hello disk must be initialized using hello Master Boot Record (MBR) partitioning style.</span></span> <span data-ttu-id="4a9f0-122">Hello stylem oddílů GUID (GPT) oddílu není podporováno.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-122">hello GUID partition table (GPT) partition style is not supported.</span></span>
* <span data-ttu-id="4a9f0-123">Hello virtuální pevný disk musí obsahovat jeden instalace systému Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-123">hello VHD must contain a single installation of Windows Server 2012 R2.</span></span> <span data-ttu-id="4a9f0-124">Může obsahovat několik svazků, ale jenom jeden s obsahuje instalace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-124">It can contain multiple volumes, but only one that contains an installation of Windows.</span></span>
* <span data-ttu-id="4a9f0-125">musí být nainstalován Hello hostitele relace vzdálené plochy (RDSH) role a funkce Možnosti práce s počítačem hello.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-125">hello Remote Desktop Session Host (RDSH) role and hello Desktop Experience feature must be installed.</span></span>
* <span data-ttu-id="4a9f0-126">role Zprostředkovatel připojení ke vzdálené ploše Hello musí *není* nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-126">hello Remote Desktop Connection Broker role must *not* be installed.</span></span>
* <span data-ttu-id="4a9f0-127">musí se zakázat Hello systém souborů (Encrypting File System).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-127">hello Encrypting File System (EFS) must be disabled.</span></span>
* <span data-ttu-id="4a9f0-128">Hello bitová kopie musí být SYSPREPed pomocí parametrů hello **/oobe / generalize/shutdown** (nesmí použít hello **/mode:vm** parametr).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-128">hello image must be SYSPREPed using hello parameters **/oobe /generalize /shutdown** (DO NOT use hello **/mode:vm** parameter).</span></span>
* <span data-ttu-id="4a9f0-129">Odesílání svůj disk VHD z řetěz snímku není podporována.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-129">Uploading your VHD from a snapshot chain is not supported.</span></span>

<span data-ttu-id="4a9f0-130">V tématu [vytvoření image Azure Remoteappu](remoteapp-imageoptions.md) Další informace o vytváření bitových kopií pro Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-130">See [Create an Azure RemoteApp image](remoteapp-imageoptions.md) for more information about creating images for Azure RemoteApp.</span></span>

