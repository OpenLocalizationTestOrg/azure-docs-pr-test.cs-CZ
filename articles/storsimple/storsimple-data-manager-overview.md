---
title: "Přehled Microsoft Azure StorSimple Data Manager | Microsoft Docs"
description: "Obsahuje přehled služby StorSimple Manager dat (soukromém náhledu)."
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
ms.openlocfilehash: aedb44610fe57055851538b9dbdb810e66e58d73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a><span data-ttu-id="b6d09-103">Přehled Data Manager zařízení StorSimple (soukromém náhledu).</span><span class="sxs-lookup"><span data-stu-id="b6d09-103">StorSimple Data Manager overview (Private Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="b6d09-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b6d09-104">Overview</span></span>

<span data-ttu-id="b6d09-105">Microsoft Azure StorSimple je řešení hybridní cloudové úložiště, které řeší složitosti nestrukturovaných dat běžně spojovaných se sdílenými složkami.</span><span class="sxs-lookup"><span data-stu-id="b6d09-105">Microsoft Azure StorSimple is a hybrid cloud storage solution that addresses the complexities of unstructured data commonly associated with file shares.</span></span> <span data-ttu-id="b6d09-106">StorSimple používá úložiště v cloudu jako rozšíření v případě místních řešení a automaticky úrovně dat mezi místní úložiště a úložiště v cloudu.</span><span class="sxs-lookup"><span data-stu-id="b6d09-106">StorSimple uses cloud storage as an extension of the on-premises solution and automatically tiers data across the on-premises storage and cloud storage.</span></span> <span data-ttu-id="b6d09-107">Integrovat ochranu dat, místní a cloudových snímků, eliminuje potřebu rozrůstající infrastruktury úložiště.</span><span class="sxs-lookup"><span data-stu-id="b6d09-107">Integrated data protection, with local and cloud snapshots, eliminates the need for a sprawling storage infrastructure.</span></span> <span data-ttu-id="b6d09-108">Archivace a zotavení po havárii je také bezproblémové s cloudem, který funguje jako mimo pracoviště.</span><span class="sxs-lookup"><span data-stu-id="b6d09-108">Archival and disaster recovery is also seamless with the cloud acting as an offsite location.</span></span>

<span data-ttu-id="b6d09-109">Služba transformace dat, která Představujeme v tomto dokumentu, můžete bezproblémově přistupovat k StorSimple dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="b6d09-109">The data transformation service that we are introducing in this document, allows you to seamlessly access the StorSimple data in the cloud.</span></span> <span data-ttu-id="b6d09-110">Tato služba poskytuje rozhraní API extrahovat data z StorSimple a prezentovat si ho k jiným službám Azure ve formátech, můžete snadno využívat.</span><span class="sxs-lookup"><span data-stu-id="b6d09-110">This service provides APIs to extract data from StorSimple and present it to other Azure services in formats that can be readily consumed.</span></span> <span data-ttu-id="b6d09-111">Formátů podporovaných v této verzi preview jsou Azure BLOB a prostředky Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="b6d09-111">The formats supported in this preview are Azure blobs and Azure Media Services assets.</span></span> <span data-ttu-id="b6d09-112">Tato transformace umožňuje snadno propojit až službám, jako je Azure Media Services, Azure HDInsight, Azure Machine Learning a Azure Search fungovat data na místní zařízení řady StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="b6d09-112">This transformation enables you to easily wire up services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search to operate data on StorSimple 8000 series on-premises device.</span></span>

<span data-ttu-id="b6d09-113">Podrobný Blokový diagram ilustrující to jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="b6d09-113">A high-level block diagram illustrating this is shown below.</span></span>

![Vysokoúrovňový diagram](./media//storsimple-data-manager-overview/high-level-diagram.png)

<span data-ttu-id="b6d09-115">Tento dokument popisuje, jak zaregistrovat privátní Preview verzi této služby.</span><span class="sxs-lookup"><span data-stu-id="b6d09-115">This document explains how you can sign up for a private preview of this service.</span></span> <span data-ttu-id="b6d09-116">Také vysvětluje, jak lze pomocí této služby pro psaní aplikací, které používají StorSimple data a jinými službami Azure v cloudu.</span><span class="sxs-lookup"><span data-stu-id="b6d09-116">It also explains how you can use this service to write applications that use StorSimple data and other Azure services in the cloud.</span></span>

## <a name="sign-up-for-data-manager-preview"></a><span data-ttu-id="b6d09-117">Zaregistrujte si Data Manager preview</span><span class="sxs-lookup"><span data-stu-id="b6d09-117">Sign up for Data Manager preview</span></span>
<span data-ttu-id="b6d09-118">Ještě než si zaregistrujete do služby Data Manager, přečtěte si následující požadavky.</span><span class="sxs-lookup"><span data-stu-id="b6d09-118">Before you sign up for the Data Manager service, review the following prerequisites.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b6d09-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b6d09-119">Prerequisites</span></span>

<span data-ttu-id="b6d09-120">Toto cvičení předpokládá, že máte</span><span class="sxs-lookup"><span data-stu-id="b6d09-120">This exercise assumes that you have</span></span>
* <span data-ttu-id="b6d09-121">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b6d09-121">an active Azure subscription.</span></span>
* <span data-ttu-id="b6d09-122">přístup k registrované zařízení StorSimple 8000 řady zařízení</span><span class="sxs-lookup"><span data-stu-id="b6d09-122">access to a registered StorSimple 8000 series device</span></span>
* <span data-ttu-id="b6d09-123">všechny klíče přidružené k zařízení řady StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="b6d09-123">all the keys associated with the StorSimple 8000 series device.</span></span>

### <a name="sign-up"></a><span data-ttu-id="b6d09-124">Registrace</span><span class="sxs-lookup"><span data-stu-id="b6d09-124">Sign up</span></span>

<span data-ttu-id="b6d09-125">Data Manager zařízení StorSimple je v privátní Preview verzi.</span><span class="sxs-lookup"><span data-stu-id="b6d09-125">The StorSimple Data Manager is in private preview.</span></span> <span data-ttu-id="b6d09-126">Proveďte následující kroky se zaregistrovat v privátní Preview verzi této služby:</span><span class="sxs-lookup"><span data-stu-id="b6d09-126">Perform the following steps to sign up for a private preview of this service:</span></span>

1.  <span data-ttu-id="b6d09-127">Přihlaste se k portálu Azure s příponou StorSimple Manager dat na: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager).</span><span class="sxs-lookup"><span data-stu-id="b6d09-127">Log into the Azure portal with the StorSimple Data Manager extension at: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager).</span></span> <span data-ttu-id="b6d09-128">Přihlaste se pomocí přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="b6d09-128">Use your Azure credentials to log in.</span></span>

