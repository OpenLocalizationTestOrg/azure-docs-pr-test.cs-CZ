---
title: "Azure Active Directory B2C: Konfigurace sítě Facebook | Microsoft Docs"
description: "Registrace a přihlášení tooconsumers poskytněte Facebook účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: sromeroz
manager: krassk
editor: sromeroz
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/7/2017
ms.author: sromeroz
ms.openlocfilehash: 4c3b79c7248bd1e789bf33fc467abb27d0170edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-facebook-accounts"></a>Azure Active Directory B2C: Zadejte tooconsumers registrace a přihlášení s účty služby Facebook
## <a name="create-a-facebook-application"></a>Vytvořit aplikaci pro Facebook
toouse Facebook jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate aplikace Facebook a přiřaďte mu hello správné parametry. Tuto funkci potřebujete toodo účet Facebooku. Pokud nemáte, můžete ho na získat [https://www.facebook.com/](https://www.facebook.com/).

1. Přejděte toohello [Facebook pro vývojáře](https://developers.facebook.com/) webu a přihlaste se pomocí vaší sítě Facebook přihlašovací údaje účtu.
2. Pokud jste tak již neučinili, musíte tooregister jako vývojář Facebook. toodo tento, klikněte na tlačítko **zaregistrovat** (na hello pravém horním rohu stránky hello), přijměte zásady pro Facebook a kroky registrace hello.
3. Klikněte na tlačítko **Moje aplikace** a pak klikněte na **přidejte novou aplikaci**. 
4. Ve formuláři hello poskytují **zobrazovaný název** platné **e-mailu kontaktujte**.
5. Klikněte na tlačítko **vytvoření ID aplikace**. To může vyžadovat tooaccept Facebook platformy zásady a dokončit kontrolu zabezpečení online.
6. V levém sloupci hello, klikněte na **nastavení** a pak vyberte **základní** Pokud již není vybrán.
7. Vyberte **kategorie**. 
8. Klikněte na tlačítko **+ přidejte platformu** a vyberte **webu**.
   
    ![Facebook – nastavení](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook – nastavení – Web](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. Zadejte `https://login.microsoftonline.com/` v hello **adresa URL webu** pole a pak klikněte na **uložit změny** na hello dolní části stránky hello.
   
    ![Facebook – adresa URL webu](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. Zkopírujte hodnotu hello **ID aplikace**. Klikněte na tlačítko **zobrazit** a zkopírujte hodnotu hello **tajný klíč aplikace**. Budete potřebovat oba dva tooconfigure Facebook jako zprostředkovatel identity ve vašem klientovi. **Tajný klíč aplikace** je důležitým bezpečnostním pověřením.
   
    ![Facebook - ID aplikace a tajný klíč aplikace](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. Klikněte na tlačítko **+ přidat produkt** hello levé navigaci a potom hello **nastavit až** tlačítko pro **Facebook přihlášení**.
   
    ![Facebook - Facebook přihlášení](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. Klikněte na tlačítko **nastavení** na správné nav hello pod **Facebook přihlášení**

    ![Facebook – nastavení přihlášení Facebook](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **identifikátory URI přesměrování platný OAuth** pole hello **nastavení klienta OAuth** části. Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com). Klikněte na tlačítko **uložit změny** na hello dolní části stránky hello.
    
    ![Facebook – identifikátor URI přesměrování OAuth](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. toomake aplikace Facebook použitelné pomocí Azure AD B2C, musíte toomake ho veřejně dostupné. To provedete kliknutím na **revize aplikace** na hello levé navigační a zapnutí hello přepínač hello horní části stránky hello příliš**Ano** a kliknutím na **potvrdit**.
    
    ![Facebook - veřejné aplikace](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Konfigurovat jako zprostředkovatel identity ve vašem klientovi sítě Facebook
1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity hello. Zadejte například "Facebook".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Facebook**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello app ID a aplikace tajný klíč (hello aplikace Facebook, který jste vytvořili dříve) v hello **ID klienta** a **tajný klíč klienta**polí v uvedeném pořadí.
7. Klikněte na tlačítko **OK**a potom klikněte na **vytvořit** toosave konfiguraci sítě Facebook.

> [!NOTE]
> Přidání **zprostředkovatele Identity** tooyour klienta nedojde ke změně existující zásady. Mějte na paměti tooupdate zásad zahrnutím hello zprostředkovatele identity, který jste právě vytvořili.
>