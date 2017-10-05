---
title: "Řešení potíží s cloud kolekce vzdálené aplikace RemoteApp – vytvoření | Microsoft Docs"
description: "Zjistěte, jak vyřešit chyby při vytváření cloudové kolekce vzdálené aplikace RemoteApp"
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
ms.openlocfilehash: 304ba7c5057b27c459bccbb75d3a711567757675
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a><span data-ttu-id="6e837-103">Řešení potíží s vytvoření cloudové kolekce vzdálené aplikace RemoteApp</span><span class="sxs-lookup"><span data-stu-id="6e837-103">Troubleshoot creating RemoteApp cloud collections</span></span>
> [!IMPORTANT]
> <span data-ttu-id="6e837-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="6e837-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="6e837-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="6e837-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="6e837-106">Pokud máte problémy s vytvářením cloudové kolekce, podívejte se na následující informace.</span><span class="sxs-lookup"><span data-stu-id="6e837-106">If you are having problems creating a cloud collection, check out the following information.</span></span>

## <a name="your-image-is-invalid"></a><span data-ttu-id="6e837-107">Bitové kopie je neplatný</span><span class="sxs-lookup"><span data-stu-id="6e837-107">Your image is invalid</span></span>
<span data-ttu-id="6e837-108">Pokud při čekají na Azure a zřídit kolekce se zobrazí zpráva podobná "GoldImageInvalid", znamená to, že nesplňuje image šablony [definované požadavky na image](remoteapp-imagereqs.md).</span><span class="sxs-lookup"><span data-stu-id="6e837-108">If you see a message like, "GoldImageInvalid" when you are waiting for Azure to provision your collection, it means that your template image doesn't meet the [defined image requirements](remoteapp-imagereqs.md).</span></span> <span data-ttu-id="6e837-109">Ano, přejděte číst ty [požadavky](remoteapp-imagereqs.md), oprava bitové kopie a pokuste se vytvořit kolekci znovu.</span><span class="sxs-lookup"><span data-stu-id="6e837-109">So, go read those [requirements](remoteapp-imagereqs.md), fix your image, and try to create your collection again.</span></span>

## <a name="common-errors-seen-in-the-azure-management-portal"></a><span data-ttu-id="6e837-110">Běžné chyby vidět v portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="6e837-110">Common errors seen in the Azure Management portal</span></span>
    DNS server could not be reached
    ProvisioningTimeout

<span data-ttu-id="6e837-111">Cloudová kolekce často selhání během vytváření kvůli můžete používají vlastní Image.</span><span class="sxs-lookup"><span data-stu-id="6e837-111">Cloud collections often fail during creation because of you are using custom images.</span></span>  <span data-ttu-id="6e837-112">Pokud najdete v jednom z výše uvedených chyb a vlastní image se používá k vytvoření této kolekce, zkontrolujte následující akce:</span><span class="sxs-lookup"><span data-stu-id="6e837-112">If you see one of the above errors and you are using a custom image to create the collection, please check the following things:</span></span>

* <span data-ttu-id="6e837-113">Zkontrolujte, zda vlastní image, který jste nahráli splňuje požadavky na image.</span><span class="sxs-lookup"><span data-stu-id="6e837-113">Ensure that the custom image you uploaded meets image requirements.</span></span>
* <span data-ttu-id="6e837-114">Nejčastěji častých problémů je, že obrázek nebyl správně syspreped.</span><span class="sxs-lookup"><span data-stu-id="6e837-114">Most often the common problem is that the image was not properly syspreped.</span></span>  
* <span data-ttu-id="6e837-115">Ověřte bitovou kopii můžete spustit v rámci technologie Hyper-V nebo zkuste vytvořit přímo ve vašem předplatném Azure pomocí bitové kopie virtuální počítač IAAS.</span><span class="sxs-lookup"><span data-stu-id="6e837-115">Verify the image can boot within Hyper-V or try creating an IAAS VM directly in your Azure subscription using the image.</span></span> <span data-ttu-id="6e837-116">Pokud virtuální počítač se nepodaří spustit a nespustí, obvykle označuje, že nebyl správně připraven vlastní image.</span><span class="sxs-lookup"><span data-stu-id="6e837-116">If the VM fails to boot and not start, then this usually indicates that the custom image was not prepared correctly.</span></span>  <span data-ttu-id="6e837-117">Zkontrolujte vlastní image byla vytvořena následujících pokynů k vytvoření vlastního image šablony pro vzdálené aplikace RemoteApp</span><span class="sxs-lookup"><span data-stu-id="6e837-117">Verify the custom image was built following the How to create a custom template image for RemoteApp</span></span>

<span data-ttu-id="6e837-118">Pokud použijete jednu z imagí Microsoft součástí vašeho předplatného, pokuste se vytvořit kolekci znovu.</span><span class="sxs-lookup"><span data-stu-id="6e837-118">If you are using one of the Microsoft images included with your subscription, try to create the collection again.</span></span> <span data-ttu-id="6e837-119">Pokud potíže potrvají kontaktujte podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6e837-119">If the issue persists then please contact Microsoft support.</span></span>

    PlatformImageTrialModeOnly

<span data-ttu-id="6e837-120">Pokud se zobrazí tato chyba to obvykle znamená, které nebyly upgradovány na placený účet, ale se pokoušíte použít image společnosti Microsoft, který je platný pouze během zkušební režim služby.</span><span class="sxs-lookup"><span data-stu-id="6e837-120">If you see this error this usually means that you been upgraded to a paid account but you are trying to use a Microsoft provided image that is valid only during the trial mode of the service.</span></span> <span data-ttu-id="6e837-121">V takovém případě zkuste vytvoření cloudové kolekce znovu, ale je nutné zadat správné bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6e837-121">In this case, try to create your cloud collection again, but be sure to specify the correct image.</span></span>

