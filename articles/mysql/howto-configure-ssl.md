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
# <a name="configure-ssl-connectivity-in-your-application-toosecurely-connect-tooazure-database-for-mysql"></a>Konfigurace protokolu SSL připojení ve vaší aplikaci toosecurely connect tooAzure databáze pro databázi MySQL
Azure databáze pro databázi MySQL podporuje připojení databáze Azure pro aplikace tooclient MySQL serveru pomocí Secure Sockets Layer (SSL). Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed hello" šifrování hello datový proud mezi hello serveru a aplikace.

## <a name="step-1-obtain-ssl-certificate"></a>Krok 1: Získání certifikátu SSL
Stahování hello zamýšlený účel toocommunicate přes SSL s Azure databáze MySQL serveru od [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) a uložte místní tooyour soubor certifikátu hello disk (v tomto kurzu použili jsme c:\ssl).
**Aplikace Microsoft Internet Explorer a Microsoft Edge:** po dokončení stahování hello přejmenovat tooBaltimoreCyberTrustRoot.crt.pem certifikát hello.

## <a name="step-2-bind-ssl"></a>Krok 2: Vytvoření vazby SSL
### <a name="connecting-tooserver-using-hello-mysql-workbench-over-ssl"></a>Připojení tooserver pomocí hello MySQL Workbench přes protokol SSL
Nakonfigurujte MySQL Workbench tooconnect bezpečně přes protokol SSL. Přejděte toohello **SSL** ve hello MySQL Workbench na hello dialog nastavit nové připojení. Zadejte umístění souboru hello hello **BaltimoreCyberTrustRoot.crt.pem** v hello **souboru certifikační Autorita protokolu SSL:** pole.
![Uložte vlastní dlaždici](./media/howto-configure-ssl/mysql-workbench-ssl.png)

### <a name="connecting-tooserver-using-hello-mysql-cli-over-ssl"></a>Připojení tooserver pomocí rozhraní příkazového řádku MySQL hello přes protokol SSL
Pomocí rozhraní příkazového řádku hello MySQL, spusťte následující příkaz hello:
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>Krok 3: Vynucování připojení SSL v Azure 
### <a name="using-azure-portal"></a>Pomocí webu Azure Portal
Pomocí hello portál Azure, navštivte Azure databáze MySQL serveru a klikněte na **zabezpečení připojení**. Použít hello přepínací tlačítko tooenable nebo zakázat hello **připojení SSL vynutit** nastavení. Potom klikněte na **Uložit**. Společnost Microsoft doporučuje povolit tooalways **připojení SSL vynutit** nastavení pro lepší zabezpečení.
![povolit ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Použití Azure CLI
Můžete povolit nebo zakázat hello **ssl vynucení** parametr pomocí povoleno nebo zakázáno hodnot v uvedeném pořadí v rozhraní příkazového řádku Azure.
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a>Krok 4: Ověření připojení SSL
Spuštění hello mysql **stav** příkaz tooverify, že jste připojení tooyour MySQL serveru pomocí protokolu SSL:
```dos
mysql> status
```
Potvrďte, že připojení hello se šifrují kontrolou výstup hello. Měl by se zobrazit: **SSL: šifer používán je AES256 SHA** 

## <a name="sample-code"></a>Ukázka kódu
### <a name="php"></a>PHP
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
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
