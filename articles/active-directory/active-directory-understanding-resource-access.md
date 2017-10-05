---
title: "Principy přístupu k prostředkům v Azure | Microsoft Docs"
description: "Toto téma vysvětluje koncepty o používání správci předplatného k řízení přístupu k prostředkům v plné verzi portálu Azure"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 174f1706-b959-4230-9a75-bf651227ebf6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: f1fda3c4192d0dae4fa60788f4d88fb72ddba4ad
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="understanding-resource-access-in-azure"></a><span data-ttu-id="c5097-103">Principy přístupu k prostředkům v Azure</span><span class="sxs-lookup"><span data-stu-id="c5097-103">Understanding resource access in Azure</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c5097-104">Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.</span><span class="sxs-lookup"><span data-stu-id="c5097-104">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="c5097-105">Portál Azure poskytuje [řízení přístupu na základě role](role-based-access-control-configure.md) , přesněji můžete spravovat prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="c5097-105">The Azure portal provides [role-based access control](role-based-access-control-configure.md) so Azure resources can be managed more precisely.</span></span>
> 
> 

<span data-ttu-id="c5097-106">V říjnu 2013 se službou Azure Active Directory integrované klasický portál Azure a rozhraní API pro správu služby, aby bylo možné jen pro zlepšení uživatelské prostředí pro správu přístupu k prostředkům Azure.</span><span class="sxs-lookup"><span data-stu-id="c5097-106">In October 2013, the Azure classic portal and Service Management APIs were integrated with Azure Active Directory in order to lay the groundwork for improving the user experience for managing access to Azure resources.</span></span> <span data-ttu-id="c5097-107">Azure Active Directory již poskytuje skvělé funkcí, jako je Správa uživatelů, synchronizace adresářů na místě, vícefaktorového ověřování a řízení přístupu aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5097-107">Azure Active Directory already provides great capabilities such as user management, on-premises directory sync, multi-factor authentication, and application access control.</span></span> <span data-ttu-id="c5097-108">Samozřejmě to má také být k dispozici pro správu prostředků Azure plošným.</span><span class="sxs-lookup"><span data-stu-id="c5097-108">Naturally, these should also be made available for managing Azure resources across-the-board.</span></span>

