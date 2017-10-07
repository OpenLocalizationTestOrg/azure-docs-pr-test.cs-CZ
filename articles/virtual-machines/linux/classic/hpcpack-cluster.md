---
title: "aaaLinux výpočetní virtuálních počítačů v clusteru služby HPC Pack | Microsoft Docs"
description: "Zjistěte, jak toocreate a použití HPC Pack clusteru v Azure pro Linux s vysokým výkonem úloh (prostředí HPC)"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 9ed20d6cd69a6472a00666caf8965e9d022698a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Začínáme s výpočetními uzly Linuxu v clusteru HPC Pack v Azure
Nastavení [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) clusteru v Azure, který obsahuje hlavního uzlu se systémem Windows Server a několika výpočetních uzlech systémem podporované distribuce systému Linux. Prozkoumejte možnosti toomove dat mezi uzly Linux hello a hello Windows hlavního uzlu clusteru hello. Zjistěte, jak toosubmit Linux HPC úlohy toohello clusteru.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Na vysoké úrovni, hello následující diagram znázorňuje clusteru HPC Pack hello vytváření a práci s.

![Cluster HPC Pack s uzly Linux][scenario]

Pro další možnosti toorun Linux HPC najdete v části úlohy v Azure, [technických prostředcích pro batch a vysoce výkonné výpočty](../../../batch/big-compute-resources.md).

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a>Nasazení clusteru HPC Pack s Linux výpočetních uzlů
Tento článek popisuje dvě možnosti toodeploy clusteru HPC Pack služby v Azure, s Linuxových výpočetních uzlů. Obě metody použít image pořízenou prostřednictvím Marketplace systému Windows Server s HPC Pack toocreate hello hlavního uzlu. 

* **Šablona Azure Resource Manageru** -použít šablonu z Azure Marketplace hello nebo šabloně pro rychlý start z hello komunity, tooautomate vytváření clusteru hello v modelu nasazení Resource Manager hello. Například hello [clusteru HPC Pack pro Linux úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) šablony v hello Azure Marketplace vytvoří kompletní infrastruktura clusteru HPC Pack pro Linux HPC úlohy.
* **Skript prostředí PowerShell** -použití hello [skript nasazení Microsoft HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) tooautomate nasazení clusterů v modelu nasazení classic hello. Tento skript prostředí Azure PowerShell používá bitovou kopii prostředí HPC Pack virtuálních počítačů v hello Azure Marketplace pro rychlé nasazení a poskytuje komplexní sadu toodeploy parametry konfigurace Linuxových výpočetních uzlů.

