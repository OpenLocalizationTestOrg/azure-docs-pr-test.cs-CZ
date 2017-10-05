---
title: "Nasazení uzlem 3 Deis clusteru | Microsoft Docs"
description: "Tento článek popisuje, jak vytvořit 3uzel Deis clusteru v Azure pomocí šablony Azure Resource Manager"
services: virtual-machines-linux
documentationcenter: 
author: HaishiBai
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5eb67eb7-95d4-461d-8eac-44925224ba5f
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/24/2015
ms.author: hbai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9a0c3dd7562dfb5ce54c2ebfd4665109f59cd8fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a>Nasaďte a nakonfigurujte 3uzlu Deis clusteru v Azure
Tento článek vás provede procesem zřizování [Deis](http://deis.io/) clusteru v Azure. Pokrývá všechny kroky ve vytváření potřebné certifikáty pro nasazování a škálování ukázku **přejděte** aplikaci na nově zřízeného clusteru.

Následující diagram znázorňuje architekturu nasazený systém. Spravuje správce systému na clusteru pomocí nástrojů, jako Deis **deis** a **deisctl**. Připojení se vytvoří pomocí služby Vyrovnávání zatížení Azure, který předává připojení k jednomu členu uzly v clusteru. Klienti přístup nasazené aplikace prostřednictvím také Vyrovnávání zatížení. V takovém případě předá nástroje pro vyrovnávání zatížení provozu, který Deis OK směrovač, který další routs provoz na odpovídající Docker kontejnery hostovaných v daném clusteru.

  ![Diagram architektury nasazené Desis clusteru](./media/deis-cluster/architecture-overview.png)

Aby bylo možné spustit následující kroky projdete, budete potřebovat:

* Aktivní předplatné Azure. Pokud nemáte, můžete získat volné záznamu [azure.com](https://azure.microsoft.com/).
* Id pracovní nebo školní použití skupin prostředků Azure. Pokud máte osobní účet a přihlaste se pomocí id společnosti Microsoft, budete muset [vytvořit id pracovní od vaše osobní](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Buď – v závislosti na váš klientský operační systém – [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo [Azure CLI pro Mac, Linux a Windows](../../cli-install-nodejs.md).
* [OpenSSL](https://www.openssl.org/). OpenSSL slouží ke generování potřebné certifikáty.
* Git klienta [Git Bash](https://git-scm.com/).
* Chcete-li otestovat vzorovou aplikaci, musíte také DNS server. Můžete použít všechny servery DNS nebo služby, které podporují záznamy se zástupným znakem A.
* Počítače ke spuštění Deis nástrojích klienta. Můžete použít místní počítač nebo virtuální počítač. Tyto nástroje můžete spustit na téměř jakoukoli distribuce systému Linux, ale Ubuntu použijte následující pokyny.

## <a name="provision-the-cluster"></a>Zřízení clusteru
V této části budete používat [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) šablony z úložiště s otevřeným zdrojem [šablon azure rychlý Start](https://github.com/Azure/azure-quickstart-templates). Nejprve budete poznamenejte šablony. Potom vytvoříte nový pár klíčů SSH pro ověřování. A pak budete konfigurovat nový identifikátor pro cluster můžete. A nakonec ke zřízení clusteru použijete skript prostředí nebo skript PowerShell.

1. Naklonujte úložiště: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. Přejděte do složky šablony:
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. Vytvořte nový pár klíčů SSH pomocí ssh-keygen:
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. Vygenerujte certifikát pomocí výše uvedených privátního klíče:
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]
5. Přejděte na [https://discovery.etcd.io/new](https://discovery.etcd.io/new) vygenerovat nový token clusteru, který vypadá zhruba v tomto tvaru:
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   Každý cluster CoreOS musí mít jedinečné tokenu z této služby service úrovně free. Najdete v tématu [CoreOS dokumentace](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) další podrobnosti.
6. Změnit **cloudu config.yaml** soubor nahradit stávající **zjišťování** tokenu s nový token:
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. Změnit **azuredeploy-Parameters.JSON tímto kódem** souboru: Otevřete certifikát, který jste vytvořili v kroku 4 v textovém editoru. Kopírovat veškerý text mezi `----BEGIN CERTIFICATE-----` a `-----END CERTIFICATE-----` do **sshKeyData** parametr (budete muset odebrat všechny znaky nového řádku).
8. Změnit **newStorageAccountName** parametr. Toto je účet úložiště pro disky s operačním systémem virtuálního počítače. Tento název účtu musí být globálně jedinečný.
9. Změnit **publicDomainName** parametr. To se stane součástí DNS název spojený s veřejnou IP nástroje pro vyrovnávání zatížení. Poslední plně kvalifikovaný název domény, bude mít formát *[hodnota tohoto parametru]*. *[Oblast]* . cloudapp.azure.com. Například pokud zadáte název jako deishbai32 a skupině prostředků se nasadí pro oblast západní USA, potom konečné plně kvalifikovaný název domény pro nástroj pro vyrovnávání zatížení bude deishbai32.westus.cloudapp.azure.com.
10. Uložte soubor parametrů. A pak můžete zřídit cluster pomocí prostředí Azure PowerShell:
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    nebo rozhraní příkazového řádku Azure:
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. Po zřízení skupiny prostředků uvidíte na portálu Azure classic všechny prostředky ve skupině. Jak je znázorněno na následujícím snímku obrazovky, skupina prostředků obsahuje virtuální síť s tři virtuální počítače, které jsou připojeny ke stejné sadě dostupnosti. Tato skupina obsahuje také Vyrovnávání zatížení, která má přidružené veřejnou IP adresu.
    
    ![Skupina zřízené prostředků na portálu Azure classic](./media/deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Instalace klienta
Je třeba **deisctl** na ovládací prvek vaší Deis clusteru. I když deisctl je automaticky nainstalován ve všech uzlech clusteru, je vhodné použít deisctl na samostatný počítač pro správu. Navíc vzhledem k tomu, že všechny uzly jsou nakonfigurované jenom privátních IP adres, musíte použít tunelování prostřednictvím Vyrovnávání zatížení, která má veřejnou IP adresu pro připojení k počítačům uzlu SSH. Následující kroky nastavení deisctl v samostatné Ubuntu fyzického nebo virtuálního počítače.

1. Instalace deisctl:mkdir deis
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. Přidejte svůj privátní klíč pro ssh agenta:
   
        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]
3. Nakonfigurujte deisctl:
   
        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

Šablona definuje příchozích pravidel NAT, které mapují 2223 do instance 1, 2224 instanci 2 a 2225 instanci 3. Toto použití nástroje deisctl poskytuje redundanci. Můžete zkontrolovat na portálu Azure classic tato pravidla:

![Pravidla NAT u nástroje pro vyrovnávání zatížení](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> Šablona aktuálně podporuje jenom 3 uzlu clusterů. To je kvůli omezením šablony Azure Resource Manageru definice pravidla NAT, který nepodporuje syntaxe smyčky.
> 
> 

## <a name="install-and-start-the-deis-platform"></a>Instalace a spuštění Deis platformy
Teď můžete použít k instalaci a spuštění deisctl Deis platformy:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> Spouštění platformou jeho zpracování chvíli trvá (až 10 minut). Spouštění služby Tvůrce zvlášť, může trvat dlouhou dobu. A někdy trvá několik pokusí úspěšné: Pokud operaci zdá se, že zablokuje, zkuste zadat `ctrl+c` rozdělit provedení příkazu, a opakujte.
> 
> 

Můžete použít `deisctl list` ověřit, zda jsou spuštěny všechny služby:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Blahopřejeme! Nyní máte k dispozici spuštěný Deis clsuter v Azure! Dále umožňuje nasadit ukázku, přejděte aplikace, abyste viděli clusteru v akci.

## <a name="deploy-and-scale-a-hello-world-application"></a>Zavádět a škálovat aplikace Hello World
Následující kroky ukazují, jak nasadit "Hello, World" přejděte aplikací do clusteru. Kroky jsou založené na [Deis dokumentaci](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Pro směrování síť fungovalo správně budete muset mít zástupný znak A záznam pro vaši doménu odkazující na veřejné IP adresy služby Vyrovnávání zatížení. Následující snímek obrazovky ukazuje záznamu A pro ukázkové domény registrace na GoDaddy:
   
    ![Záznam Godaddy A](./media/deis-cluster/go-daddy.png)
   
   <p />
2. Instalace deis:
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. Vytvořte nový klíč SSH a pak přidejte veřejný klíč do GitHub (samozřejmě můžete také znovu použít existující klíče). Pokud chcete vytvořit nový pár klíčů SSH, použijte:
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)
4. Přidejte id_rsa.pub nebo veřejný klíč podle svého výběru na Githubu. Můžete to provést pomocí přidat SSH klíče tlačítko na vaší obrazovce konfigurace klíče SSH:
   
   ![Klíč Githubu](./media/deis-cluster/github-key.png)
   
   <p />
5. Registrace nového uživatele:
   
        deis register http://deis.[your domain]
   <p />
6. Přidejte klíč SSH:
   
        deis keys:add [path to your SSH public key]
   <p />      
7. Vytvoření aplikace.
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p />
8.Git push aktivují imagí Dockeru vytvořená a nasazená, které bude trvat několik minut. Z mé prostředí v některých případech krok 10 (Pushing bitové kopie do privátní úložiště) může přestat reagovat. V takovém případě můžete zastavit proces, odeberte aplikace pomocí ' deis aplikace: zničit – a <application name> ` to remove the application and try again. You can use `deis apps:list' Chcete-li zjistit název vaší aplikace. Pokud všechno funguje, byste měli vidět něco podobného jako následující na konci výstupy příkazů:
   
        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. Ověřte, zda aplikace funguje:
   
        curl -S http://[your application name].[your domain]
   Měli byste vidět tohle:
   
        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
   <p />
10. Škálování aplikace na 3 instancí:
    
        deis scale cmd=3
    <p />
11. Volitelně můžete použít deis údaje a zkontrolujte podrobnosti o aplikaci. Jsou následující výstupy z nasazení Moje aplikace:
    
        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }
    
        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)
    
        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Další kroky
Tento článek vám projít všechny kroky pro zřízení nového Deis clusteru v Azure pomocí šablony Azure Resource Manager. Šablona podporuje redundance v tooling připojení a také Vyrovnávání zatížení pro nasazené aplikace. Šablona taky nevyužívá veřejné IP adresy na uzlech člen, které šetří cenné prostředky veřejné IP adresy a poskytuje prostředí s více zabezpečené hostování aplikací. Další informace naleznete v následujících článcích:

[Přehled Azure Resource Manageru][resource-group-overview]  
[Jak používat rozhraní příkazového řádku Azure][azure-command-line-tools]  
[Použití Azure PowerShell s Azure Resource Manager][powershell-azure-resource-manager]  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
