---
title: "aaaUpload vlastní image pro Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak tooupload vlastní obrázek pro Azure RemoteApp"
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
ms.openlocfilehash: 6ad40fe58795ece37f4c7900be01bc713938da87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a><span data-ttu-id="30ca1-103">Nahrát vlastní image pro Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="30ca1-103">Upload a custom image for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="30ca1-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="30ca1-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="30ca1-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="30ca1-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="30ca1-106">Teď, když jste vytvořili vlastního image šablony, nebo byly aktualizovány s změny, musíte tooupload bitové kopie knihovny Azure Remoteappu tooyour této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="30ca1-106">Now that you have created your custom template image or have updated it with changes, you need tooupload that image tooyour Azure RemoteApp image library.</span></span> <span data-ttu-id="30ca1-107">Pomocí těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="30ca1-107">Use these steps.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="30ca1-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="30ca1-108">Before you start</span></span>
1. <span data-ttu-id="30ca1-109">Ověřit vlastní image splňuje hello [požadavky na image](remoteapp-imagereqs.md) a [požadavky na aplikace](remoteapp-appreqs.md).</span><span class="sxs-lookup"><span data-stu-id="30ca1-109">Verify your custom image meets hello [image requirements](remoteapp-imagereqs.md) and [application requirements](remoteapp-appreqs.md).</span></span>
2. <span data-ttu-id="30ca1-110">Nainstalujte hello [modul Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="30ca1-110">Install hello [Azure PowerShell module](/powershell/azure/overview).</span></span>

## <a name="step-by-step-on-how-tooupload-custom-image"></a><span data-ttu-id="30ca1-111">Podrobný návod, jak tooupload vlastní image</span><span class="sxs-lookup"><span data-stu-id="30ca1-111">Step by step on how tooupload custom image</span></span>
1. <span data-ttu-id="30ca1-112">Otevřete portál pro správu Azure a přejděte toohello vzdálené aplikace RemoteApp stránky.</span><span class="sxs-lookup"><span data-stu-id="30ca1-112">Open Azure Management Portal and navigate toohello RemoteApp page.</span></span>
2. <span data-ttu-id="30ca1-113">Na hello **Image šablony** , klikněte na **nahrát** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="30ca1-113">On hello **Template images** tab, click **Upload** at hello bottom of hello page.</span></span>
3. <span data-ttu-id="30ca1-114">Zadejte popisný název bitové kopie a umístění účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="30ca1-114">Enter a friendly name for your image and specify hello storage account location.</span></span> <span data-ttu-id="30ca1-115">Zkontrolujte umístění hello hello stejné umístění jako vaše kolekce vzdálené aplikace RemoteApp nebo do umístění, kam chcete toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="30ca1-115">Ensure hello location is hello same location as your RemoteApp collection or a location where you want toocreate one.</span></span>
4. <span data-ttu-id="30ca1-116">Po zobrazení výzvy stáhnout hello skriptu tooyour místní počítač.</span><span class="sxs-lookup"><span data-stu-id="30ca1-116">When prompted, download hello script tooyour local PC.</span></span>
5. <span data-ttu-id="30ca1-117">Parametry příkazu hello zkopírujte do schránky tooyour pole textu hello.</span><span class="sxs-lookup"><span data-stu-id="30ca1-117">Copy hello command parameters in hello text box tooyour clipboard.</span></span>
6. <span data-ttu-id="30ca1-118">Otevřete okno prostředí Windows PowerShell se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="30ca1-118">Open an elevated Windows PowerShell window.</span></span>
7. <span data-ttu-id="30ca1-119">Z hello se zvýšenými oprávněními okno prostředí Windows PowerShell, přejděte toohello stejný adresář, kam jste stáhli hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="30ca1-119">From hello elevated Windows PowerShell window, navigate toohello same directory where you downloaded hello script.</span></span>
8. <span data-ttu-id="30ca1-120">Vložení hello zkopírovat příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="30ca1-120">Paste hello copied command and press **Enter**.</span></span>
   
   <span data-ttu-id="30ca1-121">zahájí proces odesílání Hello a doba trvání může záviset na mnoha faktorech včetně rychlosti sítě a velikost bitové kopie hello</span><span class="sxs-lookup"><span data-stu-id="30ca1-121">hello upload process will begin and duration may depend on many factors including your network speed and size of hello image</span></span>
9. <span data-ttu-id="30ca1-122">Pokud vaše nahrávání nezdaří z důvodu přerušení sítě nebo věcí jako je například, můžete vždy obnovit hello nahrávání procesu, který jste začali.</span><span class="sxs-lookup"><span data-stu-id="30ca1-122">If your upload does not succeed because of network interruption or things like that, you can always resume hello upload process you began.</span></span> <span data-ttu-id="30ca1-123">tooresume nahrávaný, spusťte skript hello znovu s použitím hello stejné příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="30ca1-123">tooresume an upload, run hello script again using hello same command line.</span></span>

> [!WARNING]
> <span data-ttu-id="30ca1-124">Nikdy změnit skriptu nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="30ca1-124">Never modify hello upload script.</span></span> <span data-ttu-id="30ca1-125">Kontrolách konkrétních byla implementovaná tooensure, který hello image splňuje hello image požadavky a požadavky na aplikace.</span><span class="sxs-lookup"><span data-stu-id="30ca1-125">Specific checks have been implemented tooensure that hello image meets hello image requirements and application requirements.</span></span>
> 
> 

## <a name="common-problems"></a><span data-ttu-id="30ca1-126">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="30ca1-126">Common problems</span></span>
* <span data-ttu-id="30ca1-127">Ujistěte se, že používáte prostředí Windows PowerShell, ne Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="30ca1-127">Make sure you use Windows PowerShell, not Azure PowerShell.</span></span> <span data-ttu-id="30ca1-128">Je nutné modul Azure PowerShell hello tooinstall, protože některé moduly jsou potřeba během procesu nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="30ca1-128">You need tooinstall hello Azure PowerShell module because certain modules are needed during hello upload process.</span></span>
* <span data-ttu-id="30ca1-129">Nikdy alter hello skriptu, ověření existují pro usnadnění vaší práce.</span><span class="sxs-lookup"><span data-stu-id="30ca1-129">Never alter hello script, validations are there for your convenience.</span></span>
* <span data-ttu-id="30ca1-130">Pokud soubor virtuálního pevného disku hello získá uzamčen během nahrávání, zkopírujte soubor hello nebo přesunout ho znovu tooa nové umístění a pokus o odeslání.</span><span class="sxs-lookup"><span data-stu-id="30ca1-130">If hello vhd file gets locked out during upload, copy hello file or move it tooa new location and attempt upload again.</span></span> <span data-ttu-id="30ca1-131">Může být některé procesu systému Windows, který brání odeslání.</span><span class="sxs-lookup"><span data-stu-id="30ca1-131">There might be some Windows process that is preventing upload.</span></span>  

