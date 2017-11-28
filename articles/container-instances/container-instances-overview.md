---
title: "Přehled služby Azure Container Instances | Dokumentace Azure"
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
ms.openlocfilehash: 3fb230c6b16a57e3650abf2000acdfe944cd633c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-container-instances"></a><span data-ttu-id="4ce4d-103">Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="4ce4d-103">Azure Container Instances</span></span>

<span data-ttu-id="4ce4d-104">Kontejnery se rychle stávají upřednostňovaným způsobem balení, nasazování a správy cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-104">Containers are quickly becoming the preferred way to package, deploy, and manage cloud applications.</span></span> <span data-ttu-id="4ce4d-105">Služba Azure Container Instances nabízí nejrychlejší a nejjednodušší způsob provozování kontejneru v Azure. Není přitom nutné zřizovat žádné virtuální počítače nebo využívat služby vyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-105">Azure Container Instances offers the fastest and simplest way to run a container in Azure, without having to provision any virtual machines and without having to adopt a higher-level service.</span></span> 

<span data-ttu-id="4ce4d-106">Azure Container Instances je skvělým řešením pro jakýkoli scénář, který může fungovat v izolovaných kontejnerech, včetně jednoduchých aplikací, automatizace úkolů a úloh sestavení.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-106">Azure Container Instances is a great solution for any scenario that can operate in isolated containers, including simple applications, task automation, and build jobs.</span></span> <span data-ttu-id="4ce4d-107">Pro scénáře, kde potřebujete úplnou orchestraci kontejnerů, včetně zjišťování služeb napříč více kontejnery, automatického škálování a koordinovaných upgradů aplikací, doporučujeme službu [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span><span class="sxs-lookup"><span data-stu-id="4ce4d-107">For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend the [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span></span>

## <a name="fast-startup-times"></a><span data-ttu-id="4ce4d-108">Rychlé časy spuštění</span><span class="sxs-lookup"><span data-stu-id="4ce4d-108">Fast startup times</span></span>

<span data-ttu-id="4ce4d-109">Kontejnery nabízejí významné výhody při spouštění oproti virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-109">Containers offer significant startup benefits over virtual machines.</span></span> <span data-ttu-id="4ce4d-110">Se službou Azure Container Instances můžete v Azure spustit kontejner během několika sekund a bez nutnosti zřizovat a spravovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-110">With Azure Container Instances, you can start a container in Azure in seconds without the need to provision and manage VMs.</span></span>

## <a name="hypervisor-level-security"></a><span data-ttu-id="4ce4d-111">Zabezpečení na úrovni hypervisoru</span><span class="sxs-lookup"><span data-stu-id="4ce4d-111">Hypervisor-level security</span></span>

<span data-ttu-id="4ce4d-112">Kontejnery tradičně nabízejí izolaci závislostí aplikace a zásady správného řízení prostředků, ale nebyly považovány za dostatečně odolné pro použití v nehostinném prostředí více tenantů.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-112">Historically, containers have offered application dependency isolation and resource governance but have not been considered sufficiently hardened for hostile multi-tenant usage.</span></span> <span data-ttu-id="4ce4d-113">Díky službě Azure Container Instances je vaše aplikace v kontejneru izolována stejně, jako by byla na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-113">With Azure Container Instances, your application is as isolated in a container as it would be in a VM.</span></span>

## <a name="custom-sizes"></a><span data-ttu-id="4ce4d-114">Vlastní velikosti</span><span class="sxs-lookup"><span data-stu-id="4ce4d-114">Custom sizes</span></span>

<span data-ttu-id="4ce4d-115">Kontejnery jsou obvykle optimalizované pro spouštění jenom jedné aplikace, ale konkrétní požadavky těchto aplikací se můžou značně lišit.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-115">Containers are typically optimized to run just a single application, but the exact needs of those applications can differ greatly.</span></span> <span data-ttu-id="4ce4d-116">Se službou Azure Container Instances si můžete vyžádat přesně to, co potřebujete z hlediska procesorových jader a paměti.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-116">With Azure Container Instances, you can request exactly what you need in terms of CPU cores and memory.</span></span> <span data-ttu-id="4ce4d-117">Platíte podle toho, co si vyžádáte, a účtuje se po sekundách, takže můžete podrobně optimalizovat náklady podle vašich potřeb.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-117">You pay based on what you request, billed by the second, so you can finely optimize your spending based on your needs.</span></span>

## <a name="public-ip-connectivity"></a><span data-ttu-id="4ce4d-118">Připojení pomocí veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="4ce4d-118">Public IP connectivity</span></span>

<span data-ttu-id="4ce4d-119">Se službou Azure Container Instances můžete své kontejnery zveřejnit přímo na internetu s použitím veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-119">With Azure Container Instances, you can expose your containers directly to the internet with a public IP address.</span></span> <span data-ttu-id="4ce4d-120">V budoucnu plánujeme možnosti sítě rozšířit o integraci s virtuálními sítěmi, nástroji pro vyrovnávání zatížení a dalšími základními částmi síťové infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-120">In the future, we will expand our networking capabilities to include integration with virtual networks, load balancers, and other core parts of the Azure networking infrastructure.</span></span>

## <a name="persistent-storage"></a><span data-ttu-id="4ce4d-121">Trvalé úložiště</span><span class="sxs-lookup"><span data-stu-id="4ce4d-121">Persistent storage</span></span>

<span data-ttu-id="4ce4d-122">Pro načtení a uložení stavu pomocí služby Azure Container Instances nabízíme přímé připojení sdílených složek Azure.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-122">To retrieve and persist state with Azure Container Instances, we offer direct mounting of Azure files shares.</span></span>

## <a name="linux-and-windows-containers"></a><span data-ttu-id="4ce4d-123">Kontejnery Windows a Linuxu</span><span class="sxs-lookup"><span data-stu-id="4ce4d-123">Linux and Windows containers</span></span>

<span data-ttu-id="4ce4d-124">Se službou Azure Container Instances můžete plánovat kontejnery Windows i Linuxu pomocí stejného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-124">With Azure Container Instances, you can schedule both Windows and Linux containers with the same API.</span></span> <span data-ttu-id="4ce4d-125">Jednoduše určíte základní typ operačního systému a všechno ostatní je stejné.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-125">Simply indicate the base OS type and everything else is identical.</span></span>

## <a name="co-scheduled-groups"></a><span data-ttu-id="4ce4d-126">Společně plánované skupiny</span><span class="sxs-lookup"><span data-stu-id="4ce4d-126">Co-scheduled groups</span></span>

<span data-ttu-id="4ce4d-127">Azure Container Instances podporuje plánování skupin více kontejnerů, které sdílejí hostitelský počítač, místní síť, úložiště a životní cyklus.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-127">Azure Container Instances supports scheduling of multi-container groups that share a host machine, local network, storage, and lifecycle.</span></span> <span data-ttu-id="4ce4d-128">Díky tomu můžete kombinovat hlavní aplikaci s dalšími, které zastávají podpůrnou roli, jako je například protokolování.</span><span class="sxs-lookup"><span data-stu-id="4ce4d-128">This enables you to combine your main application with others acting in a supporting role, such as logging.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ce4d-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4ce4d-129">Next steps</span></span>

<span data-ttu-id="4ce4d-130">Vyzkoušejte si nasazení kontejneru do Azure jediným příkazem s využitím naší [příručky Rychlý start](container-instances-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="4ce4d-130">Try deploying a container to Azure with a single command using our [quickstart guide](container-instances-quickstart.md).</span></span>