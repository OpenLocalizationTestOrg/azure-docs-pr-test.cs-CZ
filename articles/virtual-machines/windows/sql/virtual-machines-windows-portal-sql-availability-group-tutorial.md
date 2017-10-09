---
title: "aaaSQL skupin dostupnosti serveru – virtuální počítače Azure – kurz | Microsoft Docs"
description: "Tento kurz ukazuje, jak toocreate serveru vždy na skupinu dostupnosti SQL ve virtuálních počítačích Azure."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: 65b4210b0f851828a32a02053b03e4b8d469ba4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a>Konfigurovat vždy na skupiny dostupnosti ve virtuálním počítači Azure ručně

Tento kurz ukazuje, jak toocreate serveru vždy na skupinu dostupnosti SQL ve virtuálních počítačích Azure. dokončení kurzu Hello vytvoří skupinu dostupnosti s repliku databáze na dva servery SQL.

**Odhad času**: trvá asi 30 minut toocomplete, jakmile jsou splněny požadavky hello.

Hello diagram znázorňuje, co vytvoříte v kurzu hello.

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a>Požadavky

Hello kurz předpokládá, že máte základní znalosti o SQL serveru skupin dostupnosti Always On. Pokud potřebujete další informace, přečtěte si [přehled o skupin dostupnosti Always On (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).

Hello následující tabulka uvádí hello požadavky, je nutné toocomplete před zahájením tohoto kurzu:

|  |Požadavek |Popis |
|----- |----- |----- |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | Dva servery SQL | -V nastavení dostupnosti Azure <br/> -V jedné doméně <br/> -S nainstalovanou funkcí Clustering převzetí služeb při selhání |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| Windows Server | Sdílení souborů pro cluster s kopií clusteru |  
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Účet služby SQL Server | Účet domény |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Účet služby agenta systému SQL Server | Účet domény |  
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Otevřete porty brány firewall | -SQL Server: **1433** pro výchozí instanci <br/> -Koncový bod zrcadlení databáze: **5022** nebo jakýkoli dostupný port <br/> -Sondu nástroje pro vyrovnávání zatížení azure: **59999** nebo jakýkoli dostupný port |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Přidejte funkci Clustering převzetí služeb při selhání | Tato funkce vyžaduje oba servery SQL |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Účet domény instalace | – Místní správce na každém serveru SQL <br/> -Členem systému SQL Server pevné role serveru sysadmin pro každou instanci systému SQL Server  |


Než začnete hello kurz, musíte příliš[dokončení požadované součásti pro vytváření skupin dostupnosti Always On v Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md). Pokud tyto požadavky jsou již byla dokončena, můžete přeskočit příliš[vytvořením clusteru](#CreateCluster).


<!--**Procedure**: *This is hello first “step”. Make titles H2’s and short and clear – H2’s appear in hello right pane on hello web page and are important for navigation.*-->

<a name="CreateCluster"></a>
##Vytvoření clusteru hello

Po hello, které požadavky jsou dokončeny je prvním krokem hello toocreate clusteru převzetí služeb při selhání Windows serveru, který obsahuje dva servery SQL Server a server s kopií clusteru.  

1. RDP toohello první systému SQL Server pomocí účtu domény, který je správcem na servery SQL a hello monitorovací server.

   >[!TIP]
   >Pokud jste postupovali podle hello [požadovaný dokument](virtual-machines-windows-portal-sql-availability-group-prereq.md), vytvořili účet názvem **CORP\Install**. Pomocí tohoto účtu.

2. V hello **správce serveru** řídicí panel, vyberte **nástroje**a potom klikněte na **Správce clusteru převzetí služeb při selhání**.
3. V levém podokně hello, klikněte pravým tlačítkem na **Správce clusteru převzetí služeb při selhání**a potom klikněte na **vytvoření clusteru s podporou**.
   ![Vytvoření clusteru](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)
4. V hello Průvodce vytvořením clusteru, vytvořit cluster s jedním uzlem a procházení hello stránky s nastaveními hello v hello následující tabulka:

   | Stránka | Nastavení |
   | --- | --- |
   | Než začnete |Použít výchozí nastavení |
   | Vyberte servery |Typ hello název první SQL serveru v **zadejte název serveru** a klikněte na tlačítko **přidat**. |
   | Upozornění ověření |Vyberte **ne. I nevyžadují podporu společnosti Microsoft pro tento cluster a proto nechcete, aby toorun hello ověřovací testy. Po klepnutí na tlačítko Další, pokračovat ve vytváření clusteru hello**. |
   | Přístupový bod pro hello Správa clusteru |Zadejte název clusteru, například **SQLAGCluster1** v **název clusteru**.|
   | Potvrzení |Použít výchozí hodnoty, pokud nepoužíváte prostory úložiště. Další informace v poznámce hello za touto tabulkou. |

### <a name="set-hello-cluster-ip-address"></a>Nastavte IP adresu clusteru hello

1. V **Správce clusteru převzetí služeb při selhání**, posuňte se dolů příliš**základní prostředky clusteru** a rozbalte podrobnosti o clusteru hello. Měli byste vidět obou hello **název** a hello **IP adresu** prostředky v hello **se nezdařilo** stavu. Hello prostředek IP adresy nelze do online režimu protože hello clusteru je přiřazen hello stejné IP adres jako hello počítač sám sebe, a proto je duplicitní adresu.

2. Klikněte pravým tlačítkem na hello se nezdařilo **IP adresu** prostředek a pak klikněte na tlačítko **vlastnosti**.

   ![Vlastnosti clusteru](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. Vyberte **statickou IP adresu** a zadejte dostupnou adresu z podsítě, je-li hello systému SQL Server hello adresu textové pole. Potom klikněte na **OK**.
4. V hello **základní prostředky clusteru** části, klikněte pravým tlačítkem na název clusteru a klikněte na tlačítko **přepnout do režimu Online**. Potom počkejte, dokud jsou obě prostředky online. Pokud prostředek názvu clusteru hello přechodu do režimu online, se nový účet počítače AD aktualizuje hello serveru řadiče domény. Pomocí této AD účet toorun hello později služby skupiny dostupnosti v clusteru.

### <a name="addNode"></a>Přidat další toocluster systému SQL Server hello

Přidat hello jiných toohello cluster serveru SQL Server.

1. Ve stromu hello prohlížeče, klikněte pravým tlačítkem hello cluster a klikněte na tlačítko **přidat uzel**.

    ![Přidat toohello uzlu clusteru](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. V hello **Průvodce přidáním uzlu**, klikněte na tlačítko **Další**. V hello **zvolit servery** přidejte hello druhý Server SQL. Zadejte název serveru hello v **zadejte název serveru** a pak klikněte na **přidat**. Až budete hotovi, klikněte na tlačítko **Další**.

1. V hello **upozornění ověření** klikněte na tlačítko **ne** (v případě produkční měli byste provést testy pro ověření hello). Pak klikněte na **Další**.

8. V hello **potvrzení** stránky, pokud používáte prostory úložiště, zrušte hello zaškrtávací políčko s názvem bez přípony **přidejte všechna vhodná úložiště toohello cluster.**

   ![Přidání uzlu potvrzení](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   >Pokud používáte prostory úložiště a není zrušte zaškrtnutí políčka **přidejte všechna vhodná úložiště toohello cluster**, Windows hello virtuální disky odpojí během procesu clusteringu hello. V důsledku toho se v Správce disků nebo v Průzkumníku nezobrazí, dokud hello prostory úložiště jsou odebrány z hello clusteru a znovu připojit pomocí prostředí PowerShell. Prostory úložiště skupiny více disků ve fondech toostorage. Další informace najdete v tématu [prostory úložiště](https://technet.microsoft.com/library/hh831739).

1. Klikněte na **Další**.

1. Klikněte na **Dokončit**.

   Správce clusteru převzetí služeb při selhání ukazuje, že se nový uzel clusteru a seznamů v hello **uzly** kontejneru.

10. Odhlaste se z relace vzdálené plochy hello.

### <a name="add-a-cluster-quorum-file-share"></a>Přidejte sdílenou složku kvora clusteru

V tomto příkladu používá clusteru se systémem Windows hello toocreate sdílenou složku soubor kvorum clusteru. Tento kurz používá většina uzlů a sdílených sdílené složky kvora. Další informace najdete v tématu [Principy konfigurací kvora v clusteru s podporou převzetí služeb při selhání](http://technet.microsoft.com/library/cc731739.aspx).

1. Připojte toohello souboru sdílenou složku určující členský server s relaci vzdálené plochy.

1. Na **správce serveru**, klikněte na tlačítko **nástroje**. Otevřete **Správa počítače**.

1. Klikněte na tlačítko **sdílených složek**.

1. Klikněte pravým tlačítkem na **sdílené složky**a klikněte na tlačítko **novou sdílenou složku...** .

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Použití **Průvodce vytvořením sdílené složky** toocreate sdílenou složku.

1. Na **cesta ke složce**, klikněte na tlačítko **Procházet** a najít nebo vytvořit cestu pro sdílenou složku hello. Klikněte na **Další**.

1. V **název, popis a nastavení** ověřte název sdílené složky hello a cestu. Klikněte na **Další**.

1. Na **sdílené složky oprávnění** nastavit **přizpůsobit oprávnění**. Klikněte na tlačítko **vlastní...** .

1. Na **upravit oprávnění**, klikněte na tlačítko **přidat...** .

1. Zajistěte, aby hello účet používaný toocreate hello clusteru má plnou kontrolu.

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. Klikněte na **OK**.

1. V **sdílené složky oprávnění**, klikněte na tlačítko **Dokončit**. Klikněte na tlačítko **Dokončit** znovu.  

1. Odhlaste se ze serveru hello

### <a name="configure-cluster-quorum"></a>Konfigurace kvora clusteru

Dále nastavte kvorum clusteru hello.

1. Připojte toohello prvním uzlu clusteru pomocí vzdálené plochy.

1. V **Správce clusteru převzetí služeb při selhání**, klikněte pravým tlačítkem na hello clusteru, přejděte příliš**další akce**a klikněte na tlačítko **konfigurovat nastavení kvora clusteru...** .

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. V **Průvodce konfigurací kvora clusteru**, klikněte na tlačítko **Další**.

1. V **vybrat možnosti konfigurace kvora**, zvolte **vybrat určující disk kvora hello**a klikněte na tlačítko **Další**.

1. Na **vybrat určující disk kvora**, klikněte na tlačítko **nakonfigurovat určující sdílenou složku**.

   >[!TIP]
   >Windows Server 2016 podporuje určujícího cloudu. Pokud si zvolíte tento typ určující sdílené složky, není nutné soubor sdílet s kopií clusteru. Další informace najdete v tématu [nasazení cloudu určující pro Cluster s podporou převzetí služeb při selhání](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness). Tento kurz používá určující sdílenou složku souboru, který není podporován ve starších operačních systémech.

1. Na **nakonfigurovat určující sdílenou složku**, zadejte hello cestu pro sdílenou složku hello jste vytvořili. Klikněte na **Další**.

1. Ověřte nastavení hello na **potvrzení**. Klikněte na **Další**.

1. Klikněte na **Dokončit**.

základní prostředky clusteru Hello jsou nakonfigurovány s určující sdílenou složku.

## <a name="enable-availability-groups"></a>Povolte skupiny dostupnosti

Dál povolte hello **skupiny dostupnosti AlwaysOn** funkce. Proveďte tyto kroky na obou serverech SQL.

1. Z hello **spustit** obrazovky, spusťte **SQL Server Configuration Manager**.
2. Ve stromu hello prohlížeče, klikněte na tlačítko **služby SQL Server**, klikněte pravým tlačítkem hello **serveru SQL (MSSQLSERVER)** služby a klikněte na tlačítko **vlastnosti**.
3. Klikněte na tlačítko hello **vysoké dostupnosti AlwaysOn** a potom vyberte **povolte skupiny dostupnosti AlwaysOn**, a to takto:

    ![Povolte skupiny dostupnosti AlwaysOn](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. Klikněte na tlačítko **Použít**. Klikněte na tlačítko **OK** v místním dialogovém okně hello.

5. Restartujte službu SQL Server hello.

Opakujte tyto kroky na hello jiný Server SQL.

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for hello database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for hello instance of SQL Server that is used toosynchronize hello database replicas in hello Availability Groups on that instance.

On both SQL Servers, open hello firewall for hello TCP port for hello database mirroring endpoint.

1. On hello first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In hello left pane, select **Inbound Rules**. On hello right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For hello port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In hello **Action** page, keep **Allow hello connection** selected and click **Next**.
6. In hello **Profile** page, accept hello default settings and click **Next**.
7. In hello **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in hello **Name** text box, then click **Finish**.

Repeat these steps on hello second SQL Server.
-------------------------->

## <a name="create-a-database-on-hello-first-sql-server"></a>Vytvořit databázi na hello první systému SQL Server

1. Spuštění hello RDP souboru toohello prvního serveru SQL s doménou účet, který je členem skupiny sysadmin, pevné role serveru.
1. Otevřete aplikaci SQL Server Management Studio a připojte toohello první systému SQL Server.
7. V **Průzkumník objektů**, klikněte pravým tlačítkem na **databáze** a klikněte na tlačítko **novou databázi**.
8. V **název databáze**, typ **MyDB1**, pak klikněte na tlačítko **OK**.

### <a name="backupshare"></a>Vytvoří sdílenou složku zálohování

1. Na hello první SQL Server v **správce serveru**, klikněte na tlačítko **nástroje**. Otevřete **Správa počítače**.

1. Klikněte na tlačítko **sdílených složek**.

1. Klikněte pravým tlačítkem na **sdílené složky**a klikněte na tlačítko **novou sdílenou složku...** .

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Použití **Průvodce vytvořením sdílené složky** toocreate sdílenou složku.

1. Na **cesta ke složce**, klikněte na tlačítko **Procházet** a najít nebo vytvořit cestu pro sdílenou složku hello databáze zálohování. Klikněte na **Další**.

1. V **název, popis a nastavení** ověřte název sdílené složky hello a cestu. Klikněte na **Další**.

1. Na **sdílené složky oprávnění** nastavit **přizpůsobit oprávnění**. Klikněte na tlačítko **vlastní...** .

1. Na **upravit oprávnění**, klikněte na tlačítko **přidat...** .

1. Ujistěte se, že hello systému SQL Server a SQL Server Agent účty služby pro oba servery mít plnou kontrolu.

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. Klikněte na **OK**.

1. V **sdílené složky oprávnění**, klikněte na tlačítko **Dokončit**. Klikněte na tlačítko **Dokončit** znovu.  

### <a name="take-a-full-backup-of-hello-database"></a>Proveďte úplnou zálohu databáze hello

Je nutné tooback řetězem hello nové databáze tooinitialize hello protokolu. Pokud neprovedete zálohování hello nové databáze, nemůže být součástí skupiny dostupnosti.

1. V **Průzkumník objektů**, klikněte pravým tlačítkem na databázi hello, přejděte příliš**úlohy...** , klikněte na tlačítko **zálohování**.

1. Klikněte na tlačítko **OK** tootake umístění zálohy úplného zálohování toohello výchozí.

## <a name="create-hello-availability-group"></a>Vytvoření skupiny dostupnosti hello
Jste nyní připraven tooconfigure, které skupiny dostupnosti pomocí hello následující kroky:

* Vytvořit databázi na hello první systému SQL Server.
* Proveďte úplné zálohování a zálohování protokolu transakcí databáze hello
* Úplné obnovení hello a toohello zálohy protokolu druhé systému SQL Server s hello **NORECOVERY** možnost
* Vytvoření hello skupiny dostupnosti (**AG1**) se synchronním potvrzováním, automatické převzetí služeb při selhání a čitelných místních replikách

### <a name="create-hello-availability-group"></a>Vytvoření skupiny dostupnosti hello:

1. V relaci vzdálené plochy toohello první systému SQL Server. V **Průzkumník objektů** v aplikaci SSMS, klikněte pravým tlačítkem na **vysoké dostupnosti AlwaysOn** a klikněte na tlačítko **Průvodce novou skupinou dostupnosti**.

    ![Spusťte Průvodce novou skupinou dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. V hello **ÚVOD** klikněte na tlačítko **Další**. V hello **zadejte název skupiny dostupnosti** stránky, zadejte název hello skupiny dostupnosti, například **AG1**v **název skupiny dostupnosti**. Klikněte na **Další**.

    ![Průvodce novým AG, zadejte název skupiny AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. V hello **vyberte databáze** , vyberte svou databázi a klikněte na tlačítko **Další**.

   >[!NOTE]
   >databáze Hello splňuje požadavky hello pro skupinu dostupnosti, protože jste provedli aspoň jednu úplnou zálohu na hello určený primární repliky.

   ![Průvodce novým AG, vyberte možnost databáze](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. V hello **zadejte repliky** klikněte na tlačítko **přidat repliky**.

   ![Průvodce novým AG, zadejte repliky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. Hello **připojit tooServer** objeví dialogové okno. Název typu hello druhý server hello v **název serveru**. Klikněte na **Připojit**.

   Zpět v hello **zadejte repliky** stránky, měli byste vidět druhý server hello uvedené v **replik dostupnosti**. Konfigurace replik hello následujícím způsobem.

   ![Průvodce novým AG, zadejte repliky (dokončeno)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. Klikněte na tlačítko **koncové body** databáze hello toosee koncový bod zrcadlení pro tuto skupinu dostupnosti. Hello použijte stejný port, který jste použili při nastavíte hello [pravidlo brány firewall pro koncové body zrcadlení databáze](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

    ![Průvodce novým AG, vyberte Data počáteční synchronizace](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. V hello **vybrat počáteční synchronizaci dat** vyberte **úplné** a zadejte do sdíleného síťového umístění. Umístění hello použít hello [zálohování sdílené složky, kterou jste vytvořili](#backupshare). V příkladu hello byl **\\\\\<první systému SQL Server\>\Backup\**. Klikněte na **Další**.

   >[!NOTE]
   >Úplná synchronizace trvá úplnou zálohu databáze hello na hello první instance systému SQL Server a obnoví se druhá instance toohello. Pro velké databáze úplná synchronizace nedoporučuje, protože to může trvat dlouhou dobu. Tuto chvíli můžete snížit tak, že ručně zálohu databáze hello obnovení její `NO RECOVERY`. Pokud již obnovení databáze hello s `NO RECOVERY` na hello druhé před konfigurací hello skupiny dostupnosti systému SQL Server, vyberte **připojení pouze**. Pokud chcete zálohování hello tootake po dokončení konfigurace hello skupiny dostupnosti, vyberte **přeskočit počáteční data synchronizace**.

    ![Průvodce novým AG, vyberte Data počáteční synchronizace](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. V hello **ověření** klikněte na tlačítko **Další**. Tato stránka by měl vypadat podobně jako toohello následující bitové kopie:

    ![Průvodce novým AG, ověření](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    >Protože jste nenakonfigurovali naslouchací proces skupiny dostupnosti je upozornění pro konfiguraci naslouchacího procesu hello. Toto upozornění můžete ignorovat, protože na virtuálních počítačích Azure můžete vytvořit naslouchací proces hello po vytvoření pro vyrovnávání zatížení Azure hello.

10. V hello **Souhrn** klikněte na tlačítko **Dokončit**, potom počkejte, než Průvodce hello nakonfiguruje hello nové skupiny dostupnosti. V hello **průběh** stránky, můžete kliknout na **podrobnosti** tooview hello podrobný průběh. Po dokončení Průvodce hello zkontrolovat hello **výsledky** tooverify stránky, která hello skupiny dostupnosti je úspěšně vytvořen.

     ![Průvodce novým AG, výsledků](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. Klikněte na tlačítko **Zavřít** tooexit hello průvodce.

### <a name="check-hello-availability-group"></a>Zkontrolujte hello skupiny dostupnosti

1. V **Průzkumník objektů**, rozbalte položku **vysoké dostupnosti AlwaysOn**, pak rozbalte **skupiny dostupnosti**. Teď byste měli vidět text hello nové skupiny dostupnosti v tomto kontejneru. Klikněte pravým tlačítkem na hello skupiny dostupnosti a klikněte na tlačítko **zobrazit řídicí panel**.

   ![Řídicí panel zobrazit AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   Vaše **řídicí panel AlwaysOn** by měl vypadat podobně jako toothis.

   ![Řídicí panel AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   Můžete zobrazit hello repliky, hello režim převzetí služeb při selhání každý stav synchronizace repliky a hello.

2. V **Správce clusteru převzetí služeb při selhání**, klikněte na cluster. Vyberte **role**. Název skupiny dostupnosti Hello, kterého jste je role v clusteru hello. Této skupiny dostupnosti nemá IP adresu pro připojení klientů, protože jste nenakonfigurovali naslouchací proces. Po vytvoření pro vyrovnávání zatížení Azure nakonfigurujete hello naslouchací proces.

   ![AG ve Správci clusteru převzetí služeb při selhání](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > Nepokoušejte toofail přes hello skupiny dostupnosti z hello Správce clusteru převzetí služeb při selhání. Všechny operace převzetí služeb při selhání by měla provést uvnitř **řídicí panel AlwaysOn** v aplikaci SSMS. Další informace najdete v tématu [hello omezení pomocí Správce clusteru převzetí služeb při selhání se skupinami dostupnosti](https://msdn.microsoft.com/library/ff929171.aspx).
    >

Nyní máte skupinu dostupnosti s replikami na dvě instance systému SQL Server. Hello skupiny dostupnosti můžete přesouvat mezi instancemi. Toohello skupiny dostupnosti nelze ještě připojit, protože nemáte naslouchací proces. Naslouchací proces hello ve virtuálních počítačích Azure, vyžaduje nástroj pro vyrovnávání zatížení. dalším krokem Hello je toocreate hello Vyrovnávání zatížení v Azure.

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a>Vytvoření pro vyrovnávání zatížení Azure

Skupinu dostupnosti SQL Server na virtuálních počítačích Azure, vyžaduje nástroj pro vyrovnávání zatížení. Nástroj pro vyrovnávání zatížení Hello obsahuje hello IP adresu pro naslouchací proces skupiny dostupnosti hello. Tento oddíl shrnuje, jak toocreate hello pro vyrovnávání zátěže v hello portálu Azure.

1. V hello portálu Azure, přejděte toohello skupinu prostředků, kde jsou vaše servery SQL a klikněte na tlačítko **+ přidat**.
2. Vyhledejte **nástroj pro vyrovnávání zatížení**. Vyberte nástroj pro vyrovnávání zatížení hello publikovaný microsoftem.

   ![AG ve Správci clusteru převzetí služeb při selhání](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  Klikněte na možnost **Vytvořit**.
3. Nakonfigurujte následující parametry nástroje pro vyrovnávání zatížení hello hello.

   | Nastavení | Pole |
   | --- | --- |
   | **Název** |Použít název textu hello Vyrovnávání zatížení, například **sqlLB**. |
   | **Typ** |Interní |
   | **Virtuální síť** |Použijte název hello hello virtuální síť Azure. |
   | **Podsíť** |Použijte název hello hello podsítě, která hello virtuální počítač je v.  |
   | **Přiřazení IP adresy** |Statická |
   | **IP adresa** |Použijte dostupnou adresu z podsítě. |
   | **Předplatné** |Použití hello stejné předplatné jako hello virtuální počítač. |
   | **Umístění** |Použití hello stejné umístění jako virtuální počítač hello. |

   Hello Azure okno portálu by měl vypadat takto:

   ![Vytvořit nástroj pro vyrovnávání zatížení](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. Klikněte na tlačítko **vytvořit**, nástroj pro vyrovnávání zatížení toocreate hello.

Vyrovnávání zatížení tooconfigure hello potřebujete toocreate fond back-end, test a pravidel Vyrovnávání zatížení hello sady. Proveďte v hello portálu Azure.

### <a name="add-backend-pool"></a>Přidejte fond back-end

1. V hello portálu Azure přejděte tooyour skupiny dostupnosti. Může být nutné toorefresh hello zobrazení toosee hello nově vytvořený pro vyrovnávání zatížení.

   ![Najít nástroj pro vyrovnávání zatížení ve skupině prostředků](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. Klikněte na tlačítko hello nástroj pro vyrovnávání zatížení, klikněte na tlačítko **back-endové fondy**a klikněte na tlačítko **+ přidat**. Nastavte fond back-end hello následujícím způsobem:

   | Nastavení | Popis | Příklad
   | --- | --- |---
   | **Název** | Zadejte název text | SQLLBBE
   | **Přidružené k** | Vybrat ze seznamu | Skupina dostupnosti
   | **Sady dostupnosti.** | Použijte název sady dostupnosti hello, aby byly virtuální počítače serveru SQL v | sqlAvailabilitySet |
   | **Virtual Machines** |Hello dva názvy virtuální počítač Azure SQL Server | SQL Server-0, sqlserver 1

1. Zadejte název fondu back-end hello hello.

1. Klikněte na tlačítko **+ přidat virtuální počítač**.

1. Pro sadu dostupnosti hello vyberte tohoto hello, které jsou servery SQL Server ve skupině dostupnosti hello.

1. Pro virtuální počítače zahrnují oba hello servery SQL Server. Nezahrnujte určující server hello sdílené položky.

1. Klikněte na tlačítko **OK** toocreate hello back-endový fond.

### <a name="set-hello-probe"></a>Sada hello testu

1. Klikněte na tlačítko hello nástroj pro vyrovnávání zatížení, klikněte na tlačítko **testy stavu**a klikněte na tlačítko **+ přidat**.

1. Test stavu hello nastavte takto:

   | Nastavení | Popis | Příklad
   | --- | --- |---
   | **Název** | Text | SQLAlwaysOnEndPointProbe |
   | **Protokol** | Zvolte TCP | TCP |
   | **Port** | Všechny nepoužívaný port. | 59999 |
   | **Interval**  | Hello mezi testu pokusů v sekundách |5 |
   | **Prahová hodnota špatného stavu** | Hello počet po sobě jdoucích kontroly chyb, které musí dojít k toobe virtuální počítač považoval za poškozený  | 2 |

1. Klikněte na tlačítko **OK** test stavu tooset hello.

### <a name="set-hello-load-balancing-rules"></a>Nastavit pravidla Vyrovnávání zatížení hello

1. Klikněte na tlačítko hello nástroj pro vyrovnávání zatížení, klikněte na tlačítko **pravidla Vyrovnávání zatížení**a klikněte na tlačítko **+ přidat**.

1. Nastavit pravidla takto Vyrovnávání zatížení hello.
   | Nastavení | Popis | Příklad
   | --- | --- |---
   | **Název** | Text | SQLAlwaysOnEndPointListener |
   | **Adresa IP front-endu** | Zvolte adresu |Použijte hello adresu, kterou jste vytvořili při vytváření hello nástroj pro vyrovnávání zatížení. |
   | **Protokol** | Zvolte TCP |TCP |
   | **Port** | Použít hello port pro instanci systému SQL Server hello | 1433 |
   | **Back-endový Port** | Toto pole se nepoužívá při plovoucí IP adresa je nastavena pro přímý server návratový | 1433 |
   | **Test** |Zadaný u testu paměti hello název Hello | SQLAlwaysOnEndPointProbe |
   | **Trvalost relace** | Rozevírací seznam | **None** |
   | **Časový limit nečinnosti** | Otevřete minut tookeep připojení TCP | 4 |
   | **Plovoucí IP adresa (přímá odpověď ze serveru)** | |Povoleno |

   > [!WARNING]
   > Přímá odpověď ze serveru se nastavuje během vytváření. Nelze změnit.

1. Klikněte na tlačítko **OK** pravidla Vyrovnávání zatížení tooset hello.

## <a name="configure-listener"></a>Konfigurace naslouchací proces hello

Další toodo věc Hello je tooconfigure naslouchací proces skupiny dostupnosti v clusteru převzetí služeb při selhání hello.

> [!NOTE]
> Tento kurz ukazuje, jak toocreate jeden naslouchací proces - s jeden ILB IP adres. toocreate jeden nebo více naslouchací procesy pomocí jednoho nebo více IP adres, najdete v části [skupiny dostupnosti vytvořit naslouchací proces a nástroj pro vyrovnávání zatížení | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a>Port naslouchacího procesu sady

V systému SQL Server Management Studio nastavte hello port naslouchacího procesu.

1. Spusťte SQL Server Management Studio a připojte toohello primární repliky.

1. Přejděte příliš**vysoké dostupnosti AlwaysOn** | **skupiny dostupnosti** | **naslouchací procesy skupiny dostupnosti**.

1. Teď byste měli vidět název hello naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání. Klikněte pravým tlačítkem na název naslouchacího procesu hello a klikněte na tlačítko **vlastnosti**.

1. V hello **Port** zadejte číslo portu hello pro naslouchací proces skupiny dostupnosti hello pomocí hello $EndpointPort jste použili předtím (1433 byla výchozí hello), pak klikněte na tlačítko **OK**.

Nyní máte skupinu dostupnosti SQL Server na virtuálních počítačích Azure spuštěna v režimu Resource Manager.

## <a name="test-connection-toolistener"></a>Testovací připojení toolistener

tootest hello připojení:

1. RDP tooa systému SQL Server, který je v hello stejné virtuální síti však není vlastní hello repliky. Můžete použít jiný SQL Server v clusteru hello hello.

1. Použití **sqlcmd** nástroj tootest hello připojení. Například vytvoří hello následující skript **sqlcmd** připojení toohello primární repliky prostřednictvím hello naslouchací proces s ověřováním systému Windows:

    ```
    sqlcmd -S <listenerName> -E
    ```

    Pokud naslouchací proces hello používá port jiný než hello výchozí port (1433), zadejte hello port v hello připojovací řetězec. Například hello následujícího příkazu sqlcmd připojuje tooa naslouchání na portu 1435:

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Hello SQLCMD připojení automaticky připojí toowhichever instanci systému SQL Server hostitele hello primární repliky.

> [!TIP]
> Ujistěte se, že je otevřen v bráně firewall hello obou serverů SQL hello port, který zadáte. Oba servery vyžadují příchozí pravidlo pro hello port TCP, který používáte. Další informace najdete v tématu [přidat nebo upravit pravidlo brány Firewall](http://technet.microsoft.com/library/cc753558.aspx).
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by hello way” info, an Important is info users need toocomplete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is hello second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note hello format for documenting hello UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult toomaintain. Highlight areas you are referring tooin red.*-->

<!--**No. of steps**: *Make sure hello number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want toogo on.*-->

## <a name="next-steps"></a>Další kroky

- [Přidat k IP adresu tooa pro vyrovnávání zatížení pro skupinu dostupnosti druhý](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).
