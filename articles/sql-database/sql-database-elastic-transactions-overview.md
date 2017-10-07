---
title: "aaaDistributed transakce napříč databázemi cloudu"
description: "Přehled transakcí elastické databáze s databází Azure SQL"
services: sql-database
documentationcenter: 
author: torsteng
manager: jhubbard
editor: torsteng
ms.assetid: e14df7a3-7788-4cfb-bcd1-7ad6433ef1f9
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: sql-database
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 9a18f2749108d337bf115b3dc21dd0c7828dd9c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="distributed-transactions-across-cloud-databases"></a>Distribuované transakce v cloudových databázích
Transakcí elastické databáze pro Azure SQL Database (databáze SQL) umožňují toorun transakcí, které jsou rozmístěny v několika databází v databázi SQL. Jsou k dispozici pro aplikace .NET pomocí rozhraní ADO .NET transakcí elastické databáze pro databázi SQL a integrovat hello známé programovací prostředí pomocí hello [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) třídy. tooget hello knihovně, najdete v části [rozhraní .NET Framework 4.6.1 (Webová instalační služba)](https://www.microsoft.com/download/details.aspx?id=49981).

Místní takové situaci většinou vyžaduje systémem Microsoft Distributed Transaction Coordinator služba MSDTC (). Vzhledem k tomu, že služby MS DTC není k dispozici pro aplikaci platforma jako služba v Azure, hello možnost toocoordinate distribuované transakce nyní přímo integrovány do databáze SQL. Aplikace se můžou připojit tooany SQL Database toolaunch distribuovaných transakcí a jedné z databází hello se transparentně koordinaci hello distribuované transakce, jak ukazuje následující obrázek hello. 

  ![Distribuovaná transakce s Azure SQL Database pomocí transakcí elastické databáze ][1]

## <a name="common-scenarios"></a>Obvyklé scénáře
Transakcí elastické databáze pro databázi SQL povolte aplikace toomake atomic změny toodata uložené v několika různých databází SQL. Hello preview se zaměřuje na straně klienta vývoj prostředí v C# a rozhraní .NET. Na straně serveru zkušenosti s používáním T-SQL je naplánován na pozdější dobu.  
Cíle hello transakcí elastické databáze následující scénáře:

* Více databázových aplikací v Azure: Tento scénář je dat svisle rozděleného mezi několika databází v databázi SQL tak, že různé druhy dat jsou umístěné na různých databází. Některé operace vyžadují změny toodata, který je uložen ve dvou nebo více databází. aplikace Hello používá změny hello toocoordinate transakcí elastické databáze mezi databázemi a zkontrolujte nedělitelnost.
* Horizontálně dělené databázové aplikace v Azure: Tento scénář hello Datová vrstva používá hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md) nebo samoobslužné-horizontálního dělení toohorizontally oddílu hello data mezi mnoha databázemi v databázi SQL. Jeden případ použití viditelného je hello nutné tooperform atomic změny pro horizontálně dělenou víceklientské aplikace při změny span klientů. Představte si například přenos z jednoho klienta tooanother, oba umístěný na různých databází. Druhém případě je podrobných horizontálního dělení tooaccommodate požadavků na kapacitu pro velké klienta, který naopak obvykle znamená, že některé toostretch potřebám atomické operací napříč několika databází použitých pro hello, stejné klienta. Třetí případem je atomic aktualizace tooreference data, která se replikují mezi databázemi. Operace Atomic, zpracovaných, podle těchto zásad můžete nyní koordinované napříč několika databází pomocí hello preview.
  Transakcí elastické databáze pomocí dvoufázový zápis tooensure transakce nedělitelnost mezi databázemi. Je vhodné pro transakce zahrnující méně než 100 databáze v době v rámci jedné transakce. Tato omezení nejsou vynucena, ale jeden by měl očekávat výkon a úspěšnost pro toosuffer transakcí elastické databáze při překročení těchto mezních hodnot.

## <a name="installation-and-migration"></a>Instalace a migrace
prostřednictvím aktualizace knihovny .NET toohello System.Data.dll a System.Transactions.dll dostupné Hello funkce pro elastické databáze transakce v databázi SQL. Hello knihovny DLL Ujistěte se, že dvojfázového zápisu se používá potřeby tooensure nedělitelnost. vývoj aplikací toostart použití transakcí elastické databáze, nainstalujte [rozhraní .NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) nebo novější. Při spuštění na dřívější verzi rozhraní .NET framework hello, transakce se nepodaří toopromote tooa distribuované transakce a k výjimce.

