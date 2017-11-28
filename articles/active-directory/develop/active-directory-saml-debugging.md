---
title: "aaaHow toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 846c7b3497c8842947c5b406f4362b9e06785b14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-saml-based-single-sign-on-tooapplications-in-azure-active-directory"></a><span data-ttu-id="18115-103">Jak toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="18115-103">How toodebug SAML-based single sign-on tooapplications in Azure Active Directory</span></span>
<span data-ttu-id="18115-104">Při ladění aplikace založené na SAML integrace, je často užitečné toouse nástroje, jako je [Fiddler](http://www.telerik.com/fiddler) toosee hello požadavku a odpovědi SAML hello a hello skutečné SAML token, který je vydán toohello aplikace SAML.</span><span class="sxs-lookup"><span data-stu-id="18115-104">When debugging a SAML-based application integration, it is often helpful toouse a tool like [Fiddler](http://www.telerik.com/fiddler) toosee hello SAML request, hello SAML response, and hello actual SAML token that is issued toohello application.</span></span> <span data-ttu-id="18115-105">Prozkoumáním tokenu SAML hello zajistěte, aby všechny hello atributy, hello uživatelské jméno v předmětu hello SAML a hello vystavitele URI přicházejí prostřednictvím podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="18115-105">By examining hello SAML token, you can ensure that all of hello required attributes, hello username in hello SAML subject, and hello issuer URI are coming through as expected.</span></span>

![][1]

<span data-ttu-id="18115-106">Hello odpověď z Azure AD, který obsahuje hello SAML token je obvykle hello jeden, který se nachází za přesměrování HTTP 302 z https://login.windows.net a je nakonfigurován odeslané toohello **adresa URL odpovědi** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="18115-106">hello response from Azure AD that contains hello SAML token is typically hello one that occurs after an HTTP 302 redirect from https://login.windows.net, and is sent toohello configured **Reply URL** of hello application.</span></span> 

<span data-ttu-id="18115-107">Hello SAML token můžete zobrazit výběrem tohoto řádku a potom výběrem hello **inspektoři > WebForms** kartě v pravém panelu hello.</span><span class="sxs-lookup"><span data-stu-id="18115-107">You can view hello SAML token by selecting this line and then selecting hello **Inspectors > WebForms** tab in hello right panel.</span></span> <span data-ttu-id="18115-108">Zde, klikněte pravým tlačítkem na hello **SAMLResponse** hodnotu a vyberte **odeslat tooTextWizard**.</span><span class="sxs-lookup"><span data-stu-id="18115-108">From there, right-click hello **SAMLResponse** value and select **Send tooTextWizard**.</span></span> <span data-ttu-id="18115-109">Potom vyberte **z formátu Base64** z hello **transformace** nabídky toodecode hello token a zobrazit jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="18115-109">Then select **From Base64** from hello **Transform** menu toodecode hello token and see its contents.</span></span>

<span data-ttu-id="18115-110">**Poznámka:**: toosee hello obsah této HTTP požadavků, Fiddler můžete být vyzváni tooconfigure dešifrování přenosy HTTPS, které budete potřebovat toodo.</span><span class="sxs-lookup"><span data-stu-id="18115-110">**Note**: toosee hello contents of this HTTP request, Fiddler may prompt you tooconfigure decryption of HTTPS traffic, which you will need toodo.</span></span>

## <a name="related-articles"></a><span data-ttu-id="18115-111">Související články</span><span class="sxs-lookup"><span data-stu-id="18115-111">Related Articles</span></span>
* [<span data-ttu-id="18115-112">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="18115-112">Article Index for Application Management in Azure Active Directory</span></span>](../active-directory-apps-index.md)
* [<span data-ttu-id="18115-113">Konfigurace jednoho přihlášení tooapplications které nejsou v galerii aplikací Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="18115-113">Configuring single sign-on tooapplications that are not in hello Azure Active Directory application gallery</span></span>](../active-directory-saas-custom-apps.md)
* [<span data-ttu-id="18115-114">Jak tooCustomize deklarace identity vystavené v hello tokenu SAML pro Pre-Integrated aplikace</span><span class="sxs-lookup"><span data-stu-id="18115-114">How tooCustomize Claims Issued in hello SAML Token for Pre-Integrated Apps</span></span>](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png