---
title: "portál webové služby Azure Machine Learning hello aaaUse | Microsoft Docs"
description: "Spravovat přístup tooAzure Machine Learning pracovní prostory a nasadit a spravovat rozhraní API pro ML webové služby"
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: jhubbard
editor: cgronlun
ms.assetid: b62cf2ca-dd2a-4a83-bb54-469f948fb026
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: v-donglo
ms.openlocfilehash: 04b49027fc0ab227382b320310088bb66aafacc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-web-service-using-hello-azure-machine-learning-web-services-portal"></a>Správa webové služby pomocí portálu webové služby Azure Machine Learning hello
Machine Learning nové a Classic webové služby pomocí portálu Microsoft Azure Machine Learning webové služby hello lze spravovat. Vzhledem k tomu, že Classic webové služby a nové webové služby jsou založená na různých základní technologií, máte možnosti mírně lišit správy pro každý z nich.

Hello webové služby Machine Learning portálu můžete:

* Sledujte, jak je používán hello webové služby.
* Konfigurovat hello popis, aktualizujte hello klíče pro hello webové služby (pouze nové), aktualizace vašeho úložiště účet klíče (pouze nové), povolit protokolování, zakázání nebo povolení ukázková data.
* Odstraňte hello webové služby.
* Vytvoření, odstranění nebo aktualizaci fakturace plány (pouze nové).
* Přidání a odstranění koncových bodů (pouze klasické)

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="permissions-toomanage-new-resources-manager-based-web-services"></a>Oprávnění toomanage nového správce prostředků na základě webové služby

Nové webové služby jsou nasazeny jako prostředky Azure. Jako takový musí mít správná oprávnění toodeploy hello a spravovat nové webové služby.  toodeploy spravovat nové webové služby musí být přiřazena Přispěvatel nebo je nasazená role správce na hello předplatné toowhich hello webové služby. Pokud můžete vyzvat jiné uživatele tooa strojového učení pracovní prostor, musíte je přiřadit role Přispěvatel nebo správce tooa v předplatném hello před jejich nasadit nebo spravovat webové služby. 

Pokud uživatel hello nemá hello opravte oprávnění tooaccess prostředky portálu hello webové služby Azure Machine Learning, uživatelé obdrží hello následující chybě při pokusu o toodeploy webové služby:

*Nasazení webové služby se nezdařilo. Tento účet nemá dostatečný přístup toohello předplatné Azure, který obsahuje hello pracovního prostoru. V pořadí toohello pracovního prostoru pozvat toodeploy tooAzure webové služby, hello, musí být stejný účet a být přístup toohello předplatné Azure, který obsahuje hello pracovního prostoru.*

Další informace o vytváření pracovního prostoru najdete v tématu [vytvoření a sdílení pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).

Další informace o nastavení oprávnění přístupu najdete v tématu [zobrazení přiřazení přístupu pro uživatele a skupiny v hello portál Azure – ve verzi Public preview](../active-directory/role-based-access-control-manage-assignments.md).


## <a name="manage-new-web-services"></a>Správa nové webové služby
toomanage nové webové služby:

