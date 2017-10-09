---
title: "aaaAzure kontejner instancí přehled | Dokumentace Azure"
description: "Vysvětlení služby Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0662ede1260b15d9841bfc2c3c4cec4c30338d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances"></a><span data-ttu-id="97636-103">Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="97636-103">Azure Container Instances</span></span>

<span data-ttu-id="97636-104">Kontejnery se stávají rychle hello upřednostňovaný způsob, jak toopackage, nasadit a spravovat cloudové aplikace.</span><span class="sxs-lookup"><span data-stu-id="97636-104">Containers are quickly becoming hello preferred way toopackage, deploy, and manage cloud applications.</span></span> <span data-ttu-id="97636-105">Azure instancí kontejnerů nabízí hello nejrychlejší a nejjednodušší způsob, jak toorun kontejner v Azure, bez nutnosti tooprovision všechny virtuální počítače a bez nutnosti tooadopt vyšší úrovně služby.</span><span class="sxs-lookup"><span data-stu-id="97636-105">Azure Container Instances offers hello fastest and simplest way toorun a container in Azure, without having tooprovision any virtual machines and without having tooadopt a higher-level service.</span></span> 

<span data-ttu-id="97636-106">Azure Container Instances je skvělým řešením pro jakýkoli scénář, který může fungovat v izolovaných kontejnerech, včetně jednoduchých aplikací, automatizace úkolů a úloh sestavení.</span><span class="sxs-lookup"><span data-stu-id="97636-106">Azure Container Instances is a great solution for any scenario that can operate in isolated containers, including simple applications, task automation, and build jobs.</span></span> <span data-ttu-id="97636-107">Pro scénáře, kde nepotřebují orchestration kontejneru, včetně zjišťování služby napříč více kontejnerů, automatické škálování a upgrady koordinované aplikací doporučujeme hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span><span class="sxs-lookup"><span data-stu-id="97636-107">For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span></span>

## <a name="fast-startup-times"></a><span data-ttu-id="97636-108">Rychlé časy spuštění</span><span class="sxs-lookup"><span data-stu-id="97636-108">Fast startup times</span></span>

<span data-ttu-id="97636-109">Kontejnery nabízejí významné výhody při spouštění oproti virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="97636-109">Containers offer significant startup benefits over virtual machines.</span></span> <span data-ttu-id="97636-110">Kontejner instancemi Azure můžete v sekundách bez nutnosti tooprovision hello spusťte kontejner v Azure a spravovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="97636-110">With Azure Container Instances, you can start a container in Azure in seconds without hello need tooprovision and manage VMs.</span></span>

## <a name="hypervisor-level-security"></a><span data-ttu-id="97636-111">Zabezpečení na úrovni hypervisoru</span><span class="sxs-lookup"><span data-stu-id="97636-111">Hypervisor-level security</span></span>

<span data-ttu-id="97636-112">Kontejnery tradičně nabízejí izolaci závislostí aplikace a zásady správného řízení prostředků, ale nebyly považovány za dostatečně odolné pro použití v nehostinném prostředí více tenantů.</span><span class="sxs-lookup"><span data-stu-id="97636-112">Historically, containers have offered application dependency isolation and resource governance but have not been considered sufficiently hardened for hostile multi-tenant usage.</span></span> <span data-ttu-id="97636-113">Díky službě Azure Container Instances je vaše aplikace v kontejneru izolována stejně, jako by byla na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="97636-113">With Azure Container Instances, your application is as isolated in a container as it would be in a VM.</span></span>

## <a name="custom-sizes"></a><span data-ttu-id="97636-114">Vlastní velikosti</span><span class="sxs-lookup"><span data-stu-id="97636-114">Custom sizes</span></span>

