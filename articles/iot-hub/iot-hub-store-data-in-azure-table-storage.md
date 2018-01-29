---
title: "Uložení zpráv IoT hub do úložiště dat Azure | Microsoft Docs"
description: "IoT hub zprávy uložit do Azure blob storage pomocí služby IoT Hub směrování zpráv. IoT hub zprávy obsahují informace, například data snímačů, která je odeslána ze zařízení IoT."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "úložiště dat IOT, úložiště dat snímačů iot"
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/04/2017
ms.author: xshi
ms.openlocfilehash: 5419f0fb86f2a7b051ffc7fda17c74cf15178a6b
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>Uložit IoT hub zprávy, které obsahují data snímačů do Azure blob storage

![Diagram začátku do konce](media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte, jak vytvořit účet úložiště Azure a Azure funkce aplikace na ukládání IoT Centrum zpráv ve službě blob storage.

## <a name="what-you-do"></a>Co dělat

- Vytvoření účtu úložiště Azure
- Příprava služby IoT hub pro směrování zpráv do úložiště.

## <a name="what-you-need"></a>Co potřebujete

- [Nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) tak, aby pokrývalo následující požadavky:
  - Aktivní předplatné Azure
  - Služby IoT hub v rámci svého předplatného 
  - Spuštěné aplikace, která odesílá zprávy do služby IoT hub

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu úložiště Azure

1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **úložiště** > **účet úložiště**  >   **Vytvoření**.

2. Zadejte informace potřebné pro účet úložiště:

   ![Vytvoření účtu úložiště na portálu Azure](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Název**: název účtu úložiště. Název musí být globálně jedinečný.

   * **Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.

   * **Připnout na řídicí panel:** Vyberte tuto možnost pro snadný přístup k centru IoT z řídicího panelu.

3. Klikněte na možnost **Vytvořit**.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Příprava služby IoT hub pro směrování zpráv do úložiště

Směrování zpráv do úložiště Azure IoT Hub nativně podporuje jako objekty BLOB.

### <a name="add-storage-as-a-custom-endpoint"></a>Přidání úložiště jako vlastní koncový bod

Přejděte do služby IoT hub v portálu Azure. Klikněte na tlačítko **koncové body** > **přidat**. Název koncového bodu a vyberte **kontejneru úložiště Azure** jako typ koncového bodu. Pomocí nástroje pro výběr a vyberte účet úložiště, kterou jste vytvořili v předchozí části. Vytvořit kontejner úložiště a vyberte ho a pak klikněte na tlačítko **OK**.

  ![Vytvoření vlastní koncového bodu v IoT Hub](media\iot-hub-store-data-in-azure-table-storage\2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>Přidat do úložiště dat trasy pro trasy

Klikněte na tlačítko **trasy** > **přidat** a zadejte název pro tuto trasu. Vyberte **zprávy zařízení** jako zdroj dat a vyberte koncový bod úložiště, které jste právě vytvořili jako koncový bod v: trasa. Zadejte `true` jako řetězce dotazu, pak klikněte na tlačítko **Uložit**.

  ![Vytvořit trasu ve IoT Hub.](media\iot-hub-store-data-in-azure-table-storage\3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Přidat trasu pro aktivní trase telemetrie (volitelné)

Ve výchozím nastavení směruje IoT Hub všechny zprávy, které se neshodují s jiným trasám předdefinovaný koncový bod. Vzhledem k tomu, že všech telemetrických zpráv nyní shodují pravidlo, které směruje zprávy do úložiště, budete muset přidat další trasu pro zpráv, které mají být zapsána do předdefinovaný koncový bod. Není k dispozici bez dalších poplatků pro směrování zpráv do víc koncových bodů.

> [!NOTE]
> Pokud chcete další zpracování na telemetrické zprávy, můžete tento krok přeskočit.

Klikněte na tlačítko **přidat** z podokna trasy a zadejte název pro tuto trasu. Vyberte **zprávy zařízení** jako zdroj dat a **události** jako koncový bod. Zadejte `true` jako řetězce dotazu, pak klikněte na tlačítko **Uložit**.

  ![Vytvořit trasu horkou cesta IoT hub](media\iot-hub-store-data-in-azure-table-storage\4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Ověřit zprávu v vašeho kontejneru úložiště

1. Spuštění ukázkové aplikace na zařízení k odesílání zpráv do služby IoT hub.

2. [Stáhněte a nainstalujte Azure Storage Explorer](http://storageexplorer.com/).

3. Otevřete Storage Explorer, klikněte na **přidat účet Azure** > **přihlášení**a potom se přihlaste k účtu Azure.

4. Klikněte na předplatné Azure > **účty úložiště** > váš účet úložiště > **kontejnery objektů Blob** > vaše kontejneru.

   Měli byste vidět zprávy odeslané ze zařízení do služby IoT hub přihlášení kontejneru objektů blob.

## <a name="next-steps"></a>Další kroky

Úspěšně jste vytvořili účet úložiště Azure a směruje zprávy ze služby IoT Hub na kontejner objektů blob v daném účtu úložiště.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
