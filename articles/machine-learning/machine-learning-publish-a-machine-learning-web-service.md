---
title: "aaaDeploy webové službě Machine Learning | Microsoft Docs"
description: "Jak tooconvert školení experiment prediktivní experiment tooa, příprava pro nasazení a potom ho nasadit jako webovou službu Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 73a3e9c6-00d0-41d4-8cf1-2ec87713867e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 9cb7af637632b2c3688c11483f29cf24df8fd065
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-azure-machine-learning-web-service"></a>Nasazení webové služby Azure Machine Learning
Azure Machine Learning vám umožní toobuild, testování a nasazení řešení prediktivní analýzy.

Ze souhrnné bodu z – zobrazení provádí se v tři kroky:

* **[Vytvoření experimentu školení]**  -Azure Machine Learning Studio je spolupráce vizuální vývojové prostředí použít tootrain a otestovat prediktivní analýzy modelu pomocí Cvičná data, který zadáte.
* **[Ji převést prediktivní experiment tooa]**  – po modelu má cvičena s existujícími daty a vy budete připravené toouse ho tooscore nová data, připravte a zjednodušit experimentu pro předpovědi.
* **[Nasadit jako webovou službu]**  – můžete nasadit jako vaše prediktivní experiment [nové] nebo [classic] Azure webové služby. Uživatelé mohou datový model tooyour odesílat a přijímat váš model předpovědi.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Vytvoření experimentu školení
tootrain model prediktivní analýzy, můžete použít Azure Machine Learning Studio toocreate výukový experiment, kde zahrnout různých modulů tooload Cvičná data, připravit hello data podle potřeby, použít algoritmy strojového učení a vyhodnoťte výsledky hello . Můžete iterovat experiment a zkuste jinou strojového učení toocompare algoritmy a vyhodnoťte výsledky hello.

Hello proces vytváření a správa experimenty školení je zahrnutých víc důkladně jinde. Další informace najdete v těchto článcích:

* [Vytvoření jednoduchého experimentu v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md)
* [Vývoj prediktivního řešení pomocí Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)
* [Importu trénovacích dat do Azure Machine Learning Studio](machine-learning-data-science-import-data.md)
* [Správa iterací experimentu v nástroji Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md)

## <a name="convert-hello-training-experiment-tooa-predictive-experiment"></a>Převést hello výukový experiment tooa prediktivní experiment
Když jsme natrénovali model, jste připravené tooconvert vaše školení experimentovat na nová data tooscore prediktivní experiment.

Převedením tooa prediktivní experiment, vám vaše připravené toobe trained model nasadit jako vyhodnocování webovou službu. Uživatelé hello webové služby může odesílat vstupní data tooyour modelu a modelu pošle zpět hello předpovědi výsledky. Při převodu tooa prediktivní experiment, mějte na paměti jak očekáváte, že váš model toobe používat ostatní uživatelé.

tooconvert vaše školení tooa experiment prediktivní experiment, klikněte na tlačítko **spustit** hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby**, pak vyberte **prediktivní webové služby** .

