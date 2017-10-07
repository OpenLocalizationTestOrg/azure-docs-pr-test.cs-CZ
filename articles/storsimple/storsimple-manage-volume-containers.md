---
title: "aaaManage vaše kontejnery svazků zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak je možné používat hello StorSimple Manager kontejnery svazků služby stránky tooadd, úpravě nebo odstranění kontejner svazků."
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
ms.openlocfilehash: 9b29536e0072306e53ac92bacca78a13d932c2b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-storsimple-volume-containers"></a><span data-ttu-id="8c18c-103">Použití služby StorSimple Manager hello, toomanage kontejnery svazků zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="8c18c-103">Use hello StorSimple Manager service toomanage StorSimple volume containers</span></span>
## <a name="overview"></a><span data-ttu-id="8c18c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8c18c-104">Overview</span></span>
<span data-ttu-id="8c18c-105">Tento kurz vysvětluje, jak toouse hello toocreate služby StorSimple Manager a spravovat kontejnery svazků zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="8c18c-105">This tutorial explains how toouse hello StorSimple Manager service toocreate and manage StorSimple volume containers.</span></span>

<span data-ttu-id="8c18c-106">Kontejner svazků v zařízení s Microsoft Azure StorSimple obsahuje jeden nebo více svazků, které sdílejí účet úložiště, šifrování a nastavení spotřeba šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="8c18c-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="8c18c-107">Zařízení může mít několik kontejnery svazků pro všechny jeho svazky.</span><span class="sxs-lookup"><span data-stu-id="8c18c-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="8c18c-108">Kontejner svazků obsahuje hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="8c18c-108">A volume container has hello following attributes:</span></span>

* <span data-ttu-id="8c18c-109">**Svazky** – hello vrstvené nebo místně vázaný svazky zařízení StorSimple, které jsou obsaženy v rámci kontejneru svazků hello.</span><span class="sxs-lookup"><span data-stu-id="8c18c-109">**Volumes** – hello tiered or locally pinned StorSimple volumes that are contained within hello volume container.</span></span> <span data-ttu-id="8c18c-110">Kontejner svazků může obsahovat až too256 svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="8c18c-110">A volume container may contain up too256 StorSimple volumes.</span></span>
* <span data-ttu-id="8c18c-111">**Šifrování** – šifrovací klíč, který lze definovat pro každý kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="8c18c-111">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="8c18c-112">Tento klíč se používá pro šifrování dat hello, který se odesílá z vašeho cloudu toohello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="8c18c-112">This key is used for encrypting hello data that is sent from your StorSimple device toohello cloud.</span></span> <span data-ttu-id="8c18c-113">Klíč vojenských úrovni AES 256 bitů se používá s klíčem hello zadanou uživatelem.</span><span class="sxs-lookup"><span data-stu-id="8c18c-113">A military-grade AES-256 bit key is used with hello user-entered key.</span></span> <span data-ttu-id="8c18c-114">toosecure daty, doporučujeme vždy povolit šifrování úložiště cloudu.</span><span class="sxs-lookup"><span data-stu-id="8c18c-114">toosecure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="8c18c-115">**Účet úložiště** – hello účet úložiště, který je poskytovatele propojené tooyour cloudové úložiště služby.</span><span class="sxs-lookup"><span data-stu-id="8c18c-115">**Storage account** – hello storage account that is linked tooyour cloud storage service provider.</span></span> <span data-ttu-id="8c18c-116">Všechny svazky hello umístěných v kontejneru svazků sdílet tento účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="8c18c-116">All hello volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="8c18c-117">Můžete vybrat účet úložiště z existujícího seznamu nebo vytvořte nový účet při vytvoření kontejneru svazků hello a pak zadejte přihlašovací údaje pro přístup k hello k tomuto účtu.</span><span class="sxs-lookup"><span data-stu-id="8c18c-117">You can choose a storage account from an existing list, or create a new account when you create hello volume container and then specify hello access credentials for that account.</span></span>
* <span data-ttu-id="8c18c-118">**Cloud šířky pásma** – hello šířky pásma spotřebovávají hello zařízení při hello data ze zařízení hello je odesílána toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="8c18c-118">**Cloud bandwidth** – hello bandwidth consumed by hello device when hello data from hello device is being sent toohello cloud.</span></span> <span data-ttu-id="8c18c-119">Zadejte hodnotu mezi 1 a 1000 Mb/s při definování tento kontejner může vynutit řízení šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="8c18c-119">You can enforce a bandwidth control by specifying a value between 1 and 1000 Mbps when you define this container.</span></span> <span data-ttu-id="8c18c-120">Pokud chcete zařízení tooconsume hello celou dostupnou šířku pásma, nastavte tuto tooUnlimited pole.</span><span class="sxs-lookup"><span data-stu-id="8c18c-120">If you want hello device tooconsume all available bandwidth, set this field tooUnlimited.</span></span> <span data-ttu-id="8c18c-121">Můžete také vytvořit a použít šířky pásma šířky pásma šablony tooallocate podle plánu.</span><span class="sxs-lookup"><span data-stu-id="8c18c-121">You can also create and apply a bandwidth template tooallocate bandwidth based on schedule.</span></span>

