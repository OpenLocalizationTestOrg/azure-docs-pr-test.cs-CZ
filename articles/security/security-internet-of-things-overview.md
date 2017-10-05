---
title: "Zabezpečit vaše Internet věcí (IoT) v Azure | Microsoft Docs"
description: " Azure pro internet věcí (IoT) services nabízí celou řadu funkcí. Tento článek vám pomůže pochopit postup zabezpečení vašich vlastních IoT řešení v Azure. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 3793f5453b74b6c06d9e58b426d89099298e1288
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="internet-of-things-security-overview"></a><span data-ttu-id="80623-104">Přehled zabezpečení Internetu věcí</span><span class="sxs-lookup"><span data-stu-id="80623-104">Internet of Things security overview</span></span>
<span data-ttu-id="80623-105">Azure pro internet věcí (IoT) services nabízí celou řadu funkcí.</span><span class="sxs-lookup"><span data-stu-id="80623-105">Azure internet of things (IoT) services offer a broad range of capabilities.</span></span> <span data-ttu-id="80623-106">Tyto služby na úrovni řešení pro velké firmy umožňují:</span><span class="sxs-lookup"><span data-stu-id="80623-106">These enterprise grade services enable you to:</span></span>

* <span data-ttu-id="80623-107">sběr dat ze zařízení</span><span class="sxs-lookup"><span data-stu-id="80623-107">Collect data from devices</span></span>
* <span data-ttu-id="80623-108">analýzu datových proudů za provozu</span><span class="sxs-lookup"><span data-stu-id="80623-108">Analyze data streams in-motion</span></span>
* <span data-ttu-id="80623-109">ukládání objemných datových sad a dotazy na ně</span><span class="sxs-lookup"><span data-stu-id="80623-109">Store and query large data sets</span></span>
* <span data-ttu-id="80623-110">vizualizaci historických dat i dat v reálném čase</span><span class="sxs-lookup"><span data-stu-id="80623-110">Visualize both real-time and historical data</span></span>
* <span data-ttu-id="80623-111">integraci se systémy administrativní podpory (back-office)</span><span class="sxs-lookup"><span data-stu-id="80623-111">Integrate with back-office systems</span></span>

<span data-ttu-id="80623-112">K poskytování těchto možnosti, Azure IoT Suite balíčky současně více služeb Azure s vlastními rozšířeními jako předkonfigurovaná řešení.</span><span class="sxs-lookup"><span data-stu-id="80623-112">To deliver these capabilities, Azure IoT Suite packages together multiple Azure services with custom extensions as preconfigured solutions.</span></span> <span data-ttu-id="80623-113">Tato předkonfigurovaná řešení jsou základní implementací běžných vzorů řešení v oblasti IoT, které by vám měly pomoci snížit časovou náročnost implementace vašich vlastních IoT řešení.</span><span class="sxs-lookup"><span data-stu-id="80623-113">These preconfigured solutions are base implementations of common IoT solution patterns that help to reduce the time you take to deliver your IoT solutions.</span></span> <span data-ttu-id="80623-114">Pomocí software development Kit IoT, můžete přizpůsobit a rozšířit těmito řešeními, aby vyhovovala vaším požadavkům.</span><span class="sxs-lookup"><span data-stu-id="80623-114">Using the IoT software development kits, you can customize and extend these solutions to meet your own requirements.</span></span> <span data-ttu-id="80623-115">Tato řešení můžete využít i jako příklady či šablony při vývoji nových řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="80623-115">You can also use these solutions as examples or templates when you are developing new IoT solutions.</span></span>

