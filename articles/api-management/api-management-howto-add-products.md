---
title: "Postup vytvoření a publikování produktu v Azure API Management"
description: "Naučte se vytvářet a publikovat produkty ve službě Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 73bf4451ba1b71807e22440beecc73a7e8045c5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a><span data-ttu-id="2884f-103">Postup vytvoření a publikování produktu v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="2884f-103">How to create and publish a product in Azure API Management</span></span>
<span data-ttu-id="2884f-104">Ve službě Azure API Management produkt obsahuje jeden nebo více rozhraní API a také kvóty využití a podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="2884f-104">In Azure API Management, a product contains one or more APIs as well as a usage quota and the terms of use.</span></span> <span data-ttu-id="2884f-105">Po publikování produktu vývojáři můžou přihlásit k produktu a začít používat rozhraní API produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-105">Once a product is published, developers can subscribe to the product and begin to use the product's APIs.</span></span> <span data-ttu-id="2884f-106">Téma poskytuje pokyny pro vytváření produktu, přidání rozhraní API a publikování pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="2884f-106">The topic provides a guide to creating a product, adding an API, and publishing it for developers.</span></span>

## <span data-ttu-id="2884f-107"><a name="create-product"></a>Vytvoření produktu</span><span class="sxs-lookup"><span data-stu-id="2884f-107"><a name="create-product"> </a>Create a product</span></span>
<span data-ttu-id="2884f-108">Operace jsou přidány a nakonfigurovat tak, aby rozhraní API na portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="2884f-108">Operations are added and configured to an API in the publisher portal.</span></span> <span data-ttu-id="2884f-109">Chcete-li získat přístup k portálu vydavatele, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2884f-109">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="2884f-111">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="2884f-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="2884f-112">Klikněte na **produkty** v nabídce na levé straně zobrazíte **produkty** a klikněte na tlačítko **přidat produkt**.</span><span class="sxs-lookup"><span data-stu-id="2884f-112">Click on **Products** in the menu on the left to display the **Products** page, and click **Add Product**.</span></span>

![Produkty][api-management-products]

![Nového produktu][api-management-add-new-product]

<span data-ttu-id="2884f-115">Zadejte popisný název produktu v **název** pole a popis produktu v **popis** pole.</span><span class="sxs-lookup"><span data-stu-id="2884f-115">Enter a descriptive name for the product in the **Name** field and a description of the product in the **Description** field.</span></span>

<span data-ttu-id="2884f-116">Produkty v API Management můžou být **otevřete** nebo **chráněné**.</span><span class="sxs-lookup"><span data-stu-id="2884f-116">Products in API Management can be **Open** or **Protected**.</span></span> <span data-ttu-id="2884f-117">V případě chráněných produktů se musíte nejdřív přihlásit k jejich odběru a až potom je můžete používat. Otevřené produkty můžete používat bez předplatného.</span><span class="sxs-lookup"><span data-stu-id="2884f-117">Protected products must be subscribed to before they can be used, while open products can be used without a subscription.</span></span> <span data-ttu-id="2884f-118">Zkontrolujte **vyžadovat předplatné** vytvořit chráněný produkt, který vyžaduje předplatné.</span><span class="sxs-lookup"><span data-stu-id="2884f-118">Check **Require subscription** to create a protected product that requires a subscription.</span></span> <span data-ttu-id="2884f-119">Toto je výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="2884f-119">This is the default setting.</span></span>

<span data-ttu-id="2884f-120">Zkontrolujte **vyžadovat schválení předplatného** Pokud chcete, aby správci zkontrolovat a následně přijímal nebo odmítal předplatné pokusy o tohoto produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-120">Check **Require subscription approval** if you want an administrator to review and accept or reject subscription attempts to this product.</span></span> <span data-ttu-id="2884f-121">Pokud pole není zaškrtnuto, bude předplatné pokusů o automatické schválení.</span><span class="sxs-lookup"><span data-stu-id="2884f-121">If the box is unchecked, subscription attempts will be auto-approved.</span></span> <span data-ttu-id="2884f-122">Další informace o předplatných najdete v tématu [zobrazení předplatitelů produktu][View subscribers to a product].</span><span class="sxs-lookup"><span data-stu-id="2884f-122">For more information on subscriptions, see [View subscribers to a product][View subscribers to a product].</span></span>

<span data-ttu-id="2884f-123">K přihlášení k vícenásobným odběrům produktu vývojářským účtům povolit, zkontrolujte **povolit více předplatných** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="2884f-123">To allow developer accounts to subscribe multiple times to the product, check the **Allow multiple subscriptions** check box.</span></span> <span data-ttu-id="2884f-124">Pokud toto políčko není zaškrtnuté, každý vývojářský účet se mohou přihlásit jen jednou produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-124">If this box is not checked, each developer account can subscribe only a single time to the product.</span></span>

![Neomezená více předplatných][api-management-unlimited-multiple-subscriptions]

