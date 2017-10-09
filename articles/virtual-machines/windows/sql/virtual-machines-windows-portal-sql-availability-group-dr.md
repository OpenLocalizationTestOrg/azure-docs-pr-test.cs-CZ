---
title: "aaaSQL zotavení po havárii skupin dostupnosti serveru – virtuální počítače Azure - | Microsoft Docs"
description: "Tento článek vysvětluje, jak seskupit tooconfigure dostupnosti systému SQL Server na virtuálních počítačích Azure s replikou v jiné oblasti."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 388c464e-a16e-4c9d-a0d5-bb7cf5974689
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: df6238dc61c5a56879c75c9bf7314c618f43c0ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a>Konfigurace skupiny dostupnosti Always On na virtuálních počítačích, které jsou v různých oblastech Azure

Tento článek vysvětluje, jak skupiny dostupnosti SQL serveru Always On tooconfigure repliky na virtuálních počítačích Azure ve vzdáleném umístění Azure. Použijte toto obnovení po havárii toosupport konfigurace.

Tento článek se týká tooAzure virtuální počítače v režimu Resource Manager.

Hello následující obrázek ukazuje běžné nasazení skupiny dostupnosti na virtuálních počítačích Azure:

   ![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

V tomto nasazení jsou všechny virtuální počítače v jedné oblasti Azure. repliky skupin dostupnosti Hello může mít synchronním potvrzováním s automatické převzetí služeb při selhání na SQL-1 a SQL 2. toobuild této architektury, najdete v části [skupiny dostupnosti šablony nebo kurzu](virtual-machines-windows-portal-sql-availability-group-overview.md).

Tato architektura je snadno napadnutelný toodowntime hello oblast Azure přestane být nedostupné. tooovercome toto ohrožení zabezpečení, přidejte repliku v jiné oblasti Azure. Hello následující diagram znázorňuje vzhledu nové architektury hello:

   ![Zotavení po Havárii skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

Hello předchozí diagram ukazuje nový virtuální počítač názvem SQL-3. SQL 3 je v jiné oblasti Azure. SQL 3 se přidá toohello clusteru převzetí služeb při selhání systému Windows Server. SQL 3 můžete hostovat repliku skupiny dostupnosti. Všimněte si, že tento hello oblast Azure SQL 3 má nové nástroje pro vyrovnávání zatížení Azure.

>[!NOTE]
> Nastavení Azure dostupnosti je potřeba při více než jeden virtuální počítač je v hello stejné oblasti. Pokud jenom jeden virtuální počítač je v oblasti hello, pak skupinu dostupnosti hello se nevyžaduje. Virtuální počítač můžete umístit pouze ve skupině dostupnosti nastavena v okamžiku vytvoření. Pokud už hello virtuální počítač je v nastavení dostupnosti, můžete přidat virtuální počítač pro repliku další později.

V této architektuře hello repliky v oblasti vzdálené hello je obvykle nakonfigurované asynchronního potvrzování dostupnosti režim a režim ručního převzetí služeb při selhání.

Pokud replik skupin dostupnosti jsou na virtuálních počítačích Azure v různých oblastech Azure, vyžaduje každá oblast:

* Brána virtuální sítě
* Připojení brány virtuální sítě

Hello následující diagram znázorňuje způsob hello sítě komunikace mezi datovými centry.

   ![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
>Tato architektura způsobuje poplatky za odchozí data pro data replikovat mezi oblastmi Azure. V tématu [šířky pásma ceny](http://azure.microsoft.com/pricing/details/bandwidth/).  

## <a name="create-remote-replica"></a>Vytvoření vzdálené repliky

toocreate repliku v centru vzdálených dat, hello následující kroky:

1. [Vytvořit virtuální síť v oblasti nové hello](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

1. [Konfigurace připojení typu VNet-to-VNet pomocí portálu Azure hello](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).

   >[!NOTE]
   >V některých případech může mít toouse prostředí PowerShell toocreate hello VNet-to-VNet připojení. Například pokud použijete různé účty Azure nelze nakonfigurovat připojení hello hello portálu. V takovém případě najdete [konfigurace VNet-to-VNet připojení pomocí portálu Azure hello](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

1. [Vytvoření řadiče domény v nové oblasti hello](../../../active-directory/active-directory-new-forest-virtual-machine.md).

   Tento řadič domény poskytuje ověřování, pokud není dostupný řadič domény hello v primární lokalitě hello.

1. [Vytvoření virtuálního počítače s SQL serverem v nové oblasti hello](virtual-machines-windows-portal-sql-server-provision.md).

1. [Vytvoření pro vyrovnávání zatížení Azure v síti hello na novou oblast hello](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

   Tento nástroj pro vyrovnávání zatížení musí:

   - V hello stejnou síť a podsíť, protože hello nového virtuálního počítače.
   - Máte statickou IP adresu pro naslouchací proces skupiny dostupnosti hello.
   - Zahrnout back-endový fond, který se skládá z jen hello virtuálních počítačů v hello stejné oblasti jako hello nástroj pro vyrovnávání zatížení.
   - Použijte TCP port testu toohello konkrétní IP adresu.
   - Pravidlo konkrétní toohello systému SQL Server v hello Vyrovnávání zatížení mají stejné oblasti.  

1. [Přidat Clustering převzetí služeb při selhání funkce toohello nový Server SQL](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

1. [Připojit hello nové doméně systému SQL Server toohello](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

1. [Nastavit účet domény hello nového systému SQL Server service účet toouse](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).

1. [Přidání nového serveru SQL Server toohello hello clusteru převzetí služeb při selhání systému Windows Server](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).

1. Vytvořte prostředek adresy IP na hello clusteru.

   Ve Správci clusteru převzetí služeb při selhání můžete vytvořit prostředek hello IP adresy. Klikněte pravým tlačítkem na hello role skupiny dostupnosti, klikněte na tlačítko **přidat prostředek**, **více prostředků**a klikněte na tlačítko **IP adresu**.

   ![Vytvoření IP adresy](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   Tato IP adresa nakonfigurujte následujícím způsobem:

   - Použijte síť hello z hello vzdáleného datového centra.
   - Přiřadíte hello IP adresu z hello nové pro vyrovnávání zatížení Azure. 

1. Na hello nový Server SQL v SQL Server Configuration Manager [povolte skupiny dostupnosti Always On](http://msdn.microsoft.com/library/ff878259.aspx).

1. [Porty brány firewall otevřít na hello nový Server SQL](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

   je třeba tooopen čísla portů Hello závisí na vašem prostředí. Otevřené porty pro hello zrcadlení koncový bod a Azure načíst sondu nástroje pro vyrovnávání stavu.

1. [Přidejte skupinu dostupnosti toohello repliky na hello nový Server SQL](http://msdn.microsoft.com/library/hh213239.aspx).

   Pro repliku ve vzdálené oblast Azure nastavte ji pro asynchronní replikaci s ruční převzetí služeb při selhání.  

1. Přidáte prostředek hello IP adresy jako závislost pro hello naslouchací proces klientský přístup k bodu (síťový název) clusteru.

   Hello následující snímek obrazovky ukazuje prostředek clusteru správně nakonfigurovaná adresa IP:

   ![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   >skupinu prostředků clusteru Hello zahrnuje obě IP adresy. Obě IP adresy jsou závislosti pro hello naslouchací proces klientský přístupový bod. Použití hello **nebo** operátor v konfiguraci závislostí hello clusteru.

1. [Nastavení parametrů clusteru hello v prostředí PowerShell](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).

Spusťte skript prostředí PowerShell hello s hello název sítě s clustery, IP adresa a port testu, který jste nakonfigurovali na Vyrovnávání zatížení hello v nové oblasti hello.

   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster name for hello network in hello new region (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "<IPResourceName>" # hello cluster name for hello new IP Address resource.
   $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB) in hello new region. This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <nnnnn> # hello probe port you set on hello ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a>Sada připojení pro více podsítí

replika Hello v hello vzdáleného datového centra je součástí skupiny dostupnosti hello, ale je v jiné podsíti. Pokud tato replika bude hello primární replikou, může dojít, překročení časového limitu připojení aplikace. Toto chování je hello stejné jako místní skupiny dostupnosti v nasazení více podsítí. tooallow připojení z klientské aplikace, aktualizace připojení klienta hello nebo nakonfigurovat překlad ukládání do mezipaměti na prostředku názvu sítě clusteru hello.

Pokud možno aktualizovat tooset řetězce připojení klienta hello `MultiSubnetFailover=Yes`. V tématu [propojení s MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).

Pokud nelze upravit hello připojovací řetězce, můžete nakonfigurovat název řešení ukládání do mezipaměti. V tématu [časových limitů připojení ve skupině dostupnosti více podsítí](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).

## <a name="fail-over-tooremote-region"></a>Selhání tooremote oblast

tootest naslouchací proces připojení toohello oblasti vzdáleného, můžete převzít hello repliky toohello vzdálené oblasti. Replika hello je asynchronní, i když je převzetí služeb při selhání ztráty dat snadno napadnutelný toopotential. toofail přes bez ztráty dat, změňte toosynchronous režim dostupnosti hello a nastavte tooautomatic režimu hello převzetí služeb při selhání. Použijte hello následující kroky:

1. V **Průzkumník objektů**, připojte toohello instanci systému SQL Server, který je hostitelem primární repliky hello.
1. V části **skupiny dostupnosti AlwaysOn**, **skupiny dostupnosti**, klikněte pravým tlačítkem na vaší skupiny dostupnosti a klikněte na **vlastnosti**.
1. Na hello **Obecné** v části **replik dostupnosti**, sada hello sekundární replika v hello zotavení po Havárii lokality toouse **synchronní potvrdit** režim dostupnosti a **Automatické** režim převzetí služeb při selhání.
1. Pokud máte sekundární repliku ve stejné lokalitě jako váš primární repliky pro vysokou dostupnost, nastavte tuto repliku příliš**asynchronní potvrdit** a **ruční**.
1. Klikněte na tlačítko OK.
1. V **Průzkumník objektů**, klikněte pravým tlačítkem na skupinu dostupnosti hello a klikněte na tlačítko **zobrazit řídicí panel**.
1. Na řídicím panelu hello, ověřte, že hello synchronizaci repliky v lokalitě hello zotavení po Havárii.
1. V **Průzkumník objektů**, klikněte pravým tlačítkem na skupinu dostupnosti hello a klikněte na tlačítko **převzetí služeb při selhání...** . SQL Server Management studia otevře Průvodce toofail přes SQL Server.  
1. Klikněte na tlačítko **Další**a vyberte hello instance systému SQL Server v lokalitě hello zotavení po Havárii. Klikněte na tlačítko **Další** znovu.
1. Toohello instance systému SQL Server v lokalitě hello zotavení po Havárii a klikněte na tlačítko **Další**.
1. Na hello **Souhrn** , ověřte nastavení hello a klikněte na tlačítko **Dokončit**.

Poté, co testování připojení, přesuňte primární data zpět tooyour hello primární repliky na střed a nastavení hello dostupnosti režimu back tootheir normální provozní. Hello následující tabulka uvádí hello normální provozní nastavení hello architektury popsané v tomto dokumentu:

| Umístění | Instance serveru | Role | Režim dostupnosti | Režim převzetí služeb při selhání
| ----- | ----- | ----- | ----- | -----
| Primární datové centrum | SQL-1 | Primární | Synchronní | Automatické
| Primární datové centrum | SQL-2 | Sekundární | Synchronní | Automatické
| Sekundární nebo vzdáleného datového centra | SQL – 3 | Sekundární | Asynchronní | Ruční


### <a name="more-information-about-planned-and-forced-manual-failover"></a>Další informace o plánované a vynucené ruční převzetí služeb při selhání

Další informace najdete v tématu hello následující témata:

- [Provedení plánované ruční převzetí služeb při selhání skupiny dostupnosti (SQL Server)](http://msdn.microsoft.com/library/hh231018.aspx)
- [Provedení vynucené ruční převzetí služeb při selhání skupiny dostupnosti (SQL Server)](http://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a>Další odkazy

* [Always On skupiny dostupnosti](http://msdn.microsoft.com/library/hh510230.aspx)
* [Virtuální počítače Azure](http://docs.microsoft.com/azure/virtual-machines/windows/)
* [Nástroje pro vyrovnávání zatížení Azure](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [Skupiny dostupnosti Azure](../manage-availability.md)
