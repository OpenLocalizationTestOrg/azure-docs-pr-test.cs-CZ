---
title: "Neočekávaná chyba při provádění souhlasu pro aplikaci | Microsoft Docs"
description: "Popisuje chyby, které se můžou vyskytnout během procesu souhlasit s aplikace a co můžete dělat o nich"
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
ms.openlocfilehash: fd500fdd4c8642bad96dcf71eebcf1fad461a35f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a><span data-ttu-id="8d3a4-103">Neočekávaná chyba při provádění souhlas k aplikaci</span><span class="sxs-lookup"><span data-stu-id="8d3a4-103">Unexpected error when performing consent to an application</span></span>

<span data-ttu-id="8d3a4-104">Tento článek popisuje chyby, které se můžou vyskytnout během procesu souhlasit s aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-104">This article discusses errors that can occur during the process of consenting to an application.</span></span> <span data-ttu-id="8d3a4-105">Pokud Poradce při potížích neočekávané souhlasu vyzve není obsahovat chybové zprávy naleznete na adrese [scénáře ověřování pro Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span><span class="sxs-lookup"><span data-stu-id="8d3a4-105">If you are Troubleshoot unexpected consent prompts that do not contain any error messages, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span></span>

<span data-ttu-id="8d3a4-106">Mnoho aplikací, které se integrují s Azure Active Directory vyžadují oprávnění pro přístup k jiným prostředkům, aby bylo možné fungovat.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-106">Many applications that integrate with Azure Active Directory require permissions to access other resources in order to function.</span></span> <span data-ttu-id="8d3a4-107">Když tyto prostředky jsou také integrované s Azure Active Directory, oprávnění pro přístup k nim je často požadovaná pomocí společného rámce souhlasu.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-107">When these resources are also integrated with Azure Active Directory, permissions to access them is often requested using the common consent framework.</span></span> 

<span data-ttu-id="8d3a4-108">To vede výzva k povolení spuštění se zobrazuje, který obvykle probíhá poprvé aplikace se používá, ale může dojít také při následné použití aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-108">This results in a consent prompt being displayed, which generally occurs the first time an application is used but can also occur on a subsequent use of the application.</span></span>

<span data-ttu-id="8d3a4-109">Určitých podmínek musí být pro uživatele a oprávnění, která aplikace vyžaduje souhlas na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-109">Certain conditions must be true for a user to consent to the permissions an application requires.</span></span> <span data-ttu-id="8d3a4-110">Pokud nejsou tyto podmínky splněny, může dojít k různých chybám.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-110">If these conditions are not met, various errors can occur.</span></span> <span data-ttu-id="8d3a4-111">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="8d3a4-111">These include:</span></span>

## <a name="requesting-not-authorized-permissions-error"></a><span data-ttu-id="8d3a4-112">Požaduje chybě není autorizovaný oprávnění</span><span class="sxs-lookup"><span data-stu-id="8d3a4-112">Requesting not authorized permissions error</span></span>
* <span data-ttu-id="8d3a4-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; požaduje jeden nebo více oprávnění, která jste neautorizovaných udělit.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; is requesting one or more permissions that you are not authorized to grant.</span></span> <span data-ttu-id="8d3a4-114">Obraťte se na správce, který můžete vyjádřit souhlas. k této aplikaci vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-114">Contact an administrator, who can consent to this application on your behalf.</span></span>

<span data-ttu-id="8d3a4-115">K této chybě dojde, když se uživatel, který není správcem společnosti se pokusí použít aplikaci, která vyžaduje oprávnění, které můžete udělit pouze správce.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-115">This error occurs when a user who is not a company administrator attempts to use an application that is requesting permissions that only an administrator can grant.</span></span> <span data-ttu-id="8d3a4-116">Tuto chybu můžete vyřešit správcem udělení přístupu k aplikaci jménem své organizace.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-116">This error can be resolved by an administrator granting access to the application on behalf of their organization.</span></span>