<span data-ttu-id="2884f-126">Chcete-li omezit počet více souběžných předplatných, zkontrolujte **omezit počet souběžných odběrů** zaškrtněte políčko a zadejte limitu předplatného.</span><span class="sxs-lookup"><span data-stu-id="2884f-126">To limit the count of multiple simultaneous subscriptions, check the **Limit number of simultaneous subscriptions to** check box and enter the subscription limit.</span></span> <span data-ttu-id="2884f-127">V následujícím příkladu je omezen na čtyři za vývojářský účet souběžných předplatných.</span><span class="sxs-lookup"><span data-stu-id="2884f-127">In the following example, simultaneous subscriptions are limited to four per developer account.</span></span>

![Čtyři více předplatných][api-management-four-multiple-subscriptions]

<span data-ttu-id="2884f-129">Po nakonfigurování všech možností nového produktu, klikněte na tlačítko **Uložit** k vytvoření nového produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-129">Once all new product options are configured, click **Save** to create the new product.</span></span>

![Produkty][api-management-products-page]

> <span data-ttu-id="2884f-131">Ve výchozím nastavení jsou nové produkty jsou publikování a jsou viditelné pouze pro **správci** skupiny.</span><span class="sxs-lookup"><span data-stu-id="2884f-131">By default new products are unpublished, and are visible only to the  **Administrators** group.</span></span>
> 
> 

<span data-ttu-id="2884f-132">Chcete-li konfigurovat produkt, klikněte na název produktu v **produkty** kartě.</span><span class="sxs-lookup"><span data-stu-id="2884f-132">To configure a product, click on the product name in the **Products** tab.</span></span>

## <span data-ttu-id="2884f-133"><a name="add-apis"></a>Přidat rozhraní API pro určitý produkt</span><span class="sxs-lookup"><span data-stu-id="2884f-133"><a name="add-apis"> </a>Add APIs to a product</span></span>
<span data-ttu-id="2884f-134">**Produkty** stránka obsahuje čtyři odkazy pro konfiguraci: **Souhrn**, **nastavení**, **viditelnost**, a  **Odběratelé, kteří**.</span><span class="sxs-lookup"><span data-stu-id="2884f-134">The **Products** page contains four links for configuration: **Summary**, **Settings**, **Visibility**, and **Subscribers**.</span></span> <span data-ttu-id="2884f-135">**Souhrn** karta je, kde můžete přidat rozhraní API a provést nebo zrušit produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-135">The **Summary** tab is where you can add APIs and publish or unpublish a product.</span></span>

![Souhrn][api-management-new-product-summary]

<span data-ttu-id="2884f-137">Před publikováním produktu je nutné přidat jeden nebo více rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2884f-137">Before publishing your product you need to add one or more APIs.</span></span> <span data-ttu-id="2884f-138">Chcete-li to provést, klikněte na tlačítko **přidat rozhraní API do produktu**.</span><span class="sxs-lookup"><span data-stu-id="2884f-138">To do this, click **Add API to product**.</span></span>

![Přidání rozhraní API][api-management-add-apis-to-product]

<span data-ttu-id="2884f-140">Vyberte požadované rozhraní API a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2884f-140">Select the desired APIs and click **Save**.</span></span>

## <span data-ttu-id="2884f-141"><a name="add-description"></a>Přidat popisné informace pro určitý produkt</span><span class="sxs-lookup"><span data-stu-id="2884f-141"><a name="add-description"> </a>Add descriptive information to a product</span></span>
<span data-ttu-id="2884f-142">**Nastavení** karta umožňuje poskytují podrobné informace o produktu, například jeho účel, poskytuje přístup k rozhraní API a další užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="2884f-142">The **Settings** tab allows you to provide detailed information about the product such as its purpose, the APIs it provides access to, and other useful information.</span></span> <span data-ttu-id="2884f-143">Obsah je cílena na vývojáře, které bude možné volání rozhraní API a může být napsán v jako prostý text ani značka jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="2884f-143">The content is targeted at the developers that will be calling the API and can be written in plain text or HTML markup.</span></span>

![Nastavení produktu][api-management-product-settings]

<span data-ttu-id="2884f-145">Zkontrolujte **vyžadovat předplatné** vytvořit chráněný produkt, který vyžaduje předplatné použít, nebo zrušte zaškrtnutí políčka vytvořit otevřete produktu, kterou lze volat bez předplatného.</span><span class="sxs-lookup"><span data-stu-id="2884f-145">Check **Require subscription** to create a protected product that requires a subscription to be used, or clear the checkbox to create an open product that can be called without a subscription.</span></span>

<span data-ttu-id="2884f-146">Vyberte **vyžadovat schválení předplatného** Pokud chcete ručně schválit všechny požadavky odběru produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-146">Select **Require subscription approval** if you want to manually approve all product subscription requests.</span></span> <span data-ttu-id="2884f-147">Ve výchozím nastavení jsou automaticky udělí všechny odběry produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-147">By default all product subscriptions are granted automatically.</span></span>

