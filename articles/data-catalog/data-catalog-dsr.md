---
title: "Podporované zdroje dat v Azure Data Catalog | Microsoft Docs"
description: "V tomto článku jsou uvedeny specifikace zdrojů dat aktuálně podporované."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: jstevens
editor: 
tags: 
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: b76f3cc3d8538a50895f5be32a800cb469c81203
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="supported-data-sources-in-azure-data-catalog"></a>Podporované zdroje dat v Azure Data Catalog

Metadata můžete publikovat pomocí veřejné rozhraní API nebo klikněte na-jednou registrace Nástroje, nebo když ručně zadáte informace přímo do Azure Data Catalog webový portál. Následující tabulka shrnuje všechny zdroje dat, které jsou podporovány v katalogu dnes a možnosti publikování pro každou. Externí data nástroje, které všechny zdroje dat, můžete spustit z našich zkušeností portál "open in" jsou také uvedeny. Druhá tabulka obsahuje další technické specifikace každé připojení vlastnosti zdroje dat.


## <a name="list-of-supported-data-sources"></a>Seznam podporovaných zdrojů dat

<table>
    <tr>
       <td><b>Objekt zdroje dat</b></td>
       <td><b>Rozhraní API</b></td>
       <td><b>Ruční zadání</b></td>
       <td><b>Nástroj pro registraci</b></td>
       <td><b>Nástroje pro Open in</b></td>
       <td><b>Poznámky k</b></td>
    </tr>
    <tr>
      <td>Adresář Azure Data Lake Store</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Soubor Azure Data Lake Store</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Azure Blob Storage</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Adresář úložiště Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulky Azure Storage</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2">
      </td>
      <td>
        <font size="2">
      </td>
    </tr>
    <tr>
      <td>Adresář HDFS</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Soubor HDFS</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulku Hive</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel</td>
      <td></td>
    </tr>
    <tr>
      <td>Zobrazení Hive</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel</td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulka MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Aplikace Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Zobrazení MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Aplikace Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulka databáze Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Aplikace Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Zobrazení databáze Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Aplikace Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Jiné (Obecné asset)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulky Azure SQL Data Warehouse</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Nástroje aplikace Excel, Power BI Desktop, SQL Server data tools</td>
      <td></td>
    </tr>
    <tr>
      <td>Zobrazení SQL Data Warehouse</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Nástroje aplikace Excel, Power BI Desktop, SQL Server data tools</td>
      <td></td>
    </tr>
    <tr>
      <td>Dimenze služby SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Aplikace Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services klíčového ukazatele výkonu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Aplikace Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Míra SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Aplikace Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulky SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Aplikace Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Sestavy služby SQL Server Reporting Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Prohlížeč</td>
      <td>Pouze servery v nativním režimu. Režim serveru SharePoint není podporována.</td>
    </tr>
    <tr>
      <td>Tabulka SQL Serveru</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Nástroje aplikace Excel, Power BI Desktop, SQL Server data tools</td>
      <td></td>
    </tr>
    <tr>
      <td>SQL Server view</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Nástroje aplikace Excel, Power BI Desktop, SQL Server data tools</td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulka Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel</td>
      <td></td>
    </tr>
    <tr>
      <td>Teradata view</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel</td>
      <td></td>
    </tr>
    <tr>
      <td>Zobrazení SAP HANA</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulka DB2</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Zobrazení DB2</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Soubor systému souborů</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Adresář serveru FTP</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Soubor FTP</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Sestavy HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Koncový bod HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Soubor protokolu HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Sada entit OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Funkce OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulka PostgreSQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>PostgreSQL zobrazení</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Zobrazení SAP HANA</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td> Objekt služby Salesforce</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Seznam serveru SharePoint </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Azure Cosmos DB kolekce</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Obecná tabulka rozhraní ODBC</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Obecná zobrazení rozhraní ODBC</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulka Cassandra</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td>Publikovat jako obecný asset rozhraní ODBC</td>
    </tr>
    <tr>
      <td>Cassandra zobrazení</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td>Publikovat jako obecný asset rozhraní ODBC</td>
    </tr>
    <tr>
      <td>Tabulka Sybase</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Zobrazení Sybase</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Tabulka MongoDB</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td>Publikovat jako obecný asset rozhraní ODBC</td>
    </tr>
    <tr>
      <td>Zobrazení MongoDB</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td>Publikovat jako obecný asset rozhraní ODBC</td>
    </tr>
</table>

