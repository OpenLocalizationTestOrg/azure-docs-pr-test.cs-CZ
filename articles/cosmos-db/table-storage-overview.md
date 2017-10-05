---
title: "Přehled služby Azure Table Storage | Dokumentace Microsoftu"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/23/2017
ms.author: mimig
ms.openlocfilehash: 9099e90c402185b371495379db943d64fb82cdb8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-table-storage-overview"></a><span data-ttu-id="9cb8c-103">Přehled služby Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="9cb8c-103">Azure Table storage overview</span></span>

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="9cb8c-104">Azure Table Storage je služba, která ukládá strukturovaná data NoSQL do cloudu a poskytuje úložiště klíčů/atributů s návrhem bez použití schématu.</span><span class="sxs-lookup"><span data-stu-id="9cb8c-104">Azure Table storage is a service that stores structured NoSQL data in the cloud, providing a key/attribute store with a schemaless design.</span></span> <span data-ttu-id="9cb8c-105">Vzhledem k tomu, že je Table Storage bez schématu, je snadné data přizpůsobovat měnícím se potřebám vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cb8c-105">Because Table storage is schemaless, it's easy to adapt your data as the needs of your application evolve.</span></span> <span data-ttu-id="9cb8c-106">Přístup k datům Table Storage je pro mnoho typů aplikací rychlý a nákladově efektivní a pro podobné objemy dat obvykle znamená nižší náklady než tradiční SQL.</span><span class="sxs-lookup"><span data-stu-id="9cb8c-106">Access to Table storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data.</span></span>

<span data-ttu-id="9cb8c-107">Table Storage můžete používat k ukládání flexibilních datových sad, například uživatelských dat pro webové aplikace, adresářů, informací o zařízení nebo dalších typů metadat, které vaše služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="9cb8c-107">You can use Table storage to store flexible datasets like user data for web applications, address books, device information, or other types of metadata your service requires.</span></span> <span data-ttu-id="9cb8c-108">V tabulce můžete uložit libovolný počet entit a účet úložiště může obsahovat libovolný počet tabulek, až do limitu kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9cb8c-108">You can store any number of entities in a table, and a storage account may contain any number of tables, up to the capacity limit of the storage account.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a><span data-ttu-id="9cb8c-109">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9cb8c-109">Next steps</span></span>

* <span data-ttu-id="9cb8c-110">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.</span><span class="sxs-lookup"><span data-stu-id="9cb8c-110">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

* [<span data-ttu-id="9cb8c-111">Začínáme se službou Azure Table Storage pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9cb8c-111">Getting Started with Azure Table Storage in .NET</span></span>](table-storage-how-to-use-dotnet.md)

* <span data-ttu-id="9cb8c-112">Projděte si referenční dokumentaci ke službě Table, kde najdete úplné podrobnosti o dostupných rozhraních API:</span><span class="sxs-lookup"><span data-stu-id="9cb8c-112">View the Table service reference documentation for complete details about available APIs:</span></span>

    * [<span data-ttu-id="9cb8c-113">Klientská knihovna Storage pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="9cb8c-113">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

    * [<span data-ttu-id="9cb8c-114">REST API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="9cb8c-114">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
