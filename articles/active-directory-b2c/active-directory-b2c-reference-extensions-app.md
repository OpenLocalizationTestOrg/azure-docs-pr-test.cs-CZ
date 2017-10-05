---
title: "Rozšíření aplikace – Azure AD B2C | Microsoft Docs"
description: "Obnovení aplikace rozšíření b2c"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f0392e32-0771-473c-a799-81438ca2bcff
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/17/2017
ms.author: parja
ms.openlocfilehash: 17500b572a0e92c1c233c6967840a5b6d96e21cb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: Rozšíření aplikace

Když je vytvořen adresář služby Azure AD B2C, aplikace volá `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` se automaticky vytvoří do nového adresáře. Tuto aplikaci, označuje jako **aplikace b2c rozšíření**, se zobrazí na *registrace aplikace*. Pomocí služby Azure AD B2C se používá k ukládání informací o uživatelích a vlastní atributy. Pokud se odstraní aplikaci, Azure AD B2C nebude fungovat správně a bude mít vliv na produkční prostředí.

> [!IMPORTANT]
> Pokud máte v úmyslu okamžitě odstranit váš klient, neodstraňujte aplikace rozšíření b2c. Pokud aplikace zůstane odstraněné déle než 30 dní, uživatel informace budou trvale ztratí.

## <a name="verifying-that-the-extensions-app-is-present"></a>Ověření, zda je k dispozici rozšíření aplikace

Chcete-li ověřit, zda aplikace rozšíření b2c je k dispozici:

1. Ve vašem klientu Azure AD B2C, klikněte na **další služby** v levé navigační nabídce.
1. Vyhledejte a otevřete **registrace aplikace**.
1. Vyhledejte aplikaci, která začíná **aplikace b2c rozšíření**

## <a name="recover-the-extensions-app"></a>Obnovit rozšíření aplikace

Pokud jste omylem odstranili aplikace rozšíření b2c, máte 30 dní, jak jej obnovit. Můžete obnovit aplikace pomocí rozhraní Graph API:

1. Přejděte do [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).
1. Přihlaste se k webu jako globální správce adresáře Azure AD B2C, který chcete obnovit odstraněné aplikace pro.
1. Vydávat na HTTP GET na adresu URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` s verze api-version = 1.6. Nezapomeňte nahradit `{tenantName}` názvem svého klienta. Tato operace se zobrazí seznam všech aplikacích, které byly odstraněny v posledních 30 dnů.
1. Vyhledat aplikaci v seznamu, kde název začíná 'aplikace b2c rozšíření a zkopírujte její `objectid` hodnotu vlastnosti.
1. Vydání požadavku HTTP POST na adresu URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Nahraďte `{OBJECTID}` v adrese URL se `objectid` z předchozího kroku. 

Nyní byste měli být schopni [obnovené aplikace](#verifying-that-the-extensions-app-is-present) na portálu Azure.

> [!NOTE]
> Aplikace lze obnovit pouze pokud byl odstraněn během posledních 30 dnů. Pokud to bylo víc než 30 dní, data budou trvale ztratí. O další pomoc soubor lístek podpory.