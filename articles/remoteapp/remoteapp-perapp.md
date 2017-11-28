---
title: "aaaPublish aplikace tooindividual uživatele v kolekci Azure RemoteApp (Preview) | Microsoft Docs"
description: "Zjistěte, jak publikovat aplikace tooindividual uživatele, místo v závislosti na skupin v Azure Remoteappu."
services: remoteapp-preview
documentationcenter: 
author: piotrci
manager: mbaldwin
editor: 
ms.assetid: 1fd0539d-fa65-4ea5-a98e-0be0cf580690
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: piotrci
ms.openlocfilehash: 87b435552ddbfc2c6d03aeb01d95a44985e71f9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-tooindividual-users-in-an-azure-remoteapp-collection-preview"></a><span data-ttu-id="b06d6-103">Publikování aplikace tooindividual uživatele v kolekci Azure RemoteApp (Preview)</span><span class="sxs-lookup"><span data-stu-id="b06d6-103">Publish applications tooindividual users in an Azure RemoteApp collection (Preview)</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b06d6-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="b06d6-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="b06d6-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="b06d6-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="b06d6-106">Tento článek vysvětluje, jak toopublish aplikace tooindividual uživatele v kolekci Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b06d6-106">This article explains how toopublish applications tooindividual users in an Azure RemoteApp collection.</span></span> <span data-ttu-id="b06d6-107">Toto je nová funkce v Azure Remoteappu, aktuálně v soukromém náhledu a inovátoři tooselect k dispozici pouze pro účely vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="b06d6-107">This is new functionality in Azure RemoteApp, currently in private preview and available only tooselect early adopters for evaluation purposes.</span></span>

<span data-ttu-id="b06d6-108">Původně Azure Remoteappu povolen pouze jeden způsob publikování aplikací: hello správce publikoval aplikace z hello image a nebudou viditelné tooall uživatele v kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="b06d6-108">Originally Azure RemoteApp enabled only one way of publishing applications: hello administrator would publish apps from hello image and they would be visible tooall users in hello collection.</span></span>

<span data-ttu-id="b06d6-109">Obvyklým scénářem je tooinclude mnoho aplikací do jedné bitové kopie a nasadit jednu kolekci v náklady na správu tooreduce pořadí.</span><span class="sxs-lookup"><span data-stu-id="b06d6-109">A common scenario is tooinclude many applications in a single image and deploy one collection in order tooreduce management costs.</span></span> <span data-ttu-id="b06d6-110">Jsou často ne všechny aplikace relevantní tooall uživatelé a Správci by raději toopublish aplikace tooindividual uživatele, aby Ti pak nepotřebných aplikací ve svém kanálu aplikací.</span><span class="sxs-lookup"><span data-stu-id="b06d6-110">Oftentimes not all applications are relevant tooall users – administrators would prefer toopublish apps tooindividual users so they don’t see unnecessary applications in their application feed.</span></span>

<span data-ttu-id="b06d6-111">A právě to je nyní v Azure RemoteAppu možné – aktuálně jako omezeně dostupná funkce ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="b06d6-111">This is now possible in Azure RemoteApp – currently as a limited preview feature.</span></span> <span data-ttu-id="b06d6-112">Zde je uveden stručný přehled hello nové funkce:</span><span class="sxs-lookup"><span data-stu-id="b06d6-112">Here is a brief summary of hello new functionality:</span></span>

1. <span data-ttu-id="b06d6-113">Kolekce lze nastavit do jednoho ze dvou režimů:</span><span class="sxs-lookup"><span data-stu-id="b06d6-113">A collection can be set into one of two modes:</span></span>
   
   * <span data-ttu-id="b06d6-114">Hello původní režim kolekce, kde všichni uživatelé v kolekci vidí všechny publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="b06d6-114">hello original collection mode, where all users in a collection can see all published applications.</span></span> <span data-ttu-id="b06d6-115">Toto je výchozí režim hello.</span><span class="sxs-lookup"><span data-stu-id="b06d6-115">This is hello default mode.</span></span>
   * <span data-ttu-id="b06d6-116">Hello nový režim aplikací, kde uživatelé vidí pouze aplikace, které byly explicitně přiřazeny toothem</span><span class="sxs-lookup"><span data-stu-id="b06d6-116">hello new application mode, where users only see applications that have been explicitly assigned toothem</span></span>
2. <span data-ttu-id="b06d6-117">Momentálně hello režim aplikací hello lze povolit pouze pomocí rutin prostředí Azure RemoteApp PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b06d6-117">At hello moment hello application mode can only be enabled using Azure RemoteApp PowerShell cmdlets.</span></span>
   
   * <span data-ttu-id="b06d6-118">Pokud nelze nastavit tooapplication režim, přiřazení uživatelů v kolekci hello spravovat prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b06d6-118">When set tooapplication mode, user assignment in hello collection cannot be managed through hello Azure portal.</span></span> <span data-ttu-id="b06d6-119">Přiřazení uživatelů se toobe spravovat pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b06d6-119">User assignment has toobe managed through PowerShell cmdlets.</span></span>
