---
title: "Azure MFA přihlášení s dvoustupňové ověřování | Microsoft Docs"
description: "Tato stránka poskytuje pokyny na koho se obracet, pokud chcete zobrazit různé přihlášení dostupné metody s Azure MFA."
keywords: "ověřování uživatelů, přihlášení, přihlaste se pomocí mobilního telefonu, přihlaste se pomocí telefonní číslo do kanceláře"
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: c47356b7b84e38a1db9259304c2a975958b1977c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Přihlašování uživatelů s Azure Multi-Factor Authentication
> [!NOTE]
> Účelem tohoto článku je provede typických možností přihlašování. Pomoc s přihlášením nebo k řešení problémů najdete v tématu [potíže s Azure Multi-Factor Authentication s](multi-factor-authentication-end-user-troubleshoot.md).

## <a name="what-will-your-sign-in-experience-be"></a>Co bude vaše přihlášení?
Vaše přihlášení se liší v závislosti na tom, co chcete použít jako druhý faktor: telefonní hovor, ověřování aplikace nebo texty. Zvolte možnost, která nejlépe popisuje, co dělají:

| Jak se můžete přihlásit? |
| --- |
| [Pomocí telefonního hovoru na telefon mobilní telefon nebo office](#signing-in-with-a-phone-call) |
| [S textem na můj mobilní telefon](#signing-in-with-a-text-message)
| [S oznámení z aplikace Microsoft Authenticator](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [S ověřovací kódy z aplikace Microsoft Authenticator](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [S alternativní metodu protože Moje preferovanou metodu nelze použít nyní](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a>Přihlášení pomocí telefonního hovoru
Následující informace popisuje činnost dvoustupňové ověření pomocí volání do telefonu mobilní telefon nebo office.

1. Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.  
2. Microsoft zavolá.  
3. Přijměte hovor a stiskem tlačítka #.  

## <a name="signing-in-with-a-text-message"></a>Přihlášení pomocí textové zprávy
Následující informace popisuje činnost dvoustupňové ověření pomocí textové zprávy na váš mobilní telefon:

1. Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.
2. Microsoft vám pošle textovou zprávu obsahující číslo kód.
3. V poli na stránce přihlášení zadejte kód.

## <a name="signing-in-with-the-microsoft-authenticator-app"></a>Přihlášení pomocí aplikace Microsoft Authenticator
Následující informace popisují možností použití aplikace Microsoft Authenticator pro dvoustupňové ověření. Existují dva různé způsoby použití aplikace. Může přijímat nabízená oznámení na vašem zařízení, nebo můžete otevřít aplikaci získat ověřovací kód.

### <a name="to-sign-in-with-a-notification-from-the-microsoft-authenticator-app"></a>Přihlásit se přes oznámení z aplikace Microsoft Authenticator
1. Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.
2. Microsoft odešle oznámení do aplikace Microsoft Authenticator na vašem zařízení.

  ![Microsoft odesílá oznámení](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Otevřete oznámení na vaše telefonní a vyberte **ověřte** klíč. Pokud vaše společnost vyžaduje PIN, zadejte ho sem.
4. Můžete by měl nyní přihlášeni.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>K přihlášení pomocí ověřovací kód v aplikaci Microsoft Authenticator

Pokud používáte aplikaci Microsoft Authenticator získat ověřovací kódy, pak při otevření aplikace uvidíte číslo pod názvem svého účtu. Toto číslo změní každých 30 sekund, tak, aby nepoužívejte stejné číslo dvakrát. Pokud jste žádali ověřovací kód, otevřete aplikaci a používat je omezený na aktuálně zobrazený.

1. Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.
2. Microsoft vás vyzve k zadání ověřovací kód.

  ![Zadejte ověřovací kód](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Otevřete aplikaci Microsoft Authenticator na váš telefon a zadejte kód v poli, kde se přihlašujete.

## <a name="signing-in-with-an-alternate-method"></a>Přihlášení pomocí alternativní metoda
Někdy nemáte telefonu nebo zařízení, které jste nastavili jako způsob upřednostňované ověření. Tato situace je proto doporučujeme, abyste nastavili metody zálohování pro váš účet. V následující části se dozvíte, jak se přihlásit pomocí alternativní metodu, když primární metodu nemusí být k dispozici.

1. Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.
2. Vyberte **použít jinou možností ověření**. Zobrazí možnosti různých ověření založené na tom, kolik má instalační program.
3. Zvolte alternativní metodu a přihlaste se.

  ![Použití alternativní metody](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a>Další kroky

Pokud máte potíže s přihlášením k dvoustupňové ověření, získat další informace v [potíže s Azure Multi-Factor Authentication s](multi-factor-authentication-end-user-troubleshoot.md).

Zjistěte, jak [spravovat nastavení dvoustupňového ověřování](multi-factor-authentication-end-user-manage-settings.md).

Zjistěte, jak [začít pracovat s aplikací Microsoft Authenticator](microsoft-authenticator-app-how-to.md) tak, aby oznámení můžete použít k přihlášení, namísto texty a telefonních hovorů.
