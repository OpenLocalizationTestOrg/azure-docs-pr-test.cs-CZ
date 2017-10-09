---
title: "aaaConfigure sestavy pro zálohování Azure"
description: "V tomto článku bude zmíněn konfigurace sestavy Power BI pro Azure Backup pomocí trezoru služeb zotavení."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 86e465f1-8996-4a40-b582-ccf75c58ab87
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 503a240b36ea999e0fea434b6a50d26ddf7677bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-backup-reports"></a>Konfigurace sestav Azure Backup
V tomto článku bude zmíněn sestavy tooconfigure kroky pro Azure Backup pomocí trezoru služeb zotavení a tooaccess tyto sestavy pomocí Power BI. Po provedení těchto kroků, můžete přímo přejít tooview tooPower BI všechny sestavy hello, přizpůsobení a vytváření sestav. 

## <a name="supported-scenarios"></a>Podporované scénáře
1. Azure Backup sestavy nejsou podporovány pro virtuální počítač Azure backup a soubor nebo složku zálohování toocloud pomocí agenta služeb zotavení Azure.
2. V tuto chvíli nepodporuje sestav pro Azure SQL, aplikace DPM a serveru Azure Backup.
3. Sestavy můžete zobrazit různé trezory a odběry, pokud stejný účet úložiště je nakonfigurovaný pro každou trezorů hello. Vybraný účet úložiště musí být v hello stejné oblasti jako obnovení služeb trezoru.
4. frekvence Hello plánované aktualizace pro sestavy hello je 24 hodin v Power BI. Můžete také provést aktualizaci ad-hoc hello sestavy v Power BI, ve kterém případu nejnovější data v účtu úložiště zákazníka slouží pro vykreslení sestavy. 

