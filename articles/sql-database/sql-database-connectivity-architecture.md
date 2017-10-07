---
title: "aaaAzure architektura připojení k SQL Database | Microsoft Docs"
description: "Tento dokument popisuje hello Azure SQLDB připojení architektura z Azure nebo z mimo Azure."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/05/2017
ms.author: carlrab
ms.openlocfilehash: 917df6d88a16f1b841b617fb2a53025b4d14d034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-connectivity-architecture"></a>Architektura připojení k databázi Azure SQL 

Tento článek popisuje architekturu připojení k databázi SQL Azure hello a vysvětluje, jak funkce různých komponent hello toodirect provoz tooyour instance databáze SQL Azure. Tyto součásti připojení k databázi SQL Azure fungovat toodirect síťový provoz toohello databáze Azure s klienty připojení v rámci Azure a klienti připojení z mimo Azure. Tento článek také obsahuje skript ukázky toochange jak dojde k připojení a hello aspekty související nastavení připojení k výchozí toochanging hello. Pokud po přečtení tohoto článku existují nějaké dotazy, obraťte se na Dhruv v dmalik@microsoft.com. 

## <a name="connectivity-architecture"></a>Architektura připojení

Následující diagram Hello poskytuje podrobný přehled hello architektura připojení k databázi SQL Azure. 

![Přehled architektury](./media/sql-database-connectivity-architecture/architecture-overview.png)


Hello následující kroky popisují, jak připojení je navázáno tooan Azure SQL database prostřednictvím hello Azure SQL Database softwaru Vyrovnávání zatížení (SLB) a brány Azure SQL Database hello.

- Klienti v rámci Azure nebo mimo Azure připojit toohello SLB, která má veřejnou IP adresu a naslouchá na portu 1433.
- Hello SLB směrovat provoz toohello Azure SQL Database brány.
- Hello brány přesměruje hello provoz toohello správné proxy middleware.
- Hello proxy middleware přesměruje hello provoz toohello odpovídající Azure SQL database.

> [!IMPORTANT]
> Každou z těchto součástí je distribuovat útok na dostupnost služby (Denial) ochrany integrované v síti hello a vrstva aplikací hello.
>

## <a name="connectivity-from-within-azure"></a>Připojení z v rámci Azure

Pokud se připojujete ze v rámci Azure, vaše připojení mají zásady připojení **přesměrování** ve výchozím nastavení. Zásady **přesměrování** znamená, že připojení po relace TCP hello je navázáno toohello Azure SQL database, relaci klienta hello se pak přesměruje toohello proxy middlewaru s změnu toohello cílové virtuální IP adresy z který toothat brány Azure SQL Database hello hello proxy middlewaru. Následně všechny následující pakety toku přímo prostřednictvím middlewaru hello proxy, obcházení brány Azure SQL Database hello. Hello následující diagram znázorňuje tento tok přenosů.