2.  <span data-ttu-id="b6d09-129">Klikněte  **+**  ikona vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="b6d09-129">Click the **+** icon to create a service.</span></span> <span data-ttu-id="b6d09-130">Klikněte na tlačítko **úložiště** a pak klikněte na **najdete v článku všechny** v okně, které se otevře.</span><span class="sxs-lookup"><span data-stu-id="b6d09-130">Click **Storage** and then click **See All** in the blade that opens up.</span></span>

    ![Ikona Data Manager StorSimple vyhledávání](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. <span data-ttu-id="b6d09-132">Zobrazí ikona StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="b6d09-132">You see the StorSimple Data Manager icon.</span></span>

    ![Vyberte ikonu pro StorSimple Data Manager](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. <span data-ttu-id="b6d09-134">Klikněte na ikonu StorSimple Manager dat a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b6d09-134">Click StorSimple Data Manager icon and then click **Create**.</span></span> <span data-ttu-id="b6d09-135">Vyberte předplatné, které chcete povolit privátní Preview verzi a potom klikněte na **zapsat se!**</span><span class="sxs-lookup"><span data-stu-id="b6d09-135">Pick the subscription that you want to enable for the private preview and then click **Sign me up!**</span></span>

    ![Zapsat se](./media/storsimple-data-manager-overview/sign-me-up.png)

5. <span data-ttu-id="b6d09-137">Tím se odešle požadavek na základní desce můžete.</span><span class="sxs-lookup"><span data-stu-id="b6d09-137">This sends a request to onboard you.</span></span> <span data-ttu-id="b6d09-138">Nemůžeme se zaváděním je co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="b6d09-138">We will onboard you as soon as possible.</span></span> <span data-ttu-id="b6d09-139">Když je vaše předplatné povolená, můžete vytvořit služby StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="b6d09-139">After your subscription is enabled, you can create a StorSimple Data Manager service.</span></span>

6. <span data-ttu-id="b6d09-140">Snadno přístup ke službě StorSimple Manager dat, klikněte na ikonu hvězdičky. Chcete-li připnout do vašich oblíbených položek.</span><span class="sxs-lookup"><span data-stu-id="b6d09-140">To easily access the StorSimple Data Manager service, click the star icon to pin it to your favorites.</span></span>

    ![Správci StorSimple Data Access](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a><span data-ttu-id="b6d09-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6d09-142">Next steps</span></span>

<span data-ttu-id="b6d09-143">[Data Manager zařízení StorSimple pomocí uživatelského rozhraní pro transformaci dat](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="b6d09-143">[Use StorSimple Data Manager UI to transform your data](storsimple-data-manager-ui.md).</span></span>