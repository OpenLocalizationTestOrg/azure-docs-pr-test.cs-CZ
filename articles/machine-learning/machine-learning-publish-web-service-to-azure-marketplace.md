---
title: "(nepoužívané) Publikování machine learning webové služby pro Azure Marketplace | Microsoft Docs"
description: "(nepoužívané) Jak publikovat webovou službu Azure Machine Learning v Azure Marketplace"
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
redirect_document_id: TRUE
ms.openlocfilehash: 3e3420872f0c604e027d1f309a6de6f52a5a788c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>(nepoužívané) Publikovat webovou službu Azure Machine Learning v Azure Marketplace

> [!NOTE]
> DataMarket a Data služby se postupně vyřazuje z provozu a odběry vyřadí a zrušena od 3/31/2017. V důsledku toho je zastaralá v tomto článku. 
> 
> Jako alternativu, můžete publikovat Machine Learning experimentů k [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) prospěch komunity vědecké účely data. Další informace najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).

Azure Marketplace nabízí možnost publikování webové služby Azure Machine Learning jako placené nebo bezplatné služby pro spotřeba externí zákazníky. Tento článek obsahuje přehled procesu s odkazy na pokyny, které vám pomůžou začít. Pomocí tohoto procesu můžete nastavit webové služby k dispozici pro ostatní vývojáři využívat ve svých aplikacích.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Přehled procesu publikování
Tady jsou kroky pro publikování na webu Azure Marketplace webové služby Azure Machine Learning:

1. Vytvoření a publikování služby Machine Learning požadavků a odpovědí (záznamy RR)
2. Nasazení služby do produkčního prostředí a získat klíč rozhraní API a OData informace koncový bod.
3. Použijte adresu URL publikované webové služby pro publikování do [Azure Marketplace (trhu dat)](https://publish.windowsazure.com/workspace/) 
4. Po odeslání vaši nabídku zkontrolovány a vyžaduje schválení před vašim zákazníkům můžete spustit zakoupení ho. Proces publikování může trvat několik pracovních dnů. 

## <a name="walk-through"></a>Názorný postup
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Krok 1: Vytvoření a publikování služby Machine Learning požadavků a odpovědí (záznamy RR)
 Pokud jste to ještě neudělali, proveďte prosím podívejte se na to [provede](machine-learning-walkthrough-5-publish-web-service.md).

### <a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Krok 2: Nasazení služby do produkčního prostředí a získat klíč rozhraní API a informace o koncový bod OData
1. Z [portálu Azure Classic](http://manage.windowsazure.com), vyberte **MACHINE LEARNING** možnost levém navigačním panelu a vyberte pracovní prostor. 
2. Klikněte na **webové služby** a vyberte webovou službu, kterou chcete publikovat na webu marketplace.
   
    ![Azure Marketplace][workspace]
3. Vyberte, chcete mít v marketplace využívat koncový bod. Pokud jste nevytvořili žádné další koncové body, můžete vybrat **výchozí** koncový bod.
4. Po klepnutí na koncovém bodu, bude moci zobrazit **klíč rozhraní API**. Je nutné tuto část informací později v kroku 3, proto zkontrolujte jeho kopii.
   
    ![Azure Marketplace][apikey]
5. Klikněte na **požadavků a odpovědí** metody v tomto okamžiku nepodporujeme publikování služby batch provádění na Marketplace s cílem. Která se dostanete na stránku nápovědy rozhraní API pro metodu požadavků a odpovědí.
6. Kopírování **adresa koncového bodu OData**, budete potřebovat tyto informace později v kroku 3.
   
    ![Azure Marketplace][odata]

nasazení služby do produkčního prostředí.

### <a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Krok 3: Použijte adresu URL publikovanou webovou službu publikování na webu Azure Marketplace (DataMarket)
1. Přejděte na [Azure Marketplace (trhu dat)](http://datamarket.azure.com/home) 
2. Klikněte na **publikovat** odkaz v horní části stránky. Bude to trvat, abyste [publikování portálu Microsoft Azure](https://publish.windowsazure.com)
3. Klikněte na **vydavatelů** části k registraci jako vydavatel.
4. Při vytváření nové nabídky, vyberte **datové služby**, pak klikněte na tlačítko **vytvořit novou službu Data**. 
   
   ![Azure Marketplace][image1]
   
   <br />
5. V části **plány** poskytují informace o vaší nabídky, včetně cenový plán. Rozhodněte, pokud nabízíte volné nebo placené služby. K získání placené, zadání platebních údajů, jako jsou vaše informace o bank a daň.
6. V části **Marketing** poskytují informace o vaší nabídky, jako je název a popis pro vaši nabídku.
7. V části **cenová** můžete nastavit ceny pro plánu pro konkrétní země, nebo aby systém "autoprice" vaši nabídku.
8. Na **služba dat** , klikněte na **webové služby** jako **zdroj dat**.
   
    ![Azure Marketplace][image2]
9. Získání klíče služby webové adresy URL a rozhraní API z klasického portálu Azure, jak je popsáno v kroku 2 výše.
10. V dialogovém okně Nastavení Marketplace. Data Service, vložte adresu koncový bod OData do **adresa URL služby** textové pole.
11. Pro **ověřování**, zvolte **záhlaví** jako **schéma ověřování**.
    
    * Zadejte "Autorizace" **název hlavičky**.
    * Pro **hodnota hlavičky**, zadejte "Nosiče" (bez uvozovek), klikněte na tlačítko **místo** panel a vložte klíč rozhraní API.
    * Vyberte **tato služba je OData** zaškrtávací políčko.
    * Klikněte na tlačítko **otestovat připojení** k testování připojení.
12. V části **kategorie**, ujistěte se, **Machine Learning** je vybrána.
13. Po dokončení zadávání všechna metadata o vaši nabídku, klikněte na **publikovat**a potom **nabízená pracovní**. V tomto okamžiku budete upozorněni všechny zbývající problémy, které je třeba opravit.
14. Po dokončení všech zbývajících problémů zajistíte, klikněte na **požádat o schválení tak, aby nabízel do produkčního prostředí**. Proces publikování může trvat několik pracovních dnů. 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

