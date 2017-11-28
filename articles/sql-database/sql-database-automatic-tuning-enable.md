---
title: "aaaEnable automatické ladění pro Azure SQL Database | Microsoft Docs"
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
ms.openlocfilehash: af9da161eabc0f8c4cb100c050288f234efb8093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-automatic-tuning"></a><span data-ttu-id="b021a-103">Povolení automatického ladění</span><span class="sxs-lookup"><span data-stu-id="b021a-103">Enable automatic tuning</span></span>

<span data-ttu-id="b021a-104">Databáze SQL Azure je služba automaticky spravovaná data, která neustále monitoruje vaše dotazy a identifikuje hello akce, že můžete provádět tooimprove výkon vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="b021a-104">Azure SQL Database is an automatically managed data service that constantly monitors your queries and identifies hello action that you can perform tooimprove performance of your workload.</span></span> <span data-ttu-id="b021a-105">Můžete zkontrolovat doporučení a ručně je použít nebo to Azure SQL Database automaticky použít opravné akce – to se označuje jako **automatické ladění režimu**.</span><span class="sxs-lookup"><span data-stu-id="b021a-105">You can review recommendations and manually apply them, or let Azure SQL Database automatically apply corrective actions - this is known as **automatic tuning mode**.</span></span> <span data-ttu-id="b021a-106">Automatické ladění se dá nastavit na úrovni databáze hello nebo hello server.</span><span class="sxs-lookup"><span data-stu-id="b021a-106">Automatic tuning can be enabled at hello server or hello database level.</span></span>

## <a name="enable-automatic-tuning-on-server"></a><span data-ttu-id="b021a-107">Povolit automatické ladění na serveru</span><span class="sxs-lookup"><span data-stu-id="b021a-107">Enable automatic tuning on server</span></span>

<span data-ttu-id="b021a-108">tooenable automatické ladění na serveru Azure SQL Database, přejděte toohello server v Azure portálu a pak vyberte **automatické ladění** v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="b021a-108">tooenable automatic tuning on Azure SQL Database server, navigate toohello server in Azure portal and then select **Automatic tuning** in hello menu.</span></span> <span data-ttu-id="b021a-109">Vyberte hello automatické ladění možnosti tooenable a vyberte **použít**:</span><span class="sxs-lookup"><span data-stu-id="b021a-109">Select hello automatic tuning options you want tooenable and select **Apply**:</span></span>

![Server](./media/sql-database-automatic-tuning-enable/server.png)

<span data-ttu-id="b021a-111">Automatické ladění možnosti na serveru jsou použité tooall databáze na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="b021a-111">Automatic tuning options on server are applied tooall databases on hello server.</span></span> <span data-ttu-id="b021a-112">Ve výchozím nastavení všechny databáze Zdědit konfiguraci hello z jejich nadřízeného serveru, ale to můžete přepsat a zadat pro každou databázi jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="b021a-112">By default, all databases inherit hello configuration from their parent server, but this can be overridden and specified for each database individually.</span></span>

## <a name="configure-automatic-tuning-on-database"></a><span data-ttu-id="b021a-113">Konfigurovat automatické ladění databáze</span><span class="sxs-lookup"><span data-stu-id="b021a-113">Configure automatic tuning on database</span></span>

<span data-ttu-id="b021a-114">Hello Azure portálu umožňuje tooindividually můžete zadat automatickou konfiguraci ladění hello na každou databázi.</span><span class="sxs-lookup"><span data-stu-id="b021a-114">hello Azure portal enables you tooindividually specify hello automatic tuning configuration on each database.</span></span>

> [!NOTE]
> <span data-ttu-id="b021a-115">Obecné doporučení Hello je toomanage hello automatické ladění konfigurace na úrovni serveru Ano hello stejné nastavení konfigurace se dá použít na každou databázi automaticky.</span><span class="sxs-lookup"><span data-stu-id="b021a-115">hello general recommendation is toomanage hello automatic tuning configuration at server level so hello same configuration settings can be applied on every database automatically.</span></span> <span data-ttu-id="b021a-116">Konfigurujte automatické ladění na jednotlivé databáze Pokud hello databázi se liší, že jiné na stejný server hello.</span><span class="sxs-lookup"><span data-stu-id="b021a-116">Configure automatic tuning on an individual database if hello database is different that others on hello same server.</span></span>
>

<span data-ttu-id="b021a-117">toohello databáze v hello portálu Azure přejděte tooenable automatické ladění na jedné databáze a pak vyberte **automatické ladění**.</span><span class="sxs-lookup"><span data-stu-id="b021a-117">tooenable automatic tuning on a single database, navigate toohello database in hello Azure portal and then and select **Automatic tuning**.</span></span> <span data-ttu-id="b021a-118">Můžete nakonfigurovat jedné databáze tooinherit hello nastavení z databáze hello výběrem zaškrtávacího políčka hello nebo můžete zadat hello konfigurace pro databázi jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="b021a-118">You can configure a single database tooinherit hello settings from hello database by selecting hello checkbox or you can specify hello configuration for a database individually.</span></span>

![Databáze](./media/sql-database-automatic-tuning-enable/database.png)

<span data-ttu-id="b021a-120">Jakmile vyberete odpovídající konfiguraci, klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="b021a-120">Once you have selected appropriate configuration, click **Apply**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b021a-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b021a-121">Next steps</span></span>
* <span data-ttu-id="b021a-122">Čtení hello [automatické ladění článku](sql-database-automatic-tuning.md) toolearn Další informace o automatické ladění a jak se vám může pomoct zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="b021a-122">Read hello [Automatic tuning article](sql-database-automatic-tuning.md) toolearn more about automatic tuning and how it can help you improve your performance.</span></span>
* <span data-ttu-id="b021a-123">V tématu [výkonu doporučení](sql-database-advisor.md) přehled o výkonu doporučení Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b021a-123">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="b021a-124">V tématu [Query Performance Insight](sql-database-query-performance.md) toolearn o zobrazení hello vlivu na výkon nejčastějších dotazů.</span><span class="sxs-lookup"><span data-stu-id="b021a-124">See [Query Performance Insights](sql-database-query-performance.md) toolearn about viewing hello performance impact of your top queries.</span></span>
