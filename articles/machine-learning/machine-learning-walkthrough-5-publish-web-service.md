---
title: "Krok 5: Nasazení webové službě Machine Learning hello | Microsoft Docs"
description: "Krok 5 hello vývoj prediktivního řešení návod: nasazení prediktivní experiment v nástroji Machine Learning Studio jako webovou službu."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 3fca74a3-c44b-4583-a218-c14c46ee5338
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 76391010972ed1450bbda8bfb2352c7b22b51ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-5-deploy-hello-azure-machine-learning-web-service"></a>Návod krok 5: Nasazení webové služby Azure Machine Learning hello
Toto je pátý krok hello hello průvodce [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)
3. [Vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Natrénování a vyhodnocení modelů hello](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. **Nasazení webové služby hello**
6. [Přístup hello webové služby](machine-learning-walkthrough-6-access-web-service.md)

- - -
toogive jiné prvního toouse hello prediktivního modelu jsme jste vytvořili v tomto návodu, jsme můžete nasadit jako webovou službu v Azure.

Až toothis bodu jsme jste byla experimentování se cvičení naše modelu. Ale hello nasazení služby se už bude toodo školení – přechází nové předpovědi toogenerate ve vyhodnocování vstup hello uživatele na základě naše modelu. Takže vytvoříme toodo některé přípravy tooconvert, to experimentovat z ***školení*** experimentovat tooa ***prediktivní*** experiment. 

Jedná se o proces třech krocích:  

1. Odeberte jeden z modelů hello
2. Převést hello *výukový experiment* vytvořili jsme do *prediktivní experiment*
3. Nasadit prediktivní experiment hello jako webovou službu

## <a name="remove-one-of-hello-models"></a>Odeberte jeden z modelů hello

Nejdřív potřebujeme tootrim tohoto experimentu dolů trochu. V současné době máme dva různé modely v experimentu hello, ale chceme jenom jeden model toouse, když jsme nasadit jako webovou službu.  

Řekněme, že jsme jste se rozhodli, že hello boosted stromu model má lepší než hello SVM model. První věc toodo hello je odebrat hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu a hello moduly, které se používaly k cvičení ho. Může být vhodné toomake kopii hello experimentu nejprve kliknutím **uložit jako** dole hello hello experimentovat plátno.

Potřebujeme toodelete hello následující moduly:  

* [Two-Class Support Vector Machine][two-class-support-vector-machine]
* [Cvičení modelu] [ train-model] a [Score Model] [ score-model] moduly, které byly připojené tooit
* [Normalizuje Data] [ normalize-data] (obou z nich)
* [Vyhodnocení modelu] [ evaluate-model] (protože jsme hotovi, vyhodnocení modelů hello)

Vyberte každý modul a stisknout klávesu odstranit hello nebo modul hello klikněte pravým tlačítkem a vyberte **odstranit**. 

![Odebrat hello SVM modelu][3a]

Naše modelu by teď měl vypadat přibližně takto:

![Odebrat hello SVM modelu][3]

Teď máme připraven toodeploy to modelu pomocí hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].

## <a name="convert-hello-training-experiment-tooa-predictive-experiment"></a>Převést hello výukový experiment tooa prediktivní experiment

tooget to model připravené pro nasazení, budeme potřebovat tooconvert cvičení prediktivní experiment tooa experimentu. To zahrnuje tři kroky:

1. Uložit model hello jsme natrénovali a potom můžete nahradit naše školení moduly
2. Trim hello experimentu tooremove moduly, které byly jenom potřebné pro školení
3. Zadejte, kde hello webová služba bude akceptovat vstup a kde vygeneruje výstup hello

Jsme může udělat to ručně, ale naštěstí všechny tři kroky, můžete provést kliknutím na tlačítko **nastavit webové služby** dole hello plátno experimentu hello (a výběrem hello **prediktivní webové služby** možnost).

