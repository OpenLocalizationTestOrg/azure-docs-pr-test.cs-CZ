---
title: "Řešení potíží s Microsoft Power BI Embedded Preview"
description: "Řešení potíží s Microsoft Power BI Embedded Preview"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: c8aee652-ed8b-4c66-9c63-f798b7a655b4
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: f406d23e578acc825514aa5bd9eabcbf160bf9ec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Řešení potíží s Microsoft Power BI Embedded Preview
Tento článek obsahuje odpovědi pro řešení potíží s **Power BI Embedded**.

<a name="connection-string"/>

## <a name="setting-sql-server-connection-strings"></a>Nastavení systému SQL Server připojovací řetězce
Pokud chcete nastavit připojení řetězec systému SQL Server, budete muset postupovat podle konkrétním formátu. Níže je příklad připojovací řetězce pro SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Další informace o řetězce připojení SQL Server, naleznete v následujících článcích:

* [Připojovací řetězce SQL serveru](https://msdn.microsoft.com/library/jj653752.aspx)
* [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>

## <a name="setting-credentials"></a>Nastavení přihlašovacích údajů
V případě, kdy máte přihlašovací údaje pro vývoj nebo pracovní prostředí, například uživatelské jméno a heslo možná budete muset aktualizovat přihlašovací údaje, které odpovídají produkční řešení.

## <a name="see-also"></a>Viz také
* [Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)
* [Co je Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)

