---
title: "aaaSet vysokou dostupnost pro virtuální počítače Azure Resource Manager | Microsoft Docs"
description: "Tento kurz ukazuje, jak seskupit toocreate dostupnosti Always On s virtuálními počítači Azure v režimu Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 64e85527-d5c8-40d9-bbe2-13045d25fc68
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: 6f0a253d3502259a487e66fd62d92e41c379a6b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-groups-in-azure-virtual-machines-automatically-resource-manager"></a>V Azure Virtual Machines automaticky konfigurovat skupiny dostupnosti Always On: Správce prostředků

Tento kurz ukazuje, jak toocreate dostupnosti systému SQL Server skupiny této používá virtuální počítače Azure Resource Manager. kurz Hello používá oken Azure tooconfigure šablonu. Můžete zkontrolovat hello výchozí nastavení, zadejte požadovaná nastavení a aktualizujte hello okna hello portálu, jak projít tento kurz.

dokončení kurzu Hello vytvoří skupinu dostupnosti systému SQL Server v Azure Virtual Machines, které zahrnují hello následující prvky:

* Virtuální síť, která má více podsítí, včetně front-end a back-end podsítě
* Dva řadiče domény, které mají domény služby Active Directory
* Dva virtuální počítače, na kterých běží SQL Server a jsou nasazené toohello back-end podsíť a toohello připojené k doméně služby Active Directory
* Clusteru s podporou převzetí služeb při selhání tři uzly s model kvora Většina uzlů hello
* Skupiny dostupnosti, která má dva replik se synchronním potvrzováním databáze dostupnosti

Hello následující obrázek představuje hello kompletního řešení.

