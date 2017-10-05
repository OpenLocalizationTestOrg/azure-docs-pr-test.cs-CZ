---
title: "Nasazení svítilna na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Informace o instalaci zásobníku svítilna na virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: ad69876bfbeba5f948a81e5c48c659fdf2265ae2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-lamp-stack-on-azure"></a>Nasazení svítilna zásobníku v Azure
Tento článek vás provede procesem nasazení webového serveru Apache, MySQL a PHP (svítilna stack) v Azure. Potřebujete účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2). K provedení těchto kroků můžete také využít [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-command-summary"></a>Rychlý příkaz souhrn

1. Ukládání a upravování [azuredeploy.parameters.json souboru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) na vaši volbu na místním počítači.
2. Spuštěním následujících příkazů pro vytvoření skupiny prostředků a potom nasadíte šablony:

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a>Nasazení svítilna na existující virtuální počítač
Následující příkazy balíčky aktualizací a poté nainstaluje Apache, MySQL a PHP:

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Nasazení svítilna na nový virtuální počítač návod

1. Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create) tak, aby obsahovala nového virtuálního počítače:

```azurecli
az group create -l westus -n myResourceGroup
```
Pokud chcete vytvořit virtuální počítač, můžete použít již napsané šablonu Azure Resource Manager [sem na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

2. Uložit [azuredeploy.parameters.json soubor](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) do místního počítače.
3. Upravit **azuredeploy.parameters.json** soubor upřednostňované vstupy.
4. Nasazení šablony s [nasazení skupiny az vytvořit] odkazování na soubor stažený json:

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

Výstup se podobá následujícímu příkladu:

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

Nyní jste vytvořili virtuální počítač s Linuxem se SVÍTILNOU již nainstalován. Pokud chcete, můžete ověřit instalaci přechod dolů na [ověřte svítilna úspěšně nainstaloval](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Nasazení svítilna na existující návod virtuálních počítačů
Pokud potřebujete pomoc, vytvoření virtuálního počítače s Linuxem, head [zde se dozvíte, jak vytvořit virtuální počítač s Linuxem](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli). Dále je třeba SSH do virtuálního počítače s Linuxem. Pokud potřebujete pomoc s vytvářením klíč SSH, head [zde se dozvíte, jak vytvořit klíč SSH na Linux nebo Mac.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Pokud již máte klíč SSH, přejděte dopředu a SSH z příkazového řádku do virtuálním počítačům s Linuxem pomocí `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.

Teď, když pracujete v rámci vašeho virtuálního počítače s Linuxem, jsme provede instalaci zásobníku svítilna na základě Debian distribuce. Přesný příkazy se mohou lišit pro ostatní distribucích systému Linux.

#### <a name="installing-on-debianubuntu"></a>Instalace na Debian/Ubuntu
Budete potřebovat následující balíčky nainstalované: `apache2`, `mysql-server`, `php5`, a `php5-mysql`. Tyto balíčky můžete nainstalovat přímo metodou tyto balíčky nebo pomocí Tasksel.
Před instalací musíte stáhnout a aktualizovat balíček seznamy.

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

Po spuštění některé z předchozích možností, zobrazí se výzva k instalaci těchto balíčků a různé další závislosti. Chcete-li nastavit heslo správce pro databázi MySQL, stiskněte "y" a potom "Enter, chcete pokračovat a budete postupovat podle dalších pokynů. Tento proces nainstaluje minimální požadované rozšíření PHP potřeba k použití PHP s MySQL. 

![][1]

Spusťte následující příkaz, který najdete v části Další rozšíření PHP, které jsou k dispozici jako balíčky:

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a>Vytvoření dokumentu info.php
Nyní byste měli být schopna provést kontrolu, jaká verze Apache, MySQL a PHP, máte pomocí příkazového řádku zadáním `apache2 -v`, `mysql -v`, nebo `php -v`.

Pokud chcete otestovat další, můžete vytvořit rychlé PHP stránku s informacemi o zobrazíte v prohlížeči. Vytvořte soubor s Nano textový editor se tento příkaz:

```bash
sudo nano /var/www/html/info.php
```

V rámci licence GNU Nano textový editor přidejte následující řádky:

```php
<?php
phpinfo();
?>
```

Potom uložte a ukončete textový editor.

Restartujte Apache pomocí tohoto příkazu, takže všechny nové instalace vstoupila v platnost.

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a>Ověřte svítilna úspěšně nainstalován.
Nyní můžete zkontrolovat stránku s informacemi o PHP, kterou jste vytvořili otevřením prohlížeče a přejdete na http://youruniqueDNS/info.php. By měla vypadat podobně jako tento obrázek.

![][2]

Apache instalaci můžete zkontrolovat pomocí zobrazení stránky výchozí Ubuntu Apache2 přechodem na http://youruniqueDNS/ můžete. Výstup se podobá následujícímu příkladu:

![][3]

Blahopřejeme, jste nastavili jen svítilna zásobníku na vašem virtuálním počítači Azure.

## <a name="next-steps"></a>Další kroky
Projděte si dokumentaci Ubuntu v zásobníku svítilna:

* [https://help.Ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
