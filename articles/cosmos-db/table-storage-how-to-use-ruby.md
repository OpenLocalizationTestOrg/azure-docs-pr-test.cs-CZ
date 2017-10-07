---
title: "aaaHow toouse úložiště Azure Table z Ruby | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 2f9eb5a9160b551d6d1d198869787070c402b1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-ruby"></a>Jak toouse úložiště Azure Table z Ruby
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Table. Ukázky Hello jsou zapsány pomocí hello Ruby rozhraní API. Hello pokryté scénáře zahrnují **vytváření a odstraňování tabulek, vkládání a dotazování entity v tabulce**.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Vytvoření Ruby aplikace
Pokyny jak toocreate Ruby aplikace, najdete v části [Ruby, na které webové aplikace ve virtuálním počítači Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-tooaccess-storage"></a>Konfigurace vaší aplikace tooaccess úložiště
toouse Azure Storage, potřebujete toodownload a použití hello Ruby balíček azure, který obsahuje sadu knihoven pohodlí, které komunikují s hello REST úložiště služby.

### <a name="use-rubygems-tooobtain-hello-package"></a>Použijte RubyGems tooobtain hello balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).
2. Typ **gem instalace azure** v hello příkazového okna tooinstall hello gem a závislosti.

### <a name="import-hello-package"></a>Importovat balíček hello
Použít svém oblíbeném textovém editoru, přidejte následující toohello horní části hello Ruby souboru, kde chcete toouse úložiště hello:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k Azure Storage
Hello modulu azure, bude číst proměnné prostředí hello **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_přístup\_klíč**informace vyžaduje účet služby Azure Storage tooyour tooconnect. Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu hello před použitím **Azure::TableService** s hello následující kód:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

tooobtain tyto hodnoty ze Správce prostředků úložiště nebo klasický účet v hello portálu Azure:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Přejděte toohello úložiště účet, že který má toouse.
3. V okně Nastavení hello na hello správné, klikněte na tlačítko **přístupové klíče**.
4. V okně klíče přístup hello který se zobrazí uvidíte hello přístupový klíč 1 a 2 přístupový klíč. Můžete použít kteroukoli z nich.
5. Klikněte na tlačítko hello kopírování ikonu toocopy hello klíče toohello schránky.

## <a name="create-a-table"></a>Vytvoření tabulky
Hello **Azure::TableService** objekt vám umožňuje spolupracovat s tabulkami a entity. toocreate tabulky, použijte hello **vytvořit\_table()** metoda. Hello následující příklad vytvoří tabulku nebo výtisků hello chyby, pokud existuje.

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-tooa-table"></a>Přidat tooa tabulka entity
tooadd entity, nejprve vytvořit objekt algoritmu hash, která definuje vlastnosti vaší entity. Všimněte si, že pro každou entitu musíte zadat **PartitionKey** a **RowKey**. Tyto jsou hello jedinečné identifikátory vaší entit a jsou hodnoty, které může být dotazován mnohem rychleji než vaše jiných vlastností. Azure Storage používá **PartitionKey** tooautomatically distribuovat entity tabulky hello přes mnoho uzly úložiště. Entity s hello stejné **PartitionKey** jsou uložené na hello stejného uzlu. Hello **RowKey** je jedinečné ID hello hello entity v rámci oddílu hello patří do.

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>Aktualizace entity
Existuje několik metod k dispozici tooupdate stávající entity:

* **Aktualizovat\_entity():** aktualizace stávající entity nahrazením ho.
* **sloučení\_entity():** aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity hello.
* **Vložit\_nebo\_sloučení\_entity():** aktualizace stávající entity podle jeho nahrazení. Pokud žádná entita existuje, bude vložen nový:
* **Vložit\_nebo\_nahradit\_entity():** aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity hello. Pokud existuje žádné entity, se vloží novou.

Hello následující příklad ukazuje, aktualizuje entitu s využitím **aktualizace\_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

S **aktualizace\_entity()** a **sloučení\_entity()**, pokud neexistuje hello entita, která jsou aktualizace, hello operace aktualizace se nezdaří. Proto pokud chcete toostore entity bez ohledu na to, jestli už existuje, měli byste místo toho použít **vložit\_nebo\_nahradit\_entity()** nebo **vložit\_nebo \_sloučení\_entity()**.

## <a name="work-with-groups-of-entities"></a>Práce se skupinami entit
Někdy má smysl toosubmit více operací společně v batch tooensure atomic zpracování serverem hello. tooaccomplish, jestli jste nejdřív vytvořili **Batch** objekt a potom pomocí hello **provést\_batch()** metodu **TableService**. Hello následující příklad ukazuje odesílání dvě entity s RowKey 2 a 3 v dávce. Všimněte si to jen funguje u entit s hello stejný klíč oddílu.

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>Dotaz pro entitu
tooquery entitu v tabulce, použijte hello **získat\_entity()** metoda předáním hello název tabulky, **PartitionKey** a **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Dotaz na sadu entit
tooquery sady entit v tabulce, vytvořit objekt algoritmu hash dotaz a použít hello **dotazu\_entities()** metoda. Hello následující příklad ukazuje získání všech entit hello s hello stejné **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Pokud sadu výsledků hello je příliš velký pro jeden dotaz tooreturn, token pokračování, bude vrácen který můžete použít následující stránky tooretrieve.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Dotaz na podmnožinu vlastností entity
Tabulku tooa dotazu můžete načíst jenom několik z entity. Tímto způsobem, nazývá "projekce" omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity. Použití klauzule select hello a názvy hello průchodu hello vlastnosti, které byste chtěli toobring přes toohello klienta.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Odstranění entity
toodelete entitu, použijte hello **odstranit\_entity()** metoda. Je nutné toopass v názvu hello hello tabulky obsahující hello entita, hello PartitionKey a RowKey hello entity.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Odstranění tabulky
toodelete tabulky, použijte hello **odstranit\_table()** metoda a předejte jí hello název tabulky, které chcete toodelete hello.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Další kroky

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.
* [Azure SDK pro Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu

