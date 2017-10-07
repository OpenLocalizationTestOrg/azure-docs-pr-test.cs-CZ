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
# <a name="unexpected-consent-prompt-when-signing-in-tooan-application"></a>Neočekávané souhlasu řádku při přihlášení tooan aplikace

Mnoho aplikací, které se integrují s Azure Active Directory vyžadují oprávnění toovarious prostředky v toorun pořadí. Když tyto prostředky jsou také integrované s Azure Active Directory, je je požadována pomocí hello Azure AD tooaccess oprávnění souhlas framework. 

Výsledkem je výzva souhlasu se zobrazí hello poprvé, aplikace se používá, což je často o jednorázovou operaci. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Scénáře, ve kterých se uživatelům zobrazí souhlas výzvy

Další výzvy je možné očekávat v různých situacích:

* Hello sadu oprávnění vyžaduje aplikace hello změnily.

* Hello uživatele, který původně dá souhlas toohello aplikace nebyla správce a teď uživatel různých (bez oprávnění správce) používá hello aplikace pro hello poprvé.

* Hello uživatele, který původně dá souhlas toohello aplikace byla správce, ale jejich souhlas jménem aplikace hello celé organizace.

* aplikace Hello používá [přírůstkové a dynamické souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) toorequest další oprávnění po začátku udělen souhlas. To se často používá, pokud volitelné funkce další aplikace vyžadují oprávnění nad rámec těch, vyžaduje se pro základní funkce.

* Po udělení původně byl odvolán souhlasu.

* Hello vývojáře nakonfiguroval toorequire aplikace hello výzva k povolení spuštění pokaždé, když se používá (Poznámka: Toto není vhodné).

## <a name="next-steps"></a>Další kroky

-   [Aplikace, oprávnění a souhlasu v Azure Active Directory (koncový bod verze 1.0)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Obory, oprávnění a souhlasu v hello Azure Active Directory (koncový bod v2.0)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


