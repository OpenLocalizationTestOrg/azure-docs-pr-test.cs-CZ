---
title: "aaaManage pracovní prostor Machine Learning | Microsoft Docs"
description: "Spravovat přístup tooAzure Machine Learning pracovní prostory a nasadit a spravovat rozhraní API pro ML webové služby"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 7bd378d82aa46f4340ca974c7dc5a612c089cdca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Správa pracovního prostoru Azure Machine Learning

> [!NOTE]
> Informace týkající se správy webové služby hello webové služby Machine Learning portálu najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).
> 
> 

Můžete spravovat pracovní prostory Machine Learning v portálu Azure buď hello nebo hello portál Azure classic.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="use-hello-azure-portal"></a>Hello použití portálu Azure

toomanage pracovního prostoru v hello portálu Azure:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/) pomocí účtu správce předplatného Azure.
2. Hello vyhledávacího pole v horní části hello hello stránky, zadejte "počítač pracovní prostory learning" a potom vyberte **Machine Learning pracovních prostorů**.
3. Klikněte na pracovní prostor hello chcete toomanage.

Kromě informací o správu toohello standardní prostředku a možnosti, které jsou k dispozici, můžete:

- Zobrazení **vlastnosti** – Tato stránka zobrazuje informace o hello prostoru a prostředků, a můžete změnit hello předplatné a skupina prostředků, které tento pracovní prostor je propojená s.
- **Nové synchronizace klíčů k úložišti** -hello prostoru udržuje účtu úložiště toohello klíče. Pokud účet úložiště hello změny klíčů, pak můžete kliknout na **nové synchronizace klíče** toosynchronize hello klíče s hello prostoru.

toomanage hello webové služby přidružené k tomuto pracovnímu prostoru pomocí portálu hello webové služby Machine Learning. V tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md) úplné informace.

> [!NOTE]
> toodeploy spravovat nové webové služby musí být přiřazena Přispěvatel nebo je nasazená role správce na hello předplatné toowhich hello webové služby. Pokud můžete vyzvat jiné uživatele tooa strojového učení pracovní prostor, musíte je přiřadit role Přispěvatel nebo správce tooa v předplatném hello před jejich nasadit nebo spravovat webové služby. 
> 
>Další informace o nastavení oprávnění přístupu najdete v tématu [zobrazení přiřazení přístupu pro uživatele a skupiny v hello portál Azure – ve verzi Public preview](../active-directory/role-based-access-control-manage-assignments.md).

## <a name="use-hello-azure-classic-portal"></a>Hello použijte portál Azure classic

Hello portál Azure classic můžete spravovat vaše Machine Learning pracovních prostorů můžete:

* Sledování využití prostoru hello
* Konfigurace pracovního prostoru tooallow hello nebo odepřít přístup
* Správa webové služby vytvořené v pracovním prostoru hello
* Odstranit pracovní prostor hello

Kromě toho hello karty řídicí panel poskytuje přehled vaší využití prostoru a rychlého přehledu vašich údajů pracovního prostoru.  

> [!TIP]
> V Azure Machine Learning Studio, na hello **webové služby** kartě, můžete přidat, aktualizovat nebo odstranit Machine Learning webové služby.
> 
> 

toomanage pracovního prostoru v hello portál Azure classic:

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) pomocí Microsoft Azure účet – použijte hello účet, který je spojen s hello předplatného Azure.
2. V panelu služby Microsoft Azure hello, klikněte na **MACHINE LEARNING**.
3. Klikněte na pracovní prostor hello chcete toomanage.

stránka pracovní prostor Hello má tři karty:

* **Řídicí panel** -umožňuje využití prostoru tooview informace
* **KONFIGURACE** -vám umožní toomanage přístup toohello prostoru
* **WEBOVÉ služby** -vám umožní toomanage webové služby, které byly publikovány z tohoto pracovního prostoru

### <a name="toomonitor-how-hello-workspace-is-being-used"></a>toomonitor využití prostoru hello
Klikněte na tlačítko hello **řídicí panel** kartě.

Z řídicího panelu hello můžete zobrazit celkový použití pracovního prostoru a získat rychlý přehled informací pracovního prostoru.

* Hello **výpočetní** graf znázorňuje hello výpočetní prostředky používá hello prostoru. Můžete změnit toodisplay zobrazení hello relativní nebo absolutní hodnoty a můžete změnit časový rámec hello zobrazí v grafu hello.
* **Přehled využití** zobrazí používá hello prostoru úložiště Azure.
* **Rychlého přehledu** poskytuje souhrnné informace o pracovním prostoru a užitečné odkazy.

