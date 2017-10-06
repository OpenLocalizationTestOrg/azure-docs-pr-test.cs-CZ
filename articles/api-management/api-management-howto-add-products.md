---
title: "aaaHow toocreate a publikování produktu v Azure API Management"
description: "Zjistěte, jak toocreate a publikovat produkty ve službě Azure API Management."
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
ms.openlocfilehash: f0a37f08b4e29ca68be9caec4c7604e3b4b6aaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-publish-a-product-in-azure-api-management"></a><span data-ttu-id="f556d-103">Jak toocreate a publikování produktu v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="f556d-103">How toocreate and publish a product in Azure API Management</span></span>
<span data-ttu-id="f556d-104">Ve službě Azure API Management produkt obsahuje jeden nebo více rozhraní API a také využití kvóty a hello podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="f556d-104">In Azure API Management, a product contains one or more APIs as well as a usage quota and hello terms of use.</span></span> <span data-ttu-id="f556d-105">Po publikování produktu vývojáři můžou přihlášení k odběru produktu toohello a začít toouse hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f556d-105">Once a product is published, developers can subscribe toohello product and begin toouse hello product's APIs.</span></span> <span data-ttu-id="f556d-106">Hello téma obsahuje Průvodce toocreating produkt, přidání rozhraní API a publikování pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="f556d-106">hello topic provides a guide toocreating a product, adding an API, and publishing it for developers.</span></span>

## <span data-ttu-id="f556d-107"><a name="create-product"></a>Vytvoření produktu</span><span class="sxs-lookup"><span data-stu-id="f556d-107"><a name="create-product"> </a>Create a product</span></span>
<span data-ttu-id="f556d-108">Operace se přidají a nakonfigurovat tooan rozhraní API portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="f556d-108">Operations are added and configured tooan API in hello publisher portal.</span></span> <span data-ttu-id="f556d-109">Klikněte na portálu vydavatele hello tooaccess **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="f556d-109">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="f556d-111">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="f556d-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="f556d-112">Klikněte na **produkty** v hello nabídky na levém toodisplay hello hello **produkty** a klikněte na tlačítko **přidat produkt**.</span><span class="sxs-lookup"><span data-stu-id="f556d-112">Click on **Products** in hello menu on hello left toodisplay hello **Products** page, and click **Add Product**.</span></span>

![Produkty][api-management-products]

![Nového produktu][api-management-add-new-product]

<span data-ttu-id="f556d-115">Zadejte popisný název produktu hello hello **název** pole a popis hello produktu v hello **popis** pole.</span><span class="sxs-lookup"><span data-stu-id="f556d-115">Enter a descriptive name for hello product in hello **Name** field and a description of hello product in hello **Description** field.</span></span>

<span data-ttu-id="f556d-116">Produkty v API Management můžou být **otevřete** nebo **chráněné**.</span><span class="sxs-lookup"><span data-stu-id="f556d-116">Products in API Management can be **Open** or **Protected**.</span></span> <span data-ttu-id="f556d-117">Chráněných produktů musí být odebírané toobefore použitím, otevřené produkty můžete používat bez předplatného.</span><span class="sxs-lookup"><span data-stu-id="f556d-117">Protected products must be subscribed toobefore they can be used, while open products can be used without a subscription.</span></span> <span data-ttu-id="f556d-118">Zkontrolujte **vyžadovat předplatné** toocreate chráněný produkt, který vyžaduje předplatné.</span><span class="sxs-lookup"><span data-stu-id="f556d-118">Check **Require subscription** toocreate a protected product that requires a subscription.</span></span> <span data-ttu-id="f556d-119">Toto je výchozí nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="f556d-119">This is hello default setting.</span></span>