<span data-ttu-id="80623-116">Azure IoT suite je výkonné řešení pro vaše potřeby IoT.</span><span class="sxs-lookup"><span data-stu-id="80623-116">The Azure IoT suite is a powerful solution for your IoT needs.</span></span> <span data-ttu-id="80623-117">Je však upmost význam, vašich vlastních IoT řešení jsou navržená tak, s důrazem na bezpečnost od začátku.</span><span class="sxs-lookup"><span data-stu-id="80623-117">However, it’s of upmost importance that your IoT solutions are designed with security in mind from the start.</span></span> <span data-ttu-id="80623-118">Kvůli velkému počtu zařízení IoT incidentů, zabezpečení rychle stát událost související s rozšířeným se významné důsledky.</span><span class="sxs-lookup"><span data-stu-id="80623-118">Because of the sheer number of IoT devices, any security incident can quickly become a widespread event with significant consequences.</span></span>

<span data-ttu-id="80623-119">Chcete-li vám pomohou pochopit, jak zabezpečit řešení IoT, máme následující informace.</span><span class="sxs-lookup"><span data-stu-id="80623-119">To help you understand how to secure your IoT solutions, we have the following information.</span></span>

## <a name="security-architecture"></a><span data-ttu-id="80623-120">Architektura zabezpečení</span><span class="sxs-lookup"><span data-stu-id="80623-120">Security architecture</span></span>
<span data-ttu-id="80623-121">Při návrhu systému, je důležité pochopit potenciální hrozby na daný systém a přidejte příslušné obrany podle toho, jak systém je navržená tak a navržen.</span><span class="sxs-lookup"><span data-stu-id="80623-121">When designing a system, it is important to understand the potential threats to that system, and add appropriate defenses accordingly, as the system is designed and architected.</span></span> <span data-ttu-id="80623-122">Je důležité při návrhu produktu od začátku s důrazem na bezpečnost, protože pochopení, jak může být útočník mohl ohrozit systém pomáhá, ujistěte se, zda odpovídající jejich zmírnění jsou zavedené od začátku.</span><span class="sxs-lookup"><span data-stu-id="80623-122">It is important to design the product from the start with security in mind because understanding how an attacker might be able to compromise a system helps make sure appropriate mitigations are in place from the beginning.</span></span>

