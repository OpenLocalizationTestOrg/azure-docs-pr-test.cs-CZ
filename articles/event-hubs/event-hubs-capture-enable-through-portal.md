---
title: "Povolit aaaAzure zaznamenat centra událostí prostřednictvím portálu | Microsoft Docs"
description: "Povolte funkci hello zaznamenat centra událostí pomocí hello portálu Azure."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 27c7528552c497a4d98873a22bd56a991c66247c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-event-hubs-capture-using-hello-azure-portal"></a>Povolit pomocí portálu Azure hello Capture centra událostí

Zaznamenat lze nakonfigurovat v hello událostí okamžiku vytvoření centra pomocí hello [portál Azure](https://portal.azure.com). Můžete buď zachycení hello data tooan Azure [úložiště objektů Blob](https://azure.microsoft.com/services/storage/blobs/) kontejner nebo tooan [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) účtu.

## <a name="capture-data-tooan-azure-storage-account"></a>Zaznamenat účet Azure Storage tooan dat  

Při vytváření centra událostí můžete povolit zachycení kliknutím hello **na** tlačítka na hello **vytvoření centra událostí** okno portálu. Potom zadejte účet úložiště a kontejneru kliknutím **Azure Storage** v hello **zaznamenání zprostředkovatele** pole. Vzhledem k zachycení událostí centra využívá service-to-service ověřování s úložištěm, není nutné toospecify připojovací řetězec úložiště. Výběr prostředku Hello automaticky vybere hello URI pro váš účet úložiště. Pokud používáte Azure Resource Manager, musíte tento identifikátor URI explicitně zadat jako řetězec.

Hello výchozí časový interval je 5 minut. Hello minimální hodnota je 1, hello maximální 15. Hello **velikost** okno má rozsah 10 500 MB.

![][1]

## <a name="capture-data-tooan-azure-data-lake-store-account"></a>Zaznamenat účet Azure Data Lake Store tooan dat

toocapture data tooAzure Data Lake Store, vytvoření účtu Data Lake Store a centra událostí:

### <a name="create-an-azure-data-lake-store-account-and-folders"></a>Vytvoření účtu Azure Data Lake Store a složek

1. Vytvoření účtu Data Lake Store, pokynů hello v [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](../data-lake-store/data-lake-store-get-started-portal.md). 
2. Vytvořte složku pod tímto účtem, pokynů hello v hello [vytváření složek v účtu Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) části.
3. V okně účtu Data Lake Store hello, klikněte na tlačítko **Průzkumníku dat**.
4. Klikněte na **Přístup**.
5. Klikněte na tlačítko **Přidat**.
6. V hello **vyhledávání podle názvu nebo e-mailu** zadejte **Microsoft.EventHubs** a pak vyberte tuto možnost. 
7. Hello **oprávnění** se zobrazí karta. Nastavte oprávnění hello, jak ukazuje následující obrázek hello:

    ![][6]

8. Klikněte na **OK**.
9. Nyní vytvořte složku v kořenové složce hello toohello cílové složce procházení a kliknutím na název složky hello.
10. Klikněte na **Přístup**.
11. Klikněte na tlačítko **Přidat**.
12. V hello **vyhledávání podle názvu nebo e-mailu** zadejte **Microsoft.EventHubs** a pak vyberte tuto možnost.
13. Hello **oprávnění** karta se znovu zobrazí. Nastavte oprávnění hello, jak ukazuje následující obrázek hello:

    ![][5]

### <a name="create-an-event-hub"></a>Vytvoření centra událostí

1. Všimněte si, že Centrum událostí hello musí být v hello stejné předplatné jako hello Azure Data Lake Store, kterou jste právě vytvořili. Centra událostí vytvořit hello kliknutím hello **na** tlačítko pod **zaznamenat** v hello **vytvoření centra událostí** okno portálu. 
2. V hello **vytvoření centra událostí** okno portálu, vyberte **Azure Data Lake Store** z hello **zaznamenání zprostředkovatele** pole.
3. V **vyberte Data Lake Store**, zadejte hello účtu Data Lake Store, které jste vytvořili dříve a v hello **Data Lake cesta** zadejte hello cesta toohello data složku jste vytvořili.

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>Přidání nebo konfigurace funkce Capture v existujícím centru událostí

Funkci Capture můžete nakonfigurovat v existujících centrech událostí, která jsou v oborech názvů služby Event Hubs. tooenable zaznamenat na existující centra událostí nebo toochange zachycení nastavení, klikněte na tlačítko hello obor názvů tooload hello **Essentials** okno, pak klikněte na tlačítko hello centra událostí, pro kterou chcete tooenable nebo změnit nastavení zachycení hello. Nakonec klikněte na hello **vlastnosti** části hello otevřete okno a potom upravte nastavení zachycení hello, jak je znázorněno v hello následující údaje:

### <a name="azure-blob-storage"></a>Azure Blob Storage

![][2]

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
[5]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture5.png
[6]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture6.png

## <a name="next-steps"></a>Další kroky

Ke konfiguraci funkce Event Hubs Capture můžete také použít šablony Azure Resource Manageru. Další informace najdete v tématu věnovaném [povolení funkce Capture pomocí šablony Azure Resource Manageru](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).
