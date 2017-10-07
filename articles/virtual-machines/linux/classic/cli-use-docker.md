---
title: "hello aaaUsing Docker rozšíření virtuálního počítače pro systém Linux na Azure"
description: "Popisuje Docker a rozšíření Azure Virtual Machines hello a ukazuje, jak tooprogrammatically vytvořit virtuální počítače na platformě Azure, které jsou hostitelů docker z příkazového řádku hello pomocí hello rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: eaff75e3-d929-4931-a4a1-8c377a8e7302
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/29/2016
ms.author: rasquill
ms.openlocfilehash: 1e192ad7c273aa9c997ea7bfa53b7de0b41a43c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-from-hello-azure-command-line-interface-azure-cli"></a>Pomocí hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Informace o rozšíření virtuálního počítače Docker hello pomocí modelu Resource Manager hello najdete v tématu [zde](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Toto téma popisuje, jak toocreate virtuální počítač s hello Docker rozšíření virtuálního počítače z hello služby režim správy (asm) v rozhraní příkazového řádku Azure na jakékoli platformě. [Docker](https://www.docker.com/) je jedním z hello nejoblíbenější virtualizace přístupy, které používá [Linux kontejnery](http://en.wikipedia.org/wiki/LXC) místo virtuální počítače jako způsob oddělením dat a výpočty na sdílených prostředků. Můžete použít rozšíření virtuálního počítače Docker hello a hello [Azure Linux Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate Docker virtuální počítač, který je hostitelem libovolný počet kontejnerů pro vaše aplikace v Azure. toosee podrobný popis kontejnery a jejich výhody, najdete v části hello [Docker vysokou úroveň tabulí](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="how-toouse-hello-docker-vm-extension-with-azure"></a>Jak toouse hello Docker rozšíření virtuálního počítače Azure
toouse hello Docker rozšíření virtuálního počítače Azure, musíte nainstalovat verzi hello [rozhraní příkazového řádku Azure](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) vyšší než 0.8.6 (jako je tento zápis hello aktuální verze je 0.10.0). Hello rozhraní příkazového řádku Azure můžete nainstalovat na Mac, Linux a Windows.

dokončení procesu toouse Hello Docker v Azure je jednoduchý:

* Nainstalujte hello rozhraní příkazového řádku Azure a jeho závislosti hello počítače, ze kterého mají být toocontrol Azure (v systému Windows, bude jím Linux distribuční spuštěná jako virtuální počítač)
* Použít hello Azure CLI Docker příkazy toocreate hostitelů Docker virtuálních počítačů v Azure
* Použijte hello místní Docker příkazy toomanage Docker kontejnerů v Docker virtuálního počítače v Azure.

### <a name="install-hello-azure-command-line-interface-azure-cli"></a>Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI)
tooinstall a nakonfigurovat hello příkazového řádku Azure CLI, najdete v části [jak tooinstall hello rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md). tooconfirm hello instalace typu `azure` hello příkazového řádku a po krátké chvíli byste měli vidět, hello obrázky ASCII rozhraní příkazového řádku Azure, kde jsou uvedeny hello basic příkazy tooyou k dispozici. Pokud instalace hello fungovala správně, musí být schopný tootype `azure help vm` a zjistíte, že jeden z uvedených hello příkazů "docker".

