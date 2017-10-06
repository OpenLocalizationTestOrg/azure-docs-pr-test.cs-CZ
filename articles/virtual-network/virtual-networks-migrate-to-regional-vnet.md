---
title: "aaaMigrate virtuální sítě Azure (klasické) z oblast tooa skupiny vztahů | Microsoft Docs"
description: "Zjistěte, jak toomigrate virtuální sítě (klasické) z spřažení Skupina oblast tooa."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: e3a5c0f21e748912e29e2e8d03f4295720e63637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-tooa-region"></a><span data-ttu-id="58f99-103">Migrovat virtuální sítě (klasické) z oblast tooa skupiny vztahů</span><span class="sxs-lookup"><span data-stu-id="58f99-103">Migrate a virtual network (classic) from an affinity group tooa region</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58f99-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="58f99-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="58f99-105">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="58f99-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="58f99-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="58f99-106">Microsoft recommends that most new deployments use hello Resource Manager deployment model.</span></span>

<span data-ttu-id="58f99-107">Skupiny vztahů zajistěte, aby prostředky vytvořené v rámci hello stejnou skupinu vztahů, které jsou fyzicky hostované servery, které jsou blízko sebe, povolení tyto prostředky toocommunicate rychlejší.</span><span class="sxs-lookup"><span data-stu-id="58f99-107">Affinity groups ensure that resources created within hello same affinity group are physically hosted by servers that are close together, enabling these resources toocommunicate quicker.</span></span> <span data-ttu-id="58f99-108">V posledních hello skupiny vztahů byly požadavek pro vytvoření virtuální sítě (klasické).</span><span class="sxs-lookup"><span data-stu-id="58f99-108">In hello past, affinity groups were a requirement for creating virtual networks (classic).</span></span> <span data-ttu-id="58f99-109">V té době hello síťové Správce služby, který spravovat virtuální sítě (klasické) může fungovat pouze v rámci sady fyzických serverech nebo jednotky škálování.</span><span class="sxs-lookup"><span data-stu-id="58f99-109">At that time, hello network manager service that managed virtual networks (classic) could only work within a set of physical servers or scale unit.</span></span> <span data-ttu-id="58f99-110">Architektury vylepšení mají vyšší hello oboru oblasti tooa správy sítě.</span><span class="sxs-lookup"><span data-stu-id="58f99-110">Architectural improvements have increased hello scope of network management tooa region.</span></span>

<span data-ttu-id="58f99-111">V důsledku těchto vylepšení architektury jsou skupiny vztahů už doporučené, nebo požadované pro virtuální sítě (klasické).</span><span class="sxs-lookup"><span data-stu-id="58f99-111">As a result of these architectural improvements, affinity groups are no longer recommended, or required for virtual networks (classic).</span></span> <span data-ttu-id="58f99-112">použití Hello skupin vztahů pro virtuální sítě (klasické) je nahrazena oblasti.</span><span class="sxs-lookup"><span data-stu-id="58f99-112">hello use of affinity groups for virtual networks (classic) is replaced by regions.</span></span> <span data-ttu-id="58f99-113">Virtuální sítě (klasické), které jsou spojeny s oblastí se nazývají regionálních virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="58f99-113">Virtual networks (classic) that are associated with regions are called regional virtual networks.</span></span>

<span data-ttu-id="58f99-114">Doporučujeme, že skupiny vztahů nepoužíváte obecně.</span><span class="sxs-lookup"><span data-stu-id="58f99-114">We recommend that you don't use affinity groups in general.</span></span> <span data-ttu-id="58f99-115">Kromě zajištění dostatečného hello požadavek virtuální sítě skupiny vztahů byly také důležité toouse tooensure prostředky, například výpočetní (klasické) a úložiště (klasické), byly umístěny vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="58f99-115">Aside from hello virtual network requirement, affinity groups were also important toouse tooensure resources, such as compute (classic) and storage (classic), were placed near each other.</span></span> <span data-ttu-id="58f99-116">Však s hello aktuální Azure síťovou architekturu, tyto požadavky na umístění již nejsou potřebné.</span><span class="sxs-lookup"><span data-stu-id="58f99-116">However, with hello current Azure network architecture, these placement requirements are no longer necessary.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58f99-117">Přestože je technicky možné toocreate virtuální sítě, který je přidružený skupině vztahů, neexistuje žádný poutavé toodo důvod tak.</span><span class="sxs-lookup"><span data-stu-id="58f99-117">Although it is still technically possible toocreate a virtual network that is associated with an affinity group, there is no compelling reason toodo so.</span></span> <span data-ttu-id="58f99-118">Mnoho funkcí virtuální sítě, jako jsou skupiny zabezpečení sítě, jsou k dispozici pouze při použití regionální virtuální síť a nejsou k dispozici pro virtuální sítě, které jsou přidružené skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="58f99-118">Many virtual network features, such as network security groups, are only available when using a regional virtual network, and are not available for virtual networks that are associated with affinity groups.</span></span>
> 
> 

## <a name="edit-hello-network-configuration-file"></a><span data-ttu-id="58f99-119">Upravovat soubor konfigurace sítě hello</span><span class="sxs-lookup"><span data-stu-id="58f99-119">Edit hello network configuration file</span></span>

