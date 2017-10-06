---
title: "aaaCloud aplikace zjišťování nastavení registru pro Proxy služby | Microsoft Docs"
description: "Hello cílem tohoto tématu je tooprovide vám hello kroky je třeba, aby tooperform tooset hello požadované port na počítačích hello s Cloud App Discovery agent hello."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: bb1fe20016459160b4f67cb0125b1781a0260c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a><span data-ttu-id="f36f2-103">Cloud App Discovery. nastavení registru pro služby Proxy</span><span class="sxs-lookup"><span data-stu-id="f36f2-103">Cloud App Discovery Registry Settings for Proxy Services</span></span>
<span data-ttu-id="f36f2-104">Ve výchozím nastavení hello Cloud App Discovery agent je nakonfigurované toouse pouze hello porty 80 nebo 443.</span><span class="sxs-lookup"><span data-stu-id="f36f2-104">By default, hello Cloud App Discovery agent is configured toouse only hello ports 80 or 443.</span></span> <span data-ttu-id="f36f2-105">Pokud plánujete instalaci Cloud App Discovery v prostředí s proxy serveru, který používá vlastní port (80 ani 443), je třeba tooconfigure vaše agenty toouse tento port.</span><span class="sxs-lookup"><span data-stu-id="f36f2-105">If you are planning on installing Cloud App Discovery in an environment with a proxy server that is using a custom port (neither 80 nor 443), you need tooconfigure your agents toouse this port.</span></span> <span data-ttu-id="f36f2-106">Hello konfigurace je založená na klíč registru.</span><span class="sxs-lookup"><span data-stu-id="f36f2-106">hello configuration is based on a registry key.</span></span>

<span data-ttu-id="f36f2-107">Hello cílem tohoto tématu je tooprovide vám hello kroky je třeba, aby tooperform tooset hello požadované port na počítačích hello s Cloud App Discovery agent hello.</span><span class="sxs-lookup"><span data-stu-id="f36f2-107">hello objective of this topic is tooprovide you with hello steps you need tooperform tooset hello required port on hello computers running hello Cloud App Discovery agent.</span></span>

<span data-ttu-id="f36f2-108">**toomodify hello port používaný systémem hello počítači agenta Cloud App Discovery hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f36f2-108">**toomodify hello port used by hello computer running hello Cloud App Discovery agent, perform hello following steps:**</span></span>

1. <span data-ttu-id="f36f2-109">Spusťte editor registru hello.</span><span class="sxs-lookup"><span data-stu-id="f36f2-109">Start hello registry editor.</span></span> <br> ![Spusťte](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. <span data-ttu-id="f36f2-111">Přejděte tooor vytvořit hello následující klíč registru:</span><span class="sxs-lookup"><span data-stu-id="f36f2-111">Navigate tooor create hello following registry key:</span></span> <br> <span data-ttu-id="f36f2-112">**Discovery\Endpoint HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud aplikace**</span><span class="sxs-lookup"><span data-stu-id="f36f2-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span></span> 
3. <span data-ttu-id="f36f2-113">Vytvořte novou **víceřetězcovou** hodnotu s názvem **porty**.</span><span class="sxs-lookup"><span data-stu-id="f36f2-113">Create a new **multi-string** value called **Ports**.</span></span> <span data-ttu-id="f36f2-114">![Nový](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span><span class="sxs-lookup"><span data-stu-id="f36f2-114">![New](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span></span>
4. <span data-ttu-id="f36f2-115">tooopen hello **Upravit víceřetězcovou** dialogové okno, dvakrát klikněte na hodnotu porty hello.</span><span class="sxs-lookup"><span data-stu-id="f36f2-115">tooopen hello **Edit Multi-String** dialog, double-click hello Ports value.</span></span>
5. <span data-ttu-id="f36f2-116">V hello textové pole hodnoty dat zadejte následující hodnoty hello a přidejte všechny vlastní porty, které se používají ve vaší organizaci:</span><span class="sxs-lookup"><span data-stu-id="f36f2-116">In hello Value data textbox, type hello following values and add all custom ports that are used by your organization:</span></span> <br><br><span data-ttu-id="f36f2-117">
   **80**</span><span class="sxs-lookup"><span data-stu-id="f36f2-117">
   **80**</span></span> <br><span data-ttu-id="f36f2-118">
   **8080**</span><span class="sxs-lookup"><span data-stu-id="f36f2-118">
   **8080**</span></span> <br><span data-ttu-id="f36f2-119">
   **8118**</span><span class="sxs-lookup"><span data-stu-id="f36f2-119">
   **8118**</span></span> <br><span data-ttu-id="f36f2-120">
   **8888**</span><span class="sxs-lookup"><span data-stu-id="f36f2-120">
   **8888**</span></span> <br><span data-ttu-id="f36f2-121">
   **81**</span><span class="sxs-lookup"><span data-stu-id="f36f2-121">
   **81**</span></span> <br><span data-ttu-id="f36f2-122">
   **12080**</span><span class="sxs-lookup"><span data-stu-id="f36f2-122">
   **12080**</span></span> <br><span data-ttu-id="f36f2-123">
   **6999**</span><span class="sxs-lookup"><span data-stu-id="f36f2-123">
**6999**</span></span> <br><span data-ttu-id="f36f2-124">
**30606**</span><span class="sxs-lookup"><span data-stu-id="f36f2-124">
**30606**</span></span> <br><span data-ttu-id="f36f2-125">
**31595**</span><span class="sxs-lookup"><span data-stu-id="f36f2-125">
**31595**</span></span> <br><span data-ttu-id="f36f2-126">
**4080**</span><span class="sxs-lookup"><span data-stu-id="f36f2-126">
**4080**</span></span> <br><span data-ttu-id="f36f2-127">
**443**</span><span class="sxs-lookup"><span data-stu-id="f36f2-127">
**443**</span></span> <br><span data-ttu-id="f36f2-128">
**1110**</span><span class="sxs-lookup"><span data-stu-id="f36f2-128">
**1110**</span></span> <br><br><span data-ttu-id="f36f2-129">
![Upravit víceřetězcovou](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span><span class="sxs-lookup"><span data-stu-id="f36f2-129">
![Edit Multi-String](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span></span>
6. <span data-ttu-id="f36f2-130">Klikněte na tlačítko **OK** tooclose hello **Upravit víceřetězcovou** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f36f2-130">Click **OK** tooclose hello **Edit Multi-String** dialog.</span></span>

<span data-ttu-id="f36f2-131">**Další zdroje**</span><span class="sxs-lookup"><span data-stu-id="f36f2-131">**Additional Resources**</span></span>

* [<span data-ttu-id="f36f2-132">Jak může zjišťovat nedovolené cloudové aplikace, které se používají v rámci Moje organizace</span><span class="sxs-lookup"><span data-stu-id="f36f2-132">How can I discover unsanctioned cloud apps that are used within my organization</span></span>](active-directory-cloudappdiscovery-whatis.md) 

