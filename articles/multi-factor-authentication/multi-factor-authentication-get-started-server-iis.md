---
title: "aaaIIS ověřování a Azure MFA serveru | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, která vám pomůže při nasazení ověření IIS a serveru Azure Multi-Factor Authentication Server."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d1bf1c8a-2c10-4ae6-9f4b-75f0c3df43eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017,it-pro
ms.openlocfilehash: 74bd39c2644e2bca0880baea3824cad4c9215111
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-for-iis-web-apps"></a>Konfigurace serveru Azure Multi-Factor Authentication pro webové aplikace IIS

Použijte hello ověřování služby IIS část tooenable hello serveru Azure Multi-Factor Authentication (MFA) a konfigurovat ověřování služby IIS pro integraci s webovými aplikacemi Microsoft IIS. Hello Azure MFA Server nainstaluje modul plug-in, který dokáže filtrovat požadavky prováděné toohello IIS webový server tooadd Azure Multi-Factor Authentication. Hello modul plug-in služby IIS poskytuje podporu pro ověřování pomocí formuláře a integrované HTTP ověřování systému Windows. Důvěryhodné že IP adresy může být také nakonfigurované tooexempt vnitřních IP adres z dvoufaktorového ověřování.

![Ověřování IIS](./media/multi-factor-authentication-get-started-server-iis/iis.png)

## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Ověřování založené na formulářích služby IIS pomocí serveru Azure Multi-Factor Authentication
toosecure službu IIS webovou aplikaci, která používá ověřování založené na formulářích, nainstalujte hello Server Azure Multi-Factor Authentication na webový server IIS hello a nakonfigurujte Server hello za hello následující postup:

