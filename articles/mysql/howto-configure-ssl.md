---
title: "toosecurely připojení SSL aaaConfigure připojit tooAzure databáze pro databázi MySQL | Microsoft Docs"
description: "Pokyny, jak nakonfigurovat tooproperly Azure databáze MySQL a přidružené aplikace toocorrectly pomocí připojení SSL"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 07/28/2017
ms.openlocfilehash: 8c37c19d4c101abfb730f429a19441e94e52fc85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ssl-connectivity-in-your-application-toosecurely-connect-tooazure-database-for-mysql"></a><span data-ttu-id="fa6ee-103">Konfigurace protokolu SSL připojení ve vaší aplikaci toosecurely connect tooAzure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="fa6ee-103">Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL</span></span>
<span data-ttu-id="fa6ee-104">Azure databáze pro databázi MySQL podporuje připojení databáze Azure pro aplikace tooclient MySQL serveru pomocí Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="fa6ee-104">Azure Database for MySQL supports connecting your Azure Database for MySQL server tooclient applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="fa6ee-105">Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed hello" šifrování hello datový proud mezi hello serveru a aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in hello middle" attacks by encrypting hello data stream between hello server and your application.</span></span>

## <a name="step-1-obtain-ssl-certificate"></a><span data-ttu-id="fa6ee-106">Krok 1: Získání certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="fa6ee-106">Step 1: Obtain SSL Certificate</span></span>
<span data-ttu-id="fa6ee-107">Stahování hello zamýšlený účel toocommunicate přes SSL s Azure databáze MySQL serveru od [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) a uložte místní tooyour soubor certifikátu hello disk (v tomto kurzu použili jsme c:\ssl).</span><span class="sxs-lookup"><span data-stu-id="fa6ee-107">Download hello certificate needed toocommunicate over SSL with your Azure Database for MySQL server from [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) and save hello certificate file tooyour local drive (with this tutorial, we used c:\ssl).</span></span>
<span data-ttu-id="fa6ee-108">**Aplikace Microsoft Internet Explorer a Microsoft Edge:** po dokončení stahování hello přejmenovat tooBaltimoreCyberTrustRoot.crt.pem certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-108">**For Microsoft Internet Explorer and Microsoft Edge:** After hello download has completed, rename hello certificate tooBaltimoreCyberTrustRoot.crt.pem.</span></span>

