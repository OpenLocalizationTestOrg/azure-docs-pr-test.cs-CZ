---
title: "Šablony profilu uživatele ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit obsah stránky profilu uživatele v portálu pro vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2e3b73ef-d223-44fe-9280-c3af3fd4a030
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 9a11bd5800068a5725ab2f099043993bff0b28d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="user-profile-templates-in-azure-api-management"></a><span data-ttu-id="343d4-103">Šablony profilu uživatele ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="343d4-103">User profile templates in Azure API Management</span></span>
<span data-ttu-id="343d4-104">Azure API Management poskytuje schopnost přizpůsobit obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="343d4-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="343d4-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a lokalizované zadaný sadu [řetězce prostředků](api-management-template-resources.md#strings), [glyfy prostředky](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu při konfiguraci obsahu stránek, podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="343d4-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="343d4-106">Šablony v této části umožňují přizpůsobit obsah stránky profilu uživatele v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="343d4-106">The templates in this section allow you to customize the content of the User profile pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="343d4-107">Profil</span><span class="sxs-lookup"><span data-stu-id="343d4-107">Profile</span></span>](#Profile)  
  
-   [<span data-ttu-id="343d4-108">Předplatná</span><span class="sxs-lookup"><span data-stu-id="343d4-108">Subscriptions</span></span>](#Subscriptions)  
  
-   [<span data-ttu-id="343d4-109">Aplikace</span><span class="sxs-lookup"><span data-stu-id="343d4-109">Applications</span></span>](#Applications)  
  
-   [<span data-ttu-id="343d4-110">Aktualizovat informace o účtu</span><span class="sxs-lookup"><span data-stu-id="343d4-110">Update account info</span></span>](#UpdateAccountInfo)  
  
> [!NOTE]
>  <span data-ttu-id="343d4-111">Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci, ale mohou být změněna z důvodu průběžné vylepšení.</span><span class="sxs-lookup"><span data-stu-id="343d4-111">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="343d4-112">Za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře přechodem na jednotlivé požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="343d4-112">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="343d4-113">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="343d4-113">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="343d4-114"><a name="Profile"></a>Profil</span><span class="sxs-lookup"><span data-stu-id="343d4-114"><a name="Profile"></a> Profile</span></span>  
 <span data-ttu-id="343d4-115">**Profil** šablona umožňuje přizpůsobit oddíl profilu uživatele stránky profilu uživatele v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="343d4-115">The **profile** template allows you to customize the user profile section of the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="343d4-116">![Stránku profilu uživatele](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM uživatelský profil stránky")</span><span class="sxs-lookup"><span data-stu-id="343d4-116">![User Profile Page](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM User Profile Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="343d4-117">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="343d4-117">Default template</span></span>  
  
```xml  
<div class="pull-right">  
  {% if canChangePassword == true %}  
  <a class="btn btn-default" id="ChangePassword" role="button" href="{{changePasswordUrl}}">{% localized "UserProfile|ButtonLabelChangePassword" %}</a>  
  {% endif %}  
  <a id="changeAccountInfo" href="{{changeNameOrEmailUrl}}" class="btn btn-default">  
    <span class="glyphicon glyphicon-user"></span>  
    <span>{% localized "UserProfile|ButtonLabelChangeAccountInfo" %}</span>  
  </a>  
</div>  
<h2>{% localized "SubscriptionStrings|PageTitleDeveloperProfile" %}</h2>  
<div class="container-fluid">  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="Email">{% localized "UserProfile|TextboxLabelEmail" %}</label>  
    </div>  
    <div class="col-sm-9" id="Email">{{email}}</div>  
  </div>  
  
  {% if isSystemUser != true %}  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="FirstName">{% localized "UserProfile|TextboxLabelEmailFirstName" %}</label>  
    </div>  
    <div class="col-sm-9" id="FirstName">{{FirstName}}</div>  
  </div>  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="LastName">{% localized "UserProfile|TextboxLabelEmailLastName" %}</label>  
    </div>  
    <div class="col-sm-9" id="LastName">{{LastName}}</div>  
  </div>  
  {% else %}  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="CompanyName">{% localized "UserProfile|TextboxLabelOrganizationName" %}</label>  
    </div>  
    <div class="col-sm-9" id="CompanyName">{{CompanyName}}</div>  
  </div>  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="AddresserEmail">{% localized "UserProfile|TextboxLabelNotificationsSenderEmail" %}</label>  
    </div>  
    <div class="col-sm-9" id="AddresserEmail">{{AddresserEmail}}</div>  
  </div>  
  {% endif %}  
  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="343d4-118">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="343d4-118">Controls</span></span>  
 <span data-ttu-id="343d4-119">Tato šablona nesmíte používat žádné [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="343d4-119">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="343d4-120">Datový model</span><span class="sxs-lookup"><span data-stu-id="343d4-120">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="343d4-121">[Profil](#Profile), [aplikace](#Applications), a [odběry](#Subscriptions) šablony sdílet stejný datový model a přijímat data stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="343d4-121">The [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share the same data model and receive the same template data.</span></span>  
  
|<span data-ttu-id="343d4-122">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="343d4-122">Property</span></span>|<span data-ttu-id="343d4-123">Typ</span><span class="sxs-lookup"><span data-stu-id="343d4-123">Type</span></span>|<span data-ttu-id="343d4-124">Popis</span><span class="sxs-lookup"><span data-stu-id="343d4-124">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="343d4-125">FirstName</span><span class="sxs-lookup"><span data-stu-id="343d4-125">firstName</span></span>|<span data-ttu-id="343d4-126">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-126">string</span></span>|<span data-ttu-id="343d4-127">Křestní jméno aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-127">First name of the current user.</span></span>|  
|<span data-ttu-id="343d4-128">Příjmení</span><span class="sxs-lookup"><span data-stu-id="343d4-128">lastName</span></span>|<span data-ttu-id="343d4-129">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-129">string</span></span>|<span data-ttu-id="343d4-130">Příjmení aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-130">Last name of the current user.</span></span>|  
|<span data-ttu-id="343d4-131">NázevSpolečnosti</span><span class="sxs-lookup"><span data-stu-id="343d4-131">companyName</span></span>|<span data-ttu-id="343d4-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-132">string</span></span>|<span data-ttu-id="343d4-133">Název společnosti aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-133">The company name of the current user.</span></span>|  
|<span data-ttu-id="343d4-134">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="343d4-134">addresserEmail</span></span>|<span data-ttu-id="343d4-135">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-135">string</span></span>|<span data-ttu-id="343d4-136">E-mailovou adresu stávajícího uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-136">Email address of the current user.</span></span>|  
|<span data-ttu-id="343d4-137">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="343d4-137">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="343d4-138">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-138">string</span></span>|<span data-ttu-id="343d4-139">Relativní adresu URL chcete zobrazit analýzu pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-139">Relative URL to view analytics for the current user.</span></span>|  
|<span data-ttu-id="343d4-140">odběry</span><span class="sxs-lookup"><span data-stu-id="343d4-140">subscriptions</span></span>|<span data-ttu-id="343d4-141">Kolekce [předplatné](api-management-template-data-model-reference.md#Subscription) entity.</span><span class="sxs-lookup"><span data-stu-id="343d4-141">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="343d4-142">Odběry pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-142">The subscriptions for the current user.</span></span>|  
|<span data-ttu-id="343d4-143">aplikace</span><span class="sxs-lookup"><span data-stu-id="343d4-143">applications</span></span>|<span data-ttu-id="343d4-144">Kolekce [aplikace](api-management-template-data-model-reference.md#Application) entity.</span><span class="sxs-lookup"><span data-stu-id="343d4-144">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="343d4-145">Aplikace je aktuální uživatel.</span><span class="sxs-lookup"><span data-stu-id="343d4-145">The applications of the current user.</span></span>|  
|<span data-ttu-id="343d4-146">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="343d4-146">changePasswordUrl</span></span>|<span data-ttu-id="343d4-147">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-147">string</span></span>|<span data-ttu-id="343d4-148">Relativní adresa URL aktuální uživatel heslo změnit.</span><span class="sxs-lookup"><span data-stu-id="343d4-148">The relative URL to change the current user's password.</span></span>|  
|<span data-ttu-id="343d4-149">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="343d4-149">changeNameOrEmailUrl</span></span>|<span data-ttu-id="343d4-150">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-150">string</span></span>|<span data-ttu-id="343d4-151">Relativní adresa URL Chcete-li změnit název a e-mailu pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-151">The relative URL to change the name and email for the current user.</span></span>|  
|<span data-ttu-id="343d4-152">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="343d4-152">canChangePassword</span></span>|<span data-ttu-id="343d4-153">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="343d4-153">boolean</span></span>|<span data-ttu-id="343d4-154">Jestli aktuální uživatel může změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="343d4-154">Whether the current user can change their password.</span></span>|  
|<span data-ttu-id="343d4-155">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="343d4-155">isSystemUser</span></span>|<span data-ttu-id="343d4-156">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="343d4-156">boolean</span></span>|<span data-ttu-id="343d4-157">Jestli má aktuální uživatel je členem jedné z předdefinovaných [skupiny](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="343d4-157">Whether the current user is a member of one of the built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="343d4-158">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="343d4-158">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="343d4-159"><a name="Subscriptions"></a>Odběry</span><span class="sxs-lookup"><span data-stu-id="343d4-159"><a name="Subscriptions"></a> Subscriptions</span></span>  
 <span data-ttu-id="343d4-160">**Odběry** šablony můžete vytvořit vlastní nastavení v části předplatná stránky profilu uživatele v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="343d4-160">The **Subscriptions** template allows you to customize the subscriptions section of the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="343d4-161">![Stránku odběru uživatele](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "stránku odběru APIM uživatele")</span><span class="sxs-lookup"><span data-stu-id="343d4-161">![User Subscription Page](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM User Subscription Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="343d4-162">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="343d4-162">Default template</span></span>  
  
```xml  
<div class="ap-account-subscriptions">  
  <a href="{{developersUsageStatisticsLink}}" id="UsageStatistics" class="btn btn-default pull-right">  
    <span class="glyphicon glyphicon-stats"></span>  
    <span>{% localized "SubscriptionListStrings|WebDevelopersUsageStatisticsLink" %}</span>  
  </a>  
  
  <h2>{% localized "SubscriptionListStrings|WebDevelopersYourSubscriptions" %}</h2>  
  
  <table class="table">  
    <thead>  
      <tr>  
        <th>Subscription details</th>  
        <th>Product</th>  
        <th>{% localized "SubscriptionListStrings|WebDevelopersSubscriptionTableStateHeader" %}</th>  
        <th>Action</th>  
      </tr>  
    </thead>  
    <tbody>  
      {% if subscriptions.size == 0 %}  
      <tr>  
        <td class="text-center" colspan="4">  
          {% localized "CommonResources|NoItemsToDisplay" %}  
        </td>  
      </tr>  
      {% else %}  
      {% for subscription in subscriptions %}  
      <tr id="{{subscription.id}}" {% if subscription.hasExpired %} class="expired" {% endif %}>  
        <td>  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelName" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.displayName }}  
            </div>  
            <div class="col-lg-2">  
              <a class="btn-link" href="/Subscriptions/{{subscription.id}}/Rename">Rename</a>  
            </div>  
            <div class="clearfix"></div>  
          </div>  
          {% if subscription.isAwaitingApproval %}  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelRequestedDate" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.createdDate | date:"MM/dd/yyyy" }}  
            </div>  
          </div>  
          {% else %}  
          {% if subscription.isRejected == false %}  
          {% if subscription.startDate %}  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelStartedDate" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.startDate | date:"MM/dd/yyyy" }}  
            </div>  
          </div>  
          {% endif %}  
  
          <!-- ko with: Developers.Account.Root.account.key('{{subscription.primaryKey}}', '{{subscription.id}}', true) -->  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|WebDevelopersPrimaryKey" %}</label>  
            <div class="col-lg-6">  
              <code data-bind="text: $data.displayKey()" id="primary_{{subscription.id}}"></code>  
            </div>  
            <div class="col-lg-2">  
              <!-- ko if: !requestInProgress() -->  
              <div class="nowrap">  
                <a href="#" class="btn-link" id="togglePrimary_{{subscription.id}}" data-bind="click: toggleKeyDisplay, text: toggleKeyLabel"></a>  
                |  
                <a href="#" class="btn-link" id="regeneratePrimary_{{subscription.id}}" data-bind="click: regenerateKey, text: regenerateKeyLabel"></a>  
              </div>  
              <!-- /ko -->  
              <!-- ko if: requestInProgress() -->  
              <div class="progress progress-striped active">  
                <div class="progress-bar" role="progressbar" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100" style="width: 100%">  
                  <span class="sr-only"></span>  
                </div>  
              </div>  
              <!-- /ko -->  
            </div>  
            <div class="clearfix"></div>  
          </div>  
          <!-- /ko -->  
          <!-- ko with: Developers.Account.Root.account.key('{{subscription.secondaryKey}}', '{{subscription.id}}', false) -->  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|WebDevelopersSecondaryKey" %}</label>  
            <div class="col-lg-6">  
              <code data-bind="text: $data.displayKey()" id="secondary_{{subscription.id}}"></code>  
            </div>  
            <div class="col-lg-2">  
              <div class="nowrap">  
                <a href="#" class="btn-link" id="toggleSecondary_{{subscription.id}}" data-bind="click: toggleKeyDisplay, text: toggleKeyLabel">{% localized "SubscriptionListStrings|ButtonLabelShowKey" %}</a>  
                |  
                <a href="#" class="btn-link" id="regenerateSecondary_{{subscription.id}}" data-bind="click: regenerateKey, text: regenerateKeyLabel">{% localized "SubscriptionListStrings|WebDevelopersRegenerateLink" %}</a>  
              </div>  
            </div>  
            <div class="clearfix"> </div>  
          </div>  
          <!-- /ko -->  
          {% endif %}  
          {% endif %}  
        </td>  
        <td>  
          <a href="{{subscription.productDetailsUrl}}">{{subscription.productTitle}}</a>  
        </td>  
        <td>  
          <strong>  
            {{subscription.state}}  
          </strong>  
        </td>  
        <td>  
          <div class="nowrap">  
            {% if subscription.canBeCancelled %}  
            <subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }"></subscription-cancel>  
            {% endif %}  
          </div>  
        </td>  
      </tr>  
      {% endfor %}  
      {% endif %}  
    </tbody>  
  </table>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="343d4-163">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="343d4-163">Controls</span></span>  
 <span data-ttu-id="343d4-164">Tato šablona může používat následující [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="343d4-164">This template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="343d4-165">zrušení předplatného</span><span class="sxs-lookup"><span data-stu-id="343d4-165">subscription-cancel</span></span>](api-management-page-controls.md#subscription-cancel)  
  
### <a name="data-model"></a><span data-ttu-id="343d4-166">Datový model</span><span class="sxs-lookup"><span data-stu-id="343d4-166">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="343d4-167">[Profil](#Profile), [aplikace](#Applications), a [odběry](#Subscriptions) šablony sdílet stejný datový model a přijímat data stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="343d4-167">The [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share the same data model and receive the same template data.</span></span>  
  
|<span data-ttu-id="343d4-168">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="343d4-168">Property</span></span>|<span data-ttu-id="343d4-169">Typ</span><span class="sxs-lookup"><span data-stu-id="343d4-169">Type</span></span>|<span data-ttu-id="343d4-170">Popis</span><span class="sxs-lookup"><span data-stu-id="343d4-170">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="343d4-171">FirstName</span><span class="sxs-lookup"><span data-stu-id="343d4-171">firstName</span></span>|<span data-ttu-id="343d4-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-172">string</span></span>|<span data-ttu-id="343d4-173">Křestní jméno aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-173">First name of the current user.</span></span>|  
|<span data-ttu-id="343d4-174">Příjmení</span><span class="sxs-lookup"><span data-stu-id="343d4-174">lastName</span></span>|<span data-ttu-id="343d4-175">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-175">string</span></span>|<span data-ttu-id="343d4-176">Příjmení aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-176">Last name of the current user.</span></span>|  
|<span data-ttu-id="343d4-177">NázevSpolečnosti</span><span class="sxs-lookup"><span data-stu-id="343d4-177">companyName</span></span>|<span data-ttu-id="343d4-178">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-178">string</span></span>|<span data-ttu-id="343d4-179">Název společnosti aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-179">The company name of the current user.</span></span>|  
|<span data-ttu-id="343d4-180">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="343d4-180">addresserEmail</span></span>|<span data-ttu-id="343d4-181">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-181">string</span></span>|<span data-ttu-id="343d4-182">E-mailovou adresu stávajícího uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-182">Email address of the current user.</span></span>|  
|<span data-ttu-id="343d4-183">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="343d4-183">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="343d4-184">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-184">string</span></span>|<span data-ttu-id="343d4-185">Relativní adresu URL chcete zobrazit analýzu pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-185">Relative URL to view analytics for the current user.</span></span>|  
|<span data-ttu-id="343d4-186">odběry</span><span class="sxs-lookup"><span data-stu-id="343d4-186">subscriptions</span></span>|<span data-ttu-id="343d4-187">Kolekce [předplatné](api-management-template-data-model-reference.md#Subscription) entity.</span><span class="sxs-lookup"><span data-stu-id="343d4-187">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="343d4-188">Odběry pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-188">The subscriptions for the current user.</span></span>|  
|<span data-ttu-id="343d4-189">aplikace</span><span class="sxs-lookup"><span data-stu-id="343d4-189">applications</span></span>|<span data-ttu-id="343d4-190">Kolekce [aplikace](api-management-template-data-model-reference.md#Application) entity.</span><span class="sxs-lookup"><span data-stu-id="343d4-190">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="343d4-191">Aplikace je aktuální uživatel.</span><span class="sxs-lookup"><span data-stu-id="343d4-191">The applications of the current user.</span></span>|  
|<span data-ttu-id="343d4-192">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="343d4-192">changePasswordUrl</span></span>|<span data-ttu-id="343d4-193">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-193">string</span></span>|<span data-ttu-id="343d4-194">Relativní adresa URL aktuální uživatel heslo změnit.</span><span class="sxs-lookup"><span data-stu-id="343d4-194">The relative URL to change the current user's password.</span></span>|  
|<span data-ttu-id="343d4-195">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="343d4-195">changeNameOrEmailUrl</span></span>|<span data-ttu-id="343d4-196">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-196">string</span></span>|<span data-ttu-id="343d4-197">Relativní adresa URL Chcete-li změnit název a e-mailu pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-197">The relative URL to change the name and email for the current user.</span></span>|  
|<span data-ttu-id="343d4-198">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="343d4-198">canChangePassword</span></span>|<span data-ttu-id="343d4-199">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="343d4-199">boolean</span></span>|<span data-ttu-id="343d4-200">Jestli aktuální uživatel může změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="343d4-200">Whether the current user can change their password.</span></span>|  
|<span data-ttu-id="343d4-201">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="343d4-201">isSystemUser</span></span>|<span data-ttu-id="343d4-202">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="343d4-202">boolean</span></span>|<span data-ttu-id="343d4-203">Jestli má aktuální uživatel je členem jedné z předdefinovaných [skupiny](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="343d4-203">Whether the current user is a member of one of the built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="343d4-204">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="343d4-204">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="343d4-205"><a name="Applications"></a>Aplikace</span><span class="sxs-lookup"><span data-stu-id="343d4-205"><a name="Applications"></a> Applications</span></span>  
 <span data-ttu-id="343d4-206">**Aplikace** šablony můžete vytvořit vlastní nastavení v části předplatná stránky profilu uživatele v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="343d4-206">The **Applications** template allows you to customize the subscriptions section of the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="343d4-207">![Stránky aplikací účtu uživatele](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM uživatelský účet stránky aplikací")</span><span class="sxs-lookup"><span data-stu-id="343d4-207">![User Account Applications Page](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM User Account Applications Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="343d4-208">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="343d4-208">Default template</span></span>  
  
```xml  
<div class="ap-account-applications">  
  <a id="RegisterApplication" href="/Developer/Applications/Register" class="btn btn-success pull-right">  
    <span class="glyphicon glyphicon-plus"></span>  
    <span>{% localized "ApplicationListStrings|WebDevelopersRegisterAppLink" %}</span>  
  </a>  
  <h2>{% localized "ApplicationListStrings|WebDevelopersYourApplicationsHeader" %}</h2>  
  
  <table class="table">  
    <thead>  
      <tr>  
        <th class="col-md-8">{% localized "ApplicationListStrings|WebDevelopersAppTableNameHeader" %}</th>  
        <th class="col-md-2">{% localized "ApplicationListStrings|WebDevelopersAppTableCategoryHeader" %}</th>  
        <th class="col-md-2" colspan="2">{% localized "ApplicationListStrings|WebDevelopersAppTableStateHeader" %}</th>  
      </tr>  
    </thead>  
    <tbody>  
  
      {% if applications.size == 0 %}  
  
      <tr>  
        <td class="col-md-12 text-center" colspan="4">  
          {% localized "CommonResources|NoItemsToDisplay" %}  
        </td>  
      </tr>  
  
      {% else %}  
  
      {% for app in applications %}  
      <tr>  
        <td class="col-md-8">  
          {{app.title}}  
        </td>  
        <td class="col-md-2">  
          {{app.categoryName}}  
        </td>  
        <td class="col-md-2">  
          <strong>  
            {% case app.state %}  
            {% when ApplicationStateModel.Registered %}  
            {% localized "ApplicationListStrings|WebDevelopersAppNotSubminted" %}  
  
            {% when ApplicationStateModel.Unpublished %}  
            {% localized "ApplicationListStrings|WebDevelopersAppNotPublished" %}  
  
            {% else %}  
            {{ app.state }}  
            {% endcase %}  
          </strong>  
        </td>  
        <td class="col-md-1">  
          <div class="nowrap">  
            {% if app.state != ApplicationStateModel.Submitted and app.state != ApplicationStateModel.Published %}  
            <app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
            {% endif %}  
          </div>  
        </td>  
      </tr>  
      {% endfor %}  
  
      {% endif %}  
    </tbody>  
  </table>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="343d4-209">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="343d4-209">Controls</span></span>  
 <span data-ttu-id="343d4-210">Tato šablona může používat následující [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="343d4-210">This template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="343d4-211">Akce aplikace</span><span class="sxs-lookup"><span data-stu-id="343d4-211">app-actions</span></span>](api-management-page-controls.md#app-actions)  
  
### <a name="data-model"></a><span data-ttu-id="343d4-212">Datový model</span><span class="sxs-lookup"><span data-stu-id="343d4-212">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="343d4-213">[Profil](#Profile), [aplikace](#Applications), a [odběry](#Subscriptions) šablony sdílet stejný datový model a přijímat data stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="343d4-213">The [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share the same data model and receive the same template data.</span></span>  
  
|<span data-ttu-id="343d4-214">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="343d4-214">Property</span></span>|<span data-ttu-id="343d4-215">Typ</span><span class="sxs-lookup"><span data-stu-id="343d4-215">Type</span></span>|<span data-ttu-id="343d4-216">Popis</span><span class="sxs-lookup"><span data-stu-id="343d4-216">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="343d4-217">FirstName</span><span class="sxs-lookup"><span data-stu-id="343d4-217">firstName</span></span>|<span data-ttu-id="343d4-218">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-218">string</span></span>|<span data-ttu-id="343d4-219">Křestní jméno aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-219">First name of the current user.</span></span>|  
|<span data-ttu-id="343d4-220">Příjmení</span><span class="sxs-lookup"><span data-stu-id="343d4-220">lastName</span></span>|<span data-ttu-id="343d4-221">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-221">string</span></span>|<span data-ttu-id="343d4-222">Příjmení aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-222">Last name of the current user.</span></span>|  
|<span data-ttu-id="343d4-223">NázevSpolečnosti</span><span class="sxs-lookup"><span data-stu-id="343d4-223">companyName</span></span>|<span data-ttu-id="343d4-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-224">string</span></span>|<span data-ttu-id="343d4-225">Název společnosti aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-225">The company name of the current user.</span></span>|  
|<span data-ttu-id="343d4-226">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="343d4-226">addresserEmail</span></span>|<span data-ttu-id="343d4-227">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-227">string</span></span>|<span data-ttu-id="343d4-228">E-mailovou adresu stávajícího uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-228">Email address of the current user.</span></span>|  
|<span data-ttu-id="343d4-229">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="343d4-229">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="343d4-230">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-230">string</span></span>|<span data-ttu-id="343d4-231">Relativní adresu URL chcete zobrazit analýzu pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-231">Relative URL to view analytics for the current user.</span></span>|  
|<span data-ttu-id="343d4-232">odběry</span><span class="sxs-lookup"><span data-stu-id="343d4-232">subscriptions</span></span>|<span data-ttu-id="343d4-233">Kolekce [předplatné](api-management-template-data-model-reference.md#Subscription) entity.</span><span class="sxs-lookup"><span data-stu-id="343d4-233">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="343d4-234">Odběry pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-234">The subscriptions for the current user.</span></span>|  
|<span data-ttu-id="343d4-235">aplikace</span><span class="sxs-lookup"><span data-stu-id="343d4-235">applications</span></span>|<span data-ttu-id="343d4-236">Kolekce [aplikace](api-management-template-data-model-reference.md#Application) entity.</span><span class="sxs-lookup"><span data-stu-id="343d4-236">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="343d4-237">Aplikace je aktuální uživatel.</span><span class="sxs-lookup"><span data-stu-id="343d4-237">The applications of the current user.</span></span>|  
|<span data-ttu-id="343d4-238">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="343d4-238">changePasswordUrl</span></span>|<span data-ttu-id="343d4-239">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-239">string</span></span>|<span data-ttu-id="343d4-240">Relativní adresa URL aktuální uživatel heslo změnit.</span><span class="sxs-lookup"><span data-stu-id="343d4-240">The relative URL to change the current user's password.</span></span>|  
|<span data-ttu-id="343d4-241">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="343d4-241">changeNameOrEmailUrl</span></span>|<span data-ttu-id="343d4-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="343d4-242">string</span></span>|<span data-ttu-id="343d4-243">Relativní adresa URL Chcete-li změnit název a e-mailu pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="343d4-243">The relative URL to change the name and email for the current user.</span></span>|  
|<span data-ttu-id="343d4-244">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="343d4-244">canChangePassword</span></span>|<span data-ttu-id="343d4-245">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="343d4-245">boolean</span></span>|<span data-ttu-id="343d4-246">Jestli aktuální uživatel může změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="343d4-246">Whether the current user can change their password.</span></span>|  
|<span data-ttu-id="343d4-247">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="343d4-247">isSystemUser</span></span>|<span data-ttu-id="343d4-248">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="343d4-248">boolean</span></span>|<span data-ttu-id="343d4-249">Jestli má aktuální uživatel je členem jedné z předdefinovaných [skupiny](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="343d4-249">Whether the current user is a member of one of the built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="343d4-250">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="343d4-250">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="343d4-251"><a name="UpdateAccountInfo"></a>Aktualizovat informace o účtu</span><span class="sxs-lookup"><span data-stu-id="343d4-251"><a name="UpdateAccountInfo"></a> Update account info</span></span>  
 <span data-ttu-id="343d4-252">**Informace o účtu Uodate** šablona umožňuje přizpůsobit **aktualizovat informace o účtu** stránku v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="343d4-252">The **Uodate account info** template allows you to customize the **Update account information** page in the developer portal.</span></span>  
  
 <span data-ttu-id="343d4-253">![Uživatel účet údaje vývojář stránky portálu šablony](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM uživatele účtu údaje vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="343d4-253">![User Account Info Page Developer Portal Templates](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM User Account Info Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="343d4-254">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="343d4-254">Default template</span></span>  
  
```xml  
<div class="row">  
  <div class="col-sm-6 col-md-6">  
    <div class="form-group">  
      <label for="Email">{% localized "SigninResources|TextboxLabelEmail" %}</label>  
      <input autofocus="autofocus" class="form-control" id="Email" name="Email" type="text" value="{{email}}">  
    </div>  
    <div class="form-group">  
      <label for="FirstName">{% localized "SigninResources|TextboxLabelEmailFirstName" %}</label>  
      <input class="form-control" id="FirstName" name="FirstName" type="text" value="{{firstName}}">  
    </div>  
    <div class="form-group">  
      <label for="LastName">{% localized "SigninResources|TextboxLabelEmailLastName" %}</label>  
      <input class="form-control" id="LastName" name="LastName" type="text" value="{{lastName}}">  
    </div>  
    <div class="form-group">  
      <label for="Password">{% localized "SigninResources|WebAuthenticationSigninPasswordLabel" %}</label>  
      <input class="form-control" id="Password" name="Password" type="password">  
    </div>  
  </div>  
</div>  
  
<button type="submit" class="btn btn-primary" id="UpdateProfile">  
  {% localized "UpdateProfileStrings|ButtonLabelUpdateProfile" %}  
</button>  
<a class="btn btn-default" href="/developer" role="button">  
  {% localized "CommonStrings|ButtonLabelCancel" %}  
</a>  
```  
  
### <a name="controls"></a><span data-ttu-id="343d4-255">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="343d4-255">Controls</span></span>  
 <span data-ttu-id="343d4-256">Tato šablona nesmíte používat žádné [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="343d4-256">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="343d4-257">Datový model</span><span class="sxs-lookup"><span data-stu-id="343d4-257">Data model</span></span>  
 <span data-ttu-id="343d4-258">[Informace o uživatelském účtu](api-management-template-data-model-reference.md#UserAccountInfo) entity.</span><span class="sxs-lookup"><span data-stu-id="343d4-258">[User account info](api-management-template-data-model-reference.md#UserAccountInfo) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="343d4-259">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="343d4-259">Sample template data</span></span>  
  
```json  
{  
    "FirstName": "Administrator",  
    "LastName": "",  
    "Email": "admin@live.com",  
    "Password": null,  
    "NameIdentifier": null,  
    "ProviderName": null,  
    "IsBasicAccount": false  
}  
```

## <a name="next-steps"></a><span data-ttu-id="343d4-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="343d4-260">Next steps</span></span>
<span data-ttu-id="343d4-261">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="343d4-261">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>