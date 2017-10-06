---
title: aaaSQL Server FCI - Azure Virtual Machines | Microsoft Docs
description: "Tento článek vysvětluje, jak toocreate Instance clusteru převzetí služeb při selhání SQL serveru na virtuálních počítačích Azure."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: bee3b27805c5f6cc02a43b25d480c129c254cb90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>Konfigurace Instance clusteru převzetí služeb při selhání systému SQL Server na virtuálních počítačích Azure

Tento článek vysvětluje, jak toocreate selhání SQL serveru Cluster Instance (FCI) na virtuálních počítačích Azure v modelu Resource Manager. Toto řešení používá [Windows Server 2016 Datacenter edition prostory úložiště – přímé \(S2D\) ](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) jako softwarová virtuální síť SAN, synchronizuje hello úložiště (datových disků) mezi hello uzlech (virtuálních počítačích Azure) v Clusteru se systémem Windows. S2D je nového v systému Windows Server 2016.

Hello následující diagram znázorňuje hello kompletního řešení na virtuálních počítačích Azure:

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

Hello předcházející zobrazuje diagram:

- Dva virtuální počítače Azure v clusteru s podporou převzetí služeb při selhání systému Windows. Pokud virtuální počítač v clusteru s podporou převzetí služeb při selhání se také označuje jako *uzlu clusteru*, nebo *uzly*.
- Každý virtuální počítač má dva nebo více datových disků.
- S2D synchronizuje data hello na hello datový disk a uvede hello synchronizované úložiště jako fond úložiště.
- fond úložiště Hello uvede clusteru převzetí služeb při selhání toohello (CSV) pro sdílený svazek clusteru.
- role clusteru SQL serveru FCI Hello používá hello CSV hello datových jednotek.
- Zatížení Azure vyrovnávání toohold hello IP adresu pro SQL Server FCI hello.
- Nastavení Azure dostupnosti obsahuje všechny prostředky hello.

   >[!NOTE]
   >Všechny prostředky Azure jsou v diagramu hello v hello stejnou skupinu prostředků.

