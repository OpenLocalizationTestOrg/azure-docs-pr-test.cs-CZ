---
title: "Internet věcí osvědčené postupy zabezpečení | Microsoft Docs"
description: "Tento článek poskytuje kurátorované seznam Microsoft Internet věcí osvědčené postupy zabezpečení a obecná doporučení."
services: security
documentationcenter: na
author: TomShinder
manager: StevenPo
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: yurid
ms.openlocfilehash: 8efc0053458e338ac1afe98d9ce970c1d5cbfa81
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="internet-of-things-security-best-practices"></a><span data-ttu-id="a312e-103">Internet věcí osvědčené postupy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="a312e-103">Internet of Things Security Best Practices</span></span>
<span data-ttu-id="a312e-104">Zabezpečení infrastruktury Internetu věcí (IoT) je důležité podnikem pro každý, kdo spojené s řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="a312e-104">Securing the Internet of Things (IoT) infrastructure is a critical undertaking for anyone involved with IoT solutions.</span></span> <span data-ttu-id="a312e-105">Kvůli distribuovaná povaha těchto zařízení a počet zařízení související se situací dopad událostí zabezpečení související s ohrožení miliony zařízení IoT je netriviální a může mít rozšířeným dopad.</span><span class="sxs-lookup"><span data-stu-id="a312e-105">Because of the number of devices involved and the distributed nature of these devices, the impact a security event related to compromise of millions of IoT devices is non-trivial and can have widespread impact.</span></span>

<span data-ttu-id="a312e-106">Z tohoto důvodu musí IoT zabezpečení zabezpečení do hloubky přístup.</span><span class="sxs-lookup"><span data-stu-id="a312e-106">For this reason, IoT security needs a security-in-depth approach.</span></span> <span data-ttu-id="a312e-107">Data musí být zabezpečené v cloudu a při jejich přesunu přes privátní a veřejné sítě.</span><span class="sxs-lookup"><span data-stu-id="a312e-107">Data needs to be secure in the cloud and as it moves over private and public networks.</span></span> <span data-ttu-id="a312e-108">Metody musí být na místě bezpečně zřídit zařízeními IoT.</span><span class="sxs-lookup"><span data-stu-id="a312e-108">Methods need to be in place to securely provision the IoT devices themselves.</span></span> <span data-ttu-id="a312e-109">Každou vrstvu ze zařízení k síti, do cloudu back-end vyžaduje silné zabezpečení záruky.</span><span class="sxs-lookup"><span data-stu-id="a312e-109">Each layer, from device, to network, to cloud back-end needs strong security assurances.</span></span>

<span data-ttu-id="a312e-110">Osvědčené postupy IoT může být rozdělena následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a312e-110">IoT best practices can be categorized in the following way:</span></span>

* <span data-ttu-id="a312e-111">Výrobce hardwaru IoT nebo integrátor</span><span class="sxs-lookup"><span data-stu-id="a312e-111">IoT hardware manufacturer or integrator</span></span>
* <span data-ttu-id="a312e-112">Vývojář řešení IoT</span><span class="sxs-lookup"><span data-stu-id="a312e-112">IoT solution developer</span></span>
* <span data-ttu-id="a312e-113">Nástroje pro nasazení řešení IoT</span><span class="sxs-lookup"><span data-stu-id="a312e-113">IoT solution deployer</span></span>
* <span data-ttu-id="a312e-114">Operátor řešení IoT</span><span class="sxs-lookup"><span data-stu-id="a312e-114">IoT solution operator</span></span>

