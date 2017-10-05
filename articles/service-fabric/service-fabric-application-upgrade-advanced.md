---
title: "Rozšířené témata týkající se upgradu aplikace | Microsoft Docs"
description: "Tento článek se zabývá některá Pokročilá témata týkající se upgradu aplikace Service Fabric."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: 8d3b922f3d50b645ac9db2cc879a319df1262e0a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a><span data-ttu-id="e7f1a-103">Upgrade aplikace Service Fabric: advanced témata</span><span class="sxs-lookup"><span data-stu-id="e7f1a-103">Service Fabric application upgrade: advanced topics</span></span>
## <a name="adding-or-removing-services-during-an-application-upgrade"></a><span data-ttu-id="e7f1a-104">Přidání nebo odebrání služby během upgradu aplikace</span><span class="sxs-lookup"><span data-stu-id="e7f1a-104">Adding or removing services during an application upgrade</span></span>
<span data-ttu-id="e7f1a-105">Pokud nové služby je přidán do aplikace, která je již nasazen a publikovat jako upgrade, nové služby se přidá do nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-105">If a new service is added to an application that is already deployed, and published as an upgrade, the new service is added to the deployed application.</span></span>  <span data-ttu-id="e7f1a-106">Takové upgrade neovlivní žádné služby, které již byly součástí aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-106">Such an upgrade does not affect any of the services that were already part of the application.</span></span> <span data-ttu-id="e7f1a-107">Však musí být spuštěná instance služby, která byla přidána nové služby, aby byl aktivní (pomocí `New-ServiceFabricService` rutiny).</span><span class="sxs-lookup"><span data-stu-id="e7f1a-107">However, an instance of the service that was added must be started for the new service to be active (using the `New-ServiceFabricService` cmdlet).</span></span>

<span data-ttu-id="e7f1a-108">Služby může být odebrán také z aplikace jako součást upgradu.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-108">Services can also be removed from an application as part of an upgrade.</span></span> <span data-ttu-id="e7f1a-109">Ale musí být všechny aktuální instance služby k-be odstraněné zastaven před pokračováním v upgradu (pomocí `Remove-ServiceFabricService` rutiny).</span><span class="sxs-lookup"><span data-stu-id="e7f1a-109">However, all current instances of the to-be-deleted service must be stopped before proceeding with the upgrade (using the `Remove-ServiceFabricService` cmdlet).</span></span>

## <a name="manual-upgrade-mode"></a><span data-ttu-id="e7f1a-110">Režim manuálního upgradu</span><span class="sxs-lookup"><span data-stu-id="e7f1a-110">Manual upgrade mode</span></span>
> [!NOTE]
> <span data-ttu-id="e7f1a-111">Měli byste zvážit sledována ruční režim pouze pro upgrade selhání nebo pozastavený.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-111">The unmonitored manual mode should be considered only for a failed or suspended upgrade.</span></span> <span data-ttu-id="e7f1a-112">Monitorované režim je doporučené režimu upgradu pro Service Fabric aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-112">The monitored mode is the recommended upgrade mode for Service Fabric applications.</span></span>
>
>

<span data-ttu-id="e7f1a-113">Azure Service Fabric nabízí více upgradu režimy pro podporu vývoje a produkční clustery.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-113">Azure Service Fabric provides multiple upgrade modes to support development and production clusters.</span></span> <span data-ttu-id="e7f1a-114">Zvolené možnosti nasazení se může lišit pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-114">Deployment options chosen may be different for different environments.</span></span>

<span data-ttu-id="e7f1a-115">Monitorované postupného upgradu aplikace je nejčastější upgrade pro použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-115">The monitored rolling application upgrade is the most typical upgrade to use in the production environment.</span></span> <span data-ttu-id="e7f1a-116">Pokud je zadána zásad upgradu, Service Fabric zajistí, že aplikace je v pořádku, před upgradem.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-116">When the upgrade policy is specified, Service Fabric ensures that the application is healthy before the upgrade proceeds.</span></span>

 <span data-ttu-id="e7f1a-117">Správce aplikací můžete použít ruční postupného upgradu režim aplikací mít úplnou kontrolu nad průběh upgradu prostřednictvím různých domén upgradu.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-117">The application administrator can use the manual rolling application upgrade mode to have total control over the upgrade progress through the various upgrade domains.</span></span> <span data-ttu-id="e7f1a-118">Tento režim je užitečné, když se děje-konvenční upgrade nebo zásad vyhodnocení stavu přizpůsobené nebo komplexní, je požadována (například aplikace je již dojít ke ztrátě dat.).</span><span class="sxs-lookup"><span data-stu-id="e7f1a-118">This mode is useful when a customized or complex health evaluation policy is required, or a non-conventional upgrade is happening (for example, the application is already in data loss).</span></span>

