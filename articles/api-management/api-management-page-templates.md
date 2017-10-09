---
title: "aaaPage šablon ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toocustomize hello obsah stránky na portálu vývojáře pomocí sadu šablon ve službě Azure API Management."
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
ms.openlocfilehash: 84bd971ad4bcacfdd36c2ebbe05b16063f2a547b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="page-templates-in-azure-api-management"></a><span data-ttu-id="93c24-103">Stránka šablon ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="93c24-103">Page templates in Azure API Management</span></span>
<span data-ttu-id="93c24-104">Azure API Management poskytuje že Hello možnost toocustomize hello obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="93c24-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="93c24-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a hello editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a zadané sadu lokalizované [řetězce prostředků](api-management-template-resources.md#strings), [ Prostředky glyfy](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu tooconfigure hello obsah hello stránek podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="93c24-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="93c24-106">Hello šablony v této části umožňují toocustomize hello obsah hello přihlášení, přihlaste se a stránka nebyla nalezena v portálu pro vývojáře hello stránky.</span><span class="sxs-lookup"><span data-stu-id="93c24-106">hello templates in this section allow you toocustomize hello content of hello sign in, sign up, and page not found pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="93c24-107">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="93c24-107">Sign in</span></span>](#SignIn)  
  
-   [<span data-ttu-id="93c24-108">Registrace</span><span class="sxs-lookup"><span data-stu-id="93c24-108">Sign up</span></span>](#SignUp)  
  
-   [<span data-ttu-id="93c24-109">Stránka nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="93c24-109">Page not found</span></span>](#PageNotFound)  
  
> [!NOTE]
>  <span data-ttu-id="93c24-110">Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci hello, ale jsou toochange subjektu z důvodu vylepšení toocontinuous.</span><span class="sxs-lookup"><span data-stu-id="93c24-110">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="93c24-111">Hello za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře hello přechodem toohello potřeby jednotlivých šablony.</span><span class="sxs-lookup"><span data-stu-id="93c24-111">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="93c24-112">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="93c24-112">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="93c24-113"><a name="SignIn"></a>Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="93c24-113"><a name="SignIn"></a> Sign in</span></span>  
 <span data-ttu-id="93c24-114">Hello **přihlášení** šablona vám umožní toocustomize hello přihlašovací stránce v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="93c24-114">hello **sign in** template allows you toocustomize hello sign in page in hello developer portal.</span></span>  
  
 <span data-ttu-id="93c24-115">![Přihlašovací stránka](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM přihlášení vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="93c24-115">![Sign In Page](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM Sign In Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="93c24-116">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="93c24-116">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="93c24-117">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="93c24-117">Controls</span></span>  
 <span data-ttu-id="93c24-118">Tato šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="93c24-118">This template may  use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="93c24-119">Basic přihlášení</span><span class="sxs-lookup"><span data-stu-id="93c24-119">basic-signin</span></span>](api-management-page-controls.md#basic-signin)  
  
-   [<span data-ttu-id="93c24-120">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="93c24-120">providers</span></span>](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a><span data-ttu-id="93c24-121">Datový model</span><span class="sxs-lookup"><span data-stu-id="93c24-121">Data model</span></span>  
 <span data-ttu-id="93c24-122">[Přihlášení uživatele](api-management-template-data-model-reference.md#UseSignIn) entity.</span><span class="sxs-lookup"><span data-stu-id="93c24-122">[User sign in](api-management-template-data-model-reference.md#UseSignIn) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="93c24-123">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="93c24-123">Sample template data</span></span>  
  
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
  
##  <span data-ttu-id="93c24-124"><a name="SignUp"></a>Registrace</span><span class="sxs-lookup"><span data-stu-id="93c24-124"><a name="SignUp"></a> Sign up</span></span>  
 <span data-ttu-id="93c24-125">Hello **zaregistrovat** šablona vám umožní toocustomize hello registrační stránku v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="93c24-125">hello **sign up** template allows you toocustomize hello sign up page in hello developer portal.</span></span>  
  
 <span data-ttu-id="93c24-126">![Přihlašovací stránku služby](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM registrace vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="93c24-126">![Sign Up Page](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM Sign Up Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="93c24-127">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="93c24-127">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="93c24-128">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="93c24-128">Controls</span></span>  
 <span data-ttu-id="93c24-129">Tato šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="93c24-129">This template may  use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="93c24-130">registrace</span><span class="sxs-lookup"><span data-stu-id="93c24-130">sign-up</span></span>](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a><span data-ttu-id="93c24-131">Datový model</span><span class="sxs-lookup"><span data-stu-id="93c24-131">Data model</span></span>  
 <span data-ttu-id="93c24-132">[Registrace uživatele](api-management-template-data-model-reference.md#UserSignUp) entity.</span><span class="sxs-lookup"><span data-stu-id="93c24-132">[User sign up](api-management-template-data-model-reference.md#UserSignUp) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="93c24-133">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="93c24-133">Sample template data</span></span>  
  
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
  
##  <span data-ttu-id="93c24-134"><a name="PageNotFound"></a>Stránka nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="93c24-134"><a name="PageNotFound"></a> Page not found</span></span>  
 <span data-ttu-id="93c24-135">Hello **stránka nebyla nalezena** šablona umožňuje vám toocustomize hello stránka nebyla nalezena stránka v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="93c24-135">hello **page not found** template allows you toocustomize hello page not found page in hello developer portal.</span></span>  
  
 <span data-ttu-id="93c24-136">![Nebyla nalezena stránka](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM nebyl nalezen vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="93c24-136">![Not Found Page](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM Not Found Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="93c24-137">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="93c24-137">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="93c24-138">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="93c24-138">Controls</span></span>  
 <span data-ttu-id="93c24-139">Tato šablona nesmíte používat žádné [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="93c24-139">This template may  not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="93c24-140">Datový model</span><span class="sxs-lookup"><span data-stu-id="93c24-140">Data model</span></span>  
  
|<span data-ttu-id="93c24-141">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="93c24-141">Property</span></span>|<span data-ttu-id="93c24-142">Typ</span><span class="sxs-lookup"><span data-stu-id="93c24-142">Type</span></span>|<span data-ttu-id="93c24-143">Popis</span><span class="sxs-lookup"><span data-stu-id="93c24-143">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="93c24-144">referenceCode</span><span class="sxs-lookup"><span data-stu-id="93c24-144">referenceCode</span></span>|<span data-ttu-id="93c24-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="93c24-145">string</span></span>|<span data-ttu-id="93c24-146">Kód v případě, že tato stránka se nezobrazí jako výsledek hello k vnitřní chybě vygenerována.</span><span class="sxs-lookup"><span data-stu-id="93c24-146">Code generated if this page was displayed as hello result of an internal error.</span></span>|  
|<span data-ttu-id="93c24-147">Kód chyby</span><span class="sxs-lookup"><span data-stu-id="93c24-147">errorCode</span></span>|<span data-ttu-id="93c24-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="93c24-148">string</span></span>|<span data-ttu-id="93c24-149">Kód v případě, že tato stránka se nezobrazí jako výsledek hello k vnitřní chybě vygenerována.</span><span class="sxs-lookup"><span data-stu-id="93c24-149">Code generated if this page was displayed as hello result of an internal error.</span></span>|  
|<span data-ttu-id="93c24-150">emailBody</span><span class="sxs-lookup"><span data-stu-id="93c24-150">emailBody</span></span>|<span data-ttu-id="93c24-151">Řetězec</span><span class="sxs-lookup"><span data-stu-id="93c24-151">string</span></span>|<span data-ttu-id="93c24-152">E-mailu textu vygenerováno, pokud tato stránka se nezobrazí jako výsledek hello k vnitřní chybě.</span><span class="sxs-lookup"><span data-stu-id="93c24-152">Email body generated if this page was displayed as hello result of an internal error.</span></span>|  
|<span data-ttu-id="93c24-153">requestedUrl</span><span class="sxs-lookup"><span data-stu-id="93c24-153">requestedUrl</span></span>|<span data-ttu-id="93c24-154">Řetězec</span><span class="sxs-lookup"><span data-stu-id="93c24-154">string</span></span>|<span data-ttu-id="93c24-155">Adresa URL Hello vyžádá, když hello stránka nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="93c24-155">hello URL requested when hello page was not found.</span></span>|  
|<span data-ttu-id="93c24-156">referrerUrl</span><span class="sxs-lookup"><span data-stu-id="93c24-156">referrerUrl</span></span>|<span data-ttu-id="93c24-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="93c24-157">string</span></span>|<span data-ttu-id="93c24-158">toohello adresy URL odkazující server Hello požadovaná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="93c24-158">hello referrer URL toohello requested URL.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="93c24-159">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="93c24-159">Sample template data</span></span>  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a><span data-ttu-id="93c24-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93c24-160">Next steps</span></span>
<span data-ttu-id="93c24-161">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="93c24-161">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