<span data-ttu-id="f556d-120">Zkontrolujte **vyžadovat schválení předplatného** Pokud chcete tooreview správce a přijměte nebo odmítněte předplatné pokusí toothis produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-120">Check **Require subscription approval** if you want an administrator tooreview and accept or reject subscription attempts toothis product.</span></span> <span data-ttu-id="f556d-121">Pokud není zaškrtnuto políčko hello, pokusy o předplatné bude schvalovat automaticky.</span><span class="sxs-lookup"><span data-stu-id="f556d-121">If hello box is unchecked, subscription attempts will be auto-approved.</span></span> <span data-ttu-id="f556d-122">Další informace o předplatných najdete v tématu [zobrazení odběratele tooa produktu][View subscribers tooa product].</span><span class="sxs-lookup"><span data-stu-id="f556d-122">For more information on subscriptions, see [View subscribers tooa product][View subscribers tooa product].</span></span>

<span data-ttu-id="f556d-123">tooallow vývojáře účty toosubscribe produkt toohello vícekrát, zkontrolujte hello **povolit více předplatných** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="f556d-123">tooallow developer accounts toosubscribe multiple times toohello product, check hello **Allow multiple subscriptions** check box.</span></span> <span data-ttu-id="f556d-124">Pokud toto políčko není zaškrtnuté, každý vývojářský účet se mohou přihlásit pouze jednou toohello produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-124">If this box is not checked, each developer account can subscribe only a single time toohello product.</span></span>

![Neomezená více předplatných][api-management-unlimited-multiple-subscriptions]

<span data-ttu-id="f556d-126">toolimit hello počet více souběžných předplatných, zkontrolujte hello **omezit počet souběžných odběrů** zaškrtněte políčko a zadejte limitu předplatného pro hello.</span><span class="sxs-lookup"><span data-stu-id="f556d-126">toolimit hello count of multiple simultaneous subscriptions, check hello **Limit number of simultaneous subscriptions to** check box and enter hello subscription limit.</span></span> <span data-ttu-id="f556d-127">V následujícím příkladu hello souběžných předplatných jsou omezené toofour za vývojářský účet.</span><span class="sxs-lookup"><span data-stu-id="f556d-127">In hello following example, simultaneous subscriptions are limited toofour per developer account.</span></span>

![Čtyři více předplatných][api-management-four-multiple-subscriptions]

<span data-ttu-id="f556d-129">Po nakonfigurování všech možností nového produktu, klikněte na tlačítko **Uložit** toocreate hello nového produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-129">Once all new product options are configured, click **Save** toocreate hello new product.</span></span>

![Produkty][api-management-products-page]

> <span data-ttu-id="f556d-131">Ve výchozím nastavení jsou nové produkty jsou publikování a jsou viditelné pouze toohello **správci** skupiny.</span><span class="sxs-lookup"><span data-stu-id="f556d-131">By default new products are unpublished, and are visible only toohello  **Administrators** group.</span></span>
> 
> 

<span data-ttu-id="f556d-132">tooconfigure produkt, klikněte na název produktu hello v hello **produkty** kartě.</span><span class="sxs-lookup"><span data-stu-id="f556d-132">tooconfigure a product, click on hello product name in hello **Products** tab.</span></span>

## <span data-ttu-id="f556d-133"><a name="add-apis"></a>Přidat rozhraní API tooa produktu</span><span class="sxs-lookup"><span data-stu-id="f556d-133"><a name="add-apis"> </a>Add APIs tooa product</span></span>
<span data-ttu-id="f556d-134">Hello **produkty** stránka obsahuje čtyři odkazy pro konfiguraci: **Souhrn**, **nastavení**, **viditelnost**, a  **Odběratelé, kteří**.</span><span class="sxs-lookup"><span data-stu-id="f556d-134">hello **Products** page contains four links for configuration: **Summary**, **Settings**, **Visibility**, and **Subscribers**.</span></span> <span data-ttu-id="f556d-135">Hello **Souhrn** karta je, kde můžete přidat rozhraní API a provést nebo zrušit produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-135">hello **Summary** tab is where you can add APIs and publish or unpublish a product.</span></span>

![Souhrn][api-management-new-product-summary]

<span data-ttu-id="f556d-137">Před publikováním svůj produkt musíte tooadd jeden nebo více rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f556d-137">Before publishing your product you need tooadd one or more APIs.</span></span> <span data-ttu-id="f556d-138">toodo tento, klikněte na tlačítko **přidat rozhraní API tooproduct**.</span><span class="sxs-lookup"><span data-stu-id="f556d-138">toodo this, click **Add API tooproduct**.</span></span>

