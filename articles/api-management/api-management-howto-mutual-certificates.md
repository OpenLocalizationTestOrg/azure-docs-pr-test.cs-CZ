---
title: "aaaSecure back endové služby pomocí ověření klientského certifikátu – Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toosecure back endové služby pomocí klientského certifikátu ověřování ve službě Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 43453331-39b2-4672-80b8-0a87e4fde3c6
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 565bb61044fed1158944202c36e8abe30edf5729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a><span data-ttu-id="d644f-103">Jak toosecure back endové služby pomocí klientského certifikátu ověřování ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d644f-103">How toosecure back-end services using client certificate authentication in Azure API Management</span></span>
<span data-ttu-id="d644f-104">Služba API Management nabízí hello schopností toosecure přístup toohello back endové službě rozhraní API pomocí klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="d644f-104">API Management provides hello capability toosecure access toohello back-end service of an API using client certificates.</span></span> <span data-ttu-id="d644f-105">Tato příručka ukazuje, jak toomanage certifikáty hello rozhraní API portálu vydavatele a jak tooconfigure rozhraní API toouse tooaccess certifikát jeho back endové službě.</span><span class="sxs-lookup"><span data-stu-id="d644f-105">This guide shows how toomanage certificates in hello API publisher portal, and how tooconfigure an API toouse a certificate tooaccess its back-end service.</span></span>

<span data-ttu-id="d644f-106">Informace o správě certifikátů pomocí hello rozhraní API REST API správy najdete v tématu [Azure API Management REST API certifikát entity][Azure API Management REST API Certificate entity].</span><span class="sxs-lookup"><span data-stu-id="d644f-106">For information about managing certificates using hello API Management REST API, see [Azure API Management REST API Certificate entity][Azure API Management REST API Certificate entity].</span></span>

## <span data-ttu-id="d644f-107"><a name="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="d644f-107"><a name="prerequisites"> </a>Prerequisites</span></span>
<span data-ttu-id="d644f-108">Tento průvodce vám ukáže, jak tooconfigure vaše rozhraní API správy služby instance toouse klienta certifikát ověřování tooaccess hello back-end službu pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d644f-108">This guide shows you how tooconfigure your API Management service instance toouse client certificate authentication tooaccess hello back-end service for an API.</span></span> <span data-ttu-id="d644f-109">Než hello následující kroky v tomto tématu, měli byste služby back-end, který je nakonfigurován pro ověřování pomocí certifikátu klienta ([tooconfigure certifikát ověřování v weby Azure najdete v článku toothis] [ tooconfigure certificate authentication in Azure WebSites refer toothis article]), a máte heslo certifikátu a hello toohello přístup pro hello certifikát pro odesílání v portálu vydavatele hello API Management.</span><span class="sxs-lookup"><span data-stu-id="d644f-109">Before following hello steps in this topic, you should have your back-end service configured for client certificate authentication ([tooconfigure certificate authentication in Azure WebSites refer toothis article][tooconfigure certificate authentication in Azure WebSites refer toothis article]), and have access toohello certificate and hello password for hello certificate for uploading in hello API Management publisher portal.</span></span>

## <span data-ttu-id="d644f-110"><a name="step1"></a>Nahrát certifikát klienta</span><span class="sxs-lookup"><span data-stu-id="d644f-110"><a name="step1"> </a>Upload a client certificate</span></span>
<span data-ttu-id="d644f-111">tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="d644f-111">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="d644f-112">Tím přejdete portál vydavatele toohello API Management.</span><span class="sxs-lookup"><span data-stu-id="d644f-112">This takes you toohello API Management publisher portal.</span></span>

![Portál vydavatele rozhraní API][api-management-management-console]

> <span data-ttu-id="d644f-114">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="d644f-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="d644f-115">Klikněte na tlačítko **zabezpečení** z hello **API Management** nabídky na levé straně hello a klikněte na **klientské certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="d644f-115">Click **Security** from hello **API Management** menu on hello left, and click **Client certificates**.</span></span>

![Klientské certifikáty][api-management-security-client-certificates]

