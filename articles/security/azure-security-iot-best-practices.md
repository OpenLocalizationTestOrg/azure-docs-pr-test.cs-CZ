---
title: "aaaInternet osvědčené postupy zabezpečení věcí | Microsoft Docs"
description: "Hello článek obsahuje seznam kurátorované Microsoft Internet věcí osvědčené postupy zabezpečení a obecná doporučení."
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
ms.openlocfilehash: 7ee31c912e8ac230ffa5efcd5b4c2b0b0713584f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-best-practices"></a><span data-ttu-id="2bc6c-103">Internet věcí osvědčené postupy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2bc6c-103">Internet of Things Security Best Practices</span></span>
<span data-ttu-id="2bc6c-104">Zabezpečení Internetu věcí (IoT) infrastruktura hello je důležité podniku pro každý, kdo spojené s řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-104">Securing hello Internet of Things (IoT) infrastructure is a critical undertaking for anyone involved with IoT solutions.</span></span> <span data-ttu-id="2bc6c-105">Distribuovaná povaha tato zařízení hello dopad událostí zabezpečení z důvodu hello počet zařízení zahrnutých a hello související toocompromise miliony zařízení IoT je netriviální a může mít rozšířeným dopad.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-105">Because of hello number of devices involved and hello distributed nature of these devices, hello impact a security event related toocompromise of millions of IoT devices is non-trivial and can have widespread impact.</span></span>

<span data-ttu-id="2bc6c-106">Z tohoto důvodu musí IoT zabezpečení zabezpečení do hloubky přístup.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-106">For this reason, IoT security needs a security-in-depth approach.</span></span> <span data-ttu-id="2bc6c-107">Data potřebám toobe zabezpečení v cloudu hello a jak se přesune přes privátní a veřejné sítě.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-107">Data needs toobe secure in hello cloud and as it moves over private and public networks.</span></span> <span data-ttu-id="2bc6c-108">Metody potřebovat toobe v místě toosecurely zřídit hello zařízení IoT sami.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-108">Methods need toobe in place toosecurely provision hello IoT devices themselves.</span></span> <span data-ttu-id="2bc6c-109">Jednotlivé úrovně ze zařízení, toonetwork, toocloud back-end vyžaduje silné zabezpečení záruky.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-109">Each layer, from device, toonetwork, toocloud back-end needs strong security assurances.</span></span>

<span data-ttu-id="2bc6c-110">Osvědčené postupy IoT může rozdělené do hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2bc6c-110">IoT best practices can be categorized in hello following way:</span></span>

* <span data-ttu-id="2bc6c-111">Výrobce hardwaru IoT nebo integrátor</span><span class="sxs-lookup"><span data-stu-id="2bc6c-111">IoT hardware manufacturer or integrator</span></span>
* <span data-ttu-id="2bc6c-112">Vývojář řešení IoT</span><span class="sxs-lookup"><span data-stu-id="2bc6c-112">IoT solution developer</span></span>
* <span data-ttu-id="2bc6c-113">Nástroje pro nasazení řešení IoT</span><span class="sxs-lookup"><span data-stu-id="2bc6c-113">IoT solution deployer</span></span>
* <span data-ttu-id="2bc6c-114">Operátor řešení IoT</span><span class="sxs-lookup"><span data-stu-id="2bc6c-114">IoT solution operator</span></span>