## <a name="policy-prevents-granting-permissions-error"></a><span data-ttu-id="8d3a4-117">Zásada brání udělení oprávnění chyby</span><span class="sxs-lookup"><span data-stu-id="8d3a4-117">Policy prevents granting permissions error</span></span>
* <span data-ttu-id="8d3a4-118">**AADSTS90093:** správce &lt;tenantDisplayName&gt; nastavil zásadu, která brání udělení &lt;název aplikace&gt; vyžaduje oprávnění.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-118">**AADSTS90093:** An administrator of &lt;tenantDisplayName&gt; has set a policy that prevents you from granting &lt;name of app&gt; the permissions it is requesting.</span></span> <span data-ttu-id="8d3a4-119">Obraťte se na správce systému &lt;tenantDisplayName&gt;, který můžete udělit oprávnění k této aplikaci vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-119">Contact an administrator of &lt;tenantDisplayName&gt;, who can grant permissions to this app on your behalf.</span></span>

<span data-ttu-id="8d3a4-120">K této chybě dojde, když správce společnosti dojde k vypnutí možnosti pro uživatele o souhlas pro aplikace, pak uživatel není správcem se pokusí použít aplikaci, která vyžaduje souhlas.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-120">This error occurs when a company administrator turns off the ability for users to consent to applications, then a non-administrator user attempts to use an application that requires consent.</span></span> <span data-ttu-id="8d3a4-121">Tuto chybu můžete vyřešit správcem udělení přístupu k aplikaci jménem své organizace.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-121">This error can be resolved by an administrator granting access to the application on behalf of their organization.</span></span>

## <a name="intermittent-problem-error"></a><span data-ttu-id="8d3a4-122">Chyba občasný problém</span><span class="sxs-lookup"><span data-stu-id="8d3a4-122">Intermittent problem error</span></span>
* <span data-ttu-id="8d3a4-123">**AADSTS90090:** vypadá došlo občasný problém záznam jste se pokusili udělte oprávnění &lt;clientAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-123">**AADSTS90090:** It looks like we encountered an intermittent problem recording the permissions you attempted to grant to &lt;clientAppDisplayName&gt;.</span></span> <span data-ttu-id="8d3a4-124">Zkuste to znovu později.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-124">try again later.</span></span>

<span data-ttu-id="8d3a4-125">Tato chyba znamená, že došlo k problému straně přerušované služby.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-125">This error indicates that an intermittent service side issue has occurred.</span></span> <span data-ttu-id="8d3a4-126">Dají se vyřešit pokusem o souhlas aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-126">It can be resolved by attempting to consent to the application again.</span></span>

## <a name="resource-not-available-error"></a><span data-ttu-id="8d3a4-127">Prostředek není k dispozici – chyba</span><span class="sxs-lookup"><span data-stu-id="8d3a4-127">Resource not available error</span></span>
* <span data-ttu-id="8d3a4-128">**AADSTS65005:** aplikace &lt;clientAppDisplayName&gt; požadovaná oprávnění pro přístup k prostředkům &lt;resourceAppDisplayName&gt; není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-128">**AADSTS65005:** The app &lt;clientAppDisplayName&gt; requested permissions to access a resource &lt;resourceAppDisplayName&gt; that is not available.</span></span> 

<span data-ttu-id="8d3a4-129">Obraťte se na vývojáře aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-129">Contact the application developer.</span></span>

##  <a name="resource-not-available-in-tenant-error"></a><span data-ttu-id="8d3a4-130">Prostředek není k dispozici v Chyba klienta</span><span class="sxs-lookup"><span data-stu-id="8d3a4-130">Resource not available in tenant error</span></span>
* <span data-ttu-id="8d3a4-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; požaduje přístup k prostředku &lt;resourceAppDisplayName&gt; není k dispozici ve vaší organizaci &lt; tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; is requesting access to a resource &lt;resourceAppDisplayName&gt; that is not available in your organization &lt;tenantDisplayName&gt;.</span></span> 

<span data-ttu-id="8d3a4-132">Ujistěte se, že tento prostředek je k dispozici nebo se obraťte na správce &lt;tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-132">Ensure that this resource is available or contact an administrator of &lt;tenantDisplayName&gt;.</span></span>

