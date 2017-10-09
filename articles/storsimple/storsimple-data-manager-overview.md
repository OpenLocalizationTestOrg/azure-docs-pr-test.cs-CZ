---
title: "Přehled Azure StorSimple Data Manager aaaMicrosoft | Microsoft Docs"
description: "Poskytuje přehled hello služby StorSimple Manager dat (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 5d29f7d26be9f2b36857526bdea770d991cece6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a><span data-ttu-id="2def9-103">Přehled Data Manager zařízení StorSimple (soukromém náhledu).</span><span class="sxs-lookup"><span data-stu-id="2def9-103">StorSimple Data Manager overview (Private Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="2def9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2def9-104">Overview</span></span>

<span data-ttu-id="2def9-105">Microsoft Azure StorSimple je řešení hybridní cloudové úložiště, které adresy hello složitosti nestrukturovaných dat běžně spojovaných se sdílenými složkami.</span><span class="sxs-lookup"><span data-stu-id="2def9-105">Microsoft Azure StorSimple is a hybrid cloud storage solution that addresses hello complexities of unstructured data commonly associated with file shares.</span></span> <span data-ttu-id="2def9-106">StorSimple používá úložiště v cloudu jako rozšíření hello místní řešení a automaticky úrovně dat napříč hello místní úložiště a úložiště v cloudu.</span><span class="sxs-lookup"><span data-stu-id="2def9-106">StorSimple uses cloud storage as an extension of hello on-premises solution and automatically tiers data across hello on-premises storage and cloud storage.</span></span> <span data-ttu-id="2def9-107">Integrovat ochranu dat, místní a cloudových snímků, eliminuje nutnost hello rozrůstající infrastruktury úložiště.</span><span class="sxs-lookup"><span data-stu-id="2def9-107">Integrated data protection, with local and cloud snapshots, eliminates hello need for a sprawling storage infrastructure.</span></span> <span data-ttu-id="2def9-108">Archivace a zotavení po havárii je také bezproblémové hello cloudu, který funguje jako mimo pracoviště.</span><span class="sxs-lookup"><span data-stu-id="2def9-108">Archival and disaster recovery is also seamless with hello cloud acting as an offsite location.</span></span>

<span data-ttu-id="2def9-109">Hello služba transformace dat, která Představujeme v tomto dokumentu, umožní vám tooseamlessly přístup hello StorSimple dat v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="2def9-109">hello data transformation service that we are introducing in this document, allows you tooseamlessly access hello StorSimple data in hello cloud.</span></span> <span data-ttu-id="2def9-110">Tato služba poskytuje rozhraní API tooextract data ze zařízení StorSimple a je k dispozici tooother Azure services v formáty, které můžete snadno využívat.</span><span class="sxs-lookup"><span data-stu-id="2def9-110">This service provides APIs tooextract data from StorSimple and present it tooother Azure services in formats that can be readily consumed.</span></span> <span data-ttu-id="2def9-111">Hello formátů podporovaných v této verzi preview jsou Azure BLOB a prostředky Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="2def9-111">hello formats supported in this preview are Azure blobs and Azure Media Services assets.</span></span> <span data-ttu-id="2def9-112">Tato transformace umožňuje vám tooeasily přenosová až službám, jako je Azure Media Services, Azure HDInsight, Azure Machine Learning a Azure Search toooperate data na místní zařízení řady StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="2def9-112">This transformation enables you tooeasily wire up services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search toooperate data on StorSimple 8000 series on-premises device.</span></span>

<span data-ttu-id="2def9-113">Podrobný Blokový diagram ilustrující to jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="2def9-113">A high-level block diagram illustrating this is shown below.</span></span>