<span data-ttu-id="2bc6c-115">Tento článek shrnuje [Internetu z věcí osvědčené postupy zabezpečení](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="2bc6c-115">This article summarizes [Internet of Things Security Best Practices](../iot-suite/iot-security-best-practices.md).</span></span> <span data-ttu-id="2bc6c-116">Podrobnější informace naleznete v článku toothat.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-116">Please refer toothat article for more detailed information.</span></span>

## <a name="iot-hardware-manufacturer-or-integrator"></a><span data-ttu-id="2bc6c-117">Výrobce hardwaru IoT nebo integrátor</span><span class="sxs-lookup"><span data-stu-id="2bc6c-117">IoT hardware manufacturer or integrator</span></span>
<span data-ttu-id="2bc6c-118">Pokud jste IoT výrobce hardwaru nebo integrátor hardwaru, držte se osvědčených postupů hello níže:</span><span class="sxs-lookup"><span data-stu-id="2bc6c-118">Follow hello best practices below if you are an IoT hardware manufacture or a hardware integrator:</span></span>

* <span data-ttu-id="2bc6c-119">**Obor požadavky na hardware toominimum**: návrh hardwaru hello by měla zahrnovat nutných pro operaci hello hardwaru a nic další minimální funkce.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-119">**Scope hardware toominimum requirements**: hello hardware design should include minimum features required for operation of hello hardware, and nothing more.</span></span> 
* <span data-ttu-id="2bc6c-120">**Ujistěte se, hardwaru manipulovat ověření**: sestavení v mechanismy toodetect fyzické manipulaci hardwaru, jako je například otevírání hello zařízení titulní, odebrání součástí hello zařízení atd.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-120">**Make hardware tamper proof**: build in mechanisms toodetect physical tampering of hardware, such as opening hello device cover, removing a part of hello device, etc.</span></span> 
* <span data-ttu-id="2bc6c-121">**Sestavení kolem zabezpečený hardware**: Pokud [spotřebu](https://en.wikipedia.org/wiki/Cost_of_goods_sold) povolení, funkce zabezpečení, jako je zabezpečená a šifrovaná úložiště a na základě Trusted Platform Module TPM spouštěcí funkce sestavení.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-121">**Build around secure hardware**: if [COGS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) permit, build security features such as secure and encrypted storage and Trusted Platform Module (TPM)-based boot functionality.</span></span>
* <span data-ttu-id="2bc6c-122">**Zabezpečit upgrady**: upgrade firmwaru během životního cyklu zařízení hello je nevyhnutelné.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-122">**Make upgrades secure**: upgrading firmware during lifetime of hello device is inevitable.</span></span>

## <a name="iot-solution-developer"></a><span data-ttu-id="2bc6c-123">Vývojář řešení IoT</span><span class="sxs-lookup"><span data-stu-id="2bc6c-123">IoT solution developer</span></span>
<span data-ttu-id="2bc6c-124">Pokud jste vývojář řešení IoT, držte se osvědčených postupů hello níže:</span><span class="sxs-lookup"><span data-stu-id="2bc6c-124">Follow hello best practices below if you are an IoT solution developer:</span></span>

* <span data-ttu-id="2bc6c-125">**Postupujte podle zabezpečené softwaru vývoj metodika**: vývoj zabezpečené softwaru vyžaduje základů přemýšlíte o zabezpečení z hello zahájení projektu hello všechny hello způsob tooits implementace, testování a nasazení.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-125">**Follow secure software development methodology**: developing secure software requires ground-up thinking about security from hello inception of hello project all hello way tooits implementation, testing, and deployment.</span></span>
* <span data-ttu-id="2bc6c-126">**Vyberte software s otevřeným zdrojem dát pozor**: software s otevřeným zdrojem vám dává příležitost tooquickly vývoj řešení.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-126">**Choose open source software with care**: open source software provides an opportunity tooquickly develop solutions.</span></span>
* <span data-ttu-id="2bc6c-127">**Integrovat dát pozor**: řadu nedostatků zabezpečení softwaru hello existovat hello hranice knihovny a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-127">**Integrate with care**: many of hello software security flaws exist at hello boundary of libraries and APIs.</span></span> 

## <a name="iot-solution-deployer"></a><span data-ttu-id="2bc6c-128">Nástroje pro nasazení řešení IoT</span><span class="sxs-lookup"><span data-stu-id="2bc6c-128">IoT solution deployer</span></span>
<span data-ttu-id="2bc6c-129">Pokud jste deployer řešení IoT, držte se osvědčených postupů hello níže:</span><span class="sxs-lookup"><span data-stu-id="2bc6c-129">Follow hello best practices below if you are an IoT solution deployer:</span></span>

* <span data-ttu-id="2bc6c-130">**Nasazení hardwaru bezpečně**: IoT nasazení může vyžadovat toobe hardware nasazený v nezabezpečená umístění, například veřejné mezery nebo bez dohledu národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-130">**Deploy hardware securely**: IoT deployments may require hardware toobe deployed in unsecure locations, such as in public spaces or unsupervised locales.</span></span>
* <span data-ttu-id="2bc6c-131">**Chránit ověřovací klíče**: během nasazování každé zařízení vyžaduje ID zařízení a související ověřovací klíče generované hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-131">**Keep authentication keys safe**: during deployment, each device requires device IDs and associated authentication keys generated by hello cloud service.</span></span> <span data-ttu-id="2bc6c-132">Chránit tyto klíče fyzicky i po nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-132">Keep these keys physically safe even after hello deployment.</span></span> <span data-ttu-id="2bc6c-133">Všechny ohrožené klíč lze toomasquerade škodlivý zařízení jako ze stávajících zařízení.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-133">Any compromised key can be used by a malicious device toomasquerade as an existing device.</span></span>

## <a name="iot-solution-operator"></a><span data-ttu-id="2bc6c-134">Operátor řešení IoT</span><span class="sxs-lookup"><span data-stu-id="2bc6c-134">IoT solution operator</span></span>
<span data-ttu-id="2bc6c-135">Pokud jste operátor řešení IoT, držte se osvědčených postupů hello níže:</span><span class="sxs-lookup"><span data-stu-id="2bc6c-135">Follow hello best practices below if you are an IoT solution operator:</span></span>

* <span data-ttu-id="2bc6c-136">**Udržování systémů až toodate**: Zkontrolujte aktualizované toohello nejnovější verze jsou operační systémy zařízení a všechny ovladače zařízení.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-136">**Keep systems up toodate**: ensure device operating systems and all device drivers are updated toohello latest versions.</span></span> 
* <span data-ttu-id="2bc6c-137">**Ochrana proti škodlivé aktivity**: Pokud hello operačního systému povolí, umístěte hello nejnovější antivirový a antimalwarové funkce na každý operační systém zařízení.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-137">**Protect against malicious activity**: if hello operating system permits, place hello latest anti-virus and anti-malware capabilities on each device operating system.</span></span> 
* <span data-ttu-id="2bc6c-138">**Audit často**: auditování IoT infrastrukturu pro zabezpečení týkající se problémů je klíč při odpovědi toosecurity incidenty.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-138">**Audit frequently**: auditing IoT infrastructure for security related issues is key when responding toosecurity incidents.</span></span>
* <span data-ttu-id="2bc6c-139">**Fyzicky ochrana hello IoT infrastruktury**: hello nejhorší zabezpečení útoky na infrastrukturu IoT spustily pomocí toodevices fyzický přístup.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-139">**Physically protect hello IoT infrastructure**: hello worst security attacks against IoT infrastructure are launched using physical access toodevices.</span></span>
* <span data-ttu-id="2bc6c-140">**Ochranu přihlašovacích údajů cloudu**: cloudové ověřování pověření použitá pro konfiguraci a provozní nasazení služby IoT se pravděpodobně hello nejjednodušší způsob, jak toogain přístup a ohrozit systém IoT.</span><span class="sxs-lookup"><span data-stu-id="2bc6c-140">**Protect cloud credentials**: cloud authentication credentials used for configuring and operating an IoT deployment are possibly hello easiest way toogain access and compromise an IoT system.</span></span> 