<span data-ttu-id="80623-123">Můžete další informace o architektuře IoT zabezpečení načtením [Internet věcí zabezpečení architektura](../iot-suite/iot-security-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="80623-123">You can learn about IoT security architecture by reading [Internet of Things Security Architecture](../iot-suite/iot-security-architecture.md).</span></span>

<span data-ttu-id="80623-124">Tento článek popisuje v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="80623-124">This article discusses the following topics:</span></span>

* [<span data-ttu-id="80623-125">Zabezpečení začíná Model hrozeb</span><span class="sxs-lookup"><span data-stu-id="80623-125">Security Starts with a Threat Model</span></span>](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [<span data-ttu-id="80623-126">Zabezpečení v IoT</span><span class="sxs-lookup"><span data-stu-id="80623-126">Security in IoT</span></span>](../iot-suite/iot-security-architecture.md#security-in-iot)
* [<span data-ttu-id="80623-127">Azure IoT referenční architektura modelování hrozeb</span><span class="sxs-lookup"><span data-stu-id="80623-127">Threat Modeling the Azure IoT Reference Architecture</span></span>](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a><span data-ttu-id="80623-128">Zabezpečení od počátku</span><span class="sxs-lookup"><span data-stu-id="80623-128">Security from the ground up</span></span>
<span data-ttu-id="80623-129">IoT představuje jedinečné výzvy zabezpečení, ochrany osobních údajů a dodržování předpisů pro firmy po celém světě.</span><span class="sxs-lookup"><span data-stu-id="80623-129">The IoT poses unique security, privacy, and compliance challenges to businesses worldwide.</span></span> <span data-ttu-id="80623-130">Na rozdíl od tradičních internetový technologie, kde tyto problémy základem softwaru a o tom, jak je implementována IoT se vztahuje na co se stane, když internetový a fyzické světů sloučit.</span><span class="sxs-lookup"><span data-stu-id="80623-130">Unlike traditional cyber technology where these issues revolve around software and how it is implemented, IoT concerns what happens when the cyber and the physical worlds converge.</span></span> <span data-ttu-id="80623-131">Ochrana řešení IoT vyžaduje zajištění zabezpečené zřizování zařízení, zabezpečené připojení mezi tato zařízení a cloudu a ochranu zabezpečení dat v cloudu při zpracování a úložiště.</span><span class="sxs-lookup"><span data-stu-id="80623-131">Protecting IoT solutions requires ensuring secure provisioning of devices, secure connectivity between these devices and the cloud, and secure data protection in the cloud during processing and storage.</span></span> <span data-ttu-id="80623-132">Pracovat se tyto funkce, ale jsou zařízení s omezenými zdroji, geografické rozptýlení nasazení a mnoho zařízení v rámci řešení.</span><span class="sxs-lookup"><span data-stu-id="80623-132">Working against such functionality, however, are resource-constrained devices, geographic distribution of deployments, and many devices within a solution.</span></span>

<span data-ttu-id="80623-133">Můžete naučit pro zpracování zabezpečení v těchto oblastech čtení [zabezpečení Internetu věcí od základů](../iot-suite/securing-iot-ground-up.md).</span><span class="sxs-lookup"><span data-stu-id="80623-133">You can learn how to handle security in these areas by reading [Internet of Things security from the ground up](../iot-suite/securing-iot-ground-up.md).</span></span>

<span data-ttu-id="80623-134">Článek obsahuje následující témata:</span><span class="sxs-lookup"><span data-stu-id="80623-134">The article discusses the following topics:</span></span>

* [<span data-ttu-id="80623-135">Zabezpečení infrastruktury od základů</span><span class="sxs-lookup"><span data-stu-id="80623-135">Secure infrastructure from the ground up</span></span>](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [<span data-ttu-id="80623-136">Microsoft Azure – zabezpečené IoT infrastrukturu pro vaši organizaci</span><span class="sxs-lookup"><span data-stu-id="80623-136">Microsoft Azure – secure IoT infrastructure for your business</span></span>](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a><span data-ttu-id="80623-137">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="80623-137">Best Practices</span></span>
<span data-ttu-id="80623-138">Zabezpečení infrastruktury IoT vyžaduje přísných strategii zabezpečení do hloubky.</span><span class="sxs-lookup"><span data-stu-id="80623-138">Securing an IoT infrastructure requires a rigorous security-in-depth strategy.</span></span> <span data-ttu-id="80623-139">Z zabezpečení dat v cloudu, chrání integritu dat v průběhu přenosu prostřednictvím veřejného Internetu bezpečně zřízení zařízení, vytvoří každé vrstvě větší zajištění zabezpečení v celé infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="80623-139">From securing data in the cloud, protecting data integrity while in transit over the public internet, to securely provisioning devices, each layer builds greater security assurance in the overall infrastructure.</span></span>

<span data-ttu-id="80623-140">Dozvíte se o zabezpečení Internetu věcí osvědčené postupy načtením [osvědčené postupy pro zabezpečení Internetu věcí](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="80623-140">You can learn about Internet of Things security best practices by reading [Internet of Things security best practices](../iot-suite/iot-security-best-practices.md).</span></span>

<span data-ttu-id="80623-141">Článek obsahuje následující témata:</span><span class="sxs-lookup"><span data-stu-id="80623-141">The article discusses the following topics:</span></span>

* [<span data-ttu-id="80623-142">Výrobce hardwaru IoT/integrátor</span><span class="sxs-lookup"><span data-stu-id="80623-142">IoT hardware manufacturer/integrator</span></span>](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [<span data-ttu-id="80623-143">Vývojář řešení IoT</span><span class="sxs-lookup"><span data-stu-id="80623-143">IoT solution developer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [<span data-ttu-id="80623-144">Nástroje pro nasazení řešení IoT</span><span class="sxs-lookup"><span data-stu-id="80623-144">IoT solution deployer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [<span data-ttu-id="80623-145">Operátor řešení IoT</span><span class="sxs-lookup"><span data-stu-id="80623-145">IoT solution operator</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
