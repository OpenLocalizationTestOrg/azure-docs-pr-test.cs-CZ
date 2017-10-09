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
# <a name="page-templates-in-azure-api-management"></a>Stránka šablon ve službě Azure API Management
Azure API Management poskytuje že Hello možnost toocustomize hello obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu. Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a hello editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a zadané sadu lokalizované [řetězce prostředků](api-management-template-resources.md#strings), [ Prostředky glyfy](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu tooconfigure hello obsah hello stránek podle potřeby pomocí těchto šablon.  
  
 Hello šablony v této části umožňují toocustomize hello obsah hello přihlášení, přihlaste se a stránka nebyla nalezena v portálu pro vývojáře hello stránky.  
  
-   [Přihlásit se](#SignIn)  
  
-   [Registrace](#SignUp)  
  
-   [Stránka nebyla nalezena.](#PageNotFound)  
  
> [!NOTE]
>  Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci hello, ale jsou toochange subjektu z důvodu vylepšení toocontinuous. Hello za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře hello přechodem toohello potřeby jednotlivých šablony. Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
##  <a name="SignIn"></a>Přihlásit se  
 Hello **přihlášení** šablona vám umožní toocustomize hello přihlašovací stránce v portálu pro vývojáře hello.  
  
 ![Přihlašovací stránka](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM přihlášení vývojář stránky portálu šablony")  
  
### <a name="default-template"></a>Výchozí šablony  
  
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
  
### <a name="controls"></a>Ovládací prvky  
 Tato šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).  
  
-   [Basic přihlášení](api-management-page-controls.md#basic-signin)  
  
-   [Zprostředkovatelé](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a>Datový model  
 [Přihlášení uživatele](api-management-template-data-model-reference.md#UseSignIn) entity.  
  
### <a name="sample-template-data"></a>Ukázková data šablony  
  
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
  
##  <a name="SignUp"></a>Registrace  
 Hello **zaregistrovat** šablona vám umožní toocustomize hello registrační stránku v portálu pro vývojáře hello.  
  
 ![Přihlašovací stránku služby](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM registrace vývojář stránky portálu šablony")  
  
### <a name="default-template"></a>Výchozí šablony  
  
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
  
### <a name="controls"></a>Ovládací prvky  
 Tato šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).  
  
-   [registrace](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a>Datový model  
 [Registrace uživatele](api-management-template-data-model-reference.md#UserSignUp) entity.  
  
### <a name="sample-template-data"></a>Ukázková data šablony  
  
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
  
##  <a name="PageNotFound"></a>Stránka nebyla nalezena.  
 Hello **stránka nebyla nalezena** šablona umožňuje vám toocustomize hello stránka nebyla nalezena stránka v portálu pro vývojáře hello.  
  
 ![Nebyla nalezena stránka](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM nebyl nalezen vývojář stránky portálu šablony")  
  
### <a name="default-template"></a>Výchozí šablony  
  
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
  
### <a name="controls"></a>Ovládací prvky  
 Tato šablona nesmíte používat žádné [stránka ovládací prvky](api-management-page-controls.md).  
  
### <a name="data-model"></a>Datový model  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|referenceCode|Řetězec|Kód v případě, že tato stránka se nezobrazí jako výsledek hello k vnitřní chybě vygenerována.|  
|Kód chyby|Řetězec|Kód v případě, že tato stránka se nezobrazí jako výsledek hello k vnitřní chybě vygenerována.|  
|emailBody|Řetězec|E-mailu textu vygenerováno, pokud tato stránka se nezobrazí jako výsledek hello k vnitřní chybě.|  
|requestedUrl|Řetězec|Adresa URL Hello vyžádá, když hello stránka nebyla nalezena.|  
|referrerUrl|Řetězec|toohello adresy URL odkazující server Hello požadovaná adresa URL.|  
  
### <a name="sample-template-data"></a>Ukázková data šablony  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a>Další kroky
Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).
