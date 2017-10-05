---
title: "Povolit automatické ladění pro Azure SQL Database | Microsoft Docs"
description: "Můžete povolit automatické ladění na vaší databázi SQL Azure, snadno."
services: sql-database
documentationcenter: 
author: vvasic
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 06/05/2016
ms.author: vvasic
ms.openlocfilehash: b391b1f7aa37c5a06fc320ce892534187deb4959
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-automatic-tuning"></a><span data-ttu-id="efa3d-103">Povolení automatického ladění</span><span class="sxs-lookup"><span data-stu-id="efa3d-103">Enable automatic tuning</span></span>

<span data-ttu-id="efa3d-104">Databáze SQL Azure je služba automaticky spravovaná data, která neustále monitoruje vaše dotazy a identifikuje akci, která umožňuje zvýšit výkon vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="efa3d-104">Azure SQL Database is an automatically managed data service that constantly monitors your queries and identifies the action that you can perform to improve performance of your workload.</span></span> <span data-ttu-id="efa3d-105">Můžete zkontrolovat doporučení a ručně je použít nebo to Azure SQL Database automaticky použít opravné akce – to se označuje jako **automatické ladění režimu**.</span><span class="sxs-lookup"><span data-stu-id="efa3d-105">You can review recommendations and manually apply them, or let Azure SQL Database automatically apply corrective actions - this is known as **automatic tuning mode**.</span></span> <span data-ttu-id="efa3d-106">Automatické ladění se dá nastavit na úrovni databáze nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="efa3d-106">Automatic tuning can be enabled at the server or the database level.</span></span>

## <a name="enable-automatic-tuning-on-server"></a><span data-ttu-id="efa3d-107">Povolit automatické ladění na serveru</span><span class="sxs-lookup"><span data-stu-id="efa3d-107">Enable automatic tuning on server</span></span>

<span data-ttu-id="efa3d-108">Chcete-li povolit automatické ladění na serveru Azure SQL Database, přejděte na server na portálu Azure a pak vyberte **automatické ladění** v nabídce.</span><span class="sxs-lookup"><span data-stu-id="efa3d-108">To enable automatic tuning on Azure SQL Database server, navigate to the server in Azure portal and then select **Automatic tuning** in the menu.</span></span> <span data-ttu-id="efa3d-109">Vyberte možnost Automatické ladění možnosti, které chcete povolit a vyberte **použít**:</span><span class="sxs-lookup"><span data-stu-id="efa3d-109">Select the automatic tuning options you want to enable and select **Apply**:</span></span>

![Server](./media/sql-database-automatic-tuning-enable/server.png)

<span data-ttu-id="efa3d-111">Automatické možnosti ladění na serveru se použijí pro všechny databáze na serveru.</span><span class="sxs-lookup"><span data-stu-id="efa3d-111">Automatic tuning options on server are applied to all databases on the server.</span></span> <span data-ttu-id="efa3d-112">Ve výchozím nastavení všechny databáze Zdědit konfiguraci z jejich nadřízeného serveru, ale to můžete přepsat a zadat pro každou databázi jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="efa3d-112">By default, all databases inherit the configuration from their parent server, but this can be overridden and specified for each database individually.</span></span>

## <a name="configure-automatic-tuning-on-database"></a><span data-ttu-id="efa3d-113">Konfigurovat automatické ladění databáze</span><span class="sxs-lookup"><span data-stu-id="efa3d-113">Configure automatic tuning on database</span></span>

<span data-ttu-id="efa3d-114">Portál Azure umožňuje jednotlivě zadat automatickou konfiguraci ladění na každou databázi.</span><span class="sxs-lookup"><span data-stu-id="efa3d-114">The Azure portal enables you to individually specify the automatic tuning configuration on each database.</span></span>

> [!NOTE]
> <span data-ttu-id="efa3d-115">Obecné doporučení je spravovat konfiguraci automatické ladění na úrovni serveru, aby stejné nastavení konfigurace můžete použít na každou databázi automaticky.</span><span class="sxs-lookup"><span data-stu-id="efa3d-115">The general recommendation is to manage the automatic tuning configuration at server level so the same configuration settings can be applied on every database automatically.</span></span> <span data-ttu-id="efa3d-116">Konfigurujte automatické ladění na jednotlivé databáze Pokud databázi se liší jiné na stejném serveru.</span><span class="sxs-lookup"><span data-stu-id="efa3d-116">Configure automatic tuning on an individual database if the database is different that others on the same server.</span></span>
>

<span data-ttu-id="efa3d-117">Pokud chcete povolit automatické ladění na jedné databáze, přejděte do databáze na portálu Azure a pak vyberte **automatické ladění**.</span><span class="sxs-lookup"><span data-stu-id="efa3d-117">To enable automatic tuning on a single database, navigate to the database in the Azure portal and then and select **Automatic tuning**.</span></span> <span data-ttu-id="efa3d-118">Můžete nakonfigurovat jednu databázi dědit nastavení z databáze a zaškrtnutím políčka nebo můžete zadat konfiguraci pro databázi jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="efa3d-118">You can configure a single database to inherit the settings from the database by selecting the checkbox or you can specify the configuration for a database individually.</span></span>

![Databáze](./media/sql-database-automatic-tuning-enable/database.png)

<span data-ttu-id="efa3d-120">Jakmile vyberete odpovídající konfiguraci, klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="efa3d-120">Once you have selected appropriate configuration, click **Apply**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="efa3d-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="efa3d-121">Next steps</span></span>
* <span data-ttu-id="efa3d-122">Pro čtení [automatické ladění článku](sql-database-automatic-tuning.md) Další informace o automatické ladění a jak se vám může pomoct zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="efa3d-122">Read the [Automatic tuning article](sql-database-automatic-tuning.md) to learn more about automatic tuning and how it can help you improve your performance.</span></span>
* <span data-ttu-id="efa3d-123">V tématu [výkonu doporučení](sql-database-advisor.md) přehled o výkonu doporučení Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="efa3d-123">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="efa3d-124">V tématu [Query Performance Insight](sql-database-query-performance.md) Další informace o zobrazení dopad na výkon nejčastějších dotazů.</span><span class="sxs-lookup"><span data-stu-id="efa3d-124">See [Query Performance Insights](sql-database-query-performance.md) to learn about viewing the performance impact of your top queries.</span></span>
