---
title: "Azure Event Hubs zaznamenat návod | Microsoft Docs"
description: "Ukázka pomocí sady Azure SDK pro Python, která demonstruje použití funkci zachycení centra událostí."
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
ms.openlocfilehash: a764a116755c20f60e92e553bd7c896425272b85
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a><span data-ttu-id="762c0-103">Názorný postup zachycení centra událostí: Python</span><span class="sxs-lookup"><span data-stu-id="762c0-103">Event Hubs Capture walkthrough: Python</span></span>

<span data-ttu-id="762c0-104">Zaznamenat centra událostí je funkce služby Event Hubs, která umožňuje automaticky doručování streamování dat v Centru událostí k účtu úložiště objektů Blob v Azure podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="762c0-104">Event Hubs Capture is a feature of Event Hubs that enables you to automatically deliver the streaming data in your event hub to an Azure Blob storage account of your choice.</span></span> <span data-ttu-id="762c0-105">Tato funkce umožňuje snadno provádět, dávkového zpracování na data v reálném čase streamování.</span><span class="sxs-lookup"><span data-stu-id="762c0-105">This capability makes it easy to perform batch processing on real-time streaming data.</span></span> <span data-ttu-id="762c0-106">Tento článek popisuje, jak používat zaznamenat centra událostí s Python.</span><span class="sxs-lookup"><span data-stu-id="762c0-106">This article describes how to use Event Hubs Capture with Python.</span></span> <span data-ttu-id="762c0-107">Další informace o zachycení centra událostí najdete v tématu [článek s přehledem](event-hubs-archive-overview.md).</span><span class="sxs-lookup"><span data-stu-id="762c0-107">For more information about Event Hubs Capture, see the [overview article](event-hubs-archive-overview.md).</span></span>