![Přidání rozhraní API][api-management-add-apis-to-product]

<span data-ttu-id="f556d-140">Vyberte hello potřeby rozhraní API a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f556d-140">Select hello desired APIs and click **Save**.</span></span>

## <span data-ttu-id="f556d-141"><a name="add-description"></a>Přidání popisné informace tooa produktu</span><span class="sxs-lookup"><span data-stu-id="f556d-141"><a name="add-description"> </a>Add descriptive information tooa product</span></span>
<span data-ttu-id="f556d-142">Hello **nastavení** karta vám umožní tooprovide podrobné informace o produktu hello například svůj účel, hello poskytuje přístup k rozhraní API a další užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="f556d-142">hello **Settings** tab allows you tooprovide detailed information about hello product such as its purpose, hello APIs it provides access to, and other useful information.</span></span> <span data-ttu-id="f556d-143">obsah Hello je cílena na vývojáře hello, které bude volání metody hello rozhraní API a může být napsán v jako prostý text ani značka jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="f556d-143">hello content is targeted at hello developers that will be calling hello API and can be written in plain text or HTML markup.</span></span>

![Nastavení produktu][api-management-product-settings]

<span data-ttu-id="f556d-145">Zkontrolujte **vyžadovat předplatné** toocreate hello chráněný produkt, který vyžaduje předplatné toobe používá, nebo zrušte zaškrtnutí políčka toocreate otevřete produktu, kterou lze volat bez předplatného.</span><span class="sxs-lookup"><span data-stu-id="f556d-145">Check **Require subscription** toocreate a protected product that requires a subscription toobe used, or clear hello checkbox toocreate an open product that can be called without a subscription.</span></span>

<span data-ttu-id="f556d-146">Vyberte **vyžadovat schválení předplatného** Chcete-li toomanually schválení všech žádostí o odběr produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-146">Select **Require subscription approval** if you want toomanually approve all product subscription requests.</span></span> <span data-ttu-id="f556d-147">Ve výchozím nastavení jsou automaticky udělí všechny odběry produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-147">By default all product subscriptions are granted automatically.</span></span>

<span data-ttu-id="f556d-148">tooallow vývojáře účty toosubscribe produkt toohello vícekrát, zkontrolujte hello **povolit více předplatných** zaškrtněte políčko a volitelně určete omezení.</span><span class="sxs-lookup"><span data-stu-id="f556d-148">tooallow developer accounts toosubscribe multiple times toohello product, check hello **Allow multiple subscriptions** check box and optionally specify a limit.</span></span> <span data-ttu-id="f556d-149">Pokud toto políčko není zaškrtnuté, každý vývojářský účet se mohou přihlásit pouze jednou toohello produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-149">If this box is not checked, each developer account can subscribe only a single time toohello product.</span></span>

<span data-ttu-id="f556d-150">Volitelně můžete vyplnit hello **podmínky použití** pole s popisem hello podmínky použití hello produktu, kterou odběratele musí přijmout v pořadí toouse hello produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-150">Optionally fill in hello **Terms of use** field describing hello terms of use for hello product which subscribers must accept in order toouse hello product.</span></span>

## <span data-ttu-id="f556d-151"><a name="publish-product"></a>Publikování produktu</span><span class="sxs-lookup"><span data-stu-id="f556d-151"><a name="publish-product"> </a>Publish a product</span></span>
<span data-ttu-id="f556d-152">Předtím, než je možné volat hello rozhraní API v produktu, musí být publikován hello produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-152">Before hello APIs in a product can be called, hello product must be published.</span></span> <span data-ttu-id="f556d-153">Na hello **Souhrn** hello produktu, klikněte na **publikovat**a potom klikněte na **Ano, publikovat** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="f556d-153">On hello **Summary** tab for hello product, click **Publish**, and then click **Yes, publish it** tooconfirm.</span></span> <span data-ttu-id="f556d-154">Klikněte na tlačítko toomake privátního publikované dříve produktu, **zrušit publikování**.</span><span class="sxs-lookup"><span data-stu-id="f556d-154">toomake a previously published product private, click **Unpublish**.</span></span>

