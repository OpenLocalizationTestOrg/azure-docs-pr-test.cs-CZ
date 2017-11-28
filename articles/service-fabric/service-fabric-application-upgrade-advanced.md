---
title: "aaaAdvanced témata upgradu aplikace | Microsoft Docs"
description: "Tento článek se zabývá některá Pokročilá témata, která se týkají tooupgrading aplikace Service Fabric."
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
ms.openlocfilehash: bdaf3db6209c574d39f57e0bf9951fad5ad1cbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a><span data-ttu-id="9e148-103">Upgrade aplikace Service Fabric: advanced témata</span><span class="sxs-lookup"><span data-stu-id="9e148-103">Service Fabric application upgrade: advanced topics</span></span>
## <a name="adding-or-removing-services-during-an-application-upgrade"></a><span data-ttu-id="9e148-104">Přidání nebo odebrání služby během upgradu aplikace</span><span class="sxs-lookup"><span data-stu-id="9e148-104">Adding or removing services during an application upgrade</span></span>
<span data-ttu-id="9e148-105">Pokud nové služby je přidali tooan aplikace, která je již nasazen a publikovat jako upgrade, je nová služba hello přidané toohello nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e148-105">If a new service is added tooan application that is already deployed, and published as an upgrade, hello new service is added toohello deployed application.</span></span>  <span data-ttu-id="9e148-106">Takové upgrade neovlivní žádné hello služeb, které již byly součástí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9e148-106">Such an upgrade does not affect any of hello services that were already part of hello application.</span></span> <span data-ttu-id="9e148-107">Však musí být spuštěná instance hello služby, která byla přidána pro hello nové služby toobe active (pomocí hello `New-ServiceFabricService` rutiny).</span><span class="sxs-lookup"><span data-stu-id="9e148-107">However, an instance of hello service that was added must be started for hello new service toobe active (using hello `New-ServiceFabricService` cmdlet).</span></span>

<span data-ttu-id="9e148-108">Služby může být odebrán také z aplikace jako součást upgradu.</span><span class="sxs-lookup"><span data-stu-id="9e148-108">Services can also be removed from an application as part of an upgrade.</span></span> <span data-ttu-id="9e148-109">Ale musí být všechny aktuální instance služby k-be odstraněné hello zastaven před pokračováním upgradu hello (pomocí hello `Remove-ServiceFabricService` rutiny).</span><span class="sxs-lookup"><span data-stu-id="9e148-109">However, all current instances of hello to-be-deleted service must be stopped before proceeding with hello upgrade (using hello `Remove-ServiceFabricService` cmdlet).</span></span>

## <a name="manual-upgrade-mode"></a><span data-ttu-id="9e148-110">Režim manuálního upgradu</span><span class="sxs-lookup"><span data-stu-id="9e148-110">Manual upgrade mode</span></span>
> [!NOTE]
> <span data-ttu-id="9e148-111">měli byste zvážit Hello sledována ruční režim pouze pro upgrade selhání nebo pozastavený.</span><span class="sxs-lookup"><span data-stu-id="9e148-111">hello unmonitored manual mode should be considered only for a failed or suspended upgrade.</span></span> <span data-ttu-id="9e148-112">režim monitorovaných Hello je, že hello doporučená režimu upgradu pro Service Fabric aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e148-112">hello monitored mode is hello recommended upgrade mode for Service Fabric applications.</span></span>
>
>

<span data-ttu-id="9e148-113">Azure Service Fabric nabízí více upgradu režimy toosupport vývoj a produkční clustery.</span><span class="sxs-lookup"><span data-stu-id="9e148-113">Azure Service Fabric provides multiple upgrade modes toosupport development and production clusters.</span></span> <span data-ttu-id="9e148-114">Zvolené možnosti nasazení se může lišit pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e148-114">Deployment options chosen may be different for different environments.</span></span>

<span data-ttu-id="9e148-115">Hello monitorovat postupného upgradu aplikace je nejčastější upgradu toouse hello hello produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e148-115">hello monitored rolling application upgrade is hello most typical upgrade toouse in hello production environment.</span></span> <span data-ttu-id="9e148-116">Při upgradu hello zadat zásady, Service Fabric zajišťuje, že aplikace hello je v pořádku, před provedením upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="9e148-116">When hello upgrade policy is specified, Service Fabric ensures that hello application is healthy before hello upgrade proceeds.</span></span>

 <span data-ttu-id="9e148-117">Hello aplikace může správce ruční hello vrácení aplikace režimu upgradu toohave celkový kontrolu nad průběh upgradu hello prostřednictvím hello různých domén upgradu.</span><span class="sxs-lookup"><span data-stu-id="9e148-117">hello application administrator can use hello manual rolling application upgrade mode toohave total control over hello upgrade progress through hello various upgrade domains.</span></span> <span data-ttu-id="9e148-118">Tento režim je užitečné, když se děje-konvenční upgrade nebo zásad vyhodnocení stavu přizpůsobené nebo komplexní, je požadována (například hello aplikace je již dojít ke ztrátě dat.).</span><span class="sxs-lookup"><span data-stu-id="9e148-118">This mode is useful when a customized or complex health evaluation policy is required, or a non-conventional upgrade is happening (for example, hello application is already in data loss).</span></span>

