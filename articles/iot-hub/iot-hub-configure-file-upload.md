---
title: "Pomocí portálu Azure ke konfiguraci nahrávání souborů | Microsoft Docs"
description: "Postup konfigurace služby IoT hub, chcete-li povolit nahrávání souborů z připojených zařízení pomocí portálu Azure. Obsahuje informace o konfiguraci cílového účtu úložiště Azure."
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
ms.openlocfilehash: 149dd84d7d8f4ff9cd30f9fc649ced3cb364cfb7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a><span data-ttu-id="8b46b-104">Konfigurace centra IoT nahrávání souborů pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8b46b-104">Configure IoT Hub file uploads using the Azure portal</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a><span data-ttu-id="8b46b-105">Nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="8b46b-105">File upload</span></span>

<span data-ttu-id="8b46b-106">Použít [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet služby Azure Storage pomocí centra.</span><span class="sxs-lookup"><span data-stu-id="8b46b-106">To use the [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your hub.</span></span> <span data-ttu-id="8b46b-107">Vyberte **nahrávání souborů** zobrazíte seznam vlastností nahrávání souboru pro službu IoT hub, která je upravována.</span><span class="sxs-lookup"><span data-stu-id="8b46b-107">Select **File upload** to display a list of file upload properties for the IoT hub that is being modified.</span></span>

![Zobrazení IoT Hub soubor nahrát nastavení na portálu][13]

<span data-ttu-id="8b46b-109">**Kontejner úložiště**: pomocí portálu Azure vyberte kontejner objektů blob v účtu Azure Storage v aktuálním předplatném Azure přidružit služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8b46b-109">**Storage container**: Use the Azure portal to select a blob container in an Azure Storage account in your current Azure subscription to associate with your IoT Hub.</span></span> <span data-ttu-id="8b46b-110">Pokud potřeby můžete vytvořit účet úložiště Azure na **účty úložiště** okno a objektů blob kontejneru na **kontejnery** okno.</span><span class="sxs-lookup"><span data-stu-id="8b46b-110">If necessary, you can create an Azure Storage account on the **Storage accounts** blade and blob container on the **Containers** blade.</span></span> <span data-ttu-id="8b46b-111">IoT Hub automaticky vygeneruje identifikátory URI SAS s oprávněními k zápisu do tohoto kontejneru objektů blob pro zařízení se má použít při odesílají soubory.</span><span class="sxs-lookup"><span data-stu-id="8b46b-111">IoT Hub automatically generates SAS URIs with write permissions to this blob container for devices to use when they upload files.</span></span>

![Zobrazit kontejnery úložiště pro nahrávání souborů na portálu][14]

<span data-ttu-id="8b46b-113">**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru přes přepínač.</span><span class="sxs-lookup"><span data-stu-id="8b46b-113">**Receive notifications for uploaded files**: Enable or disable file upload notifications via the toggle.</span></span>

<span data-ttu-id="8b46b-114">**Hodnota TTL SAS**: Toto nastavení je time-to-live identifikátorů URI SAS, vrácený do zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8b46b-114">**SAS TTL**: This setting is the time-to-live of the SAS URIs returned to the device by IoT Hub.</span></span> <span data-ttu-id="8b46b-115">Ve výchozím nastavení má jednu hodinu, ale můžete přizpůsobit tak, aby ostatní hodnoty pomocí posuvníku.</span><span class="sxs-lookup"><span data-stu-id="8b46b-115">Set to one hour by default but can be customized to other values using the slider.</span></span>

<span data-ttu-id="8b46b-116">**Upozornění na nastavení výchozí hodnota TTL souboru**: time-to-live souboru odeslat oznámení, než vyprší jejich platnost.</span><span class="sxs-lookup"><span data-stu-id="8b46b-116">**File notification settings default TTL**: The time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="8b46b-117">Ve výchozím nastavení má jeden den, ale můžete přizpůsobit tak, aby ostatní hodnoty pomocí posuvníku.</span><span class="sxs-lookup"><span data-stu-id="8b46b-117">Set to one day by default but can be customized to other values using the slider.</span></span>

<span data-ttu-id="8b46b-118">**Soubor doručení maximální počet oznámení**: počet, kolikrát se Centrum IoT pokusí doručit soubor odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="8b46b-118">**File notification maximum delivery count**: The number of times the IoT Hub attempts to deliver a file upload notification.</span></span> <span data-ttu-id="8b46b-119">Ve výchozím nastavení má 10, ale můžete přizpůsobit tak, aby ostatní hodnoty pomocí posuvníku.</span><span class="sxs-lookup"><span data-stu-id="8b46b-119">Set to 10 by default but can be customized to other values using the slider.</span></span>

![Konfigurace služby IoT Hub nahrávání souborů na portálu][15]

## <a name="next-steps"></a><span data-ttu-id="8b46b-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8b46b-121">Next steps</span></span>

<span data-ttu-id="8b46b-122">Další informace o možnostech nahrávání souboru Centrum IoT najdete v tématu [nahrání souborů ze zařízení] [ lnk-upload] v příručce pro vývojáře IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8b46b-122">For more information about the file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload] in the IoT Hub developer guide.</span></span>

<span data-ttu-id="8b46b-123">Další informace o správě Azure IoT Hub na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="8b46b-123">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="8b46b-124">[Hromadné spravovat zařízení IoT][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="8b46b-124">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="8b46b-125">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="8b46b-125">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="8b46b-126">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="8b46b-126">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="8b46b-127">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="8b46b-127">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="8b46b-128">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="8b46b-128">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="8b46b-129">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="8b46b-129">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="8b46b-130">[Zabezpečení řešení IoT od základů nahoru][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="8b46b-130">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

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
