---
title: "aaaConnect tooa virtuálního počítače systému SQL Server (Resource Manager) | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooSQL serveru spuštěny na virtuálním počítači v Azure. Toto téma používá model nasazení classic hello. scénáře Hello se liší v závislosti na konfiguraci sítí hello a umístění hello hello klienta."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: 7b127c14c37b9a72c19ed17f8b1dad61c7bc2d38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-sql-server-virtual-machine-on-azure-resource-manager"></a>Připojit tooa virtuálního počítače systému SQL Server na platformě Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-connect.md)
> * [Classic](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Přehled

Toto téma popisuje, jak tooconnect tooyour systému SQL Server instance spuštěna na virtuálním počítači Azure. Některé pokrývá [obecné připojení scénáře](#connection-scenarios) a pak poskytuje [podrobné kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Pokud máte by místo úplné návodu zřizování a připojení, najdete v části [zřizování virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Scénáře připojení

způsob Hello klient připojí tooSQL Server běžící na virtuálním počítači, se může lišit v závislosti na umístění hello hello klienta a síťové konfigurace hello.

Pokud zřizujete virtuální počítač SQL Server v hello portálu Azure, máte možnost hello určující typ hello **připojení SQL**.

![Veřejné možnosti připojení SQL během zřizování](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Možnosti připojení patří:

| Možnost | Popis |
|---|---|
| **Veřejné** | Připojit tooSQL serveru přes hello Internetu |
| **Privátní** | Připojit tooSQL serveru v hello stejné virtuální síti |
| **Místní** | Připojit tooSQL serveru místně na hello stejného virtuálního počítače | 

Hello následující části popisují hello **veřejné** a **privátní** možnosti podrobněji.

## <a name="connect-toosql-server-over-hello-internet"></a>Připojit tooSQL serveru přes Internet hello

Pokud chcete, aby databázový stroj SQL Server tooconnect tooyour z hello Internet, vyberte **veřejné** pro hello **připojení SQL** typ portálu hello během zřizování. Hello portálu automaticky hello následující kroky:

* Umožňuje hello protokol TCP/IP pro SQL Server.
* Nakonfiguruje hello tooopen pravidlo brány firewall port TCP systému SQL Server (standardně 1433).
* Umožňuje ověřování systému SQL Server, vyžaduje se pro veřejný přístup.
* Nakonfiguruje skupinu zabezpečení sítě hello na hello přenosy TCP tooall virtuálních počítačů na hello port serveru SQL Server.

> [!IMPORTANT]
> Hello bitové kopie virtuálních počítačů pro hello vývojáře serveru SQL a edice Express automaticky nepovolujte protokol hello protokolu TCP/IP. Pro edice Developer a Express, musíte použít SQL Server Configuration Manager příliš[ručně povolte protokol hello TCP/IP](#manualtcp) po vytvoření hello virtuálních počítačů.

Libovolného klienta s přístupem k Internetu, můžete připojit toohello instance systému SQL Server zadáním hello veřejnou IP adresu hello virtuálního počítače nebo libovolný popisek DNS přiřazenou toothat IP adresu. Pokud hello port serveru SQL Server je 1433, není nutné toospecify v hello připojovací řetězec. Hello následující připojovací řetězec připojí tooa virtuální počítač SQL s popiskem DNS z `sqlvmlabel.eastus.cloudapp.azure.com` pomocí ověřování SQL (můžete také použít hello veřejné IP adresy).

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

I když to umožňuje připojení klientů přes internet Dobrý den, to neznamená, že každý, kdo se může připojit tooyour systému SQL Server. Mimo klienti mají toohello správné uživatelské jméno a heslo. Pro dodatečné zabezpečení se však vyhnout hello dobře známého portu 1433. Například pokud jste nakonfigurovali toolisten systému SQL Server na portu 1500 a zavedené správné brány firewall a pravidel skupiny zabezpečení sítě, může připojení připojením hello port číslo toohello serveru název. Hello následující příklad mění hello předchozí přidáním vlastní číslo portu, **1 500**, toohello název serveru:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> Při dotazování systému SQL Server ve virtuálním počítači přes internet, všechna odchozí data hello z hello Azure datacenter je subjektu toonormal [ceny na odchozí přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="connect-toosql-server-within-a-virtual-network"></a>Připojit tooSQL serveru v rámci virtuální sítě

Pokud vyberete **privátní** pro hello **připojení SQL** typ hello portálu Azure nakonfiguruje většinu nastavení hello identické příliš**veřejné**. Hello jeden rozdílem je, že neexistuje žádná síť zabezpečení skupiny pravidlo tooallow mimo přenosy na portu hello systému SQL Server (standardně 1433).

> [!IMPORTANT]
> Hello bitové kopie virtuálních počítačů pro hello vývojáře serveru SQL a edice Express automaticky nepovolujte protokol hello protokolu TCP/IP. Pro edice Developer a Express, musíte použít SQL Server Configuration Manager příliš[ručně povolte protokol hello TCP/IP](#manualtcp) po vytvoření hello virtuálních počítačů.

Privátní připojení se často používá ve spojení s [virtuální sítě](../../../virtual-network/virtual-networks-overview.md), což umožňuje několik scénářů. Můžete se připojit virtuální počítače ve stejné virtuální síti, i když tyto virtuální počítače existovat v různých skupinách prostředků hello. A s [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), můžete vytvořit hybridní architekturu, která připojí virtuální počítače s místními sítěmi a počítače.

Virtuální sítě také povolit toojoin jste doménu tooa virtuálních počítačích Azure. Toto je hello pouze způsob toouse ověřování systému Windows tooSQL serveru. Hello jiné připojení scénáře vyžadují ověřování SQL pomocí uživatelského jména a hesla.

Za předpokladu, že nakonfigurujete DNS ve virtuální síti, můžete připojit tooyour instance systému SQL Server zadáním názvu počítače hello virtuální počítač s SQL serverem v hello připojovací řetězec. Hello také následující příklad předpokládá, že ověřování systému Windows je také nakonfigurovaný a tento uživatel hello byla udělena instance systému SQL Server toohello přístup.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a>Změňte nastavení připojení k SQL

Pro virtuální počítač systému SQL Server v hello portálu Azure, můžete změnit nastavení připojení k hello.

1. V hello portálu Azure, vyberte **virtuální počítače**.

2. Vyberte virtuální počítač systému SQL Server.

3. V části **nastavení**, klikněte na tlačítko **konfigurace systému SQL Server**.

4. Změna hello **úroveň připojení SQL** tooyour požadované nastavení. Volitelně můžete této oblasti toochange hello port serveru SQL Server nebo hello nastavení ověřování SQL.

   ![Změnit připojení SQL](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Počkejte několik minut, než toocomplete aktualizace hello.

   ![Virtuální počítač SQL aktualizace oznámení](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a>Povolte protokol TCP/IP pro edice Developer a Express

Při změně nastavení připojení k systému SQL Server, Azure automaticky neumožňuje protokol hello TCP/IP pro vývojáře serveru SQL a edice Express. Hello kroky vysvětlují, jak toomanually povolte protokol TCP/IP, takže se můžete vzdáleně připojit pomocí IP adresy.

Nejdřív připojte počítač systému SQL Server toohello pomocí vzdálené plochy.

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Dál povolte protokol hello TCP/IP s **SQL Server Configuration Manager**.

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>Připojení přes SSMS

Hello následující kroky ukazují, jak toocreate volitelné DNS popisek pro virtuální počítač Azure a potom se připojte pomocí SQL Server Management Studio (SSMS).

[!INCLUDE [Connect tooSQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Další kroky

zřizování pokyny toosee společně s postupem připojení naleznete v části [zřizování virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).

Další témata související s toorunning systému SQL Server ve virtuálních počítačích Azure, najdete v části [systému SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
