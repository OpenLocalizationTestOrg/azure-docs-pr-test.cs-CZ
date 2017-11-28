---
title: "AAA neočekávané chybě při provádění souhlasu tooan aplikace | Microsoft Docs"
description: "Popisuje chyby, které se můžou vyskytnout během procesu hello souhlas tooan aplikace a co můžete dělat o nich"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: dfee35f3a10e3cc4313109cedd972499452320f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-error-when-performing-consent-tooan-application"></a><span data-ttu-id="a2b3b-103">Neočekávaná chyba při provádění souhlasu tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="a2b3b-103">Unexpected error when performing consent tooan application</span></span>

<span data-ttu-id="a2b3b-104">Tento článek popisuje chyby, které se můžou vyskytnout během procesu hello souhlas tooan aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-104">This article discusses errors that can occur during hello process of consenting tooan application.</span></span> <span data-ttu-id="a2b3b-105">Pokud Poradce při potížích neočekávané souhlasu vyzve není obsahovat chybové zprávy naleznete na adrese [scénáře ověřování pro Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span><span class="sxs-lookup"><span data-stu-id="a2b3b-105">If you are Troubleshoot unexpected consent prompts that do not contain any error messages, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span></span>

<span data-ttu-id="a2b3b-106">Mnoho aplikací, které se integrují s Azure Active Directory vyžadují oprávnění tooaccess další prostředky ve toofunction pořadí.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-106">Many applications that integrate with Azure Active Directory require permissions tooaccess other resources in order toofunction.</span></span> <span data-ttu-id="a2b3b-107">Když tyto prostředky jsou také integrované s Azure Active Directory, je je často požadovaná pomocí hello běžné tooaccess oprávnění souhlas framework.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-107">When these resources are also integrated with Azure Active Directory, permissions tooaccess them is often requested using hello common consent framework.</span></span> 

<span data-ttu-id="a2b3b-108">Výsledkem výzva k povolení spuštění se zobrazuje, který obvykle probíhá hello poprvé, aplikace se používá, ale může dojít také při následné použití aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-108">This results in a consent prompt being displayed, which generally occurs hello first time an application is used but can also occur on a subsequent use of hello application.</span></span>

<span data-ttu-id="a2b3b-109">Určitých podmínek musí být pro oprávnění toohello tooconsent uživatele, které aplikace vyžaduje na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-109">Certain conditions must be true for a user tooconsent toohello permissions an application requires.</span></span> <span data-ttu-id="a2b3b-110">Pokud nejsou tyto podmínky splněny, může dojít k různých chybám.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-110">If these conditions are not met, various errors can occur.</span></span> <span data-ttu-id="a2b3b-111">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="a2b3b-111">These include:</span></span>

## <a name="requesting-not-authorized-permissions-error"></a><span data-ttu-id="a2b3b-112">Požaduje chybě není autorizovaný oprávnění</span><span class="sxs-lookup"><span data-stu-id="a2b3b-112">Requesting not authorized permissions error</span></span>
* <span data-ttu-id="a2b3b-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; požaduje jeden nebo více oprávnění, která není autorizovaný toogrant.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; is requesting one or more permissions that you are not authorized toogrant.</span></span> <span data-ttu-id="a2b3b-114">Obraťte se na správce, který může souhlas toothis aplikací vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-114">Contact an administrator, who can consent toothis application on your behalf.</span></span>

<span data-ttu-id="a2b3b-115">K této chybě dojde, když se uživatel, který není správcem společnosti pokusí toouse aplikace, která vyžaduje oprávnění, které můžete udělit pouze správce.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-115">This error occurs when a user who is not a company administrator attempts toouse an application that is requesting permissions that only an administrator can grant.</span></span> <span data-ttu-id="a2b3b-116">Tuto chybu můžete vyřešit správcem udělení přístupu aplikace toohello jménem své organizace.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-116">This error can be resolved by an administrator granting access toohello application on behalf of their organization.</span></span>

