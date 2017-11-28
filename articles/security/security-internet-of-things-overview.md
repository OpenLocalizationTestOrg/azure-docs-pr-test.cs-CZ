---
title: "aaaSecure vaše Internet věcí (IoT) v Azure | Microsoft Docs"
description: " Azure pro internet věcí (IoT) services nabízí celou řadu funkcí. Tento článek vám pomůže pochopit jak toosecure řešení IoT v Azure. "
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
ms.openlocfilehash: b6cb2ea1c1facada854fb52c55066f34a8289e47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-overview"></a><span data-ttu-id="aca67-104">Přehled zabezpečení Internetu věcí</span><span class="sxs-lookup"><span data-stu-id="aca67-104">Internet of Things security overview</span></span>
<span data-ttu-id="aca67-105">Azure pro internet věcí (IoT) services nabízí celou řadu funkcí.</span><span class="sxs-lookup"><span data-stu-id="aca67-105">Azure internet of things (IoT) services offer a broad range of capabilities.</span></span> <span data-ttu-id="aca67-106">Tyto služby na úrovni řešení pro velké firmy umožňují:</span><span class="sxs-lookup"><span data-stu-id="aca67-106">These enterprise grade services enable you to:</span></span>

* <span data-ttu-id="aca67-107">sběr dat ze zařízení</span><span class="sxs-lookup"><span data-stu-id="aca67-107">Collect data from devices</span></span>
* <span data-ttu-id="aca67-108">analýzu datových proudů za provozu</span><span class="sxs-lookup"><span data-stu-id="aca67-108">Analyze data streams in-motion</span></span>
* <span data-ttu-id="aca67-109">ukládání objemných datových sad a dotazy na ně</span><span class="sxs-lookup"><span data-stu-id="aca67-109">Store and query large data sets</span></span>
* <span data-ttu-id="aca67-110">vizualizaci historických dat i dat v reálném čase</span><span class="sxs-lookup"><span data-stu-id="aca67-110">Visualize both real-time and historical data</span></span>
* <span data-ttu-id="aca67-111">integraci se systémy administrativní podpory (back-office)</span><span class="sxs-lookup"><span data-stu-id="aca67-111">Integrate with back-office systems</span></span>

<span data-ttu-id="aca67-112">Tyto funkce Azure IoT Suite balíčky současně toodeliver více Azure služby s vlastními rozšířeními jako předkonfigurovaná řešení.</span><span class="sxs-lookup"><span data-stu-id="aca67-112">toodeliver these capabilities, Azure IoT Suite packages together multiple Azure services with custom extensions as preconfigured solutions.</span></span> <span data-ttu-id="aca67-113">Tato předkonfigurovaná řešení jsou základní implementací běžných vzorů řešení IoT, které pomáhají tooreduce hello čas trvat toodeliver řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="aca67-113">These preconfigured solutions are base implementations of common IoT solution patterns that help tooreduce hello time you take toodeliver your IoT solutions.</span></span> <span data-ttu-id="aca67-114">Hello IoT software development Kit můžete přizpůsobit a rozšířit těchto řešení toomeet vlastní požadavky.</span><span class="sxs-lookup"><span data-stu-id="aca67-114">Using hello IoT software development kits, you can customize and extend these solutions toomeet your own requirements.</span></span> <span data-ttu-id="aca67-115">Tato řešení můžete využít i jako příklady či šablony při vývoji nových řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="aca67-115">You can also use these solutions as examples or templates when you are developing new IoT solutions.</span></span>

<span data-ttu-id="aca67-116">Hello Azure IoT suite je výkonné řešení pro vaše potřeby IoT.</span><span class="sxs-lookup"><span data-stu-id="aca67-116">hello Azure IoT suite is a powerful solution for your IoT needs.</span></span> <span data-ttu-id="aca67-117">Je však upmost význam, vašich vlastních IoT řešení jsou navržená tak, s důrazem na bezpečnost od začátku hello.</span><span class="sxs-lookup"><span data-stu-id="aca67-117">However, it’s of upmost importance that your IoT solutions are designed with security in mind from hello start.</span></span> <span data-ttu-id="aca67-118">Kvůli hello velkému počtu zařízení IoT incidentů, zabezpečení rychle stát událost související s rozšířeným se významné důsledky.</span><span class="sxs-lookup"><span data-stu-id="aca67-118">Because of hello sheer number of IoT devices, any security incident can quickly become a widespread event with significant consequences.</span></span>

