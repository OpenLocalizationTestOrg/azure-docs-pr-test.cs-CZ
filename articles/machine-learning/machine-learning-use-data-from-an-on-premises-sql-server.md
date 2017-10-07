---
title: "aaaUse místní SQL Server v Azure Machine Learning | Microsoft Docs"
description: "Použijte data z analytics místní databázi systému SQL Server tooperform advanced pomocí Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 08e4610d-02b6-4071-aad7-a2340ad8e2ea
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: garye;krishnan
ms.openlocfilehash: c0e9908e296b97b31611ef0192744a59073acd80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Provádění pokročilých analýz dat místní databáze SQL Serveru pomocí služby Azure Machine Learning

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Často podnikům, které pracují s místními daty přeje tootake výhod hello škálování a flexibility hello cloudu pro jejich strojového učení úlohy. Ale budou nechcete, aby toodisrupt jejich aktuální podnikové procesy a pracovní postupy přesunutím jejich místní dat toohello v cloudu. Azure Machine Learning teď podporuje čtení dat z databáze SQL serveru na místě a pak školení a vyhodnocování model se tato data. Už máte toomanually kopii a synchronizaci dat hello mezi hello cloudové a místní server. Místo toho hello **importovat Data** modulu v Azure Machine Learning Studio nyní může načíst přímo z vaší místní databáze SQL serveru pro váš školení a vyhodnocování úlohy.

Tento článek obsahuje přehled o tom, jak tooingress místní data SQL serveru do Azure Machine Learning. Předpokládá, že jste obeznámeni s koncepty Azure Machine Learning pracovních prostorů, moduly, datové sady, experimenty, *atd*.

> [!NOTE]
> Tato funkce není k dispozici pro bezplatné pracovní prostory. Další informace o cenách Machine Learning a úrovně najdete v tématu [Azure Machine Learning – ceny](https://azure.microsoft.com/pricing/details/machine-learning/).
>
>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="install-hello-microsoft-data-management-gateway"></a>Nainstalujte hello Brána pro správu dat
tooaccess místní databázi systému SQL Server v Azure Machine Learning, potřebujete toodownload a instalace hello Brána pro správu dat společnosti Microsoft. Když konfigurujete připojení hello brány v nástroji Machine Learning Studio, máte možnost hello stáhnout a nainstalovat bránu hello pomocí hello **stahování a registrace brány dat** dialogové okno popsané dole.

Také můžete nainstalovat hello Brána pro správu dat předem stažení a spuštění hello MSI instalační balíček z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).
Vyberte nejnovější verzi hello výběr 32bitové nebo 64bitové vhodnou pro váš počítač. Hello MSI lze také použít tooupgrade existující brány pro správu dat toohello nejnovější verze, se všemi možnými nastavení zachovaná.

Hello brány má hello následující požadavky:

* verze operačních systémů Windows Hello podporována jsou Windows 7, Windows 8 nebo 8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.
* Hello doporučená konfigurace pro počítač brány hello je alespoň 2 GHz, 4 jádra, 8 GB paměti RAM a 80 GB místa na disku.
* Pokud hello hostitelský počítač přejde do režimu spánku, brána hello nebude odpovídat toodata požadavky. Proto konfigurovat příslušné napájení počítače hello před instalací hello brány. Pokud je počítač hello nakonfigurované toohibernate, instalace brány hello zobrazí zprávu.
* Protože aktivity kopírování dojde k určité frekvencí, následuje hello využití prostředků (procesor, paměť) na počítači hello také hello stejný vzor s ve špičce a doby nečinnosti. Využití prostředků závisí také výraznou na hello množství dat přesouvá. Pokud více úloh kopie jsou v průběhu, budete sledovat využití prostředků během špiček přejděte. Když je technicky dostatečná hello minimální konfigurace uvedené výše, můžete v závislosti na konkrétní zatížení toohave konfigurací s více prostředků než hello minimální konfigurace pro přesun dat.

Vezměte v úvahu následující hello při nastavování a používání Brána pro správu dat:

* Můžete nainstalovat pouze jedna instance brány pro správu dat v jednom počítači.
* Jedna brána můžete použít pro více zdrojů dat na místě.
* Více bran na různých počítačích toohello se můžete připojit stejný zdroj dat na místě.
* Nakonfigurovat bránu pouze jednoho pracovního prostoru v čase. V současné době brány se nedají sdílet mezi pracovních prostorů.
* Můžete nakonfigurovat více bran pro jednoho pracovního prostoru. Můžete například toouse bránu, která je připojená tooyour test zdroje dat během vývoje a bránu produkční až budete toooperationalize připraven.
* Hello brány není nutné toobe na stejný počítač jako zdroj dat hello hello. Ale zachování blíže toohello zdroj dat snižuje dobu hello pro zdroj dat toohello tooconnect brány hello. Doporučujeme nainstalovat hello bránu na počítači, který se liší od hello jednoho zdroje dat hello místního hostitele tak, aby hello brány a zdroj dat není pokouší pro prostředky.
* Pokud je již nainstalovaná ve vašem počítači obsluhující Power BI nebo Azure Data Factory scénáře, nainstalujte samostatnou bránu pro Azure Machine Learning na jiném počítači.

  > [!NOTE]
  > Brána pro správu dat a Power BI Gateway nelze spustit na hello stejný počítač.
  >
  >
