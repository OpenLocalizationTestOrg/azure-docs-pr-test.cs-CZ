---
title: "aaaUse Azure portálu tooget začít s Data Lake Store | Microsoft Docs"
description: "Použít hello Azure toocreate portálu účtu Data Lake Store a provádění základních operací v Data Lake Store hello"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: fea324d0-ad1a-4150-81f0-8682ddb4591c
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/06/2017
ms.author: nitinme
ms.openlocfilehash: 6bb3413f00bfa4393f08aed18bc1d5f8a2f28fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-hello-azure-portal"></a>Začínáme s Azure Data Lake Store pomocí portálu Azure hello
> [!div class="op_single_selector"]
> * [Azure Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Zjistěte, jak toouse hello Azure toocreate portálu účtu Azure Data Lake Store a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace najdete v tématu [Přehled služby Azure Data Lake Store](data-lake-store-overview.md).

Hello následující dvě videa obsahují hello stejné informace, jak je popsáno v tomto článku:

* [Vytvoření účtu Data Lake Store](https://mix.office.com/watch/1k1cycy4l4gen)
* [Správa dat v Data Lake Store pomocí Průzkumníku dat hello](https://mix.office.com/watch/icletrxrh6pc)

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít hello následující položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-an-azure-data-lake-store-account"></a>Vytvoření účtu Azure Data Lake Store

1. Přihlášení toohello nové [portál Azure](https://portal.azure.com).
2. Klikněte na položku **NOVÝ**, **Data + úložiště** a potom **Azure Data Lake Store**. Číst informace o hello v hello **Azure Data Lake Store** okna a pak klikněte na tlačítko **vytvořit** v hello levém dolním rohu okna hello.
3. V hello **nová služba Data Lake Store** okno, zadejte hodnoty hello, jak ukazuje následující snímek obrazovky hello:
   
    ![Vytvoření nového účtu Azure Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Vytvoření nového účtu Azure Data Lake Store")
   
   * **Název**. Zadejte jedinečný název pro hello účtu Data Lake Store.
   * **Předplatné**. Vyberte předplatné hello, pod kterým chcete toocreate nový účet Data Lake Store.
   * **Skupina prostředků**. Vyberte existující skupinu prostředků nebo hello **vytvořit nový** možnost toocreate jeden. Skupina prostředků je kontejner, který obsahuje související prostředky pro aplikaci. Další informace najdete v tématu [Skupiny prostředků v Azure](../azure-resource-manager/resource-group-overview.md#resource-groups).
   * **Umístění**: Vyberte umístění, kam chcete účtu Data Lake Store toocreate hello.
   * **Nastavení šifrování**. Existují tři možnosti:
     
     * **Nepovolovat šifrování**.
     * **Používat klíče spravované službou Azure Data Lake**.  Pokud chcete Azure Data Lake Store toomanage šifrovacích klíčů.
     * **Zvolit klíče z Azure Key Vault**. Můžete vybrat existující službu Azure Key Vault nebo vytvořit novou. toouse hello klíče z trezoru klíč, je nutné přiřadit oprávnění pro účet Azure Data Lake Store, tooaccess hello Azure Key Vault hello. Hello pokyny najdete v tématu [přiřadit oprávnění tooAzure Key Vault](#assign-permissions-to-azure-key-vault).
       
        ![Šifrování Data Lake Storu](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Šifrování Data Lake Storu")
       
        Klikněte na tlačítko **OK** v hello **nastavení šifrování** okno.

        Další informace najdete v tématu [Šifrování dat v Azure Data Lake Store](./data-lake-store-encryption.md).

4. Klikněte na možnost **Vytvořit**. Pokud jste zvolili toopin hello účet toohello řídicí panel, budete přesměrováni zpět toohello řídicí panel a zobrazí se průběh hello zřizování účtu Data Lake Store. Jednou je zajištěna hello účtu Data Lake Store, se zobrazí okno účtu hello.

Účet Data Lake Store můžete vytvořit také pomocí šablon Azure Resource Manageru. Tyto šablony jsou dostupné na stránce [Šablony rychlého startu Azure](https://azure.microsoft.com/resources/templates/?term=data+lake+store):

- Bez šifrování dat: [Nasazení účtu Azure Data Lake Store bez šifrování dat](https://azure.microsoft.com/en-us/resources/templates/101-data-lake-store-no-encryption/).
- S šifrováním dat pomocí služby Data Lake Store: [Nasazení účtu Data Lake Store s šifrováním (Data Lake)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/).
- S šifrováním dat pomocí služby Azure Key Vault: [Nasazení účtu Data Lake Store s šifrováním (Key Vault)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/).

### <a name="assign-permissions-to-azure-key-vault"></a>Přiřazení oprávnění tooAzure Key Vault
Pokud jste použili klíče z Azure Key Vault tooconfigure šifrování na hello účtu Data Lake Store, musíte nakonfigurovat přístup mezi hello účtu Data Lake Store a účet Azure Key Vault hello. Proveďte následující kroky toodo tak hello.

1. Pokud jste použili klíče z hello Azure Key Vault, hello okně hello účtu Data Lake Store zobrazí upozornění v horní části hello. Klikněte na tlačítko hello upozornění tooopen hello **nakonfigurovat oprávnění klíč trezoru** okno.
   
    ![Šifrování Data Lake Storu](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Šifrování Data Lake Storu")
2. okno Hello zobrazí dvě možnosti tooconfigure přístup.
   
   * V první možnosti hello, klikněte na tlačítko **udělit oprávnění** tooconfigure přístup. první možnost Hello je povolená jenom v případě, že hello uživatel, který vytvořili účet Data Lake Store hello je také správce pro hello Azure Key Vault.
   * Hello Další možností je rutiny prostředí PowerShell hello toorun zobrazí v okně hello. Je třeba vlastník hello toobe hello Azure Key Vault nebo mít hello možnost toogrant oprávnění na hello Azure Key Vault. Po spuštění rutiny hello vraťte toohello okně a klikněte na tlačítko **povolit** tooconfigure přístup.

## <a name="createfolder"></a>Vytváření složek v účtu služby Azure Data Lake Store
Můžete vytvořit složky pod vaší toomanage účet Data Lake Store a ukládat data.

1. Otevřete účet Data Lake Store hello, kterou jste vytvořili. V levém podokně hello, klikněte na **Procházet**, klikněte na tlačítko **Data Lake Store**a potom v okně Data Lake Store hello, klikněte na název účtu hello, pod kterým chcete toocreate složek. Pokud jste připnuli hello účet toohello úvodní panel, klikněte na dlaždici tohoto účtu.
2. V okně účtu Data Lake Store klikněte na možnost **Průzkumník dat**.
   
    ![Vytváření složek v účtu Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Vytváření složek v účtu Data Lake Store")
3. V okně účtu Data Lake Store, klikněte na tlačítko **novou složku**, zadejte název nové složky hello a pak klikněte na tlačítko **OK**.
   
    ![Vytváření složek v účtu Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Vytváření složek v účtu Data Lake Store")
   
    nově vytvořený Hello složky, je uvedena ve hello **Průzkumníku dat** okno. Můžete vytvářet vnořené složky do libovolné úrovně.
   
    ![Vytváření složek v účtu Data Lake Store](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Vytváření složek v účtu Data Lake Store")

## <a name="uploaddata"></a>Nahrání dat účtu Data Lake Store tooAzure
Můžete nahrát vaše data tooan účtu Azure Data Lake Store přímo na hello kořenové úrovni nebo tooa složky, která jste vytvořili v rámci účtu hello. V hello následující snímek obrazovky, postupujte podle hello kroky tooupload podsložku tooa souboru z hello **Průzkumníku dat** okno. Na tomto snímku obrazovky je soubor hello nahrané tooa podsložky ukazuje hello popisu cesty (označená červeným rámečkem).

Pokud hledáte některé tooupload ukázková data, můžete získat hello **Ambulance Data** složky z hello [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Nahrání dat](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Nahrání dat")

## <a name="properties"></a>Data uložena vlastností a akcí, které jsou k dispozici na hello
Klikněte na tlačítko hello nově přidaný soubor tooopen hello **vlastnosti** okno. Hello vlastnosti přidružené k souboru hello a hello akce, které můžete provádět v souboru hello jsou k dispozici v tomto okně. Můžete také zkopírovat úplnou cestu toofile hello v účtu Azure Data Lake Store, zvýrazněných v hello red pole v hello následující snímek obrazovky:

![Vlastnosti dat hello](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "vlastnosti hello dat")

* Klikněte na tlačítko **Preview** toosee náhled souboru hello přímo z prohlížeče hello. Můžete určit formát hello hello Preview také. Klikněte na tlačítko **Preview**, klikněte na tlačítko **formátu** v hello **náhled souboru** okno a v hello **formát náhledu souboru** zadejte hello možnosti, například jako počet toodisplay řádků, kódování toouse toouse oddělovač, atd.
  
  ![Formát náhledu souboru](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Formát náhledu souboru")
* Klikněte na tlačítko **Stáhnout** toodownload hello souboru tooyour počítače.
* Klikněte na tlačítko **přejmenovat soubor** toorename hello souboru.
* Klikněte na tlačítko **odstranit soubor** toodelete hello souboru.

## <a name="secure-your-data"></a>Zabezpečení dat
Můžete zabezpečit hello data uložená v účtu Azure Data Lake Store pomocí Azure Active Directory a řízení přístupu (ACL). Návod, jak toodo, který najdete v části [zabezpečení dat v Azure Data Lake Store](data-lake-store-secure-data.md).

## <a name="delete-azure-data-lake-store-account"></a>Odstranění účtu Azure Data Lake Store
Klikněte na účet Azure Data Lake Store, z okně Data Lake Store, toodelete **odstranit**. tooconfirm hello akce, budete výzvami tooenter hello název hello účet, který že chcete toodelete. Zadejte název hello hello účtu a pak klikněte na tlačítko **odstranit**.

![Odstranění účtu Data Lake](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Odstranění účtu Data Lake")

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Zobrazení protokolů diagnostiky pro Data Lake Store](data-lake-store-diagnostic-logs.md)

