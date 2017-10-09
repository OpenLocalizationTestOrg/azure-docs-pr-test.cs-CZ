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
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a><span data-ttu-id="4da74-103">Konfigurace připojení SSL ve službě Azure Database pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4da74-103">Configure SSL connectivity in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="4da74-104">Azure databázi PostgreSQL upřednostní připojení vaší klientské aplikace toohello PostgreSQL služby pomocí Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="4da74-104">Azure Database for PostgreSQL prefers connecting your client applications toohello PostgreSQL service using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="4da74-105">Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed hello" šifrování hello datový proud mezi hello serveru a aplikace.</span><span class="sxs-lookup"><span data-stu-id="4da74-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in hello middle" attacks by encrypting hello data stream between hello server and your application.</span></span>

<span data-ttu-id="4da74-106">Ve výchozím nastavení je hello služba databáze PostgreSQL nakonfigurované toorequire připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="4da74-106">By default, hello PostgreSQL database service is configured toorequire SSL connection.</span></span> <span data-ttu-id="4da74-107">Volitelně můžete zakázat, budete požadovat protokol SSL pro připojení služby tooyour databáze, pokud klientská aplikace nepodporuje připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="4da74-107">Optionally, you can disable requiring SSL for connecting tooyour database service if your client application does not support SSL connectivity.</span></span> 

## <a name="enforcing-ssl-connections"></a><span data-ttu-id="4da74-108">Vynucení připojení SSL</span><span class="sxs-lookup"><span data-stu-id="4da74-108">Enforcing SSL connections</span></span>
<span data-ttu-id="4da74-109">Pro všechny databáze Azure pro servery PostgreSQL, které jsou zřízené prostřednictvím hello portál Azure a rozhraní příkazového řádku je ve výchozím nastavení povolené vynucení připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="4da74-109">For all Azure Database for PostgreSQL servers provisioned through hello Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="4da74-110">Připojovací řetězce, které jsou předem definované v nastavení "Připojovací řetězce" hello v rámci vašeho serveru v hello portál Azure, zahrnují hello požadované parametry pro běžné jazyky tooconnect tooyour databáze serveru pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="4da74-110">Likewise, connection strings that are pre-defined in hello "Connection Strings" settings under your server in hello Azure portal include hello required parameters for common languages tooconnect tooyour database server using SSL.</span></span> <span data-ttu-id="4da74-111">Hello parametr SSL se liší podle hello konektor, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.</span><span class="sxs-lookup"><span data-stu-id="4da74-111">hello SSL parameter varies based on hello connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

## <a name="configure-enforcement-of-ssl"></a><span data-ttu-id="4da74-112">Konfigurace vynucení protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="4da74-112">Configure Enforcement of SSL</span></span>
<span data-ttu-id="4da74-113">Volitelně můžete zakázat vynucení připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="4da74-113">You can optionally disable enforcing SSL connectivity.</span></span> <span data-ttu-id="4da74-114">Microsoft Azure se doporučuje povolit tooalways **připojení SSL vynutit** nastavení pro lepší zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4da74-114">Microsoft Azure recommends tooalways enable **Enforce SSL connection** setting for enhanced security.</span></span>

### <a name="using-hello-azure-portal"></a><span data-ttu-id="4da74-115">Pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4da74-115">Using hello Azure portal</span></span>
<span data-ttu-id="4da74-116">Navštivte vaší databázi Azure pro PostgreSQL server a klikněte na tlačítko **zabezpečení připojení**.</span><span class="sxs-lookup"><span data-stu-id="4da74-116">Visit your Azure Database for PostgreSQL server and click **Connection security**.</span></span> <span data-ttu-id="4da74-117">Použít hello přepínací tlačítko tooenable nebo zakázat hello **připojení SSL vynutit** nastavení.</span><span class="sxs-lookup"><span data-stu-id="4da74-117">Use hello toggle button tooenable or disable hello **Enforce SSL connection** setting.</span></span> <span data-ttu-id="4da74-118">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4da74-118">Then click **Save**.</span></span> 

