---
title: "Připojení k webové službě Machine Learning | Microsoft Docs"
description: "Pomocí jazyka C# nebo Python připojte k Azure Machine Learning webové služby pomocí autorizační klíč."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 59b07bab-b60f-48c4-a385-a162e50ec7c2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: garye
ROBOTS: NOINDEX
redirect_url: machine-learning-consume-web-services
redirect_document_id: TRUE
ms.openlocfilehash: 0fc6c7e921b18eb14a95fb737d8fb5ab5cc7e687
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-an-azure-machine-learning-web-service"></a>Připojení k webové službě Azure Machine Learning
Možnosti pro vývojáře Azure Machine Learning je webové rozhraní API služby k vytvoření predikcím ze vstupní data v reálném čase nebo v dávkovém režimu. Azure Machine Learning Studio použijete k vytvoření předpovědi a nasazení Azure Machine Learning webové služby.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Další informace o tom, jak vytvořit a nasadit Machine Learning webové služby pomocí nástroje Machine Learning Studio:

* Kurz týkající se vytvoření experimentu v nástroji Machine Learning Studio, najdete v části [vytvoření prvního experimentu](machine-learning-create-experiment.md).
* Podrobnosti o tom, jak nasadit webovou službu najdete v tématu [nasazení služby Machine Learning webové](machine-learning-publish-a-machine-learning-web-service.md).
* Další informace o Machine Learning obecně naleznete [dokumentace k centru pro Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure Machine Learning webové služby
Pomocí Azure Machine Learning webové služby externí aplikace komunikuje se vyhodnocování model Machine Learning pracovního postupu v reálném čase. Volání Machine Learning webové služby vrátí výsledky předpovědi externí aplikací. Chcete-li volání Machine Learning webové služby, předáte klíč rozhraní API, která se vytvoří při nasazení předpovědi. Machine Learning webové služby je založena na REST, možnost populární architektury pro webové projekty programování.

Azure Machine Learning zahrnuje dva typy služeb:

* Požadavků a odpovědí služby (záznamy RR) – s nízkou latencí, vysoce škálovatelná služba, která poskytuje rozhraní pro bezstavové modely vytvořené a nasazené z Machine Learning Studio.
* Spuštění služby Batch (BES) – asynchronní služby tohoto skóre a dávky pro datových záznamů.

Další informace o Machine Learning webových služeb najdete v tématu [nasazení služby Machine Learning webové](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Získání klíče autorizace Azure Machine Learning
Když nasadíte experimentu, klíče rozhraní API se generují pro webovou službu. Klíče můžete načíst z několika umístění.

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Z portálu Microsoft Azure Machine Learning webové služby
Přihlaste se k [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net) portálu.

Načíst klíč rozhraní API pro nové Machine Learning webové služby:

1. Na portálu webové služby Azure Machine Learning, klikněte na tlačítko **webové služby** v hlavní nabídce.
2. Klikněte na webovou službu, pro který chcete načíst klíč.
3. V horní nabídce klikněte na tlačítko **spotřebě**.
4. Zkopírujte a uložte **primární klíč**.

Načíst klíč rozhraní API pro Classic Machine Learning webové služby:

1. Na portálu webové služby Azure Machine Learning, klikněte na tlačítko **Classic webové služby** v hlavní nabídce.
2. Klikněte na webovou službu, se kterým pracujete.
3. Klikněte na koncový bod, pro který chcete načíst klíč.
4. V horní nabídce klikněte na tlačítko **spotřebě**.
5. Zkopírujte a uložte **primární klíč**.

### <a name="classic-web-service"></a>Classic webové služby
 Můžete také načíst klíč pro Classic webové služby Machine Learning Studio nebo portálu Azure classic.

#### <a name="machine-learning-studio"></a>Machine Learning Studio
1. V nástroji Machine Learning Studio, klikněte na tlačítko **webové služby** na levé straně.
2. Klikněte na webovou službu. **Klíč rozhraní API** na **řídicí panel** kartě.

#### <a name="azure-classic-portal"></a>klasický portál Azure
1. Klikněte na tlačítko **MACHINE LEARNING** na levé straně.
2. Klikněte na pracovním prostoru, ve kterém se nachází webové služby.
3. Klikněte na tlačítko **webové služby**.
4. Klikněte na webovou službu.
5. Klikněte na koncový bod. "API KEY" je mimo provoz v pravém dolním.

## <a id="connect"></a>Připojení k webové služby Machine Learning
Můžete připojit k službě Machine Learning webové pomocí programovací jazyk, který podporuje žádostí HTTP a odpovědí. Příklady můžete zobrazit v C#, Python a R z Machine Learning webové stránky nápovědy služby.

**Počítač Learning API nápovědy** Machine Learning API nápovědy se vytvoří při nasazení webové služby. V tématu [Azure Machine Learning návod - nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md).
Machine Learning API nápovědy obsahuje podrobnosti o předpovědi webové služby.

1. Klikněte na webovou službu, se kterým pracujete.
2. Klikněte na koncový bod, pro který chcete zobrazit stránce nápovědy k rozhraní API.
3. V horní nabídce klikněte na tlačítko **spotřebě**.
4. Klikněte na tlačítko **stránku nápovědy rozhraní API** pod buď požadavků a odpovědí nebo Batch Execution koncových bodů.

**Zobrazení Machine Learning API pomoci pro novou webovou službu**

V Azure Machine Learning webový portál služby:

1. Klikněte na tlačítko **webové služby** v horní nabídce.
2. Klikněte na webovou službu, pro který chcete načíst klíč.

Klikněte na tlačítko **spotřebě** získat identifikátory URI pro požadavek Reposonse a spuštění služby Batch a ukázkový kód v jazyce C#, R a Python.

Klikněte na tlačítko **rozhraní API Swaggeru** získat Swagger základě dokumentace pro rozhraní API volat z zadané identifikátory URI.

### <a name="c-sample"></a>Ukázka C#
Pro připojení k webové služby Machine Learning, použijte **HttpClient** předávání ScoreData. ScoreData obsahuje FeatureVector, n dimenzí vektor číselné funkce představující ScoreData. Ověření ke službě Machine Learning s klíčem rozhraní API.

Pro připojení k Machine Learning webové služby, **Microsoft.AspNet.WebApi.Client** musí být nainstalován balíček NuGet.

**Nainstalovat Microsoft.AspNet.WebApi.Client NuGet v sadě Visual Studio**

1. Publikování datovou sadu stáhnout z UCI: dataset – třída pro dospělé 2 webové služby.
2. Klikněte na **Nástroje**  >  **Správce balíčků NuGet**  >  **Konzola správce balíčků**.
3. Zvolte **Install-Package Microsoft.AspNet.WebApi.Client**.

**Ke spuštění ukázka kódu**

1. Publikování "Příklad 1: Stáhněte datové sady z UCI: datové sady pro dospělé 2 – třída" experiment, součástí kolekce ukázka Machine Learning.
2. Přiřaďte apiKey klíčem z webové služby. V tématu **získání klíče autorizace Azure Machine Learning** výše.
3. Přiřaďte serviceUri s identifikátor URI požadavku.

### <a name="python-sample"></a>Ukázka Pythonu
Pro připojení k webové služby Machine Learning, použijte **urllib2** předávání ScoreData knihovny. ScoreData obsahuje FeatureVector, n dimenzí vektor číselné funkce představující ScoreData. Ověření ke službě Machine Learning s klíčem rozhraní API.

**Ke spuštění ukázka kódu**

1. Nasadit "Příklad 1: Stáhněte datové sady z UCI: datové sady pro dospělé 2 – třída" experiment, součástí kolekce ukázka Machine Learning.
2. Přiřaďte apiKey klíčem z webové služby. Najdete v článku **získání klíče autorizace Azure Machine Learning** části téměř začátku tohoto článku.
3. Přiřaďte serviceUri s identifikátor URI požadavku.

