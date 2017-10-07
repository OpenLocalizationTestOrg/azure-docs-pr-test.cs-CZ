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
# <a name="listing-your-application-in-hello-azure-active-directory-application-gallery"></a>Výpis aplikace v galerii aplikací Azure Active Directory hello
toolist aplikace, která podporuje jednotné přihlašování s Azure Active Directory v hello [Galerie Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/), aplikace hello musí nejprve tooimplement jeden hello následující režimy integrace:

* **OpenID Connect** – přímá integrace s Azure AD pro ověřování pomocí OpenID Connect a hello Azure AD souhlasu rozhraní API pro konfiguraci. Pokud právě začínáte integrační a aplikace nepodporuje SAML, jde hello doporučujeme režimu.
* **SAML** – aplikace již má hello možnost tooconfigure poskytovatelů identit třetích stran pomocí protokolu SAML hello.

Požadavky na výpis pro oba režimy jsou níže.

## <a name="openid-connect-integration"></a>OpenID Connect integrace
toointegrate vaší aplikace pomocí Azure AD, následující hello [vývojáře pokyny](active-directory-authentication-scenarios.md). Potom dokončete níže uvedené otázky hello a odeslat toowaadpartners@microsoft.com.

* Zadejte přihlašovací údaje k účtu nebo testovacím klientem s vaší aplikací, které je možné hello integrace hello tootest týmu Azure AD.  
* Poskytnout pokyny jak team hello Azure AD přihlášení a připojte se instanci tooyour aplikaci Azure AD pomocí hello [framework souhlasu Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework). 
* Zadejte, že žádné další pokyny potřebné pro hello Azure AD team tootest jednotné přihlašování s vaší aplikací. 
* Zadejte informace o hello níže:

> Název společnosti:
> 
> Web společnosti:
> 
> Název aplikace:
> 
> Popis aplikace (limit 200 znaků):
> 
> Web Application (informativní):
> 
> Web podpory Technical aplikace nebo kontaktní údaje:
> 
> ID aplikace hello, jak je znázorněno v hello podrobností o aplikaci v https://portal.azure.com:
> 
> Adresa URL aplikace poskytovateli kde Zákazníci přejděte toosign zaregistrovat a/nebo zakoupit hello aplikace:
> 
> Zvolte si toothree kategorie pro vaše aplikace toobe uvedené v části (kategorie k dispozici v tématu hello Azure Active Directory Marketplace):
> 
> Připojte malé ikony aplikace (soubor PNG, 45px podle 45px, plná barva pozadí):
> 
> Připojte velkých ikon aplikace (soubor PNG, 215px podle 215px, plná barva pozadí):
> 
> Připojte Logo aplikace (soubor PNG, 150px podle 122px, průhlednou barvu pozadí):
> 
> 

## <a name="saml-integration"></a>Integrace SAML
Jakékoli aplikaci, která podporuje SAML 2.0 umožňuje integraci přímo se klient služby Azure AD pomocí [tyto pokyny tooadd vlastní aplikaci](../active-directory-saas-custom-apps.md). Jakmile testování, svoji integraci aplikace funguje s Azure AD, odeslání hello následující informace příliš<mailto:waadpartners@microsoft.com>.

* Zadejte přihlašovací údaje k účtu nebo testovacím klientem s vaší aplikací, které je možné hello integrace hello tootest týmu Azure AD.  
* Zadejte text hello SAML přihlašovací adresa URL, adresa URL vystavitele (entity ID), a adresa URL odpovědi (služba assertion příjemce) hodnoty pro vaši aplikaci, jak je popsáno [zde](../active-directory-saas-custom-apps.md). Pokud tyto hodnoty se obvykle poskytují jako součást souboru metadat SAML, potom odešlete, také.
* Zadejte stručný popis toho, jak tooconfigure Azure AD jako zprostředkovatele identity do vaší aplikace pomocí SAML 2.0. Pokud vaše aplikace podporuje konfiguraci Azure AD jako zprostředkovatele identity prostřednictvím samoobslužný portál pro správu, potom ověřte, zda pověření hello výše uvedeného zahrnují možnost tooset hello to.
* Zadejte informace o hello níže:

> Název společnosti:
> 
> Web společnosti:
> 
> Název aplikace:
> 
> Popis aplikace (limit 200 znaků):
> 
> Web Application (informativní):
> 
> Web podpory Technical aplikace nebo kontaktní údaje:
> 
> Adresa URL aplikace poskytovateli kde Zákazníci přejděte toosign zaregistrovat a/nebo zakoupit hello aplikace:
> 
> Zvolte si toothree kategorie pro vaše aplikace toobe uvedený v seznamu (kategorie k dispozici, najdete v tématu hello [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Připojte malé ikony aplikace (soubor PNG, 45px podle 45px, plná barva pozadí):
> 
> Připojte velkých ikon aplikace (soubor PNG, 215px podle 215px, plná barva pozadí):
> 
> Připojte Logo aplikace (soubor PNG, 150px podle 122px, průhlednou barvu pozadí):
> 
> 