<span data-ttu-id="aca67-119">toohelp porozumíte jak toosecure řešení IoT máme hello následující informace.</span><span class="sxs-lookup"><span data-stu-id="aca67-119">toohelp you understand how toosecure your IoT solutions, we have hello following information.</span></span>

## <a name="security-architecture"></a><span data-ttu-id="aca67-120">Architektura zabezpečení</span><span class="sxs-lookup"><span data-stu-id="aca67-120">Security architecture</span></span>
<span data-ttu-id="aca67-121">Při návrhu systému, je důležité toounderstand hello potenciální hrozby toothat systém a přidejte příslušné obrany podle toho, jak systém hello je navržená tak a navržen.</span><span class="sxs-lookup"><span data-stu-id="aca67-121">When designing a system, it is important toounderstand hello potential threats toothat system, and add appropriate defenses accordingly, as hello system is designed and architected.</span></span> <span data-ttu-id="aca67-122">Je důležité, protože pochopení, jak útočník může být schopný toocompromise systému pomáhá, ujistěte se, že jsou splněné od začátku hello odpovídající jejich zmírnění toodesign hello produktu od začátku hello s důrazem na bezpečnost.</span><span class="sxs-lookup"><span data-stu-id="aca67-122">It is important toodesign hello product from hello start with security in mind because understanding how an attacker might be able toocompromise a system helps make sure appropriate mitigations are in place from hello beginning.</span></span>

