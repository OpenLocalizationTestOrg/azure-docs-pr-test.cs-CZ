---
title: aaaSQL Azure s Azure Remoteappem | Microsoft Docs
description: "Zjistěte, jak toouse SQL Azure s Azure Remoteappem."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
editor: 
ms.assetid: 35f81d75-bfd7-4980-807e-00339f2cb2a4
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: fec4cb1f1ab3cde03b6ff613650e01bae3552824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-azure-with-azure-remoteapp"></a>SQL Azure s Azure RemoteAppem
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Často, když se zákazníci rozhodnou toohost své aplikace systému Windows v hello cloudu s Azure Remoteappem chtějí také toomigrate svá data, například SQL Server do hello cloudu celý cloudu nasazení. Tímto způsobem lze vytvářet řešení hostovaná čistě v cloudu, ke kterým lze kdykoliv a kdekoliv přistupovat pomocí Azure RemoteAppu. Dole jsou odkazy a odkazuje na společně s pokyny toohelp jste s tímto procesem.  

## <a name="migrate-your-sql-data"></a>Migrace dat SQL
Začněte s [migrace tooAzure databáze systému SQL Server databáze SQL](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Konfigurace Azure RemoteAppu
Aplikaci pro Windows hostujte v Azure RemoteAppu. Zde jsou základní kroky:

1. Vytvoření hello [virtuální počítač podle šablony Azure RemoteApp](remoteapp-imageoptions.md). 
2. Hello požadované aplikace nainstalujte na hello virtuálních počítačů.
3. Nakonfigurujte aplikace hello tak, aby se připojí toohello databázi SQL a potvrďte, že funguje.
4. Nástroj Sysprep a vypnutí hello virtuálních počítačů. Výsledek zaznamenejte jako image k použití s Azurem. **Poznámka:** budete potřebovat tooensure, hello aplikace je možné tooretain informace o připojení k hello DB prostřednictvím procesu sysprep hello. Pokud je aplikace hello informace o připojení nelze tooretain hello DB, můžete tooengage hello dodavatele toocheck aplikace hello jak lze zadat hello připojovací řetězec.
5. Importujte vlastní image hello do knihovny Azure Remoteappu a výběr hello správné zeměpisné umístění svého nasazení SQL Azure nachází. 
6. Nasaďte kolekci Remoteappu ve hello stejná data centru jako své nasazení SQL Azure pomocí hello výše šablony a publikování aplikace hello. Nasazení Azure RemoteApp v hello stejném datovém centru jako své nasazení SQL Azure pomáhá zajistit hello nejrychlejší připojení a snížení latence. 

## <a name="app-and-sql-configuration-considerations"></a>Požadavky na konfiguraci aplikace a SQL:
Při používání Azure SQL s Remoteappem se tooconsider několik bodů:

Další informace [jak tooconfigure Azure SQL databáze brány firewall](../sql-database/sql-database-firewall-configure.md). Výňatek ze článku uvádí hello: "zpočátku všechny server Azure SQL Database tooyour přístup hello brána firewall zablokovala. V pořadí toobegin pomocí serveru Azure SQL Database musíte přejděte toohello portálu Classic a zadat jedno nebo více pravidel brány firewall na úrovni serveru, které umožňují přístup k serveru Azure SQL Database tooyour. Použití toospecify pravidla brány firewall hello rozsahy IP adresu, která z hello Internetu jsou povolené a zda aplikací Azure se mohou pokusit serveru Azure SQL Database tooyour tooconnect."

Také, když se počítač pokusí tooconnect tooyour databázového serveru z hello Internetu, brána firewall hello zkontroluje, hello pocházející IP adresu požadavku hello vůči hello úplnou sadu úrovni serveru a (v případě potřeby) pravidla brány firewall na úrovni databáze. "Pokud hello IP adresu požadavku hello je do jedné z hello rozsahů určených v hello pravidla brány firewall na úrovni serveru, připojení hello je uděleno tooyour serveru Azure SQL Database." Lze tedy používat rozsahy IP adres, a není nutné používat pouze jednotlivé zdrojové IP adresy.

Postupujte podle pokynů hello krok za krokem v [postupy: Konfigurace nastavení brány firewall pro službu SQL Database pomocí portálu Azure hello](../sql-database/sql-database-configure-firewall-settings.md) rozsah IP adres toospecify hello. Při konfiguraci pravidel brány Firewall SQL hello, zadejte rozsah IP hello hello podsítě, která je zadána pro hello kolekci Azure Remoteappu. To by měla umožnit serverům ARA hello tooconnect toohello SQL DB i v případě, že budou mít dynamicky přidělované IP adresy.

## <a name="troubleshooting"></a>Řešení potíží
Pokud dojde hello použití klientská aplikace hostovaná v Azure Remoteappu, která se připojuje tooa SQL databáze, kde jsou hostované v Azure nebo místní je pomalu může dojít z několika důvodů proč.  

* Latence sítě z tooAzure vaše zařízení je vysoká. Přesuňte toohello nejlepší a nejrychlejší síťové připojení, může pro nejlepší výkon. Použití [azurespeed.com](http://azurespeed.com/) jako obecný nástroj tootest vaše zařízení latence tooAzure datového centra.  
* Klientská aplikace hostovaná v Azure RemoteAppu je vytížená. Vyberte jiný fakturační plán, například Premium, který umožní zvýšení výkonu. Další možností je toomonitor hello prostředky, vaše aplikace využívala: během aktivní relaci provádět posloupnost kláves ctrl + alt + end, který se spustí hello SAS obrazovky vyberte Správce úloh a sledovat využití prostředků pro vaši aplikaci.
* Server SQL je vytížený nebo není optimalizovaný. Postupujte pokynů k odstraňování potíží se serverem SQL. 

