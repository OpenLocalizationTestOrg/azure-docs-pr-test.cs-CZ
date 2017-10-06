---
title: 'Azure Active Directory B2C: Konfigurace Weibo | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte Weibo účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1860de34-94cb-4ceb-851e-102f930f7230
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: c0648620f318046c1d9d24feb92a0c5f19c6a91a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-weibo-accounts"></a>Azure Active Directory B2C: Poskytnout účty Weibo tooconsumers registrace a přihlášení

> [!NOTE]
> Tato funkce je ve verzi preview. Nepoužívejte tohoto zprostředkovatele identity v provozním prostředí.
> 

## <a name="create-a-weibo-application"></a>Vytvoření aplikace Weibo

toouse Weibo jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate Weibo aplikace a přiřaďte mu hello správné parametry. Tuto funkci potřebujete toodo Weibo účtu. Pokud nemáte, můžete jej získat v [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).

### <a name="register-for-hello-weibo-developer-program"></a>Registrace pro programu pro vývojáře Weibo hello

1. Přejděte toohello [portál pro vývojáře Weibo](http://open.weibo.com/) a přihlaste se pomocí přihlašovacích údajů účtu Weibo.
2. Po přihlášení, klikněte na název zobrazení v pravém horním rohu hello.
3. V rozevírací nabídce hello, vyberte**编辑开发者信息**(Upravit informace pro vývojáře).
4. Zadejte do formuláře hello hello požadované informace a klikněte na tlačítko**提交**(Odeslat).
5. Dokončení procesu ověření e-mailu hello.
6. Přejděte toohello [stránky ověření identit](http://open.weibo.com/developers/identity/edit).
7. Zadejte do formuláře hello hello požadované informace a klikněte na tlačítko**提交**(Odeslat).

### <a name="register-a-weibo-application"></a>Registrace aplikace Weibo

1. Přejděte toohello [novou stránku registrace aplikace Weibo](http://open.weibo.com/apps/new).
2. Zadejte informace potřebné aplikace hello.
3. Klikněte na tlačítko**创建**(vytvořit).
4. Zkopírujte hodnoty hello **klíč aplikace** a **tajný klíč aplikace**. Budete potřebovat později.
5. Nahrání hello požadované fotografie a zadejte nezbytné informace hello.
6. Klikněte na tlačítko**保存以上信息**(Uložit).
7. Klikněte na tlačítko**高级信息**(rozšířené informace).
8. Klikněte na tlačítko**编辑**(Upravit) další toohello pole pro OAuth2.0**授权设置**(přesměrování URL adresy).
9. Zadejte `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` pro OAuth2.0**授权设置**(přesměrování URL adresy). Například pokud vaše `tenant_name` je contoso.onmicrosoft.com, toobe adresu URL sady hello `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Klikněte na tlačítko**提交**(Odeslat).  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a>Konfigurace Weibo jako zprostředkovatele identity ve vašem klientovi
1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity hello. Zadejte například "Weibo".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Weibo**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity**
7. Zadejte hello **klíč aplikace** který jste zkopírovali dříve jako hello **ID klienta**.
8. Zadejte hello **tajný klíč aplikace** který jste zkopírovali dříve jako hello **tajný klíč klienta**.
9. Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave Weibo konfiguraci.

