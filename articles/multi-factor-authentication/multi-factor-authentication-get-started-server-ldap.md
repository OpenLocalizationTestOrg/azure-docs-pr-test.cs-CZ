---
title: "aaaLDAP ověřování a Azure MFA serveru | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, která vám pomůže při nasazení ověření LDAP a Server Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: e1a68568-53d1-4365-9e41-50925ad00869
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: 17a26b57dbf6afa2fcfdb3d19c5b5ba2987a9f79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>Ověření služby LDAP a Server Azure Multi-Factor Authentication
Ve výchozím nastavení hello Azure Multi-Factor Authentication Server je nakonfigurovaný tooimport nebo synchronizaci uživatelů ze služby Active Directory. Však může být nakonfigurované toobind adresáře LDAP toodifferent, například adresář ADAM nebo konkrétní řadič domény služby Active Directory. Když připojené tooa adresáři prostřednictvím protokolu LDAP, hello Azure Multi-Factor Authentication Server může fungovat jako ověřování tooperform proxy serveru LDAP. Umožňuje také pro hello použití vazby protokolu LDAP jako cíl RADIUS pro předběžné ověření uživatele pomocí ověřování služby IIS, nebo pro primární ověřování hello Azure MFA uživatelského portálu.

toouse Azure Multi-Factor Authentication jako proxy server LDAP, vložte hello Azure Multi-Factor Authentication Server mezi klientem LDAP hello (například zařízení VPN, aplikace) a adresářový server LDAP hello. Hello Azure Multi-Factor Authentication Server musí být nakonfigurované toocommunicate s hello klientské servery a adresářem LDAP hello. V této konfiguraci hello Server Azure Multi-Factor Authentication přijímá požadavky LDAP od klientských serverů a aplikací a předává je toohello cíl LDAP directory server toovalidate hello primární pověření. Pokud adresář LDAP hello ověří hello primární pověření, Azure Multi-Factor Authentication provede druhé ověření identity a odešle odpověď zpět toohello klienta LDAP. celý proces ověřování Hello úspěšné pouze v případě, že ověřování serveru LDAP hello i hello druhý krok ověření úspěšné.

## <a name="configure-ldap-authentication"></a>Konfigurace ověřování pomocí protokolu LDAP
tooconfigure ověřování LDAP, hello instalace Azure Multi-Factor Authentication Server v systému Windows server. Hello použijte následující postup:

### <a name="add-an-ldap-client"></a>Přidání klienta protokolu LDAP

1. V hello Azure Multi-Factor Authentication Server vyberte ikonu hello ověřování pomocí protokolu LDAP v levé nabídce hello.
2. Zkontrolujte hello **povolit ověřování pomocí protokolu LDAP** zaškrtávací políčko.

   ![Ověřování pomocí protokolu LDAP](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)

3. Na kartě klienti hello změňte hello TCP port a SSL port, pokud hello služba LDAP Azure Multi-Factor Authentication by měl vytvořit vazbu toolisten toonon standardní porty požadavků protokolu LDAP.
4. Pokud máte v plánu toouse LDAPS z klienta toohello hello Azure Multi-Factor Authentication Server, certifikát SSL musí být nainstalovaný na hello stejný server jako MFA Server. Klikněte na tlačítko **Procházet** další toohello SSL certifikát pole a vyberte certifikát toouse pro zabezpečené připojení hello.
5. Klikněte na tlačítko **Přidat**.
6. V dialogové okno Přidat klienta LDAP hello zadejte IP adresu hello hello zařízení, server nebo aplikaci, která ověří toohello serveru a název aplikace (volitelné). název aplikace Hello se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci zpráv SMS nebo mobilních aplikací ověřování.
7. Zkontrolujte hello **shodu uživatele vyžadují ověřování Azure Multi-Factor Authentication** pole Pokud všichni uživatelé byli nebo budou importovány do hello serveru a subjektu tootwo krok ověření. Pokud velký počet uživatelů dosud nebyl importován do hello serveru a/nebo se vyloučí z dvoustupňové ověřování, nechte hello políčko nezaškrtnuté. Najdete v souboru nápovědy MFA Server hello Další informace o této funkci.

Opakujte tyto kroky tooadd dalších klientů LDAP.

### <a name="configure-hello-ldap-directory-connection"></a>Konfigurace připojení adresáře LDAP hello

Při hello Azure Multi-Factor Authentication je nakonfigurované tooreceive ověřování LDAP, musí směrovat proxy těchto adresář LDAP toohello ověřování. Proto karta cíl hello se zobrazí pouze jedinou šedě možnost toouse cíle LDAP.