> [!NOTE]
> Docker má nástroje pro systém Windows, [počítač Docker](https://docs.docker.com/installation/windows/), které můžete použít také vytvoření hello tooautomate docker klienta, které jako hostitelů docker můžete toowork s virtuálními počítači Azure.
> 
> 

### <a name="connect-hello-azure-cli-tootooyour-azure-account"></a>Připojení rozhraní příkazového řádku Azure tootooyour hello účet Azure
Než budete moci použít hello rozhraní příkazového řádku Azure je nutné přidružit přihlašovací údaje účtu Azure s hello rozhraní příkazového řádku Azure na vaši platformu. Hello části [jak tooconnect tooyour předplatného Azure](../../../xplat-cli-connect.md) vysvětluje, jak tooeither stahování a import vaše **.publishsettings** souboru nebo Azure CLI přidružit id organizace.

> [!NOTE]
> Existují určité rozdíly v chování při použití jednoho nebo hello jiných metod ověřování, takže se zda dokument hello tooread výše toounderstand hello různé funkce.
> 
> 

### <a name="install-docker-and-use-hello-docker-vm-extension-for-azure"></a>Docker nainstalovat a používat hello Docker rozšíření virtuálního počítače pro Azure.
Postupujte podle hello [pokyny k instalaci Docker](https://docs.docker.com/installation/#installation) tooinstall Docker místně na vašem počítači.

toouse Docker s virtuální počítač Azure, hello Linux obrázek použitý pro hello virtuálního počítače musí mít hello [agenta virtuálního počítače Azure Linux](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nainstalována. V současné době existují jenom dva typy bitových kopií, které poskytují toto:

* Ubuntu obrázek z Galerie obrázků Azure hello nebo
* Vlastní image Linux, kterou jste vytvořili pomocí hello Azure Linux Agent virtuálního počítače nainstalovaný a nakonfigurovaný. V tématu [agenta virtuálního počítače Azure Linux](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pro další informace o toobuild vlastní virtuální počítač s Linuxem pomocí hello agenta virtuálního počítače Azure.

### <a name="using-hello-azure-image-gallery"></a>Pomocí Galerie obrázků Azure hello
Z Bash nebo relaci Terminálové služby použijte následující příkaz rozhraní příkazového řádku Azure toolocate hello nejnovější bitovou kopii Ubuntu v toouse Galerie virtuálních počítačů hello zadáním hello

`azure vm image list | grep Ubuntu-14_04`

a vyberte jednu z hello image názvy, například `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, a použijte hello následující příkaz toocreate nového virtuálního počítače pomocí této bitové kopie.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

Kde:

* *&lt;název virtuálního počítače cloudservice&gt;*  je název hello hello virtuální počítač, který se stane hello Docker kontejneru hostitelský počítač v Azure
* *&lt;uživatelské jméno&gt;*  je uživatelské jméno hello hello výchozí kořenový uživatel hello virtuálních počítačů
* *&lt;heslo&gt;*  je hello heslo hello *uživatelské jméno* účet, který splňuje standardy hello složitější pro Azure.

> [!NOTE]
> V současné době heslo musí být dlouhé alespoň 8 znaků, musí obsahovat jeden malá písmena a jedno velké písmeno, číslo a zvláštní znak například jeden z hello následující znaky: `!@#$%^&+=`. Ne, hello tečka na konci hello hello předcházející větu není speciální znak.
> 
> 

Pokud příkaz hello byl úspěšný, byste měli vidět něco podobného jako hello následující, v závislosti na přesné argumenty hello a možnosti, které používáte:

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> Vytvoření virtuálního počítače může trvat několik minut, ale poté, co se zřizují (hodnota stavu hello je `ReadyRole`) hello spustí Docker démon (hello Docker service) a hostitele kontejner Docker toohello se můžete připojit.
> 
> 

hello tootest Docker virtuálních počítačů, které jste vytvořili v Azure, typu

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

kde  *&lt;vm název--použili&gt;*  je název hello hello virtuálního počítače, který jste použili v volání příliš`azure vm docker create`. Měli byste vidět něco podobné toohello následující text, který označuje, že je váš virtuální počítač Docker hostitele se spuštěným v Azure a čekání příkazech. 

Nyní můžete zkusit tooconnect pomocí vaše informace o docker klienta tooobtain (v některých nastavení klienta Docker, jako je například v systému Mac, který může mít toouse `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Jenom toobe jisti, že je všechny funkční, můžete zkontrolovat hello virtuálních počítačů pro hello Docker rozšíření:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Ověřování docker hostitele virtuálního počítače
Kromě toho toocreating hello virtuální počítač Docker, hello `azure vm docker create` příkaz také automaticky vytvoří hello potřebné certifikáty tooallow Docker klientský počítač tooconnect toohello kontejner Azure hostiteli pomocí protokolu HTTPS a hello certifikáty jsou uložené na obou Hello klientských a hostitelských počítačů, podle potřeby. Při dalších pokusech hello existující certifikáty jsou opakovaně a sdílet s hello nového hostitele.

Ve výchozím nastavení, certifikáty jsou umístěny v `~/.docker`, a Docker budou nakonfigurované toorun na portu **. 2376**. Pokud byste chtěli toouse jiný port nebo adresář, pak můžete použít jednu z následujících akcí hello `azure vm docker create` příkazového řádku možnosti tooconfigure Docker kontejneru hostitele virtuálních počítačů toouse jiný port nebo různé certifikáty pro připojení klientů:

```
-dp, --docker-port [port]              Port toouse for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Hello Docker démon na hostiteli hello je nakonfigurované toolisten pro a ověření klienta připojení na hello zadaný port přes hello certifikáty generované hello `azure vm docker create` příkaz. Hello klientský počítač musí mít tyto certifikáty toogain přístup toohello Docker hostitele.

> [!NOTE]
> Síťová hostitele se systémem bez tyto certifikáty budou snadno napadnutelný tooanyone, který může tooconnect toohello počítače. Než změníte hello výchozí konfiguraci, ujistěte se, že rozumíte hello rizika tooyour počítače a aplikace.
> 
> 

## <a name="next-steps"></a>Další kroky
* Jsou připraveny toogo toohello [Docker uživatelská příručka] a používat virtuální počítač Docker. toocreate virtuální počítač Docker povoleno hello nového portálu, najdete v části [jak toouse hello Docker rozšíření virtuálního počítače s hello portál].
* Hello rozšíření virtuálního počítače Azure Docker také podporuje Docker Compose, který používá deklarativní tootake souboru YAML aplikaci na vývojáře modelován v každém prostředí a generování konzistentní nasazení. V tématu [začít pracovat s Docker a napište toodefine a spusťte aplikaci služby kontejneru na virtuální počítač Azure].  

<!--Anchors-->
[Subheading 1]:#subheading-1
[Subheading 2]:#subheading-2
[Subheading 3]:#subheading-3
[Next steps]:#next-steps

[How toouse hello Docker VM Extension with Azure]:#How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]:#Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]:#Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 tooanother azure.microsoft.com documentation topic]:../../virtual-machines-windows-hero-tutorial.md
[Link 2 tooanother azure.microsoft.com documentation topic]:../../../app-service-web/web-sites-custom-domain-name.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md
[jak toouse hello Docker rozšíření virtuálního počítače s hello portál]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker uživatelská příručka]:https://docs.docker.com/userguide/

[začít pracovat s Docker a napište toodefine a spusťte aplikaci služby kontejneru na virtuální počítač Azure]:../docker-compose-quickstart.md
