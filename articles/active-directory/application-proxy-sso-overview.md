---
title: "aaaManage jednotné přihlašování pro proxy aplikace služby Azure AD | Microsoft Docs"
description: "Další informace o hello základy jednotné přihlašování pomocí Proxy aplikace"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a278751a5cb1bf98c970a4e5d2eb3edc3b784096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Jak Azure AD Application Proxy poskytovat jednotné přihlašování?

Jednotné přihlašování je klíčovým prvkem proxy aplikace služby Azure AD.  Poskytuje hello nejlepších výsledků, protože mají vaši uživatelé jenom toosign v tooAzure služby Active Directory v cloudu hello. Po ověření tooAzure služby Active Directory, konektor Proxy aplikace hello zpracovává hello ověřování toohello místní aplikace. back-end aplikace Hello nelze zjistit hello rozdíl mezi vzdáleného uživatele přihlásit pomocí Proxy aplikací a regulární použití na zařízení připojeném k doméně. 

toouse Azure Active Directory pro tooyour přihlášení aplikace, budete potřebovat tooselect **Azure Active Directory** jako metoda předběžného ověřování hello. Pokud vyberete **průchozí** pak uživatelé nejsou vůbec ověření tooAzure služby Active Directory, ale jsou řízené přímých toohello aplikace. Toto nastavení můžete nakonfigurovat při prvním publikování aplikace, nebo přejděte tooyour aplikace hello portál Azure a upravit nastavení Proxy aplikace hello. 

toosee jedné možnosti přihlašování, postupujte takto:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Přejděte příliš**Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace**.
3. Vyberte hello aplikace, jejichž jednotné přihlašování možnosti, můžete chtít toomanage.
4. Vyberte **jednotného přihlašování**.

   ![Jednotné přihlašování rozevírací nabídce](./media/application-proxy-sso-overview/single-sign-on-mode.png)

Hello rozevírací nabídce ukazuje pět možnosti pro aplikaci tooyour přihlašování:

* Azure AD jednotné přihlašování zakázáno
* Založené na heslech přihlášení
* Propojené přihlášení
* Integrované ověřování systému Windows
* Na základě záhlaví přihlášení

## <a name="azure-ad-single-sign-on-disabled"></a>Azure AD jednotné přihlašování zakázáno

Pokud nechcete, aby integrace služby Active Directory Azure toouse pro tooyour přihlašování v aplikaci, vyberte **Azure AD jednotné přihlašování zakázáno**. Tuto možnost mohou uživatelé ověřit dvakrát. Nejprve ověřit tooAzure služby Active Directory a potom se přihlaste toohello vlastní aplikace. 

Tato možnost je vhodná, pokud vaše místní aplikace nevyžaduje tooauthenticate uživatele, ale chcete tooadd Azure Active Directory jako úroveň zabezpečení pro vzdálený přístup. 

## <a name="password-based-sign-on"></a>Založené na heslech přihlášení

Pokud chcete toouse Azure Active Directory jako služba vault heslo pro místní aplikace, vyberte **založené na heslech přihlašování**. Tato možnost je vhodná, pokud vaše aplikace ověří uživatelské jméno a heslo pole se seznamem místo přístupové tokeny nebo hlavičky. S založené na heslech přihlašování uživatelé potřebovat toosign v toohello aplikace hello poprvé přístup. Potom poskytuje Azure Active Directory hello uživatelské jméno a heslo jménem uživatele hello. 

Informace o nastavení založené na heslech přihlašování najdete v tématu [heslo vaulting pro jednotné přihlašování pomocí Proxy aplikace](application-proxy-sso-azure-portal.md).

## <a name="linked-sign-on"></a>Propojené přihlášení

Pokud již máte jeden řešení přihlašování nastavení pro místních identit, zvolte **propojené přihlášení**. Tato možnost umožňuje Azure Active Directory tooleverage existující jednotného přihlašování k řešení, ale stále bude uživatelům aplikace toohello vzdáleného přístupu. 

Informace o propojené přihlášení (dříve označované jako existující jednotné přihlašování) najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Integrované ověřování systému Windows

Pokud vaše místní aplikace používat integrované Authentication(IWA) Windows nebo pokud chcete toouse použitím (KCD Kerberos omezeného delegování) pro jednotné přihlašování, zvolte **integrované ověřování systému Windows**. Tato možnost uživatelé potřebují pouze tooauthenticate tooAzure služby Active Directory a potom konektor Proxy aplikace hello zosobňuje uživatele tooget hello token protokolu Kerberos a přihlaste se toohello aplikace. 

Informace o nastavení integrované ověřování systému Windows najdete v tématu [omezené delegování Kerberos pro jednotné přihlašování pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md).

## <a name="header-based-sign-on"></a>Na základě záhlaví přihlášení 

Pokud vaše aplikace používat hlavičky pro ověřování, zvolte **na základě záhlaví přihlašování**. Tato možnost uživatelé potřebovat pouze tooauthentication hello Azure Active Directory. Partneři společnosti Microsoft službou ověřování třetích stran volá PingAccess, který přeložit hello Azure Active Directory přístupový token na formát hlavičky pro aplikaci hello. 

Informace o nastavení ověřování na základě záhlaví najdete v tématu [ověřování na základě záhlaví pro jednotné přihlašování pomocí Proxy aplikace](application-proxy-ping-access.md).

## <a name="next-steps"></a>Další kroky

- [Heslo vaulting pro jednotné přihlašování pomocí Proxy aplikace](application-proxy-sso-azure-portal.md)
- [Omezené delegování Kerberos pro jednotné přihlašování pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
- [Ověřování na základě záhlaví pro jednotné přihlašování pomocí Proxy aplikace](application-proxy-ping-access.md) 
