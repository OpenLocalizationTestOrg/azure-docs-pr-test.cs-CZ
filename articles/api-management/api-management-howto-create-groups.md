---
title: "aaaManage vývojářským účtům používání skupin v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak účtů toomanage vývojáře pomocí skupin v Azure API Management"
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
ms.openlocfilehash: c46e010e41d9705ae161dcd60d734a76d19c9e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-groups-toomanage-developer-accounts-in-azure-api-management"></a><span data-ttu-id="c69be-103">Jak toocreate a používání skupin toomanage vývojáře účty v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="c69be-103">How toocreate and use groups toomanage developer accounts in Azure API Management</span></span>
<span data-ttu-id="c69be-104">Skupiny ve službě API Management jsou použité toomanage hello viditelnost toodevelopers produkty.</span><span class="sxs-lookup"><span data-stu-id="c69be-104">In API Management, groups are used toomanage hello visibility of products toodevelopers.</span></span> <span data-ttu-id="c69be-105">Produkty jsou první učiněna viditelné toogroups, a pak mohou vývojáři v těchto skupinách zobrazovat a odebírat toohello produkty, které jsou spojeny se skupinami hello.</span><span class="sxs-lookup"><span data-stu-id="c69be-105">Products are first made visible toogroups, and then developers in those groups can view and subscribe toohello products that are associated with hello groups.</span></span> 

<span data-ttu-id="c69be-106">API Management má následující neměnné systémové skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="c69be-106">API Management has hello following immutable system groups.</span></span>

* <span data-ttu-id="c69be-107">**Správci** – členy této skupiny jsou správci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c69be-107">**Administrators** - Azure subscription administrators are members of this group.</span></span> <span data-ttu-id="c69be-108">Správci spravují instance služby API Management, vytváření hello rozhraní API, operace a produkty, které používají vývojáři.</span><span class="sxs-lookup"><span data-stu-id="c69be-108">Administrators manage API Management service instances, creating hello APIs, operations, and products that are used by developers.</span></span>
* <span data-ttu-id="c69be-109">**Vývojáři** – do této skupiny patří ověření uživatelé portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="c69be-109">**Developers** - Authenticated developer portal users fall into this group.</span></span> <span data-ttu-id="c69be-110">Vývojáři jsou zákazníci hello, kteří vytvářejí aplikace pomocí vašich rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c69be-110">Developers are hello customers that build applications using your APIs.</span></span> <span data-ttu-id="c69be-111">Vývojáři mají přístup k portálu pro vývojáře toohello a vytvářet aplikace, které volají operace rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="c69be-111">Developers are granted access toohello developer portal and build applications that call hello operations of an API.</span></span>
* <span data-ttu-id="c69be-112">**Hosté** -neověření uživatelé portálu pro vývojáře, například potenciální zákazníci, kteří navštěvují portál pro vývojáře hello patří instance API Management do této skupiny.</span><span class="sxs-lookup"><span data-stu-id="c69be-112">**Guests** - Unauthenticated developer portal users, such as prospective customers visiting hello developer portal of an API Management instance fall into this group.</span></span> <span data-ttu-id="c69be-113">Je možné udělit určité jen pro čtení přístup, jako je například hello možnost tooview rozhraní API, ale není volat.</span><span class="sxs-lookup"><span data-stu-id="c69be-113">They can be granted certain read-only access, such as hello ability tooview APIs but not call them.</span></span>

<span data-ttu-id="c69be-114">V přidání toothese systémových skupin můžou správci vytvářet vlastní skupiny nebo [využívat externí skupiny v přidružených klientech služby Azure Active Directory][leverage external groups in associated Azure Active Directory tenants].</span><span class="sxs-lookup"><span data-stu-id="c69be-114">In addition toothese system groups, administrators can create custom groups or [leverage external groups in associated Azure Active Directory tenants][leverage external groups in associated Azure Active Directory tenants].</span></span> <span data-ttu-id="c69be-115">Vlastní a externí skupiny můžete používat společně se systémovými skupinami, která poskytuje vývojářům viditelnost a přístup k tooAPI produkty.</span><span class="sxs-lookup"><span data-stu-id="c69be-115">Custom and external groups can be used alongside system groups in giving developers visibility and access tooAPI products.</span></span> <span data-ttu-id="c69be-116">Například může vytvořit jednu vlastní skupinu pro vývojáře spojené s konkrétní partnerské organizaci účty a povolit jim přístup toohello rozhraní API z produktu, který obsahuje jenom příslušná rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c69be-116">For example, you could create one custom group for developers affiliated with a specific partner organization and allow them access toohello APIs from a product containing relevant APIs only.</span></span> <span data-ttu-id="c69be-117">Uživatel může být členem několika skupin.</span><span class="sxs-lookup"><span data-stu-id="c69be-117">A user can be a member of more than one group.</span></span>

