---
title: "Postup přidání víceklientské aplikace pro galerii aplikací Azure AD | Microsoft Docs"
description: "Vysvětluje, jak můžete vytvořit seznam vlastní vyvinuté víceklientské aplikace v galerii aplikací Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 208f0d40bd7a8e8f35f16e1fcb09c305d833dbb2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a><span data-ttu-id="52e20-103">Postup přidání víceklientské aplikace pro galerii aplikací Azure AD</span><span class="sxs-lookup"><span data-stu-id="52e20-103">How to add a multi-tenant application to the Azure AD application gallery</span></span>

## <a name="what-is-the-azure-ad-application-gallery"></a><span data-ttu-id="52e20-104">Co je Azure AD Application Gallery?</span><span class="sxs-lookup"><span data-stu-id="52e20-104">What is the Azure AD Application Gallery?</span></span>

<span data-ttu-id="52e20-105">Azure AD Application Gallery je skvělým způsobem, jak dostat aplikaci před miliony Azure Active Directory zákazníky oslovit vaší aplikace na webu Marketplace a rozšíří dopad.</span><span class="sxs-lookup"><span data-stu-id="52e20-105">The Azure AD Application Gallery is a great way to get your application in front of all the millions of Azure Active Directory customers to broaden the impact and reach of your application in the marketplace.</span></span> <span data-ttu-id="52e20-106">Níže uvedených pokynů vysvětlují, jak můžete vytvořit seznam aplikace v galerii aplikací Azure AD.</span><span class="sxs-lookup"><span data-stu-id="52e20-106">The below steps explain how you can list your application in the Azure AD Application Gallery.</span></span>

## <a name="if-your-application-supports-saml-or-openidconnect"></a><span data-ttu-id="52e20-107">Pokud vaše aplikace podporuje SAML nebo OpenIDConnect</span><span class="sxs-lookup"><span data-stu-id="52e20-107">If your application supports SAML or OpenIDConnect</span></span>
<span data-ttu-id="52e20-108">Pokud máte více klientů aplikace, kterou chcete zobrazit seznam v galerii aplikací Azure AD, musíte nejprve provedete jistotu, že vaše aplikace podporuje jednu z následujících jeden přihlašování technologií:</span><span class="sxs-lookup"><span data-stu-id="52e20-108">If you have a multi-tenant application you'd like to list in the Azure AD Application Gallery, you must first make sure that your application supports one of the following single sign-on technologies:</span></span>

1. <span data-ttu-id="52e20-109">**OpenID Connect** -přímá integrace s Azure AD pomocí OpenID Connect pro ověřování a Azure AD souhlas rozhraní API pro konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="52e20-109">**OpenID Connect** - Direct integration with Azure AD using OpenID Connect for authentication and the Azure AD consent API for configuration.</span></span> <span data-ttu-id="52e20-110">Pokud právě začínáte integrační a aplikace nepodporuje SAML, jedná se o doporučujeme režim.</span><span class="sxs-lookup"><span data-stu-id="52e20-110">If you are just starting an integration and your application does not support SAML, then this is the recommend mode.</span></span>
2. <span data-ttu-id="52e20-111">**SAML** – aplikace již má možnost konfigurace poskytovatelů identit třetích stran pomocí protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="52e20-111">**SAML** – Your application already has the ability to configure third-party identity providers using the SAML protocol.</span></span>

<span data-ttu-id="52e20-112">Pokud vaše aplikace podporuje jeden z těchto režimů jednoho přihlášení a chcete uvedení víceklientské aplikace v galerii aplikací Azure AD, můžete podle kroků v dokumentu níže.</span><span class="sxs-lookup"><span data-stu-id="52e20-112">If your application supports one of these single sign-on modes and you'd like to list your multi-tenant application in the Azure AD Application Gallery, you can follow the steps in the document below.</span></span> <span data-ttu-id="52e20-113">Abyste mohli rychle začít e-mailovou zprávu na  **waadpartners@microsoft.com** .</span><span class="sxs-lookup"><span data-stu-id="52e20-113">To get started quickly send an email to **waadpartners@microsoft.com**.</span></span>

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a><span data-ttu-id="52e20-114">Pokud vaše aplikace nepodporuje SAML nebo OpenIDConnect</span><span class="sxs-lookup"><span data-stu-id="52e20-114">If your application does not support SAML or OpenIDConnect</span></span>
<span data-ttu-id="52e20-115">I v případě, že vaše aplikace nepodporuje jeden z těchto režimů, jsme můžete stále integrovat do naší Galerie pomocí technologie naše heslo jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="52e20-115">Even if your application does not support one of these modes, we can still integrate it into our gallery using our Password Single Sign-on technology.</span></span> <span data-ttu-id="52e20-116">Pokud chcete prozkoumat tuto možnost, můžete odeslat e-mail na  **waadpartners@microsoft.com** .</span><span class="sxs-lookup"><span data-stu-id="52e20-116">If you'd like to explore this option, you can send an email to **waadpartners@microsoft.com**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52e20-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52e20-117">Next steps</span></span>
[<span data-ttu-id="52e20-118">Postup uvedení aplikace v galerii aplikací Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="52e20-118">How to list your application in the Azure Active Directory application gallery</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
