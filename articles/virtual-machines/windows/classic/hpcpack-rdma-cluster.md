---
title: "aaaSet nahoru aplikací Windows RDMA clusteru toorun MPI | Microsoft Docs"
description: "Zjistěte, jak toocreate clusteru Windows HPC Pack se velikost H16r, H16mr, A8 a A9 VMs toouse hello aplikací MPI toorun sítě Azure RDMA."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 7d9f5bc8-012f-48dd-b290-db81c7592215
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 23bc8740dbd05a7c7ab3f998489a41d0df4520a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-toorun-mpi-applications"></a>Nastavení clusteru s podporou Windows RDMA s aplikací MPI toorun HPC Pack
Nastavení clusteru s podporou Windows RDMA v Azure pomocí [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) a [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun paralelních aplikací Message Passing Interface (MPI). Při nastavování podporu rdma, systémem Windows Server uzly v clusteru služby HPC Pack aplikací MPI efektivně komunikují přes nízkou latencí a vysokou propustnost sítě v Azure, která je založena na technologii do paměti vzdáleného přímý přístup do (počítače RDMA).

Pokud chcete, úlohy MPI toorun na virtuální počítače s Linuxem tuto síť Azure RDMA hello přístup, najdete v části [nastavení aplikací MPI toorun clusteru Linux RDMA](../../linux/classic/rdma-cluster.md).

## <a name="hpc-pack-cluster-deployment-options"></a>Možnosti nasazení clusteru HPC Pack
Microsoft HPC Pack je nástroj, zadaný v bez dalších nákladů toocreate clusterů HPC místně nebo v Azure toorun Windows nebo Linux HPC aplikace. HPC Pack zahrnuje běhového prostředí pro implementaci Microsoft hello hello zpráva předávání rozhraní pro Windows (MS-MPI). Při použití s podporou RDMA instancí podporovaným operačním systémem Windows Server, HPC Pack poskytuje aplikací Windows MPI toorun efektivní možnost tuto síť Azure RDMA hello přístup. 

Tento článek nabízí dva scénáře a odkazy toodetailed pokyny tooset do clusteru s podporou Windows RDMA pomocí sady Microsoft HPC Pack. 

* Scénář 1. Nasazení instance rolí pracovního procesu náročné (PaaS)
* Scénář 2. Nasaďte výpočetní uzly ve virtuálních počítačích náročné (IaaS)

Obecné předpoklady toouse náročné instancí se systémem Windows, najdete v části [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Scénář 1: Nasazení instance rolí pracovního procesu náročné (PaaS)
Z existujícího clusteru HPC Pack přidejte další výpočetní prostředky v instance rolí pracovního procesu systému Azure (Azure uzlů) spuštěna v rámci cloudové služby (PaaS). Tato funkce, také nazývaná "shluků tooAzure" ze sady HPC Pack podporuje celou řadu velikosti pro instance rolí pracovního procesu hello. Při přidávání hello uzlů Azure, zadejte jednu velikostí hello RDMA podporovat.

Následují požadavky a kroky tooburst podporující tooRDMA instancemi Azure z existující (obvykle místní) clusteru. Použijte podobné postupy tooadd pracovní role instance tooan HPC Pack hlavního uzlu, který je nasazen virtuální počítač Azure.

> [!NOTE]
> Kurz tooburst tooAzure pomocí sady HPC Pack, naleznete v části [nastavení hybridního clusteru pomocí sady HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Všimněte si hello aspekty hello následující kroky, které platí konkrétně podporující tooRDMA uzlů Azure.
> 
> 

![TooAzure shluku][burst]

### <a name="steps"></a>Kroky
1. **Nasaďte a nakonfigurujte hlavnímu uzlu HPC Pack 2012 R2**
   
    Stažení instalačního balíčku nejnovější HPC Pack hello z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Tooprepare požadavky a pokyny pro nasazení služby Azure shluků, najdete v části [Burst tooAzure instancí pracovního procesu pomocí sady Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).
2. **Konfigurovat certifikát správy v hello předplatného Azure**
   
    Konfigurace certifikátu připojení hello toosecure mezi hello hlavního uzlu a Azure. Možnosti a postupy najdete v tématu [scénáře tooConfigure hello certifikát pro správu Azure pro HPC Pack](http://technet.microsoft.com/library/gg481759.aspx). Pro testovací nasazení nainstaluje HPC Pack výchozí Microsoft HPC Azure certifikát pro správu můžete rychle nahrát tooyour předplatného Azure.
3. **Vytvořte novou cloudovou službu a účet úložiště**
   
    Použijte hello Azure portálu toocreate cloudové služby a účet úložiště pro nasazení hello v oblasti, kde jsou k dispozici instance hello RDMA podporovat.
4. **Vytvořit šablonu Azure uzlu**
   
    Hello pomocí Průvodce vytvořením uzel šablony ve Správci clusteru HPC. Pokyny najdete v tématu [vytvořit šablonu Azure uzlu](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) v "Kroky tooDeploy uzlů Azure pomocí sady Microsoft HPC Pack".
   
    Pro počáteční testy doporučujeme nakonfigurujete zásady ruční dostupnosti v šabloně hello.
5. **Přidat uzly clusteru toohello**
   
    Pomocí Průvodce přidáním uzlu hello ve Správci clusteru HPC. Další informace najdete v tématu [toohello přidat Azure uzly clusteru Windows HPC](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).
   
    Při zadávání hello velikost hello uzlů, vyberte jednu instanci velikostí podporující RDMA hello.
   
   > [!NOTE]
   > V každé shluků tooAzure nasazení s instancemi náročné hello HPC Pack automaticky nasadí minimálně dvě instance RDMA podporovat (například A8) jako proxy uzly, kromě instance rolí pracovního procesu Azure toohello zadáte. uzly proxy Hello používat jádra, které jsou přiděleny toohello předplatné a platit poplatky společně s instancí rolí pracovního procesu Azure hello.
   > 
   > 
6. **Spustit uzly hello (zřídit) a přiřaďte je online toorun úlohy**
   
    Vyberte uzel hello a použít hello **spustit** akce ve Správci clusteru HPC. Jakmile je zřizování dokončeno, vyberte uzel hello a použít hello **přepnout do režimu Online** akce ve Správci clusteru HPC. Hello uzly jsou připravené toorun úlohy.
7. **Odeslání úlohy toohello clusteru**
   
   Pomocí sady HPC Pack úlohy odeslání nástroje toorun clusteru úloh. V tématu [sady Microsoft HPC Pack: Správa úloh](http://technet.microsoft.com/library/jj899585.aspx).
8. **Zastavit uzly hello (deprovision)**
   
   Po dokončení probíhající úlohy, do offline režimu hello uzly a používat hello **Zastavit** akce ve Správci clusteru HPC.

## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Scénář 2: Nasaďte výpočetní uzly ve virtuálních počítačích náročné (IaaS)
V tomto scénáři nasazení hlavního uzlu HPC Pack hello a výpočetní uzly clusteru na virtuálních počítačích v virtuální sítě Azure. HPC Pack nabízí několik [možnosti nasazení ve virtuálních počítačích Azure](../../linux/hpcpack-cluster-options.md), včetně skriptů automatizované nasazení a šablony Azure rychlý start. Jako příklad hello následující důležité informace a kroky vás toouse hello [skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md) k automatizaci hello nasazení clusteru HPC Pack 2012 R2 v Azure.

![Cluster ve virtuálních počítačích Azure][iaas]

### <a name="steps"></a>Kroky
1. **Vytvoření hlavního uzlu clusteru a výpočetní uzel virtuální počítače tak, že spustíte skript nasazení HPC Pack IaaS hello na klientském počítači**
   
    Stáhněte si balíček skript nasazení IaaS HPC Pack hello z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922).
   
    tooprepare hello klientský počítač, vytvořit konfigurační soubor skriptu hello a spuštění hello skriptu, najdete v části [vytvoření clusteru prostředí HPC s hello skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md). 
   
    toodeploy podporující RDMA výpočetní uzly, Poznámka hello následující další důležité informace:
   
   * **Virtuální síť**: Zadejte novou virtuální síť v oblasti, ve které hello podporující RDMA velikost instance chcete toouse je k dispozici.
   * **Operační systém Windows Server**: toosupport připojení RDMA, zadejte operační systém Windows Server 2012 R2 nebo Windows Server 2012 pro hello výpočetním uzlu virtuálních počítačů.
   * **Cloudové služby**: doporučujeme, abyste nasazení vaší hlavního uzlu v rámci jednoho cloudové služby a výpočetní uzly v rámci různých cloudové služby.
   * **HEAD velikost uzlu**: pro tento scénář, zvažte velikost alespoň A4 (Extra velký) pro hlavní uzel hello.
   * **Rozšíření HpcVmDrivers**: hello skript nasazení nainstaluje hello agenta virtuálního počítače Azure a hello HpcVmDrivers rozšíření automaticky při nasazení velikosti A8 a A9 výpočetní uzly s operačním systémem Windows Server. HpcVmDrivers nainstaluje ovladače na výpočetním uzlu hello virtuální počítače, aby se mohli přihlásit toohello RDMA sítě. Na virtuálních počítačích podporující RDMA H-series musíte ručně nainstalovat rozšíření HpcVmDrivers hello. V tématu [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   * **Konfiguraci sítě s clustery**: skript nasazení hello automaticky nastaví clusteru HPC Pack hello v topologii 5 (do všech uzlů v podnikové síti hello). Tato topologie je vyžadována pro všechna nasazení clusteru HPC Pack ve virtuálních počítačích. Později neměňte hello clusteru síťové topologie.
2. **Přepněte hello výpočetní uzly online toorun úlohy**
   
    Vyberte uzel hello a použít hello **přepnout do režimu Online** akce ve Správci clusteru HPC. Hello uzly jsou připravené toorun úlohy.
3. **Odeslání úlohy toohello clusteru**
   
    Připojte toohello hlavního uzlu toosubmit úlohy, nebo nastavit místní počítač toodo to. Informace najdete v tématu [tooan odeslání úlohy HPC cluster v Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
4. **Uzly hello proveďte offline a zastavení (zrušit přidělení) je**
   
    Po dokončení probíhající úlohy, proveďte offline hello uzly ve Správci clusteru HPC. Poté použijte tooshut nástroje pro správu Azure je mimo provoz.

## <a name="run-mpi-applications-on-hello-cluster"></a>Spouštění aplikací MPI na clusteru hello
### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Příklad: Spusťte mpipingpong v clusteru HPC Pack
tooverify nasazení služby HPC Pack instancí hello podporu rdma, spusťte hello HPC Pack **mpipingpong** příkazu v clusteru hello. **mpipingpong** odesílá pakety dat mezi uzly spárované opakovaně toocalculate latence a propustnosti měření a statistiku sítě hello aplikací, které podporují RDMA. Tento příklad ukazuje typický vzor pro spuštění úlohu MPI (v tomto případě **mpipingpong**) pomocí clusteru hello **mpiexec** příkaz.

Tento příklad předpokládá, že jste přidali uzlů Azure v konfiguraci "burst tooAzure" ([scénáři 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-\(PaaS\) in this article). Pokud jste nasadili HPC Pack v clusteru virtuálních počítačů Azure, budete potřebovat toomodify hello příkaz syntaxe toospecify jiný uzel skupiny a nastavit další prostředí proměnné toodirect síťový provoz toohello RDMA sítě.

toorun mpipingpong na hello clusteru:

1. Hello hlavního uzlu nebo správnou konfiguraci klientských počítačích otevřete příkazový řádek.
2. latence tooestimate mezi páry uzlů v Azure burst nasazení čtyři uzly hello zadejte následující příkaz toosubmit mpipingpong toorun úlohy s velikost malých paketu a velký počet iterací:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```
   
    příkaz Hello vrátí ID hello hello úlohy, které je odeslána.
   
    Pokud jste nasadili cluster HPC Pack hello nasazené na virtuálních počítačích Azure, zadejte skupinu uzlu, který obsahuje výpočetní uzel virtuálních počítačích nasazených v jednom cloudové služby a upravit hello **mpiexec** příkaz takto:
   
    ```Command
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```
3. Po dokončení úlohy hello výstup tooview hello (v tomto případě se výstup hello úloha 1 úlohy hello), typ hello následující
   
    ```Command
    task view <JobID>.1
    ```
   
    kde &lt; *JobID* &gt; je hello ID hello úlohu, která byla odeslána.
   
    výstup Hello zahrnuje následující podobné toohello latence výsledky.
   
    ![Latence pong příkazu ping][pingpong1]
4. propustnost tooestimate mezi páry Azure burst uzly, typ hello následující příkaz toosubmit úlohy toorun **mpipingpong** s velikostí velkých paketů a několik iterací:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```
   
    příkaz Hello vrátí ID hello hello úlohy, které je odeslána.
   
    V clusteru HPC Pack nasazena na virtuálních počítačích Azure upravte příkaz hello jak jsme uvedli v kroku 2.
5. Po dokončení úlohy hello tooview hello výstup (v tomto případě se výstup hello úloha 1 úlohy hello), typ hello následující:
   
    ```Command
    task view <JobID>.1
    ```
   
   výstup Hello zahrnuje následující podobné toohello propustnost výsledky.
   
   ![Propustnost pong příkazu ping][pingpong2]

### <a name="mpi-application-considerations"></a>Aspekty aplikací MPI
Tady jsou důležité informace týkající se s aplikací MPI HPC Pack v Azure. Některé platí pouze toodeployments uzlů Azure (Přidat v konfiguraci "shluků tooAzure" instance role pracovního procesu.).

* Instance role pracovního procesu v rámci cloudové služby jsou pravidelně znovu poskytnuto bez upozornění v Azure (například za účelem údržby systému nebo v případě, kdy selže instance). Pokud instance je znovu poskytnuto, když je spuštěná úloha MPI, hello instance ztratí svoje data a vrátí toohello stavu, pokud bylo poprvé nasazeno, což může způsobit toofail úlohy MPI hello. Hello další uzly, které můžete používat v rámci jedné úlohy MPI a hello již hello spuštění úlohy hello vyšší pravděpodobnost, že jedna z instancí hello je znovu poskytnuto když úloha běží. To taky zvážit, pokud do jednoho uzlu v nasazení hello určíte jako souborový server.
* toorun úloh MPI ve službě Azure, nemáte toouse hello podporující RDMA instance. Můžete použít libovolnou velikost instance, která je podporována sadou HPC Pack. Hello podporující RDMA instance jsou však doporučováno pro spouštění poměrně rozsáhlé úlohy MPI, které jsou citlivé toohello latencí a šířkou pásma hello hello sítě, která se připojuje hello uzly. Pokud používáte jiné úlohy MPI citlivý na latenci a šířky pásma toorun velikosti, doporučujeme spustit malé přenosy, ve kterých běží jeden úkol na pouze několik uzlů.
* Aplikace nasazené tooAzure instance jsou subjektu toohello licenční smlouvy, které jsou přidružené aplikace hello. Zeptejte se dodavatele hello všechny obchodní aplikace pro licencování nebo jiná omezení pro spouštění v cloudu hello. Ne všichni dodavatelé nabízejí licencování formou průběžných plateb.
* Azure instancí potřebujete další instalační tooaccess místní uzlů, sdílených složek a licenčních serverů. Například tooenable hello tooaccess uzlů Azure se místní licenční server, můžete nakonfigurovat virtuální síť Azure site-to-site.
* aplikací MPI toorun s instancemi Azure zaregistrovat každou aplikaci MPI s bránou Windows Firewall v instancích hello spuštěním hello **hpcfwutil** příkaz. To umožňuje místní tootake MPI komunikaci na portu, který je přiřazen dynamicky bránou hello firewall.
  
  > [!NOTE]
  > Pro nasazení tooAzure shluků můžete taky nakonfigurovat toorun příkaz výjimky brány firewall automaticky na všechny nové uzly Azure, které jsou přidány tooyour clusteru. Po spuštění hello **hpcfwutil** příkazů a ověřte, že vaše aplikace funguje, přidejte hello příkaz tooa spouštěcí skript pro Azure uzly. Další informace najdete v tématu [pomocí spouštěcího skriptu pro Azure uzly](https://technet.microsoft.com/library/jj899632.aspx).
  > 
  > 
* HPC Pack používá hello CCP_MPI_NETMASK clusteru prostředí proměnné toospecify rozsah adres přijatelné MPI komunikace. Spouštění v prostředí HPC Pack 2012 R2, proměnné prostředí clusteru CCP_MPI_NETMASK hello se týká pouze MPI komunikace mezi výpočetní uzly clusteru připojené k doméně (místně nebo ve virtuálních počítačích Azure). Proměnná Hello ignorována přidat v konfiguraci tooAzure shluků uzly.
* Úloh MPI nelze spustit napříč instancemi Azure, které jsou nasazeny v různých cloudové služby (například v případě nasazení tooAzure shluků s jiný uzel šablony nebo virtuálního počítače Azure výpočetní uzly nasazení ve více cloudových službách). Pokud máte více nasazení Azure uzlu zahájených šablonami jiného uzlu, úlohy MPI hello musíte spustit na jen jednu sadu uzlů Azure.
* Při přidání uzlů Azure tooyour clusteru a přiřaďte je okamžitě online, hello služba Plánovač úloh HPC pokusí toostart úlohy v uzlech hello. Pokud jenom část úlohu můžete spustit v Azure, ujistěte se, aktualizovat nebo vytvořit úlohu šablony toodefine jaké úlohu typy můžete spustit v Azure. Například tooensure, úlohy, odeslané s šablonu úlohy spustit jen u uzlů Azure přidejte hello uzlu skupiny vlastnost toohello úlohy šablony a vyberte položku AzureNodes hello vyžaduje hodnotu. vlastní skupiny toocreate pro Azure uzly, pomocí rutiny Add-HpcGroup prostředí HPC PowerShell hello.

## <a name="next-steps"></a>Další kroky
* Jako alternativní toousing HPC Pack vývoj pomocí toorun služby Azure Batch hello MPI aplikace na spravovaný fond výpočetních uzlů v Azure. V tématu [použití s více instancemi úloh toorun aplikací rozhraní MPI (Message Passing) v Azure Batch](../../../batch/batch-mpi.md).
* Pokud chcete, aby toorun Linux MPI najdete v části aplikace, které přístup k síti hello Azure RDMA, [nastavení aplikací MPI toorun clusteru Linux RDMA](../../linux/classic/rdma-cluster.md).

<!--Image references-->
[burst]:media/hpcpack-rdma-cluster/burst.png
[iaas]:media/hpcpack-rdma-cluster/iaas.png
[pingpong1]:media/hpcpack-rdma-cluster/pingpong1.png
[pingpong2]:media/hpcpack-rdma-cluster/pingpong2.png