## <a name="permissions-mismatch-error"></a><span data-ttu-id="8d3a4-133">Chybová zpráva o neshodě oprávnění</span><span class="sxs-lookup"><span data-stu-id="8d3a4-133">Permissions mismatch error</span></span>
* <span data-ttu-id="8d3a4-134">**AADSTS65005:** aplikace požadovaný souhlas k přístupu k prostředku &lt;resourceAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-134">**AADSTS65005:** The app requested consent to access resource &lt;resourceAppDisplayName&gt;.</span></span> <span data-ttu-id="8d3a4-135">Tento požadavek se nezdařil, protože neodpovídá jak byla předem nakonfigurovaná aplikace při registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-135">This request failed because it does not match how the app was pre-configured during app registration.</span></span> <span data-ttu-id="8d3a4-136">Obraťte se na aplikace vendor.* *</span><span class="sxs-lookup"><span data-stu-id="8d3a4-136">Contact the app vendor.**</span></span>

<span data-ttu-id="8d3a4-137">Tyto chyby, které všechny dojít při pokusu aplikace uživatele o souhlas vyžaduje oprávnění pro přístup k aplikaci prostředku, který nebyl nalezen v adresáři organizace (klientů).</span><span class="sxs-lookup"><span data-stu-id="8d3a4-137">These errors all occur when the application a user is trying to consent to is requesting permissions to access a resource application that cannot be found in the organization’s directory (tenant).</span></span> <span data-ttu-id="8d3a4-138">Tato situace může nastat z několika důvodů:</span><span class="sxs-lookup"><span data-stu-id="8d3a4-138">This can occur for several reasons:</span></span>

-   <span data-ttu-id="8d3a4-139">Vývojář aplikace klienta má své aplikace nesprávně nakonfigurované, způsobuje ho chcete požadovat přístup k je neplatný prostředek.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-139">The client application developer has configured their application incorrectly, causing it to request access to an invalid resource.</span></span> <span data-ttu-id="8d3a4-140">V takovém případě musí vývojář aplikace aktualizovat konfiguraci aplikace klienta, chcete-li vyřešit tento problém.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-140">In this case, the application developer must update the configuration of the client application to resolve this issue.</span></span>

-   <span data-ttu-id="8d3a4-141">Instanční objekt reprezentující cílová prostředků aplikace v organizaci, neexistuje nebo existovalo v minulosti, ale byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-141">A Service Principal representing the target resource application does not exist in the organization, or existed in the past but has been removed.</span></span> <span data-ttu-id="8d3a4-142">Chcete-li vyřešit tento problém, musí být zřízená objekt služby pro aplikaci prostředků v organizaci, tak klientské aplikace můžete požádat o oprávnění k němu.</span><span class="sxs-lookup"><span data-stu-id="8d3a4-142">To resolve this issue, a Service Principal for the resource application must be provisioned in the organization so the client application can request permissions to it.</span></span> <span data-ttu-id="8d3a4-143">To může dojít v počtu způsoby, v závislosti na typu aplikace, včetně:</span><span class="sxs-lookup"><span data-stu-id="8d3a4-143">This can happen in an number of ways, depending on the type of application, including:</span></span>

    -   <span data-ttu-id="8d3a4-144">Získávání předplatné prostředků aplikace (Microsoft publikované aplikace)</span><span class="sxs-lookup"><span data-stu-id="8d3a4-144">Acquiring a subscription for the resource application (Microsoft published applications)</span></span>

    -   <span data-ttu-id="8d3a4-145">Souhlas prostředků aplikace</span><span class="sxs-lookup"><span data-stu-id="8d3a4-145">Consenting to the resource application</span></span>

    -   <span data-ttu-id="8d3a4-146">Udělení oprávnění aplikací prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8d3a4-146">Granting the application permissions via the Azure Portal</span></span>

    -   <span data-ttu-id="8d3a4-147">Přidání aplikace z Azure AD Application Gallery</span><span class="sxs-lookup"><span data-stu-id="8d3a4-147">Adding the application from the Azure AD Application Gallery</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d3a4-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8d3a4-148">Next steps</span></span> 

[<span data-ttu-id="8d3a4-149">Aplikace, oprávnění a souhlasu v Azure Active Directory (koncový bod v1)</span><span class="sxs-lookup"><span data-stu-id="8d3a4-149">Apps, permissions, and consent in Azure Active Directory (v1 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[<span data-ttu-id="8d3a4-150">Obory, oprávnění a souhlasu ve službě Azure Active Directory (koncový bod v2.0)</span><span class="sxs-lookup"><span data-stu-id="8d3a4-150">Scopes, permissions, and consent in the Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


