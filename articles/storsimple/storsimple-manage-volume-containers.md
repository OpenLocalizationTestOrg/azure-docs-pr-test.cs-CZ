---
title: "Spravovat vaše kontejnery svazků zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak můžete použít stránku svazku kontejnery služby StorSimple Manager přidat, upravit nebo odstranit kontejner svazků."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: bb55a7a4bff0fd4319de6f6ce958686ad8a4142b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a><span data-ttu-id="007ab-103">Použít službu StorSimple Manager ke správě kontejnery svazků zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="007ab-103">Use the StorSimple Manager service to manage StorSimple volume containers</span></span>
## <a name="overview"></a><span data-ttu-id="007ab-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="007ab-104">Overview</span></span>
<span data-ttu-id="007ab-105">Tento kurz vysvětluje, jak používat službu StorSimple Manager k vytváření a správě kontejnery svazků zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="007ab-105">This tutorial explains how to use the StorSimple Manager service to create and manage StorSimple volume containers.</span></span>

<span data-ttu-id="007ab-106">Kontejner svazků v zařízení s Microsoft Azure StorSimple obsahuje jeden nebo více svazků, které sdílejí účet úložiště, šifrování a nastavení spotřeba šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="007ab-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="007ab-107">Zařízení může mít několik kontejnery svazků pro všechny jeho svazky.</span><span class="sxs-lookup"><span data-stu-id="007ab-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="007ab-108">Kontejner svazků má následující atributy:</span><span class="sxs-lookup"><span data-stu-id="007ab-108">A volume container has the following attributes:</span></span>

* <span data-ttu-id="007ab-109">**Svazky** – vrstvené nebo místně vázaný svazky zařízení StorSimple, které jsou obsaženy v rámci kontejneru svazků.</span><span class="sxs-lookup"><span data-stu-id="007ab-109">**Volumes** – The tiered or locally pinned StorSimple volumes that are contained within the volume container.</span></span> <span data-ttu-id="007ab-110">Kontejner svazků může obsahovat až 256 svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="007ab-110">A volume container may contain up to 256 StorSimple volumes.</span></span>
* <span data-ttu-id="007ab-111">**Šifrování** – šifrovací klíč, který lze definovat pro každý kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="007ab-111">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="007ab-112">Tento klíč se používá pro šifrování dat, který se odesílá z zařízení StorSimple do cloudu.</span><span class="sxs-lookup"><span data-stu-id="007ab-112">This key is used for encrypting the data that is sent from your StorSimple device to the cloud.</span></span> <span data-ttu-id="007ab-113">Klíč vojenských úrovni AES 256 bitů se používá s klíčem zadanou uživatelem.</span><span class="sxs-lookup"><span data-stu-id="007ab-113">A military-grade AES-256 bit key is used with the user-entered key.</span></span> <span data-ttu-id="007ab-114">K zabezpečení dat, doporučujeme vždy povolit šifrování úložiště cloudu.</span><span class="sxs-lookup"><span data-stu-id="007ab-114">To secure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="007ab-115">**Účet úložiště** – účet úložiště, který je propojený s poskytovatele cloudových služeb úložiště.</span><span class="sxs-lookup"><span data-stu-id="007ab-115">**Storage account** – The storage account that is linked to your cloud storage service provider.</span></span> <span data-ttu-id="007ab-116">Všechny svazky, které se nacházejí v kontejneru svazků sdílet tento účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="007ab-116">All the volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="007ab-117">Můžete vybrat účet úložiště z existujícího seznamu nebo vytvořte nový účet při vytváření kontejneru svazku a pak zadejte přihlašovací údaje pro tento účet přístup.</span><span class="sxs-lookup"><span data-stu-id="007ab-117">You can choose a storage account from an existing list, or create a new account when you create the volume container and then specify the access credentials for that account.</span></span>
* <span data-ttu-id="007ab-118">**Cloud šířky pásma** – šířku pásma spotřebovávají zařízení při odesílání dat ze zařízení do cloudu.</span><span class="sxs-lookup"><span data-stu-id="007ab-118">**Cloud bandwidth** – The bandwidth consumed by the device when the data from the device is being sent to the cloud.</span></span> <span data-ttu-id="007ab-119">Zadejte hodnotu mezi 1 a 1000 Mb/s při definování tento kontejner může vynutit řízení šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="007ab-119">You can enforce a bandwidth control by specifying a value between 1 and 1000 Mbps when you define this container.</span></span> <span data-ttu-id="007ab-120">Pokud chcete spotřebovávat veškerou dostupnou šířku pásma na zařízení, nastavte pole na neomezený.</span><span class="sxs-lookup"><span data-stu-id="007ab-120">If you want the device to consume all available bandwidth, set this field to Unlimited.</span></span> <span data-ttu-id="007ab-121">Můžete také vytvořit a použít šablonu šířky pásma přidělení šířky pásma na základě plánu.</span><span class="sxs-lookup"><span data-stu-id="007ab-121">You can also create and apply a bandwidth template to allocate bandwidth based on schedule.</span></span>

![Stránka kontejnery svazku](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

<span data-ttu-id="007ab-123">Tato následující postupy popisují, jak se StorSimple **kontejnery svazků** stránka dokončení následující běžné operace:</span><span class="sxs-lookup"><span data-stu-id="007ab-123">This following procedures explain how to use the StorSimple **Volume containers** page to complete the following common operations:</span></span>

* <span data-ttu-id="007ab-124">Přidat kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="007ab-124">Add a volume container</span></span> 
* <span data-ttu-id="007ab-125">Upravit kontejneru svazků</span><span class="sxs-lookup"><span data-stu-id="007ab-125">Modify a volume container</span></span> 
* <span data-ttu-id="007ab-126">Odstranit kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="007ab-126">Delete a volume container</span></span> 

## <a name="add-a-volume-container"></a><span data-ttu-id="007ab-127">Přidat kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="007ab-127">Add a volume container</span></span>
<span data-ttu-id="007ab-128">Proveďte následující postup pro přidání kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="007ab-128">Perform the following steps to add a volume container.</span></span>

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="007ab-129">Upravit kontejneru svazků</span><span class="sxs-lookup"><span data-stu-id="007ab-129">Modify a volume container</span></span>
<span data-ttu-id="007ab-130">Proveďte následující kroky k úpravě kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="007ab-130">Perform the following steps to modify a volume container.</span></span>

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="007ab-131">Odstranit kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="007ab-131">Delete a volume container</span></span>
<span data-ttu-id="007ab-132">Kontejner svazků obsahuje svazky v něm.</span><span class="sxs-lookup"><span data-stu-id="007ab-132">A volume container has volumes within it.</span></span> <span data-ttu-id="007ab-133">Lze odstranit pouze v případě, že všechny svazky, které v ní jsou nejprve odstranit.</span><span class="sxs-lookup"><span data-stu-id="007ab-133">It can be deleted only if all the volumes contained in it are first deleted.</span></span> <span data-ttu-id="007ab-134">Proveďte následující kroky se odstranit kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="007ab-134">Perform the following steps to delete a volume container.</span></span>

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="007ab-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="007ab-135">Next steps</span></span>
* <span data-ttu-id="007ab-136">Další informace o [správu svazků zařízení StorSimple](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="007ab-136">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span> 
* <span data-ttu-id="007ab-137">Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="007ab-137">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

