---
title: "aaaGet začít s pracovní prostor služby Azure Log Analytics | Microsoft Docs"
description: "Pracovní prostor ve službě Log Analytics můžete zprovoznit během několika minut."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 508716de-72d3-4c06-9218-1ede631f23a6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/08/2017
ms.author: magoedte
ms.openlocfilehash: 442a9258a37ee79e8f0b45759ef24b5e3dae0130
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-a-log-analytics-workspace"></a>Začínáme s pracovním prostorem služby Log Analytics
Službu Azure Log Analytics, která vám usnadní vyhodnocování informací o provozu získaných z infrastruktury IT, můžete zprovoznit během několika minut. S použitím informací z tohoto článku můžete snadno začít zkoumat a analyzovat data, která *zdarma* shromáždíte, a pracovat s nimi.

Tento článek slouží jako úvod tooLog Analytics pomocí stručný kurz toowalk můžete prostřednictvím minimální nasazení v Azure tak, že budete moct začít používat službu hello. logický kontejner Hello ukládat data správy z Azure se nazývá pracovního prostoru. Po kontrole tyto informace a dokončit vlastní testování, můžete odebrat hello vyhodnocení prostoru. Tento článek je kurz, a proto se nezabývá obchodními požadavky, plánováním ani doporučeními ohledně architektury.

