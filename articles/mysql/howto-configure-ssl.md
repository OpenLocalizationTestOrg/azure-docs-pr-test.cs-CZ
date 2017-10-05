---
title: "Konfigurace připojení SSL k bezpečnému připojování k databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Pokyny, jak správně nakonfigurovat databázi Azure pro MySQL a přidružené aplikace správně používat připojení SSL"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 07/28/2017
ms.openlocfilehash: 77e1b6266a2cf47949fa06358ec003f6b6b38065
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a><span data-ttu-id="fb617-103">Konfigurace připojení SSL v aplikaci pro zabezpečené připojení k databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="fb617-103">Configure SSL connectivity in your application to securely connect to Azure Database for MySQL</span></span>
<span data-ttu-id="fb617-104">Azure databáze pro databázi MySQL podporuje připojení databáze Azure pro server databáze MySQL pro klientské aplikace pomocí Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="fb617-104">Azure Database for MySQL supports connecting your Azure Database for MySQL server to client applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="fb617-105">Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.</span><span class="sxs-lookup"><span data-stu-id="fb617-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" attacks by encrypting the data stream between the server and your application.</span></span>

## <a name="step-1-obtain-ssl-certificate"></a><span data-ttu-id="fb617-106">Krok 1: Získání certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="fb617-106">Step 1: Obtain SSL Certificate</span></span>
<span data-ttu-id="fb617-107">Stáhněte si certifikát potřebné pro komunikaci pomocí protokolu SSL s Azure databáze MySQL serveru od [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) a uložte soubor certifikátu na místní disk (v tomto kurzu použili jsme c:\ssl).</span><span class="sxs-lookup"><span data-stu-id="fb617-107">Download the certificate needed to communicate over SSL with your Azure Database for MySQL server from [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) and save the certificate file to your local drive (with this tutorial, we used c:\ssl).</span></span>
<span data-ttu-id="fb617-108">**Aplikace Microsoft Internet Explorer a Microsoft Edge:** po dokončení stahování, přejmenujte BaltimoreCyberTrustRoot.crt.pem certifikát.</span><span class="sxs-lookup"><span data-stu-id="fb617-108">**For Microsoft Internet Explorer and Microsoft Edge:** After the download has completed, rename the certificate to BaltimoreCyberTrustRoot.crt.pem.</span></span>

