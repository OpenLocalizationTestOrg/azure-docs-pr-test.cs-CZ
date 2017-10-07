---
title: 'Azure Active Directory B2C: Konfigurace WeChat | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte WeChat účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d2424c66-ba68-4d82-847e-d137683527b0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 92cc3579d818d2379a503ccc695138b33a34466d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-wechat-accounts"></a>Azure Active Directory B2C: Poskytnout účty WeChat tooconsumers registrace a přihlášení

> [!NOTE]
> Tato funkce je ve verzi preview.
> 

## <a name="create-a-wechat-application"></a>Vytvoření aplikace WeChat

toouse WeChat jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate WeChat aplikace a přiřaďte mu hello správné parametry. Tuto funkci potřebujete toodo WeChat účtu. Pokud nemáte, můžete jej získat registrací prostřednictvím jednoho z jejich mobilních aplikací nebo pomocí své QQ číslo. Potom získáte vašeho účtu zaregistrovat pomocí programu pro vývojáře WeChat hello. Další informace můžete najít [zde](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Registrace aplikace WeChat

1. Přejděte příliš[https://open.weixin.qq.com/](https://open.weixin.qq.com/) a přihlaste se.
2. Klikněte na**管理中心**(management center).
3. Postupujte podle hello potřebné kroky tooregister novou aplikaci.
4. Pro**授权回调域**(zpětného volání URL), zadejte `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Například pokud vaše `tenant_name` je contoso.onmicrosoft.com, toobe adresu URL sady hello `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Najít a zkopírujte hello **ID aplikace** a **klíč aplikace**. Budete potřebovat později.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Konfigurace WeChat jako zprostředkovatele identity ve vašem klientovi
1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity hello. Zadejte například "WeChat".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **WeChat**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity**
7. Zadejte hello **klíč aplikace** který jste zkopírovali dříve jako hello **ID klienta**.
8. Zadejte hello **tajný klíč aplikace** který jste zkopírovali dříve jako hello **tajný klíč klienta**.
9. Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave WeChat konfiguraci.

