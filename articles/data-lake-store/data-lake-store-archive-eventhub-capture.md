---
title: "aaaCapture data ze služby Event Hubs do Azure Data Lake Store | Microsoft Docs"
description: "Použití Azure Data Lake Store toocapture data ze služby Event Hubs"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 09b17bd0b47043bd2c83dba72c01a8064f206a0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-store-toocapture-data-from-event-hubs"></a>Použití Azure Data Lake Store toocapture data ze služby Event Hubs

Zjistěte, jak toouse Azure Data Lake Store toocapture dat přijatých Azure Event Hubs.

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Účet Azure Data Lake Store**. Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md).

*  **Obor názvů Event Hubs**. Pokyny najdete v tématu [vytvoření oboru názvů Event Hubs](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Zkontrolujte, zda jsou v hello hello účtu Data Lake Store a obor názvů služby Event Hubs hello stejného předplatného Azure.


## <a name="assign-permissions-tooevent-hubs"></a>Přiřazení oprávnění tooEvent rozbočovače

V této části vytvoříte v rámci účtu hello do složky, kam chcete toocapture hello data ze služby Event Hubs. Můžete také přiřadit oprávnění tooEvent centra tak, aby ho můžete zapsat data do účtu Data Lake Store. 

1. Otevřete účet Data Lake Store hello, kam chcete toocapture data ze služby Event Hubs a potom klikněte na **Průzkumníku dat**.

    ![Data Lake Store Průzkumníku dat](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Průzkumníku dat v Data Lake Store")

2.  Klikněte na tlačítko **novou složku** a pak zadejte název složky, kam chcete toocapture hello data.

    ![Vytvořte novou složku v Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "vytvořte novou složku v Data Lake Store")

3. Přiřadíte oprávnění v kořenové hello hello Data Lake Store. 

    a. Klikněte na tlačítko **Průzkumníku dat**, vyberte hello kořenovém hello účtu Data Lake Store a pak klikněte na tlačítko **přístup**.

    ![Přiřazení oprávnění pro Data Lake Store kořenové](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "přiřadit oprávnění pro kořenový adresář Data Lake Store")

    b. V části **přístup**, klikněte na tlačítko **přidat**, klikněte na tlačítko **vybrat uživatele nebo skupinu**a poté vyhledejte `Microsoft.EventHubs`. 

    ![Přiřazení oprávnění pro Data Lake Store kořenové](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "přiřadit oprávnění pro kořenový adresář Data Lake Store")
    
    Klikněte na **Vybrat**.

    c. V části **přiřadit oprávnění**, klikněte na tlačítko **vyberte oprávnění**. Nastavit **oprávnění** příliš**Execute**. Nastavit **přidat do** příliš**tuto složku a všechny podřízené objekty**. Nastavit **přidat jako** příliš**položka oprávnění k přístupu a výchozí položku oprávnění**.

    ![Přiřazení oprávnění pro Data Lake Store kořenové](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "přiřadit oprávnění pro kořenový adresář Data Lake Store")

    Klikněte na **OK**.

4. Přiřazení oprávnění pro složku hello v rámci účtu Data Lake Store, které chcete toocapture data.

    a. Klikněte na tlačítko **Průzkumníku dat**, vyberte složku hello v hello účtu Data Lake Store a pak klikněte na tlačítko **přístup**.

    ![Přiřazení oprávnění pro Data Lake Store složku](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "přiřadit oprávnění pro složku Data Lake Store")

    b. V části **přístup**, klikněte na tlačítko **přidat**, klikněte na tlačítko **vybrat uživatele nebo skupinu**a poté vyhledejte `Microsoft.EventHubs`. 

    ![Přiřazení oprávnění pro Data Lake Store složku](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "přiřadit oprávnění pro složku Data Lake Store")
    
    Klikněte na **Vybrat**.

    c. V části **přiřadit oprávnění**, klikněte na tlačítko **vyberte oprávnění**. Nastavit **oprávnění** příliš**číst, zapisovat,** a **Execute**. Nastavit **přidat do** příliš**tuto složku a všechny podřízené objekty**. Nakonec nastavte **přidat jako** příliš**položka oprávnění k přístupu a výchozí položku oprávnění**.

    ![Přiřazení oprávnění pro Data Lake Store složku](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "přiřadit oprávnění pro složku Data Lake Store")
    
    Klikněte na **OK**. 

## <a name="configure-event-hubs-toocapture-data-toodata-lake-store"></a>Konfigurace služby Event Hubs toocapture data tooData Lake Store

V této části vytvoříte Centrum událostí v rámci oboru názvů Event Hubs. Můžete také nakonfigurovat hello centra událostí toocapture data tooan účtu Azure Data Lake Store. V této části se předpokládá, že jste již vytvořili na obor názvů služby Event Hubs.

2. Z hello **přehled** podokně hello Event Hubs oboru názvů, klikněte na tlačítko **+ centra událostí**.

    ![Vytvoření centra událostí](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "vytvoření centra událostí")

3. Poskytují následující hello hodnoty tooconfigure Event Hubs toocapture data tooData Lake Store.

    ![Vytvoření centra událostí](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "vytvoření centra událostí")

    a. Zadejte název pro hello centra událostí.
    
    b. V tomto kurzu nastavit **počet oddílů** a **uchování zpráv** toohello výchozí hodnoty.
    
    c. Nastavit **zaznamenat** příliš**na**. Sada hello **časové okno** (jak často toocapture) a **velikost okna** (toocapture velikost dat). 
    
    d. Pro **zaznamenání zprostředkovatele**, vyberte **Azure Data Lake Store** a hello vyberte hello Data Lake Store jste vytvořili dříve. Pro **Data Lake cesta**, zadejte název hello hello složky, které jste vytvořili v hello účtu Data Lake Store. Potřebujete jenom tooprovide hello relativní cesta toohello složky.

    e. Nechte hello **ukázka zachycení formátů název** toohello výchozí hodnotu. Tato možnost řídí hello struktura složek, který se vytvoří ve složce zachycení hello.

    f. Klikněte na možnost **Vytvořit**.

## <a name="test-hello-setup"></a>Nastavení testu hello

Nyní můžete otestovat hello řešení odesláním toohello datového centra událostí Azure. Postupujte podle pokynů hello [odesílat události tooAzure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Po spuštění odeslání dat hello zobrazí hello dat v Data Lake Store pomocí hello struktura složek, které zadáte. Například struktura složek, uvidíte, jak je znázorněno v následujícím snímku obrazovky v Data Lake Store hello.

![Ukázka dat pro EventHub v Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "EventHub ukázkových dat v Data Lake Store")

> [!NOTE]
> I když nemáte zprávy přicházející do centra událostí, Event Hubs prázdné soubory s právě hello hlavičky zapisuje do hello účtu Data Lake Store. Hello soubory jsou zapsány v hello stejné časový interval, který jste zadali při vytváření centra událostí hello.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>Analýza dat v Data Lake Store

Jakmile hello dat v Data Lake Store, můžete spustit analytické úlohy tooprocess a zpracujte data hello. V tématu [USQL Avro příklad](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) o toodo tento pomocí Azure Data Lake Analytics.
  

## <a name="see-also"></a>Viz také
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
