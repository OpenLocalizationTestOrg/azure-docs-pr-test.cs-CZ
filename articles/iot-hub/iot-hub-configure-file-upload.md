---
title: "nahrávání souborů Azure portálu tooconfigure hello aaaUse | Microsoft Docs"
description: "Jak toouse hello Azure portálu tooconfigure souboru IoT hub tooenable ukládání z připojených zařízení. Obsahuje informace o konfiguraci hello, cílový účet úložiště Azure."
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
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: b90c3fbed47b4eb144d3cb7480068b7cfc776ba6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-hello-azure-portal"></a><span data-ttu-id="8019c-104">Konfigurace služby IoT Hub nahrávání souborů pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8019c-104">Configure IoT Hub file uploads using hello Azure portal</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a><span data-ttu-id="8019c-105">Nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="8019c-105">File upload</span></span>

<span data-ttu-id="8019c-106">toouse hello [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet služby Azure Storage pomocí centra.</span><span class="sxs-lookup"><span data-stu-id="8019c-106">toouse hello [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your hub.</span></span> <span data-ttu-id="8019c-107">Vyberte **nahrávání souborů** toodisplay seznam vlastností nahrávání souboru pro hello IoT hub, která je upravována.</span><span class="sxs-lookup"><span data-stu-id="8019c-107">Select **File upload** toodisplay a list of file upload properties for hello IoT hub that is being modified.</span></span>

![Nastavení portálu hello odesílání souboru IoT Hub zobrazení][13]

<span data-ttu-id="8019c-109">**Kontejner úložiště**: použití hello Azure portálu tooselect kontejner objektů blob v účtu Azure Storage ve vaší aktuální předplatné tooassociate službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8019c-109">**Storage container**: Use hello Azure portal tooselect a blob container in an Azure Storage account in your current Azure subscription tooassociate with your IoT Hub.</span></span> <span data-ttu-id="8019c-110">Pokud potřeby můžete vytvořit účet úložiště Azure na hello **účty úložiště** okno a objektů blob kontejneru na hello **kontejnery** okno.</span><span class="sxs-lookup"><span data-stu-id="8019c-110">If necessary, you can create an Azure Storage account on hello **Storage accounts** blade and blob container on hello **Containers** blade.</span></span> <span data-ttu-id="8019c-111">Identifikátory URI SAS služby IoT Hub automaticky generuje s zápisu oprávnění toothis kontejner objektů blob pro toouse zařízení, když odesílají soubory.</span><span class="sxs-lookup"><span data-stu-id="8019c-111">IoT Hub automatically generates SAS URIs with write permissions toothis blob container for devices toouse when they upload files.</span></span>

![Zobrazení portálu hello kontejnery úložiště pro nahrávání souborů][14]

<span data-ttu-id="8019c-113">**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru prostřednictvím přepnutí hello.</span><span class="sxs-lookup"><span data-stu-id="8019c-113">**Receive notifications for uploaded files**: Enable or disable file upload notifications via hello toggle.</span></span>

<span data-ttu-id="8019c-114">**Hodnota TTL SAS**: Toto nastavení je hello time-to-live z hello identifikátory URI SAS vrácený toohello zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8019c-114">**SAS TTL**: This setting is hello time-to-live of hello SAS URIs returned toohello device by IoT Hub.</span></span> <span data-ttu-id="8019c-115">Ve výchozím nastavení tooone hodinu, ale může být vlastní tooother hodnoty pomocí posuvníku hello.</span><span class="sxs-lookup"><span data-stu-id="8019c-115">Set tooone hour by default but can be customized tooother values using hello slider.</span></span>

<span data-ttu-id="8019c-116">**Upozornění na nastavení výchozí hodnota TTL souboru**: hello time-to-live oznámení nahrávání souboru předtím, než vyprší jejich platnost.</span><span class="sxs-lookup"><span data-stu-id="8019c-116">**File notification settings default TTL**: hello time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="8019c-117">Ve výchozím nastavení tooone den, ale může být vlastní tooother hodnoty pomocí posuvníku hello.</span><span class="sxs-lookup"><span data-stu-id="8019c-117">Set tooone day by default but can be customized tooother values using hello slider.</span></span>

<span data-ttu-id="8019c-118">**Soubor doručení maximální počet oznámení**: hello kolikrát hello toodeliver pokusy o IoT Hub oznámení nahrávání souboru.</span><span class="sxs-lookup"><span data-stu-id="8019c-118">**File notification maximum delivery count**: hello number of times hello IoT Hub attempts toodeliver a file upload notification.</span></span> <span data-ttu-id="8019c-119">Ve výchozím nastavení too10, ale může být vlastní tooother hodnoty pomocí posuvníku hello.</span><span class="sxs-lookup"><span data-stu-id="8019c-119">Set too10 by default but can be customized tooother values using hello slider.</span></span>

![Konfigurace služby IoT Hub nahrávání souborů hello portálu][15]

## <a name="next-steps"></a><span data-ttu-id="8019c-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8019c-121">Next steps</span></span>

<span data-ttu-id="8019c-122">Další informace o možnostech nahrávání souboru hello Centrum IoT najdete v tématu [nahrání souborů ze zařízení] [ lnk-upload] v hello Příručka vývojáře pro službu IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8019c-122">For more information about hello file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload] in hello IoT Hub developer guide.</span></span>

<span data-ttu-id="8019c-123">Použijte tyto odkazy toolearn informace o správě Azure IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8019c-123">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="8019c-124">[Hromadné spravovat zařízení IoT][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="8019c-124">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="8019c-125">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="8019c-125">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="8019c-126">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="8019c-126">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="8019c-127">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="8019c-127">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="8019c-128">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="8019c-128">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="8019c-129">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="8019c-129">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="8019c-130">[Zabezpečení řešení IoT z hello pozadí][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="8019c-130">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

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