## <a name="prerequisites"></a>Požadavky
1. Vytvoření [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) tooconfigure pro sestavy. Tento účet úložiště se používá pro ukládání souvisejících dat sestavy.
2. [Vytvořit účet Power BI](https://powerbi.microsoft.com/landing/signin/) tooview, přizpůsobení a vytváření vlastních sestav pomocí portálu Power BI.
3. Registrace poskytovatele prostředků hello **Microsoft.insights** Pokud není již zaregistrován, s předplatným hello účtu úložiště a také s předplatným služeb zotavení hello trezoru tooenable reporting toohello tooflow dat účet úložiště. toodo hello stejné, je nutné přejít tooAzure portál > předplatné > Zprostředkovatelé prostředků a zkontrolujte tento zprostředkovatel tooregister ho. 

## <a name="configure-storage-account-for-reports"></a>Konfigurace účtu úložiště pro sestavy
Pomocí následujících kroků tooconfigure hello účet úložiště pomocí portálu Azure trezoru služeb zotavení hello. Jedná se o jednorázovou konfiguraci a jakmile je nakonfigurovaný účet úložiště, můžete přejít tooPower BI přímo tooview balíček obsahu a využívání sestav.
1. Pokud již máte otevřete trezoru služeb zotavení, pokračujte toonext krok. Pokud nemáte otevřený trezor služeb zotavení, ale jsou v hello portál Azure, v nabídce centra hello, klikněte na tlačítko **Procházet**.

   * V seznamu hello prostředků, zadejte **služeb zotavení**.
   * Během zadávání hello seznamu filtrů podle vašeho zadání. Až uvidíte **Trezory Recovery Services**, klikněte na ně.

      ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Zobrazí se seznam trezorů služeb zotavení Hello. Hello seznamu trezorů služeb zotavení vyberte trezor.

     Otevře se řídicí panel vybraného trezoru Hello.
2. Seznamu hello, položek, které se zobrazí v trezoru, klikněte na tlačítko **zálohování sestavy** v části monitorování a sestavy části tooconfigure hello účet úložiště pro sestavy.

      ![Vyberte zálohu sestavy nabídky položky krok 2](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. V okně Zálohování sestavy hello, klikněte na tlačítko **konfigurace** tlačítko. Otevře se okno hello Azure Application Insights, která se používá k vkládání dat toocustomer úložiště účtu.

      ![Konfigurace úložiště účet krok 3](./media/backup-azure-configure-reports/configure-storage-account.PNG)
4. Nastavit hello stav přepínací tlačítko příliš**na** a vyberte **archivu tooa účet úložiště** políčko tak, aby vytváření sestav dat můžete spustit v účtu úložiště toohello toku.

      ![Povolíte diagnostiku krok 4](./media/backup-azure-configure-reports/set-status-on.png)
5. Klikněte na výběr účtu úložiště a účet úložiště vyberte hello hello seznamu pro uložení sestavy dat a klikněte na tlačítko **OK**.

      ![Vyberte úložiště účet krok 5](./media/backup-azure-configure-reports/select-storage-account.png)
6. Vyberte **AzureBackupReport** zaškrtněte políčko a také přesunout hello posuvníku tooselect doba uchování pro tato data pro vytváření sestav. Vytváření sestav dat v účtu úložiště hello je udržováno dobu hello vybraných pomocí této posuvníku.

      ![Vyberte úložiště účet krok 6](./media/backup-azure-configure-reports/save-configuration.png)
7. Zkontrolujte všechny hello změny a klikněte na **Uložit** tlačítko nahoře, jak ukazuje předchozí obrázek hello. Tato akce zajistí, aby se ukládají všechny změny a účet úložiště je nyní nakonfigurována pro ukládání dat, vytváření sestav.

> [!NOTE]
> Jakmile nakonfigurujete sestavy uložením účet úložiště, měli byste **počkejte 24 hodin** pro toocomplete nabízené počáteční data. Balíček obsahu Azure Backup v Power BI importujte pouze po uplynutí této doby. Odkazovat [v části Nejčastější dotazy](#frequently-asked-questions) další podrobnosti. 
>
>

## <a name="view-reports-in-power-bi"></a>Zobrazit sestavy v Power BI 
Po konfiguraci účtu úložiště pro sestavy na základě trezoru služeb zotavení, trvá přibližně 24 hodin pro vytváření sestav v toostart data. Po 24 hodinách nastavení účtu úložiště použijte hello tyto kroky tooview sestavy v Power BI:
1. [Přihlaste se](https://powerbi.microsoft.com/landing/signin/) tooPower BI.
2. Klikněte na tlačítko **načíst Data** a klikněte na tlačítko Get pod **služby** v knihovně obsahu Pack. Použijte postup uvedený v [pack obsahu Power BI dokumentaci tooaccess](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-packs-services/).

     ![Importovat balíček obsahu](./media/backup-azure-configure-reports/content-pack-import.png)
3. Typ **Azure Backup** v panelu vyhledávání a klikněte na **ho získat**.

      ![Získat balíček obsahu](./media/backup-azure-configure-reports/content-pack-get.png)
4. Zadejte název účtu úložiště hello nakonfigurovali v kroku 5 výše a klikněte na **Další** tlačítko.

    ![Zadejte název účtu úložiště](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Zadejte klíč účtu úložiště hello pro tento účet úložiště. Můžete [zobrazení a zkopírování přístupových klíčů k úložišti](../storage/common/storage-create-storage-account.md#manage-your-storage-account) přechodem tooyour účet úložiště na portálu Azure. 

     ![Zadejte účet úložiště](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>
     
6. Klikněte na tlačítko **přihlášení** tlačítko. Po přihlášení úspěšné, můžete získat **import dat** oznámení.

    ![Import balíčku obsahu](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>
    
    Po určité době získáte **úspěch** oznámení po dokončení importu hello. Může trvat málo delší tooimport hello balíček obsahu, pokud je velké množství dat v účtu úložiště hello.
    
    ![Importovat balíček obsahu úspěch](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>
    
7. Po úspěšném importu **Azure Backup** balíček obsahu je zobrazen v **aplikace** v navigačním podokně hello. seznam Hello teď obsahuje řídicí panel Azure Backup, sestavy a datové sady se žlutou hvězdičku označující nově importované sestavy. 

     ![Balíček obsahu Azure Backup](./media/backup-azure-configure-reports/content-pack-azure-backup.png) <br/>
     
8. Klikněte na tlačítko **Azure Backup** pod řídicí panely, které zobrazuje sadu definovaného klíče sestavy.

      ![Řídicí panel Azure Backup](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. tooview hello kompletní sadu sestav, klikněte na tlačítko žádnou sestavu hello řídicím panelu.

      ![Azure stavu úlohy zálohování](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Klikněte na každé kartě v sestavách tooview hello sestavy v této oblasti.

      ![Azure Backup sestavy karty](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Nejčastější dotazy
1. **Jak zkontrolovat Pokud data sestavy zahájil toku v toostorage účtu?**
    
    Můžete přejít toohello úložiště účet nakonfigurovaný a vyberte kontejnery. Pokud kontejner hello má záznam pro insights-logs-azurebackupreport, znamená to, že data pro vytváření sestav byla spuštěna toku.

2. **Co je četnost hello účet toostorage nabízené dat a balíček obsahu Azure Backup v Power BI?**

   Pro uživatele, dne 0 by byly třeba účet toostorage data toopush přibližně 24 hodin. Jakmile bude tato počáteční nabízené compelete, data se aktualizují s hello následující frekvence uvedený na následujícím obrázku hello. 
      * Data související s příliš**úlohy, výstrahy, zálohování položek, trezory, chráněných serverů a zásady** se instaluje jako toocustomer účet úložiště a při protokolování.
      * Data související s příliš**úložiště** nabídnutých účet úložiště toocustomer každých 24 hodin.
   
    ![Četnost nabízené Azure sestavy zálohování dat](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  Má Power BI [plánovaná aktualizace jednou denně](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Můžete provést ruční aktualizaci hello data v Power BI pro balíček obsahu hello.

3. **Jak dlouho může zachovat hello sestavy?** 

   Při konfiguraci účtu úložiště, můžete vybrat dobu uchování generování sestav dat v účtu úložiště hello (pomocí kroku 6 v účtu úložiště konfigurace pro oddíl sestavy výše). Kromě toho, že můžete [sestavy doplňku analyzovat v aplikaci excel](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) a uložit je delší dobu uchovávání dat, podle potřeb. 

4. **Zobrazí všechna data v sestavách po dokončení konfigurace účtu úložiště hello?**

   Všechny hello data generovaná po **"účet úložiště konfigurace"** nabídnou toohello účet úložiště a bude k dispozici v sestavách. Ale **nejsou v průběhu úlohy nabídnutých** pro vytváření sestav. Jakmile úloha hello neskončí nebo neselže, odesláním tooreports.

5. **Pokud I hello úložiště účet tooview sestavy již nakonfigurována, lze změnit hello konfigurace toouse jiný účet úložiště?** 

   Ano, můžete změnit hello konfigurace toopoint tooa jiný účet úložiště. Hello nově nakonfigurovaný účet úložiště byste měli používat při připojování balíček obsahu tooAzure zálohování. Kromě toho po nakonfigurování jiný účet úložiště by toku nová data v rámci tohoto účtu úložiště. Ale starší data (před změnou konfigurace hello) by nadále zůstávají v hello starší účet úložiště.

6. **Můžete zobrazit různé trezory a odběry sestav?** 

   Ano, můžete nakonfigurovat hello stejný účet úložiště napříč různými trezory tooview mezi trezoru sestavy. Také můžete nakonfigurovat hello stejný účet úložiště pro trezory ve předplatných. Pak můžete tento účet úložiště při připojování tooAzure balíček obsahu zálohování v sestavách hello tooview Power BI. Ale vybrat účet úložiště hello by hello stejné oblasti jako obnovení služeb trezoru.
   
## <a name="next-steps"></a>Další kroky
Teď, když jste nakonfigurovali hello účtu úložiště a importované balíček obsahu Azure Backup, hello dalším krokem je toocustomize tyto sestavy a používání sestav sestavy toocreate dat modelu. Hello následující články pro další podrobnosti naleznete v nápovědě.

* [Pomocí služby Azure Backup reporting datový model](backup-azure-reports-data-model.md)
* [Filtrování sestavy v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Vytváření sestav v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)