## <a name="policy-prevents-granting-permissions-error"></a><span data-ttu-id="a2b3b-117">Zásada brání udělení oprávnění chyby</span><span class="sxs-lookup"><span data-stu-id="a2b3b-117">Policy prevents granting permissions error</span></span>
* <span data-ttu-id="a2b3b-118">**AADSTS90093:** správce &lt;tenantDisplayName&gt; nastavil zásadu, která brání udělení &lt;název aplikace&gt; hello vyžaduje oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-118">**AADSTS90093:** An administrator of &lt;tenantDisplayName&gt; has set a policy that prevents you from granting &lt;name of app&gt; hello permissions it is requesting.</span></span> <span data-ttu-id="a2b3b-119">Obraťte se na správce systému &lt;tenantDisplayName&gt;, který můžete udělit oprávnění aplikace toothis vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-119">Contact an administrator of &lt;tenantDisplayName&gt;, who can grant permissions toothis app on your behalf.</span></span>

<span data-ttu-id="a2b3b-120">Tato chyba nastane, když správce společnosti vypne hello schopnost tooapplications tooconsent uživatele, pak pokusu toouse aplikaci, která vyžaduje souhlas uživatele bez oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-120">This error occurs when a company administrator turns off hello ability for users tooconsent tooapplications, then a non-administrator user attempts toouse an application that requires consent.</span></span> <span data-ttu-id="a2b3b-121">Tuto chybu můžete vyřešit správcem udělení přístupu aplikace toohello jménem své organizace.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-121">This error can be resolved by an administrator granting access toohello application on behalf of their organization.</span></span>

## <a name="intermittent-problem-error"></a><span data-ttu-id="a2b3b-122">Chyba občasný problém</span><span class="sxs-lookup"><span data-stu-id="a2b3b-122">Intermittent problem error</span></span>
* <span data-ttu-id="a2b3b-123">**AADSTS90090:** vypadá došlo občasný problém záznam hello oprávnění, které jste se pokusili toogrant příliš&lt;clientAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-123">**AADSTS90090:** It looks like we encountered an intermittent problem recording hello permissions you attempted toogrant too&lt;clientAppDisplayName&gt;.</span></span> <span data-ttu-id="a2b3b-124">Zkuste to znovu později.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-124">try again later.</span></span>

<span data-ttu-id="a2b3b-125">Tato chyba znamená, že došlo k problému straně přerušované služby.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-125">This error indicates that an intermittent service side issue has occurred.</span></span> <span data-ttu-id="a2b3b-126">Dají se vyřešit pokusem tooconsent toohello aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-126">It can be resolved by attempting tooconsent toohello application again.</span></span>

## <a name="resource-not-available-error"></a><span data-ttu-id="a2b3b-127">Prostředek není k dispozici – chyba</span><span class="sxs-lookup"><span data-stu-id="a2b3b-127">Resource not available error</span></span>
* <span data-ttu-id="a2b3b-128">**AADSTS65005:** aplikace hello &lt;clientAppDisplayName&gt; požadovaná oprávnění tooaccess prostředek &lt;resourceAppDisplayName&gt; není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-128">**AADSTS65005:** hello app &lt;clientAppDisplayName&gt; requested permissions tooaccess a resource &lt;resourceAppDisplayName&gt; that is not available.</span></span> 

<span data-ttu-id="a2b3b-129">Obraťte se na vývojáře aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-129">Contact hello application developer.</span></span>

##  <a name="resource-not-available-in-tenant-error"></a><span data-ttu-id="a2b3b-130">Prostředek není k dispozici v Chyba klienta</span><span class="sxs-lookup"><span data-stu-id="a2b3b-130">Resource not available in tenant error</span></span>
* <span data-ttu-id="a2b3b-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; požaduje přístup k prostředku tooa &lt;resourceAppDisplayName&gt; není k dispozici ve vaší organizaci &lt; tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; is requesting access tooa resource &lt;resourceAppDisplayName&gt; that is not available in your organization &lt;tenantDisplayName&gt;.</span></span> 

<span data-ttu-id="a2b3b-132">Ujistěte se, že tento prostředek je k dispozici nebo se obraťte na správce &lt;tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-132">Ensure that this resource is available or contact an administrator of &lt;tenantDisplayName&gt;.</span></span>

