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
# <a name="how-toomanage-user-accounts-in-azure-api-management"></a><span data-ttu-id="b828b-103">Jak toomanage uživatelských účtů ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="b828b-103">How toomanage user accounts in Azure API Management</span></span>
<span data-ttu-id="b828b-104">Ve službě API Management jsou vývojáři hello uživateli hello rozhraní API, která vystavit použití služby API Management.</span><span class="sxs-lookup"><span data-stu-id="b828b-104">In API Management, developers are hello users of hello APIs that you expose using API Management.</span></span> <span data-ttu-id="b828b-105">Tento průvodce ukazuje toohow toocreate a pozvání vývojáři toouse hello rozhraní API a produkty, abyste vytvořili dostupné toothem s vaší instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="b828b-105">This guide shows toohow toocreate and invite developers toouse hello APIs and products that you make available toothem with your API Management instance.</span></span> <span data-ttu-id="b828b-106">Informace o správě uživatelských účtů prostřednictvím kódu programu, najdete v části hello [entitu uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentaci v hello [rozhraní API služby REST pro správu](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkaz.</span><span class="sxs-lookup"><span data-stu-id="b828b-106">For information on managing user accounts programmatically, see hello [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span>

## <span data-ttu-id="b828b-107"><a name="create-developer"></a>Vytvořit nové vývojáře</span><span class="sxs-lookup"><span data-stu-id="b828b-107"><a name="create-developer"> </a>Create a new developer</span></span>
<span data-ttu-id="b828b-108">Klikněte na tlačítko toocreate vývojář nové **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="b828b-108">toocreate a new developer, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="b828b-109">Tím přejdete portál vydavatele toohello API Management.</span><span class="sxs-lookup"><span data-stu-id="b828b-109">This takes you toohello API Management publisher portal.</span></span> <span data-ttu-id="b828b-110">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="b828b-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Portál vydavatele][api-management-management-console]

<span data-ttu-id="b828b-112">Klikněte na tlačítko **uživatelé** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b828b-112">Click **Users** from hello **API Management** menu on hello left, and then click **add user**.</span></span>

![Vytvoření vývojáře][api-management-create-developer]

<span data-ttu-id="b828b-114">Zadejte hello **e-mailu**, **heslo**, a **název** pro vývojáře nové hello a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b828b-114">Enter hello **Email**, **Password**, and **Name** for hello new developer and click **Save**.</span></span>

![Vytvoření vývojáře][api-management-add-new-user]

<span data-ttu-id="b828b-116">Ve výchozím nastavení, jsou nově vytvořený vývojářským účtům **Active**a přidružené hello **vývojáři** skupiny.</span><span class="sxs-lookup"><span data-stu-id="b828b-116">By default, newly created developer accounts are **Active**, and associated with hello **Developers** group.</span></span>

![Nové vývojáře][api-management-new-developer]

<span data-ttu-id="b828b-118">Vývojářským účtům, které jsou v **active** stavu může být použité tooaccess všechny hello rozhraní API, k němuž mají odběry.</span><span class="sxs-lookup"><span data-stu-id="b828b-118">Developer accounts that are in an **active** state can be used tooaccess all of hello APIs for which they have subscriptions.</span></span> <span data-ttu-id="b828b-119">tooassociate hello nově vytvořený vývojáře s další skupiny, najdete v části [jak tooassociate skupin k vývojářům][How tooassociate groups with developers].</span><span class="sxs-lookup"><span data-stu-id="b828b-119">tooassociate hello newly created developer with additional groups, see [How tooassociate groups with developers][How tooassociate groups with developers].</span></span>

## <span data-ttu-id="b828b-120"><a name="invite-developer"></a>Pozvat vývojář</span><span class="sxs-lookup"><span data-stu-id="b828b-120"><a name="invite-developer"> </a>Invite a developer</span></span>
<span data-ttu-id="b828b-121">tooinvite vývojář, klikněte na tlačítko **uživatelé** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b828b-121">tooinvite a developer, click **Users** from hello **API Management** menu on hello left, and then click **Invite User**.</span></span>

![Pozvěte vývojáře][api-management-invite-developer]

<span data-ttu-id="b828b-123">Zadejte jméno a e-mailovou adresu hello hello vývojáře a klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="b828b-123">Enter hello name and email address of hello developer, and click **Invite**.</span></span>

![Pozvěte vývojáře][api-management-invite-developer-window]

<span data-ttu-id="b828b-125">Zobrazí se zpráva s potvrzením, ale hello nově pozvat vývojáře nejsou uvedené v seznamu hello až po přijetí pozvánky hello.</span><span class="sxs-lookup"><span data-stu-id="b828b-125">A confirmation message is displayed, but hello newly invited developer does not appear in hello list until after they accept hello invitation.</span></span> 

![Pozvěte potvrzení][api-management-invite-developer-confirmation]

<span data-ttu-id="b828b-127">Když je pozvat vývojář, e-mail je odeslán toohello developer.</span><span class="sxs-lookup"><span data-stu-id="b828b-127">When a developer is invited, an email is sent toohello developer.</span></span> <span data-ttu-id="b828b-128">Tento e-mail je generována pomocí šablony a přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="b828b-128">This email is generated using a template and is customizable.</span></span> <span data-ttu-id="b828b-129">Další informace najdete v tématu [konfigurovat e-mailových šablon][Configure email templates].</span><span class="sxs-lookup"><span data-stu-id="b828b-129">For more information, see [Configure email templates][Configure email templates].</span></span>

<span data-ttu-id="b828b-130">Po přijetí pozvání hello se stane aktivní účet hello.</span><span class="sxs-lookup"><span data-stu-id="b828b-130">Once hello invitation is accepted, hello account becomes active.</span></span>

## <span data-ttu-id="b828b-131"><a name="block-developer"></a> , Deaktivujte nebo opětovnou aktivací vývojářského účtu</span><span class="sxs-lookup"><span data-stu-id="b828b-131"><a name="block-developer"> </a> Deactivate or reactivate a developer account</span></span>
<span data-ttu-id="b828b-132">Ve výchozím nastavení, jsou nově vytvořené nebo pozvané vývojářským účtům **Active**.</span><span class="sxs-lookup"><span data-stu-id="b828b-132">By default, newly created or invited developer accounts are **Active**.</span></span> <span data-ttu-id="b828b-133">Klikněte na tlačítko toodeactivate vývojářský účet **bloku**.</span><span class="sxs-lookup"><span data-stu-id="b828b-133">toodeactivate a developer account, click **Block**.</span></span> <span data-ttu-id="b828b-134">Klikněte na tlačítko tooreactivate blokované vývojářského účtu, **aktivovat**.</span><span class="sxs-lookup"><span data-stu-id="b828b-134">tooreactivate a blocked developer account, click **Activate**.</span></span> <span data-ttu-id="b828b-135">Blokované vývojářského účtu nelze přístup k portálu pro vývojáře hello nebo volání všechny rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b828b-135">A blocked developer account can not access hello developer portal or call any APIs.</span></span> <span data-ttu-id="b828b-136">toodelete uživatelský účet, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b828b-136">toodelete a user account, click **Delete**.</span></span>

![Blok vývojáře][api-management-new-developer]

## <a name="reset-a-user-password"></a><span data-ttu-id="b828b-138">Resetování hesla uživatele</span><span class="sxs-lookup"><span data-stu-id="b828b-138">Reset a user password</span></span>
<span data-ttu-id="b828b-139">tooreset hello heslo pro uživatelský účet, klikněte na název hello hello účtu.</span><span class="sxs-lookup"><span data-stu-id="b828b-139">tooreset hello password for a user account, click hello name of hello account.</span></span>

![Resetování hesla][api-management-view-developer]

<span data-ttu-id="b828b-141">Klikněte na tlačítko **resetovat heslo** toosend na odkaz toohello uživatele tooreset své heslo.</span><span class="sxs-lookup"><span data-stu-id="b828b-141">Click **Reset password** toosend a link toohello user tooreset their password.</span></span>

![Resetování hesla][api-management-reset-password]

<span data-ttu-id="b828b-143">tooprogrammatically práci s uživatelskými účty, najdete v části hello [entitu uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentaci v hello [rozhraní API služby REST pro správu](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkaz.</span><span class="sxs-lookup"><span data-stu-id="b828b-143">tooprogrammatically work with user accounts, see hello [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span> <span data-ttu-id="b828b-144">tooreset účet heslo tooa konkrétní hodnotu uživatele, můžete použít hello [aktualizaci uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operaci a zadejte požadované heslo hello.</span><span class="sxs-lookup"><span data-stu-id="b828b-144">tooreset a user account password tooa specific value, you can use hello [Update a user](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operation and specify hello desired password.</span></span>

## <a name="pending-verification"></a><span data-ttu-id="b828b-145">Čeká na ověření</span><span class="sxs-lookup"><span data-stu-id="b828b-145">Pending verification</span></span>
![Čeká na ověření][api-management-pending-verification]

## <span data-ttu-id="b828b-147"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="b828b-147"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="b828b-148">Po vytvoření vývojářského účtu můžete přiřadit k rolím a přihlásit se tooproducts a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b828b-148">Once a developer account is created, you can associate it with roles and subscribe it tooproducts and APIs.</span></span> <span data-ttu-id="b828b-149">Další informace najdete v tématu [jak toocreate a používání skupin][How toocreate and use groups].</span><span class="sxs-lookup"><span data-stu-id="b828b-149">For more information, see [How toocreate and use groups][How toocreate and use groups].</span></span>

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