![Publikování produktu][api-management-publish-product]

## <span data-ttu-id="f556d-156"><a name="make-visible"></a>Zkontrolujte viditelné toodevelopers produktu</span><span class="sxs-lookup"><span data-stu-id="f556d-156"><a name="make-visible"> </a>Make a product visible toodevelopers</span></span>
<span data-ttu-id="f556d-157">Hello **viditelnost** karta vám umožní toochoose rolí, které jsou možné toosee hello produktu na portál pro vývojáře hello a přihlášení k odběru produktu toohello.</span><span class="sxs-lookup"><span data-stu-id="f556d-157">hello **Visibility** tab allows you toochoose which roles are able toosee hello product on hello developer portal and subscribe toohello product.</span></span>

![Přehled produktu][api-management-product-visiblity]

<span data-ttu-id="f556d-159">tooenable nebo vypnutí viditelnosti produktů pro vývojáře hello ve skupině, nebo zrušte zaškrtnutí políčka hello zaškrtněte políčko vedle skupiny hello a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f556d-159">tooenable or disable visibility of a product for hello developers in a group, check or uncheck hello check box beside hello group and then click **Save**.</span></span>

> <span data-ttu-id="f556d-160">Další informace najdete v tématu [jak toocreate a používání skupin toomanage vývojáře účty v Azure API Management][How toocreate and use groups toomanage developer accounts in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="f556d-160">For more information, see [How toocreate and use groups toomanage developer accounts in Azure API Management][How toocreate and use groups toomanage developer accounts in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="f556d-161"><a name="view-subscribers"></a>Zobrazení odběratele tooa produktu</span><span class="sxs-lookup"><span data-stu-id="f556d-161"><a name="view-subscribers"> </a>View subscribers tooa product</span></span>
<span data-ttu-id="f556d-162">Hello **Odběratelé, kteří** karta Vypíše seznam hello vývojáři, kteří odebírají toohello produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-162">hello **Subscribers** tab lists hello developers who have subscribed toohello product.</span></span> <span data-ttu-id="f556d-163">Hello podrobnosti a nastavení pro každý vývojář lze zobrazit kliknutím na název pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="f556d-163">hello details and settings for each developer can be viewed by clicking on hello developer's name.</span></span> <span data-ttu-id="f556d-164">V tomto příkladu jste žádné vývojáři ještě předplatné toohello produktu.</span><span class="sxs-lookup"><span data-stu-id="f556d-164">In this example no developers have yet subscribed toohello product.</span></span>

![Vývojáři][api-management-developer-list]

## <span data-ttu-id="f556d-166"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="f556d-166"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="f556d-167">Jednou hello požadované přidání rozhraní API, a publikovali hello produktu, vývojáři můžou přihlášení k odběru produktu toohello a začít toocall hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f556d-167">Once hello desired APIs are added and hello product published, developers can subscribe toohello product and begin toocall hello APIs.</span></span> <span data-ttu-id="f556d-168">Kurz, který ukazuje, tyto položky, jakož i konfigurace pokročilé produktu najdete v části [vytvoření a konfigurace pokročilých nastavení produktu v Azure API Management][How create and configure advanced product settings in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="f556d-168">For a tutorial that demonstrates these items as well as advanced product configuration see [How create and configure advanced product settings in Azure API Management][How create and configure advanced product settings in Azure API Management].</span></span>

<span data-ttu-id="f556d-169">Další informace o práci s produkty najdete v následující video hello.</span><span class="sxs-lookup"><span data-stu-id="f556d-169">For more information about working with products, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs tooa product]: #add-apis
[Add descriptive information tooa product]: #add-description
[Publish a product]: #publish-product
[Make a product visible toodevelopers]: #make-visible
[View subscribers tooa product]: #view-subscribers
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


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How toocreate and use groups toomanage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
