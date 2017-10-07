---
title: "aaaHow toouse úložiště Queue z Ruby | Microsoft Docs"
description: "Zjistěte, jak toouse hello fronty Azure service toocreate a odstranění fronty a vložení, získání a odstranění zprávy. Ukázky napsané v Ruby."
services: storage
documentationcenter: ruby
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 59c2d81b-db9c-46ee-ade2-2f0caae6b1e6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: c8eacac058442419cb9e8fe62cb69ad7ef1e2fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-ruby"></a>Jak toouse úložiště Queue z Ruby
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak tooperform běžné scénáře s využitím hello služby Microsoft Azure Queue Storage. Ukázky Hello jsou zapsány pomocí hello Ruby rozhraní API Azure.
Hello pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také  **vytváření a odstraňování front**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Vytvoření Ruby aplikace
Vytvořte aplikaci pro poznámky Ruby. Pokyny najdete v tématu [Ruby, na které webové aplikace ve virtuálním počítači Azure](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-tooaccess-storage"></a>Nakonfigurujte si aplikace tooAccess úložiště
toouse úložiště Azure, budete potřebovat toodownload a použití hello Ruby balíček azure, který obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.

### <a name="use-rubygems-tooobtain-hello-package"></a>Použijte RubyGems tooobtain hello balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).
2. Zadejte "gem instalace azure" hello příkazového okna tooinstall hello gem a závislosti.

### <a name="import-hello-package"></a>Importovat balíček hello
Použít svém oblíbeném textovém editoru, přidejte následující toohello horní části hello Ruby souboru, kde chcete úložiště toouse hello:

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a>Nastavit připojení k úložišti Azure
modul Hello azure, bude číst proměnné prostředí hello **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_ACCESS_KEY** pro účet úložiště Azure tooyour tooconnect jsou požadovány informace. Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu hello před použitím **Azure::QueueService** s hello následující kód:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

tooobtain tyto hodnoty ze Správce prostředků úložiště nebo klasický účet v hello portálu Azure:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Přejděte toohello úložiště účet, že který má toouse.
3. V okně Nastavení hello na hello správné, klikněte na tlačítko **přístupové klíče**.
4. V okně klíče přístup hello který se zobrazí uvidíte hello přístupový klíč 1 a 2 přístupový klíč. Můžete použít kteroukoli z nich. 
5. Klikněte na tlačítko hello kopírování ikonu toocopy hello klíče toohello schránky. 

## <a name="how-to-create-a-queue"></a>Postupy: Vytvoření fronty
Hello následující kód vytvoří **Azure::QueueService** objekt, který vám umožní toowork s fronty.

```ruby
azure_queue_service = Azure::QueueService.new
```

Použití hello **create_queue()** toocreate metoda fronta se hello zadaný název.

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Postupy: Vložit zprávu do fronty
tooinsert zprávu do fronty, použijte hello **create_message()** metoda toocreate novou zprávu a přidat ji toohello fronty.

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-hello-next-message"></a>Postupy: Zobrazení náhledu další zprávy hello
Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **funkce Náhled\_messages()** metoda. Ve výchozím nastavení **funkce Náhled\_messages()** prohlédne do jedné zprávy. Můžete také určit, kolik zpráv, které chcete toopeek.

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-hello-next-message"></a>Postupy: Dequeue – hello další zprávy
Můžete odebrat zprávu z fronty ve dvou krocích.

1. Při volání **seznamu\_messages()**, získat hello další zprávu ve frontě ve výchozím nastavení. Můžete také určit, kolik zpráv, které chcete tooget. Hello zpráv vrácených z **seznamu\_messages()** stane neviditelnou tooany další kód, který čte zprávy z této fronty. Můžete předat hello viditelnost vypršení časového limitu v sekundách jako parametr.
2. toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat **delete_message()**.

Tento dvoukrokový proces odebrání zprávy zaručuje, když vaše tooprocess kódu nezdaří, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejné zprávy a zkuste to znovu. Váš kód zavolá metodu **odstranit\_message()** hned po zpracování zprávy hello.

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Postupy: Změna hello obsah zprávy ve frontě
Můžete změnit obsah zprávy přímo ve frontě hello hello. Následující kód Hello používá hello **update_message()** metoda tooupdate zprávu. Hello metoda vrátí řazené kolekce členů, která obsahuje hello pop přijetí zprávy fronty hello a hodnotu Datum čas UTC, která představuje při uvítací zprávu bude zobrazovat na hello fronty.

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Postupy: Další možnosti pro vyřazení zprávy
Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.

1. Můžete načíst dávku zpráv.
2. Můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv.

Hello následující příklad kódu používá hello **seznamu\_messages()** metoda tooget 15 zpráv v jednom volání. Pak se vytiskne a odstraní se každá zpráva. Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu.

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-hello-queue-length"></a>Postupy: Získání hello délka fronty
Můžete získat odhad hello počet zpráv ve frontě hello. Hello **získat\_fronty\_metadata()** metoda zeptá na počet hello fronty služby tooreturn hello přibližnou zpráv a metadata o hello fronty.

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a>Postupy: Odstranění fronty
toodelete frontu a všechny zprávy hello obsažené v něm volání hello **odstranit\_queue()** metoda na objekt fronty hello.

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.

* Navštivte hello [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)
* Navštivte hello [Azure SDK pro Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu

Porovnání mezi hello službu front Azure popsané v tomto článku a fronty služby Service Bus Azure popsané v hello [jak toouse fronty služby Service Bus](/develop/ruby/how-to-guides/service-bus-queues/) článku najdete v tématu [fronty Azure a fronty služby Service Bus - porovnání a Rozdíl od aktualizovaného](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
