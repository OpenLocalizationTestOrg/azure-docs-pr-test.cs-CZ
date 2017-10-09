---
title: "připojení SSL aaaConfigure v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Pokyny a informace o tooconfigure databáze Azure pro PostgreSQL a přidružené aplikace tooproperly pomocí připojení SSL."
services: postgresql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql
ms.custom: 
ms.topic: article
ms.date: 05/15/2017
ms.openlocfilehash: 96a68088acd924196701e8d618d9d5edf44cb548
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>Konfigurace připojení SSL ve službě Azure Database pro PostgreSQL
Azure databázi PostgreSQL upřednostní připojení vaší klientské aplikace toohello PostgreSQL služby pomocí Secure Sockets Layer (SSL). Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed hello" šifrování hello datový proud mezi hello serveru a aplikace.

Ve výchozím nastavení je hello služba databáze PostgreSQL nakonfigurované toorequire připojení SSL. Volitelně můžete zakázat, budete požadovat protokol SSL pro připojení služby tooyour databáze, pokud klientská aplikace nepodporuje připojení SSL. 

## <a name="enforcing-ssl-connections"></a>Vynucení připojení SSL
Pro všechny databáze Azure pro servery PostgreSQL, které jsou zřízené prostřednictvím hello portál Azure a rozhraní příkazového řádku je ve výchozím nastavení povolené vynucení připojení SSL. 

Připojovací řetězce, které jsou předem definované v nastavení "Připojovací řetězce" hello v rámci vašeho serveru v hello portál Azure, zahrnují hello požadované parametry pro běžné jazyky tooconnect tooyour databáze serveru pomocí protokolu SSL. Hello parametr SSL se liší podle hello konektor, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.

## <a name="configure-enforcement-of-ssl"></a>Konfigurace vynucení protokolu SSL
Volitelně můžete zakázat vynucení připojení SSL. Microsoft Azure se doporučuje povolit tooalways **připojení SSL vynutit** nastavení pro lepší zabezpečení.

### <a name="using-hello-azure-portal"></a>Pomocí hello portálu Azure
Navštivte vaší databázi Azure pro PostgreSQL server a klikněte na tlačítko **zabezpečení připojení**. Použít hello přepínací tlačítko tooenable nebo zakázat hello **připojení SSL vynutit** nastavení. Potom klikněte na **Uložit**. 

