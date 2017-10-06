---
title: "aaaHow tooconfigure novou aplikaci víceklientské | Microsoft Docs"
description: "Jak tooconfigure jednotné přihlašování pro vlastní aplikaci vývoji a Probíhá registrace ve službě Azure AD."
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
ms.openlocfilehash: 4d3499d8885933516d6597fa9f87bcf88cd5a428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-a-new-multi-tenant-application"></a>Jak tooconfigure novou aplikaci pro více klientů

Povolení federované jednotné přihlašování (SSO) v aplikaci je automaticky povolen při federaci prostřednictvím služby Azure AD pro OpenID Connect SAML 2.0 a WS-Fed. Pokud vaši koncoví uživatelé jsou s toosign navzdory již má existující relaci s Azure AD, je pravděpodobné, že vaše aplikace může být nesprávné.

* Pokud používáte ADAL/MSAL, ujistěte se, máte **PromptBehavior** nastavit příliš**automaticky** místo **vždy**.

* Pokud vytváříte mobilní aplikace, budete potřebovat další konfigurace tooenable zprostředkované nebo jiných zprostředkované jednotné přihlašování.

Pro Android, najdete v části [povolení mezi aplikace jednotné přihlašování v Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

Pro iOS, najdete v části [povolení mezi aplikace jednotné přihlašování v iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Další kroky

[Jednotné přihlašování Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Povolení křížové jednotného přihlašování k aplikaci v Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[Povolení křížové aplikace jednotné přihlašování v iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Integrace aplikace tooAzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Souhlasu a systému oprávnění rolích pro AzureAD v2.0 konvergované aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
