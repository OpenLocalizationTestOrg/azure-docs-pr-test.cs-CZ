---
title: "aaaConsume pomocí šablony webové aplikace webové služby Machine Learning | Microsoft Docs"
description: "Použití šablony webové aplikace v Azure Marketplace tooconsume prediktivní webové služby v Azure Machine Learning."
keywords: "Webová služba, operationalization, REST API strojového učení"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.openlocfilehash: 1199377bead470807d58ca7f7a667175cbb88450
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Využívání webové služby Azure Machine Learning pomocí šablony webové aplikace

Jednou jste vyvinuté prediktivního modelu a nasadit jako Azure webové služby pomocí Machine Learning Studio, nebo pomocí nástrojů, jako je R nebo Python, můžete přístup hello operationalized model pomocí rozhraní REST API.

Existuje několik způsobů tooconsume hello REST API a přístup hello webové služby. Například můžete napsat aplikaci v jazyce C#, R, nebo Python pomocí hello ukázkový kód pro vás vygeneroval při nasazení hello webové služby (k dispozici v hello [Machine Learning Web Services portálu](https://services.azureml.net/quickstart) nebo v řídicím panelu hello webových služeb v Machine Learning Studio). Nebo můžete použít sešit aplikace Excel ukázka hello vytvořená v hello stejnou dobu.

