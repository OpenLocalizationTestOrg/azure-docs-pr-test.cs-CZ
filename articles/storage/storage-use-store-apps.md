---
title: "aaaUse úložiště Azure v aplikacích pro Windows Store | Microsoft Docs"
description: "Zjistěte, jak toocreate Windows Store aplikaci, která používá úložiště objektů Azure Blob, fronty, tabulka nebo souboru."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 63c4b29d-b2f2-4d7c-b164-a0d38f4d14f6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: ac21b0695c0d77c1d81c1b921718fb929945d45e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-storage-in-windows-store-apps"></a>Jak toouse Azure Storage v aplikacích pro Windows Store
## <a name="overview"></a>Přehled
Tato příručka ukazuje, jak se tooget začít s vývojem aplikace pro Windows Store, která se využívá Azure Storage.

## <a name="download-required-tools"></a>Stáhněte požadované nástroje
* [Visual Studio](https://www.visualstudio.com/downloads/) umožňuje snadno toobuild, ladění, lokalizace, balíčků a nasazování aplikací pro Windows Store. Visual Studio 2012 nebo novější je povinný.
* Hello [Klientská knihovna pro úložiště Azure](https://www.nuget.org/packages/WindowsAzure.Storage) obsahuje prostředí Windows Runtime knihovny tříd pro práci s Azure Storage.
* [WCF Data Services nástroje pro aplikace Windows Store](http://www.microsoft.com/download/details.aspx?id=30714) rozšiřuje možnosti Přidat odkaz na službu hello s podporou OData na straně klienta pro aplikace Windows Store v sadě Visual Studio.

## <a name="develop-apps"></a>Vývoj aplikací
### <a name="getting-ready"></a>Příprava
Vytvořte nový projekt aplikace Windows Store v sadě Visual Studio 2012 nebo novější:

![úložiště – aplikace úložiště vs projektu][store-apps-storage-vs-project]

Dál přidejte kliknutím pravým tlačítkem na odkaz toohello Klientská knihovna pro úložiště Azure **odkazy**, kliknete na příkaz **přidat odkaz na**a potom procházením toohello úložiště klientské knihovny pro prostředí Windows Runtime, které stáhnout:

![úložiště – aplikace--zvolte – knihovna pro úložiště][store-apps-storage-choose-library]

### <a name="using-hello-library-with-hello-blob-and-queue-services"></a>Použití knihovny hello s hello objektů Blob a fronty služby
V tomto okamžiku je vaše aplikace služby Azure Blob a fronty připravené toocall hello. Přidejte následující hello **pomocí** příkazy tak, aby typy úložiště Azure může být odkazováno přímo:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
```

V dalším kroku přidáte stránku tooyour tlačítko. Přidejte následující kód tooits hello **klikněte na tlačítko** události a změnit metodu obslužných rutin událostí pomocí hello [async – klíčové slovo](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var blobClient = account.CreateCloudBlobClient();
var container = blobClient.GetContainerReference("container1");
await container.CreateIfNotExistsAsync();
```

Tento kód předpokládá, že máte dvě proměnné řetězec, *accountName* a *accountKey*. Představují hello název účtu a hello klíč účtu úložiště přidružený k tomuto účtu.

Sestavení a spuštění aplikace hello. Kliknutím na tlačítko hello zkontroluje, zda kontejner s názvem *container1* existuje ve vašem účtu a pak ji vytvořit, pokud není.

### <a name="using-hello-library-with-hello-table-service"></a>Pomocí hello knihovně hello služby Table
Typy, které jsou používané toocommunicate službou Azure Table hello závisí na součásti WCF Data Services pro knihovny app Windows Store hello. V dalším kroku přidáte že odkaz na toohello požadované knihovny WCF pomocí hello Konzola správce balíčků:

![úložiště – aplikace úložiště--Správce balíčků][store-apps-storage-package-manager]

Použijte následující příkaz toopoint Správce balíčků toohello umístění na počítači hello:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Tento příkaz automaticky přidá všechny požadované odkazy tooyour projektu. Pokud nechcete, aby toouse hello Konzola správce balíčků, můžete přidat složku hello WCF Data Services NuGet ve vašem místním počítači toohello seznamu zdroji balíčků a pak přidat odkaz hello prostřednictvím hello uživatelského rozhraní, jak je popsáno v [správa pomocí balíčků NuGet Dialogové okno Hello](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Když budete mít balíček WCF Data Services NuGet hello, změnit hello kód ve vaší tlačítko **klikněte na tlačítko** událostí:

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var tableClient = account.CreateCloudTableClient();
var table = tableClient.GetTableReference("table1");
await table.CreateIfNotExistsAsync();
```

Tento kód zkontroluje, jestli tabulka s názvem *tabulky1* existuje ve vašem účtu a pak ji vytvoří, pokud není.

Můžete také přidat tooMicrosoft.WindowsAzure.Storage.Table.dll referenční dokumentace, která je dostupná v hello stejný balíček, který jste si stáhli. Tato knihovna obsahuje další funkce, například na základě reflexe serializace a generických dotazech. Všimněte si, že tato knihovna nepodporuje jazyk JavaScript.

[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
