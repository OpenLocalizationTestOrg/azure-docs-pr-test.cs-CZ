---
title: "aaaDesign první databázi SQL Azure - C# | Microsoft Docs"
description: "Další informace toodesign svoji první databázi Azure SQL a připojit tooit s programu v C# pomocí ADO.NET."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 07/31/2017
ms.author: genemi;carlrab
ms.openlocfilehash: 8161de24bff1ec2fa307efa93adab2bd1b761fd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Návrh Azure SQL database a připojení s C & #x23; a ADO.NET

Databáze SQL Azure je relační databáze jako a služba (DBaaS) v hello cloudu Microsoftu ("Azure"). V tomto kurzu zjistíte, jak toouse hello portál Azure a ADO.NET pomocí sady Visual Studio: 

> [!div class="checklist"]
> * Vytvoření databáze v hello portálu Azure
> * Nastavit pravidlo brány firewall na úrovni serveru v hello portálu Azure
> * Připojit databáze toohello s ADO.NET a Visual Studio
> * Vytváření tabulek s ADO.NET
> * Vložit, aktualizovat a odstranit data s ADO.NET 
> * Dotaz na data ADO.NET

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="prerequisites"></a>Požadavky

Instalaci sady [Visual Studio Community 2017, Visual Studio Professional 2017 nebo Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

<!-- hello following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- hello following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli, že databáze basic úlohy, jako například vytvořit databáze a tabulky, načítat a zadávat dotazy na data a obnovit hello databáze tooa předchozího bodu v čase. Naučili jste se tyto postupy:
> [!div class="checklist"]
> * Vytvoření databáze
> * Nastavit pravidlo brány firewall
> * Připojit databáze toohello s [Visual Studio a C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Vytváření tabulek
> * Vložit, aktualizovat a odstranit data
> * Dotazování dat

Další kurz toolearn toohello o migraci dat zálohy.

> [!div class="nextstepaction"]
>[Migrace vaší tooAzure databáze systému SQL Server databáze SQL](sql-database-migrate-your-sql-server-database.md)