<span data-ttu-id="aca67-123">Můžete další informace o architektuře IoT zabezpečení načtením [Internet věcí zabezpečení architektura](../iot-suite/iot-security-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="aca67-123">You can learn about IoT security architecture by reading [Internet of Things Security Architecture](../iot-suite/iot-security-architecture.md).</span></span>

<span data-ttu-id="aca67-124">Tento článek popisuje hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="aca67-124">This article discusses hello following topics:</span></span>

* [<span data-ttu-id="aca67-125">Zabezpečení začíná Model hrozeb</span><span class="sxs-lookup"><span data-stu-id="aca67-125">Security Starts with a Threat Model</span></span>](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [<span data-ttu-id="aca67-126">Zabezpečení v IoT</span><span class="sxs-lookup"><span data-stu-id="aca67-126">Security in IoT</span></span>](../iot-suite/iot-security-architecture.md#security-in-iot)
* [<span data-ttu-id="aca67-127">Hello modelování hrozeb referenční architektura IoT Azure</span><span class="sxs-lookup"><span data-stu-id="aca67-127">Threat Modeling hello Azure IoT Reference Architecture</span></span>](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-hello-ground-up"></a><span data-ttu-id="aca67-128">Zabezpečení z hello pozadí</span><span class="sxs-lookup"><span data-stu-id="aca67-128">Security from hello ground up</span></span>
<span data-ttu-id="aca67-129">Hello IoT představuje jedinečné zabezpečení, ochrany osobních údajů a dodržování předpisů výzvy toobusinesses po celém světě.</span><span class="sxs-lookup"><span data-stu-id="aca67-129">hello IoT poses unique security, privacy, and compliance challenges toobusinesses worldwide.</span></span> <span data-ttu-id="aca67-130">Na rozdíl od tradičních internetový technologie, kde tyto problémy základem softwaru a o tom, jak je implementována IoT se vztahuje na co se stane, když internetový hello a fyzické světů hello sloučit.</span><span class="sxs-lookup"><span data-stu-id="aca67-130">Unlike traditional cyber technology where these issues revolve around software and how it is implemented, IoT concerns what happens when hello cyber and hello physical worlds converge.</span></span> <span data-ttu-id="aca67-131">Ochrana řešení IoT vyžaduje zajištění zabezpečené zřizování zařízení, zabezpečené připojení mezi těchto zařízení a hello cloudu a ochranu zabezpečení dat v cloudu hello při zpracování a úložiště.</span><span class="sxs-lookup"><span data-stu-id="aca67-131">Protecting IoT solutions requires ensuring secure provisioning of devices, secure connectivity between these devices and hello cloud, and secure data protection in hello cloud during processing and storage.</span></span> <span data-ttu-id="aca67-132">Pracovat se tyto funkce, ale jsou zařízení s omezenými zdroji, geografické rozptýlení nasazení a mnoho zařízení v rámci řešení.</span><span class="sxs-lookup"><span data-stu-id="aca67-132">Working against such functionality, however, are resource-constrained devices, geographic distribution of deployments, and many devices within a solution.</span></span>

<span data-ttu-id="aca67-133">Dozvíte, jak toohandle zabezpečení v těchto oblastech načtením [zabezpečení Internetu věcí z hello pozadí](../iot-suite/securing-iot-ground-up.md).</span><span class="sxs-lookup"><span data-stu-id="aca67-133">You can learn how toohandle security in these areas by reading [Internet of Things security from hello ground up](../iot-suite/securing-iot-ground-up.md).</span></span>

<span data-ttu-id="aca67-134">Hello článek popisuje hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="aca67-134">hello article discusses hello following topics:</span></span>

* [<span data-ttu-id="aca67-135">Zabezpečení infrastruktury z hello pozadí</span><span class="sxs-lookup"><span data-stu-id="aca67-135">Secure infrastructure from hello ground up</span></span>](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [<span data-ttu-id="aca67-136">Microsoft Azure – zabezpečené IoT infrastrukturu pro vaši organizaci</span><span class="sxs-lookup"><span data-stu-id="aca67-136">Microsoft Azure – secure IoT infrastructure for your business</span></span>](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a><span data-ttu-id="aca67-137">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="aca67-137">Best Practices</span></span>
<span data-ttu-id="aca67-138">Zabezpečení infrastruktury IoT vyžaduje přísných strategii zabezpečení do hloubky.</span><span class="sxs-lookup"><span data-stu-id="aca67-138">Securing an IoT infrastructure requires a rigorous security-in-depth strategy.</span></span> <span data-ttu-id="aca67-139">Z zabezpečení dat v cloudu hello chrání integritu dat při přenosu přes hello veřejný internet, toosecurely zřizování zařízení, jednotlivé úrovně sestavení větší zajištění zabezpečení hello celé infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="aca67-139">From securing data in hello cloud, protecting data integrity while in transit over hello public internet, toosecurely provisioning devices, each layer builds greater security assurance in hello overall infrastructure.</span></span>

<span data-ttu-id="aca67-140">Dozvíte se o zabezpečení Internetu věcí osvědčené postupy načtením [osvědčené postupy pro zabezpečení Internetu věcí](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="aca67-140">You can learn about Internet of Things security best practices by reading [Internet of Things security best practices](../iot-suite/iot-security-best-practices.md).</span></span>

<span data-ttu-id="aca67-141">Hello článek popisuje hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="aca67-141">hello article discusses hello following topics:</span></span>

* [<span data-ttu-id="aca67-142">Výrobce hardwaru IoT/integrátor</span><span class="sxs-lookup"><span data-stu-id="aca67-142">IoT hardware manufacturer/integrator</span></span>](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [<span data-ttu-id="aca67-143">Vývojář řešení IoT</span><span class="sxs-lookup"><span data-stu-id="aca67-143">IoT solution developer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [<span data-ttu-id="aca67-144">Nástroje pro nasazení řešení IoT</span><span class="sxs-lookup"><span data-stu-id="aca67-144">IoT solution deployer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [<span data-ttu-id="aca67-145">Operátor řešení IoT</span><span class="sxs-lookup"><span data-stu-id="aca67-145">IoT solution operator</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
