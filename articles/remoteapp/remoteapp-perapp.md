---
title: "Publikování aplikací pro jednotlivé uživatele v kolekci Azure RemoteAppu (verze Preview) | Dokumentace Microsoftu"
description: "Přečtěte si, jak v Azure RemoteAppu publikovat aplikace místo skupin pro jednotlivé uživatele."
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
ms.openlocfilehash: c94ffffdec3e46ed08d941ee58dcf17b432e1dad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a><span data-ttu-id="2e6c4-103">Publikování aplikací pro jednotlivé uživatele v kolekci Azure RemoteAppu (verze Preview)</span><span class="sxs-lookup"><span data-stu-id="2e6c4-103">Publish applications to individual users in an Azure RemoteApp collection (Preview)</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2e6c4-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="2e6c4-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="2e6c4-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="2e6c4-106">Tento článek vysvětluje, jak publikovat aplikace pro jednotlivé uživatele v kolekci Azure RemoteAppu.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-106">This article explains how to publish applications to individual users in an Azure RemoteApp collection.</span></span> <span data-ttu-id="2e6c4-107">Toto je nová funkce v Azure RemoteAppu, která je aktuálně ve verzi Preview a je k dispozici pouze vybraným uživatelům se zájmem o nové funkce pro účely vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-107">This is new functionality in Azure RemoteApp, currently in private preview and available only to select early adopters for evaluation purposes.</span></span>

<span data-ttu-id="2e6c4-108">Původně byl v Azure RemoteAppu povolen pouze jeden způsob „publikování“ aplikací: správce publikoval aplikace z image a ty se pak staly viditelnými pro všechny uživatele v kolekci.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-108">Originally Azure RemoteApp enabled only one way of publishing applications: the administrator would publish apps from the image and they would be visible to all users in the collection.</span></span>

<span data-ttu-id="2e6c4-109">Obvyklým scénářem je zahrnout mnoho aplikací do jediného image a nasadit jednu kolekci, aby byly náklady na správu minimální.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-109">A common scenario is to include many applications in a single image and deploy one collection in order to reduce management costs.</span></span> <span data-ttu-id="2e6c4-110">Často ale nejsou všechny aplikace relevantní pro všechny uživatele a správci by raději publikovali aplikace pro jednotlivé uživatele, aby ti pak ve svém kanálu aplikací neviděli aplikace, které nepotřebují.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-110">Oftentimes not all applications are relevant to all users – administrators would prefer to publish apps to individual users so they don’t see unnecessary applications in their application feed.</span></span>

<span data-ttu-id="2e6c4-111">A právě to je nyní v Azure RemoteAppu možné – aktuálně jako omezeně dostupná funkce ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-111">This is now possible in Azure RemoteApp – currently as a limited preview feature.</span></span> <span data-ttu-id="2e6c4-112">Zde je stručný popis nové funkce:</span><span class="sxs-lookup"><span data-stu-id="2e6c4-112">Here is a brief summary of the new functionality:</span></span>

1. <span data-ttu-id="2e6c4-113">Kolekce lze nastavit do jednoho ze dvou režimů:</span><span class="sxs-lookup"><span data-stu-id="2e6c4-113">A collection can be set into one of two modes:</span></span>
   
   * <span data-ttu-id="2e6c4-114">Původní „režim kolekce“, ve kterém všichni uživatelé v kolekci vidí všechny publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-114">the original collection mode, where all users in a collection can see all published applications.</span></span> <span data-ttu-id="2e6c4-115">To je výchozí režim.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-115">This is the default mode.</span></span>
   * <span data-ttu-id="2e6c4-116">Nový „režim aplikací“, ve kterém uživatelé vidí pouze aplikace, které jim byly explicitně přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-116">the new application mode, where users only see applications that have been explicitly assigned to them</span></span>
