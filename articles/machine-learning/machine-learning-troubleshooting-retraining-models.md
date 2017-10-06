---
title: "aaaTroubleshoot retraining Azure Machine Learning Classic webové služby | Microsoft Docs"
description: "Identifikujte a opravte narazil běžné problémy, když jsou retraining hello modelu pro webové služby Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 2b6a78eaba161877106dccdc23437b5e454fca7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-retraining-of-an-azure-machine-learning-classic-web-service"></a>Řešení potíží s hello retraining Azure Machine Learning Classic webové služby
## <a name="retraining-overview"></a>Retraining – přehled
Když nasadíte prediktivní experiment jako vyhodnocování webové služby je statický model. Jakmile nová data k dispozici nebo pokud příjemce hello hello API má svá vlastní data, musí hello modelu toobe retrained. 

Kompletní návod hello retraining proces Classic webové služby, najdete v části [Přeučování Machine Learning modely prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Retraining procesu
Pokud budete potřebovat tooretrain hello webové služby, musíte přidat další některé jeho součásti:

* Webovou službu nasazenou z hello výukový Experiment. musí mít Hello experiment **výstup webové služby** modulu připojené toohello výstup hello **Train Model** modulu.  
  
    ![Připojte hello webové služby výstupní toohello train model.][image1]
* Nový koncový bod přidat tooyour vyhodnocování webové služby.  Můžete přidat koncový bod hello programově pomocí hello ukázkový kód v hello odkazuje programovém přeučení modelů Machine Learning tématu nebo prostřednictvím hello portál Azure classic.

Potom můžete hello ukázkový kód C# z modelu tooretrain stránky nápovědy API hello školení webové služby. Jakmile máte vyhodnotit hello výsledky a spokojeni s nimi, aktualizujete hello trained model vyhodnocování webové službě pomocí nový koncový bod hello, kterou jste přidali.

Hello hlavních kroků, je nutné provést tooretrain hello modelu se všechny kusy hello na místě, jsou následující:

1. Volání hello školení webové služby: volání hello je toohello spuštění služby Batch (BES), není hello žádosti o služby odpovědi (RR). Můžete vytvořit hello ukázkový kód C# hello rozhraní API nápovědy stránky toomake hello volání. 
2. Najít hello hodnoty pro hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken*: tyto hodnoty jsou vráceny ve výstupu hello z vaší toohello volání webové školení Služba. 
   ![zobrazuje výstup hello hello retraining ukázka a hello BaseLocation, RelativeLocation a SasBlobToken hodnoty.][image6]
3. Aktualizace hello přidat koncový bod z hello vyhodnocování webové služby s hello nové Trénink modelu: pomocí hello ukázkový kód zadaný v hello Machine Learning Přeučování modelů prostřednictvím kódu programu, aktualizace nový koncový bod hello jste přidali toohello nově vyhodnocování model s hello trained model z hello školení webové služby.

## <a name="common-obstacles"></a>Běžné překážek
### <a name="check-toosee-if-you-have-hello-correct-patch-url"></a>Zkontrolujte toosee, pokud máte hello opravte oprava adresy URL
Hello oprava URL používáte musí být hello jeden přidružené hello nový vyhodnocovací koncový bod přidat toohello vyhodnocování webové služby. Existuje několik způsobů tooobtain hello oprava adresy URL:

**Možnost 1: programově**

tooget hello opravte oprava adresy URL:

1. Spustit hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.
2. Z výstupu hello AddEndpoint, najde hello *HelpLocation* hodnotu a zkopírujte adresu URL hello.
   
   ![HelpLocation ve výstupu hello hello addEndpoint vzorku.][image2]
3. Vložte hello URL do prohlížeče toonavigate tooa stránky, který poskytuje odkazy na nápovědu pro hello webové služby.
4. Klikněte na tlačítko hello **aktualizace prostředků** stránku nápovědy oprava hello tooopen odkaz.

**Možnost 2: Použití hello portál Azure classic**

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Karta otevřete hello Machine Learning. ![Karta opřené počítače.][image4]
3. Pak klikněte na název pracovního prostoru, **webové služby**.
4. Klikněte na tlačítko hello vyhodnocování webové služby, kterou pracujete. (Pokud jste nezměnila hello výchozí název hello webové služby, se bude končit [vyhodnocování Exp.].)
5. Klikněte na tlačítko **přidání koncového bodu**.
6. Po přidání koncového bodu hello klikněte na název koncového bodu hello. Pak klikněte na tlačítko **aktualizace prostředků** tooopen hello oprav stránku nápovědy.

> [!NOTE]
> Pokud jste přidali hello koncový bod toohello školení webové služby místo hello prediktivní webové služby, zobrazí se následující chyba po kliknutí na tlačítko hello hello **aktualizace prostředků** odkaz: je nám líto, ale tato funkce není podporována nebo v tomto kontextu dostupné. Tato webová služba nemá žádné aktualizovat prostředky. Omlouváme se za nepříjemnosti hello a práce na zlepšení tento pracovní postup.
> 
> 

![Nový koncový bod řídicí panel.][image3]

stránka nápovědy oprava Hello obsahuje hello oprava adresa URL musí používat a poskytuje ukázkový kód můžete použít toocall ho.

![Adresa URL opravy.][image5]

### <a name="check-toosee-that-you-are-updating-hello-correct-scoring-endpoint"></a>Zkontrolujte toosee aktualizaci hello správný vyhodnocování koncový bod
* Není oprava hello školení webové služby: hello operace opravy se musí provést na hello vyhodnocování webové služby.
* Není oprava hello výchozí koncový bod webové služby: hello operace opravy se musí provést na hello nové vyhodnocování koncový bod webové služby, které jste přidali.

Můžete ověřit, které koncový bod webové služby hello je zapnutá ve hostujícími hello portál Azure classic. 

> [!NOTE]
> Ujistěte se, že přidáváte hello koncový bod toohello prediktivní webové služby, hello školení webové služby. Pokud jste nasadili správně školení a prediktivní webové služby, měli byste vidět dvě samostatné webové služby uvedené. Hello prediktivní webové služby musí končit "[prediktivní exp.]".
> 
> 

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Karta otevřete hello Machine Learning. ![Machine learning prostoru uživatelského rozhraní.][image4]
3. Vyberte pracovní prostor.
4. Klikněte na tlačítko **webové služby**.
5. Vyberte prediktivní webové služby.
6. Ověřte, že váš nový koncový bod byl přidán toohello webové služby.

### <a name="check-hello-workspace-that-your-web-service-is-in-tooensure-it-is-in-hello-correct-region"></a>Zkontrolujte hello pracovní prostor, který je webová služba v tooensure, který je v oblasti správné hello
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Vyberte z nabídky hello Machine Learning.
   ![Machine learning oblast uživatelského rozhraní.][image4]
3. Zkontrolujte umístění hello pracovního prostoru.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png
