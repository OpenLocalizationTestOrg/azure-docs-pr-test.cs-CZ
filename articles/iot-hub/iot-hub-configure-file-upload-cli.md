---
title: "tooIoT nahrávání souboru aaaConfigure rozbočovač za použití rozhraní příkazového řádku Azure (az.py) | Microsoft Docs"
description: "Jak tooconfigure fileuploads tooAzure IoT Hub pomocí hello a platformy Azure CLI 2.0 (az.py)."
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
ms.openlocfilehash: 390113df2d96df9833b6aa383ed66805528614a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a><span data-ttu-id="8b1f2-103">Konfigurace centra IoT nahrávání souborů pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8b1f2-103">Configure IoT Hub file uploads using Azure CLI</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="8b1f2-104">toouse hello [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet služby Azure Storage pomocí služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-104">toouse hello [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your IoT hub.</span></span> <span data-ttu-id="8b1f2-105">Můžete použít existující účet úložiště nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-105">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="8b1f2-106">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-106">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="8b1f2-107">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-107">An active Azure account.</span></span> <span data-ttu-id="8b1f2-108">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="8b1f2-108">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="8b1f2-109">[Rozhraní příkazového řádku Azure 2.0][lnk-CLI-install].</span><span class="sxs-lookup"><span data-stu-id="8b1f2-109">[Azure CLI 2.0][lnk-CLI-install].</span></span>
* <span data-ttu-id="8b1f2-110">Centrum Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-110">An Azure IoT hub.</span></span> <span data-ttu-id="8b1f2-111">Pokud nemáte Centrum IoT, můžete použít hello `az iot hub create` [příkaz] [ lnk-cli-create-iothub] toocreate jednu, nebo použijte hello portál příliš [vytvoření služby IoT hub] [lnk-portal-hub].</span><span class="sxs-lookup"><span data-stu-id="8b1f2-111">If you don't have an IoT hub, you can use hello `az iot hub create` [command][lnk-cli-create-iothub] toocreate one or use hello portal too[Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="8b1f2-112">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-112">An Azure Storage account.</span></span> <span data-ttu-id="8b1f2-113">Pokud nemáte účet úložiště Azure, můžete použít hello [Azure CLI 2.0 - spravovat účty úložiště] [ lnk-manage-storage] toocreate, jednu, nebo použijte hello portál příliš[vytvořit účet úložiště] [lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="8b1f2-113">If you don't have an Azure Storage account, you can use hello [Azure CLI 2.0 - Manage storage accounts][lnk-manage-storage] toocreate one or use hello portal too[Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="8b1f2-114">Přihlaste se a nastavit váš účet Azure</span><span class="sxs-lookup"><span data-stu-id="8b1f2-114">Sign in and set your Azure account</span></span>

<span data-ttu-id="8b1f2-115">Přihlaste se tooyour účet Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-115">Sign in tooyour Azure account and select your subscription.</span></span>

1. <span data-ttu-id="8b1f2-116">Na příkazovém řádku hello spustit hello [přihlášení příkaz][lnk-login-command]:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-116">At hello command prompt, run hello [login command][lnk-login-command]:</span></span>

    ```azurecli
    az login
    ```

    <span data-ttu-id="8b1f2-117">Postupujte podle tooauthenticate pokyny hello pomocí kódu hello a přihlaste se tooyour účet Azure prostřednictvím webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-117">Follow hello instructions tooauthenticate using hello code and sign in tooyour Azure account through a web browser.</span></span>

1. <span data-ttu-id="8b1f2-118">Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello Azure účty přidružené k přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-118">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure accounts associated with your credentials.</span></span> <span data-ttu-id="8b1f2-119">Použijte hello [toolist příkaz hello účty Azure] [ lnk-az-account-command] k dispozici pro toouse můžete:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-119">Use hello following [command toolist hello Azure accounts][lnk-az-account-command] available for you toouse:</span></span>

    ```azurecli
    az account list
    ```

    <span data-ttu-id="8b1f2-120">Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toocreate služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-120">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="8b1f2-121">Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-121">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="8b1f2-122">Načtení podrobností o účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="8b1f2-122">Retrieve your storage account details</span></span>

<span data-ttu-id="8b1f2-123">Hello následující kroky Předpokládejme, že váš účet úložiště pomocí hello **Resource Manager** modelu nasazení a ne hello **Classic** modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-123">hello following steps assume that you created your storage account using hello **Resource Manager** deployment model, and not hello **Classic** deployment model.</span></span>

<span data-ttu-id="8b1f2-124">nahrávání souborů tooconfigure z vašich zařízení, musíte hello připojovací řetězec pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-124">tooconfigure file uploads from your devices, you need hello connection string for an Azure storage account.</span></span> <span data-ttu-id="8b1f2-125">účet úložiště Hello musí být v hello stejnému předplatnému jako služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-125">hello storage account must be in hello same subscription as your IoT hub.</span></span> <span data-ttu-id="8b1f2-126">Budete také potřebovat hello název kontejneru objektů blob v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-126">You also need hello name of a blob container in hello storage account.</span></span> <span data-ttu-id="8b1f2-127">Použijte následující příkaz tooretrieve hello klíče účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-127">Use hello following command tooretrieve your storage account keys:</span></span>

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

<span data-ttu-id="8b1f2-128">Poznamenejte si hello **connectionString** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-128">Make a note of hello **connectionString** value.</span></span> <span data-ttu-id="8b1f2-129">Musíte ho v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-129">You need it in hello following steps.</span></span>

<span data-ttu-id="8b1f2-130">Můžete použít existující kontejner objektů blob pro vaše nahrávání souborů nebo vytvořte nové:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="8b1f2-131">toolist hello existující objekt blob kontejnery v účtu úložiště, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-131">toolist hello existing blob containers in your storage account, use hello following command:</span></span>

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* <span data-ttu-id="8b1f2-132">toocreate kontejner objektů blob v účtu úložiště, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-132">toocreate a blob container in your storage account, use hello following command:</span></span>

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a><span data-ttu-id="8b1f2-133">Nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="8b1f2-133">File upload</span></span>

<span data-ttu-id="8b1f2-134">Teď můžete nakonfigurovat vaše IoT hub tooenable [souboru nahrávání funkce] [ lnk-upload] pomocí údaje o vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-134">You can now configure your IoT hub tooenable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="8b1f2-135">Konfigurace Hello vyžaduje hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-135">hello configuration requires hello following values:</span></span>

<span data-ttu-id="8b1f2-136">**Kontejner úložiště**: kontejner objektů blob v účtu úložiště Azure ve vaší aktuální předplatné tooassociate službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription tooassociate with your IoT hub.</span></span> <span data-ttu-id="8b1f2-137">Můžete načíst informace o účtu úložiště potřebné hello v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-137">You retrieved hello necessary storage account information in hello preceding section.</span></span> <span data-ttu-id="8b1f2-138">Identifikátory URI SAS služby IoT Hub automaticky generuje s zápisu oprávnění toothis kontejner objektů blob pro toouse zařízení, když odesílají soubory.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-138">IoT Hub automatically generates SAS URIs with write permissions toothis blob container for devices toouse when they upload files.</span></span>

<span data-ttu-id="8b1f2-139">**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="8b1f2-140">**Hodnota TTL SAS**: Toto nastavení je hello time-to-live z hello identifikátory URI SAS vrácený toohello zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-140">**SAS TTL**: This setting is hello time-to-live of hello SAS URIs returned toohello device by IoT Hub.</span></span> <span data-ttu-id="8b1f2-141">Ve výchozím nastavení tooone hodinu.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-141">Set tooone hour by default.</span></span>

<span data-ttu-id="8b1f2-142">**Upozornění na nastavení výchozí hodnota TTL souboru**: hello time-to-live oznámení nahrávání souboru předtím, než vyprší jejich platnost.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-142">**File notification settings default TTL**: hello time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="8b1f2-143">Ve výchozím nastavení tooone den.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-143">Set tooone day by default.</span></span>

<span data-ttu-id="8b1f2-144">**Soubor doručení maximální počet oznámení**: hello kolikrát hello toodeliver pokusy o IoT Hub oznámení nahrávání souboru.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-144">**File notification maximum delivery count**: hello number of times hello IoT Hub attempts toodeliver a file upload notification.</span></span> <span data-ttu-id="8b1f2-145">Ve výchozím nastavení too10.</span><span class="sxs-lookup"><span data-stu-id="8b1f2-145">Set too10 by default.</span></span>

<span data-ttu-id="8b1f2-146">Použijte hello následující tooconfigure příkazy rozhraní příkazového řádku Azure k hello nastavení nahrávání souborů ve službě IoT hub:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-146">Use hello following Azure CLI commands tooconfigure hello file upload settings on your IoT hub:</span></span>

<span data-ttu-id="8b1f2-147">K využívání prostředí bash:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-147">In a bash shell use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="8b1f2-148">Na Windows použijte příkazový řádek:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-148">At a Windows command prompt use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="8b1f2-149">Konfigurace nahrávání souborů hello můžete zkontrolovat ve službě IoT hub pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-149">You can review hello file upload configuration on your IoT hub using hello following command:</span></span>

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a><span data-ttu-id="8b1f2-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8b1f2-150">Next steps</span></span>

<span data-ttu-id="8b1f2-151">Další informace o možnostech nahrávání souboru hello Centrum IoT najdete v tématu [nahrání souborů ze zařízení][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="8b1f2-151">For more information about hello file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="8b1f2-152">Použijte tyto odkazy toolearn informace o správě Azure IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-152">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="8b1f2-153">[Hromadné spravovat zařízení IoT][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="8b1f2-153">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="8b1f2-154">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="8b1f2-154">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="8b1f2-155">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="8b1f2-155">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="8b1f2-156">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="8b1f2-156">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="8b1f2-157">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="8b1f2-157">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="8b1f2-158">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="8b1f2-158">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="8b1f2-159">[Zabezpečení řešení IoT z hello pozadí][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="8b1f2-159">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

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