> [!TIP]
> Pokud chcete podrobnosti na co se stane, když je převést školení tooa experiment prediktivní experiment, najdete v části [jak tooprepare vašeho modelu pro nasazení v nástroji Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Když kliknete na tlačítko **nastavit webové služby**, dojde k několika změnám:

* Hello trained model je převeden tooa jeden **Trained Model** modulu a uložené v hello modulu palety toohello nalevo od hello Experimentujte plátno (najdete ji v části **Trénované modely**)
* Moduly, které jste použili pro školení jsou odebrány; konkrétně:
  * [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree]
  * [Train Model][train-model]
  * [Rozdělení dat][split]
  * druhý Hello [spustit skript jazyka R] [ execute-r-script] modul, který byl použit pro testovací data
* Hello uložit trained model je přidat zpět do experimentu hello
* **Webové služby vstup** a **webové služby výstup** moduly jsou přidány (ty identifikují kde hello uživatelská data budou zadejte hello modelu, a jaká data se vrátí, při přístupu k hello webová služba)

> [!NOTE]
> Uvidíte, že hello experiment uloží ve dvou částech na kartách, které byly přidány hello horní části plátna experimentu hello. Hello původní výukový experiment je kartě hello **výukový experiment**, a nově vytvořený prediktivní experiment hello je pod **prediktivní experiment**. Prediktivní experiment Hello je hello jeden, který jsme budete nasadit jako webovou službu.

Potřebujeme tootake další krok se tento konkrétní experimentu.
Jsme přidali dvě [spustit skript jazyka R] [ execute-r-script] tooprovide moduly vyvážení funkce toohello data. Proto jsme vyjměte tyto moduly v hello konečné modelu, který byl právě efektu potřebnou pro trénování a testování.
Machine Learning Studio odebrat jeden [spustit skript jazyka R] [ execute-r-script] modulu odebrán hello [rozdělení] [ split] modulu. Nyní jsme můžete odebrat hello navzájem a připojení [Metadata Editor] [ metadata-editor] přímo příliš[Score Model][score-model].    

Naše experiment by měl nyní vypadat takto:  

![Vyhodnocování hello trained model][4]  

> [!NOTE]
> Asi vás zajímá, proč jsme left hello UCI němčina platební karty dat datové sady v prediktivní experiment hello. Hello služby, který se bude tooscore hello uživatelská data, není hello původní datové sady, takže proč nechte hello původní datové sady v modelu hello?
> 
> Je PRAVDA, že není nutné hello služby hello původní data platební karty. Ale potřebuje hello schématu pro tato data, což zahrnuje informace, jako je počet sloupců, jsou a sloupce, které jsou číselná. Tato informace o schématu je nezbytné toointerpret hello uživatelská data. Jsme ponechat tyto součásti připojení tak, aby hello vyhodnocování modul má schéma datové sady hello když je spuštěna služba hello. Hello data se nepoužívá, právě hello schématu.  
> 
> 

Spusťte experiment hello jednou (klikněte na tlačítko **spustit**.) Pokud chcete přesto funguje tooverify, který hello model, klikněte na tlačítko hello výstup hello [Score Model] [ score-model] modul a vyberte **zobrazení výsledků**. Uvidíte, že se zobrazí původní data hello, spolu s hello úvěrové riziko hodnotu ("popisky vyhodnocení") a hello vyhodnocování hodnotu pravděpodobnosti ("skóre pro Magnitudu Pravděpodobnostech".) 

## <a name="deploy-hello-web-service"></a>Nasazení webové služby hello
Hello experimentu můžete nasadit jako buď Classic webovou službu nebo jako novou webovou službu, která je založená na Azure Resource Manager.

### <a name="deploy-as-a-classic-web-service"></a>Nasadit jako webovou službu Classic
Klikněte na tlačítko toodeploy klasický webové služby, které jsou odvozené z našich experimentu **nasazení webové služby** níže hello plátno a vyberte **nasazení webové služby [Classic]**. Machine Learning Studio nasadí hello experimentu jako webovou službu a přejdete toohello řídicí panel pro tuto webovou službu. Z této stránky můžete se vrátit toohello experimentu (**zobrazení snímku** nebo **zobrazit nejnovější**) a spusťte jednoduchá testovací hello webové služby (najdete v části **testování hello webové služby** níže). Je zde také informace pro vytváření aplikací, kteří mohou přistupovat k hello webové služby (Další informace o, v dalším kroku hello tohoto názorného postupu).

![Řídicí panel webové služby][6]

Můžete nakonfigurovat službu hello kliknutím hello **konfigurace** kartě. Zde můžete upravit název služby hello (je název experimentu dané hello ve výchozím nastavení) a zadejte jeho popis. Můžete také udělit další popisný popisky pro hello vstupní a výstupní data.  

![Konfigurovat webovou službu hello][5]  

### <a name="deploy-as-a-new-web-service"></a>Nasadit jako novou webovou službu

> [!NOTE] 
> toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné, které nasazujete hello webové služby. Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md). 

