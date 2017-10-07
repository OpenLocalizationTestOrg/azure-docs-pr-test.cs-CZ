---
title: "aaaExtend místní skupiny dostupnosti Always On tooAzure | Microsoft Docs"
description: "Tento kurz používá prostředky, které jsou vytvořené pomocí modelu nasazení classic hello a popisuje, jak toouse hello průvodce přidat repliky v nástroji SQL Server Management Studio (SSMS) tooadd repliku vždy na skupiny dostupnosti v Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 7ca7c423-8342-4175-a70b-d5101dfb7f23
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 51fe117b40d69706b35f30101538a7a36116e128
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="extend-on-premises-always-on-availability-groups-tooazure"></a>Rozšíření tooAzure skupin dostupnosti Always On na místě
Skupiny dostupnosti Always On zajištění vysoké dostupnosti pro skupiny databáze přidáním sekundární repliky. Povolit tyto repliky přebírání služeb při selhání databáze v případě selhání. Kromě toho mohou být použité toooffload číst úlohy nebo úlohy zálohování.

Místní skupiny dostupnosti tooMicrosoft Azure můžete rozšířit zřizování jeden nebo více virtuálních počítačů Azure se systémem SQL Server a potom je přidat jako repliky tooyour místní skupiny dostupnosti.

Tento kurz předpokládá, že máte hello následující:

* Aktivní předplatné Azure. Můžete [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* Existující vždy na skupinu dostupnosti místní. Další informace o skupinách dostupnosti najdete v tématu [skupin dostupnosti Always On](https://msdn.microsoft.com/library/hh510230.aspx).
* Připojení mezi hello místní sítě a virtuální sítě Azure. Další informace o vytváření této virtuální sítě najdete v tématu [vytvořit Site-to-Site připojení pomocí portálu Azure (klasický) hello](../../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md).

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

## <a name="add-azure-replica-wizard"></a>Průvodce Azure repliky přidáním
V této části se dozvíte, jak toouse hello **Průvodce přidáním Azure repliky** tooextend tooinclude vaše vždy na skupiny dostupnosti řešení Azure repliky.

> [!IMPORTANT]
> Hello **Průvodce přidáním Azure repliky** podporuje pouze virtuální počítače vytvořené pomocí modelu nasazení Classic hello. Nové nasazení virtuálního počítače by měla používat hello novější modelu Resource Manager. Pokud používáte virtuální počítače s Resource Managerem, je třeba ručně přidat hello sekundární Azure repliky pomocí jazyka Transact-SQL commmands (není tady zobrazené). Tento průvodce nebudou fungovat ve scénáři hello Resource Manager.

1. Z v systému SQL Server Management Studio rozbalte **vždy na vysokou dostupnost** > **skupiny dostupnosti** > **[název skupiny dostupnosti]**.
2. Klikněte pravým tlačítkem na **replik dostupnosti**, pak klikněte na tlačítko **přidat repliky**.
3. Ve výchozím nastavení, hello **přidat repliky tooAvailability Průvodce skupiny** se zobrazí. Klikněte na **Další**.  Pokud jste vybrali hello **tuto stránku již příště nezobrazovat** možnost v hello dolní části stránky hello při předchozím spuštění tohoto průvodce, tato obrazovka se nezobrazí.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)
4. Bude požadováno tooconnect tooall existující sekundární repliky. Kliknutím na **připojení...** vedle položky každou repliku nebo můžete kliknout na tlačítko **připojit všechny...** v hello dolní části obrazovky hello. Po ověření, klikněte na tlačítko **Další** tooadvance toohello další obrazovce.
5. Na hello **zadejte repliky** stránky, jsou uvedeny několik karet v horní části hello: **repliky**, **koncové body**, **zálohování Předvolby**, a **Naslouchací proces**. Z hello **repliky** , klikněte na **přidat repliky Azure...** toolaunch hello Průvodce přidáním Azure repliky.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)
6. Vyberte certifikát pro správu Azure z místního úložiště certifikátů Windows hello, pokud jste nainstalovali jednu před. Vyberte nebo zadejte hello id předplatného Azure, pokud jste už použili před. Můžete klikněte na tlačítko toodownload stáhnout a nainstalovat certifikát pro správu Azure a stáhnout hello seznam předplatných, pomocí účtu Azure.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)
7. Zaplníte každé pole na stránce hello s hodnotami, které budou použité toocreate hello Azure virtuálního počítače (VM), který bude hostitelem hello repliky.
   
   | Nastavení | Popis |
   | --- | --- |
   | **Bitové kopie** |Vyberte kombinace hello požadovaného operačního systému a SQL Server |
   | **Velikost virtuálního počítače** |Vyberte hello velikost virtuálního počítače, který nejlépe vyhovuje vašim obchodním potřebám |
   | **Název virtuálního počítače.** |Zadejte jedinečný název pro hello nového virtuálního počítače. Hello název musí obsahovat 3 až 15 znaků, může obsahovat pouze písmena, číslice a pomlčky a musí začínat písmenem a končit písmenem nebo číslicí. |
   | **Uživatelské jméno virtuálních počítačů** |Zadejte uživatelské jméno, které bude účet správce hello na hello virtuálních počítačů |
   | **Heslo správce virtuálních počítačů** |Zadejte heslo pro nový účet hello |
   | **Potvrzení hesla** |Potvrzení hesla hello hello nového účtu |
   | **Virtual Network** |Zadejte hello Azure virtuální sítě tento text hello, který by měl používat nový virtuální počítač. Další informace o virtuálních sítích najdete v tématu [Přehled virtuálních sítí](../../../virtual-network/virtual-networks-overview.md). |
   | **Podsíť virtuální sítě** |Zadejte podsíť virtuální sítě hello této hello, který by měl používat nový virtuální počítač |
   | **Domény** |Potvrďte správnost hello předem vyplněná hodnotu pro doménu hello |
   | **Uživatelské jméno domény** |Zadejte účet, který je v hello místní skupiny administrators v místním clusteru uzlech hello |
   | **Heslo** |Zadejte hello heslo pro uživatelské jméno domény hello |