Po instalaci můžete hello distribuované transakce rozhraní API v System.Transactions – s tooSQL připojení databáze. Pokud máte existující služby MSDTC aplikací pomocí těchto rozhraní API, po instalaci hello 4.6.1 jednoduše Sestavit vaše stávající aplikace pro 4.6 rozhraní .NET Framework. Pokud projekty cílí na .NET 4.6, automaticky použijí hello aktualizovat knihovny DLL z nové verze Framework hello a distribuované transakce volání rozhraní API v kombinaci s tooSQL připojení, které teď bude úspěšné DB.

Mějte na paměti, že transakcí elastické databáze nevyžadují instalaci služby MSDTC. Místo toho transakcí elastické databáze jsou spravovány přímo a v databázi SQL. To výrazně zjednodušuje scénářích cloudu, protože nasazení služby MS DTC není nutné toouse distribuované transakce s databázi SQL. Oddíl 4 podrobněji vysvětluje, jak toodeploy transakcí elastické databáze a hello vyžaduje rozhraní .NET framework společně s tooAzure vaší cloudové aplikace.

## <a name="development-experience"></a>Vývojové prostředí
### <a name="multi-database-applications"></a>Více databázové aplikace
Hello následující vzorový kód používá programovací prostředí hello obeznámeni s System.Transactions – rozhraní .NET. Hello TransactionScope třída vytváří ambientní transakce v rozhraní .NET. ("Vedlejším transakcí" je ten, který je umístěn v aktuální vlákno hello.) Všechna připojení otevřené v rámci hello TransactionScope účastnit hello transakce. Jestliže se účastní různých databází, hello transakce je automaticky zvýšenými tooa distribuované transakce. výsledek Hello hello transakce je řízena nastavením hello oboru toocomplete tooindicate potvrzení.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Horizontálně dělené databázové aplikace
Transakcí elastické databáze pro databázi SQL také podporují koordinace distribuované transakce, kde použijete metodu OpenConnectionForKey hello hello elastické databáze klienta knihovny tooopen připojení pro škálovaný datové vrstvy. Vezměte v úvahu případech, kde je nutné tooguarantee transakční konzistence na změny napříč několika různých horizontálního dělení hodnoty klíče. Připojení toohello horizontálních oddílů hostování hello různých horizontálního dělení klíčové hodnoty jsou zprostředkované pomocí OpenConnectionForKey. V případě obecné hello hello připojení může být horizontálních oddílů toodifferent tak, že zajistíte transakční záruky vyžaduje distribuované transakce. Následující ukázka kódu Hello ukazuje tento přístup. Se předpokládá, že se proměnné s názvem shardmap použité toorepresent horizontálního oddílu namapujte hello klientské knihovny pro elastické databáze:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>Instalace rozhraní .NET pro Azure Cloud Services
Azure poskytuje několik nabídek toohost aplikací .NET. Porovnání hello různé nabídky jsou k dispozici v [porovnání služby Azure App Service, cloudové služby a virtuální počítače](../app-service-web/choose-web-site-cloud-service-vm.md). Pokud hello hostovaný operační systém nabídky hello je menší než .NET 4.6.1 vyžaduje pro elastické transakce, je třeba tooupgrade hello hostovaného operačního systému too4.6.1. 

