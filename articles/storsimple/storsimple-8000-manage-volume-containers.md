---
title: "aaaManage vaše kontejnery svazků zařízení StorSimple na zařízení řady StorSimple 8000 hello | Microsoft Docs"
description: "Vysvětluje, jak je možné používat hello Správce zařízení StorSimple kontejnery svazků služby stránky tooadd, úpravě nebo odstranění kontejner svazků."
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
ms.openlocfilehash: 7374d4ab9aecd6280ae1d93a29f17d12d28c9362
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-volume-containers"></a><span data-ttu-id="e8a28-103">Použití služby StorSimple Manager zařízení hello, toomanage kontejnery svazků zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="e8a28-103">Use hello StorSimple Device Manager service toomanage StorSimple volume containers</span></span>

## <a name="overview"></a><span data-ttu-id="e8a28-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e8a28-104">Overview</span></span>
<span data-ttu-id="e8a28-105">Tento kurz vysvětluje, jak toouse hello toocreate service Manager zařízení StorSimple a spravovat kontejnery svazků zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="e8a28-105">This tutorial explains how toouse hello StorSimple Device Manager service toocreate and manage StorSimple volume containers.</span></span>

<span data-ttu-id="e8a28-106">Kontejner svazků v zařízení s Microsoft Azure StorSimple obsahuje jeden nebo více svazků, které sdílejí účet úložiště, šifrování a nastavení spotřeba šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="e8a28-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="e8a28-107">Zařízení může mít několik kontejnery svazků pro všechny jeho svazky.</span><span class="sxs-lookup"><span data-stu-id="e8a28-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="e8a28-108">Kontejner svazků obsahuje hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="e8a28-108">A volume container has hello following attributes:</span></span>

* <span data-ttu-id="e8a28-109">**Svazky** – hello vrstvené nebo místně vázaný svazky zařízení StorSimple, které jsou obsaženy v rámci kontejneru svazků hello.</span><span class="sxs-lookup"><span data-stu-id="e8a28-109">**Volumes** – hello tiered or locally pinned StorSimple volumes that are contained within hello volume container.</span></span> 
* <span data-ttu-id="e8a28-110">**Šifrování** – šifrovací klíč, který lze definovat pro každý kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="e8a28-110">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="e8a28-111">Tento klíč se používá pro šifrování dat hello, který se odesílá z vašeho cloudu toohello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="e8a28-111">This key is used for encrypting hello data that is sent from your StorSimple device toohello cloud.</span></span> <span data-ttu-id="e8a28-112">Klíč vojenských úrovni AES 256 bitů se používá s klíčem hello zadanou uživatelem.</span><span class="sxs-lookup"><span data-stu-id="e8a28-112">A military-grade AES-256 bit key is used with hello user-entered key.</span></span> <span data-ttu-id="e8a28-113">toosecure daty, doporučujeme vždy povolit šifrování úložiště cloudu.</span><span class="sxs-lookup"><span data-stu-id="e8a28-113">toosecure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="e8a28-114">**Účet úložiště** – hello účtu úložiště Azure, který je použité toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="e8a28-114">**Storage account** – hello Azure storage account that is used toostore hello data.</span></span> <span data-ttu-id="e8a28-115">Všechny svazky hello umístěných v kontejneru svazků sdílet tento účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8a28-115">All hello volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="e8a28-116">Můžete vybrat účet úložiště z existujícího seznamu nebo vytvořte nový účet při vytvoření kontejneru svazků hello a pak zadejte přihlašovací údaje pro přístup k hello k tomuto účtu.</span><span class="sxs-lookup"><span data-stu-id="e8a28-116">You can choose a storage account from an existing list, or create a new account when you create hello volume container and then specify hello access credentials for that account.</span></span>
* <span data-ttu-id="e8a28-117">**Cloud šířky pásma** – hello šířky pásma spotřebovávají hello zařízení při hello data ze zařízení hello je odesílána toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="e8a28-117">**Cloud bandwidth** – hello bandwidth consumed by hello device when hello data from hello device is being sent toohello cloud.</span></span> <span data-ttu-id="e8a28-118">Řízení šířky pásma můžete vynutit tak, že zadáte hodnotu v rozmezí 1 MB/s až 1 000 MB/s, při vytváření tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e8a28-118">You can enforce a bandwidth control by specifying a value between 1 Mbps and 1,000 Mbps when you create this container.</span></span> <span data-ttu-id="e8a28-119">Pokud chcete zařízení tooconsume hello celou dostupnou šířku pásma, nastavte toto pole příliš**neomezený**.</span><span class="sxs-lookup"><span data-stu-id="e8a28-119">If you want hello device tooconsume all available bandwidth, set this field too**Unlimited**.</span></span> <span data-ttu-id="e8a28-120">Můžete také vytvořit a použít šířky pásma šířky pásma šablony tooallocate podle plánu.</span><span class="sxs-lookup"><span data-stu-id="e8a28-120">You can also create and apply a bandwidth template tooallocate bandwidth based on schedule.</span></span>

<span data-ttu-id="e8a28-121">Hello následující postupy popisují, jak toouse hello StorSimple **kontejnery svazků** hello toocomplete okno následující běžné operace:</span><span class="sxs-lookup"><span data-stu-id="e8a28-121">hello following procedures explain how toouse hello StorSimple **Volume containers** blade toocomplete hello following common operations:</span></span>

* <span data-ttu-id="e8a28-122">Přidat kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="e8a28-122">Add a volume container</span></span>
* <span data-ttu-id="e8a28-123">Upravit kontejneru svazků</span><span class="sxs-lookup"><span data-stu-id="e8a28-123">Modify a volume container</span></span>
* <span data-ttu-id="e8a28-124">Odstranit kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="e8a28-124">Delete a volume container</span></span>

## <a name="add-a-volume-container"></a><span data-ttu-id="e8a28-125">Přidat kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="e8a28-125">Add a volume container</span></span>
<span data-ttu-id="e8a28-126">Proveďte následující kroky tooadd kontejner svazků hello.</span><span class="sxs-lookup"><span data-stu-id="e8a28-126">Perform hello following steps tooadd a volume container.</span></span>

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="e8a28-127">Upravit kontejneru svazků</span><span class="sxs-lookup"><span data-stu-id="e8a28-127">Modify a volume container</span></span>
<span data-ttu-id="e8a28-128">Proveďte následující kroky toomodify kontejner svazků hello.</span><span class="sxs-lookup"><span data-stu-id="e8a28-128">Perform hello following steps toomodify a volume container.</span></span>

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="e8a28-129">Odstranit kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="e8a28-129">Delete a volume container</span></span>
<span data-ttu-id="e8a28-130">Kontejner svazků obsahuje svazky v něm.</span><span class="sxs-lookup"><span data-stu-id="e8a28-130">A volume container has volumes within it.</span></span> <span data-ttu-id="e8a28-131">Lze odstranit pouze v případě, že všechny hello svazky, které v ní jsou nejprve odstranit.</span><span class="sxs-lookup"><span data-stu-id="e8a28-131">It can be deleted only if all hello volumes contained in it are first deleted.</span></span> <span data-ttu-id="e8a28-132">Proveďte následující kroky toodelete kontejner svazků hello.</span><span class="sxs-lookup"><span data-stu-id="e8a28-132">Perform hello following steps toodelete a volume container.</span></span>

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="e8a28-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e8a28-133">Next steps</span></span>
* <span data-ttu-id="e8a28-134">Další informace o [správu svazků zařízení StorSimple](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="e8a28-134">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span> 
* <span data-ttu-id="e8a28-135">Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="e8a28-135">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

