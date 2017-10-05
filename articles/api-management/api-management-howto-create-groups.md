---
title: "Spravovat účty pro vývojáře používání skupin v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak chcete spravovat účty pro vývojáře používání skupin v Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: b4d71cdfbab535b02542fbb26c7555265e5f9c37
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a><span data-ttu-id="fdbd3-103">Postup vytvoření a používání skupin Správa účtů pro vývojáře ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="fdbd3-103">How to create and use groups to manage developer accounts in Azure API Management</span></span>
<span data-ttu-id="fdbd3-104">Ve službě API Management se ke správě viditelnosti produktů pro vývojáře používají skupiny.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-104">In API Management, groups are used to manage the visibility of products to developers.</span></span> <span data-ttu-id="fdbd3-105">Produkty se první dostupná pro skupiny, a pak mohou vývojáři v těchto skupinách zobrazovat a odebírat produkty, které jsou přidružené skupiny.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-105">Products are first made visible to groups, and then developers in those groups can view and subscribe to the products that are associated with the groups.</span></span> 

<span data-ttu-id="fdbd3-106">Služba API Management má následující neměnné systémové skupiny.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-106">API Management has the following immutable system groups.</span></span>

* <span data-ttu-id="fdbd3-107">**Správci** – členy této skupiny jsou správci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-107">**Administrators** - Azure subscription administrators are members of this group.</span></span> <span data-ttu-id="fdbd3-108">Správci spravují instance služby API Management, vytváření rozhraní API, operace a produkty, které používají vývojáři.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-108">Administrators manage API Management service instances, creating the APIs, operations, and products that are used by developers.</span></span>
* <span data-ttu-id="fdbd3-109">**Vývojáři** – do této skupiny patří ověření uživatelé portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-109">**Developers** - Authenticated developer portal users fall into this group.</span></span> <span data-ttu-id="fdbd3-110">Vývojáři jsou zákazníci, kteří vytvářejí aplikace pomocí vašich rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-110">Developers are the customers that build applications using your APIs.</span></span> <span data-ttu-id="fdbd3-111">Vývojáři mají přístup k portálu pro vývojáře a vytvářejí aplikace, které volají operace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-111">Developers are granted access to the developer portal and build applications that call the operations of an API.</span></span>
* <span data-ttu-id="fdbd3-112">**Hosté** – do této skupiny patří neověření uživatelé portálu pro vývojáře, například potenciální zákazníci, kteří navštěvují portál pro vývojáře v instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-112">**Guests** - Unauthenticated developer portal users, such as prospective customers visiting the developer portal of an API Management instance fall into this group.</span></span> <span data-ttu-id="fdbd3-113">Můžete jim udělit omezený přístup jenom ke čtení, například k zobrazení rozhraní API bez možnosti jeho volání.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-113">They can be granted certain read-only access, such as the ability to view APIs but not call them.</span></span>

<span data-ttu-id="fdbd3-114">Kromě těchto systémových skupin můžou správci vytvářet vlastní skupiny nebo [využívat externí skupiny v přidružených klientech služby Azure Active Directory][leverage external groups in associated Azure Active Directory tenants].</span><span class="sxs-lookup"><span data-stu-id="fdbd3-114">In addition to these system groups, administrators can create custom groups or [leverage external groups in associated Azure Active Directory tenants][leverage external groups in associated Azure Active Directory tenants].</span></span> <span data-ttu-id="fdbd3-115">Vlastní a externí skupiny můžete používat společně se systémovými skupinami, a nastavovat tak vývojářům viditelnost produktů s rozhraním API a přístup k nim.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-115">Custom and external groups can be used alongside system groups in giving developers visibility and access to API products.</span></span> <span data-ttu-id="fdbd3-116">Můžete například vytvořit jednu vlastní skupinu pro vývojáře spojené s konkrétní partnerskou organizací a povolit jim přístup k rozhraním API z produktu, který obsahuje jenom příslušná rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-116">For example, you could create one custom group for developers affiliated with a specific partner organization and allow them access to the APIs from a product containing relevant APIs only.</span></span> <span data-ttu-id="fdbd3-117">Uživatel může být členem několika skupin.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-117">A user can be a member of more than one group.</span></span>

<span data-ttu-id="fdbd3-118">Tato příručka ukazuje, jak přidat nové skupiny a přiřadit je k produktů a vývojáři správci instance API Management.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-118">This guide shows how administrators of an API Management instance can add new groups and associate them with products and developers.</span></span>

