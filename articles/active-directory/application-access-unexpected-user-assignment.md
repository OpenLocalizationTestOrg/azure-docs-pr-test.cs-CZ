---
title: "aaaHow tooAssign uživatelé tooapplications | Microsoft Docs"
description: "Pochopit, jak uživatelé přiřazovány tooan aplikace ve vašem klientovi"
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
ms.openlocfilehash: 4df60c7d723140d0d1bbd6ba8b34aa4e762d1138
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-tooapplications"></a>Jak uživatelé tooapplications tooassign

Tento článek vám pomůže toounderstand jak uživatelé přiřazovány tooan aplikace ve vašem klientovi.

## <a name="how-do-users-get-assigned-tooan-application-in-azure-ad"></a>Jak uživatelé přiřazovány tooan aplikace ve službě Azure AD?

Pro uživatele tooaccess aplikace se musí být přiřazena tooit nějakým způsobem. Přiřazení můžete provést pomocí správce, delegovaného obchodní nebo v některých případech hello uživatele sami. Níže jsou popsány hello způsoby, které uživatelé mohou přiřadit tooapplications:

1.  Správce [přiřadí uživatele](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) přímo toohello aplikace

2.  Správce [přiřadí skupinu](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) tento hello uživatel je členem toohello aplikace, včetně:

  * Skupinu, která byla synchronizované z místní

  * Skupiny zabezpečení statické vytvořeny v cloudu hello

  * A [skupiny dynamické zabezpečení](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) vytvořeny v cloudu hello

  * Skupiny Office 365 v cloudu hello

  * Hello [všichni uživatelé](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) skupiny

3.  Správce povolí [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow tooadd uživatele aplikace pomocí hello [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **přidat aplikaci** funkce **bez obchodní schválení**

4.  Správce povolí [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow tooadd uživatele aplikace pomocí hello [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **přidat aplikaci** funkce, ale pouze w **i-té předchozí schválení od vybraná sada business schvalovatelů**

5.  Správce povolí [Samoobslužná správa skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow toojoin uživatele skupiny přiřazené aplikace je příliš**bez obchodní schválení**

6.  Správce povolí [Samoobslužná správa skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow toojoin uživatelské skupiny, která aplikace je přiřazena k, ale pouze **s předchozí schválení od vybraná sada business schvalovatelů**

7.  Správce přiřadí licence tooa uživatele přímo pro první strany aplikace, jako je třeba [Microsoft Office 365](http://products.office.com/)

8.  Správce přiřadí tooa skupinu licencí, které hello uživatel je členem tooa první strany aplikace, jako je [Microsoft Office 365](http://products.office.com/)

9.  [Správce souhlasí tooan aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toobe používány všemi uživateli a potom se uživatel přihlásí v aplikaci toohello

10. Uživatel [souhlasí tooan aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) sami přihlášením toohello aplikace

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
