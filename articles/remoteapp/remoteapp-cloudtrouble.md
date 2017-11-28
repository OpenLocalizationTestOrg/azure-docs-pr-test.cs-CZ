---
title: "aaaTroubleshoot vzdálené aplikace RemoteApp cloudové kolekce – vytvoření | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot vzdálené aplikace RemoteApp cloudové chyby při vytváření kolekce"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: 95eb7797-7b5e-4781-ad23-f15dd85b213d
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 9484ecbdb048ede8df725215b313e049cc7648f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a><span data-ttu-id="32fee-103">Řešení potíží s vytvoření cloudové kolekce vzdálené aplikace RemoteApp</span><span class="sxs-lookup"><span data-stu-id="32fee-103">Troubleshoot creating RemoteApp cloud collections</span></span>
> [!IMPORTANT]
> <span data-ttu-id="32fee-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="32fee-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="32fee-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="32fee-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="32fee-106">Pokud máte problémy s vytvářením cloudové kolekce, podívejte se na hello následující informace.</span><span class="sxs-lookup"><span data-stu-id="32fee-106">If you are having problems creating a cloud collection, check out hello following information.</span></span>

## <a name="your-image-is-invalid"></a><span data-ttu-id="32fee-107">Bitové kopie je neplatný</span><span class="sxs-lookup"><span data-stu-id="32fee-107">Your image is invalid</span></span>
<span data-ttu-id="32fee-108">Pokud při čekání Azure tooprovision kolekce se zobrazí zpráva podobná "GoldImageInvalid", znamená to, že image šablony nesplňuje hello [definované požadavky na image](remoteapp-imagereqs.md).</span><span class="sxs-lookup"><span data-stu-id="32fee-108">If you see a message like, "GoldImageInvalid" when you are waiting for Azure tooprovision your collection, it means that your template image doesn't meet hello [defined image requirements](remoteapp-imagereqs.md).</span></span> <span data-ttu-id="32fee-109">Ano, přejděte číst ty [požadavky](remoteapp-imagereqs.md)opravte bitové kopie a akci toocreate kolekci znovu.</span><span class="sxs-lookup"><span data-stu-id="32fee-109">So, go read those [requirements](remoteapp-imagereqs.md), fix your image, and try toocreate your collection again.</span></span>

## <a name="common-errors-seen-in-hello-azure-management-portal"></a><span data-ttu-id="32fee-110">Běžné chyby vidět v portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="32fee-110">Common errors seen in hello Azure Management portal</span></span>
    DNS server could not be reached
    ProvisioningTimeout

<span data-ttu-id="32fee-111">Cloudová kolekce často selhání během vytváření kvůli můžete používají vlastní Image.</span><span class="sxs-lookup"><span data-stu-id="32fee-111">Cloud collections often fail during creation because of you are using custom images.</span></span>  <span data-ttu-id="32fee-112">Pokud dojde k jedné hello výše chyby a používáte vlastní image toocreate hello kolekce, Zkontrolujte prosím hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="32fee-112">If you see one of hello above errors and you are using a custom image toocreate hello collection, please check hello following things:</span></span>

* <span data-ttu-id="32fee-113">Zajistěte, aby této hello vlastní bitové kopie, který jste nahráli splňuje požadavky na image.</span><span class="sxs-lookup"><span data-stu-id="32fee-113">Ensure that hello custom image you uploaded meets image requirements.</span></span>
* <span data-ttu-id="32fee-114">Nejčastěji hello častých problémů je že této bitové kopie hello nebyla správně syspreped.</span><span class="sxs-lookup"><span data-stu-id="32fee-114">Most often hello common problem is that hello image was not properly syspreped.</span></span>  
* <span data-ttu-id="32fee-115">Ověřte hello image můžete spustit v rámci technologie Hyper-V nebo zkuste vytvořit přímo ve vašem předplatném Azure pomocí bitové kopie hello virtuálních počítačů IAAS.</span><span class="sxs-lookup"><span data-stu-id="32fee-115">Verify hello image can boot within Hyper-V or try creating an IAAS VM directly in your Azure subscription using hello image.</span></span> <span data-ttu-id="32fee-116">Pokud hello virtuálního počítače selže tooboot a nespustí, pak to obvykle znamená, že této bitové kopie vlastní hello nebyl připraven správně.</span><span class="sxs-lookup"><span data-stu-id="32fee-116">If hello VM fails tooboot and not start, then this usually indicates that hello custom image was not prepared correctly.</span></span>  <span data-ttu-id="32fee-117">Ověřte, zda byl vytvořený vlastní image hello následující hello jak toocreate vlastní šablony obrázků pro vzdálené aplikace RemoteApp</span><span class="sxs-lookup"><span data-stu-id="32fee-117">Verify hello custom image was built following hello How toocreate a custom template image for RemoteApp</span></span>

<span data-ttu-id="32fee-118">Pokud použijete jednu z imagí Microsoft hello součástí vašeho předplatného, zkuste toocreate hello kolekci znovu.</span><span class="sxs-lookup"><span data-stu-id="32fee-118">If you are using one of hello Microsoft images included with your subscription, try toocreate hello collection again.</span></span> <span data-ttu-id="32fee-119">Pokud hello potíže potrvají, kontaktujte podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32fee-119">If hello issue persists then please contact Microsoft support.</span></span>

    PlatformImageTrialModeOnly

<span data-ttu-id="32fee-120">Pokud se zobrazí tato chyba obvykle znamená to, byl upgradován tooa placené účet ale pokoušíte toouse bitovou kopii společnosti Microsoft, který je platný pouze během zkušební režim hello hello služby.</span><span class="sxs-lookup"><span data-stu-id="32fee-120">If you see this error this usually means that you been upgraded tooa paid account but you are trying toouse a Microsoft provided image that is valid only during hello trial mode of hello service.</span></span> <span data-ttu-id="32fee-121">V takovém případě zkuste toocreate cloudové kolekce znovu, ale být jisti toospecify hello správné bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="32fee-121">In this case, try toocreate your cloud collection again, but be sure toospecify hello correct image.</span></span>

