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
# <a name="configure-iot-hub-file-uploads-using-powershell"></a>Konfigurace centra IoT nahrávání souborů pomocí prostředí PowerShell

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

toouse hello [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet úložiště Azure službou IoT hub. Můžete použít existující účet úložiště nebo vytvořte novou.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Rutiny Azure PowerShell][lnk-powershell-install].
* Centrum Azure IoT. Pokud nemáte Centrum IoT, můžete použít hello [rutiny New-AzureRmIoTHub] [ lnk-powershell-iothub] toocreate jednu, nebo použijte hello portál příliš[vytvoření služby IoT hub] [ lnk-portal-hub].
* Účet úložiště Azure. Pokud nemáte účet úložiště Azure, můžete použít hello [rutin Azure Powershellu úložiště] [ lnk-powershell-storage] toocreate jednu, nebo použijte hello portál příliš[vytvořit účet úložiště] [ lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Přihlaste se a nastavit váš účet Azure

Přihlaste se tooyour účet Azure a vybrat své předplatné.

1. V příkazovém řádku prostředí PowerShell text hello, spusťte hello **Login-AzureRmAccount** rutiny:

    ```powershell
    Login-AzureRmAccount
    ```

1. Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello předplatná Azure přidružená přihlašovacích údajů. Použijte následující příkaz toolist hello předplatná Azure k dispozici pro vás toouse hello:

    ```powershell
    Get-AzureRMSubscription
    ```

    Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toomanage služby IoT hub hello. Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a>Načtení podrobností o účtu úložiště

Hello následující kroky Předpokládejme, že váš účet úložiště pomocí hello **Resource Manager** modelu nasazení a ne hello **Classic** modelu nasazení.

nahrávání souborů tooconfigure z vašich zařízení, musíte hello připojovací řetězec pro účet úložiště Azure. účet úložiště Hello musí být v hello stejnému předplatnému jako služby IoT hub. Budete také potřebovat hello název kontejneru objektů blob v účtu úložiště hello. Použijte následující příkaz tooretrieve hello klíče účtu úložiště:

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

Poznamenejte si hello **key1** hodnotu klíče účtu úložiště. Musíte ho v hello následující kroky.

Můžete použít existující kontejner objektů blob pro vaše nahrávání souborů nebo vytvořte nové:

* toolist hello existující objekt blob kontejnery v účtu úložiště používají hello následující příkazy:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* toocreate kontejner objektů blob v účtu úložiště, hello použijte následující příkazy:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a>Konfigurace centra IoT

Teď můžete nakonfigurovat vaše IoT hub tooenable [souboru nahrávání funkce] [ lnk-upload] pomocí údaje o vašem účtu úložiště.

Konfigurace Hello vyžaduje hello následující hodnoty:

**Kontejner úložiště**: kontejner objektů blob v účtu úložiště Azure ve vaší aktuální předplatné tooassociate službou IoT hub. Můžete načíst informace o účtu úložiště potřebné hello v předcházející části hello. Identifikátory URI SAS služby IoT Hub automaticky generuje s zápisu oprávnění toothis kontejner objektů blob pro toouse zařízení, když odesílají soubory.

**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru.

**Hodnota TTL SAS**: Toto nastavení je hello time-to-live z hello identifikátory URI SAS vrácený toohello zařízení IoT Hub. Ve výchozím nastavení tooone hodinu.

**Upozornění na nastavení výchozí hodnota TTL souboru**: hello time-to-live oznámení nahrávání souboru předtím, než vyprší jejich platnost. Ve výchozím nastavení tooone den.

**Soubor doručení maximální počet oznámení**: hello kolikrát hello toodeliver pokusy o IoT Hub oznámení nahrávání souboru. Ve výchozím nastavení too10.

Použijte následující nastavení nahrávání souborů hello tooconfigure pro rutiny prostředí PowerShell ve službě IoT hub hello:

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