![Zabezpečení připojení - zakázat vynutit SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

<span data-ttu-id="4da74-120">Hello nastavení si můžete ověřit zobrazením hello **přehled** stránku hello toosee **SSL vynutit stav** indikátoru.</span><span class="sxs-lookup"><span data-stu-id="4da74-120">You can confirm hello setting by viewing hello **Overview** page toosee hello **SSL enforce status** indicator.</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="4da74-121">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4da74-121">Using Azure CLI</span></span>
<span data-ttu-id="4da74-122">Můžete povolit nebo zakázat hello **ssl vynucení** pomocí parametru `Enabled` nebo `Disabled` hodnot v uvedeném pořadí v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4da74-122">You can enable or disable hello **ssl-enforcement** parameter using `Enabled` or `Disabled` values respectively in Azure CLI.</span></span>

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a><span data-ttu-id="4da74-123">Ujistěte se, vaše aplikace nebo framework podporuje připojení SSL</span><span class="sxs-lookup"><span data-stu-id="4da74-123">Ensure your application or framework supports SSL connections</span></span>
<span data-ttu-id="4da74-124">Mnoho běžné architektury aplikace, které používají PostgreSQL pro databázi služby, jako je například Drupal a Django, nepovolujte SSL ve výchozím nastavení během instalace.</span><span class="sxs-lookup"><span data-stu-id="4da74-124">Many common application frameworks that use PostgreSQL for database services, such as Drupal and Django, do not enable SSL by default during installation.</span></span> <span data-ttu-id="4da74-125">Povolení připojení SSL je třeba provést po instalaci, nebo pomocí rozhraní příkazového řádku příkazy toohello konkrétní aplikací.</span><span class="sxs-lookup"><span data-stu-id="4da74-125">Enabling SSL connectivity must be done after installation or through CLI commands specific toohello application.</span></span> <span data-ttu-id="4da74-126">Pokud váš server PostgreSQL vynucuje připojení SSL a hello přidružené aplikace není správně nakonfigurována, hello aplikace mohou selhat tooconnect tooyour databázový server.</span><span class="sxs-lookup"><span data-stu-id="4da74-126">If your PostgreSQL server is enforcing SSL connections and hello associated application is not configured properly, hello application may fail tooconnect tooyour database server.</span></span> <span data-ttu-id="4da74-127">Jak najdete toolearn dokumentaci k vaší aplikace tooenable připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="4da74-127">Consult your application's documentation toolearn how tooenable SSL connections.</span></span>


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a><span data-ttu-id="4da74-128">Aplikace, které vyžadují ověření certifikátu pro připojení SSL</span><span class="sxs-lookup"><span data-stu-id="4da74-128">Applications that require certificate verification for SSL connectivity</span></span>
<span data-ttu-id="4da74-129">V některých případech aplikace vyžadují do místního souboru certifikátu bezpečně vygenerovat z důvěryhodné tooconnect souboru (.cer) certifikát certifikační autority (CA).</span><span class="sxs-lookup"><span data-stu-id="4da74-129">In some cases, applications require a local certificate file generated from a trusted Certificate Authority (CA) certificate file (.cer) tooconnect securely.</span></span> <span data-ttu-id="4da74-130">Najdete v následující soubor .cer hello tooobtain kroky hello, dekódovat hello certifikátu a navázat jej tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="4da74-130">See hello following steps tooobtain hello .cer file, decode hello certificate and bind it tooyour application.</span></span>

### <a name="download-hello-certificate-file-from-hello-certificate-authority-ca"></a><span data-ttu-id="4da74-131">Stáhnout soubor certifikátu hello z hello certifikační autoritou (CA)</span><span class="sxs-lookup"><span data-stu-id="4da74-131">Download hello certificate file from hello Certificate Authority (CA)</span></span> 
<span data-ttu-id="4da74-132">Hello certifikát potřebný toocommunicate přes protokol SSL s vaší databázi Azure k je umístěn PostgreSQL server [zde](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span><span class="sxs-lookup"><span data-stu-id="4da74-132">hello certificate needed toocommunicate over SSL with your Azure Database for PostgreSQL server is located [here](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span></span> <span data-ttu-id="4da74-133">Stáhněte si soubor certifikátu hello místně.</span><span class="sxs-lookup"><span data-stu-id="4da74-133">Download hello certificate file locally.</span></span>

### <a name="download-and-install-openssl-on-your-machine"></a><span data-ttu-id="4da74-134">Stáhněte a nainstalujte OpenSSL na počítači</span><span class="sxs-lookup"><span data-stu-id="4da74-134">Download and install OpenSSL on your machine</span></span> 
<span data-ttu-id="4da74-135">soubor certifikátu hello toodecode potřebné pro vaše aplikace tooconnect bezpečně tooyour databázový server, budete potřebovat tooinstall OpenSSL v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="4da74-135">toodecode hello certificate file needed for your application tooconnect securely tooyour database server, you need tooinstall OpenSSL on your local computer.</span></span>

#### <a name="for-linux-os-x-or-unix"></a><span data-ttu-id="4da74-136">Pro Linux, OS X nebo Unix</span><span class="sxs-lookup"><span data-stu-id="4da74-136">For Linux, OS X, or Unix</span></span>
<span data-ttu-id="4da74-137">knihovny OpenSSL Hello jsou součástí zdrojového kódu přímo z hello [Foundation softwaru OpenSSL](http://www.openssl.org).</span><span class="sxs-lookup"><span data-stu-id="4da74-137">hello OpenSSL libraries are provided in source code directly from hello [OpenSSL Software Foundation](http://www.openssl.org).</span></span> <span data-ttu-id="4da74-138">Hello následující pokyny vás provedou hello kroky nezbytné tooinstall OpenSSL ve vašem počítači Linux.</span><span class="sxs-lookup"><span data-stu-id="4da74-138">hello following instructions guide you through hello steps necessary tooinstall OpenSSL on your Linux PC.</span></span> <span data-ttu-id="4da74-139">Tento článek používá příkazy známé toowork na Ubuntu 12.04 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="4da74-139">This article uses commands known toowork on Ubuntu 12.04 and higher.</span></span>

<span data-ttu-id="4da74-140">Otevřete relaci terminálu a nainstalujte OpenSSL</span><span class="sxs-lookup"><span data-stu-id="4da74-140">Open a terminal session and install OpenSSL</span></span>
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
<span data-ttu-id="4da74-141">Extrahujte soubory hello z balíčku hello</span><span class="sxs-lookup"><span data-stu-id="4da74-141">Extract hello files from hello download package</span></span>
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
<span data-ttu-id="4da74-142">Zadejte adresář hello, které byly extrahovali soubory hello.</span><span class="sxs-lookup"><span data-stu-id="4da74-142">Enter hello directory where hello files were extracted.</span></span> <span data-ttu-id="4da74-143">Ve výchozím nastavení je nutné následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="4da74-143">By default, it should be as follows.</span></span>

```bash
cd openssl-1.1.0e
```
<span data-ttu-id="4da74-144">Nakonfigurujte OpenSSL tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="4da74-144">Configure OpenSSL by executing hello following command.</span></span> <span data-ttu-id="4da74-145">Pokud chcete hello souborů ve složce liší od /usr/local/openssl, nastavit následující zda toochange hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4da74-145">If you want hello files in a folder different than /usr/local/openssl, make sure toochange hello following as appropriate.</span></span>

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
<span data-ttu-id="4da74-146">Teď, když OpenSSL je správně nakonfigurovaná, je nutné toocompile ho tooconvert svůj certifikát.</span><span class="sxs-lookup"><span data-stu-id="4da74-146">Now that OpenSSL is configured properly, you need toocompile it tooconvert your certificate.</span></span> <span data-ttu-id="4da74-147">toocompile, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4da74-147">toocompile, run hello following command:</span></span>

```bash
make
```
<span data-ttu-id="4da74-148">Po dokončení kompilace jste připravené tooinstall OpenSSL jako spustitelný soubor spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4da74-148">Once compiling is complete, you're ready tooinstall OpenSSL as an executable by running hello following command:</span></span>
```bash
make install
```
<span data-ttu-id="4da74-149">tooconfirm, že jste úspěšně nainstalovali OpenSSL ve vašem systému, hello spusťte následující příkaz a zkontrolujte, zda získat hello toomake stejný výstup.</span><span class="sxs-lookup"><span data-stu-id="4da74-149">tooconfirm that you've successfully installed OpenSSL on your system, run hello following command and check toomake sure you get hello same output.</span></span>

```bash
/usr/local/openssl/bin/openssl version
```
<span data-ttu-id="4da74-150">V případě úspěšného byste měli vidět následující zprávou hello.</span><span class="sxs-lookup"><span data-stu-id="4da74-150">If successful you should see hello following message.</span></span>
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a><span data-ttu-id="4da74-151">Pro Windows</span><span class="sxs-lookup"><span data-stu-id="4da74-151">For Windows</span></span>
<span data-ttu-id="4da74-152">Instalace OpenSSL na počítači s Windows můžete provést hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="4da74-152">Installing OpenSSL on a Windows PC can be done in hello following ways:</span></span>
1. <span data-ttu-id="4da74-153">**(Doporučeno)**  Pomocí integrované funkce Bash pro Windows hello ve Windows 10 a vyšší, OpenSSL instaluje ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4da74-153">**(Recommended)** Using hello built-in Bash for Windows functionality in Window 10 and above, OpenSSL is installed by default.</span></span> <span data-ttu-id="4da74-154">Pokyny, jak tooenable funkce Bash pro Windows v systému Windows 10 najdete [zde](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span><span class="sxs-lookup"><span data-stu-id="4da74-154">Instructions on how tooenable Bash for Windows functionality in Windows 10 can be found [here](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span></span>
2. <span data-ttu-id="4da74-155">Prostřednictvím stahování k aplikaci Win32/64 poskytované hello komunity.</span><span class="sxs-lookup"><span data-stu-id="4da74-155">Through downloading a Win32/64 application provided by hello community.</span></span> <span data-ttu-id="4da74-156">Při hello Foundation softwaru OpenSSL neposkytuje ani neschvaluje žádné konkrétní instalační služba systému Windows, poskytují seznam k dispozici instalační programy [sem](https://wiki.openssl.org/index.php/Binaries)</span><span class="sxs-lookup"><span data-stu-id="4da74-156">While hello OpenSSL Software Foundation does not provide or endorse any specific Windows installers, they provide a list of available installers [here](https://wiki.openssl.org/index.php/Binaries)</span></span>

### <a name="decode-your-certificate-file"></a><span data-ttu-id="4da74-157">Dekódovat soubor certifikátu</span><span class="sxs-lookup"><span data-stu-id="4da74-157">Decode your certificate file</span></span>
<span data-ttu-id="4da74-158">Hello stáhnout kořenové certifikační Autority je soubor v šifrovaném tvaru.</span><span class="sxs-lookup"><span data-stu-id="4da74-158">hello downloaded Root CA file is in encrypted format.</span></span> <span data-ttu-id="4da74-159">Použijte soubor OpenSSL toodecode hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="4da74-159">Use OpenSSL toodecode hello certificate file.</span></span> <span data-ttu-id="4da74-160">toodo Ano, spusťte tento příkaz OpenSSL:</span><span class="sxs-lookup"><span data-stu-id="4da74-160">toodo so, run this OpenSSL command:</span></span>

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-tooazure-database-for-postgresql-with-ssl-certificate-authentication"></a><span data-ttu-id="4da74-161">Připojením tooAzure databáze PostgreSQL s ověřováním certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="4da74-161">Connecting tooAzure Database for PostgreSQL with SSL certificate authentication</span></span>
<span data-ttu-id="4da74-162">Teď, když jste úspěšně dekódovat vašeho certifikátu, můžete teď připojit tooyour databázový server bezpečně přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="4da74-162">Now that you have successfully decoded your certificate, you can now connect tooyour database server securely over SSL.</span></span> <span data-ttu-id="4da74-163">ověřování certifikátu serveru tooallow, hello certifikátu musí být umístěny v souboru ~/.postgresql/root.crt hello domovské adresáře uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4da74-163">tooallow server certificate verification, hello certificate must be placed in hello file ~/.postgresql/root.crt in hello user's home directory.</span></span> <span data-ttu-id="4da74-164">(V souboru Microsoft Windows hello je s názvem % APPDATA%\postgresql\root.crt.).</span><span class="sxs-lookup"><span data-stu-id="4da74-164">(On Microsoft Windows hello file is named %APPDATA%\postgresql\root.crt.).</span></span> <span data-ttu-id="4da74-165">Následující Hello poskytuje pokyny pro připojením tooAzure databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="4da74-165">hello following provides instructions for connecting tooAzure Database for PostgreSQL.</span></span>

> [!NOTE]
> <span data-ttu-id="4da74-166">V současné době je známý problém. Pokud používáte "sslmode = ověřte úplná" ve službě toohello připojení hello připojení se nezdaří s hello následující chyba: _certifikát serveru pro "&lt;oblast&gt;. Control.Database.Windows.NET"(a 7 jiné názvy) neodpovídá název hostitele"&lt;servername&gt;. postgres.database.azure.com "._</span><span class="sxs-lookup"><span data-stu-id="4da74-166">Currently, there is a known issue if you use "sslmode=verify-full" in your connection toohello service, hello connection will fail with hello following error: _server certificate for "&lt;region&gt;.control.database.windows.net" (and 7 other names) does not match host name "&lt;servername&gt;.postgres.database.azure.com"._</span></span>
> <span data-ttu-id="4da74-167">Pokud "sslmode = ověřte úplná" je potřeba, použijte prosím hello zásady vytváření názvů  **&lt;servername&gt;. database.windows.net** jako název hostitele vašeho připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="4da74-167">If "sslmode=verify-full" is required, please use hello server naming convention **&lt;servername&gt;.database.windows.net** as your connection string host name.</span></span> <span data-ttu-id="4da74-168">Plánujeme tooremove toto omezení v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="4da74-168">We plan tooremove this limitation in hello future.</span></span> <span data-ttu-id="4da74-169">Připojení pomocí dalších [SSL režimy](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) by měly pokračovat ve vytváření názvů hostitele hello upřednostňovaný toouse  **&lt;servername&gt;. postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="4da74-169">Connections using other [SSL modes](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) should continue toouse hello preferred host naming convention **&lt;servername&gt;.postgres.database.azure.com**.</span></span>

#### <a name="using-psql-command-line-utility"></a><span data-ttu-id="4da74-170">Pomocí nástroje příkazového řádku psql</span><span class="sxs-lookup"><span data-stu-id="4da74-170">Using psql command-line utility</span></span>
<span data-ttu-id="4da74-171">Hello následující příklad ukazuje, jak připojit toosuccessfully tooyour PostgreSQL serveru pomocí nástroje příkazového řádku psql hello.</span><span class="sxs-lookup"><span data-stu-id="4da74-171">hello following example shows you how toosuccessfully connect tooyour PostgreSQL server using hello psql command-line utility.</span></span> <span data-ttu-id="4da74-172">Použití hello `root.crt` soubor vytvořený a hello `sslmode=verify-ca` nebo `sslmode=verify-full` možnost.</span><span class="sxs-lookup"><span data-stu-id="4da74-172">Use hello `root.crt` file created and hello `sslmode=verify-ca` or `sslmode=verify-full` option.</span></span>

<span data-ttu-id="4da74-173">Pomocí rozhraní příkazového řádku hello PostgreSQL, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4da74-173">Using hello PostgreSQL command-line interface, execute hello following command:</span></span>
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
<span data-ttu-id="4da74-174">Pokud bylo úspěšné, zobrazí se následující výstup hello:</span><span class="sxs-lookup"><span data-stu-id="4da74-174">If successful, you receive hello following output:</span></span>
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

#### <a name="using-pgadmin-gui-tool"></a><span data-ttu-id="4da74-175">Pomocí nástroje pgAdmin grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="4da74-175">Using pgAdmin GUI tool</span></span>
<span data-ttu-id="4da74-176">Konfigurace pgAdmin 4 tooconnect bezpečně přes protokol SSL vyžaduje tooset hello `SSL mode = Verify-CA` nebo `SSL mode = Verify-Full` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4da74-176">Configuring pgAdmin 4 tooconnect securely over SSL requires you tooset hello `SSL mode = Verify-CA` or `SSL mode = Verify-Full` as follows:</span></span>

![Snímek obrazovky režimu protokolu SSL pgAdmin – připojení – vyžadovat](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a><span data-ttu-id="4da74-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4da74-178">Next steps</span></span>
<span data-ttu-id="4da74-179">Přečtěte si následující různé možnosti připojení aplikace [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="4da74-179">Review various application connectivity options following [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
