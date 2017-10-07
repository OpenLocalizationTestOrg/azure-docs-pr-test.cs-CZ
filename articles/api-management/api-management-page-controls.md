---
title: "ovládací prvky stránky aaaAzure API Management | Microsoft Docs"
description: "Další informace o hello ovládací prvky stránky k dispozici pro použití v šablonách portálu vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 03e0ac8d-64ff-4e9a-b029-d7be14fb31e3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 1a16a6fce197c0a2e14807ac21e81a9a73b515b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-page-controls"></a>Ovládací prvky stránky Azure API Management
Azure API Management obsahuje následující ovládací prvky pro použití v šablonách portálu vývojáře hello hello.  
  
 toouse ovládacího prvku ji umístíte do hello požadovaného umístění v šabloně portálu vývojáře hello. Některé ovládací prvky, jako je například hello [akcí aplikace](#app-actions) řízení, musí mít parametry, jak ukazuje následující příklad hello.  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 Hello hodnoty parametrů hello jsou předána jako součást hello datový model pro šablonu hello. Ve většině případů můžete jednoduše vložit hello poskytuje příklad pro každý ovládací prvek pro něj toowork správně. Další informace o hello hodnoty parametrů uvidíte oddíl modelu dat hello ke každé šabloně, ve kterém může použít ovládacího prvku.  
  
 Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
## <a name="developer-portal-template-page-controls"></a>Ovládací prvky stránky šablony portálu vývojáře  
  
-   [Akce aplikace](#app-actions)  
  
-   [Basic přihlášení](#basic-signin)  
  
-   [ovládací prvek stránkování](#paging-control)  
  
-   [Zprostředkovatelé](#providers)  
  
-   [ovládací prvek vyhledávání](#search-control)  
  
-   [registrace](#sign-up)  
  
-   [přihlášení k odběru tlačítko](#subscribe-button)  
  
-   [zrušení předplatného](#subscription-cancel)  
  
##  <a name="app-actions"></a>Akce aplikace  
 Hello `app-actions` řízení poskytuje uživatelské rozhraní pro interakci s aplikací na stránce profilu uživatele hello v portálu pro vývojáře hello.  
  
 ![aplikace & č. 45; akce řízení](./media/api-management-page-controls/APIM-app-actions-control.png "APIM akcí aplikace řízení")  
  
### <a name="usage"></a>Využití  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parametry  
  
|Parametr|Popis|  
|---------------|-----------------|  
|appId|Hello id aplikace hello.|  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 Hello `app-actions` řízení je možné použít následující šablony na portálu vývojáře hello.  
  
-   [Aplikace](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a>Basic přihlášení  
 Hello `basic-signin` řízení poskytuje ovládací prvek pro shromažďování přihlášení uživatele informace v hello přihlašovací stránku v portálu pro vývojáře hello.  
  
 ![Basic & č. 45; přihlášení řízení](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic přihlášení řízení")  
  
### <a name="usage"></a>Využití  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 Hello `basic-signin` řízení je možné použít následující šablony na portálu vývojáře hello.  
  
-   [Přihlásit se](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a>ovládací prvek stránkování  
 Hello `paging-control` poskytuje funkce stránkování vývojáře portálu stránky zobrazující seznam položek.  
  
 ![stránkování řízení](./media/api-management-page-controls/APIM-paging-control.png "APIM stránkování ovládacího prvku")  
  
### <a name="usage"></a>Využití  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 Hello `paging-control` řízení je možné použít následující šablony na portálu vývojáře hello.  
  
-   [Rozhraní API seznamu](api-management-api-templates.md#APIList)  
  
-   [Seznam problémů](api-management-issue-templates.md#IssueList)  
  
-   [Seznam produktů](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a>Zprostředkovatelé  
 Hello `providers` řízení poskytuje ovládací prvek pro výběr zprostředkovatelů ověřování v hello přihlašovací stránku v portálu pro vývojáře hello.  
  
 ![poskytovatelé řízení](./media/api-management-page-controls/APIM-providers-control.png "APIM poskytovatelé řízení")  
  
### <a name="usage"></a>Využití  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 Hello `providers` řízení je možné použít následující šablony na portálu vývojáře hello.  
  
-   [Přihlásit se](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a>ovládací prvek vyhledávání  
 Hello `search-control` poskytuje funkce hledání vývojáře portálu stránky zobrazující seznam položek.  
  
 ![hledání řízení](./media/api-management-page-controls/APIM-search-control.png "APIM vyhledávání řízení")  
  
### <a name="usage"></a>Využití  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 Hello `search-control` řízení je možné použít následující šablony na portálu vývojáře hello.  
  
-   [Rozhraní API seznamu](api-management-api-templates.md#APIList)  
  
-   [Seznam produktů](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a>registrace  
 Hello `sign-up` řízení poskytuje ovládací prvek pro shromažďování informací o profilu uživatele hello registrační stránku v portálu pro vývojáře hello.  
  
 ![přihlašovací & č. 45; až řízení](./media/api-management-page-controls/APIM-sign-up-control.png "APIM registrace ovládacího prvku")  
  
### <a name="usage"></a>Využití  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 Hello `sign-up` řízení je možné použít následující šablony na portálu vývojáře hello.  
  
-   [Registrace](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a>přihlášení k odběru tlačítko  
 Hello `subscribe-button` poskytuje ovládací prvek pro přihlášení k odběru produktu tooa uživatele.  
  
 ![přihlášení k odběru & č. 45; tlačítko – ovládací prvek](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM přihlášení k odběru tlačítko – ovládací prvek")  
  
### <a name="usage"></a>Využití  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 Hello `subscribe-button` řízení je možné použít následující šablony na portálu vývojáře hello.  
  
-   [Produktu](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a>zrušení předplatného  
 Hello `subscription-cancel` řízení poskytuje ovládací prvek pro zrušení předplatného produktu tooa v hello profilu uživatele na stránce portálu pro vývojáře hello.  
  
 ![předplatné & č. 45; zrušit řízení](./media/api-management-page-controls/APIM-subscription-cancel-control.png "řízení APIM zrušit předplatné")  
  
### <a name="usage"></a>Využití  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Parametry  
  
|Parametr|Popis|  
|---------------|-----------------|  
|subscriptionId|Hello id předplatného toocancel hello.|  
|cancelUrl|Hello předplatné zrušit adresy URL.|  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 Hello `subscription-cancel` řízení je možné použít následující šablony na portálu vývojáře hello.  
  
-   [Produktu](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Další kroky
Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).