![Zabezpečení připojení - zakázat vynutit SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Hello nastavení si můžete ověřit zobrazením hello **přehled** stránku hello toosee **SSL vynutit stav** indikátoru.

### <a name="using-azure-cli"></a>Použití Azure CLI
Můžete povolit nebo zakázat hello **ssl vynucení** pomocí parametru `Enabled` nebo `Disabled` hodnot v uvedeném pořadí v rozhraní příkazového řádku Azure.

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Ujistěte se, vaše aplikace nebo framework podporuje připojení SSL
Mnoho běžné architektury aplikace, které používají PostgreSQL pro databázi služby, jako je například Drupal a Django, nepovolujte SSL ve výchozím nastavení během instalace. Povolení připojení SSL je třeba provést po instalaci, nebo pomocí rozhraní příkazového řádku příkazy toohello konkrétní aplikací. Pokud váš server PostgreSQL vynucuje připojení SSL a hello přidružené aplikace není správně nakonfigurována, hello aplikace mohou selhat tooconnect tooyour databázový server. Jak najdete toolearn dokumentaci k vaší aplikace tooenable připojení SSL.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Aplikace, které vyžadují ověření certifikátu pro připojení SSL
V některých případech aplikace vyžadují do místního souboru certifikátu bezpečně vygenerovat z důvěryhodné tooconnect souboru (.cer) certifikát certifikační autority (CA). Najdete v následující soubor .cer hello tooobtain kroky hello, dekódovat hello certifikátu a navázat jej tooyour aplikace.

### <a name="download-hello-certificate-file-from-hello-certificate-authority-ca"></a>Stáhnout soubor certifikátu hello z hello certifikační autoritou (CA) 
Hello certifikát potřebný toocommunicate přes protokol SSL s vaší databázi Azure k je umístěn PostgreSQL server [zde](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Stáhněte si soubor certifikátu hello místně.

### <a name="download-and-install-openssl-on-your-machine"></a>Stáhněte a nainstalujte OpenSSL na počítači 
soubor certifikátu hello toodecode potřebné pro vaše aplikace tooconnect bezpečně tooyour databázový server, budete potřebovat tooinstall OpenSSL v místním počítači.

#### <a name="for-linux-os-x-or-unix"></a>Pro Linux, OS X nebo Unix
knihovny OpenSSL Hello jsou součástí zdrojového kódu přímo z hello [Foundation softwaru OpenSSL](http://www.openssl.org). Hello následující pokyny vás provedou hello kroky nezbytné tooinstall OpenSSL ve vašem počítači Linux. Tento článek používá příkazy známé toowork na Ubuntu 12.04 a vyšší.

Otevřete relaci terminálu a nainstalujte OpenSSL
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
Extrahujte soubory hello z balíčku hello
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Zadejte adresář hello, které byly extrahovali soubory hello. Ve výchozím nastavení je nutné následujícím způsobem.

```bash
cd openssl-1.1.0e
```
Nakonfigurujte OpenSSL tak, že spustíte následující příkaz hello. Pokud chcete hello souborů ve složce liší od /usr/local/openssl, nastavit následující zda toochange hello podle potřeby.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
Teď, když OpenSSL je správně nakonfigurovaná, je nutné toocompile ho tooconvert svůj certifikát. toocompile, spusťte následující příkaz hello:

```bash
make
```
Po dokončení kompilace jste připravené tooinstall OpenSSL jako spustitelný soubor spuštěním hello následující příkaz:
```bash
make install
```
tooconfirm, že jste úspěšně nainstalovali OpenSSL ve vašem systému, hello spusťte následující příkaz a zkontrolujte, zda získat hello toomake stejný výstup.

```bash
/usr/local/openssl/bin/openssl version
```
V případě úspěšného byste měli vidět následující zprávou hello.
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>Pro Windows
Instalace OpenSSL na počítači s Windows můžete provést hello následující způsoby:
1. **(Doporučeno)**  Pomocí integrované funkce Bash pro Windows hello ve Windows 10 a vyšší, OpenSSL instaluje ve výchozím nastavení. Pokyny, jak tooenable funkce Bash pro Windows v systému Windows 10 najdete [zde](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).
2. Prostřednictvím stahování k aplikaci Win32/64 poskytované hello komunity. Při hello Foundation softwaru OpenSSL neposkytuje ani neschvaluje žádné konkrétní instalační služba systému Windows, poskytují seznam k dispozici instalační programy [sem](https://wiki.openssl.org/index.php/Binaries)

### <a name="decode-your-certificate-file"></a>Dekódovat soubor certifikátu
Hello stáhnout kořenové certifikační Autority je soubor v šifrovaném tvaru. Použijte soubor OpenSSL toodecode hello certifikátu. toodo Ano, spusťte tento příkaz OpenSSL:

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-tooazure-database-for-postgresql-with-ssl-certificate-authentication"></a>Připojením tooAzure databáze PostgreSQL s ověřováním certifikátu SSL
Teď, když jste úspěšně dekódovat vašeho certifikátu, můžete teď připojit tooyour databázový server bezpečně přes protokol SSL. ověřování certifikátu serveru tooallow, hello certifikátu musí být umístěny v souboru ~/.postgresql/root.crt hello domovské adresáře uživatele hello. (V souboru Microsoft Windows hello je s názvem % APPDATA%\postgresql\root.crt.). Následující Hello poskytuje pokyny pro připojením tooAzure databázi PostgreSQL.

> [!NOTE]
> V současné době je známý problém. Pokud používáte "sslmode = ověřte úplná" ve službě toohello připojení hello připojení se nezdaří s hello následující chyba: _certifikát serveru pro "&lt;oblast&gt;. Control.Database.Windows.NET"(a 7 jiné názvy) neodpovídá název hostitele"&lt;servername&gt;. postgres.database.azure.com "._
> Pokud "sslmode = ověřte úplná" je potřeba, použijte prosím hello zásady vytváření názvů  **&lt;servername&gt;. database.windows.net** jako název hostitele vašeho připojovacího řetězce. Plánujeme tooremove toto omezení v budoucnu hello. Připojení pomocí dalších [SSL režimy](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) by měly pokračovat ve vytváření názvů hostitele hello upřednostňovaný toouse  **&lt;servername&gt;. postgres.database.azure.com**.

#### <a name="using-psql-command-line-utility"></a>Pomocí nástroje příkazového řádku psql
Hello následující příklad ukazuje, jak připojit toosuccessfully tooyour PostgreSQL serveru pomocí nástroje příkazového řádku psql hello. Použití hello `root.crt` soubor vytvořený a hello `sslmode=verify-ca` nebo `sslmode=verify-full` možnost.

Pomocí rozhraní příkazového řádku hello PostgreSQL, spusťte následující příkaz hello:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
Pokud bylo úspěšné, zobrazí se následující výstup hello:
```bash
Password for user mylogin@mypgserver-20170401:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

#### <a name="using-pgadmin-gui-tool"></a>Pomocí nástroje pgAdmin grafického uživatelského rozhraní
Konfigurace pgAdmin 4 tooconnect bezpečně přes protokol SSL vyžaduje tooset hello `SSL mode = Verify-CA` nebo `SSL mode = Verify-Full` následujícím způsobem:

![Snímek obrazovky režimu protokolu SSL pgAdmin – připojení – vyžadovat](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Další kroky
Přečtěte si následující různé možnosti připojení aplikace [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
