---
title: "aaaConnect tooa webová služba Machine Learning | Microsoft Docs"
description: "Pomocí jazyka C# nebo Python připojte tooan Azure Machine Learning webové službě pomocí autorizační klíč."
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
redirect_document_id: True
ms.openlocfilehash: 0108e71e30a05539a8c0ee93d5aadb07e3d1efa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-machine-learning-web-service"></a>Připojit tooan webovou službu Azure Machine Learning
Hello prostředí pro vývojáře Azure Machine Learning je Web service API toomake predikcím ze vstupní data v reálném čase nebo v dávkovém režimu. Pomocí Azure Machine Learning Studio toocreate předpovědi a nasazení Azure Machine Learning webové služby.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

toolearn o toocreate a nasadit Machine Learning webové služby pomocí nástroje Machine Learning Studio:

* Kurz týkající se jak toocreate experimentu v nástroji Machine Learning Studio, najdete v části [vytvoření prvního experimentu](machine-learning-create-experiment.md).
* Podrobnosti o tom najdete v části toodeploy webové služby, [nasazení služby Machine Learning webové](machine-learning-publish-a-machine-learning-web-service.md).
* Další informace o Machine Learning obecně naleznete hello [dokumentace k centru pro Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure Machine Learning webové služby
S hello Azure Machine Learning webové služby externí aplikace komunikuje se vyhodnocování model Machine Learning pracovního postupu v reálném čase. Volání Machine Learning webové služby vrátí výsledky předpovědi tooan externí aplikaci. toomake volání Machine Learning webové služby, předáte klíč rozhraní API, která se vytvoří při nasazení předpovědi. Hello Machine Learning webové služby je založena na REST, možnost populární architektury pro webové projekty programování.

Azure Machine Learning zahrnuje dva typy služeb:

* Požadavků a odpovědí služby (záznamy RR) – s nízkou latencí, vysoce škálovatelná služba, která poskytuje rozhraní toohello bezstavové modely vytvořené a nasazené z hello Machine Learning Studio.
* Spuštění služby Batch (BES) – asynchronní služby tohoto skóre a dávky pro datových záznamů.

Další informace o Machine Learning webových služeb najdete v tématu [nasazení služby Machine Learning webové](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Získání klíče autorizace Azure Machine Learning
Když nasadíte experimentu, klíče rozhraní API se generují pro hello webové služby. Můžete získat hello klíče v několika umístěních.

### <a name="from-hello-microsoft-azure-machine-learning-web-services-portal"></a>Z portálu Microsoft Azure Machine Learning webové služby hello
Přihlaste se toohello [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net) portálu.

klíč tooretrieve hello rozhraní API pro nové Machine Learning webové služby:

1. Hello portálu webové služby Azure Machine Learning, klikněte na tlačítko **webové služby** hello horní nabídce.
2. Klikněte na tlačítko hello webové služby, pro kterou chcete tooretrieve hello klíč.
3. V horní nabídce hello, klikněte na tlačítko **spotřebě**.
4. Zkopírujte a uložte hello **primární klíč**.

klíč tooretrieve hello rozhraní API pro Classic Machine Learning webové služby:

1. Hello portálu webové služby Azure Machine Learning, klikněte na tlačítko **Classic webové služby** hello horní nabídce.
2. Klikněte na tlačítko hello webové služby, se kterým pracujete.
3. Klikněte na tlačítko hello koncový bod, pro kterou chcete tooretrieve hello klíč.
4. V horní nabídce hello, klikněte na tlačítko **spotřebě**.
5. Zkopírujte a uložte hello **primární klíč**.

### <a name="classic-web-service"></a>Classic webové služby
 Můžete také načíst klíč pro Classic webové služby Machine Learning Studio nebo hello portál Azure classic.

#### <a name="machine-learning-studio"></a>Machine Learning Studio
1. V nástroji Machine Learning Studio, klikněte na tlačítko **webové služby** na levé straně hello.
2. Klikněte na webovou službu. Hello **klíč rozhraní API** na hello **řídicí panel** kartě.

#### <a name="azure-classic-portal"></a>klasický portál Azure
1. Klikněte na tlačítko **MACHINE LEARNING** na levé straně hello.
2. Klikněte na tlačítko hello prostoru, ve kterém se nachází webové služby.
3. Klikněte na tlačítko **webové služby**.
4. Klikněte na webovou službu.
5. Klikněte na koncový bod. Hello "API KEY" je mimo provoz v pravém dolním hello.

## <a id="connect"></a>Připojit tooa Machine Learning webové služby
Tooa Machine Learning webové službě pomocí programovací jazyk, který podporuje žádostí HTTP a odpovědí se můžete připojit. Příklady můžete zobrazit v C#, Python a R z Machine Learning webové stránky nápovědy služby.

**Počítač Learning API nápovědy** Machine Learning API nápovědy se vytvoří při nasazení webové služby. V tématu [Azure Machine Learning návod - nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md).
Hello Machine Learning API nápovědy obsahuje podrobnosti o předpovědi webové služby.

1. Klikněte na tlačítko hello webové služby, se kterým pracujete.
2. Klikněte na tlačítko hello koncový bod, pro které chcete tooview hello stránce nápovědy k rozhraní API.
3. V horní nabídce hello, klikněte na tlačítko **spotřebě**.
4. Klikněte na tlačítko **stránku nápovědy rozhraní API** pod hello požadavků a odpovědí nebo Batch Execution koncové body.

**tooview Machine Learning API nápovědy pro novou webovou službu**

V portálu služby Azure Machine Learning webové hello:

1. Klikněte na tlačítko **webové služby** v horní nabídce hello.
2. Klikněte na tlačítko hello webové služby, pro kterou chcete tooretrieve hello klíč.

Klikněte na tlačítko **spotřebě** tooget hello identifikátory URI pro hello Reposonse požadavku a spuštění služby Batch a ukázkový kód v jazyce C#, R a Python.

Klikněte na tlačítko **rozhraní API Swaggeru** tooget Swagger založené na dokumentaci hello rozhraní API volat z hello zadané identifikátory URI.

### <a name="c-sample"></a>Ukázka C#
tooconnect tooa Machine Learning webové služby, použijte **HttpClient** předávání ScoreData. ScoreData obsahuje FeatureVector, n dimenzí vektor číselné funkce představující hello ScoreData. Ověřování služby Machine Learning toohello s klíčem rozhraní API.

tooconnect tooa webové služby Machine Learning hello **Microsoft.AspNet.WebApi.Client** musí být nainstalován balíček NuGet.

**Nainstalovat Microsoft.AspNet.WebApi.Client NuGet v sadě Visual Studio**

1. Publikování hello datovou sadu stáhnout z UCI: dataset – třída pro dospělé 2 webové služby.
2. Klikněte na **Nástroje**  >  **Správce balíčků NuGet**  >  **Konzola správce balíčků**.
3. Zvolte **Install-Package Microsoft.AspNet.WebApi.Client**.

**Ukázka kódu toorun hello**

1. Publikování "Příklad 1: Stáhněte datové sady z UCI: datovou sadu pro dospělé 2 – třída" experiment, součástí hello Machine Learning ukázka kolekce.
2. Přiřaďte apiKey klíčem hello z webové služby. V tématu **získání klíče autorizace Azure Machine Learning** výše.
3. Přiřaďte serviceUri s hello URI požadavku.

### <a name="python-sample"></a>Ukázka Pythonu
tooconnect tooa Machine Learning webové služby, použijte hello **urllib2** předávání ScoreData knihovny. ScoreData obsahuje FeatureVector, n dimenzí vektor číselné funkce představující hello ScoreData. Ověřování služby Machine Learning toohello s klíčem rozhraní API.

**Ukázka kódu toorun hello**

1. Nasadit "Příklad 1: Stáhněte datové sady z UCI: datovou sadu pro dospělé 2 – třída" experiment, součástí hello Machine Learning ukázka kolekce.
2. Přiřaďte apiKey klíčem hello z webové služby. V tématu hello **získání klíče autorizace Azure Machine Learning** části téměř hello začátku tohoto článku.
3. Přiřaďte serviceUri s hello URI požadavku.

