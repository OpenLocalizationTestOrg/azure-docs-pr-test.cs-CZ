---
title: "Back endové služby pomocí klienta Secure certifikát ověřování – Azure API Management | Microsoft Docs"
description: "Zjistěte, jak zabezpečit back endové služby pomocí ověření klientského certifikátu ve službě Azure API Management."
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
ms.openlocfilehash: 2ebe71c96fd9076a48f689041634dbd23d3d8414
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a><span data-ttu-id="a4ebd-103">Jak zabezpečit back endové služby pomocí klienta pro ověřování pomocí certifikátu ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="a4ebd-103">How to secure back-end services using client certificate authentication in Azure API Management</span></span>
<span data-ttu-id="a4ebd-104">API Management poskytuje možnost zabezpečený přístup ke službě back endové rozhraní API pomocí klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-104">API Management provides the capability to secure access to the back-end service of an API using client certificates.</span></span> <span data-ttu-id="a4ebd-105">Tato příručka ukazuje, jak ke správě certifikátů v rozhraní API portálu vydavatele a postup konfigurace rozhraní API používat certifikát pro přístup k jeho back endové službě.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-105">This guide shows how to manage certificates in the API publisher portal, and how to configure an API to use a certificate to access its back-end service.</span></span>

<span data-ttu-id="a4ebd-106">Informace o správě certifikátů pomocí rozhraní API REST API správy najdete v tématu [Azure API Management REST API certifikát entity][Azure API Management REST API Certificate entity].</span><span class="sxs-lookup"><span data-stu-id="a4ebd-106">For information about managing certificates using the API Management REST API, see [Azure API Management REST API Certificate entity][Azure API Management REST API Certificate entity].</span></span>

## <span data-ttu-id="a4ebd-107"><a name="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="a4ebd-107"><a name="prerequisites"> </a>Prerequisites</span></span>
<span data-ttu-id="a4ebd-108">Tento průvodce vám ukáže, jak nakonfigurovat instanci služby API Management používat ověřování pomocí certifikátu klienta pro přístup k službě back-end pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-108">This guide shows you how to configure your API Management service instance to use client certificate authentication to access the back-end service for an API.</span></span> <span data-ttu-id="a4ebd-109">Před provedením postupu v tomto tématu, byste měli mít služby back-end, který je nakonfigurován pro ověřování pomocí certifikátu klienta ([konfigurace ověřování pomocí certifikátu v weby Azure najdete v tomto článku] [ to configure certificate authentication in Azure WebSites refer to this article]), a mít přístup k certifikátu a heslo pro certifikát pro odesílání v portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-109">Before following the steps in this topic, you should have your back-end service configured for client certificate authentication ([to configure certificate authentication in Azure WebSites refer to this article][to configure certificate authentication in Azure WebSites refer to this article]), and have access to the certificate and the password for the certificate for uploading in the API Management publisher portal.</span></span>

## <span data-ttu-id="a4ebd-110"><a name="step1"></a>Nahrát certifikát klienta</span><span class="sxs-lookup"><span data-stu-id="a4ebd-110"><a name="step1"> </a>Upload a client certificate</span></span>
<span data-ttu-id="a4ebd-111">Začněte tak, že na webu Azure Portal dané služby API Management kliknete na **Portál vydavatele**.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-111">To get started, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="a4ebd-112">Tím přejdete na portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-112">This takes you to the API Management publisher portal.</span></span>

![Portál vydavatele rozhraní API][api-management-management-console]

> <span data-ttu-id="a4ebd-114">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="a4ebd-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="a4ebd-115">Klikněte na tlačítko **zabezpečení** z **API Management** nabídky na levé straně a klikněte na **klientské certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-115">Click **Security** from the **API Management** menu on the left, and click **Client certificates**.</span></span>

![Klientské certifikáty][api-management-security-client-certificates]

<span data-ttu-id="a4ebd-117">Chcete-li nahrát nový certifikát, klikněte na tlačítko **nahrání certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-117">To upload a new certificate, click **Upload certificate**.</span></span>

![Nahrání certifikátu][api-management-upload-certificate]

<span data-ttu-id="a4ebd-119">Přejděte na svůj certifikát a pak zadejte heslo pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-119">Browse to your certificate, and then enter the password for the certificate.</span></span>

> <span data-ttu-id="a4ebd-120">Certifikát musí být v **.pfx** formátu.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-120">The certificate must be in **.pfx** format.</span></span> <span data-ttu-id="a4ebd-121">Certifikáty podepsané svým držitelem jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-121">Self-signed certificates are allowed.</span></span>
> 
> 

![Nahrání certifikátu][api-management-upload-certificate-form]

<span data-ttu-id="a4ebd-123">Klikněte na tlačítko **nahrát** na kterou odešlete certifikát.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-123">Click **Upload** to upload the certificate.</span></span>

> <span data-ttu-id="a4ebd-124">V tuto chvíli je ověřit heslo certifikátu.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-124">The certificate password is validated at this time.</span></span> <span data-ttu-id="a4ebd-125">Chybová zpráva se zobrazí, pokud je nesprávná.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-125">If it is incorrect an error message is displayed.</span></span>
> 
> 

![Nahrát certifikát][api-management-certificate-uploaded]

