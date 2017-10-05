---
title: "Spravovat vaše kontejnery svazků zařízení StorSimple v zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Vysvětluje, jak můžete použít stránku kontejnery svazku služby StorSimple Manager zařízení přidat, upravit nebo odstranit kontejner svazků."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 0f8e00d6d07224f56625482f339e612e68914be2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-volume-containers"></a><span data-ttu-id="89c5b-103">Použít službu StorSimple Manager zařízení ke správě kontejnery svazků zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="89c5b-103">Use the StorSimple Device Manager service to manage StorSimple volume containers</span></span>

## <a name="overview"></a><span data-ttu-id="89c5b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="89c5b-104">Overview</span></span>
<span data-ttu-id="89c5b-105">Tento kurz vysvětluje, jak používat službu StorSimple Manager zařízení k vytváření a správě kontejnery svazků zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="89c5b-105">This tutorial explains how to use the StorSimple Device Manager service to create and manage StorSimple volume containers.</span></span>

<span data-ttu-id="89c5b-106">Kontejner svazků v zařízení s Microsoft Azure StorSimple obsahuje jeden nebo více svazků, které sdílejí účet úložiště, šifrování a nastavení spotřeba šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="89c5b-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="89c5b-107">Zařízení může mít několik kontejnery svazků pro všechny jeho svazky.</span><span class="sxs-lookup"><span data-stu-id="89c5b-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="89c5b-108">Kontejner svazků má následující atributy:</span><span class="sxs-lookup"><span data-stu-id="89c5b-108">A volume container has the following attributes:</span></span>

* <span data-ttu-id="89c5b-109">**Svazky** – vrstvené nebo místně vázaný svazky zařízení StorSimple, které jsou obsaženy v rámci kontejneru svazků.</span><span class="sxs-lookup"><span data-stu-id="89c5b-109">**Volumes** – The tiered or locally pinned StorSimple volumes that are contained within the volume container.</span></span> 
* <span data-ttu-id="89c5b-110">**Šifrování** – šifrovací klíč, který lze definovat pro každý kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="89c5b-110">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="89c5b-111">Tento klíč se používá pro šifrování dat, který se odesílá z zařízení StorSimple do cloudu.</span><span class="sxs-lookup"><span data-stu-id="89c5b-111">This key is used for encrypting the data that is sent from your StorSimple device to the cloud.</span></span> <span data-ttu-id="89c5b-112">Klíč vojenských úrovni AES 256 bitů se používá s klíčem zadanou uživatelem.</span><span class="sxs-lookup"><span data-stu-id="89c5b-112">A military-grade AES-256 bit key is used with the user-entered key.</span></span> <span data-ttu-id="89c5b-113">K zabezpečení dat, doporučujeme vždy povolit šifrování úložiště cloudu.</span><span class="sxs-lookup"><span data-stu-id="89c5b-113">To secure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="89c5b-114">**Účet úložiště** – účet úložiště Azure, který se používá k ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="89c5b-114">**Storage account** – The Azure storage account that is used to store the data.</span></span> <span data-ttu-id="89c5b-115">Všechny svazky, které se nacházejí v kontejneru svazků sdílet tento účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="89c5b-115">All the volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="89c5b-116">Můžete vybrat účet úložiště z existujícího seznamu nebo vytvořte nový účet při vytváření kontejneru svazku a pak zadejte přihlašovací údaje pro tento účet přístup.</span><span class="sxs-lookup"><span data-stu-id="89c5b-116">You can choose a storage account from an existing list, or create a new account when you create the volume container and then specify the access credentials for that account.</span></span>
* <span data-ttu-id="89c5b-117">**Cloud šířky pásma** – šířku pásma spotřebovávají zařízení při odesílání dat ze zařízení do cloudu.</span><span class="sxs-lookup"><span data-stu-id="89c5b-117">**Cloud bandwidth** – The bandwidth consumed by the device when the data from the device is being sent to the cloud.</span></span> <span data-ttu-id="89c5b-118">Řízení šířky pásma můžete vynutit tak, že zadáte hodnotu v rozmezí 1 MB/s až 1 000 MB/s, při vytváření tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="89c5b-118">You can enforce a bandwidth control by specifying a value between 1 Mbps and 1,000 Mbps when you create this container.</span></span> <span data-ttu-id="89c5b-119">Pokud chcete zařízení může používat všechny dostupné šířky pásma, nastavte pole na **neomezený**.</span><span class="sxs-lookup"><span data-stu-id="89c5b-119">If you want the device to consume all available bandwidth, set this field to **Unlimited**.</span></span> <span data-ttu-id="89c5b-120">Můžete také vytvořit a použít šablonu šířky pásma přidělení šířky pásma na základě plánu.</span><span class="sxs-lookup"><span data-stu-id="89c5b-120">You can also create and apply a bandwidth template to allocate bandwidth based on schedule.</span></span>

<span data-ttu-id="89c5b-121">Následující postupy popisují, jak se StorSimple **kontejnery svazků** okno a dokončete následující běžné operace:</span><span class="sxs-lookup"><span data-stu-id="89c5b-121">The following procedures explain how to use the StorSimple **Volume containers** blade to complete the following common operations:</span></span>

* <span data-ttu-id="89c5b-122">Přidat kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="89c5b-122">Add a volume container</span></span>
* <span data-ttu-id="89c5b-123">Upravit kontejneru svazků</span><span class="sxs-lookup"><span data-stu-id="89c5b-123">Modify a volume container</span></span>
* <span data-ttu-id="89c5b-124">Odstranit kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="89c5b-124">Delete a volume container</span></span>

## <a name="add-a-volume-container"></a><span data-ttu-id="89c5b-125">Přidat kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="89c5b-125">Add a volume container</span></span>
<span data-ttu-id="89c5b-126">Proveďte následující postup pro přidání kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="89c5b-126">Perform the following steps to add a volume container.</span></span>

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="89c5b-127">Upravit kontejneru svazků</span><span class="sxs-lookup"><span data-stu-id="89c5b-127">Modify a volume container</span></span>
<span data-ttu-id="89c5b-128">Proveďte následující kroky k úpravě kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="89c5b-128">Perform the following steps to modify a volume container.</span></span>

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="89c5b-129">Odstranit kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="89c5b-129">Delete a volume container</span></span>
<span data-ttu-id="89c5b-130">Kontejner svazků obsahuje svazky v něm.</span><span class="sxs-lookup"><span data-stu-id="89c5b-130">A volume container has volumes within it.</span></span> <span data-ttu-id="89c5b-131">Lze odstranit pouze v případě, že všechny svazky, které v ní jsou nejprve odstranit.</span><span class="sxs-lookup"><span data-stu-id="89c5b-131">It can be deleted only if all the volumes contained in it are first deleted.</span></span> <span data-ttu-id="89c5b-132">Proveďte následující kroky se odstranit kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="89c5b-132">Perform the following steps to delete a volume container.</span></span>

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="89c5b-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="89c5b-133">Next steps</span></span>
* <span data-ttu-id="89c5b-134">Další informace o [správu svazků zařízení StorSimple](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="89c5b-134">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span> 
* <span data-ttu-id="89c5b-135">Další informace o [pomocí služby StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="89c5b-135">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

