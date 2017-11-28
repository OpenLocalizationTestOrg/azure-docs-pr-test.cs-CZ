---
title: "aaaUnexpected výzva k povolení spuštění při přihlašování v aplikaci tooan | Microsoft Docs"
description: "Jak tootroubleshoot, když uživatel vidí výzva k povolení spuštění pro aplikaci jste spojili s Azure AD, která nepoznáváte"
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
ms.openlocfilehash: 32b7a5e6256faee0dcfe2e1644da3d3428812d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-consent-prompt-when-signing-in-tooan-application"></a><span data-ttu-id="400f6-103">Neočekávané souhlasu řádku při přihlášení tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="400f6-103">Unexpected consent prompt when signing in tooan application</span></span>

<span data-ttu-id="400f6-104">Mnoho aplikací, které se integrují s Azure Active Directory vyžadují oprávnění toovarious prostředky v toorun pořadí.</span><span class="sxs-lookup"><span data-stu-id="400f6-104">Many applications that integrate with Azure Active Directory require permissions toovarious resources in order toorun.</span></span> <span data-ttu-id="400f6-105">Když tyto prostředky jsou také integrované s Azure Active Directory, je je požadována pomocí hello Azure AD tooaccess oprávnění souhlas framework.</span><span class="sxs-lookup"><span data-stu-id="400f6-105">When these resources are also integrated with Azure Active Directory, permissions tooaccess them is requested using hello Azure AD consent framework.</span></span> 

<span data-ttu-id="400f6-106">Výsledkem je výzva souhlasu se zobrazí hello poprvé, aplikace se používá, což je často o jednorázovou operaci.</span><span class="sxs-lookup"><span data-stu-id="400f6-106">This results in a consent prompt being shown hello first time an application is used, which is often a one-time operation.</span></span> 

## <a name="scenarios-in-which-users-see-consent-prompts"></a><span data-ttu-id="400f6-107">Scénáře, ve kterých se uživatelům zobrazí souhlas výzvy</span><span class="sxs-lookup"><span data-stu-id="400f6-107">Scenarios in which users see consent prompts</span></span>

<span data-ttu-id="400f6-108">Další výzvy je možné očekávat v různých situacích:</span><span class="sxs-lookup"><span data-stu-id="400f6-108">Additional prompts can be expected in various scenarios:</span></span>

* <span data-ttu-id="400f6-109">Hello sadu oprávnění vyžaduje aplikace hello změnily.</span><span class="sxs-lookup"><span data-stu-id="400f6-109">hello set of permissions required by hello application have changed.</span></span>

* <span data-ttu-id="400f6-110">Hello uživatele, který původně dá souhlas toohello aplikace nebyla správce a teď uživatel různých (bez oprávnění správce) používá hello aplikace pro hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="400f6-110">hello user who originally consented toohello application was not an administrator, and now a different (non-admin) User is using hello application for hello first time.</span></span>

* <span data-ttu-id="400f6-111">Hello uživatele, který původně dá souhlas toohello aplikace byla správce, ale jejich souhlas jménem aplikace hello celé organizace.</span><span class="sxs-lookup"><span data-stu-id="400f6-111">hello user who originally consented toohello application was an administrator, but they did not consent on-behalf of hello entire organization.</span></span>

* <span data-ttu-id="400f6-112">aplikace Hello používá [přírůstkové a dynamické souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) toorequest další oprávnění po začátku udělen souhlas.</span><span class="sxs-lookup"><span data-stu-id="400f6-112">hello application is using [incremental and dynamic consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) toorequest additional permissions after consent was initially granted.</span></span> <span data-ttu-id="400f6-113">To se často používá, pokud volitelné funkce další aplikace vyžadují oprávnění nad rámec těch, vyžaduje se pro základní funkce.</span><span class="sxs-lookup"><span data-stu-id="400f6-113">This is often used when optional features of an application additional require permissions beyond those required for baseline functionality.</span></span>

* <span data-ttu-id="400f6-114">Po udělení původně byl odvolán souhlasu.</span><span class="sxs-lookup"><span data-stu-id="400f6-114">Consent was revoked after being granted initially.</span></span>

* <span data-ttu-id="400f6-115">Hello vývojáře nakonfiguroval toorequire aplikace hello výzva k povolení spuštění pokaždé, když se používá (Poznámka: Toto není vhodné).</span><span class="sxs-lookup"><span data-stu-id="400f6-115">hello developer has configured hello application toorequire a consent prompt every time it is used (note: this is not best practice).</span></span>

## <a name="next-steps"></a><span data-ttu-id="400f6-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="400f6-116">Next steps</span></span>

-   [<span data-ttu-id="400f6-117">Aplikace, oprávnění a souhlasu v Azure Active Directory (koncový bod verze 1.0)</span><span class="sxs-lookup"><span data-stu-id="400f6-117">Apps, permissions, and consent in Azure Active Directory (v1.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [<span data-ttu-id="400f6-118">Obory, oprávnění a souhlasu v hello Azure Active Directory (koncový bod v2.0)</span><span class="sxs-lookup"><span data-stu-id="400f6-118">Scopes, permissions, and consent in hello Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


