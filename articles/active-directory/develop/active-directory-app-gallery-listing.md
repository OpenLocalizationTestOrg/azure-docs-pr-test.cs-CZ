---
title: "Výpis aplikace v galerii aplikací Azure Active Directory"
description: "Jak zobrazit aplikace, která podporuje jednotné přihlašování v galerii Azure Active Directory | Microsoft Azure"
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
ms.openlocfilehash: cf25772bd9d92b59401aa5da76e6bbd5fa5ee3e5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a><span data-ttu-id="a8bfc-103">Výpis aplikace v galerii aplikací Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8bfc-103">Listing your application in the Azure Active Directory application gallery</span></span>
<span data-ttu-id="a8bfc-104">Aplikace, která podporuje jednotné přihlašování s Azure Active Directory na seznam [Galerie Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/), aplikace se nejdřív musí implementovat jednu z následujících režimů integrace:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-104">To list an application that supports single sign-on with Azure Active Directory in the [Azure AD gallery](https://azure.microsoft.com/marketplace/active-directory/all/), the application first needs to implement one of the following integration modes:</span></span>

* <span data-ttu-id="a8bfc-105">**OpenID Connect** -přímá integrace s Azure AD pomocí OpenID Connect pro ověřování a Azure AD souhlas rozhraní API pro konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-105">**OpenID Connect** - Direct integration with Azure AD using OpenID Connect for authentication and the Azure AD consent API for configuration.</span></span> <span data-ttu-id="a8bfc-106">Pokud právě začínáte integrační a aplikace nepodporuje SAML, jedná se o doporučujeme režim.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-106">If you are just starting an integration and your application does not support SAML, then this is the recommend mode.</span></span>
* <span data-ttu-id="a8bfc-107">**SAML** – aplikace již má možnost konfigurace poskytovatelů identit třetích stran pomocí protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-107">**SAML** – Your application already has the ability to configure third-party identity providers using the SAML protocol.</span></span>

<span data-ttu-id="a8bfc-108">Požadavky na výpis pro oba režimy jsou níže.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-108">Listing requirements for each mode are below.</span></span>