1. V hello Azure Multi-Factor Authentication Server klikněte na ikonu ověřování IIS hello v levé nabídce hello.
2. Klikněte na tlačítko hello **založené na formulářích** kartě.
3. Klikněte na tlačítko **Přidat**.
4. uživatelské jméno, heslo a doménu proměnné toodetect automaticky, zadejte hello adresu URL pro přihlášení (např. https://localhost/contoso/auth/login.aspx) v rámci dialogové okno automaticky konfigurovat formulářový web hello a klikněte na **OK**.
5. Zkontrolujte hello **Vícefaktorové ověřování vyžadovat shodu uživatele** pole, pokud byli nebo budou importovány do hello serveru a ověřování subjektu toomulti všichni uživatelé. Pokud velký počet uživatelů dosud nebyl importován do hello serveru a/nebo budou vyloučeni z Multi-Factor authentication, nechejte pole hello nezaškrtnuté.
6. Pokud hello proměnné stránky nelze rozpoznat automaticky, klikněte na tlačítko **zadat ručně** v dialogovém okně automaticky konfigurovat formulářový web hello.
7. V dialogovém okně Přidat web hello zadejte v poli Adresa URL pro odeslání hello hello URL toohello přihlašovací stránku a zadejte název aplikace (volitelné). název aplikace Hello se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci zpráv SMS nebo mobilních aplikací ověřování.
8. Vyberte správný formát požadavku hello. To je nastaven příliš**Metoda POST nebo GET** u většiny webových aplikací.
9. Zadejte proměnnou uživatelského jména hello proměnnou hesla a proměnnou domény (Pokud se zobrazí na přihlašovací stránku hello). názvy hello toofind hello vstupních polí, přejděte toohello přihlašovací stránku ve webovém prohlížeči, kliknout pravým tlačítkem na stránku hello a vyberte **zobrazit zdroj**.
10. Zkontrolujte hello **shodu uživatele vyžadují ověřování Azure Multi-Factor Authentication** pole, pokud byli nebo budou importovány do hello serveru a ověřování subjektu toomulti všichni uživatelé. Pokud velký počet uživatelů dosud nebyl importován do hello serveru a/nebo budou vyloučeni z Multi-Factor authentication, nechejte pole hello nezaškrtnuté.
11. Klikněte na tlačítko **Upřesnit** tooreview upřesňující nastavení, včetně:

  - Výběr vlastního souboru odmítnutí stránky
  - Web toohello úspěšných ověření mezipaměti dobu používání souborů cookie
  - Vyberte, zda tooauthenticate hello primární pověření vůči doméně systému Windows, adresáři LDAP. nebo serveru RADIUS.

12. Klikněte na tlačítko **OK** dialogové okno Přidat web toohello tooreturn.
13. Klikněte na **OK**.
14. Jednou hello adresy URL a zjištění nebo zadání proměnné stránky, hello web data se zobrazí v hello panely na základě formulářů.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Používání integrovaného ověřování systému Windows pomocí serveru Azure Multi-Factor Authentication Server
toosecure službu IIS webové aplikace, která používá integrované HTTP ověřování systému Windows, nainstalujte na webový server IIS hello hello Azure MFA serveru a potom nakonfigurujte hello serveru s hello následující kroky:

1. V hello Azure Multi-Factor Authentication Server klikněte na ikonu ověřování IIS hello v levé nabídce hello.
2. Klikněte na tlačítko hello **HTTP** kartě.
3. Klikněte na tlačítko **Přidat**.
4. V dialogové okno Přidat základní adresu URL hello zadejte adresu URL hello hello web, kde se provádí ověřování HTTP (např. http://localhost/owa) a zadejte název aplikace (volitelné). název aplikace Hello se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci zpráv SMS nebo mobilních aplikací ověřování.
5. Hello časový limit nečinnosti a maximální dobu trvání relace v případě potřeby upravte výchozí hello není dostatečná.
6. Zkontrolujte hello **Vícefaktorové ověřování vyžadovat shodu uživatele** pole, pokud byli nebo budou importovány do hello serveru a ověřování subjektu toomulti všichni uživatelé. Pokud velký počet uživatelů dosud nebyl importován do hello serveru a/nebo budou vyloučeni z Multi-Factor authentication, nechejte pole hello nezaškrtnuté.
7. Zkontrolujte hello **mezipaměti souborů Cookie** pole v případě potřeby.
8. Klikněte na **OK**.

## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Povolit moduly plug-in služby IIS a server Azure Multi-Factor Authentication
Po dokončení konfigurace hello založené na formulářích nebo adres URL protokolu HTTP ověřování a nastavení, vyberte hello místo, kde by měl hello moduly plug-in Azure Multi-Factor Authentication IIS načteny a povolena ve službě IIS. Hello použijte následující postup:

1. Pokud používáte službu IIS 6, klikněte na tlačítko hello **ISAPI** kartě. Vyberte hello web, který hello webová aplikace běží pod (například výchozí web) tooenable hello Azure Multi-Factor Authentication filtr ISAPI modulu plug-in pro danou lokalitu.
2. Pokud systém ve službě IIS 7 nebo vyšší, klikněte na tlačítko hello **nativní modul** kartě. Vyberte hello server, weby nebo aplikace tooenable hello modul plug-in IIS na úrovních hello potřeby.
3. Klikněte na tlačítko hello **povolit IIS ověřování** pole hello horní části úvodní obrazovka. Azure Multi-Factor Authentication teď zabezpečuje hello vybrané aplikace služby IIS. Zajistěte, aby byli uživatelé naimportováni do hello serveru.

## <a name="trusted-ips"></a>Důvěryhodné IP adresy
Hello důvěryhodné IP adresy umožňují uživatelům toobypass Azure Multi-Factor Authentication u požadavků webů pocházejících z konkrétní IP adresy nebo podsítě. Například můžete tooexempt uživatele z ověřování Azure Multi-Factor Authentication při přihlašování z kanceláře hello. V takovém případě zadáte podsíť office hello jako položku Důvěryhodné adresy IP. tooconfigure důvěryhodné IP adresy, použijte následující postup hello:

1. V hello část ověření služby IIS, klikněte na tlačítko hello **důvěryhodné IP adresy** kartě.
2. Klikněte na tlačítko **Přidat**.
3. Jakmile se zobrazí dialogové okno Přidat důvěryhodné IP adresy hello, vyberte hello **jednu IP adresu**, **rozsah IP adres**, nebo **podsíť** přepínač.
4. Zadejte hello IP adresu, rozsah IP adres nebo podsíť, která by měla být seznam povolených adres. Pokud zadáte podsíť, vyberte hello příslušnou síťovou masku a klikněte na tlačítko **OK**. Hello seznam povolených adres byl přidán.