>[!NOTE]
>Pokud používáte hello cloudu Microsoft Azure Government, použijte [Azure Government monitorování a správu dokumentace](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#log-analytics) místo.

Tady je rychlý přehled tooget hello proces spuštění:

![diagram procesu](./media/log-analytics-get-started/onboard-oms.png)

## <a name="1-create-an-azure-account-and-sign-in"></a>1. Vytvoření účtu Azure a přihlášení

Pokud nemáte účet Azure, je třeba jeden toouse toocreate analýzy protokolů. Můžete si vytvořit [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup k veškerým službám Azure po dobu 30 dní.

### <a name="toocreate-a-free-account-and-sign-in"></a>toocreate bezplatný účet a přihlaste se
1. Postupujte podle pokynů hello v [vytvořit účet Azure zdarma](https://azure.microsoft.com/free/).
2. Přejděte toohello [portál Azure](https://portal.azure.com) a přihlaste se.

## <a name="2-create-a-workspace"></a>2. Vytvoření pracovního prostoru

dalším krokem Hello je toocreate pracovního prostoru.

1. V hello portálu Azure, hledání hello seznam služeb v hello Marketplace pro *analýzy protokolů*a potom vyberte **analýzy protokolů**.  
    ![Azure Portal](./media/log-analytics-get-started/log-analytics-portal.png)
2. Klikněte na tlačítko **vytvořit**, vyberte možnosti pro hello následující položky:
   * **Pracovní prostor OMS** – zadejte název pracovního prostoru.
   * **Předplatné** – Pokud máte více předplatných, vyberte ten, který chcete tooassociate s nový pracovní prostor hello hello.
   * **Skupina prostředků**
   * **Umístění**
   * **Cenová úroveň**  
       ![Rychlé vytvoření](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klikněte na tlačítko **OK** toosee seznam pracovní prostory.
4. V hello portálu Azure vyberte pracovní prostor toosee její podrobnosti.       
    ![Podrobnosti o pracovním prostoru](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         

## <a name="3-upgrade-workspace-toonew-log-search"></a>Hledání protokolů toonew 3 upgradu prostoru
Byla vydána nová dotazovací jazyk analýzy protokolů a v pořadí tootake jejich výhod, třeba tooconvert pracovního prostoru.  V případě hello oblasti, které pracovního prostoru je hostován v upgradu, měli byste vidět fialové banner napříč hello horní části pracovního prostoru pozvat jste tooconvert. Hello upgrade je zcela dobrovolná a nemá negativní vliv na prostředí práce analýzy protokolů a žádné řešení, které přidáte.  

Další informace o toounderstand hello výhody, požadavky a proces tooupgrade najdete v tématu [hledání protokolů toonew upgrade Azure Log Analytics](log-analytics-log-search-upgrade.md).  

## <a name="4-add-solutions-and-solution-offerings"></a>4. Přidávání řešení a nabídek řešení

V další fázi přidejte řešení pro správu a nabídky řešení. Řešení pro správu jsou kolekce pravidel pro logiku, vizualizace a získávání dat, které poskytují metriky zaměřené na oblast konkrétního problému oblast. Nabídka řešení je sada řešení pro správu.

Přidání prostoru tooyour řešení umožňuje analýzy protokolů toocollect různé typy dat z počítače, které jsou připojené tooyour prostoru pomocí agentů. Připojováním agentů se budeme zabývat později.

### <a name="tooadd-solutions-and-solution-offerings"></a>tooadd řešení a řešení nabídky

1. Na portálu Azure, klikněte na tlačítko **nový** a pak v hello **vyhledávání hello marketplace** zadejte **analýzy protokolů aktivity** a stiskněte klávesu ENTER.
2. V hello všechno vyberte **analýzy protokolů aktivity** a pak klikněte na **vytvořit**.  
    ![Activity Log Analytics](./media/log-analytics-get-started/activity-log-analytics.png)  
3. V hello *název řešení pro správu* okně vyberte pracovní prostor, které chcete tooassociate hello řešení pro správu.
4. Klikněte na možnost **Vytvořit**.  
    ![pracovní prostor řešení](./media/log-analytics-get-started/solution-workspace.png)  
5. Opakujte kroky 1 – 4 tooadd:
    - Hello **dodržování předpisů a zabezpečení** s řešeními antimalwarových hodnocení a zabezpečení a Audit hello nabídky služeb.
    - Hello **automatizace a řízení** služby nabízí hello Automation Hybrid Worker, sledování změn a řešení pro vyhodnocení aktualizací systému (také nazývané správy aktualizací). Když přidáte hello řešení nabídky, musíte vytvořit účet Automation.  
        ![Účet Automation](./media/log-analytics-get-started/automation-account.png)  
6. Můžete zobrazit řešení pro správu hello, které jste přidali tooyour prostoru přechodem příliš**analýzy protokolů** > **odběry** > ***název pracovního prostoru***  >  **Přehled**. Dlaždice pro řešení pro správu hello, které jste přidali, se zobrazí.  
    >[!NOTE]
    >Protože jsme se ještě nepřipojila prostoru toohello všechny agenty, se nezobrazí žádná data pro hello řešení, které jste přidali.  

    ![dlaždice řešení bez dat](./media/log-analytics-get-started/solutions-no-data.png)

## <a name="4-create-a-vm-and-onboard-an-agent"></a>4. Vytvoření virtuálního počítače a připojení agenta

V dalším kroku vytvoříte v Azure jednoduchý virtuální počítač. Po vytvoření virtuálního počítače, tooenable agenta OMS zařadit hello ho. Povolení hello agent začne shromažďování dat z hello virtuálních počítačů a odesílá data tooLog Analytics.

### <a name="toocreate-a-virtual-machine"></a>toocreate virtuálního počítače

- Postupujte podle pokynů hello v [vytvořit svůj první virtuální počítač Windows v hello portál Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md) a spusťte hello nového virtuálního počítače.

### <a name="connect-hello-virtual-machine-toolog-analytics"></a>Připojit virtuální počítač tooLog hello Analytics

- Postupujte podle pokynů hello v [tooLog virtuální počítače Azure připojit Analytics](log-analytics-azure-vm-extension.md) tooconnect hello virtuální počítač tooLog Analytics pomocí portálu Azure hello.

## <a name="6-view-and-act-on-data"></a>6. Zobrazování dat a práce s nimi

Dřív jste povolili hello analýzy protokolů aktivity řešení a nabídky hello zabezpečení a dodržování předpisů a automatizace & řízení služeb. V dalším kroku začneme s prohlížením dat shromažďovaných řešeními a výsledků v rámci prohledávání protokolů.

toostart, podívejte se na data, která se zobrazí z v rámci řešení. Pak se podívejte na některá z dostupných prohledávání protokolů. Protokol hledání umožňují toocombine a korelovat žádná data počítače z více zdrojů ve vašem prostředí. Další informace najdete v tématu [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) nebo pokud jste převedli prostoru toohello nový dotaz jazyka, najdete v části [principy protokolu vyhledá v analýzy protokolů](log-analytics-log-search-new.md). 

### <a name="tooview-antimalware-data"></a>tooview antimalwarových dat

1. V hello portálu Azure, přejděte příliš**analýzy protokolů** > ***pracovního prostoru***.
2. V okně hello vašeho pracovního prostoru v části **Obecné**, klikněte na tlačítko **přehled**.  
    ![Přehled](./media/log-analytics-get-started/overview.png)
3. Klikněte na tlačítko hello **antimalwarových Assessment** dlaždici. V tomto příkladu je vidět, že v jednom počítači je nainstalován program Windows Defender, ale jeho podpis je zastaralý.  
    ![Antimalware](./media/log-analytics-get-started/solution-antimalware.png)
4. V tomto příkladu v části **stav ochrany**, klikněte na tlačítko **podpis zastaralý** tooopen protokolu vyhledávání a zobrazení podrobností o hello počítačů, které mají zastaralá podpisy. V tomto příkladu, Všimněte si, že počítač hello se nazývá *getstarted*. Kdyby existovalo víc než jeden počítač s aktuální podpisy, by všechny se v hello hledání protokolů výsledky.  
    ![Zastaralý antimalware](./media/log-analytics-get-started/antimalware-search.png)

### <a name="tooview-security-and-audit-data"></a>tooview zabezpečení a Audit dat

1. V okně hello vašeho pracovního prostoru v části **Obecné**, klikněte na tlačítko **přehled**.  
2. Klikněte na tlačítko hello **zabezpečení a Audit** dlaždici. V tomto příkladu jsou vidět dva výrazné problémy: v jednom počítači chybí důležité aktualizace a ochrana jednoho počítače je nedostatečná.  
    ![Zabezpečení a audit](./media/log-analytics-get-started/security-audit.png)
3. V tomto příkladu v části **významné problémy**, klikněte na tlačítko **počítače s chybějícími důležitými aktualizacemi** tooopen protokolu vyhledávání a zobrazení podrobností o počítače s chybějícími důležitými aktualizacemi. V tomto příkladu chybí jedna důležitá aktualizace a 63 dalších aktualizací.  
    ![Prohledávání protokolů zabezpečení a auditu](./media/log-analytics-get-started/security-audit-log-search.png)

### <a name="tooview-and-act-on-system-update-data"></a>tooview a postupovat podle data aktualizace systému

1. V okně hello vašeho pracovního prostoru v části **Obecné**, klikněte na tlačítko **přehled**.  
2. Klikněte na tlačítko hello **vyhodnocení aktualizací systému** dlaždici. V tomto příkladu je vidět jeden počítač se systémem Windows s názvem *getstarted*, který vyžaduje důležité aktualizace, a jeden, který vyžaduje aktualizace definic.  
    ![Aktualizace systému](./media/log-analytics-get-started/system-updates.png)
3. V tomto příkladu v části **chybějící aktualizace**, klikněte na tlačítko **kritické aktualizace** tooopen protokolu vyhledávání a zobrazení podrobností o počítače s chybějícími důležitými aktualizacemi. V tomto příkladu chybí jedna důležitá aktualizace a jedna požadovaná aktualizace.  
    ![Prohledávání protokolů aktualizací systému](./media/log-analytics-get-started/system-updates-log-search.png)
4. Přejděte toohello [Operations Management Suite](http://microsoft.com/oms) webu a přihlaste se pomocí účtu Azure. Při přihlášení a Všimněte si, že informace o řešení hello je podobné toowhat jste uvidíte v hello portálu Azure.  
    ![Portál OMS](./media/log-analytics-get-started/oms-portal.png)
5. Klikněte na tlačítko hello **správy aktualizací** dlaždici.
6. Na řídicím panelu aktualizace správu hello Všimněte si, že informace o aktualizaci systému hello jsou podobné toohello aktualizace systému informace, které jste viděli v hello portál Azure. Ale hello **Spravovat nasazení aktualizací** je nové dlaždice. Klikněte na tlačítko hello **Spravovat nasazení aktualizací** dlaždici.  
    ![Dlaždice Správa aktualizací](./media/log-analytics-get-started/update-management.png)
7. Na hello **aktualizace nasazení** klikněte na tlačítko **přidat** toocreate *Hromadná postupná aktualizace*.  
    ![Nasazení aktualizace](./media/log-analytics-get-started/update-management-update-deployments.png)
8.  Na hello **nové nasazení aktualizace** stránky, zadejte název pro nasazení aktualizace hello, vyberte tooupdate počítače (v tomto příkladu *getstarted*), vyberte plán a pak klikněte na tlačítko **Uložit**.  
    ![Nové nasazení](./media/log-analytics-get-started/new-deployment.png)  
    Po uložení hello aktualizace nasazení se zobrazí hello naplánované aktualizace.  
    ![plánovaná aktualizace](./media/log-analytics-get-started/scheduled-update.png)  
    Po dokončení hromadné postupné aktualizace hello hello ukazuje stav **dokončeno**.
    ![dokončená aktualizace](./media/log-analytics-get-started/completed-update.png)
9. Po dokončení hromadné postupné aktualizace hello můžete zobrazit, zda hello spustit byla úspěšná a můžete zobrazit podrobnosti o co aktualizací používat.

## <a name="after-evaluation"></a>Po vyzkoušení

V tomto kurzu jste nainstalovali agenta na virtuálním počítači a začali s prací. Hello kroky, které jste udělali byly rychlé a jednoduché. Místní infrastruktura většiny velkých organizací a podniků je však složitá. Ano shromažďování dat z těchto komplexní prostředí trvá další plánování a úsilí než hello kurzu. Zkontrolujte hello informace v následující části Další kroky článků toohelpful odkazy hello.

Volitelně můžete odebrat hello pracovní prostor, který jste vytvořili v tomto kurzu.

## <a name="next-steps"></a>Další kroky
* Další informace o připojení [agenty se systémem Windows](log-analytics-windows-agents.md) tooLog Analytics.
* Další informace o připojení [agenty nástroje Operations Manager](log-analytics-om-agents.md) tooLog Analytics.
* [Přidat řešení pro analýzu protokolu z hello řešení Galerie](log-analytics-add-solutions.md) tooadd funkce a shromáždění dat.
* Seznamte se s [protokolu hledání](log-analytics-log-searches.md) tooview podrobné informace shromážděné řešení.
