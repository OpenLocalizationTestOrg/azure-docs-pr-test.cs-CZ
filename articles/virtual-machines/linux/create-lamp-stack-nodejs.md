---
title: "aaaDeploy svítilna na virtuální počítač s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
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
ms.devlang: NA
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: e78a82d388ce68710933b9b673aa1b2460bdbb14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-lamp-stack-with-hello-azure-cli-10"></a>Nasazení svítilna zásobník hello Azure CLI 1.0
Tento článek vás provede jak toodeploy platformě Apache webový server, MySQL a PHP (hello svítilna stack) v Azure. Potřebujete účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) tedy [připojené tooyour účet Azure](../../xplat-cli-connect.md).

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0] – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

```
# One command toocreate a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* Nasazení svítilna na existující virtuální počítač

```
# Two commands: one updates packages, hello other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Nasazení svítilna na nový virtuální počítač návod
Můžete spustit tak, že vytvoříte [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md) která bude obsahovat hello nového virtuálního počítače:

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

toocreate hello virtuální počítač, můžete použít již napsané šablonu Azure Resource Manager [sem na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Byste měli vidět odpověď výzvy některé další vstupy:

    info:    Executing command group deployment create
    info:    Supply values for hello following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment toocomplete
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

Nyní jste vytvořili virtuální počítač s Linuxem se SVÍTILNOU již nainstalován. Pokud chcete, můžete ověřit instalaci hello přechod dolů příliš[ověřte svítilna úspěšně nainstaloval](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Nasazení svítilna na existující návod virtuálních počítačů
Pokud potřebujete pomoc, vytvoření virtuálního počítače s Linuxem, head [sem toolearn jak toocreate virtuálního počítače s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dále musíte tooSSH do hello virtuálního počítače s Linuxem. Pokud potřebujete pomoc s vytvářením klíč SSH, head [sem toolearn jak toocreate klíče SSH na Linux nebo Mac.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Pokud již máte klíč SSH, přejděte dopředu a SSH z příkazového řádku do virtuálním počítačům s Linuxem pomocí `ssh exampleUsername@exampleDNS`.

Teď, když pracujete v rámci vašeho virtuálního počítače s Linuxem, jsme provede instalaci hello svítilna zásobníku na základě Debian distribuce. přesný příkazy Hello se mohou lišit pro ostatní distribucích systému Linux.

#### <a name="installing-on-debianubuntu"></a>Instalace na Debian/Ubuntu
Budete potřebovat následující balíčky nainstalované hello: `apache2`, `mysql-server`, `php5`, a `php5-mysql`. Tyto balíčky můžete nainstalovat přímo metodou tyto balíčky nebo pomocí Tasksel. Pokyny pro obě možnosti jsou uvedeny níže.
Před instalací potřebovat toodownload a aktualizovat balíček seznamy.

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a>Jednotlivé balíčky
Pomocí výstižný get:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Pomocí tasksel
Případně si můžete stáhnout Tasksel, Debian/Ubuntu nástroj, který nainstaluje více souvisejících balíčků jako koordinované "úloha" vašeho systému.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Po spuštění, buď z předchozích možností hello, budete mít výzvami tooinstall tyto balíčky a různé další závislosti. Stiskněte "y" a potom toocontinue, zadejte"a podle jakékoli další výzvy tooset hesla pro správu pro databázi MySQL. Tím se nainstaluje hello minimální požadované PHP rozšíření potřeby toouse PHP s MySQL. 

![][1]

Spusťte následující příkaz toosee hello další rozšíření PHP, které jsou k dispozici jako balíčky:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Vytvoření dokumentu info.php
Teď byste měli být schopný toocheck Apache, MySQL a PHP, jaká verze máte prostřednictvím hello příkazového řádku zadáním `apache2 -v`, `mysql -v`, nebo `php -v`.

Pokud by například tootest další, můžete vytvořit rychlé tooview PHP informace o stránku v prohlížeči. Vytvořte soubor s Nano textový editor se tento příkaz:

    user@ubuntu$ sudo nano /var/www/html/info.php

V rámci hello GNU Nano textový editor přidejte následující řádky hello:

    <?php
    phpinfo();
    ?>

Potom uložte a zavřete hello textový editor.

Restartujte Apache pomocí tohoto příkazu, takže všechny nové instalace vstoupila v platnost.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Ověřte svítilna úspěšně nainstalován.
Nyní můžete zkontrolovat, stránku s informacemi o PHP hello, kterou jste vytvořili otevřením prohlížeče a budete toohttp://youruniqueDNS/info.php. Měl by vypadat podobně jako toothis bitové kopie.

![][2]

Apache instalaci můžete zkontrolovat zobrazením hello Apache2 Ubuntu výchozí stránka přechodem tooyou http://youruniqueDNS/. Měli byste vidět něco podobného jako tuto bitovou kopii.

![][3]

Blahopřejeme, jste nastavili jen svítilna zásobníku na vašem virtuálním počítači Azure.

## <a name="next-steps"></a>Další kroky
Projděte si hello Ubuntu dokumentace v zásobníku svítilna hello:

* [https://help.Ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png