3. <span data-ttu-id="b06d6-120">Uživatelům se zobrazí pouze publikovaná aplikace hello přímo toothem.</span><span class="sxs-lookup"><span data-stu-id="b06d6-120">Users will only see hello applications published directly toothem.</span></span> <span data-ttu-id="b06d6-121">Ale je stále možné pro uživatele toolaunch hello jiné aplikace, které jsou k dispozici na bitovou kopii hello podle k nim přistupovat přímo v hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="b06d6-121">However, it may still be possible for a user toolaunch hello other applications available on hello image by accessing them directly in hello operating system.</span></span>
   
   * <span data-ttu-id="b06d6-122">Tato funkce nenabízí zabezpečené uzamčení aplikací. omezuje pouze jejich viditelnost v kanálu aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="b06d6-122">This feature does not provide a secure lockdown of applications; it only limits visibility in hello application feed.</span></span>
   * <span data-ttu-id="b06d6-123">Pokud potřebujete tooisolate uživatelů z aplikací, musíte pro tento toouse samostatné kolekce.</span><span class="sxs-lookup"><span data-stu-id="b06d6-123">If you need tooisolate users from applications, you will need toouse separate collections for that.</span></span>

## <a name="how-tooget-azure-remoteapp-powershell-cmdlets"></a><span data-ttu-id="b06d6-124">Jak tooget rutin prostředí Azure RemoteApp PowerShell</span><span class="sxs-lookup"><span data-stu-id="b06d6-124">How tooget Azure RemoteApp PowerShell cmdlets</span></span>
<span data-ttu-id="b06d6-125">tootry hello novou funkci ve verzi preview, budete potřebovat toouse rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b06d6-125">tootry hello new preview functionality, you will need toouse Azure PowerShell cmdlets.</span></span> <span data-ttu-id="b06d6-126">Aktuálně není možné toouse hello Azure Management portal tooenable hello nový režim publikování aplikací.</span><span class="sxs-lookup"><span data-stu-id="b06d6-126">It is currently not possible toouse hello Azure Management portal tooenable hello new application publishing mode.</span></span>

<span data-ttu-id="b06d6-127">Nejprve zkontrolujte, zda máte hello [modul Azure PowerShell](/powershell/azure/overview) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="b06d6-127">First, make sure you have hello [Azure PowerShell module](/powershell/azure/overview) installed.</span></span>

<span data-ttu-id="b06d6-128">Pak spusťte konzolu prostředí PowerShell hello v režimu správce a spusťte hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="b06d6-128">Then launch hello PowerShell console in administrator mode and run hello following cmdlet:</span></span>

        Add-AzureAccount

<span data-ttu-id="b06d6-129">Zobrazí se výzva k zadání vašeho uživatelského jména a hesla pro Azure.</span><span class="sxs-lookup"><span data-stu-id="b06d6-129">It will prompt you for your Azure user name and password.</span></span> <span data-ttu-id="b06d6-130">Po přihlášení, bude možné toorun rutiny Azure Remoteappu pro svá předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="b06d6-130">Once signed in, you will be able toorun Azure RemoteApp cmdlets against your Azure subscriptions.</span></span>

## <a name="how-toocheck-which-mode-a-collection-is-in"></a><span data-ttu-id="b06d6-131">Jak toocheck kterém režimu kolekce je v</span><span class="sxs-lookup"><span data-stu-id="b06d6-131">How toocheck which mode a collection is in</span></span>
<span data-ttu-id="b06d6-132">Spusťte následující rutinu hello:</span><span class="sxs-lookup"><span data-stu-id="b06d6-132">Run hello following cmdlet:</span></span>

        Get-AzureRemoteAppCollection <collectionName>

![Zkontrolujte režim kolekce hello](./media/remoteapp-perapp/araacllelvel.png)

<span data-ttu-id="b06d6-134">Hello vlastnost AclLevel může mít hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b06d6-134">hello AclLevel property can have hello following values:</span></span>

* <span data-ttu-id="b06d6-135">Kolekce: hello původní režim publikování.</span><span class="sxs-lookup"><span data-stu-id="b06d6-135">Collection: hello original publishing mode.</span></span> <span data-ttu-id="b06d6-136">Všichni uživatelé vidí všechny publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="b06d6-136">All users see all published apps.</span></span>
* <span data-ttu-id="b06d6-137">Aplikace: hello nový režim publikování.</span><span class="sxs-lookup"><span data-stu-id="b06d6-137">Application: hello new publishing mode.</span></span> <span data-ttu-id="b06d6-138">Uživatelé vidí pouze aplikace hello publikovat přímo toothem.</span><span class="sxs-lookup"><span data-stu-id="b06d6-138">Users see only hello apps published directly toothem.</span></span>

