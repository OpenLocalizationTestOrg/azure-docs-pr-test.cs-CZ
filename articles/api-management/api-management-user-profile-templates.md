---
title: "AAA \"šablony profilu uživatele ve službě Azure API Management | Microsoft Docs\""
description: "Zjistěte, jak se obsah hello toocustomize hello profilu uživatele stránky v hello portál pro vývojáře ve službě Azure API Management."
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
ms.openlocfilehash: c8f153b310221164809acf58e4af236928ceb41d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-profile-templates-in-azure-api-management"></a><span data-ttu-id="4b19e-103">Šablony profilu uživatele ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="4b19e-103">User profile templates in Azure API Management</span></span>
<span data-ttu-id="4b19e-104">Azure API Management poskytuje že Hello možnost toocustomize hello obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="4b19e-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="4b19e-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a hello editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a zadané sadu lokalizované [řetězce prostředků](api-management-template-resources.md#strings), [ Prostředky glyfy](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu tooconfigure hello obsah hello stránek podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="4b19e-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="4b19e-106">Hello šablony v této části Povolit toocustomize hello obsah hello stránek profilu uživatele na portál pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-106">hello templates in this section allow you toocustomize hello content of hello User profile pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="4b19e-107">Profil</span><span class="sxs-lookup"><span data-stu-id="4b19e-107">Profile</span></span>](#Profile)  
  
-   [<span data-ttu-id="4b19e-108">Předplatná</span><span class="sxs-lookup"><span data-stu-id="4b19e-108">Subscriptions</span></span>](#Subscriptions)  
  
-   [<span data-ttu-id="4b19e-109">Aplikace</span><span class="sxs-lookup"><span data-stu-id="4b19e-109">Applications</span></span>](#Applications)  
  
-   [<span data-ttu-id="4b19e-110">Aktualizovat informace o účtu</span><span class="sxs-lookup"><span data-stu-id="4b19e-110">Update account info</span></span>](#UpdateAccountInfo)  
  
> [!NOTE]
>  <span data-ttu-id="4b19e-111">Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci hello, ale jsou toochange subjektu z důvodu vylepšení toocontinuous.</span><span class="sxs-lookup"><span data-stu-id="4b19e-111">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="4b19e-112">Hello za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře hello přechodem toohello potřeby jednotlivých šablony.</span><span class="sxs-lookup"><span data-stu-id="4b19e-112">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="4b19e-113">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="4b19e-113">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="4b19e-114"><a name="Profile"></a>Profil</span><span class="sxs-lookup"><span data-stu-id="4b19e-114"><a name="Profile"></a> Profile</span></span>  
 <span data-ttu-id="4b19e-115">Hello **profil** šablona vám umožní oddíl profilu uživatele toocustomize hello hello stránce profilu uživatele v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-115">hello **profile** template allows you toocustomize hello user profile section of hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="4b19e-116">![Stránku profilu uživatele](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM uživatelský profil stránky")</span><span class="sxs-lookup"><span data-stu-id="4b19e-116">![User Profile Page](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM User Profile Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="4b19e-117">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="4b19e-117">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="4b19e-118">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="4b19e-118">Controls</span></span>  
 <span data-ttu-id="4b19e-119">Tato šablona nesmíte používat žádné [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="4b19e-119">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="4b19e-120">Datový model</span><span class="sxs-lookup"><span data-stu-id="4b19e-120">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="4b19e-121">Hello [profil](#Profile), [aplikace](#Applications), a [odběry](#Subscriptions) šablony sdílet hello stejný datový model a přijímat hello stejná data šablony.</span><span class="sxs-lookup"><span data-stu-id="4b19e-121">hello [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share hello same data model and receive hello same template data.</span></span>  
  
|<span data-ttu-id="4b19e-122">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4b19e-122">Property</span></span>|<span data-ttu-id="4b19e-123">Typ</span><span class="sxs-lookup"><span data-stu-id="4b19e-123">Type</span></span>|<span data-ttu-id="4b19e-124">Popis</span><span class="sxs-lookup"><span data-stu-id="4b19e-124">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="4b19e-125">FirstName</span><span class="sxs-lookup"><span data-stu-id="4b19e-125">firstName</span></span>|<span data-ttu-id="4b19e-126">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-126">string</span></span>|<span data-ttu-id="4b19e-127">Křestní jméno hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-127">First name of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-128">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4b19e-128">lastName</span></span>|<span data-ttu-id="4b19e-129">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-129">string</span></span>|<span data-ttu-id="4b19e-130">Příjmení hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-130">Last name of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-131">NázevSpolečnosti</span><span class="sxs-lookup"><span data-stu-id="4b19e-131">companyName</span></span>|<span data-ttu-id="4b19e-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-132">string</span></span>|<span data-ttu-id="4b19e-133">Název společnosti Hello hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-133">hello company name of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-134">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="4b19e-134">addresserEmail</span></span>|<span data-ttu-id="4b19e-135">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-135">string</span></span>|<span data-ttu-id="4b19e-136">E-mailová adresa hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-136">Email address of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-137">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="4b19e-137">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="4b19e-138">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-138">string</span></span>|<span data-ttu-id="4b19e-139">Analýzy tooview relativní adresa URL pro aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-139">Relative URL tooview analytics for hello current user.</span></span>|  
|<span data-ttu-id="4b19e-140">odběry</span><span class="sxs-lookup"><span data-stu-id="4b19e-140">subscriptions</span></span>|<span data-ttu-id="4b19e-141">Kolekce [předplatné](api-management-template-data-model-reference.md#Subscription) entity.</span><span class="sxs-lookup"><span data-stu-id="4b19e-141">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="4b19e-142">Hello odběry pro aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-142">hello subscriptions for hello current user.</span></span>|  
|<span data-ttu-id="4b19e-143">aplikace</span><span class="sxs-lookup"><span data-stu-id="4b19e-143">applications</span></span>|<span data-ttu-id="4b19e-144">Kolekce [aplikace](api-management-template-data-model-reference.md#Application) entity.</span><span class="sxs-lookup"><span data-stu-id="4b19e-144">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="4b19e-145">Hello aplikace hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-145">hello applications of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-146">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="4b19e-146">changePasswordUrl</span></span>|<span data-ttu-id="4b19e-147">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-147">string</span></span>|<span data-ttu-id="4b19e-148">Hello relativní adresa URL toochange hello aktuální heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-148">hello relative URL toochange hello current user's password.</span></span>|  
|<span data-ttu-id="4b19e-149">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="4b19e-149">changeNameOrEmailUrl</span></span>|<span data-ttu-id="4b19e-150">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-150">string</span></span>|<span data-ttu-id="4b19e-151">Dobrý den, relativní název hello toochange adresy URL a e-mailu pro aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-151">hello relative URL toochange hello name and email for hello current user.</span></span>|  
|<span data-ttu-id="4b19e-152">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="4b19e-152">canChangePassword</span></span>|<span data-ttu-id="4b19e-153">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4b19e-153">boolean</span></span>|<span data-ttu-id="4b19e-154">Jestli hello aktuální uživatel může změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="4b19e-154">Whether hello current user can change their password.</span></span>|  
|<span data-ttu-id="4b19e-155">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="4b19e-155">isSystemUser</span></span>|<span data-ttu-id="4b19e-156">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4b19e-156">boolean</span></span>|<span data-ttu-id="4b19e-157">Jestli hello aktuální uživatel je členem jedné z předdefinovaných hello [skupiny](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="4b19e-157">Whether hello current user is a member of one of hello built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="4b19e-158">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="4b19e-158">Sample template data</span></span>  
  
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
            "ProductDescription": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
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
            "ProductDescription": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
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
  
##  <span data-ttu-id="4b19e-159"><a name="Subscriptions"></a>Odběry</span><span class="sxs-lookup"><span data-stu-id="4b19e-159"><a name="Subscriptions"></a> Subscriptions</span></span>  
 <span data-ttu-id="4b19e-160">Hello **odběry** šablona vám umožní toocustomize hello odběry oddílu hello stránce profilu uživatele v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-160">hello **Subscriptions** template allows you toocustomize hello subscriptions section of hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="4b19e-161">![Stránku odběru uživatele](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "stránku odběru APIM uživatele")</span><span class="sxs-lookup"><span data-stu-id="4b19e-161">![User Subscription Page](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM User Subscription Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="4b19e-162">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="4b19e-162">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="4b19e-163">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="4b19e-163">Controls</span></span>  
 <span data-ttu-id="4b19e-164">Tato šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="4b19e-164">This template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="4b19e-165">zrušení předplatného</span><span class="sxs-lookup"><span data-stu-id="4b19e-165">subscription-cancel</span></span>](api-management-page-controls.md#subscription-cancel)  
  
### <a name="data-model"></a><span data-ttu-id="4b19e-166">Datový model</span><span class="sxs-lookup"><span data-stu-id="4b19e-166">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="4b19e-167">Hello [profil](#Profile), [aplikace](#Applications), a [odběry](#Subscriptions) šablony sdílet hello stejný datový model a přijímat hello stejná data šablony.</span><span class="sxs-lookup"><span data-stu-id="4b19e-167">hello [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share hello same data model and receive hello same template data.</span></span>  
  
|<span data-ttu-id="4b19e-168">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4b19e-168">Property</span></span>|<span data-ttu-id="4b19e-169">Typ</span><span class="sxs-lookup"><span data-stu-id="4b19e-169">Type</span></span>|<span data-ttu-id="4b19e-170">Popis</span><span class="sxs-lookup"><span data-stu-id="4b19e-170">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="4b19e-171">FirstName</span><span class="sxs-lookup"><span data-stu-id="4b19e-171">firstName</span></span>|<span data-ttu-id="4b19e-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-172">string</span></span>|<span data-ttu-id="4b19e-173">Křestní jméno hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-173">First name of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-174">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4b19e-174">lastName</span></span>|<span data-ttu-id="4b19e-175">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-175">string</span></span>|<span data-ttu-id="4b19e-176">Příjmení hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-176">Last name of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-177">NázevSpolečnosti</span><span class="sxs-lookup"><span data-stu-id="4b19e-177">companyName</span></span>|<span data-ttu-id="4b19e-178">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-178">string</span></span>|<span data-ttu-id="4b19e-179">Název společnosti Hello hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-179">hello company name of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-180">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="4b19e-180">addresserEmail</span></span>|<span data-ttu-id="4b19e-181">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-181">string</span></span>|<span data-ttu-id="4b19e-182">E-mailová adresa hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-182">Email address of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-183">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="4b19e-183">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="4b19e-184">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-184">string</span></span>|<span data-ttu-id="4b19e-185">Analýzy tooview relativní adresa URL pro aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-185">Relative URL tooview analytics for hello current user.</span></span>|  
|<span data-ttu-id="4b19e-186">odběry</span><span class="sxs-lookup"><span data-stu-id="4b19e-186">subscriptions</span></span>|<span data-ttu-id="4b19e-187">Kolekce [předplatné](api-management-template-data-model-reference.md#Subscription) entity.</span><span class="sxs-lookup"><span data-stu-id="4b19e-187">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="4b19e-188">Hello odběry pro aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-188">hello subscriptions for hello current user.</span></span>|  
|<span data-ttu-id="4b19e-189">aplikace</span><span class="sxs-lookup"><span data-stu-id="4b19e-189">applications</span></span>|<span data-ttu-id="4b19e-190">Kolekce [aplikace](api-management-template-data-model-reference.md#Application) entity.</span><span class="sxs-lookup"><span data-stu-id="4b19e-190">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="4b19e-191">Hello aplikace hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-191">hello applications of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-192">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="4b19e-192">changePasswordUrl</span></span>|<span data-ttu-id="4b19e-193">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-193">string</span></span>|<span data-ttu-id="4b19e-194">Hello relativní adresa URL toochange hello aktuální heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-194">hello relative URL toochange hello current user's password.</span></span>|  
|<span data-ttu-id="4b19e-195">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="4b19e-195">changeNameOrEmailUrl</span></span>|<span data-ttu-id="4b19e-196">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-196">string</span></span>|<span data-ttu-id="4b19e-197">Dobrý den, relativní název hello toochange adresy URL a e-mailu pro aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-197">hello relative URL toochange hello name and email for hello current user.</span></span>|  
|<span data-ttu-id="4b19e-198">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="4b19e-198">canChangePassword</span></span>|<span data-ttu-id="4b19e-199">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4b19e-199">boolean</span></span>|<span data-ttu-id="4b19e-200">Jestli hello aktuální uživatel může změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="4b19e-200">Whether hello current user can change their password.</span></span>|  
|<span data-ttu-id="4b19e-201">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="4b19e-201">isSystemUser</span></span>|<span data-ttu-id="4b19e-202">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4b19e-202">boolean</span></span>|<span data-ttu-id="4b19e-203">Jestli hello aktuální uživatel je členem jedné z předdefinovaných hello [skupiny](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="4b19e-203">Whether hello current user is a member of one of hello built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="4b19e-204">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="4b19e-204">Sample template data</span></span>  
  
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
            "ProductDescription": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
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
            "ProductDescription": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
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
  
##  <span data-ttu-id="4b19e-205"><a name="Applications"></a>Aplikace</span><span class="sxs-lookup"><span data-stu-id="4b19e-205"><a name="Applications"></a> Applications</span></span>  
 <span data-ttu-id="4b19e-206">Hello **aplikace** šablona vám umožní toocustomize hello odběry oddílu hello stránce profilu uživatele v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-206">hello **Applications** template allows you toocustomize hello subscriptions section of hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="4b19e-207">![Stránky aplikací účtu uživatele](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM uživatelský účet stránky aplikací")</span><span class="sxs-lookup"><span data-stu-id="4b19e-207">![User Account Applications Page](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM User Account Applications Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="4b19e-208">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="4b19e-208">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="4b19e-209">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="4b19e-209">Controls</span></span>  
 <span data-ttu-id="4b19e-210">Tato šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="4b19e-210">This template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="4b19e-211">Akce aplikace</span><span class="sxs-lookup"><span data-stu-id="4b19e-211">app-actions</span></span>](api-management-page-controls.md#app-actions)  
  
### <a name="data-model"></a><span data-ttu-id="4b19e-212">Datový model</span><span class="sxs-lookup"><span data-stu-id="4b19e-212">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="4b19e-213">Hello [profil](#Profile), [aplikace](#Applications), a [odběry](#Subscriptions) šablony sdílet hello stejný datový model a přijímat hello stejná data šablony.</span><span class="sxs-lookup"><span data-stu-id="4b19e-213">hello [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share hello same data model and receive hello same template data.</span></span>  
  
|<span data-ttu-id="4b19e-214">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4b19e-214">Property</span></span>|<span data-ttu-id="4b19e-215">Typ</span><span class="sxs-lookup"><span data-stu-id="4b19e-215">Type</span></span>|<span data-ttu-id="4b19e-216">Popis</span><span class="sxs-lookup"><span data-stu-id="4b19e-216">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="4b19e-217">FirstName</span><span class="sxs-lookup"><span data-stu-id="4b19e-217">firstName</span></span>|<span data-ttu-id="4b19e-218">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-218">string</span></span>|<span data-ttu-id="4b19e-219">Křestní jméno hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-219">First name of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-220">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4b19e-220">lastName</span></span>|<span data-ttu-id="4b19e-221">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-221">string</span></span>|<span data-ttu-id="4b19e-222">Příjmení hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-222">Last name of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-223">NázevSpolečnosti</span><span class="sxs-lookup"><span data-stu-id="4b19e-223">companyName</span></span>|<span data-ttu-id="4b19e-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-224">string</span></span>|<span data-ttu-id="4b19e-225">Název společnosti Hello hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-225">hello company name of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-226">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="4b19e-226">addresserEmail</span></span>|<span data-ttu-id="4b19e-227">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-227">string</span></span>|<span data-ttu-id="4b19e-228">E-mailová adresa hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-228">Email address of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-229">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="4b19e-229">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="4b19e-230">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-230">string</span></span>|<span data-ttu-id="4b19e-231">Analýzy tooview relativní adresa URL pro aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-231">Relative URL tooview analytics for hello current user.</span></span>|  
|<span data-ttu-id="4b19e-232">odběry</span><span class="sxs-lookup"><span data-stu-id="4b19e-232">subscriptions</span></span>|<span data-ttu-id="4b19e-233">Kolekce [předplatné](api-management-template-data-model-reference.md#Subscription) entity.</span><span class="sxs-lookup"><span data-stu-id="4b19e-233">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="4b19e-234">Hello odběry pro aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-234">hello subscriptions for hello current user.</span></span>|  
|<span data-ttu-id="4b19e-235">aplikace</span><span class="sxs-lookup"><span data-stu-id="4b19e-235">applications</span></span>|<span data-ttu-id="4b19e-236">Kolekce [aplikace](api-management-template-data-model-reference.md#Application) entity.</span><span class="sxs-lookup"><span data-stu-id="4b19e-236">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="4b19e-237">Hello aplikace hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-237">hello applications of hello current user.</span></span>|  
|<span data-ttu-id="4b19e-238">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="4b19e-238">changePasswordUrl</span></span>|<span data-ttu-id="4b19e-239">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-239">string</span></span>|<span data-ttu-id="4b19e-240">Hello relativní adresa URL toochange hello aktuální heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b19e-240">hello relative URL toochange hello current user's password.</span></span>|  
|<span data-ttu-id="4b19e-241">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="4b19e-241">changeNameOrEmailUrl</span></span>|<span data-ttu-id="4b19e-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b19e-242">string</span></span>|<span data-ttu-id="4b19e-243">Dobrý den, relativní název hello toochange adresy URL a e-mailu pro aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-243">hello relative URL toochange hello name and email for hello current user.</span></span>|  
|<span data-ttu-id="4b19e-244">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="4b19e-244">canChangePassword</span></span>|<span data-ttu-id="4b19e-245">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4b19e-245">boolean</span></span>|<span data-ttu-id="4b19e-246">Jestli hello aktuální uživatel může změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="4b19e-246">Whether hello current user can change their password.</span></span>|  
|<span data-ttu-id="4b19e-247">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="4b19e-247">isSystemUser</span></span>|<span data-ttu-id="4b19e-248">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4b19e-248">boolean</span></span>|<span data-ttu-id="4b19e-249">Jestli hello aktuální uživatel je členem jedné z předdefinovaných hello [skupiny](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="4b19e-249">Whether hello current user is a member of one of hello built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="4b19e-250">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="4b19e-250">Sample template data</span></span>  
  
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
            "ProductDescription": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
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
            "ProductDescription": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
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
  
##  <span data-ttu-id="4b19e-251"><a name="UpdateAccountInfo"></a>Aktualizovat informace o účtu</span><span class="sxs-lookup"><span data-stu-id="4b19e-251"><a name="UpdateAccountInfo"></a> Update account info</span></span>  
 <span data-ttu-id="4b19e-252">Hello **informace o účtu Uodate** šablona vám umožní toocustomize hello **aktualizovat informace o účtu** stránku v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="4b19e-252">hello **Uodate account info** template allows you toocustomize hello **Update account information** page in hello developer portal.</span></span>  
  
 <span data-ttu-id="4b19e-253">![Uživatel účet údaje vývojář stránky portálu šablony](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM uživatele účtu údaje vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="4b19e-253">![User Account Info Page Developer Portal Templates](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM User Account Info Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="4b19e-254">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="4b19e-254">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="4b19e-255">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="4b19e-255">Controls</span></span>  
 <span data-ttu-id="4b19e-256">Tato šablona nesmíte používat žádné [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="4b19e-256">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="4b19e-257">Datový model</span><span class="sxs-lookup"><span data-stu-id="4b19e-257">Data model</span></span>  
 <span data-ttu-id="4b19e-258">[Informace o uživatelském účtu](api-management-template-data-model-reference.md#UserAccountInfo) entity.</span><span class="sxs-lookup"><span data-stu-id="4b19e-258">[User account info](api-management-template-data-model-reference.md#UserAccountInfo) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="4b19e-259">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="4b19e-259">Sample template data</span></span>  
  
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

## <a name="next-steps"></a><span data-ttu-id="4b19e-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b19e-260">Next steps</span></span>
<span data-ttu-id="4b19e-261">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="4b19e-261">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