<span data-ttu-id="97636-115">Kontejnery jsou obvykle optimalizované toorun, který se může značně lišit právě jednu aplikaci, ale hello konkrétním potřebám těchto aplikací.</span><span class="sxs-lookup"><span data-stu-id="97636-115">Containers are typically optimized toorun just a single application, but hello exact needs of those applications can differ greatly.</span></span> <span data-ttu-id="97636-116">Se službou Azure Container Instances si můžete vyžádat přesně to, co potřebujete z hlediska procesorových jader a paměti.</span><span class="sxs-lookup"><span data-stu-id="97636-116">With Azure Container Instances, you can request exactly what you need in terms of CPU cores and memory.</span></span> <span data-ttu-id="97636-117">Platí podle co budete požadovat, se účtují podle hello druhé, tak můžete jemně optimalizovat výdajů na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="97636-117">You pay based on what you request, billed by hello second, so you can finely optimize your spending based on your needs.</span></span>

## <a name="public-ip-connectivity"></a><span data-ttu-id="97636-118">Připojení pomocí veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="97636-118">Public IP connectivity</span></span>

<span data-ttu-id="97636-119">Instancemi kontejner Azure můžou zpřístupnit kontejnerů přímo toohello internet s veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="97636-119">With Azure Container Instances, you can expose your containers directly toohello internet with a public IP address.</span></span> <span data-ttu-id="97636-120">V budoucích hello jsme bude rozbalte naše sítě integrace tooinclude možnosti s virtuálními sítěmi, zatížení nástroje pro vyrovnávání a dalšími částmi základní hello Azure síťové infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="97636-120">In hello future, we will expand our networking capabilities tooinclude integration with virtual networks, load balancers, and other core parts of hello Azure networking infrastructure.</span></span>

## <a name="persistent-storage"></a><span data-ttu-id="97636-121">Trvalé úložiště</span><span class="sxs-lookup"><span data-stu-id="97636-121">Persistent storage</span></span>

<span data-ttu-id="97636-122">tooretrieve a zachování stavu s instancemi Azure kontejneru, nabízíme přímé připojení z Azure souborů sdílených složek.</span><span class="sxs-lookup"><span data-stu-id="97636-122">tooretrieve and persist state with Azure Container Instances, we offer direct mounting of Azure files shares.</span></span>

## <a name="linux-and-windows-containers"></a><span data-ttu-id="97636-123">Kontejnery Windows a Linuxu</span><span class="sxs-lookup"><span data-stu-id="97636-123">Linux and Windows containers</span></span>

<span data-ttu-id="97636-124">Kontejner instancemi Azure můžete naplánovat systému Windows a Linux kontejnery s hello stejné rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="97636-124">With Azure Container Instances, you can schedule both Windows and Linux containers with hello same API.</span></span> <span data-ttu-id="97636-125">Jednoduše znamenat hello základní typ operačního systému a všem ostatním identické.</span><span class="sxs-lookup"><span data-stu-id="97636-125">Simply indicate hello base OS type and everything else is identical.</span></span>

## <a name="co-scheduled-groups"></a><span data-ttu-id="97636-126">Společně plánované skupiny</span><span class="sxs-lookup"><span data-stu-id="97636-126">Co-scheduled groups</span></span>

<span data-ttu-id="97636-127">Azure Container Instances podporuje plánování skupin více kontejnerů, které sdílejí hostitelský počítač, místní síť, úložiště a životní cyklus.</span><span class="sxs-lookup"><span data-stu-id="97636-127">Azure Container Instances supports scheduling of multi-container groups that share a host machine, local network, storage, and lifecycle.</span></span> <span data-ttu-id="97636-128">Díky tomu můžete toocombine hlavní aplikace s ostatními funguje v roli podpůrné například protokolování.</span><span class="sxs-lookup"><span data-stu-id="97636-128">This enables you toocombine your main application with others acting in a supporting role, such as logging.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97636-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97636-129">Next steps</span></span>

<span data-ttu-id="97636-130">Vyzkoušejte nasazení kontejneru tooAzure pomocí jednoduchého příkazu naše [Průvodce rychlým zahájením](container-instances-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="97636-130">Try deploying a container tooAzure with a single command using our [quickstart guide](container-instances-quickstart.md).</span></span>
