---
title: "nahrávání souborů aaaUse hello prostředí Azure PowerShell tooconfigure | Microsoft Docs"
description: "Jak tooconfigure rutin prostředí Azure PowerShell hello toouse souboru IoT hub tooenable ukládání z připojených zařízení. Obsahuje informace o konfiguraci hello, cílový účet úložiště Azure."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 9dcdc41693c09cece411921b30c91d7b3db47395
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a><span data-ttu-id="ca967-104">Konfigurace centra IoT nahrávání souborů pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca967-104">Configure IoT Hub file uploads using PowerShell</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="ca967-105">toouse hello [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet úložiště Azure službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ca967-105">toouse hello [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure storage account with your IoT hub.</span></span> <span data-ttu-id="ca967-106">Můžete použít existující účet úložiště nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="ca967-106">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="ca967-107">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="ca967-107">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="ca967-108">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ca967-108">An active Azure account.</span></span> <span data-ttu-id="ca967-109">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="ca967-109">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="ca967-110">[Rutiny Azure PowerShell][lnk-powershell-install].</span><span class="sxs-lookup"><span data-stu-id="ca967-110">[Azure PowerShell cmdlets][lnk-powershell-install].</span></span>
* <span data-ttu-id="ca967-111">Centrum Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="ca967-111">An Azure IoT hub.</span></span> <span data-ttu-id="ca967-112">Pokud nemáte Centrum IoT, můžete použít hello [rutiny New-AzureRmIoTHub] [ lnk-powershell-iothub] toocreate jednu, nebo použijte hello portál příliš[vytvoření služby IoT hub] [ lnk-portal-hub].</span><span class="sxs-lookup"><span data-stu-id="ca967-112">If you don't have an IoT hub, you can use hello [New-AzureRmIoTHub cmdlet][lnk-powershell-iothub] toocreate one or use hello portal too[Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="ca967-113">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ca967-113">An Azure storage account.</span></span> <span data-ttu-id="ca967-114">Pokud nemáte účet úložiště Azure, můžete použít hello [rutin Azure Powershellu úložiště] [ lnk-powershell-storage] toocreate jednu, nebo použijte hello portál příliš[vytvořit účet úložiště] [ lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="ca967-114">If you don't have an Azure storage account, you can use hello [Azure Storage PowerShell cmdlets][lnk-powershell-storage] toocreate one or use hello portal too[Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="ca967-115">Přihlaste se a nastavit váš účet Azure</span><span class="sxs-lookup"><span data-stu-id="ca967-115">Sign in and set your Azure account</span></span>

<span data-ttu-id="ca967-116">Přihlaste se tooyour účet Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="ca967-116">Sign in tooyour Azure account and select your subscription.</span></span>

1. <span data-ttu-id="ca967-117">V příkazovém řádku prostředí PowerShell text hello, spusťte hello **Login-AzureRmAccount** rutiny:</span><span class="sxs-lookup"><span data-stu-id="ca967-117">At hello PowerShell prompt, run hello **Login-AzureRmAccount** cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="ca967-118">Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="ca967-118">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="ca967-119">Použijte následující příkaz toolist hello předplatná Azure k dispozici pro vás toouse hello:</span><span class="sxs-lookup"><span data-stu-id="ca967-119">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="ca967-120">Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toomanage služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="ca967-120">Use hello following command tooselect subscription that you want toouse toorun hello commands toomanage your IoT hub.</span></span> <span data-ttu-id="ca967-121">Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:</span><span class="sxs-lookup"><span data-stu-id="ca967-121">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="ca967-122">Načtení podrobností o účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="ca967-122">Retrieve your storage account details</span></span>

<span data-ttu-id="ca967-123">Hello následující kroky Předpokládejme, že váš účet úložiště pomocí hello **Resource Manager** modelu nasazení a ne hello **Classic** modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="ca967-123">hello following steps assume that you created your storage account using hello **Resource Manager** deployment model, and not hello **Classic** deployment model.</span></span>

<span data-ttu-id="ca967-124">nahrávání souborů tooconfigure z vašich zařízení, musíte hello připojovací řetězec pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ca967-124">tooconfigure file uploads from your devices, you need hello connection string for an Azure storage account.</span></span> <span data-ttu-id="ca967-125">účet úložiště Hello musí být v hello stejnému předplatnému jako služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ca967-125">hello storage account must be in hello same subscription as your IoT hub.</span></span> <span data-ttu-id="ca967-126">Budete také potřebovat hello název kontejneru objektů blob v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ca967-126">You also need hello name of a blob container in hello storage account.</span></span> <span data-ttu-id="ca967-127">Použijte následující příkaz tooretrieve hello klíče účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="ca967-127">Use hello following command tooretrieve your storage account keys:</span></span>

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

<span data-ttu-id="ca967-128">Poznamenejte si hello **key1** hodnotu klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ca967-128">Make a note of hello **key1** storage account key value.</span></span> <span data-ttu-id="ca967-129">Musíte ho v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="ca967-129">You need it in hello following steps.</span></span>

<span data-ttu-id="ca967-130">Můžete použít existující kontejner objektů blob pro vaše nahrávání souborů nebo vytvořte nové:</span><span class="sxs-lookup"><span data-stu-id="ca967-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="ca967-131">toolist hello existující objekt blob kontejnery v účtu úložiště používají hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ca967-131">toolist hello existing blob containers in your storage account, use hello following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* <span data-ttu-id="ca967-132">toocreate kontejner objektů blob v účtu úložiště, hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ca967-132">toocreate a blob container in your storage account, use hello following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a><span data-ttu-id="ca967-133">Konfigurace centra IoT</span><span class="sxs-lookup"><span data-stu-id="ca967-133">Configure your IoT hub</span></span>

<span data-ttu-id="ca967-134">Teď můžete nakonfigurovat vaše IoT hub tooenable [souboru nahrávání funkce] [ lnk-upload] pomocí údaje o vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ca967-134">You can now configure your IoT hub tooenable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="ca967-135">Konfigurace Hello vyžaduje hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ca967-135">hello configuration requires hello following values:</span></span>

<span data-ttu-id="ca967-136">**Kontejner úložiště**: kontejner objektů blob v účtu úložiště Azure ve vaší aktuální předplatné tooassociate službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ca967-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription tooassociate with your IoT hub.</span></span> <span data-ttu-id="ca967-137">Můžete načíst informace o účtu úložiště potřebné hello v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="ca967-137">You retrieved hello necessary storage account information in hello preceding section.</span></span> <span data-ttu-id="ca967-138">Identifikátory URI SAS služby IoT Hub automaticky generuje s zápisu oprávnění toothis kontejner objektů blob pro toouse zařízení, když odesílají soubory.</span><span class="sxs-lookup"><span data-stu-id="ca967-138">IoT Hub automatically generates SAS URIs with write permissions toothis blob container for devices toouse when they upload files.</span></span>

<span data-ttu-id="ca967-139">**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="ca967-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="ca967-140">**Hodnota TTL SAS**: Toto nastavení je hello time-to-live z hello identifikátory URI SAS vrácený toohello zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ca967-140">**SAS TTL**: This setting is hello time-to-live of hello SAS URIs returned toohello device by IoT Hub.</span></span> <span data-ttu-id="ca967-141">Ve výchozím nastavení tooone hodinu.</span><span class="sxs-lookup"><span data-stu-id="ca967-141">Set tooone hour by default.</span></span>

<span data-ttu-id="ca967-142">**Upozornění na nastavení výchozí hodnota TTL souboru**: hello time-to-live oznámení nahrávání souboru předtím, než vyprší jejich platnost.</span><span class="sxs-lookup"><span data-stu-id="ca967-142">**File notification settings default TTL**: hello time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="ca967-143">Ve výchozím nastavení tooone den.</span><span class="sxs-lookup"><span data-stu-id="ca967-143">Set tooone day by default.</span></span>

<span data-ttu-id="ca967-144">**Soubor doručení maximální počet oznámení**: hello kolikrát hello toodeliver pokusy o IoT Hub oznámení nahrávání souboru.</span><span class="sxs-lookup"><span data-stu-id="ca967-144">**File notification maximum delivery count**: hello number of times hello IoT Hub attempts toodeliver a file upload notification.</span></span> <span data-ttu-id="ca967-145">Ve výchozím nastavení too10.</span><span class="sxs-lookup"><span data-stu-id="ca967-145">Set too10 by default.</span></span>

<span data-ttu-id="ca967-146">Použijte následující nastavení nahrávání souborů hello tooconfigure pro rutiny prostředí PowerShell ve službě IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="ca967-146">Use hello following PowerShell cmdlet tooconfigure hello file upload settings on your IoT hub:</span></span>

```powershell
Set-AzureRmIotHub `
    -ResourceGroupName "{your iot hub resource group}" `
    -Name "{your iot hub name}" `
    -FileUploadNotificationTtl "01:00:00" `
    -FileUploadSasUriTtl "01:00:00" `
    -EnableFileUploadNotifications $true `
    -FileUploadStorageConnectionString "DefaultEndpointsProtocol=https;AccountName={your storage account name};AccountKey={your storage account key};EndpointSuffix=core.windows.net" `
    -FileUploadContainerName "{your blob container name}" `
    -FileUploadNotificationMaxDeliveryCount 10
```

## <a name="next-steps"></a><span data-ttu-id="ca967-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ca967-147">Next steps</span></span>

<span data-ttu-id="ca967-148">Další informace o možnostech nahrávání souboru hello Centrum IoT najdete v tématu [nahrání souborů ze zařízení][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="ca967-148">For more information about hello file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="ca967-149">Použijte tyto odkazy toolearn informace o správě Azure IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="ca967-149">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="ca967-150">[Hromadné spravovat zařízení IoT][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="ca967-150">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="ca967-151">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="ca967-151">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="ca967-152">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="ca967-152">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="ca967-153">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="ca967-153">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="ca967-154">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="ca967-154">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="ca967-155">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="ca967-155">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="ca967-156">[Zabezpečení řešení IoT z hello pozadí][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="ca967-156">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-powershell-storage]: https://docs.microsoft.com/powershell/module/azurerm.storage/
[lnk-powershell-iothub]: https://docs.microsoft.com/powershell/module/azurerm.iothub/new-azurermiothub
[lnk-portal-hub]: iot-hub-create-through-portal.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md