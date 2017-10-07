---
title: "aaaConnect tooa virtuálního počítače systému SQL Server na platformě Azure (klasický) | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooSQL serveru spuštěny na virtuálním počítači v Azure. Toto téma používá model nasazení classic hello. scénáře Hello se liší v závislosti na konfiguraci sítí hello a umístění hello hello klienta."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: 416948af-454f-4cfe-8fd2-7cf971cbd3e9
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: jroth
experimental_id: d51f3cc6-753b-4e
ms.openlocfilehash: 4fff3936ad0bcfd3a56855a8436991e19b92522b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-sql-server-virtual-machine-on-azure-classic-deployment"></a>Připojit tooa virtuálního počítače systému SQL Server na platformě Azure (nasazení Classic)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-connect.md)
> * [Classic](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Přehled
Toto téma popisuje, jak tooconnect tooyour systému SQL Server instance spuštěna na virtuálním počítači Azure. Některé pokrývá [obecné připojení scénáře](#connection-scenarios) a pak poskytuje [podrobné kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Pokud používáte Správce prostředků virtuálních počítačů, přečtěte si téma [připojit tooa virtuálního počítače systému SQL Server v Azure pomocí Správce prostředků](../sql/virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Scénáře připojení
způsob Hello klient připojí tooSQL Server běžící na virtuálním počítači, se může lišit v závislosti na umístění hello hello klienta a konfigurace počítače/sítě hello. Mezi tyto scénáře patří:

* [Připojit tooSQL serveru v hello stejné cloudové služby](#connect-to-sql-server-in-the-same-cloud-service)
* [Připojit tooSQL serveru přes hello Internetu](#connect-to-sql-server-over-the-internet)
* [Připojit tooSQL serveru v hello stejné virtuální síti](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Než připojíte pomocí některé z těchto metod, je třeba postupovat podle hello [kroky v této připojení tooconfigure článku](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).
> 
> 

### <a name="connect-toosql-server-in-hello-same-cloud-service"></a>Připojit tooSQL serveru v hello stejné cloudové služby
Více virtuálních počítačů lze vytvořit v hello stejné cloudové služby. toounderstand tento virtuální počítače, scénář, najdete v části [jak tooconnect virtuálních počítačů s virtuální sítě nebo cloudovou službu](../classic/connect-vms.md#connect-vms-in-a-standalone-cloud-service). Tento scénář je při spuštění serveru tooSQL tooconnect klienta na jeden virtuální počítač na jiný virtuální počítač v hello stejné cloudové služby.

V tomto scénáři můžete připojit pomocí hello virtuálních počítačů **název** (také uvedené jako **název počítače** nebo **hostname** hello portálu). Toto je hello název, který jste zadali pro hello virtuálních počítačů během vytváření. Například, pokud jste s názvem virtuální počítač SQL **mysqlvm**, klienta virtuálního počítače ve stejné cloudové služby může použít hello hello následující tooconnect řetězec připojení:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-toosql-server-over-hello-internet"></a>Připojit tooSQL serveru přes Internet hello
Pokud chcete, aby databázový stroj SQL Server tooconnect tooyour z hello Internet, musíte vytvořit koncový bod virtuálního počítače pro příchozí komunikaci TCP. Tento krok konfigurace Azure bude směrovat příchozí TCP port provoz tooa port TCP, který je přístupný toohello virtuální počítač.

hello tooconnect přes internet, je nutné použít hello Virtuálního počítače DNS název a hello virtuálních počítačů koncový bod číslo portu (nakonfigurované později v tomto článku). toofind hello název DNS, přejděte toohello Azure portálu a vyberte možnost **virtuálních počítačů (klasické)**. Potom vyberte virtuální počítač. Hello **název DNS** se zobrazí v hello **přehled** části.

Představte si třeba klasické virtuální počítač s názvem **mysqlvm** s názvem DNS **mysqlvm7777.cloudapp.net** a koncový bod virtuálního počítače z **57500**. Za předpokladu, že správně nakonfigurované připojení, hello následující připojovací řetězec může být použité tooaccess hello virtuálního počítače z libovolného místa na hello Internetu:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

I když to umožňuje připojení klientů přes internet Dobrý den, to neznamená, že každý, kdo se může připojit tooyour systému SQL Server. Mimo klienti mají toohello správné uživatelské jméno a heslo. Pro dodatečné zabezpečení nepoužívejte hello dobře známého portu 1433 pro koncový bod hello veřejné virtuálního počítače. A pokud je to možné, zvažte přidání ACL na váš koncový bod toorestrict provoz jenom toohello klientů, které je povolit. Pokyny pro koncové body pomocí seznamů řízení přístupu, v tématu [spravovat hello seznamu ACL v koncovém bodě](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

> [!NOTE]
> Je důležité, použijete-li tato technika toocommunicate s SQL serverem, všechny odchozí data z hello datové centrum Azure toonote subjektu toonormal [ceny na odchozí přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

### <a name="connect-toosql-server-in-hello-same-virtual-network"></a>Připojit tooSQL serveru v hello stejné virtuální síti
[Virtuální síť](../../../virtual-network/virtual-networks-overview.md) další scénáře. Můžete propojit virtuální počítače v hello stejné virtuální síti, i když tyto virtuální počítače existovat v různých cloudové služby. A s [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), můžete vytvořit hybridní architekturu, která připojí virtuální počítače s místními sítěmi a počítače.

Virtuální sítě také umožňuje vám toojoin doménu tooa virtuálních počítačích Azure. Toto je hello pouze způsob toouse ověřování systému Windows tooSQL serveru. Hello jiné připojení scénáře vyžadují ověřování SQL pomocí uživatelského jména a hesla.

Pokud chcete tooconfigure prostředí domény a ověřování systému Windows, není nutné toouse hello kroky v této veřejný koncový bod článku tooconfigure hello nebo hello ověřování SQL a přihlášení. V tomto scénáři můžete připojit zadáním hello virtuální počítač SQL Server název v připojovacím řetězci hello tooyour instance systému SQL Server. Hello následující příklad předpokládá, že ověřování systému Windows je také nakonfigurovaný a tento uživatel hello byla udělena instance systému SQL Server toohello přístup.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure
Hello následující kroky ukazují, jak hello tooconnect toohello instance systému SQL Server přes internet pomocí služby SQL Server Management Studio (SSMS). Ale hello stejný postup platí toomaking virtuálního počítače systému SQL Server dostupné pro vaše aplikace spuštěna v místním a v Azure.

Než můžete připojit toohello instance systému SQL Server z jiného virtuálního počítače nebo hello internet, je třeba provést následující úlohy, jak je popsáno v části hello hello:

* [Vytvoření koncového bodu protokolu TCP pro virtuální počítač hello](#create-a-tcp-endpoint-for-the-virtual-machine)
* [Otevřete porty protokolu TCP v bráně Windows firewall hello](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [Konfigurace systému SQL Server toolisten na hello protokolu TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [Konfigurace systému SQL Server pro smíšený režim ověřování](#configure-sql-server-for-mixed-mode-authentication)
* [Vytvoření přihlášení ověřování serveru SQL](#create-sql-server-authentication-logins)
* [Určení názvu DNS hello hello virtuálního počítače](#determine-the-dns-name-of-the-virtual-machine)
* [Připojit toohello databázový stroj z jiného počítače](#connect-to-the-database-engine-from-another-computer)

Cesta připojení Hello je shrnuté podle hello následující diagram:

![Připojení virtuálního počítače tooa SQL serverem](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect tooSQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect tooSQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect tooSQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Další kroky
Pokud plánujete také toouse skupiny dostupnosti AlwaysOn pro vysokou dostupnost a zotavení po havárii, měli byste zvážit implementace naslouchací proces. Databáze klienti připojovat naslouchací proces toohello místo přímo tooone hello instance systému SQL Server. Hello naslouchací proces trasy klienti toohello primární repliky ve skupině dostupnosti hello. Další informace najdete v tématu [konfigurace o ILB naslouchací proces skupiny dostupnosti AlwaysOn v Azure](../classic/ps-sql-int-listener.md).

Je důležité tooreview všechny hello zabezpečení osvědčené postupy pro SQL Server běžící na virtuálním počítači Azure. Další informace najdete v tématu [Informace o zabezpečení pro SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-security.md).

[Prozkoumejte hello studijní](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro SQL Server na virtuálních počítačích Azure. 

Další témata související s toorunning systému SQL Server ve virtuálních počítačích Azure, najdete v části [systému SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