> [!NOTE]
> Hello **přihlášení tooML Studio** odkaz otevře Machine Learning Studio pomocí hello Account Microsoft jste aktuálně přihlášeni. Hello Account Microsoft používá toosign v toohello Azure classic portálu toocreate pracovního prostoru nemá automaticky tooopen oprávnění tohoto pracovního prostoru. tooopen pracovního prostoru, musíte být přihlášeni toohello Account Microsoft, který byl definován jako vlastník hello hello pracovního prostoru, nebo musíte tooreceive Pozvánka z pracovního prostoru hello vlastníka toojoin hello.
> 
> 

### <a name="toogrant-or-suspend-access-for-users"></a>toogrant nebo pozastavit přístup pro uživatele
Klikněte na tlačítko hello **konfigurace** kartě.

Na kartě Konfigurace hello můžete:

* Pozastavení pracovního prostoru Machine Learning toohello přístup kliknutím na tlačítko zakázat. Uživatelé již nebude možné tooopen hello prostoru v nástroji Machine Learning Studio. toorestore přístup, klikněte na tlačítko Povolit.

toomanage další účty, kteří mají přístup toohello prostoru v nástroji Machine Learning Studio, klikněte na tlačítko **přihlášení tooML Studio** v hello **řídicí panel** karta (viz poznámka předcházející hello ohledně  **Sign-in tooML Studio**). Otevře se hello prostoru v nástroji Machine Learning Studio. Zde klikněte na tlačítko hello **nastavení** kartu a potom **uživatelé**. Můžete kliknout na **POZVAT uživatele více** toogive uživatelé přistupovat toohello prostoru, nebo vyberte uživatele a klikněte na **odebrat**.

### <a name="toomanage-web-services-in-this-workspace"></a>toomanage webových služeb v tomto pracovním prostoru
Klikněte na tlačítko hello **webové služby** kartě.

Zobrazí se seznam webových služeb publikována z tohoto pracovního prostoru.
toomanage webové služby, klikněte na název hello v stránku hello seznamu tooopen hello webové služby.

Webové služby může mít jeden nebo více koncové body definované.

* Můžete definovat další koncové body v přidání toohello "Výchozí" koncový bod. tooadd hello koncový bod, klikněte na tlačítko **spravovat koncové body** dolnímu hello hello řídicí panel tooopen hello webové služby Azure Machine Learning portálu.
* toodelete koncový bod (nejde odstranit koncový bod "Výchozí" hello), zaškrtněte políčko hello od začátku hello hello koncový bod řádku a klikněte na tlačítko **odstranit**. Tím se odebere koncový bod hello z hello webové služby.
  
  > [!NOTE]
  > Pokud aplikace používá koncový bod webové služby hello při odstranění hello koncový bod, hello aplikace obdrží hello k chybě při příštím pokusí tooaccess hello služby.
  > 
  > 

Klikněte na název hello tooopen koncový bod webové služby je. 

Z řídicího panelu hello můžete zobrazit celkové využití webové služby v časovém intervalu. Období tooview hello můžete vybrat z hello období rozevírací nabídce v pravé horní hello hello využití grafů. řídicí panel Hello zobrazuje hello následující informace:

* **Požadavky v čase** zobrazuje graf krok hello počet požadavků, které přes hello vybrané časové období. Může pomoct určit, zda dochází k špičky využití.
* **Požadavky požadavků a odpovědí** zobrazí hello celkový počet požadavků a odpovědí volání hello Služba obdržela přes hello vybrané časové období a kolik z nich se nezdařilo.
* **Průměrná doba výpočetní požadavků a odpovědí** zobrazí na průměrný čas hello potřeby tooexecute hello přijatých požadavků.
* **Požadavky služby batch** zobrazí celkový počet hello dávkových žádostí hello služby přijal přes hello vybrané časové období a kolik z nich se nezdařilo.
* **Průměrná latence úlohy** zobrazí na průměrný čas hello potřeby tooexecute hello přijatých požadavků.
* **Chyby** zobrazí hello agregační počet chyb, k nimž došlo na volání toohello webové služby.
* **Služby náklady** zobrazí hello poplatky za fakturační plán hello související se službou hello.

Na stránce konfigurace hello můžete aktualizovat hello následující vlastnosti:

* **Popis** vám umožní tooenter popis hello webové služby. Popis je povinné pole.
* **Protokolování** vám umožní tooenable nebo zakázat protokolování na koncovém bodu hello chyb. Další informace o protokolování naleznete v tématu Povolení [protokolování pro webové služby Machine Learning](machine-learning-web-services-logging.md).
* **Povolit ukázková data** vám umožní tooprovide ukázková data, které můžete použít službu tootest hello požadavků a odpovědí. Pokud jste vytvořili hello webové služby v nástroji Machine Learning Studio, hello ukázková data jsou převzaty z dat hello vaší použité tootrain modelu. Pokud jste vytvořili hello služby prostřednictvím kódu programu hello dat je převzat ze hello příklad dat, která jste zadali jako součást balíčku JSON hello.