![Přehled architektury](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Připojení z mimo Azure

Pokud se připojujete ze mimo Azure, vaše připojení mají zásady připojení **Proxy** ve výchozím nastavení. Zásady **Proxy** hello znamená, že je vytvořena relace TCP hello prostřednictvím brány Azure SQL Database hello se všechny následující pakety toku prostřednictvím brány. Hello následující diagram znázorňuje tento tok přenosů.

![Přehled architektury](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL Database brány IP adresy

databáze Azure SQL tooconnect tooan z místních prostředků, musíte tooallow odchozí síťový provoz toohello Azure SQL Database brány pro vaši oblast Azure. Připojení, jdou pouze prostřednictvím brány hello při připojení v režimu proxy serveru, který je výchozí hello při připojení z místních prostředků.

Následující tabulka seznamy Hello hello primární a sekundární IP adresy brány Azure SQL Database hello pro všechny datové oblasti. Pro některé oblasti jsou dvě IP adresy. V těchto oblastech hello primární IP adresa je hello aktuální IP adresu brány hello a druhou IP adresu hello je IP adresa převzetí služeb při selhání. Adresa Hello převzetí služeb při selhání je toowhich adresu hello jsme může váš server tookeep hello dostupnost služby vysoké přesunout. Pro tyto oblasti doporučujeme povolit odchozí tooboth hello IP adresy. Hello druhou IP adresu je vlastnictví společnosti Microsoft a nepřijímá požadavky na žádné služby, dokud je aktivován Azure SQL Database tooaccept připojení.

| Název oblasti | Primární adresa IP | Sekundární adresa IP |
| --- | --- |--- |
| Austrálie – východ | 191.238.66.109 | 13.75.149.87 |
| Austrálie – jihovýchod | 191.239.192.109 | 13.73.109.251 |
| Brazílie – jih | 104.41.11.5 | |    
| Střední Kanada | 40.85.224.249 | |    
| Východní Kanada | 40.86.226.166 | |
| Střed USA | 23.99.160.139 | 13.67.215.62 |
| Východní Asie | 191.234.2.139 | 52.175.33.150 |
| Východní USA 1 | 191.238.6.43 | 40.121.158.30 |
| Východní USA 2 | 191.239.224.107 | 40.79.84.180 |
| Indie – střed | 104.211.96.159  | |   
| Indie – jih | 104.211.224.146  | |
| Indie – západ | 104.211.160.80 | |
| Japonsko – východ | 191.237.240.43 | 13.78.61.196 |
| Japonsko – západ | 191.238.68.11 | 104.214.148.156 |
| Korea – střed | 52.231.32.42 | |
| Korea – jih | 52.231.200.86 |  |
| Střed USA – sever | 23.98.55.75 | 23.96.178.199 |
| Severní Evropa | 191.235.193.75 | 40.113.93.91 |
| Střed USA – jih | 23.98.162.75 | 13.66.62.124 |
| Jihovýchodní Asie | 23.100.117.95 | 104.43.15.0 |
| Spojené království – sever | 13.87.97.210 | |
| Spojené království – jih 1 | 51.140.184.11 | |    
| Spojené království – jih 2 | 13.87.34.7 | |
| Spojené království – západ | 51.141.8.11  | |
| Západní střed USA | 13.78.145.25 | |
| Západní Evropa | 191.237.232.75 | 40.68.37.158 |
| Západní USA 1 | 23.99.34.75 | 104.42.238.205 |
| Západní USA 2 | 13.66.226.202  | |
||||

## <a name="change-azure-sql-database-connection-policy"></a>Změnit zásady připojení databáze SQL Azure

toochange hello Azure SQL Database zásady připojení pro server Azure SQL Database, použijte hello [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx). 

- Pokud vaše zásady připojení je nastaven příliš**Proxy**, všechny síťové pakety toku prostřednictvím brány Azure SQL Database hello. U tohoto nastavení musíte IP brány tooallow odchozí tooonly hello Azure SQL Database. Pomocí nastavení **Proxy** má více latenci než nastavení **přesměrování**. 
- Pokud je nastavení zásady vaší připojení **přesměrování**, všech síťových paketů toku přímo toohello middleware proxy. U tohoto nastavení musíte odchozí toomultiple tooallow IP adresy. 

## <a name="script-toochange-connection-settings"></a>Nastavení připojení toochange skriptu

> [!IMPORTANT]
> Tento skript vyžaduje hello [modul Azure PowerShell](/powershell/azure/install-azurerm-ps).
>

Hello následující skript prostředí PowerShell ukazuje, jak toochange hello zásady připojení.

```powershell
import-module azureRm
Login-AzureRmAccount

$tenantId =  #your AAD tenant ID
$subscriptionId = #Azure SubscriptionID
$uri = #AAD uri
$authUrl = "https://login.microsoftonline.com/$tenantId"
$serverName = #sqldb server name 
$resourceGroupName=#sqldb resource group
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl

$result = $AuthContext.AcquireToken("https://management.core.windows.net/",
$clientId,
[Uri]$uri, 
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#getting hello current connection property
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#setting hello property too‘Proxy’
$connectionType=”Proxy” <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="next-steps"></a>Další kroky

- Informace o tom, jak toochange hello Azure SQL Database zásady připojení pro server Azure SQL Database, najdete v části [vytvořit nebo aktualizovat zásady připojení serveru pomocí rozhraní REST API hello](https://msdn.microsoft.com/library/azure/mt604439.aspx).
- Informace o chování Azure SQL Database připojení pro klienty, kteří používají ADO.NET 4.5 nebo novější verze najdete v tématu [porty nad rámec 1433 pro technologii ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Obecná aplikace vývoj přehled informace najdete v tématu [přehled vývoje aplikace databáze SQL](sql-database-develop-overview.md).
