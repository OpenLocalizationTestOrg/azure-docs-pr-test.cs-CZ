---
title: "aaaIntegrate účet úložiště Azure s Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure sítě pro doručování obsahu (CDN) toodeliver širokopásmového obsahu ukládání do mezipaměti objektů blob z úložiště Azure."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e44716969d6a784265cc4b1da34f0d021a17b38d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Účet úložiště Azure integrovat Azure CDN
CDN může být povoleno toocache obsahu z úložiště Azure. Nabízí vývojářům globální řešení pro doručování širokopásmového obsahu pomocí ukládání do mezipaměti objektů BLOB a statického obsahu výpočetní instance na fyzických uzlech hello USA, Evropa, Asie, Austrálie a Jižní Ameriky.

## <a name="step-1-create-a-storage-account"></a>Krok 1: Vytvoření účtu úložiště
Použijte následující postup toocreate nový účet úložiště pro předplatné Azure hello. Účet úložiště poskytuje přístup ke službám úložiště Azure. účet úložiště Hello představuje nejvyšší úroveň hello hello oboru názvů pro přístup k jednotlivých součástí služby úložiště Azure hello: objektu Blob služby, služba fronty a tabulky služby. Další informace najdete v části toohello [tooMicrosoft Úvod Azure Storage](../storage/common/storage-introduction.md).

toocreate účet úložiště, musí být buď správce služby hello správce nebo spolusprávce pro hello přidružené předplatné.

> [!NOTE]
> Existuje několik metod, které můžete použít toocreate účet úložiště, včetně hello portálu Azure a prostředí Powershell.  V tomto kurzu budeme používat hello portálu Azure.  
> 
> 

**toocreate účet úložiště pro předplatné Azure**

1. Přihlaste se toohello [portálu Azure](https://portal.azure.com).
2. V hello levém horním rohu, vyberte **nový**. V hello **nový** vyberte **Data + úložiště**, pak klikněte na tlačítko **účet úložiště**.
    
    Hello **vytvořit účet úložiště** otevře se okno.   

    ![Vytvořit účet úložiště][create-new-storage-account]  

3. V hello **název** pole, zadejte název subdomény. Tato položka může obsahovat 3 až 24 malých písmen a číslic.
   
    Tato hodnota se změní na název hostitele hello v rámci hello identifikátor URI, který se používá k adresování objektů Blob, fronty nebo tabulky prostředky pro předplatné hello. Chcete-li vyřešit kontejner prostředku v hello služby objektů Blob, použijte identifikátor URI v hello formátu, kde  *&lt;StorageAccountLabel&gt;*  odkazuje toohello hodnoty, které jste zadali v **zadejte adresu URL**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;můj_kontejner&gt;*
   
    **Důležité:** hello adresy URL popisku forms hello subdomény účtu úložiště hello URI a musí být jedinečný mezi všechny hostované služby v Azure.
   
    Tato hodnota se používá jako hello název tohoto účtu úložiště hello portálu nebo při přístupu k tomuto účtu prostřednictvím kódu programu.
4. Ponechte výchozí hodnoty hello **modelu nasazení**, **účet druhu**, **výkonu**, a **replikace**. 
5. Vyberte hello **předplatné** že hello účet úložiště se použije s.
6. Vyberte nebo vytvořte **skupinu prostředků**.  Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Vyberte umístění vašeho účtu úložiště.
8. Klikněte na možnost **Vytvořit**. Vytvoření účtu úložiště hello Hello proces může trvat několik minut toocomplete.

## <a name="step-2-enable-cdn-for-hello-storage-account"></a>Krok 2: Povolení CDN pro účet úložiště hello

Díky integraci nejnovější hello můžete teď povolit CDN pro váš účet úložiště bez opuštění portálu rozšíření úložiště. 

1. Vyberte účet úložiště hello, hledání "CDN" nebo se posouvají dolů z hello levé navigační nabídku a pak klikněte na "Azure CDN".
    
    Hello **Azure CDN** otevře se okno.

    ![povolení navigace CDN][cdn-enable-navigation]
    
2. Vytvořte nový koncový bod zadáním hello požadované informace
    - **Profil CDN**: můžete vytvořit novou nebo použít stávající profil.
    - **Cenová úroveň**: stačí tooselect cenovou úroveň, pokud vytvoříte nový profil CDN.
    - **Název koncového bodu CDN**: Zadejte název koncového bodu podle svého výběru.

    > [!TIP]
    > koncový bod CDN Hello vytvořili používá název hostitele hello svého účtu úložiště jako původní ve výchozím nastavení.

    ! [cdn nové vytvoření koncového bodu] [cdn--koncový bod vytvoření nové]

3. Po vytvoření hello nový koncový bod se zobrazí v seznamu koncový bod hello výše.

    ![Nový koncový bod CDN úložiště][cdn-storage-new-endpoint]

> [!NOTE]
> Můžete také přejít tooAzure CDN rozšíření tooenable CDN. [Kurzu](#Tutorial-cdn-create-profile).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a>Krok 3: Povolení dalších funkcí CDN

V okně "Azure CDN" účet úložiště klikněte na koncový bod CDN hello z okno Konfigurace hello seznamu tooopen CDN. Můžete povolit další funkce CDN pro doručení, jako je například kompresi, řetězce dotazu, geograficky filtrování. Můžete také přidat vlastní doménu koncový bod CDN tooyour mapování a povolit HTTPS pro vlastní doménu.
    
![Konfigurace cdn úložiště CDN][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a>Krok 4: Přístup k obsahu CDN
tooaccess v mezipaměti obsahu na hello CDN, použijte hello CDN adresy URL uvedené v hello portálu. Hello adresy pro v mezipaměti objektů blob budou podobné toohello následující:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [!NOTE]
> Jakmile povolíte účet úložiště tooa CDN přístup, jsou způsobilé pro ukládání do mezipaměti CDN edge všechny veřejně dostupné objekty. Můžete upravit objekt, který je aktuálně uložené do mezipaměti v hello CDN, nový obsah hello nebudete mít k dispozici prostřednictvím hello CDN, dokud hello CDN aktualizuje jeho obsah, když vyprší platnost hello uložené v mezipaměti obsahu time to live období.
> 
> 

## <a name="step-5-remove-content-from-hello-cdn"></a>Krok 5: Odeberte obsah z CDN hello
Nechcete-li již toocache objekt v hello Azure Content Delivery Network (CDN), můžete provést jednu z následujících kroků hello:

* Hello můžete nastavit kontejner privátní místo veřejné. V tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](../storage/blobs/storage-manage-access-to-resources.md) Další informace.
* Můžete zakázat nebo odstranit koncový bod CDN hello pomocí hello portálu pro správu.
* Můžete upravit vaší hostované služby toono delší reakce toorequests hello objektu.

Objekt již uložené v mezipaměti v hello CDN zůstane v mezipaměti, dokud neuplyne období time to live hello hello objektu, nebo dokud se vyprazdňují hello koncový bod. Když hello time to live období vyprší, bude hello CDN toosee zkontrolujte, jestli koncový bod CDN hello je stále platný a hello objekt anonymně přístupné. Pokud není, pak hello objektu bude už do mezipaměti.

## <a name="additional-resources"></a>Další zdroje
* [Jak tooMap obsahu CDN tooa vlastní domény](cdn-map-content-to-custom-domain.md)
* [Povolit HTTPS pro vaši vlastní doménu.](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
