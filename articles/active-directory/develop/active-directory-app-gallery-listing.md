---
title: "aaaListing aplikace v galerii aplikací Azure Active Directory hello"
description: "Jak hello toolist aplikace, která podporuje jednotné přihlašování v galerii Azure Active Directory | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 09ccd3b4645a180059b9a9d502e39f1b8933c988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="listing-your-application-in-hello-azure-active-directory-application-gallery"></a><span data-ttu-id="cab14-103">Výpis aplikace v galerii aplikací Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="cab14-103">Listing your application in hello Azure Active Directory application gallery</span></span>
<span data-ttu-id="cab14-104">toolist aplikace, která podporuje jednotné přihlašování s Azure Active Directory v hello [Galerie Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/), aplikace hello musí nejprve tooimplement jeden hello následující režimy integrace:</span><span class="sxs-lookup"><span data-stu-id="cab14-104">toolist an application that supports single sign-on with Azure Active Directory in hello [Azure AD gallery](https://azure.microsoft.com/marketplace/active-directory/all/), hello application first needs tooimplement one of hello following integration modes:</span></span>

* <span data-ttu-id="cab14-105">**OpenID Connect** – přímá integrace s Azure AD pro ověřování pomocí OpenID Connect a hello Azure AD souhlasu rozhraní API pro konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cab14-105">**OpenID Connect** - Direct integration with Azure AD using OpenID Connect for authentication and hello Azure AD consent API for configuration.</span></span> <span data-ttu-id="cab14-106">Pokud právě začínáte integrační a aplikace nepodporuje SAML, jde hello doporučujeme režimu.</span><span class="sxs-lookup"><span data-stu-id="cab14-106">If you are just starting an integration and your application does not support SAML, then this is hello recommend mode.</span></span>
* <span data-ttu-id="cab14-107">**SAML** – aplikace již má hello možnost tooconfigure poskytovatelů identit třetích stran pomocí protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="cab14-107">**SAML** – Your application already has hello ability tooconfigure third-party identity providers using hello SAML protocol.</span></span>

<span data-ttu-id="cab14-108">Požadavky na výpis pro oba režimy jsou níže.</span><span class="sxs-lookup"><span data-stu-id="cab14-108">Listing requirements for each mode are below.</span></span>

## <a name="openid-connect-integration"></a><span data-ttu-id="cab14-109">OpenID Connect integrace</span><span class="sxs-lookup"><span data-stu-id="cab14-109">OpenID Connect Integration</span></span>
<span data-ttu-id="cab14-110">toointegrate vaší aplikace pomocí Azure AD, následující hello [vývojáře pokyny](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="cab14-110">toointegrate your application with Azure AD, following hello [developer instructions](active-directory-authentication-scenarios.md).</span></span> <span data-ttu-id="cab14-111">Potom dokončete níže uvedené otázky hello a odeslat toowaadpartners@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="cab14-111">Then complete hello questions below and send toowaadpartners@microsoft.com.</span></span>

* <span data-ttu-id="cab14-112">Zadejte přihlašovací údaje k účtu nebo testovacím klientem s vaší aplikací, které je možné hello integrace hello tootest týmu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cab14-112">Provide credentials for a test tenant or account with your application that can be used by hello Azure AD team tootest hello integration.</span></span>  
* <span data-ttu-id="cab14-113">Poskytnout pokyny jak team hello Azure AD přihlášení a připojte se instanci tooyour aplikaci Azure AD pomocí hello [framework souhlasu Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework).</span><span class="sxs-lookup"><span data-stu-id="cab14-113">Provide instructions on how hello Azure AD team can sign in and connect an instance of Azure AD tooyour application using hello [Azure AD consent framework](active-directory-integrating-applications.md#overview-of-the-consent-framework).</span></span> 
* <span data-ttu-id="cab14-114">Zadejte, že žádné další pokyny potřebné pro hello Azure AD team tootest jednotné přihlašování s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="cab14-114">Provide any further instructions required for hello Azure AD team tootest single sign-on with your application.</span></span> 
* <span data-ttu-id="cab14-115">Zadejte informace o hello níže:</span><span class="sxs-lookup"><span data-stu-id="cab14-115">Provide hello info below:</span></span>

> <span data-ttu-id="cab14-116">Název společnosti:</span><span class="sxs-lookup"><span data-stu-id="cab14-116">Company Name:</span></span>
> 
> <span data-ttu-id="cab14-117">Web společnosti:</span><span class="sxs-lookup"><span data-stu-id="cab14-117">Company Website:</span></span>
> 
> <span data-ttu-id="cab14-118">Název aplikace:</span><span class="sxs-lookup"><span data-stu-id="cab14-118">Application Name:</span></span>
> 
> <span data-ttu-id="cab14-119">Popis aplikace (limit 200 znaků):</span><span class="sxs-lookup"><span data-stu-id="cab14-119">Application Description (200 character limit):</span></span>
> 
> <span data-ttu-id="cab14-120">Web Application (informativní):</span><span class="sxs-lookup"><span data-stu-id="cab14-120">Application Website (informational):</span></span>
> 
> <span data-ttu-id="cab14-121">Web podpory Technical aplikace nebo kontaktní údaje:</span><span class="sxs-lookup"><span data-stu-id="cab14-121">Application Technical Support Website or Contact Info:</span></span>
> 
> <span data-ttu-id="cab14-122">ID aplikace hello, jak je znázorněno v hello podrobností o aplikaci v https://portal.azure.com:</span><span class="sxs-lookup"><span data-stu-id="cab14-122">Application  ID of hello application, as shown in hello application details at https://portal.azure.com:</span></span>
> 
> <span data-ttu-id="cab14-123">Adresa URL aplikace poskytovateli kde Zákazníci přejděte toosign zaregistrovat a/nebo zakoupit hello aplikace:</span><span class="sxs-lookup"><span data-stu-id="cab14-123">Application Sign-Up URL where customers go toosign up for and /or purchase hello application:</span></span>
> 
> <span data-ttu-id="cab14-124">Zvolte si toothree kategorie pro vaše aplikace toobe uvedené v části (kategorie k dispozici v tématu hello Azure Active Directory Marketplace):</span><span class="sxs-lookup"><span data-stu-id="cab14-124">Choose up toothree categories for your application toobe listed under (for available categories see hello Azure Active Directory Marketplace):</span></span>
> 
> <span data-ttu-id="cab14-125">Připojte malé ikony aplikace (soubor PNG, 45px podle 45px, plná barva pozadí):</span><span class="sxs-lookup"><span data-stu-id="cab14-125">Attach Application Small Icon (PNG file, 45px by 45px, solid background color):</span></span>
> 
> <span data-ttu-id="cab14-126">Připojte velkých ikon aplikace (soubor PNG, 215px podle 215px, plná barva pozadí):</span><span class="sxs-lookup"><span data-stu-id="cab14-126">Attach Application Large Icon (PNG file, 215px by 215px, solid background color):</span></span>
> 
> <span data-ttu-id="cab14-127">Připojte Logo aplikace (soubor PNG, 150px podle 122px, průhlednou barvu pozadí):</span><span class="sxs-lookup"><span data-stu-id="cab14-127">Attach Application Logo (PNG file, 150px by 122px, transparent background color):</span></span>
> 
> 

## <a name="saml-integration"></a><span data-ttu-id="cab14-128">Integrace SAML</span><span class="sxs-lookup"><span data-stu-id="cab14-128">SAML Integration</span></span>
<span data-ttu-id="cab14-129">Jakékoli aplikaci, která podporuje SAML 2.0 umožňuje integraci přímo se klient služby Azure AD pomocí [tyto pokyny tooadd vlastní aplikaci](../active-directory-saas-custom-apps.md).</span><span class="sxs-lookup"><span data-stu-id="cab14-129">Any app that supports SAML 2.0 can be integrated directly with an Azure AD tenant using [these instructions tooadd a custom application](../active-directory-saas-custom-apps.md).</span></span> <span data-ttu-id="cab14-130">Jakmile testování, svoji integraci aplikace funguje s Azure AD, odeslání hello následující informace příliš<mailto:waadpartners@microsoft.com>.</span><span class="sxs-lookup"><span data-stu-id="cab14-130">Once you have tested that your application integration works with Azure AD, send hello following information too<mailto:waadpartners@microsoft.com>.</span></span>

* <span data-ttu-id="cab14-131">Zadejte přihlašovací údaje k účtu nebo testovacím klientem s vaší aplikací, které je možné hello integrace hello tootest týmu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cab14-131">Provide credentials for a test tenant or account with your application that can be used by hello Azure AD team tootest hello integration.</span></span>  
* <span data-ttu-id="cab14-132">Zadejte text hello SAML přihlašovací adresa URL, adresa URL vystavitele (entity ID), a adresa URL odpovědi (služba assertion příjemce) hodnoty pro vaši aplikaci, jak je popsáno [zde](../active-directory-saas-custom-apps.md).</span><span class="sxs-lookup"><span data-stu-id="cab14-132">Provide hello SAML Sign-On URL, Issuer URL (entity ID), and Reply URL (assertion consumer service) values for your application, as described [here](../active-directory-saas-custom-apps.md).</span></span> <span data-ttu-id="cab14-133">Pokud tyto hodnoty se obvykle poskytují jako součást souboru metadat SAML, potom odešlete, také.</span><span class="sxs-lookup"><span data-stu-id="cab14-133">If you typically provide these values as part of a SAML metadata file, then please send that as well.</span></span>
* <span data-ttu-id="cab14-134">Zadejte stručný popis toho, jak tooconfigure Azure AD jako zprostředkovatele identity do vaší aplikace pomocí SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="cab14-134">Provide a brief description of how tooconfigure Azure AD as an identity provider in your application using SAML 2.0.</span></span> <span data-ttu-id="cab14-135">Pokud vaše aplikace podporuje konfiguraci Azure AD jako zprostředkovatele identity prostřednictvím samoobslužný portál pro správu, potom ověřte, zda pověření hello výše uvedeného zahrnují možnost tooset hello to.</span><span class="sxs-lookup"><span data-stu-id="cab14-135">If your application supports configuring Azure AD as an identity provider through a self-service administrative portal, then please ensure hello credentials provided above include hello ability tooset this up.</span></span>
* <span data-ttu-id="cab14-136">Zadejte informace o hello níže:</span><span class="sxs-lookup"><span data-stu-id="cab14-136">Provide hello info below:</span></span>

> <span data-ttu-id="cab14-137">Název společnosti:</span><span class="sxs-lookup"><span data-stu-id="cab14-137">Company Name:</span></span>
> 
> <span data-ttu-id="cab14-138">Web společnosti:</span><span class="sxs-lookup"><span data-stu-id="cab14-138">Company Website:</span></span>
> 
> <span data-ttu-id="cab14-139">Název aplikace:</span><span class="sxs-lookup"><span data-stu-id="cab14-139">Application Name:</span></span>
> 
> <span data-ttu-id="cab14-140">Popis aplikace (limit 200 znaků):</span><span class="sxs-lookup"><span data-stu-id="cab14-140">Application Description (200 character limit):</span></span>
> 
> <span data-ttu-id="cab14-141">Web Application (informativní):</span><span class="sxs-lookup"><span data-stu-id="cab14-141">Application Website (informational):</span></span>
> 
> <span data-ttu-id="cab14-142">Web podpory Technical aplikace nebo kontaktní údaje:</span><span class="sxs-lookup"><span data-stu-id="cab14-142">Application Technical Support Website or Contact Info:</span></span>
> 
> <span data-ttu-id="cab14-143">Adresa URL aplikace poskytovateli kde Zákazníci přejděte toosign zaregistrovat a/nebo zakoupit hello aplikace:</span><span class="sxs-lookup"><span data-stu-id="cab14-143">Application Sign-Up URL where customers go toosign up for and /or purchase hello application:</span></span>
> 
> <span data-ttu-id="cab14-144">Zvolte si toothree kategorie pro vaše aplikace toobe uvedený v seznamu (kategorie k dispozici, najdete v tématu hello [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):</span><span class="sxs-lookup"><span data-stu-id="cab14-144">Choose up toothree categories for your application toobe listed under (for available categories see hello [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):</span></span>
> 
> <span data-ttu-id="cab14-145">Připojte malé ikony aplikace (soubor PNG, 45px podle 45px, plná barva pozadí):</span><span class="sxs-lookup"><span data-stu-id="cab14-145">Attach Application Small Icon (PNG file, 45px by 45px, solid background color):</span></span>
> 
> <span data-ttu-id="cab14-146">Připojte velkých ikon aplikace (soubor PNG, 215px podle 215px, plná barva pozadí):</span><span class="sxs-lookup"><span data-stu-id="cab14-146">Attach Application Large Icon (PNG file, 215px by 215px, solid background color):</span></span>
> 
> <span data-ttu-id="cab14-147">Připojte Logo aplikace (soubor PNG, 150px podle 122px, průhlednou barvu pozadí):</span><span class="sxs-lookup"><span data-stu-id="cab14-147">Attach Application Logo (PNG file, 150px by 122px, transparent background color):</span></span>
> 
> 

