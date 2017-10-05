---
title: "Ověření pomocí místní služby Active Directory v aplikaci Azure | Microsoft Docs"
description: "Další informace o různých možností pro-obchodní aplikace ve službě Azure App Service k ověřování místní služby Active Directory"
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
ms.openlocfilehash: a68bcd7040498515a6e35a87ee6e6940a84506d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Ověření pomocí místní služby Active Directory v aplikaci Azure
V tomto článku se dozvíte, jak ověřit pomocí místní služby Active Directory (AD) v [Azure App Service](../app-service/app-service-value-prop-what-is.md). Aplikace Azure je hostovaná v cloudu, ale existují způsoby, jak ověřit místní uživatele AD bezpečně. 

## <a name="authenticate-through-azure-active-directory"></a>Ověřování pomocí služby Azure Active Directory
Klient služby Azure Active Directory může být directory synchronizované s místní AD. Tento přístup umožňuje AD uživatelům přístup k vaší aplikaci z Internetu a provést ověření pomocí jejich místních přihlašovacích údajů. Kromě toho poskytuje Azure App Service [řešení na klíč pro tuto metodu](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Pomocí několika kliknutí tlačítko můžete povolit ověřování pomocí klienta directory synchronizovat pro vaše aplikace Azure. Tento přístup má následující výhody:

* Nevyžaduje žádné ověřovací kód ve vaší aplikaci. Umožní služby App Service se ověřování za vás a vynaložit doby na zajištění funkcí, které ve vaší aplikaci.
* [Azure AD Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) umožňuje přístup k datům adresáře z vaší aplikace Azure.
* Poskytuje jednotné přihlašování k [všechny aplikace podporované službou Azure Active Directory](/marketplace/active-directory/), včetně služeb Office 365, Dynamics CRM Online, Microsoft Intune a tisíce jiných společností než Microsoft cloudové aplikace. 
* Azure Active Directory podporuje řízení přístupu na základě rolí. Můžete vzoru [Authorize(Roles="X")] s minimálními změnami kódu.

Jak napsat-obchodní aplikace Azure, který ověřuje s Azure Active Directory najdete v sekci [vytvoření-obchodní aplikace pro Azure pomocí ověřování Azure Active Directory](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Ověřování pomocí služby tokenů zabezpečení na místě
Pokud máte místní zabezpečené tokenu služby (STS) jako je Active Directory Federation Services (AD FS), který můžete použít pro vytvoření federace ověřování pro aplikaci Azure. Tento přístup je nejvhodnější při ukládají v Azure AD data zakáže zásady společnosti. Ale pamatujte na následující:

* Topologie služby tokenů zabezpečení musí být nasazené na místních počítačích s náklady a správa režijní náklady.
* Můžete nakonfigurovat pouze správci služby AD FS [předávající strany vztahy důvěryhodnosti a pravidla deklarací identity](http://technet.microsoft.com/library/dd807108.aspx), který může omezit možnosti pro vývojáře. Na druhé straně je možné spravovat a přizpůsobit [deklarace identity](http://technet.microsoft.com/library/ee913571.aspx) na základě jednotlivých aplikací.
* Přístup k místnímu data služby AD vyžaduje samostatné řešení prostřednictvím podnikové brány firewall.

Jak napsat-obchodní aplikace Azure, který ověřuje pomocí služby tokenů zabezpečení místní najdete v sekci [vytvořit aplikaci Azure-obchodní ověřování službou AD FS](web-sites-dotnet-lob-application-adfs.md).

