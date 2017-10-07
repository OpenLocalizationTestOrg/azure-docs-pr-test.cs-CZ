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
# <a name="how-toouse-azure-storage-in-windows-store-apps"></a><span data-ttu-id="1e187-103">Jak toouse Azure Storage v aplikacích pro Windows Store</span><span class="sxs-lookup"><span data-stu-id="1e187-103">How toouse Azure Storage in Windows Store apps</span></span>
## <a name="overview"></a><span data-ttu-id="1e187-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1e187-104">Overview</span></span>
<span data-ttu-id="1e187-105">Tato příručka ukazuje, jak se tooget začít s vývojem aplikace pro Windows Store, která se využívá Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="1e187-105">This guide shows how tooget started with developing a Windows Store app that makes use of Azure Storage.</span></span>

## <a name="download-required-tools"></a><span data-ttu-id="1e187-106">Stáhněte požadované nástroje</span><span class="sxs-lookup"><span data-stu-id="1e187-106">Download required tools</span></span>
* <span data-ttu-id="1e187-107">[Visual Studio](https://www.visualstudio.com/downloads/) umožňuje snadno toobuild, ladění, lokalizace, balíčků a nasazování aplikací pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="1e187-107">[Visual Studio](https://www.visualstudio.com/downloads/) makes it easy toobuild, debug, localize, package, and deploy Windows Store apps.</span></span> <span data-ttu-id="1e187-108">Visual Studio 2012 nebo novější je povinný.</span><span class="sxs-lookup"><span data-stu-id="1e187-108">Visual Studio 2012 or later is required.</span></span>
* <span data-ttu-id="1e187-109">Hello [Klientská knihovna pro úložiště Azure](https://www.nuget.org/packages/WindowsAzure.Storage) obsahuje prostředí Windows Runtime knihovny tříd pro práci s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="1e187-109">hello [Azure Storage Client Library](https://www.nuget.org/packages/WindowsAzure.Storage) provides a Windows Runtime class library for working with Azure Storage.</span></span>
* <span data-ttu-id="1e187-110">[WCF Data Services nástroje pro aplikace Windows Store](http://www.microsoft.com/download/details.aspx?id=30714) rozšiřuje možnosti Přidat odkaz na službu hello s podporou OData na straně klienta pro aplikace Windows Store v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e187-110">[WCF Data Services Tools for Windows Store Apps](http://www.microsoft.com/download/details.aspx?id=30714) extends hello Add Service Reference experience with client-side OData support for Windows Store apps in Visual Studio.</span></span>

## <a name="develop-apps"></a><span data-ttu-id="1e187-111">Vývoj aplikací</span><span class="sxs-lookup"><span data-stu-id="1e187-111">Develop apps</span></span>
### <a name="getting-ready"></a><span data-ttu-id="1e187-112">Příprava</span><span class="sxs-lookup"><span data-stu-id="1e187-112">Getting ready</span></span>
<span data-ttu-id="1e187-113">Vytvořte nový projekt aplikace Windows Store v sadě Visual Studio 2012 nebo novější:</span><span class="sxs-lookup"><span data-stu-id="1e187-113">Create a new Windows Store app project in Visual Studio 2012 or later:</span></span>

![úložiště – aplikace úložiště vs projektu][store-apps-storage-vs-project]

<span data-ttu-id="1e187-115">Dál přidejte kliknutím pravým tlačítkem na odkaz toohello Klientská knihovna pro úložiště Azure **odkazy**, kliknete na příkaz **přidat odkaz na**a potom procházením toohello úložiště klientské knihovny pro prostředí Windows Runtime, které stáhnout:</span><span class="sxs-lookup"><span data-stu-id="1e187-115">Next, add a reference toohello Azure Storage Client Library by right-clicking **References**, clicking **Add Reference**, and then browsing toohello Storage Client Library for Windows Runtime that you downloaded:</span></span>

![úložiště – aplikace--zvolte – knihovna pro úložiště][store-apps-storage-choose-library]

### <a name="using-hello-library-with-hello-blob-and-queue-services"></a><span data-ttu-id="1e187-117">Použití knihovny hello s hello objektů Blob a fronty služby</span><span class="sxs-lookup"><span data-stu-id="1e187-117">Using hello library with hello Blob and Queue services</span></span>
<span data-ttu-id="1e187-118">V tomto okamžiku je vaše aplikace služby Azure Blob a fronty připravené toocall hello.</span><span class="sxs-lookup"><span data-stu-id="1e187-118">At this point, your app is ready toocall hello Azure Blob and Queue services.</span></span> <span data-ttu-id="1e187-119">Přidejte následující hello **pomocí** příkazy tak, aby typy úložiště Azure může být odkazováno přímo:</span><span class="sxs-lookup"><span data-stu-id="1e187-119">Add hello following **using** statements so that Azure Storage types can be referenced directly:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
```

<span data-ttu-id="1e187-120">V dalším kroku přidáte stránku tooyour tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1e187-120">Next, add a button tooyour page.</span></span> <span data-ttu-id="1e187-121">Přidejte následující kód tooits hello **klikněte na tlačítko** události a změnit metodu obslužných rutin událostí pomocí hello [async – klíčové slovo](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):</span><span class="sxs-lookup"><span data-stu-id="1e187-121">Add hello following code tooits **Click** event and modify your event handler method by using hello [async keyword](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):</span></span>

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var blobClient = account.CreateCloudBlobClient();
var container = blobClient.GetContainerReference("container1");
await container.CreateIfNotExistsAsync();
```

<span data-ttu-id="1e187-122">Tento kód předpokládá, že máte dvě proměnné řetězec, *accountName* a *accountKey*.</span><span class="sxs-lookup"><span data-stu-id="1e187-122">This code assumes that you have two string variables, *accountName* and *accountKey*.</span></span> <span data-ttu-id="1e187-123">Představují hello název účtu a hello klíč účtu úložiště přidružený k tomuto účtu.</span><span class="sxs-lookup"><span data-stu-id="1e187-123">They represent hello name of your storage account and hello account key that is associated with that account.</span></span>

<span data-ttu-id="1e187-124">Sestavení a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="1e187-124">Build and run hello application.</span></span> <span data-ttu-id="1e187-125">Kliknutím na tlačítko hello zkontroluje, zda kontejner s názvem *container1* existuje ve vašem účtu a pak ji vytvořit, pokud není.</span><span class="sxs-lookup"><span data-stu-id="1e187-125">Clicking hello button will check whether a container named *container1* exists in your account and then create it if not.</span></span>

### <a name="using-hello-library-with-hello-table-service"></a><span data-ttu-id="1e187-126">Pomocí hello knihovně hello služby Table</span><span class="sxs-lookup"><span data-stu-id="1e187-126">Using hello library with hello Table service</span></span>
<span data-ttu-id="1e187-127">Typy, které jsou používané toocommunicate službou Azure Table hello závisí na součásti WCF Data Services pro knihovny app Windows Store hello.</span><span class="sxs-lookup"><span data-stu-id="1e187-127">Types that are used toocommunicate with hello Azure Table service depend on WCF Data Services for hello Windows Store app library.</span></span> <span data-ttu-id="1e187-128">V dalším kroku přidáte že odkaz na toohello požadované knihovny WCF pomocí hello Konzola správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="1e187-128">Next, add a reference toohello required WCF libraries by using hello Package Manager Console:</span></span>

![úložiště – aplikace úložiště--Správce balíčků][store-apps-storage-package-manager]

<span data-ttu-id="1e187-130">Použijte následující příkaz toopoint Správce balíčků toohello umístění na počítači hello:</span><span class="sxs-lookup"><span data-stu-id="1e187-130">Use hello following command toopoint Package Manager toohello location on your machine:</span></span>

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

<span data-ttu-id="1e187-131">Tento příkaz automaticky přidá všechny požadované odkazy tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="1e187-131">This command will automatically add all required references tooyour project.</span></span> <span data-ttu-id="1e187-132">Pokud nechcete, aby toouse hello Konzola správce balíčků, můžete přidat složku hello WCF Data Services NuGet ve vašem místním počítači toohello seznamu zdroji balíčků a pak přidat odkaz hello prostřednictvím hello uživatelského rozhraní, jak je popsáno v [správa pomocí balíčků NuGet Dialogové okno Hello](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).</span><span class="sxs-lookup"><span data-stu-id="1e187-132">If you do not want toouse hello Package Manager Console, you can add hello WCF Data Services NuGet folder on your local machine toohello list of package sources and then add hello reference through hello UI, as described in [Managing NuGet Packages Using hello Dialog](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).</span></span>

<span data-ttu-id="1e187-133">Když budete mít balíček WCF Data Services NuGet hello, změnit hello kód ve vaší tlačítko **klikněte na tlačítko** událostí:</span><span class="sxs-lookup"><span data-stu-id="1e187-133">When you have referenced hello WCF Data Services NuGet package, change hello code in your button's **Click** event:</span></span>

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var tableClient = account.CreateCloudTableClient();
var table = tableClient.GetTableReference("table1");
await table.CreateIfNotExistsAsync();
```

<span data-ttu-id="1e187-134">Tento kód zkontroluje, jestli tabulka s názvem *tabulky1* existuje ve vašem účtu a pak ji vytvoří, pokud není.</span><span class="sxs-lookup"><span data-stu-id="1e187-134">This code checks whether a table named *table1* exists in your account, and then creates it if not.</span></span>

<span data-ttu-id="1e187-135">Můžete také přidat tooMicrosoft.WindowsAzure.Storage.Table.dll referenční dokumentace, která je dostupná v hello stejný balíček, který jste si stáhli.</span><span class="sxs-lookup"><span data-stu-id="1e187-135">You can also add a reference tooMicrosoft.WindowsAzure.Storage.Table.dll, which is available in hello same package that you downloaded.</span></span> <span data-ttu-id="1e187-136">Tato knihovna obsahuje další funkce, například na základě reflexe serializace a generických dotazech.</span><span class="sxs-lookup"><span data-stu-id="1e187-136">This library contains additional functionality, such as reflection-based serialization and generic queries.</span></span> <span data-ttu-id="1e187-137">Všimněte si, že tato knihovna nepodporuje jazyk JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1e187-137">Note that this library does not support JavaScript.</span></span>

[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
