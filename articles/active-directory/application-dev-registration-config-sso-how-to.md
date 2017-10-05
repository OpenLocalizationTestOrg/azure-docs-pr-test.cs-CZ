---
title: "Postup konfigurace novou aplikaci víceklientské | Microsoft Docs"
description: "Postup konfigurace jednotného přihlašování pro vlastní aplikaci vývoji a Probíhá registrace ve službě Azure AD."
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
ms.openlocfilehash: 0fdc58d82d9cd2e7edac33cc5af4b98d2fd06c56
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-a-new-multi-tenant-application"></a><span data-ttu-id="19e7a-103">Postup konfigurace nové víceklientské aplikace</span><span class="sxs-lookup"><span data-stu-id="19e7a-103">How to configure a new multi-tenant application</span></span>

<span data-ttu-id="19e7a-104">Povolení federované jednotné přihlašování (SSO) v aplikaci je automaticky povolen při federaci prostřednictvím služby Azure AD pro OpenID Connect SAML 2.0 a WS-Fed.</span><span class="sxs-lookup"><span data-stu-id="19e7a-104">Enabling federated single sign-on (SSO) in your app is automatically enabled when federating through Azure AD for OpenID Connect, SAML 2.0, or WS-Fed.</span></span> <span data-ttu-id="19e7a-105">Pokud vaši koncoví uživatelé se musejí přihlásit přes již existující relaci s Azure AD, je pravděpodobné, že vaše aplikace může být nesprávné.</span><span class="sxs-lookup"><span data-stu-id="19e7a-105">If your end users are having to sign in despite already having an existing session with Azure AD, it’s likely your app may be misconfigured.</span></span>

* <span data-ttu-id="19e7a-106">Pokud používáte ADAL/MSAL, ujistěte se, máte **PromptBehavior** nastavena na **automaticky** místo **vždy**.</span><span class="sxs-lookup"><span data-stu-id="19e7a-106">If you’re using ADAL/MSAL, make sure you have **PromptBehavior** set to **Auto** rather than **Always**.</span></span>

* <span data-ttu-id="19e7a-107">Pokud vytváříte mobilní aplikace, budete potřebovat další konfigurace, které umožňují zprostředkované nebo jiných zprostředkované jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="19e7a-107">If you’re building a mobile app, you may need additional configurations to enable brokered or non-brokered SSO.</span></span>

<span data-ttu-id="19e7a-108">Pro Android, najdete v části [povolení mezi aplikace jednotné přihlašování v Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).</span><span class="sxs-lookup"><span data-stu-id="19e7a-108">For Android, see [Enabling Cross App SSO in Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).</span></span><br>

<span data-ttu-id="19e7a-109">Pro iOS, najdete v části [povolení mezi aplikace jednotné přihlašování v iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).</span><span class="sxs-lookup"><span data-stu-id="19e7a-109">For iOS, see [Enabling Cross App SSO in iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).</span></span>

## <a name="next-steps"></a><span data-ttu-id="19e7a-110">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19e7a-110">Next steps</span></span>

[<span data-ttu-id="19e7a-111">Jednotné přihlašování Azure AD</span><span class="sxs-lookup"><span data-stu-id="19e7a-111">Azure AD SSO</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[<span data-ttu-id="19e7a-112">Povolení křížové jednotného přihlašování k aplikaci v Android</span><span class="sxs-lookup"><span data-stu-id="19e7a-112">Enabling Cross App SSO in Android</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[<span data-ttu-id="19e7a-113">Povolení křížové aplikace jednotné přihlašování v iOS</span><span class="sxs-lookup"><span data-stu-id="19e7a-113">Enabling Cross App SSO in iOS</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[<span data-ttu-id="19e7a-114">Integrace aplikace AzureAD</span><span class="sxs-lookup"><span data-stu-id="19e7a-114">Integrating Apps to AzureAD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[<span data-ttu-id="19e7a-115">Souhlasu a systému oprávnění rolích pro AzureAD v2.0 konvergované aplikace</span><span class="sxs-lookup"><span data-stu-id="19e7a-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="19e7a-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="19e7a-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
