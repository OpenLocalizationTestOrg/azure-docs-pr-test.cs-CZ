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
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a><span data-ttu-id="42a27-103">Konfigurace připojení SSL ve službě Azure Database pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="42a27-103">Configure SSL connectivity in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="42a27-104">Azure databázi PostgreSQL upřednostní připojení vaší klientské aplikace do služby PostgreSQL pomocí Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="42a27-104">Azure Database for PostgreSQL prefers connecting your client applications to the PostgreSQL service using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="42a27-105">Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.</span><span class="sxs-lookup"><span data-stu-id="42a27-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" attacks by encrypting the data stream between the server and your application.</span></span>

<span data-ttu-id="42a27-106">Ve výchozím nastavení je služba databáze PostgreSQL nakonfigurovaná tak, aby vyžadovala připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="42a27-106">By default, the PostgreSQL database service is configured to require SSL connection.</span></span> <span data-ttu-id="42a27-107">Volitelně můžete zakázat, budete požadovat protokol SSL pro připojení k databázi služby, pokud klientská aplikace nepodporuje připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="42a27-107">Optionally, you can disable requiring SSL for connecting to your database service if your client application does not support SSL connectivity.</span></span> 

## <a name="enforcing-ssl-connections"></a><span data-ttu-id="42a27-108">Vynucení připojení SSL</span><span class="sxs-lookup"><span data-stu-id="42a27-108">Enforcing SSL connections</span></span>
<span data-ttu-id="42a27-109">Pro všechny databáze Azure pro servery PostgreSQL, které jsou zřízené prostřednictvím portálu Azure a rozhraní příkazového řádku je ve výchozím nastavení povolené vynucení připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="42a27-109">For all Azure Database for PostgreSQL servers provisioned through the Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="42a27-110">Připojovací řetězce, které jsou předem definované v nastavení "Připojovací řetězce" v rámci vašeho serveru na portálu Azure, zahrnout požadované parametry pro běžné jazyky pro připojení k databázovému serveru pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="42a27-110">Likewise, connection strings that are pre-defined in the "Connection Strings" settings under your server in the Azure portal include the required parameters for common languages to connect to your database server using SSL.</span></span> <span data-ttu-id="42a27-111">Parametr SSL se liší v závislosti na konektoru, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.</span><span class="sxs-lookup"><span data-stu-id="42a27-111">The SSL parameter varies based on the connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

## <a name="configure-enforcement-of-ssl"></a><span data-ttu-id="42a27-112">Konfigurace vynucení protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="42a27-112">Configure Enforcement of SSL</span></span>
<span data-ttu-id="42a27-113">Volitelně můžete zakázat vynucení připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="42a27-113">You can optionally disable enforcing SSL connectivity.</span></span> <span data-ttu-id="42a27-114">Microsoft Azure se doporučuje vždy povolit **připojení SSL vynutit** nastavení pro lepší zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="42a27-114">Microsoft Azure recommends to always enable **Enforce SSL connection** setting for enhanced security.</span></span>

### <a name="using-the-azure-portal"></a><span data-ttu-id="42a27-115">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="42a27-115">Using the Azure portal</span></span>
<span data-ttu-id="42a27-116">Navštivte vaší databázi Azure pro PostgreSQL server a klikněte na tlačítko **zabezpečení připojení**.</span><span class="sxs-lookup"><span data-stu-id="42a27-116">Visit your Azure Database for PostgreSQL server and click **Connection security**.</span></span> <span data-ttu-id="42a27-117">Přepínací tlačítko použít k povolení nebo zakázání **připojení SSL vynutit** nastavení.</span><span class="sxs-lookup"><span data-stu-id="42a27-117">Use the toggle button to enable or disable the **Enforce SSL connection** setting.</span></span> <span data-ttu-id="42a27-118">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="42a27-118">Then click **Save**.</span></span> 