<span data-ttu-id="c69be-118">Tato příručka ukazuje, jak přidat nové skupiny a přiřadit je k produktů a vývojáři správci instance API Management.</span><span class="sxs-lookup"><span data-stu-id="c69be-118">This guide shows how administrators of an API Management instance can add new groups and associate them with products and developers.</span></span>

> [!NOTE]
> <span data-ttu-id="c69be-119">Kromě toho toocreating a Správa skupin v hello portálu vydavatele, můžete vytvořit a spravovat vaše skupiny pomocí rozhraní API REST API správy hello [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span><span class="sxs-lookup"><span data-stu-id="c69be-119">In addition toocreating and managing groups in hello publisher portal, you can create and manage your groups using hello API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>
> 
> 

## <span data-ttu-id="c69be-120"><a name="create-group"></a>Vytvořit skupinu</span><span class="sxs-lookup"><span data-stu-id="c69be-120"><a name="create-group"> </a>Create a group</span></span>
<span data-ttu-id="c69be-121">toocreate novou skupinu, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c69be-121">toocreate a new group, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="c69be-122">Tím přejdete portál vydavatele toohello API Management.</span><span class="sxs-lookup"><span data-stu-id="c69be-122">This takes you toohello API Management publisher portal.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="c69be-124">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="c69be-124">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="c69be-125">Klikněte na tlačítko **skupiny** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **přidat skupinu**.</span><span class="sxs-lookup"><span data-stu-id="c69be-125">Click **Groups** from hello **API Management** menu on hello left, and then click **Add Group**.</span></span>

![Přidat nové skupiny][api-management-add-group]

<span data-ttu-id="c69be-127">Zadejte jedinečný název pro skupinu hello a volitelný popis a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c69be-127">Enter a unique name for hello group and an optional description, and click **Save**.</span></span>

![Přidat nové skupiny][api-management-add-group-window]

<span data-ttu-id="c69be-129">Hello nové skupiny se zobrazí v hello skupiny kartě tooedit hello **název** nebo **popis** hello skupiny, klikněte na název hello hello skupiny v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="c69be-129">hello new group is displayed in hello groups tab. tooedit hello **Name** or **Description** of hello group, click hello name of hello group in hello list.</span></span> <span data-ttu-id="c69be-130">toodelete hello skupiny, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c69be-130">toodelete hello group, click **Delete**.</span></span>

![Přidat skupinu][api-management-new-group]

<span data-ttu-id="c69be-132">Teď, když hello skupina vytvořená, může být související s produkty a vývojáři.</span><span class="sxs-lookup"><span data-stu-id="c69be-132">Now that hello group is created, it can be associated with products and developers.</span></span>

## <span data-ttu-id="c69be-133"><a name="associate-group-product"></a>Spojit skupinu s produktem</span><span class="sxs-lookup"><span data-stu-id="c69be-133"><a name="associate-group-product"> </a>Associate a group with a product</span></span>
<span data-ttu-id="c69be-134">Klikněte na tlačítko tooassociate skupinu s produktem, **produkty** z hello **API Management** nabídce hello left a potom klikněte na název hello požadované produktu hello.</span><span class="sxs-lookup"><span data-stu-id="c69be-134">tooassociate a group with a product, click **Products** from hello **API Management** menu on hello left, and then click hello name of hello desired product.</span></span>

![Nastavit viditelnost][api-management-add-group-to-product]

<span data-ttu-id="c69be-136">Vyberte hello **viditelnost** kartě tooadd a odebrat skupiny a tooview hello aktuální skupiny hello produktu.</span><span class="sxs-lookup"><span data-stu-id="c69be-136">Select hello **Visibility** tab tooadd and remove groups, and tooview hello current groups for hello product.</span></span> <span data-ttu-id="c69be-137">tooadd nebo odebrat skupiny, zaškrtněte nebo zrušte zaškrtnutí políček hello pro potřeby hello skupiny a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c69be-137">tooadd or remove groups, check or uncheck hello checkboxes for hello desired groups and click **Save**.</span></span>

![Nastavit viditelnost][api-management-add-group-to-product-visibility]

> [!NOTE]
> <span data-ttu-id="c69be-139">tooadd skupiny Azure Active Directory, najdete v části [jak tooauthorize vývojáře účtů pomocí služby Azure Active Directory v Azure API Management](api-management-howto-aad.md).</span><span class="sxs-lookup"><span data-stu-id="c69be-139">tooadd Azure Active Directory groups, see [How tooauthorize developer accounts using Azure Active Directory in Azure API Management](api-management-howto-aad.md).</span></span>
> 
> <span data-ttu-id="c69be-140">tooconfigure skupiny z hello **viditelnost** pro produkt, klikněte na **spravovat skupiny**.</span><span class="sxs-lookup"><span data-stu-id="c69be-140">tooconfigure groups from hello **Visibility** tab for a product, click **Manage Groups**.</span></span>
> 
> 

<span data-ttu-id="c69be-141">Jakmile produkt je přidružena ke skupině, vývojáři v této skupině můžou zobrazovat a odebírat toohello produktu.</span><span class="sxs-lookup"><span data-stu-id="c69be-141">Once a product is associated with a group, developers in that group can view and subscribe toohello product.</span></span>

## <span data-ttu-id="c69be-142"><a name="associate-group-developer"></a>Přidružení skupin k vývojářům</span><span class="sxs-lookup"><span data-stu-id="c69be-142"><a name="associate-group-developer"> </a>Associate groups with developers</span></span>
<span data-ttu-id="c69be-143">tooassociate skupin s vývojáři, klikněte na **uživatelé** z hello **API Management** nabídky na levé hello a poté hello políčko vedle položky hello vývojáři chcete tooassociate se skupinou.</span><span class="sxs-lookup"><span data-stu-id="c69be-143">tooassociate groups with developers, click **Users** from hello **API Management** menu on hello left, and then check hello box beside hello developers you wish tooassociate with a group.</span></span>

![Přidat toogroup vývojáře][api-management-add-group-to-developer]

<span data-ttu-id="c69be-145">Po hello potřeby, se kontroluje vývojáři, klikněte na požadovanou skupinu hello v hello **přidat tooGroup** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="c69be-145">Once hello desired developers are checked, click hello desired group in hello **Add tooGroup** drop-down.</span></span> <span data-ttu-id="c69be-146">Vývojářům můžete odebrat ze skupiny pomocí hello **odebrat ze skupiny** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="c69be-146">Developers can be removed from groups by using hello **Remove from Group** drop-down.</span></span> 

![Vývojáři][api-management-add-group-to-developer-saved]

<span data-ttu-id="c69be-148">Po přidání přidružení hello mezi hello developer a hello skupiny, můžete ji zobrazit v hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="c69be-148">Once hello association is added between hello developer and hello group, you can view it in hello **Users** tab.</span></span>

## <span data-ttu-id="c69be-149"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="c69be-149"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="c69be-150">Po přidání skupiny tooa vývojář se můžou zobrazovat a odebírat produkty toohello přidružené k této skupině.</span><span class="sxs-lookup"><span data-stu-id="c69be-150">Once a developer is added tooa group, they can view and subscribe toohello products associated with that group.</span></span> <span data-ttu-id="c69be-151">Další informace najdete v tématu [vytvoření a publikování produktu v Azure API Management][How create and publish a product in Azure API Management],</span><span class="sxs-lookup"><span data-stu-id="c69be-151">For more information, see [How create and publish a product in Azure API Management][How create and publish a product in Azure API Management],</span></span>
* <span data-ttu-id="c69be-152">Kromě toho toocreating a Správa skupin v hello portálu vydavatele, můžete vytvořit a spravovat vaše skupiny pomocí rozhraní API REST API správy hello [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span><span class="sxs-lookup"><span data-stu-id="c69be-152">In addition toocreating and managing groups in hello publisher portal, you can create and manage your groups using hello API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>

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
