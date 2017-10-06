---
title: "aaaConfiguring a pomocí hello emulátor úložiště pomocí sady Visual Studio | Microsoft Docs"
description: "Konfigurace a použití hello emulátor úložiště pomocí sady Visual Studio"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: c8e7996f-6027-4762-806e-614b93131867
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/17/2017
ms.author: kraigb
ms.openlocfilehash: d590f21146c86bcb7bfa6b6164b92c6df5938d5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-and-using-hello-storage-emulator-with-visual-studio"></a>Konfigurace a použití hello emulátor úložiště pomocí sady Visual Studio
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Přehled
Hello Azure SDK vývojové prostředí nabízí hello emulátor úložiště, nástroj, který simuluje hello Blob, fronty a tabulky úložiště služby v Azure k dispozici na místním vývojovém počítači. Pokud jste vytváření cloudové služby, který využívá hello služby Azure storage nebo zápis žádné externí aplikací, že volání hello služby úložiště, můžete otestovat kód místně emulátoru hello úložiště. Hello nástroje Azure pro sadu Microsoft Visual Studio integrovat správu emulátor úložiště hello do sady Visual Studio. Nástroje Azure Hello inicializace databáze emulátor úložiště hello na první použití, spustí hello služba emulátor úložiště, když spustíte nebo ladění kódu ze sady Visual Studio a poskytuje přístup jen pro čtení toohello úložiště emulátoru data prostřednictvím hello Azure Storage Explorer.

Podrobné informace o hello emulátor úložiště, včetně požadavky na systém a vlastní konfigurace pokyny najdete v tématu [hello pomocí emulátoru úložiště Azure pro vývoj a testování](storage/common/storage-use-emulator.md).

> [!NOTE]
> Existují určité rozdíly ve funkcích mezi simulace emulátor úložiště hello a hello služby Azure storage. V tématu [rozdíly mezi hello emulátor úložiště a služby úložiště Azure](storage/common/storage-use-emulator.md) v hello dokumentace sady Azure SDK pro informace o konkrétních rozdílech hello.
> 
> 

## <a name="configuring-a-connection-string-for-hello-storage-emulator"></a>Konfigurace připojovací řetězec pro emulátor úložiště hello
emulátor úložiště tooaccess hello z kódu v rámci role, budete chtít tooconfigure připojení řetězec této emulátor úložiště toohello body a který lze později změněné toopoint tooan účtu úložiště Azure. Připojovací řetězec je nastavení konfigurace, který může číst vaše role v účtu úložiště tooa tooconnect modulu runtime. Další informace o tom, najdete v části toocreate připojovací řetězce, [hello konfigurace aplikace Azure](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

> [!NOTE]
> Odkaz na účet emulátor úložiště toohello může vrátit z kódu pomocí hello **DevelopmentStorageAccount** vlastnost. Tento postup funguje správně, pokud chcete tooaccess hello emulátor úložiště z vašeho kódu, ale pokud máte v plánu toopublish tooAzure vaší aplikace, bude nutné toocreate tooaccess řetězec připojení účtu úložiště Azure a upravit váš kód toouse, Před publikováním připojovací řetězec. Pokud přepínáte mezi hello účet emulátor úložiště a účet úložiště Azure často, bude tento proces zjednodušit připojovací řetězec.
> 
> 

## <a name="initializing-and-running-hello-storage-emulator"></a>Inicializace a spouštění emulátor úložiště hello
Můžete určit, že při spuštění nebo ladění služby v sadě Visual Studio, Visual Studio automaticky spustí emulátor úložiště hello. V Průzkumníku řešení otevřete hello místní nabídku pro vaše **Azure** projektu a zvolte **vlastnosti**. Na hello **vývoj** na kartě hello **spusťte emulátor úložiště Azure** vyberte **True** (Pokud již není nastavena hodnota toothat).

Hello poprvé spustit nebo ladění služby ze sady Visual Studio, emulátor úložiště hello spouští inicializace procesu. Tento proces rezervuje místní porty pro emulátor úložiště hello a vytvoří databázi emulátor úložiště hello. Po dokončení tohoto procesu není nutné toorun znovu Pokud databáze, emulátor úložiště hello se odstraní.

> [!NOTE]
> Od verze červen 2012 hello hello nástroje Azure, hello emulátor úložiště, ve výchozím nastavení spustí, v SQL Express LocalDB. V dřívějších verzích hello nástroje Azure emulátor úložiště hello spuštěno u výchozí instance systému SQL Express 2005 nebo 2008, který je nutno nainstalovat předtím, než můžete nainstalovat hello Azure SDK. Můžete také spustit emulátor úložiště hello proti pojmenované instance SQL Express nebo pojmenovaná nebo výchozí instanci serveru Microsoft SQL Server. Pokud potřebujete tooconfigure hello úložiště emulátoru toorun proti instanci než hello výchozí instance, přečtěte si [hello pomocí emulátoru úložiště Azure pro vývoj a testování](storage/common/storage-use-emulator.md).
> 
> 

emulátor úložiště Hello poskytuje uživatelské rozhraní tooview hello stav služby hello místní úložiště a toostart, zastavte a je resetujete. Jakmile byla spuštěna služba emulátor úložiště hello, můžete zobrazit uživatelské rozhraní hello nebo spuštění nebo zastavení služby hello kliknutím pravým tlačítkem na ikonu v oznamovací oblasti hello pro hello emulátoru Microsoft Azure na hlavním panelu Windows hello.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Zobrazení dat emulátoru úložiště v Průzkumníku serveru
Hello uzlu Azure Storage v Průzkumníku serveru vám umožní tooview dat a změna nastavení pro objekt blob a dat v tabulce v účtech úložiště, včetně hello emulátoru úložiště. V tématu [prostředků spravovat Azure Blob Storage pomocí Storage Exploreru (Preview)](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) Další informace.