Ale hello tooaccess nejrychlejší a nejjednodušší způsob je webová služba prostřednictvím hello webové aplikace šablony dostupné na hello [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-azure-machine-learning-web-app-templates"></a>Hello Azure Machine Learning webové aplikace šablony
Hello webové aplikace šablony dostupné na hello Azure Marketplace můžete vytvořit vlastní webové aplikace, kterou zná vstupní data webové služby a očekávané výsledky. Stačí toodo je poskytnout webové služby přístup tooyour hello webové aplikace a data a šablony hello hello rest.

Dvě šablony jsou k dispozici:

* [Šablony aplikace webové služby Azure ML požadavků a odpovědí](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Šablony aplikace webové služby provedení dávky Azure ML](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Každá šablona vytvoří ukázkovou aplikaci ASP.NET, pomocí hello API URI a klíč pro webovou službu a nasadí jej jako tooAzure webu. Šablona služby požadavků a odpovědí (záznamy RR) Hello vytvoří webovou aplikaci, která vám umožní toosend jednoho řádku dat toohello webové služby tooget jeden výsledek. Hello spuštění služby Batch (BES) šablona vytvoří webovou aplikaci, která vám umožní toosend mnoho řádků dat tooget více výsledků.

Žádné kódování je požadovaná toouse tyto šablony. Stačí zadat hello klíč rozhraní API a identifikátoru URI a hello šablona vytvoří hello aplikaci za vás.

klíč tooget hello rozhraní API a URI požadavku pro webovou službu:

1. V hello [webový portál služby](https://services.azureml.net/quickstart), novou webovou službu, klikněte na tlačítko **webové služby** v horní části hello. Nebo pro web Classic, klikněte na tlačítko služby **Classic webové služby**.
2. Klikněte na tlačítko chcete tooaccess hello webové služby.
3. Klasické webové služby klikněte na koncový bod hello chcete tooaccess.
4. Klikněte na tlačítko **spotřebě** v horní části hello.
5. Kopírování hello **primární** nebo **sekundární klíč** a uložte ho.
6. Pokud vytváříte šablonu služby požadavků a odpovědí (záznamy RR), zkopírovat hello **požadavků a odpovědí** URI a uložte ho. Pokud vytváříte šablonu spuštění služby Batch (BES), zkopírovat hello **dávkových žádostí** URI a uložte ho.


## <a name="how-toouse-hello-request-response-service-rrs-template"></a>Jak toouse hello šablony služby požadavků a odpovědí (záznamy RR)
Postupujte podle těchto kroků toouse hello RRS webové aplikace šablony, jak je znázorněno v následujícím diagramu hello.

![Proces toouse RRS webové šablony][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Přejděte toohello [portál Azure](https://portal.azure.com), **přihlášení**, klikněte na tlačítko **nový**, vyhledejte a vyberte **webové aplikace Azure ML požadavků a odpovědí Service**, pak klikněte na **Vytvořit**. 
   
   * Zadejte jedinečný název webové aplikace. Adresa URL Hello hello webové aplikace bude tento název, za nímž následuje `.azurewebsites.net.` například`http://carprediction.azurewebsites.net.`
   * Vyberte hello předplatného Azure a služby, pod kterým běží webové služby.
   * Klikněte na možnost **Vytvořit**.
     
     ![Vytvoření webové aplikace][image5]

4. Po dokončení nasazení hello webové aplikace Azure, klikněte na tlačítko hello **URL** na hello webové stránky nastavení aplikace v Azure, nebo zadejte adresu URL hello ve webovém prohlížeči. Například `http://carprediction.azurewebsites.net.`.
5. Když hello spustí první webové aplikace se zobrazí výzvu k hello **Post URL rozhraní API** a **klíč rozhraní API**.
   Zadejte hello hodnoty, které jste předtím uložili (**URI požadavku** a **klíč rozhraní API**, v uvedeném pořadí).
     
     Klikněte na tlačítko **odeslání**.
     
     ![Zadejte Post URI a klíč rozhraní API][image6]

6. Hello webové aplikace zobrazí jeho **konfiguraci webové aplikace** stránka s hello aktuální nastavení webové služby. Zde můžete provádět změny nastavení toohello používané hello webové aplikace.
   
   > [!NOTE]
   > Nastavení hello zde pouze změna pro tuto webovou aplikaci. Neprojeví hello výchozí nastavení webové služby. Například, pokud změníte hello **popis** zde nemění hello Popis zobrazený na řídicí panel hello webové služby v nástroji Machine Learning Studio.
   > 
   > 
   
    Když jste hotovi, klikněte na tlačítko **uložit změny**a potom klikněte na **přejděte tooHome stránky**.

7. Z hello domovské stránky, které můžete zadat hodnoty toosend tooyour webové služby. Klikněte na tlačítko **odeslání** když jste hotovi, a bude vrácen hello výsledek.

Pokud chcete, aby tooreturn toohello **konfigurace** stránky, přejděte toohello `setting.aspx` stránku hello webové aplikace. Příklad: `http://carprediction.azurewebsites.net/setting.aspx.` bude výzvami tooenter hello API klíč znovu – budete potřebovat, tooaccess hello stránky a aktualizovat nastavení hello.

Můžete zastavit, restartovat nebo odstranit hello webové aplikace ve hello portálu Azure stejně jako jiná webové aplikace. Tak dlouho, dokud běží můžete procházet toohello domácí webovou adresu a zadejte nové hodnoty.

## <a name="how-toouse-hello-batch-execution-service-bes-template"></a>Jak toouse hello šablony spuštění služby Batch (BES)
Můžete použít hello BES šablony webové aplikace v hello stejný způsobem jako hello záznamy o prostředku šablony, s výjimkou tohoto hello webovou aplikaci, která je vytvořena vám umožní toosubmit více řádků dat a příjem více výsledků.

Hello vstupní hodnoty pro spuštění webové služby batch mohou pocházet z úložiště Azure nebo místního souboru; Hello výsledky ukládat do kontejneru úložiště Azure.
Ano, budete potřebovat toohold kontejneru úložiště Azure hello výsledků vrácených hello webové aplikace a budete potřebovat tooget vstupní data připravena.

![Zpracování toouse BES webové šablony][image2]

1. Postupujte podle hello stejný postup toocreate hello BES webová aplikace jako šablony hello RRS, s výjimkou přejděte příliš[Azure ML dávky spuštění služby webové aplikace šablona](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) tooopen hello BES šablony v Azure Marketplace a klikněte na tlačítko **vytvoření webové aplikace** .

2. toospecify místo hello výsledky uložené, zadejte informace o kontejneru cílové hello na domovskou stránku hello webové aplikace. Také určete, kde hello webovou aplikaci můžete získat hello vstupní hodnoty, buď v případě místního souboru nebo kontejner úložiště Azure.
   Klikněte na tlačítko **odeslání**.
   
    ![Informace o úložiště][image7]

Hello webové aplikace se zobrazí stránka s stav úlohy.
Po dokončení úlohy hello budete mít k dispozici hello umístění hello výsledky v Azure blob storage. Máte také možnost hello stahování hello výsledky tooa místního souboru.

## <a name="for-more-information"></a>Další informace
Další informace o toolearn...

* Vytvoření experimentu machine learning s Machine Learning Studio najdete v části [vytvoření prvního experimentu v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md)
* jak zjistit, toodeploy strojové učení experimentovat jako webovou službu, [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md)
* jiné způsoby tooaccess vaší webové služby, najdete v části [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md)

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png