<span data-ttu-id="d644f-117">tooupload nový certifikát, klikněte na tlačítko **nahrání certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="d644f-117">tooupload a new certificate, click **Upload certificate**.</span></span>

![Nahrání certifikátu][api-management-upload-certificate]

<span data-ttu-id="d644f-119">Procházet tooyour certifikát a potom zadejte hello heslo pro certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="d644f-119">Browse tooyour certificate, and then enter hello password for hello certificate.</span></span>

> <span data-ttu-id="d644f-120">Hello certifikát musí být ve **.pfx** formátu.</span><span class="sxs-lookup"><span data-stu-id="d644f-120">hello certificate must be in **.pfx** format.</span></span> <span data-ttu-id="d644f-121">Certifikáty podepsané svým držitelem jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="d644f-121">Self-signed certificates are allowed.</span></span>
> 
> 

![Nahrání certifikátu][api-management-upload-certificate-form]

<span data-ttu-id="d644f-123">Klikněte na tlačítko **nahrát** tooupload hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d644f-123">Click **Upload** tooupload hello certificate.</span></span>

> <span data-ttu-id="d644f-124">v tuto chvíli je ověřen Hello heslo certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d644f-124">hello certificate password is validated at this time.</span></span> <span data-ttu-id="d644f-125">Chybová zpráva se zobrazí, pokud je nesprávná.</span><span class="sxs-lookup"><span data-stu-id="d644f-125">If it is incorrect an error message is displayed.</span></span>
> 
> 

![Nahrát certifikát][api-management-certificate-uploaded]

<span data-ttu-id="d644f-127">Po nahrání certifikátu hello se zobrazí na hello **klientské certifikáty** kartě. Pokud máte víc certifikátů, poznamenejte hello předmět nebo text hello poslední čtyři znaky hello kryptografický otisk, které jsou používané tooselect hello certifikátu, při konfiguraci rozhraní API toouse certifikáty, jak je popsané v následující hello [konfigurace Rozhraní API toouse klientský certifikát pro ověřování brány] [ Configure an API toouse a client certificate for gateway authentication] části.</span><span class="sxs-lookup"><span data-stu-id="d644f-127">Once hello certificate is uploaded, it appears on hello **Client certificates** tab. If you have multiple certificates, make a note of hello subject, or hello last four characters of hello thumbprint, which are used tooselect hello certificate when configuring an API toouse certificates, as covered in hello following [Configure an API toouse a client certificate for gateway authentication][Configure an API toouse a client certificate for gateway authentication] section.</span></span>

> <span data-ttu-id="d644f-128">tooturn vypnout ověřování řetězu certifikátů, pokud například používáte certifikát podepsaný svým držitelem, postupujte podle kroků hello popsaných v této – nejčastější dotazy [položky](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).</span><span class="sxs-lookup"><span data-stu-id="d644f-128">tooturn off certificate chain validation when using, for example, a self-signed certificate, follow hello steps described in this FAQ [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).</span></span>
> 
> 

## <span data-ttu-id="d644f-129"><a name="step1a"></a>Odstranit certifikát klienta</span><span class="sxs-lookup"><span data-stu-id="d644f-129"><a name="step1a"> </a>Delete a client certificate</span></span>
<span data-ttu-id="d644f-130">toodelete certifikát, klikněte na tlačítko **odstranit** vedle hello požadovaný certifikát.</span><span class="sxs-lookup"><span data-stu-id="d644f-130">toodelete a certificate, click **Delete** beside hello desired certificate.</span></span>

![Odstranění certifikátu][api-management-certificate-delete]

<span data-ttu-id="d644f-132">Klikněte na tlačítko **Ano, odstraňte jej** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="d644f-132">Click **Yes, delete it** tooconfirm.</span></span>

![Potvrzení odstranění][api-management-confirm-delete]