## <a name="permissions-mismatch-error"></a><span data-ttu-id="a2b3b-133">Chybová zpráva o neshodě oprávnění</span><span class="sxs-lookup"><span data-stu-id="a2b3b-133">Permissions mismatch error</span></span>
* <span data-ttu-id="a2b3b-134">**AADSTS65005:** aplikace hello požadovaný prostředek tooaccess souhlasu &lt;resourceAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-134">**AADSTS65005:** hello app requested consent tooaccess resource &lt;resourceAppDisplayName&gt;.</span></span> <span data-ttu-id="a2b3b-135">Tento požadavek se nezdařil, protože neodpovídá jak byla aplikace hello předem nakonfigurovaná během registrace aplikací.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-135">This request failed because it does not match how hello app was pre-configured during app registration.</span></span> <span data-ttu-id="a2b3b-136">Obraťte se na hello aplikace vendor.* *</span><span class="sxs-lookup"><span data-stu-id="a2b3b-136">Contact hello app vendor.**</span></span>

<span data-ttu-id="a2b3b-137">Všechny tyto chyby nastane, když aplikace hello uživatel se pokouší tooconsent toois požaduje oprávnění tooaccess prostředků aplikace, který nebyl nalezen v adresáři organizace hello (klientů).</span><span class="sxs-lookup"><span data-stu-id="a2b3b-137">These errors all occur when hello application a user is trying tooconsent toois requesting permissions tooaccess a resource application that cannot be found in hello organization’s directory (tenant).</span></span> <span data-ttu-id="a2b3b-138">Tato situace může nastat z několika důvodů:</span><span class="sxs-lookup"><span data-stu-id="a2b3b-138">This can occur for several reasons:</span></span>

-   <span data-ttu-id="a2b3b-139">Vývojář aplikace Hello klient má své aplikace nesprávně nakonfigurované, způsobuje ho toorequest přístup tooan neplatný prostředek.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-139">hello client application developer has configured their application incorrectly, causing it toorequest access tooan invalid resource.</span></span> <span data-ttu-id="a2b3b-140">Vývojář aplikace hello v takovém případě musíte aktualizovat konfiguraci hello hello klienta aplikace tooresolve tento problém.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-140">In this case, hello application developer must update hello configuration of hello client application tooresolve this issue.</span></span>

-   <span data-ttu-id="a2b3b-141">Instanční objekt reprezentující hello cílová prostředků aplikace v organizaci hello neexistuje nebo existovala v posledních hello ale byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-141">A Service Principal representing hello target resource application does not exist in hello organization, or existed in hello past but has been removed.</span></span> <span data-ttu-id="a2b3b-142">tooresolve-li tento problém, hlavní název služby pro hello prostředků aplikace musí být zřízená v organizaci hello, takže hello klientská aplikace může požádat o oprávnění tooit.</span><span class="sxs-lookup"><span data-stu-id="a2b3b-142">tooresolve this issue, a Service Principal for hello resource application must be provisioned in hello organization so hello client application can request permissions tooit.</span></span> <span data-ttu-id="a2b3b-143">To může dojít v počtu způsoby, v závislosti na typu hello aplikace, včetně:</span><span class="sxs-lookup"><span data-stu-id="a2b3b-143">This can happen in an number of ways, depending on hello type of application, including:</span></span>

    -   <span data-ttu-id="a2b3b-144">Získávání odběr pro hello prostředků aplikace (Microsoft publikované aplikace)</span><span class="sxs-lookup"><span data-stu-id="a2b3b-144">Acquiring a subscription for hello resource application (Microsoft published applications)</span></span>

    -   <span data-ttu-id="a2b3b-145">Souhlas toohello prostředků aplikace</span><span class="sxs-lookup"><span data-stu-id="a2b3b-145">Consenting toohello resource application</span></span>

    -   <span data-ttu-id="a2b3b-146">Udělení oprávnění aplikací hello prostřednictvím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a2b3b-146">Granting hello application permissions via hello Azure Portal</span></span>

    -   <span data-ttu-id="a2b3b-147">Přidání aplikace hello z hello Azure AD Application Gallery</span><span class="sxs-lookup"><span data-stu-id="a2b3b-147">Adding hello application from hello Azure AD Application Gallery</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2b3b-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2b3b-148">Next steps</span></span> 

[<span data-ttu-id="a2b3b-149">Aplikace, oprávnění a souhlasu v Azure Active Directory (koncový bod v1)</span><span class="sxs-lookup"><span data-stu-id="a2b3b-149">Apps, permissions, and consent in Azure Active Directory (v1 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[<span data-ttu-id="a2b3b-150">Obory, oprávnění a souhlasu v hello Azure Active Directory (koncový bod v2.0)</span><span class="sxs-lookup"><span data-stu-id="a2b3b-150">Scopes, permissions, and consent in hello Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


