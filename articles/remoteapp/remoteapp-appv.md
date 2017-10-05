---
title: "Pomocí aplikace App-V s Azure Remoteappem | Microsoft Docs"
description: "Další informace o použití aplikace App-V v Azure Remoteappu."
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
ms.openlocfilehash: e55bb8db83c04025c46b383a9ebbef4399178116
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a><span data-ttu-id="2fc37-103">Pomocí aplikace App-V v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="2fc37-103">Using App-V apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2fc37-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="2fc37-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="2fc37-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="2fc37-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="2fc37-106">Aplikace App-V můžete použít v hybridní kolekce Azure Remoteappu, který vyžaduje připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="2fc37-106">You can use App-V applications in a Azure RemoteApp hybrid collection, which requires domain join.</span></span>

<span data-ttu-id="2fc37-107">Než začnete, ujistěte se, zda je instalace klienta App-V 5.1 pomocí nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2fc37-107">Before you get started, make sure to install the App-V 5.1 client with the latest updates.</span></span> <span data-ttu-id="2fc37-108">Budete muset vytvořit [vlastní image](remoteapp-create-custom-image.md) , který obsahuje klienta sady App-V.</span><span class="sxs-lookup"><span data-stu-id="2fc37-108">You will need to create a [custom image](remoteapp-create-custom-image.md) that includes the App-V client.</span></span>  

<span data-ttu-id="2fc37-109">Je snadné použití existující infrastruktury sady App-V s Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="2fc37-109">It’s easy to use your existing App-V infrastructure with Azure RemoteApp.</span></span> <span data-ttu-id="2fc37-110">Vzhledem k tomu, že hybridní kolekce je nasazený do virtuální sítě Azure, který má přístup k řadiči domény a virtuální počítače jsou připojené k doméně, můžete využít vaší existující sady App-v infrastruktury a nasazení metody easyily hostitele aplikace App-V v Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="2fc37-110">Since a hybrid collection is deployed into an Azure VNET that has access to your domain controller and the VMs are domain joined, you can leverage your existing App-v infrastructure and deployment methods to easyily host App-V application in Azure RemoteApp.</span></span> <span data-ttu-id="2fc37-111">Zde jsou některé aspekty, které byste měli vědět, na základě typu nasazení App-V, které aktuálně máte:</span><span class="sxs-lookup"><span data-stu-id="2fc37-111">Here are some considerations that you should be aware of based on the type of App-V deployment you currently have:</span></span>

| <span data-ttu-id="2fc37-112">Možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="2fc37-112">Configuration options</span></span> |  | <span data-ttu-id="2fc37-113">Kladné</span><span class="sxs-lookup"><span data-stu-id="2fc37-113">Positive</span></span> | <span data-ttu-id="2fc37-114">Záporná</span><span class="sxs-lookup"><span data-stu-id="2fc37-114">Negative</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2fc37-115">Metoda doručování</span><span class="sxs-lookup"><span data-stu-id="2fc37-115">Delivery method</span></span> |<span data-ttu-id="2fc37-116">Streamování na vyžádání)</span><span class="sxs-lookup"><span data-stu-id="2fc37-116">Streaming (on-demand)</span></span> |<span data-ttu-id="2fc37-117">Aplikace je vždy nejnovější a čerstvou</span><span class="sxs-lookup"><span data-stu-id="2fc37-117">App is always the latest and fresh</span></span> |<span data-ttu-id="2fc37-118">První časovou náročnost</span><span class="sxs-lookup"><span data-stu-id="2fc37-118">First time latency</span></span> |
| <span data-ttu-id="2fc37-119">Připojit</span><span class="sxs-lookup"><span data-stu-id="2fc37-119">Mounted</span></span> |<span data-ttu-id="2fc37-120">Nejrychlejší; aplikace se již nachází ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="2fc37-120">Fastest; app is already present on the VM</span></span> |<span data-ttu-id="2fc37-121">Tomu - trvá místo bitové kopie (limit 127 GB)</span><span class="sxs-lookup"><span data-stu-id="2fc37-121">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="2fc37-122">Aplikace umístění úložiště</span><span class="sxs-lookup"><span data-stu-id="2fc37-122">App location storage</span></span> |<span data-ttu-id="2fc37-123">Sdílený obsah</span><span class="sxs-lookup"><span data-stu-id="2fc37-123">Shared content</span></span> |<span data-ttu-id="2fc37-124">Spuštění aplikace v paměti instance Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="2fc37-124">App runs in memory of Azure RemoteApp instance</span></span> |<span data-ttu-id="2fc37-125">Eats paměti a dobré připojení k vysílání datového proudu (soubor) serveru, kde se nachází aplikace</span><span class="sxs-lookup"><span data-stu-id="2fc37-125">Eats memory and good connection to streaming (file) server where the app resides</span></span> |
| <span data-ttu-id="2fc37-126">Disk (v mezipaměti)</span><span class="sxs-lookup"><span data-stu-id="2fc37-126">Disk (Cached)</span></span> |<span data-ttu-id="2fc37-127">Rychlé spuštění.</span><span class="sxs-lookup"><span data-stu-id="2fc37-127">Fast execution.</span></span> <span data-ttu-id="2fc37-128">Aplikace není závislý na dostupnosti zdroje obsahu</span><span class="sxs-lookup"><span data-stu-id="2fc37-128">App not dependent on availability of Content Source</span></span> |<span data-ttu-id="2fc37-129">Tomu - trvá místo bitové kopie (limit 127 GB)</span><span class="sxs-lookup"><span data-stu-id="2fc37-129">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="2fc37-130">Cílení na</span><span class="sxs-lookup"><span data-stu-id="2fc37-130">Targeting</span></span> |<span data-ttu-id="2fc37-131">Uživatel</span><span class="sxs-lookup"><span data-stu-id="2fc37-131">User</span></span> |<span data-ttu-id="2fc37-132">Vyžaduje infrastrukturu úplné samostatné sady App-V</span><span class="sxs-lookup"><span data-stu-id="2fc37-132">Requires full standalone App-V infrastructure</span></span> | |
| <span data-ttu-id="2fc37-133">Globální (počítače)</span><span class="sxs-lookup"><span data-stu-id="2fc37-133">Global (machine)</span></span> |<span data-ttu-id="2fc37-134">Předem publikovat nebo cílení pomocí publikování serveru</span><span class="sxs-lookup"><span data-stu-id="2fc37-134">Pre-publish or target using Publishing server</span></span> |<span data-ttu-id="2fc37-135">Potřeba aktualizovat Azure image, pokud chcete aktualizovat aplikaci (velký).</span><span class="sxs-lookup"><span data-stu-id="2fc37-135">Need to update your Azure image if you want to update the app (huge).</span></span> <span data-ttu-id="2fc37-136">Trvá místo na bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="2fc37-136">Takes up some space on image.</span></span> | |

 <span data-ttu-id="2fc37-137">Jakmile vytvoříte vlastní bitovou kopii a hybridní kolekci, publikování aplikace, přiřazení uživatelů a získejte vaší stávající aplikace App-V, které jsou hostované v Azure Remoteappu doručí na jakémkoliv zařízení a odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="2fc37-137">After you create your custom image and your hybrid collection, publish your application, assign users and enjoy your existing App-V applications hosted in Azure RemoteApp delivered to any device anywhere.</span></span>