2. <span data-ttu-id="2e6c4-117">V současnosti lze režim aplikací povolit pouze pomocí rutin prostředí Azure RemoteApp PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-117">At the moment the application mode can only be enabled using Azure RemoteApp PowerShell cmdlets.</span></span>
   
   * <span data-ttu-id="2e6c4-118">Když je nastavený režim aplikací, přiřazení uživatelů v kolekci nelze spravovat prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-118">When set to application mode, user assignment in the collection cannot be managed through the Azure portal.</span></span> <span data-ttu-id="2e6c4-119">Přiřazení uživatelů se musí spravovat pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-119">User assignment has to be managed through PowerShell cmdlets.</span></span>
3. <span data-ttu-id="2e6c4-120">Uživatelům se zobrazí pouze aplikace, které byly publikovány přímo jim.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-120">Users will only see the applications published directly to them.</span></span> <span data-ttu-id="2e6c4-121">Uživatel však stále může spustit i další aplikace, které jsou dostupné v imagi, když k nim bude přistupovat přímo přes operační systém.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-121">However, it may still be possible for a user to launch the other applications available on the image by accessing them directly in the operating system.</span></span>
   
   * <span data-ttu-id="2e6c4-122">Tato funkce nenabízí zabezpečené uzamčení aplikací. Omezuje pouze jejich viditelnost v kanálu aplikací.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-122">This feature does not provide a secure lockdown of applications; it only limits visibility in the application feed.</span></span>
   * <span data-ttu-id="2e6c4-123">Pokud potřebujete uživatele od určitých aplikací izolovat, musíte pro tento účel použít samostatné kolekce.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-123">If you need to isolate users from applications, you will need to use separate collections for that.</span></span>

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a><span data-ttu-id="2e6c4-124">Jak získat rutiny prostředí Azure RemoteApp PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e6c4-124">How to get Azure RemoteApp PowerShell cmdlets</span></span>
<span data-ttu-id="2e6c4-125">Pokud chcete vyzkoušet tuto novou funkci ve verzi Preview, budete muset použít rutiny prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-125">To try the new preview functionality, you will need to use Azure PowerShell cmdlets.</span></span> <span data-ttu-id="2e6c4-126">Nový režim publikování aplikací aktuálně není možné povolit pomocí portálu pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-126">It is currently not possible to use the Azure Management portal to enable the new application publishing mode.</span></span>

