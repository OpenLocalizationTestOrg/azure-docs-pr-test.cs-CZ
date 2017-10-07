---
title: aaaUse Azure MFA serveru s AD FS 2.0 | Microsoft Docs
description: "Toto je stránka Azure Multi-Factor authentication hello, který popisuje, jak tooget pracovat s Azure MFA a AD FS 2.0."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 96168849-241a-4499-a224-d829913caa7e
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: 15011a94ca69dc6e51aa3edf74f5db6cd44d697a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-toowork-with-ad-fs-20"></a>Konfigurace serveru Azure Multi-Factor Authentication Server toowork se službou AD FS 2.0
Tento článek je pro organizace, které jsou sdružených se službou Azure Active Directory a chcete toosecure prostředky, které jsou místní nebo v cloudu hello. Chránit prostředky pomocí hello Azure Multi-Factor Authentication Server a nastavit ho toowork se službou AD FS, aby se pro koncové body vysoké hodnoty spouštělo dvoustupňové ověření.

Tato dokumentace popisuje používání hello Azure Multi-Factor Authentication Server se službou AD FS 2.0. Další informace o AD FS najdete v tématu [Zabezpečení cloudových a místních prostředků pomocí Azure Multi-Factor Authentication Serveru s Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md).

## <a name="secure-ad-fs-20-with-a-proxy"></a>Zabezpečení AD FS 2.0 pomocí serveru proxy
toosecure služby AD FS 2.0 pomocí serveru proxy, nainstalujte hello Azure Multi-Factor Authentication Server na hello AD FS proxy serveru.

### <a name="configure-iis-authentication"></a>Konfigurace ověřování IIS
1. V hello Azure Multi-Factor Authentication Server, klikněte na tlačítko hello **ověřování služby IIS** ikonu v levé nabídce hello.
2. Klikněte na tlačítko hello **založené na formulářích** kartě.
3. Klikněte na tlačítko **Přidat**.

   <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>