toodeploy novou webovou službu získané z našich experimentu:

1. Klikněte na tlačítko **nasazení webové služby** níže hello plátno a vyberte **nasazení [nové] webové služby**. Machine Learning Studio přenosy, webové služby Azure Machine Learning toohello **nasazení experimentu** stránky.

2. Zadejte název pro hello webovou službu. 

3. Pro **cena plán**, můžete vybrat existující cenový plán, nebo vyberte možnost "vytvořit nový" a udělte hello nový plán název a vyberte hello měsíční možnost plánu. plán Hello, že vrstvy výchozí toohello plány pro vaši oblast výchozí a webové služby je nasazené toothat oblast.

4. Klikněte na tlačítko **nasazení**.

Po několika minutách hello **rychlý Start** otevře se stránka pro webovou službu.

Můžete nakonfigurovat službu hello kliknutím hello **konfigurace** kartě. Zde můžete upravit hello služby title a zadejte jeho popis. 

tootest hello webové služby, klikněte na tlačítko hello **Test** karta (najdete v části **testování hello webové služby** níže). Informace o vytváření aplikací, kteří mohou přistupovat k hello webové služby, klikněte na tlačítko hello **spotřebě** karta (hello další krok v tomto návodu přejde do více podrobností).

> [!TIP]
> Poté, co nasadíte ji můžete aktualizovat hello webové služby. Například pokud chcete toochange modelu, pak můžete upravit hello výukový experiment, upravit parametry modelu hello a klikněte na tlačítko **nasazení webové služby**, vyberete **nasazení webové služby [Classic]** nebo  **Nasazení webové služby [nové]**. Při nasazení hello experiment, nahradí hello webové službě, teď pomocí aktualizovaném modelu.  
> 
> 

## <a name="test-hello-web-service"></a>Testování hello webové služby

Při přístupu k webové službě hello hello uživatel zadá prostřednictvím hello **webové služby vstup** modulu, kde úspěšně prošel toohello [Score Model] [ score-model] modulu a skóre. Hello způsob, jakým jsme zřídili hello prediktivní experiment, hello modelu očekává data hello stejný formát jako hello původní úvěrové riziko datové sady.
Hello budou vráceny výsledky toohello uživatele z hello webové služby prostřednictvím hello **webové služby výstup** modulu.

> [!TIP]
> způsob Hello máme hello prediktivní experiment nakonfigurované, hello celý výsledkem hello [Score Model] [ score-model] modulu jsou vráceny. To zahrnuje všechny vstupní data hello plus hello úvěrové riziko hodnotu a hello vyhodnocování pravděpodobnosti. Ale něco jiného může vrátit, pokud chcete – například může vrátit právě hello úvěrové riziko hodnotu. toodo tento, insert [sloupce projektu] [ project-columns] modulu mezi [Score Model] [ score-model] a hello **webové služby výstup** tooeliminate sloupce, které nechcete hello tooreturn webové služby. 
> 
> 

Můžete otestovat Classic webové služby buď v **Machine Learning Studio** nebo v hello **webové služby Azure Machine Learning** portálu.
Novou webovou službu můžete otestovat pouze v hello **webové služby Machine Learning** portálu.

> [!TIP]
> Při testování portálu hello webové služby Azure Machine Learning, může mít portál hello vytvoření ukázkových dat, které můžete použít službu tootest hello požadavků a odpovědí. Na hello **konfigurace** vyberte "Ano" pro **ukázkových dat povolené?**. Když otevřete kartu hello požadavků a odpovědí na hello **Test** stránce portálu hello vyplní ukázková data přijatá z hello původní úvěrové riziko datové sady.