![Převést tooscoring experimentu](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Další informace o tom, tooperform tento převod, najdete v části [jak tooprepare vašeho modelu pro nasazení v nástroji Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Hello následující kroky popisují nasazení prediktivní experiment jako novou webovou službu. Hello experimentu můžete taky nasadit jako webovou službu Classic.

## <a name="deploy-it-as-a-web-service"></a>Nasadit jako webovou službu

Prediktivní experiment hello můžete nasadit jako novou webovou službu nebo jako webovou službu Classic.

### <a name="deploy-hello-predictive-experiment-as-a-new-web-service"></a>Nasadit prediktivní experiment hello jako novou webovou službu
Teď, když byl připraven hello prediktivní experiment, můžete jej nasadit jako novou Azure webovou službu. Pomocí hello webové služby, mohou uživatelé odesílat data tooyour modelu a modelu hello vrátí jeho předpovědi.

toodeploy vaše prediktivní experiment, klikněte na tlačítko **spustit** dole hello hello experimentovat plátno. Po dokončení běhu hello experimentu klikněte na **nasazení webové služby** a vyberte **nasazení webové služby [nový]**.  Otevře se stránka nasazení Hello hello webová služba Machine Learning portálu.

> [!NOTE] 
> toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby. Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md). 

#### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Nasadit experimentu stránce portálu pro Machine Learning webové služby
Na stránce experimentu nasazení hello zadejte název pro hello webovou službu.
Vyberte cenový plán. Pokud máte existující cenový plán, že můžete ji vybrat, v opačném případě musíte vytvořit nový plán ceny služby hello.

1. V hello **cena plán** rozevírací nabídky vyberte existující plán nebo hello **vyberte nový plán** možnost.
2. V **název plánu**, zadejte název, který bude identifikovat hello plán ve vašem vyúčtování.
3. Vyberte jednu z hello **měsíční plánování vrstev**. plán Hello, že vrstvy výchozí toohello plány pro vaši oblast výchozí a webové služby je nasazené toothat oblast.

Klikněte na tlačítko **nasadit** a hello **rychlý Start** otevře se stránka pro webovou službu.

Rychlý start stránku Hello webové služby obsahuje přístup a pokyny o hello nejčastějších úloh, které provedete po vytvoření webové služby. Tady jsou snadno přístupné zkušební stránku hello a spotřebě stránky.

<!-- ![Deploy hello web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

#### <a name="test-your-new-web-service"></a>Otestovat novou webovou službu
tootest novou webovou službu, klikněte na tlačítko **testování webové služby** v části Běžné úlohy. Na stránce Test hello můžete otestovat webové služby jako požadavků a odpovědí služby (RR) nebo služba Batch Execution (BES).

zkušební stránku Hello RRS zobrazí hello vstupy, výstupy a globální parametry, které jste definovali hello experimentu. tootest hello webové služby, lze zadat ručně odpovídající hodnoty pro vstupy hello nebo zadejte formátovaný soubor oddělený čárkami hodnotu (CSV) obsahující hello testování hodnoty.

tootest pomocí RRS, z režimu zobrazení seznamu hello, zadejte příslušné hodnoty pro vstupy hello a klikněte na tlačítko **testu požadavků a odpovědí**. V toohello sloupec výstup hello left zobrazí výsledky předpovědi.

![Nasazení webové služby hello](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Klikněte na vaše BES tootest **Batch**. Na stránce test Batch hello pod váš vstup, klikněte na tlačítko Procházet a vyberte soubor CSV obsahující hodnoty odpovídající vzorku. Pokud nemáte soubor CSV a vytvořili experimentu prediktivní pomocí Machine Learning Studio, můžete stáhnout hello datové sady pro prediktivní experiment a používat ho.

toodownload hello datové sady, otevřete Machine Learning Studio. Otevřete prediktivní experiment a klikněte pravým tlačítkem na hello vstup pro experimentu. Hello místní nabídce vyberte **datovou sadu** a pak vyberte **Stáhnout**.

![Nasazení webové služby hello](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Klikněte na tlačítko **Test**. Hello stavu úlohy dávkového spuštění zobrazí toohello vpravo pod **testovací úlohy Batch**.

![Nasazení webové služby hello](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test hello web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

Na hello **konfigurace** stránky, můžete změnit popis hello, title, aktualizovat klíč účtu úložiště hello a povolit ukázková data pro webovou službu.

![Konfigurovat webovou službu hello](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Jakmile nasadíte hello webové služby, můžete:

* **Přístup k** přes rozhraní API hello webové služby.
* **Spravovat** přes Azure Machine Learning webový portál služeb nebo hello portál Azure classic.
* **Aktualizace** je-li změny modelu.

#### <a name="access-your-new-web-service"></a>Přístup k vaší nové webové služby
Jakmile nasadíte webovou službu z Machine Learning Studio, můžete odeslat data toohello služby a příjem odpovědí prostřednictvím kódu programu.

Hello **spotřebě** stránka obsahuje všechny informace hello potřebujete tooaccess webové služby. Například klíč hello rozhraní API je k dispozici služba toohello tooallow oprávnění přístupu.

Další informace o přístup k webové službě Machine Learning najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

#### <a name="manage-your-new-web-service"></a>Spravovat novou webovou službu
Můžete spravovat nový webový portál služby webové služby Machine Learning. Z hello [hlavní stránky portálu](https://services.azureml-test.net/), klikněte na tlačítko **webové služby**. Z hello webové stránky služby můžete odstranit nebo zkopírujte služby. toomonitor specifické služby, klikněte na tlačítko hello služby a potom klikněte na **řídicí panel**. úlohy batch toomonitor přidružené hello webové služby, klikněte na tlačítko **dávkové žádosti protokolu**.

### <a name="deploy-hello-predictive-experiment-as-a-classic-web-service"></a>Nasadit prediktivní experiment hello jako webovou službu Classic

Teď, když dostatečně připravený hello prediktivní experiment, můžete nasadit jako webovou službu Classic Azure. Pomocí hello webové služby, mohou uživatelé odesílat data tooyour modelu a modelu hello vrátí jeho předpovědi.

toodeploy vaše prediktivní experiment, klikněte na tlačítko **spustit** dole hello hello experimentovat plátno a potom klikněte na **nasazení webové služby**. Hello webové služby je nastavit a jsou umístěny v řídicím panelu hello webových služeb.

![Nasazení webové služby hello](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

#### <a name="test-your-classic-web-service"></a>Testování vaší Classic webové služby

Na portálu webové služby Machine Learning hello nebo Machine Learning Studio můžete testovat hello webové služby.

tootest hello Request Response webové služby, klikněte na tlačítko hello **Test** tlačítka na řídicí panel hello webové služby. Zobrazí se dialogové okno se zobrazí tooask můžete pro hello vstupní data pro službu hello. Jedná se o očekávanou hello vyhodnocování experimentu sloupce hello. Zadejte sadu dat a pak klikněte na tlačítko **OK**. v dolní části hello hello řídicího panelu se nezobrazí výsledky Hello generované hello webové služby.

Můžete kliknout na hello **Test** náhled tootest odkaz služby portálu hello webové služby Azure Machine Learning, jak je znázorněno dříve hello novou webovou část služby.

Klikněte na tlačítko tootest hello spuštění služby Batch, **testovací** preview odkaz. Na stránce test Batch hello pod váš vstup, klikněte na tlačítko Procházet a vyberte soubor CSV obsahující hodnoty odpovídající vzorku. Pokud nemáte soubor CSV a vytvořili experimentu prediktivní pomocí Machine Learning Studio, můžete stáhnout hello datové sady pro prediktivní experiment a používat ho.

![Testování hello webové služby](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

Na hello **konfigurace** stránky, můžete změnit hello zobrazovaný název služby hello a a popis. Hello název a popis se zobrazí v hello [portál Azure classic](http://manage.windowsazure.com/) kde budete spravovat webové služby.

Můžete zadat popis vstupních dat, výstupní data a webové služby parametry tak, že zadáte řetězec pro každý sloupec v části **vstupní schéma**, **výstupního schématu**, a **parametr webové služby**. Tyto popisy se používají v hello ukázkový kód dokumenty poskytnuté pro hello webovou službu.

Můžete povolit protokolování toodiagnose všechny chyby, které se zobrazuje, když je přístup k webové služby. Další informace najdete v tématu [povolení protokolování pro webové služby Machine Learning](machine-learning-web-services-logging.md).

![Konfigurovat webovou službu hello](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Můžete také nakonfigurovat hello koncové body pro webovou službu hello v hello webové služby Azure Machine Learning portálu podobné toohello postup uvedený výše v hello novou část webové služby. Hello možnosti se liší, můžete přidat nebo změnit popis služby hello, povolte protokolování a povolit ukázkových dat pro testování.

#### <a name="access-your-classic-web-service"></a>Přístup ke službě web Classic
Jakmile nasadíte webovou službu z Machine Learning Studio, můžete odeslat data toohello služby a příjem odpovědí prostřednictvím kódu programu.

řídicí panel Hello poskytuje všechny hello informace, které budete potřebovat tooaccess webové služby. Například klíč rozhraní API hello je zadané tooallow autorizovaný přístup toohello služby a stránkám nápovědy rozhraní API poskytované toohelp usnadňují psaní kódu.

Další informace o přístup k webové službě Machine Learning najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

#### <a name="manage-your-classic-web-service"></a>Správa webové služby Classic
Existují různé akce můžete provádět toomonitor webové služby. Můžete jej aktualizovat a odstranit ji. Můžete také přidat další koncové body tooa Classic webové služby v přidání toohello výchozí koncový bod vytvořený při jeho nasazení.

Další informace najdete v tématu [spravovat pracovní prostor služby Azure Machine Learning](machine-learning-manage-workspace.md) a [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix hello link and uncomment
For more information on how toomanage Azure Machine Learning web service endpoints using hello REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-hello-web-service"></a>Aktualizovat hello webové služby
Můžete provést změny tooyour webové služby, například aktualizace hello modelu s další Cvičná data a nasadit ho znovu přepsal hello původní webové služby.

tooupdate hello webové služby, otevřete hello původní prediktivní experiment používá toodeploy hello webové služby a proveďte upravitelné kopie kliknutím **uložit jako**. Provedené změny a pak klikněte na **nasazení webové služby**.

Protože jste nasadili tohoto experimentu před, zobrazí se výzva, pokud chcete toooverwrite (Classic webová služba) nebo existující službu hello aktualizace (novou webovou službu). Kliknutím na tlačítko **Ano** nebo **aktualizace** zastaví hello existující webovou službu a nasadí nový experiment prediktivní hello je nasazen na příslušné místo.

> [!NOTE]
> Pokud jste udělali změny konfigurace v hello původní webové služby, například zadáte nový zobrazovaný název nebo popis, budete potřebovat tooenter tyto hodnoty znovu.
> 
> 

Jednou z možností pro aktualizaci webové služby je tooretrain hello model prostřednictvím kódu programu. Další informace najdete v tématu o [programovém přeučení modelů Machine Learning](machine-learning-retrain-models-programmatically.md).

<!-- internal links -->
[Vytvoření experimentu školení]: #create-a-training-experiment
[Ji převést prediktivní experiment tooa]: #convert-the-training-experiment-to-a-predictive-experiment
[Nasadit jako webovou službu]: #deploy-it-as-a-web-service
[nové]: #deploy-the-predictive-experiment-as-a-new-Web-service
[classic]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
