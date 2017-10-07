---
title: "aaaDeploy svítilna na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Kurz – zásobník svítilna hello instalace na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: a3d0ecb3277f15bd0a2fdc0d85b738a760e68865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a>Nainstalujte svítilna webový server na virtuální počítač Azure
Tento článek vás provede jak toodeploy platformě Apache webový server, MySQL a PHP (hello svítilna stack) na virtuálního počítače s Ubuntu v Azure. Pokud dáváte přednost hello NGINX webový server, najdete v části hello [LEMP zásobníku](tutorial-lemp-stack.md) kurzu. toosee hello svítilna serveru v akci, můžete volitelně nainstalovat a nakonfigurovat web WordPress. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu (hello "L" v zásobníku svítilna hello)
> * Otevření portu 80 pro webový provoz
> * Instalace Apache, MySQL a PHP
> * Ověření instalace a konfigurace
> * Instalace aplikace WordPress na serveru svítilna hello


Další informace o hello svítilna zásobníku, včetně doporučení pro produkční prostředí, najdete v části hello [Ubuntu dokumentaci](https://help.ubuntu.com/community/ApacheMySQLPHP).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Instalace Apache, MySQL a PHP

Spusťte následující příkaz tooupdate Ubuntu balíček zdroje hello a nainstalujte Apache, MySQL a PHP. Všimněte si hello šipka nahoru (^) na konci hello hello příkazu.


```bash
sudo apt update && sudo apt install lamp-server^
```



Jste výzvami tooinstall hello balíčky a další závislosti. Po zobrazení výzvy nastavte kořenové heslo pro MySQL a potom toocontinue [Enter]. Postupujte podle zbývajících pokynů hello. Tento proces nainstaluje hello minimální požadované PHP rozšíření potřeby toouse PHP s MySQL. 

![Stránka MySQL kořenového hesla][1]

## <a name="verify-installation-and-configuration"></a>Ověření instalace a konfigurace


### <a name="apache"></a>Apache

Zkontrolujte verzi hello Apache s hello následující příkaz:
```bash
apache2 -v
```

S Apache nainstalovaná a port 80 otevřete tooyour virtuálních počítačů, hello webový server je nyní přístupná z hello Internetu. hello tooview Apache2 Ubuntu výchozí stránka otevřete webový prohlížeč a zadejte hello veřejnou IP adresu hello virtuálních počítačů. Použijte hello veřejnou IP adresu použitou tooSSH toohello virtuálních počítačů:

![Apache výchozí stránka][3]


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

Pokud chcete další tootest, vytvořte rychlé tooview PHP informace o stránku v prohlížeči. Hello následující příkaz vytvoří stránku s informacemi o PHP hello:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Nyní můžete zkontrolovat, stránku s informacemi o PHP hello, kterou jste vytvořili. Otevřete prohlížeč a přejděte příliš`http://yourPublicIPAddress/info.php`. Nahraďte hello veřejnou IP adresu vašeho virtuálního počítače. Měl by vypadat podobně jako toothis bitové kopie.

![Stránka informace o PHP][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nasadili server svítilna v Azure. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu
> * Otevření portu 80 pro webový provoz
> * Instalace Apache, MySQL a PHP
> * Ověření instalace a konfigurace
> * Instalace aplikace WordPress na serveru svítilna hello

Jak zálohy další kurz toolearn toohello toosecure webových serverů s certifikáty protokolu SSL.

> [!div class="nextstepaction"]
> [Zabezpečení webového serveru pomocí protokolu SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png