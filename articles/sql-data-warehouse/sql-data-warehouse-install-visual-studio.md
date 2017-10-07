---
title: aaaInstall Visual Studio a SSDT pro SQL Data Warehouse | Microsoft Docs
description: Instalace sady Visual Studio a SQL Server Development Tools (SSDT) pro Azure SQL Data Warehouse
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 03/30/2017
ms.author: anvang;barbkess
ms.openlocfilehash: cf49c13d5cab598ed127f5702c04168b62ede0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Instalace sady Visual Studio a SSDT pro SQL Data Warehouse
toodevelop aplikací pro SQL Data Warehouse, doporučujeme používat nejnovější verzi sady Visual Studio hello s hello nejnovější verzi systému SQL Server Data Tools (SSDT).  Kvůli zpětné kompatibilitě se rovněž podporuje Visual Studio 2013 Update 5 s rozšířením SSDT.  

Pomocí sady Visual Studio s rozšířením SSDT vám umožní toovisually Průzkumník objektů systému SQL Server hello toouse zkoumat tabulky, zobrazení, uložené procedury a celou řadu dalších objektů v SQL Data Warehouse a také spouštět dotazy.

> [!NOTE]
> SQL Data Warehouse zatím nepodporuje databázové projekty sady Visual Studio.  Tato funkce bude přidána v budoucí verzi.
> 
> 

## <a name="step-1-install-visual-studio"></a>Krok 1: Instalace sady Visual Studio
Postupujte podle těchto toodownload odkazy a instalaci sady Visual Studio. Pokud již máte Visual Studio 2013 nebo novější verzi, můžete přeskočit tooStep 2, instalaci rozšíření SSDT.

1. [Stáhněte si Visual Studio][].
2. Postupujte podle hello [instalaci sady Visual Studio] [ Installing Visual Studio] Průvodce na webu MSDN a zvolte hello výchozí konfigurace.

## <a name="step-2-install-ssdt"></a>Krok 2: Instalace rozšíření SSDT
tooinstall rozšíření SSDT pro Visual Studio jednoduše vyhledejte aktualizace SSDT ze sady Visual Studio pomocí následujících kroků.

1. V sadě Visual Studio klikněte na **nástroje** / **rozšíření a aktualizace...** / **Aktualizace**
2. Vyberte **Aktualizace produktu** a potom vyhledejte položku **Microsoft SQL Server Update for database tooling** (Aktualizace Microsoft SQL Serveru pro databázové nástroje).

Pokud není nalezen aktualizace, byste měli mít nainstalovanou nejnovější verzi hello.  tooconfirm rozšíření SSDT je nainstalován, klikněte na **pomoci** / **o sadě Microsoft Visual Studio** a podívejte se v seznamu hello SQL Server Data Tools.  Hello nejnovější verzi rozšíření SSDT je 14.0.60525.0.  Pokud tooinstall hello možnost není k dispozici ze sady Visual Studio, případně můžete navštívit hello [si rozšíření SSDT Stáhnout] [ SSDT Download] stránka toodownload a nainstalovat ručně.

## <a name="next-steps"></a>Další kroky
Teď, když máte nejnovější verzi rozšíření SSDT hello, jste připraveni příliš[připojit] [ connect] tooyour SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Stáhněte si Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