1. Přihlaste se toohello [Microsoft Azure Machine Learning webové služby](https://services.azureml.net/quickstart) portál, pomocí účtu Microsoft Azure - použít hello účet, který je spojen s hello předplatného Azure.
2. V nabídce hello, klikněte na tlačítko **webové služby**.

Zobrazí se seznam nasazených webových služeb pro vaše předplatné. 

toomanage webové služby, klikněte na tlačítko webové služby. Ze stránky webových služeb hello můžete:

* Klikněte na tlačítko hello webové služby toomanage ho.
* Klikněte na tlačítko hello fakturace plánování hello webové služby tooupdate ho.
* Odstraňte webovou službu.
* Zkopírujte webové služby a nasaďte ji tooanother oblast.

Klepnutím na webové služby, otevřete stránku rychlý start hello webové služby. Rychlý start stránku Hello webové služby má dvě možnosti nabídky, které vám umožňují toomanage webové služby:

* **Řídicí panel** -vám umožní tooview webové služby usage.
* **KONFIGURACE** – umožňuje tooadd popisný text, aktualizace hello klíč účtu úložiště hello přidružené k hello webové služby a povolit nebo zakázat ukázková data.

### <a name="monitoring-how-hello-web-service-is-being-used"></a>Monitorování, jak je používán hello webové služby
Klikněte na tlačítko hello **řídicí panel** kartě.

Z řídicího panelu hello můžete zobrazit celkové využití webové služby v časovém intervalu. Období tooview hello můžete vybrat z hello období rozevírací nabídce v pravé horní hello hello využití grafů. řídicí panel Hello zobrazuje hello následující informace:

* **Požadavky v čase** zobrazuje graf krok hello počet požadavků, které přes hello vybrané časové období. Může pomoct určit, zda dochází k špičky využití.
* **Požadavky požadavků a odpovědí** zobrazí hello celkový počet požadavků a odpovědí volání hello Služba obdržela přes hello vybrané časové období a kolik z nich se nezdařilo.
* **Průměrná doba výpočetní požadavků a odpovědí** zobrazí na průměrný čas hello potřeby tooexecute hello přijatých požadavků.
* **Požadavky služby batch** zobrazí celkový počet hello dávkových žádostí hello služby přijal přes hello vybrané časové období a kolik z nich se nezdařilo.
* **Průměrná latence úlohy** zobrazí na průměrný čas hello potřeby tooexecute hello přijatých požadavků.
* **Chyby** zobrazí hello agregační počet chyb, k nimž došlo na volání toohello webové služby.
* **Služby náklady** zobrazí hello poplatky za fakturační plán hello související se službou hello.

### <a name="configuring-hello-web-service"></a>Konfigurace hello webové služby
Klikněte na tlačítko hello **konfigurace** možnost nabídky.

Můžete aktualizovat hello následující vlastnosti:

* **Popis** vám umožní tooenter popis hello webové služby.
* **Název** vám umožní tooenter název hello webové služby
* **Klíče** vám umožní toorotate primární a sekundární klíče rozhraní API.
* **Klíč účtu úložiště** vám umožní tooupdate hello klíč pro účet úložiště hello přidružený změny hello webové služby. 
* **Povolit ukázková data** vám umožní tooprovide ukázková data, které můžete použít službu tootest hello požadavků a odpovědí. Pokud jste vytvořili hello webové služby v nástroji Machine Learning Studio, hello ukázková data jsou převzaty z dat hello vaší použité tootrain modelu. Pokud jste vytvořili hello služby prostřednictvím kódu programu hello dat je převzat ze hello příklad dat, která jste zadali jako součást balíčku JSON hello.

### <a name="managing-billing-plans"></a>Správa fakturace plány
Klikněte na tlačítko hello **plány** nabídky ze stránky rychlé spuštění služby webové hello. Můžete také kliknout na plán hello spojené s konkrétní toomanage webové služby, který plánování.

* **Nové** vám umožní toocreate nový plán.
* **Přidat nebo odebrat plán instance** vám umožní příliš "škálovat" existující tooadd kapacity plánu.
* **Upgrade nebo přechod na starší verzi** vám umožní příliš "škálovat" existující tooadd kapacity plánu.
* **Odstranit** vám umožní toodelete plánu.

Klikněte na plán tooview jeho řídicí panel. řídicí panel Hello vám použití snímků nebo se chystáte vybrané období. tooselect hello tooview časové období, klikněte na tlačítko hello **období** rozevírací nabídce v pravé horní hello řídicího panelu. 

řídicí panel plán Hello poskytuje hello následující informace:

* **Plánování popis** zobrazí informace o hello náklady a kapacity, které jsou přidružené k plánu hello.
* **Plánování použití** zobrazí počet hello transakce a výpočetní hodiny, které byly účtovat proti hello plánu.
* **Webové služby** zobrazí hello počet webové služby, které používají tento plán.
* **TOP webové služby pomocí volání** zobrazí hello horní čtyři webové služby, které jsou volání které připsány hello plánu.
* **Nejlepších webové služby podle hodin výpočetní** zobrazí hello horní čtyři webové služby, které používají výpočetní prostředky, které budou účtovat proti hello plánu.

## <a name="manage-classic-web-services"></a>Spravovat Classic webové služby
> [!NOTE]
> Hello postupy v této části jsou relevantní toomanaging Classic webové služby prostřednictvím portálu hello webové služby Azure Machine Learning. Informace o správě Classic webové služby prostřednictvím hello Machine Learning Studio a hello Azure classic portálu, najdete v tématu [spravovat pracovní prostor služby Azure Machine Learning](machine-learning-manage-workspace.md).
> 
> 

toomanage Classic webové služby:

1. Přihlaste se toohello [Microsoft Azure Machine Learning webové služby](https://services.azureml.net/quickstart) portál, pomocí účtu Microsoft Azure - použít hello účet, který je spojen s hello předplatného Azure.
2. V nabídce hello, klikněte na tlačítko **Classic webové služby**.

toomanage Classic webové služby, klikněte na tlačítko **Classic webové služby**. Ze stránky hello Classic webové služby můžete:

* Klikněte na tlačítko hello koncových bodů hello přidružené tooview webové služby.
* Odstraňte webovou službu.

Když spravujete Classic webové služby, můžete spravovat všechny koncové body hello samostatně. Když kliknete na webové stránce hello webové služby, otevře hello seznam koncových bodů, které přísluší službě hello. 

Na stránce koncový bod hello Classic webové služby můžete přidat a odstranit koncové body služby hello. Další informace o přidání koncové body, najdete v části [vytváření koncových bodů](machine-learning-create-endpoint.md).

Klikněte na jednu z hello koncové body tooopen stránku rychlý start hello webové služby. Na stránce Rychlý start hello existují dvě možnosti nabídky, které vám umožňují toomanage webové služby:

* **Řídicí panel** -vám umožní tooview webové služby usage.
* **KONFIGURACE** -vám umožní tooadd popisný text, zapněte Chyba přihlášení a odhlášení, aktualizace hello klíč účtu úložiště hello přidružené k hello webové služby a povolit nebo zakázat ukázková data.

### <a name="monitoring-how-hello-web-service-is-being-used"></a>Monitorování, jak je používán hello webové služby
Klikněte na tlačítko hello **řídicí panel** kartě.

Z řídicího panelu hello můžete zobrazit celkové využití webové služby v časovém intervalu. Období tooview hello můžete vybrat z hello období rozevírací nabídce v pravé horní hello hello využití grafů. řídicí panel Hello zobrazuje hello následující informace:

* **Požadavky v čase** zobrazuje graf krok hello počet požadavků, které přes hello vybrané časové období. Může pomoct určit, zda dochází k špičky využití.
* **Požadavky požadavků a odpovědí** zobrazí hello celkový počet požadavků a odpovědí volání hello Služba obdržela přes hello vybrané časové období a kolik z nich se nezdařilo.
* **Průměrná doba výpočetní požadavků a odpovědí** zobrazí na průměrný čas hello potřeby tooexecute hello přijatých požadavků.
* **Požadavky služby batch** zobrazí celkový počet hello dávkových žádostí hello služby přijal přes hello vybrané časové období a kolik z nich se nezdařilo.
* **Průměrná latence úlohy** zobrazí na průměrný čas hello potřeby tooexecute hello přijatých požadavků.
* **Chyby** zobrazí hello agregační počet chyb, k nimž došlo na volání toohello webové služby.
* **Služby náklady** zobrazí hello poplatky za fakturační plán hello související se službou hello.

### <a name="configuring-hello-web-service"></a>Konfigurace hello webové služby
Klikněte na tlačítko hello **konfigurace** možnost nabídky.

Můžete aktualizovat hello následující vlastnosti:

* **Popis** vám umožní tooenter popis hello webové služby. Popis je povinné pole.
* **Protokolování** vám umožní tooenable nebo zakázat protokolování na koncovém bodu hello chyb. Další informace o protokolování naleznete v tématu Povolení [protokolování pro webové služby Machine Learning](machine-learning-web-services-logging.md).
* **Povolit ukázková data** vám umožní tooprovide ukázková data, které můžete použít službu tootest hello požadavků a odpovědí. Pokud jste vytvořili hello webové služby v nástroji Machine Learning Studio, hello ukázková data jsou převzaty z dat hello vaší použité tootrain modelu. Pokud jste vytvořili hello služby prostřednictvím kódu programu hello dat je převzat ze hello příklad dat, která jste zadali jako součást balíčku JSON hello.

## <a name="grant-or-suspend-access-tooweb-services-for-users-in-hello-portal"></a>Udělit nebo pozastavení služby tooWeb přístupu pro uživatele portálu hello
Hello portál Azure classic můžete povolit nebo odepřít přístup uživatelům toospecific.

### <a name="access-for-users-of-new-web-services"></a>Přístup pro uživatele nové webové služby
tooenable ostatní uživatelé toowork s vaší webové služby v portálu hello webové služby Azure Machine Learning, musíte je přidat jako správci co ve vašem předplatném Azure.

Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) pomocí Microsoft Azure účet – použijte hello účet, který je spojen s hello předplatného Azure.

1. V navigačním podokně hello, klikněte na tlačítko **nastavení**, pak klikněte na tlačítko **správci**.
2. V dolní části hello hello okna, klikněte na tlačítko **přidat**. 
3. V dialogovém okně Přidat A SPOLUSPRÁVCE hello zadejte e-mailovou adresu hello hello osoby, které chcete používat jako spolusprávce a pak vyberte hello předplatné, které chcete hello spolusprávcem tooaccess tooadd.
4. Klikněte na **Uložit**.

### <a name="access-for-users-of-classic-web-services"></a>Přístup pro uživatele Classic webových služeb
toomanage pracovního prostoru:

Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) pomocí Microsoft Azure účet – použijte hello účet, který je spojen s hello předplatného Azure.