## <a name="how-tooswitch-tooapplication-publishing-mode"></a><span data-ttu-id="b06d6-139">Jak režim publikování tooapplication tooswitch</span><span class="sxs-lookup"><span data-stu-id="b06d6-139">How tooswitch tooapplication publishing mode</span></span>
<span data-ttu-id="b06d6-140">Spusťte následující rutinu hello:</span><span class="sxs-lookup"><span data-stu-id="b06d6-140">Run hello following cmdlet:</span></span>

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

<span data-ttu-id="b06d6-141">Stav publikování jednotlivých aplikací bude zachován: původně všichni uživatelé uvidí všechny původně publikované aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b06d6-141">Application publishing state will be preserved: initially all users will see all of hello original published apps.</span></span>

## <a name="how-toolist-users-who-can-see-a-specific-application"></a><span data-ttu-id="b06d6-142">Jak toolist uživatelé, kteří mohou vidět konkrétní aplikaci</span><span class="sxs-lookup"><span data-stu-id="b06d6-142">How toolist users who can see a specific application</span></span>
<span data-ttu-id="b06d6-143">Spusťte následující rutinu hello:</span><span class="sxs-lookup"><span data-stu-id="b06d6-143">Run hello following cmdlet:</span></span>

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

<span data-ttu-id="b06d6-144">Rutina Vypíše seznam všech uživatelů, kteří mohou vidět hello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b06d6-144">This lists all users who can see hello application.</span></span>

<span data-ttu-id="b06d6-145">Poznámka: Zobrazí se hello aliasy aplikací (nazývané v hello syntaxi výše "app alias") spuštěním příkazu Get-AzureRemoteAppProgram - Názevkolekce <collectionName>.</span><span class="sxs-lookup"><span data-stu-id="b06d6-145">Note: You can see hello application aliases (called "app alias" in hello syntax above) by running Get-AzureRemoteAppProgram -CollectionName <collectionName>.</span></span>

## <a name="how-tooassign-an-application-tooa-user"></a><span data-ttu-id="b06d6-146">Jak tooassign uživatel tooa aplikace</span><span class="sxs-lookup"><span data-stu-id="b06d6-146">How tooassign an application tooa user</span></span>
<span data-ttu-id="b06d6-147">Spusťte následující rutinu hello:</span><span class="sxs-lookup"><span data-stu-id="b06d6-147">Run hello following cmdlet:</span></span>

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

<span data-ttu-id="b06d6-148">Hello uživatel nyní uvidí aplikaci hello v klientovi Azure Remoteappu hello a bude moct tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="b06d6-148">hello user will now see hello application in hello Azure RemoteApp client and will be able tooconnect tooit.</span></span>

## <a name="how-tooremove-an-application-from-a-user"></a><span data-ttu-id="b06d6-149">Jak tooremove aplikace od uživatele.</span><span class="sxs-lookup"><span data-stu-id="b06d6-149">How tooremove an application from a user</span></span>
<span data-ttu-id="b06d6-150">Spusťte následující rutinu hello:</span><span class="sxs-lookup"><span data-stu-id="b06d6-150">Run hello following cmdlet:</span></span>

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a><span data-ttu-id="b06d6-151">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="b06d6-151">Providing feedback</span></span>
<span data-ttu-id="b06d6-152">Oceníme vaše názory a návrhy týkající se této funkce ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="b06d6-152">We appreciate your feedback and suggestions regarding this preview feature.</span></span> <span data-ttu-id="b06d6-153">Vyplňte prosím hello [průzkum](http://www.instant.ly/s/FDdrb) toolet nám vědět, co si myslíte.</span><span class="sxs-lookup"><span data-stu-id="b06d6-153">Please fill out hello [survey](http://www.instant.ly/s/FDdrb) toolet us know what you think.</span></span>

## <a name="havent-had-a-chance-tootry-hello-preview-feature"></a><span data-ttu-id="b06d6-154">Neměli jste možnost tootry hello Ukázková funkce?</span><span class="sxs-lookup"><span data-stu-id="b06d6-154">Haven't had a chance tootry hello preview feature?</span></span>
<span data-ttu-id="b06d6-155">Pokud jste se dosud neúčastnili hello preview ještě, použijte prosím tento [průzkum](http://www.instant.ly/s/AY83p) toorequest přístup.</span><span class="sxs-lookup"><span data-stu-id="b06d6-155">If you have not participated in hello preview yet, please use this [survey](http://www.instant.ly/s/AY83p) toorequest access.</span></span>

