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
# <a name="how-toochange-hello-token-lifetime-defaults-for-a-custom-developed-application"></a>Jak životnost tokenu hello toochange výchozí nastavení pro aplikace vyvinuté vlastní

Azure AD Premium umožňuje vývojáři aplikací a klienta admins tooconfigure hello životnost tokeny vydané pro-důvěrné klienty. Životnost tokenu zásady jsou nastaveny na základě klienta celou nebo hello prostředkům přistupuje.

 * tooset zásadu životnost tokenu, je nutné toodownload hello [modulu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).

 * Spustit hello **Connect-AzureAD-potvrďte** příkaz.

 * Tady je příklad zásady, které nastavuje hello maximální stáří jeden faktor obnovovací token. Vytvořte zásadu hello:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Najdete v článku věnovaném hello [životnost tokenu konfigurace](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) dokumentu toolearn jak toocreate další vlastní.

## <a name="next-steps"></a>Další kroky
[Konfigurace životnost tokenu](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Odkaz tokenu Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)