8. Klikněte na tlačítko **OK** nastavení nasazení toovalidate hello.
9. Zobrazí se další právní podmínky. Přečtěte si a klikněte na tlačítko **OK** Pokud souhlasíte s tím toothese podmínky.
10. Hello **zadejte repliky** znovu se zobrazí stránka. Ověřte nastavení hello hello nové Azure repliky na hello **repliky**, **koncové body**, a **zálohování Předvolby** karty. Upravte nastavení toomeet vaše podnikové požadavky.  Další informace o parametrech hello obsažené v těchto karet najdete v tématu [určení stránky repliky (dostupnosti průvodce novou skupinou průvodce nebo přidat repliky)](https://msdn.microsoft.com/library/hh213088.aspx). Všimněte si, že naslouchací procesy nelze vytvořit pomocí karty hello naslouchací proces pro skupiny dostupnosti, které obsahují Azure repliky. Kromě toho pokud naslouchací proces byl již vytvořen předchozí toolaunching hello průvodce, obdržíte zprávu s upozorněním, že se nepodporuje v Azure. Podíváme se na to, jak toocreate naslouchací procesy v hello **vytvořit naslouchací proces skupiny dostupnosti** části.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)
11. Klikněte na **Další**.
12. Vyberte metodu synchronizace dat hello chcete toouse na hello **vybrat počáteční synchronizaci dat** a klikněte na tlačítko **Další**. Pro většinu scénářů, vyberte **úplná synchronizace dat**. Další informace o metodách synchronizace dat najdete v tématu [vyberte počáteční synchronizace stránku dat (vždy na dostupnosti skupiny průvodci)](https://msdn.microsoft.com/library/hh231021.aspx).
13. Zkontrolujte výsledky hello na hello **ověření** stránky. Opravte zbývající potíže a znovu spusťte hello ověření v případě potřeby. Klikněte na **Další**.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)
14. Zkontrolujte nastavení hello na hello **Souhrn** stránky a pak klikněte na **Dokončit**.
15. zahájí proces zajišťování Hello. Po úspěšném dokončení Průvodce hello klikněte na tlačítko **Zavřít** tooexit mimo hello průvodce.

> [!NOTE]
> Hello Průvodce přidáním Azure repliky soubor protokolu se vytvoří v Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Tento soubor protokolu může být nasazení Azure repliky použité tootroubleshoot se nezdařilo. Pokud hello průvodce selže, provádění všech akcí, budou vráceny všechny předchozí operace, včetně odstraňování hello zřízení virtuálního počítače.
> 
> 

## <a name="create-an-availability-group-listener"></a>Vytvořte naslouchací proces skupiny dostupnosti
Po vytvoření skupiny dostupnosti hello, měli byste vytvořit naslouchací proces pro klienty tooconnect toohello repliky. Naslouchací procesy směrují příchozí tooeither připojení hello primární nebo sekundární repliku jen pro čtení. Další informace o naslouchací procesy najdete v tématu [nakonfigurovat modul pro naslouchání ILB pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-int-listener.md).

## <a name="next-steps"></a>Další kroky
V přidání toousing hello **Průvodce přidáním Azure repliky** tooextend vaše tooAzure vždy na skupiny dostupnosti, může také přesunout některé úlohy SQL serveru zcela tooAzure. tooget začít, najdete v části [zřizování virtuálního počítače systému SQL Server na platformě Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Další témata související s toorunning systému SQL Server ve virtuálních počítačích Azure, najdete v části [systému SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

