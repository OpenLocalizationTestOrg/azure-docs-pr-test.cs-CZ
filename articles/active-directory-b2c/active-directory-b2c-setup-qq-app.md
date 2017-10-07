---
title: 'Azure Active Directory B2C: Konfigurace QQ | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte QQ účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 18c2cf94-8004-4de1-81c2-e45be65ce12d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 896d6221e01d15de1652a5717cf1f65619101e0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-qq-accounts"></a>Azure Active Directory B2C: Poskytnout účty QQ tooconsumers registrace a přihlášení

> [!NOTE]
> Tato funkce je ve verzi preview.
> 

## <a name="create-a-qq-application"></a>Vytvoření aplikace QQ

toouse QQ jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate QQ aplikace a přiřaďte mu hello správné parametry. Tuto funkci potřebujete toodo QQ účtu. Pokud nemáte, můžete jej získat v [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-hello-qq-developer-program"></a>Registrace pro programu pro vývojáře QQ hello

1. Přejděte toohello [portál pro vývojáře QQ](http://open.qq.com) a přihlaste se pomocí přihlašovacích údajů účtu QQ.
2. Po přihlášení, přejděte příliš[http://open.qq.com/reg](http://open.qq.com/reg) tooregister sami jako vývojář.
3. V nabídce hello vyberte**个人**(jednotlivé vývojáře).
4. Zadejte do formuláře hello hello požadované informace a klikněte na tlačítko**下一步**(Další krok).
5. Dokončení procesu ověření e-mailu hello.

> [!NOTE]
> Je nutné toowait několik dní toobe schválení po registraci jako vývojář. 

### <a name="register-a-qq-application"></a>Registrace aplikace QQ

1. Přejděte příliš[https://connect.qq.com/index.html](https://connect.qq.com/index.html).
2. Klikněte na**应用管理**(Správa aplikací).
3. Klikněte na**创建应用**(vytvořit aplikaci).
4. Zadejte informace potřebné aplikace hello.
5. Klikněte na**创建应用**(vytvořit aplikaci).
6. Zadejte informace požadované hello.
7. Pro hello**授权回调域**(zpětného volání URL) zadejte `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Například pokud vaše `tenant_name` je contoso.onmicrosoft.com, toobe adresu URL sady hello `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
8. Klikněte na**创建应用**(vytvořit aplikaci).
9. Na stránce pro potvrzení hello, klikněte na**应用管理**stránce management (Správa aplikací) tooreturn toohello aplikace.
10. Klikněte na**查看**(zobrazení) další toohello aplikaci jste právě vytvořili.
11. Klikněte na**修改**(Upravit).
12. Z hello horní části stránky hello, zkopírujte hello **ID aplikace** a **klíč aplikace**.

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a>Konfigurace QQ jako zprostředkovatele identity ve vašem klientovi
1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity hello. Zadejte například "QQ".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **QQ**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity**
7. Zadejte hello **klíč aplikace** který jste zkopírovali dříve jako hello **ID klienta**.
8. Zadejte hello **tajný klíč aplikace** který jste zkopírovali dříve jako hello **tajný klíč klienta**.
9. Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave QQ konfiguraci.

