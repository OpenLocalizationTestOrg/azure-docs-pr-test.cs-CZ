---
title: "Azure Active Directory B2C: Úvodní stránka vlastních zásad | Dokumentace Microsoftu"
description: "Vývoj aplikací určených uživatelům v Azure Active Directory B2C s využitím vlastních zásad"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f2079f53-a637-4f2d-b3a0-61a9647ad433
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 5/06/2017
ms.author: parakhj
ms.openlocfilehash: cb7a9f01e43d41eb7315cb37a41e69f044ce5566
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications-using-custom-policies"></a><span data-ttu-id="2ca8a-103">Azure Active Directory B2C: Registrace a přihlašování uživatelů aplikace s využitím vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="2ca8a-103">Azure Active Directory B2C: Sign up and sign in consumers in your applications using custom policies</span></span>
<span data-ttu-id="2ca8a-104">Vlastní zásady jsou konfigurační soubory, které určují chování tenanta služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2ca8a-104">Custom policies are configuration files that define the behavior of your Azure AD B2C tenant.</span></span> <span data-ttu-id="2ca8a-105">Vývojář identit je může plně upravit a přizpůsobit je pro skoro neomezené množství úloh.</span><span class="sxs-lookup"><span data-stu-id="2ca8a-105">They can be fully edited by an identity developer to complete a near unlimited number of tasks.</span></span>

## <a name="how-to-articles"></a><span data-ttu-id="2ca8a-106">Články s návody</span><span class="sxs-lookup"><span data-stu-id="2ca8a-106">How-to articles</span></span>
<span data-ttu-id="2ca8a-107">Další informace o použití konkrétních funkcí Azure Active Directory B2C:</span><span class="sxs-lookup"><span data-stu-id="2ca8a-107">Learn how to use specific Azure Active Directory B2C features:</span></span>

* [<span data-ttu-id="2ca8a-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="2ca8a-108">Get Started</span></span>](active-directory-b2c-overview-custom.md)
* <span data-ttu-id="2ca8a-109">Konfigurovat zprostředkovatelů OIDC, jako je [Azure AD](active-directory-b2c-setup-aad-custom.md).</span><span class="sxs-lookup"><span data-stu-id="2ca8a-109">Configure OIDC Providers such as [Azure AD](active-directory-b2c-setup-aad-custom.md).</span></span>
* <span data-ttu-id="2ca8a-110">Konfigurovat zprostředkovatelů SAML, jako je [Salesforce](active-directory-b2c-setup-sf-app-custom.md).</span><span class="sxs-lookup"><span data-stu-id="2ca8a-110">Configure SAML Providers such as [Salesforce](active-directory-b2c-setup-sf-app-custom.md).</span></span>
* <span data-ttu-id="2ca8a-111">Integrace rozhraní API RESTful:</span><span class="sxs-lookup"><span data-stu-id="2ca8a-111">Integrate RESTful APIs:</span></span>
    * <span data-ttu-id="2ca8a-112">[Získání dalších deklarací identity](active-directory-b2c-rest-api-step-custom.md).</span><span class="sxs-lookup"><span data-stu-id="2ca8a-112">[Obtain additional claims](active-directory-b2c-rest-api-step-custom.md).</span></span>
    * <span data-ttu-id="2ca8a-113">[Ověření vstupu uživatele](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="2ca8a-113">[Validate user input](active-directory-b2c-rest-api-validation-custom.md).</span></span>
* <span data-ttu-id="2ca8a-114">Přizpůsobení přihlášení:</span><span class="sxs-lookup"><span data-stu-id="2ca8a-114">Customize Login:</span></span>
    * [<span data-ttu-id="2ca8a-115">Konfigurace vstupu uživatele</span><span class="sxs-lookup"><span data-stu-id="2ca8a-115">Configure User Input</span></span>](active-directory-b2c-configure-signup-self-asserted-custom.md)
    * [<span data-ttu-id="2ca8a-116">Přizpůsobení uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="2ca8a-116">Customize UI</span></span>](active-directory-b2c-ui-customization-custom.md)
    * [<span data-ttu-id="2ca8a-117">Přizpůsobení tokenu</span><span class="sxs-lookup"><span data-stu-id="2ca8a-117">Customize Token</span></span>](active-directory-b2c-reference-manage-sso-and-token-configuration.md)
* <span data-ttu-id="2ca8a-118">Řešení potíží [shromažďováním protokolů pomocí Application Insights](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="2ca8a-118">Troubleshoot by [collecting logs using Application Insights](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="whats-new"></a><span data-ttu-id="2ca8a-119">Co je nového</span><span class="sxs-lookup"><span data-stu-id="2ca8a-119">What's new</span></span>
<span data-ttu-id="2ca8a-120">Pravidelně kontrolujte tuto stránku, abyste se dovědět o budoucích změnách Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="2ca8a-120">Check back here often to learn about future changes to the Azure Active Directory B2C.</span></span> <span data-ttu-id="2ca8a-121">O všech aktualizacích budeme také tweetovat na @AzureAD.</span><span class="sxs-lookup"><span data-stu-id="2ca8a-121">We'll also tweet about any updates by using @AzureAD.</span></span>



