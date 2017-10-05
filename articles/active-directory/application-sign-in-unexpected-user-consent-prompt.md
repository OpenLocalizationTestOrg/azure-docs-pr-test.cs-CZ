---
title: "Neočekávané souhlasu řádku při přihlášení k aplikaci | Microsoft Docs"
description: "Řešení potíží, když uživatel vidí výzva k povolení spuštění pro aplikace, které mají integrované s Azure AD, která nepoznáváte"
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
ms.openlocfilehash: e5b823e1251a7221f73efe6838d439f827f9665d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a><span data-ttu-id="7dd4e-103">Neočekávané souhlasu řádku při přihlášení k aplikaci</span><span class="sxs-lookup"><span data-stu-id="7dd4e-103">Unexpected consent prompt when signing in to an application</span></span>

<span data-ttu-id="7dd4e-104">Mnoho aplikací, které se integrují s Azure Active Directory vyžaduje oprávnění k různým prostředkům, aby bylo možné spustit.</span><span class="sxs-lookup"><span data-stu-id="7dd4e-104">Many applications that integrate with Azure Active Directory require permissions to various resources in order to run.</span></span> <span data-ttu-id="7dd4e-105">Když tyto prostředky jsou také integrované s Azure Active Directory, oprávnění pro přístup k nim je požadována pomocí rozhraní Azure AD souhlasu.</span><span class="sxs-lookup"><span data-stu-id="7dd4e-105">When these resources are also integrated with Azure Active Directory, permissions to access them is requested using the Azure AD consent framework.</span></span> 

<span data-ttu-id="7dd4e-106">To vede souhlasu výzva se zobrazí při prvním aplikaci používají, což je často o jednorázovou operaci.</span><span class="sxs-lookup"><span data-stu-id="7dd4e-106">This results in a consent prompt being shown the first time an application is used, which is often a one-time operation.</span></span> 

## <a name="scenarios-in-which-users-see-consent-prompts"></a><span data-ttu-id="7dd4e-107">Scénáře, ve kterých se uživatelům zobrazí souhlas výzvy</span><span class="sxs-lookup"><span data-stu-id="7dd4e-107">Scenarios in which users see consent prompts</span></span>

<span data-ttu-id="7dd4e-108">Další výzvy je možné očekávat v různých situacích:</span><span class="sxs-lookup"><span data-stu-id="7dd4e-108">Additional prompts can be expected in various scenarios:</span></span>

* <span data-ttu-id="7dd4e-109">Změnily se sadu oprávnění, které jsou požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="7dd4e-109">The set of permissions required by the application have changed.</span></span>

* <span data-ttu-id="7dd4e-110">Uživatel, který původně souhlasu ze strany aplikace nebyla správce a nyní různých (bez oprávnění správce) uživatel používá aplikaci poprvé.</span><span class="sxs-lookup"><span data-stu-id="7dd4e-110">The user who originally consented to the application was not an administrator, and now a different (non-admin) User is using the application for the first time.</span></span>

* <span data-ttu-id="7dd4e-111">Uživatel, který původně dá souhlas k aplikaci byl správce, ale jejich souhlas jménem aplikace v celé organizaci.</span><span class="sxs-lookup"><span data-stu-id="7dd4e-111">The user who originally consented to the application was an administrator, but they did not consent on-behalf of the entire organization.</span></span>

* <span data-ttu-id="7dd4e-112">Aplikace používá [přírůstkové a dynamické souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) s žádostí o další oprávnění po začátku udělen souhlas.</span><span class="sxs-lookup"><span data-stu-id="7dd4e-112">The application is using [incremental and dynamic consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) to request additional permissions after consent was initially granted.</span></span> <span data-ttu-id="7dd4e-113">To se často používá, pokud volitelné funkce další aplikace vyžadují oprávnění nad rámec těch, vyžaduje se pro základní funkce.</span><span class="sxs-lookup"><span data-stu-id="7dd4e-113">This is often used when optional features of an application additional require permissions beyond those required for baseline functionality.</span></span>

* <span data-ttu-id="7dd4e-114">Po udělení původně byl odvolán souhlasu.</span><span class="sxs-lookup"><span data-stu-id="7dd4e-114">Consent was revoked after being granted initially.</span></span>

* <span data-ttu-id="7dd4e-115">Vývojář nakonfiguroval aplikaci tak, aby vyžadovala výzva k povolení spuštění pokaždé, když se používá (Poznámka: Toto není vhodné).</span><span class="sxs-lookup"><span data-stu-id="7dd4e-115">The developer has configured the application to require a consent prompt every time it is used (note: this is not best practice).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7dd4e-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7dd4e-116">Next steps</span></span>

-   [<span data-ttu-id="7dd4e-117">Aplikace, oprávnění a souhlasu v Azure Active Directory (koncový bod verze 1.0)</span><span class="sxs-lookup"><span data-stu-id="7dd4e-117">Apps, permissions, and consent in Azure Active Directory (v1.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [<span data-ttu-id="7dd4e-118">Obory, oprávnění a souhlasu ve službě Azure Active Directory (koncový bod v2.0)</span><span class="sxs-lookup"><span data-stu-id="7dd4e-118">Scopes, permissions, and consent in the Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


