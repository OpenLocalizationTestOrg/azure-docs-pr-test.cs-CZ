---
title: "Pomocí prostředí Azure PowerShell ke konfiguraci nahrávání souborů | Microsoft Docs"
description: "Postup použití rutin prostředí Azure PowerShell ke konfiguraci služby IoT hub, aby soubor ukládání z připojených zařízení. Obsahuje informace o konfiguraci cílového účtu úložiště Azure."
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
ms.openlocfilehash: a72bda794b2da3e044c46249559610d06b1f1843
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a><span data-ttu-id="3eb18-104">Konfigurace centra IoT nahrávání souborů pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3eb18-104">Configure IoT Hub file uploads using PowerShell</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="3eb18-105">Použít [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet úložiště Azure službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3eb18-105">To use the [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure storage account with your IoT hub.</span></span> <span data-ttu-id="3eb18-106">Můžete použít existující účet úložiště nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="3eb18-106">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="3eb18-107">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3eb18-107">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="3eb18-108">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3eb18-108">An active Azure account.</span></span> <span data-ttu-id="3eb18-109">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="3eb18-109">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="3eb18-110">[Rutiny Azure PowerShell][lnk-powershell-install].</span><span class="sxs-lookup"><span data-stu-id="3eb18-110">[Azure PowerShell cmdlets][lnk-powershell-install].</span></span>
* <span data-ttu-id="3eb18-111">Centrum Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="3eb18-111">An Azure IoT hub.</span></span> <span data-ttu-id="3eb18-112">Pokud nemáte Centrum IoT, můžete použít [rutiny New-AzureRmIoTHub] [ lnk-powershell-iothub] vytvořit nebo použít portálu [vytvoření služby IoT hub][lnk-portal-hub].</span><span class="sxs-lookup"><span data-stu-id="3eb18-112">If you don't have an IoT hub, you can use the [New-AzureRmIoTHub cmdlet][lnk-powershell-iothub] to create one or use the portal to [Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="3eb18-113">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3eb18-113">An Azure storage account.</span></span> <span data-ttu-id="3eb18-114">Pokud nemáte účet úložiště Azure, můžete použít [rutin Azure Powershellu úložiště] [ lnk-powershell-storage] vytvořit nebo použít portálu [vytvořit účet úložiště][lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="3eb18-114">If you don't have an Azure storage account, you can use the [Azure Storage PowerShell cmdlets][lnk-powershell-storage] to create one or use the portal to [Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="3eb18-115">Přihlaste se a nastavit váš účet Azure</span><span class="sxs-lookup"><span data-stu-id="3eb18-115">Sign in and set your Azure account</span></span>

<span data-ttu-id="3eb18-116">Přihlaste se k účtu Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="3eb18-116">Sign in to your Azure account and select your subscription.</span></span>

1. <span data-ttu-id="3eb18-117">V řádku prostředí PowerShell, spusťte **Login-AzureRmAccount** rutiny:</span><span class="sxs-lookup"><span data-stu-id="3eb18-117">At the PowerShell prompt, run the **Login-AzureRmAccount** cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="3eb18-118">Pokud máte víc předplatných Azure, přihlášení do Azure uděluje přístup do všech předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="3eb18-118">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="3eb18-119">Pomocí následujícího příkazu zobrazíte seznam předplatných Azure, které je k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="3eb18-119">Use the following command to list the Azure subscriptions available for you to use:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="3eb18-120">Pomocí následujícího příkazu vyberte předplatné, které chcete použít ke spuštění příkazů ke správě služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3eb18-120">Use the following command to select subscription that you want to use to run the commands to manage your IoT hub.</span></span> <span data-ttu-id="3eb18-121">Z výstupu předchozí příkaz můžete použít buď název odběru nebo ID:</span><span class="sxs-lookup"><span data-stu-id="3eb18-121">You can use either the subscription name or ID from the output of the previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="3eb18-122">Načtení podrobností o účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="3eb18-122">Retrieve your storage account details</span></span>

<span data-ttu-id="3eb18-123">Následující postup předpokládá, že jste vytvořili pomocí účtu úložiště **Resource Manager** model nasazení a ne **Classic** modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="3eb18-123">The following steps assume that you created your storage account using the **Resource Manager** deployment model, and not the **Classic** deployment model.</span></span>

<span data-ttu-id="3eb18-124">Pokud chcete konfigurovat nahrávání souborů ze zařízení, musíte připojovací řetězec pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3eb18-124">To configure file uploads from your devices, you need the connection string for an Azure storage account.</span></span> <span data-ttu-id="3eb18-125">Účet úložiště musí být ve stejném předplatném jako služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3eb18-125">The storage account must be in the same subscription as your IoT hub.</span></span> <span data-ttu-id="3eb18-126">Musíte také název kontejneru objektů blob v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3eb18-126">You also need the name of a blob container in the storage account.</span></span> <span data-ttu-id="3eb18-127">Použijte následující příkaz k načtení klíče účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="3eb18-127">Use the following command to retrieve your storage account keys:</span></span>

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

<span data-ttu-id="3eb18-128">Poznamenejte si **key1** hodnotu klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3eb18-128">Make a note of the **key1** storage account key value.</span></span> <span data-ttu-id="3eb18-129">Musíte ho v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="3eb18-129">You need it in the following steps.</span></span>

<span data-ttu-id="3eb18-130">Můžete použít existující kontejner objektů blob pro vaše nahrávání souborů nebo vytvořte nové:</span><span class="sxs-lookup"><span data-stu-id="3eb18-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="3eb18-131">K zobrazení seznamu existující kontejnery objektů blob v účtu úložiště, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="3eb18-131">To list the existing blob containers in your storage account, use the following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* <span data-ttu-id="3eb18-132">Pokud chcete vytvořit kontejner objektů blob v účtu úložiště, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="3eb18-132">To create a blob container in your storage account, use the following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a><span data-ttu-id="3eb18-133">Konfigurace centra IoT</span><span class="sxs-lookup"><span data-stu-id="3eb18-133">Configure your IoT hub</span></span>

<span data-ttu-id="3eb18-134">Teď můžete nakonfigurovat službu IoT hub, chcete-li povolit [souboru nahrávání funkce] [ lnk-upload] pomocí údaje o vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3eb18-134">You can now configure your IoT hub to enable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="3eb18-135">Následující konfigurace vyžaduje následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="3eb18-135">The configuration requires the following values:</span></span>

<span data-ttu-id="3eb18-136">**Kontejner úložiště**: kontejner objektů blob v účtu úložiště Azure v aktuálním předplatném Azure přidružit služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3eb18-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription to associate with your IoT hub.</span></span> <span data-ttu-id="3eb18-137">Můžete načíst informace o účtu potřeby úložiště v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="3eb18-137">You retrieved the necessary storage account information in the preceding section.</span></span> <span data-ttu-id="3eb18-138">IoT Hub automaticky vygeneruje identifikátory URI SAS s oprávněními k zápisu do tohoto kontejneru objektů blob pro zařízení se má použít při odesílají soubory.</span><span class="sxs-lookup"><span data-stu-id="3eb18-138">IoT Hub automatically generates SAS URIs with write permissions to this blob container for devices to use when they upload files.</span></span>

<span data-ttu-id="3eb18-139">**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="3eb18-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="3eb18-140">**Hodnota TTL SAS**: Toto nastavení je time-to-live identifikátorů URI SAS, vrácený do zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="3eb18-140">**SAS TTL**: This setting is the time-to-live of the SAS URIs returned to the device by IoT Hub.</span></span> <span data-ttu-id="3eb18-141">Ve výchozím nastavení má jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="3eb18-141">Set to one hour by default.</span></span>

<span data-ttu-id="3eb18-142">**Upozornění na nastavení výchozí hodnota TTL souboru**: time-to-live souboru odeslat oznámení, než vyprší jejich platnost.</span><span class="sxs-lookup"><span data-stu-id="3eb18-142">**File notification settings default TTL**: The time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="3eb18-143">Ve výchozím nastavení má jeden den.</span><span class="sxs-lookup"><span data-stu-id="3eb18-143">Set to one day by default.</span></span>

<span data-ttu-id="3eb18-144">**Soubor doručení maximální počet oznámení**: počet, kolikrát se Centrum IoT pokusí doručit soubor odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="3eb18-144">**File notification maximum delivery count**: The number of times the IoT Hub attempts to deliver a file upload notification.</span></span> <span data-ttu-id="3eb18-145">Ve výchozím nastavení má 10.</span><span class="sxs-lookup"><span data-stu-id="3eb18-145">Set to 10 by default.</span></span>

<span data-ttu-id="3eb18-146">Pomocí následující rutiny prostředí PowerShell můžete nakonfigurovat soubor nahrát nastavení ve službě IoT hub:</span><span class="sxs-lookup"><span data-stu-id="3eb18-146">Use the following PowerShell cmdlet to configure the file upload settings on your IoT hub:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="3eb18-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3eb18-147">Next steps</span></span>

<span data-ttu-id="3eb18-148">Další informace o možnostech nahrávání souboru Centrum IoT najdete v tématu [nahrání souborů ze zařízení][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="3eb18-148">For more information about the file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="3eb18-149">Další informace o správě Azure IoT Hub na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="3eb18-149">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="3eb18-150">[Hromadné spravovat zařízení IoT][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="3eb18-150">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="3eb18-151">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="3eb18-151">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="3eb18-152">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="3eb18-152">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="3eb18-153">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="3eb18-153">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="3eb18-154">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="3eb18-154">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="3eb18-155">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="3eb18-155">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="3eb18-156">[Zabezpečení řešení IoT od základů nahoru][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="3eb18-156">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

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