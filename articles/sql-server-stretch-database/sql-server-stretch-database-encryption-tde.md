---
title: "Povolit transparentní šifrování dat pro funkce Stretch Database - Azure | Microsoft Docs"
description: "Povolit transparentní šifrování dat (šifrování TDE) pro SQL Server Stretch Database na Azure"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: barbkess
editor: 
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
ms.openlocfilehash: ceb355d2ba872ed5d3886c6dc82ca75b1854db0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a><span data-ttu-id="ad3ac-103">Povolit transparentní šifrování dat (šifrování TDE) pro funkce Stretch Database na Azure</span><span class="sxs-lookup"><span data-stu-id="ad3ac-103">Enable Transparent Data Encryption (TDE) for Stretch Database on Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ad3ac-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ad3ac-104">Azure portal</span></span>](sql-server-stretch-database-encryption-tde.md)
> * [<span data-ttu-id="ad3ac-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="ad3ac-105">TSQL</span></span>](sql-server-stretch-database-tde-tsql.md)
>
>

<span data-ttu-id="ad3ac-106">Transparentní šifrování šifrování dat (TDE) pomáhá chránit před ohrožením škodlivých aktivit provedením v reálném čase šifrování a dešifrování databáze, přidružených záloh a souborů protokolů transakci bez nutnosti změny aplikace.</span><span class="sxs-lookup"><span data-stu-id="ad3ac-106">Transparent Data Encryption (TDE) helps protect against the threat of malicious activity by performing real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application.</span></span>

<span data-ttu-id="ad3ac-107">Šifrování TDE zašifruje úložiště celé databáze pomocí symetrický klíč s názvem šifrovací klíč databáze.</span><span class="sxs-lookup"><span data-stu-id="ad3ac-107">TDE encrypts the storage of an entire database by using a symmetric key called the database encryption key.</span></span> <span data-ttu-id="ad3ac-108">Šifrovací klíč databáze je chráněn certifikát integrovaného serveru.</span><span class="sxs-lookup"><span data-stu-id="ad3ac-108">The database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="ad3ac-109">Certifikát integrovaného serveru je jedinečný pro každý server Azure.</span><span class="sxs-lookup"><span data-stu-id="ad3ac-109">The built-in server certificate is unique for each Azure server.</span></span> <span data-ttu-id="ad3ac-110">Microsoft automaticky otočí tyto certifikáty alespoň jednou za 90 dní.</span><span class="sxs-lookup"><span data-stu-id="ad3ac-110">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="ad3ac-111">Obecný popis TDE, najdete v části [transparentní šifrování šifrování dat (TDE)].</span><span class="sxs-lookup"><span data-stu-id="ad3ac-111">For a general description of TDE, see [Transparent Data Encryption (TDE)].</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="ad3ac-112">Povolení šifrování</span><span class="sxs-lookup"><span data-stu-id="ad3ac-112">Enabling Encryption</span></span>
<span data-ttu-id="ad3ac-113">Chcete-li povolit šifrování TDE Azure migrovat databázi, která ukládá data z databáze serveru SQL povolenou funkcí Stretch, provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="ad3ac-113">To enable TDE for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="ad3ac-114">Otevřít v databázi [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="ad3ac-114">Open the database in the [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="ad3ac-115">V okně databáze klikněte na **nastavení** tlačítko</span><span class="sxs-lookup"><span data-stu-id="ad3ac-115">In the database blade, click the **Settings** button</span></span>
3. <span data-ttu-id="ad3ac-116">Vyberte **transparentní šifrování dat** možnost![][1]</span><span class="sxs-lookup"><span data-stu-id="ad3ac-116">Select the **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="ad3ac-117">Vyberte **na** nastavení a potom vyberte **uložit**
   ![][2]</span><span class="sxs-lookup"><span data-stu-id="ad3ac-117">Select the **On** setting, and then select **Save**
![][2]</span></span>

## <a name="disabling-encryption"></a><span data-ttu-id="ad3ac-118">Zakázáním šifrování</span><span class="sxs-lookup"><span data-stu-id="ad3ac-118">Disabling Encryption</span></span>
<span data-ttu-id="ad3ac-119">Zakázat šifrování TDE Azure migrovat databázi, která ukládá data z databáze povolenou funkcí Stretch SQL Server, provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="ad3ac-119">To disable TDE for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="ad3ac-120">Otevřít v databázi [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="ad3ac-120">Open the database in the [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="ad3ac-121">V okně databáze klikněte na **nastavení** tlačítko</span><span class="sxs-lookup"><span data-stu-id="ad3ac-121">In the database blade, click the **Settings** button</span></span>
3. <span data-ttu-id="ad3ac-122">Vyberte **transparentní šifrování dat** možnost</span><span class="sxs-lookup"><span data-stu-id="ad3ac-122">Select the **Transparent data encryption** option</span></span>
4. <span data-ttu-id="ad3ac-123">Vyberte **vypnout** nastavení a potom vyberte **uložit**</span><span class="sxs-lookup"><span data-stu-id="ad3ac-123">Select the **Off** setting, and then select **Save**</span></span>

<!--Anchors-->
<span data-ttu-id="ad3ac-124">[transparentní šifrování šifrování dat (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx</span><span class="sxs-lookup"><span data-stu-id="ad3ac-124">[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx</span></span>


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