<span data-ttu-id="2e6c4-127">Nejprve zkontrolujte, zda máte nainstalovaný [modul Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2e6c4-127">First, make sure you have the [Azure PowerShell module](/powershell/azure/overview) installed.</span></span>

<span data-ttu-id="2e6c4-128">Potom spusťte konzolu PowerShellu v režimu správce a spusťte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="2e6c4-128">Then launch the PowerShell console in administrator mode and run the following cmdlet:</span></span>

        Add-AzureAccount

<span data-ttu-id="2e6c4-129">Zobrazí se výzva k zadání vašeho uživatelského jména a hesla pro Azure.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-129">It will prompt you for your Azure user name and password.</span></span> <span data-ttu-id="2e6c4-130">Jakmile se přihlásíte, budete moci spouštět rutiny Azure RemoteAppu pro svá předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-130">Once signed in, you will be able to run Azure RemoteApp cmdlets against your Azure subscriptions.</span></span>

## <a name="how-to-check-which-mode-a-collection-is-in"></a><span data-ttu-id="2e6c4-131">Jak zjistit, ve kterém režimu kolekce je</span><span class="sxs-lookup"><span data-stu-id="2e6c4-131">How to check which mode a collection is in</span></span>
<span data-ttu-id="2e6c4-132">Spusťte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="2e6c4-132">Run the following cmdlet:</span></span>

        Get-AzureRemoteAppCollection <collectionName>

![Zkontrolujte režim kolekce](./media/remoteapp-perapp/araacllelvel.png)

<span data-ttu-id="2e6c4-134">Vlastnost AclLevel může mít následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2e6c4-134">The AclLevel property can have the following values:</span></span>

* <span data-ttu-id="2e6c4-135">Collection: původní režim publikování.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-135">Collection: the original publishing mode.</span></span> <span data-ttu-id="2e6c4-136">Všichni uživatelé vidí všechny publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-136">All users see all published apps.</span></span>
* <span data-ttu-id="2e6c4-137">Application: nový režim publikování.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-137">Application: the new publishing mode.</span></span> <span data-ttu-id="2e6c4-138">Uživatelé vidí pouze aplikace, které byly publikovány přímo pro ně.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-138">Users see only the apps published directly to them.</span></span>

## <a name="how-to-switch-to-application-publishing-mode"></a><span data-ttu-id="2e6c4-139">Jak přepnout na režim publikování aplikací</span><span class="sxs-lookup"><span data-stu-id="2e6c4-139">How to switch to application publishing mode</span></span>
<span data-ttu-id="2e6c4-140">Spusťte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="2e6c4-140">Run the following cmdlet:</span></span>

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

<span data-ttu-id="2e6c4-141">Stav publikování jednotlivých aplikací bude zachován: na počátku všichni uživatelé uvidí všechny původně publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-141">Application publishing state will be preserved: initially all users will see all of the original published apps.</span></span>

## <a name="how-to-list-users-who-can-see-a-specific-application"></a><span data-ttu-id="2e6c4-142">Jak zobrazit seznam uživatelů, kteří mohou vidět konkrétní aplikaci</span><span class="sxs-lookup"><span data-stu-id="2e6c4-142">How to list users who can see a specific application</span></span>
<span data-ttu-id="2e6c4-143">Spusťte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="2e6c4-143">Run the following cmdlet:</span></span>

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

<span data-ttu-id="2e6c4-144">Rutina vypíše seznam všech uživatelů, kteří mohou vidět danou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-144">This lists all users who can see the application.</span></span>

<span data-ttu-id="2e6c4-145">Poznámka: Aliasy aplikací (v syntaxi výše „app alias“) si můžete zobrazit spuštěním příkazu Get-AzureRemoteAppProgram -NázevKolekce <collectionName>.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-145">Note: You can see the application aliases (called "app alias" in the syntax above) by running Get-AzureRemoteAppProgram -CollectionName <collectionName>.</span></span>

## <a name="how-to-assign-an-application-to-a-user"></a><span data-ttu-id="2e6c4-146">Jak přiřadit aplikaci uživateli</span><span class="sxs-lookup"><span data-stu-id="2e6c4-146">How to assign an application to a user</span></span>
<span data-ttu-id="2e6c4-147">Spusťte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="2e6c4-147">Run the following cmdlet:</span></span>

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

<span data-ttu-id="2e6c4-148">Uživatel nyní uvidí aplikaci v klientovi Azure RemoteAppu a bude se k ní moci připojit.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-148">The user will now see the application in the Azure RemoteApp client and will be able to connect to it.</span></span>

## <a name="how-to-remove-an-application-from-a-user"></a><span data-ttu-id="2e6c4-149">Jak odebrat aplikaci uživateli</span><span class="sxs-lookup"><span data-stu-id="2e6c4-149">How to remove an application from a user</span></span>
<span data-ttu-id="2e6c4-150">Spusťte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="2e6c4-150">Run the following cmdlet:</span></span>

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a><span data-ttu-id="2e6c4-151">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="2e6c4-151">Providing feedback</span></span>
<span data-ttu-id="2e6c4-152">Oceníme vaše názory a návrhy týkající se této funkce ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-152">We appreciate your feedback and suggestions regarding this preview feature.</span></span> <span data-ttu-id="2e6c4-153">Vyplňte prosím náš [průzkum](http://www.instant.ly/s/FDdrb) a dejte nám vědět, co si myslíte.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-153">Please fill out the [survey](http://www.instant.ly/s/FDdrb) to let us know what you think.</span></span>

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a><span data-ttu-id="2e6c4-154">Neměli jste možnost vyzkoušet si funkci ve verzi Preview?</span><span class="sxs-lookup"><span data-stu-id="2e6c4-154">Haven't had a chance to try the preview feature?</span></span>
<span data-ttu-id="2e6c4-155">Pokud jste se dosud neúčastnili žádného vyhodnocování verzí Preview, použijte prosím tento [průzkum](http://www.instant.ly/s/AY83p) a požádejte o přístup.</span><span class="sxs-lookup"><span data-stu-id="2e6c4-155">If you have not participated in the preview yet, please use this [survey](http://www.instant.ly/s/AY83p) to request access.</span></span>

