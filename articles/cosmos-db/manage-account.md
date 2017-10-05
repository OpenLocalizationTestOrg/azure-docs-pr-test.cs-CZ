---
title: "Správa účtu Azure Cosmos DB prostřednictvím portálu Azure | Microsoft Docs"
description: "Zjistěte, jak spravovat váš účet Azure Cosmos DB prostřednictvím portálu Azure. Najít průvodce na pomocí portálu Azure k zobrazení, kopírování, odstranění a přístup k účtům."
keywords: "Portál Azure, azure documentdb, Microsoft azure"
services: cosmos-db
documentationcenter: 
author: kirillg
manager: jhubbard
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kirillg
ms.openlocfilehash: a0c6ec8d490e1adacc96758971ab91d8eaeab45c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a><span data-ttu-id="e11f1-105">Správa účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e11f1-105">How to manage an Azure Cosmos DB account</span></span>
<span data-ttu-id="e11f1-106">Zjistěte, jak nastavit globální konzistence, práce s klíči a odstranit účet Azure Cosmos DB na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e11f1-106">Learn how to set global consistency, work with keys, and delete an Azure Cosmos DB account in the Azure portal.</span></span>

## <span data-ttu-id="e11f1-107"><a id="consistency"></a>Spravovat nastavení konzistence Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e11f1-107"><a id="consistency"></a>Manage Azure Cosmos DB consistency settings</span></span>
<span data-ttu-id="e11f1-108">Výběr správné konzistence úroveň závisí na sémantiku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e11f1-108">Selecting the right consistency level depends on the semantics of your application.</span></span> <span data-ttu-id="e11f1-109">Byste si měli přečíst s úrovní konzistence k dispozici v Azure Cosmos DB načtením [využití úrovní konzistence pro maximalizaci dostupnosti a výkonu v Azure Cosmos DB][consistency].</span><span class="sxs-lookup"><span data-stu-id="e11f1-109">You should familiarize yourself with the available consistency levels in Azure Cosmos DB by reading [Using consistency levels to maximize availability and performance in Azure Cosmos DB][consistency].</span></span> <span data-ttu-id="e11f1-110">Azure Cosmos DB poskytuje konzistence, dostupnosti a výkonu záruky, na všech úrovních konzistence k dispozici pro váš účet databáze.</span><span class="sxs-lookup"><span data-stu-id="e11f1-110">Azure Cosmos DB provides consistency, availability, and performance guarantees, at every consistency level available for your database account.</span></span> <span data-ttu-id="e11f1-111">Konfigurace účtu databáze s úrovní konzistence silným vyžaduje, aby vaše data uzavřeného k jedné oblasti Azure a globálně dostupnou.</span><span class="sxs-lookup"><span data-stu-id="e11f1-111">Configuring your database account with a consistency level of Strong requires that your data is confined to a single Azure region and not globally available.</span></span> <span data-ttu-id="e11f1-112">Na druhé straně, úrovně volný konzistence - typu s ohraničenou prošlostí, session nebo případné povolení k přidružení libovolný počet oblastí Azure s vaším účtem databáze.</span><span class="sxs-lookup"><span data-stu-id="e11f1-112">On the other hand, the relaxed consistency levels - bounded staleness, session or eventual enable you to associate any number of Azure regions with your database account.</span></span> <span data-ttu-id="e11f1-113">Jednoduché následující kroky ukazují, jak vybrat výchozí úroveň konzistence pro váš účet databáze.</span><span class="sxs-lookup"><span data-stu-id="e11f1-113">The following simple steps show you how to select the default consistency level for your database account.</span></span> 

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a><span data-ttu-id="e11f1-114">Chcete-li určit výchozí konzistence účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e11f1-114">To specify the default consistency for an Azure Cosmos DB account</span></span>
1. <span data-ttu-id="e11f1-115">V [portál Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e11f1-115">In the [Azure portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="e11f1-116">V okně účtu klikněte na **výchozí konzistence**.</span><span class="sxs-lookup"><span data-stu-id="e11f1-116">In the account blade, click **Default consistency**.</span></span>
3. <span data-ttu-id="e11f1-117">V **výchozí konzistence** okně, vyberte novou úroveň konzistence a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e11f1-117">In the **Default Consistency** blade, select the new consistency level and click **Save**.</span></span>
    <span data-ttu-id="e11f1-118">![Výchozí konzistence relace][5]</span><span class="sxs-lookup"><span data-stu-id="e11f1-118">![Default consistency session][5]</span></span>

## <span data-ttu-id="e11f1-119"><a id="keys"></a>Zobrazení, kopírování a opětovné vytváření přístupových klíčů</span><span class="sxs-lookup"><span data-stu-id="e11f1-119"><a id="keys"></a>View, copy, and regenerate access keys</span></span>
<span data-ttu-id="e11f1-120">Když vytvoříte účet Azure Cosmos DB, generuje tato služba dva hlavní přístupové klíče, které lze použít pro ověření při přístupu k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e11f1-120">When you create an Azure Cosmos DB account, the service generates two master access keys that can be used for authentication when the Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="e11f1-121">Poskytnutím dvou přístupových klíčů Azure Cosmos DB umožňuje znovu vygenerovat klíče bez přerušení ke svému účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e11f1-121">By providing two access keys, Azure Cosmos DB enables you to regenerate the keys with no interruption to your Azure Cosmos DB account.</span></span> 

<span data-ttu-id="e11f1-122">V [portál Azure](https://portal.azure.com/), přístup **klíče** okno na v nabídce prostředků **účet Azure Cosmos DB** okna zobrazení, kopírování a opětovné vygenerování přístupových klíčů, které se používají pro přístup k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e11f1-122">In the [Azure portal](https://portal.azure.com/), access the **Keys** blade from the resource menu on the **Azure Cosmos DB account** blade to view, copy, and regenerate the access keys that are used to access your Azure Cosmos DB account.</span></span>

![Azure Portal snímku obrazovky okna klíče](./media/manage-account/keys.png)

> [!NOTE]
> <span data-ttu-id="e11f1-124">**Klíče** okno také obsahuje primární a sekundární připojovací řetězce, které lze použít pro připojení k účtu z [nástroj pro migraci dat](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="e11f1-124">The **Keys** blade also includes primary and secondary connection strings that can be used to connect to your account from the [Data Migration Tool](import-data.md).</span></span>
> 
> 

<span data-ttu-id="e11f1-125">Klíče jen pro čtení jsou také k dispozici v tomto okně.</span><span class="sxs-lookup"><span data-stu-id="e11f1-125">Read-only keys are also available on this blade.</span></span> <span data-ttu-id="e11f1-126">Čtení a dotazy jsou jen pro čtení operace, než vytvoří, odstranění, a nahradí nejsou.</span><span class="sxs-lookup"><span data-stu-id="e11f1-126">Reads and queries are read-only operations, while creates, deletes, and replaces are not.</span></span>

### <a name="copy-an-access-key-in-the-azure-portal"></a><span data-ttu-id="e11f1-127">Zkopírovat přístupový klíč na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e11f1-127">Copy an access key in the Azure Portal</span></span>
<span data-ttu-id="e11f1-128">Na **klíče** okně klikněte na tlačítko **kopie** tlačítku klíče, které chcete kopírovat.</span><span class="sxs-lookup"><span data-stu-id="e11f1-128">On the **Keys** blade, click the **Copy** button to the right of the key you wish to copy.</span></span>

![Zobrazení a zkopírování přístupového klíče na portálu Azure, okna klíče](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a><span data-ttu-id="e11f1-130">Opětovné vygenerování přístupových klíčů</span><span class="sxs-lookup"><span data-stu-id="e11f1-130">Regenerate access keys</span></span>
<span data-ttu-id="e11f1-131">Měli byste změnit přístupové klíče k účtu Azure Cosmos DB pravidelně pro zvýšení zabezpečení připojení.</span><span class="sxs-lookup"><span data-stu-id="e11f1-131">You should change the access keys to your Azure Cosmos DB account periodically to help keep your connections more secure.</span></span> <span data-ttu-id="e11f1-132">Abyste mohli udržovat připojení k účtu Azure Cosmos DB používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč jsou přiřazeny dva přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="e11f1-132">Two access keys are assigned to enable you to maintain connections to the Azure Cosmos DB account using one access key while you regenerate the other access key.</span></span>

> [!WARNING]
> <span data-ttu-id="e11f1-133">Opětovné generování přístupových klíčů ovlivní všechny aplikace, které jsou závislé na aktuální klíč.</span><span class="sxs-lookup"><span data-stu-id="e11f1-133">Regenerating your access keys affects any applications that are dependent on the current key.</span></span> <span data-ttu-id="e11f1-134">Všichni klienti, kteří používají přístupový klíč pro přístup k účtu Azure Cosmos DB musí být aktualizovány na používání nového klíče.</span><span class="sxs-lookup"><span data-stu-id="e11f1-134">All clients that use the access key to access the Azure Cosmos DB account must be updated to use the new key.</span></span>
> 
> 

<span data-ttu-id="e11f1-135">Pokud máte aplikace nebo cloudové služby, pomocí účtu Azure Cosmos DB, ztratíte připojení Pokud obnovit klíče, klíče nezaregistrujete.</span><span class="sxs-lookup"><span data-stu-id="e11f1-135">If you have applications or cloud services using the Azure Cosmos DB account, you will lose the connections if you regenerate keys, unless you roll your keys.</span></span> <span data-ttu-id="e11f1-136">Následující kroky popisují proces účastní vrácení klíče.</span><span class="sxs-lookup"><span data-stu-id="e11f1-136">The following steps outline the process involved in rolling your keys.</span></span>

1. <span data-ttu-id="e11f1-137">Aktualizujte přístupový klíč v kódu aplikace tak, aby odkazovaly sekundární přístupový klíč účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e11f1-137">Update the access key in your application code to reference the secondary access key of the Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="e11f1-138">Znovu vygenerujte primární přístupový klíč pro účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e11f1-138">Regenerate the primary access key for your Azure Cosmos DB account.</span></span> <span data-ttu-id="e11f1-139">V [portálu Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e11f1-139">In the [Azure Portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
3. <span data-ttu-id="e11f1-140">V **Azure Cosmos DB účet** okně klikněte na tlačítko **klíče**.</span><span class="sxs-lookup"><span data-stu-id="e11f1-140">In the **Azure Cosmos DB Account** blade, click **Keys**.</span></span>
4. <span data-ttu-id="e11f1-141">Na **klíče** okně klikněte na tlačítko znovu generovat a pak klikněte na **Ok** potvrďte, že chcete vygenerovat nový klíč.</span><span class="sxs-lookup"><span data-stu-id="e11f1-141">On the **Keys** blade, click the regenerate button, then click **Ok** to confirm that you want to generate a new key.</span></span>
    <span data-ttu-id="e11f1-142">![Opětovné vygenerování přístupových klíčů](./media/manage-account/regenerate-keys.png)</span><span class="sxs-lookup"><span data-stu-id="e11f1-142">![Regenerate access keys](./media/manage-account/regenerate-keys.png)</span></span>
5. <span data-ttu-id="e11f1-143">Jakmile si ověříte, že nový klíč je k dispozici pro použití (přibližně 5 minut po opětovné generování), aktualizujte přístupový klíč v kódu aplikace tak, aby odkazovaly nový primární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="e11f1-143">Once you have verified that the new key is available for use (approximately 5 minutes after regeneration), update the access key in your application code to reference the new primary access key.</span></span>
6. <span data-ttu-id="e11f1-144">Stejným způsobem pak opětovně vygenerujte sekundární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="e11f1-144">Regenerate the secondary access key.</span></span>
   
    ![Opětovné vygenerování přístupových klíčů](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> <span data-ttu-id="e11f1-146">Může trvat několik minut, než nově vygenerovaný klíč můžete použít pro přístup k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e11f1-146">It can take several minutes before a newly generated key can be used to access your Azure Cosmos DB account.</span></span>
> 
> 

## <a name="get-the--connection-string"></a><span data-ttu-id="e11f1-147">Získat připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="e11f1-147">Get the  connection string</span></span>
<span data-ttu-id="e11f1-148">Pokud chcete načíst připojovací řetězec, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e11f1-148">To retrieve your connection string, do the following:</span></span> 

1. <span data-ttu-id="e11f1-149">V [portál Azure](https://portal.azure.com), přístup k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e11f1-149">In the [Azure portal](https://portal.azure.com), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="e11f1-150">V nabídce prostředků, klikněte na tlačítko **klíče**.</span><span class="sxs-lookup"><span data-stu-id="e11f1-150">In the resource menu, click **Keys**.</span></span>
3. <span data-ttu-id="e11f1-151">Klikněte **kopie** vedle položky **primární připojovací řetězec** nebo **sekundární připojovací řetězec** pole.</span><span class="sxs-lookup"><span data-stu-id="e11f1-151">Click the **Copy** button next to the **Primary Connection String** or **Secondary Connection String** box.</span></span> 

<span data-ttu-id="e11f1-152">Pokud používáte připojovací řetězec [nástroj pro migraci databáze Azure Cosmos DB](import-data.md), připojte na konec připojovací řetězec název databáze.</span><span class="sxs-lookup"><span data-stu-id="e11f1-152">If you are using the connection string in the [Azure Cosmos DB Database Migration Tool](import-data.md), append the database name to the end of the connection string.</span></span> <span data-ttu-id="e11f1-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span><span class="sxs-lookup"><span data-stu-id="e11f1-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span></span>

## <span data-ttu-id="e11f1-154"><a id="delete"></a>Odstranit účet Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e11f1-154"><a id="delete"></a> Delete an Azure Cosmos DB account</span></span>
<span data-ttu-id="e11f1-155">Chcete-li odebrat účet pro Azure Cosmos DB z portálu Azure, který už nepoužíváte, klikněte pravým tlačítkem na název účtu a klikněte na **odstranit účet**.</span><span class="sxs-lookup"><span data-stu-id="e11f1-155">To remove an Azure Cosmos DB account from the Azure Portal that you are no longer using, right-click the account name, and click **Delete account**.</span></span>

![Postup odstranění účtu Azure Cosmos DB na portálu Azure](./media/manage-account/deleteaccount.png)

1. <span data-ttu-id="e11f1-157">V [portál Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="e11f1-157">In the [Azure portal](https://portal.azure.com/), access the Azure Cosmos DB account you wish to delete.</span></span>
2. <span data-ttu-id="e11f1-158">Na **účet Azure Cosmos DB** , klikněte pravým tlačítkem na účet a pak klikněte na tlačítko **odstranit účet**.</span><span class="sxs-lookup"><span data-stu-id="e11f1-158">On the **Azure Cosmos DB account** blade, right-click the account, and then click **Delete Account**.</span></span> 
3. <span data-ttu-id="e11f1-159">V okně výsledné potvrzení zadejte název účtu Azure Cosmos DB potvrďte, že chcete odstranit účet.</span><span class="sxs-lookup"><span data-stu-id="e11f1-159">On the resulting confirmation blade, type the Azure Cosmos DB account name to confirm that you want to delete the account.</span></span>
4. <span data-ttu-id="e11f1-160">Klikněte **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e11f1-160">Click the **Delete** button.</span></span>

![Postup odstranění účtu Azure Cosmos DB na portálu Azure](./media/manage-account/delete-account-confirm.png)

## <span data-ttu-id="e11f1-162"><a id="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="e11f1-162"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="e11f1-163">Zjistěte, jak [začít pracovat s vaším účtem Azure Cosmos DB](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span><span class="sxs-lookup"><span data-stu-id="e11f1-163">Learn how to [get started with your Azure Cosmos DB account](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span></span>

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
