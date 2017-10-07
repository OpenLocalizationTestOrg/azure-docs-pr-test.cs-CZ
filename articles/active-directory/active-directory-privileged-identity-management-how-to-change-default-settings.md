---
title: "nastavení aktivace role toomanage aaaHow | Microsoft Docs"
description: "Zjistěte, jak toochange hello výchozí nastavení pro privilegované identity s hello rozšíření Azure Active Directory Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: f6cbcb6a-8a89-4077-afd8-06c94a64f4aa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 453bb6f8f8e0fd2598cb073ef86c1dd55458dc72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Jak toomanage nastavení aktivace role v Azure AD Privileged Identity Management
Správce privilegovaných rolí můžete přizpůsobit Azure AD Privileged Identity Management (PIM) ve své organizaci, včetně změny hello prostředí pro uživatele, který je aktivace přiřazení role vhodné.

## <a name="manage-hello-role-activation-settings"></a>Spravovat nastavení aktivace role hello
1. Přejděte toohello [portál Azure](https://portal.azure.com) a vyberte hello **Azure AD Privileged Identity Management** aplikace z řídicího panelu hello.
2. Vyberte **spravovat privilegované role** > **nastavení** > **privilegované role**.
3. Zvolte roli hello, jehož nastavení chcete toomanage.

Na stránce nastavení hello u každé role existuje několik nastavení, které můžete konfigurovat. Tato nastavení ovlivňují jenom uživatelé, kteří jsou správci oprávněné, není trvalých správců.

**Aktivace**: hello doba v hodinách, které role zůstává aktivní, než vyprší její platnost. To může být v rozmezí 1 až 72 hodin.

**Oznámení**: můžete zvolit, zda systém hello odešle e-mailů tooadmins potvrzení, že se mají aktivovat roli. To může být užitečné pro zjišťování neoprávněným nebo nezákonných aktivací.

**Incident nebo žádost o lístek**: můžete zvolit, zda toorequire oprávněné admins tooinclude lístek číselné při aktivují jejich role. To může být užitečné při provádění auditů role přístup.

**Služby Multi-Factor Authentication**: můžete zvolit, jestli uživatelé tooverify toorequire svou identitu pomocí vícefaktorového ověřování před aktivací jejich rolí. Mají pouze tooverify tento jednou na relaci, není pokaždé, když se aktivovat roli. Existují dva typy tookeep na paměti, když povolíte vícefaktorového ověřování:

* Uživatelé, kteří mají účty Microsoft pro jejich e-mailové adresy (obvykle @outlook.com, ale ne vždy) nelze zaregistrovat pro Azure MFA. Pokud chcete tooassign role toousers s účty Microsoft, by je provést trvalých správců nebo zakázat MFA pro tuto roli.
* Nelze zakázat MFA pro vysoce privilegované role pro Azure AD a Office 365. Toto je funkce zabezpečení, protože tyto role je třeba pečlivě chránit:  
  
  * Správce aplikací
  * Správce serveru Proxy aplikace
  * Správce fakturace  
  * Správce dodržování předpisů  
  * Správce služeb CRM
  * Schvalovatel přístup bezpečnostního modulu zákazníka
  * Zapisovač adresáře  
  * Správce serveru Exchange  
  * Globální správce
  * Správce služby Intune
  * Poštovní schránky správce  
  * Podpora tier1 partnera  
  * Podpora tier2 partnera  
  * Správce privilegovaných rolí   
  * Správce zabezpečení  
  * Správce služby SharePoint.  
  * Správce Skypu pro firmy  
  * Správce účtu uživatele  

Další informace o použití vícefaktorového ověřování se PIM najdete v části [jak tooRequire MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what hello temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

