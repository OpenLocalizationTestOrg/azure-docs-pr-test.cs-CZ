---
title: "aaaRun HVĚZDIČKY – CCM + s HPC Pack na virtuální počítače s Linuxem | Microsoft Docs"
description: "Nasazení clusteru s podporou sady Microsoft HPC Pack v Azure a spuštění HVĚZDIČKOU – CCM + úlohy na několika Linux výpočetní uzly přes sítě RDMA."
services: virtual-machines-linux
documentationcenter: 
author: xpillons
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 75523406-d268-4623-ac3e-811c7b74de4b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 09/13/2016
ms.author: xpillons
ms.openlocfilehash: 8265013cb295f53d6d4354ab2f100ef20d9f4c8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Spustit HVĚZDIČKY – CCM + pomocí sady Microsoft HPC Pack na Linux RDMA cluster v Azure
Tento článek ukazuje, jak toodeploy sady Microsoft HPC Pack clusteru v Azure a spusťte [HVĚZDIČKY CD adapco-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) úlohy na několika výpočetních uzlech Linux, které jsou vzájemně propojeny InfiniBand.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack poskytuje funkce toorun celou řadu rozsáhlé HPC a paralelní aplikace, včetně aplikací MPI, v clusterech virtuální počítače Microsoft Azure. HPC Pack také podporuje spuštěné aplikace prostředí HPC Linux virtuálních počítačů Linux výpočetní uzly, které jsou nasazené na clusteru HPC Pack. Úvod toousing Linux výpočetní uzly s HPC Pack, najdete v části [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Nastavení clusteru služby HPC Pack
Stáhnout skriptů nasazení HPC Pack IaaS hello z hello [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) a extrahovat je místně.

Prostředí Azure PowerShell je požadována. Pokud PowerShell není nakonfigurovaný v místním počítači, přečtěte si článek hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

V době psaní tohoto textu hello hello Linux Image z hello Azure Marketplace, (který obsahuje hello InfiniBand ovladače pro Azure) jsou pro SLES 12, CentOS 6.5 a CentOS 7.1. Tento článek je založená na využití hello SLES 12. Název hello tooretrieve všechny Image Linux, které podporují prostředí HPC v hello Marketplace, můžete spustit následující příkaz prostředí PowerShell hello:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

výstup Hello uvádí hello umístění, ve kterém se tyto Image jsou k dispozici a hello název bitové kopie (**ImageName**) toobe použitých v šabloně nasazení hello později.

Před nasazením hello clusteru, máte toobuild soubor HPC Pack nasazení šablony. Protože jsme se cílení na clusteru s podporou malé, hlavního uzlu hello bude řadič domény hello a hostování místní databáze SQL.

Hello následující šablony bude nasazení hlavního uzlu, vytvořte soubor XML s názvem **MyCluster.xml**a nahraďte hodnoty hello **SubscriptionId**, **StorageAccount**,  **Umístění**, **VMName**, a **ServiceName** s tímto počítačem.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Spusťte hello hlavní uzel vytvoření spuštěním hello příkaz prostředí PowerShell v příkazovém řádku se zvýšenými oprávněními:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Po 20 minutách too30 hello hlavního uzlu by měl být připraven. Tooit můžete připojit z portálu Azure hello kliknutím hello **Connect** ikonu hello virtuálního počítače.

Nakonec máte toofix hello, server DNS pro předávání. toodo Ano, spusťte Správce DNS.

1. Klikněte pravým tlačítkem na název serveru hello ve Správci DNS, vyberte **vlastnosti**a potom klikněte na hello **předávání** kartě.
2. Klikněte na tlačítko hello **upravit** tlačítko tooremove veškeré služby předávání a potom klikněte na **OK**.
3. Ujistěte se, že hello **pomocí odkazů na kořenový server, pokud jsou k dispozici žádné servery pro předávání** zaškrtávací políčko je vybraná a pak klikněte na tlačítko **OK**.

## <a name="set-up-linux-compute-nodes"></a>Nastavit Linuxových výpočetních uzlů
Nasazení hello Linux výpočetní uzly s hello stejné šablony nasazení, kterou jste použili toocreate hello hlavního uzlu.

Kopírovat soubor hello **MyCluster.xml** z hlavního uzlu toohello místního počítače a aktualizace hello **NodeCount** značka s číslem hello uzlů, které chcete toodeploy (< = 20). Být opatrní toohave dostatek dostupné jader v rámci svojí Azure kvóty, protože každá instance A9 spotřebuje 16 jader v rámci vašeho předplatného. A8 instancí (8 jader) můžete použít místo A9, pokud chcete toouse více virtuálních počítačů v hello stejný rozpočet.

Hello hlavního uzlu zkopírujte skriptů nasazení hello HPC Pack IaaS.

Spusťte následující příkazy prostředí Azure PowerShell v příkazovém řádku se zvýšenými hello:

1. Spustit **Add-AzureAccount** tooconnect tooyour předplatného Azure.
2. Pokud máte více předplatných, spusťte **Get-AzureSubscription** toolist je.
3. Nastavit výchozí předplatné spuštěním hello **Select-AzureSubscription - Název_předplatného xxxx-výchozí** příkaz.
4. Spustit **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** toostart nasazení Linuxových výpočetních uzlů.
   
   ![Nasazení hlavního uzlu v akci][hndeploy]

Otevřete nástroj Správce clusterů HPC Pack hello. Za několik minut bude pravidelně Linux výpočetní uzly zobrazí v seznamu výpočetních uzlů clusteru. V režimu nasazení classic hello se vytvoří virtuální počítače IaaS postupně. Takže pokud hello počet uzlů je důležité, pak získávání všechny nasazené může trvat dlouhou dobu.

![Linuxové uzly ve Správci clusteru HPC Pack][clustermanager]

Teď, když jsou všechny uzly jsou spuštěny v clusteru hello, existují další infrastrukturu toomake nastavení.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Nastavit sdílená Azure pro Windows a Linux uzly
Můžete použít skripty toostore služby Azure File hello, balíčky aplikací a datových souborů. Azure File nabízí funkce CIFS nad úložiště objektů Blob v Azure jako trvalé úložiště. Uvědomte si, že to není hello nejvíce škálovatelným řešením, ale je hello nejjednodušší jeden a nevyžaduje vyhrazených virtuálních počítačích.

Vytvoření Azure sdílené složky podle hello pokynů v článku hello [Začínáme s Azure File storage ve Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).

Zachovat hello název účtu úložiště jako **saname**, název sdílené složky souborů hello jako **sharename**a klíč účtu úložiště hello jako **sakey**.

### <a name="mount-hello-azure-file-share-on-hello-head-node"></a>Připojit sdílenou složku Azure File hello hello hlavního uzlu
Otevřete příkazový řádek se zvýšenými oprávněními a spusťte následující příkaz toostore hello přihlašovací údaje v úložišti místního počítače hello hello:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Potom toomount hello sdílenou složku Azure File, spusťte:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-hello-azure-file-share-on-linux-compute-nodes"></a>Připojit sdílenou složku Azure File hello na Linuxových výpočetních uzlů
Užitečné nástroj, který se dodává s HPC Pack je nástroj clusrun hello. Tento nástroj příkazového řádku toorun hello stejný příkaz můžete použít současně se sadou výpočetních uzlů. V našem případě se používá sdílenou složku Azure File hello toomount a zachovat ji toosurvive restartování.
V příkazovém řádku se zvýšenými hello hlavního uzlu spusťte následující příkazy hello.

toocreate hello přípojného adresáře:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

toomount hello sdílenou složku Azure:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

toopersist hello připojení sdílené složky:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Nainstalujte HVĚZDIČKY – CCM +
Instancemi Azure virtuální počítač A8 a A9 poskytnout podporu InfiniBand a RDMA. Hello jádra ovladače, které umožňují tyto funkce jsou dostupné pro Windows Server 2012 R2, SUSE 12, CentOS 6.5 a CentOS 7.1 obrázků v hello Azure Marketplace. Microsoft MPI a Intel MPI (verze 5.x) jsou hello dvě MPI knihovny, které podporují tyto ovladače v Azure.

CD adapco HVĚZDIČKY – CCM + verze 11.x a později je instalován s verzí Intel MPI 5.x, tak, aby zahrnuté InfiniBand podpora pro Azure.

Získat hello Linux64 HVĚZDIČKY – CCM + balíček z hello [CD adapco portál](https://steve.cd-adapco.com). V našem případě jsme použili verze 11.02.010 ve smíšeném přesnost.

Hello hlavního uzlu v hello **/hpcdata** Azure File sdílenou složku, vytvořit skript prostředí s názvem **setupstarccm.sh** s hello následující obsah. Tento skript se spustí na každý výpočetní uzel tooset až HVĚZDIČKY – CCM + místně.

#### <a name="sample-setupstarcmsh-script"></a>Ukázkový skript setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh tooset up STAR-CCM+ locally

    # Create hello CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy hello STAR-CCM package from hello file share toohello local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract hello package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without hello FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Nyní, tooset až HVĚZDIČKY – CCM + na všechny Linux výpočetní uzly, otevřete příkazový řádek se zvýšenými oprávněními a spusťte následující příkaz hello:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Když je spuštěný příkaz hello, můžete sledovat využití procesoru hello pomocí hello heat mapa ze Správce clusteru. Za několik minut všechny uzly by měl být správně nastavena.

## <a name="run-star-ccm-jobs"></a>Spustit HVĚZDIČKY – CCM + úlohy
HPC Pack se používá pro možnosti plánovače úloh v pořadí toorun HVĚZDIČKY – CCM + úlohy. toodo tak, budeme potřebovat hello podporu několik skriptů, které jsou používané toostart hello úlohy a spusťte HVĚZDIČKY – CCM +. Hello vstupní data se ukládají na sdílenou složku Azure File hello první pro jednoduchost.

Následující skript prostředí PowerShell Hello je použité tooqueue HVĚZDU – CCM + úlohy. Trvá tři argumenty:

* Název modelu Hello
* Hello počet uzlů toobe použít
* Hello počet jader na každý uzel toobe použít

Protože HVĚZDIČKY – CCM + můžete vyplnit hello paměti šířky pásma, jeho obvykle lepší toouse menší počet jader na výpočetní uzly a přidejte nové uzly. Hello přesný počet jader na uzel, bude záviset na třídu hello procesoru a rychlost propojení hello.

Hello uzly jsou přiděleny výhradně pro úlohu hello a nemohou být sdíleny s ostatními úlohami. Úloha Hello není spuštěna jako úloha MPI přímo. Hello **runstarccm.sh** Spouštěč MPI hello se spustit skript prostředí.

Hello vstupní modelu a hello **runstarccm.sh** skriptů jsou uložené v hello **/hpcdata** sdílené složce, která byla dříve připojena.

Soubory protokolu jsou pojmenované s ID úlohy hello a jsou uloženy v hello **/hpcdata sdílení**, společně s hello HVĚZDIČKY – CCM + výstupní soubory.

#### <a name="sample-submitstarccmjobps1-script"></a>Ukázkový skript SubmitStarccmJob.ps1
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us hello job ID that's used tooidentify hello name of hello uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit hello job     
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Nahraďte **runner.java** vaše upřednostňované hvězdičkou-Spouštěče modelu CCM + Java a kód protokolování.

#### <a name="sample-runstarccmsh-script"></a>Ukázkový skript runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # hello path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set hello mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into hello hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with hello hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

V našem testu jsme použili token licence Power na vyžádání. Token, je nutné tooset hello **$CDLMD_LICENSE_FILE** proměnnou prostředí příliš **1999@flex.cd-adapco.com**  a hello klíč v hello **- podkey** možnost hello příkazového řádku .

Po inicializaci, skript hello extrahuje--z hello **$CCP_NODES_CORES** proměnné prostředí, které HPC Pack nastavit – hello seznam uzlů toobuild hostfile, který hello MPI Spouštěč používá. Tato hostfile bude obsahovat seznam hello výpočetní uzel názvy, které se používají pro úlohu hello, jeden název na každý řádek.

Formát Hello **$CCP_NODES_CORES** následuje tento vzor:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Kde:

* `<Number of nodes>`je hello počet uzlů přidělených toothis úlohy.
* `<Name of node_n_...>`je název hello každého uzlu přidělené toothis úlohy.
* `<Cores of node_n_...>`je hello počet jader na uzel hello přidělené toothis úlohy.

Hello počet jader (**$NBCORES**) je také počítané hello na základě počtu uzlů (**$NBNODES**) a hello počet jader na uzel (jako parametr **$NBCORESPERNODE**).

Možnosti MPI hello hello ty, které se používají s Intel MPI v Azure jsou:

* `-mpi intel`toospecify Intel MPI.
* `-fabric UDAPL`příkazy toouse Azure InfiniBand.
* `-cpubind bandwidth,v`toooptimize šířky pásma pro MPI s HVĚZDIČKOU – CCM +.
* `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`toomake Intel MPI pracovat s Azure InfiniBand a tooset hello požadovaný počet jader na uzel.
* `-batch`toostart HVĚZDIČKY – CCM + v dávkovém režimu s žádné uživatelské rozhraní.

Nakonec toostart úlohu, ujistěte se, že uzly jsou spuštěny a jsou online ve Správci clusteru. Z příkazového řádku prostředí PowerShell, spusťte toto:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Zastavit uzly
Později na po dokončení testů, můžete použít následující příkazy toostop prostředí HPC Pack PowerShell hello a spustit uzly:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Další kroky
Zkuste spuštěny další zátěže systému Linux. Například v tématu:

* [Spuštění NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](hpcpack-cluster-namd.md)
* [Spustit OpenFOAM na clusteru s podporou Linux RDMA v Azure pomocí sady Microsoft HPC Pack](hpcpack-cluster-openfoam.md)

<!--Image references-->
[hndeploy]:media/hpcpack-cluster-starccm/hndeploy.png
[clustermanager]:media/hpcpack-cluster-starccm/ClusterManager.png
