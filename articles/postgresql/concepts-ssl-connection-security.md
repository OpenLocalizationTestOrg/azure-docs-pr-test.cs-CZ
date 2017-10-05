---
title: "Konfigurace připojení SSL ve službě Azure Database pro PostgreSQL | Microsoft Docs"
description: "Pokyny a informace pro konfiguraci Azure databáze PostgreSQL a přidružené aplikace odpovídajícím způsobem používat připojení SSL."
services: postgresql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql
ms.custom: 
ms.topic: article
ms.date: 05/15/2017
ms.openlocfilehash: 685aa4c2f75b7c3260ca737f7c786157480b2d90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>Konfigurace připojení SSL ve službě Azure Database pro PostgreSQL
Azure databázi PostgreSQL upřednostní připojení vaší klientské aplikace do služby PostgreSQL pomocí Secure Sockets Layer (SSL). Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.

Ve výchozím nastavení je služba databáze PostgreSQL nakonfigurovaná tak, aby vyžadovala připojení SSL. Volitelně můžete zakázat, budete požadovat protokol SSL pro připojení k databázi služby, pokud klientská aplikace nepodporuje připojení SSL. 

## <a name="enforcing-ssl-connections"></a>Vynucení připojení SSL
Pro všechny databáze Azure pro servery PostgreSQL, které jsou zřízené prostřednictvím portálu Azure a rozhraní příkazového řádku je ve výchozím nastavení povolené vynucení připojení SSL. 

Připojovací řetězce, které jsou předem definované v nastavení "Připojovací řetězce" v rámci vašeho serveru na portálu Azure, zahrnout požadované parametry pro běžné jazyky pro připojení k databázovému serveru pomocí protokolu SSL. Parametr SSL se liší v závislosti na konektoru, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.

## <a name="configure-enforcement-of-ssl"></a>Konfigurace vynucení protokolu SSL
Volitelně můžete zakázat vynucení připojení SSL. Microsoft Azure se doporučuje vždy povolit **připojení SSL vynutit** nastavení pro lepší zabezpečení.

### <a name="using-the-azure-portal"></a>Použití webu Azure Portal
Navštivte vaší databázi Azure pro PostgreSQL server a klikněte na tlačítko **zabezpečení připojení**. Přepínací tlačítko použít k povolení nebo zakázání **připojení SSL vynutit** nastavení. Potom klikněte na **Uložit**. 