1. tooconfigure hello připojení adresáře LDAP, klikněte na tlačítko hello **integrace adresáře** ikonu.
2. Na kartě nastavení hello, vyberte hello **použít specifickou konfiguraci LDAP** přepínač.
3. Vyberte **Upravit...**
4. V dialogové okno Upravit konfiguraci LDAP hello vyplňte pole hello s adresářem LDAP toohello tooconnect informace požadované hello. Popisy polí hello jsou zahrnuty v souboru nápovědy serveru Azure Multi-Factor Authentication Server hello.

    ![Integrace adresáře](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)

5. Otestujte připojení LDAP hello kliknutím hello **Test** tlačítko.
6. Pokud byl test připojení LDAP hello úspěšný, klikněte na tlačítko hello **OK** tlačítko.
7. Klikněte na tlačítko hello **filtry** kartě hello Server je předem nakonfigurovaný tooload kontejnerů, skupin zabezpečení a uživatelů ze služby Active Directory. Pokud provádíte navázání tooa jiný adresář LDAP, bude pravděpodobně nutné zobrazené filtry tooedit hello. Klikněte na tlačítko hello **pomoci** odkaz na další informace o filtrech.
8. Klikněte na tlačítko hello **atributy** kartě hello Server je předem nakonfigurovaný toomap atributů ze služby Active Directory.
9. Pokud jste vazbu tooa jiný adresář nebo toochange hello předem nakonfigurovaná mapování atributů protokolu LDAP, klikněte na tlačítko **upravit...**
10. V dialogovém okně upravit atributy hello upravte hello mapování atributů LDAP pro váš adresář. Názvy atributů lze zadat nebo vybrat kliknutím hello **...** tlačítko Další tooeach pole. Klikněte na tlačítko hello **pomoci** odkazu pro další informace o atributech.
11. Klikněte na tlačítko hello **OK** tlačítko.
12. Klikněte na tlačítko hello **nastavení společnosti** ikonu a vyberte hello **překlad uživatelského jména** kartě.
13. Pokud se připojujete tooActive Directory ze serveru připojený k doméně, nechte hello **identifikátory zabezpečení používání systému Windows (SID) pro porovnávání uživatelských jmen** vybraný přepínač. V opačném případě vyberte hello **atribut jedinečného identifikátoru LDAP používá pro porovnávání uživatelských jmen** přepínač. 

Když hello **atribut jedinečného identifikátoru LDAP používá pro porovnávání uživatelských jmen** přepínače, hello Azure Multi-Factor Authentication Server pokusí tooresolve každé uživatelské jméno tooa jedinečný identifikátor v adresáři LDAP hello. Vyhledávání protokolem LDAP se provádí na hello uživatelského jména definovaných v hello integrace adresáře atributů -> karta atributy. Při ověření uživatele, uživatelské jméno hello je vyřešen toohello jedinečný identifikátor v adresáři LDAP hello. Jedinečný identifikátor Hello se používá pro odpovídající hello uživatele v datovém souboru Azure Multi-Factor Authentication hello. To umožňuje porovnávání s rozlišování velikosti písmen a dlouhé a krátké formáty uživatelských jmen.

Po dokončení těchto kroků sleduje MFA Server hello na hello nakonfigurované portech pro požadavky na přístup protokolu LDAP v hello konfigurovaných klientů a jednání jako proxy pro ty požadavky toohello adresáře LDAP pro ověřování.

## <a name="configure-ldap-client"></a>Konfigurace klienta LDAP
tooconfigure hello klienta LDAP, použijte pokyny hello:

* Nakonfigurujte zařízení, serveru nebo v aplikaci tooauthenticate prostřednictvím toohello LDAP Azure Multi-Factor Authentication Server, jako kdyby šlo o váš adresář LDAP. Použití hello stejné nastavení, které by normálně používat tooconnect přímo adresář tooyour LDAP, s výjimkou hello název serveru nebo IP adresu, která budou hello Azure Multi-Factor Authentication Server.
* Konfigurace hello LDAP časový limit too30 – 60 sekund, aby byl čas toovalidate hello pověření uživatele v adresáři LDAP hello, druhý krok ověření hello, obdržení odpovědi a reagovat toohello žádost o přístup protokolu LDAP.
* V případě použití LDAPS hello zařízení nebo server provádějící dotazů protokolu LDAP hello musí důvěřovat certifikátu SSL hello nainstalovaném na hello Azure Multi-Factor Authentication Server.