Aplikační služby Azure provede upgrade hostované toohello OS nejsou aktuálně podporovány. Pro virtuální počítače Azure, jednoduše přihlaste hello virtuální počítač a spusťte instalační program hello hello nejnovější rozhraní .NET framework. Pro Azure Cloud Services budete potřebovat tooinclude hello instalaci novější verze rozhraní .NET do hello spuštění úlohy nasazení. Koncepty Hello a kroky popsané v [nainstalovat rozhraní .NET v roli služby Cloud](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Všimněte si, že hello instalační program pro .NET 4.6.1 může vyžadovat další dočasné úložiště během zavádění procesu v cloudových služeb Azure, než instalační program pro .NET 4.6 hello hello. tooensure úspěšnou instalaci, musíte tooincrease dočasné úložiště pro cloudové služby Azure v souboru ServiceDefinition.csdef v části LocalResources hello a nastavení prostředí hello spuštění úkolu, jak je znázorněno v následující hello Ukázka:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>

## <a name="transactions-across-multiple-servers"></a>Transakce na více serverech
Transakce elastické databáze jsou podporovány na různé servery logické ve službě Azure SQL Database. Pokud transakce kříží hranice logického serveru, nejprve zúčastněných serverů hello toobe vloženy do vztahu vzájemné komunikaci. Po vytvoření relace hello komunikace, všechny databáze v některém z hello dva servery mohl účastnit elastické transakcí s databází z hello jiný server. S transakcemi pokrývání uzlů více než dva logické servery vztahu komunikace pro jakýkoli pár logické servery potřebuje toobe na místě.

Použijte hello následující rutiny prostředí PowerShell toomanage komunikaci mezi servery vztahy transakcí elastické databáze:

* **Nové AzureRmSqlServerCommunicationLink**: pomocí této rutiny toocreate nový vztah komunikaci mezi dvěma logické servery v databázi SQL Azure. Hello relace je symetrický, což znamená, že oba servery můžete zahájit transakce s hello jiný server.
* **Get-AzureRmSqlServerCommunicationLink**: pomocí této rutiny tooretrieve stávající komunikace vztahy a jejich vlastnosti.
* **Odebrat AzureRmSqlServerCommunicationLink**: pomocí této rutiny tooremove existující relaci komunikace. 

## <a name="monitoring-transaction-status"></a>Monitorování stavu transakce
Pomocí zobrazení dynamické správy (zobrazení dynamické správy) v databázi SQL toomonitor stav a průběh transakcí probíhající elastické databáze. Všechny související tootransactions zobrazení dynamické správy jsou relevantní pro distribuované transakce v databázi SQL. Můžete najít hello odpovídající seznam zobrazení dynamické správy zde: [transakce související dynamické správy zobrazení a funkce (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Tato zobrazení dynamické správy jsou obzvláště užitečná:

* **Sys.DM\_tran\_active\_transakce**: uvádí aktuálně aktivních transakcí a jejich stav. Hello sloupec UOW (jednotka práce) lze identifikovat hello jiné podřízené transakcí, které patří toohello stejné distribuované transakce. Všechny transakce v rámci stejné distribuované transakce provádění hello hello stejnou hodnotu parametru UOW. V tématu hello [DMV dokumentaci](https://msdn.microsoft.com/library/ms174302.aspx) další podrobnosti.
* **Sys.DM\_tran\_databáze\_transakce**: poskytuje další informace o transakcích, jako je například umístění hello transakce v protokolu hello. V tématu hello [DMV dokumentaci](https://msdn.microsoft.com/library/ms186957.aspx) další podrobnosti.
* **Sys.DM\_tran\_zámky**: poskytuje informace o hello zámky, které jsou aktuálně uložené probíhající transakce. V tématu hello [DMV dokumentaci](https://msdn.microsoft.com/library/ms190345.aspx) další podrobnosti.

## <a name="limitations"></a>Omezení
Hello aktuálně následující omezení platí tooelastic databázové transakce v databázi SQL:

* Jsou podporovány pouze transakce napříč databázemi v databázi SQL. Další [X / otevřete XA](https://en.wikipedia.org/wiki/X/Open_XA) zprostředkovatelé prostředků a databáze mimo SQL DB nemůže se podílet na transakcí elastické databáze. To znamená, že transakcí elastické databáze nelze roztáhnout na místní systém SQL Server a databáze SQL Azure. Pro distribuované transakce místně pokračujte toouse služba MSDTC. 
* Jsou podporovány pouze klienta koordinované transakce z aplikace .NET. Podpora serverové T-SQL, jako je například začít DISTRIBUOVANÝCH transakcí je plánované, ale ještě není k dispozici. 
* Transakce ve službách WCF nejsou podporovány. Například máte metodu služby WCF, které provádí transakce. Jako uzavření hello volání v oboru transakce se nezdaří [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="next-steps"></a>Další kroky
Máte dotazy, kontaktujte toous na hello [SQL Database fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a pro žádosti o funkce, přidejte je toohello [fóru pro zpětnou vazbu SQL Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