![Zabezpečení připojení - zakázat vynutit SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Toto nastavení si můžete ověřit zobrazením **přehled** stránky zobrazíte **SSL vynutit stav** indikátoru.

### <a name="using-azure-cli"></a>Použití Azure CLI
Můžete povolit nebo zakázat **ssl vynucení** pomocí parametru `Enabled` nebo `Disabled` hodnot v uvedeném pořadí v rozhraní příkazového řádku Azure.

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Ujistěte se, vaše aplikace nebo framework podporuje připojení SSL
Mnoho běžné architektury aplikace, které používají PostgreSQL pro databázi služby, jako je například Drupal a Django, nepovolujte SSL ve výchozím nastavení během instalace. Povolení připojení SSL je třeba provést po instalaci nebo prostřednictvím rozhraní příkazového řádku, které jsou specifické pro aplikaci. Pokud váš server PostgreSQL vynucuje připojení SSL a přidružené aplikace není správně nakonfigurována, aplikace se pravděpodobně nepodaří připojit k databázovému serveru. Projděte si dokumentaci k vaší aplikace se dozvíte, jak povolit připojení SSL.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Aplikace, které vyžadují ověření certifikátu pro připojení SSL
V některých případech aplikace vyžadují do místního souboru certifikátu vygenerovat z důvěryhodné certifikační autority (CA) soubor certifikátu (.cer) na zabezpečené připojení. Prohlédněte si následující postup získat soubor .cer, dekódovat certifikát a navázat jej do vaší aplikace.

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>Stáhněte si soubor certifikátu z certifikační autoritou (CA) 
Certifikát potřebný pro komunikaci pomocí protokolu SSL s vaší databázi Azure pro je umístěn PostgreSQL server [zde](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Stáhněte si soubor certifikátu místně.

### <a name="download-and-install-openssl-on-your-machine"></a>Stáhněte a nainstalujte OpenSSL na počítači 
Chcete-li dekódovat soubor certifikátu pro vaši aplikaci bezpečně připojit k databázovému serveru, musíte nainstalovat OpenSSL v místním počítači.

#### <a name="for-linux-os-x-or-unix"></a>Pro Linux, OS X nebo Unix
Knihovny OpenSSL jsou součástí zdrojového kódu přímo z [Foundation softwaru OpenSSL](http://www.openssl.org). Následující pokyny vás provede kroky potřebnými k instalaci OpenSSL ve vašem počítači Linux. Tento článek používá příkazy známé pracovat na Ubuntu 12.04 a vyšší.

Otevřete relaci terminálu a nainstalujte OpenSSL
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
Extrahujte soubory ze staženého balíčku
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Zadejte adresář, kde byly soubory extrahovány. Ve výchozím nastavení je nutné následujícím způsobem.

```bash
cd openssl-1.1.0e
```
Nakonfigurujte OpenSSL spuštěním následujícího příkazu. Pokud chcete soubory ve složce liší od /usr/local/openssl, nezapomeňte změnit z níže uvedených kroků.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
Teď, když OpenSSL je správně nakonfigurován, budete muset zkompilovat ji převést svůj certifikát. Kompilace, spusťte následující příkaz:

```bash
make
```
Po dokončení kompilace jste připraveni k instalaci OpenSSL jako spustitelný soubor tak, že spustíte následující příkaz:
```bash
make install
```
Chcete-li potvrdit, že jste úspěšně nainstalovali OpenSSL ve vašem systému, spusťte následující příkaz a zkontrolujte, zda že získat stejný výstup.

```bash
/usr/local/openssl/bin/openssl version
```
V případě úspěchu, měli byste vidět následující zpráva.
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>Pro Windows
Instalace OpenSSL na počítači s Windows můžete udělat následujícími způsoby:
1. **(Doporučeno)**  Pomocí integrované funkce Bash pro systém Windows ve Windows 10 a vyšší, OpenSSL instaluje ve výchozím nastavení. Najdete pokyny k povolení funkce Bash pro Windows v systému Windows 10 [zde](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).
2. Prostřednictvím stahování k aplikaci Win32/64 poskytované komunitou. Když OpenSSL Software Foundation neposkytuje ani neschvaluje žádné konkrétní instalační služba systému Windows, poskytují seznam k dispozici instalační programy [sem](https://wiki.openssl.org/index.php/Binaries)

### <a name="decode-your-certificate-file"></a>Dekódovat soubor certifikátu
Stažený soubor kořenové certifikační Autority je v šifrovaném tvaru. Použijte OpenSSL k dekódování soubor certifikátu. Chcete-li tak učinit, spusťte tento příkaz OpenSSL:

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Připojení k databázi Azure pro PostgreSQL s ověřováním certifikátu SSL
Teď, když jste úspěšně dekódovat vašeho certifikátu, můžete nyní připojíte k databázovému serveru bezpečně přes protokol SSL. Povolit certifikát ověření serveru, certifikát musí být umístěny v souboru ~/.postgresql/root.crt v domovském adresáři uživatele. (V systému Windows soubor je s názvem % APPDATA%\postgresql\root.crt.). Následující část obsahuje pokyny pro připojení k databázi Azure pro PostgreSQL.

> [!NOTE]
> V současné době je známý problém. Pokud používáte "sslmode = ověřte úplná" v připojení ke službě, připojení se nezdaří s následující chybou: _certifikát serveru pro "&lt;oblast&gt;. control.database.windows.net" (a 7 jiné názvy) se neshoduje s názvem hostitele "&lt;servername&gt;. postgres.database.azure.com"._
> Pokud "sslmode = ověřte úplná" je potřeba, použijte zásady vytváření názvů serveru  **&lt;servername&gt;. database.windows.net** jako název hostitele vašeho připojovacího řetězce. Plánujeme odebrat v budoucnu toto omezení. Připojení pomocí dalších [SSL režimy](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) by měly být nadále používat zásady vytváření názvů upřednostňované hostitele  **&lt;servername&gt;. postgres.database.azure.com**.

#### <a name="using-psql-command-line-utility"></a>Pomocí nástroje příkazového řádku psql
Následující příklad ukazuje, jak se úspěšně připojit k serveru pomocí nástroje příkazového řádku psql PostgreSQL. Použití `root.crt` soubor vytvořený a `sslmode=verify-ca` nebo `sslmode=verify-full` možnost.

Pomocí rozhraní příkazového řádku PostgreSQL, spusťte následující příkaz:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
Pokud bylo úspěšné, zobrazí se následující výstup:
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
Konfigurace pgAdmin 4 do zabezpečené připojení prostřednictvím protokolu SSL vyžaduje, abyste nastavili `SSL mode = Verify-CA` nebo `SSL mode = Verify-Full` následujícím způsobem:

![Snímek obrazovky režimu protokolu SSL pgAdmin – připojení – vyžadovat](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Další kroky
Přečtěte si následující různé možnosti připojení aplikace [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
