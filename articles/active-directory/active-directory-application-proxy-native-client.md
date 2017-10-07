---
title: "nativní klientské aplikace aaaPublish – Azure AD | Microsoft Docs"
description: "Popisuje, jak tooenable nativního klienta aplikace toocommunicate s konektoru Proxy aplikace služby Azure AD tooprovide zabezpečený vzdálený přístup tooyour místní aplikace."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 0ed2be217bf992f034d8321d5e66569b4cace24f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-native-client-apps-toointeract-with-proxy-applications"></a>Jak tooenable nativního klienta aplikace toointeract s proxy aplikace

V aplikacích tooweb přidání Proxy Azure Active Directory aplikace lze také použít toopublish nativní klientské aplikace. Nativní klientské aplikace se liší od webové aplikace, protože se instalují na zařízení, když webové aplikace jsou dostupné prostřednictvím prohlížeče. 

Proxy aplikací podporuje nativní klient aplikace přijímá Azure AD, které jsou vystavené tokeny, které se odesílají v hlavičky standardních autorizovat HTTP.

![Vztah mezi koncovým uživatelům, Azure Active Directory a publikování aplikace](./media/active-directory-application-proxy-native-client/richclientflow.png)

Použijte hello knihovna ověřování Azure AD, který se stará o ověřování a podporuje mnoho klienta prostředí, toopublish nativních aplikací. Proxy aplikací zapadá do hello [nativní aplikace tooWeb rozhraní API scénář](develop/active-directory-authentication-scenarios.md#native-application-to-web-api). Tento článek vás provede hello čtyři kroky toopublish nativní aplikace s Proxy aplikace a hello knihovna ověřování Azure AD. 

## <a name="step-1-publish-your-application"></a>Krok 1: Publikování aplikace
Publikování aplikace proxy, stejně jako všechny ostatní aplikace a přiřaďte tooaccess uživatelů vaší aplikace. Další informace najdete v tématu [publikování aplikací pomocí Proxy aplikace](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Krok 2: Konfigurace aplikace
Nativní aplikace nakonfigurujte následujícím způsobem:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Přejděte příliš**Azure Active Directory** > **registrace aplikace**.
3. Vyberte **nové registrace aplikace**.
4. Zadejte název pro vaši aplikaci, vyberte **nativní** jako typ aplikace hello a zadejte hello identifikátor URI pro přesměrování pro aplikaci. 

   ![Vytvořit novou registraci aplikace](./media/active-directory-application-proxy-native-client/create.png)
5. Vyberte **Vytvořit**.

Podrobné informace o vytváření nové registrace aplikace, najdete v části [integrace aplikací s Azure Active Directory](.//develop/active-directory-integrating-applications.md).


## <a name="step-3-grant-access-tooother-applications"></a>Krok 3: Udělení přístupu tooother aplikace
Povolte hello nativní aplikace toobe zveřejněné tooother aplikace ve vašem adresáři:

1. Pořád ještě v **registrace aplikace**, vyberte hello nové nativní aplikaci, kterou jste právě vytvořili.
2. Vyberte **požadovaná oprávnění**.
3. Vyberte **Přidat**.
4. Prvním krokem otevřete hello, **vybrat rozhraní API**.
5. Pomocí hello vyhledávání panelu toofind hello aplikace Proxy aplikace, která jste publikovali v první části hello. Vyberte aplikaci a pak klikněte na **vyberte**. 

   ![Vyhledejte hello proxy aplikace](./media/active-directory-application-proxy-native-client/select_api.png)
6. Otevřete hello druhý krok, **vyberte oprávnění**.
7. Použijte nativní aplikace přístup tooyour proxy aplikace hello toogrant zaškrtávací políčko a potom klikněte na tlačítko **vyberte**.

   ![Udělení přístupu tooproxy aplikace](./media/active-directory-application-proxy-native-client/select_perms.png)
8. Vyberte **provádí**.


## <a name="step-4-edit-hello-active-directory-authentication-library"></a>Krok 4: Úprava hello knihovna ověřování Active Directory
Upravte kód nativní aplikace hello v kontextu ověřování hello hello Active Directory Authentication Library (ADAL) tooinclude hello následující text:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of hello Native app>",
        new Uri("<Redirect Uri of hello Native App>"),
        PromptBehavior.Never);

//Use hello Access Token tooaccess hello Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

Hello proměnné v hello ukázkový kód by měla být nahrazená následujícím způsobem:

* **ID klienta** lze nalézt v hello portálu Azure. Přejděte příliš**Azure Active Directory** > **vlastnosti** a kopírování hello ID adresáře. 
* **Externí adresu URL** hello front-end adresa URL, jste zadali v hello Proxy aplikace. toofind, tato hodnota, přejděte toohello **proxy aplikace** části hello proxy aplikace.
* **ID aplikace** z hello nativní aplikace naleznete na hello **vlastnosti** stránku hello nativní aplikaci.
* **Identifikátor URI přesměrování nativní aplikace hello** naleznete na hello **identifikátory URI přesměrování** stránku hello nativní aplikaci.


## <a name="see-also"></a>Viz také

Další informace o toku nativní aplikace hello najdete v tématu [tooweb nativní aplikace API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)

Hello nejnovější novinky a aktualizace, najdete na naší hello [blogu Proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)
