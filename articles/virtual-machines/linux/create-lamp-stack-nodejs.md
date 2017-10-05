---
title: "Nasazení na virtuální počítač s Linuxem pomocí Azure CLI 1.0 svítilna | Microsoft Docs"
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
ms.devlang: NA
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: feba2fb20d1831e92358ff5d1b4c9589d63d28dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-lamp-stack-with-the-azure-cli-10"></a>Nasazení svítilna zásobník 1.0 rozhraní příkazového řádku Azure
Tento článek vás provede procesem nasazení webového serveru Apache, MySQL a PHP (svítilna stack) v Azure. Potřebujete účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) tedy [připojený ke svému účtu Azure](../../xplat-cli-connect.md).

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy
K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

- [Azure CLI 1.0] – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)
- [Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* Nasazení svítilna na existující virtuální počítač

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Nasazení svítilna na nový virtuální počítač návod
Můžete spustit tak, že vytvoříte [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md) která bude obsahovat nový virtuální počítač:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Pokud chcete vytvořit virtuální počítač, můžete použít již napsané šablonu Azure Resource Manager [sem na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Byste měli vidět odpověď výzvy některé další vstupy:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Nyní jste vytvořili virtuální počítač s Linuxem se SVÍTILNOU již nainstalován. Pokud chcete, můžete ověřit instalaci přechod dolů na [ověřte svítilna úspěšně nainstaloval](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Nasazení svítilna na existující návod virtuálních počítačů
Pokud potřebujete pomoc, vytvoření virtuálního počítače s Linuxem, head [zde se dozvíte, jak vytvořit virtuální počítač s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dále je třeba SSH do virtuálního počítače s Linuxem. Pokud potřebujete pomoc s vytvářením klíč SSH, head [zde se dozvíte, jak vytvořit klíč SSH na Linux nebo Mac.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Pokud již máte klíč SSH, přejděte dopředu a SSH z příkazového řádku do virtuálním počítačům s Linuxem pomocí `ssh exampleUsername@exampleDNS`.

Teď, když pracujete v rámci vašeho virtuálního počítače s Linuxem, jsme provede instalaci zásobníku svítilna na základě Debian distribuce. Přesný příkazy se mohou lišit pro ostatní distribucích systému Linux.

#### <a name="installing-on-debianubuntu"></a>Instalace na Debian/Ubuntu
Budete potřebovat následující balíčky nainstalované: `apache2`, `mysql-server`, `php5`, a `php5-mysql`. Tyto balíčky můžete nainstalovat přímo metodou tyto balíčky nebo pomocí Tasksel. Pokyny pro obě možnosti jsou uvedeny níže.
Před instalací musíte stáhnout a aktualizovat balíček seznamy.

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a>Jednotlivé balíčky
Pomocí výstižný get:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Pomocí tasksel
Případně si můžete stáhnout Tasksel, Debian/Ubuntu nástroj, který nainstaluje více souvisejících balíčků jako koordinované "úloha" vašeho systému.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Po spuštění některé z předchozích možností, zobrazí se výzva k instalaci těchto balíčků a různé další závislosti. Stisknutím tlačítka 'y' a potom "Enter, chcete pokračovat a budete postupovat podle dalších pokynů k nastavení hesla pro správu pro databázi MySQL. Tím se nainstaluje minimální požadované rozšíření PHP potřeba k použití PHP s MySQL. 

![][1]

Spusťte následující příkaz, který najdete v části Další rozšíření PHP, které jsou k dispozici jako balíčky:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Vytvoření dokumentu info.php
Nyní byste měli být schopna provést kontrolu, jaká verze Apache, MySQL a PHP, máte pomocí příkazového řádku zadáním `apache2 -v`, `mysql -v`, nebo `php -v`.

Pokud chcete otestovat další, můžete vytvořit rychlé PHP stránku s informacemi o zobrazíte v prohlížeči. Vytvořte soubor s Nano textový editor se tento příkaz:

    user@ubuntu$ sudo nano /var/www/html/info.php

V rámci licence GNU Nano textový editor přidejte následující řádky:

    <?php
    phpinfo();
    ?>

Potom uložte a ukončete textový editor.

Restartujte Apache pomocí tohoto příkazu, takže všechny nové instalace vstoupila v platnost.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Ověřte svítilna úspěšně nainstalován.
Nyní můžete zkontrolovat stránku s informacemi o PHP, kterou jste vytvořili otevřením prohlížeče a přejdete na http://youruniqueDNS/info.php. By měla vypadat podobně jako tento obrázek.

![][2]

Apache instalaci můžete zkontrolovat pomocí zobrazení stránky výchozí Ubuntu Apache2 přechodem na http://youruniqueDNS/ můžete. Měli byste vidět něco podobného jako tuto bitovou kopii.

![][3]

Blahopřejeme, jste nastavili jen svítilna zásobníku na vašem virtuálním počítači Azure.

## <a name="next-steps"></a>Další kroky
Projděte si dokumentaci Ubuntu v zásobníku svítilna:

* [https://help.Ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png