## <a name="openid-connect-integration"></a><span data-ttu-id="a8bfc-109">OpenID Connect integrace</span><span class="sxs-lookup"><span data-stu-id="a8bfc-109">OpenID Connect Integration</span></span>
<span data-ttu-id="a8bfc-110">K integraci aplikace s Azure AD, následující [vývojáře pokyny](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfc-110">To integrate your application with Azure AD, following the [developer instructions](active-directory-authentication-scenarios.md).</span></span> <span data-ttu-id="a8bfc-111">Potom dokončete níže uvedené otázky a poslat waadpartners@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-111">Then complete the questions below and send to waadpartners@microsoft.com.</span></span>

* <span data-ttu-id="a8bfc-112">Zadejte přihlašovací údaje k účtu nebo testovacím klientem s vaší aplikací, které je možné otestovat integrace tým služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-112">Provide credentials for a test tenant or account with your application that can be used by the Azure AD team to test the integration.</span></span>  
* <span data-ttu-id="a8bfc-113">Poskytnout pokyny jak tým Azure AD přihlášení a připojit instanci Azure AD k aplikaci pomocí [framework souhlasu Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework).</span><span class="sxs-lookup"><span data-stu-id="a8bfc-113">Provide instructions on how the Azure AD team can sign in and connect an instance of Azure AD to your application using the [Azure AD consent framework](active-directory-integrating-applications.md#overview-of-the-consent-framework).</span></span> 
* <span data-ttu-id="a8bfc-114">Zadejte jakékoli další pokyny potřebné pro tento tým služby Azure AD k testování jednotné přihlašování s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-114">Provide any further instructions required for the Azure AD team to test single sign-on with your application.</span></span> 
* <span data-ttu-id="a8bfc-115">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-115">Provide the info below:</span></span>

> <span data-ttu-id="a8bfc-116">Název společnosti:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-116">Company Name:</span></span>
> 
> <span data-ttu-id="a8bfc-117">Web společnosti:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-117">Company Website:</span></span>
> 
> <span data-ttu-id="a8bfc-118">Název aplikace:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-118">Application Name:</span></span>
> 
> <span data-ttu-id="a8bfc-119">Popis aplikace (limit 200 znaků):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-119">Application Description (200 character limit):</span></span>
> 
> <span data-ttu-id="a8bfc-120">Web Application (informativní):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-120">Application Website (informational):</span></span>
> 
> <span data-ttu-id="a8bfc-121">Web podpory Technical aplikace nebo kontaktní údaje:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-121">Application Technical Support Website or Contact Info:</span></span>
> 
> <span data-ttu-id="a8bfc-122">ID aplikace, jak je znázorněno v podrobností o aplikaci v https://portal.azure.com:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-122">Application  ID of the application, as shown in the application details at https://portal.azure.com:</span></span>
> 
> <span data-ttu-id="a8bfc-123">Adresa URL aplikace poskytovateli kde Zákazníci moct zaregistrovat a/nebo zakoupit aplikace:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-123">Application Sign-Up URL where customers go to sign up for and /or purchase the application:</span></span>
> 
> <span data-ttu-id="a8bfc-124">Vyberte až tři kategorie pro aplikace uvedené v části (pro Azure Active Directory Marketplace naleznete kategorie k dispozici v):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-124">Choose up to three categories for your application to be listed under (for available categories see the Azure Active Directory Marketplace):</span></span>
> 
> <span data-ttu-id="a8bfc-125">Připojte malé ikony aplikace (soubor PNG, 45px podle 45px, plná barva pozadí):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-125">Attach Application Small Icon (PNG file, 45px by 45px, solid background color):</span></span>
> 
> <span data-ttu-id="a8bfc-126">Připojte velkých ikon aplikace (soubor PNG, 215px podle 215px, plná barva pozadí):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-126">Attach Application Large Icon (PNG file, 215px by 215px, solid background color):</span></span>
> 
> <span data-ttu-id="a8bfc-127">Připojte Logo aplikace (soubor PNG, 150px podle 122px, průhlednou barvu pozadí):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-127">Attach Application Logo (PNG file, 150px by 122px, transparent background color):</span></span>
> 
> 

## <a name="saml-integration"></a><span data-ttu-id="a8bfc-128">Integrace SAML</span><span class="sxs-lookup"><span data-stu-id="a8bfc-128">SAML Integration</span></span>
<span data-ttu-id="a8bfc-129">Jakékoli aplikaci, která podporuje SAML 2.0 umožňuje integraci přímo se klient služby Azure AD pomocí [těchto pokynů můžete přidat vlastní aplikaci](../active-directory-saas-custom-apps.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfc-129">Any app that supports SAML 2.0 can be integrated directly with an Azure AD tenant using [these instructions to add a custom application](../active-directory-saas-custom-apps.md).</span></span> <span data-ttu-id="a8bfc-130">Jakmile testování, svoji integraci aplikace funguje s Azure AD, poslat následující informace, které < mailto:waadpartners@microsoft.com >.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-130">Once you have tested that your application integration works with Azure AD, send the following information to <mailto:waadpartners@microsoft.com>.</span></span>

* <span data-ttu-id="a8bfc-131">Zadejte přihlašovací údaje k účtu nebo testovacím klientem s vaší aplikací, které je možné otestovat integrace tým služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-131">Provide credentials for a test tenant or account with your application that can be used by the Azure AD team to test the integration.</span></span>  
* <span data-ttu-id="a8bfc-132">Zadejte adresy přihlašování SAML, URL vystavitele (entity ID) a adresa URL odpovědi (služba assertion příjemce) hodnoty pro vaši aplikaci, jak je popsáno [zde](../active-directory-saas-custom-apps.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfc-132">Provide the SAML Sign-On URL, Issuer URL (entity ID), and Reply URL (assertion consumer service) values for your application, as described [here](../active-directory-saas-custom-apps.md).</span></span> <span data-ttu-id="a8bfc-133">Pokud tyto hodnoty se obvykle poskytují jako součást souboru metadat SAML, potom odešlete, také.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-133">If you typically provide these values as part of a SAML metadata file, then please send that as well.</span></span>
* <span data-ttu-id="a8bfc-134">Zadejte stručný popis, jak nakonfigurovat Azure AD jako zprostředkovatele identity do vaší aplikace pomocí SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-134">Provide a brief description of how to configure Azure AD as an identity provider in your application using SAML 2.0.</span></span> <span data-ttu-id="a8bfc-135">Pokud vaše aplikace podporuje konfiguraci Azure AD jako zprostředkovatele identity prostřednictvím samoobslužný portál pro správu, potom ověřte, zda pověření výše uvedeného, zahrnují možnost nastavit tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="a8bfc-135">If your application supports configuring Azure AD as an identity provider through a self-service administrative portal, then please ensure the credentials provided above include the ability to set this up.</span></span>
* <span data-ttu-id="a8bfc-136">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-136">Provide the info below:</span></span>

> <span data-ttu-id="a8bfc-137">Název společnosti:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-137">Company Name:</span></span>
> 
> <span data-ttu-id="a8bfc-138">Web společnosti:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-138">Company Website:</span></span>
> 
> <span data-ttu-id="a8bfc-139">Název aplikace:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-139">Application Name:</span></span>
> 
> <span data-ttu-id="a8bfc-140">Popis aplikace (limit 200 znaků):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-140">Application Description (200 character limit):</span></span>
> 
> <span data-ttu-id="a8bfc-141">Web Application (informativní):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-141">Application Website (informational):</span></span>
> 
> <span data-ttu-id="a8bfc-142">Web podpory Technical aplikace nebo kontaktní údaje:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-142">Application Technical Support Website or Contact Info:</span></span>
> 
> <span data-ttu-id="a8bfc-143">Adresa URL aplikace poskytovateli kde Zákazníci moct zaregistrovat a/nebo zakoupit aplikace:</span><span class="sxs-lookup"><span data-stu-id="a8bfc-143">Application Sign-Up URL where customers go to sign up for and /or purchase the application:</span></span>
> 
> <span data-ttu-id="a8bfc-144">Vyberte maximálně tři kategorie pro aplikace uvedené v části (kategorie k dispozici, najdete v tématu [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-144">Choose up to three categories for your application to be listed under (for available categories see the [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):</span></span>
> 
> <span data-ttu-id="a8bfc-145">Připojte malé ikony aplikace (soubor PNG, 45px podle 45px, plná barva pozadí):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-145">Attach Application Small Icon (PNG file, 45px by 45px, solid background color):</span></span>
> 
> <span data-ttu-id="a8bfc-146">Připojte velkých ikon aplikace (soubor PNG, 215px podle 215px, plná barva pozadí):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-146">Attach Application Large Icon (PNG file, 215px by 215px, solid background color):</span></span>
> 
> <span data-ttu-id="a8bfc-147">Připojte Logo aplikace (soubor PNG, 150px podle 122px, průhlednou barvu pozadí):</span><span class="sxs-lookup"><span data-stu-id="a8bfc-147">Attach Application Logo (PNG file, 150px by 122px, transparent background color):</span></span>
> 
> 

