---
title: "aaaRADIUS ověřování a Azure MFA serveru | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, která vám pomůže při nasazení ověření RADIUS a Server Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f4ba0fb2-2be9-477e-9bea-04c7340c8bce
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: dac061b83f2657c67192a7aa9c5de63ffeffaaa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>Integrace ověření služby RADIUS se serverem Azure Multi-Factor Authentication
Použijte hello ověřování pomocí protokolu RADIUS část tooenable Azure MFA serveru a konfigurovat ověřování RADIUS. RADIUS je standardní protokol tooaccept žádosti o ověření a tooprocess tyto požadavky. Hello Azure Multi-Factor Authentication Server funguje jako RADIUS server. Vložte jej mezi vašeho klienta protokolu RADIUS (zařízení VPN) a cíl ověřování, které by mohly být služby Active Directory (AD), adresář LDAP nebo jiný tooadd serveru RADIUS Azure Multi-Factor Authentication. Pro toofunction Azure Multi-Factor Authentication (MFA) musíte nakonfigurovat hello Azure MFA serveru, aby mohl komunikovat se servery klienta hello i cíl ověřování hello. Hello Azure MFA serveru přijímá požadavky od klienta RADIUS, ověřuje pověření proti hello cíl ověřování, přidává ověřování Azure Multi-Factor Authentication a odešle odpověď zpět toohello klienta RADIUS. žádosti o ověření Hello pouze úspěšné, pokud primární ověřování hello i hello Azure Multi-Factor Authentication úspěšné.

> [!NOTE]
> Hello MFA Server podporuje pouze PAP (protokol ověřování hesla) a MSCHAPv2 (protokol ověřování Challenge Handshake společnosti Microsoft) RADIUS protokoly když funguje jako RADIUS server.  Jiné protokoly, jako je protokol EAP (extensible authentication protocol), můžete použít při hello MFA server funguje jako server RADIUS tooanother proxy server RADIUS, který podporuje tento protokol.
>
> V této konfiguraci není jednosměrné tokeny SMS a OATH fungovat, protože hello MFA serveru nelze zahájit úspěšné odpovědi RADIUS výzvu pomocí alternativní protokolů.

![Ověřování Radius](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="add-a-radius-client"></a>Přidání klienta protokolu RADIUS
ověřování RADIUS tooconfigure, hello instalace Azure Multi-Factor Authentication Server v systému Windows server. Pokud máte prostředí služby Active Directory, by měl být hello server toohello připojené k doméně uvnitř sítě hello. Použijte následující postup tooconfigure hello Azure Multi-Factor Authentication Server hello:

1. V hello Azure Multi-Factor Authentication Server klikněte na ikonu hello ověřování pomocí protokolu RADIUS v levé nabídce hello.
2. Zkontrolujte hello **ověřování RADIUS povolit** zaškrtávací políčko.
3. Na kartě klienti hello změňte hello ověřování a porty monitorování účtů, pokud hello služba Azure MFA RADIUS potřebuje toolisten pro požadavky protokolu RADIUS na nestandardní porty.
4. Klikněte na tlačítko **Přidat**.
5. Zadejte IP adresu hello hello zařízení/serveru, které se budou ověřovat toohello Azure Multi-Factor Authentication Server, název aplikace (volitelné) a sdílený tajný klíč.

  název aplikace Hello se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci zpráv SMS nebo mobilních aplikací ověřování.

  Hello sdílený tajný potřebám toobe hello stejné u obou hello Azure Multi-Factor Authentication Server a zařízení/serveru.

6. Zkontrolujte hello **Vícefaktorové ověřování vyžadovat shodu uživatele** pole, pokud byli nebo budou importovány do hello serveru a ověřování subjektu toomulti všichni uživatelé. Pokud velký počet uživatelů dosud nebyl importován do hello serveru a/nebo budou vyloučeni z dvoustupňové ověřování, nechte hello políčko nezaškrtnuté.
7. Zkontrolujte hello **povolit záložní token OATH** pole, chcete-li hesla OATH toouse z ověření mobilní aplikace jako záložní toohello out-of-band telefonní hovor, SMS, nebo nabízená oznámení.
8. Klikněte na **OK**.

Opakujte kroky 4 až 8 tooadd libovolný počet dalších klientů RADIUS podle potřeby.

## <a name="configure-your-radius-client"></a>Konfigurace klienta protokolu RADIUS

1. Klikněte na tlačítko hello **cíl** kartě.
2. Pokud hello Azure MFA serveru je nainstalovaná na serveru připojeném k doméně v prostředí služby Active Directory, vyberte doménu systému Windows.
3. Pokud musí být uživatelé ověřováni proti adresáři LDAP, vyberte **Vázání protokolu LDAP**.

  toouse vazby protokolu LDAP, klikněte na ikonu integrace adresáře hello a upravit konfiguraci LDAP hello na kartě nastavení hello tak, aby hello Server mohl vytvořit vazbu tooyour adresáře. Pokyny ke konfiguraci LDAP naleznete v hello [Průvodci konfigurací proxy serveru LDAP](multi-factor-authentication-get-started-server-ldap.md).

4. Pokud mají být uživatelé ověřování proti jinému serveru RADIUS, vyberte servery RADIUS.
5. Klikněte na tlačítko **přidat** požadavky tooconfigure hello serveru toowhich hello Azure MFA serveru bude proxy hello protokolu RADIUS.
6. V dialogové okno Přidat Server RADIUS hello zadejte IP adresu hello hello serveru RADIUS a sdílený tajný klíč.

  Hello sdílené tajné potřebám toobe hello stejné u obou hello Azure Multi-Factor Authentication Server a RADIUS server. Změňte port ověřování hello a port monitorování účtů, pokud hello RADIUS server používají jiné porty.

7. Klikněte na **OK**.
8. Přidejte hello Azure MFA serveru jako klienta protokolu RADIUS v hello jiný server RADIUS, aby mohl zpracovávat požadavky na přístup odeslané tooit z hello Azure MFA serveru. Použijte stejný sdílený tajný klíč konfigurovaný na hello Azure Multi-Factor Authentication Server hello.

Opakujte tyto kroky tooadd další servery RADIUS a konfigurovat hello pořadí, ve které hello Azure MFA serveru by měly volat je s hello **nahoru** a **přesunout dolů** tlačítka.

Dokončení konfigurace serveru Azure Multi-Factor Authentication Server hello. Hello Server nyní naslouchá požadavkům přístupu protokolu RADIUS od klientů hello nakonfigurované na hello nakonfigurované porty.   

## <a name="radius-client-configuration"></a>Konfigurace klienta protokolu RADIUS
tooconfigure hello klienta RADIUS, použijte pokyny hello:

* Nakonfigurujte vaše zařízení/serveru tooauthenticate prostřednictvím toohello Azure Multi-Factor Authentication serveru RADIUS IP adresu, která bude fungovat jako hello RADIUS server.
* Použijte stejný sdílený tajný klíč, který byl dříve nakonfigurován hello.
* Konfigurace hello RADIUS časový limit too30 – 60 sekund, aby byl čas toovalidate hello přihlašovacích údajů uživatele, provádět dvoustupňové ověřování, obdržení odpovědi a pak odpověď toohello žádost o přístup protokolu RADIUS.
