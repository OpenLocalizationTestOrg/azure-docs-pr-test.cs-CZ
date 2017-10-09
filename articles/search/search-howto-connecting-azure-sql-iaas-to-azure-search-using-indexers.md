---
title: "aaaSQL virtuálního počítače připojení tooAzure vyhledávání | Microsoft Docs"
description: "Povolit šifrované připojení a nakonfigurujte hello brány firewall tooallow připojení tooSQL Server na virtuální počítač Azure (VM) z indexer na Azure Search."
services: search
documentationcenter: 
author: HeidiSteen
manager: pablocas
editor: 
ms.assetid: 46e42e0e-c8de-4fec-b11a-ed132db7e7bc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: heidist
ms.openlocfilehash: 1f0db8a2812b0a7d012e58bb873c4b2b29fa1338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-connection-from-an-azure-search-indexer-toosql-server-on-an-azure-vm"></a>Konfigurujte připojení z tooSQL indexer Azure Search Server na virtuálním počítači Azure
Jak jsme uvedli v [tooAzure připojení Azure SQL Database vyhledávání pomocí indexery](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq), vytváření indexery proti **systému SQL Server na virtuálních počítačích Azure** (nebo **virtuálních počítačích SQL Azure** pro zkrácení) je podporuje Azure Search, ale existuje několik předpokladů souvisejících s zabezpečení tootake péče o první. 

**Doba trvání úkolu:** asi 30 minut za předpokladu, že jste již nainstalovali certifikát na hello virtuálních počítačů.

## <a name="enable-encrypted-connections"></a>Povolit šifrované připojení
Vyhledávání systému Azure vyžaduje šifrovaný kanál pro všechny požadavky indexer přes veřejný připojení k Internetu. Tato část obsahuje kroky toomake hello činnost.