1. <span data-ttu-id="58f99-120">Exportujte hello sítě konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="58f99-120">Export hello network configuration file.</span></span> <span data-ttu-id="58f99-121">najdete v části toolearn jak tooexport konfiguraci sítě souboru pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure (CLI) 1.0, hello [konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md#export).</span><span class="sxs-lookup"><span data-stu-id="58f99-121">toolearn how tooexport a network configuration file using PowerShell or hello Azure command-line interface (CLI) 1.0, see [Configure a virtual network using a network configuration file](virtual-networks-using-network-configuration-file.md#export).</span></span>
2. <span data-ttu-id="58f99-122">Upravte soubor konfigurace sítě hello nahrazení **AffinityGroup** s **umístění**.</span><span class="sxs-lookup"><span data-stu-id="58f99-122">Edit hello network configuration file, replacing **AffinityGroup** with **Location**.</span></span> <span data-ttu-id="58f99-123">Zadejte Azure [oblast](https://azure.microsoft.com/regions) pro **umístění**.</span><span class="sxs-lookup"><span data-stu-id="58f99-123">You specify an Azure [region](https://azure.microsoft.com/regions) for **Location**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="58f99-124">Hello **umístění** hello oblast, která jste zadali pro skupiny vztahů hello, který je přidružený virtuální sítě (klasické).</span><span class="sxs-lookup"><span data-stu-id="58f99-124">hello **Location** is hello region that you specified for hello affinity group that is associated with your virtual network (classic).</span></span> <span data-ttu-id="58f99-125">Pokud virtuální sítě (klasické) souvisí s skupinu vztahů, který je umístěný v západní USA, když provádíte migraci, například vaše **umístění** musí odkazovat tooWest USA.</span><span class="sxs-lookup"><span data-stu-id="58f99-125">For example, if your virtual network (classic) is associated with an affinity group that is located in West US, when you migrate, your **Location** must point tooWest US.</span></span> 
   > 
   > 
   
    <span data-ttu-id="58f99-126">Úpravy hello následující řádky v konfiguračním souboru sítě, nahraďte vlastními hodnotami hello:</span><span class="sxs-lookup"><span data-stu-id="58f99-126">Edit hello following lines in your network configuration file, replacing hello values with your own:</span></span> 
   
    <span data-ttu-id="58f99-127">**Původní hodnota:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\></span><span class="sxs-lookup"><span data-stu-id="58f99-127">**Old value:** \<VirtualNetworkSitename="VNetUSWest" AffinityGroup="VNetDemoAG"\></span></span> 
   
    <span data-ttu-id="58f99-128">**Nová hodnota:** \<VirtualNetworkSitename = "VNetUSWest" umístění = "Západní USA"\></span><span class="sxs-lookup"><span data-stu-id="58f99-128">**New value:** \<VirtualNetworkSitename="VNetUSWest" Location="West US"\></span></span>
3. <span data-ttu-id="58f99-129">Uložte změny a [importovat](virtual-networks-using-network-configuration-file.md#import) hello tooAzure konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="58f99-129">Save your changes and [import](virtual-networks-using-network-configuration-file.md#import) hello network configuration tooAzure.</span></span>

> [!NOTE]
> <span data-ttu-id="58f99-130">Tato migrace nezpůsobí žádné výpadky tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="58f99-130">This migration does NOT cause any downtime tooyour services.</span></span>
> 
> 

## <a name="what-toodo-if-you-have-a-vm-classic-in-an-affinity-group"></a><span data-ttu-id="58f99-131">Jaké toodo, pokud máte virtuální počítač (klasický) ve skupině vztahů</span><span class="sxs-lookup"><span data-stu-id="58f99-131">What toodo if you have a VM (classic) in an affinity group</span></span>
<span data-ttu-id="58f99-132">Virtuální počítače (klasické) jsou aktuálně ve skupině vztahů nemusí toobe odebrat ze skupiny vztahů hello.</span><span class="sxs-lookup"><span data-stu-id="58f99-132">VMs (classic) that are currently in an affinity group do not need toobe removed from hello affinity group.</span></span> <span data-ttu-id="58f99-133">Po nasazení virtuálního počítače je jednotka nasazené tooa jeden škálování.</span><span class="sxs-lookup"><span data-stu-id="58f99-133">Once a VM is deployed, it is deployed tooa single scale unit.</span></span> <span data-ttu-id="58f99-134">Skupiny vztahů můžete omezit hello sadu dostupných velikostí virtuálních počítačů pro nové nasazení virtuálního počítače, ale existující virtuální počítač, který je nasazen s omezeným přístupem již k dispozici v jednotce škálování hello, ve které hello virtuální počítač nasazen velikosti toohello sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="58f99-134">Affinity groups can restrict hello set of available VM sizes for a new VM deployment, but any existing VM that is deployed is already restricted toohello set of VM sizes available in hello scale unit in which hello VM is deployed.</span></span> <span data-ttu-id="58f99-135">Protože hello virtuální počítač je již nasazen jednotky škálování tooa, odebrání virtuálního počítače ze skupiny vztahů nemá žádný vliv na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="58f99-135">Because hello VM is already deployed tooa scale unit, removing a VM from an affinity group has no effect on hello VM.</span></span>