![Vysokoúrovňový diagram](./media//storsimple-data-manager-overview/high-level-diagram.png)

<span data-ttu-id="2def9-115">Tento dokument popisuje, jak zaregistrovat privátní Preview verzi této služby.</span><span class="sxs-lookup"><span data-stu-id="2def9-115">This document explains how you can sign up for a private preview of this service.</span></span> <span data-ttu-id="2def9-116">Také vysvětluje, jak lze pomocí této aplikace toowrite služby, které používají StorSimple data a jinými službami Azure v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="2def9-116">It also explains how you can use this service toowrite applications that use StorSimple data and other Azure services in hello cloud.</span></span>

## <a name="sign-up-for-data-manager-preview"></a><span data-ttu-id="2def9-117">Zaregistrujte si Data Manager preview</span><span class="sxs-lookup"><span data-stu-id="2def9-117">Sign up for Data Manager preview</span></span>
<span data-ttu-id="2def9-118">Ještě než si zaregistrujete služby hello Data Manager, zkontrolujte hello následující požadavky.</span><span class="sxs-lookup"><span data-stu-id="2def9-118">Before you sign up for hello Data Manager service, review hello following prerequisites.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="2def9-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2def9-119">Prerequisites</span></span>

<span data-ttu-id="2def9-120">Toto cvičení předpokládá, že máte</span><span class="sxs-lookup"><span data-stu-id="2def9-120">This exercise assumes that you have</span></span>
* <span data-ttu-id="2def9-121">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2def9-121">an active Azure subscription.</span></span>
* <span data-ttu-id="2def9-122">zaregistrované zařízení řady StorSimple 8000 tooa přístup</span><span class="sxs-lookup"><span data-stu-id="2def9-122">access tooa registered StorSimple 8000 series device</span></span>
* <span data-ttu-id="2def9-123">všechny hello klíče přidružené zařízení řady StorSimple 8000 hello.</span><span class="sxs-lookup"><span data-stu-id="2def9-123">all hello keys associated with hello StorSimple 8000 series device.</span></span>

### <a name="sign-up"></a><span data-ttu-id="2def9-124">Registrace</span><span class="sxs-lookup"><span data-stu-id="2def9-124">Sign up</span></span>

<span data-ttu-id="2def9-125">Hello StorSimple Data Manager je v privátní Preview verzi.</span><span class="sxs-lookup"><span data-stu-id="2def9-125">hello StorSimple Data Manager is in private preview.</span></span> <span data-ttu-id="2def9-126">Proveďte následující kroky toosign pro privátní Preview verzi této služby hello:</span><span class="sxs-lookup"><span data-stu-id="2def9-126">Perform hello following steps toosign up for a private preview of this service:</span></span>

1.  <span data-ttu-id="2def9-127">Přihlaste se k hello portál Azure s příponou hello StorSimple Manager dat na: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager).</span><span class="sxs-lookup"><span data-stu-id="2def9-127">Log into hello Azure portal with hello StorSimple Data Manager extension at: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager).</span></span> <span data-ttu-id="2def9-128">Použijte toolog vaše přihlašovací údaje Azure v.</span><span class="sxs-lookup"><span data-stu-id="2def9-128">Use your Azure credentials toolog in.</span></span>

2.  <span data-ttu-id="2def9-129">Klikněte na tlačítko hello  **+**  toocreate ikonu služby.</span><span class="sxs-lookup"><span data-stu-id="2def9-129">Click hello **+** icon toocreate a service.</span></span> <span data-ttu-id="2def9-130">Klikněte na tlačítko **úložiště** a pak klikněte na **najdete v článku všechny** v okně hello, které se otevře.</span><span class="sxs-lookup"><span data-stu-id="2def9-130">Click **Storage** and then click **See All** in hello blade that opens up.</span></span>

    ![Ikona Data Manager StorSimple vyhledávání](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. <span data-ttu-id="2def9-132">Zobrazí ikona hello StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="2def9-132">You see hello StorSimple Data Manager icon.</span></span>

    ![Vyberte ikonu pro StorSimple Data Manager](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. <span data-ttu-id="2def9-134">Klikněte na ikonu StorSimple Manager dat a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2def9-134">Click StorSimple Data Manager icon and then click **Create**.</span></span> <span data-ttu-id="2def9-135">Vyberte předplatné hello má tooenable hello privátní Preview verzi a potom klikněte na **zapsat se!**</span><span class="sxs-lookup"><span data-stu-id="2def9-135">Pick hello subscription that you want tooenable for hello private preview and then click **Sign me up!**</span></span>

    ![Zapsat se](./media/storsimple-data-manager-overview/sign-me-up.png)

5. <span data-ttu-id="2def9-137">Tím se odešle požadavek tooonboard můžete.</span><span class="sxs-lookup"><span data-stu-id="2def9-137">This sends a request tooonboard you.</span></span> <span data-ttu-id="2def9-138">Nemůžeme se zaváděním je co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="2def9-138">We will onboard you as soon as possible.</span></span> <span data-ttu-id="2def9-139">Když je vaše předplatné povolená, můžete vytvořit služby StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="2def9-139">After your subscription is enabled, you can create a StorSimple Data Manager service.</span></span>

6. <span data-ttu-id="2def9-140">tooeasily přístup služby StorSimple Manager dat hello, klikněte na ikonu hvězdičky toopin hello ho tooyour Oblíbené položky.</span><span class="sxs-lookup"><span data-stu-id="2def9-140">tooeasily access hello StorSimple Data Manager service, click hello star icon toopin it tooyour favorites.</span></span>

    ![Správci StorSimple Data Access](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a><span data-ttu-id="2def9-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2def9-142">Next steps</span></span>

<span data-ttu-id="2def9-143">[Pomocí uživatelského rozhraní Správce dat StorSimple tootransform dat](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="2def9-143">[Use StorSimple Data Manager UI tootransform your data](storsimple-data-manager-ui.md).</span></span>
