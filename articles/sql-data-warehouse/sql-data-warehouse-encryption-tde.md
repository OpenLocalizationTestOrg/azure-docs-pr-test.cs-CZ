---
title: "aaaTransparent šifrování dat v SQL Data Warehouse (portál) | Microsoft Docs"
description: "Transparentní šifrování dat (šifrování TDE) v SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: fabf75d3-9bbf-4e0d-9b31-8b5a8713f08d
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 8233886ecf170844104e0d1459e2a829cafa9b8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a><span data-ttu-id="fdf5f-103">Začínáme s transparentní dat šifrování (TDE) v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="fdf5f-103">Get started with Transparent Data Encryption (TDE) in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fdf5f-104">Přehled zabezpečení</span><span class="sxs-lookup"><span data-stu-id="fdf5f-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="fdf5f-105">Ověřování</span><span class="sxs-lookup"><span data-stu-id="fdf5f-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="fdf5f-106">Šifrování (portál)</span><span class="sxs-lookup"><span data-stu-id="fdf5f-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="fdf5f-107">Šifrování (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="fdf5f-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="fdf5f-108">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="fdf5f-108">Required Permssions</span></span>
<span data-ttu-id="fdf5f-109">tooenable transparentní dat šifrování (šifrování TDE), musíte být správce nebo člen hello dbmanager role.</span><span class="sxs-lookup"><span data-stu-id="fdf5f-109">tooenable Transparent Data Encryption (TDE), you must be an administrator or a member of hello dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="fdf5f-110">Povolení šifrování</span><span class="sxs-lookup"><span data-stu-id="fdf5f-110">Enabling Encryption</span></span>
<span data-ttu-id="fdf5f-111">tooenable TDE pro SQL Data Warehouse, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="fdf5f-111">tooenable TDE for a SQL Data Warehouse, follow hello steps below:</span></span>

1. <span data-ttu-id="fdf5f-112">Otevřete hello databáze v hello [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="fdf5f-112">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="fdf5f-113">V okně databáze hello, klikněte na tlačítko hello **nastavení** tlačítko</span><span class="sxs-lookup"><span data-stu-id="fdf5f-113">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="fdf5f-114">Vyberte hello **transparentní šifrování dat** možnost![][1]</span><span class="sxs-lookup"><span data-stu-id="fdf5f-114">Select hello **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="fdf5f-115">Vyberte hello **na** nastavení![][2]</span><span class="sxs-lookup"><span data-stu-id="fdf5f-115">Select hello **On** setting ![][2]</span></span>
5. <span data-ttu-id="fdf5f-116">Vyberte **uložit**
   ![][3]</span><span class="sxs-lookup"><span data-stu-id="fdf5f-116">Select **Save**
![][3]</span></span>  

## <a name="disabling-encryption"></a><span data-ttu-id="fdf5f-117">Zakázáním šifrování</span><span class="sxs-lookup"><span data-stu-id="fdf5f-117">Disabling Encryption</span></span>
<span data-ttu-id="fdf5f-118">toodisable TDE pro SQL Data Warehouse, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="fdf5f-118">toodisable TDE for a SQL Data Warehouse, follow hello steps below:</span></span>

1. <span data-ttu-id="fdf5f-119">Otevřete hello databáze v hello [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="fdf5f-119">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="fdf5f-120">V okně databáze hello, klikněte na tlačítko hello **nastavení** tlačítko</span><span class="sxs-lookup"><span data-stu-id="fdf5f-120">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="fdf5f-121">Vyberte hello **transparentní šifrování dat** možnost![][1]</span><span class="sxs-lookup"><span data-stu-id="fdf5f-121">Select hello **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="fdf5f-122">Vyberte hello **vypnout** nastavení![][4]</span><span class="sxs-lookup"><span data-stu-id="fdf5f-122">Select hello **Off** setting ![][4]</span></span>
5. <span data-ttu-id="fdf5f-123">Vyberte **uložit**
   ![][5]</span><span class="sxs-lookup"><span data-stu-id="fdf5f-123">Select **Save**
![][5]</span></span>  

## <a name="encryption-dmvs"></a><span data-ttu-id="fdf5f-124">Šifrování zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="fdf5f-124">Encryption DMVs</span></span>
<span data-ttu-id="fdf5f-125">Šifrování lze potvrdit s hello následující zobrazení dynamické správy:</span><span class="sxs-lookup"><span data-stu-id="fdf5f-125">Encryption can be confirmed with hello following DMVs:</span></span>

* <span data-ttu-id="fdf5f-126">[zobrazení Sys.Databases]</span><span class="sxs-lookup"><span data-stu-id="fdf5f-126">[sys.databases]</span></span>
* <span data-ttu-id="fdf5f-127">[Sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="fdf5f-127">[sys.dm_pdw_nodes_database_encryption_keys]</span></span>

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[zobrazení Sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[Sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