## <a name="step-2-bind-ssl"></a><span data-ttu-id="fb617-109">Krok 2: Vytvoření vazby SSL</span><span class="sxs-lookup"><span data-stu-id="fb617-109">Step 2: Bind SSL</span></span>
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a><span data-ttu-id="fb617-110">Připojování k serveru pomocí nástroje MySQL Workbench přes protokol SSL</span><span class="sxs-lookup"><span data-stu-id="fb617-110">Connecting to server using the MySQL Workbench over SSL</span></span>
<span data-ttu-id="fb617-111">Nakonfigurujte MySQL Workbench pro zabezpečené připojení prostřednictvím protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="fb617-111">Configure MySQL Workbench to connect securely over SSL.</span></span> <span data-ttu-id="fb617-112">Přejděte na **SSL** ve MySQL Workbench o dialogu nastavit nové připojení.</span><span class="sxs-lookup"><span data-stu-id="fb617-112">Navigate to the **SSL** tab in the MySQL Workbench on the Setup New Connection dialogue.</span></span> <span data-ttu-id="fb617-113">Zadejte umístění souboru **BaltimoreCyberTrustRoot.crt.pem** v **souboru certifikační Autorita protokolu SSL:** pole.</span><span class="sxs-lookup"><span data-stu-id="fb617-113">Enter the file location of the **BaltimoreCyberTrustRoot.crt.pem** in the **SSL CA File:** field.</span></span>
<span data-ttu-id="fb617-114">![Uložte vlastní dlaždici](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="fb617-114">![save customized tile](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span></span>

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a><span data-ttu-id="fb617-115">Připojování k serveru pomocí rozhraní příkazového řádku MySQL přes protokol SSL</span><span class="sxs-lookup"><span data-stu-id="fb617-115">Connecting to server using the MySQL CLI over SSL</span></span>
<span data-ttu-id="fb617-116">Pomocí rozhraní příkazového řádku MySQL, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fb617-116">Using the MySQL command-line interface, execute the following command:</span></span>
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a><span data-ttu-id="fb617-117">Krok 3: Vynucování připojení SSL v Azure</span><span class="sxs-lookup"><span data-stu-id="fb617-117">Step 3:  Enforcing SSL connections in Azure</span></span> 
### <a name="using-azure-portal"></a><span data-ttu-id="fb617-118">Pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fb617-118">Using Azure portal</span></span>
<span data-ttu-id="fb617-119">Pomocí portálu Azure přejděte Azure databáze MySQL serveru a klikněte na **zabezpečení připojení**.</span><span class="sxs-lookup"><span data-stu-id="fb617-119">Using the Azure portal, visit your Azure Database for MySQL server and click **Connection security**.</span></span> <span data-ttu-id="fb617-120">Přepínací tlačítko použít k povolení nebo zakázání **připojení SSL vynutit** nastavení.</span><span class="sxs-lookup"><span data-stu-id="fb617-120">Use the toggle button to enable or disable the **Enforce SSL connection** setting.</span></span> <span data-ttu-id="fb617-121">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fb617-121">Then click **Save**.</span></span> <span data-ttu-id="fb617-122">Microsoft doporučuje, vždy povolit **připojení SSL vynutit** nastavení pro lepší zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fb617-122">Microsoft recommends to always enable **Enforce SSL connection** setting for enhanced security.</span></span>
<span data-ttu-id="fb617-123">![povolit ssl](./media/howto-configure-ssl/enable-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="fb617-123">![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="fb617-124">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fb617-124">Using Azure CLI</span></span>
<span data-ttu-id="fb617-125">Můžete povolit nebo zakázat **ssl vynucení** parametr pomocí povoleno nebo zakázáno hodnot v uvedeném pořadí v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="fb617-125">You can enable or disable the **ssl-enforcement** parameter using Enabled or Disabled values respectively in Azure CLI.</span></span>
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a><span data-ttu-id="fb617-126">Krok 4: Ověření připojení SSL</span><span class="sxs-lookup"><span data-stu-id="fb617-126">Step 4: Verify SSL Connection</span></span>
<span data-ttu-id="fb617-127">Spuštění mysql **stav** příkazu ověřte, že jste připojení k serveru databáze MySQL pomocí protokolu SSL:</span><span class="sxs-lookup"><span data-stu-id="fb617-127">Execute the mysql **status** command to verify that you have connected to your MySQL server using SSL:</span></span>
```dos
mysql> status
```
<span data-ttu-id="fb617-128">Potvrďte, že připojení je zašifrovaná kontrolou výstupu.</span><span class="sxs-lookup"><span data-stu-id="fb617-128">Confirm the connection is encrypted by reviewing the output.</span></span> <span data-ttu-id="fb617-129">Měl by se zobrazit: **SSL: šifer používán je AES256 SHA**</span><span class="sxs-lookup"><span data-stu-id="fb617-129">It should show:  **SSL: Cipher in use is AES256-SHA**</span></span> 

## <a name="sample-code"></a><span data-ttu-id="fb617-130">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="fb617-130">Sample code</span></span>
### <a name="php"></a><span data-ttu-id="fb617-131">PHP</span><span class="sxs-lookup"><span data-stu-id="fb617-131">PHP</span></span>
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a><span data-ttu-id="fb617-132">Python</span><span class="sxs-lookup"><span data-stu-id="fb617-132">Python</span></span>
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a><span data-ttu-id="fb617-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb617-133">Next steps</span></span>
<span data-ttu-id="fb617-134">Přečtěte si následující různé možnosti připojení aplikace [knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="fb617-134">Review various application connectivity options following [Connection libraries for Azure Database for MySQL](concepts-connection-libraries.md)</span></span>
