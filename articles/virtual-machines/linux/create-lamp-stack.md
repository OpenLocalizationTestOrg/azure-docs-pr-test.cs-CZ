---
title: "aaaDeploy svítilna na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall hello svítilna zásobníku na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jluk
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: 42d887bb9f78becc02505e336be25fdaaf78df70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-lamp-stack-on-azure"></a>Nasazení svítilna zásobníku v Azure
Tento článek vás provede jak toodeploy platformě Apache webový server, MySQL a PHP (hello svítilna stack) v Azure. Potřebujete účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2). Můžete také provést tyto kroky hello [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-command-summary"></a>Rychlý příkaz souhrn

1. Ukládání a upravování hello [azuredeploy.parameters.json soubor](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour předvoleb na místním počítači.
2. Spusťte následující dva příkazy toocreate hello skupinu prostředků a potom nasazení vaší šablony:

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a>Nasazení svítilna na existující virtuální počítač
Hello následující příkazy balíčky aktualizací a poté nainstaluje Apache, MySQL a PHP:

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Nasazení svítilna na nový virtuální počítač návod

1. Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create) toocontain hello nového virtuálního počítače:

```azurecli
az group create -l westus -n myResourceGroup
```
toocreate hello virtuální počítač, můžete použít již napsané šablonu Azure Resource Manager [sem na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

2. Uložit hello [azuredeploy.parameters.json soubor](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour místního počítače.
3. Upravit hello **azuredeploy.parameters.json** tooyour souboru preferované vstupy.
4. Nasazení šablony hello s [nasazení skupiny az vytvořit] odkazující na hello stáhli soubor json:

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

Hello výstup je podobné toohello následující ukázka:

```json
{
"id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Resources/deployments/azuredeploy",
"name": "azuredeploy",
"properties": {
    "correlationId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "debugSetting": null,
}
...
"provisioningState": "Succeeded",
"template": null,
"templateLink": {
    "contentVersion": "1.0.0.0",
    "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json"
    },
    "timestamp": "2017-02-22T00:05:51.860411+00:00"
},
"resourceGroup": "myResourceGroup"
}
```

Nyní jste vytvořili virtuální počítač s Linuxem se SVÍTILNOU již nainstalován. Pokud chcete, můžete ověřit instalaci hello přechod dolů příliš[ověřte svítilna úspěšně nainstaloval](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Nasazení svítilna na existující návod virtuálních počítačů
Pokud potřebujete pomoc, vytvoření virtuálního počítače s Linuxem, head [sem toolearn jak toocreate virtuálního počítače s Linuxem](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli). Dále musíte tooSSH do hello virtuálního počítače s Linuxem. Pokud potřebujete pomoc s vytvářením klíč SSH, head [sem toolearn jak toocreate klíče SSH na Linux nebo Mac.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Pokud již máte klíč SSH, přejděte dopředu a SSH z příkazového řádku do virtuálním počítačům s Linuxem pomocí `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.

Teď, když pracujete v rámci vašeho virtuálního počítače s Linuxem, jsme provede instalaci hello svítilna zásobníku na základě Debian distribuce. přesný příkazy Hello se mohou lišit pro ostatní distribucích systému Linux.

#### <a name="installing-on-debianubuntu"></a>Instalace na Debian/Ubuntu
Budete potřebovat následující balíčky nainstalované hello: `apache2`, `mysql-server`, `php5`, a `php5-mysql`. Tyto balíčky můžete nainstalovat přímo metodou tyto balíčky nebo pomocí Tasksel.
Před instalací potřebovat toodownload a aktualizovat balíček seznamy.

```bash
sudo apt-get update
```

##### <a name="individual-packages"></a>Jednotlivé balíčky
Pomocí výstižný get:

```bash
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

##### <a name="using-tasksel"></a>Pomocí tasksel
Případně si můžete stáhnout Tasksel, Debian/Ubuntu nástroj, který nainstaluje více souvisejících balíčků jako koordinované "úloha" vašeho systému.

```bash
sudo apt-get install tasksel
sudo tasksel install lamp-server
```

Po spuštění, buď z předchozích možností hello, budete mít výzvami tooinstall tyto balíčky a různé další závislosti. tooset heslo správce pro databázi MySQL, stiskněte "y" a potom toocontinue, zadejte"a budete postupovat podle dalších pokynů. Tento proces nainstaluje hello minimální požadované PHP rozšíření potřeby toouse PHP s MySQL. 

![][1]

Spusťte následující příkaz toosee hello další rozšíření PHP, které jsou k dispozici jako balíčky:

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a>Vytvoření dokumentu info.php
Teď byste měli být schopný toocheck Apache, MySQL a PHP, jaká verze máte prostřednictvím hello příkazového řádku zadáním `apache2 -v`, `mysql -v`, nebo `php -v`.

Pokud by například tootest další, můžete vytvořit rychlé tooview PHP informace o stránku v prohlížeči. Vytvořte soubor s Nano textový editor se tento příkaz:

```bash
sudo nano /var/www/html/info.php
```

V rámci hello GNU Nano textový editor přidejte následující řádky hello:

```php
<?php
phpinfo();
?>
```

Potom uložte a zavřete hello textový editor.

Restartujte Apache pomocí tohoto příkazu, takže všechny nové instalace vstoupila v platnost.

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a>Ověřte svítilna úspěšně nainstalován.
Nyní můžete zkontrolovat, stránku s informacemi o PHP hello, kterou jste vytvořili otevřením prohlížeče a budete toohttp://youruniqueDNS/info.php. Měl by vypadat podobně jako toothis bitové kopie.

![][2]

Apache instalaci můžete zkontrolovat zobrazením hello Apache2 Ubuntu výchozí stránka přechodem tooyou http://youruniqueDNS/. Hello výstup je podobné toohello následující ukázka:

![][3]

Blahopřejeme, jste nastavili jen svítilna zásobníku na vašem virtuálním počítači Azure.

## <a name="next-steps"></a>Další kroky
Projděte si hello Ubuntu dokumentace v zásobníku svítilna hello:

* [https://help.Ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
