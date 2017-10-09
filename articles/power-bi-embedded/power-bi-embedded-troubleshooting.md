---
title: "řešení potíží s aaaMicrosoft Power BI Embedded Preview"
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
ms.openlocfilehash: a0a25cd73977c0ea0bd6b7c82e215412245771bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Řešení potíží s Microsoft Power BI Embedded Preview
Tento článek obsahuje odpovědi jak tootroubleshoot **Power BI Embedded**.

<a name="connection-string"/>

## <a name="setting-sql-server-connection-strings"></a>Nastavení systému SQL Server připojovací řetězce
tooset připojovací řetězec systému SQL Server, musíte toofollow konkrétním formátu. Níže je příklad připojovací řetězce pro SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

toolearn Další informace o řetězce připojení SQL Server, najdete v části hello následující články:

* [Připojovací řetězce SQL serveru](https://msdn.microsoft.com/library/jj653752.aspx)
* [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>

## <a name="setting-credentials"></a>Nastavení přihlašovacích údajů
V případě hello kde máte přihlašovací údaje pro vývoj nebo pracovní prostředí, například uživatelské jméno a heslo bude pravděpodobně nutné tooupdate přihlašovací údaje, které odpovídají produkční řešení.

## <a name="see-also"></a>Viz také
* [Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)
* [Co je Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)