1. Zkontrolujte vlastnosti hello název subjektu hello certifikátu tooverify hello hello plně kvalifikovaný název domény (FQDN) hello virtuálního počítače Azure. Můžete použít nástroje, jako je CertUtils nebo hello vlastnosti hello tooview modul snap-in Certifikáty. Hello plně kvalifikovaný název domény můžete získat z části Essentials hello virtuálních počítačů služby okno na v hello **veřejných IP adres a DNS název popisku** pole v hello [portál Azure](https://portal.azure.com/).
   
   * Pro virtuální počítače vytvořené pomocí hello novější **Resource Manager** šablony, hello plně kvalifikovaný název domény je formátováno jako `<your-VM-name>.<region>.cloudapp.azure.com`. 
   * Pro starší virtuální počítače vytvořené jako **Classic** virtuálních počítačů, hello plně kvalifikovaný název domény je formátováno jako `<your-cloud-service-name.cloudapp.net>`. 
2. Konfigurace systému SQL Server toouse hello certifikát pomocí hello Editor registru (regedit). 
   
    I když Správce konfigurace systému SQL Server se často používá pro tuto úlohu, nelze ho použít pro tento scénář. Nenajde, že hello importovat certifikát, protože hello plně kvalifikovaný název domény hello virtuálního počítače na platformě Azure se neshoduje se hello plně kvalifikovaný název domény, počítáno od hello virtuálních počítačů (označuje hello domény jako hello místního počítače nebo hello síťové domény toowhich, který je připojen). Když se názvy neshodují, používejte regedit toospecify hello certifikát.
   
   * V editoru registru, vyhledejte klíč registru toothis: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
     Hello `[MSSQL13.MSSQLSERVER]` částí se liší podle verze a instance název. 
   * Nastavte hodnotu hello hello **certifikát** klíče toohello **kryptografický otisk** certifikátu protokolu SSL hello jste naimportovali toohello virtuálních počítačů.
     
     Existuje několik způsobů tooget hello kryptografický otisk, některé lépe než jiné. Při kopírování z hello **certifikáty** modul snap-in konzoly MMC, budete pravděpodobně vyzvedne, až bude neviditelná úvodní znak [jak je popsáno v tomto článku podpory](https://support.microsoft.com/kb/2023869/), což vede k chybě při pokusu o připojení. Existuje několik alternativní řešení pro odstranění tohoto problému. Hello nejjednodušší je toobackspace nad a znovu zadejte hello první znak hello kryptografický otisk tooremove hello úvodní znak v poli hodnota klíče hello v editoru registru. Alternativně můžete použít kryptografickým otiskem jiný nástroj toocopy hello.
3. Udělte oprávnění účtu služby toohello. 
   
    Zkontrolujte, zda text hello účet služby SQL Server je udělena příslušná oprávnění na hello privátní klíč certifikátu protokolu SSL hello. Pokud jste přehlédnout, tento krok, nebude spustit systém SQL Server. Můžete použít hello **certifikáty** modul snap-in nebo **CertUtils** pro tuto úlohu.
4. Restartujte službu SQL Server hello.

## <a name="configure-sql-server-connectivity-in-hello-vm"></a>Konfigurace připojení k SQL serveru v hello virtuálních počítačů
Po nastavení hello šifrované připojení vyžaduje Azure Search, existují další konfigurační kroky vnitřní tooSQL serveru na virtuálních počítačích Azure. Pokud jste tak ještě neučinili, hello dalším krokem je konfigurace toofinish pomocí kterékoli z těchto článků:

* Pro **Resource Manager** virtuálních počítačů, najdete v části [připojit tooa virtuálního počítače systému SQL Server v Azure pomocí Správce prostředků](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md). 
* Pro **Classic** virtuálních počítačů, najdete v části [připojit tooa virtuálního počítače systému SQL Server na Azure Classic](../virtual-machines/windows/classic/sql-connect.md).

Konkrétně, projděte si část hello v jednotlivých článků pro "internet připojující se přes hello".

## <a name="configure-hello-network-security-group-nsg"></a>Konfigurace hello skupina zabezpečení sítě (NSG)
Není tooconfigure hello NSG a odpovídající koncového bodu Azure nebo seznamu řízení přístupu (ACL) toomake vaší strany přístupné tooother virtuálního počítače Azure. Pravděpodobné, že kroky dokončíte před tooallow vlastní aplikace logiky tooconnect tooyour virtuální počítač SQL Azure. Je nejsou jiné u Azure Search připojení tooyour virtuální počítač SQL Azure. 

Hello odkazy níže poskytují pokyny NSG konfigurace pro nasazení virtuálních počítačů. Použijte tyto pokyny že tooacl koncový bod Azure SEarch na základě jeho IP adresy.

> [!NOTE]
> Pro informace viz [co je skupina zabezpečení sítě?](../virtual-network/virtual-networks-nsg.md)
> 
> 

* Pro **Resource Manager** virtuálních počítačů, najdete v části [jak toocreate skupiny Nsg pro nasazení ARM](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 
* Pro **Classic** virtuálních počítačů, najdete v části [jak toocreate skupiny Nsg pro nasazení Classic](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP adresy může představovat několik výzvy, které jsou snadno překonat, pokud jste si vědomi hello problému a potenciální řešení. zbývající části Hello poskytuje doporučení pro zpracování problémy související tooIP adresy v hello seznamu ACL.

#### <a name="restrict-access-toohello-search-service-ip-address"></a>Omezit přístup toohello vyhledávání služby IP adresu
Důrazně doporučujeme omezit hello přístup toohello IP adresu služby search v hello seznamu ACL místo provedení virtuální počítače Azure SQL celý otevřete tooany žádosti o připojení. Můžete snadno zjistit hello IP adresu pomocí příkazu ping hello plně kvalifikovaný název domény (například `<your-search-service-name>.search.windows.net`) služby search.

#### <a name="managing-ip-address-fluctuations"></a>Správa IP adres kolísání
Pokud vaši službu vyhledávání má jenom jednu jednotku vyhledávání (to znamená, jednu repliku a jeden oddíl), hello IP adresa se změní v průběhu běžné služby restartování zneplatnění existující ACL s IP adresou vaši službu vyhledávání.

Jedním ze způsobů tooavoid hello následné připojení chyba je jedna replika a toouse víc než jeden oddíl ve službě Azure Search. Tím se zvyšuje náklady na hello, ale také řeší problém adresu IP hello. Ve službě Azure Search neměnit IP adresy, když máte více než jednu jednotku vyhledávání.

Druhý přístup je tooallow hello připojení toofail a překonfigurujte hello seznamy ACL v hello NSG. V průměru můžete očekávat, že IP adresy toochange každých několik týdnů. Pro zákazníky, kteří řízené indexu na základě jen zřídka může být vhodným tento přístup.

Třetí přístup přijatelná (ale zvlášť zabezpečené) je toospecify hello rozsah IP adres hello oblast Azure, kde je zřízený vaši službu vyhledávání. Hello seznam rozsahů IP adres, ze kterých se veřejné IP adresy přidělené prostředky tooAzure je publikován v [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-hello-azure-search-portal-ip-addresses"></a>Zahrnout hello Azure Search portálu IP adresy
Pokud používáte hello Azure portálu toocreate indexer, logiky na portálu Azure Search také musí přístup tooyour virtuální počítač SQL Azure při vytváření. IP adresy portálu Azure search najdete otestováním pomocí `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Další kroky
S konfigurací mimo hello způsob nyní můžete určit systému SQL Server na virtuálním počítači Azure jako hello zdroj dat pro indexer Azure Search. V tématu [tooAzure připojení Azure SQL Database vyhledávání pomocí indexery](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) Další informace.

