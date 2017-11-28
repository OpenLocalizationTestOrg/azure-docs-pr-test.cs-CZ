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
# <a name="event-hubs-capture-walkthrough-python"></a><span data-ttu-id="c1e64-103">Názorný postup zachycení centra událostí: Python</span><span class="sxs-lookup"><span data-stu-id="c1e64-103">Event Hubs Capture walkthrough: Python</span></span>

<span data-ttu-id="c1e64-104">Zaznamenat centra událostí je funkce služby Event Hubs, která umožňuje tooautomatically můžete poskytovat hello streamování dat ve vaší tooan centra událostí účtu Azure Blob storage podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="c1e64-104">Event Hubs Capture is a feature of Event Hubs that enables you tooautomatically deliver hello streaming data in your event hub tooan Azure Blob storage account of your choice.</span></span> <span data-ttu-id="c1e64-105">Tato funkce umožňuje snadno tooperform dávkového zpracování na data v reálném čase streamování.</span><span class="sxs-lookup"><span data-stu-id="c1e64-105">This capability makes it easy tooperform batch processing on real-time streaming data.</span></span> <span data-ttu-id="c1e64-106">Tento článek popisuje, jak toouse Event Hubs zaznamenat s Python.</span><span class="sxs-lookup"><span data-stu-id="c1e64-106">This article describes how toouse Event Hubs Capture with Python.</span></span> <span data-ttu-id="c1e64-107">Další informace o zachycení centra událostí najdete v tématu hello [článek s přehledem](event-hubs-archive-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c1e64-107">For more information about Event Hubs Capture, see hello [overview article](event-hubs-archive-overview.md).</span></span>

<span data-ttu-id="c1e64-108">Tato ukázka používá hello [Azure Python SDK](https://azure.microsoft.com/develop/python/) toodemonstrate hello zachycení funkce.</span><span class="sxs-lookup"><span data-stu-id="c1e64-108">This sample uses hello [Azure Python SDK](https://azure.microsoft.com/develop/python/) toodemonstrate hello Capture feature.</span></span> <span data-ttu-id="c1e64-109">Hello sender.py program odešle simulovanou telemetrii prostředí tooEvent Hubs ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="c1e64-109">hello sender.py program sends simulated environmental telemetry tooEvent Hubs in JSON format.</span></span> <span data-ttu-id="c1e64-110">Hello Centrum událostí je nakonfigurované toouse hello zachycení funkci toowrite toto úložiště tooblob data v dávkách.</span><span class="sxs-lookup"><span data-stu-id="c1e64-110">hello event hub is configured toouse hello Capture feature toowrite this data tooblob storage in batches.</span></span> <span data-ttu-id="c1e64-111">Hello capturereader.py aplikace pak přečte tyto objekty BLOB a vytvoří soubor připojení na zařízení a pak zapíše hello data do souborů CSV.</span><span class="sxs-lookup"><span data-stu-id="c1e64-111">hello capturereader.py app then reads these blobs and creates an append file per device, then writes hello data into .csv files.</span></span>

## <a name="what-will-be-accomplished"></a><span data-ttu-id="c1e64-112">Co všechno zvládneme</span><span class="sxs-lookup"><span data-stu-id="c1e64-112">What will be accomplished</span></span>

1. <span data-ttu-id="c1e64-113">Vytvořte účet Azure Blob Storage a kontejner objektů blob v něm, pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c1e64-113">Create an Azure Blob Storage account and a blob container within it, using hello Azure portal.</span></span>
2. <span data-ttu-id="c1e64-114">Vytvořte na obor názvů centra událostí, pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c1e64-114">Create an Event Hub namespace, using hello Azure portal.</span></span>
3. <span data-ttu-id="c1e64-115">Vytvoření centra událostí hello zachycení funkce povolena, pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c1e64-115">Create an event hub with hello Capture feature enabled, using hello Azure portal.</span></span>
4. <span data-ttu-id="c1e64-116">Odeslání dat toohello centra událostí se skript v jazyce Python.</span><span class="sxs-lookup"><span data-stu-id="c1e64-116">Send data toohello event hub with a Python script.</span></span>
5. <span data-ttu-id="c1e64-117">Číst soubory hello z hello zachycení a zpracovat je jiný skript v jazyce Python.</span><span class="sxs-lookup"><span data-stu-id="c1e64-117">Read hello files from hello capture and process them with another Python script.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1e64-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c1e64-118">Prerequisites</span></span>

- <span data-ttu-id="c1e64-119">Python 2.7.x</span><span class="sxs-lookup"><span data-stu-id="c1e64-119">Python 2.7.x</span></span>
- <span data-ttu-id="c1e64-120">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="c1e64-120">An Azure subscription</span></span>
- <span data-ttu-id="c1e64-121">Aktivní [Event Hubs obor názvů a události rozbočovače.](event-hubs-create.md)</span><span class="sxs-lookup"><span data-stu-id="c1e64-121">An active [Event Hubs namespace and event hub.](event-hubs-create.md)</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="c1e64-122">Vytvoření účtu služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c1e64-122">Create an Azure Storage account</span></span>
1. <span data-ttu-id="c1e64-123">Přihlaste se toohello [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="c1e64-123">Log on toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="c1e64-124">V levém navigačním podokně hello hello portálu, klikněte na **nový**, pak klikněte na tlačítko **úložiště**a potom klikněte na **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c1e64-124">In hello left navigation pane of hello portal, click **New**, then click **Storage**, and then click **Storage Account**.</span></span>
3. <span data-ttu-id="c1e64-125">Vyplňte pole hello v okně účtu úložiště hello a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c1e64-125">Complete hello fields in hello storage account blade and then click **Create**.</span></span>
   
   ![][1]
4. <span data-ttu-id="c1e64-126">Jakmile ověříte hello **úspěšné nasazení** zprávy, klikněte na název hello hello nový účet úložiště a v hello **Essentials** okně klikněte na tlačítko **objekty BLOB**.</span><span class="sxs-lookup"><span data-stu-id="c1e64-126">After you see hello **Deployments Succeeded** message, click hello name of hello new storage account and in hello **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="c1e64-127">Když hello **služba objektů Blob** okno otevře, klikněte na tlačítko **+ kontejner** v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="c1e64-127">When hello **Blob service** blade opens, click **+ Container** at hello top.</span></span> <span data-ttu-id="c1e64-128">Název kontejneru hello **zaznamenat**, pak zavřete hello **služba objektů Blob** okno.</span><span class="sxs-lookup"><span data-stu-id="c1e64-128">Name hello container **capture**, then close hello **Blob service** blade.</span></span>
5. <span data-ttu-id="c1e64-129">Klikněte na tlačítko **přístupové klíče** v hello levém okně a zkopírujte hello název účtu úložiště hello a hodnotu hello **key1**.</span><span class="sxs-lookup"><span data-stu-id="c1e64-129">Click **Access keys** in hello left blade and copy hello name of hello storage account and hello value of **key1**.</span></span> <span data-ttu-id="c1e64-130">Uložte tyto hodnoty tooNotepad nebo některé dočasné umístění.</span><span class="sxs-lookup"><span data-stu-id="c1e64-130">Save these values tooNotepad or some other temporary location.</span></span>

## <a name="create-a-python-script-toosend-events-tooyour-event-hub"></a><span data-ttu-id="c1e64-131">Vytvoření Python skript toosend události tooyour centra událostí</span><span class="sxs-lookup"><span data-stu-id="c1e64-131">Create a Python script toosend events tooyour event hub</span></span>
1. <span data-ttu-id="c1e64-132">Otevřete svém oblíbeném editoru Python, jako například [Visual Studio Code][Visual Studio Code].</span><span class="sxs-lookup"><span data-stu-id="c1e64-132">Open your favorite Python editor, such as [Visual Studio Code][Visual Studio Code].</span></span>
2. <span data-ttu-id="c1e64-133">Vytvořte skript volá **sender.py**.</span><span class="sxs-lookup"><span data-stu-id="c1e64-133">Create a script called **sender.py**.</span></span> <span data-ttu-id="c1e64-134">Tento skript odešle 200 centra událostí tooyour události.</span><span class="sxs-lookup"><span data-stu-id="c1e64-134">This script sends 200 events tooyour event hub.</span></span> <span data-ttu-id="c1e64-135">Jsou jednoduché prostředí odečty odeslaných ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="c1e64-135">They are simple environmental readings sent in JSON.</span></span>
3. <span data-ttu-id="c1e64-136">Vložte následující kód do sender.py hello:</span><span class="sxs-lookup"><span data-stu-id="c1e64-136">Paste hello following code into sender.py:</span></span>
   
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
4. <span data-ttu-id="c1e64-137">Aktualizujte hello předcházející toouse kódu vašeho oboru názvů, hodnota klíče a název centra událostí, který jste získali při vytvoření oboru názvů služby Event Hubs hello.</span><span class="sxs-lookup"><span data-stu-id="c1e64-137">Update hello preceding code toouse your namespace name, key value, and event hub name that you obtained when you created hello Event Hubs namespace.</span></span>

## <a name="create-a-python-script-tooread-your-capture-files"></a><span data-ttu-id="c1e64-138">Vytvoření Python skriptu tooread zachycení souborů</span><span class="sxs-lookup"><span data-stu-id="c1e64-138">Create a Python script tooread your Capture files</span></span>

1. <span data-ttu-id="c1e64-139">Vyplňte hello okně a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c1e64-139">Fill out hello blade and click **Create**.</span></span>
2. <span data-ttu-id="c1e64-140">Vytvořte skript volá **capturereader.py**.</span><span class="sxs-lookup"><span data-stu-id="c1e64-140">Create a script called **capturereader.py**.</span></span> <span data-ttu-id="c1e64-141">Tento skript přečte hello zaznamenat soubory a vytvoří soubor na zařízení toowrite hello dat pouze pro toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="c1e64-141">This script reads hello captured files and creates a file per device toowrite hello data only for that device.</span></span>
3. <span data-ttu-id="c1e64-142">Vložte následující kód do capturereader.py hello:</span><span class="sxs-lookup"><span data-stu-id="c1e64-142">Paste hello following code into capturereader.py:</span></span>
   
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
4. <span data-ttu-id="c1e64-143">Být jisti toopaste hello odpovídající hodnoty pro název účtu úložiště a klíč v hello volání příliš`startProcessing`.</span><span class="sxs-lookup"><span data-stu-id="c1e64-143">Be sure toopaste hello appropriate values for your storage account name and key in hello call too`startProcessing`.</span></span>

## <a name="run-hello-scripts"></a><span data-ttu-id="c1e64-144">Spouštění skriptů hello</span><span class="sxs-lookup"><span data-stu-id="c1e64-144">Run hello scripts</span></span>
1. <span data-ttu-id="c1e64-145">Otevřete příkazový řádek, který má Python v jeho cestu a potom spuštěním těchto příkazů tooinstall Python požadované balíčky:</span><span class="sxs-lookup"><span data-stu-id="c1e64-145">Open a command prompt that has Python in its path, and then run these commands tooinstall Python prerequisite packages:</span></span>
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  <span data-ttu-id="c1e64-146">Pokud máte starší verzi azure-storage nebo azure, může být nutné toouse hello **– upgrade** možnost</span><span class="sxs-lookup"><span data-stu-id="c1e64-146">If you have an earlier version of either azure-storage or azure, you may need toouse hello **--upgrade** option</span></span>
   
  <span data-ttu-id="c1e64-147">Můžete také potřebovat následující hello toorun (není nutné u většiny systémů):</span><span class="sxs-lookup"><span data-stu-id="c1e64-147">You might also need toorun hello following (not necessary on most systems):</span></span>
   
  ```
  pip install cryptography
  ```
2. <span data-ttu-id="c1e64-148">Změňte váš adresář toowherever jste uložili sender.py a capturereader.py a spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="c1e64-148">Change your directory toowherever you saved sender.py and capturereader.py, and run this command:</span></span>
   
  ```
  start python sender.py
  ```
   
  <span data-ttu-id="c1e64-149">Tento příkaz spustí novou toorun hello odesílatele proces Python.</span><span class="sxs-lookup"><span data-stu-id="c1e64-149">This command starts a new Python process toorun hello sender.</span></span>
3. <span data-ttu-id="c1e64-150">Nyní Počkejte několik minut toorun zachycení hello.</span><span class="sxs-lookup"><span data-stu-id="c1e64-150">Now wait a few minutes for hello capture toorun.</span></span> <span data-ttu-id="c1e64-151">Poté zadejte hello do původní okno příkazového řádku následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c1e64-151">Then type hello following command into your original command window:</span></span>
   
   ```
   python capturereader.py
   ```

   <span data-ttu-id="c1e64-152">Tento snímek procesoru používá hello místního adresáře toodownload všech objektů BLOB hello z kontejneru účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="c1e64-152">This capture processor uses hello local directory toodownload all hello blobs from hello storage account/container.</span></span> <span data-ttu-id="c1e64-153">Zpracuje všechny, které nejsou prázdné a zapíše hello výsledky jako soubory .csv do místního adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="c1e64-153">It processes any that are not empty, and writes hello results as .csv files into hello local directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1e64-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1e64-154">Next steps</span></span>

<span data-ttu-id="c1e64-155">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="c1e64-155">You can learn more about Event Hubs by visiting hello following links:</span></span>

* <span data-ttu-id="c1e64-156">[Přehled služby Event Hubs zachycení][Overview of Event Hubs Capture]</span><span class="sxs-lookup"><span data-stu-id="c1e64-156">[Overview of Event Hubs Capture][Overview of Event Hubs Capture]</span></span>
* <span data-ttu-id="c1e64-157">Úplná [ukázková aplikace, která používá službu Event Hubs][sample application that uses Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="c1e64-157">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs].</span></span>
* <span data-ttu-id="c1e64-158">Hello [horizontální navýšení kapacity zpracování událostí ve službě Event Hubs] [ Scale out Event Processing with Event Hubs] ukázka.</span><span class="sxs-lookup"><span data-stu-id="c1e64-158">hello [Scale out Event Processing with Event Hubs][Scale out Event Processing with Event Hubs] sample.</span></span>
* <span data-ttu-id="c1e64-159">[Přehled služby Event Hubs][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="c1e64-159">[Event Hubs overview][Event Hubs overview]</span></span>

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
