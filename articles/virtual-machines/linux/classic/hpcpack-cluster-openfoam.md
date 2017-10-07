---
title: "aaaRun OpenFOAM pomocí sady HPC Pack na virtuální počítače s Linuxem | Microsoft Docs"
description: "Nasazení clusteru s podporou sady Microsoft HPC Pack v Azure a spusťte úlohu OpenFOAM v několika výpočetních uzlech Linux přes sítě RDMA."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: 1a51ffe6804abb5156f01d580c9b7a4a9649377a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Spuštění OpenFoam se sadou Microsoft HPC Pack v clusteru Linux RDMA v Azure
Tento článek ukazuje jeden ze způsobů toorun OpenFoam ve virtuálních počítačích Azure. V tomto nasazení clusteru Microsoft HPC Pack s Linux výpočetní uzly na Azure a spusťte [OpenFoam](http://openfoam.com/) úlohy s Intel MPI. RDMA podporovat virtuálních počítačích Azure můžete použít pro hello výpočetní uzly, tak, aby výpočetní uzly hello komunikovat přes síť Azure RDMA hello. Další možnosti toorun OpenFoam v Azure zahrnout plně nakonfigurované komerční bitové kopie k dispozici v hello Marketplace, jako je například na UberCloud [OpenFoam 2.3 na CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)a spuštěním na [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (pro operaci otevřete pole a manipulace s) je balíček softwaru open source výpočet dynamiky kapaliny (CFD), který se často používá v inženýrství a vědecké účely, v organizacích komerční i academic. Obsahuje nástroje pro meshing, zejména snappyHexMesh, parallelized mesher pro komplexní geometrie CAD a před a po zpracování. Téměř všechny procesy spustit souběžně, povolíte uživatelům tootake všech výhod hardware počítače mají k dispozici.  

Microsoft HPC Pack poskytuje funkce toorun rozsáhlé HPC a paralelní aplikace, včetně aplikací MPI, v clusterech virtuální počítače Microsoft Azure. HPC Pack také podporuje spuštěné Linux HPC aplikace v systému Linux výpočetního uzlu, které virtuální počítače jsou nasazené v clusteru služby HPC Pack. V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) pro toousing Úvod Linuxových výpočetních uzlů pomocí sady HPC Pack.

> [!NOTE]
> Tento článek ukazuje, jak toorun úlohy Linux MPI pomocí sady HPC Pack. Předpokládá se, že máte některé znalosti s správu systému Linux a úlohy MPI systémem Linux clusterů. Pokud používáte verze MPI a OpenFOAM liší od hello ty, které jsou uvedené v tomto článku, může být toomodify některé kroky instalace a konfigurace. 
> 
> 