![Stránka kontejnery svazku](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

<span data-ttu-id="8c18c-123">Tato následující postupy popisují, jak toouse hello StorSimple **kontejnery svazků** stránku hello toocomplete následující běžné operace:</span><span class="sxs-lookup"><span data-stu-id="8c18c-123">This following procedures explain how toouse hello StorSimple **Volume containers** page toocomplete hello following common operations:</span></span>

* <span data-ttu-id="8c18c-124">Přidat kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="8c18c-124">Add a volume container</span></span> 
* <span data-ttu-id="8c18c-125">Upravit kontejneru svazků</span><span class="sxs-lookup"><span data-stu-id="8c18c-125">Modify a volume container</span></span> 
* <span data-ttu-id="8c18c-126">Odstranit kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="8c18c-126">Delete a volume container</span></span> 

## <a name="add-a-volume-container"></a><span data-ttu-id="8c18c-127">Přidat kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="8c18c-127">Add a volume container</span></span>
<span data-ttu-id="8c18c-128">Proveďte následující kroky tooadd kontejner svazků hello.</span><span class="sxs-lookup"><span data-stu-id="8c18c-128">Perform hello following steps tooadd a volume container.</span></span>

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="8c18c-129">Upravit kontejneru svazků</span><span class="sxs-lookup"><span data-stu-id="8c18c-129">Modify a volume container</span></span>
<span data-ttu-id="8c18c-130">Proveďte následující kroky toomodify kontejner svazků hello.</span><span class="sxs-lookup"><span data-stu-id="8c18c-130">Perform hello following steps toomodify a volume container.</span></span>

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="8c18c-131">Odstranit kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="8c18c-131">Delete a volume container</span></span>
<span data-ttu-id="8c18c-132">Kontejner svazků obsahuje svazky v něm.</span><span class="sxs-lookup"><span data-stu-id="8c18c-132">A volume container has volumes within it.</span></span> <span data-ttu-id="8c18c-133">Lze odstranit pouze v případě, že všechny hello svazky, které v ní jsou nejprve odstranit.</span><span class="sxs-lookup"><span data-stu-id="8c18c-133">It can be deleted only if all hello volumes contained in it are first deleted.</span></span> <span data-ttu-id="8c18c-134">Proveďte následující kroky toodelete kontejner svazků hello.</span><span class="sxs-lookup"><span data-stu-id="8c18c-134">Perform hello following steps toodelete a volume container.</span></span>

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="8c18c-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c18c-135">Next steps</span></span>
* <span data-ttu-id="8c18c-136">Další informace o [správu svazků zařízení StorSimple](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="8c18c-136">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span> 
* <span data-ttu-id="8c18c-137">Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="8c18c-137">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