* Musíte toouse hello Brána pro správu dat pro Azure Machine Learning, i když používáte Azure ExpressRoute pro další data. Zdroje dat by měly zpracovávat jako místní zdroj dat (který je za bránou firewall) i při použití ExpressRoute. Použijte hello Brána pro správu dat tooestablish připojení mezi Machine Learning a zdroj dat hello.

Podrobné informace o požadavky pro instalaci, kroky instalace a tipy k řešení potíží najdete v článku hello [Brána pro správu dat](../data-factory/data-factory-data-management-gateway.md).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Příchozí přenos dat z vaší místní databáze SQL serveru do Azure Machine Learning
V tomto návodu nastavit Brána pro správu dat v pracovním prostoru Azure Machine Learning, nakonfigurovat ji a potom číst data z databáze SQL serveru místní.

> [!TIP]
> Než začnete, zakažte blokování automaticky otevíraných oken v prohlížeči pro `studio.azureml.net`. Pokud používáte hello prohlížeč Google Chrome, stáhněte a nainstalujte jednu hello několik moduly plug-in k dispozici na webu Google Chrome WebStore [klikněte jednou rozšíření aplikace](https://chrome.google.com/webstore/search/clickonce?_category=extensions).
>
>

### <a name="step-1-create-a-gateway"></a>Krok 1: Vytvoření brány
prvním krokem Hello je toocreate a nastavit hello brány tooaccess místní databáze SQL.

1. Přihlaste se příliš[Azure Machine Learning Studio](https://studio.azureml.net/Home/) a vyberte hello pracovní prostor, který chcete toowork v.
2. Klikněte na tlačítko hello **nastavení** okně hello left a potom klikněte na hello **brány DATA GATEWAYS** karty v horní části hello.
3. Klikněte na tlačítko **novou BRÁNU dat** v hello dolní části obrazovky hello.

    ![Nová brána dat](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. V hello **novou bránu dat** dialogové okno, zadejte hello **název brány** a volitelně přidejte **popis**. Klikněte na šipku hello hello dolní pravém rohu toogo toohello další krok konfigurace hello.

    ![Zadejte název brány a popis](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. V hello stáhnout a zaregistrujte dialogovém okně brány dat, kopírování hello registrační klíč brány toohello schránky.

    ![Stáhněte si a registraci brány dat](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>Pokud jste ještě stáhli a nainstalovali hello Brána pro správu dat a pak klikněte na **Brána pro správu dat stažení**. Tento trvá toothe Microsoft Download Center kde můžete vybrat hello verze brány je nutné, ho stáhnout na stránce a nainstalujte ji. Podrobné informace o požadavky pro instalaci, kroky instalace a tipy k řešení potíží najdete v částech hello začátku článku hello [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).
7. Po instalaci brány hello hello Správce konfigurace brány pro správu dat se jim otevře a hello **registrace brány** se zobrazí dialogové okno. Vložení hello **registrační klíč brány** jste zkopírovali toohello schránky a klikněte na tlačítko **zaregistrovat**.
8. Pokud již máte nainstalovaná, spusťte Správce konfigurace brány pro správu dat hello. Klikněte na tlačítko **změnit klíč**, vložte **registrační klíč brány** jste zkopírovali toohello schránky v předchozím kroku hello a klikněte na tlačítko **OK**.
9. Po dokončení instalace hello hello **registrace brány** zobrazí dialog pro Microsoft Data Management Gateway Configuration Manager. Vložte hello registrační klíč brány, kterou jste zkopírovali do schránky hello v předchozím kroku a klikněte na tlačítko **zaregistrovat**.

    ![Registraci brány](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. Hello konfigurace brány je dokončená, jakmile hello následující hodnoty jsou nastaveny na hello **Domů** kartě v Microsoft Data Management Gateway Configuration Manager:

    * **Název brány** a **název Instance** jsou nastavené toohello název brány hello.
    * **Registrace** je nastaven příliš**registrovaná**.
    * **Stav** je nastaven příliš**Začínáme**.
    * Hello stavového řádku v hello dolní zobrazí **připojené tooData cloudové službě Brána pro správu** společně s zelená značka zaškrtnutí.

      ![Správce brány pro správu dat](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      Azure Machine Learning Studio získá také aktualizovat, po úspěšné registraci hello.

    ![Registrace brány úspěšné](media/machine-learning-use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. V hello **stáhnout a bránu dat zaregistrujte** dialogové okno, klikněte na tlačítko Nastavení zaškrtnutí toocomplete hello. Hello **nastavení** stránce se zobrazuje stav brány jako "Online". V pravém podokně hello zjistíte stav a další užitečné informace.

    ![Nastavení brány](media/machine-learning-use-data-from-an-on-premises-sql-server/gateway-status.png)
12. Přepněte na hello Správce konfigurace brány pro správu dat společnosti Microsoft toohello **certifikátu** certifikát hello kartě zadané na této kartě je pověření použité tooencrypt/dešifrování pro hello místní datové úložiště, které zadáte hello portálu. Tento certifikát je hello výchozí certifikát. Společnost Microsoft doporučuje změna tento tooyour vlastní certifikát, který zálohování v systému správy certifikátů. Klikněte na tlačítko **změnu** toouse svůj vlastní certifikát místo.

    ![Certifikát brány změn](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. (volitelné) Pokud chcete podrobné protokolování tooenable při řešení problémů s hello brány, v hello Správce konfigurace brány pro správu dat společnosti Microsoft přepínač toothe **diagnostiky** kartě a zkontrolujte hello **povolit podrobné protokolování pro účely odstraňování potíží** možnost. Hello informace o protokolování naleznete v hello Prohlížeč událostí systému Windows v rámci hello **protokoly aplikací a služeb**  - &gt; **Brána pro správu dat** uzlu. Můžete taky hello **diagnostiky** kartě tootest hello připojení tooan místní zdroje dat pomocí hello brány.

    ![Povolit podrobné protokolování](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

Dokončení procesu instalace brány hello v Azure Machine Learning.
Můžete se teď připravena toouse vaše místní data.

Můžete vytvořit a nastavit více bran v Studio pro každý pracovní prostor. Například můžete mít bránu pro produkční zdroje dat mají tooconnect tooyour testovací datové zdroje během vývoje a jinou bránu. Azure Machine Learning poskytuje flexibilitu tooset až více bran, v závislosti na vašem podnikovém prostředí. V současné době nelze sdílet brána mezi pracovními prostory a pouze jeden gateway se dá nainstalovat v jednom počítači. Další informace najdete v tématu [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-hello-gateway-tooread-data-from-an-on-premises-data-source"></a>Krok 2: Použijte hello brány tooread data z místního zdroje dat
Po nastavení hello brány, můžete přidat **importovat Data** modulu experimentu vstup hello data z databáze systému SQL Server hello místně.

1. V nástroji Machine Learning Studio, vyberte hello **EXPERIMENTY** , klikněte na **+ nový** v hello levém dolním rohu a vyberte **prázdný Experiment** (nebo vybrat některou z několika ukázka experimentů k dispozici).
2. Najděte a přetáhněte hello **importovat Data** modulu toohello experimentovat plátno.
3. Klikněte na tlačítko **uložit jako** níže hello plátno. Zadejte "Azure kurz strojového učení místní SQL Server" pro název experimentu hello, vyberte pracovní prostor a klikněte na tlačítko hello **OK** zaškrtnutí.

   ![Uložte s novým názvem experimentu](media/machine-learning-use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. Klikněte na tlačítko hello **importovat Data** tooselect modulu, potom v **vlastnosti** toohello podokně napravo od plátna hello vyberte "Místní databáze SQL" v hello **zdroj dat** rozevírací seznam.
5. Vyberte hello **bránu Data gateway** instalaci a registraci. Jinou bránu můžete nastavit tak, že vyberete "(Přidat novou bránu dat...)".

   ![Vyberte bránu dat pro Import dat modul](media/machine-learning-use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. Zadejte hello SQL **název databázového serveru** a **název databáze**, společně s hello SQL **databázový dotaz** chcete tooexecute.
7. Klikněte na tlačítko **zadejte hodnoty** pod **uživatelské jméno a heslo** a zadejte své přihlašovací údaje databáze. Můžete použít integrované ověřování systému Windows nebo ověřování systému SQL Server v závislosti na konfiguraci vaší místní SQL Server.

   ![Zadejte přihlašovací údaje databáze](media/machine-learning-use-data-from-an-on-premises-sql-server/database-credentials.png)

   uvítací zprávu "hodnoty požadované" změny příliš "hodnoty sadu" s zelená značka zaškrtnutí. Potřebujete jenom tooenter hello pověření jednou pouze v případě změní informace o databázi hello nebo heslo. Azure Machine Learning používá hello certifikát, který jste zadali při instalaci hello brány tooencrypt hello pověření v cloudu hello. Azure nikdy ukládá místních přihlašovacích údajů bez šifrování.

   ![Import modulu vlastnosti dat](media/machine-learning-use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. Klikněte na tlačítko **spustit** toorun hello experimentu.

Po dokončení experimentu hello spuštěná, můžete vizualizovat data hello importovány z databáze hello kliknutím hello výstupní port modulu hello **importovat Data** modulu a výběrem **vizualizovat**.

Jakmile dokončíte vývoj experimentu, můžete nasadit a zprovoznit model. Pomocí hello spuštění služby Batch, data z databáze systému SQL Server hello místní nakonfigurované v hello **importovat Data** modul bude číst a používat pro vyhodnocování. I když můžete používat hello žádosti o služby odpovědi pro vyhodnocování místní data, společnost Microsoft doporučuje používat [doplněk aplikace Excel](machine-learning-excel-add-in-for-web-services.md) místo. V současné době zápisu tooan místní databázi systému SQL Server prostřednictvím **Export dat** není podporované buď v experimentů nebo publikované webové služby.
