---
title: "aaaPower řídicí panel BI vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti hello Cortana Intelligence toogain v reálném čase a prediktivní Statistika na vehicle stavu a řídí zvyklosti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 0bd054d943387ecad7301236eebae22458173aba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Vehicle telemetrie analytics řešení šablony řídicí panel Power BI pokyny pro instalaci
To **nabídky** odkazy kapitolám toohello v této scénářem. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Hello řešení Vehicle Telemetrie analýzy umožňující prezentovat jak dealerům car, automobilů výrobců a pojištění společností můžete využít možnosti hello přehledy v reálném čase a prediktivní toogain Cortana Intelligence na vehicle stavu a řízení zvyklosti toodrive vylepšení v oblasti hello zákazníka prostředí R & D a marketingových kampaní. Tento dokument obsahuje pokyny krok za krokem, jak můžete nakonfigurovat hello Power BI sestavy a řídicí panel po nasazení řešení hello ve vašem předplatném. 

## <a name="prerequisites"></a>Požadavky
1. Nasazení hello [Telemetrie Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) řešení  
2. [Nainstalujte Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331)
3. [Předplatné](https://azure.microsoft.com/pricing/free-trial/). Pokud nemáte předplatné Azure, Začínáme s Azure bezplatné předplatné
4. Účet Microsoft Power BI

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence Suite součásti
V rámci šablony řešení Analytics Telemetrie Vehicle hello hello následující služby Cortana Intelligence nasazených v rámci vašeho předplatného.

* **Centra událostí** pro příjem miliony událostí vehicle telemetrická data do Azure.
* **Stream Analytics** pro získání přehledy v reálném čase na vehicle stavu a přetrvává tato data do dlouhodobého úložiště pro širší batch analýzu.
* **Strojového učení** pro zjišťování anomálií v reálném čase a dávkového zpracování toogain prediktivní statistiky.
* **HDInsight** data tootransform využít ve velkém měřítku
* **Objekt pro vytváření dat** zpracovává orchestration, plánování, správy prostředků a monitorování hello dávkové zpracování kanálu.

**Power BI** toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy. 

řešení Hello používá dvou různých zdrojů dat.: **Simulated vehicle signály a diagnostiky datovou sadu** a **vehicle katalogu**.

Vehicle telematika jsme je součástí tohoto řešení. Vysílá diagnostické informace a signály odpovídající toohello stav hello vehicle a podporovat jeho vzor k danému bodu v čase. 

Hello Vehicle katalogu je referenční datová sada obsahující VIN toomodel mapování

## <a name="power-bi-dashboard-preparation"></a>Příprava Power BI řídicí panel
### <a name="setup-power-bi-real-time-dashboard"></a>Instalační program v reálném čase řídicí panel Power BI

**Spuštění aplikace v reálném čase řídicí panel hello** po dokončení nasazení hello, postupujte podle pokynů operaci ruční hello

* Stažení aplikace řídicího panelu v reálném čase RealtimeDashboardApp.zip a rozbalte ho.
*  V rozbalené složce hello otevřete aplikaci konfiguračního souboru 'RealtimeDashboardApp.exe.config', nahraďte appSettings pro Eventhub, úložiště objektů Blob a ML připojení služby s hodnotami hello v hello pokyny pro ruční provoz a uložte změny.
* Spusťte aplikaci RealtimeDashboardApp.exe. Okno přihlášení bude překryvné, zadejte svoje platné přihlašovací údaje PowerBI a klikněte na hello **přijmout** tlačítko. Pak spustí aplikaci hello toorun.

   ![Přihlášení tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI Dashboard oprávnění](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* Web tooPowerBI přihlášení a vytvořit řídicí panel v reálném čase.

Nyní jsou připravené tooconfigure řídicí panel Power BI hello s bohatých vizualizací toogain v reálném čase a zvyklosti prediktivní Statistika na vehicle stavu a řídí. Jak dlouho trvá přibližně 45 minut tooan hodinu toocreate všechny hello sestavy a řídicí panel hello konfigurace. 

### <a name="configure-power-bi-reports"></a>Konfigurace sestav Power BI
Hello v reálném čase sestavy a řídicí panel hello trvat o toocomplete 30 – 45 minut. Procházet příliš[http://powerbi.com](http://powerbi.com) a přihlaste se.

![Přihlášení tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Nová datová sada se generuje ve službě Power BI. Klikněte na tlačítko hello **ConnectedCarsRealtime** datovou sadu.

![Vybráno připojené v reálném čase datové sady automobilů](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Uložit pomocí prázdné sestavy hello **Ctrl + s**.

![Uložit prázdné sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Zadejte název sestavy *Vehicle Telemetrie analýzy v reálném čase - sestavy*.

![Zadejte název sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Sestavy v reálném čase
V tomto řešení jsou tři sestavy v reálném čase:

1. Vozidel v operaci
2. Vozidel údržby
3. Statistiky vozidel stavu

Můžete zvolit tooconfigure všechny hello tři v reálném čase sestavy nebo zastavena po jakékoli fázi a pokračovat další části toohello konfigurace hello batch sestavy. Doporučujeme toocreate všechny hello tři sestavy toovisualize hello úplné statistiky v reálném čase cesty hello hello řešení.  

### <a name="1-vehicles-in-operation"></a>1. Vozidel v operaci
Klikněte dvakrát na **stránka 1** a přejmenujte ji příliš "Vozidel v operaci"  
    ![Připojených vozidel v operaci aut-](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Vyberte **vin** pole z **pole** a vyberte typ vizualizace jako **"Kartou"**.  

Karta vizualizace se vytvoří, jak je znázorněno na obrázku.  
    ![Připojené aut - vyberte vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.  

Vyberte **města** a **vin** z pole. Změnit vizualizace příliš**"Map"**. Přetáhněte **vin** v oblasti hodnoty. Přetáhněte **města** z pole příliš**legendy** oblasti.   
    ![Připojené aut – karta vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

Vyberte **formátu** část z **vizualizace**, klikněte na tlačítko **název** a změňte hello **Text** příliš**"vozidel v operace ve městě"**.  
    ![Připojených vozidel v operaci podle města aut-](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Poslední vizualizace vypadá, jak je znázorněno na obrázku.    
    ![Připojené aut - poslední vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.  

Vyberte **města** a **vin**, změnit typ vizualizace příliš**skupinový sloupcový graf**. Ujistěte se, **města** pole **osy oblasti** a **vin** v **hodnotu oblasti**  

Graf řazení podle **"Počet vin"**  
    ![Připojené aut - počet vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Změna grafu **název** příliš**"Vozidel v operaci podle města"**  

Klikněte na tlačítko hello **formátu** části a pak vyberte **Data barvy**, klikněte na tlačítko hello **"Na"** příliš**Zobrazit vše**  
    ![Připojené aut – zobrazit všechny Data barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Změnit barvu hello jednotlivých města kliknutím na ikonu barev.  
    ![Připojené aut - Změna barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.  

Vyberte **skupinový sloupcový graf** vizualizace z vizualizace, přetáhněte **města** pole **osy** oblasti **modelu** v **legendy** oblasti a **vin** v **hodnotu** oblasti.  
    ![Připojené aut - skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Připojené aut - vykreslování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

Změna uspořádání všechny vizualizace na této stránce, jak je znázorněno na obrázku.  
    ![Připojené aut - vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Úspěšně jste nakonfigurovali v reálném čase sestavy "Vozidel v operaci" hello. Můžete pokračovat v reálném čase sestavy toocreate hello další nebo zde zastavte a konfigurujte hello řídicího panelu. 

### <a name="2-vehicles-requiring-maintenance"></a>2. Vozidel údržby
Klikněte na tlačítko ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd nové sestavy, přejmenujte ji příliš**"Vozidel nutnosti Údržba"**

![Připojené aut - vozidel údržby](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Vyberte **vin** pole a změnit typ vizualizace příliš**karty**.  
    ![Připojené aut – karta Vin vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Máme pole s názvem "MaintenanceLabel" v datové sadě hello. Toto pole může mít hodnotu "0" nebo "1"." Je nastavena podle model Azure Machine Learning hello zřízené v rámci řešení a integraci s cestou hello v reálném čase. Hello hodnotu "1" označuje, že že vehicle vyžaduje údržbu. 

tooadd **úrovně stránky** filtr zobrazující vozidel data, která jsou nutnosti údržby: 

1. Přetáhněte hello **"MaintenanceLabel"** pole do **stránky úroveň filtry**.  
   ![Připojené aut - filtry úrovně stránky](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  
2. Klikněte na tlačítko **základní filtrování** nabídky nachází v dolní části MaintenanceLabel stránky úroveň filtru.  
   ![Připojené aut – základní filtrování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  
3. Nastavení jeho hodnoty filtru příliš**"1"**    
   ![Připojené aut - hodnota filtru](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.  

Vyberte **skupinový sloupcový graf** z vizualizace  
![Připojené aut – karta Vind vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Připojené aut - skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Přetáhněte pole **modelu** do **osy** oblasti **Vin** příliš**hodnotu** oblasti. Seřaďte vizualizace podle **počet vin**.  Změna grafu **název** příliš**"Vozidel nutnosti údržby modelem"**  

Přetáhněte **vin** polí do **sytost barev** u **pole** ![pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) části **vizualizace** karta  
![Připojené aut - sytost barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Změna **Data barvy** v vizualizace z **formát** části  
Změnit barvu minimální: **F2C812**  
Změnit barvu maximální: **FF6300**  
![Připojené aut - změny barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Připojené aut - nové Vizualizační barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.  

Vyberte **Clustered sloupcový graf** z vizualizace, přetáhněte **vin** pole do **hodnotu** oblasti, přetáhněte **města** pole do **osy** oblasti. Graf řazení podle **"Počet vin"**. Změna grafu **název** příliš**"Vozidel údržby podle města"**   
![Připojené aut - vozidel údržby podle města](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.  

Vyberte **více řádků karty** vizualizace z vizualizace, přetáhněte **modelu** a **vin** do hello **pole** oblasti.  
![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Změna uspořádání všechny hello vizualizace, závěrečnou zprávu hello vypadá takto:  
![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Úspěšně jste nakonfigurovali v reálném čase sestavy "Vozidel nutnosti Údržba" hello. Můžete pokračovat v reálném čase sestavy toocreate hello další nebo zde zastavte a konfigurujte hello řídicího panelu. 

### <a name="3-vehicles-health-statistics"></a>3. Statistiky vozidel stavu
Klikněte na tlačítko ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd nové sestavy, přejmenujte ji příliš**"Vozidel stavu statistika"**  

Vyberte **měřidla** vizualizace z vizualizace, přetáhněte hello **rychlost** pole do **hodnotu, hodnotu minimální, maximální hodnota** oblasti.  
![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Změňte výchozí agregace hello **rychlost** v **hodnotu oblasti** příliš**průměrná** 

Změňte výchozí agregace hello **rychlost** v **minimální oblasti** příliš**minimální**

Změňte výchozí agregace hello **rychlost** v **maximální oblasti** příliš**maximální**

![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Přejmenujte hello **název měřidla** příliš**"Průměrná rychlost"** 

![Připojené aut - měřidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.  

Podobně přidat **měřidla** pro **průměrná těžba ropy modul**, **průměrná paliva**, a **průměrná modul mírného**.  

Změňte výchozí agregace hello polí v každé měřidla dle výše uvedených kroků v **"Průměrná rychlost"** měřidla.

![Připojené aut - měřidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.

Vyberte **spojnicový a skupinový sloupcový graf** z vizualizace, přetáhněte **města** pole do **sdílené osy**, přetáhněte **rychlost**, **pole tirepressure a engineoil** do **hodnoty ve sloupcích** oblasti, změnit jejich typ agregace příliš**průměrná**. 

Přetáhněte hello **engineTemperature** pole do **hodnoty řádku** oblasti změnit typ agregace hello příliš**průměrná**. 

![Připojené aut - vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Změna hello grafu **název** příliš**"Průměrná rychlost, můžete zadat naléhavost, modul těžba ropy a teploty modul"**.  

![Připojené aut - vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.

Vyberte **vlastnosti Treemap** vizualizace z vizualizace, přetáhněte hello **modelu** pole do hello **skupiny** oblasti a přetáhněte pole hello  **MaintenanceProbability** do hello **hodnoty** oblasti.

Změna hello grafu **název** příliš**"Vehicle modely údržby"**.

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.

Vyberte **100 % skládaný sloupcový graf** z vizualizace, přetáhněte hello **města** pole do hello **osy** oblasti a přetáhněte hello **MaintenanceProbability**, **RecallProbability** pole do hello **hodnotu** oblasti.

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klikněte na tlačítko **formátu**, vyberte **Data barvy**a sadu hello **MaintenanceProbability** barvu, která hodnota toohello **"F2C80F"**.

Změna hello **název** z hello grafu příliš**"pravděpodobnosti z Vehicle údržby a odvolat pomocí Město,**.

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klikněte na tlačítko hello prázdnou oblast tooadd novou vizualizaci.

Vyberte **plošný graf** z vizualizace z vizualizace, přetáhněte hello **modelu** pole do hello **osy** oblasti a přetáhněte hello **engineOil, tirepressure, rychlosti a MaintenanceProbability** pole do hello **hodnoty** oblasti. Změnit jejich typ agregace příliš**"Průměrné"**. 

![Připojené aut - změnit typ agregace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Změňte název hello hello grafu příliš**"Průměrná těžba ropy modul, přestanou zatížení, rychlosti a údržba pravděpodobnosti modelem"**.

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klikněte na novou vizualizaci tooadd hello prázdnou oblast:

1. Vyberte **bodový graf** vizualizace z vizualizace.
2. Přetáhněte hello **modelu** pole do hello **podrobnosti** a **legendy** oblasti.
3. Přetáhněte hello **paliva** pole do hello **osy x** oblasti změnit hello agregace příliš**průměrná**.
4. Přetáhněte **engineTemparature** do **osy y oblasti**, změňte hello agregace příliš**průměrná**
5. Přetáhněte hello **vin** pole do hello **velikost** oblasti.

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Změna hello grafu **název** příliš**"Průměry paliva, modul teploty modelem"**.

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

závěrečnou zprávu Hello bude vypadat, jak je uvedeno níže.

![Připojené aut konečné sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-hello-reports-toohello-real-time-dashboard"></a>Vizualizace kódu PIN z řídicího panelu hello sestavy toohello v reálném čase
Kliknutím na další tooDashboards hello plus ikona vytvořte prázdný řídicí panel. Pojmenujte ji "Panelu analýzy Telemetrie Vehicle"

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Režim PIN kódu hello vizualizace z hello výše sestavy toohello řídicího panelu. 

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

řídicí panel Hello by měla vypadat takto definovaného toohello řídicí panel po všech hello, které jsou vytvořeny tři sestavy a hello odpovídající vizualizace. Pokud jste dosud nevytvořili všechny sestavy hello, může vypadat jinak řídicího panelu. 

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Blahopřejeme! Úspěšně jste vytvořili v reálném čase řídicí panel hello. Jak budete pokračovat, tooexecute CarEventGenerator.exe a RealtimeDashboardApp.exe, měli byste vidět živé aktualizace na řídicím panelu hello. Má trvat asi 10 too15 minut toocomplete hello následující kroky.

## <a name="setup-power-bi-batch-processing-dashboard"></a>Instalační program řídicí panel Power BI dávkové zpracování
> [!NOTE]
> Trvá přibližně 2 hodiny (z hello úspěšné dokončení nasazení hello) pro hello end tooend dávkového zpracování kanálu toofinish provádění a zpracovat vygenerovaný data za rok. Proto počkejte hello zpracování toofinish než budete pokračovat v dalších krocích hello. 
> 
> 

**Stáhněte si soubor návrháře Power BI hello**

* Je součástí nasazení hello pokyny pro ruční provoz předem nakonfigurovaný soubor návrháře Power BI
* Podívejte se na 2. Instalační program PowerBI dávkové zpracování řídicí panel si můžete stáhnout hello PowerBI šablonu pro dávkové zpracování řídicí panel zde názvem **ConnectedCarsPbiReport.pbix**.
* Uložit místně

**Konfigurace sestav Power BI**

* Otevřete hello návrháře souboru '**ConnectedCarsPbiReport.pbix**' pomocí Power BI Desktop. Pokud již máte, nenainstalujete hello Power BI Desktop z [instalace Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331). 
* Klikněte na tlačítko hello **upravit dotazy**.

![Upravit dotaz Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* Klikněte dvakrát na hello **zdroj**.

![Zdroj sady Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* Aktualizujte připojovací řetězec serveru hello server Azure SQL, které byly zřízeny jako součást nasazení hello.  Hledat v hello ruční operace pokyny v části 

    4. Azure SQL Database
    
    * Server: somethingsrv.database.windows.net
    * Databáze: connectedcar
    * Uživatelské jméno: uživatelské jméno
    * Heslo: Heslo SQL serveru můžete spravovat z portálu Azure

* Nechte **databáze** jako *connectedcar*.

![Nastavte Power BI databázi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* Klikněte na **OK**.
* Zobrazí se **pověření systému Windows** vybraná karta ve výchozím nastavení, změňte jej příliš**databáze pověření** kliknutím na **databáze** kartě vpravo.
* Zadejte hello **uživatelské jméno** a **heslo** vaší databáze SQL Azure, který byl zadán během instalace jeho nasazení.

![Zadejte přihlašovací údaje databáze](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* Klikněte na tlačítko **připojení**
* Hello výše kroky opakujte pro každý hello tři zbývající dotazů nachází v pravém podokně a pak aktualizujte podrobnosti připojení zdroje dat hello.
* Klikněte na tlačítko **zavřete a načíst**. Power BI Desktop souboru datové sady jsou připojené tooSQL Azure databázových tabulek.
* **Zavřít** souboru Power BI Desktop.

![Zavřít Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* Klikněte na tlačítko **Uložit** tlačítko toosave hello změny. 

Nyní máte nakonfigurovaný všechny sestavy hello odpovídající toohello dávkové zpracování cestu hello řešení. 

## <a name="upload-toopowerbicom"></a>Nahrát příliš*powerbi.com*
1. Přejděte toohello Power BI webový portál na http://powerbi.com a přihlášení.
2. Klikněte na tlačítko **získat Data**  
3. Nahrajte hello soubor Power BI Desktop.  
4. tooupload, klikněte na tlačítko **načíst Data -> získat soubory -> místního souboru**  
5. Přejděte toohello **"**ConnectedCarsPbiReport.pbix**"**  
6. Po nahrání souboru hello bude navigaci back tooyour Power BI pracovní prostor.  

Datové sady, sestavy a řídicí panel prázdné, bude vytvořena pro vás.  

PIN kód grafy tooa novým řídicím panelem názvem **panelu analýzy Telemetrie Vehicle** v **Power BI**. Klikněte na hello prázdný řídicí panel, vytvořili výše a potom přejděte toohello **sestavy** klikněte na část hello nově nahrát sestavu.  

![Vehicle Telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

**Poznámka: hello sestava obsahuje šest stránky:**  
Stránka 1: Hustotu Vehicle  
Stránka 2: Stav v reálném čase vehicle  
Stránka 3: Intenzivně řízené vozidel   
Stránka 4: Připomenout vozidel  
5 stránky: Efektivní řízené vozidel paliva  
6 stránky: Logo společnosti Contoso  

![Připojené aut Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

**Ze stránky 3**, připnout hello následující:  

1. Počet VIN  
   ![Připojené aut Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. Intenzivně řídí vozidel modelu – vodopádu grafu  
   ![Vehicle Telemetrie – Pin grafy 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Ze stránky 5**, připnout hello následující: 

1. Počet vin    
   ![Vehicle Telemetrie – Pin grafy 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. Paliva efektivní vozidel modelem: Skupinový sloupcový graf  
   ![Vehicle Telemetrie – Pin grafy 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Stránka 4**, připnout hello následující:  

1. Počet vin  
   ![Vehicle Telemetrie – Pin grafy 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. Odvolané vozidel podle města, model: vlastnosti Treemap  
   ![Vehicle Telemetrie – Pin grafy 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Ze stránky 6**, připnout hello následující:  

1. Logo společnosti Contoso motory  
   ![Vehicle Telemetrie – Pin grafy 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Uspořádání hello řídicí panel**  

1. Přejděte toohello řídicí panel
2. Pozastavte ukazatel myši nad každým grafem a přejmenujte ji podle hello pojmenování uvedené v následující obrázek hello kompletní řídicí panel. Také přesunete hello grafy kolem toolook jako řídicí panel hello níže.  
   ![Vehicle Telemetrie – uspořádání řídicí panel 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle Telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. Pokud jste vytvořili všechny hello sestavy, jak je uvedeno v tomto dokumentu, hello poslední dokončené řídicí panel by měl vypadat jako hello následující obrázek. 

![Vehicle Telemetrie – uspořádání řídicí panel 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Blahopřejeme! Úspěšně jste vytvořili hello sestavy a řídicí panel toogain v reálném čase, prediktivní hello a zvyklosti batch Statistika na vehicle stavu a řídí.  
