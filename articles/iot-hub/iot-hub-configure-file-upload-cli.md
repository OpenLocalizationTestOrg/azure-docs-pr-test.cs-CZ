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
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>Konfigurace centra IoT nahrávání souborů pomocí rozhraní příkazového řádku Azure

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

toouse hello [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet služby Azure Storage pomocí služby IoT hub. Můžete použít existující účet úložiště nebo vytvořte novou.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Rozhraní příkazového řádku Azure 2.0][lnk-CLI-install].
* Centrum Azure IoT. Pokud nemáte Centrum IoT, můžete použít hello `az iot hub create` [příkaz] [ lnk-cli-create-iothub] toocreate jednu, nebo použijte hello portál příliš [vytvoření služby IoT hub] [lnk-portal-hub].
* Účet úložiště Azure. Pokud nemáte účet úložiště Azure, můžete použít hello [Azure CLI 2.0 - spravovat účty úložiště] [ lnk-manage-storage] toocreate, jednu, nebo použijte hello portál příliš[vytvořit účet úložiště] [lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Přihlaste se a nastavit váš účet Azure

Přihlaste se tooyour účet Azure a vybrat své předplatné.

1. Na příkazovém řádku hello spustit hello [přihlášení příkaz][lnk-login-command]:

    ```azurecli
    az login
    ```

    Postupujte podle tooauthenticate pokyny hello pomocí kódu hello a přihlaste se tooyour účet Azure prostřednictvím webového prohlížeče.

1. Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello Azure účty přidružené k přihlašovacích údajů. Použijte hello [toolist příkaz hello účty Azure] [ lnk-az-account-command] k dispozici pro toouse můžete:

    ```azurecli
    az account list
    ```

    Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toocreate služby IoT hub hello. Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>Načtení podrobností o účtu úložiště

Hello následující kroky Předpokládejme, že váš účet úložiště pomocí hello **Resource Manager** modelu nasazení a ne hello **Classic** modelu nasazení.

nahrávání souborů tooconfigure z vašich zařízení, musíte hello připojovací řetězec pro účet úložiště Azure. účet úložiště Hello musí být v hello stejnému předplatnému jako služby IoT hub. Budete také potřebovat hello název kontejneru objektů blob v účtu úložiště hello. Použijte následující příkaz tooretrieve hello klíče účtu úložiště:

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

Poznamenejte si hello **connectionString** hodnotu. Musíte ho v hello následující kroky.

Můžete použít existující kontejner objektů blob pro vaše nahrávání souborů nebo vytvořte nové:

* toolist hello existující objekt blob kontejnery v účtu úložiště, použijte následující příkaz hello:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* toocreate kontejner objektů blob v účtu úložiště, hello použijte následující příkaz:

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Nahrávání souborů

Teď můžete nakonfigurovat vaše IoT hub tooenable [souboru nahrávání funkce] [ lnk-upload] pomocí údaje o vašem účtu úložiště.

Konfigurace Hello vyžaduje hello následující hodnoty:

**Kontejner úložiště**: kontejner objektů blob v účtu úložiště Azure ve vaší aktuální předplatné tooassociate službou IoT hub. Můžete načíst informace o účtu úložiště potřebné hello v předcházející části hello. Identifikátory URI SAS služby IoT Hub automaticky generuje s zápisu oprávnění toothis kontejner objektů blob pro toouse zařízení, když odesílají soubory.

**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru.

**Hodnota TTL SAS**: Toto nastavení je hello time-to-live z hello identifikátory URI SAS vrácený toohello zařízení IoT Hub. Ve výchozím nastavení tooone hodinu.

**Upozornění na nastavení výchozí hodnota TTL souboru**: hello time-to-live oznámení nahrávání souboru předtím, než vyprší jejich platnost. Ve výchozím nastavení tooone den.

**Soubor doručení maximální počet oznámení**: hello kolikrát hello toodeliver pokusy o IoT Hub oznámení nahrávání souboru. Ve výchozím nastavení too10.

Použijte hello následující tooconfigure příkazy rozhraní příkazového řádku Azure k hello nastavení nahrávání souborů ve službě IoT hub:

K využívání prostředí bash:

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Na Windows použijte příkazový řádek:

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Konfigurace nahrávání souborů hello můžete zkontrolovat ve službě IoT hub pomocí hello následující příkaz:

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a>Další kroky

Další informace o možnostech nahrávání souboru hello Centrum IoT najdete v tématu [nahrání souborů ze zařízení][lnk-upload].

Použijte tyto odkazy toolearn informace o správě Azure IoT Hub:

* [Hromadné spravovat zařízení IoT][lnk-bulk]
* [Metriky služby IoT Hub][lnk-metrics]
* [Monitorování operací][lnk-monitor]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s hranou IoT][lnk-iotedge]
* [Zabezpečení řešení IoT z hello pozadí][lnk-securing]

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