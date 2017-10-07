---
title: "aaaHow toouse úložiště Queue z Pythonu | Microsoft Docs"
description: "Zjistěte, jak toouse hello služby Azure Queue z Pythonu toocreate odstranit fronty a vložit, získání a odstranění zprávy."
services: storage
documentationcenter: python
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: cc0d2da2-379a-4b58-a234-8852b4e3d99d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: f4f902a2c314401e5c1768fbc80566c8ba25c058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-python"></a>Jak toouse úložiště Queue z Pythonu
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Queue storage. Hello ukázky jsou napsané v Pythonu a používají hello [Microsoft Azure SDK úložiště pro jazyk Python]. Hello pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také  **vytváření a odstraňování front**. Další informace o frontách najdete v části toohello [Další kroky].

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Postupy: Vytvoření fronty
Hello **QueueService** objekt vám umožňuje spolupracovat s fronty. Hello následující kód vytvoří **QueueService** objektu. Přidejte následující hello v horní hello jakéhokoliv Python souboru, ve kterém chcete tooprogrammatically přístupu Azure Storage:

```python
from azure.storage.queue import QueueService
```

Hello následující kód vytvoří **QueueService** objekt, který používá hello klíč účtu úložiště účet a název. Nahraďte název účtu a klíč 'stránku Můj účet' a 'mykey.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Postupy: Vložit zprávu do fronty
tooinsert zprávu do fronty, použijte hello **put\_zpráva** metoda vytvořte novou zprávu a přidat ji toohello fronty.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-hello-next-message"></a>Postupy: Zobrazení náhledu další zprávy hello
Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **funkce Náhled\_zprávy** metoda. Ve výchozím nastavení **funkce Náhled\_zprávy** prohlédne do jedné zprávy.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Postupy: Dequeue – zprávy
Váš kód odebere zprávu z fronty ve dvou krocích. Při volání **získat\_zprávy**, získat hello další zprávu ve frontě ve výchozím nastavení. Zpráva vrácená metodou **získat\_zprávy** stane neviditelnou tooany další kód, který čte zprávy z této fronty. Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund. toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat **odstranit\_zpráva**. Tento dvoukrokový proces odebrání zprávy zaručuje, že pokud se váš kód nezdaří tooprocess zprávu z důvodu selhání hardwaru nebo softwaru, jiná instance vašeho kódu můžete stejnou zprávu získat a akci opakujte. Váš kód zavolá metodu **odstranit\_zpráva** hned po zpracování zprávy hello.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.
Nejprve můžete načíst dávku zpráv (až too32). Druhý můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv. Hello následující příklad kódu používá **získat\_zprávy** metoda tooget 16 zpráv v jednom volání. Pak se každá zpráva zpracuje pomocí pro smyčky. Také pět minut pro každou zprávu nastaví časový limit neviditelnosti hello.

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Postupy: Změna hello obsah zprávy ve frontě
Můžete změnit obsah zprávy přímo ve frontě hello hello. Pokud zpráva představuje pracovní úlohu, můžete použít tuto funkci tooupdate stav hello pracovní úlohy. Následující kód Hello používá hello **aktualizace\_zpráva** metoda tooupdate zprávu. časový limit viditelnosti Hello nastaven too0, což znamená, tato zpráva se zobrazí okamžitě a hello obsah aktualizován.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-hello-queue-length"></a>Postupy: Získání hello délka fronty
Můžete získat odhad hello počet zpráv ve frontě. **Získat\_fronty\_metadata** metoda požádá hello fronty služby tooreturn metadata o hello fronty a hello **approximate_message_count**. výsledek Hello je pouze přibližné, protože zprávy můžete přidat nebo odebrat po tooyour žádost odpoví služby front.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Postupy: Odstranění fronty
toodelete frontu a všechny zprávy hello obsažené v něm, volejte **odstranit\_fronty** metoda.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn Další.

* [Středisko pro vývojáře programující v Pythonu](/develop/python/)
* [REST API služby Azure Storage](http://msdn.microsoft.com/library/azure/dd179355)
* [Blog týmu Azure Storage]
* [Microsoft Azure SDK úložiště pro jazyk Python]

[Blog týmu Azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure SDK úložiště pro jazyk Python]: https://github.com/Azure/azure-storage-python