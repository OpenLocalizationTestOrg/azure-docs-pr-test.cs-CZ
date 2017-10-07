---
title: "Azure Multi-Factor Authentication Server spuštěna aaaGetting | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, který popisuje, jak tooget pracovat s Azure MFA serveru."
services: multi-factor-authentication
keywords: "authentication server,stránka pro aktivaci aplikace azure multi factor authentication,stažení authentication serveru"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: e94120e4-ed77-44b8-84e4-1c5f7e186a6b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 92a6a586eb96375e92a9455ad64e67221001db81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-azure-multi-factor-authentication-server"></a>Začínáme s Azure Multi-Factor Authentication Server hello

<center>![Místní MFA](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Teď, když jsme určili toouse místní aplikace Multi-Factor Authentication Server, můžeme začít. Tato stránka popisuje novou instalaci hello serveru a nastavení se místní službou Active Directory. Pokud jste již nainstalován server MFA hello a hledáte tooupgrade, přečtěte si téma [Upgrade toohello nejnovější Azure Multi-Factor Authentication Server](multi-factor-authentication-server-upgrade.md). Pokud hledáte informace o instalaci právě hello webové služby, přečtěte si téma [hello nasazení Azure Multi-Factor Authentication Server webové služby mobilní aplikace](multi-factor-authentication-get-started-server-webservice.md).

## <a name="plan-your-deployment"></a>Plánování nasazení

Před stažením hello Azure Multi-Factor Authentication Server, rozmyslete si, jaké jsou vaše požadavky na vysokou dostupnost a zatížení. Použít tento toodecide informace, jak a kde toodeploy.

Vhodné pro hello množství paměti, je nutné je hello počet uživatelů, můžete očekávat tooauthenticate v pravidelných intervalech.

| Uživatelé | Paměť RAM |
| ----- | --- |
| 1-10,000 | 4 GB |
| 10,001-50,000 | 8 GB |
| 50,001-100,000 | 12 GB |
| 100,000-200,001 | 16 GB |
| 200,001+ | 32 GB |

Třeba tooset až více serverů pro vysokou dostupnost nebo Vyrovnávání zatížení? Existuje několik způsobů tooset tuto konfiguraci s Azure MFA serverem. Když instalujete první Azure MFA serveru, stane se hlavní server hello. Všechny další servery stane podřízená a automatickou synchronizaci uživatelů a konfigurace se hlavní server hello. Potom můžete nakonfigurovat jeden primární server a mít hello rest fungovat jako zálohování, nebo můžete nastavit vyrovnávání zatížení mezi všechny servery hello.

Pokud hlavní Azure MFA serveru přejde do režimu offline, hello podřízené servery můžete stále zpracovat dvoustupňové ověření žádosti. Však nelze přidat nový uživatelům a stávajícím uživatelům nelze aktualizovat jejich nastavení, dokud hlavní server hello je zpět do online režimu nebo získá povýší podřízenou položkou.

### <a name="prepare-your-environment"></a>Příprava prostředí

Zkontrolujte, zda server hello, který používáte pro Azure Multi-Factor Authentication splňuje hello následující požadavky:

| Požadavky pro Azure Multi-Factor Authentication Server | Popis |
|:--- |:--- |
| Hardware |<li>200 MB volného místa na pevném disku</li><li>Procesor kompatibilní s x32 nebo x64</li><li>1 GB RAM nebo víc</li> |
| Software |<li>Windows Server 2008 nebo vyšší, pokud je hostitel hello OS serveru</li><li>Windows 7 nebo novější, pokud je hostitel hello klientského operačního systému</li><li>Microsoft .NET 4.0 Framework</li><li>Službu IIS 7.0 nebo novější, pokud instalujete hello uživatele portálu nebo webové služby sady SDK</li> |

### <a name="azure-mfa-server-components"></a>Komponenty Azure MFA Serveru

Azure MFA Server se skládá ze tří webových komponent:

* Webová služba SDK – umožňuje komunikaci s hello ostatní součásti a je nainstalován na serveru aplikace hello Azure MFA
* Uživatelský portál - webovou stránku IIS, která umožňuje uživatelům tooenroll v Azure Multi-Factor Authentication (MFA) a spravovat své účty.
* Mobilní aplikace webové služby - umožňuje pomocí mobilní aplikace, jako je aplikace Microsoft Authenticator hello pro dvoustupňové ověření.

Všechny tři součásti bude možné nainstalovat na hello stejný server, pokud je internetový hello server. Pokud rozdělení hello součásti, hello sady SDK webové služby je nainstalovaná na serveru aplikace hello Azure MFA a hello portálu User Portal a webová služba mobilní aplikace jsou nainstalovány na internetovém serveru.

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Požadavky na firewall pro Azure Multi-Factor Authentication Server

Každý server MFA musí být schopný toocommunicate na portu 443 odchozí toohello následující adresy:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Pokud firewall omezuje odchozí na portu 443, otevřete hello následující rozsahy IP adres:

| Podsíť IP | Síťová maska | Rozsah IP adres |
|:---: |:---: |:---: |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1 – 134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1 – 134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129 – 70.37.154.254 |

Pokud nepoužíváte funkce potvrzení události hello a vaši uživatelé nejsou v podnikové síti hello používá tooverify mobilní aplikace ze zařízení, stačí hello následující rozsahy:

| Podsíť IP | Síťová maska | Rozsah IP adres |
|:---: |:---: |:---: |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72 – 134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72 – 134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201 – 70.37.154.206 |

## <a name="download-hello-azure-multi-factor-authentication-server"></a>Stažení Azure Multi-Factor Authentication Server hello

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako správce.
2. Na levé straně hello vyberte **služby Active Directory**
3. Klikněte na **Uživatelé a skupiny**.
4. Klikněte na **Všichni uživatelé**.
5. Klikněte na **Multi-Factor Authentication**.
6. V části týkající se **vícefaktorového ověřování** klikněte na **Nastavení služby**.

   ![Stránka Nastavení služby](./media/multi-factor-authentication-get-started-server/servicesettings.png)

6. Na stránce nastavení služby hello klikněte v hello dolní části obrazovky hello **přejděte toohello portál**. Otevře se nová stránka.
7. Klikněte na **Soubory ke stažení**.
8. Klikněte na tlačítko hello **Stáhnout** propojení a uložte instalační program hello.

   ![Stažení MFA Serveru](./media/multi-factor-authentication-get-started-server/download4.png)

9. Tato stránka nechte otevřený, jako jsme bude odkazovat tooit po spuštěné hello Instalační služby.

## <a name="install-and-configure-hello-azure-multi-factor-authentication-server"></a>Instalace a konfigurace serveru Azure Multi-Factor Authentication Server hello

Teď, když jste si stáhli hello serveru můžete nainstalovat a nakonfigurovat ho. Ujistěte se, že tento server hello, kterou instalujete na splňuje požadavky uvedené v části Plánování hello.

1. Poklikejte na spustitelný soubor hello.
2. Na obrazovce výběr instalační složky hello, zkontrolujte, zda tato složka hello je správný a klikněte na tlačítko **Další**.
3. Po dokončení instalace hello klikněte na tlačítko **Dokončit**.  Spustí se Průvodce konfigurací Hello.
4. Na úvodní konfigurace Průvodce úvodní obrazovka, zkontrolujte **hello přeskočit použití Průvodce konfigurací ověřování** a klikněte na tlačítko **Další**.  Hello průvodce se zavře a spustí hello server.

   ![Cloud](./media/multi-factor-authentication-get-started-server/skip2.png)

5. Zpět na stránce hello, které jsme server hello z stáhli, klikněte na tlačítko hello **generovat přihlašovacích údajů pro aktivaci** tlačítko. Tyto údaje zkopírujte do hello Azure MFA serveru v oknech hello zadaný a klikněte na tlačítko **aktivovat**.

## <a name="send-users-an-email"></a>Zaslání e-mailu uživatelům

zavedení tooease, povolit toocommunicate MFA serveru s uživateli. MFA Server může odeslat tooinform e-mailu je, že jsou zapsaní pro dvoustupňové ověření.

jak nakonfigurovat uživatele pro dvoustupňové ověření je třeba stanovit Hello e-mailu, která posíláte. Například pokud jste možnost tooimport telefonní čísla z adresáře společnosti hello, e-mailu hello by měla obsahovat hello výchozí telefonní čísla tak, aby uživatelé věděli, jaké tooexpect. Pokud neproběhne import telefonní čísla nebo vaši uživatelé budou toouse hello mobilních aplikací, jejich odeslání e-mailu, který směruje toocomplete zápisu jejich účtu. Zahrňte e-mailu hello hyperlink toohello Azure Multi-Factor Authentication User Portal.

Hello obsah e-mailů hello se liší v závislosti na hello metodu ověřování, která byla nastavena pro uživatele hello (telefonní hovor, SMS nebo mobilní aplikace).  Například pokud uživatel hello požadované toouse kódu PIN při ověření, e-mailu hello sdělením, co byla nastavena počáteční kód PIN na.  Uživatelé jsou požadované toochange PIN kód během jejich prvního ověřování.

### <a name="configure-email-and-email-templates"></a>Konfigurace e-mailu a šablon e-mailu

Klikněte na ikonu e-mailu hello na levém tooset hello nastavení hello pro odesílání těchto e-mailů. Tato stránka je, kde můžete zadat informace hello SMTP vašeho e-mailu serveru a odesílání e-mailu kontrolou hello **odesílat e-maily toousers** zaškrtávací políčko.

![Konfigurace e-mailu MFA Serveru](./media/multi-factor-authentication-get-started-server/email1.png)

Na kartě obsah e-mailu hello uvidíte hello e-mailových šablon, které jsou k dispozici toochoose z. V závislosti na tom, jak jste nakonfigurovali vaši uživatelé tooperform dvoustupňové ověření zvolte šablonu hello, který vám nejlépe vyhovuje.

![Šablony e-mailů MFA Serveru](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="import-users-from-active-directory"></a>Import uživatelů ze služby Active Directory

Nyní je nainstalován tento server hello můžete tooadd uživatele. Můžete zvolit toocreate je ručně, import uživatelů ze služby Active Directory nebo nakonfigurovat automatické synchronizace služby Active Directory.

### <a name="manual-import-from-active-directory"></a>Ruční import ze služby Active Directory

1. V hello Azure MFA serveru, na levé straně hello vyberte **uživatelé**.
2. V dolní části hello, vyberte **Import ze služby Active Directory**.
3. Nyní můžete buď hledání pro jednotlivé uživatele nebo vyhledávání hello adresář AD pro organizační jednotky s uživateli v nich.  V takovém případě určíme hello uživatele organizační jednotky.
4. Zvýrazněte všechny uživatele hello hello správné a klikněte na tlačítko **Import**.  Mělo by se zobrazit vyskakovací okno s informací, že akce proběhla úspěšně.  Hello zavřít okno importu.

   ![Import uživatelů do MFA Serveru](./media/multi-factor-authentication-get-started-server/import2.png)

### <a name="automated-synchronization-with-active-directory"></a>Automatizovaná synchronizace se službou Active Directory

1. V hello Azure MFA serveru, na levé straně hello vyberte **integrace adresáře**.
2. Přejděte toohello **synchronizace** kartě.
3. V dolní části hello, zvolte **přidat**
4. V hello **přidat položku synchronizace** zobrazeném dialogovém okně vyberte hello doménu, organizační jednotky **nebo** skupiny zabezpečení, nastavení, výchozí hodnoty metody a výchozí jazyk pro tato synchronizace úloh a klikněte na tlačítko **Přidat**.
5. Zaškrtávací políčko hello s názvem **povolit synchronizaci se službou Active Directory** a vyberte **interval synchronizace** minutu až 24 hodin.

## <a name="how-hello-azure-multi-factor-authentication-server-handles-user-data"></a>Jak hello Azure Multi-Factor Authentication Server nakládá s uživatelskými daty

Použijete-li hello serveru Multi-Factor Authentication (MFA) místně, data uživatele uložena v hello na místní servery. Žádná trvalá data se ukládají v cloudu hello. Když uživatel hello provede dvoustupňové ověření, hello MFA Server odešle data toohello vícefaktorového ověřování Azure cloud service tooperform hello ověření. Pokud tyto požadavky na ověření pošlou toohello Cloudová služba, hello následující pole jsou zasílány v požadavku hello a protokoly, aby byly k dispozici v sestavách používání/ověřování zákazníka hello. Některá pole hello jsou volitelné, takže můžete třeba povolit nebo zakázat v rámci hello aplikace Multi-Factor Authentication Server. Hello komunikace z cloudové služby MFA toohello MFA Server hello používá protokol SSL/TLS přes port 443 odchozí. Tato pole jsou:

* Jedinečné ID - uživatelské jméno nebo interní ID serveru MFA
* Jméno a příjmení (volitelné)
* E-mailová adresa (volitelné)
* Telefonní číslo – při ověření přes telefonní hovor nebo zprávu SMS
* Token zařízení - při ověření přes mobilní aplikaci
* Režim ověřování
* Výsledek ověřování
* Název MFA Serveru
* IP adresa serveru MFA
* IP adresa klienta – pokud je dostupná

Kromě toho toohello pole výše, hello výsledek ověření (úspěch/zamítnuto) a důvod zamítnutí je také uloženou s daty hello ověřování a k dispozici prostřednictvím hello ověřování/používání sestav.

## <a name="back-up-and-restore-azure-mfa-server"></a>Zálohování a obnovení Azure MFA Serveru

A ujistěte se, zda máte správné zálohy je důležitým krokem tootake s jakéhokoli systému.

tooback server Azure MFA, ujistěte se, že máte kopii hello **C:\Program Files\Multi-Factor Authentication Server\Data** složky včetně hello **PhoneFactor.pfdata** souboru. 

V případě a obnovení je potřebné dokončení hello následující kroky:

1. Přeinstalujte Azure MFA Server na nový server.
2. Aktivovat hello nový Server Azure MFA.
3. Zastavení hello **MultiFactorAuth** služby.
4. Přepsat hello **PhoneFactor.pfdata** s hello zálohy kopie.
5. Spustit hello **MultiFactorAuth** služby.

nový server Hello je teď spuštěná s hello původní zálohovaná konfigurace a uživatelská data.

## <a name="next-steps"></a>Další kroky

- Nastavení a konfigurace hello [portálu User Portal](multi-factor-authentication-get-started-portal.md) pro uživatele samoobslužné služby.
- Nastavení a konfigurace hello Azure MFA serveru s [Active Directory Federation Service](multi-factor-authentication-get-started-adfs.md), [ověřování pomocí protokolu RADIUS](multi-factor-authentication-get-started-server-radius.md), nebo [ověřování pomocí protokolu LDAP](multi-factor-authentication-get-started-server-ldap.md).
- Nastavení a konfigurace [Brány vzdálené plochy a Azure Multi-Factor Authentication Serveru používajícího protokol RADIUS](multi-factor-authentication-get-started-server-rdg.md).
- [Nasazení hello Azure Multi-Factor Authentication Server webové služby mobilní aplikace](multi-factor-authentication-get-started-server-webservice.md).
- [Pokročilé scénáře se službou Azure Multi-Factor Authentication a sítěmi VPN třetích stran](multi-factor-authentication-advanced-vpn-configurations.md).
