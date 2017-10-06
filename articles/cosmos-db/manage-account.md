---
title: "aaaManage účet Azure Cosmos DB prostřednictvím hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage vaše Azure DB Cosmos účet prostřednictvím hello portálu Azure. Najít průvodce na pomocí tooview hello portálu Azure, kopírování, odstranění a přístup k účtům."
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
ms.openlocfilehash: 77ad953cf558a519674be08ad913a12202f69496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-an-azure-cosmos-db-account"></a><span data-ttu-id="9bb38-105">Jak toomanage účet Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9bb38-105">How toomanage an Azure Cosmos DB account</span></span>
<span data-ttu-id="9bb38-106">Zjistěte, jak pracovat s klíči tooset globální konzistence a odstranit účet Azure Cosmos DB v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9bb38-106">Learn how tooset global consistency, work with keys, and delete an Azure Cosmos DB account in hello Azure portal.</span></span>

## <span data-ttu-id="9bb38-107"><a id="consistency"></a>Spravovat nastavení konzistence Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9bb38-107"><a id="consistency"></a>Manage Azure Cosmos DB consistency settings</span></span>
<span data-ttu-id="9bb38-108">Výběr úrovně konzistence správné hello závisí na sémantiku hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9bb38-108">Selecting hello right consistency level depends on hello semantics of your application.</span></span> <span data-ttu-id="9bb38-109">Byste si měli přečíst hello úrovně konzistence dostupné v Azure Cosmos DB načtením [pomocí konzistence úrovně toomaximize dostupnosti a výkonu v Azure Cosmos DB][consistency].</span><span class="sxs-lookup"><span data-stu-id="9bb38-109">You should familiarize yourself with hello available consistency levels in Azure Cosmos DB by reading [Using consistency levels toomaximize availability and performance in Azure Cosmos DB][consistency].</span></span> <span data-ttu-id="9bb38-110">Azure Cosmos DB poskytuje konzistence, dostupnosti a výkonu záruky, na všech úrovních konzistence k dispozici pro váš účet databáze.</span><span class="sxs-lookup"><span data-stu-id="9bb38-110">Azure Cosmos DB provides consistency, availability, and performance guarantees, at every consistency level available for your database account.</span></span> <span data-ttu-id="9bb38-111">Konfigurace účtu databáze s úrovní konzistence silným vyžaduje, aby vaše data uzavřeného tooa jedné oblasti Azure a globálně dostupnou.</span><span class="sxs-lookup"><span data-stu-id="9bb38-111">Configuring your database account with a consistency level of Strong requires that your data is confined tooa single Azure region and not globally available.</span></span> <span data-ttu-id="9bb38-112">Na druhé straně text hello, hello úrovně konzistence volný - typu s ohraničenou prošlostí, session nebo případné povolit jste tooassociate libovolný počet oblastí Azure s vaším účtem databáze.</span><span class="sxs-lookup"><span data-stu-id="9bb38-112">On hello other hand, hello relaxed consistency levels - bounded staleness, session or eventual enable you tooassociate any number of Azure regions with your database account.</span></span> <span data-ttu-id="9bb38-113">Hello následující jednoduchými kroky vám ukážou, jak tooselect hello výchozí úroveň konzistence pro váš účet databáze.</span><span class="sxs-lookup"><span data-stu-id="9bb38-113">hello following simple steps show you how tooselect hello default consistency level for your database account.</span></span> 