<span data-ttu-id="d644f-134">Pokud je certifikát hello používá rozhraní API, se zobrazí obrazovka upozornění.</span><span class="sxs-lookup"><span data-stu-id="d644f-134">If hello certificate is in use by an API, then a warning screen is displayed.</span></span> <span data-ttu-id="d644f-135">toodelete hello certifikátu, je nutné nejprve odebrat hello certifikátu z jakékoli rozhraní API, které jsou nakonfigurované toouse ho.</span><span class="sxs-lookup"><span data-stu-id="d644f-135">toodelete hello certificate you must first remove hello certificate from any APIs that are configured toouse it.</span></span>

![Potvrzení odstranění][api-management-confirm-delete-policy]

## <span data-ttu-id="d644f-137"><a name="step2"></a>Nakonfigurovat rozhraní API toouse klientský certifikát pro ověřování brány</span><span class="sxs-lookup"><span data-stu-id="d644f-137"><a name="step2"> </a>Configure an API toouse a client certificate for gateway authentication</span></span>
<span data-ttu-id="d644f-138">Klikněte na tlačítko **rozhraní API** z hello **API Management** levé nabídce na hello, klikněte na název hello hello potřeby rozhraní API a klikněte na hello **zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="d644f-138">Click **APIs** from hello **API Management** menu on hello left, click hello name of hello desired API, and click hello **Security** tab.</span></span>

![Zabezpečení rozhraní API][api-management-api-security]

<span data-ttu-id="d644f-140">Vyberte **klientské certifikáty** z hello **s přihlašovacími údaji** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="d644f-140">Select **Client certificates** from hello **With credentials** drop-down list.</span></span>

![Klientské certifikáty][api-management-mutual-certificates]

<span data-ttu-id="d644f-142">Vyberte hello požadovaný certifikát z hello **klientský certifikát** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="d644f-142">Select hello desired certificate from hello **Client certificate** drop-down list.</span></span> <span data-ttu-id="d644f-143">Pokud máte víc certifikátů můžete prohlížet hello předmět nebo hello poslední čtyři znaky hello kryptografický otisk, jak jsme uvedli v hello předchozí část toodetermine hello správný certifikát.</span><span class="sxs-lookup"><span data-stu-id="d644f-143">If there are multiple certificates you can look at hello subject or hello last four characters of hello thumbprint as noted in hello previous section toodetermine hello correct certificate.</span></span>

![Vyberte certifikát][api-management-select-certificate]

<span data-ttu-id="d644f-145">Klikněte na tlačítko **Uložit** toosave hello konfigurace změnu toohello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d644f-145">Click **Save** toosave hello configuration change toohello API.</span></span>

> <span data-ttu-id="d644f-146">Tuto změnu je hned platná, a volání, které budou používat toooperations toto rozhraní API hello tooauthenticate certifikát na hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="d644f-146">This change is effective immediately, and calls toooperations of that API will use hello certificate tooauthenticate on hello back-end server.</span></span>
> 
> 

![Uložit změny rozhraní API][api-management-save-api]

> <span data-ttu-id="d644f-148">Pokud je zadán pro ověřování brány pro back-end službu hello rozhraní API, se stane součástí hello zásady pro toto rozhraní API a lze zobrazit v editoru zásad hello.</span><span class="sxs-lookup"><span data-stu-id="d644f-148">When a certificate is specified for gateway authentication for hello back-end service of an API, it becomes part of hello policy for that API, and can be viewed in hello policy editor.</span></span>
> 
> 

![Zásady certifikátu][api-management-certificate-policy]

## <a name="next-steps"></a><span data-ttu-id="d644f-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d644f-150">Next steps</span></span>
<span data-ttu-id="d644f-151">Další informace o dalších způsobů toosecure službě back-end, jako je například ověřování protokolu HTTP základní nebo sdílený tajný, najdete v části hello následující videa.</span><span class="sxs-lookup"><span data-stu-id="d644f-151">For more information on other ways toosecure your backend service, such as HTTP basic or shared secret authentication, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Last-mile-Security/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Azure API Management REST API Certificate entity]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[tooconfigure certificate authentication in Azure WebSites refer toothis article]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API toouse a client certificate for gateway authentication]: #step2
[Test hello configuration by calling an operation in hello Developer Portal]: #step3
[Next steps]: #next-steps



