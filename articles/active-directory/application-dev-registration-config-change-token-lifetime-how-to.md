---
title: "Jak změnit dobu životnosti tokenu výchozí nastavení pro aplikaci zákaznických | Microsoft Docs"
description: "Postup aktualizace životnost tokenu zásady pro aplikace, které vyvíjíte na Azure AD"
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
ms.openlocfilehash: a28eacd820ed28a6470992ce86b060e886c00bcb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a><span data-ttu-id="8c592-103">Jak změnit výchozí dobu životnosti tokenu aplikace vyvinuté vlastní</span><span class="sxs-lookup"><span data-stu-id="8c592-103">How to change the token lifetime defaults for a custom-developed application</span></span>

<span data-ttu-id="8c592-104">Azure AD Premium umožňuje vývojáři aplikace a správci klientů nakonfigurovat životnost tokeny vydané pro-důvěrné klienty.</span><span class="sxs-lookup"><span data-stu-id="8c592-104">Azure AD Premium allows app developers and tenant admins to configure the lifetime of tokens issued for non-confidential clients.</span></span> <span data-ttu-id="8c592-105">Životnost tokenu zásady jsou nastaveny na základě klienta celou nebo prostředkům přistupuje.</span><span class="sxs-lookup"><span data-stu-id="8c592-105">Token lifetime policies are set on a tenant-wide basis or the resources being accessed.</span></span>

 * <span data-ttu-id="8c592-106">Pokud chcete nastavit dobu životnosti tokenu zásady, budete muset stáhnout [modulu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).</span><span class="sxs-lookup"><span data-stu-id="8c592-106">To set a token lifetime policy, you need to download the [Azure AD PowerShell Module](https://www.powershellgallery.com/packages/AzureADPreview).</span></span>

 * <span data-ttu-id="8c592-107">Spustit **Connect-AzureAD-potvrďte** příkaz.</span><span class="sxs-lookup"><span data-stu-id="8c592-107">Run the **Connect-AzureAD -Confirm** command.</span></span>

 * <span data-ttu-id="8c592-108">Tady je příklad zásady, která nastaví maximální stáří jeden faktor obnovovací token.</span><span class="sxs-lookup"><span data-stu-id="8c592-108">Here’s an example policy that sets the max age single factor refresh token.</span></span> <span data-ttu-id="8c592-109">Vytvořte zásadu:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span><span class="sxs-lookup"><span data-stu-id="8c592-109">Create the policy: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span></span>

 * <span data-ttu-id="8c592-110">Najdete v článku věnovaném [životnost tokenu konfigurace](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) dokumentu se dozvíte, jak vytvořit další vlastní.</span><span class="sxs-lookup"><span data-stu-id="8c592-110">Checkout the [Configuring token lifetime](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)   document to learn how to create other custom.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c592-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c592-111">Next steps</span></span>
[<span data-ttu-id="8c592-112">Konfigurace životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="8c592-112">Configuring Token Lifetime</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[<span data-ttu-id="8c592-113">Odkaz tokenu Azure AD</span><span class="sxs-lookup"><span data-stu-id="8c592-113">Azure AD Token Reference</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)

