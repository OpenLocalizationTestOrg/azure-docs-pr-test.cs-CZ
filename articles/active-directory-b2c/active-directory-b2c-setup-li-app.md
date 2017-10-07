---
title: 'Azure Active Directory B2C: Konfigurace LinkedIn | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytnout LinkedIn účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: fa51a16b-9ce9-4e27-9eff-0869b4c4f0ef
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6c5233ef48b24968fd6383f470b5d8a969a78ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Poskytnout účty LinkedIn tooconsumers registrace a přihlášení
## <a name="create-a-linkedin-application"></a>Vytvoření aplikace LinkedIn
toouse LinkedIn jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate LinkedIn aplikace a přiřaďte mu hello správné parametry. Tuto funkci potřebujete účet toodo LinkedIn. Pokud nemáte, můžete ho na získat [https://www.linkedin.com/](https://www.linkedin.com/).

1. Přejděte toohello [LinkedIn vývojáři webu](https://www.developer.linkedin.com/) a přihlaste se pomocí přihlašovacích údajů účtu LinkedIn.
2. Klikněte na tlačítko **Moje aplikace** v hello horní nabídce a potom klikněte na **vytvořit aplikaci**.
   
    ![LinkedIn - nové aplikace](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. V hello **vytvořte novou aplikaci** formuláři, zadejte příslušné informace hello (**název společnosti**, **název**, **popis**, **Adresa URL aplikace Logo**, **využívání aplikací**, **adresu URL webu**, **e-mailová adresa** a **Telefon do zaměstnání**).
4. Souhlasím toohello **LinkedIn API podmínky použití** a klikněte na tlačítko **odeslání**.
   
    ![LinkedIn – registrace aplikace](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Zkopírujte hodnoty hello **ID klienta** a **tajný klíč klienta**. (Je najdete v části **ověřovací klíče**.) Budete potřebovat oba dva tooconfigure LinkedIn jako zprostředkovatel identity ve vašem klientovi.
   
   > [!NOTE]
   > **Tajný klíč klienta** je důležitým bezpečnostním pověřením.
   > 
   > 
6. Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **oprávnění adres URL pro přesměrování** pole (v části **OAuth 2.0**). Nahraďte **{klient}** s názvem vašeho klienta (například contoso.onmicrosoft.com). Klikněte na tlačítko **přidat**a potom klikněte na **aktualizace**. Hello **{klient}** hodnota je malá a velká písmena.
   
    ![LinkedIn – instalační program aplikace](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Konfigurace LinkedIn jako zprostředkovatele identity ve vašem klientovi
1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity hello. Zadejte například "LI".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **LinkedIn**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello ID a klienta tajný klíč klienta z hello LinkedIn aplikace, kterou jste vytvořili dříve.
7. Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave konfiguraci LinkedIn.

