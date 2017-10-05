---
title: "Transparentní šifrování dat v SQL Data Warehouse (portál) | Microsoft Docs"
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
ms.openlocfilehash: b1db3bdfdfb54bda325c9b971cfcb4dd5efa333a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a><span data-ttu-id="312d2-103">Začínáme s transparentní dat šifrování (TDE) v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="312d2-103">Get started with Transparent Data Encryption (TDE) in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="312d2-104">Přehled zabezpečení</span><span class="sxs-lookup"><span data-stu-id="312d2-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="312d2-105">Ověřování</span><span class="sxs-lookup"><span data-stu-id="312d2-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="312d2-106">Šifrování (portál)</span><span class="sxs-lookup"><span data-stu-id="312d2-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="312d2-107">Šifrování (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="312d2-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="312d2-108">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="312d2-108">Required Permssions</span></span>
<span data-ttu-id="312d2-109">Pokud chcete povolit šifrování transparentní dat (TDE), musíte být správce nebo člen dbmanager role.</span><span class="sxs-lookup"><span data-stu-id="312d2-109">To enable Transparent Data Encryption (TDE), you must be an administrator or a member of the dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="312d2-110">Povolení šifrování</span><span class="sxs-lookup"><span data-stu-id="312d2-110">Enabling Encryption</span></span>
<span data-ttu-id="312d2-111">Pokud chcete povolit šifrování TDE pro SQL Data Warehouse, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="312d2-111">To enable TDE for a SQL Data Warehouse, follow the steps below:</span></span>

1. <span data-ttu-id="312d2-112">Otevřít v databázi [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="312d2-112">Open the database in the [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="312d2-113">V okně databáze klikněte na **nastavení** tlačítko</span><span class="sxs-lookup"><span data-stu-id="312d2-113">In the database blade, click the **Settings** button</span></span>
3. <span data-ttu-id="312d2-114">Vyberte **transparentní šifrování dat** možnost![][1]</span><span class="sxs-lookup"><span data-stu-id="312d2-114">Select the **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="312d2-115">Vyberte **na** nastavení![][2]</span><span class="sxs-lookup"><span data-stu-id="312d2-115">Select the **On** setting ![][2]</span></span>
5. <span data-ttu-id="312d2-116">Vyberte **uložit**
   ![][3]</span><span class="sxs-lookup"><span data-stu-id="312d2-116">Select **Save**
![][3]</span></span>  

## <a name="disabling-encryption"></a><span data-ttu-id="312d2-117">Zakázáním šifrování</span><span class="sxs-lookup"><span data-stu-id="312d2-117">Disabling Encryption</span></span>
<span data-ttu-id="312d2-118">Zakázat šifrování TDE pro SQL Data Warehouse, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="312d2-118">To disable TDE for a SQL Data Warehouse, follow the steps below:</span></span>

1. <span data-ttu-id="312d2-119">Otevřít v databázi [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="312d2-119">Open the database in the [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="312d2-120">V okně databáze klikněte na **nastavení** tlačítko</span><span class="sxs-lookup"><span data-stu-id="312d2-120">In the database blade, click the **Settings** button</span></span>
3. <span data-ttu-id="312d2-121">Vyberte **transparentní šifrování dat** možnost![][1]</span><span class="sxs-lookup"><span data-stu-id="312d2-121">Select the **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="312d2-122">Vyberte **vypnout** nastavení![][4]</span><span class="sxs-lookup"><span data-stu-id="312d2-122">Select the **Off** setting ![][4]</span></span>
5. <span data-ttu-id="312d2-123">Vyberte **uložit**
   ![][5]</span><span class="sxs-lookup"><span data-stu-id="312d2-123">Select **Save**
![][5]</span></span>  

## <a name="encryption-dmvs"></a><span data-ttu-id="312d2-124">Šifrování zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="312d2-124">Encryption DMVs</span></span>
<span data-ttu-id="312d2-125">Šifrování lze potvrdit s následující zobrazení dynamické správy:</span><span class="sxs-lookup"><span data-stu-id="312d2-125">Encryption can be confirmed with the following DMVs:</span></span>

* <span data-ttu-id="312d2-126">[zobrazení Sys.Databases]</span><span class="sxs-lookup"><span data-stu-id="312d2-126">[sys.databases]</span></span>
* <span data-ttu-id="312d2-127">[Sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="312d2-127">[sys.dm_pdw_nodes_database_encryption_keys]</span></span>

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
<span data-ttu-id="312d2-128">[zobrazení Sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx</span><span class="sxs-lookup"><span data-stu-id="312d2-128">[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx</span></span>
<span data-ttu-id="312d2-129">[Sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx</span><span class="sxs-lookup"><span data-stu-id="312d2-129">[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx</span></span>

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->