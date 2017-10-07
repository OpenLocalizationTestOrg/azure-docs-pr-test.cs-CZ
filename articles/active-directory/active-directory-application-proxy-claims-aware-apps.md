---
title: "aplikace využívající technologii aaaClaims - Proxy aplikace Azure AD | Microsoft Docs"
description: "Jak toopublish místní aplikace ASP.NET, které přijímat deklarace identity služby AD FS pro vaši uživatelé zabezpečený vzdálený přístup."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 91e6211b-fe6a-42c6-bdb3-1fff0312db15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 7be633225de700226c7c94815eb91b3de2b61cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Práce s deklaracemi identity aplikace v Proxy aplikace
[Deklaracemi identity aplikace](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) provádět přesměrování toohello tokenu služby zabezpečení (STS). Služba tokenů zabezpečení Hello požadavky přihlašovací údaje od uživatele hello za token a pak přesměruje hello uživatelská toohello aplikace. Existuje několik způsobů tooenable Proxy aplikace toowork s tyto přesměrování. Použijte tento článek tooconfigure nasazení pro deklaracemi identity aplikace. 

## <a name="prerequisites"></a>Požadavky
Ujistěte se, že tento hello služby tokenů zabezpečení, která hello deklaracemi identity aplikace přesměruje toois dostupné v místní síti. Můžete zpřístupnit hello STS vystavení prostřednictvím proxy serveru nebo tím, že se mimo připojení. 

## <a name="publish-your-application"></a>Publikování aplikace

1. Publikování aplikace podle toohello pokynů v tématu [publikování aplikací pomocí Proxy aplikace](application-proxy-publish-azure-portal.md).
2. Přejděte toohello stránka aplikace v portálu a vyberte možnost hello **jednotného přihlašování**.
3. Pokud jste zvolili **Azure Active Directory** jako vaše **metoda předběžného ověření**, vyberte **Azure AD jednotné přihlašování zakázáno** jako vaše **interní metodu ověřování**. Pokud jste zvolili **průchozí** jako vaše **metoda předběžného ověření**, nepotřebujete toochange cokoli.

## <a name="configure-adfs"></a>Konfigurace služby AD FS

Služba AD FS můžete nakonfigurovat pro deklaracemi identity aplikace v jednom ze dvou způsobů. Hello je nejdřív pomocí vlastní domény. Hello druhou je s WS-Federation. 

### <a name="option-1-custom-domains"></a>Možnost 1: Vlastní domény

Pokud všechny hello interní adresy URL pro vaše aplikace jsou plně kvalifikované názvy domény (FQDN), pak můžete nakonfigurovat [vlastní domény](active-directory-application-proxy-custom-domains.md) pro vaše aplikace. Použití hello vlastní domény toocreate externí adresy URL, které jsou hello stejná hodnota jako hello interní adresy URL. Při vašem externí adresy URL odpovídat vaše interní adresy URL, přesměrování služby tokenů zabezpečení hello fungovat, zda jsou vaši uživatelé místní nebo vzdálené. 

### <a name="option-2-ws-federation"></a>Možnost 2: WS-Federation

1. Otevřete modul Správa služby AD FS.
2. Přejděte příliš**vztahy důvěryhodnosti předávající strany**, klikněte pravým tlačítkem na hello aplikace publikujete pomocí Proxy aplikace a zvolte **vlastnosti**.  

   ![Vztahy důvěryhodnosti předávající strany, klikněte pravým tlačítkem na název aplikace – snímek obrazovky](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. Na hello **koncové body** v části **typ koncového bodu**, vyberte **WS-Federation**.
4. V části **důvěryhodné adresy URL**, zadejte adresu URL hello jste zadali v hello Proxy aplikace pod **externí adresu URL** a klikněte na tlačítko **OK**.  

   ![Přidání koncového bodu - nastavit důvěryhodné adresy URL hodnotu – snímek obrazovky](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Další kroky
* [Povolit jednotné přihlašování na](application-proxy-sso-overview.md) pro aplikace, které nejsou pracujícím s deklaracemi
* [Povolení nativního klienta aplikace toointeract s proxy aplikace](active-directory-application-proxy-native-client.md)


