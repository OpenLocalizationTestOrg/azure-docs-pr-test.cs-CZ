---
title: "Jak spravovat uživatelské účty v Azure API Management | Microsoft Docs"
description: "Naučte se vytvořit nebo pozvat uživatele ve službě Azure API Management"
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
ms.openlocfilehash: d3a50f6d22cbf1797f580078bc0d2cc9cefe5064
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a><span data-ttu-id="e8d5d-103">Správa uživatelských účtů ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="e8d5d-103">How to manage user accounts in Azure API Management</span></span>
<span data-ttu-id="e8d5d-104">Vývojáři ve službě API Management, jsou uživatelé rozhraní API, která vystavit použití služby API Management.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-104">In API Management, developers are the users of the APIs that you expose using API Management.</span></span> <span data-ttu-id="e8d5d-105">Tato příručka obsahuje k vytváření a zvaní vývojářů používat rozhraní API a produkty, abyste vytvořili pro ně k dispozici s vaší instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-105">This guide shows to how to create and invite developers to use the APIs and products that you make available to them with your API Management instance.</span></span> <span data-ttu-id="e8d5d-106">Informace o správě uživatelských účtů prostřednictvím kódu programu, najdete v článku [entitu uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentaci v [rozhraní API služby REST pro správu](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkaz.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-106">For information on managing user accounts programmatically, see the [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in the [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span>

## <span data-ttu-id="e8d5d-107"><a name="create-developer"></a>Vytvořit nové vývojáře</span><span class="sxs-lookup"><span data-stu-id="e8d5d-107"><a name="create-developer"> </a>Create a new developer</span></span>
<span data-ttu-id="e8d5d-108">Chcete-li vytvořit nové vývojáře, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-108">To create a new developer, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="e8d5d-109">Tím přejdete na portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-109">This takes you to the API Management publisher portal.</span></span> <span data-ttu-id="e8d5d-110">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="e8d5d-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Portál vydavatele][api-management-management-console]

<span data-ttu-id="e8d5d-112">Klikněte na tlačítko **uživatelé** z **API Management** nabídky na levé straně a pak klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-112">Click **Users** from the **API Management** menu on the left, and then click **add user**.</span></span>

![Vytvoření vývojáře][api-management-create-developer]

<span data-ttu-id="e8d5d-114">Zadejte **e-mailu**, **heslo**, a **název** pro nové vývojáře a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-114">Enter the **Email**, **Password**, and **Name** for the new developer and click **Save**.</span></span>

![Vytvoření vývojáře][api-management-add-new-user]

<span data-ttu-id="e8d5d-116">Ve výchozím nastavení, jsou nově vytvořený vývojářským účtům **Active**a přidružené **vývojáři** skupiny.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-116">By default, newly created developer accounts are **Active**, and associated with the **Developers** group.</span></span>

![Nové vývojáře][api-management-new-developer]

<span data-ttu-id="e8d5d-118">Vývojářským účtům, které jsou v **active** stavu lze použít pro přístup k veškerému rozhraní API, k němuž mají odběry.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-118">Developer accounts that are in an **active** state can be used to access all of the APIs for which they have subscriptions.</span></span> <span data-ttu-id="e8d5d-119">Nově vytvořený vývojáře přidružit další skupiny, najdete v tématu [postup přidružení skupin k vývojářům][How to associate groups with developers].</span><span class="sxs-lookup"><span data-stu-id="e8d5d-119">To associate the newly created developer with additional groups, see [How to associate groups with developers][How to associate groups with developers].</span></span>

## <span data-ttu-id="e8d5d-120"><a name="invite-developer"></a>Pozvat vývojář</span><span class="sxs-lookup"><span data-stu-id="e8d5d-120"><a name="invite-developer"> </a>Invite a developer</span></span>
<span data-ttu-id="e8d5d-121">Pozvaným vývojář, klikněte na tlačítko **uživatelé** z **API Management** nabídky na levé straně a pak klikněte na **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-121">To invite a developer, click **Users** from the **API Management** menu on the left, and then click **Invite User**.</span></span>

![Pozvěte vývojáře][api-management-invite-developer]

<span data-ttu-id="e8d5d-123">Zadejte jméno a e-mailovou adresu od vývojářů a klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-123">Enter the name and email address of the developer, and click **Invite**.</span></span>

![Pozvěte vývojáře][api-management-invite-developer-window]

<span data-ttu-id="e8d5d-125">Zobrazí se zpráva s potvrzením, ale nově pozvané vývojáře nejsou uvedené v seznamu až po jejich přijetí pozvánky.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-125">A confirmation message is displayed, but the newly invited developer does not appear in the list until after they accept the invitation.</span></span> 

![Pozvěte potvrzení][api-management-invite-developer-confirmation]

<span data-ttu-id="e8d5d-127">Když je pozvat vývojář, e-mail je odeslán pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-127">When a developer is invited, an email is sent to the developer.</span></span> <span data-ttu-id="e8d5d-128">Tento e-mail je generována pomocí šablony a přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-128">This email is generated using a template and is customizable.</span></span> <span data-ttu-id="e8d5d-129">Další informace najdete v tématu [konfigurovat e-mailových šablon][Configure email templates].</span><span class="sxs-lookup"><span data-stu-id="e8d5d-129">For more information, see [Configure email templates][Configure email templates].</span></span>

<span data-ttu-id="e8d5d-130">Po přijetí pozvání se stane aktivní účet.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-130">Once the invitation is accepted, the account becomes active.</span></span>

## <span data-ttu-id="e8d5d-131"><a name="block-developer"></a> , Deaktivujte nebo opětovnou aktivací vývojářského účtu</span><span class="sxs-lookup"><span data-stu-id="e8d5d-131"><a name="block-developer"> </a> Deactivate or reactivate a developer account</span></span>
<span data-ttu-id="e8d5d-132">Ve výchozím nastavení, jsou nově vytvořené nebo pozvané vývojářským účtům **Active**.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-132">By default, newly created or invited developer accounts are **Active**.</span></span> <span data-ttu-id="e8d5d-133">Chcete-li deaktivovat účet pro vývojáře, klikněte na tlačítko **bloku**.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-133">To deactivate a developer account, click **Block**.</span></span> <span data-ttu-id="e8d5d-134">Chcete-li znovu aktivovat účet zablokovaný vývojáře, klikněte na tlačítko **aktivovat**.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-134">To reactivate a blocked developer account, click **Activate**.</span></span> <span data-ttu-id="e8d5d-135">Blokované vývojářského účtu nelze přístup k portálu pro vývojáře nebo volání všechny rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-135">A blocked developer account can not access the developer portal or call any APIs.</span></span> <span data-ttu-id="e8d5d-136">Chcete-li odstranit uživatelský účet, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-136">To delete a user account, click **Delete**.</span></span>

![Blok vývojáře][api-management-new-developer]

## <a name="reset-a-user-password"></a><span data-ttu-id="e8d5d-138">Resetování hesla uživatele</span><span class="sxs-lookup"><span data-stu-id="e8d5d-138">Reset a user password</span></span>
<span data-ttu-id="e8d5d-139">Pokud chcete resetovat heslo pro uživatelský účet, klikněte na název účtu.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-139">To reset the password for a user account, click the name of the account.</span></span>

![Resetování hesla][api-management-view-developer]

<span data-ttu-id="e8d5d-141">Klikněte na tlačítko **resetovat heslo** poslat odkaz uživateli, aby obnovit své heslo.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-141">Click **Reset password** to send a link to the user to reset their password.</span></span>

![Resetování hesla][api-management-reset-password]

<span data-ttu-id="e8d5d-143">Prostřednictvím kódu programu pracovat s uživatelskými účty, najdete v článku [entitu uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentaci v [rozhraní API služby REST pro správu](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkaz.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-143">To programmatically work with user accounts, see the [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in the [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span> <span data-ttu-id="e8d5d-144">Pokud chcete resetovat heslo uživatelského účtu na určitou hodnotu, můžete použít [aktualizaci uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operaci a zadejte požadované heslo.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-144">To reset a user account password to a specific value, you can use the [Update a user](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operation and specify the desired password.</span></span>

## <a name="pending-verification"></a><span data-ttu-id="e8d5d-145">Čeká na ověření</span><span class="sxs-lookup"><span data-stu-id="e8d5d-145">Pending verification</span></span>
![Čeká na ověření][api-management-pending-verification]

## <span data-ttu-id="e8d5d-147"><a name="next-steps"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="e8d5d-147"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="e8d5d-148">Jakmile se vytvoří účet pro vývojáře, můžete přiřadit k rolím a přihlášení k odběru produktů a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e8d5d-148">Once a developer account is created, you can associate it with roles and subscribe it to products and APIs.</span></span> <span data-ttu-id="e8d5d-149">Další informace najdete v tématu [postup vytvoření a používání skupin][How to create and use groups].</span><span class="sxs-lookup"><span data-stu-id="e8d5d-149">For more information, see [How to create and use groups][How to create and use groups].</span></span>

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
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