<span data-ttu-id="a312e-115">Tento článek shrnuje [Internetu z věcí osvědčené postupy zabezpečení](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="a312e-115">This article summarizes [Internet of Things Security Best Practices](../iot-suite/iot-security-best-practices.md).</span></span> <span data-ttu-id="a312e-116">Naleznete v tomto článku najdete podrobnější informace.</span><span class="sxs-lookup"><span data-stu-id="a312e-116">Please refer to that article for more detailed information.</span></span>

## <a name="iot-hardware-manufacturer-or-integrator"></a><span data-ttu-id="a312e-117">Výrobce hardwaru IoT nebo integrátor</span><span class="sxs-lookup"><span data-stu-id="a312e-117">IoT hardware manufacturer or integrator</span></span>
<span data-ttu-id="a312e-118">Použijte následující osvědčené postupy, pokud jste IoT výrobce hardwaru nebo integrátor hardwaru:</span><span class="sxs-lookup"><span data-stu-id="a312e-118">Follow the best practices below if you are an IoT hardware manufacture or a hardware integrator:</span></span>

* <span data-ttu-id="a312e-119">**Obor hardware a požadavky na minimální**: návrh hardwaru by měla zahrnovat nutných pro operaci hardwaru a nic další minimální funkce.</span><span class="sxs-lookup"><span data-stu-id="a312e-119">**Scope hardware to minimum requirements**: the hardware design should include minimum features required for operation of the hardware, and nothing more.</span></span> 
* <span data-ttu-id="a312e-120">**Ujistěte se, hardwaru manipulovat ověření**: sestavení v mechanismy pro zjistit případnou manipulaci fyzického hardwaru, jako je například otevírání krytu zařízení, odebrání součástí zařízení atd.</span><span class="sxs-lookup"><span data-stu-id="a312e-120">**Make hardware tamper proof**: build in mechanisms to detect physical tampering of hardware, such as opening the device cover, removing a part of the device, etc.</span></span> 
* <span data-ttu-id="a312e-121">**Sestavení kolem zabezpečený hardware**: Pokud [spotřebu](https://en.wikipedia.org/wiki/Cost_of_goods_sold) povolení, funkce zabezpečení, jako je zabezpečená a šifrovaná úložiště a na základě Trusted Platform Module TPM spouštěcí funkce sestavení.</span><span class="sxs-lookup"><span data-stu-id="a312e-121">**Build around secure hardware**: if [COGS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) permit, build security features such as secure and encrypted storage and Trusted Platform Module (TPM)-based boot functionality.</span></span>
* <span data-ttu-id="a312e-122">**Zabezpečit upgrady**: upgrade firmwaru během životního cyklu zařízení je nevyhnutelné.</span><span class="sxs-lookup"><span data-stu-id="a312e-122">**Make upgrades secure**: upgrading firmware during lifetime of the device is inevitable.</span></span>

## <a name="iot-solution-developer"></a><span data-ttu-id="a312e-123">Vývojář řešení IoT</span><span class="sxs-lookup"><span data-stu-id="a312e-123">IoT solution developer</span></span>
<span data-ttu-id="a312e-124">Použijte následující osvědčené postupy, pokud jste vývojář řešení IoT:</span><span class="sxs-lookup"><span data-stu-id="a312e-124">Follow the best practices below if you are an IoT solution developer:</span></span>

* <span data-ttu-id="a312e-125">**Postupujte podle zabezpečené softwaru vývoj metodika**: vývoj zabezpečené softwaru vyžaduje základů přemýšlení o zabezpečení od zahájení projektu zcela k jeho implementaci, testování a nasazení.</span><span class="sxs-lookup"><span data-stu-id="a312e-125">**Follow secure software development methodology**: developing secure software requires ground-up thinking about security from the inception of the project all the way to its implementation, testing, and deployment.</span></span>
* <span data-ttu-id="a312e-126">**Vyberte software s otevřeným zdrojem dát pozor**: software s otevřeným zdrojem vám dává příležitost k rychlému vývoji řešení.</span><span class="sxs-lookup"><span data-stu-id="a312e-126">**Choose open source software with care**: open source software provides an opportunity to quickly develop solutions.</span></span>
* <span data-ttu-id="a312e-127">**Integrovat dát pozor**: řadu nedostatků zabezpečení softwaru existovat hranice knihovny a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a312e-127">**Integrate with care**: many of the software security flaws exist at the boundary of libraries and APIs.</span></span> 

## <a name="iot-solution-deployer"></a><span data-ttu-id="a312e-128">Nástroje pro nasazení řešení IoT</span><span class="sxs-lookup"><span data-stu-id="a312e-128">IoT solution deployer</span></span>
<span data-ttu-id="a312e-129">Použijte následující osvědčené postupy, pokud jste deployer řešení IoT:</span><span class="sxs-lookup"><span data-stu-id="a312e-129">Follow the best practices below if you are an IoT solution deployer:</span></span>

* <span data-ttu-id="a312e-130">**Nasazení hardwaru bezpečně**: IoT nasazení může vyžadovat hardwaru, který má být nasazený v nezabezpečená umístění, například veřejné mezery nebo bez dohledu národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="a312e-130">**Deploy hardware securely**: IoT deployments may require hardware to be deployed in unsecure locations, such as in public spaces or unsupervised locales.</span></span>
* <span data-ttu-id="a312e-131">**Chránit ověřovací klíče**: během nasazování každé zařízení vyžaduje ID zařízení a související ověřovací klíče generované cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="a312e-131">**Keep authentication keys safe**: during deployment, each device requires device IDs and associated authentication keys generated by the cloud service.</span></span> <span data-ttu-id="a312e-132">Chránit tyto klíče fyzicky i po jeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="a312e-132">Keep these keys physically safe even after the deployment.</span></span> <span data-ttu-id="a312e-133">Všechny ohrožené klíč lze škodlivý zařízení jako ze stávajících zařízení.</span><span class="sxs-lookup"><span data-stu-id="a312e-133">Any compromised key can be used by a malicious device to masquerade as an existing device.</span></span>

## <a name="iot-solution-operator"></a><span data-ttu-id="a312e-134">Operátor řešení IoT</span><span class="sxs-lookup"><span data-stu-id="a312e-134">IoT solution operator</span></span>
<span data-ttu-id="a312e-135">Použijte následující osvědčené postupy, pokud jste operátor řešení IoT:</span><span class="sxs-lookup"><span data-stu-id="a312e-135">Follow the best practices below if you are an IoT solution operator:</span></span>

* <span data-ttu-id="a312e-136">**Aktuálnost systémy**: Ujistěte se, operační systémy zařízení a všechny ovladače zařízení jsou aktualizovány na nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="a312e-136">**Keep systems up to date**: ensure device operating systems and all device drivers are updated to the latest versions.</span></span> 
* <span data-ttu-id="a312e-137">**Ochrana proti škodlivé aktivity**: Pokud operačního systému povolí, umístěte nejnovějších funkcích antivirový a proti malwaru na každý operační systém zařízení.</span><span class="sxs-lookup"><span data-stu-id="a312e-137">**Protect against malicious activity**: if the operating system permits, place the latest anti-virus and anti-malware capabilities on each device operating system.</span></span> 
* <span data-ttu-id="a312e-138">**Audit často**: auditování IoT infrastruktury pro týkající se problémů je klíč při odpovědi na bezpečnostní incidenty v oblasti zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a312e-138">**Audit frequently**: auditing IoT infrastructure for security related issues is key when responding to security incidents.</span></span>
* <span data-ttu-id="a312e-139">**Fyzicky ochrana infrastruktury IoT**: nejhorší zabezpečení útoky na infrastrukturu IoT spustily pomocí fyzický přístup k zařízením.</span><span class="sxs-lookup"><span data-stu-id="a312e-139">**Physically protect the IoT infrastructure**: the worst security attacks against IoT infrastructure are launched using physical access to devices.</span></span>
* <span data-ttu-id="a312e-140">**Ochranu přihlašovacích údajů cloudu**: cloudové ověřování pověření použitá pro konfiguraci a provozní nasazení služby IoT jsou pravděpodobně nejjednodušší způsob, jak získat přístup a ohrozit systém IoT.</span><span class="sxs-lookup"><span data-stu-id="a312e-140">**Protect cloud credentials**: cloud authentication credentials used for configuring and operating an IoT deployment are possibly the easiest way to gain access and compromise an IoT system.</span></span> 