<span data-ttu-id="9e148-119">Nakonec hello automatizované postupného upgradu aplikace je užitečné pro vývoj nebo testování prostředí tooprovide rychlé iterace cyklus během vývoje služby.</span><span class="sxs-lookup"><span data-stu-id="9e148-119">Finally, hello automated rolling application upgrade is useful for development or testing environments tooprovide a fast iteration cycle during service development.</span></span>

## <a name="change-toomanual-upgrade-mode"></a><span data-ttu-id="9e148-120">Změna režimu upgradu toomanual</span><span class="sxs-lookup"><span data-stu-id="9e148-120">Change toomanual upgrade mode</span></span>
<span data-ttu-id="9e148-121">**Ruční**– upgradu aplikace hello zastavit v aktuální UD a změňte hello hello upgrade režimu tooUnmonitored ručně.</span><span class="sxs-lookup"><span data-stu-id="9e148-121">**Manual**--Stop hello application upgrade at hello current UD and change hello upgrade mode tooUnmonitored Manual.</span></span> <span data-ttu-id="9e148-122">Hello musí správce volání toomanually **MoveNextApplicationUpgradeDomainAsync** tooproceed s hello upgradovat nebo aktivovat vrácení zpět pomocí inicializace nového upgradu.</span><span class="sxs-lookup"><span data-stu-id="9e148-122">hello administrator needs toomanually call **MoveNextApplicationUpgradeDomainAsync** tooproceed with hello upgrade or trigger a rollback by initiating a new upgrade.</span></span> <span data-ttu-id="9e148-123">Po upgradu hello vstoupí v ručním režimu hello, zůstane v režimu ručního hello dokud nového upgradu je zahájeno.</span><span class="sxs-lookup"><span data-stu-id="9e148-123">Once hello upgrade enters into hello Manual mode, it stays in hello Manual mode until a new upgrade is initiated.</span></span> <span data-ttu-id="9e148-124">Hello **GetApplicationUpgradeProgressAsync** příkaz vrátí FABRIC\_aplikace\_UPGRADE\_stavu\_kolejová\_dál\_čeká na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="9e148-124">hello **GetApplicationUpgradeProgressAsync** command returns FABRIC\_APPLICATION\_UPGRADE\_STATE\_ROLLING\_FORWARD\_PENDING.</span></span>

## <a name="upgrade-with-a-diff-package"></a><span data-ttu-id="9e148-125">Upgrade s balíčkem rozdílů</span><span class="sxs-lookup"><span data-stu-id="9e148-125">Upgrade with a diff package</span></span>
<span data-ttu-id="9e148-126">Aplikace Service Fabric může být upgradována pomocí zřizování s balíčkem aplikace úplné, úplný a samostatný.</span><span class="sxs-lookup"><span data-stu-id="9e148-126">A Service Fabric application can be upgraded by provisioning with a full, self-contained application package.</span></span> <span data-ttu-id="9e148-127">Aplikace lze také upgradovat pomocí rozdílové balíček, který obsahuje pouze soubory aplikace hello aktualizovat, hello aktualizovala manifest aplikace a soubory manifestu hello služby.</span><span class="sxs-lookup"><span data-stu-id="9e148-127">An application can also be upgraded by using a diff package that contains only hello updated application files, hello updated application manifest, and hello service manifest files.</span></span>

<span data-ttu-id="9e148-128">Celou aplikaci balíčku obsahuje všechny potřebné toostart hello soubory a spuštění aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9e148-128">A full application package contains all hello files necessary toostart and run a Service Fabric application.</span></span> <span data-ttu-id="9e148-129">Diff balíček obsahuje pouze hello soubory, které změnily mezi hello poslední zřídit a aktuální upgrade hello plus manifest úplné aplikace hello a hello service manifest soubory.</span><span class="sxs-lookup"><span data-stu-id="9e148-129">A diff package contains only hello files that changed between hello last provision and hello current upgrade, plus hello full application manifest and hello service manifest files.</span></span> <span data-ttu-id="9e148-130">Všechny odkazy v manifestu aplikace hello nebo manifest služby, který nebyl nalezen v sestavení rozložení hello je hledán v hello úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="9e148-130">Any reference in hello application manifest or service manifest that can't be found in hello build layout is searched for in hello image store.</span></span>

<span data-ttu-id="9e148-131">Celou aplikaci balíčky se vyžadují pro instalaci první hello clusteru toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e148-131">Full application packages are required for hello first installation of an application toohello cluster.</span></span> <span data-ttu-id="9e148-132">Následné aktualizace může být balíček celou aplikaci nebo balíček rozdílové.</span><span class="sxs-lookup"><span data-stu-id="9e148-132">Subsequent updates can be either a full application package or a diff package.</span></span>

