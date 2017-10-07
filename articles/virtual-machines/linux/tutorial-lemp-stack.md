---
title: "aaaDeploy LEMP na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Kurz – zásobník LEMP hello instalace na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: d8f9d84c5e9c0df4e9e985c10fe10f63a2f88214
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a>Nainstalujte LEMP webový server na virtuálním počítači Azure
Tento článek vás provede jak toodeploy NGINX webový server, MySQL a PHP (hello LEMP stack) na virtuálního počítače s Ubuntu v Azure. Zásobník LEMP Hello je alternativní toohello oblíbených [svítilna zásobníku](tutorial-lamp-stack.md), které si můžete také nainstalovat v Azure. toosee hello LEMP serveru v akci, můžete volitelně nainstalovat a nakonfigurovat web WordPress. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu (hello "L" v zásobníku LEMP hello)
> * Otevření portu 80 pro webový provoz
> * Instalace NGINX, MySQL a PHP
> * Ověření instalace a konfigurace
> * Instalace aplikace WordPress na serveru LEMP hello


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>Instalace NGINX, MySQL a PHP

Spusťte následující příkaz tooupdate Ubuntu balíček zdroje hello a nainstalujte NGINX, MySQL a PHP. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

Jste výzvami tooinstall hello balíčky a další závislosti. Po zobrazení výzvy nastavte kořenové heslo pro MySQL a potom toocontinue [Enter]. Postupujte podle zbývajících pokynů hello. Tento proces nainstaluje hello minimální požadované PHP rozšíření potřeby toouse PHP s MySQL. 

![Stránka MySQL kořenového hesla][1]

## <a name="verify-installation-and-configuration"></a>Ověření instalace a konfigurace


### <a name="nginx"></a>NGINX

Zkontrolujte verzi hello NGINX s hello následující příkaz:
```bash
nginx -v
```

S NGINX nainstalovaná a port 80 otevřete tooyour virtuálních počítačů, hello webový server je nyní přístupná z hello Internetu. tooview hello NGINX úvodní stránku, otevřete webový prohlížeč a zadejte hello veřejnou IP adresu hello virtuálních počítačů. Použijte hello veřejnou IP adresu použitou tooSSH toohello virtuálních počítačů:

![NGINX výchozí stránka][3]


### <a name="mysql"></a>MySQL

Zkontrolujte verzi hello MySQL s hello následující příkaz (Všimněte si, velké hello `V` parametr):

```bash
msql -V
```

Doporučujeme spustit následující skript toohelp zabezpečené hello instalaci MySQL hello:

```bash
mysql_secure_installation
```

Zadejte své heslo kořenové MySQL a konfigurovat nastavení zabezpečení hello pro vaše prostředí.

Pokud chcete toocreate databázi MySQL, přidat uživatele nebo změnit nastavení konfigurace, tooMySQL přihlášení:

```bash
mysql -u root -p
```

Až budete hotoví, ukončete hello mysql řádku zadáním `\q`.

### <a name="php"></a>PHP

Zkontrolujte hello verzi PHP s hello následující příkaz:

```bash
php -v
```

Nakonfigurujte NGINX toouse hello správce proces FastCGI PHP (PHP paměť FPM). Spusťte následující příkazy tooback původní server NGINX hello blokovat konfiguračního souboru a pak upravte hello původní soubor v editoru podle svého výběru hello:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

V editoru hello nahraďte obsah hello `/etc/nginx/sites-available/default` s následující hello. Viz hello poznámek vysvětlení hello nastavení. Nahraďte hello veřejnou IP adresu vašeho virtuálního počítače pro *yourPublicIPAddress*a nechat hello zbývající nastavení. Pak hello soubor uložte.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

Zkontrolujte konfiguraci NGINX hello chyby syntaxe:

```bash
sudo nginx -t
```

Pokud je správná syntaxe hello, restartujte NGINX s hello následující příkaz:

```bash
sudo service nginx restart
```

Pokud chcete další tootest, vytvořte rychlé tooview PHP informace o stránku v prohlížeči. Hello následující příkaz vytvoří stránku s informacemi o PHP hello:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Nyní můžete zkontrolovat, stránku s informacemi o PHP hello, kterou jste vytvořili. Otevřete prohlížeč a přejděte příliš`http://yourPublicIPAddress/info.php`. Nahraďte hello veřejnou IP adresu vašeho virtuálního počítače. Měl by vypadat podobně jako toothis bitové kopie.

![Stránka informace o PHP][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nasadili server LEMP v Azure. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu
> * Otevření portu 80 pro webový provoz
> * Instalace NGINX, MySQL a PHP
> * Ověření instalace a konfigurace
> * Instalace aplikace WordPress v zásobníku LEMP hello

Jak zálohy další kurz toolearn toohello toosecure webových serverů s certifikáty protokolu SSL.

> [!div class="nextstepaction"]
> [Zabezpečení webového serveru pomocí protokolu SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