### <a name="test-a-classic-web-service"></a>Testování Classic webové služby

Webová služba Classic můžete otestovat v nástroji Machine Learning Studio nebo hello webové služby Machine Learning portálu. 

#### <a name="test-in-machine-learning-studio"></a>Testování v nástroji Machine Learning Studio

1. Na hello **řídicí panel** stránky pro webovou službu hello, klikněte na tlačítko hello **Test** tlačítko pod **výchozí koncový bod**. Zobrazí se dialogové okno se zobrazí a se vás zeptá na hello vstupní data pro službu hello. Tyto jsou hello stejné sloupce, které se zobrazovaly v hello původní úvěrové riziko datové sady.  

2. Zadejte sadu dat a pak klikněte na tlačítko **OK**. 

#### <a name="test-in-hello-machine-learning-web-services-portal"></a>Test webové služby Machine Learning portálu hello

1. Na hello **řídicí panel** stránky pro webovou službu hello, klikněte na tlačítko hello **testovací preview** v části **výchozí koncový bod**. Hello zkušební stránku hello webové služby Azure Machine Learning portálu pro koncový bod webové služby hello otevře a zobrazí výzvu k hello vstupní data pro službu hello. Tyto jsou hello stejné sloupce, které se zobrazovaly v hello původní úvěrové riziko datové sady.

2. Klikněte na tlačítko **testování požadavků a odpovědí**. 

### <a name="test-a-new-web-service"></a>Otestovat novou webovou službu

Pouze na portál hello webové služby Machine Learning můžete testovat novou webovou službu.

1. V hello [webové služby Azure Machine Learning](https://services.azureml.net/quickstart) portálu klikněte na **Test** hello horní části stránky hello. Hello **Test** otevře se stránka a můžete vstupní data pro službu hello. vstupní pole Hello zobrazí odpovídají toohello sloupce, které se zobrazovaly v hello původní úvěrové riziko datové sady. 

2. Zadejte sadu dat a pak klikněte na tlačítko **testu požadavků a odpovědí**.

Hello výsledky testu hello zobrazí na pravé straně hello hello stránku hello výstupního sloupce. 


## <a name="manage-hello-web-service"></a>Spravovat hello webové služby

### <a name="manage-a-classic-web-service-in-hello-azure-classic-portal"></a>Správa webové služby Classic v hello portál Azure classic

Když máte ukázku nasazenou webovou službu Classic, můžete je spravovat z hello [portál Azure classic](https://manage.windowsazure.com).

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com)
2. V panelu služby Microsoft Azure hello, klikněte na tlačítko **MACHINE LEARNING**
3. Klikněte na pracovní prostor
4. Klikněte na tlačítko hello **webové služby** karta
5. Klikněte na tlačítko hello webové služby, kterou jsme vytvořili
6. Klikněte na tlačítko "Výchozí" hello koncový bod

Zde lze provádět akce, jako jsou monitorování, jak je to hello webové služby a provést vylepšení výkonu změnou, kolik souběžných volání hello služby může zpracovat.

Další podrobnosti najdete v tématu:

* [Vytváření koncových bodů](machine-learning-create-endpoint.md)
* [Škálování webové služby](machine-learning-scaling-webservice.md)

### <a name="manage-a-classic-or-new-web-service-in-hello-azure-machine-learning-web-services-portal"></a>Spravovat na klasickém nebo novou webovou službu portálu hello webové služby Azure Machine Learning

Po nasazení webové služby, zda Classic nebo nové, můžete je spravovat z hello [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) portálu.

výkon hello toomonitor webové služby:

1. Přihlaste se toohello [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) portálu
2. Klikněte na tlačítko **webové služby**
3. Klikněte na webovou službu
4. Klikněte na tlačítko hello **řídicí panel**

- - -
**Další krok: [přístup k webové službě hello](machine-learning-walkthrough-6-access-web-service.md)**

[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[3a]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3a.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
