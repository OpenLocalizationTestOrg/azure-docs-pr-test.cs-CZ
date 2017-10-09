---
title: "Rychlý úvod: Ruční instalace jedné instance SAP HANA ve virtuálních počítačích Azure | Microsoft Docs"
description: "Průvodce rychlým zahájením pro ruční instalaci jedné instance SAP HANA ve virtuálních počítačích Azure"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 57b58b8e07379eed5641f5f89d55b38f52c69e44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Rychlý úvod: Ruční instalace jedné instance SAP HANA na virtuálních počítačích Azure
## <a name="introduction"></a>Úvod
Tento průvodce vám pomůže nastavit jedné instance SAP HANA na virtuálních počítačích Azure (VM) při instalaci SAP NetWeaver 7.5 a SAP HANA 1.0 SP12 ručně. Hello cílem tohoto průvodce je k nasazení SAP HANA v Azure. Nenahrazuje SAP dokumentaci. 

>[!Note]
>Tato příručka popisuje nasazení SAP HANA do virtuálních počítačů Azure. Informace o nasazení SAP HANA do instance velké HANA najdete v tématu [pomocí SAP na virtuálních počítačích Azure (VM)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Požadavky
Tato příručka předpokládá, že jste obeznámeni s takovou infrastrukturu jako službu (IaaS) základy jako:
 * Jak toodeploy virtuální počítače nebo virtuální sítě prostřednictvím hello portál Azure nebo PowerShell.
 * Hello Azure napříč platformami rozhraní příkazového řádku (CLI), včetně hello možnost toouse JavaScript Object Notation (JSON) šablony.

Tento průvodce také předpokládá, že jste obeznámeni s:
* SAP HANA a SAP NetWeaver a jak tooinstall je na místě.
* Instalaci a provozování SAP HANA a SAP instance aplikace na platformě Azure.
* Hello následující koncepty a procedury:
   * Plánování nasazení SAP na Azure, včetně Azure Virtual Network plánování a využívání Azure Storage. V tématu [SAP NetWeaver ve virtuálních počítačích Azure (VM) – Příručka plánování a implementace](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Nasazení zásad a způsoby toodeploy virtuálních počítačů v Azure. V tématu [nasazení virtuálních počítačů Azure pro SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Vysoká dostupnost pro SAP NetWeaver ASC (ABAP SAP centrální služby), SCS (SAP centrální služby) a YBRAT (vyhodnotit vyrovnání příjmu) v Azure. V tématu [vysoká dostupnost pro SAP NetWeaver na virtuálních počítačích Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * Informace o tom, tooimprove efektivita při využívání a více SID instalace ASC nebo SCS v Azure. V tématu [vytvořit konfiguraci více SID SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Principy spuštěných SAP NetWeaver založené na základě Linux virtuálních počítačů v Azure. V tématu [systémem SAP NetWeaver na virtuálních počítačích Microsoft Azure SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Tato příručka obsahuje nastavení specifická pro Linux ve virtuálních počítačích Azure a podrobnosti o tom, jak připojit tooproperly tooLinux úložiště Azure disky virtuálních počítačů.

V tomto okamžiku virtuálních počítačích Azure certifikovány SAP pro SAP HANA škálování pouze konfigurace. Konfigurace Škálováním na více systémů s úlohami SAP HANA ještě nejsou podporovány. SAP HANA vysoké dostupnosti v případech škálování konfigurace, najdete v části [vysokou dostupnost SAP HANA na virtuálních počítačích Azure (VM)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

Pokud hledáte tooget SAP HANA instance nebo S nebo 4HANA nebo BW/4HANA systému nasazené v čase velmi rychlé, měli byste zvážit použití hello [knihovny zařízení cloudu SAP](http://cal.sap.com). Můžete najít dokumentaci o nasazení, například S nebo 4HANA systému prostřednictvím SAP CAL na platformě Azure v [Tato příručka](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Stačí toohave je předplatné Azure a SAP uživatele, který lze registrovat pomocí knihovny zařízení SAP cloudu.

## <a name="additional-resources"></a>Další zdroje
### <a name="sap-hana-backup"></a>Zálohování SAP HANA
Informace o zálohování databáze SAP HANA na virtuálních počítačích Azure najdete v tématu:
* [Příručce zálohování pro SAP HANA ve virtuálních počítačích Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [SAP HANA Azure Backup na úrovni souborů](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
* [SAP HANA zálohování podle úložiště snímků](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

### <a name="sap-cloud-appliance-library"></a>Knihovna zařízení cloudu SAP
Informace o používání knihovny zařízení cloudu SAP toodeploy S nebo 4HANA nebo BW/4HANA najdete v tématu [nasazení SAP S nebo 4HANA nebo BW/4HANA v Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>SAP HANA podporované operační systémy
Informace o SAP HANA podporované operační systémy, v tématu [SAP podporu Poznámka #2235581 - SAP HANA: podporované operační systémy](https://launchpad.support.sap.com/#/notes/2235581/E). Virtuální počítače Azure podporují jenom podmnožinu těchto operačních systémů. Hello následující operační systémy jsou podporované toodeploy SAP HANA v Azure: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

Další dokumentaci SAP o SAP HANA a různých operačních systémů Linux najdete v tématu:

* [Podpora Poznámka SAP #171356 - SAP softwaru v systému Linux: Obecné informace](https://launchpad.support.sap.com/#/notes/1984787)
* [Poznámka: podpora #1944799 - SAP HANA pokyny pro instalaci operačního systému SLES SAP](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
* [Podpora Poznámka SAP #2205917 - SAP HANA DB doporučené nastavení operačního systému pro SLES 12 pro aplikace SAP](https://launchpad.support.sap.com/#/notes/2205917/E)
* [Podpory Poznámka SAP #1984787 - SUSE Linux Enterprise Server 12: Poznámky k instalaci](https://launchpad.support.sap.com/#/notes/1984787)
* [Podpora Poznámka SAP #1391070 - Linux UUID řešení](https://launchpad.support.sap.com/#/notes/1391070)
* [SAP podporu Poznámka #2009879 - SAP HANA pokyny pro operační systém Red Hat Enterprise Linux (RHEL)](https://launchpad.support.sap.com/#/notes/2009879)
* [2292690 - SAP HANA DB: OS doporučená nastavení pro RHEL 7](https://launchpad.support.sap.com/#/notes/2292690/E)

### <a name="sap-monitoring-in-azure"></a>SAP monitorování v Azure
Informace o SAP monitorování v Azure najdete v tématu:

* [Poznámka SAP 2191498](https://launchpad.support.sap.com/#/notes/2191498/E). Tato poznámka se věnuje SAP "rozšířené monitorování" s virtuální počítače s Linuxem v Azure. 
* [Poznámka SAP 1102124](https://launchpad.support.sap.com/#/notes/1102124/E). Tato poznámka popisuje informace o SAPOSCOL v systému Linux. 
* [Poznámka SAP 2178632](https://launchpad.support.sap.com/#/notes/2178632/E). Tato poznámka se věnuje monitorování klíčové metriky pro SAP v Microsoft Azure.

### <a name="azure-vm-types"></a>Azure typy virtuálních počítačů
Azure typy virtuálních počítačů a scénáře podporované SAP zatížení použít s SAP HANA jsou dokumentovány v článku [SAP certifikované platformy IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

Azure typy virtuálních počítačů, které certifikovány SAP pro SAP NetWeaver nebo hello S nebo 4HANA aplikační vrstvu jsou dokumentovány v článku [1928533 Poznámka SAP - SAP aplikace v Azure: typy podporovaných produktů a virtuální počítač Azure](https://launchpad.support.sap.com/#/notes/1928533/E).

>[!Note]
>Integrace se službou SAP. Linux Azure je podporována pouze na Azure Resource Manageru a hello modelu nasazení classic. 

## <a name="manual-installation-of-sap-hana"></a>Ruční instalace SAP HANA
Tato příručka popisuje, jak toomanually instalovat SAP HANA na virtuálních počítačích Azure dvěma různými způsoby:

* Pomocí SAP softwaru zřizování Manager (SWPM) jako součást distribuované instalace NetWeaver v kroku "instalace instance databáze" hello
* Databáze nástroje Správce životního cyklu, HDBLCM a následnou instalaci NetWeaver pomocí hello SAP HANA

Můžete také použít SWPM tooinstall všechny součásti (SAP HANA, hello SAP aplikační server a instanci ASC hello) v jeden virtuální počítač, jak je popsáno v tomto [SAP HANA blog oznámení](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Tato možnost není popsané v tomto průvodci rychlé spuštění, ale hello, které jsou problémy, které musí vzít v úvahu hello stejné.

Před zahájením instalace, doporučujeme, abyste si přečetli hello "Příprava Azure virtuálních počítačů pro ruční instalaci sady SAP HANA" dál v této příručce. Díky tomu můžete předejít několik základní chyb, které můžou nastat, když používáte pouze výchozí konfigurace virtuálního počítače Azure.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>Klíčové kroky pro instalaci SAP HANA při použití SAP SWPM
Tato část obsahuje seznam hello klíčové kroky pro ruční instalaci SAP HANA jedné instance při instalaci tooperform distribuované 7.5 NetWeaver SAP SAP SWPM. Hello jednotlivé kroky jsou podrobně popsány na snímcích obrazovky v této příručce.

1. Vytvoření virtuální sítě Azure, která zahrnuje dvě testovací virtuální počítače.
2. Nasaďte hello dva virtuální počítače Azure s operačními systémy (v našem příkladu SUSE Linux Enterprise Server (SLES) a SLES pro SAP aplikace 12 SP1), podle modelu toohello Azure Resource Manager.
3. Připojení dvě Azure standard nebo premium storage disky (například disky 75 GB nebo 500 GB) toohello aplikačního serveru virtuálních počítačů.
4. Připojte server HANA DB toohello disky úložiště premium virtuálních počítačů. Podrobnosti najdete v tématu "Instalace disku" části hello později v tomto průvodci.
5. V závislosti na požadavcích velikost nebo propustnost připojení více disků a pak vytvořte prokládané svazky s využitím správu logické svazku nebo nástroj pro správu více zařízení (MDADM) na úrovni hello OS uvnitř hello virtuálních počítačů.
6. Vytvořte systémy souborů XFS hello připojené disky nebo logické svazky.
7. Připojte hello nových XFS souboru systémů na úrovni hello operačního systému. Použijte jeden systému souborů pro veškerý software SAP hello. Použití hello jiném systému souborů pro adresář /sapmnt hello a zálohování, například. Na serveru SAP HANA DB hello připojte na discích úložiště premium hello jako /hana a /usr/sap hello XFS souborové systémy. Tento proces je nezbytné tooprevent hello kořenového souboru systém, který není na virtuálních počítačích Azure Linux, velké zaplnění.
8. Zadejte hello místních IP adres hello testovací virtuální počítače v souboru hello/etc/hosts.
9. Zadejte hello **nofail** parametr v hello/etc/fstab souboru.
10. Nastavte parametry jádra Linux podle verze toohello operační systém Linux, který používáte. Další informace najdete v tématu hello odpovídající SAP poznámky, které popisují HANA a hello "Jádra parametry" části této příručky.
11. Zvětšete odkládací soubor.
12. Volitelně můžete nainstalujte grafické plochy na hello testovací virtuální počítače. Jinak použijte vzdálenou instalaci SAPinst.
13. Stahování softwaru SAP hello z hello SAP služby Marketplace.
14. Nainstalujte instanci SAP ASC hello na serveru aplikace hello virtuálních počítačů.
15. Sdílené složky hello /sapmnt directory mezi hello testovací virtuální počítače pomocí systému souborů NFS. Aplikační server Hello virtuální počítač je server systému souborů NFS hello.
16. Nainstalujte hello instance databáze, včetně HANA, pomocí SWPM na serveru databáze hello virtuálních počítačů.
17. Instalace serveru primární aplikace hello (Pa adresy) na serveru aplikace hello virtuálních počítačů.
18. Spusťte konzolu pro správu SAP (SAP MC). Propojit s grafickým uživatelským rozhraním SAP nebo HANA Studio, například.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>Klíčové kroky pro instalaci SAP HANA při použití HDBLCM
Tato část obsahuje seznam hello klíčové kroky pro ruční instalaci SAP HANA jedné instance při instalaci tooperform distribuované 7.5 NetWeaver SAP SAP HDBLCM. Hello jednotlivé kroky jsou podrobně popsány na snímcích obrazovky v této příručce.

1. Vytvoření virtuální sítě Azure, která zahrnuje dvě testovací virtuální počítače.
2. Dva virtuální počítače Azure s operačními systémy (v našem příkladu SLES a SLES pro SAP aplikace 12 SP1) nasazení podle modelu toohello Azure Resource Manager.
3. Připojte dva Azure standard nebo premium storage disky (například disky 75 GB nebo 500 GB) toohello aplikace serveru virtuálního počítače.
4. Připojte server HANA DB toohello disky úložiště premium virtuálních počítačů. Podrobnosti najdete v tématu "Instalace disku" části hello později v tomto průvodci.
5. V závislosti na požadavcích velikost nebo propustnost připojení více disků a vytvořte prokládané svazky pomocí správu logické svazku nebo nástroj pro správu více zařízení (MDADM) na úrovni hello OS uvnitř hello virtuálních počítačů.
6. Vytvořte systémy souborů XFS hello připojené disky nebo logické svazky.
7. Připojte hello nových XFS souboru systémů na úrovni hello operačního systému. Použijte jeden systému souborů pro veškerý software SAP hello a například použití jiných jednu pro adresář /sapmnt hello a zálohování, hello. Na serveru SAP HANA DB hello připojte na discích úložiště premium hello jako /hana a /usr/sap hello XFS souborové systémy. Tento proces je nutné toohelp zabránit hello kořenového souboru systém, který není na virtuálních počítačích Azure Linux velká, naplňování.
8. Zadejte hello místních IP adres hello testovací virtuální počítače v souboru hello/etc/hosts.
9. Zadejte hello **nofail** parametr v hello/etc/fstab souboru.
10. Nastavit parametry jádra podle verze toohello operační systém Linux, který používáte. Další informace najdete v tématu hello odpovídající SAP poznámky, které popisují HANA a hello "Jádra parametry" části této příručky.
11. Zvětšete odkládací soubor.
12. Volitelně můžete nainstalujte grafické plochy na hello testovací virtuální počítače. Jinak použijte vzdálenou instalaci SAPinst.
13. Stahování softwaru SAP hello z hello SAP služby Marketplace.
14. Vytvořte skupinu, sapsys, se skupinou ID 1001 na serveru HANA DB hello virtuálních počítačů.
15. Nainstalujte SAP HANA na serveru databáze hello virtuálních počítačů pomocí HANA databáze Lifecycle Manager (HDBLCM).
16. Nainstalujte instanci SAP ASC hello na serveru aplikace hello virtuálních počítačů.
17. Sdílené složky hello /sapmnt directory mezi hello testovací virtuální počítače pomocí systému souborů NFS. Aplikační server Hello virtuální počítač je server systému souborů NFS hello.
18. Nainstalujte hello instance databáze, včetně HANA, pomocí SWPM na serveru HANA DB hello virtuálních počítačů.
19. Instalace serveru primární aplikace hello (Pa adresy) na serveru aplikace hello virtuálních počítačů.
20. Spusťte SAP MC. Připojení přes grafické uživatelské rozhraní SAP nebo HANA Studio.

## <a name="preparing-azure-vms-for-a-manual-installation-of-sap-hana"></a>Příprava virtuálních počítačích Azure pro ruční instalaci sady SAP HANA
Tato část se zabývá hello následující témata:

* Aktualizace operačního systému
* Instalační program disku
* Parametry jádra
* systémy souborů
* soubor Hello/etc/hosts
* Hello/etc/fstab souboru

### <a name="os-updates"></a>Aktualizace operačního systému
Zkontrolujte operační systém Linux aktualizace a opravy před instalací další software. Instalací opravy, může být schopný tooavoid podpory toohello volání.

Ujistěte se, že používáte:
* SUSE Linux Enterprise Server pro aplikace SAP.
* Red Hat Enterprise Linux pro aplikace SAP nebo Red Hat Enterprise Linux pro SAP HANA. 

Pokud jste to ještě neudělali, zaregistrujte se na vaše předplatné Linux od dodavatele Linux hello hello nasazení operačního systému. Všimněte si, že má SUSE bitové kopie operačního systému pro SAP aplikace, které již obsahují služby a které jsou registrovány automaticky.

Tady je příklad Kontrola dostupných oprav pro SUSE Linux pomocí hello **zypper** příkaz:

 `sudo zypper list-patches`

V závislosti na druhu hello problém jsou opravy klasifikovány podle kategorie a závažnost. Kategorie běžně používané hodnoty jsou: **zabezpečení**, **doporučená**, **volitelné**, **funkce**, **dokumentu**, nebo **yast**.
Běžně používané hodnoty závažnosti jsou: **kritické**, **důležité**, **střední**, **nízkou**, nebo **neurčené**.

Hello **zypper** příkaz vypadá jen pro hello aktualizace, které musí nainstalované balíčky. Můžete například použít tento příkaz:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Můžete přidat parametr hello `--dry-run` tootest hello aktualizace bez ve skutečnosti aktualizace systému hello.


### <a name="disk-setup"></a>Instalační program disku
systém souborů kořenové Hello ve virtuální počítač s Linuxem v Azure má omezenou velikost. Proto je nutné tooattach další disk místo tooan virtuálního počítače Azure pro spuštění SAP. Pro aplikační server SAP virtuálních počítačích Azure může být dostatečná hello použití úložiště Azure standardní disky. Pro SAP HANA databázového systému virtuální počítače Azure, je však povinné použití hello Azure Premium Storage disků pro produkční a mimo produkční implementace.

Podle hello [požadavky na úložiště SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), navržený hello následující konfigurace Azure Premium Storage: 

| VIRTUÁLNÍ POČÍTAČ SKU | Paměť RAM |  / hana/protokolu a/hana/data <br /> rozdělená s LVM nebo MDADM | / hana/sdílené | / root svazku | / usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

V konfiguraci navrhované disku hello hello HANA datový svazek a svazek protokolu jsou umístěny na stejnou sadu Azure prémiové disky úložiště, které jsou rozložené s LVM nebo MDADM hello. Není nutné toodefine všechny redundance RAID úrovně protože Azure Premium Storage udržuje tři bitové kopie disků hello redundanci. toomake potřeba nakonfigurovat dostatečně velké úložiště najdete hello [požadavky na úložiště SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) a [aktualizace příručce k instalaci serveru SAP HANA a](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Také zvážit hello jiný virtuální pevný disk (VHD) propustnost objemy hello různých Azure premium storage disky podle postupu uvedeného v [vysoce výkonné úložiště Premium a spravované disky pro virtuální počítače](https://docs.microsoft.com/azure/storage/storage-premium-storage). 

Můžete přidat že další úložiště premium disky virtuálních počítačů databázového systému HANA toohello pro ukládání záloh databáze nebo transakčního protokolu.

Další informace o tooconfigure dva hlavní nástroje používané hello prokládání najdete v části hello následující články:

* [Konfigurace softwaru diskového pole RAID v systému Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Konfigurace LVM na virtuální počítač s Linuxem v Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Další informace o připojení disků tooAzure virtuální počítače se systémem Linux jako hostovaný operační systém, najdete v části [přidat tooa disku virtuálního počítače s Linuxem](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure Premium Storage vám umožní toodefine disku ukládání do mezipaměti režimy. Pro sadu hello rozdělená, která uchovává /hana/data a /hana/log by mělo být zakázáno ukládání do mezipaměti na disku. Pro hello jiných svazků (disky), hello ukládání do mezipaměti v režimu, musí být nastavená příliš**jen pro čtení**.

Další informace najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../../storage/common/storage-premium-storage.md).

toofind ukázkové JSON šablony pro vytvoření virtuálních počítačů, přejděte příliš[šablon Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).
Šablona virtuálního počítače. jednoduchý sles Hello je základní šablonu. Úložiště v tématu, s diskem další 100 GB dat zahrnuje. Tuto šablonu můžete použít jako základ. Můžete přizpůsobit hello šablony tooyour specifickou konfiguraci.

>[!Note]
>Je důležité tooattach hello úložiště Azure disku pomocí UUID, jak je uvedeno v [systémem SAP NetWeaver na virtuálních počítačích Microsoft Azure SUSE Linux](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

V testovacím prostředí hello dva disky úložiště Azure úrovně standard byly připojené toohello SAP aplikačního serveru virtuální počítač, jak ukazuje následující snímek obrazovky hello. Jeden disk ukládají veškerý software SAP hello (včetně NetWeaver 7.5, SAP grafickým uživatelským rozhraním a SAP HANA) pro instalaci. druhý disk Hello zajistit, že by dostatek volného místa k dispozici pro další požadavky (například zálohování a testovací data) a pro hello /sapmnt adresáři (tj, SAP profily) toobe sdílí všechny virtuální počítače, které patří toohello stejné SAP šířku.

![Aplikace serveru SAP HANA disky okno zobrazování dvě datové disky a jejich velikosti](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Parametry jádra
SAP HANA vyžaduje konkrétní nastavení jádra systému Linux, které nejsou součástí hello standardní Azure Galerie obrázků a musí být nastavena ručně. V závislosti na tom, jestli používáte SUSE nebo Red Hat hello parametry můžou být různé. Poznámky k SAP Hello uvedena i výše poskytnout informace o těchto parametrů. Na snímcích obrazovky hello vidět byl použit SUSE Linux 12 SP1. 

SLES pro SAP aplikace 12 GA a SLES pro SP1 12 SAP aplikací mít nový nástroj, **přizpůsobená adm**, že hello nahradí původní **sapconf** nástroj. Je k dispozici pro speciální profil SAP HANA **přizpůsobená adm**. systém hello tootune pro SAP HANA, zadejte následující hello jako kořenové uživatele:

   `tuned-adm profile sap-hana`

Další informace o **přizpůsobená adm**, najdete v části hello [SUSE dokumentaci o přizpůsobená adm](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

V hello následující snímek obrazovky, uvidíte jak **přizpůsobená adm** změněné hello `transparent_hugepage` a `numa_balancing` hodnoty, podle toohello požadované nastavení SAP HANA.

![Nástroj přizpůsobená adm Hello změní hodnoty podle toorequired nastavení SAP HANA](./media/hana-get-started/image005.jpg)

použít toomake hello SAP HANA jádra nastavení trvalé, **grub2** na SLES 12. Další informace o **grub2**, přejděte toohello [Struktura konfiguračního souboru](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) části hello SUSE dokumentaci.

Hello následující snímek obrazovky ukazuje, jak nastavení jádra hello měla změnit v konfiguračním souboru hello a pak zkompilovat pomocí **grub2 mkconfig**:

![Nastavení jádra změnit v konfiguračním souboru hello a zkompilovat pomocí grub2 mkconfig](./media/hana-get-started/image006.jpg)

Další možností je nastavení hello toochange pomocí YaST a hello **spouštěcí zavaděč** > **jádra parametry** nastavení:

![Karta nastavení Hello jádra parametry v YaST spouštěcí zavaděč](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>systémy souborů
Hello následující snímek obrazovky ukazuje dva systémy souborů, které byly vytvořeny na serveru aplikace SAP virtuálních počítačů hello nad hello dva disky připojené úložiště Azure úrovně standard. Oba systémy souborů se typu XFS a jsou připojené příliš/sapdata a /sapsoftware.

Ho není povinné toostructure vaše systémy souborů tímto způsobem. Máte další možnosti pro vytvoření struktury hello místa na disku. Hello nejvíce důležitý aspekt spočívá v systému souborů kořenové tooprevent hello z nedostatku volného místa.

![Dva systémy souborů vytvořena na serveru aplikace SAP hello virtuálních počítačů](./media/hana-get-started/image008.jpg)

Ohledně hello SAP HANA DB virtuálních počítačů, během instalace databáze, pokud používáte SAPinst (SWPM) a hello **typické** možnost instalace, všechny součásti nainstalovány v rámci /hana a /usr/sap. Hello výchozím umístěním pro zálohu protokolu SAP HANA hello je pod /usr/sap. Znovu protože je důležité tooprevent hello kořenové systém souborů vyčerpání volného místa, ujistěte se, zda je dostatek volného místa pod /hana a /usr/sap před instalací SAP HANA pomocí SWPM.

Popis hello standardní systém souborů rozložení SAP HANA najdete v tématu hello [aktualizace příručce k instalaci serveru SAP HANA a](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![Další souborové systémy vytvořena na serveru aplikace SAP hello virtuálních počítačů](./media/hana-get-started/image009.jpg)

Když instalujete SAP NetWeaver na standardní SLES/SLES pro bitovou kopii Galerie Azure 12 aplikace SAP, zobrazí se zpráva s informacemi o tom, že neexistuje žádná odkládacího souboru, jak ukazuje následující snímek obrazovky hello. toodismiss to zprávy, můžete ručně přidat odkládací soubor pomocí **dd**, **mkswap**, a **swapon**. toolearn jak, vyhledejte "Přidání odkládací soubor ručně" v hello [pomocí hello dělicí metody YaST](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) části hello SUSE dokumentaci.

Další možností je tooconfigure velikosti odkládacího souboru pomocí hello agenta virtuálního počítače s Linuxem. Další informace najdete v tématu hello [Azure Linux Agent uživatelská příručka](../../linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

![Místní zpráva radí, že je nedostatečné velikosti odkládacího souboru](./media/hana-get-started/image010.jpg)


### <a name="hello-etchosts-file"></a>soubor Hello/etc/hosts
Než začnete tooinstall SAP, ujistěte se, že jste zahrnuli hello názvy hostitelů a IP adresy hello SAP virtuální počítače v souboru/etc/hosts hello. Nasaďte všechny virtuální počítače hello SAP v rámci jednu virtuální síť Azure a pak použijte hello interní IP adresy, jak je vidět tady:

![Názvy hostitelů a IP adresy virtuálních počítačů SAP hello uvedené v souboru/etc/hosts hello](./media/hana-get-started/image011.jpg)

### <a name="hello-etcfstab-file"></a>Hello/etc/fstab souboru

Je užitečné tooadd hello **nofail** parametr toohello fstab souboru. Tímto způsobem, pokud se operace nezdaří s disky hello hello virtuálního počítače není přečnívat v procesu spouštění hello. Ale mějte na paměti, že nemusí být k dispozici další místo na disku a procesů může zaplnit hello kořenovém souboru systému. Pokud chybí /hana, SAP HANA se nespustí.

![Přidání hello nofail parametr toohello fstab souboru](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>Grafické GNOME plocha u SLES 12/SLES 12 aplikace SAP
Tato část se zabývá hello následující témata:

* Instalace hello GNOME desktop a xrdp na SLES 12/SLES pro SAP aplikace 12
* Spuštěné MC SAP založené na jazyce Java pomocí prohlížeče Firefox na SLES 12/SLES 12 aplikace SAP

Můžete taky alternativy například Xterminal nebo VNC (nejsou popsané v tomto průvodci).

### <a name="installing-hello-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>Instalace hello GNOME desktop a xrdp na SLES 12/SLES pro SAP aplikace 12
Pokud máte pozadí systému Windows, můžete snadno použít grafické plochy přímo v rámci hello virtuální počítače s Linuxem SAP toorun Firefox, SAPinst, SAP grafickým uživatelským rozhraním, SAP MC nebo HANA Studio a připojení toohello virtuálních počítačů přes hello protokolu RDP (Remote Desktop) z počítače Windows. Závisí na vaší zásady společnosti o přidání grafické uživatelské rozhraní tooproduction a mimo produkční systémy založenými na systému Linux, je vhodné tooinstall GNOME na vašem serveru. plocha GNOME tooinstall hello na Azure SLES 12/SLES pro virtuální počítač 12 SAP aplikace:

1. Nainstalujte hello GNOME plochy tak, že zadáte následující příkaz (například v PuTTY okně) hello:

   `zypper in -t pattern gnome-basic`

2. Nainstalujte xrdp tooallow toohello připojení virtuálních počítačů pomocí protokolu RDP:

   `zypper in xrdp`

3. Upravte /etc/sysconfig/windowmanager a nastavte hello výchozí okno Správce tooGNOME:

   `DEFAULT_WM="gnome"`

4. Spustit **chkconfig** toomake jistotu, že xrdp spustí automaticky po restartování systému:

   `chkconfig -level 3 xrdp on`

5. Pokud máte potíže s hello připojení RDP, zkuste toorestart (z PuTTY okna, např.):

   `/etc/xrdp/xrdp.sh restart`

6. Pokud restartování xrdp uvedeno v předchozí hello, že krok nefunguje, můžete zkontrolujte soubor .pid:

   `check /var/run` 

   Vyhledejte `xrdp.pid`. Pokud vám hledání, odeberte jej a opakujte toorestart.

### <a name="starting-sap-mc"></a>Počáteční MC SAP
Po instalaci hello GNOME plochy spouštění hello grafické MC SAP založené na jazyce Java z Firefox při spuštění služby Azure SLES 12/SLES pro virtuální počítač 12 aplikace SAP se může zobrazit k chybě z důvodu chybějící modul plug-in Java prohlížeče hello.

Hello URL toostart hello je SAP MC `<server>:5<instance_number>13`.

Další informace najdete v tématu [počáteční hello webové konzoly pro správu SAP](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

Hello následující snímek obrazovky ukazuje hello chybovou zprávu, která se zobrazí v případě, že chybí hello modul plug-in Java prohlížeče:

![Chybová zpráva označující chybějící Java – modul plug-in prohlížeče](./media/hana-get-started/image013.jpg)

Jedním ze způsobů toosolve hello problému je chybějící modul plug-in pomocí YaST, jak ukazuje následující snímek obrazovky hello hello tooinstall:

![Pomocí YaST tooinstall chybí modulu plug-in](./media/hana-get-started/image014.jpg)

Když znovu zadáte hello adresa URL konzoly správy SAP, zobrazí se zpráva s dotazem můžete tooactivate hello modulu plug-in:

![Dialogové okno modul plug-in aktivace](./media/hana-get-started/image015.jpg)

Může také zobrazit chybovou zprávu o soubor chybějící javafx.properties. Toto je požadavek související toohello Oracle Java 1.8 pro SAP 7.4 grafickým uživatelským rozhraním. (Viz [Poznámka SAP 2059429](https://launchpad.support.sap.com/#/notes/2059424).) Hello verzi Javy IBM ani balíček openjdk hello doručeny s SLES/SLES pro SAP aplikace 12 obsahuje soubor potřebné javafx.properties hello. řešení Hello je toodownload a instalaci Java SE 8 z databáze Oracle.

Informace o podobném problému s openjdk na openSUSE najdete v tématu hello diskusní téma [SAPGui 7.4 Java pro openSUSE 42.1 Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>Ruční instalace SAP HANA: SWPM
Hello řadu snímky obrazovky v této části ukazuje hello klíčové kroky pro instalaci SAP NetWeaver 7.5 a SAP HANA SP12 při použití SWPM (SAPinst). Jako součást instalace NetWeaver 7.5 můžete SWPM také nainstalovat hello HANA databáze jako jedna instance.

V prostředí testovací ukázka jsme nainstalovali jenom jeden rozšířená obchodní aplikace programování (ABAP) aplikačního serveru. Jak ukazuje následující snímek obrazovky hello, použili jsme hello **systému DFS** možnost tooinstall hello ASC a instance serveru primární aplikace v jeden virtuální počítač Azure a SAP HANA jako systém hello databázi na jiný virtuální počítač Azure.

![ASC a instance serveru primární aplikace nainstalovat pomocí možnosti systému DFS hello](./media/hana-get-started/image012.jpg)

Po hello ASC instance je nainstalován na serveru aplikace hello virtuálních počítačů a nastavte příliš "zelený" v hello SAP konzoly pro správu (zobrazené v následující snímek obrazovky hello), adresář /sapmnt hello (včetně hello SAP profil adresáře) je potřeba sdílet s hello SAP HANA Databázového serveru virtuálního počítače. krok instalace Hello DB potřebuje přístup k informacím toothis. toouse systém souborů NFS, kterého lze nakonfigurovat pomocí YaST je Hello nejlepší způsob, jak tooprovide přístup.

![SAP konzoly pro správu zobrazující hello ASC instance nainstalován na serveru aplikace hello virtuálních počítačů a nastavte příliš "zelený"](./media/hana-get-started/image016.jpg)

Na serveru aplikace hello virtuálních počítačů, hello/sapmnt directory by měla být sdílena prostřednictvím systému souborů NFS pomocí hello **rw** a **no_root_squash** možnosti. Hello výchozí hodnoty jsou **ro** a **root_squash**, což může způsobit tooproblems při instalaci hello instanci databáze.

![Sdílení hello /sapmnt adresáři prostřednictvím systému souborů NFS pomocí možnosti rw a no_root_squash hello](./media/hana-get-started/image017b.jpg)

Jak ukazuje následující snímek obrazovky hello, hello /sapmnt sdílenou složku ze serveru aplikace hello virtuálních počítačů musíte nakonfigurovat na serveru SAP HANA DB hello virtuálních počítačů pomocí **klient NFS** (a YaST).

![sdílená složka /sapmnt Hello nakonfigurovaný pomocí systému souborů NFS klienta](./media/hana-get-started/image018b.jpg)

instalace tooperform distribuované 7.5 NetWeaver (**Instance databáze**), jako hello uvedené v následující snímek obrazovky, přihlásit toohello SAP HANA Databázového serveru virtuálního počítače a spustit SWPM.

![Nainstalovat instanci databáze přihlášení toohello SAP HANA Databázového serveru virtuálního počítače a spustit SWPM](./media/hana-get-started/image019.jpg)

Po výběru **typické** instalace a hello cesta toohello instalačního média, zadejte DB SID, název hostitele hello, čísla instance hello a hello DB heslo správce systému.

![stránku Hello SAP HANA databáze systému správce přihlášení](./media/hana-get-started/image035b.jpg)

Zadejte heslo hello hello DBACOCKPIT schématu:

![pole Hello zadávání hesel pro schéma DBACOCKPIT hello](./media/hana-get-started/image036b.jpg)

Zadejte dotaz hello SAPABAP1 schématu hesla:

![Zadejte otázka pro heslo schématu SAPABAP1 hello](./media/hana-get-started/image037b.jpg)

Po dokončení každé úlohy se zobrazí zelená značka zaškrtnutí další fáze tooeach hello DB procesu instalace. uvítací zprávu "spuštění služby... Zobrazí se dokončil Instance databáze".

![Úloha dokončena okno se zprávou potvrzení](./media/hana-get-started/image023.jpg)

Po úspěšné instalaci by měla hello konzoly pro správu SAP také zobrazit instance DB hello jako "green" a zobrazit úplný seznam hello SAP HANA procesů (hdbindexserver, hdbcompileserver a tak dále).

![Okno konzoly pro správu SAP s seznam procesů, SAP HANA](./media/hana-get-started/image024.jpg)

Hello následující snímek obrazovky znázorňuje hello části hello strukturu souborů v adresáři /hana/shared hello, který SWPM vytvoří během instalace HANA hello. Protože neexistuje žádná možnost toospecify jinou cestu, je důležité toomount další místo na disku v adresáři /hana hello před hello SAP HANA instalace pomocí SWPM. Systém souborů kořenové hello zabrání nedostatku volného místa.

![Hello /hana/shared adresářovou strukturu soubor vytvořen během instalace HANA](./media/hana-get-started/image025.jpg)

Tento snímek obrazovky ukazuje strukturu souborů hello hello /usr/sap adresáře:

![Struktura souborů directory /usr/sap Hello](./media/hana-get-started/image026.jpg)

posledním krokem Hello hello distribuované ABAP instalace je instance serveru primární aplikace hello tooinstall:

![Instance serveru ABAP instalace zobrazující primární aplikace jako poslední krok hello](./media/hana-get-started/image027b.jpg)

Po instalaci instance serveru primární aplikace hello a SAP grafickým uživatelským rozhraním, použít hello **řídící panel DBA** tooconfirm transakce, který hello SAP HANA instalace dokončila správně:

![Řídící panel DBA okno potvrzení úspěšné instalace](./media/hana-get-started/image028b.jpg)

V posledním kroku můžete má toofirst instalaci HANA Studio hello SAP aplikačního serveru virtuálního počítače a připojte instanci SAP HANA toohello, který běží na serveru databáze hello virtuálních počítačů:

![Instalace SAP HANA Studio v hello SAP aplikace server virtuálního počítače](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>Ruční instalace SAP HANA: HDBLCM
V přidání tooinstalling SAP HANA jako součást distribuované instalaci pomocí SWPM můžete nainstalovat samostatnou HANA hello nejdřív pomocí HDBLCM. Můžete nainstalovat SAP NetWeaver 7.5, např. snímky obrazovky Hello v této části ukazují, jak tento proces funguje.

Další informace o hello HANA HDBLCM nástroj najdete v tématu:

* [Výběrem možnosti hello opravte SAP HANA HDBLCM pro vaše úlohy](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)
* [Nástrojích správy životního cyklu SAP HANA](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)
* [Průvodce aktualizací a instalaci serveru SAP HANA](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)

nastavení ID pro hello tooavoid problémy s výchozí skupiny `\<HANA SID\>adm user` (vytvořený pomocí nástroje HDBLCM hello), zadejte novou skupinu s názvem `sapsys` pomocí ID skupiny `1001` před instalací SAP HANA prostřednictvím HDBLCM:

![Nové skupiny "sapsys" definované za použití skupiny ID 1001](./media/hana-get-started/image030.jpg)

Při prvním spuštění HDBLCM hello se zobrazí nabídka jednoduché start. Vyberte položku 1, **nainstalovat nový systém**, jak ukazuje následující snímek obrazovky hello:

![Možnost "Nainstalovat nový systém" v okně počáteční HDBLCM hello](./media/hana-get-started/image031.jpg)

Hello následující snímek obrazovky zobrazí všechny hello klíče možnosti, které jste vybrali dříve.

> [!IMPORTANT]
> Adresáře, které jsou pojmenované HANA protokolu a datové svazky, jakož i hello instalační cesty (/ hana/sdílené v této ukázce) a /usr/sap, nesmí být součástí hello kořenovém souboru systému. Tyto adresáře patří toohello Azure datových disků, které byly připojené toohello virtuálních počítačů (popsaný v části instalace hello"Disk"). Tento přístup pomáhá zabránit systému souborů kořenové hello z nedostatku místa. V hello následující snímek obrazovky, uvidíte, že správce systému HANA hello má ID uživatele `1005` a je součástí hello `sapsys` skupiny (ID `1001`), byl definován před instalací hello.

![Seznam všech klíčové komponenty SAP HANA vybrali dříve](./media/hana-get-started/image032.jpg)

Můžete zkontrolovat hello `\<HANA SID\>adm user` (`azdadm` v hello následující snímek obrazovky) podrobností v hello/etc/hesel adresáře:

![HANA \<HANA SID\>adm uživatele podrobnosti uvedené v hello/etc/hesel adresáře](./media/hana-get-started/image033.jpg)

Po instalaci SAP HANA pomocí HDBLCM, uvidíte hello strukturu souborů v sadě SAP HANA Studio, jak ukazuje následující snímek obrazovky hello. Hello SAPABAP1 schématu, která obsahuje všechny tabulky SAP NetWeaver hello, ještě není k dispozici.

![Struktura souborů SAP HANA v SAP HANA Studio](./media/hana-get-started/image034.jpg)

Po instalaci SAP HANA, můžete nainstalovat SAP NetWeaver nad ho. Jako hello uvedené v následující snímek obrazovky hello instalace byla provedena jako distribuovaná instalace pomocí SWPM (jak je popsáno v předchozí části hello). Když nainstalujete instanci databáze hello pomocí SWPM, zadáte hello stejná data pomocí HDBLCM (například název hostitele, HANA SID a číslo instance). SWPM pak používá existující instalaci HANA hello a přidá další schémat.

![Distribuované instalaci provést pomocí SWPM](./media/hana-get-started/image035b.jpg)

Hello následující snímek obrazovky ukazuje kroku instalace SWPM hello kam zadat data o hello DBACOCKPIT schématu:

![krok instalace SWPM Hello, kde je zadán DBACOCKPIT schémat dat](./media/hana-get-started/image036b.jpg)

Zadejte data o hello SAPABAP1 schématu:

![Zadávání dat o hello SAPABAP1 schématu](./media/hana-get-started/image037b.jpg)

Po dokončení hello SWPM instalaci instance databáze, můžete zjistit hello SAPABAP1 schéma v SAP HANA Studio:

![schéma Hello SAPABAP1 v SAP HANA Studio](./media/hana-get-started/image038b.jpg)

Nakonec po dokončení hello SAP aplikační server a instalací s grafickým uživatelským rozhraním SAP si můžete ověřit hello HANA DB instance pomocí hello **řídící panel DBA** transakce:

![instance databáze HANA Hello ověření u hello řídící panel DBA transakce](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>Stahování softwaru SAP
Software můžete stáhnout z hello SAP služby Marketplace, jak je znázorněno v následujícím snímky obrazovky hello.

Stáhněte si NetWeaver 7.5 pro Linux/HANA:

 ![Instalace služby SAP a Upgrade okna pro stahování NetWeaver 7.5](./media/hana-get-started/image001.jpg)

Stáhněte si HANA SP12 platformy edice:

 ![Instalace služby SAP a Upgrade okna pro stahování HANA SP12 platformy Edition](./media/hana-get-started/image002.jpg)

