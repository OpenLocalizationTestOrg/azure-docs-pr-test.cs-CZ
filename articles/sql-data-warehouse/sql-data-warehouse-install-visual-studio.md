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
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a><span data-ttu-id="5c6ad-103">Instalace sady Visual Studio a SSDT pro SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5c6ad-103">Install Visual Studio and SSDT for SQL Data Warehouse</span></span>
<span data-ttu-id="5c6ad-104">toodevelop aplikací pro SQL Data Warehouse, doporučujeme používat nejnovější verzi sady Visual Studio hello s hello nejnovější verzi systému SQL Server Data Tools (SSDT).</span><span class="sxs-lookup"><span data-stu-id="5c6ad-104">toodevelop applications for SQL Data Warehouse, we recommend using hello most recent version of Visual Studio with hello most recent version of SQL Server Data Tools (SSDT).</span></span>  <span data-ttu-id="5c6ad-105">Kvůli zpětné kompatibilitě se rovněž podporuje Visual Studio 2013 Update 5 s rozšířením SSDT.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-105">Visual Studio 2013 Update 5 with SSDT is also supported for backward compatibility.</span></span>  

<span data-ttu-id="5c6ad-106">Pomocí sady Visual Studio s rozšířením SSDT vám umožní toovisually Průzkumník objektů systému SQL Server hello toouse zkoumat tabulky, zobrazení, uložené procedury a celou řadu dalších objektů v SQL Data Warehouse a také spouštět dotazy.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-106">Using Visual Studio with SSDT will allow you toouse hello SQL Server Object Explorer toovisually explore tables, views, stored procedures and many more objects in your SQL Data Warehouse as well as run queries.</span></span>

> [!NOTE]
> <span data-ttu-id="5c6ad-107">SQL Data Warehouse zatím nepodporuje databázové projekty sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-107">SQL Data Warehouse does not yet support Visual Studio Database Projects.</span></span>  <span data-ttu-id="5c6ad-108">Tato funkce bude přidána v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-108">This feature will be added in a future version.</span></span>
> 
> 

## <a name="step-1-install-visual-studio"></a><span data-ttu-id="5c6ad-109">Krok 1: Instalace sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c6ad-109">Step 1: Install Visual Studio</span></span>
<span data-ttu-id="5c6ad-110">Postupujte podle těchto toodownload odkazy a instalaci sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-110">Follow these links toodownload and install Visual Studio.</span></span> <span data-ttu-id="5c6ad-111">Pokud již máte Visual Studio 2013 nebo novější verzi, můžete přeskočit tooStep 2, instalaci rozšíření SSDT.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-111">If you already have Visual Studio 2013 or later installed, you can skip tooStep 2, install SSDT.</span></span>

1. <span data-ttu-id="5c6ad-112">[Stáhněte si Visual Studio][].</span><span class="sxs-lookup"><span data-stu-id="5c6ad-112">[Download Visual Studio][].</span></span>
2. <span data-ttu-id="5c6ad-113">Postupujte podle hello [instalaci sady Visual Studio] [ Installing Visual Studio] Průvodce na webu MSDN a zvolte hello výchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-113">Follow hello [Installing Visual Studio][Installing Visual Studio] guide on MSDN and choose hello default configurations.</span></span>

## <a name="step-2-install-ssdt"></a><span data-ttu-id="5c6ad-114">Krok 2: Instalace rozšíření SSDT</span><span class="sxs-lookup"><span data-stu-id="5c6ad-114">Step 2: Install SSDT</span></span>
<span data-ttu-id="5c6ad-115">tooinstall rozšíření SSDT pro Visual Studio jednoduše vyhledejte aktualizace SSDT ze sady Visual Studio pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-115">tooinstall SSDT for Visual Studio simply check for an SSDT update from within Visual Studio by following these steps.</span></span>

1. <span data-ttu-id="5c6ad-116">V sadě Visual Studio klikněte na **nástroje** / **rozšíření a aktualizace...**</span><span class="sxs-lookup"><span data-stu-id="5c6ad-116">In Visual Studio click on **Tools** / **Extensions and Updates…**</span></span><span data-ttu-id="5c6ad-117"> / **Aktualizace**</span><span class="sxs-lookup"><span data-stu-id="5c6ad-117"> / **Updates**</span></span>
2. <span data-ttu-id="5c6ad-118">Vyberte **Aktualizace produktu** a potom vyhledejte položku **Microsoft SQL Server Update for database tooling** (Aktualizace Microsoft SQL Serveru pro databázové nástroje).</span><span class="sxs-lookup"><span data-stu-id="5c6ad-118">Select **Product Updates** and then look for **Microsoft SQL Server Update for database tooling**</span></span>

<span data-ttu-id="5c6ad-119">Pokud není nalezen aktualizace, byste měli mít nainstalovanou nejnovější verzi hello.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-119">If an update is not found, then you should have hello latest version installed.</span></span>  <span data-ttu-id="5c6ad-120">tooconfirm rozšíření SSDT je nainstalován, klikněte na **pomoci** / **o sadě Microsoft Visual Studio** a podívejte se v seznamu hello SQL Server Data Tools.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-120">tooconfirm SSDT is installed, click on **Help** / **About Microsoft Visual Studio** and look for SQL Server Data Tools in hello list.</span></span>  <span data-ttu-id="5c6ad-121">Hello nejnovější verzi rozšíření SSDT je 14.0.60525.0.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-121">hello latest version of SSDT is 14.0.60525.0.</span></span>  <span data-ttu-id="5c6ad-122">Pokud tooinstall hello možnost není k dispozici ze sady Visual Studio, případně můžete navštívit hello [si rozšíření SSDT Stáhnout] [ SSDT Download] stránka toodownload a nainstalovat ručně.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-122">If hello option tooinstall is not available from Visual Studio, alternatively you can visit hello [SSDT Download][SSDT Download] page toodownload and install SSDT manually.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c6ad-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c6ad-123">Next steps</span></span>
<span data-ttu-id="5c6ad-124">Teď, když máte nejnovější verzi rozšíření SSDT hello, jste připraveni příliš[připojit] [ connect] tooyour SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5c6ad-124">Now that you have hello latest version of SSDT, you are ready too[connect][connect] tooyour SQL Data Warehouse.</span></span>

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Stáhněte si Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
