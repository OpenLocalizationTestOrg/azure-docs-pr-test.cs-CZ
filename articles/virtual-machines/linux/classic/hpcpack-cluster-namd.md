---
title: "aaaNAMD pomocí sady Microsoft HPC Pack na virtuální počítače s Linuxem | Microsoft Docs"
description: "Nasazení clusteru s podporou sady Microsoft HPC Pack v Azure a spustit simulaci NAMD s charmrun v několika výpočetních uzlech Linux"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 76072c6b-ac35-4729-ba67-0d16f9443bd7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/13/2016
ms.author: danlep
ms.openlocfilehash: 90c722f66b494335a62c0613079366ae99002076
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Spuštění NAMD se sadou Microsoft HPC Pack ve výpočetních uzlech Linuxu v Azure
Tento článek ukazuje jeden ze způsobů toorun Linux vysoce výkonné výpočty (HPC) zatížení na virtuálních počítačích Azure. Zde můžete nastavit [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster v Azure s Linuxem výpočetních uzlů a spusťte [NAMD](http://www.ks.uiuc.edu/Research/namd/) toocalculate simulace a vizualizovat hello struktura velké biomolekulárních systému.  

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (pro program molekulární Dynamics Nanoscale) je paralelní molekulární dynamics balíček určený pro vysoce výkonné simulace velké biomolekulárních systémů, který obsahuje až toomillions atomů. Mezi tyto systémy patří například viry, struktury buňky a velké bílkoviny. NAMD škáluje toohundreds jader pro typické simulace a toomore než 500 000 jader pro největší simulace hello.
* **Microsoft HPC Pack** poskytuje funkce toorun rozsáhlé HPC a paralelní aplikace v clusterech místního počítače nebo virtuální počítače Azure. Původně vyvinut jako řešení pro úlohy Windows HPC, HPC Pack teď podporuje spouštění aplikací prostředí HPC pro Linux v systému Linux výpočetní uzel virtuální počítače nasazené na clusteru HPC Pack. V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) úvod.

Pro další možnosti toorun Linux HPC najdete v části úlohy v Azure, [technických prostředcích pro batch a vysoce výkonné výpočty](../../../batch/batch-hpc-solutions.md).

## <a name="prerequisites"></a>Požadavky
* **Výpočetní uzly clusteru HPC Pack operačního systému Linux** -nasazení clusteru HPC Pack s Linux výpočetní uzly v Azure pomocí buď [šablony Azure Resource Manageru](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) nebo [skript prostředí Azure PowerShell](hpcpack-cluster-powershell-script.md) . V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) hello požadavky a kroky pro jednu z možností. Pokud si zvolíte hello možnost nasazení skriptu prostředí PowerShell, najdete v části hello vzorový konfigurační soubor v hello ukázkových souborů v hello konci tohoto článku. Tento soubor nakonfiguruje clusteru služby založené na Azure HPC Pack skládající se z hlavního uzlu systému Windows Server 2012 R2 a čtyři výpočetní uzly velké 6.6 CentOS velikost. Tento soubor přizpůsobte podle potřeby pro vaše prostředí.
* **Soubory softwaru a kurzu NAMD** -NAMD stažení softwaru pro Linux z hello [NAMD](http://www.ks.uiuc.edu/Research/namd/) lokality (vyžaduje registraci). Tento článek je založena na NAMD verze 2.10 a používá hello [Linux-x86_64 (64-bit Intel nebo AMD s sítě Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archivu. Také stáhnout hello [NAMD kurz soubory](http://www.ks.uiuc.edu/Training/Tutorials/#namd). soubory ke stažení Hello jsou soubory .tar a potřebujete souborů hello tooextract nástroj Windows hello hlavního uzlu clusteru. soubory hello tooextract, postupujte podle pokynů hello později v tomto článku. 
* **VMD** (volitelné) – toosee hello výsledků úlohy NAMD, stáhněte a nainstalujte program molekulární vizualizace hello [VMD](http://www.ks.uiuc.edu/Research/vmd/) na počítači podle svého výběru. aktuální verze Hello je 1.9.2. V tématu hello VMD stáhnout tooget lokality spuštěn.  

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Nastavení vzájemného vztahu důvěryhodnosti mezi výpočetní uzly
Spuštění úlohy mezi uzly ve více uzlech Linux vyžaduje hello uzly tootrust, vzájemně (podle **rsh** nebo **ssh**). Při vytváření clusteru HPC Pack hello s hello skript nasazení Microsoft HPC Pack IaaS, hello skript automaticky nastaví trvalé vzájemné důvěryhodnosti hello správce účtu, který zadáte. Pro uživatele bez oprávnění správce, které vytvoříte v doméně hello clusteru máte tooset až dočasné vzájemné vztah důvěryhodnosti mezi uzly hello při přidělení toothem úlohu. Po dokončení úlohy hello pak, odstraňte hello vztah. toodo to pro každého uživatele, zadejte clusteru toohello páru klíčů RSA které HPC Pack používá tooestablish hello důvěryhodný vztah. Postupujte podle pokynů.

### <a name="generate-an-rsa-key-pair"></a>Vygenerovat pár klíče RSA
Je snadno toogenerate pár klíče RSA, který obsahuje veřejný klíč a soukromý klíč, spuštěním hello Linux **ssh-keygen** příkaz.

1. Přihlaste se tooa počítač se systémem Linux.
2. Spusťte následující příkaz hello:
   
   ```bash
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
1. [Připojit pomocí vzdálené plochy](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello hlavního uzlu virtuální počítač pomocí hello pověření domény, které jste zadali při nasazení clusteru hello (například hpc\clusteradmin). Spravujete hello clusteru z hlavního uzlu hello.
2. Použijte standardní systému Windows Server postupy toocreate účet uživatele domény v doméně služby Active Directory hello clusteru. Například použijte nástroj počítače a hello uživatele služby Active Directory hello hlavního uzlu. Hello příklady v tomto článku předpokládá, že vytvoříte uživatele domény s názvem hpcuser v doméně hpclab hello (hpclab\hpcuser).
3. Přidejte hello domény uživatele toohello HPC Pack clusteru jako uživatel clusteru. Pokyny najdete v tématu [přidat nebo odebrat uživatele clusteru](https://technet.microsoft.com/library/ff919330.aspx).
4. Vytvořte soubor s názvem C:\cred.xml a zkopírujte hello RSA klíčová data do ní. Příklad můžete najít v hello ukázkových souborů v hello konci tohoto článku.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
5. Otevřete příkazový řádek a zadejte následující příkaz údaje tooset hello data pro účet hpclab\hpcuser hello hello. Použít hello **extendeddata** toopass parametr hello název souboru hello C:\cred.xml jste vytvořili pro hello klíčová data.
   
   ```command
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Tento příkaz se dokončí úspěšně bez výstupu. Po nastavení hello přihlašovací údaje pro hello uživatelské účty, které že budete potřebovat toorun úlohy, hello cred.xml soubor uložte na bezpečném místě, nebo ho odstranit.
6. Pokud jste vygenerovali hello páru klíčů RSA na jednom z uzlů Linux, mějte na paměti klíče hello toodelete po dokončení jejich používání. HPC Pack nejsou nastaveny vzájemné vztahu důvěryhodnosti, pokud najde existující soubor id_rsa nebo id_rsa.pub souboru.

> [!IMPORTANT]
> Nedoporučujeme spuštěna úlohu Linux jako správce clusteru na sdílený clusteru, protože úlohu odeslání správcem spouští hello kořenového účtu na uzlech Linux hello. Úloha odeslána uživatel není správcem spouští pod místním uživatelským účtem Linux s hello stejný název jako uživatel úlohu hello. V takovém případě HPC Pack nastaví vzájemné vztahu důvěryhodnosti pro tohoto uživatele Linux pro všechny uzly hello přidělené toohello úlohy. Můžete nastavit uživatele Linux hello ručně na hello Linuxové uzly před spuštěním úlohy hello nebo HPC Pack hello uživatele automaticky vytvoří při odeslání úlohy hello. Pokud HPC Pack vytvoří hello uživatele, HPC Pack neodstraní po dokončení úlohy hello. tooreduce ohrožení zabezpečení, hello, které jsou odebrány klíče po dokončení úlohy hello na uzlech hello.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Nastavení sdílené složky pro Linuxové uzly
Nyní nastavit sdílenou složku SMB a připojit hello sdílenou složku na všechny Linux uzly tooallow hello Linux uzly tooaccess NAMD soubory s běžné cestou. Tady jsou kroky toomount sdílenou složku hello hlavního uzlu. Sdílenou složku se doporučuje pro distribuce například CentOS 6.6, které aktuálně nepodporují službu Azure File hello. Pokud uzly Linux podporují sdílenou složku Azure, najdete v části [jak toouse Azure File storage s Linuxem](../../../storage/files/storage-how-to-use-files-linux.md). Další možnosti sdílení s HPC Pack souborů, najdete v části [začít pracovat s Linux výpočetní uzly v clusteru HPC Pack v Azure](hpcpack-cluster.md).

1. Vytvořte složku hello hlavního uzlu a sdílejte ji tooEveryone nastavením oprávnění pro čtení a zápis. V tomto příkladu \\ \\CentOS66HN\Namd je název hello hello složky, kde CentOS66HN je název hostitele hello hello hlavního uzlu.
2. Vytvořte podsložku s názvem namd2 ve sdílené složce hello. V namd2 vytvořte další podsložku s názvem namdsample.
3. Extrahujte hello NAMD souborů ve složce hello tak, že používáte verzi systému Windows **vkládání** nebo jiný nástroj Windows, který pracuje s archivy .tar. 
   
   * Extrahujte archivní vkládání NAMD hello příliš\\\\CentOS66HN\Namd\namd2.
   * Extrahujte soubory hello kurzu v části \\ \\CentOS66HN\Namd\namd2\namdsample.
4. Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy toomount hello sdílenou složku na Linuxových uzlů hello hello.
   
    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

První příkaz Hello vytvoří složku s názvem /namd2 ve všech uzlech v hello LinuxNodes skupiny. druhý příkaz Hello připojí //CentOS66HN/Namd/namd2 hello sdílené složky do složky hello s dir_mode a file_mode too777 sadu bitů. Hello *uživatelské jméno* a *heslo* hello příkazu musí být hello přihlašovací údaje uživatele hello hlavního uzlu.

> [!NOTE]
> Hello "\`" symbol v druhém příkazu hello je symbol řídicí pro prostředí PowerShell. "\`,"rozumí hello"," (čárku) je součástí příkaz hello.
> 
> 

## <a name="create-a-bash-script-toorun-a-namd-job"></a>Vytvořit úlohu NAMD toorun Bash skriptu
Potřebuje vaše úlohy NAMD *seznamu* souboru **charmrun** toodetermine hello počet uzlů toouse při spouštění NAMD procesy. Použít skript Bash, který generuje souboru seznamu hello a spouští **charmrun** s tohoto souboru seznamu. Potom můžete odeslat úlohu NAMD ve Správci clusteru HPC který volá tento skript.

Pomocí textového editoru podle vaší volby, vytvořte skript Bash ve složce /namd2 hello obsahující soubory programu hello NAMD a pojmenujte ji hpccharmrun.sh. Pro rychlé testování konceptu, zkopírujte skript hpccharmrun.sh příklad hello uvedených v hello konci tohoto článku, přejděte příliš[odeslat úlohu NAMD](#submit-a-namd-job).

> [!TIP]
> Uložte skript jako textový soubor s Linuxem řádek zakončení (pouze LF, není CR LF). To zajišťuje, že ho běží správně na uzlech Linux hello.
> 
> 

Následují informace o jaké tento skript bash. 

1. Definujte některé proměnné.
   
   ```bash
   #!/bin/bash
   
   # hello path of this script
   SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
   # Charmrun command
   CHARMRUN=${SCRIPT_PATH}/charmrun
   # Argument of ++nodelist
   NODELIST_OPT="++nodelist"
   # Argument of ++p
   NUMPROCESS="+p"
   ```
2. Získáte informace o uzlu ze hello proměnné prostředí. $NODESCORES ukládá seznam rozdělení slov z $CCP_NODES_CORES. $COUNT je hello velikost $NODESCORES.
   
   ```
   # Get node information from hello environment variables
   NODESCORES=(${CCP_NODES_CORES})
   COUNT=${#NODESCORES[@]}
   ```    
   
   Formát Hello hello $CCP_NODES_CORES proměnné je následující:
   
   ```
   <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
   ```
   
   Tato proměnná uvádí hello celkový počet uzlů, názvy a počet jader na každém uzlu, které jsou přiděleny toohello úlohy. Například pokud úloha hello potřebuje 10 toorun jader, hello $CCP_NODES_CORES hodnotu podobně jako:
   
   ```
   3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
   ```
3. Pokud hello $CCP_NODES_CORES proměnná není nastavena, spusťte **charmrun** přímo. (To by mělo pouze nastat při spuštění tohoto skriptu přímo na Linuxových uzlů.)
   
   ```
   if [ ${COUNT} -eq 0 ]
   then
     # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
     #echo ${CHARMRUN} $*
     ${CHARMRUN} $*
   ```
4. Nebo vytvořte souboru seznamu pro **charmrun**.
   
   ```
   else
     # Create hello nodelist file
     NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$
   
     # Write hello head line
     echo "group main" > ${NODELIST_PATH}
   
     # Get every node name and number of cores and write into hello nodelist file
     I=1
     while [ ${I} -lt ${COUNT} ]
     do
         echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
         let "I=${I}+2"
     done
   ```
5. Spustit **charmrun** s hello souboru seznamu, získat návratový stav a odeberte hello souboru seznamu na konci hello.
   
   ${CCP_NUMCPUS} je jiné proměnné prostředí, která nastavuje hlavního uzlu HPC Pack hello. Ukládají se hello počet celkový počet jader přidělené toothis úlohy. Budeme používat toospecify hello počet procesů charmrun.
   
   ```
   # Run charmrun with nodelist arg
   #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   
   RTNSTS=$?
   rm -f ${NODELIST_PATH}
   fi
   
   ```
6. Ukončení s hello **charmrun** návratový stav.
   
   ```
   exit ${RTNSTS}
   ```

Toto je hello informace v souboru seznamu hello, které hello skript generuje:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Například:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Odeslání úlohy NAMD
Teď je připraven toosubmit úlohu NAMD ve Správci clusteru HPC.

1. Připojit tooyour hlavního uzlu clusteru a spusťte Správce clusteru HPC.
2. V **Správa prostředků**, zajistěte, aby hello Linux výpočetní uzly v hello **Online** stavu. Pokud tomu tak není, vyberte je a klikněte na tlačítko **přepnout do režimu Online**.
3. V **úlohy správy**, klikněte na tlačítko **nová úloha**.
4. Zadejte název úlohy, jako *hpccharmrun*.
   
   ![Nové úlohy HPC][namd_job]
5. Na hello **podrobnosti úlohy** v části **úlohy prostředky**, vyberte typ prostředku jako hello **uzlu** a sadu hello **minimální** too3. , jsme spustit úlohu hello na tři uzly Linux a každý uzel má čtyři jádra.
   
   ![Úloha prostředky][job_resources]
6. Klikněte na tlačítko **upravit úlohy** v hello levé navigaci a potom klikněte na **přidat** tooadd úlohu toohello úloh.    
7. Na hello **podrobností úlohy a přesměrování vstupu a výstupu** stránku hello sadu následující hodnoty:
   
   * **Příkazový řádek**-
     `/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`
     
     > [!TIP]
     > Hello předchozí příkazový řádek je jeden příkaz bez konce řádků. Ho zabalí tooappear na několika řádcích pod **příkazového řádku**.
     > 
     > 
   * **Pracovní adresář** -/namd2
   * **Minimální** – 3
     
     ![Podrobnosti úlohy][task_details]
     
     > [!NOTE]
     > Hello pracovní adresář tady nastavíte protože **charmrun** pokusí toonavigate toohello stejný pracovní adresář na každém uzlu. Pokud není nastavena hello pracovní adresář, HPC Pack spustí příkaz hello náhodným názvem složky vytvořené v jednom z uzlů Linux hello. To způsobí, že hello následující chyba na hello dalších uzlů: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` tooavoid tyto potíže odstranit, zadejte cestu ke složce, která je přístupná ve všech uzlech jako hello pracovní adresář.
     > 
     > 
8. Klikněte na tlačítko **OK** a pak klikněte na **odeslání** toorun tuto úlohu.
   
   Ve výchozím nastavení odešle HPC Pack úlohy hello jako váš aktuální účet přihlášeného uživatele. Dialogové okno mohou vyžadovat tooenter hello uživatelské jméno a heslo po kliknutí na tlačítko **odeslání**.
   
   ![Přihlašovací údaje úlohy][creds]
   
   Za určitých podmínek pamatuje HPC Pack hello vstupní před a nezobrazí toto dialogové okno informace o uživateli. toomake HPC Pack jej znovu zobrazit, zadejte následující příkaz na příkazovém řádku hello a pak odeslat úlohu hello.
   
   ```command
   hpccred delcreds
   ```
9. Úloha Hello trvá několik minut toofinish.
10. Najít protokol hello úlohy v \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log a hello výstupní soubory v \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.
11. Volitelně můžete spusťte VMD tooview výsledky úlohy. Hello kroky pro vizualizaci hello NAMD výstupní soubory (v tomto případě molekuly bílkovin ubiquitin v oblasti horních) jsou nad rámec tohoto článku hello. V tématu [NAMD kurzu](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) podrobnosti.
    
    ![Výsledky úlohy][vmd_view]

## <a name="sample-files"></a>Ukázkové soubory
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Ukázkový soubor XML konfigurace pro nasazení clusteru pomocí skriptu prostředí PowerShell
```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
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

### <a name="sample-hpccharmrunsh-script"></a>Ukázkový skript hpccharmrun.sh
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run hello charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create hello nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write hello head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into hello nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 

<!--Image references-->
[keygen]:media/hpcpack-cluster-namd/keygen.png
[keys]:media/hpcpack-cluster-namd/keys.png
[namd_job]:media/hpcpack-cluster-namd/namd_job.png
[job_resources]:media/hpcpack-cluster-namd/job_resources.png
[creds]:media/hpcpack-cluster-namd/creds.png
[task_details]:media/hpcpack-cluster-namd/task_details.png
[vmd_view]:media/hpcpack-cluster-namd/vmd_view.png