<span data-ttu-id="e7f1a-119">Nakonec automatizované postupného upgradu aplikace je užitečné pro vývoj nebo testování prostředí zajistit rychlou iterační cyklus během vývoje služby.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-119">Finally, the automated rolling application upgrade is useful for development or testing environments to provide a fast iteration cycle during service development.</span></span>

## <a name="change-to-manual-upgrade-mode"></a><span data-ttu-id="e7f1a-120">Změnit na režim manuálního upgradu</span><span class="sxs-lookup"><span data-stu-id="e7f1a-120">Change to manual upgrade mode</span></span>
<span data-ttu-id="e7f1a-121">**Ruční**– zastavit upgradu aplikace na aktuální UD a změňte režim upgradu na sledována ručně.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-121">**Manual**--Stop the application upgrade at the current UD and change the upgrade mode to Unmonitored Manual.</span></span> <span data-ttu-id="e7f1a-122">Správci musí ručně volání **MoveNextApplicationUpgradeDomainAsync** pokračovat v upgradu nebo aktivovat vrácení zpět pomocí inicializace nového upgradu.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-122">The administrator needs to manually call **MoveNextApplicationUpgradeDomainAsync** to proceed with the upgrade or trigger a rollback by initiating a new upgrade.</span></span> <span data-ttu-id="e7f1a-123">Po upgradu vstoupí v ručním režimu, zůstane v ručním režimu dokud nového upgradu je zahájeno.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-123">Once the upgrade enters into the Manual mode, it stays in the Manual mode until a new upgrade is initiated.</span></span> <span data-ttu-id="e7f1a-124">**GetApplicationUpgradeProgressAsync** příkaz vrátí FABRIC\_aplikace\_UPGRADE\_stavu\_kolejová\_dál\_čeká na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-124">The **GetApplicationUpgradeProgressAsync** command returns FABRIC\_APPLICATION\_UPGRADE\_STATE\_ROLLING\_FORWARD\_PENDING.</span></span>

## <a name="upgrade-with-a-diff-package"></a><span data-ttu-id="e7f1a-125">Upgrade s balíčkem rozdílů</span><span class="sxs-lookup"><span data-stu-id="e7f1a-125">Upgrade with a diff package</span></span>
<span data-ttu-id="e7f1a-126">Aplikace Service Fabric může být upgradována pomocí zřizování s balíčkem aplikace úplné, úplný a samostatný.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-126">A Service Fabric application can be upgraded by provisioning with a full, self-contained application package.</span></span> <span data-ttu-id="e7f1a-127">Aplikace lze také upgradovat pomocí rozdílové balíček, který obsahuje pouze soubory aktualizované aplikace, manifest aktualizované aplikace a soubory manifestu služby.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-127">An application can also be upgraded by using a diff package that contains only the updated application files, the updated application manifest, and the service manifest files.</span></span>

<span data-ttu-id="e7f1a-128">Celou aplikaci balíček obsahuje všechny soubory potřebné ke spuštění a spuštění aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-128">A full application package contains all the files necessary to start and run a Service Fabric application.</span></span> <span data-ttu-id="e7f1a-129">Diff balíček obsahuje pouze soubory, které změnily mezi poslední zřídit a aktuální upgrade a další manifest úplné aplikace a služby manifest soubory.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-129">A diff package contains only the files that changed between the last provision and the current upgrade, plus the full application manifest and the service manifest files.</span></span> <span data-ttu-id="e7f1a-130">Všechny odkazy v manifestu aplikace nebo služby manifestu, který nebyl nalezen v sestavení rozložení je hledán v úložišti bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-130">Any reference in the application manifest or service manifest that can't be found in the build layout is searched for in the image store.</span></span>

<span data-ttu-id="e7f1a-131">Celou aplikaci balíčky jsou požadovány pro první instalaci aplikace do clusteru.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-131">Full application packages are required for the first installation of an application to the cluster.</span></span> <span data-ttu-id="e7f1a-132">Následné aktualizace může být balíček celou aplikaci nebo balíček rozdílové.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-132">Subsequent updates can be either a full application package or a diff package.</span></span>

