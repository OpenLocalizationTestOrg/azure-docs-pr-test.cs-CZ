---
title: "Nahrát vlastní image pro Azure Remoteappu | Microsoft Docs"
description: "Naučte se nahrát vlastní image pro Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: 299e0510-1a6b-4fdf-914a-3631b061a360
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: ericor
ms.openlocfilehash: 5a235fac88d6e95ea294bda197641108acb4a09f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a><span data-ttu-id="9d8f9-103">Nahrát vlastní image pro Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="9d8f9-103">Upload a custom image for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9d8f9-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="9d8f9-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="9d8f9-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="9d8f9-106">Teď, když jste vytvořili vlastního image šablony, nebo byly aktualizovány s změny, budete muset nahrát této bitové kopie do bitové kopie knihovny Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-106">Now that you have created your custom template image or have updated it with changes, you need to upload that image to your Azure RemoteApp image library.</span></span> <span data-ttu-id="9d8f9-107">Pomocí těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-107">Use these steps.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="9d8f9-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9d8f9-108">Before you start</span></span>
1. <span data-ttu-id="9d8f9-109">Ověřit vlastní image splňuje [požadavky na image](remoteapp-imagereqs.md) a [požadavky na aplikace](remoteapp-appreqs.md).</span><span class="sxs-lookup"><span data-stu-id="9d8f9-109">Verify your custom image meets the [image requirements](remoteapp-imagereqs.md) and [application requirements](remoteapp-appreqs.md).</span></span>
2. <span data-ttu-id="9d8f9-110">Nainstalujte [modul Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9d8f9-110">Install the [Azure PowerShell module](/powershell/azure/overview).</span></span>

## <a name="step-by-step-on-how-to-upload-custom-image"></a><span data-ttu-id="9d8f9-111">Podrobný návod, jak nahrát vlastní image</span><span class="sxs-lookup"><span data-stu-id="9d8f9-111">Step by step on how to upload custom image</span></span>
1. <span data-ttu-id="9d8f9-112">Otevřete portál pro správu Azure a přejděte na stránku vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-112">Open Azure Management Portal and navigate to the RemoteApp page.</span></span>
2. <span data-ttu-id="9d8f9-113">Na **Image šablony** , klikněte na **nahrát** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-113">On the **Template images** tab, click **Upload** at the bottom of the page.</span></span>
3. <span data-ttu-id="9d8f9-114">Zadejte popisný název bitové kopie a umístění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-114">Enter a friendly name for your image and specify the storage account location.</span></span> <span data-ttu-id="9d8f9-115">Zkontrolujte, zda umístění stejné umístění jako vaše kolekce vzdálené aplikace RemoteApp nebo umístění, kde chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-115">Ensure the location is the same location as your RemoteApp collection or a location where you want to create one.</span></span>
4. <span data-ttu-id="9d8f9-116">Po zobrazení výzvy stáhněte skript na vašem místním počítači.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-116">When prompted, download the script to your local PC.</span></span>
5. <span data-ttu-id="9d8f9-117">Parametry příkazu v textovém poli zkopírujte do schránky.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-117">Copy the command parameters in the text box to your clipboard.</span></span>
6. <span data-ttu-id="9d8f9-118">Otevřete okno prostředí Windows PowerShell se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-118">Open an elevated Windows PowerShell window.</span></span>
7. <span data-ttu-id="9d8f9-119">Z okna zvýšenými oprávněními prostředí Windows PowerShell přejděte do stejného adresáře, kam jste stáhli skript.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-119">From the elevated Windows PowerShell window, navigate to the same directory where you downloaded the script.</span></span>
8. <span data-ttu-id="9d8f9-120">Vložte zkopírovaný příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-120">Paste the copied command and press **Enter**.</span></span>
   
   <span data-ttu-id="9d8f9-121">Zahájí proces odesílání a doba trvání může záviset na mnoha faktorech včetně rychlosti sítě a velikost bitové kopie</span><span class="sxs-lookup"><span data-stu-id="9d8f9-121">The upload process will begin and duration may depend on many factors including your network speed and size of the image</span></span>
9. <span data-ttu-id="9d8f9-122">Pokud vaše nahrávání nezdaří z důvodu přerušení sítě nebo věcí jako je například, můžete vždy obnovit proces odesílání, které jste začali.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-122">If your upload does not succeed because of network interruption or things like that, you can always resume the upload process you began.</span></span> <span data-ttu-id="9d8f9-123">Obnovit odeslání, spusťte skript znovu s použitím stejném příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-123">To resume an upload, run the script again using the same command line.</span></span>

> [!WARNING]
> <span data-ttu-id="9d8f9-124">Nikdy upravit nahrávání skript.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-124">Never modify the upload script.</span></span> <span data-ttu-id="9d8f9-125">Chcete-li zajistit, aby splňoval požadavky na image a požadavky na aplikace bitovou kopii je implementovaná specifických kontrolách.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-125">Specific checks have been implemented to ensure that the image meets the image requirements and application requirements.</span></span>
> 
> 

## <a name="common-problems"></a><span data-ttu-id="9d8f9-126">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="9d8f9-126">Common problems</span></span>
* <span data-ttu-id="9d8f9-127">Ujistěte se, že používáte prostředí Windows PowerShell, ne Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-127">Make sure you use Windows PowerShell, not Azure PowerShell.</span></span> <span data-ttu-id="9d8f9-128">Potřebujete nainstalovat modul Azure PowerShell, protože některé moduly jsou potřeba během procesu nahrávání.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-128">You need to install the Azure PowerShell module because certain modules are needed during the upload process.</span></span>
* <span data-ttu-id="9d8f9-129">Nikdy alter skript, ověření existují pro usnadnění vaší práce.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-129">Never alter the script, validations are there for your convenience.</span></span>
* <span data-ttu-id="9d8f9-130">Pokud soubor vhd získá uzamčen během nahrávání, zkopírujte soubor nebo ho přesunout do nového umístění a pokus o odeslání znovu.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-130">If the vhd file gets locked out during upload, copy the file or move it to a new location and attempt upload again.</span></span> <span data-ttu-id="9d8f9-131">Může být některé procesu systému Windows, který brání odeslání.</span><span class="sxs-lookup"><span data-stu-id="9d8f9-131">There might be some Windows process that is preventing upload.</span></span>  

