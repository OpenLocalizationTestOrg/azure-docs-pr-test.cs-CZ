---
title: "Konfigurace nahrávání souborů do služby IoT Hub pomocí rozhraní příkazového řádku Azure (az.py) | Microsoft Docs"
description: "Postup konfigurace fileuploads ke službě Azure IoT Hub pomocí Azure CLI a platformy 2.0 (az.py)."
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
ms.openlocfilehash: a9af26d7ebacf5513952786621aaa92f64be263b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a><span data-ttu-id="58cbf-103">Konfigurace centra IoT nahrávání souborů pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="58cbf-103">Configure IoT Hub file uploads using Azure CLI</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="58cbf-104">Použít [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet služby Azure Storage pomocí služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="58cbf-104">To use the [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your IoT hub.</span></span> <span data-ttu-id="58cbf-105">Můžete použít existující účet úložiště nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="58cbf-105">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="58cbf-106">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="58cbf-106">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="58cbf-107">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="58cbf-107">An active Azure account.</span></span> <span data-ttu-id="58cbf-108">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="58cbf-108">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="58cbf-109">[Rozhraní příkazového řádku Azure 2.0][lnk-CLI-install].</span><span class="sxs-lookup"><span data-stu-id="58cbf-109">[Azure CLI 2.0][lnk-CLI-install].</span></span>
* <span data-ttu-id="58cbf-110">Centrum Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="58cbf-110">An Azure IoT hub.</span></span> <span data-ttu-id="58cbf-111">Pokud nemáte Centrum IoT, můžete použít `az iot hub create` [příkaz] [ lnk-cli-create-iothub] vytvořit nebo použít na portálu pro [vytvoření služby IoT hub] [lnk-portal-hub].</span><span class="sxs-lookup"><span data-stu-id="58cbf-111">If you don't have an IoT hub, you can use the `az iot hub create` [command][lnk-cli-create-iothub] to create one or use the portal to [Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="58cbf-112">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="58cbf-112">An Azure Storage account.</span></span> <span data-ttu-id="58cbf-113">Pokud nemáte účet úložiště Azure, můžete použít [Azure CLI 2.0 - spravovat účty úložiště] [ lnk-manage-storage] vytvořit nebo použít portálu [vytvořit účet úložiště][lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="58cbf-113">If you don't have an Azure Storage account, you can use the [Azure CLI 2.0 - Manage storage accounts][lnk-manage-storage] to create one or use the portal to [Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="58cbf-114">Přihlaste se a nastavit váš účet Azure</span><span class="sxs-lookup"><span data-stu-id="58cbf-114">Sign in and set your Azure account</span></span>

<span data-ttu-id="58cbf-115">Přihlaste se k účtu Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="58cbf-115">Sign in to your Azure account and select your subscription.</span></span>

1. <span data-ttu-id="58cbf-116">Na příkazovém řádku, spusťte [přihlášení příkaz][lnk-login-command]:</span><span class="sxs-lookup"><span data-stu-id="58cbf-116">At the command prompt, run the [login command][lnk-login-command]:</span></span>

    ```azurecli
    az login
    ```

    <span data-ttu-id="58cbf-117">Postupujte podle pokynů k ověření pomocí kódu a přihlaste se k účtu Azure prostřednictvím webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="58cbf-117">Follow the instructions to authenticate using the code and sign in to your Azure account through a web browser.</span></span>

1. <span data-ttu-id="58cbf-118">Pokud máte víc předplatných Azure, přihlášení do Azure uděluje přístup k Azure účty přidružené přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="58cbf-118">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure accounts associated with your credentials.</span></span> <span data-ttu-id="58cbf-119">Použijte následující [seznam účtů Azure příkazu] [ lnk-az-account-command] k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="58cbf-119">Use the following [command to list the Azure accounts][lnk-az-account-command] available for you to use:</span></span>

    ```azurecli
    az account list
    ```

    <span data-ttu-id="58cbf-120">Pomocí následujícího příkazu vyberte předplatné, které chcete použít ke spuštění příkazů pro vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="58cbf-120">Use the following command to select subscription that you want to use to run the commands to create your IoT hub.</span></span> <span data-ttu-id="58cbf-121">Z výstupu předchozí příkaz můžete použít buď název odběru nebo ID:</span><span class="sxs-lookup"><span data-stu-id="58cbf-121">You can use either the subscription name or ID from the output of the previous command:</span></span>

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="58cbf-122">Načtení podrobností o účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="58cbf-122">Retrieve your storage account details</span></span>

<span data-ttu-id="58cbf-123">Následující postup předpokládá, že jste vytvořili pomocí účtu úložiště **Resource Manager** model nasazení a ne **Classic** modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="58cbf-123">The following steps assume that you created your storage account using the **Resource Manager** deployment model, and not the **Classic** deployment model.</span></span>

<span data-ttu-id="58cbf-124">Pokud chcete konfigurovat nahrávání souborů ze zařízení, musíte připojovací řetězec pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="58cbf-124">To configure file uploads from your devices, you need the connection string for an Azure storage account.</span></span> <span data-ttu-id="58cbf-125">Účet úložiště musí být ve stejném předplatném jako služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="58cbf-125">The storage account must be in the same subscription as your IoT hub.</span></span> <span data-ttu-id="58cbf-126">Musíte také název kontejneru objektů blob v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="58cbf-126">You also need the name of a blob container in the storage account.</span></span> <span data-ttu-id="58cbf-127">Použijte následující příkaz k načtení klíče účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="58cbf-127">Use the following command to retrieve your storage account keys:</span></span>

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

<span data-ttu-id="58cbf-128">Poznamenejte si **connectionString** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="58cbf-128">Make a note of the **connectionString** value.</span></span> <span data-ttu-id="58cbf-129">Musíte ho v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="58cbf-129">You need it in the following steps.</span></span>

<span data-ttu-id="58cbf-130">Můžete použít existující kontejner objektů blob pro vaše nahrávání souborů nebo vytvořte nové:</span><span class="sxs-lookup"><span data-stu-id="58cbf-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="58cbf-131">K zobrazení seznamu existující kontejnery objektů blob v účtu úložiště, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="58cbf-131">To list the existing blob containers in your storage account, use the following command:</span></span>

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* <span data-ttu-id="58cbf-132">Pokud chcete vytvořit kontejner objektů blob v účtu úložiště, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="58cbf-132">To create a blob container in your storage account, use the following command:</span></span>

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a><span data-ttu-id="58cbf-133">Nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="58cbf-133">File upload</span></span>

<span data-ttu-id="58cbf-134">Teď můžete nakonfigurovat službu IoT hub, chcete-li povolit [souboru nahrávání funkce] [ lnk-upload] pomocí údaje o vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="58cbf-134">You can now configure your IoT hub to enable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="58cbf-135">Následující konfigurace vyžaduje následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="58cbf-135">The configuration requires the following values:</span></span>

<span data-ttu-id="58cbf-136">**Kontejner úložiště**: kontejner objektů blob v účtu úložiště Azure v aktuálním předplatném Azure přidružit služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="58cbf-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription to associate with your IoT hub.</span></span> <span data-ttu-id="58cbf-137">Můžete načíst informace o účtu potřeby úložiště v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="58cbf-137">You retrieved the necessary storage account information in the preceding section.</span></span> <span data-ttu-id="58cbf-138">IoT Hub automaticky vygeneruje identifikátory URI SAS s oprávněními k zápisu do tohoto kontejneru objektů blob pro zařízení se má použít při odesílají soubory.</span><span class="sxs-lookup"><span data-stu-id="58cbf-138">IoT Hub automatically generates SAS URIs with write permissions to this blob container for devices to use when they upload files.</span></span>

<span data-ttu-id="58cbf-139">**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="58cbf-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="58cbf-140">**Hodnota TTL SAS**: Toto nastavení je time-to-live identifikátorů URI SAS, vrácený do zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="58cbf-140">**SAS TTL**: This setting is the time-to-live of the SAS URIs returned to the device by IoT Hub.</span></span> <span data-ttu-id="58cbf-141">Ve výchozím nastavení má jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="58cbf-141">Set to one hour by default.</span></span>

<span data-ttu-id="58cbf-142">**Upozornění na nastavení výchozí hodnota TTL souboru**: time-to-live souboru odeslat oznámení, než vyprší jejich platnost.</span><span class="sxs-lookup"><span data-stu-id="58cbf-142">**File notification settings default TTL**: The time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="58cbf-143">Ve výchozím nastavení má jeden den.</span><span class="sxs-lookup"><span data-stu-id="58cbf-143">Set to one day by default.</span></span>

<span data-ttu-id="58cbf-144">**Soubor doručení maximální počet oznámení**: počet, kolikrát se Centrum IoT pokusí doručit soubor odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="58cbf-144">**File notification maximum delivery count**: The number of times the IoT Hub attempts to deliver a file upload notification.</span></span> <span data-ttu-id="58cbf-145">Ve výchozím nastavení má 10.</span><span class="sxs-lookup"><span data-stu-id="58cbf-145">Set to 10 by default.</span></span>

<span data-ttu-id="58cbf-146">Pomocí následujících příkazů příkazového řádku Azure CLI ke konfiguraci nastavení nahrávání souborů ve službě IoT hub:</span><span class="sxs-lookup"><span data-stu-id="58cbf-146">Use the following Azure CLI commands to configure the file upload settings on your IoT hub:</span></span>

<span data-ttu-id="58cbf-147">K využívání prostředí bash:</span><span class="sxs-lookup"><span data-stu-id="58cbf-147">In a bash shell use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="58cbf-148">Na Windows použijte příkazový řádek:</span><span class="sxs-lookup"><span data-stu-id="58cbf-148">At a Windows command prompt use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="58cbf-149">Konfigurace nahrávání souborů můžete zkontrolovat ve službě IoT hub pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="58cbf-149">You can review the file upload configuration on your IoT hub using the following command:</span></span>

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a><span data-ttu-id="58cbf-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="58cbf-150">Next steps</span></span>

<span data-ttu-id="58cbf-151">Další informace o možnostech nahrávání souboru Centrum IoT najdete v tématu [nahrání souborů ze zařízení][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="58cbf-151">For more information about the file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="58cbf-152">Další informace o správě Azure IoT Hub na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="58cbf-152">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="58cbf-153">[Hromadné spravovat zařízení IoT][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="58cbf-153">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="58cbf-154">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="58cbf-154">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="58cbf-155">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="58cbf-155">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="58cbf-156">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="58cbf-156">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="58cbf-157">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="58cbf-157">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="58cbf-158">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="58cbf-158">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="58cbf-159">[Zabezpečení řešení IoT od základů nahoru][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="58cbf-159">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md


[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-manage-storage]:../storage/common/storage-azure-cli.md#manage-storage-accounts
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md
[lnk-cli-create-iothub]: https://docs.microsoft.com/cli/azure/iot/hub#create