> [!NOTE]
> <span data-ttu-id="fdbd3-119">Kromě vytváření a Správa skupin na portálu vydavatele, můžete vytvořit a spravovat vaše skupiny pomocí rozhraní API REST API správy [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-119">In addition to creating and managing groups in the publisher portal, you can create and manage your groups using the API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>
> 
> 

## <span data-ttu-id="fdbd3-120"><a name="create-group"></a>Vytvořit skupinu</span><span class="sxs-lookup"><span data-stu-id="fdbd3-120"><a name="create-group"> </a>Create a group</span></span>
<span data-ttu-id="fdbd3-121">Chcete-li vytvořit novou skupinu, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-121">To create a new group, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="fdbd3-122">Tím přejdete na portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-122">This takes you to the API Management publisher portal.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="fdbd3-124">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="fdbd3-124">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="fdbd3-125">Klikněte na tlačítko **skupiny** z **API Management** nabídky na levé straně a pak klikněte na **přidat skupinu**.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-125">Click **Groups** from the **API Management** menu on the left, and then click **Add Group**.</span></span>

![Přidat nové skupiny][api-management-add-group]

<span data-ttu-id="fdbd3-127">Zadejte jedinečný název pro skupinu a volitelný popis a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-127">Enter a unique name for the group and an optional description, and click **Save**.</span></span>

![Přidat nové skupiny][api-management-add-group-window]

<span data-ttu-id="fdbd3-129">Do nové skupiny se zobrazí na kartě skupiny.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-129">The new group is displayed in the groups tab.</span></span> <span data-ttu-id="fdbd3-130">Chcete-li upravit **název** nebo **popis** skupiny, klikněte na název skupiny v seznamu.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-130">To edit the **Name** or **Description** of the group, click the name of the group in the list.</span></span> <span data-ttu-id="fdbd3-131">Chcete-li odstranit skupinu, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-131">To delete the group, click **Delete**.</span></span>

![Přidat skupinu][api-management-new-group]

<span data-ttu-id="fdbd3-133">Teď, když je skupina vytvořená, může být přidružen produktů a vývojáři.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-133">Now that the group is created, it can be associated with products and developers.</span></span>

## <span data-ttu-id="fdbd3-134"><a name="associate-group-product"></a>Spojit skupinu s produktem</span><span class="sxs-lookup"><span data-stu-id="fdbd3-134"><a name="associate-group-product"> </a>Associate a group with a product</span></span>
<span data-ttu-id="fdbd3-135">Chcete-li přidružit skupinu s produktem, klikněte na tlačítko **produkty** z **API Management** nabídky na levé straně a potom klikněte na název požadovaného produktu.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-135">To associate a group with a product, click **Products** from the **API Management** menu on the left, and then click the name of the desired product.</span></span>

![Nastavit viditelnost][api-management-add-group-to-product]

<span data-ttu-id="fdbd3-137">Vyberte **viditelnost** k přidání a odebrání skupiny, a chcete zobrazit aktuální skupiny pro produkt.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-137">Select the **Visibility** tab to add and remove groups, and to view the current groups for the product.</span></span> <span data-ttu-id="fdbd3-138">Pokud chcete přidat nebo odebrat skupiny, zaškrtněte nebo zrušte zaškrtnutí políček pro požadované skupiny a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-138">To add or remove groups, check or uncheck the checkboxes for the desired groups and click **Save**.</span></span>

![Nastavit viditelnost][api-management-add-group-to-product-visibility]

> [!NOTE]
> <span data-ttu-id="fdbd3-140">Chcete-li přidat skupiny Azure Active Directory, přečtěte si téma [autorizace vývojářským účtům pomocí služby Azure Active Directory v Azure API Management](api-management-howto-aad.md).</span><span class="sxs-lookup"><span data-stu-id="fdbd3-140">To add Azure Active Directory groups, see [How to authorize developer accounts using Azure Active Directory in Azure API Management](api-management-howto-aad.md).</span></span>
> 
> <span data-ttu-id="fdbd3-141">Ke konfiguraci skupin z **viditelnost** pro produkt, klikněte na **spravovat skupiny**.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-141">To configure groups from the **Visibility** tab for a product, click **Manage Groups**.</span></span>
> 
> 

<span data-ttu-id="fdbd3-142">Jakmile produkt je přidružena ke skupině, vývojáři v této skupině můžou zobrazovat a odebírat produktu.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-142">Once a product is associated with a group, developers in that group can view and subscribe to the product.</span></span>

## <span data-ttu-id="fdbd3-143"><a name="associate-group-developer"></a>Přidružení skupin k vývojářům</span><span class="sxs-lookup"><span data-stu-id="fdbd3-143"><a name="associate-group-developer"> </a>Associate groups with developers</span></span>
<span data-ttu-id="fdbd3-144">Přidružení skupin k vývojářům, klikněte na tlačítko **uživatelé** z **API Management** nabídky na levé straně a pak zaškrtněte políčko vedle položky vývojáře, které chcete přiřadit ke skupině.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-144">To associate groups with developers, click **Users** from the **API Management** menu on the left, and then check the box beside the developers you wish to associate with a group.</span></span>

![Přidat vývojáře do skupiny][api-management-add-group-to-developer]

<span data-ttu-id="fdbd3-146">Jakmile jsou zaškrtnutá políčka požadované vývojáři, klikněte na požadovanou skupinu v **přidat do skupiny** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-146">Once the desired developers are checked, click the desired group in the **Add to Group** drop-down.</span></span> <span data-ttu-id="fdbd3-147">Vývojářům můžete odebrat ze skupiny pomocí **odebrat ze skupiny** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-147">Developers can be removed from groups by using the **Remove from Group** drop-down.</span></span> 

![Vývojáři][api-management-add-group-to-developer-saved]

<span data-ttu-id="fdbd3-149">Po přidání přidružení mezi na vývojáře a skupiny můžete zobrazit v **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-149">Once the association is added between the developer and the group, you can view it in the **Users** tab.</span></span>

## <span data-ttu-id="fdbd3-150"><a name="next-steps"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="fdbd3-150"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="fdbd3-151">Jakmile vývojář se přidá do skupiny, se můžou zobrazovat a odebírat produkty, které jsou přidružené k této skupině.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-151">Once a developer is added to a group, they can view and subscribe to the products associated with that group.</span></span> <span data-ttu-id="fdbd3-152">Další informace najdete v tématu [vytvoření a publikování produktu v Azure API Management][How create and publish a product in Azure API Management],</span><span class="sxs-lookup"><span data-stu-id="fdbd3-152">For more information, see [How create and publish a product in Azure API Management][How create and publish a product in Azure API Management],</span></span>
* <span data-ttu-id="fdbd3-153">Kromě vytváření a Správa skupin na portálu vydavatele, můžete vytvořit a spravovat vaše skupiny pomocí rozhraní API REST API správy [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span><span class="sxs-lookup"><span data-stu-id="fdbd3-153">In addition to creating and managing groups in the publisher portal, you can create and manage your groups using the API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