<span data-ttu-id="c5097-109">Spustí se z hlediska fakturační řízení přístupu v Azure.</span><span class="sxs-lookup"><span data-stu-id="c5097-109">Access control in Azure starts from a billing perspective.</span></span> <span data-ttu-id="c5097-110">Vlastník účtu Azure, navštivte stránky přístup [centra účtů Azure](https://account.windowsazure.com/subscriptions), je správce účtu (AA).</span><span class="sxs-lookup"><span data-stu-id="c5097-110">The owner of an Azure account, accessed by visiting the  [Azure Accounts Center](https://account.windowsazure.com/subscriptions), is the Account Administrator (AA).</span></span> <span data-ttu-id="c5097-111">Předplatné je kontejner pro fakturaci, ale také slouží jako hranice zabezpečení: každého předplatného se služby správce (SA), který můžete přidat, odebrat a úpravy prostředků v tomto předplatném Azure pomocí [portál Azure classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="c5097-111">Subscriptions are a container for billing, but they also act as a security boundary: each subscription has a Service Administrator (SA) who can add, remove, and modify Azure resources in that subscription by using the [Azure classic portal](https://manage.windowsazure.com/).</span></span> <span data-ttu-id="c5097-112">Výchozí SA nového předplatného je AA, ale AA změnit přidružení zabezpečení v centru účtů Azure.</span><span class="sxs-lookup"><span data-stu-id="c5097-112">The default SA of a new subscription is the AA, but the AA can change the SA in the Azure Accounts Center.</span></span>

<br><br>![Účty Azure][1]

<span data-ttu-id="c5097-114">Odběry mají také přidružení s adresářem.</span><span class="sxs-lookup"><span data-stu-id="c5097-114">Subscriptions also have an association with a directory.</span></span> <span data-ttu-id="c5097-115">Adresář definuje sadu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c5097-115">The directory defines a set of users.</span></span> <span data-ttu-id="c5097-116">Může jít o uživatele z práce nebo škola vytvořený adresář nebo může se jednat o externí uživatele (to znamená, Accounts Microsoft).</span><span class="sxs-lookup"><span data-stu-id="c5097-116">These can be users from the work or school that created the directory or they can be external users (that is, Microsoft Accounts).</span></span> <span data-ttu-id="c5097-117">Odběry jsou přístupné pro podmnožinu těchto directory uživatelů, kteří mají přiřazený jako služba správce nebo Spolusprávce (CA); Jedinou výjimkou je, že starší verze důvodů Accounts Microsoft (dříve Windows Live ID) může být přiřazen jako SA nebo certifikační Autority bez se nachází v adresáři.</span><span class="sxs-lookup"><span data-stu-id="c5097-117">Subscriptions are accessible by a subset of those directory users who have been assigned as either Service Administrator (SA) or Co-Administrator (CA); the only exception is that, for legacy reasons, Microsoft Accounts (formerly Windows Live ID) can be assigned as SA or CA without being present in the directory.</span></span>

<br><br>![Řízení přístupu v Azure][2]

<span data-ttu-id="c5097-119">Funkcionalitu v rámci portálu Azure classic umožňuje SAs, které jsou podepsané pomocí Account Microsoft Chcete-li změnit adresář, který je přidružen odběru pomocí **upravit adresář** příkaz na **odběry** stránky v **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c5097-119">Functionality within the Azure classic portal enables SAs that are signed in using a Microsoft Account to change the directory that a subscription is associated with by using the **Edit Directory** command on the **Subscriptions** page in **Settings**.</span></span> <span data-ttu-id="c5097-120">Všimněte si, že tato operace má dopad na řízení přístupu daného předplatného.</span><span class="sxs-lookup"><span data-stu-id="c5097-120">Note that this operation has implications on the access control of that subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="c5097-121">**Upravit adresář** příkaz na portálu Azure classic není k dispozici pro uživatele, kteří jsou přihlášení pomocí pracovního nebo školního účtu, protože tyto účty se můžete přihlásit pouze k adresáři, do které patří.</span><span class="sxs-lookup"><span data-stu-id="c5097-121">The **Edit Directory** command in the Azure classic portal is not available to users who are signed in using a work or school account because those accounts can sign in only to the directory to which they belong.</span></span>
> 
> 

<br><br>![Tok přihlášení jednoduchá uživatele][3]

<span data-ttu-id="c5097-123">V případě jednoduchého bude organizace (například Contoso) vynutit fakturace a řízení přístupu v stejnou sadu odběry.</span><span class="sxs-lookup"><span data-stu-id="c5097-123">In the simple case, an organization (such as Contoso) will enforce billing and access control across the same set of subscriptions.</span></span> <span data-ttu-id="c5097-124">Adresář je přidružen k odběry, které jsou vlastněny jeden účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c5097-124">That is, the directory is associated to subscriptions that are owned by a single Azure Account.</span></span> <span data-ttu-id="c5097-125">Po úspěšném přihlášení k portálu Azure classic se uživatelům zobrazí dvě kolekce prostředků (znázorněný v oranžová na předchozím obrázku):</span><span class="sxs-lookup"><span data-stu-id="c5097-125">Upon successful login to the Azure classic portal, users see two collections of resources (depicted in orange in the previous illustration):</span></span>

* <span data-ttu-id="c5097-126">Adresáře, kde existuje uživatelského účtu (jako zdroj nebo přidat jako cizí objekt zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="c5097-126">Directories where their user account exists (sourced or added as a foreign principal).</span></span> <span data-ttu-id="c5097-127">Všimněte si, že adresáři použít pro přihlášení není relevantní pro tento výpočet tak adresářů se vždy zobrazí bez ohledu na to, kde jste se přihlásili.</span><span class="sxs-lookup"><span data-stu-id="c5097-127">Note that the directory used for login isn’t relevant to this computation, so your directories will always be shown regardless of where you logged in.</span></span>
* <span data-ttu-id="c5097-128">Prostředky, které jsou součástí odběry, které jsou přidruženy k adresáři použít pro přihlášení a který má uživatel přístup (kde se jedná SA nebo certifikační Autority).</span><span class="sxs-lookup"><span data-stu-id="c5097-128">Resources that are part of subscriptions that are associated with the directory used for login AND which the user can access (where they are an SA or CA).</span></span>

<br><br>![Uživatel s více předplatnými a adresáři][4]

<span data-ttu-id="c5097-130">Možnost přepnout aktuální kontext portálu Azure classic pomocí filtru předplatné mít uživatelé s odběry v několika adresářích.</span><span class="sxs-lookup"><span data-stu-id="c5097-130">Users with subscriptions across multiple directories have the ability to switch the current context of the Azure classic portal by using the subscription filter.</span></span> <span data-ttu-id="c5097-131">V pozadí výsledkem je samostatný přihlášení do jiného adresáře, ale to se provádí bezproblémově pomocí jednotného přihlašování (SSO).</span><span class="sxs-lookup"><span data-stu-id="c5097-131">Under the covers, this results in a separate login to a different directory, but this is accomplished seamlessly using single sign-on (SSO).</span></span>

<span data-ttu-id="c5097-132">Operace, například přesun prostředků mezi předplatnými může být obtížnější v důsledku toto zobrazení jednoho adresářového předplatných.</span><span class="sxs-lookup"><span data-stu-id="c5097-132">Operations such as moving resources between subscriptions can be more difficult as a result of this single directory view of subscriptions.</span></span> <span data-ttu-id="c5097-133">Pokud chcete provést přenos prostředků, může být potřeba první použití **upravit adresář** příkaz na stránce předplatných v **nastavení** přidružení předplatných do stejného adresáře.</span><span class="sxs-lookup"><span data-stu-id="c5097-133">To perform the resource transfer, it may be necessary to first use the **Edit Directory** command on the Subscriptions page in **Settings** to associate the subscriptions to the same directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5097-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c5097-134">Next Steps</span></span>
* <span data-ttu-id="c5097-135">Další informace o tom, jak změnit správce pro předplatné služby Azure naleznete v tématu [Postup přidání nebo změna role správce služby Azure](../billing/billing-add-change-azure-subscription-administrator.md)</span><span class="sxs-lookup"><span data-stu-id="c5097-135">To learn more about how to change administrators for an Azure subscription, see [How to add or change Azure administrator roles](../billing/billing-add-change-azure-subscription-administrator.md)</span></span>
* <span data-ttu-id="c5097-136">Další informace o tom, jak Azure Active Directory má vztah k předplatnému Azure, najdete v části [asociování předplatných Azure se službou Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)</span><span class="sxs-lookup"><span data-stu-id="c5097-136">For more information on how Azure Active Directory relates to your Azure subscription, see [How Azure subscriptions are associated with Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)</span></span>
* <span data-ttu-id="c5097-137">Další informace o tom, jak přiřadit role ve službě Azure AD, najdete v tématu [Přiřazení rolí správce ve službě Azure Active Directory](active-directory-assign-admin-roles.md)</span><span class="sxs-lookup"><span data-stu-id="c5097-137">For more information on how to assign roles in Azure AD, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md)</span></span>

<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png
