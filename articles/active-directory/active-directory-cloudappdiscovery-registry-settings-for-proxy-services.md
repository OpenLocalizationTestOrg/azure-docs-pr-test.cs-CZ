---
title: "Cloud App Discovery. nastavení registru pro Proxy služby | Microsoft Docs"
description: "Cílem tohoto tématu je poskytnout kroky, které je třeba provést nastavit požadovaný port v počítačích se systémem agenta Cloud App Discovery."
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
ms.openlocfilehash: ea15dc9a9f20a296e622c8fb1011f7ee99de3e99
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a><span data-ttu-id="faad1-103">Cloud App Discovery. nastavení registru pro služby Proxy</span><span class="sxs-lookup"><span data-stu-id="faad1-103">Cloud App Discovery Registry Settings for Proxy Services</span></span>
<span data-ttu-id="faad1-104">Ve výchozím nastavení agenta Cloud App Discovery konfigurován pro použití pouze porty 80 nebo 443.</span><span class="sxs-lookup"><span data-stu-id="faad1-104">By default, the Cloud App Discovery agent is configured to use only the ports 80 or 443.</span></span> <span data-ttu-id="faad1-105">Pokud plánujete instalaci Cloud App Discovery v prostředí s proxy serveru, který používá vlastní port (80 ani 443), budete muset nakonfigurovat agenty na tento port použít.</span><span class="sxs-lookup"><span data-stu-id="faad1-105">If you are planning on installing Cloud App Discovery in an environment with a proxy server that is using a custom port (neither 80 nor 443), you need to configure your agents to use this port.</span></span> <span data-ttu-id="faad1-106">Konfigurace je založena na klíč registru.</span><span class="sxs-lookup"><span data-stu-id="faad1-106">The configuration is based on a registry key.</span></span>

<span data-ttu-id="faad1-107">Cílem tohoto tématu je poskytnout kroky, které je třeba provést nastavit požadovaný port v počítačích se systémem agenta Cloud App Discovery.</span><span class="sxs-lookup"><span data-stu-id="faad1-107">The objective of this topic is to provide you with the steps you need to perform to set the required port on the computers running the Cloud App Discovery agent.</span></span>

<span data-ttu-id="faad1-108">**Pokud chcete upravit portu používá počítač se spuštěným agentem Cloud App Discovery, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="faad1-108">**To modify the port used by the computer running the Cloud App Discovery agent, perform the following steps:**</span></span>

1. <span data-ttu-id="faad1-109">Spusťte editor registru.</span><span class="sxs-lookup"><span data-stu-id="faad1-109">Start the registry editor.</span></span> <br> ![Spuštění](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. <span data-ttu-id="faad1-111">Přejděte na nebo vytvořte následující klíč registru:</span><span class="sxs-lookup"><span data-stu-id="faad1-111">Navigate to or create the following registry key:</span></span> <br> <span data-ttu-id="faad1-112">**Discovery\Endpoint HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud aplikace**</span><span class="sxs-lookup"><span data-stu-id="faad1-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span></span> 
3. <span data-ttu-id="faad1-113">Vytvořte novou **víceřetězcovou** hodnotu s názvem **porty**.</span><span class="sxs-lookup"><span data-stu-id="faad1-113">Create a new **multi-string** value called **Ports**.</span></span> <span data-ttu-id="faad1-114">![Nový](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span><span class="sxs-lookup"><span data-stu-id="faad1-114">![New](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span></span>
4. <span data-ttu-id="faad1-115">Chcete-li otevřít **Upravit víceřetězcovou** dialogové okno, dvakrát klikněte na hodnotu porty.</span><span class="sxs-lookup"><span data-stu-id="faad1-115">To open the **Edit Multi-String** dialog, double-click the Ports value.</span></span>
5. <span data-ttu-id="faad1-116">Do textového pole hodnoty dat zadejte následující hodnoty a přidejte všechny vlastní porty, které se používají ve vaší organizaci:</span><span class="sxs-lookup"><span data-stu-id="faad1-116">In the Value data textbox, type the following values and add all custom ports that are used by your organization:</span></span> <br><br><span data-ttu-id="faad1-117">
   **80**</span><span class="sxs-lookup"><span data-stu-id="faad1-117">
   **80**</span></span> <br><span data-ttu-id="faad1-118">
   **8080**</span><span class="sxs-lookup"><span data-stu-id="faad1-118">
   **8080**</span></span> <br><span data-ttu-id="faad1-119">
   **8118**</span><span class="sxs-lookup"><span data-stu-id="faad1-119">
   **8118**</span></span> <br><span data-ttu-id="faad1-120">
   **8888**</span><span class="sxs-lookup"><span data-stu-id="faad1-120">
   **8888**</span></span> <br><span data-ttu-id="faad1-121">
   **81**</span><span class="sxs-lookup"><span data-stu-id="faad1-121">
   **81**</span></span> <br><span data-ttu-id="faad1-122">
   **12080**</span><span class="sxs-lookup"><span data-stu-id="faad1-122">
   **12080**</span></span> <br><span data-ttu-id="faad1-123">
   **6999**</span><span class="sxs-lookup"><span data-stu-id="faad1-123">
**6999**</span></span> <br><span data-ttu-id="faad1-124">
**30606**</span><span class="sxs-lookup"><span data-stu-id="faad1-124">
**30606**</span></span> <br><span data-ttu-id="faad1-125">
**31595**</span><span class="sxs-lookup"><span data-stu-id="faad1-125">
**31595**</span></span> <br><span data-ttu-id="faad1-126">
**4080**</span><span class="sxs-lookup"><span data-stu-id="faad1-126">
**4080**</span></span> <br><span data-ttu-id="faad1-127">
**443**</span><span class="sxs-lookup"><span data-stu-id="faad1-127">
**443**</span></span> <br><span data-ttu-id="faad1-128">
**1110**</span><span class="sxs-lookup"><span data-stu-id="faad1-128">
**1110**</span></span> <br><br><span data-ttu-id="faad1-129">
![Upravit víceřetězcovou](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span><span class="sxs-lookup"><span data-stu-id="faad1-129">
![Edit Multi-String](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span></span>
6. <span data-ttu-id="faad1-130">Klikněte na tlačítko **OK** zavřete **Upravit víceřetězcovou** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="faad1-130">Click **OK** to close the **Edit Multi-String** dialog.</span></span>

<span data-ttu-id="faad1-131">**Další zdroje**</span><span class="sxs-lookup"><span data-stu-id="faad1-131">**Additional Resources**</span></span>

* [<span data-ttu-id="faad1-132">Jak může zjišťovat nedovolené cloudové aplikace, které se používají v rámci Moje organizace</span><span class="sxs-lookup"><span data-stu-id="faad1-132">How can I discover unsanctioned cloud apps that are used within my organization</span></span>](active-directory-cloudappdiscovery-whatis.md) 

