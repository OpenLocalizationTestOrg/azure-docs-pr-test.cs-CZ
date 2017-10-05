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
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Konfigurace připojení SSL v aplikaci pro zabezpečené připojení k databázi Azure pro databázi MySQL
Azure databáze pro databázi MySQL podporuje připojení databáze Azure pro server databáze MySQL pro klientské aplikace pomocí Secure Sockets Layer (SSL). Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.

## <a name="step-1-obtain-ssl-certificate"></a>Krok 1: Získání certifikátu SSL
Stáhněte si certifikát potřebné pro komunikaci pomocí protokolu SSL s Azure databáze MySQL serveru od [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) a uložte soubor certifikátu na místní disk (v tomto kurzu použili jsme c:\ssl).
**Aplikace Microsoft Internet Explorer a Microsoft Edge:** po dokončení stahování, přejmenujte BaltimoreCyberTrustRoot.crt.pem certifikát.

## <a name="step-2-bind-ssl"></a>Krok 2: Vytvoření vazby SSL
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>Připojování k serveru pomocí nástroje MySQL Workbench přes protokol SSL
Nakonfigurujte MySQL Workbench pro zabezpečené připojení prostřednictvím protokolu SSL. Přejděte na **SSL** ve MySQL Workbench o dialogu nastavit nové připojení. Zadejte umístění souboru **BaltimoreCyberTrustRoot.crt.pem** v **souboru certifikační Autorita protokolu SSL:** pole.
![Uložte vlastní dlaždici](./media/howto-configure-ssl/mysql-workbench-ssl.png)

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Připojování k serveru pomocí rozhraní příkazového řádku MySQL přes protokol SSL
Pomocí rozhraní příkazového řádku MySQL, spusťte následující příkaz:
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>Krok 3: Vynucování připojení SSL v Azure 
### <a name="using-azure-portal"></a>Pomocí webu Azure Portal
Pomocí portálu Azure přejděte Azure databáze MySQL serveru a klikněte na **zabezpečení připojení**. Přepínací tlačítko použít k povolení nebo zakázání **připojení SSL vynutit** nastavení. Potom klikněte na **Uložit**. Microsoft doporučuje, vždy povolit **připojení SSL vynutit** nastavení pro lepší zabezpečení.
![povolit ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Použití Azure CLI
Můžete povolit nebo zakázat **ssl vynucení** parametr pomocí povoleno nebo zakázáno hodnot v uvedeném pořadí v rozhraní příkazového řádku Azure.
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a>Krok 4: Ověření připojení SSL
Spuštění mysql **stav** příkazu ověřte, že jste připojení k serveru databáze MySQL pomocí protokolu SSL:
```dos
mysql> status
```
Potvrďte, že připojení je zašifrovaná kontrolou výstupu. Měl by se zobrazit: **SSL: šifer používán je AES256 SHA** 

## <a name="sample-code"></a>Ukázka kódu
### <a name="php"></a>PHP
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a>Python
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a>Další kroky
Přečtěte si následující různé možnosti připojení aplikace [knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
