---
title: "Postup zaznamenání centra událostí aaaAzure | Microsoft Docs"
description: "Ukázku, která používá toodemonstrate Azure Python SDK hello pomocí funkce hello zaznamenat centra událostí."
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: darosa;sethm
ms.openlocfilehash: 1737dcca283711d863aa970db0e80ae71814e666
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a>Názorný postup zachycení centra událostí: Python

Zaznamenat centra událostí je funkce služby Event Hubs, která umožňuje tooautomatically můžete poskytovat hello streamování dat ve vaší tooan centra událostí účtu Azure Blob storage podle vašeho výběru. Tato funkce umožňuje snadno tooperform dávkového zpracování na data v reálném čase streamování. Tento článek popisuje, jak toouse Event Hubs zaznamenat s Python. Další informace o zachycení centra událostí najdete v tématu hello [článek s přehledem](event-hubs-archive-overview.md).

Tato ukázka používá hello [Azure Python SDK](https://azure.microsoft.com/develop/python/) toodemonstrate hello zachycení funkce. Hello sender.py program odešle simulovanou telemetrii prostředí tooEvent Hubs ve formátu JSON. Hello Centrum událostí je nakonfigurované toouse hello zachycení funkci toowrite toto úložiště tooblob data v dávkách. Hello capturereader.py aplikace pak přečte tyto objekty BLOB a vytvoří soubor připojení na zařízení a pak zapíše hello data do souborů CSV.

## <a name="what-will-be-accomplished"></a>Co všechno zvládneme

1. Vytvořte účet Azure Blob Storage a kontejner objektů blob v něm, pomocí hello portálu Azure.
2. Vytvořte na obor názvů centra událostí, pomocí hello portálu Azure.
3. Vytvoření centra událostí hello zachycení funkce povolena, pomocí hello portálu Azure.
4. Odeslání dat toohello centra událostí se skript v jazyce Python.
5. Číst soubory hello z hello zachycení a zpracovat je jiný skript v jazyce Python.

## <a name="prerequisites"></a>Požadavky

- Python 2.7.x
- Předplatné Azure
- Aktivní [Event Hubs obor názvů a události rozbočovače.](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu služby Azure Storage
1. Přihlaste se toohello [portál Azure][Azure portal].
2. V levém navigačním podokně hello hello portálu, klikněte na **nový**, pak klikněte na tlačítko **úložiště**a potom klikněte na **účet úložiště**.
3. Vyplňte pole hello v okně účtu úložiště hello a pak klikněte na **vytvořit**.
   
   ![][1]
4. Jakmile ověříte hello **úspěšné nasazení** zprávy, klikněte na název hello hello nový účet úložiště a v hello **Essentials** okně klikněte na tlačítko **objekty BLOB**. Když hello **služba objektů Blob** okno otevře, klikněte na tlačítko **+ kontejner** v horní části hello. Název kontejneru hello **zaznamenat**, pak zavřete hello **služba objektů Blob** okno.
5. Klikněte na tlačítko **přístupové klíče** v hello levém okně a zkopírujte hello název účtu úložiště hello a hodnotu hello **key1**. Uložte tyto hodnoty tooNotepad nebo některé dočasné umístění.

## <a name="create-a-python-script-toosend-events-tooyour-event-hub"></a>Vytvoření Python skript toosend události tooyour centra událostí
1. Otevřete svém oblíbeném editoru Python, jako například [Visual Studio Code][Visual Studio Code].
2. Vytvořte skript volá **sender.py**. Tento skript odešle 200 centra událostí tooyour události. Jsou jednoduché prostředí odečty odeslaných ve formátu JSON.
3. Vložte následující kód do sender.py hello:
   
  ```python
  import uuid
  import datetime
  import random
  import json
  from azure.servicebus import ServiceBusService
   
  sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
  devices = []
  for x in range(0, 10):
      devices.append(str(uuid.uuid4()))
   
  for y in range(0,20):
      for dev in devices:
          reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
          s = json.dumps(reading)
          sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
      print y
  ```
4. Aktualizujte hello předcházející toouse kódu vašeho oboru názvů, hodnota klíče a název centra událostí, který jste získali při vytvoření oboru názvů služby Event Hubs hello.

## <a name="create-a-python-script-tooread-your-capture-files"></a>Vytvoření Python skriptu tooread zachycení souborů

1. Vyplňte hello okně a klikněte na tlačítko **vytvořit**.
2. Vytvořte skript volá **capturereader.py**. Tento skript přečte hello zaznamenat soubory a vytvoří soubor na zařízení toowrite hello dat pouze pro toto zařízení.
3. Vložte následující kód do capturereader.py hello:
   
  ```python
  import os
  import string
  import json
  import avro.schema
  from avro.datafile import DataFileReader, DataFileWriter
  from avro.io import DatumReader, DatumWriter
  from azure.storage.blob import BlockBlobService
   
  def processBlob(filename):
      reader = DataFileReader(open(filename, 'rb'), DatumReader())
      dict = {}
      for reading in reader:
          parsed_json = json.loads(reading["Body"])
          if not 'id' in parsed_json:
              return
          if not dict.has_key(parsed_json['id']):
              list = []
              dict[parsed_json['id']] = list
          else:
              list = dict[parsed_json['id']]
              list.append(parsed_json)
      reader.close()
      for device in dict.keys():
          deviceFile = open(device + '.csv', "a")
          for r in dict[device]:
              deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
  def startProcessing(accountName, key, container):
      print 'Processor started using path: ' + os.getcwd()
      block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
      generator = block_blob_service.list_blobs(container)
      for blob in generator:
          if blob.properties.content_length != 0:
              print('Downloaded a non empty blob: ' + blob.name)
              cleanName = string.replace(blob.name, '/', '_')
              block_blob_service.get_blob_to_path(container, blob.name, cleanName)
              processBlob(cleanName)
              os.remove(cleanName)
          block_blob_service.delete_blob(container, blob.name)
  startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
  ```
4. Být jisti toopaste hello odpovídající hodnoty pro název účtu úložiště a klíč v hello volání příliš`startProcessing`.

## <a name="run-hello-scripts"></a>Spouštění skriptů hello
1. Otevřete příkazový řádek, který má Python v jeho cestu a potom spuštěním těchto příkazů tooinstall Python požadované balíčky:
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  Pokud máte starší verzi azure-storage nebo azure, může být nutné toouse hello **– upgrade** možnost
   
  Můžete také potřebovat následující hello toorun (není nutné u většiny systémů):
   
  ```
  pip install cryptography
  ```
2. Změňte váš adresář toowherever jste uložili sender.py a capturereader.py a spusťte tento příkaz:
   
  ```
  start python sender.py
  ```
   
  Tento příkaz spustí novou toorun hello odesílatele proces Python.
3. Nyní Počkejte několik minut toorun zachycení hello. Poté zadejte hello do původní okno příkazového řádku následující příkaz:
   
   ```
   python capturereader.py
   ```

   Tento snímek procesoru používá hello místního adresáře toodownload všech objektů BLOB hello z kontejneru účtu úložiště hello. Zpracuje všechny, které nejsou prázdné a zapíše hello výsledky jako soubory .csv do místního adresáře hello.

## <a name="next-steps"></a>Další kroky

Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs zachycení][Overview of Event Hubs Capture]
* Úplná [ukázková aplikace, která používá službu Event Hubs][sample application that uses Event Hubs]
* Hello [horizontální navýšení kapacity zpracování událostí ve službě Event Hubs] [ Scale out Event Processing with Event Hubs] ukázka.
* [Přehled služby Event Hubs][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