1. V panelu služby Microsoft Azure hello, klikněte na **MACHINE LEARNING**.
2. Klikněte na pracovní prostor hello chcete toomanage.
3. Klikněte na tlačítko hello **konfigurace** kartě.

Na kartě Konfigurace hello, je možné pozastavit pracovní prostor Machine Learning toohello přístup kliknutím **ODEPŘÍT**. Uživatelé již nebude možné tooopen hello prostoru v nástroji Machine Learning Studio. toorestore přístup, klikněte na tlačítko **povolit**.

toospecific uživatelé:

toomanage další účty, kteří mají přístup toohello prostoru v nástroji Machine Learning Studio, klikněte na tlačítko **přihlášení tooML Studio** v hello **řídicí panel** kartě. Otevře se hello prostoru v nástroji Machine Learning Studio. Zde klikněte na tlačítko hello **nastavení** kartu a potom **uživatelé**. Můžete kliknout na **POZVAT uživatele více** toogive uživatelé přistupovat toohello prostoru, nebo vyberte uživatele a klikněte na **odebrat**.

> [!NOTE]
> Hello **přihlášení tooML Studio** odkaz otevře Machine Learning Studio pomocí hello Account Microsoft jste aktuálně přihlášeni. Hello Account Microsoft používá toosign v toohello Azure classic portálu toocreate pracovního prostoru nemá automaticky tooopen oprávnění tohoto pracovního prostoru. tooopen pracovního prostoru, musíte být přihlášeni toohello Account Microsoft, který byl definován jako vlastník hello hello pracovního prostoru, nebo musíte tooreceive Pozvánka z pracovního prostoru hello vlastníka toojoin hello.
> 
> 