<span data-ttu-id="a4ebd-127">Po nahrání certifikátu se zobrazí na **klientské certifikáty** kartě.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-127">Once the certificate is uploaded, it appears on the **Client certificates** tab.</span></span> <span data-ttu-id="a4ebd-128">Pokud máte víc certifikátů, zajistěte, aby poznámku o předmět nebo poslední čtyři znaky kryptografický otisk, které se používají k výběru certifikátu, při konfiguraci rozhraní API používat certifikáty, jak je popsané v následující [konfigurace rozhraní API pro použití klientský certifikát pro ověřování brány] [ Configure an API to use a client certificate for gateway authentication] části.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-128">If you have multiple certificates, make a note of the subject, or the last four characters of the thumbprint, which are used to select the certificate when configuring an API to use certificates, as covered in the following [Configure an API to use a client certificate for gateway authentication][Configure an API to use a client certificate for gateway authentication] section.</span></span>

> <span data-ttu-id="a4ebd-129">Chcete-li vypnout ověřování řetězu certifikátů při použití, například certifikát podepsaný svým držitelem, postupujte podle kroků popsaných v této – nejčastější dotazy [položky](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).</span><span class="sxs-lookup"><span data-stu-id="a4ebd-129">To turn off certificate chain validation when using, for example, a self-signed certificate, follow the steps described in this FAQ [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).</span></span>
> 
> 

## <span data-ttu-id="a4ebd-130"><a name="step1a"></a>Odstranit certifikát klienta</span><span class="sxs-lookup"><span data-stu-id="a4ebd-130"><a name="step1a"> </a>Delete a client certificate</span></span>
<span data-ttu-id="a4ebd-131">Pokud chcete odstranit certifikát, klikněte na tlačítko **odstranit** vedle požadovaný certifikát.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-131">To delete a certificate, click **Delete** beside the desired certificate.</span></span>

![Odstranění certifikátu][api-management-certificate-delete]

<span data-ttu-id="a4ebd-133">Klikněte na tlačítko **Ano, odstraňte jej** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-133">Click **Yes, delete it** to confirm.</span></span>

![Potvrzení odstranění][api-management-confirm-delete]

<span data-ttu-id="a4ebd-135">Pokud se certifikát používá rozhraní API, se zobrazí obrazovka upozornění.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-135">If the certificate is in use by an API, then a warning screen is displayed.</span></span> <span data-ttu-id="a4ebd-136">Chcete-li odstranit certifikát nutno nejprve odstranit certifikát z jakékoli rozhraní API, které jsou nakonfigurovány pro použití.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-136">To delete the certificate you must first remove the certificate from any APIs that are configured to use it.</span></span>

![Potvrzení odstranění][api-management-confirm-delete-policy]

## <span data-ttu-id="a4ebd-138"><a name="step2"></a>Konfigurace rozhraní API používat klientský certifikát pro ověřování brány</span><span class="sxs-lookup"><span data-stu-id="a4ebd-138"><a name="step2"> </a>Configure an API to use a client certificate for gateway authentication</span></span>
<span data-ttu-id="a4ebd-139">Klikněte na tlačítko **rozhraní API** z **API Management** nabídky na levé straně klikněte na název požadované rozhraní API a klikněte na tlačítko **zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-139">Click **APIs** from the **API Management** menu on the left, click the name of the desired API, and click the **Security** tab.</span></span>

![Zabezpečení rozhraní API][api-management-api-security]

<span data-ttu-id="a4ebd-141">Vyberte **klientské certifikáty** z **s přihlašovacími údaji** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-141">Select **Client certificates** from the **With credentials** drop-down list.</span></span>

![Klientské certifikáty][api-management-mutual-certificates]

<span data-ttu-id="a4ebd-143">Vyberte požadovaný certifikát z **klientský certifikát** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-143">Select the desired certificate from the **Client certificate** drop-down list.</span></span> <span data-ttu-id="a4ebd-144">Pokud máte víc certifikátů, že můžete se podívat na předmět nebo poslední čtyři znaky kryptografický otisk, jak je uvedeno v předchozí části zjistit správný certifikát.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-144">If there are multiple certificates you can look at the subject or the last four characters of the thumbprint as noted in the previous section to determine the correct certificate.</span></span>

![Vyberte certifikát][api-management-select-certificate]

<span data-ttu-id="a4ebd-146">Klikněte na tlačítko **Uložit** se uložit změnu konfigurace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-146">Click **Save** to save the configuration change to the API.</span></span>

> <span data-ttu-id="a4ebd-147">Tuto změnu je hned platná, a volání operace tohoto rozhraní API bude používat certifikát k ověření na serveru back-end.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-147">This change is effective immediately, and calls to operations of that API will use the certificate to authenticate on the back-end server.</span></span>
> 
> 

![Uložit změny rozhraní API][api-management-save-api]

> <span data-ttu-id="a4ebd-149">Pokud je zadán pro ověřování brány pro back endové službě rozhraní API, se stane součástí zásad pro toto rozhraní API a lze zobrazit v editoru zásad.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-149">When a certificate is specified for gateway authentication for the back-end service of an API, it becomes part of the policy for that API, and can be viewed in the policy editor.</span></span>
> 
> 

![Zásady certifikátu][api-management-certificate-policy]

## <a name="next-steps"></a><span data-ttu-id="a4ebd-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a4ebd-151">Next steps</span></span>
<span data-ttu-id="a4ebd-152">Další informace o jiných způsobech zabezpečení službě back-end, jako je například ověřování protokolu HTTP základní nebo sdílený tajný najdete v následujícím videu.</span><span class="sxs-lookup"><span data-stu-id="a4ebd-152">For more information on other ways to secure your backend service, such as HTTP basic or shared secret authentication, see the following video.</span></span>

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



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Azure API Management REST API Certificate entity]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[to configure certificate authentication in Azure WebSites refer to this article]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API to use a client certificate for gateway authentication]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps



