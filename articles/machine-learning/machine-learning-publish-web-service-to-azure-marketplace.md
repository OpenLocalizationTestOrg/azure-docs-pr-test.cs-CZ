---
title: "AAA(Deprecated) publikovat strojového učení webové služby tooAzure Marketplace | Microsoft Docs"
description: "(nepoužívané) Jak toopublish vaše webová služba Azure Machine Learning toohello Azure Marketplace"
services: machine-learning
documentationcenter: 
author: BharathS
manager: jhubbard
editor: cgronlun
ms.assetid: 68e908be-3a99-4cd7-9517-e2b5f2f341b8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: machine-learning-gallery-experiments
redirect_document_id: True
ms.openlocfilehash: 149abc3df9b79c1b37d233d5e85e803592ff1020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-toohello-azure-marketplace"></a>(nepoužívané) Publikovat webovou službu Azure Machine Learning toohello Azure Marketplace

> [!NOTE]
> DataMarket a Data služby se postupně vyřazuje z provozu a odběry vyřadí a zrušena od 3/31/2017. V důsledku toho je zastaralá v tomto článku. 
> 
> Jako alternativu, můžete publikovat vaše Machine Learning experimenty toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello Benefit hello datové vědy komunity. Další informace najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).

Hello Azure Marketplace nabízí možnost hello webové služby Azure Machine Learning toopublish jako placené nebo bezplatné služby pro spotřeba externí zákazníky. Tento článek obsahuje přehled procesu s tooget tooguidelines odkazy, které jste spustili. Pomocí tohoto procesu můžete nastavit webové služby, které jsou k dispozici pro ostatní vývojáři tooconsume ve svých aplikacích.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-hello-publishing-process"></a>Přehled procesu publikování hello
Hello následují hello kroky pro publikování služby tooAzure web Azure Machine Learning Marketplace:

1. Vytvoření a publikování služby Machine Learning požadavků a odpovědí (záznamy RR)
2. Nasaďte tooproduction hello služby a získat informace o koncovém klíč rozhraní API a OData hello.
3. Použití hello URL hello publikované webové služby toopublish příliš[Azure Marketplace (trhu dat)](https://publish.windowsazure.com/workspace/) 
4. Po odeslání vaši nabídku zkontrolovány a musí toobe schválení před vašim zákazníkům začít zakoupení ho. proces publikování Hello může trvat několik pracovních dnů. 

## <a name="walk-through"></a>Názorný postup
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Krok 1: Vytvoření a publikování služby Machine Learning požadavků a odpovědí (záznamy RR)
 Pokud jste to ještě neudělali, proveďte prosím podívejte se na to [provede](machine-learning-walkthrough-5-publish-web-service.md).

### <a name="step-2-deploy-hello-service-tooproduction-and-obtain-hello-api-key-and-odata-endpoint-information"></a>Krok 2: Nasazení tooproduction hello služby a získat informace o koncovém klíč rozhraní API a OData hello
1. Z hello [portálu Azure Classic](http://manage.windowsazure.com), vyberte hello **MACHINE LEARNING** možnost z hello levém navigačním panelu a vyberte pracovní prostor. 
2. Klikněte na hello **webové služby** kartě a webové služby vyberte hello chcete toopublish toohello marketplace.
   
    ![Azure Marketplace][workspace]
3. Koncový bod vyberte hello by jako toohave hello marketplace spotřebujete. Pokud jste nevytvořili žádné další koncové body, můžete si vybrat hello **výchozí** koncový bod.
4. Po klepnutí na koncovém bodu hello, nebudete moct toosee hello **klíč rozhraní API**. Je nutné tuto část informací později v kroku 3, proto zkontrolujte jeho kopii.
   
    ![Azure Marketplace][apikey]
5. Klikněte na hello **požadavků a odpovědí** metody v tomto okamžiku nepodporujeme publikování dávkového spuštění služeb toohello marketplace. Který vás nasměruje stránku nápovědy toohello rozhraní API pro hello metoda žádosti a odpovědi.
6. Kopírování hello **adresa koncového bodu OData**, budete potřebovat tyto informace později v kroku 3.
   
    ![Azure Marketplace][odata]

hello službu nasaďte do produkčního prostředí.

### <a name="step-3-use-hello-url-of-hello-published-web-service-toopublish-tooazure-marketplace-datamarket"></a>Krok 3: Použití hello URL hello publikované webové služby toopublish tooAzure Marketplace (DataMarket)
1. Přejděte příliš[Azure Marketplace (trhu dat)](http://datamarket.azure.com/home) 
2. Klikněte na hello **publikovat** odkaz hello horní části stránky hello. Tím přejdete toohello [publikování portálu Microsoft Azure](https://publish.windowsazure.com)
3. Klikněte na hello **vydavatelů** části tooregister jako vydavatel.
4. Při vytváření nové nabídky, vyberte **datové služby**, pak klikněte na tlačítko **vytvořit novou službu Data**. 
   
   ![Azure Marketplace][image1]
   
   <br />
5. V části **plány** poskytují informace o vaší nabídky, včetně cenový plán. Rozhodněte, pokud nabízíte volné nebo placené služby. tooget placené, zadání platebních údajů, jako jsou vaše informace o bank a daň.
6. V části **Marketing** poskytují informace o vaší nabídky, jako je například hello název a popis pro vaši nabídku.
7. V části **cenová** můžete nastavit hello ceny pro plánu pro konkrétní země, nebo to hello systému "autoprice" vaši nabídku.
8. Na hello **služba dat** , klikněte na **webové služby** jako hello **zdroj dat**.
   
    ![Azure Marketplace][image2]
9. Získáte klíč adresy URL a rozhraní API webové služby hello z hello portálu Azure Classic, jak je popsáno v kroku 2 výše.
10. V hello Marketplace. Data služby instalace dialogové okno, vložte do hello adresa koncového bodu OData hello **adresa URL služby** textové pole.
11. Pro **ověřování**, zvolte **záhlaví** jako hello **schéma ověřování**.
    
    * Zadejte "Autorizace" hello **název hlavičky**.
    * Pro hello **hodnota hlavičky**, zadejte "Nosiče" (bez uvozovek hello), klikněte na hello **místo** panel a potom vložte klíč hello rozhraní API.
    * Vyberte hello **tato služba je OData** zaškrtávací políčko.
    * Klikněte na tlačítko **Test připojení** tootest hello připojení.
12. V části **kategorie**, ujistěte se, **Machine Learning** je vybrána.
13. Po dokončení všech zadávání hello metadata o vaši nabídku, klikněte na **publikovat**a potom **Push tooStaging**. V tomto okamžiku jste budou informováni o všechny zbývající problémy, je nutné, aby toofix.
14. Po dokončení všech zbývajících problémů hello zajistíte, klikněte na **požádat o schválení toopush tooProduction**. proces publikování Hello může trvat několik pracovních dnů. 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