![Zabezpečení připojení - zakázat vynutit SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

<span data-ttu-id="42a27-120">Toto nastavení si můžete ověřit zobrazením **přehled** stránky zobrazíte **SSL vynutit stav** indikátoru.</span><span class="sxs-lookup"><span data-stu-id="42a27-120">You can confirm the setting by viewing the **Overview** page to see the **SSL enforce status** indicator.</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="42a27-121">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="42a27-121">Using Azure CLI</span></span>
<span data-ttu-id="42a27-122">Můžete povolit nebo zakázat **ssl vynucení** pomocí parametru `Enabled` nebo `Disabled` hodnot v uvedeném pořadí v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="42a27-122">You can enable or disable the **ssl-enforcement** parameter using `Enabled` or `Disabled` values respectively in Azure CLI.</span></span>

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a><span data-ttu-id="42a27-123">Ujistěte se, vaše aplikace nebo framework podporuje připojení SSL</span><span class="sxs-lookup"><span data-stu-id="42a27-123">Ensure your application or framework supports SSL connections</span></span>
<span data-ttu-id="42a27-124">Mnoho běžné architektury aplikace, které používají PostgreSQL pro databázi služby, jako je například Drupal a Django, nepovolujte SSL ve výchozím nastavení během instalace.</span><span class="sxs-lookup"><span data-stu-id="42a27-124">Many common application frameworks that use PostgreSQL for database services, such as Drupal and Django, do not enable SSL by default during installation.</span></span> <span data-ttu-id="42a27-125">Povolení připojení SSL je třeba provést po instalaci nebo prostřednictvím rozhraní příkazového řádku, které jsou specifické pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42a27-125">Enabling SSL connectivity must be done after installation or through CLI commands specific to the application.</span></span> <span data-ttu-id="42a27-126">Pokud váš server PostgreSQL vynucuje připojení SSL a přidružené aplikace není správně nakonfigurována, aplikace se pravděpodobně nepodaří připojit k databázovému serveru.</span><span class="sxs-lookup"><span data-stu-id="42a27-126">If your PostgreSQL server is enforcing SSL connections and the associated application is not configured properly, the application may fail to connect to your database server.</span></span> <span data-ttu-id="42a27-127">Projděte si dokumentaci k vaší aplikace se dozvíte, jak povolit připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="42a27-127">Consult your application's documentation to learn how to enable SSL connections.</span></span>


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a><span data-ttu-id="42a27-128">Aplikace, které vyžadují ověření certifikátu pro připojení SSL</span><span class="sxs-lookup"><span data-stu-id="42a27-128">Applications that require certificate verification for SSL connectivity</span></span>
<span data-ttu-id="42a27-129">V některých případech aplikace vyžadují do místního souboru certifikátu vygenerovat z důvěryhodné certifikační autority (CA) soubor certifikátu (.cer) na zabezpečené připojení.</span><span class="sxs-lookup"><span data-stu-id="42a27-129">In some cases, applications require a local certificate file generated from a trusted Certificate Authority (CA) certificate file (.cer) to connect securely.</span></span> <span data-ttu-id="42a27-130">Prohlédněte si následující postup získat soubor .cer, dekódovat certifikát a navázat jej do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="42a27-130">See the following steps to obtain the .cer file, decode the certificate and bind it to your application.</span></span>

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a><span data-ttu-id="42a27-131">Stáhněte si soubor certifikátu z certifikační autoritou (CA)</span><span class="sxs-lookup"><span data-stu-id="42a27-131">Download the certificate file from the Certificate Authority (CA)</span></span> 
<span data-ttu-id="42a27-132">Certifikát potřebný pro komunikaci pomocí protokolu SSL s vaší databázi Azure pro je umístěn PostgreSQL server [zde](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span><span class="sxs-lookup"><span data-stu-id="42a27-132">The certificate needed to communicate over SSL with your Azure Database for PostgreSQL server is located [here](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span></span> <span data-ttu-id="42a27-133">Stáhněte si soubor certifikátu místně.</span><span class="sxs-lookup"><span data-stu-id="42a27-133">Download the certificate file locally.</span></span>

### <a name="download-and-install-openssl-on-your-machine"></a><span data-ttu-id="42a27-134">Stáhněte a nainstalujte OpenSSL na počítači</span><span class="sxs-lookup"><span data-stu-id="42a27-134">Download and install OpenSSL on your machine</span></span> 
<span data-ttu-id="42a27-135">Chcete-li dekódovat soubor certifikátu pro vaši aplikaci bezpečně připojit k databázovému serveru, musíte nainstalovat OpenSSL v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="42a27-135">To decode the certificate file needed for your application to connect securely to your database server, you need to install OpenSSL on your local computer.</span></span>

#### <a name="for-linux-os-x-or-unix"></a><span data-ttu-id="42a27-136">Pro Linux, OS X nebo Unix</span><span class="sxs-lookup"><span data-stu-id="42a27-136">For Linux, OS X, or Unix</span></span>
<span data-ttu-id="42a27-137">Knihovny OpenSSL jsou součástí zdrojového kódu přímo z [Foundation softwaru OpenSSL](http://www.openssl.org).</span><span class="sxs-lookup"><span data-stu-id="42a27-137">The OpenSSL libraries are provided in source code directly from the [OpenSSL Software Foundation](http://www.openssl.org).</span></span> <span data-ttu-id="42a27-138">Následující pokyny vás provede kroky potřebnými k instalaci OpenSSL ve vašem počítači Linux.</span><span class="sxs-lookup"><span data-stu-id="42a27-138">The following instructions guide you through the steps necessary to install OpenSSL on your Linux PC.</span></span> <span data-ttu-id="42a27-139">Tento článek používá příkazy známé pracovat na Ubuntu 12.04 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="42a27-139">This article uses commands known to work on Ubuntu 12.04 and higher.</span></span>

<span data-ttu-id="42a27-140">Otevřete relaci terminálu a nainstalujte OpenSSL</span><span class="sxs-lookup"><span data-stu-id="42a27-140">Open a terminal session and install OpenSSL</span></span>
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
<span data-ttu-id="42a27-141">Extrahujte soubory ze staženého balíčku</span><span class="sxs-lookup"><span data-stu-id="42a27-141">Extract the files from the download package</span></span>
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
<span data-ttu-id="42a27-142">Zadejte adresář, kde byly soubory extrahovány.</span><span class="sxs-lookup"><span data-stu-id="42a27-142">Enter the directory where the files were extracted.</span></span> <span data-ttu-id="42a27-143">Ve výchozím nastavení je nutné následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="42a27-143">By default, it should be as follows.</span></span>

```bash
cd openssl-1.1.0e
```
<span data-ttu-id="42a27-144">Nakonfigurujte OpenSSL spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="42a27-144">Configure OpenSSL by executing the following command.</span></span> <span data-ttu-id="42a27-145">Pokud chcete soubory ve složce liší od /usr/local/openssl, nezapomeňte změnit z níže uvedených kroků.</span><span class="sxs-lookup"><span data-stu-id="42a27-145">If you want the files in a folder different than /usr/local/openssl, make sure to change the following as appropriate.</span></span>

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
<span data-ttu-id="42a27-146">Teď, když OpenSSL je správně nakonfigurován, budete muset zkompilovat ji převést svůj certifikát.</span><span class="sxs-lookup"><span data-stu-id="42a27-146">Now that OpenSSL is configured properly, you need to compile it to convert your certificate.</span></span> <span data-ttu-id="42a27-147">Kompilace, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="42a27-147">To compile, run the following command:</span></span>

```bash
make
```
<span data-ttu-id="42a27-148">Po dokončení kompilace jste připraveni k instalaci OpenSSL jako spustitelný soubor tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="42a27-148">Once compiling is complete, you're ready to install OpenSSL as an executable by running the following command:</span></span>
```bash
make install
```
<span data-ttu-id="42a27-149">Chcete-li potvrdit, že jste úspěšně nainstalovali OpenSSL ve vašem systému, spusťte následující příkaz a zkontrolujte, zda že získat stejný výstup.</span><span class="sxs-lookup"><span data-stu-id="42a27-149">To confirm that you've successfully installed OpenSSL on your system, run the following command and check to make sure you get the same output.</span></span>

```bash
/usr/local/openssl/bin/openssl version
```
<span data-ttu-id="42a27-150">V případě úspěchu, měli byste vidět následující zpráva.</span><span class="sxs-lookup"><span data-stu-id="42a27-150">If successful you should see the following message.</span></span>
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a><span data-ttu-id="42a27-151">Pro Windows</span><span class="sxs-lookup"><span data-stu-id="42a27-151">For Windows</span></span>
<span data-ttu-id="42a27-152">Instalace OpenSSL na počítači s Windows můžete udělat následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="42a27-152">Installing OpenSSL on a Windows PC can be done in the following ways:</span></span>
1. <span data-ttu-id="42a27-153">**(Doporučeno)**  Pomocí integrované funkce Bash pro systém Windows ve Windows 10 a vyšší, OpenSSL instaluje ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="42a27-153">**(Recommended)** Using the built-in Bash for Windows functionality in Window 10 and above, OpenSSL is installed by default.</span></span> <span data-ttu-id="42a27-154">Najdete pokyny k povolení funkce Bash pro Windows v systému Windows 10 [zde](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span><span class="sxs-lookup"><span data-stu-id="42a27-154">Instructions on how to enable Bash for Windows functionality in Windows 10 can be found [here](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span></span>
2. <span data-ttu-id="42a27-155">Prostřednictvím stahování k aplikaci Win32/64 poskytované komunitou.</span><span class="sxs-lookup"><span data-stu-id="42a27-155">Through downloading a Win32/64 application provided by the community.</span></span> <span data-ttu-id="42a27-156">Když OpenSSL Software Foundation neposkytuje ani neschvaluje žádné konkrétní instalační služba systému Windows, poskytují seznam k dispozici instalační programy [sem](https://wiki.openssl.org/index.php/Binaries)</span><span class="sxs-lookup"><span data-stu-id="42a27-156">While the OpenSSL Software Foundation does not provide or endorse any specific Windows installers, they provide a list of available installers [here](https://wiki.openssl.org/index.php/Binaries)</span></span>

### <a name="decode-your-certificate-file"></a><span data-ttu-id="42a27-157">Dekódovat soubor certifikátu</span><span class="sxs-lookup"><span data-stu-id="42a27-157">Decode your certificate file</span></span>
<span data-ttu-id="42a27-158">Stažený soubor kořenové certifikační Autority je v šifrovaném tvaru.</span><span class="sxs-lookup"><span data-stu-id="42a27-158">The downloaded Root CA file is in encrypted format.</span></span> <span data-ttu-id="42a27-159">Použijte OpenSSL k dekódování soubor certifikátu.</span><span class="sxs-lookup"><span data-stu-id="42a27-159">Use OpenSSL to decode the certificate file.</span></span> <span data-ttu-id="42a27-160">Chcete-li tak učinit, spusťte tento příkaz OpenSSL:</span><span class="sxs-lookup"><span data-stu-id="42a27-160">To do so, run this OpenSSL command:</span></span>

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a><span data-ttu-id="42a27-161">Připojení k databázi Azure pro PostgreSQL s ověřováním certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="42a27-161">Connecting to Azure Database for PostgreSQL with SSL certificate authentication</span></span>
<span data-ttu-id="42a27-162">Teď, když jste úspěšně dekódovat vašeho certifikátu, můžete nyní připojíte k databázovému serveru bezpečně přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="42a27-162">Now that you have successfully decoded your certificate, you can now connect to your database server securely over SSL.</span></span> <span data-ttu-id="42a27-163">Povolit certifikát ověření serveru, certifikát musí být umístěny v souboru ~/.postgresql/root.crt v domovském adresáři uživatele.</span><span class="sxs-lookup"><span data-stu-id="42a27-163">To allow server certificate verification, the certificate must be placed in the file ~/.postgresql/root.crt in the user's home directory.</span></span> <span data-ttu-id="42a27-164">(V systému Windows soubor je s názvem % APPDATA%\postgresql\root.crt.).</span><span class="sxs-lookup"><span data-stu-id="42a27-164">(On Microsoft Windows the file is named %APPDATA%\postgresql\root.crt.).</span></span> <span data-ttu-id="42a27-165">Následující část obsahuje pokyny pro připojení k databázi Azure pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="42a27-165">The following provides instructions for connecting to Azure Database for PostgreSQL.</span></span>

> [!NOTE]
> <span data-ttu-id="42a27-166">V současné době je známý problém. Pokud používáte "sslmode = ověřte úplná" v připojení ke službě, připojení se nezdaří s následující chybou: _certifikát serveru pro "&lt;oblast&gt;. control.database.windows.net" (a 7 jiné názvy) se neshoduje s názvem hostitele "&lt;servername&gt;. postgres.database.azure.com"._</span><span class="sxs-lookup"><span data-stu-id="42a27-166">Currently, there is a known issue if you use "sslmode=verify-full" in your connection to the service, the connection will fail with the following error: _server certificate for "&lt;region&gt;.control.database.windows.net" (and 7 other names) does not match host name "&lt;servername&gt;.postgres.database.azure.com"._</span></span>
> <span data-ttu-id="42a27-167">Pokud "sslmode = ověřte úplná" je potřeba, použijte zásady vytváření názvů serveru  **&lt;servername&gt;. database.windows.net** jako název hostitele vašeho připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="42a27-167">If "sslmode=verify-full" is required, please use the server naming convention **&lt;servername&gt;.database.windows.net** as your connection string host name.</span></span> <span data-ttu-id="42a27-168">Plánujeme odebrat v budoucnu toto omezení.</span><span class="sxs-lookup"><span data-stu-id="42a27-168">We plan to remove this limitation in the future.</span></span> <span data-ttu-id="42a27-169">Připojení pomocí dalších [SSL režimy](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) by měly být nadále používat zásady vytváření názvů upřednostňované hostitele  **&lt;servername&gt;. postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="42a27-169">Connections using other [SSL modes](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) should continue to use the preferred host naming convention **&lt;servername&gt;.postgres.database.azure.com**.</span></span>

#### <a name="using-psql-command-line-utility"></a><span data-ttu-id="42a27-170">Pomocí nástroje příkazového řádku psql</span><span class="sxs-lookup"><span data-stu-id="42a27-170">Using psql command-line utility</span></span>
<span data-ttu-id="42a27-171">Následující příklad ukazuje, jak se úspěšně připojit k serveru pomocí nástroje příkazového řádku psql PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="42a27-171">The following example shows you how to successfully connect to your PostgreSQL server using the psql command-line utility.</span></span> <span data-ttu-id="42a27-172">Použití `root.crt` soubor vytvořený a `sslmode=verify-ca` nebo `sslmode=verify-full` možnost.</span><span class="sxs-lookup"><span data-stu-id="42a27-172">Use the `root.crt` file created and the `sslmode=verify-ca` or `sslmode=verify-full` option.</span></span>

<span data-ttu-id="42a27-173">Pomocí rozhraní příkazového řádku PostgreSQL, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="42a27-173">Using the PostgreSQL command-line interface, execute the following command:</span></span>
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
<span data-ttu-id="42a27-174">Pokud bylo úspěšné, zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="42a27-174">If successful, you receive the following output:</span></span>
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

#### <a name="using-pgadmin-gui-tool"></a><span data-ttu-id="42a27-175">Pomocí nástroje pgAdmin grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="42a27-175">Using pgAdmin GUI tool</span></span>
<span data-ttu-id="42a27-176">Konfigurace pgAdmin 4 do zabezpečené připojení prostřednictvím protokolu SSL vyžaduje, abyste nastavili `SSL mode = Verify-CA` nebo `SSL mode = Verify-Full` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="42a27-176">Configuring pgAdmin 4 to connect securely over SSL requires you to set the `SSL mode = Verify-CA` or `SSL mode = Verify-Full` as follows:</span></span>

![Snímek obrazovky režimu protokolu SSL pgAdmin – připojení – vyžadovat](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a><span data-ttu-id="42a27-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42a27-178">Next steps</span></span>
<span data-ttu-id="42a27-179">Přečtěte si následující různé možnosti připojení aplikace [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="42a27-179">Review various application connectivity options following [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
