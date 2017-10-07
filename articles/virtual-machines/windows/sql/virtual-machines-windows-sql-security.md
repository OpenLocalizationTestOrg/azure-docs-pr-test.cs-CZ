---
title: "aaaSecurity důležité informace týkající se systému SQL Server v Azure | Microsoft Docs"
description: "Toto téma obsahuje obecné pokyny pro zabezpečení SQL Server běžící virtuálním počítači Azure."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2017
ms.author: jroth
ms.openlocfilehash: 14c3d828fa87446da67beea6d28886de254afe15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Informace o zabezpečení pro SQL Server v Azure Virtual Machines

Toto téma obsahuje celkového zabezpečení pokyny, které pomáhají stanovit instance serveru tooSQL zabezpečený přístup v Azure virtuální počítač (VM).

Azure v souladu s několika oborové směrnice brát a standardů, které můžete umožňují toobuild řešení kompatibilní se systémem SQL Server běží na virtuálním počítači. Informace o dodržování legislativní předpisů s Azure najdete v tématu [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="control-access-toohello-sql-vm"></a>Řízení přístupu toohello virtuální počítač SQL

Při vytváření virtuálního počítače systému SQL Server, zvažte, jak toocarefully řídit, kdo má přístup toohello počítač a tooSQL serveru. Obecně platí, měli byste udělat následující hello:

- Omezte přístup k tooSQL serveru tooonly hello aplikacím a klienti, kteří ho potřebují.
- Dodržujte doporučené postupy pro správu uživatelské účty a hesla.

Hello následující oddíly poskytují návrhy na přemýšlení prostřednictvím těchto bodů.

## <a name="secure-connections"></a>Zabezpečené připojení

Při vytváření virtuálního počítače s SQL serverem pomocí bitové kopie galerie, hello **připojení k serveru SQL** možnost poskytuje hello volbu **místní (uvnitř virtuálního počítače)**, **privátní (uvnitř virtuální sítě)** , nebo **veřejné (Internet)**.

![Připojení k SQL serveru](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

Hello nejvyšší zabezpečení vyberte možnost nejvíc omezující hello pro váš scénář. Například pokud používáte aplikaci, která přistupuje k systému SQL Server na hello stejného virtuálního počítače, pak **místní** je nejbezpečnější volbou hello. Pokud používáte Azure aplikaci, která vyžaduje toohello přístup k systému SQL Server, pak **privátní** zabezpečuje komunikaci tooSQL Server pouze v rámci hello zadaný [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). Pokud budete potřebovat **veřejné** (internest) přístup toohello virtuální počítač serveru SQL Server, pak zkontrolujte že toofollow jiných osvědčených postupů v této tooreduce tématu útoku útoku.

Hello vybrané možnosti portálu hello použít pravidla zabezpečení příchozích na hello virtuální počítače [skupinu zabezpečení sítě](../../../virtual-network/virtual-networks-nsg.md) tooallow (NSG) nebo odepřít síťový provoz tooyour virtuálního počítače. Můžete upravit nebo vytvořit nový příchozí pravidla NSG tooallow provoz toohello systému SQL Server port (standardně 1433). Můžete také zadat konkrétní IP adresy, které jsou povoleny toocommunicate prostřednictvím tohoto portu.

![Pravidla skupiny zabezpečení sítě](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

Kromě toho tooNSG pravidla toorestrict síťový provoz, můžete použít také hello brány Windows Firewall na virtuálním počítači hello.

Pokud používáte koncových bodů s modelem nasazení classic hello, odeberte žádné koncové body na virtuálním počítači hello, pokud je nepoužíváte. Pokyny pro koncové body pomocí seznamů řízení přístupu, v tématu [spravovat hello seznamu ACL v koncovém bodě](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint). To není nutné pro virtuální počítače, které používají hello Resource Manager.

Nakonec je třeba zvážit povolení šifrované připojení pro hello instanci hello databázového stroje SQL Server ve virtuálním počítači Azure. Instance systému SQL server nakonfigurujte certifikát podepsaný držitelem. Další informace najdete v tématu [toohello povolit šifrované připojení databázový stroj](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine) a [syntaxi připojovacího řetězce](https://msdn.microsoft.com/library/ms254500.aspx).

## <a name="use-a-non-default-port"></a>Použijte jiný než výchozí port

Ve výchozím nastavení SQL Server naslouchá na dobře známé port 1433. Pro zvýšení zabezpečení nakonfigurujte na jiný než výchozí port, jako je například 1401 toolisten systému SQL Server. Pokud zřizujete Galerie bitovou kopii systému SQL Server v hello portálu Azure, můžete zadat Tento port v hello **nastavení systému SQL Server** okno.

tooconfigure tento po zřízení, máte dvě možnosti:

- Pro virtuální počítače správce prostředků, můžete vybrat **konfigurace systému SQL Server** v okně Přehled hello virtuálních počítačů. To nabízí možnost toochange hello portu.

  ![Změnit TCP port portálu](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- Klasické virtuální počítače nebo virtuální počítače serveru SQL, které nebyly zřízeny hello portálu můžete ručně nakonfigurovat hello port připojením vzdáleně toohello virtuálních počítačů. Postup konfigurace hello najdete v tématu [konfigurace serveru tooListen na specifickém portu TCP](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port). Pokud chcete použít tento Ruční postup, musíte taky tooadd brány Windows Firewall pravidlo tooallow příchozí přenosy na portu TCP.

> [!IMPORTANT]
> Určení jiný než výchozí port je vhodné, pokud je připojení k Internetu otevřete toopublic vaše port serveru SQL Server.

Pokud SQL Server naslouchá na jiný než výchozí port, zadejte hello port při připojení. Zvažte například scénář, kde je adresa IP serveru hello 13.55.255.255 a SQL Server naslouchá na portu 1401. tooconnect tooSQL serveru, zadali byste `13.55.255.255,1401` hello připojovacího řetězce.

## <a name="manage-accounts"></a>Správa účtů

Nechcete, aby útočníci tooeasily odhad účet jména ani hesla. Použijte následující tipy toohelp hello:

- Vytvořte účet jedinečný místního správce, který není s názvem **správce**.

- Používejte silná hesla, komplexní pro všechny účty. Další informace o tom, najdete v části toocreate silné heslo, [vytvořit silné heslo](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password) článku.

- Ve výchozím nastavení vybere Azure ověřování systému Windows během instalace virtuálního počítače systému SQL Server. Proto hello **SA** přihlášení je zakázané a heslo přiděluje instalační program. Doporučujeme, abyste tento hello **SA** přihlášení by neměly být použít nebo povoleno. Pokud máte přihlašovací jméno SQL, použijte jednu z následujících strategií hello:

  - Vytvoření účtu SQL s jedinečným názvem, který má **sysadmin** členství. Můžete k tomu z portálu hello povolením **ověřování SQL** během zřizování.

    > [!TIP] 
    > Pokud nepovolíte ověřování SQL při zřizování, musíte ručně změnit režim ověřování hello příliš**systému SQL Server a režim ověřování systému Windows**. Další informace najdete v tématu [Změna režimu ověřování serveru](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode).

  - Pokud musíte použít hello **SA** přihlášení, povolit hello přihlášení po zřízení a přiřazení nové silné heslo.

## <a name="follow-on-premises-best-practices"></a>Použijte místní osvědčené postupy

Kromě toho toohello postupy popsané v tomto tématu, doporučujeme, zkontrolujte a implementovat postupy zabezpečení tradičních místních hello, kde je to možné. Další informace najdete v tématu [důležité informace o zabezpečení pro instalaci serveru SQL](https://docs.microsoft.com/sql/sql-server/install/security-considerations-for-a-sql-server-installation)

## <a name="next-steps"></a>Další kroky

Pokud vás zajímá také osvědčené postupy týkající se výkonu, najdete v části [osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Další témata související s toorunning systému SQL Server ve virtuálních počítačích Azure, najdete v části [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