![Architektura testovací laboratoř pro skupiny dostupnosti v Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Všechny prostředky v tomto řešení patří tooa jedna skupina prostředků.

Než začnete tento kurz, potvrďte hello následující:

* Už máte účet Azure. Pokud nemáte, [si zaregistrovat zkušební účet](http://azure.microsoft.com/pricing/free-trial/).
* Už víte, jak toouse hello tooprovision grafického uživatelského rozhraní virtuálního počítače s SQL serverem z Galerie hello virtuálního počítače. Další informace najdete v tématu [zřízení virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).
* Už máte plnou Principy skupin dostupnosti. Další informace najdete v tématu [Always On skupiny dostupnosti (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Pokud vás zajímá pomocí skupin dostupnosti s SharePoint, také zobrazit [nakonfigurovat SQL Server 2012 Always On skupin dostupnosti pro službu SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).
>
>

V tomto kurzu použít hello Azure na portálu:

* Zvolte hello Always On šablonu z portálu hello.
* Zkontrolujte nastavení šablony hello a aktualizovat několik nastavení konfigurace pro vaše prostředí.
* Monitorování Azure, jelikož vytvoří hello celé prostředí.
* Připojte tooa řadič domény a pak tooa serveru, na kterém běží SQL Server.

[!INCLUDE [availability-group-template](../../../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

## <a name="provision-hello-cluster-from-hello-gallery"></a>Zřízení clusteru hello z Galerie hello
Azure poskytuje bitovou kopii Galerie pro celé řešení hello. Šablona toolocate hello:

1. Přihlaste se toohello portálu Azure pomocí účtu.
2. V hello portálu Azure, klikněte na **+ nový** tooopen hello **nový** okno.
3. Na hello **nový** okno, vyhledejte **AlwaysOn**.
   ![Najít šablonu AlwaysOn](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
4. Ve výsledcích hledání hello, vyhledejte **clusteru serveru SQL Server AlwaysOn**.
   ![Šablona AlwaysOn](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
5. Na **vybrat model nasazení**, zvolte **Resource Manager**.

### <a name="basics"></a>Základy
Klikněte na tlačítko **Základy** a nakonfigurujte hello následující nastavení:

* **Uživatelské jméno správce** je uživatelský účet, který má oprávnění správce domény a je členem skupiny hello systému SQL Server pevné role serveru sysadmin na obou instancí systému SQL Server. V tomto kurzu použít **DomainAdmin**.
* **Heslo** je hello heslo pro účet správce domény hello. Použijte složité heslo. Potvrzení hesla hello.
* **Předplatné** hello odběr, že Azure bills toorun všechny nasazené prostředky pro skupinu dostupnosti hello. Pokud má váš účet více předplatných, můžete zadat jiný odběr.
* **Skupina prostředků** je název hello toowhich hello skupiny patří všechny prostředky Azure, které jsou vytvořené pomocí této šablony. V tomto kurzu použít **SQL-HA-RG**. Další informace naleznete v tématu [Přehled Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md#resource-groups).
* **Umístění** je hello oblast Azure, kde hello kurzu vytváří prostředky hello. Vyberte oblast Azure.

Hello následující snímek obrazovky je dokončené **Základy** okno:

![Základy](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

Klikněte na **OK**.

### <a name="domain-and-network-settings"></a>Nastavení sítě a domény
Tato šablona Galerie Azure vytvoří domény a řadiče domény. Vytvoří také síť a dvě podsítě. Hello šablony nelze vytvořit servery v existující doméně nebo virtuální sítě. dalším krokem Hello nakonfiguruje nastavení sítě a domény hello.

Na hello **nastavení sítě a domény** okno, zkontrolujte hello přednastavení hodnoty pro nastavení sítě a domény hello:

* **Název kořenové domény doménové struktury** je hello název domény pro doménu služby Active Directory hello hello clusteru hostitelů. Hello kurzu použijte **contoso.com**.
* **Název virtuální sítě** je název sítě hello hello virtuální síť Azure. Hello kurzu použijte **autohaVNET**.
* **Název podsítě řadiče domény** je název hello část virtuální sítě hello tohoto řadiče domény hello hostitele. Použití **podsíť 1**. Tato podsíť používá předpona adresy **10.0.0.0/24**.
* **Název systému SQL Server podsítě** je název hello část hello virtuální sítě, že hostitelských hello serverech se systémem SQL Server a souboru hello sdílet s kopií clusteru. Použití **podsíť 2**. Tato podsíť používá předpona adresy **10.0.1.0/26**.

toolearn Další informace o virtuálních sítí v Azure, najdete v části [Přehled virtuálních sítí](../../../virtual-network/virtual-networks-overview.md).  

Hello **nastavení sítě a domény** by měl vypadají hello následující snímek obrazovky:

![Nastavení sítě a domény](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

V případě potřeby můžete tyto hodnoty změnit. V tomto kurzu použít hello přednastavení hodnoty.

Zkontrolujte nastavení hello a pak klikněte na tlačítko **OK**.

### <a name="availability-group-settings"></a>Nastavení skupiny dostupnosti
Na **nastavení skupiny dostupnosti**, zkontrolujte hello přednastavení hodnoty pro skupinu dostupnosti hello a hello naslouchací proces.

* **Název skupiny dostupnosti** je hello clusterový prostředek název pro skupinu dostupnosti hello. V tomto kurzu použít **Contoso-ag**.
* **Název naslouchacího procesu skupiny dostupnosti** je používán hello clusteru a hello interní vyrovnávání zátěže. Klienti, kteří připojují tooSQL serveru můžete použít tento název tooconnect toohello repliky příslušné databáze hello. V tomto kurzu použít **Contoso-naslouchací proces**.
* **Port naslouchacího procesu skupiny dostupnosti** určuje hello port TCP systému SQL Server hello naslouchací proces. V tomto kurzu použít výchozí port hello, **1433**.

V případě potřeby můžete tyto hodnoty změnit. V tomto kurzu použít hello přednastavení hodnoty.  

![Nastavení skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

Klikněte na **OK**.

### <a name="virtual-machine-size-storage-settings"></a>Velikost virtuálního počítače, nastavení úložiště
Na **velikost virtuálního počítače, nastavení úložiště**, zvolte velikost virtuálního počítače systému SQL Server, a zkontrolujte hello další nastavení.

* **Velikost virtuálního počítače systému SQL Server** je hello velikost pro virtuální počítače se systémem SQL Server. Zvolte velikost příslušné virtuální počítač pro úlohy. Pokud vytváříte tuto prostředí hello kurzu, použijte **DS2**. Pro úlohy v produkčním prostředí zvolte velikost virtuálního počítače, který může podporovat hello zatížení. Vyžadovat mnoho úlohy v produkčním prostředí **DS4** nebo větší. Šablona Hello sestavení dva virtuální počítače této velikosti a nainstaluje systém SQL Server na každém z nich. Další informace najdete v tématu [velikosti virtuálních počítačů](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Hello Azure nainstaluje edici Enterprise systému SQL Server. náklady na Hello závisí na edici hello a hello velikost virtuálního počítače. Podrobné informace o aktuální náklady, najdete v části [ceny virtuálních počítačů](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).
>
>

* **Velikost virtuálního počítače řadiče domény** je hello velikost virtuálního počítače pro hello řadiče domény. Pro tento kurz použití **D2**.
* **Velikost virtuálního počítače určující sdílenou složku souboru** je hello velikost virtuálního počítače pro hello určující sdílenou složku. V tomto kurzu použít **A1**.
* **Účet úložiště SQL** je název hello hello účtu úložiště, který obsahuje data systému SQL Server hello a disky operačního systému. V tomto kurzu použít **alwaysonsql01**.
* **Účet řadiče domény úložiště** je hello název účtu úložiště hello hello řadiče domény. V tomto kurzu použít **alwaysondc01**.
* **Kapacita disku dat systému SQL Server** v TB je hello velikost hello systému SQL Server datový disk v TB. Zadejte číslo od 1 do 4. V tomto kurzu použít **1**.
* **Optimalizace úložiště** nastaví nastavení konfigurace konkrétní úložiště pro virtuální počítače systému SQL Server hello, který je založený na typu úloh hello. Všechny virtuální počítače systému SQL Server v tomto scénáři používat úložiště úrovně premium s mezipamětí Azure disk hostitele pouze tooread. Kromě toho můžete optimalizovat nastavení systému SQL Server pro pracovní vytížení hello volbou jedné z těchto tří nastavení:

  * **Obecné úlohy** nastaví žádné specifické nastavení.
  * **Zpracování transakcí** nastaví příznak. 1117 a 1118 trasování.
  * **Datové sklady** nastaví příznak. 1117 trasování a 610.

V tomto kurzu použít **obecné zatížení**.

![Nastavení velikosti úložiště pro virtuální počítač](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

Zkontrolujte nastavení hello a pak klikněte na tlačítko **OK**.

#### <a name="a-note-about-storage"></a>Poznámka o úložiště
Další optimalizace závisí na velikosti hello hello systému SQL Server datových disků. Pro každý terabajt datový disk Azure přidá další úložiště premium 1 TB. Pokud server vyžaduje 2 TB nebo více, vytvoří šablona hello fondu úložiště na každém virtuálním počítači systému SQL Server. Fond úložiště je formu virtualizace úložiště, kde víc disků jsou nakonfigurované tooprovide vyšší kapacity, odolnost proti chybám a výkonu.  Šablona Hello potom vytvoří prostor úložiště u fondu úložiště hello a uvede do jednoho datového disku toohello operačního systému. Hello šablonu označí tento disk jako hello datový disk pro SQL Server. Šablona Hello vyladí hello fondu úložiště pro SQL Server pomocí hello následující nastavení:

* Stripe velikost je hello interleave nastavení pro virtuální disk hello. Transakční úlohy používají 64 KB. Datového skladu úlohy použijte 256 KB.
* Odolnost proti chybám je jednoduché (bez odolnosti).

> [!NOTE]
> Azure premium storage je místně redundantní a udržuje tři kopie dat hello v jedné oblasti, tak další odolnosti ve fondu úložiště hello se nevyžaduje.
>
>

* Počet sloupců se rovná hello počet disků ve fondu úložiště hello.

Další informace o prostor úložiště a fondy úložiště, najdete v tématu:

* [Prostory úložiště – přehled](http://technet.microsoft.com/library/hh831739.aspx)
* [Zálohování serveru a fondy úložiště](http://technet.microsoft.com/library/dn390929.aspx)

Další informace o osvědčenými postupy konfigurace systému SQL Server najdete v tématu [nejlepší postupy z hlediska výkonu pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md).

### <a name="sql-server-settings"></a>Nastavení SQL Serveru
Na **nastavení systému SQL Server**, zkontrolovat a upravit předponu názvu virtuálního počítače systému SQL Server hello, verze systému SQL Server, účet služby SQL Server a heslo a hello SQL automatické opravy plán údržby.

* **SQL Server název předpony** je použité toocreate název pro každý virtuální počítač systému SQL Server. V tomto kurzu použít **sqlserver**. Hello šablony názvy hello virtuální počítače systému SQL Server *sqlserver-0* a *sqlserver-1*.
* **Verze systému SQL Server** je hello verzi systému SQL Server. Pro tento kurz použití **SQL Server 2014**. Můžete také **SQL Server 2012** nebo **SQL Server 2016**.
* **Uživatelské jméno účtu služby SQL Server** je název účtu domény hello hello služby SQL Server. V tomto kurzu použít **byl**.
* **Heslo** je hello heslo pro účet služby SQL Server hello.  Použijte složité heslo. Potvrzení hesla hello.
* **Plán údržby SQL automatické opravy** identifikuje hello den týdnu hello Azure automaticky opravy hello SQL Server. V tomto kurzu zadejte **neděle**.
* **Automatické opravy SQL údržby spustit hodinu** je čas hello hello oblast Azure, automatické opravy zahájení.

> [!NOTE]
> Hello opravy okna pro každý virtuální počítač rozložena o jednu hodinu. Jenom jeden virtuální počítač je nainstalována oprava na čas tooprevent narušení služeb.
>
>

![Nastavení SQL Serveru](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Zkontrolujte nastavení hello a pak klikněte na tlačítko **OK**.

### <a name="summary"></a>Souhrn
Na stránce Souhrn hello Azure ověří nastavení hello. Můžete také stáhnout šablony hello. Hello Zkontrolujte souhrn. Klikněte na **OK**.

### <a name="buy"></a>Koupit
Tento poslední okno obsahuje **podmínky použití**, a **zásady ochrany osobních údajů**. Přečtěte si tyto informace. Až budete připravení pro Azure toostart toocreate hello virtuální počítače a všechny další požadované prostředky pro skupinu dostupnosti hello hello, klikněte na tlačítko **vytvořit**.

Hello portál Azure vytvoří skupinu prostředků hello a všechny prostředky hello.

## <a name="monitor-deployment"></a>Monitorování nasazení
Sledovat průběh nasazení hello z hello portálu Azure. Ikona, která představuje hello nasazení je automaticky definovaného toohello řídicí panel portálu Azure.

![Řídicí panel Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

## <a name="connect-toosql-server"></a>Připojit tooSQL serveru
Hello nové instance systému SQL Server běží na virtuálních počítačích, které mají připojené k Internetu IP adresy. Můžete vzdálené plochy (RDP) přímo tooeach virtuálního počítače SQL serverem.

tooRDP tooa SQL Server, postupujte takto:

1. Z hello řídicí panel portálu Azure ověřte, že má hello nasazení bylo úspěšné.
2. Klikněte na tlačítko **prostředky**.
3. V hello **prostředky** okně klikněte na tlačítko **sqlserver-0**, což je název počítače hello jednoho hello virtuálních počítačů se systémem SQL Server.
4. V okně hello **sqlserver-0**, klikněte na tlačítko **Connect**. Prohlížeč zobrazí dotaz, pokud mají být tooopen nebo uložit objekt hello vzdáleného připojení. Klikněte na tlačítko **otevřete**.
5. **Připojení ke vzdálené ploše** může varovat, že hello nelze identifikovat vydavatele tohoto vzdáleného připojení. Klikněte na **Připojit**.
6. Zabezpečení systému Windows zobrazí výzvu tooenter vám vaše přihlašovací údaje tooconnect toohello IP adresu hello primární řadič domény. Klikněte na tlačítko **použít jiný účet**. Pro **uživatelské jméno**, typ **contoso\DomainAdmin**. Tento účet jste nakonfigurovali, když nastavíte uživatelské jméno správce hello v šabloně hello. Použijte hello složité heslo, které jste zvolili, když jste nakonfigurovali šablonu hello.
7. **Vzdálená plocha** může varovat, že vzdálený počítač hello nemohlo být ověřeno kvůli tooproblems s certifikátem zabezpečení. Zobrazuje název certifikátu zabezpečení hello. Pokud jste postupovali podle hello kurz, je název hello **sqlserver 0.contoso.com**. Klikněte na tlačítko **Ano**.

Nyní jste připojeni pomocí protokolu RDP toohello systému SQL Server virtuálního počítače. Otevřete aplikaci SQL Server Management Studio, připojte toohello výchozí instanci systému SQL Server a ověřte, zda že je nakonfigurován této skupiny dostupnosti hello.