Další informace o možnostech nasazení clusteru HPC Pack v Azure najdete v tématu [možnosti toocreate a spravovat cluster vysoce výkonné výpočty (HPC) v Azure pomocí sady Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="prerequisites"></a>Požadavky
* **Předplatné Azure** -v buď hello globální Azure nebo Azure China služby uživatelům pomocí odběru. Pokud účet nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) si během několika minut.
* **Kvóta jader** -může být nutné tooincrease hello kvóty jader, obzvláště pokud se rozhodnete toodeploy několik uzlů clusteru s vícejádrovými velikosti virtuálních počítačů. tooincrease kvótu, otevřete žádosti o podporu online zákazníka zdarma.
* **Linuxových distribucích** -aktuálně HPC Pack podporuje hello následující Linuxových distribucích pro výpočetní uzly. Můžete použít Marketplace verze těchto distribuce, pokud jsou k dispozici, nebo zadat vlastní.
  
  * **Na základě centOS**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, verze 6.5 HPC, 7.1 HPC
  * **Red Hat Enterprise Linux**: 6.7, 6.8, 7.2
  * **SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 pro prostředí HPC, SLES 12 pro HPC (Premium)
  * **Ubuntu Server**: 14.04 LTS, 16.04 LTS
    
    > [!TIP]
    > toouse hello Azure RDMA síť s jedním velikostí virtuálních počítačů hello podporu rdma, zadejte SUSE Linux Enterprise Server 12 HPC nebo na základě CentOS HPC bitovou kopii z hello Azure Marketplace. Další informace najdete v tématu [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

Další požadavky toodeploy hello clusteru pomocí skriptu nasazení HPC Pack IaaS hello:

* **Klientský počítač** -je nutné klienta se systémem Windows toorun hello clusteru skript nasazení do počítače.
* **Prostředí Azure PowerShell** - [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) (verze 0.8.10 nebo novější) na klientském počítači.
* **Skript nasazení HPC Pack IaaS** – stáhněte a rozbalte hello nejnovější verzi hello skript z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Verze hello hello skriptu můžete zkontrolovat spuštěním `.\New-HPCIaaSCluster.ps1 –Version`. Tento článek je založen na verzi 4.4.1 nebo později hello skriptu.

### <a name="deployment-option-1-use-a-resource-manager-template"></a>Možnost nasazení 1. Pomocí šablony Resource Manageru
1. Přejděte toohello [clusteru HPC Pack pro Linux úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) šablony v hello Azure Marketplace a klikněte na **nasadit**.
2. V hello portálu Azure, zkontrolujte hello informace a pak klikněte na tlačítko **vytvořit**.
   
    ![Vytvoření portálu][portal]
3. Na hello **Základy** okno, zadejte název clusteru hello, což také názvy hello hlavního uzlu virtuálního počítače. Můžete vybrat existující skupinu prostředků nebo vytvořte skupinu pro hello nasazení v umístění, které je k dispozici tooyou. Hello umístění má vliv na dostupnost hello určité velikosti virtuálních počítačů a dalším službám Azure (viz [produkty podle oblasti](https://azure.microsoft.com/regions/services/)).
4. Na hello **hlavního uzlu nastavení** okně pro první nasazení, můžete obvykle přijmout výchozí nastavení hello. 
   
   > [!NOTE]
   > Hello **adresu URL skriptu po konfiguraci** je volitelné nastavení toospecify veřejně dostupné skript prostředí Windows PowerShell, které mají toorun hello hlavního uzlu virtuálního počítače po je spuštěná. 
   > 
   > 
5. Na hello **výpočetní uzel nastavení** okně vyberte vzoru pro pojmenovávání pro hello uzly, hello počet a velikost uzlů hello a hello toodeploy distribuce systému Linux.
6. Na hello **nastavení infrastruktury** okno, zadejte pro hello virtuální sítě a služby Active Directory název domény, domény a přihlašovací údaje Správce virtuálních počítačů a vzoru pro pojmenovávání pro účty úložiště hello.
   
   > [!NOTE]
   > HPC Pack používá hello služby Active Directory domény tooauthenticate clusteru uživatele. 
   > 
   > 
7. Po spuštění hello ověřovací testy a zkontrolujte hello podmínky použití, klikněte na tlačítko **nákupu**.

### <a name="deployment-option-2-use-hello-iaas-deployment-script"></a>Možnost nasazení 2. Použít skript nasazení IaaS hello
Toto jsou další požadavky toodeploy hello clusteru pomocí skriptu nasazení HPC Pack IaaS hello:

* **Klientský počítač** -je nutné klienta se systémem Windows toorun hello clusteru skript nasazení do počítače.
* **Prostředí Azure PowerShell** - [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) (verze 0.8.10 nebo novější) na klientském počítači.
* **Skript nasazení HPC Pack IaaS** – stáhněte a rozbalte hello nejnovější verzi hello skript z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Verze hello hello skriptu můžete zkontrolovat spuštěním `.\New-HPCIaaSCluster.ps1 –Version`. Tento článek je založen na verzi 4.4.1 nebo později hello skriptu.

**Konfigurační soubor XML.**

Hello skript nasazení HPC Pack IaaS používá konfigurační soubor XML jako vstupní toodescribe clusteru HPC hello. Hello následující vzorový konfigurační soubor Určuje malé cluster obsahuje hlavnímu uzlu HPC Pack a dvě velikost A7 CentOS 7.0 Linux výpočetních uzlů. 

Upravte soubor hello podle potřeby pro vaše prostředí a konfiguraci požadovaných clusteru a uložte s názvem, jako je například HPCDemoConfig.xml. Například musíte toosupply název předplatného a jedinečný název účtu úložiště a název cloudové služby. Kromě toho můžete chtít podporovaném toochoose jinou bitovou kopii systému Linux pro hello výpočetních uzlů. Další informace o hello elementy v konfiguračním souboru hello najdete v tématu hello Manual.rtf souboru ve složce hello skriptu a [vytvoření clusteru prostředí HPC s hello skript nasazení HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

**toorun hello skript nasazení HPC Pack IaaS**

1. Otevřete prostředí Windows PowerShell v klientském počítači hello jako správce.
2. Změna toohello adresář, kde je skript hello nainstalovaný (E:\IaaSClusterScript v tomto příkladu).
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. Spusťte následující příkaz toodeploy hello HPC Pack clusteru hello. Tento příklad předpokládá, že hello konfigurační soubor se nachází v E:\HPCDemoConfig.xml
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    a. Protože hello **AdminPassword** není zadán v hello předcházející příkaz, jsou výzvami tooenter hello heslo pro uživatele *MyAdminName*.
   
    b. skript Hello pak spustí toovalidate hello konfigurační soubor. To může trvat až tooseveral minut v závislosti na hello síťové připojení.
   
    ![Ověření][validate]
   
    c. Po ověření úspěšně, uvádí hello skriptu toocreate prostředky clusteru hello. Zadejte *Y* toocontinue.
   
    ![Zdroje][resources]
   
    d. skript Hello clusteru HPC Pack hello toodeploy při spuštění a dokončení konfigurace hello bez další ruční kroky. Hello skript můžete spustit několik minut.
   
    ![Nasazení][deploy]
   
   > [!NOTE]
   > V tomto příkladu hello skript generuje soubor protokolu automaticky od hello **- LogFile** není zadán parametr. protokoly Hello nezapisují v reálném čase, ale se shromažďují na konci hello hello vyhodnocení a nasazení hello. Pokud hello procesu prostředí PowerShell je zastavena v průběhu hello skript spuštěn, budou ztraceny některé protokoly.
   > 
   > 

## <a name="connect-toohello-head-node"></a>Připojit toohello hlavního uzlu
Po nasazení clusteru HPC Pack hello v Azure, [připojit pomocí vzdálené plochy](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello hlavního uzlu virtuální počítač pomocí hello pověření domény, které jste zadali při nasazení clusteru hello (například *hpc\\ clusteradmin*). Spravujete hello clusteru z hlavního uzlu hello.

Hello hlavního uzlu spusťte Správce clusteru HPC toocheck hello stav clusteru HPC Pack hello. Můžete spravovat a monitorovat Linux výpočetní uzly hello stejným způsobem jako pracujete s Windows výpočetních uzlů. Například můžete zjistit uzly Linux hello uvedené v **Správa prostředků** (tyto uzly se nasadí s hello **LinuxNode** šablony).

![Uzel správy][management]

Zobrazí také hello Linuxových uzlů v hello **Heat mapa** zobrazení.

![Heat mapa][heatmap]

## <a name="how-toomove-data-in-a-cluster-with-linux-nodes"></a>Jak toomove data v clusteru s uzly Linux
Máte několik možností toomove dat mezi uzly Linux a hello Windows hlavního uzlu clusteru hello. Tady jsou tři běžné metody popsané v podrobněji hello následující části:

* **Azure File** -zpřístupňuje spravované sdílené složky toostore dat souboru protokolu SMB soubory v úložišti Azure. Uzly Windows a Linux uzly můžete připojit Azure sdílené složky jako jednotky nebo složku v hello stejný čas, i v případě, že nasazené v různých virtuálních sítích.
* **Sdílet hlavního uzlu SMB** -připojí standardní sdílené složky Windows hello hlavního uzlu na Linuxových uzlů.
* **Server systému souborů NFS uzlu HEAD** -poskytuje řešení sdílení souborů pro smíšená prostředí systému Windows a Linux.

### <a name="azure-file-storage"></a>Úložiště Azure File
Hello [Azure File](https://azure.microsoft.com/services/storage/files/) služby zpřístupní sdílené složky pomocí standardní protokol SMB 2.1 hello. Virtuální počítače Azure a cloudových služeb můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a místní aplikace můžou k souborovým datům ve sdílené složce prostřednictvím hello API služby File storage. 

Podrobné kroky toocreate Azure File sdílet a připojit ho hello hlavního uzlu, najdete v části [Začínáme s Azure File storage ve Windows](../../../storage/files/storage-how-to-use-files-windows.md). sdílenou složku Azure File hello toomount na uzlech hello Linux, najdete v části [jak toouse Azure File storage s Linuxem](../../../storage/files/storage-how-to-use-files-linux.md). tooset až zachování připojení, najdete v části [Persisting připojení tooMicrosoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

V následujícím příkladu hello vytvoření účtu úložiště Azure sdílené složky. toomount hello sdílet hello hlavního uzlu, otevřete příkazový řádek a zadejte hello následující příkazy:

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

V tomto příkladu allvhdsje je váš název účtu úložiště, storageaccountkey je klíč účtu úložiště a rdma je název sdílené složky Azure File hello. sdílenou složku Azure File Hello je připojit jako Z: hello hlavního uzlu.

sdílenou složku Azure File hello toomount na uzlech Linux, spusťte **clusrun** příkaz hello hlavního uzlu. **[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  je užitečné toocarry nástroj HPC Pack Správce úloh ve více uzlech. (Viz také [Clusrun pro Linuxové uzly](#Clusrun-for-Linux-nodes) v tomto článku.)

Otevřete okno prostředí Windows PowerShell a zadejte hello následující příkazy:

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

První příkaz Hello vytvoří složku s názvem /rdma ve všech uzlech v hello LinuxNodes skupiny. druhý příkaz Hello připojí allvhdsjw.file.core.windows.net/rdma sdílenou složku Azure File hello do složky /rdma hello s dir a soubor too777 sadu bitů režimu. V druhém příkazu hello allvhdsje je váš název účtu úložiště a storageaccountkey je klíč účtu úložiště.

> [!NOTE]
> Hello "\`" symbol v druhém příkazu hello je symbol řídicí pro prostředí PowerShell. "\`,"znamená, že hello"," (čárku) je součástí příkaz hello.
> 
> 

### <a name="head-node-share"></a>Sdílené složky hlavního uzlu
Alternativně připojte sdílenou složku hello hlavního uzlu na Linuxových uzlů. Sdílené složky poskytuje hello nejjednodušší způsob, jak tooshare soubory, ale hello hlavního uzlu a všech uzlech Linux musí být nasazený v hello stejné virtuální síti. Tady jsou kroky hello.

1. Vytvořte složku hello hlavního uzlu a sdílet tooEveryone se oprávnění pro čtení a zápis. Například sdílet D:\OpenFOAM hello hlavního uzlu jako \\CentOS7RDMA HN\OpenFOAM. Zde je CentOS7RDMA HN hello hostname hello hlavního uzlu.
   
    ![Oprávnění ke sdílené složce][fileshareperms]
   
    ![Sdílení souborů][filesharing]
2. Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy hello:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

První příkaz Hello vytvoří složku s názvem /openfoam ve všech uzlech v hello LinuxNodes skupiny. druhý příkaz Hello připojí //CentOS7RDMA-HN/OpenFOAM hello sdílené složky do složky hello s dir a soubor too777 sadu bitů režimu. Hello uživatelské jméno a heslo v příkazu hello by měl být hello uživatelské jméno a heslo uživatele hello hlavního uzlu clusteru. (Viz [přidat nebo odebrat uživatele clusteru](https://technet.microsoft.com/library/ff919330.aspx).)

> [!NOTE]
> Hello "\`" symbol v druhém příkazu hello je symbol řídicí pro prostředí PowerShell. "\`,"znamená, že hello"," (čárku) je součástí příkaz hello.
> 
> 

### <a name="nfs-server"></a>Server systému souborů NFS
Hello systému souborů NFS služby vám umožní tooshare nebo migrace souborů mezi počítače se systémem Windows Server 2012 hello operačního systému pomocí protokolu SMB hello a počítačů se systémem Linux pomocí protokolu NFS hello. Hello systému souborů NFS server a všechny ostatní uzly mají toobe nasazené v hello stejné virtuální síti. Poskytuje lepší kompatibilitu s Linux uzly ve srovnání s sdílené složce SMB. Například podporuje odkazy souboru.

1. tooinstall a nastavení serveru NFS, postupujte podle kroků hello v [Server pro systém první sdílení souborů pro kompletní](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).
   
    Můžete například vytvořte sdílené složky NFS s názvem systému souborů nfs se hello následující vlastnosti:
   
    ![Autorizace systému souborů NFS][nfsauth]
   
    ![Oprávnění k sdíleným složkám NFS][nfsshare]
   
    ![Oprávnění NTFS pro systém souborů NFS][nfsperm]
   
    ![Vlastnosti správy systému souborů NFS][nfsmanage]
2. Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy hello:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   První příkaz Hello vytvoří složku s názvem /nfsshared ve všech uzlech v hello LinuxNodes skupiny. Hello druhý příkaz připojení zařízení hello systému souborů NFS sdílet CentOS7RDMA HN: / systému souborů nfs na hello složky. Zde CentOS7RDMA HN: systém souborů nfs je hello vzdálené cestě sdílené složky NFS.

## <a name="how-toosubmit-jobs"></a>Jak toosubmit úlohy
Existuje několik způsobů toosubmit úlohy toohello HPC Pack clusteru:

* Správce clusteru HPC nebo grafického uživatelského rozhraní Správce úloh HPC
* Webového portálu HPC
* REST API

Odeslání úlohy clusteru toohello v Azure přes HPC Pack grafického uživatelského rozhraní nástroje a webového portálu HPC hello jsou hello stejné jako u Windows výpočetních uzlů. V tématu [Správce úloh HPC Pack](https://technet.microsoft.com/library/ff919691.aspx) a [jak toosubmit úlohy z klientského počítače k místní](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

úlohy toosubmit prostřednictvím hello REST API najdete příliš[vytváření a odesílání úloh pomocí rozhraní API REST v Microsoft HPC Pack hello](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). toosubmit úlohy z klienta Linux najdete také toohello Python vzorkováním hello [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).

## <a name="clusrun-for-linux-nodes"></a>Clusrun pro Linuxové uzly
Hello HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) nástroj lze použít tooexecute příkazů na Linuxových uzlů buď prostřednictvím příkazového řádku nebo Správce clusteru HPC. Tady jsou některé základní příklady.

* Zobrazit aktuální uživatelská jména na všech uzlech v clusteru hello.
  
    ```command
    clusrun whoami
    ```
* Nainstalujte hello **gdb** ladicí nástroj s **yum** ve všech uzlech v hello linuxnodes skupiny a pak restartujte hello uzly po 10 minutách.
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* Vytvořit zobrazení všechna čísla 1 až 10 pro jednu sekundu v každém Linux uzlu v clusteru hello skript prostředí, spustit a zobrazit výstup hello uzlů okamžitě.
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> Může být nutné toouse určité řídicí znaky v **clusrun** příkazy. Jak je znázorněno v tomto příkladu, použijte ^ v příkazovém řádku tooescape hello ">" symbol.
> 
> 

## <a name="next-steps"></a>Další kroky
* Zkuste vertikálním navýšení kapacity hello clusteru tooa větší počet uzlů, nebo zkuste spustit úlohu Linux na hello clusteru. Příklad, naleznete v části [spustit NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](hpcpack-cluster-namd.md).
* Zkuste cluster s [virtuální počítače podporuje RDMA, náročné](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) úlohy MPI toorun. Příklad, naleznete v části [spustit OpenFOAM pomocí sady Microsoft HPC Pack na Linux RDMA cluster v Azure](hpcpack-cluster-openfoam.md).
* Pokud vás zajímá při práci s Linux uzly v clusteru HPC Pack služby místně, najdete v části hello [TechNet pokyny](https://technet.microsoft.com/library/mt595803.aspx).

<!--Image references-->
[scenario]:media/hpcpack-cluster/scenario.png
[portal]:media/hpcpack-cluster/portal.png
[validate]:media/hpcpack-cluster/validate.png
[resources]:media/hpcpack-cluster/resources.png
[deploy]:media/hpcpack-cluster/deploy.png
[management]:media/hpcpack-cluster/management.png
[heatmap]:media/hpcpack-cluster/heatmap.png
[fileshareperms]:media/hpcpack-cluster/fileshare1.png
[filesharing]:media/hpcpack-cluster/fileshare2.png
[nfsauth]:media/hpcpack-cluster/nfsauth.png
[nfsshare]:media/hpcpack-cluster/nfsshare.png
[nfsperm]:media/hpcpack-cluster/nfsperm.png
[nfsmanage]:media/hpcpack-cluster/nfsmanage.png