4. uživatelské jméno, heslo a doménu proměnné toodetect automaticky, zadejte hello adresu URL pro přihlášení (např. https://sso.contoso.com/adfs/ls) v rámci dialogového okno hello automaticky konfigurovat formulářový web a klikněte na **OK**.
5. Zkontrolujte hello **shodu uživatele vyžadují ověřování Azure Multi-Factor Authentication** pole Pokud všichni uživatelé byli nebo budou importovány do hello serveru a subjektu tootwo krok ověření. Pokud velký počet uživatelů dosud nebyl importován do hello serveru a/nebo budou vyloučeni z dvoustupňové ověřování, nechte hello políčko nezaškrtnuté.
6. Pokud hello proměnné stránky nelze rozpoznat automaticky, klikněte na tlačítko hello **zadat ručně...** tlačítka v dialogovém okně automaticky konfigurovat formulářový web hello.
7. V dialogovém okně Přidat web hello do pole Adresa URL pro odeslání hello (např. https://sso.contoso.com/adfs/ls) zadejte hello URL toohello AD FS přihlašovací stránku a zadejte název aplikace (volitelné). název aplikace Hello se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci zpráv SMS nebo mobilních aplikací ověřování.
8. Nastavte formát požadavku hello příliš**Metoda POST nebo GET**.
9. Zadejte hello proměnné uživatelského jména (ctl00$ ContentPlaceHolder1$ UsernameTextBox) a proměnnou hesla (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Pokud vaše formulářová přihlašovací stránka zobrazí pole pro doménu, zadejte taky proměnnou domény hello. názvy hello toofind hello vstupních polí na přihlašovací stránku hello, přejděte toohello přihlašovací stránku ve webovém prohlížeči, kliknout pravým tlačítkem na stránku hello a vyberte **zobrazit zdroj**.
10. Zkontrolujte hello **shodu uživatele vyžadují ověřování Azure Multi-Factor Authentication** pole Pokud všichni uživatelé byli nebo budou importovány do hello serveru a subjektu tootwo krok ověření. Pokud velký počet uživatelů dosud nebyl importován do hello serveru a/nebo budou vyloučeni z dvoustupňové ověřování, nechte hello políčko nezaškrtnuté.
    <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klikněte na **Upřesnit...** tooreview upřesňující nastavení. Mezi nastavení, která můžete konfigurovat, patří:

    - Výběr vlastního souboru odmítnutí stránky
    - Mezipaměť úspěšných ověření toohello webu pomocí souborů cookie
    - Vyberte, jak tooauthenticate hello primární pověření

12. Vzhledem k tomu, že hello AD FS proxy serveru není pravděpodobné toobe připojila k doméně toohello, můžete použít řadič domény tooyour tooconnect LDAP pro předběžné ověření a import uživatelů. V dialogovém okně Upřesnit webové hello, klikněte na tlačítko hello **primární ověřování** a vyberte **LDAP Bind** pro hello typ ověření předběžného ověření.
13. Po dokončení klikněte na tlačítko **OK** dialogové okno Přidat web toohello tooreturn.
14. Klikněte na tlačítko **OK** dialogové okno tooclose hello.
15. Jednou hello adresy URL a zjištění nebo zadání proměnné stránky, hello web data se zobrazí v hello panely na základě formulářů.
16. Klikněte na tlačítko hello **nativní modul** kartě a vyberte server hello hello web, který proxy hello AD FS běží pod (například "výchozí web"), nebo hello služby AD FS proxy aplikace (např. "ls" v "adfs") tooenable hello v hello modul plug-in služby IIS požadované úrovni.
17. Klikněte na tlačítko hello **povolit IIS ověřování** pole hello horní části úvodní obrazovka.

ověřování služby IIS Hello je teď povolené.

### <a name="configure-directory-integration"></a>Konfigurace integrace adresáře
Povolit ověřování pomocí služby IIS, ale tooperform hello předběžné ověření tooyour Active Directory (AD) prostřednictvím protokolu LDAP musíte nakonfigurovat hello řadič domény toohello připojení LDAP.

1. Klikněte na tlačítko hello **integrace adresáře** ikonu.
2. Na kartě nastavení hello, vyberte hello **použít specifickou konfiguraci LDAP** přepínač.

   <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>

3. Klikněte na **Upravit**.
4. V dialogové okno Upravit konfiguraci LDAP hello vyplňte pole hello s řadičem domény hello informace požadované tooconnect toohello AD. Popisy polí hello jsou zahrnuty v souboru nápovědy serveru Azure Multi-Factor Authentication Server hello.
5. Otestujte připojení LDAP hello kliknutím hello **Test** tlačítko.

   <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>

6. Pokud byl test připojení LDAP hello úspěšný, klikněte na tlačítko **OK**.

### <a name="configure-company-settings"></a>Konfigurace nastavení společnosti
1. Klikněte na tlačítko hello **nastavení společnosti** ikonu a vyberte hello **překlad uživatelského jména** kartě.
2. Vyberte hello **atribut jedinečného identifikátoru LDAP používá pro porovnávání uživatelských jmen** přepínač.
3. Pokud uživatelé zadat svoje uživatelské jméno ve formátu "doména\uživatelské jméno", musí hello Server při vytváření dotazu LDAP hello toobe možné toostrip hello domény vypnout hello uživatelské jméno. To můžete udělat nastavením registrů.
4. Otevřete editor registru hello a přejděte tooHKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/Positive sítě/PhoneFactor na 64bitovém serveru. Pokud na 32bitovém serveru trvat hello "Wow6432Node" z cesty hello. Vytvoření typu DWORD klíč registru názvem "UsernameCxz_stripPrefixDomain" a nastavte hodnotu too1 hello. Azure Multi-Factor Authentication teď zabezpečuje proxy hello služby AD FS.

Zajistěte, aby se uživatelé naimportovali z Active Directory do hello serveru. V tématu hello [část důvěryhodné IP adresy](#trusted-ips) Pokud byste chtěli toowhitelist interních IP adres, aby při přihlášení toohello webu z těchto umístění není vyžadován dvoustupňové ověřování.

<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 Direct bez serveru proxy
Pokud se nepoužívá hello AD FS proxy můžete zabezpečit služby AD FS. Nainstalujte hello Azure Multi-Factor Authentication Server na server hello služby AD FS a nakonfigurujte hello Server podle hello následující kroky:

1. V rámci hello Azure Multi-Factor Authentication Server, klikněte na hello **ověřování služby IIS** ikonu v levé nabídce hello.
2. Klikněte na tlačítko hello **HTTP** kartě.
3. Klikněte na tlačítko **Přidat**.
4. V dialogové okno Přidat základní adresu URL hello zadejte adresu URL hello hello webu služby AD FS, kde se provádí ověřování HTTP (např. https://sso.domain.com/adfs/ls/auth/integrated) do pole Základní adresa URL hello. Potom zadejte název aplikace (volitelné). název aplikace Hello se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci zpráv SMS nebo mobilních aplikací ověřování.
5. V případě potřeby upravte hello časový limit nečinnosti a maximální dobu trvání relace.
6. Zkontrolujte hello **shodu uživatele vyžadují ověřování Azure Multi-Factor Authentication** pole Pokud všichni uživatelé byli nebo budou importovány do hello serveru a subjektu tootwo krok ověření. Pokud velký počet uživatelů dosud nebyl importován do hello serveru a/nebo budou vyloučeni z dvoustupňové ověřování, nechte hello políčko nezaškrtnuté.
7. V případě potřeby zaškrtněte políčko mezipaměti souborů cookie hello.

   <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>

8. Klikněte na **OK**.
9. Klikněte na tlačítko hello **nativní modul** kartě a vyberte hello serveru, webu hello (jako "výchozí web") nebo hello služby AD FS aplikace (např. "ls" v "adfs") tooenable hello v hello modul plug-in služby IIS potřeby úroveň.
10. Klikněte na tlačítko hello **povolit IIS ověřování** pole hello horní části úvodní obrazovka.

Azure Multi-Factor Authentication teď zabezpečuje službu AD FS.

Zajistěte, aby se uživatelé naimportovali z Active Directory do hello serveru. V tématu hello část důvěryhodné IP adresy, pokud chcete toowhitelist interních IP adres, aby při přihlášení toohello webu z těchto umístění není vyžadován dvoustupňové ověřování.

## <a name="trusted-ips"></a>Důvěryhodné IP adresy
Důvěryhodné IP adresy umožňují uživatelům toobypass Azure Multi-Factor Authentication u požadavků webů pocházejících z konkrétní IP adresy nebo podsítě. Můžete například tooexempt uživatele z dvoustupňové ověření při přihlášení z kanceláře hello. V takovém případě zadáte podsíť office hello jako položku Důvěryhodné adresy IP.

### <a name="tooconfigure-trusted-ips"></a>tooconfigure důvěryhodné IP adresy
1. V hello část ověření služby IIS, klikněte na tlačítko hello **důvěryhodné IP adresy** kartě.
2. Klikněte na tlačítko hello **přidat...** .
3. Jakmile se zobrazí dialogové okno Přidat důvěryhodné IP adresy hello, vyberte jednu z hello **jednu IP adresu**, **rozsah IP adres**, nebo **podsíť** přepínač tlačítka.
4. Zadejte hello IP adresu, rozsah IP adres nebo podsíť, která by měla být seznam povolených adres. Pokud zadáváte podsíť, vyberte hello příslušnou síťovou masku a klikněte na tlačítko hello **OK** tlačítko. Hello důvěryhodných že IP se přidal.

<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
