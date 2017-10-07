---
title: aaaDeploy Deis 3uzlu clusteru | Microsoft Docs
description: "Tento článek popisuje, jak toocreate uzlem 3 Deis clusteru v Azure pomocí šablony Azure Resource Manager"
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
ms.openlocfilehash: a4c0fb8cbb849264e64b433540157c9afecd184e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a>Nasaďte a nakonfigurujte 3uzlu Deis clusteru v Azure
Tento článek vás provede procesem zřizování [Deis](http://deis.io/) clusteru v Azure. Pokrývá všechny kroky hello z vytváření hello potřebné certifikáty toodeploying a škálování ukázku **přejděte** aplikaci na nově zřízeného clusteru hello.

Hello následující diagram ukazuje hello architekturu systému hello nasazení. Spravuje správce systému hello clusteru pomocí nástrojů, jako Deis **deis** a **deisctl**. Připojení se vytvoří pomocí služby Vyrovnávání zatížení Azure, který předává hello připojení tooone hello člena uzly v clusteru hello. Hello klientům přístup nasazené aplikace prostřednictvím služby Vyrovnávání zatížení hello také. V takovém případě nástroj pro vyrovnávání zatížení hello předává hello provoz tooa Deis OK směrovač, který další routs provoz toocorresponding Docker kontejnery hostovaná v clusteru hello.

  ![Diagram architektury nasazené Desis clusteru](./media/deis-cluster/architecture-overview.png)

V pořadí toorun prostřednictvím hello následující kroky budete potřebovat:

* Aktivní předplatné Azure. Pokud nemáte, můžete získat volné záznamu [azure.com](https://azure.microsoft.com/).
* Pracovní nebo školní id toouse prostředků Azure skupiny. Pokud máte osobní účet a přihlaste se pomocí id společnosti Microsoft, je třeba příliš[vytvořit id pracovní od vaše osobní](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Buď – v závislosti na váš klientský operační systém – hello [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo hello [Azure CLI pro Mac, Linux a Windows](../../cli-install-nodejs.md).
* [OpenSSL](https://www.openssl.org/). OpenSSL je použité toogenerate hello potřebné certifikáty.
* Git klienta [Git Bash](https://git-scm.com/).
* tootest hello ukázkovou aplikaci, musíte také DNS server. Můžete použít všechny servery DNS nebo služby, které podporují záznamy se zástupným znakem A.
* Počítač toorun Deis nástrojích klienta. Můžete použít místní počítač nebo virtuální počítač. Tyto nástroje můžete spustit na téměř jakoukoli distribuce systému Linux, ale hello následující pokyny používají Ubuntu.

## <a name="provision-hello-cluster"></a>Zřízení hello clusteru
V této části budete používat [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) šablony z úložiště s otevřeným zdrojem hello [šablon azure rychlý Start](https://github.com/Azure/azure-quickstart-templates). Nejprve budete poznamenejte hello šablony. Potom vytvoříte nový pár klíčů SSH pro ověřování. A pak budete konfigurovat nový identifikátor pro cluster můžete. A nakonec použijete skript prostředí hello nebo hello prostředí PowerShell skriptu tooprovision hello clusteru.

1. Klonování úložiště hello: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. Složka šablon přejděte toohello:
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. Vytvořte nový pár klíčů SSH pomocí ssh-keygen:
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. Vygenerujte certifikát pomocí hello výše privátního klíče:
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file toobe generated]
5. Přejděte příliš[https://discovery.etcd.io/new](https://discovery.etcd.io/new) toogenerate nový token clusteru, který vypadá zhruba v tomto tvaru:
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   Každý cluster CoreOS musí toohave jedinečný tokenu z této služby service úrovně free. Najdete v tématu [CoreOS dokumentace](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) další podrobnosti.
6. Upravit hello **cloudu config.yaml** souboru existující hello tooreplace **zjišťování** tokenu s hello nový token:
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment hello following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. Upravit hello **azuredeploy-Parameters.JSON tímto kódem** souboru: Otevřete hello certifikátů, které jste vytvořili v kroku 4 v textovém editoru. Kopírovat veškerý text mezi `----BEGIN CERTIFICATE-----` a `-----END CERTIFICATE-----` do hello **sshKeyData** parametr (budete potřebovat tooremove všechny znaky nového řádku).
8. Upravit hello **newStorageAccountName** parametr. Toto je hello účet úložiště pro disky s operačním systémem virtuálního počítače. Tento název účtu má toobe globálně jedinečný.
9. Upravit hello **publicDomainName** parametr. To se stane součástí hello DNS název spojený s veřejnou IP adresu aplikace hello zatížení vyrovnávání. Hello konečné plně kvalifikovaný název domény bude mít formát hello *[hodnota tohoto parametru]*. *[Oblast]* . cloudapp.azure.com. Například pokud zadáte název hello jako deishbai32 a skupina prostředků hello je nasazené toohello oblast západní USA, pak hello konečné bude plně kvalifikovaný název domény pro vyrovnávání zatížení tooyour deishbai32.westus.cloudapp.azure.com.
10. Uložte soubor parametrů hello. A pak můžete zřídit hello clusteru pomocí Azure PowerShell:
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    nebo rozhraní příkazového řádku Azure:
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. Po zřízení hello skupiny prostředků uvidíte všechny hello prostředky ve skupině hello na portálu Azure classic. Jak ukazuje následující snímek obrazovky, hello hello skupina prostředků obsahuje virtuální síť s tři virtuální počítače, které jsou připojené k toohello stejné skupině dostupnosti. Skupina Hello také obsahuje Vyrovnávání zatížení, která má přidružené veřejnou IP adresu.
    
    ![Hello zřídit skupinu prostředků na portálu Azure classic](./media/deis-cluster/resource-group.png)

## <a name="install-hello-client"></a>Instalace klienta na hello
Je třeba **deisctl** toocontrol vaše Deis clusteru. I když deisctl je automaticky nainstalován ve všech uzlech clusteru hello, je deisctl toouse dobrým zvykem na samostatný počítač pro správu. Navíc vzhledem k tomu, že všechny uzly jsou nakonfigurované jenom privátní IP adresy, budete potřebovat toouse tunelování SSH prostřednictvím hello Vyrovnávání zatížení, která má veřejnou IP adresu, tooconnect toohello uzlu počítače. Hello následují hello kroky nastavení deisctl na samostatné Ubuntu fyzické nebo virtuální počítač.

1. Instalace deisctl:mkdir deis
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. Přidáte vaší privátní klíče toossh agenta:
   
        eval `ssh-agent -s`
        ssh-add [path toohello private key file, see step 1 in hello previous section]
3. Nakonfigurujte deisctl:
   
        export DEISCTL_TUNNEL=[public ip of hello load balancer]:2223

Hello Šablona definuje příchozích pravidel NAT, které mapují 2223 tooinstance 1, 2224 tooinstance 2 a 2225 tooinstance 3. To poskytuje redundanci pro použití nástroje deisctl hello. Můžete zkontrolovat na portálu Azure classic tato pravidla:

![Pravidla NAT u nástroje pro vyrovnávání zatížení hello](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> Šablona hello aktuálně podporuje jenom 3 uzlu clusterů. To je kvůli omezením šablony Azure Resource Manageru definice pravidla NAT, který nepodporuje syntaxe smyčky.
> 
> 

## <a name="install-and-start-hello-deis-platform"></a>Instalace a spuštění hello Deis platformy
Teď můžete použít deisctl tooinstall a začít hello Deis platformy:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path toohello private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> Počáteční platformy hello jeho zpracování chvíli trvá (až 10 minut). Spouštění služby Tvůrce hello zvlášť, může trvat dlouhou dobu. A někdy trvá několik toosucceed pokusů: Pokud operace hello zdá se, že toohang, zkuste zadat `ctrl+c` toobreak provádění příkazu hello a zkuste to znovu.
> 
> 

Můžete použít `deisctl list` tooverify, pokud jsou spuštěné všechny služby:

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

Blahopřejeme! Nyní máte k dispozici spuštěný Deis clsuter v Azure! Dále umožňuje nasadit ukázku, přejděte aplikace toosee hello clusteru v akci.

## <a name="deploy-and-scale-a-hello-world-application"></a>Zavádět a škálovat aplikace Hello World
Hello následující kroky ukazují, jak toodeploy "Hello World" přejděte aplikace toohello clusteru. Hello kroky jsou založeny na [Deis dokumentaci](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Pro směrování toowork OK hello správně, budete potřebovat toohave zástupný znak A záznam pro vaši doménu odkazující toohello veřejné IP adresy služby Vyrovnávání zatížení hello. Hello následující snímek obrazovky ukazuje hello A záznam pro ukázkové domény registrace na GoDaddy:
   
    ![Záznam Godaddy A](./media/deis-cluster/go-daddy.png)
   
   <p />
2. Instalace deis:
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. Vytvořte nový klíč SSH a poté přidejte veřejný klíč tooGitHub hello (samozřejmě můžete také znovu použít existující klíče). toocreate nový pár klíčů SSH, použijte:
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s toouse default file names and empty passcode)
4. Přidejte id_rsa.pub nebo hello zvoleného tooGitHub veřejný klíč. Můžete to provést pomocí hello přidat SSH klíče tlačítko na vaší obrazovce konfigurace klíče SSH:
   
   ![Klíč Githubu](./media/deis-cluster/github-key.png)
   
   <p />
5. Registrace nového uživatele:
   
        deis register http://deis.[your domain]
   <p />
6. Přidejte klíč SSH hello:
   
        deis keys:add [path tooyour SSH public key]
   <p />      
7. Vytvoření aplikace.
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p />
8.Hello git push aktivují toobe bitové kopie Docker vytvořené a nasazení, který bude trvat několik minut. Z mé prostředí v některých případech krok 10 (Pushing bitové kopie tooprivate úložiště) může přestat reagovat. Pokud k tomu dojde, můžete zastavit proces hello odebrat hello aplikace pomocí ' deis aplikace: zrušení – a <application name> ` tooremove hello application and try again. You can use `deis apps:list' toofind out hello název vaší aplikace. Pokud všechno funguje, byste měli vidět něco podobného jako následující hello na konci hello výstupy příkazů:
   
        -----> Launching...
               done, lambda-underdog:v2 deployed tooDeis
               http://lambda-underdog.artitrack.com
               toolearn more, use `deis help` or visit http://deis.io
        toossh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. Ověřte, zda aplikace hello funguje:
   
        curl -S http://[your application name].[your domain]
   Měli byste vidět tohle:
   
        Welcome tooDeis!
        See hello documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list tooget hello name of your application).
   <p />
10. Škálování instancemi too3 aplikace hello:
    
        deis scale cmd=3
    <p />
11. Volitelně můžete deis podrobnosti tooexamine informace o vaší aplikace. Hello následující výstupy jsou z mé nasazení aplikace:
    
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
Tento článek vám projít všechny kroky tooprovision hello Deis nový cluster v Azure pomocí šablony Azure Resource Manager. Šablona Hello podporuje redundance v tooling připojení a také Vyrovnávání zatížení pro nasazené aplikace. Šablona Hello také nevyužívá veřejné IP adresy na uzlech člen, které šetří cenné prostředky veřejné IP adresy a poskytuje prostředí s více zabezpečené toohost aplikace. toolearn více, najdete v části hello následující články:

[Přehled Azure Resource Manageru][resource-group-overview]  
[Jak toouse hello rozhraní příkazového řádku Azure][azure-command-line-tools]  
[Použití Azure PowerShell s Azure Resource Manager][powershell-azure-resource-manager]  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
