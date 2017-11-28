---
title: "aaaUsing App-V aplikací s Azure Remoteappem | Microsoft Docs"
description: "Zjistěte, jak aplikace toouse App-V v Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e2292cb2-5c89-4b2b-ab11-74dbacd07c31
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9cf5c2eeee2a0ce2cf98e1cf6497dffbc6eff016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a><span data-ttu-id="54586-103">Pomocí aplikace App-V v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="54586-103">Using App-V apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="54586-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="54586-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="54586-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="54586-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="54586-106">Aplikace App-V můžete použít v hybridní kolekce Azure Remoteappu, který vyžaduje připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="54586-106">You can use App-V applications in a Azure RemoteApp hybrid collection, which requires domain join.</span></span>

<span data-ttu-id="54586-107">Než začnete, ujistěte se, že klient App-V 5.1 hello tooinstall s nejnovějšími aktualizacemi hello.</span><span class="sxs-lookup"><span data-stu-id="54586-107">Before you get started, make sure tooinstall hello App-V 5.1 client with hello latest updates.</span></span> <span data-ttu-id="54586-108">Budete potřebovat toocreate [vlastní image](remoteapp-create-custom-image.md) , zahrnuje client hello App-V.</span><span class="sxs-lookup"><span data-stu-id="54586-108">You will need toocreate a [custom image](remoteapp-create-custom-image.md) that includes hello App-V client.</span></span>  

<span data-ttu-id="54586-109">Je snadno toouse stávající infrastruktury sady App-V s Azure Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="54586-109">It’s easy toouse your existing App-V infrastructure with Azure RemoteApp.</span></span> <span data-ttu-id="54586-110">Vzhledem k tomu, že hybridní kolekce je nasazený do virtuální sítě Azure, který má přístup k řadiči domény tooyour a hello virtuální počítače jsou připojené k doméně, můžete využít stávající App-v infrastruktury a nasazení metody tooeasyily hostitele App-V aplikaci v Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="54586-110">Since a hybrid collection is deployed into an Azure VNET that has access tooyour domain controller and hello VMs are domain joined, you can leverage your existing App-v infrastructure and deployment methods tooeasyily host App-V application in Azure RemoteApp.</span></span> <span data-ttu-id="54586-111">Zde jsou některé aspekty, které byste měli vědět, na základě typu nasazení App-V, které máte aktuálně hello:</span><span class="sxs-lookup"><span data-stu-id="54586-111">Here are some considerations that you should be aware of based on hello type of App-V deployment you currently have:</span></span>

| <span data-ttu-id="54586-112">Možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="54586-112">Configuration options</span></span> |  | <span data-ttu-id="54586-113">Kladné</span><span class="sxs-lookup"><span data-stu-id="54586-113">Positive</span></span> | <span data-ttu-id="54586-114">Záporná</span><span class="sxs-lookup"><span data-stu-id="54586-114">Negative</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="54586-115">Metoda doručování</span><span class="sxs-lookup"><span data-stu-id="54586-115">Delivery method</span></span> |<span data-ttu-id="54586-116">Streamování na vyžádání)</span><span class="sxs-lookup"><span data-stu-id="54586-116">Streaming (on-demand)</span></span> |<span data-ttu-id="54586-117">Aplikace je vždy hello nejnovější a připravený</span><span class="sxs-lookup"><span data-stu-id="54586-117">App is always hello latest and fresh</span></span> |<span data-ttu-id="54586-118">První časovou náročnost</span><span class="sxs-lookup"><span data-stu-id="54586-118">First time latency</span></span> |
| <span data-ttu-id="54586-119">Připojit</span><span class="sxs-lookup"><span data-stu-id="54586-119">Mounted</span></span> |<span data-ttu-id="54586-120">Nejrychlejší; aplikace se již nachází na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="54586-120">Fastest; app is already present on hello VM</span></span> |<span data-ttu-id="54586-121">Tomu - trvá místo bitové kopie (limit 127 GB)</span><span class="sxs-lookup"><span data-stu-id="54586-121">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="54586-122">Aplikace umístění úložiště</span><span class="sxs-lookup"><span data-stu-id="54586-122">App location storage</span></span> |<span data-ttu-id="54586-123">Sdílený obsah</span><span class="sxs-lookup"><span data-stu-id="54586-123">Shared content</span></span> |<span data-ttu-id="54586-124">Spuštění aplikace v paměti instance Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="54586-124">App runs in memory of Azure RemoteApp instance</span></span> |<span data-ttu-id="54586-125">Eats paměť a správné připojení toostreaming (soubor) serveru, kde se nachází aplikace hello</span><span class="sxs-lookup"><span data-stu-id="54586-125">Eats memory and good connection toostreaming (file) server where hello app resides</span></span> |
| <span data-ttu-id="54586-126">Disk (v mezipaměti)</span><span class="sxs-lookup"><span data-stu-id="54586-126">Disk (Cached)</span></span> |<span data-ttu-id="54586-127">Rychlé spuštění.</span><span class="sxs-lookup"><span data-stu-id="54586-127">Fast execution.</span></span> <span data-ttu-id="54586-128">Aplikace není závislý na dostupnosti zdroje obsahu</span><span class="sxs-lookup"><span data-stu-id="54586-128">App not dependent on availability of Content Source</span></span> |<span data-ttu-id="54586-129">Tomu - trvá místo bitové kopie (limit 127 GB)</span><span class="sxs-lookup"><span data-stu-id="54586-129">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="54586-130">Cílení na</span><span class="sxs-lookup"><span data-stu-id="54586-130">Targeting</span></span> |<span data-ttu-id="54586-131">Uživatel</span><span class="sxs-lookup"><span data-stu-id="54586-131">User</span></span> |<span data-ttu-id="54586-132">Vyžaduje infrastrukturu úplné samostatné sady App-V</span><span class="sxs-lookup"><span data-stu-id="54586-132">Requires full standalone App-V infrastructure</span></span> | |
| <span data-ttu-id="54586-133">Globální (počítače)</span><span class="sxs-lookup"><span data-stu-id="54586-133">Global (machine)</span></span> |<span data-ttu-id="54586-134">Předem publikovat nebo cílení pomocí publikování serveru</span><span class="sxs-lookup"><span data-stu-id="54586-134">Pre-publish or target using Publishing server</span></span> |<span data-ttu-id="54586-135">Třeba tooupdate Azure image, pokud chcete aplikace hello tooupdate (velký).</span><span class="sxs-lookup"><span data-stu-id="54586-135">Need tooupdate your Azure image if you want tooupdate hello app (huge).</span></span> <span data-ttu-id="54586-136">Trvá místo na bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="54586-136">Takes up some space on image.</span></span> | |

 <span data-ttu-id="54586-137">Jakmile vytvoříte vlastní bitovou kopii a hybridní kolekci, publikování aplikace, přiřazení uživatelů a získejte vaší stávající aplikace App-V, které jsou hostované v Azure Remoteappu doručit tooany zařízení a odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="54586-137">After you create your custom image and your hybrid collection, publish your application, assign users and enjoy your existing App-V applications hosted in Azure RemoteApp delivered tooany device anywhere.</span></span>

