---
title: "aaaSet až aplikací MPI toorun clusteru Linux RDMA | Microsoft Docs"
description: "Vytvořit cluster Linux velikost H16r, H16mr, A8 a A9 VMs toouse hello Azure RDMA sítě toorun MPI aplikace"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.openlocfilehash: 3199317a37b095e80718d6724954687d30aea3a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-linux-rdma-cluster-toorun-mpi-applications"></a>Nastavení aplikací MPI toorun clusteru Linux RDMA
Zjistěte, jak tooset až Linux RDMA cluster v Azure s [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toorun paralelních aplikací Message Passing Interface (MPI). Tento článek obsahuje kroky tooprepare toorun bitové kopie prostředí HPC Linux Intel MPI v clusteru. Po přípravě nasazení clusteru virtuálních počítačů pomocí tuto bitovou kopii a jeden velikostí hello podporující RDMA virtuálních počítačů Azure (aktuálně H16r, H16mr, A8 a A9). Použití hello clusteru toorun aplikací MPI, které efektivně komunikovat přes nízkou latencí, vysokou propustnost sítě na základě přímého přístupu do paměti (vzdáleného počítače RDMA) technologie.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic. Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

## <a name="cluster-deployment-options"></a>Možnosti nasazení clusteru
Toto jsou metody toocreate clusteru s podporou Linux RDMA můžete použít s nebo bez Plánovač úloh.

* **Azure CLI skripty**: Jak uvidíte později v tomto článku, použijte hello [rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md) (CLI) tooscript hello nasazení clusteru s podporou RDMA podporovat virtuálních počítačů. Hello rozhraní příkazového řádku v režimu správy služby se vytvoří hello uzly clusteru sériově v modelu nasazení classic hello, takže nasazení mnoho výpočetních uzlů může trvat několik minut. tooenable hello síťového připojení RDMA při použití modelu nasazení classic hello, nasazení virtuálních počítačů hello v hello stejné cloudové služby.
* **Šablony Azure Resource Manageru**: Můžete taky hello Resource Manager nasazení modelu toodeploy clusteru podporující RDMA virtuálních počítačů, který se připojuje toohello RDMA sítě. Můžete [vytvořit vlastní šablonu](../../../resource-group-authoring-templates.md), nebo zkontrolujte hello [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) pro šablony přispěli Microsoft hello komunity toodeploy hello řešení nebo chcete. Šablony Resource Manageru nabízejí rychlý a spolehlivý toodeploy Linux cluster. tooenable hello síťového připojení RDMA při použití hello modelu nasazení Resource Manager, nasazení virtuálních počítačů hello v hello stejné skupině dostupnosti.
* **HPC Pack**: vytvoření clusteru s podporou sady Microsoft HPC Pack v Azure a přidat podporu rdma výpočetních uzlech, které spustit podporované hello RDMA síť tooaccess distribuce systému Linux. Další informace najdete v tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-hello-classic-model"></a>Ukázka nasazení kroky v modelu classic hello
Hello následující kroky ukazují, jak přizpůsobit toouse hello rozhraní příkazového řádku Azure toodeploy virtuální počítač SUSE Linux Enterprise Server (SLES) 12 SP1 HPC z hello Azure Marketplace a vytvořit vlastní image virtuálního počítače. Potom můžete image hello tooscript hello nasazení clusteru s podporou RDMA podporovat virtuálních počítačů.

> [!TIP]
> Použijte podobné toodeploy kroky, které clusteru s podporou RDMA podporovat virtuálních počítačů založené na imagích na základě CentOS HPC v hello Azure Marketplace. Některé kroky poněkud, jak jsme uvedli lišit. 
>
>

### <a name="prerequisites"></a>Požadavky
* **Klientský počítač**: budete potřebovat toocommunicate počítače Mac, Linux nebo Windows klienta s Azure. Tento postup předpokládá, že používáte klienta Linux.
* **Předplatné Azure**: Pokud nemáte předplatné, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut. Pro větší clustery zvažte průběžnými platbami předplatné nebo jiné možnosti nákupu.
* **Virtuální počítač velikost dostupnosti**: hello následující instance velikosti jsou podporující RDMA: H16r, H16mr, A8 a A9. Zkontrolujte [produkty podle oblasti](https://azure.microsoft.com/regions/services/) pro dostupnost v oblastech Azure.
* **Kvóta jader**: může být nutné tooincrease hello kvóty jader toodeploy cluster virtuálních počítačů náročné. Pokud chcete, aby toodeploy 8 A9 virtuálních počítačů, jak je znázorněno v tomto článku se například potřebovat nejméně 128 jader. Vaše předplatné může také omezit hello počet jader, který můžete nasadit v určité rodiny velikost virtuálního počítače, včetně hello H-series. toorequest kvótu zvýšit, [otevřete žádosti o podporu online zákazníka](../../../azure-supportability/how-to-create-azure-support-request.md) zdarma.
* **Rozhraní příkazového řádku Azure**: [nainstalovat](../../../cli-install-nodejs.md) hello rozhraní příkazového řádku Azure a [připojit tooyour předplatného Azure](../../../xplat-cli-connect.md) z hello klientského počítače.

### <a name="provision-an-sles-12-sp1-hpc-vm"></a>Zřizování virtuálních počítačů SLES 12 SP1 HPC
Po přihlášení tooAzure s hello příkazového řádku Azure CLI, spusťte `azure config list` tooconfirm, který hello výstup ukazuje režimu správy služby. Pokud ne, nastavení režimu hello spuštěním tohoto příkazu:

    azure config mode asm


Zadejte všechny odběry hello jsou autorizovaní toouse hello toolist následující:

    azure account list

Hello aktuální aktivní předplatné, je označen `Current` nastavit příliš`true`. Pokud toto předplatné není hello, kterou má být toouse toocreate hello cluster, nastavit ID hello příslušné předplatné jako aktivní předplatné hello:

    azure account set <subscription-Id>

toosee hello veřejně dostupné SLES 12 SP1 HPC bitové kopie v Azure, spusťte příkaz jako hello následující, za předpokladu, že vaše prostředí podporuje prostředí **grep**:

    azure vm image list | grep "suse.*hpc"

Zřídit podporou RDMA virtuální počítač s bitovou kopii SLES 12 SP1 HPC spuštěním příkazu jako hello následující:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

Kde:

* Hello velikost (v tomto příkladu A9) je jedním z velikosti virtuálních počítačů hello RDMA podporovat.
* Hello externí SSH port číslo (22 v tomto příkladu, což je výchozí SSH hello) je platné číslo portu. číslo portu SSH interní Hello nastavena too22.
* Nová Cloudová služba je vytvořená ve hello určeného umístění hello oblast Azure. Zadejte umístění, ve které hello virtuálního počítače je k dispozici velikost, které zvolíte.
* Pro podporu s prioritou SUSE (který budou vám účtovány dodatečné poplatky) název bitové kopie hello SLES 12 SP1 aktuálně může být jedna z těchto dvou možností: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-hello-vm"></a>Přizpůsobení hello virtuálních počítačů
Po dokončení zřizování hello virtuálních počítačů SSH toohello virtuálních počítačů pomocí hello externí IP adresu Virtuálního počítače (nebo název DNS) a hello externí port číslo jste nakonfigurovali a pak ho přizpůsobit. Podrobnosti připojení najdete v tématu [jak toolog na tooa virtuální počítač se systémem Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Proveďte příkazy hello uživatele, které jste nakonfigurovali na hello virtuálních počítačů, pokud je kořenový přístup vyžaduje toocomplete krok.

> [!IMPORTANT]
> Microsoft Azure neposkytuje kořenový přístup tooLinux virtuálních počítačů. toogain přístup pro správu při připojení jako uživatel toohello virtuálního počítače, spusťte příkazy pomocí `sudo`.
>
>

* **Aktualizace**: nainstalujte aktualizace pomocí zypper. Také můžete chtít tooinstall nástrojů systému souborů NFS.

  > [!IMPORTANT]
  > V SLES 12 SP1 HPC virtuální počítač doporučujeme nepoužijete jádra aktualizace, které může způsobit problémy s hello Linux RDMA ovladače.
  >
  >
* **Intel MPI**: dokončí instalaci hello Intel MPI v hello SLES 12 SP1 HPC virtuálních počítačů tak, že spustíte následující příkaz hello:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* **Uzamknout paměť**: MPI kódy toolock hello paměti k dispozici pro RDMA, přidat nebo změnit následující nastavení v souboru /etc/security/limits.conf hello hello. Budete potřebovat kořenový přístup tooedit tento soubor.

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > Pro účely testování můžete také nastavit memlock toounlimited. Například: `<User or group name>    hard    memlock unlimited`. Další informace najdete v tématu [známé nejlepší metody pro nastavení uzamčení velikost paměti](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).
  >
  >
* **Klíče SSH pro virtuální počítače SLES**: generování SSH klíče tooestablish vztahu důvěryhodnosti pro váš uživatelský účet mezi hello výpočetní uzly v clusteru SLES hello při spouštění úloh MPI. Pokud nasadíte virtuální počítač na základě CentOS HPC, nemusíte postupujte podle tohoto kroku. Po zachycení bitové kopie hello a nasazení hello clusteru, najdete v pokynech později v tooset Tento článek se passwordless SSH vztah důvěryhodnosti mezi uzly clusteru hello.

    klíče SSH toocreate, spusťte následující příkaz hello. Po zobrazení výzvy pro vstup, vyberte **Enter** toogenerate hello klíčů ve výchozí umístění hello bez nastavení hesla.

        ssh-keygen

    Připojí hello veřejného klíče toohello authorized_keys souboru známé veřejných klíčů.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    V adresáři ~/.ssh hello upravovat nebo vytvářet hello konfiguračního souboru. Zadejte rozsah IP adres hello hello privátní sítě, abyste naplánovali toouse v Azure (v tomto příkladu 10.32.0.0/16):

        host 10.32.0.*
        StrictHostKeyChecking no

    Alternativně seznamu hello privátní síť IP adresu každého virtuálního počítače v clusteru následujícím způsobem:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > Konfigurace `StrictHostKeyChecking no` můžete vytvořit představuje potenciální bezpečnostní riziko, pokud není zadán konkrétní IP adresu nebo rozsah.
  >
  >
* **Aplikace**: Nainstalujte aplikace potřebují nebo provedení jiných úprav, než zaznamenáte hello image.

### <a name="capture-hello-image"></a>Zachycení bitové kopie hello
Obrázek hello toocapture nejprve spusťte následující příkaz na hello virtuálního počítače s Linuxem hello. Tento příkaz deprovisions hello virtuálních počítačů, ale spravuje uživatelské účty a klíče SSH, které nastavíte.

```
sudo waagent -deprovision
```

Z klientského počítače spusťte hello následující obrázek hello toocapture příkazy rozhraní příkazového řádku Azure. Další informace najdete v tématu [jak toocapture klasické virtuální počítač s Linuxem jako obrázek](capture-image.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Po spuštění těchto příkazů zachycenou image virtuálního počítače hello pro vaše použití a hello virtuálních počítačů se odstraní. Nyní máte připravené toodeploy vaše vlastní image clusteru.

### <a name="deploy-a-cluster-with-hello-image"></a>Nasazení clusteru s bitovou kopií hello
Upravit hello následující skript Bash s příslušnými hodnotami pro vaše prostředí a spusťte jej z klientského počítače. Protože Azure nasadí virtuálních počítačů hello sériově v modelu nasazení classic hello, trvá několik minut toodeploy hello osm virtuálních počítačů A9 navržený v tento skript.

```
#!/bin/bash -x
# Script toocreate a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where hello VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All hello compute-intensive instances need toobe in hello same cloud service for Linux RDMA toowork across InfiniBand.
# Note: hello current maximum number of VMs in a cloud service is 50. If you need tooprovision more than 50 VMs in hello same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want tooturn off external ports and use only internal ports toocommunicate between compute nodes via port 22, don’t use this option. Since port numbers up too10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 toocluster18. Specify your captured image in <image-name>. Specify hello username and password you used when creating hello SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment tooprovision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Důležité informace pro cluster prostředí HPC CentOS
Pokud chcete, aby tooset clusteru na základě jedné z bitové kopie založené na CentOS HPC hello v hello Azure Marketplace místo SLES 12 pro prostředí HPC, postupujte podle hello obecné kroky v předcházející části hello. Všimněte si hello následující rozdíly při zřídíte a nakonfigurujete hello virtuálních počítačů:

- Intel MPI je již nainstalován na virtuální počítač z bitové kopie založené na CentOS HPC zřízený.
- Nastavení uzamčení paměti jsou již přidán do souboru /etc/security/limits.conf hello Virtuálního počítače.
- Nedojde k vytvoření klíčů SSH na hello zřídíte virtuální počítač na zachycení. Místo toho doporučujeme nastavení ověřování na základě uživatele po nasazení clusteru hello. Další informace najdete v tématu hello následující části.  

### <a name="set-up-passwordless-ssh-trust-on-hello-cluster"></a>Nastavení passwordless SSH důvěryhodnosti v clusteru hello
V clusteru HPC na základě CentOS, existují dvě metody pro vytvoření vztahu důvěryhodnosti mezi hello výpočetní uzly: ověřování na základě hostitele a ověřování na základě uživatele. Ověřování na základě hostitele je mimo rámec tohoto článku hello a obecně je třeba provést pomocí skriptu rozšíření během nasazení. Ověřování na základě uživatele je vhodné pro navázání vztahu důvěryhodnosti po nasazení a vyžaduje hello generování a sdílení klíčů SSH mezi hello výpočetní uzly v clusteru hello. Tato metoda se běžně označuje jako passwordless přihlašování přes SSH a je požadován při spouštění úloh MPI.

Ukázkový skript podílí z komunity hello je k dispozici na [Githubu](https://github.com/tanewill/utils/blob/master/user_authentication.sh) tooenable snadno uživatele ověřování na základě CentOS HPC clusteru. Stáhnout a použít tento skript pomocí hello následující kroky. Můžete také upravit tento skript nebo použít jakékoli jiné metoda tooestablish passwordless SSH ověřování mezi hello clusteru výpočetních uzlů.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

toorun hello skriptu, musíte tooknow hello předponu pro podsíť IP adresy. Předpona hello získáte tak, že spustíte následující příkaz na jeden z uzlů clusteru hello hello. Výstup by měl vypadat podobně jako 10.1.3.5 a hello předpona je hello 10.1.3 část.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Nyní spusťte skript hello pomocí tři parametry: hello běžné uživatelské jméno v hello výpočetní uzly, hello společné heslo pro tohoto uživatele na hello výpočetních uzlů a předpony hello podsítě, která byla vrácena z předchozí příkaz hello.

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Tento skript hello následující:

* Vytvoří adresář na uzel hello hostitele s názvem .ssh, což je vyžadováno pro passwordless přihlášení.
* Vytvoří konfigurační soubor v adresáři .ssh hello, která nastaví přihlášení tooallow passwordless přihlášení z libovolného uzlu v clusteru hello.
* Vytvoří soubory obsahující názvy hello a uzel IP adresy pro všechny hello uzly v clusteru hello. Tyto soubory jsou ponechány po spuštění skriptu hello pro pozdější použití.
* Vytvoří pár klíčů privátní a veřejné pro každý uzel clusteru (včetně uzlu hostitele hello) a vytvoří záznamy v souboru authorized_keys hello.

> [!WARNING]
> Spuštěním tohoto skriptu můžete vytvořit představuje potenciální bezpečnostní riziko. Ujistěte se, že není hello informace o veřejném klíči v ~/.ssh distribuován.
>
>

## <a name="configure-intel-mpi"></a>Konfigurace Intel MPI
toorun aplikací MPI na Azure Linux RDMA, je nutné tooconfigure konkrétní tooIntel určité prostředí proměnné MPI. Zde je ukázkové Bash skriptu tooconfigure hello proměnných potřebných toorun aplikace. Podle potřeby pro instalaci nástroje Intel MPI, změňte cestu toompivars.sh hello.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting hello variable tooshm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line toorun hello job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path toohello application exe> <arguments specific toohello application>

#end
```

Hello formát souboru hostitele hello je následující. Přidejte jeden řádek pro každý uzel v clusteru. Zadejte, že privátní IP adresy z virtuální sítě hello definovaného dříve, není názvy DNS. Například pro dva hostitele s IP adresami 10.32.0.1 a 10.32.0.2 hello soubor obsahuje hello následující:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Spustit MPI na Základní dvojuzlový cluster
Pokud jste tak již neučinili, nejprve nastavte hello prostředí pro Intel MPI.

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>Spusťte příkaz MPI
Spusťte příkaz MPI na jednom z tooshow hello výpočetní uzly, který MPI je správně nainstalována a může komunikovat mezi aspoň dva výpočetní uzly. Následující Hello **mpirun** příkaz spustí hello **hostname** na dvou uzlech.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Vaše výstup by měl obsahovat hello názvy všech hello uzlů, které předávají jako vstup pro `-hosts`. Například **mpirun** příkaz s dvěma uzly vrátí výstup jako hello následující:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Spuštění MPI srovnávacího testu
Následující příkaz Intel MPI Hello spustí pingpong srovnávacího testu tooverify hello síť s clustery konfiguraci a připojení toohello RDMA.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Na pracovní clusteru se dvěma uzly měli byste vidět výstup podobný následující hello. V síti Azure RDMA hello očekávejte latenci nebo pod 3 mikrosekundách zpráva velikostí až too512 bajtů.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# hello number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected toobe exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through hello flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks toorun:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Další kroky
* Nasazení a spuštění vaší Linux MPI aplikace na cluster systému Linux.
* V tématu hello [dokumentaci ke knihovně MPI Intel](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) pokyny k Intel MPI.
* Zkuste [šablony rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) toocreate Lustre Intel clusteru pomocí bitové kopie založené na CentOS HPC. Podrobnosti najdete v tématu [nasazení Intel cloudu Edition pro počítače s Lustre v Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