Podrobnosti o S2D najdete v tématu [Windows Server 2016 Datacenter edition prostory úložiště – přímé \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

S2D podporuje dva typy architektury - sblížené a hyperkonvergované. Architektura Hello v tomto dokumentu je hyperkonvergované. Úložiště hello místech hyperkonvergované infrastruktury na hello stejných serverů aplikace hello clusteru hostitele. V této architektuře úložiště hello je na každém uzlu SQL serveru FCI.

### <a name="example-azure-template"></a>Příklad šablony Azure

Hello celé řešení v Azure můžete vytvořit ze šablony. Příklad šablony je k dispozici v hello Githubu [šablon Azure rychlý Start](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad). V tomto příkladu není určená nebo testována pro všechny konkrétní úlohu. Hello šablony toocreate může běžet SQL Server FCI s S2D úložiště připojené tooyour domény. Můžete vyhodnotit hello šablony a upravit pro vaše záměry.

## <a name="before-you-begin"></a>Než začnete

Existuje několik možností, které budete potřebovat tooknow a několik věcí, které musíte v místě, než budete pokračovat.

### <a name="what-tooknow"></a>Jaké tooknow
Musí mít provozní znalosti o hello následující technologie:

- [Technologie clusteru systému Windows](http://technet.microsoft.com/library/hh831579.aspx)
-  [Instance clusteru převzetí služeb při selhání SQL serveru](http://msdn.microsoft.com/library/ms189134.aspx).

Kromě toho musí mít obecné znalosti o hello následující technologie:

- [Konvergované Hyper řešení pomocí prostory úložiště – přímé v systému Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [Skupiny prostředků Azure.](../../../azure-resource-manager/resource-group-portal.md)

### <a name="what-toohave"></a>Jaké toohave

Než budete postupovat hello pokyny v tomto článku, byste již měli mít:

- Předplatné Microsoft Azure.
- Domény systému Windows na virtuálních počítačích Azure.
- Účet s objekty toocreate oprávnění v hello virtuální počítač Azure.
- Virtuální síť Azure a podsíť s dostatečný Adresní prostor IP adres pro hello následující součásti:
   - Virtuální počítače.
   - IP adresu clusteru převzetí služeb při selhání Hello.
   - IP adresa pro každý FCI.
- DNS nakonfigurovaný v hello síť Azure, odkazující toohello řadiče domény.

Tyto požadavky splněny můžete pokračovat s vytvářením clusteru převzetí služeb při selhání. prvním krokem Hello je toocreate hello virtuálních počítačů.

## <a name="step-1-create-virtual-machines"></a>Krok 1: Vytvoření virtuálních počítačů

1. Přihlaste se toohello [portál Azure](http://portal.azure.com) s vaším předplatným.

1. [Vytvořit skupinu dostupnosti Azure](../tutorial-availability-sets.md).

   Hello dostupnosti v rámci domén selhání nastavit skupiny virtuálních počítačů a aktualizaci domény. Skupina dostupnosti Hello zajišťuje, že vaše aplikace není ovlivněn jediný bod selhání, jako je síťový přepínač hello nebo jednotka power hello rack serverů.

   Pokud jste dosud nevytvořili hello skupinu prostředků pro virtuální počítače, když vytvoříte skupinu dostupnosti Azure ho proveďte. Pokud používáte skupiny dostupnosti hello Azure portálu toocreate hello, hello následující kroky:

   - V hello portálu Azure, klikněte na  **+**  tooopen hello Azure Marketplace. Vyhledejte **sadu dostupnosti**.
   - Klikněte na tlačítko **sadu dostupnosti**.
   - Klikněte na možnost **Vytvořit**.
   - Na hello **vytvořit skupinu dostupnosti** okně sadu hello následující hodnoty:
      - **Název**: název sady dostupnosti hello.
      - **Předplatné**: Azure vaše předplatné.
      - **Skupina prostředků**: Pokud chcete, aby toouse existující skupiny, klikněte na tlačítko **použít existující** a vyberte hello skupinu z rozevíracího seznamu hello. Jinak vyberte **vytvořit nový** a zadejte název pro skupinu hello.
      - **Umístění**: nastavit hello umístění, kam budete toocreate virtuálních počítačů.
      - **Poruch domén**: použijte výchozí hello (3).
      - **Aktualizovat domén**: použijte výchozí hello (5).
   - Klikněte na tlačítko **vytvořit** skupinu dostupnosti toocreate hello.

1. Vytvoření hello virtuálních počítačů v nastavení dostupnosti hello.

   Zřídit dva virtuální počítače systému SQL Server v sadě Azure dostupnosti hello. Pokyny najdete v tématu [zřízení virtuálního počítače s SQL Server v hello portál Azure](virtual-machines-windows-portal-sql-server-provision.md).

   Umístíte virtuální počítače:

   - V hello stejné skupiny prostředků Azure, které vaše dostupnosti je v.
   - Na hello stejné síti jako řadiče domény.
   - V podsíti s dostatkem místa IP adresu pro virtuální počítače a všech instancích Fci, které může nakonec použijete na tomto clusteru.
   - V sadě Azure dostupnosti hello.   

      >[!IMPORTANT]
      >Nelze nastavit nebo změnit dostupnosti nastavit po vytvoření virtuálního počítače.

   Vyberte bitovou kopii z hello Azure Marketplace. Můžete použít na trhu bitové kopie s, který zahrnuje Windows Server a SQL Server nebo jenom hello systému Windows Server. Podrobnosti najdete v tématu [přehled systému SQL Server na virtuálních počítačích Azure](../../virtual-machines-windows-sql-server-iaas-overview.md)

   Hello oficiální imagí SQL serveru v Azure Gallery hello zahrnovat nainstalovaná instance systému SQL Server, plus hello systému SQL Server instalace softwaru a hello vyžadovaný klíč.

   Vyberte obrázek vpravo hello podle toohow, které chcete toopay pro licenci na SQL Server hello:

   - **Platba za použití licencování**: náklady za minutu hello těchto bitových kopií zahrnuje hello licencování SQL serveru:
      - **SQL Server 2016 Enterprise na Windows Server Datacenter 2016**
      - **SQL Server 2016 Standard na Windows Server Datacenter 2016**
      - **SQL Server 2016 vývojáře v systému Windows Server Datacenter 2016**

   - **Převést – vaše – vlastní – licence (BYOL)**

      - **{BYOL} SQL Server 2016 Enterprise na Windows Server Datacenter 2016**
      - **{BYOL} SQL Server 2016 Standard na Windows Server Datacenter 2016**

   >[!IMPORTANT]
   >Po vytvoření virtuálního počítače hello odeberte instanci systému SQL Server předem nainstalovaná samostatné hello. Po konfiguraci clusteru převzetí služeb při selhání hello a S2D použijete hello předem nainstalovaná systému SQL Server média toocreate hello SQL Server FCI.

   Alternativně můžete použít Image Azure Marketplace s právě hello operačního systému. Vyberte **Windows Server 2016 Datacenter** bitovou kopii a instalace hello SQL Server FCI po konfiguraci clusteru převzetí služeb při selhání hello a S2D. Tento image neobsahuje instalačního média systému SQL Server. Umístíte do umístění, kde můžete spouštět hello instalace systému SQL Server pro každý server hello instalačního média.

1. Jakmile Azure vytvoří virtuální počítače, připojte pomocí protokolu RDP tooeach virtuálního počítače.

   Když poprvé připojíte tooa virtuálního počítače pomocí protokolu RDP, počítač hello dotáže se tooallow tento počítač toobe zjistitelný v síti hello. Klikněte na **Ano**.

1. Pokud použijete jednu z imagí virtuálního počítače se systémem SQL Server hello, odeberte hello instance systému SQL Server.

   - V **programy a funkce**, klikněte pravým tlačítkem na **Microsoft SQL Server 2016 (64 bitů)** a klikněte na tlačítko **odinstalovat nebo změnit**.
   - Klikněte na tlačítko **odebrat**.
   - Vyberte výchozí instanci hello.
   - Odeberte všechny funkce v části **služby databázového stroje**. Neodebírejte **sdílené součásti**. Viz následující obrázek hello:

      ![Odebrat funkce](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - Klikněte na tlačítko **Další**a potom klikněte na **odebrat**.

1. <a name="ports"></a>Otevřete porty brány firewall hello.

   Na každém virtuálním počítači otevřete hello následující porty na hello brány Windows Firewall.

   | Účel | TCP Port | Poznámky
   | ------ | ------ | ------
   | SQL Server | 1433 | Normální port pro výchozí instance systému SQL Server. Pokud jste použili bitovou kopii z Galerie hello, se automaticky otevře tento port.
   | Test stavu | 59999 | Žádné otevřete TCP port. Později, nakonfigurujte nástroj pro vyrovnávání zatížení hello [test stavu](#probe) a hello clusteru toouse tento port.  

1. Přidejte úložiště toohello virtuálního počítače. Podrobné informace najdete v tématu [přidejte úložiště](../../../storage/common/storage-premium-storage.md).

   Virtuální počítače nutné aspoň dva datových disků.

   Připojte nezpracovaná disky - není NTFS naformátovaný disky.
      >[!NOTE]
      >Pokud připojíte naformátovaném systémem souborů NTFS disků, můžete povolit jenom S2D s žádná kontrola způsobilosti disku.  

   Připojte minimálně dvě tooeach Storage úrovně Premium (SSD disky) virtuálních počítačů. Doporučujeme alespoň P30 disky (1 TB).

   Sada hostitele ukládání do mezipaměti příliš**jen pro čtení**.

   Hello kapacitu úložiště, které použijete v produkčním prostředí závisí na velikosti pracovní zátěže. Hello hodnoty popsané v tomto článku jsou pro demonstrační a testování.

1. [Přidat existující doménu tooyour hello virtuálních počítačů](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Po hello virtuální počítače jsou vytvoření a konfiguraci, můžete nakonfigurovat hello převzetí služeb při selhání clusteru.

## <a name="step-2-configure-hello-windows-failover-cluster-with-s2d"></a>Krok 2: Konfigurace clusteru převzetí služeb při selhání se systémem Windows hello s S2D

dalším krokem Hello je tooconfigure hello převzetí služeb při selhání clusteru s S2D. V tomto kroku provedete hello následujících dílčích kroků:

1. Přidejte funkci Clustering převzetí služeb při selhání systému Windows
1. Ověření clusteru hello
1. Vytvoření clusteru převzetí služeb při selhání hello
1. Vytvoření určujícího cloudu hello
1. Přidání úložiště

### <a name="add-windows-failover-clustering-feature"></a>Přidejte funkci Clustering převzetí služeb při selhání systému Windows

1. toobegin, toohello prvním virtuálním počítači připojit pomocí protokolu RDP pomocí účtu domény, který je členem místní skupiny administrators a má oprávnění toocreate objekty ve službě Active Directory. Tento účet použijte pro hello zbytek hello konfigurace.

1. [Přidat Clustering převzetí služeb při selhání funkce tooeach virtuální počítač](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   funkci Clustering převzetí služeb při selhání tooinstall z hello uživatelského rozhraní, hello následující kroky na virtuální počítače.
   - V **správce serveru**, klikněte na tlačítko **spravovat**a potom klikněte na **přidat role a funkce**.
   - V **Průvodce přidáním rolí a funkcí**, klikněte na tlačítko **Další** dokud nezískáte příliš**vybrat funkce**.
   - V **vybrat funkce**, klikněte na tlačítko **Clustering převzetí služeb při selhání**. Zahrnout všechny požadované funkce a nástroje pro správu hello. Klikněte na tlačítko **přidat funkce**.
   - Klikněte na tlačítko **Další** a pak klikněte na **Dokončit** tooinstall hello funkce.

   tooinstall hello převzetí služeb při selhání funkce Clustering s prostředím PowerShell, spusťte následující skript z relace prostředí PowerShell správce na jednom z virtuálních počítačů hello hello.

   ```PowerShell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

Pro referenci hello další kroky, postupujte podle pokynů hello v kroku 3 [konvergované Hyper řešení pomocí prostory úložiště – přímé v systému Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).

### <a name="validate-hello-cluster"></a>Ověření clusteru hello

Tato příručka označuje tooinstructions pod [ověření clusteru](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).

Ověření clusteru hello v hello uživatelského rozhraní nebo pomocí prostředí PowerShell.

cluster hello toovalidate s hello uživatelského rozhraní, hello postupem z jednoho hello virtuálních počítačů.

1. V **správce serveru**, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Správce clusteru převzetí služeb při selhání**.
1. V **Správce clusteru převzetí služeb při selhání**, klikněte na tlačítko **akce**, pak klikněte na tlačítko **ověřit konfiguraci...** .
1. Klikněte na **Další**.
1. Na **vybrat servery nebo Cluster**, název typu hello i virtuálních počítačů.
1. Na **testování možnosti**, zvolte **spustit jenom testy, vyberte**. Klikněte na **Další**.
1. Na **testování výběr**, zahrnout všechny testy s výjimkou **úložiště**. Viz následující obrázek hello:

   ![Ověřit testy](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. Klikněte na **Další**.
1. Na **potvrzení**, klikněte na tlačítko **Další**.

Hello **Průvodce ověřením konfigurace** spustí hello testy pro ověření.

toovalidate hello clusteru pomocí prostředí PowerShell, spusťte následující skript z relace prostředí PowerShell správce na jednom z virtuálních počítačů hello hello.

   ```PowerShell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

Po ověření clusteru hello vytvořte cluster převzetí služeb při selhání hello.

### <a name="create-hello-failover-cluster"></a>Vytvoření clusteru převzetí služeb při selhání hello

Tato příručka označuje příliš[vytvořit hello převzetí služeb při selhání clusteru](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).

toocreate hello převzetí služeb při selhání clusteru, je třeba:
- názvy Hello hello virtuálních počítačů, které se stanou hello uzly clusteru.
- Název pro cluster převzetí služeb při selhání hello
- IP adresa pro hello převzetí služeb při selhání clusteru. Můžete použít IP adresu, která se nepoužije na hello stejnou virtuální síť Azure a podsíť jako hello uzly clusteru.

Hello následující prostředí PowerShell vytváří cluster s podporou převzetí služeb při selhání. Aktualizace hello skriptu s názvy hello hello uzlů (hello názvy virtuálních počítačů) a dostupnou IP adresu z hello virtuální sítě Azure:

```PowerShell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a>Vytvoření určujícího cloudu

Cloud s kopií clusteru je nový typ určující disk kvora clusteru, která je uložená v objektu Blob úložiště Azure. Touto akcí odeberete hello potřebám samostatný virtuální počítač hostování určující sdílenou složku.

1. [Vytvoření určujícího cloudu pro cluster převzetí služeb při selhání hello](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Vytvořte kontejner objektů blob.

1. Uložte hello přístupových klíčů a adresy URL kontejneru hello.

1. Nakonfigurujte hello určující disk kvora clusteru převzetí služeb při selhání clusteru. Viz [Konfigurace hello určující disk kvora v uživatelském rozhraní hello]. (http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) v hello uživatelského rozhraní.

### <a name="add-storage"></a>Přidání úložiště

Hello disky pro S2D potřebovat toobe prázdný a bez oddílů nebo jiná data. postupujte podle tooclean disky [hello kroky v této příručce](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).

1. [Prostory úložiště povolení přímé \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   Hello následující prostředí PowerShell umožňuje prostory úložiště – přímé.  

   ```PowerShell
   Enable-ClusterS2D
   ```

   V **Správce clusteru převzetí služeb při selhání**, se zobrazí hello fondu úložiště.

1. [Vytvoření svazku](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   Jedním z hello funkce S2D je, že automaticky vytvoří fond úložiště při jeho povolení. Nyní je připraven toocreate svazku. Hello prostředí PowerShell `New-Volume` automatizuje proces vytvoření svazku hello, včetně formátování, přidání toohello clusteru a vytvoření sdíleného svazku clusteru (CSV). Hello následující ukázka vytvoří 800 gigabajt (GB) sdílených svazků clusteru.

   ```PowerShell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   Po dokončení tohoto příkazu je připojen 800 GB svazek jako prostředku clusteru. svazek Hello je v `C:\ClusterStorage\Volume1\`.

   Hello následující diagram znázorňuje sdílený svazek clusteru s S2D:

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>Krok 3: Testování převzetí služeb při selhání clusteru převzetí služeb při selhání

Ve Správci clusteru převzetí služeb při selhání ověřte, že můžete přesunout toohello prostředků úložiště hello jiného uzlu clusteru. Pokud se můžete připojit cluster převzetí služeb při selhání toohello s **Správce clusteru převzetí služeb při selhání** a přesunout úložiště hello z jednoho uzlu toohello jiné, jsou připravené tooconfigure hello FCI.

## <a name="step-4-create-sql-server-fci"></a>Krok 4: Vytvoření systému SQL Server FCI

Po nakonfigurování clusteru převzetí služeb při selhání hello a všechny součásti clusteru, včetně úložiště, můžete vytvořit hello SQL Server FCI.

1. Toohello prvním virtuálním počítači připojte pomocí protokolu RDP.

1. V **Správce clusteru převzetí služeb při selhání**, zkontrolujte, zda jsou všechny základní prostředky clusteru na prvním virtuálním počítači hello. V případě potřeby přesuňte všechny prostředky toothis virtuálního počítače.

1. Vyhledejte hello instalačního média. Pokud hello virtuální počítač používá jednu z imagí hello Azure Marketplace, se nachází na médium hello `C:\SQLServer_<version number>_Full`. Klikněte na tlačítko **instalační program**.

1. V hello **centrum instalace SQL serveru**, klikněte na tlačítko **instalace**.

1. Klikněte na tlačítko **nová instalace SQL serveru převzetí služeb při selhání clusteru**. Postupujte podle pokynů hello hello Průvodce tooinstall hello SQL Server FCI.

   Hello FCI datové adresáře potřebovat toobe na úložiště v clusteru. S S2D není sdíleného disku, ale svazek přípojného bodu tooa na každém serveru. S2D synchronizuje hello svazku mezi obou uzlů. svazek Hello je zobrazen toohello clusteru jako sdílený svazek clusteru. Použijte hello CSV přípojného bodu pro hello dat adresáře.

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. Po dokončení Průvodce hello, instalační program nainstaluje SQL Server FCI hello prvního uzlu.

1. Poté, co instalační program úspěšně nainstaluje hello FCI hello prvního uzlu, připojte pomocí protokolu RDP toohello druhého uzlu.

1. Otevřete hello **centrum instalace SQL serveru**. Klikněte na tlačítko **instalace**.

1. Klikněte na tlačítko **clusteru převzetí služeb při selhání systému SQL Server se přidat uzly tooa**. Postupujte podle pokynů hello v hello Průvodce tooinstall SQL server a přidejte tento server toohello FCI.

   >[!NOTE]
   >Pokud jste použili bitovou kopii Galerie Azure Marketplace s SQL serverem, byly součástí bitové kopie hello nástroje SQL Server. Pokud jste nepoužili tuto bitovou kopii, nainstalujte nástroje SQL Server hello samostatně. V tématu [stáhnout SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-azure-load-balancer"></a>Krok 5: Vytvoření pro vyrovnávání zatížení Azure

Na virtuálních počítačích Azure použijte clustery toohold nástroje pro vyrovnávání zatížení IP adresu, která potřebuje toobe na jednom uzlu clusteru současně. V tomto řešení obsahuje nástroj pro vyrovnávání zatížení hello hello IP adresu pro SQL Server FCI hello.

[Vytvoření a konfigurace pro vyrovnávání zatížení Azure](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-hello-load-balancer-in-hello-azure-portal"></a>Vytvořit nástroj pro vyrovnávání zatížení hello v hello portálu Azure

Vyrovnávání zatížení toocreate hello:

1. V hello portálu Azure přejděte toohello skupinu prostředků s virtuálními počítači hello.

1. Klikněte na tlačítko **+ přidat**. Hledání hello Marketplace pro **nástroj pro vyrovnávání zatížení**. Klikněte na tlačítko **nástroj pro vyrovnávání zatížení**.

1. Klikněte na možnost **Vytvořit**.

1. Konfigurace vyrovnávání zatížení hello:

   - **Název**: název, který identifikuje hello nástroj pro vyrovnávání zatížení.
   - **Typ**: nástroj pro vyrovnávání zatížení hello může být veřejné nebo soukromé. Nástroj pro vyrovnávání zatížení privátní je přístupná prostřednictvím hello stejnou virtuální síť. Většina Azure aplikace můžete použít nástroj pro vyrovnávání zatížení privátní. Pokud aplikace potřebuje přístup tooSQL serveru přímo přes hello Internetu, použijte nástroj pro vyrovnávání zatížení veřejné.
   - **Virtuální síť**: hello stejné sítě jako hello virtuální počítače.
   - **Podsíť**: hello stejné podsíti jako hello virtuální počítače.
   - **Privátní IP adresa**: hello stejnou IP adresu, který jste přiřadili toohello SQL Server FCI clusteru síťovému prostředku.
   - **předplatné**: Azure vaše předplatné.
   - **Skupina prostředků**: použití hello stejné skupině prostředků jako virtuální počítače.
   - **Umístění**: použití hello stejné umístění jako virtuální počítače Azure.
   Viz následující obrázek hello:

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-hello-load-balancer-backend-pool"></a>Nakonfigurujte fond back-end pro vyrovnávání zatížení hello

1. Vrátí toohello skupiny prostředků Azure s virtuálními počítači hello a vyhledejte hello nový nástroj pro vyrovnávání zatížení. Můžete mít toorefresh hello zobrazení na hello skupinu prostředků. Klikněte na nástroj pro vyrovnávání zatížení hello.

1. V okně nástroje pro vyrovnávání zatížení hello, klikněte na tlačítko **back-endové fondy**.

1. Klikněte na tlačítko **+ přidat** tooadd fond back-end.

1. Zadejte název pro fond back-end hello.

1. Klikněte na tlačítko **přidat virtuální počítač**.

1. Na hello **vyberte virtuální počítače** okně klikněte na tlačítko **zvolit skupinu dostupnosti**.

1. Vyberte sadu dostupnosti hello, že jste umístili virtuální počítače systému SQL Server hello v.

1. Na hello **vyberte virtuální počítače** okně klikněte na tlačítko **zvolte hello virtuálních počítačů**.

   Portálu Azure by měl vypadat podobně jako hello následujícím obrázku:

   ![CreateLoadBalancerBackEnd](./media/virtual-machines-windows-portal-sql-create-failover-cluster/33-load-balancer-back-end.png)

1. Klikněte na tlačítko **vyberte** na hello **vyberte virtuální počítače** okno.

1. Klikněte na tlačítko **OK** dvakrát.

### <a name="configure-a-load-balancer-health-probe"></a>Konfigurace stavu sondu nástroje pro vyrovnávání zatížení.

1. V okně nástroje pro vyrovnávání zatížení hello, klikněte na tlačítko **testy stavu**.

1. Klikněte na tlačítko **+ přidat**.

1. Na hello **test stavu přidat** okně <a name="probe"> </a>nastavit parametry testu stavu hello:

   - **Název**: název pro test stavu hello.
   - **Protokol**: TCP.
   - **Port**: nastavte tooan dostupný port TCP. Tento port vyžaduje k portu brány firewall otevřít. Použití hello [stejný port](#ports) nastavení pro test stavu hello v hello brány firewall.
   - **Interval**: 5 sekund.
   - **Prahová hodnota špatného stavu**: 2 po sobě jdoucích selhání.

1. Klikněte na tlačítko OK.

### <a name="set-load-balancing-rules"></a>Nastavit pravidla Vyrovnávání zatížení

1. V okně nástroje pro vyrovnávání zatížení hello, klikněte na tlačítko **pravidla Vyrovnávání zatížení**.

1. Klikněte na tlačítko **+ přidat**.

1. Nastavte parametry pravidla vyrovnávání zátěže hello:

   - **Název**: název pravidla Vyrovnávání zatížení hello.
   - **Adresa IP front-endu**: používat hello IP adresu pro hello SQL Server FCI clusteru síťovému prostředku.
   - **Port**: nastavení pro hello port TCP systému SQL Server FCI. Hello výchozí instance port je 1433.
   - **Back-endový port**: Tato hodnota používá hello stejný port jako hello **Port** hodnotu, pokud povolíte **plovoucí IP adresa (přímá odpověď ze serveru)**.
   - **Fond back-end**: použití hello back-end fondu název, který jste dříve nakonfigurovali.
   - **Test stavu**: Test stavu hello použití, který jste nakonfigurovali dříve.
   - **Trvalost relace**: žádné.
   - **Časový limit (v minutách) nečinnosti**: 4.
   - **Plovoucí IP adresa (přímá odpověď ze serveru)**: povoleno

1. Klikněte na **OK**.

## <a name="step-6-configure-cluster-for-probe"></a>Krok 6: Konfigurace clusteru pro test paměti

Nastavit parametr port testu clusteru hello v prostředí PowerShell.

tooset hello parametr port testu clusteru, aktualizujte proměnné ve hello následující skript z prostředí.

  ```PowerShell
   $ClusterNetworkName = "<Cluster Network Name>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "IP Address Resource Name" # hello IP Address cluster resource name.
   $ILBIP = "<10.0.0.x>" # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <59999>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```


## <a name="step-7-test-fci-failover"></a>Krok 7: Testování převzetí služeb při selhání FCI

Testovací převzetí služeb při selhání hello fungování clusteru toovalidate FCI. Hello následující kroky:

1. Tooone uzlů clusteru SQL serveru FCI hello připojte pomocí protokolu RDP.

1. Otevřete **Správce clusteru převzetí služeb při selhání**. Klikněte na tlačítko **role**. Všimněte si, který uzel vlastní role SQL serveru FCI hello.

1. Klikněte pravým tlačítkem na roli SQL serveru FCI hello.

1. Klikněte na tlačítko **přesunout** a klikněte na tlačítko **nejvhodnějšího uzlu**.

**Správce clusteru převzetí služeb při selhání** ukazuje hello role a její prostředky přejít do režimu offline. Hello prostředky pak přesunout a uvést do režimu online na hello jiného uzlu.

### <a name="test-connectivity"></a>Test připojení

připojení k tootest, přihlášení tooanother virtuálního počítače v hello stejné virtuální síti. Otevřete **SQL Server Management Studio** a připojte se název SQL serveru FCI toohello.

>[!NOTE]
>Pokud třeba, můžete [stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Omezení
Na virtuálních počítačích Azure Microsoft koordinátoru DTC (Distributed Transaction) není podporována v instancích Fci protože hello RPC port není podporována pro vyrovnávání zatížení hello.

## <a name="see-also"></a>Viz také

[Instalační program S2D pomocí vzdálené plochy (Azure)](http://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

[Konvergované Hyper řešení s prostory úložiště direct](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).

[Prostory úložiště – přímé přehled](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[Podpora systému SQL Server pro S2D](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
