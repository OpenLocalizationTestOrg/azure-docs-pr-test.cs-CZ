---
title: "Stránka šablon ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit obsah stránky na portálu vývojáře pomocí sadu šablon ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: e57df269-1019-4b74-b74d-53155b809d59
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 7f9ef37a694bce786b6acaa428df83f0cb23c2dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="page-templates-in-azure-api-management"></a><span data-ttu-id="88195-103">Stránka šablon ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="88195-103">Page templates in Azure API Management</span></span>
<span data-ttu-id="88195-104">Azure API Management poskytuje schopnost přizpůsobit obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="88195-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="88195-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a lokalizované zadaný sadu [řetězce prostředků](api-management-template-resources.md#strings), [glyfy prostředky](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu při konfiguraci obsahu stránek, podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="88195-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="88195-106">Šablony v této části umožňují přizpůsobit obsah přihlášení, přihlaste se a stránka nebyla nalezena stránky v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="88195-106">The templates in this section allow you to customize the content of the sign in, sign up, and page not found pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="88195-107">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="88195-107">Sign in</span></span>](#SignIn)  
  
-   [<span data-ttu-id="88195-108">Registrace</span><span class="sxs-lookup"><span data-stu-id="88195-108">Sign up</span></span>](#SignUp)  
  
-   [<span data-ttu-id="88195-109">Stránka nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="88195-109">Page not found</span></span>](#PageNotFound)  
  
> [!NOTE]
>  <span data-ttu-id="88195-110">Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci, ale mohou být změněna z důvodu průběžné vylepšení.</span><span class="sxs-lookup"><span data-stu-id="88195-110">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="88195-111">Za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře přechodem na jednotlivé požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="88195-111">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="88195-112">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="88195-112">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="88195-113"><a name="SignIn"></a>Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="88195-113"><a name="SignIn"></a> Sign in</span></span>  
 <span data-ttu-id="88195-114">**Přihlášení** šablona umožňuje přizpůsobit přihlašovací stránku v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="88195-114">The **sign in** template allows you to customize the sign in page in the developer portal.</span></span>  
  
 <span data-ttu-id="88195-115">![Přihlašovací stránka](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM přihlášení vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="88195-115">![Sign In Page](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM Sign In Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="88195-116">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="88195-116">Default template</span></span>  
  
```xml  
<h2 class="text-center">{% localized "SigninStrings|WebAuthenticationSigninTitle" %}</h2>  
{% if registrationEnabled == true %}  
<p class="text-center">{% localized "SigninStrings|WebAuthenticationNotAMember" %}</p>  
{% endif %}  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    {% if registrationEnabled == true %}  
        <p>{% localized "SigninStrings|WebAuthenticationSigininWithPassword" %}</p>  
    <basic-SignIn></basic-SignIn>  
    {% endif %}  
  </div>  
  
    {% if registrationEnabled != true and providers.size == 0 %}  
        {% localized "ProviderInfoStrings|TextboxExternalIdentitiesDisabled" %}  
  {% else %}  
        {% if providers.size > 0 %}  
      <div class="col-md-6">  
            <div class="providers-list">  
                <p class="text-left">  
                {% if registrationEnabled == true %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitation" %}  
                {% else %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitationPrimary" %}  
                {% endif %}  
                </p>  
        <providers></providers>  
            </div>  
    </div>  
        {% endif %}  
    {% endif %}  
  
  {% if userRegistrationTermsEnabled == true %}  
    <div class="col-md-6">  
        <div id="terms" class="modal" role="dialog" tabindex="-1">  
            <div class="modal-dialog">  
                <div class="modal-content">  
                    <div class="modal-header">  
                        <h4 class="modal-title">{% localized "SigninResources|DialogHeadingTermsOfUse" %}</h4>  
                    </div>  
                    <div class="modal-body break-all">{{userRegistrationTerms}}</div>  
                    <div class="modal-footer">  
                        <button type="button" class="btn btn-default" data-dismiss="modal">{% localized "CommonStrings|ButtonLabelClose" %}</button>  
                    </div>  
                </div>  
            </div>  
        </div>  
        <p>{% localized "SigninResources|TextblockUserRegistrationTermsProvided" %}</p>  
    </div>  
    {% endif %}  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="88195-117">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="88195-117">Controls</span></span>  
 <span data-ttu-id="88195-118">Tato šablona může používat následující [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="88195-118">This template may  use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="88195-119">Basic přihlášení</span><span class="sxs-lookup"><span data-stu-id="88195-119">basic-signin</span></span>](api-management-page-controls.md#basic-signin)  
  
-   [<span data-ttu-id="88195-120">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="88195-120">providers</span></span>](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a><span data-ttu-id="88195-121">Datový model</span><span class="sxs-lookup"><span data-stu-id="88195-121">Data model</span></span>  
 <span data-ttu-id="88195-122">[Přihlášení uživatele](api-management-template-data-model-reference.md#UseSignIn) entity.</span><span class="sxs-lookup"><span data-stu-id="88195-122">[User sign in](api-management-template-data-model-reference.md#UseSignIn) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="88195-123">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="88195-123">Sample template data</span></span>  
  
```json  
{  
    "Email": null,  
    "Password": null,  
    "ReturnUrl": null,  
    "RememberMe": false,  
    "RegistrationEnabled": true,  
    "DelegationEnabled": false,  
    "DelegationUrl": null,  
    "SsoSignUpUrl": null,  
    "AuxServiceUrl": "https://manage.windowsazure.com/#Workspaces/ApiManagementExtension/service/contoso5/dashboard",  
    "Providers": [  
        {  
            "Properties": {  
                "AuthenticationType": "Aad",  
                "Caption": "Azure Active Directory"  
            },  
            "AuthenticationType": "Aad",  
            "Caption": "Azure Active Directory"  
        }  
    ],  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsEnabled": false  
}  
```  
  
##  <span data-ttu-id="88195-124"><a name="SignUp"></a>Registrace</span><span class="sxs-lookup"><span data-stu-id="88195-124"><a name="SignUp"></a> Sign up</span></span>  
 <span data-ttu-id="88195-125">**Zaregistrovat** šablona umožňuje přizpůsobit přihlašovací stránku v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="88195-125">The **sign up** template allows you to customize the sign up page in the developer portal.</span></span>  
  
 <span data-ttu-id="88195-126">![Přihlašovací stránku služby](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM registrace vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="88195-126">![Sign Up Page](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM Sign Up Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="88195-127">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="88195-127">Default template</span></span>  
  
```xml  
<h2 class="text-center">{% localized "SignupStrings|PageTitleSignup" %}</h2>  
<p class="text-center">  
  {% localized "SignupStrings|WebAuthenticationAlreadyAMember" %} <a href="/signin">{% localized "SignupStrings|WebAuthenticationSigninNow" %}</a>  
</p>  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    <p>{% localized "SignupStrings|WebAuthenticationCreateNewAccount" %}</p>  
    <sign-up></sign-up>  
  </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="88195-128">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="88195-128">Controls</span></span>  
 <span data-ttu-id="88195-129">Tato šablona může používat následující [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="88195-129">This template may  use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="88195-130">registrace</span><span class="sxs-lookup"><span data-stu-id="88195-130">sign-up</span></span>](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a><span data-ttu-id="88195-131">Datový model</span><span class="sxs-lookup"><span data-stu-id="88195-131">Data model</span></span>  
 <span data-ttu-id="88195-132">[Registrace uživatele](api-management-template-data-model-reference.md#UserSignUp) entity.</span><span class="sxs-lookup"><span data-stu-id="88195-132">[User sign up](api-management-template-data-model-reference.md#UserSignUp) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="88195-133">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="88195-133">Sample template data</span></span>  
  
```json  
{  
    "PasswordConfirm": null,  
    "Password": null,  
    "PasswordVerdictLevel": 0,  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsOptions": 0,  
    "ConsentAccepted": false,  
    "Email": null,  
    "FirstName": null,  
    "LastName": null,  
    "UserData": null,  
    "NameIdentifier": null,  
    "ProviderName": null  
}  
```  
  
##  <span data-ttu-id="88195-134"><a name="PageNotFound"></a>Stránka nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="88195-134"><a name="PageNotFound"></a> Page not found</span></span>  
 <span data-ttu-id="88195-135">**Stránka nebyla nalezena** šablona umožňuje přizpůsobit stránka nebyla nalezena stránka v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="88195-135">The **page not found** template allows you to customize the page not found page in the developer portal.</span></span>  
  
 <span data-ttu-id="88195-136">![Nebyla nalezena stránka](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM nebyl nalezen vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="88195-136">![Not Found Page](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM Not Found Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="88195-137">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="88195-137">Default template</span></span>  
  
```xml  
<h2>{% localized "NotFoundStrings|PageTitleNotFound" %}</h2>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialCause" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseOldLink" %}</li>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseMisspelledUrl" %}</li>  
</ul>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialSolution" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialSolutionRetype" %}</li>  
  <li>  
    {% capture textPotentialSolutionStartOver %}{% localized "NotFoundStrings|TextblockPotentialSolutionStartOver" %}{% endcapture %}  
    {% capture homeLink %}<a href="/">{% localized "NotFoundStrings|LinkLabelHomePage" %}</a>{% endcapture %}  
    {% assign replaceString = '{0}' %}  
  
    {{ textPotentialSolutionStartOver | replace : replaceString, homeLink }}  
  </li>  
</ul>  
  
<p>  
  {% capture textReportProblem %}{% localized "NotFoundStrings|TextReportProblem" %}{% endcapture %}  
  {% capture emailLink %}<a href="mailto:apimgmt@microsoft.com" target="_self" title="API Management Support">{% localized "NotFoundStrings|LinkLabelSendUsEmail" %}</a>{% endcapture %}  
  {% assign replaceString = '{0}' %}  
  
  {{ textReportProblem | replace : replaceString, emailLink }}  
</p>  
```  
  
### <a name="controls"></a><span data-ttu-id="88195-138">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="88195-138">Controls</span></span>  
 <span data-ttu-id="88195-139">Tato šablona nesmíte používat žádné [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="88195-139">This template may  not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="88195-140">Datový model</span><span class="sxs-lookup"><span data-stu-id="88195-140">Data model</span></span>  
  
|<span data-ttu-id="88195-141">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="88195-141">Property</span></span>|<span data-ttu-id="88195-142">Typ</span><span class="sxs-lookup"><span data-stu-id="88195-142">Type</span></span>|<span data-ttu-id="88195-143">Popis</span><span class="sxs-lookup"><span data-stu-id="88195-143">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="88195-144">referenceCode</span><span class="sxs-lookup"><span data-stu-id="88195-144">referenceCode</span></span>|<span data-ttu-id="88195-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88195-145">string</span></span>|<span data-ttu-id="88195-146">Kód vygenerováno, pokud tato stránka se nezobrazí v důsledku vnitřní chyby.</span><span class="sxs-lookup"><span data-stu-id="88195-146">Code generated if this page was displayed as the result of an internal error.</span></span>|  
|<span data-ttu-id="88195-147">Kód chyby</span><span class="sxs-lookup"><span data-stu-id="88195-147">errorCode</span></span>|<span data-ttu-id="88195-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88195-148">string</span></span>|<span data-ttu-id="88195-149">Kód vygenerováno, pokud tato stránka se nezobrazí v důsledku vnitřní chyby.</span><span class="sxs-lookup"><span data-stu-id="88195-149">Code generated if this page was displayed as the result of an internal error.</span></span>|  
|<span data-ttu-id="88195-150">emailBody</span><span class="sxs-lookup"><span data-stu-id="88195-150">emailBody</span></span>|<span data-ttu-id="88195-151">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88195-151">string</span></span>|<span data-ttu-id="88195-152">E-mailu textu vygenerováno, pokud tato stránka se nezobrazí v důsledku vnitřní chyby.</span><span class="sxs-lookup"><span data-stu-id="88195-152">Email body generated if this page was displayed as the result of an internal error.</span></span>|  
|<span data-ttu-id="88195-153">requestedUrl</span><span class="sxs-lookup"><span data-stu-id="88195-153">requestedUrl</span></span>|<span data-ttu-id="88195-154">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88195-154">string</span></span>|<span data-ttu-id="88195-155">Adresa URL vyžádá, když stránka nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="88195-155">The URL requested when the page was not found.</span></span>|  
|<span data-ttu-id="88195-156">referrerUrl</span><span class="sxs-lookup"><span data-stu-id="88195-156">referrerUrl</span></span>|<span data-ttu-id="88195-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88195-157">string</span></span>|<span data-ttu-id="88195-158">Odkazující server Adresa URL pro požadovanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88195-158">The referrer URL to the requested URL.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="88195-159">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="88195-159">Sample template data</span></span>  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a><span data-ttu-id="88195-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="88195-160">Next steps</span></span>
<span data-ttu-id="88195-161">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="88195-161">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>