<span data-ttu-id="2884f-148">K přihlášení k vícenásobným odběrům produktu vývojářským účtům povolit, zkontrolujte **povolit více předplatných** zaškrtněte políčko a volitelně určete omezení.</span><span class="sxs-lookup"><span data-stu-id="2884f-148">To allow developer accounts to subscribe multiple times to the product, check the **Allow multiple subscriptions** check box and optionally specify a limit.</span></span> <span data-ttu-id="2884f-149">Pokud toto políčko není zaškrtnuté, každý vývojářský účet se mohou přihlásit jen jednou produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-149">If this box is not checked, each developer account can subscribe only a single time to the product.</span></span>

<span data-ttu-id="2884f-150">Volitelně můžete vyplnit **podmínky použití** pole s popisem podmínky použití pro produkt, které odběratele musí přijmout, abyste mohli používat produkt.</span><span class="sxs-lookup"><span data-stu-id="2884f-150">Optionally fill in the **Terms of use** field describing the terms of use for the product which subscribers must accept in order to use the product.</span></span>

## <span data-ttu-id="2884f-151"><a name="publish-product"></a>Publikování produktu</span><span class="sxs-lookup"><span data-stu-id="2884f-151"><a name="publish-product"> </a>Publish a product</span></span>
<span data-ttu-id="2884f-152">Předtím, než je možné volat rozhraní API v produktu, musí být publikován produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-152">Before the APIs in a product can be called, the product must be published.</span></span> <span data-ttu-id="2884f-153">Na **Souhrn** pro produkt, klikněte na **publikovat**a potom klikněte na **Ano, publikovat** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="2884f-153">On the **Summary** tab for the product, click **Publish**, and then click **Yes, publish it** to confirm.</span></span> <span data-ttu-id="2884f-154">Chcete-li změnit publikované dříve produktu soukromé, klikněte na tlačítko **zrušit publikování**.</span><span class="sxs-lookup"><span data-stu-id="2884f-154">To make a previously published product private, click **Unpublish**.</span></span>

![Publikování produktu][api-management-publish-product]

## <span data-ttu-id="2884f-156"><a name="make-visible"></a>Zviditelnit produktu pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="2884f-156"><a name="make-visible"> </a>Make a product visible to developers</span></span>
<span data-ttu-id="2884f-157">**Viditelnost** karta umožňuje vybrat, které role se můžou zobrazit produktu na portál pro vývojáře a přihlásit se k produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-157">The **Visibility** tab allows you to choose which roles are able to see the product on the developer portal and subscribe to the product.</span></span>

![Přehled produktu][api-management-product-visiblity]

<span data-ttu-id="2884f-159">K povolení nebo zakázání viditelnost produktu pro vývojáře ve skupině, zaškrtněte nebo zrušte zaškrtnutí políček vedle skupiny a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2884f-159">To enable or disable visibility of a product for the developers in a group, check or uncheck the check box beside the group and then click **Save**.</span></span>

> <span data-ttu-id="2884f-160">Další informace najdete v tématu [postup vytvoření a používání skupin Správa účtů pro vývojáře ve službě Azure API Management][How to create and use groups to manage developer accounts in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="2884f-160">For more information, see [How to create and use groups to manage developer accounts in Azure API Management][How to create and use groups to manage developer accounts in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="2884f-161"><a name="view-subscribers"></a>Zobrazení předplatitelů produktu</span><span class="sxs-lookup"><span data-stu-id="2884f-161"><a name="view-subscribers"> </a>View subscribers to a product</span></span>
<span data-ttu-id="2884f-162">**Odběratelé, kteří** karta Vypíše seznam vývojáři, kteří se přihlásili k odběru produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-162">The **Subscribers** tab lists the developers who have subscribed to the product.</span></span> <span data-ttu-id="2884f-163">Podrobnosti a nastavení pro každý vývojář lze zobrazit kliknutím na název pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="2884f-163">The details and settings for each developer can be viewed by clicking on the developer's name.</span></span> <span data-ttu-id="2884f-164">V tomto příkladu žádné vývojáři mají ještě přihlásit k odběru produktu.</span><span class="sxs-lookup"><span data-stu-id="2884f-164">In this example no developers have yet subscribed to the product.</span></span>

![Vývojáři][api-management-developer-list]

## <span data-ttu-id="2884f-166"><a name="next-steps"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="2884f-166"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="2884f-167">Po přidání požadované rozhraní API a publikování produktu vývojáři můžou přihlášení k odběru produktu a začít volat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2884f-167">Once the desired APIs are added and the product published, developers can subscribe to the product and begin to call the APIs.</span></span> <span data-ttu-id="2884f-168">Kurz, který ukazuje, tyto položky, jakož i konfigurace pokročilé produktu najdete v části [vytvoření a konfigurace pokročilých nastavení produktu v Azure API Management][How create and configure advanced product settings in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="2884f-168">For a tutorial that demonstrates these items as well as advanced product configuration see [How create and configure advanced product settings in Azure API Management][How create and configure advanced product settings in Azure API Management].</span></span>

<span data-ttu-id="2884f-169">Další informace o práci s produkty najdete v následujícím videu.</span><span class="sxs-lookup"><span data-stu-id="2884f-169">For more information about working with products, see the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[View subscribers to a product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How to create and use groups to manage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
