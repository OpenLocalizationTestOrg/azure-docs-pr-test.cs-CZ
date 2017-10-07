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
# <a name="how-toosecure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Jak toosecure back endové služby pomocí klientského certifikátu ověřování ve službě Azure API Management
Služba API Management nabízí hello schopností toosecure přístup toohello back endové službě rozhraní API pomocí klientských certifikátů. Tato příručka ukazuje, jak toomanage certifikáty hello rozhraní API portálu vydavatele a jak tooconfigure rozhraní API toouse tooaccess certifikát jeho back endové službě.

Informace o správě certifikátů pomocí hello rozhraní API REST API správy najdete v tématu [Azure API Management REST API certifikát entity][Azure API Management REST API Certificate entity].

## <a name="prerequisites"></a>Požadavky
Tento průvodce vám ukáže, jak tooconfigure vaše rozhraní API správy služby instance toouse klienta certifikát ověřování tooaccess hello back-end službu pro rozhraní API. Než hello následující kroky v tomto tématu, měli byste služby back-end, který je nakonfigurován pro ověřování pomocí certifikátu klienta ([tooconfigure certifikát ověřování v weby Azure najdete v článku toothis] [ tooconfigure certificate authentication in Azure WebSites refer toothis article]), a máte heslo certifikátu a hello toohello přístup pro hello certifikát pro odesílání v portálu vydavatele hello API Management.

## <a name="step1"></a>Nahrát certifikát klienta
tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management. Tím přejdete portál vydavatele toohello API Management.

![Portál vydavatele rozhraní API][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

Klikněte na tlačítko **zabezpečení** z hello **API Management** nabídky na levé straně hello a klikněte na **klientské certifikáty**.

![Klientské certifikáty][api-management-security-client-certificates]

tooupload nový certifikát, klikněte na tlačítko **nahrání certifikátu**.

![Nahrání certifikátu][api-management-upload-certificate]

Procházet tooyour certifikát a potom zadejte hello heslo pro certifikát hello.

> Hello certifikát musí být ve **.pfx** formátu. Certifikáty podepsané svým držitelem jsou povoleny.
> 
> 

![Nahrání certifikátu][api-management-upload-certificate-form]

Klikněte na tlačítko **nahrát** tooupload hello certifikátu.

> v tuto chvíli je ověřen Hello heslo certifikátu. Chybová zpráva se zobrazí, pokud je nesprávná.
> 
> 

![Nahrát certifikát][api-management-certificate-uploaded]

Po nahrání certifikátu hello se zobrazí na hello **klientské certifikáty** kartě. Pokud máte víc certifikátů, poznamenejte hello předmět nebo text hello poslední čtyři znaky hello kryptografický otisk, které jsou používané tooselect hello certifikátu, při konfiguraci rozhraní API toouse certifikáty, jak je popsané v následující hello [konfigurace Rozhraní API toouse klientský certifikát pro ověřování brány] [ Configure an API toouse a client certificate for gateway authentication] části.

> tooturn vypnout ověřování řetězu certifikátů, pokud například používáte certifikát podepsaný svým držitelem, postupujte podle kroků hello popsaných v této – nejčastější dotazy [položky](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).
> 
> 

## <a name="step1a"></a>Odstranit certifikát klienta
toodelete certifikát, klikněte na tlačítko **odstranit** vedle hello požadovaný certifikát.

![Odstranění certifikátu][api-management-certificate-delete]

Klikněte na tlačítko **Ano, odstraňte jej** tooconfirm.

![Potvrzení odstranění][api-management-confirm-delete]

Pokud je certifikát hello používá rozhraní API, se zobrazí obrazovka upozornění. toodelete hello certifikátu, je nutné nejprve odebrat hello certifikátu z jakékoli rozhraní API, které jsou nakonfigurované toouse ho.

![Potvrzení odstranění][api-management-confirm-delete-policy]

## <a name="step2"></a>Nakonfigurovat rozhraní API toouse klientský certifikát pro ověřování brány
Klikněte na tlačítko **rozhraní API** z hello **API Management** levé nabídce na hello, klikněte na název hello hello potřeby rozhraní API a klikněte na hello **zabezpečení** kartě.

![Zabezpečení rozhraní API][api-management-api-security]

Vyberte **klientské certifikáty** z hello **s přihlašovacími údaji** rozevíracího seznamu.

![Klientské certifikáty][api-management-mutual-certificates]

Vyberte hello požadovaný certifikát z hello **klientský certifikát** rozevíracího seznamu. Pokud máte víc certifikátů můžete prohlížet hello předmět nebo hello poslední čtyři znaky hello kryptografický otisk, jak jsme uvedli v hello předchozí část toodetermine hello správný certifikát.

![Vyberte certifikát][api-management-select-certificate]

Klikněte na tlačítko **Uložit** toosave hello konfigurace změnu toohello rozhraní API.

> Tuto změnu je hned platná, a volání, které budou používat toooperations toto rozhraní API hello tooauthenticate certifikát na hello back-end serverů.
> 
> 

![Uložit změny rozhraní API][api-management-save-api]

> Pokud je zadán pro ověřování brány pro back-end službu hello rozhraní API, se stane součástí hello zásady pro toto rozhraní API a lze zobrazit v editoru zásad hello.
> 
> 

![Zásady certifikátu][api-management-certificate-policy]

## <a name="next-steps"></a>Další kroky
Další informace o dalších způsobů toosecure službě back-end, jako je například ověřování protokolu HTTP základní nebo sdílený tajný, najdete v části hello následující videa.

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