## <a name="step-2-bind-ssl"></a><span data-ttu-id="fa6ee-109">Krok 2: Vytvoření vazby SSL</span><span class="sxs-lookup"><span data-stu-id="fa6ee-109">Step 2: Bind SSL</span></span>
### <a name="connecting-tooserver-using-hello-mysql-workbench-over-ssl"></a><span data-ttu-id="fa6ee-110">Připojení tooserver pomocí hello MySQL Workbench přes protokol SSL</span><span class="sxs-lookup"><span data-stu-id="fa6ee-110">Connecting tooserver using hello MySQL Workbench over SSL</span></span>
<span data-ttu-id="fa6ee-111">Nakonfigurujte MySQL Workbench tooconnect bezpečně přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-111">Configure MySQL Workbench tooconnect securely over SSL.</span></span> <span data-ttu-id="fa6ee-112">Přejděte toohello **SSL** ve hello MySQL Workbench na hello dialog nastavit nové připojení.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-112">Navigate toohello **SSL** tab in hello MySQL Workbench on hello Setup New Connection dialogue.</span></span> <span data-ttu-id="fa6ee-113">Zadejte umístění souboru hello hello **BaltimoreCyberTrustRoot.crt.pem** v hello **souboru certifikační Autorita protokolu SSL:** pole.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-113">Enter hello file location of hello **BaltimoreCyberTrustRoot.crt.pem** in hello **SSL CA File:** field.</span></span>
<span data-ttu-id="fa6ee-114">![Uložte vlastní dlaždici](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="fa6ee-114">![save customized tile](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span></span>

### <a name="connecting-tooserver-using-hello-mysql-cli-over-ssl"></a><span data-ttu-id="fa6ee-115">Připojení tooserver pomocí rozhraní příkazového řádku MySQL hello přes protokol SSL</span><span class="sxs-lookup"><span data-stu-id="fa6ee-115">Connecting tooserver using hello MySQL CLI over SSL</span></span>
<span data-ttu-id="fa6ee-116">Pomocí rozhraní příkazového řádku hello MySQL, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="fa6ee-116">Using hello MySQL command-line interface, execute hello following command:</span></span>
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a><span data-ttu-id="fa6ee-117">Krok 3: Vynucování připojení SSL v Azure</span><span class="sxs-lookup"><span data-stu-id="fa6ee-117">Step 3:  Enforcing SSL connections in Azure</span></span> 
### <a name="using-azure-portal"></a><span data-ttu-id="fa6ee-118">Pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fa6ee-118">Using Azure portal</span></span>
<span data-ttu-id="fa6ee-119">Pomocí hello portál Azure, navštivte Azure databáze MySQL serveru a klikněte na **zabezpečení připojení**.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-119">Using hello Azure portal, visit your Azure Database for MySQL server and click **Connection security**.</span></span> <span data-ttu-id="fa6ee-120">Použít hello přepínací tlačítko tooenable nebo zakázat hello **připojení SSL vynutit** nastavení.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-120">Use hello toggle button tooenable or disable hello **Enforce SSL connection** setting.</span></span> <span data-ttu-id="fa6ee-121">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-121">Then click **Save**.</span></span> <span data-ttu-id="fa6ee-122">Společnost Microsoft doporučuje povolit tooalways **připojení SSL vynutit** nastavení pro lepší zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-122">Microsoft recommends tooalways enable **Enforce SSL connection** setting for enhanced security.</span></span>
<span data-ttu-id="fa6ee-123">![povolit ssl](./media/howto-configure-ssl/enable-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="fa6ee-123">![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="fa6ee-124">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fa6ee-124">Using Azure CLI</span></span>
<span data-ttu-id="fa6ee-125">Můžete povolit nebo zakázat hello **ssl vynucení** parametr pomocí povoleno nebo zakázáno hodnot v uvedeném pořadí v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-125">You can enable or disable hello **ssl-enforcement** parameter using Enabled or Disabled values respectively in Azure CLI.</span></span>
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a><span data-ttu-id="fa6ee-126">Krok 4: Ověření připojení SSL</span><span class="sxs-lookup"><span data-stu-id="fa6ee-126">Step 4: Verify SSL Connection</span></span>
<span data-ttu-id="fa6ee-127">Spuštění hello mysql **stav** příkaz tooverify, že jste připojení tooyour MySQL serveru pomocí protokolu SSL:</span><span class="sxs-lookup"><span data-stu-id="fa6ee-127">Execute hello mysql **status** command tooverify that you have connected tooyour MySQL server using SSL:</span></span>
```dos
mysql> status
```
<span data-ttu-id="fa6ee-128">Potvrďte, že připojení hello se šifrují kontrolou výstup hello.</span><span class="sxs-lookup"><span data-stu-id="fa6ee-128">Confirm hello connection is encrypted by reviewing hello output.</span></span> <span data-ttu-id="fa6ee-129">Měl by se zobrazit: **SSL: šifer používán je AES256 SHA**</span><span class="sxs-lookup"><span data-stu-id="fa6ee-129">It should show:  **SSL: Cipher in use is AES256-SHA**</span></span> 

## <a name="sample-code"></a><span data-ttu-id="fa6ee-130">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="fa6ee-130">Sample code</span></span>
### <a name="php"></a><span data-ttu-id="fa6ee-131">PHP</span><span class="sxs-lookup"><span data-stu-id="fa6ee-131">PHP</span></span>
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a><span data-ttu-id="fa6ee-132">Python</span><span class="sxs-lookup"><span data-stu-id="fa6ee-132">Python</span></span>
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a><span data-ttu-id="fa6ee-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa6ee-133">Next steps</span></span>
<span data-ttu-id="fa6ee-134">Přečtěte si následující různé možnosti připojení aplikace [knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="fa6ee-134">Review various application connectivity options following [Connection libraries for Azure Database for MySQL](concepts-connection-libraries.md)</span></span>