### <a name="toospecify-hello-default-consistency-for-an-azure-cosmos-db-account"></a><span data-ttu-id="9bb38-114">toospecify hello výchozí konzistence účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9bb38-114">toospecify hello default consistency for an Azure Cosmos DB account</span></span>
1. <span data-ttu-id="9bb38-115">V hello [portál Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bb38-115">In hello [Azure portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="9bb38-116">V okně účtu hello, klikněte na tlačítko **výchozí konzistence**.</span><span class="sxs-lookup"><span data-stu-id="9bb38-116">In hello account blade, click **Default consistency**.</span></span>
3. <span data-ttu-id="9bb38-117">V hello **výchozí konzistence** , vyberte hello novou úroveň konzistence a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9bb38-117">In hello **Default Consistency** blade, select hello new consistency level and click **Save**.</span></span>
    <span data-ttu-id="9bb38-118">![Výchozí konzistence relace][5]</span><span class="sxs-lookup"><span data-stu-id="9bb38-118">![Default consistency session][5]</span></span>

## <span data-ttu-id="9bb38-119"><a id="keys"></a>Zobrazení, kopírování a opětovné vytváření přístupových klíčů</span><span class="sxs-lookup"><span data-stu-id="9bb38-119"><a id="keys"></a>View, copy, and regenerate access keys</span></span>
<span data-ttu-id="9bb38-120">Při vytváření účtu Azure Cosmos DB hello služba generuje dva hlavní přístupové klíče, které lze použít pro ověření při přístupu k účtu Azure Cosmos DB hello.</span><span class="sxs-lookup"><span data-stu-id="9bb38-120">When you create an Azure Cosmos DB account, hello service generates two master access keys that can be used for authentication when hello Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="9bb38-121">Poskytnutím dvou přístupových klíčů Azure Cosmos DB vám umožní tooregenerate hello klíče bez přerušení tooyour účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bb38-121">By providing two access keys, Azure Cosmos DB enables you tooregenerate hello keys with no interruption tooyour Azure Cosmos DB account.</span></span> 

<span data-ttu-id="9bb38-122">V hello [portál Azure](https://portal.azure.com/), hello přístup **klíče** okno na v nabídce hello prostředků hello **účet Azure Cosmos DB** okno tooview, kopírování a opětovné vytváření hello přístupových klíčů, který jsou použité tooaccess účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bb38-122">In hello [Azure portal](https://portal.azure.com/), access hello **Keys** blade from hello resource menu on hello **Azure Cosmos DB account** blade tooview, copy, and regenerate hello access keys that are used tooaccess your Azure Cosmos DB account.</span></span>

![Azure Portal snímku obrazovky okna klíče](./media/manage-account/keys.png)

> [!NOTE]
> <span data-ttu-id="9bb38-124">Hello **klíče** okno také obsahuje primární a sekundární připojovací řetězce, které se dají použít tooconnect tooyour účet z hello [nástroj pro migraci dat](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="9bb38-124">hello **Keys** blade also includes primary and secondary connection strings that can be used tooconnect tooyour account from hello [Data Migration Tool](import-data.md).</span></span>
> 
> 

<span data-ttu-id="9bb38-125">Klíče jen pro čtení jsou také k dispozici v tomto okně.</span><span class="sxs-lookup"><span data-stu-id="9bb38-125">Read-only keys are also available on this blade.</span></span> <span data-ttu-id="9bb38-126">Čtení a dotazy jsou jen pro čtení operace, než vytvoří, odstranění, a nahradí nejsou.</span><span class="sxs-lookup"><span data-stu-id="9bb38-126">Reads and queries are read-only operations, while creates, deletes, and replaces are not.</span></span>

### <a name="copy-an-access-key-in-hello-azure-portal"></a><span data-ttu-id="9bb38-127">Zkopírovat přístupový klíč v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9bb38-127">Copy an access key in hello Azure Portal</span></span>
<span data-ttu-id="9bb38-128">Na hello **klíče** okně klikněte na tlačítko hello **kopie** toohello tlačítko napravo od hello klíč chcete toocopy.</span><span class="sxs-lookup"><span data-stu-id="9bb38-128">On hello **Keys** blade, click hello **Copy** button toohello right of hello key you wish toocopy.</span></span>

![Zobrazení a zkopírovat přístupový klíč v hello portálu Azure, okna klíče](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a><span data-ttu-id="9bb38-130">Opětovné vygenerování přístupových klíčů</span><span class="sxs-lookup"><span data-stu-id="9bb38-130">Regenerate access keys</span></span>
<span data-ttu-id="9bb38-131">Měli byste změnit účet Azure Cosmos DB hello přístupu k klíče tooyour pravidelně toohelp připojení lépe zabezpečit.</span><span class="sxs-lookup"><span data-stu-id="9bb38-131">You should change hello access keys tooyour Azure Cosmos DB account periodically toohelp keep your connections more secure.</span></span> <span data-ttu-id="9bb38-132">Dva přístupové klíče se přiřazují tooenable hello je toomaintain připojení toohello účet Azure Cosmos DB používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="9bb38-132">Two access keys are assigned tooenable you toomaintain connections toohello Azure Cosmos DB account using one access key while you regenerate hello other access key.</span></span>

> [!WARNING]
> <span data-ttu-id="9bb38-133">Opětovné generování přístupových klíčů ovlivní všechny aplikace, které jsou závislé na aktuální klíč hello.</span><span class="sxs-lookup"><span data-stu-id="9bb38-133">Regenerating your access keys affects any applications that are dependent on hello current key.</span></span> <span data-ttu-id="9bb38-134">Všichni klienti, kteří používají účet Azure Cosmos DB hello přístupu k klíče tooaccess hello musí být aktualizované toouse hello nový klíč.</span><span class="sxs-lookup"><span data-stu-id="9bb38-134">All clients that use hello access key tooaccess hello Azure Cosmos DB account must be updated toouse hello new key.</span></span>
> 
> 

<span data-ttu-id="9bb38-135">Pokud máte aplikace nebo cloudové služby pomocí účtu Azure Cosmos DB hello, pokud, ztratíte hello připojení obnovit klíče, klíče nezaregistrujete.</span><span class="sxs-lookup"><span data-stu-id="9bb38-135">If you have applications or cloud services using hello Azure Cosmos DB account, you will lose hello connections if you regenerate keys, unless you roll your keys.</span></span> <span data-ttu-id="9bb38-136">Hello následující kroky popisují proces hello účastní vrácení klíče.</span><span class="sxs-lookup"><span data-stu-id="9bb38-136">hello following steps outline hello process involved in rolling your keys.</span></span>

1. <span data-ttu-id="9bb38-137">Aktualizace hello přístupový klíč ve vaší aplikaci kód tooreference hello sekundární přístupový klíč hello účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bb38-137">Update hello access key in your application code tooreference hello secondary access key of hello Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="9bb38-138">Znovu vygenerujte primární přístupový klíč hello pro váš účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bb38-138">Regenerate hello primary access key for your Azure Cosmos DB account.</span></span> <span data-ttu-id="9bb38-139">V hello [portálu Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bb38-139">In hello [Azure Portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
3. <span data-ttu-id="9bb38-140">V hello **Azure Cosmos DB účet** okně klikněte na tlačítko **klíče**.</span><span class="sxs-lookup"><span data-stu-id="9bb38-140">In hello **Azure Cosmos DB Account** blade, click **Keys**.</span></span>
4. <span data-ttu-id="9bb38-141">Na hello **klíče** okně klikněte na tlačítko znovu generovat hello a pak klikněte na **Ok** tooconfirm, které chcete toogenerate nový klíč.</span><span class="sxs-lookup"><span data-stu-id="9bb38-141">On hello **Keys** blade, click hello regenerate button, then click **Ok** tooconfirm that you want toogenerate a new key.</span></span>
    <span data-ttu-id="9bb38-142">![Opětovné vygenerování přístupových klíčů](./media/manage-account/regenerate-keys.png)</span><span class="sxs-lookup"><span data-stu-id="9bb38-142">![Regenerate access keys](./media/manage-account/regenerate-keys.png)</span></span>
5. <span data-ttu-id="9bb38-143">Jakmile si ověříte, že je k dispozici pro použití této hello nový klíč (přibližně 5 minut po opětovné generování), aktualizujte hello přístupový klíč ve vaší aplikaci kód tooreference hello nové primární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="9bb38-143">Once you have verified that hello new key is available for use (approximately 5 minutes after regeneration), update hello access key in your application code tooreference hello new primary access key.</span></span>
6. <span data-ttu-id="9bb38-144">Znova vygenerujte sekundární přístupový klíč hello.</span><span class="sxs-lookup"><span data-stu-id="9bb38-144">Regenerate hello secondary access key.</span></span>
   
    ![Opětovné vygenerování přístupových klíčů](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> <span data-ttu-id="9bb38-146">Může trvat několik minut, než nově vygenerovaný klíč může být použité tooaccess účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bb38-146">It can take several minutes before a newly generated key can be used tooaccess your Azure Cosmos DB account.</span></span>
> 
> 

## <a name="get-hello--connection-string"></a><span data-ttu-id="9bb38-147">Získat připojovací řetězec hello</span><span class="sxs-lookup"><span data-stu-id="9bb38-147">Get hello  connection string</span></span>
<span data-ttu-id="9bb38-148">tooretrieve váš připojovací řetězec, hello následující:</span><span class="sxs-lookup"><span data-stu-id="9bb38-148">tooretrieve your connection string, do hello following:</span></span> 

1. <span data-ttu-id="9bb38-149">V hello [portál Azure](https://portal.azure.com), přístup k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bb38-149">In hello [Azure portal](https://portal.azure.com), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="9bb38-150">V nabídce hello prostředků, klikněte na tlačítko **klíče**.</span><span class="sxs-lookup"><span data-stu-id="9bb38-150">In hello resource menu, click **Keys**.</span></span>
3. <span data-ttu-id="9bb38-151">Klikněte na tlačítko hello **kopie** tlačítko Další toohello **primární připojovací řetězec** nebo **sekundární připojovací řetězec** pole.</span><span class="sxs-lookup"><span data-stu-id="9bb38-151">Click hello **Copy** button next toohello **Primary Connection String** or **Secondary Connection String** box.</span></span> 

<span data-ttu-id="9bb38-152">Pokud používáte hello připojovací řetězec v hello [nástroj pro migraci databáze Azure Cosmos DB](import-data.md), připojit hello databáze název toohello konec hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="9bb38-152">If you are using hello connection string in hello [Azure Cosmos DB Database Migration Tool](import-data.md), append hello database name toohello end of hello connection string.</span></span> <span data-ttu-id="9bb38-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span><span class="sxs-lookup"><span data-stu-id="9bb38-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span></span>

## <span data-ttu-id="9bb38-154"><a id="delete"></a>Odstranit účet Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9bb38-154"><a id="delete"></a> Delete an Azure Cosmos DB account</span></span>
<span data-ttu-id="9bb38-155">tooremove Azure DB Cosmos účet z hello Azure portál, který už používáte, klikněte pravým tlačítkem na název účtu hello a klikněte na tlačítko **odstranit účet**.</span><span class="sxs-lookup"><span data-stu-id="9bb38-155">tooremove an Azure Cosmos DB account from hello Azure Portal that you are no longer using, right-click hello account name, and click **Delete account**.</span></span>

![Jak toodelete Azure DB Cosmos účet v hello portálu Azure](./media/manage-account/deleteaccount.png)

1. <span data-ttu-id="9bb38-157">V hello [portál Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB hello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="9bb38-157">In hello [Azure portal](https://portal.azure.com/), access hello Azure Cosmos DB account you wish toodelete.</span></span>
2. <span data-ttu-id="9bb38-158">Na hello **účet Azure Cosmos DB** , klikněte pravým tlačítkem na účet hello a pak klikněte na tlačítko **odstranit účet**.</span><span class="sxs-lookup"><span data-stu-id="9bb38-158">On hello **Azure Cosmos DB account** blade, right-click hello account, and then click **Delete Account**.</span></span> 
3. <span data-ttu-id="9bb38-159">V okně potvrzení výsledné hello zadejte hello Azure Cosmos DB účet název tooconfirm, které chcete toodelete hello účtu.</span><span class="sxs-lookup"><span data-stu-id="9bb38-159">On hello resulting confirmation blade, type hello Azure Cosmos DB account name tooconfirm that you want toodelete hello account.</span></span>
4. <span data-ttu-id="9bb38-160">Klikněte na tlačítko hello **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9bb38-160">Click hello **Delete** button.</span></span>

![Jak toodelete Azure DB Cosmos účet v hello portálu Azure](./media/manage-account/delete-account-confirm.png)

## <span data-ttu-id="9bb38-162"><a id="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="9bb38-162"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="9bb38-163">Zjistěte, jak příliš[začít pracovat s vaším účtem Azure Cosmos DB](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span><span class="sxs-lookup"><span data-stu-id="9bb38-163">Learn how too[get started with your Azure Cosmos DB account](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span></span>

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
