---
title: "životnost tokenu hello aaaHow toochange výchozí nastavení pro aplikaci zákaznických | Microsoft Docs"
description: "Jak tooupdate životnost tokenu zásady pro aplikace, které vyvíjíte na Azure AD"
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
ms.openlocfilehash: 6e1aa1f2a7c33c1f55c5fb619c618ad43cd96273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochange-hello-token-lifetime-defaults-for-a-custom-developed-application"></a><span data-ttu-id="5c01b-103">Jak životnost tokenu hello toochange výchozí nastavení pro aplikace vyvinuté vlastní</span><span class="sxs-lookup"><span data-stu-id="5c01b-103">How toochange hello token lifetime defaults for a custom-developed application</span></span>

<span data-ttu-id="5c01b-104">Azure AD Premium umožňuje vývojáři aplikací a klienta admins tooconfigure hello životnost tokeny vydané pro-důvěrné klienty.</span><span class="sxs-lookup"><span data-stu-id="5c01b-104">Azure AD Premium allows app developers and tenant admins tooconfigure hello lifetime of tokens issued for non-confidential clients.</span></span> <span data-ttu-id="5c01b-105">Životnost tokenu zásady jsou nastaveny na základě klienta celou nebo hello prostředkům přistupuje.</span><span class="sxs-lookup"><span data-stu-id="5c01b-105">Token lifetime policies are set on a tenant-wide basis or hello resources being accessed.</span></span>

 * <span data-ttu-id="5c01b-106">tooset zásadu životnost tokenu, je nutné toodownload hello [modulu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).</span><span class="sxs-lookup"><span data-stu-id="5c01b-106">tooset a token lifetime policy, you need toodownload hello [Azure AD PowerShell Module](https://www.powershellgallery.com/packages/AzureADPreview).</span></span>

 * <span data-ttu-id="5c01b-107">Spustit hello **Connect-AzureAD-potvrďte** příkaz.</span><span class="sxs-lookup"><span data-stu-id="5c01b-107">Run hello **Connect-AzureAD -Confirm** command.</span></span>

 * <span data-ttu-id="5c01b-108">Tady je příklad zásady, které nastavuje hello maximální stáří jeden faktor obnovovací token.</span><span class="sxs-lookup"><span data-stu-id="5c01b-108">Here’s an example policy that sets hello max age single factor refresh token.</span></span> <span data-ttu-id="5c01b-109">Vytvořte zásadu hello:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span><span class="sxs-lookup"><span data-stu-id="5c01b-109">Create hello policy: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span></span>

 * <span data-ttu-id="5c01b-110">Najdete v článku věnovaném hello [životnost tokenu konfigurace](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) dokumentu toolearn jak toocreate další vlastní.</span><span class="sxs-lookup"><span data-stu-id="5c01b-110">Checkout hello [Configuring token lifetime](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)   document toolearn how toocreate other custom.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c01b-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c01b-111">Next steps</span></span>
[<span data-ttu-id="5c01b-112">Konfigurace životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="5c01b-112">Configuring Token Lifetime</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[<span data-ttu-id="5c01b-113">Odkaz tokenu Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c01b-113">Azure AD Token Reference</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)

