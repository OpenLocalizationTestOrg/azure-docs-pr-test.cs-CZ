---
title: "aaaAuthenticate s místní služby Active Directory v aplikaci Azure | Microsoft Docs"
description: "Další informace o hello různé možnosti pro-obchodní aplikace v Azure App Service tooauthenticate s místní služby Active Directory"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: dde6b7e6-bf6a-4fa5-8390-3a18155d21bd
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/31/2016
ms.author: cephalin
ms.openlocfilehash: 65bf25aaa0447fbbea7c754db55842d57e70757e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Ověření pomocí místní služby Active Directory v aplikaci Azure
Tento článek ukazuje, jak tooauthenticate s místní služby Active Directory (AD) v [Azure App Service](../app-service/app-service-value-prop-what-is.md). Aplikace Azure je hostovaná v cloudu hello, ale existují způsoby tooauthenticate místní uživatele AD bezpečně. 

## <a name="authenticate-through-azure-active-directory"></a>Ověřování pomocí služby Azure Active Directory
Klient služby Azure Active Directory může být directory synchronizované s místní AD. Tento přístup AD uživatelům umožňuje přístup k aplikaci z Internetu hello a provést ověření pomocí jejich místních přihlašovacích údajů. Kromě toho poskytuje Azure App Service [řešení na klíč pro tuto metodu](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Pomocí několika kliknutí tlačítko můžete povolit ověřování pomocí klienta directory synchronizovat pro vaše aplikace Azure. Tento přístup má hello následující výhody:

* Nevyžaduje žádné ověřovací kód ve vaší aplikaci. Umožní služby App Service hello ověřování za vás a vynaložit doby na zajištění funkcí, které ve vaší aplikaci.
* [Azure AD Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) umožňuje přístup k datům toodirectory z vaší aplikace Azure.
* Poskytuje jednotné přihlašování příliš[všechny aplikace podporované službou Azure Active Directory](/marketplace/active-directory/), včetně služeb Office 365, Dynamics CRM Online, Microsoft Intune a tisíce jiných společností než Microsoft cloudové aplikace. 
* Azure Active Directory podporuje řízení přístupu na základě rolí. Můžete použít vzor hello [Authorize(Roles="X")] s kódem tooyour minimální změny.

jak zjistit, toowrite-obchodní aplikace Azure, který ověřuje s Azure Active Directory, toosee [vytvoření-obchodní aplikace pro Azure pomocí ověřování Azure Active Directory](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Ověřování pomocí služby tokenů zabezpečení na místě
Pokud máte místní zabezpečené tokenu služby (STS) jako je Active Directory Federation Services (AD FS), můžete pro vaše aplikace Azure, toofederate ověření. Tento přístup je nejvhodnější při ukládají v Azure AD data zakáže zásady společnosti. Pamatujte však hello následující:

* Topologie služby tokenů zabezpečení musí být nasazené na místních počítačích s náklady a správa režijní náklady.
* Můžete nakonfigurovat pouze správci služby AD FS [předávající strany vztahy důvěryhodnosti a pravidla deklarací identity](http://technet.microsoft.com/library/dd807108.aspx), který může omezit možnosti pro vývojáře hello. Na hello druhé straně, je možné toomanage a přizpůsobit [deklarace identity](http://technet.microsoft.com/library/ee913571.aspx) na základě jednotlivých aplikací.
* Přístup k místním tooon data služby AD vyžaduje samostatné řešení přes firewall podnikové hello.

jak zjistit, toowrite-obchodní aplikace Azure, který ověřuje pomocí služby tokenů zabezpečení místní toosee [vytvořit aplikaci Azure-obchodní ověřování službou AD FS](web-sites-dotnet-lob-application-adfs.md).

