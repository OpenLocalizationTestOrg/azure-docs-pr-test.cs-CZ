---
title: aaaRegister dat z Data Lake Store v Azure Data Catalog | Microsoft Docs
description: Zaregistrovat v Azure Data Catalog dat z Data Lake Store
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3e895b42cab4ba39d288950763312a243883cbdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Zaregistrovat v Azure Data Catalog dat z Data Lake Store
V tomto článku se dozvíte, jak toointegrate Azure Data Lake s Azure Data Catalog toomake data uložena v rámci organizace zjistitelný integrací s katalogem Data Catalog. Další informace o vytváření katalogu dat najdete v tématu [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md). toounderstand scénáře, ve kterých se dá použít katalogu Data Catalog najdete v části [běžné scénáře Azure Data Catalog](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Aktivujte předplatné Azure** pro verzi Public Preview Data Lake Store. Viz [pokyny](data-lake-store-get-started-portal.md).
* **Účet Azure Data Lake Store**. Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md). V tomto kurzu, dejte nám vytvořit účet Data Lake Store s názvem **datacatalogstore**.

    Po vytvoření účtu hello nahrajte tooit ukázkové datové sady. V tomto kurzu, dejte nám odeslat všechny soubory .csv hello pod hello **AmbulanceData** složky v hello [úložiště Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Můžete používat různé klienty, jako třeba [Azure Storage Explorer](http://storageexplorer.com/), kontejner objektů blob tooa tooupload data.
* **Azure Data Catalog**. Vaše organizace již musí mít Azure Data Catalog pro vaši organizaci vytvořený. Pro každou organizaci, je povolen pouze jeden katalog.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Registrace Data Lake Store jako zdroj pro katalog Data Catalog

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Přejděte příliš`https://azure.microsoft.com/services/data-catalog`a klikněte na tlačítko **Začínáme**.
2. Přihlaste se k portálu Azure Data Catalog hello a klikněte na tlačítko **publikovat data**.

    ![Registrace zdroje dat](./media/data-lake-store-with-data-catalog/register-data-source.png "registrace zdroje dat")
3. Na další stránku hello, klikněte na tlačítko **spustit aplikaci**. To bude stažení souboru manifestu aplikace hello ve vašem počítači. Dvakrát klikněte na hello souboru manifestu toostart hello aplikace.
4. Na úvodní stránku hello, klikněte na tlačítko **přihlášení**a zadejte svá pověření.

    ![Úvodní obrazovka](./media/data-lake-store-with-data-catalog/welcome.screen.png "úvodní obrazovce")
5. Na hello vyberte na stránce zdroj dat, vyberte **Azure Data Lake**a potom klikněte na **Další**.

    ![Vyberte zdroj dat](./media/data-lake-store-with-data-catalog/select-source.png "vyberte zdroj dat")
6. Na další stránku hello zadejte název účtu Data Lake Store, které chcete tooregister v katalogu Data Catalog hello. Nechte hello jiné možnosti jako výchozí a pak klikněte na tlačítko **Connect**.

    ![Zdroj připojení toodata](./media/data-lake-store-with-data-catalog/connect-to-source.png "Connect toodata zdroje")
7. Další stránku Hello je možné rozdělit do hello následující segmenty.

    a. Hello **hierarchii serverů** pole představuje struktura složek účet Data Lake Store hello. **$Root** představuje hello kořenového účtu Data Lake Store, a **AmbulanceData** představuje hello složky vytvořené v kořenovém adresáři hello hello účtu Data Lake Store.

    b. Hello **dostupné objekty** pole zobrazuje hello souborů a složek v části hello **AmbulanceData** složky.

    c. **Registrované objekty toobe pole** seznamy hello soubory a složky, které chcete tooregister v Azure Data Catalog.

    ![Zobrazit datové struktury](./media/data-lake-store-with-data-catalog/view-data-structure.png "zobrazit datové struktury")
8. V tomto kurzu byste měli zaregistrovat všechny hello soubory v adresáři hello. Pro tento, klikněte na tlačítko hello (![přesun objektů](./media/data-lake-store-with-data-catalog/move-objects.png "přesun objektů")) toomove tlačítko všechny hello soubory příliš**toobe objekty zaregistrován** pole.

    Vzhledem k tomu hello data budou zaregistrovány ve katalog dat pro celou organizaci, je doporučená metoda tooadd některá metadata, který můžete později použít tooquickly umísťovat hello data. Například můžete přidat e-mailovou adresu pro vlastníka dat hello, (například jeden, který odesílá hello data) nebo přidání značka tooidentify hello data. Následující snímek obrazovky Hello ukazuje značku, že přidáme toohello data.

    ![Zobrazit datové struktury](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "zobrazit datové struktury")

    Klikněte na tlačítko **zaregistrovat**.
9. Hello následující snímek obrazovky označuje, že hello data je úspěšně zaregistrován v hello katalogu Data Catalog.

    ![Registrace je dokončena](./media/data-lake-store-with-data-catalog/registration-complete.png "zobrazit datové struktury")
10. Klikněte na tlačítko **zobrazit portál** toogo zpět toohello katalogu Data Catalog portál a ověřte, můžete teď přístup hello zaregistrován dat z portálu hello. toosearch hello data, můžete použít hello – značka, které jste použili při registraci hello data.

     ![Vyhledávání dat v katalogu](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "vyhledávání dat v katalogu")
11. Teď můžete dělat operace, jako je přidání poznámek a dokumentaci toohello data. Další informace najdete v tématu hello následující odkazy.

    * [Přidání poznámek ke zdrojům dat v katalogu Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
    * [Zdroje dat dokumentu v katalogu Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Viz také
* [Přidání poznámek ke zdrojům dat v katalogu Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
* [Zdroje dat dokumentu v katalogu Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)
* [Integrace s jinými službami Azure Data Lake Store](data-lake-store-integrate-with-other-services.md)
