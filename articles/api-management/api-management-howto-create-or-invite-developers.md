---
title: "aaaHow Správa uživatelských účtů ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toocreate nebo pozvání uživatelů ve službě Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 078abfa5-1e4f-4c9d-b9c7-a172bd19c1a2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3966f4454e29621d7c615beefee352ec91b48b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-user-accounts-in-azure-api-management"></a>Jak toomanage uživatelských účtů ve službě Azure API Management
Ve službě API Management jsou vývojáři hello uživateli hello rozhraní API, která vystavit použití služby API Management. Tento průvodce ukazuje toohow toocreate a pozvání vývojáři toouse hello rozhraní API a produkty, abyste vytvořili dostupné toothem s vaší instance služby API Management. Informace o správě uživatelských účtů prostřednictvím kódu programu, najdete v části hello [entitu uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentaci v hello [rozhraní API služby REST pro správu](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkaz.

## <a name="create-developer"></a>Vytvořit nové vývojáře
Klikněte na tlačítko toocreate vývojář nové **portál vydavatele** v hello portál Azure pro služby API Management. Tím přejdete portál vydavatele toohello API Management. Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.

![Portál vydavatele][api-management-management-console]

Klikněte na tlačítko **uživatelé** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **přidat uživatele**.

![Vytvoření vývojáře][api-management-create-developer]

Zadejte hello **e-mailu**, **heslo**, a **název** pro vývojáře nové hello a klikněte na tlačítko **Uložit**.

![Vytvoření vývojáře][api-management-add-new-user]

Ve výchozím nastavení, jsou nově vytvořený vývojářským účtům **Active**a přidružené hello **vývojáři** skupiny.

![Nové vývojáře][api-management-new-developer]

Vývojářským účtům, které jsou v **active** stavu může být použité tooaccess všechny hello rozhraní API, k němuž mají odběry. tooassociate hello nově vytvořený vývojáře s další skupiny, najdete v části [jak tooassociate skupin k vývojářům][How tooassociate groups with developers].

## <a name="invite-developer"></a>Pozvat vývojář
tooinvite vývojář, klikněte na tlačítko **uživatelé** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **pozvat uživatele**.

![Pozvěte vývojáře][api-management-invite-developer]

Zadejte jméno a e-mailovou adresu hello hello vývojáře a klikněte na tlačítko **pozvat**.

![Pozvěte vývojáře][api-management-invite-developer-window]

Zobrazí se zpráva s potvrzením, ale hello nově pozvat vývojáře nejsou uvedené v seznamu hello až po přijetí pozvánky hello. 

![Pozvěte potvrzení][api-management-invite-developer-confirmation]

Když je pozvat vývojář, e-mail je odeslán toohello developer. Tento e-mail je generována pomocí šablony a přizpůsobit. Další informace najdete v tématu [konfigurovat e-mailových šablon][Configure email templates].

Po přijetí pozvání hello se stane aktivní účet hello.

## <a name="block-developer"></a> , Deaktivujte nebo opětovnou aktivací vývojářského účtu
Ve výchozím nastavení, jsou nově vytvořené nebo pozvané vývojářským účtům **Active**. Klikněte na tlačítko toodeactivate vývojářský účet **bloku**. Klikněte na tlačítko tooreactivate blokované vývojářského účtu, **aktivovat**. Blokované vývojářského účtu nelze přístup k portálu pro vývojáře hello nebo volání všechny rozhraní API. toodelete uživatelský účet, klikněte na tlačítko **odstranit**.

![Blok vývojáře][api-management-new-developer]

## <a name="reset-a-user-password"></a>Resetování hesla uživatele
tooreset hello heslo pro uživatelský účet, klikněte na název hello hello účtu.

![Resetování hesla][api-management-view-developer]

Klikněte na tlačítko **resetovat heslo** toosend na odkaz toohello uživatele tooreset své heslo.

![Resetování hesla][api-management-reset-password]

tooprogrammatically práci s uživatelskými účty, najdete v části hello [entitu uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentaci v hello [rozhraní API služby REST pro správu](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkaz. tooreset účet heslo tooa konkrétní hodnotu uživatele, můžete použít hello [aktualizaci uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operaci a zadejte požadované heslo hello.

## <a name="pending-verification"></a>Čeká na ověření
![Čeká na ověření][api-management-pending-verification]

## <a name="next-steps"></a>Další kroky
Po vytvoření vývojářského účtu můžete přiřadit k rolím a přihlásit se tooproducts a rozhraní API. Další informace najdete v tématu [jak toocreate a používání skupin][How toocreate and use groups].

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
