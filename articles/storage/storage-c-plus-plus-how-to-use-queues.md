---
title: "aaaHow toouse fronty úložiště (C++) | Microsoft Docs"
description: "Zjistěte, jak toouse hello fronty služby úložiště v Azure. Ukázky jsou napsané v jazyce C++."
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: jahogg
editor: tysonn
ms.assetid: c8a36365-29f6-404d-8fd1-858a7f33b50a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 05/11/2017
ms.author: cbrooksmsft
ms.openlocfilehash: 755380824890ad83774e14d258975915e10cfede
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-c"></a>Jak toouse Queue Storage z jazyka C++
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Queue storage. Hello ukázky jsou napsané v jazyce C++ a používají hello [Klientská knihovna pro úložiště Azure pro jazyk C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Hello pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také  **vytváření a odstraňování front**.

> [!NOTE]
> Tato příručka cílí hello Klientská knihovna pro úložiště Azure pro jazyk C++ verze 1.0.0 a vyšší. Hello doporučená verze je klientská knihovna pro úložiště 2.2.0, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](http://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Vytvoření aplikace C++
V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace C++.

toodo tedy budete potřebovat tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++ a vytvoření účtu úložiště Azure ve vašem předplatném Azure.

tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++, můžete použít následující metody hello:

* **Linux:** postupujte podle pokynů hello uvedenému v hello [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.
* **Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje > Správce balíčků NuGet > Konzola správce balíčků**. Typ hello následující příkaz do hello [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu **ENTER**.

```  
Install-Package wastorage
```

## <a name="configure-your-application-tooaccess-queue-storage"></a>Konfigurace vaší aplikace tooaccess Queue Storage
Přidáte následující hello obsahovat toohello příkazy na začátek souboru C++ hello místo toouse hello úložiště Azure rozhraní API tooaccess fronty:  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Nastavit připojovací řetězec úložiště Azure
Klienta Azure storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy. Když spustíte v aplikaci klienta, je nutné zadat připojovací řetězec úložiště hello v hello formátu, pomocí hello název účtu a hello úložiště přístupový klíč k úložišti pro účet úložiště hello uvedené v hello [portálu Azure](https://portal.azure.com)pro hello *AccountName* a *AccountKey* hodnoty. Informace o účtech úložiště a přístupové klávesy, najdete v části [o účtech úložiště Azure](storage-create-storage-account.md). Tento příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec:  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

tootest aplikace v místním počítači s Windows, můžete použít hello Microsoft Azure [emulátor úložiště](storage-use-emulator.md) který se instaluje s hello [Azure SDK](https://azure.microsoft.com/downloads/). emulátor úložiště Hello je nástroj, který simuluje hello objektů Blob, Queue a Table služby v Azure k dispozici na místním vývojovém počítači. Hello následující příklad ukazuje, jak lze deklarovat statické pole toohold hello připojovací řetězec tooyour emulátor místního úložiště:  

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

emulátor úložiště Azure toostart hello, vyberte hello **spustit** tlačítko nebo klikněte na tlačítko hello **Windows** klíč. Začněte psát **emulátoru úložiště Azure**a vyberte **emulátor úložiště Microsoft Azure** hello seznamu aplikací.

Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.

## <a name="retrieve-your-connection-string"></a>Načtení připojovacího řetězce
Můžete použít hello **cloud_storage_account** třídy toorepresent informací o vašem účtu úložiště. tooretrieve účet úložiště informací z připojovacího řetězce úložiště hello, můžete použít hello **analyzovat** metoda.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a>Postupy: vytvoření fronty
A **cloud_queue_client** objektu umožňuje získat odkaz na objekty pro fronty. Hello následující kód vytvoří **cloud_queue_client** objektu.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

Použití hello **cloud_queue_client** tooget chcete toouse fronty toohello odkaz na objekt. Pokud neexistuje, můžete vytvořit hello fronty.

```cpp
// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Postupy: vložit zprávu do fronty
tooinsert zprávu do existující fronty, vytvořte nejdříve novou **cloud_queue_message**. Pak zavolejte hello **add_message** metoda. A **cloud_queue_message** můžete vytvořit buď z řetězce nebo **bajtů** pole. Zde je kód, který vytvoří frontu (pokud neexistuje) a vloží uvítací zprávu "Hello, World":

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it toohello queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-hello-next-message"></a>Postupy: zobrazení náhledu další zprávy hello
Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **peek_message** metoda.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at hello next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output hello message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Postupy: Změna obsahu zpráv zařazených ve frontě hello
Můžete změnit obsah zprávy přímo ve frontě hello hello. Pokud hello zpráva představuje pracovní úlohu, můžete použít tuto funkci tooupdate hello stav hello pracovní úlohy. Hello následující kód aktualizuje zprávy fronty hello nový obsah, a nastaví hello tooextend časový limit viditelnosti jiné 60 sekund. Uloží hello stav práce spojený s uvítací zprávu a poskytuje další minutu toocontinue pracující na uvítací zprávu klienta hello. Můžete použít tento postup tootrack vícekrokového pracovní postupy pro zprávy ve frontě, bez nutnosti toostart přes od začátku hello, pokud se nezdaří krok zpracování z důvodu selhání toohardware nebo softwaru. Obvykle byste udržovali také počet opakování, a pokud uvítací zprávu se pokus o více než n krát, odstranili byste ji. Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello message from hello queue and update hello message contents.
// hello visibility timeout "0" means make it visible immediately.
// hello visibility timeout "60" means hello client can get another minute toocontinue
// working on hello message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output hello message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-hello-next-message"></a>Postupy: zrušte hello další zprávu ve frontě
Váš kód vyřazuje zprávy z fronty ve dvou krocích. Při volání **get_message**, získat hello další zprávu ve frontě. Zpráva vrácená metodou **get_message** stane neviditelnou tooany další kód, který čte zprávy z této fronty. toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat **delete_message**. Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu. Váš kód zavolá metodu **delete_message** hned po zpracování zprávy hello.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete hello message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Postupy: využívání dalších možností pro vyřazování zpráv z fronty
Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby. Nejprve můžete načíst dávku zpráv (až too32). Druhý můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv. Hello následující příklad kódu používá hello **get_messages** metoda tooget 20 zpráv v jednom volání. Pak se každá zpráva zpracuje pomocí **pro** smyčky. Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu. Všimněte si, že hello 5 minut spustí pro všechny zprávy s hello stejný čas, takže po 5 minut od volání hello příliš předané**get_messages**, všechny zprávy, které nebyly odstraněny, opět viditelné.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from hello queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display hello contents of hello message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-hello-queue-length"></a>Postupy: získání délky fronty hello
Můžete získat odhad hello počet zpráv ve frontě. Hello **download_attributes** metoda požádá hello fronty služby tooretrieve hello atributů fronty, včetně počtu zpráv hello. Hello **approximate_message_count** metoda získá hello přibližný počet zpráv ve frontě hello.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch hello queue attributes.
queue.download_attributes();

// Retrieve hello cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a>Postupy: odstranění fronty
fronty a všechny zprávy hello obsažené v něm volání hello toodelete **delete_queue_if_exists** metoda na objekt fronty hello.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If hello queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn Další informace o Azure Storage.

* [Jak toouse úložiště objektů Blob z jazyka C++](storage-c-plus-plus-how-to-use-blobs.md)
* [Jak toouse úložiště Table z jazyka C++](storage-c-plus-plus-how-to-use-tables.md)
* [Seznam prostředků úložiště Azure v jazyce C++](storage-c-plus-plus-enumeration.md)
* [Klientská knihovna pro úložiště pro C++ – referenční informace](http://azure.github.io/azure-storage-cpp)
* [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)