<span data-ttu-id="9e148-133">Situace při pomocí balíčku rozdílové by být vhodné použít:</span><span class="sxs-lookup"><span data-stu-id="9e148-133">Occasions when using a diff package would be a good choice:</span></span>

* <span data-ttu-id="9e148-134">Diff balíček je upřednostňované, když máte velké aplikace balíček, který odkazuje na několik souborů manifestu služby nebo několik balíčků kódu, konfigurace balíčky nebo balíčky data.</span><span class="sxs-lookup"><span data-stu-id="9e148-134">A diff package is preferred when you have a large application package that references several service manifest files and/or several code packages, config packages, or data packages.</span></span>
* <span data-ttu-id="9e148-135">Balíček rozdílové jsou upřednostňované, když máte nasazení systém, který generuje hello sestavení rozložení přímo z vašeho procesu sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e148-135">A diff package is preferred when you have a deployment system that generates hello build layout directly from your application build process.</span></span> <span data-ttu-id="9e148-136">V takovém případě sice hello kódu se nezměnila, nově vytvořený sestavení získá různé kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="9e148-136">In this case, even though hello code hasn't changed, newly built assemblies get a different checksum.</span></span> <span data-ttu-id="9e148-137">Pomocí balíčku pro celou aplikaci by vyžadovaly jste tooupdate hello verze na všechny balíčky kódu.</span><span class="sxs-lookup"><span data-stu-id="9e148-137">Using a full application package would require you tooupdate hello version on all code packages.</span></span> <span data-ttu-id="9e148-138">Pomocí balíčku rozdílů, pouze poskytnete hello soubory, které změnily a soubory manifestu hello nichž došlo ke změně verze hello.</span><span class="sxs-lookup"><span data-stu-id="9e148-138">Using a diff package, you only provide hello files that changed and hello manifest files where hello version has changed.</span></span>

<span data-ttu-id="9e148-139">Při upgradu aplikace pomocí sady Visual Studio hello rozdílové balíček je automaticky publikován.</span><span class="sxs-lookup"><span data-stu-id="9e148-139">When an application is upgraded using Visual Studio, hello diff package is published automatically.</span></span> <span data-ttu-id="9e148-140">toocreate balíček rozdílové ručně hello manifest aplikace a manifesty hello služby musí být aktualizované, ale jenom balíčky hello změnit by měl být součástí balíčku poslední aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9e148-140">toocreate a diff package manually, hello application manifest, and hello service manifests must be updated, but only hello changed packages should be included in hello final application package.</span></span>

<span data-ttu-id="9e148-141">Například začneme hello následující aplikace (čísla verzí zadaná pro snadné pochopení):</span><span class="sxs-lookup"><span data-stu-id="9e148-141">For example, let's start with hello following application (version numbers provided for ease of understanding):</span></span>

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

<span data-ttu-id="9e148-142">Nyní předpokládejme chtěli tooupdate pouze hello kód balíček service1 pomocí balíčku rozdílové pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e148-142">Now, let's assume you wanted tooupdate only hello code package of service1 using a diff package using PowerShell.</span></span> <span data-ttu-id="9e148-143">Nyní aplikace aktualizovaná má hello následující strukturu složek:</span><span class="sxs-lookup"><span data-stu-id="9e148-143">Now, your updated application has hello following folder structure:</span></span>

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

<span data-ttu-id="9e148-144">V takovém případě je aktualizovat too2.0.0 manifestu aplikace hello a hello service manifest pro service1 tooreflect hello kód balíčku aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9e148-144">In this case, you update hello application manifest too2.0.0, and hello service manifest for service1 tooreflect hello code package update.</span></span> <span data-ttu-id="9e148-145">Hello složku balíčku aplikace by mít hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="9e148-145">hello folder for your application package would have hello following structure:</span></span>

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a><span data-ttu-id="9e148-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e148-146">Next steps</span></span>
<span data-ttu-id="9e148-147">[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e148-147">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>

<span data-ttu-id="9e148-148">[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e148-148">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>

<span data-ttu-id="9e148-149">Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="9e148-149">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>

<span data-ttu-id="9e148-150">Aby aplikace upgradů kompatibilní metodou učení jak toouse [serializace dat](service-fabric-application-upgrade-data-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="9e148-150">Make your application upgrades compatible by learning how toouse [Data Serialization](service-fabric-application-upgrade-data-serialization.md).</span></span>

<span data-ttu-id="9e148-151">Řešení běžných potíží v upgradů aplikací tím, že odkazuje toohello kroky v [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="9e148-151">Fix common problems in application upgrades by referring toohello steps in [Troubleshooting Application Upgrades](service-fabric-application-upgrade-troubleshooting.md).</span></span>