Pokud chcete najdete v části konkrétní zdroj dat. podporována, ho navrhne (nebo hlasové podporu, pokud již byla navrhované) přechodem na [na fórech zpětnou vazbu o Azure Data Catalog](https://feedback.azure.com/forums/906052-data-catalog).


## <a name="data-source-reference-specification"></a>Specifikace odkaz na zdroj dat
> [!NOTE]
> **DSL struktura** sloupci v následující tabulce jsou uvedeny pouze vlastnosti připojení pro kontejner objektů a dat "address" používané Azure Data Catalog. To znamená balík vlastností "address" může obsahovat další vlastnosti připojení zdroje dat, které Azure Data Catalog potrvají, ale nepoužívá.

<table>
    <tr>
       <td><b>Typ zdroje</b></td>
       <td><b>Typ prostředku</b></td>
       <td><b>Typy objektů</b></td>
       <td><b>Struktura DSL<b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Kontejner</td>
      <td>Data Lake</td>
      <td>
        
            Protocol: webhdfs
            <br>Authentication: {basic, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Table</td>
      <td>Directory, file</td>
      <td>
        
            Protocol: webhdfs
            <br>Authentication: {basic, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        
            Protocol: azure-blobs
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container
        
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Table</td>
      <td>Blob, directory</td>
      <td>
        
            Protocol: azure-blobs
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name
        
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        
            Protocol: azure-tables
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account
        
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Table</td>
      <td>Table</td>
      <td>
        
            Protocol: azure-tables
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name
        
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Container</td>
      <td>Virtual cluster</td>
      <td>
        
            Protocol: cosmos
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Table</td>
      <td>Stream, stream set, view</td>
      <td>
        
            Protocol: cosmos
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Report</td>
      <td>Report, dashboard</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: db2
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: db2
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
        
      </td>
    </tr>
    <tr>
      <td>File system</td>
      <td>Table</td>
      <td>File</td>
      <td>
        
            Protocol: file
            <br>Authentication: {none, basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path
        
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Table</td>
      <td>Directory, file</td>
      <td>
        
            Protocol: ftp
            <br>Authentication: {none, basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Container</td>
      <td>Cluster</td>
      <td>
        
            Protocol: webhdfs
            <br>Authentication: {basic, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Table</td>
      <td>Directory, file</td>
      <td>
        
            Protocol: webhdfs
            <br>Authentication: {basic, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: hive
            <br>Authentication: {HDInsight, basic, username, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>connectionProperties:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2}
        
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: hive
            <br>Authentication: {HDInsight, basic, username, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>connectionProperties:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2}
        
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Report</td>
      <td>Report, dashboard</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Table</td>
      <td>Endpoint, file</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: mysql
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: mysql
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Entity container</td>
      <td>
        
            Protocol: odata
            <br>Authentication: {none, basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Table</td>
      <td>Entity set, function</td>
      <td>
        
            Protocol: odata
            <br>Authentication: {none, basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; resource
        
      </td>
    </tr>
    <tr>
      <td>Oracle Database</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: oracle
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Oracle Database</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: oracle
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: postgresql
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: postgresql
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>Power BI Desktop</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Power BI Desktop</td>
      <td>Report</td>
      <td>Report, dashboard</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Table</td>
      <td>Data mashup</td>
      <td>
        
            Protocol: power-query
            <br>Authentication: {oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Table</td>
      <td>Object</td>
      <td>
        
            Protocol: salesforce-com
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; loginServer
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itemName
        
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        
            Protocol: sap-hana-sql
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
        
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Table</td>
      <td>View</td>
      <td>
        
            Protocol: sap-hana-sql
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Table</td>
      <td>List</td>
      <td>
        
            Protocol: sharepoint-list
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Command</td>
      <td>Stored procedure</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>TableValuedFunction</td>
      <td>Table-valued function</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: tds
          <br>Authentication: {protocol, windows}
          <br>Address:
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Command</td>
      <td>Stored procedure</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Table-valued function</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>Measure</td>
      <td>Measure</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>Table</td>
      <td>Dimension</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Dimension}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabular</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabular</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabular</td>
      <td>Measure</td>
      <td>Measure</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabular</td>
      <td>Table</td>
      <td>Table</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Table}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        
            Protocol: reporting-services
            <br>Authentication: {windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Report</td>
      <td>Report</td>
      <td>
        
            Protocol: reporting-services
            <br>Authentication: {windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010}
        
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: teradata
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: teradata
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        <font size="2">
          Protocol: mssql-mds
          <br>Authentication: {windows}
          <br>Address:
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Table</td>
      <td>Entity</td>
      <td>
        <font size="2">
          Protocol: mssql-mds
          <br>Authentication: {windows}
          <br>Address:
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; entity
        
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: document-db
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Collection</td>
      <td>Collection</td>
      <td>
        
            Protocol: document-db
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; collection
        
      </td>
    </tr>
    <tr>
      <td>Generic ODBC</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: odbc
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Generic ODBC</td>
      <td>Table</td>
      <td>Table, View</td>
      <td>
        
            Protocol: odbc
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
        
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            protocol: sybase
            <br>authentication: {basic, windows}
            <br>address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Table</td>
      <td>Table, View</td>
      <td>
        
            protocol: sybase
            <br>authentication: {basic, windows}
            <br>address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>Other (none of the above)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        
            Protocol: generic-asset
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; assetId
        
      </td>
    </tr>
</table>