<span data-ttu-id="762c0-108">V tomto příkladu [Azure Python SDK](https://azure.microsoft.com/develop/python/) k předvedení funkci zachycení.</span><span class="sxs-lookup"><span data-stu-id="762c0-108">This sample uses the [Azure Python SDK](https://azure.microsoft.com/develop/python/) to demonstrate the Capture feature.</span></span> <span data-ttu-id="762c0-109">Sender.py program odešle simulovanou telemetrii prostředí do centra událostí ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="762c0-109">The sender.py program sends simulated environmental telemetry to Event Hubs in JSON format.</span></span> <span data-ttu-id="762c0-110">Centrum událostí je nakonfigurované použití funkce zachycení zápis tato data do úložiště objektů blob v dávkách.</span><span class="sxs-lookup"><span data-stu-id="762c0-110">The event hub is configured to use the Capture feature to write this data to blob storage in batches.</span></span> <span data-ttu-id="762c0-111">Aplikace capturereader.py pak přečte tyto objekty BLOB a vytvoří soubor připojení na zařízení a pak zapíše data do souborů CSV.</span><span class="sxs-lookup"><span data-stu-id="762c0-111">The capturereader.py app then reads these blobs and creates an append file per device, then writes the data into .csv files.</span></span>

## <a name="what-will-be-accomplished"></a><span data-ttu-id="762c0-112">Co všechno zvládneme</span><span class="sxs-lookup"><span data-stu-id="762c0-112">What will be accomplished</span></span>

1. <span data-ttu-id="762c0-113">Vytvořte účet Azure Blob Storage a kontejner objektů blob v něm, pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="762c0-113">Create an Azure Blob Storage account and a blob container within it, using the Azure portal.</span></span>
2. <span data-ttu-id="762c0-114">Vytvořte na obor názvů centra událostí, pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="762c0-114">Create an Event Hub namespace, using the Azure portal.</span></span>
3. <span data-ttu-id="762c0-115">Vytvoření centra událostí s funkci zachycení povoleno, pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="762c0-115">Create an event hub with the Capture feature enabled, using the Azure portal.</span></span>
4. <span data-ttu-id="762c0-116">Posílat data do centra událostí se skript v jazyce Python.</span><span class="sxs-lookup"><span data-stu-id="762c0-116">Send data to the event hub with a Python script.</span></span>
5. <span data-ttu-id="762c0-117">Čtení souborů z zachytávání a zpracovat je jiný skript v jazyce Python.</span><span class="sxs-lookup"><span data-stu-id="762c0-117">Read the files from the capture and process them with another Python script.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="762c0-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="762c0-118">Prerequisites</span></span>

- <span data-ttu-id="762c0-119">Python 2.7.x</span><span class="sxs-lookup"><span data-stu-id="762c0-119">Python 2.7.x</span></span>
- <span data-ttu-id="762c0-120">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="762c0-120">An Azure subscription</span></span>
- <span data-ttu-id="762c0-121">Aktivní [Event Hubs obor názvů a události rozbočovače.](event-hubs-create.md)</span><span class="sxs-lookup"><span data-stu-id="762c0-121">An active [Event Hubs namespace and event hub.](event-hubs-create.md)</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="762c0-122">Vytvoření účtu služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="762c0-122">Create an Azure Storage account</span></span>
1. <span data-ttu-id="762c0-123">Přihlaste se k webu [Azure Portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="762c0-123">Log on to the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="762c0-124">V levém navigačním podokně portálu klikněte na **nový**, pak klikněte na tlačítko **úložiště**a potom klikněte na **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="762c0-124">In the left navigation pane of the portal, click **New**, then click **Storage**, and then click **Storage Account**.</span></span>
3. <span data-ttu-id="762c0-125">Vyplňte pole v okně účtu úložiště a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="762c0-125">Complete the fields in the storage account blade and then click **Create**.</span></span>
   
   ![][1]
4. <span data-ttu-id="762c0-126">Jakmile ověříte **úspěšné nasazení** zprávy, klikněte na název nového účtu úložiště a v **Essentials** okně klikněte na tlačítko **objekty BLOB**.</span><span class="sxs-lookup"><span data-stu-id="762c0-126">After you see the **Deployments Succeeded** message, click the name of the new storage account and in the **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="762c0-127">Když **služba objektů Blob** okno otevře, klikněte na tlačítko **+ kontejner** v horní části.</span><span class="sxs-lookup"><span data-stu-id="762c0-127">When the **Blob service** blade opens, click **+ Container** at the top.</span></span> <span data-ttu-id="762c0-128">Název kontejneru **zaznamenat**, pak zavřete **služba objektů Blob** okno.</span><span class="sxs-lookup"><span data-stu-id="762c0-128">Name the container **capture**, then close the **Blob service** blade.</span></span>
5. <span data-ttu-id="762c0-129">Klikněte na tlačítko **přístupové klíče** v levém okně a zkopírujte název účtu úložiště a hodnotu **key1**.</span><span class="sxs-lookup"><span data-stu-id="762c0-129">Click **Access keys** in the left blade and copy the name of the storage account and the value of **key1**.</span></span> <span data-ttu-id="762c0-130">Uložte tyto hodnoty do programu Poznámkový blok nebo některé dočasné umístění.</span><span class="sxs-lookup"><span data-stu-id="762c0-130">Save these values to Notepad or some other temporary location.</span></span>

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a><span data-ttu-id="762c0-131">Vytvořit skript v jazyce Python odesílat události do vašeho centra událostí</span><span class="sxs-lookup"><span data-stu-id="762c0-131">Create a Python script to send events to your event hub</span></span>
1. <span data-ttu-id="762c0-132">Otevřete svém oblíbeném editoru Python, jako například [Visual Studio Code][Visual Studio Code].</span><span class="sxs-lookup"><span data-stu-id="762c0-132">Open your favorite Python editor, such as [Visual Studio Code][Visual Studio Code].</span></span>
2. <span data-ttu-id="762c0-133">Vytvořte skript volá **sender.py**.</span><span class="sxs-lookup"><span data-stu-id="762c0-133">Create a script called **sender.py**.</span></span> <span data-ttu-id="762c0-134">Tento skript odesílá 200 události do vašeho centra událostí.</span><span class="sxs-lookup"><span data-stu-id="762c0-134">This script sends 200 events to your event hub.</span></span> <span data-ttu-id="762c0-135">Jsou jednoduché prostředí odečty odeslaných ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="762c0-135">They are simple environmental readings sent in JSON.</span></span>
3. <span data-ttu-id="762c0-136">Vložte následující kód do sender.py:</span><span class="sxs-lookup"><span data-stu-id="762c0-136">Paste the following code into sender.py:</span></span>
   
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
4. <span data-ttu-id="762c0-137">Aktualizujte předchozí kód, který použije váš obor názvů, hodnota klíče a název centra událostí, který jste získali při vytvoření oboru názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="762c0-137">Update the preceding code to use your namespace name, key value, and event hub name that you obtained when you created the Event Hubs namespace.</span></span>

## <a name="create-a-python-script-to-read-your-capture-files"></a><span data-ttu-id="762c0-138">Vytvořit skript v jazyce Python číst vaše soubory zachytávání</span><span class="sxs-lookup"><span data-stu-id="762c0-138">Create a Python script to read your Capture files</span></span>

1. <span data-ttu-id="762c0-139">Vyplňte okno a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="762c0-139">Fill out the blade and click **Create**.</span></span>
2. <span data-ttu-id="762c0-140">Vytvořte skript volá **capturereader.py**.</span><span class="sxs-lookup"><span data-stu-id="762c0-140">Create a script called **capturereader.py**.</span></span> <span data-ttu-id="762c0-141">Tento skript načte zaznamenané soubory a vytvoří soubor na zařízení k zápisu dat pouze pro toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="762c0-141">This script reads the captured files and creates a file per device to write the data only for that device.</span></span>
3. <span data-ttu-id="762c0-142">Vložte následující kód do capturereader.py:</span><span class="sxs-lookup"><span data-stu-id="762c0-142">Paste the following code into capturereader.py:</span></span>
   
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
4. <span data-ttu-id="762c0-143">Nezapomeňte vložit na odpovídající hodnoty pro název účtu úložiště a klíč ve volání `startProcessing`.</span><span class="sxs-lookup"><span data-stu-id="762c0-143">Be sure to paste the appropriate values for your storage account name and key in the call to `startProcessing`.</span></span>

## <a name="run-the-scripts"></a><span data-ttu-id="762c0-144">Spuštěním skriptů</span><span class="sxs-lookup"><span data-stu-id="762c0-144">Run the scripts</span></span>
1. <span data-ttu-id="762c0-145">Otevřete příkazový řádek, který má Python v jeho cestu a poté spusťte tyto příkazy pro instalaci požadovaných balíčků Python:</span><span class="sxs-lookup"><span data-stu-id="762c0-145">Open a command prompt that has Python in its path, and then run these commands to install Python prerequisite packages:</span></span>
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  <span data-ttu-id="762c0-146">Pokud máte starší verzi azure-storage nebo azure, budete možná muset použít **– upgrade** možnost</span><span class="sxs-lookup"><span data-stu-id="762c0-146">If you have an earlier version of either azure-storage or azure, you may need to use the **--upgrade** option</span></span>
   
  <span data-ttu-id="762c0-147">Může se také muset spustit následující (není nutné u většiny systémů):</span><span class="sxs-lookup"><span data-stu-id="762c0-147">You might also need to run the following (not necessary on most systems):</span></span>
   
  ```
  pip install cryptography
  ```
2. <span data-ttu-id="762c0-148">Přejděte do adresáře kdekoli uloženy sender.py a capturereader.py a spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="762c0-148">Change your directory to wherever you saved sender.py and capturereader.py, and run this command:</span></span>
   
  ```
  start python sender.py
  ```
   
  <span data-ttu-id="762c0-149">Tento příkaz spustí nový proces Python ke spuštění odesílatele.</span><span class="sxs-lookup"><span data-stu-id="762c0-149">This command starts a new Python process to run the sender.</span></span>
3. <span data-ttu-id="762c0-150">Nyní Počkejte několik minut pro zaznamenání ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="762c0-150">Now wait a few minutes for the capture to run.</span></span> <span data-ttu-id="762c0-151">Potom zadejte do původní okno příkazového řádku následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="762c0-151">Then type the following command into your original command window:</span></span>
   
   ```
   python capturereader.py
   ```

   <span data-ttu-id="762c0-152">Tento snímek procesoru používá místní adresář ke stažení všechny objekty BLOB z účtu úložiště nebo kontejneru.</span><span class="sxs-lookup"><span data-stu-id="762c0-152">This capture processor uses the local directory to download all the blobs from the storage account/container.</span></span> <span data-ttu-id="762c0-153">Zpracuje všechny, které nejsou prázdné a zapíše výsledky jako soubory .csv do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="762c0-153">It processes any that are not empty, and writes the results as .csv files into the local directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="762c0-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="762c0-154">Next steps</span></span>

<span data-ttu-id="762c0-155">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="762c0-155">You can learn more about Event Hubs by visiting the following links:</span></span>

* <span data-ttu-id="762c0-156">[Přehled služby Event Hubs zachycení][Overview of Event Hubs Capture]</span><span class="sxs-lookup"><span data-stu-id="762c0-156">[Overview of Event Hubs Capture][Overview of Event Hubs Capture]</span></span>
* <span data-ttu-id="762c0-157">Úplná [ukázková aplikace, která používá službu Event Hubs][sample application that uses Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="762c0-157">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs].</span></span>
* <span data-ttu-id="762c0-158">[Horizontální navýšení kapacity zpracování událostí ve službě Event Hubs][Scale out Event Processing with Event Hubs] – ukázka</span><span class="sxs-lookup"><span data-stu-id="762c0-158">The [Scale out Event Processing with Event Hubs][Scale out Event Processing with Event Hubs] sample.</span></span>
* <span data-ttu-id="762c0-159">[Přehled služby Event Hubs][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="762c0-159">[Event Hubs overview][Event Hubs overview]</span></span>

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