## <a name="prerequisites"></a>Požadavky
* **HPC Pack clusteru s podporou RDMA Linux výpočetní uzly** – nasazení clusteru HPC Pack velikosti A8, A9, H16r, nebo H16rm Linuxových výpočetních uzlů, buď pomocí [šablony Azure Resource Manageru](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) nebo [skript prostředí Azure PowerShell](hpcpack-cluster-powershell-script.md). V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) hello požadavky a kroky pro jednu z možností. Pokud si zvolíte hello možnost nasazení skriptu prostředí PowerShell, najdete v části hello vzorový konfigurační soubor v hello ukázkových souborů v hello konci tohoto článku. Pomocí této konfigurace clusteru HPC Pack toodeploy služby Azure na základě skládající se z hlavního uzlu velikosti A8 Windows Server 2012 R2 a 2 velikosti A8 SUSE Linux Enterprise Server 12 výpočetních uzlů. Nahraďte příslušnými hodnotami pro vaše předplatné a služby názvy. 
  
  **Další využití tooknow**
  
  * Linux RDMA síťové požadavky v Azure, najdete v části [vysokovýkonné výpočetní velikosti virtuálních počítačů](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  * Pokud používáte hello možnost nasazení skriptu prostředí Powershell, nasaďte všechny hello Linux výpočetní uzly v rámci jednoho cloudu služby toouse hello RDMA síťové připojení.
  * Po nasazení hello Linuxových uzlů, připojte pomocí SSH tooperform žádné další úlohy správy. Najít hello podrobnosti připojení SSH pro každý virtuální počítač s Linuxem v hello portálu Azure.  
* **Intel MPI** -toorun OpenFOAM na SLES 12 HPC výpočetních uzlech v Azure, musíte tooinstall hello Intel MPI knihovny 5 runtime z: hello [Intel.com lokality](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 je předinstalován v bitové kopie založené na CentOS HPC).  V pozdější fázi v případě potřeby nainstalujte Intel MPI na výpočetní uzly Linux. tooprepare pro tento krok po registraci pomocí Intel, postupujte podle hello odkaz hello potvrzení e-mailu toohello související webové stránce. Potom kopie hello stáhnout odkaz hello .tgz soubor pro příslušnou verzi Intel MPI hello. Tento článek je založena na Intel MPI verze 5.0.3.048.
* **OpenFOAM zdroj Pack** -stáhnout software hello OpenFOAM zdroj aktualizací Service Pack pro Linux z hello [OpenFOAM Foundation lokality](http://openfoam.org/download/2-3-1-source/). Tento článek je založen na verzi zdroje Pack 2.3.1, k dispozici ke stažení OpenFOAM 2.3.1.tgz. Postupujte podle pokynů hello později v této toounpack článku a kompilace OpenFOAM hello Linuxových výpočetních uzlů.
* **EnSight** (volitelné) – výsledky hello toosee OpenFOAM simulace, stáhněte a nainstalujte hello [EnSight](https://www.ceisoftware.com/download/) program vizualizaci a analýzu. Informace o licencování a stažení jsou v lokalitě EnSight hello.

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Nastavení vzájemného vztahu důvěryhodnosti mezi výpočetní uzly
Spuštění úlohy mezi uzly ve více uzlech Linux vyžaduje hello uzly tootrust, vzájemně (podle **rsh** nebo **ssh**). Při vytváření clusteru HPC Pack hello s hello skript nasazení Microsoft HPC Pack IaaS, hello skript automaticky nastaví trvalé vzájemné důvěryhodnosti hello správce účtu, který zadáte. Pro uživatele bez oprávnění správce, které vytvoříte v doméně hello clusteru máte tooset až dočasné vzájemné vztah důvěryhodnosti mezi uzly hello při úlohy je přidělen toothem a odstraňte hello relace po dokončení úlohy hello. tooestablish důvěryhodnosti pro každého uživatele, zadejte RSA páru klíčů toohello clusteru, který HPC Pack používá pro vztah důvěryhodnosti hello.

### <a name="generate-an-rsa-key-pair"></a>Vygenerovat pár klíče RSA
Je snadno toogenerate pár klíče RSA, který obsahuje veřejný klíč a soukromý klíč, spuštěním hello Linux **ssh-keygen** příkaz.

1. Přihlaste se tooa počítač se systémem Linux.
2. Spusťte následující příkaz hello:
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > Stiskněte klávesu **Enter** toouse hello výchozí nastavení až do dokončení příkazu hello. Nezadávejte přístupové heslo zde; Po zobrazení výzvy k zadání hesla, stačí stisknout klávesu **Enter**.
   > 
   > 
   
   ![Vygenerovat pár klíče RSA][keygen]
3. Změňte adresář toohello ~/.ssh adresář. Hello privátní klíč uložen v id_rsa a hello veřejný klíč v id_rsa.pub.
   
   ![Privátní a veřejné klíče][keys]

### <a name="add-hello-key-pair-toohello-hpc-pack-cluster"></a>Přidejte cluster HPC Pack toohello páru klíčů hello
1. Zkontrolujte vzdálené plochy připojení tooyour hlavního uzlu pomocí účtu správce HPC Pack (hello účet správce, které můžete nastavit při spuštění skriptu nasazení hello).
2. Použijte standardní systému Windows Server postupy toocreate účet uživatele domény v doméně služby Active Directory hello clusteru. Například použijte nástroj počítače a hello uživatele služby Active Directory hello hlavního uzlu. Hello příklady v tomto článku předpokládá, že vytvoříte uživatele domény s názvem hpclab\hpcuser.
3. Vytvořte soubor s názvem C:\cred.xml a zkopírujte hello RSA klíčová data do ní. Ukázkový soubor cred.xml je na konci hello tohoto článku.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. Otevřete příkazový řádek a zadejte následující příkaz údaje tooset hello data pro účet hpclab\hpcuser hello hello. Použít hello **extendeddata** toopass parametr hello název souboru C:\cred.xml jste vytvořili pro hello klíčová data.
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Tento příkaz se dokončí úspěšně bez výstupu. Po nastavení hello přihlašovací údaje pro hello uživatelské účty, které že budete potřebovat toorun úlohy, hello cred.xml soubor uložte na bezpečném místě, nebo ho odstranit.
5. Pokud jste vygenerovali hello páru klíčů RSA na jednom z uzlů Linux, mějte na paměti klíče hello toodelete po dokončení jejich používání. Pokud HPC Pack najde existující id_rsa soubor nebo soubor id_rsa.pub, nenastaví až vzájemné vztah důvěryhodnosti.

> [!IMPORTANT]
> Nedoporučujeme spuštěna úlohu Linux jako správce clusteru na sdílený clusteru, protože úlohu odeslání správcem spouští hello kořenového účtu na uzlech Linux hello. Však úloha odeslána uživatel není správcem spouští pod místním uživatelským účtem Linux s hello stejný název jako uživatel úlohu hello. V takovém případě HPC Pack nastaví vzájemné vztahu důvěryhodnosti pro tohoto uživatele Linux mezi uzly hello přidělené toohello úlohy. Můžete nastavit uživatele Linux hello ručně na hello Linuxové uzly před spuštěním úlohy hello nebo HPC Pack hello uživatele automaticky vytvoří při odeslání úlohy hello. Pokud HPC Pack vytvoří hello uživatele, HPC Pack neodstraní po dokončení úlohy hello. tooreduce bezpečnostní hrozby HPC Pack odebere hello klíče po dokončení úlohy.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Nastavení sdílené složky pro Linuxové uzly
Nyní nastavte standardní sdílené složky protokolu SMB ve složce hello hlavního uzlu. tooallow hello Linux uzly tooaccess soubory aplikace pomocí běžných cestu, hello připojení sdílené složky na uzlech Linux hello. Pokud chcete, můžete použít jinou možnost, jako je například Azure Files sdílená složka – doporučené pro mnoho scénářů - nebo sdílené složky NFS pro sdílení souborů. Naleznete v souboru hello sdílení informací a podrobné pokyny v [začít pracovat s Linux výpočetní uzly v clusteru HPC Pack v Azure](hpcpack-cluster.md).

1. Vytvořte složku hello hlavního uzlu a sdílejte ji tooEveryone nastavením oprávnění pro čtení a zápis. Například sdílet C:\OpenFOAM hello hlavního uzlu jako \\ \\SUSE12RDMA HN\OpenFOAM. Zde *SUSE12RDMA HN* je název hostitele hello hello hlavního uzlu.
2. Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy hello:
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

První příkaz Hello vytvoří složku s názvem /openfoam ve všech uzlech v hello LinuxNodes skupiny. druhý příkaz Hello připojí //SUSE12RDMA-HN/OpenFOAM hello sdílené složky na uzlech hello Linux s dir_mode a file_mode too777 sadu bitů. Hello *uživatelské jméno* a *heslo* hello příkazu musí být hello přihlašovací údaje uživatele hello hlavního uzlu.

> [!NOTE]
> Hello "\`" symbol v druhém příkazu hello je symbol řídicí pro prostředí PowerShell. "\`,"rozumí hello"," (čárku) je součástí příkaz hello.
> 
> 

## <a name="install-mpi-and-openfoam"></a>Instalace MPI a OpenFOAM
toorun OpenFOAM jako úloha MPI v síti hello RDMA, musíte toocompile OpenFOAM s knihovnami Intel MPI hello. 

Nejprve spustit několik **clusrun** příkazy tooinstall Intel MPI knihovny (pokud ještě není nainstalovaná) a OpenFOAM na Linuxových uzlů. Použití hello hlavního uzlu sdílení nakonfigurovali dříve tooshare hello instalačních souborů mezi uzly Linux hello.

> [!IMPORTANT]
> Tato instalace a kompilace kroky jsou příklady. Budete potřebovat některé znalosti tooensure správy systému Linux správně nainstalován závislý kompilátory a knihovny. Vaše verze Intel MPI a OpenFOAM bude pravděpodobně nutné toomodify určité proměnné prostředí nebo jiná nastavení. Podrobnosti najdete v tématu [Intel MPI knihovny pro Průvodce instalací Linux](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) a [instalace sady zdroj OpenFOAM](http://openfoam.org/download/2-3-1-source/) pro vaše prostředí.
> 
> 

### <a name="install-intel-mpi"></a>Nainstalujte Intel MPI
Uložte hello stažený instalační balíček pro Intel MPI (l_mpi_p_5.0.3.048.tgz v tomto příkladu) v C:\OpenFoam hello hlavního uzlu tak, aby uzly Linux hello přístup z /openfoam tento soubor. Spusťte **clusrun** tooinstall Intel MPI knihovny na všech uzlech Linux hello.

1. Hello následující příkazy kopírování hello instalační balíček a rozbalte ho příliš/opt/intel na každém uzlu.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. tooinstall Intel MPI knihovny tiše, použijte soubor silent.cfg. Příklad můžete najít v hello ukázkových souborů v hello konci tohoto článku. Tento soubor hello místní sdílené složky /openfoam. Podrobnosti o hello silent.cfg souboru najdete v tématu [Intel MPI knihovny pro Průvodce instalací Linux - tichou instalaci](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).
   
   > [!TIP]
   > Ujistěte se, že můžete uložit vaše silent.cfg soubor jako textový soubor s Linux konce řádků, (pouze LF, není CR LF). Tento krok zajistí, že správně běží na uzlech Linux hello.
   > 
   > 
3. Knihovna MPI Intel instalaci v bezobslužném režimu.
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a>Konfigurace MPI
Pro testování, měli byste přidat hello následující řádky toohello /etc/security/limits.conf na každém uzlu hello Linux:

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Po aktualizaci souboru limits.conf hello, restartujte hello Linuxových uzlů. Například použijte hello **clusrun** příkaz:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Po restartování počítače, zkontrolujte, zda že je této sdílené složce hello připojit jako /openfoam.

### <a name="compile-and-install-openfoam"></a>Kompilace a nainstalujte OpenFOAM
Uložte stažený instalační balíček hello hello tooC:\OpenFoam OpenFOAM zdroj aktualizací Service Pack (OpenFOAM-2.3.1.tgz v tomto příkladu) hello hlavního uzlu, aby uzly Linux hello přístup z /openfoam tento soubor. Spusťte **clusrun** příkazy hello toocompile OpenFOAM na všech uzlech Linux.

1. Vytvořte složku /opt/OpenFOAM na každý uzel, Linux, kopírovat hello zdroje balíčku toothis složku a rozbalte ho.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. toocompile OpenFOAM s hello knihovně MPI Intel, nejprve nastavte některé proměnné prostředí pro Intel MPI i OpenFOAM. Pomocí skriptu bash nazývané settings.sh tooset hello proměnné. Příklad můžete najít v hello ukázkových souborů v hello konci tohoto článku. Místní tohoto souboru (Uložit s konce řádků Linux) v hello sdílené složky /openfoam. Tento soubor zároveň obsahuje nastavení pro hello MPI a OpenFOAM runtimes použít novější toorun úlohu OpenFOAM.
3. Nainstalujte závislé balíčky vyžadované toocompile OpenFOAM. V závislosti na vaší distribuci systému Linux bude pravděpodobně nutné nejprve tooadd úložiště. Spustit **clusrun** podobné toohello následující příkazy:
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    V případě potřeby SSH tooeach Linux uzlu toorun hello příkazy tooconfirm, které fungují správně.
4. Spusťte následující příkaz toocompile OpenFOAM hello. Hello kompilaci proces trvá některé toocomplete čas a generuje velké množství výstup toostandard informace protokolu, takže použití hello **/ prokládaný** možnost výstup hello toodisplay prokládaný.
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > Hello "\`" symbol v příkazu hello je symbol řídicí pro prostředí PowerShell. "\`&" znamená hello "a" je součástí příkaz hello.
   > 
   > 

## <a name="prepare-toorun-an-openfoam-job"></a>Příprava toorun úlohu OpenFOAM
Nyní get připravené toorun úlohu MPI volá sloshingTank3D, což je jedna ze hello OpenFoam ukázky, na dva uzly Linux. 

### <a name="set-up-hello-runtime-environment"></a>Nastavení prostředí runtime hello
tooset hello prostředí runtime pro MPI a OpenFOAM na uzlech hello Linux, spusťte následující příkaz v okně prostředí Windows PowerShell hlavního uzlu hello hello. (Tento příkaz je platný pro SUSE Linux pouze.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Příprava ukázková data
Použití hello hlavního uzlu sdílení jste nakonfigurovali dříve tooshare soubory mezi uzly Linux hello (připojit jako /openfoam).

1. SSH tooone vaše Linux výpočetních uzlů.
2. Pokud jste to ještě neudělali, spusťte následující příkaz tooset hello OpenFOAM modulu runtime prostředí, hello.
   
   ```
   $ source /openfoam/settings.sh
   ```
3. Zkopírujte hello sloshingTank3D ukázka toohello sdílené složky a přejděte tooit.
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. Pokud použijete výchozí parametry hello této ukázky, může trvat desítkami toorun minut, proto je vhodné toomodify některé parametry toomake pracovat rychleji. Jeden jednoduchý volba je toomodify hello čas krok proměnné deltaT a writeInterval v souboru systému/controlDict hello. Tento soubor uloží veškerá vstupní data týkající se řízení toohello čas a čtení a zápis dat řešení. Můžete například změnit hodnotu hello deltaT z 0,05 too0.5 a hello hodnotu writeInterval z 0,05 too0.5.
   
   ![Umožňuje změnit proměnné krok][step_variables]
5. Zadejte požadované hodnoty pro proměnné hello v souboru systému/decomposeParDict hello. Tento příklad používá dva Linux uzly každý s 8 jádry tak nastavit numberOfSubdomains too16 a n hierarchicalCoeffs too(1 1 16), což znamená spustit OpenFOAM souběžně s 16 procesy. Další informace najdete v tématu [OpenFOAM uživatelská příručka: 3,4 aplikace běží paralelně](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).
   
   ![Rozložit procesy][decompose]
6. Spuštěním následujících příkazů z hello sloshingTank3D directory tooprepare hello ukázková data hello.
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. Hello hlavního uzlu měli byste vidět, že se zkopírují hello ukázkových datových souborů do C:\OpenFoam\sloshingTank3D. (C:\OpenFoam je hello sdílenou složku na hello hlavního uzlu.)
   
   ![Datové soubory hello hlavního uzlu][data_files]

### <a name="host-file-for-mpirun"></a>Soubor hostitele pro mpirun
V tomto kroku vytvoříte soubor hostitele (seznam výpočetní uzly) které hello **mpirun** příkaz používá.

1. Na jednom z uzlů hello Linux vytvořte soubor s názvem hostfile pod /openfoam, takže tento soubor lze dosáhnout za /openfoam/hostfile na všech uzlech Linux.
2. Názvy uzlu Linux zapisovat do tohoto souboru. V tomto příkladu hello soubor obsahuje hello následující názvy:
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > Tento soubor můžete vytvořit také v C:\OpenFoam\hostfile hello hlavního uzlu. Pokud zvolíte tuto možnost, uložte ho jako textový soubor s Linuxem konce řádků (pouze LF, není CR LF). To zajišťuje, že ho běží správně na uzlech Linux hello.
   > 
   > 
   
   **Obálka bash skriptu**
   
   Pokud máte mnoho Linuxových uzlů a chcete, aby vaše úlohy toorun na jenom některé z nich, není vhodné toouse, pevné hostitele souboru, protože nevíte uzlů, které se přidělí tooyour úlohy. V takovém případě zápisu Obálka skriptů bash pro **mpirun** toocreate hello soubor hostitele automaticky. Můžete najít obálku skriptů bash příklad názvem hpcimpirun.sh na konci hello tohoto článku a uložte ho jako /openfoam/hpcimpirun.sh. Tento ukázkový skript hello následující:
   
   1. Nastaví hello proměnných prostředí pro **mpirun**a některé příkaz přidání parametrů toorun hello MPI úlohy přes síť hello RDMA. V takovém případě nastaví hello následující proměnné:
      
      * I_MPI_FABRICS = shm:dapl
      * I_MPI_DAPL_PROVIDER = správcem v2-ib0
      * I_MPI_DYNAMIC_CONNECTION = 0
   2. Vytvoří soubor hostitele podle toohello $ proměnné prostředí CCP_NODES_CORES, jež je nastavena pomocí hlavního uzlu HPC hello při aktivaci hello úlohy.
      
      Formát Hello $CCP_NODES_CORES zahrnuje tento vzor:
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      kde
      
      * `<Number of nodes>`-hello počet uzlů přidělené toothis úlohy.  
      * `<Name of node_n_...>`-hello název každého uzlu přidělené toothis úlohy.
      * `<Cores of node_n_...>`-hello počet jader na hello uzlu přidělené toothis úlohy.
      
      Například pokud hello úlohy dvou uzlů toorun, $CCP_NODES_CORES je podobná
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. Volání hello **mpirun** příkazů a připojí dva parametry toohello příkazového řádku.
      
      * `--hostfile <hostfilepath>: <hostfilepath>`-Vytvoří hello cestu skript hello souboru hostitele hello
      * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`– Proměnná prostředí, která nastavuje hello HPC Pack hlavního uzlu, který ukládá číslo hello celkový počet jader přidělené toothis úlohy. V takovém případě určuje hello počet procesů pro **mpirun**.

## <a name="submit-an-openfoam-job"></a>Odeslat úlohu OpenFOAM
Teď můžete odeslat úlohu ve Správci clusteru HPC. Je nutné toopass hello skriptu hpcimpirun.sh v hello příkazových řádků pro některé úlohy hello.

1. Připojit tooyour hlavního uzlu clusteru a spusťte Správce clusteru HPC.
2. **Ve správě prostředků**, zajistěte, aby hello Linux výpočetní uzly v hello **Online** stavu. Pokud tomu tak není, vyberte je a klikněte na tlačítko **přepnout do režimu Online**.
3. V **úlohy správy**, klikněte na tlačítko **nová úloha**.
4. Zadejte název úlohy, jako *sloshingTank3D*.
   
   ![Podrobnosti úlohy][job_details]
5. V **úlohy prostředky**, vyberte typ prostředku jako "Uzel" hello a nastavte too2 minimální hello. Tato konfigurace se spouští úlohy hello na dva uzly Linux, z nichž každá má osm jader v tomto příkladu.
   
   ![Úloha prostředky][job_resources]
6. Klikněte na tlačítko **upravit úlohy** v hello levé navigaci a potom klikněte na **přidat** tooadd úlohu toohello úloh. Přidat čtyři úlohu toohello úlohy s hello následující nastavení a příkazové řádky.
   
   > [!NOTE]
   > Spuštění `source /openfoam/settings.sh` nastaví hello OpenFOAM a MPI běhová prostředí, takže každý z následujících úloh hello volá před hello OpenFOAM příkaz.
   > 
   > 
   
   * **Úloha 1**. Spustit **decomposePar** toogenerate datové soubory pro spuštění **interDyMFoam** paralelně.
     
     * Přiřadit jeden úkol toohello uzlu
     * **Příkazový řádek** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
     * **Pracovní adresář** -/ openfoam/sloshingTank3D
     
     Viz následující obrázek hello. Podobně nakonfigurujete zbývajících úkolech hello.
     
     ![Podrobnosti úlohy 1][task_details1]
   * **Úloha 2**. Spustit **interDyMFoam** v ukázce paralelní toocompute hello.
     
     * Přiřazení úkolů toohello dva uzly
     * **Příkazový řádek** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`
     * **Pracovní adresář** -/ openfoam/sloshingTank3D
   * **Úloha 3**. Spustit **reconstructPar** toomerge hello nastaví čas adresářů z každý adresář processor_N_ do jedné sady.
     
     * Přiřadit jeden úkol toohello uzlu
     * **Příkazový řádek** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`
     * **Pracovní adresář** -/ openfoam/sloshingTank3D
   * **Úloha 4**. Spustit **foamToEnsight** v paralelní tooconvert hello OpenFOAM výsledek soubory do EnSight formátu a hello EnSight soubory umístit do adresáře s názvem Ensight v adresáři případu hello.
     
     * Přiřazení úkolů toohello dva uzly
     * **Příkazový řádek** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`
     * **Pracovní adresář** -/ openfoam/sloshingTank3D
7. Přidání úkolů toothese závislosti ve vzestupném pořadí úkolů.
   
   ![Závislosti úkolů][task_dependencies]
8. Klikněte na tlačítko **odeslání** toorun tuto úlohu.
   
   Ve výchozím nastavení odešle HPC Pack úlohy hello jako váš aktuální účet přihlášeného uživatele. Po kliknutí na tlačítko **odeslání**, může se zobrazit dialogové okno s výzvou tooenter hello uživatelské jméno a heslo.
   
   ![Přihlašovací údaje úlohy][creds]
   
   Za určitých podmínek pamatuje HPC Pack hello vstupní před a nezobrazí toto dialogové okno informace o uživateli. toomake HPC Pack jej znovu zobrazit, zadejte následující příkaz na příkazovém řádku hello a pak odeslat úlohu hello.
   
   ```
   hpccred delcreds
   ```
9. Úloha Hello trvá z desítek minut tooseveral hodin podle toohello parametrů, které jste nastavili pro ukázku hello. V hello heat mapa najdete v části hello úlohy spuštěné v uzlech Linux hello. 
   
   ![Heat mapa][heat_map]
   
   Na každém uzlu jsou osm procesy spuštěné.
   
   ![Procesy Linux][linux_processes]
10. Po dokončení úlohy hello najdete ve složkách v rámci C:\OpenFoam\sloshingTank3D a souborů protokolů hello C:\OpenFoam výsledky úlohy hello.

## <a name="view-results-in-ensight"></a>Zobrazení výsledků v EnSight
Volitelně použijte [EnSight](https://www.ceisoftware.com/) toovisualize a analýza výsledků hello hello OpenFOAM úlohy. Další informace o vizualizace a animace v EnSight najdete v tématu to [video průvodce](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1. Po instalaci EnSight hello hlavního uzlu, spusťte ji.
2. Otevřete C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.
   
   Zobrazí se nádrží v prohlížeči hello.
   
   ![Nádrž v EnSight][tank]
3. Vytvoření **Isosurface** z **internalMesh**a potom zvolte hello proměnná **alpha_water**.
   
   ![Vytvoření isosurface][isosurface]
4. Nastavit barvu hello **Isosurface_part** vytvořili v předchozím kroku hello. Například nastavte toowater blue.
   
   ![Upravit isosurface barev][isosurface_color]
5. Vytvoření **Iso svazku** z **bariéry** výběrem **bariéry** v hello **částí** panelu a klikněte na tlačítko hello **Isosurfaces**  tlačítka na panelu nástrojů hello.
6. V dialogovém okně hello, vyberte **typ** jako **Isovolume** a nastavte hello minimum z **Isovolume rozsah** too0.5. toocreate hello isovolume, klikněte na tlačítko **vytvořit s vybraných částí**.
7. Nastavit barvu hello **Iso_volume_part** vytvořili v předchozím kroku hello. Například nastavte toodeep horních blue.
8. Nastavit barvu hello **bariéry**. Například nastavte tootransparent bílé.
9. Klikněte na tlačítko **přehrání** toosee hello výsledky hello simulace.
   
    ![Výsledek nádrž][tank_result]

## <a name="sample-files"></a>Ukázkové soubory
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Ukázkový soubor XML konfigurace pro nasazení clusteru pomocí skriptu prostředí PowerShell
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Ukázkový soubor cred.xml
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-tooinstall-mpi"></a>Ukázka silent.cfg soubor tooinstall MPI
```
# Patterns used toocheck silent configuration file
#
# anythingpat - any string
# filepat     - hello file location pattern (/file/location/to/license.lic)
# lspat       - hello license server address pattern (0123@hostname)
# snpat       - hello serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components tooinstall, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path toohello cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes tooenable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes tooupdate ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Ukázkový skript settings.sh
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a>Ukázkový skript hpcimpirun.sh
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run hello mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into hello hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]:media/hpcpack-cluster-openfoam/keygen.png
[keys]:media/hpcpack-cluster-openfoam/keys.png
[step_variables]:media/hpcpack-cluster-openfoam/step_variables.png
[data_files]:media/hpcpack-cluster-openfoam/data_files.png
[decompose]:media/hpcpack-cluster-openfoam/decompose.png
[job_details]:media/hpcpack-cluster-openfoam/job_details.png
[job_resources]:media/hpcpack-cluster-openfoam/job_resources.png
[task_details1]:media/hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]:media/hpcpack-cluster-openfoam/task_dependencies.png
[creds]:media/hpcpack-cluster-openfoam/creds.png
[heat_map]:media/hpcpack-cluster-openfoam/heat_map.png
[tank]:media/hpcpack-cluster-openfoam/tank.png
[tank_result]:media/hpcpack-cluster-openfoam/tank_result.png
[isosurface]:media/hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]:media/hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]:media/hpcpack-cluster-openfoam/linux_processes.png
