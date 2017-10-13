---
title: "Připojit k virtuálnímu počítači serveru SQL v Azure (klasický) | Microsoft Docs"
description: "Zjistěte, jak se připojit k serveru SQL, které jsou spuštěny na virtuálním počítači v Azure. Toto téma používá model nasazení classic. Scénáře se liší v závislosti na konfiguraci sítě a umístění klienta."
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
ms.openlocfilehash: 67b328cb754e49fe1dea9d57f74dd31793acd93c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Připojení k virtuálnímu počítači s SQL Serverem v Azure (klasické nasazení)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-connect.md)
> * [Classic](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Přehled
Toto téma popisuje, jak se připojit k instanci systému SQL Server spuštěna na virtuálním počítači Azure. Některé pokrývá [obecné připojení scénáře](#connection-scenarios) a pak poskytuje [podrobné kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Pokud používáte Správce prostředků virtuálních počítačů, přečtěte si téma [připojení SQL serveru virtuálnímu počítači na platformě Azure pomocí Resource Manager](../sql/virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Scénáře připojení
Způsob, jakým se klient připojí k serveru SQL Server spuštěny na virtuálním počítači se liší v závislosti na umístění klienta a konfigurace počítače nebo sítě. Mezi tyto scénáře patří:

* [Připojení k systému SQL Server v rámci stejné cloudové služby](#connect-to-sql-server-in-the-same-cloud-service)
* [Připojení k systému SQL Server přes internet](#connect-to-sql-server-over-the-internet)
* [Připojení k systému SQL Server ve stejné virtuální síti](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Než připojíte pomocí některé z těchto metod, je třeba postupovat podle [kroky v tomto článku ke konfiguraci připojení](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).
> 
> 

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Připojení k systému SQL Server v rámci stejné cloudové služby
Více virtuálních počítačů lze vytvořit v rámci stejné cloudové služby. Zjistit tento scénář virtuálních počítačů najdete v části [postupy pro připojení virtuálních počítačů pomocí virtuální sítě nebo cloudové služby](../classic/connect-vms.md#connect-vms-in-a-standalone-cloud-service). V tomto scénáři klient na jeden virtuální počítač pokusí připojit k SQL Server běžící na jiný virtuální počítač v rámci stejné cloudové služby.

V tomto scénáři můžete připojit pomocí virtuální počítač **název** (také uvedené jako **název počítače** nebo **hostname** na portálu). Toto je název, který jste zadali pro virtuální počítač při vytvoření. Například, pokud jste s názvem virtuální počítač SQL **mysqlvm**, může klient virtuálních počítačů v rámci stejné cloudové služby může používat následující připojovací řetězec pro připojení:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Připojení k systému SQL Server přes Internet
Pokud se chcete připojit k vaší databázový stroj SQL Server z Internetu, musíte vytvořit koncový bod virtuálního počítače pro příchozí komunikaci TCP. Tento krok konfigurace Azure směruje příchozí provoz portu TCP na port TCP, který je pro virtuální počítač přístupný.

K připojení přes internet, musíte použít název DNS Virtuálního počítače a číslo portu koncový bod virtuálního počítače (nakonfigurované později v tomto článku). K vyhledání názvu DNS, přejděte na portál Azure a vyberte **virtuálních počítačů (klasické)**. Potom vyberte virtuální počítač. **Název DNS** se zobrazí v **přehled** části.

Představte si třeba klasické virtuální počítač s názvem **mysqlvm** s názvem DNS **mysqlvm7777.cloudapp.net** a koncový bod virtuálního počítače z **57500**. Za předpokladu, že správně nakonfigurované připojení, může být následující připojovací řetězec použity pro přístup virtuální počítač z libovolného místa v síti internet:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

I když tento připojovací řetězec umožňuje připojení klientů přes internet, to neznamená, že každý, kdo může připojit k systému SQL Server. Klienti nemusí mimo správné uživatelské jméno a heslo. Pro dodatečné zabezpečení nepoužívejte dobře známého portu 1433 pro koncový bod veřejné virtuálního počítače. A pokud je to možné, zvažte přidání ACL na váš koncový bod omezit jenom na klienty provoz je povolit. Pokyny pro koncové body pomocí seznamů řízení přístupu, v tématu [spravovat seznam ACL u koncového bodu](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

> [!NOTE]
> Pokud tento postup se používá ke komunikaci se serverem SQL Server, všechny odchozí data z datové centrum Azure podléhá normální [ceny na odchozí přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Připojení k systému SQL Server ve stejné virtuální síti
[Virtuální síť](../../../virtual-network/virtual-networks-overview.md) další scénáře. Můžete připojit virtuální počítače ve stejné virtuální síti, i když tyto virtuální počítače existovat v různých cloudové služby. A s [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), můžete vytvořit hybridní architekturu, která připojí virtuální počítače s místními sítěmi a počítače.

Virtuální sítě umožňují připojit virtuální počítače Azure k doméně. Připojení k doméně je jediným způsobem, jak použít ověřování systému Windows se systémem SQL Server. Jiné připojení scénáře vyžadují ověřování SQL pomocí uživatelského jména a hesla.

Pokud chcete konfigurovat prostředí domény a ověřování systému Windows, není potřeba konfigurovat koncový bod veřejné nebo ověřování SQL a přihlášení. V tomto scénáři můžete připojit k instanci SQL serveru tak, že zadáte název virtuálního počítače SQL serveru v připojovacím řetězci. V následujícím příkladu se předpokládá, že ověřování systému Windows byla nakonfigurována a aby uživatel byl udělen přístup k instanci systému SQL Server.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure
Následující kroky ukazují, jak se připojit k instanci systému SQL Server přes internet pomocí služby SQL Server Management Studio (SSMS). Stejný postup však použít k vytvoření virtuálního počítače systému SQL Server dostupné pro vaše aplikace spuštěna v místním a v Azure.

Než může připojit k instanci systému SQL Server z jiného virtuálního počítače nebo internet, musíte provést následující úlohy:

* [Vytvoření koncového bodu protokolu TCP pro virtuální počítač](#create-a-tcp-endpoint-for-the-virtual-machine)
* [V bráně Windows firewall otevřete porty TCP](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [Konfigurace SQL serveru pro naslouchání na protokolu TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [Konfigurace systému SQL Server pro smíšený režim ověřování](#configure-sql-server-for-mixed-mode-authentication)
* [Vytvoření přihlášení ověřování serveru SQL](#create-sql-server-authentication-logins)
* [Zjistit název DNS virtuálního počítače](#determine-the-dns-name-of-the-virtual-machine)
* [Připojit k databázovému stroji z jiného počítače](#connect-to-the-database-engine-from-another-computer)

Cesta připojení je shrnuté podle následující diagram:

![Připojení k virtuálnímu počítači s SQL serverem](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect to SQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect to SQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Další kroky
Pokud plánujete také použití skupin dostupnosti AlwaysOn pro vysokou dostupnost a zotavení po havárii, měli byste zvážit implementace naslouchací proces. Databáze klienti připojovat k naslouchacímu procesu, nikoli přímo do jedna z instancí systému SQL Server. Naslouchací proces směrování klienty na primární repliku ve skupině dostupnosti. Další informace najdete v tématu [konfigurace o ILB naslouchací proces skupiny dostupnosti AlwaysOn v Azure](../classic/ps-sql-int-listener.md).

Je důležité zkontrolovat všechny osvědčené postupy zabezpečení pro SQL Server běžící na virtuálním počítači Azure. Další informace najdete v tématu [Informace o zabezpečení pro SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-security.md).

[Projděte si mapy kurzů](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro SQL Server na virtuálních počítačích Azure. 

Další témata související se systémem SQL Server ve virtuálních počítačích Azure, najdete v části [systému SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