<span data-ttu-id="e7f1a-133">Situace při pomocí balíčku rozdílové by být vhodné použít:</span><span class="sxs-lookup"><span data-stu-id="e7f1a-133">Occasions when using a diff package would be a good choice:</span></span>

* <span data-ttu-id="e7f1a-134">Diff balíček je upřednostňované, když máte velké aplikace balíček, který odkazuje na několik souborů manifestu služby nebo několik balíčků kódu, konfigurace balíčky nebo balíčky data.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-134">A diff package is preferred when you have a large application package that references several service manifest files and/or several code packages, config packages, or data packages.</span></span>
* <span data-ttu-id="e7f1a-135">Diff balíčku jsou upřednostňované, když máte nasazení systému, který generuje rozložení sestavení přímo z vašeho procesu sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-135">A diff package is preferred when you have a deployment system that generates the build layout directly from your application build process.</span></span> <span data-ttu-id="e7f1a-136">V tomto případě to i v případě, že kód se nezměnila, nově vytvořený sestavení získat různé kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-136">In this case, even though the code hasn't changed, newly built assemblies get a different checksum.</span></span> <span data-ttu-id="e7f1a-137">Pomocí balíčku pro celou aplikaci by vyžadují aktualizaci verze na všechny balíčky kódu.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-137">Using a full application package would require you to update the version on all code packages.</span></span> <span data-ttu-id="e7f1a-138">Pomocí balíčku rozdílů, je jenom zadat soubory, které změnily a souborů manifestu nichž došlo ke změně verze.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-138">Using a diff package, you only provide the files that changed and the manifest files where the version has changed.</span></span>

<span data-ttu-id="e7f1a-139">Při upgradu aplikace pomocí sady Visual Studio rozdílové balíček je automaticky publikován.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-139">When an application is upgraded using Visual Studio, the diff package is published automatically.</span></span> <span data-ttu-id="e7f1a-140">Vytvoření balíčku rozdílové ručně, musí být aktualizovány manifest aplikace a služby manifesty, ale pouze změněné balíčky by měl být součástí balíčku poslední aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-140">To create a diff package manually, the application manifest, and the service manifests must be updated, but only the changed packages should be included in the final application package.</span></span>

<span data-ttu-id="e7f1a-141">Například začneme následující aplikace (čísla verzí zadaná pro snadné pochopení):</span><span class="sxs-lookup"><span data-stu-id="e7f1a-141">For example, let's start with the following application (version numbers provided for ease of understanding):</span></span>

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

<span data-ttu-id="e7f1a-142">Nyní předpokládejme, chcete-li aktualizovat pouze kód balíček service1 pomocí balíčku rozdílové pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-142">Now, let's assume you wanted to update only the code package of service1 using a diff package using PowerShell.</span></span> <span data-ttu-id="e7f1a-143">Nyní aplikace aktualizovaná má následující strukturu složek:</span><span class="sxs-lookup"><span data-stu-id="e7f1a-143">Now, your updated application has the following folder structure:</span></span>

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

<span data-ttu-id="e7f1a-144">V takovém případě je aktualizovat manifest aplikace a 2.0.0, service manifest pro service1 tak, aby odrážela aktualizace balíčku kódu.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-144">In this case, you update the application manifest to 2.0.0, and the service manifest for service1 to reflect the code package update.</span></span> <span data-ttu-id="e7f1a-145">Složku balíčku aplikace by mít následující strukturu:</span><span class="sxs-lookup"><span data-stu-id="e7f1a-145">The folder for your application package would have the following structure:</span></span>

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a><span data-ttu-id="e7f1a-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7f1a-146">Next steps</span></span>
<span data-ttu-id="e7f1a-147">[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-147">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>

<span data-ttu-id="e7f1a-148">[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e7f1a-148">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>

<span data-ttu-id="e7f1a-149">Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="e7f1a-149">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>

<span data-ttu-id="e7f1a-150">Zkontrolujte upgradů aplikace kompatibilní podle naučit se používat [serializace dat](service-fabric-application-upgrade-data-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="e7f1a-150">Make your application upgrades compatible by learning how to use [Data Serialization](service-fabric-application-upgrade-data-serialization.md).</span></span>

<span data-ttu-id="e7f1a-151">Řešení běžných potíží v upgradů aplikací podle kroků v části [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e7f1a-151">Fix common problems in application upgrades by referring to the steps in [Troubleshooting Application Upgrades](service-fabric-application-upgrade-troubleshooting.md).</span></span>
