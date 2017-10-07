---
title: "přihlášení aaaAzure MFA s dvoustupňové ověřování | Microsoft Docs"
description: "Tato stránka poskytuje pokyny na kde toogo toosee hello různé metody přihlašování k dispozici s Azure MFA."
keywords: "ověřování uživatelů, přihlášení, přihlaste se pomocí mobilního telefonu, přihlaste se pomocí telefonní číslo do kanceláře"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: fcd5eb5e8426eda537db9e099bf247bde29c195b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-sign-in-experience-with-azure-multi-factor-authentication"></a>Hello přihlašování uživatelů s Azure Multi-Factor Authentication
> [!NOTE]
> účelem Hello tohoto článku je toowalk prostřednictvím typických možností přihlašování. Pomoc s přihlášením nebo tootroubleshoot problémy, najdete v tématu [potíže s Azure Multi-Factor Authentication s](multi-factor-authentication-end-user-troubleshoot.md).

## <a name="what-will-your-sign-in-experience-be"></a>Co bude vaše přihlášení?
Vaše přihlášení se liší v závislosti na volbách toouse jako druhý faktor: telefonní hovor, ověřování aplikace nebo texty. Vyberte možnost hello, která nejlépe vystihuje co dělají:

| Jak se můžete přihlásit? | 
| --- |
| [Po telefonu volání toomy mobilní nebo telefonní číslo do kanceláře](#signing-in-with-a-phone-call) |
| [S mobilním telefonem toomy textu](#signing-in-with-a-text-message)
| [S oznámení z aplikace Microsoft Authenticator hello](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [S ověřovací kódy z aplikace Microsoft Authenticator hello](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [S alternativní metodu protože Moje preferovanou metodu nelze použít nyní](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a>Přihlášení pomocí telefonního hovoru
Následující informace Hello popisuje hello dvoustupňové ověření zkušenosti s mobilním tooyour volání nebo telefonní číslo do kanceláře.

1. Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla.  
2. Microsoft zavolá.  
3. Odpovězte hello phone a stiskněte klávesu # hello.  

## <a name="signing-in-with-a-text-message"></a>Přihlášení pomocí textové zprávy
Následující informace Hello popisuje hello dvoustupňové ověření zkušenosti s text zprávy tooyour mobilního telefonu:

1. Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla. 
2. Microsoft vám pošle textovou zprávu obsahující číslo kód. 
3. Zadejte kód hello v poli hello na přihlašovací stránku hello. 

## <a name="signing-in-with-hello-microsoft-authenticator-app"></a>Přihlášení pomocí aplikace Microsoft Authenticator hello 
Hello následující informace popisují činnost hello pomocí aplikace Microsoft Authenticator hello pro dvoustupňové ověření. Existují dva různé způsoby toouse hello aplikace. Může přijímat nabízená oznámení na vašem zařízení, nebo můžete otevřít tooget aplikace hello ověřovací kód.

### <a name="toosign-in-with-a-notification-from-hello-microsoft-authenticator-app"></a>toosign pomocí oznámení z aplikace Microsoft Authenticator hello
1. Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla.
2. Microsoft odesílá aplikaci Microsoft Authenticator toohello oznámení na svém zařízení.

  ![Microsoft odesílá oznámení](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Otevřete hello oznámení na vaše telefonní a vyberte hello **ověřte** klíč. Pokud vaše společnost vyžaduje PIN, zadejte ho sem.
4. Můžete by měl nyní přihlášeni.

### <a name="toosign-in-using-a-verification-code-with-hello-microsoft-authenticator-app"></a>toosign v aplikaci Microsoft Authenticator hello pomocí ověřovací kód

Pokud používáte hello Microsoft Authenticator aplikace tooget ověřovací kódy, pak při otevření aplikace hello zobrazí číslo pod názvem svého účtu. Toto číslo změní každých 30 sekund, tak, že nepoužíváte hello stejné číslo dvakrát. Po zobrazení dotazu pro ověřovací kód, otevřete aplikaci hello a použít je omezený na aktuálně zobrazený. 

1. Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla.
2. Microsoft vás vyzve k zadání ověřovací kód.

  ![Zadejte ověřovací kód](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Otevřete aplikaci Microsoft Authenticator hello na váš telefon a zadejte kód hello hello pole, kde se přihlašujete.

## <a name="signing-in-with-an-alternate-method"></a>Přihlášení pomocí alternativní metoda
Někdy nemáte hello telefonu nebo zařízení, které jste nastavili jako způsob upřednostňované ověření. Tato situace je proto doporučujeme, abyste nastavili metody zálohování pro váš účet. Hello následující části se dozvíte, jak toosign pomocí alternativní metodu, když primární metodu nemusí být k dispozici.

1. Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla.
2. Vyberte **použít jinou možností ověření**. Zobrazí možnosti různých ověření založené na tom, kolik má instalační program.
3. Zvolte alternativní metodu a přihlaste se.

  ![Použití alternativní metody](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a>Další kroky

Pokud máte potíže s přihlášením k dvoustupňové ověření, získat další informace v [potíže s Azure Multi-Factor Authentication s](multi-factor-authentication-end-user-troubleshoot.md).

Zjistěte, jak příliš[spravovat nastavení dvoustupňového ověřování](multi-factor-authentication-end-user-manage-settings.md).

Zjistěte, jak příliš[začít pracovat s aplikací Microsoft Authenticator hello](microsoft-authenticator-app-how-to.md) tak, aby můžete toosign oznámení, namísto texty a telefonních hovorů. 
