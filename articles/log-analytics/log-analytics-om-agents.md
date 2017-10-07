---
title: "aaaConnect nástroje Operations Manager tooLog Analytics | Microsoft Docs"
description: "toomaintain stávajících investic do služby System Center Operations Manager a použití rozšířené možnosti analýzy protokolů, Operations Manager lze integrovat s vaším pracovním prostorem OMS."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: magoedte
ms.openlocfilehash: b2841c7aa209fec7357dc4c8b1ff4325fdaa37ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-operations-manager-toolog-analytics"></a>Připojení nástroje Operations Manager tooLog Analytics
toomaintain stávajících investic do služby System Center Operations Manager a použití rozšířené možnosti analýzy protokolů, Operations Manager lze integrovat s vaším pracovním prostorem OMS.  To umožňuje využít hello příležitostí OMS přitom dál toouse nástroje Operations Manager do:

* Pokračovat v monitorování stavu hello vašich IT služeb s nástrojem Operations Manager
* Udržovat integrace s vašimi řešeními ITSM podpora správy incidentů a problémů
* Správa životního cyklu hello agentů nasazených tooon místní a virtuální počítače IaaS veřejného cloudu, které můžete monitorovat pomocí nástroje Operations Manager

Integrace s nástrojem System Center Operations Manager přidá hodnotu tooyour služby operations strategie pomocí hello rychlost a účinnost OMS v shromažďování, ukládání a analýzu dat z nástroje Operations Manager.  Porovnejte pomáhá OMS a pracovat na identifikace hello chyb, problémů a zpřístupnění opakování na podporu vašeho stávajícího procesu správy problém.   Hello flexibilitu hello vyhledávání modul tooexamine výkon, události a upozornění dat, pomocí podrobných řídicích panelů a sestav možnosti tooexpose tato data smysluplnějšími způsoby, předvádí hello síly, které přináší OMS v complimenting nástroje Operations Manager.

agenti Hello reporting skupiny pro správu nástroje Operations Manager toohello shromažďování dat ze serverů, na základě zdroje dat hello analýzy protokolů a řešení, které jste povolili ve vašem předplatném OMS.  V závislosti na hello řešení, které jste povolili jsou data z těchto řešení buď toohello OMS webové služby, nebo z důvodu hello objem dat shromážděných hello spravovaný agent systému, jsou odesílány přímo odesílání přímo z serveru pro správu nástroje Operations Manager z hello agenta tooOMS webové služby. server pro správu Hello předává hello OMS data přímo toohello OMS webové služby; nikdy je zapsán toohello OperationsManager nebo OperationsManagerDW databáze.  Pokud server pro správu ztratí spojení s hello OMS webové služby, je hello ukládá data do mezipaměti místně, dokud komunikace je navázat znovu s OMS.  Pokud server pro správu hello offline z důvodu údržby tooplanned nebo neplánovanému výpadku, jinému serveru pro správu ve skupině pro správu hello obnoví připojení k OMS.  

Hello následující diagram znázorňuje hello připojení mezi servery pro správu hello a agentů skupiny pro správu System Center Operations Manager a OMS, včetně hello směr a porty.   

![OMS – operace manager integrace – diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

Pokud vaše zásady zabezpečení IT neumožňují počítače ve vaší síti tooconnect toohello Internetu, můžete být nakonfigurované tooconnect toohello OMS brány tooreceive informace o konfiguraci a odeslat shromážděná data v závislosti na řešení hello můžete servery pro správu povolili.  Další informace a na tom, jak tooconfigure vaše nástroje Operations Manager management skupiny toocommunicate prostřednictvím brány OMS toohello OMS služby, najdete v části kroky [připojit počítače tooOMS pomocí hello OMS brány](log-analytics-oms-gateway.md).  

## <a name="system-requirements"></a>Požadavky na systém
Než začnete, zkontrolujte hello následující podrobnosti tooverify splňují požadavky.

* OMS podporuje pouze Operations Manager 2016 UR6 Operations Manager 2012 SP1 a vyšší a UR2 Operations Manager 2012 R2 a vyšší.  V nástrojích Operations Manager 2012 SP1 UR7 a Operations Manager 2012 R2 UR3 je přidaná podpora proxy serverů.
* Všichni agenti nástroje Operations Manager musí splňovat požadavky na minimální podporu. Zajistěte, aby agenti jsou minimální aktualizace hello, jinak může dojít k selhání provoz agenta Windows a mnoho chyb může vyplnit hello protokolu událostí nástroje Operations Manager.
* Předplatné OMS.  Další informace najdete v tématu [začít pracovat s analýzy protokolů](log-analytics-get-started.md).

### <a name="network"></a>Síť
Následující seznam hello proxy a firewall informace o konfiguraci vyžadované pro hello agent nástroje Operations Manager, servery pro správu a operace konzoly toocommunicate s OMS Hello informace.  Provoz z jednotlivých součástí je odchozí z vaše síťová služba toohello OMS.     

|Prostředek | Číslo portu| Vynechat kontroly protokolu HTTP|  
|---------|------|-----------------------|  
|**Agent**|||  
|\*.ods.opinsights.azure.com| 443 |Ano|  
|\*.oms.opinsights.azure.com| 443|Ano|  
|\*.blob.core.windows.net| 443|Ano|  
|\*.azure-automation.net| 443|Ano|  
|**Server pro správu**|||  
|\*.service.opinsights.azure.com| 443||  
|\*.blob.core.windows.net| 443| Ano|  
|\*.ods.opinsights.azure.com| 443| Ano|  
|*.azure-automation.net | 443| Ano|  
|**TooOMS konzoly Operations Manager**|||  
|service.systemcenteradvisor.com| 443||  
|\*.service.opinsights.azure.com| 443||  
|\*.live.com| 80 a 443||  
|\*.microsoft.com| 80 a 443||  
|\*.microsoftonline.com| 80 a 443||  
|\*.mms.microsoft.com| 80 a 443||  
|login.windows.net| 80 a 443||  


## <a name="connecting-operations-manager-toooms"></a>Připojení nástroje Operations Manager tooOMS
Proveďte následující sérii kroků tooconfigure hello tooone nástroje Operations Manager management skupiny tooconnect vašich pracovních prostorů OMS.

1. V konzole nástroje Operations Manager hello vyberte hello **správy** prostoru.
2. Rozbalte uzel hello Operations Management Suite a klikněte na tlačítko **připojení**.
3. Klikněte na tlačítko hello **zaregistrovat tooOperations Management Suite** odkaz.
4. Na hello **Průvodce registrací ve službě Operations Management Suite: ověření** zadejte hello e-mailovou adresu nebo telefonní číslo a heslo účtu správce hello, která souvisí s vaším předplatným OMS a klikněte na tlačítko  **Přihlaste se**.
5. Poté, co jsou úspěšně ověřen, na hello **Průvodce registrací ve službě Operations Management Suite: Vyberte pracovní prostor** stránky, se zobrazí výzva tooselect vaším pracovním prostorem OMS.  Pokud máte více než jednoho pracovního prostoru, vyberte hello prostoru tooregister hello skupiny pro správu nástroje Operations Manager z rozevíracího seznamu hello a pak klikněte na **Další**.
   
   > [!NOTE]
   > Nástroj Operations Manager podporuje jenom jeden pracovní prostor OMS najednou. Hello připojení a hello počítačů, které byly registrované tooOMS s předchozí prostoru hello se odeberou z OMS.
   > 
   > 
6. Na hello **Průvodce registrací ve službě Operations Management Suite: Souhrn** , potvrďte nastavení a pokud jsou správné, klikněte na tlačítko **vytvořit**.
7. Na hello **Průvodce registrací ve službě Operations Management Suite: dokončení** klikněte na tlačítko **Zavřít**.

### <a name="add-agent-managed-computers"></a>Přidání počítačů spravovaných agentem
Jakmile nakonfigurujete integraci s vaším pracovním prostorem OMS, to jenom naváže připojení se OMS, je od agentů hello reporting skupiny pro správu tooyour nebyla shromážděna žádná data. Tím nedojde k ní až po dokončení konfigurace, které konkrétní počítače spravované pomocí agentů shromažďuje data pro analýzu protokolu. Můžete buď vybrat objekty počítačů hello samostatně nebo můžete vybrat skupinu, která obsahuje objekty počítače Windows. Nelze vybrat skupinu, která obsahuje instancí jiné třídy, jako jsou logické disky nebo databáze SQL.

1. Otevřete hello Konzola nástroje Operations Manager a vyberte hello **správy** prostoru.
2. Rozbalte uzel hello Operations Management Suite a klikněte na tlačítko **připojení**.
3. Klikněte na tlačítko hello **přidat počítač nebo skupinu** odkazu na hello akce záhlaví na hello pravé straně panelu hello.
4. V hello **hledání počítače** dialogové okno, můžete vyhledat počítače nebo skupiny sledovaných nástrojem Operations Manager. Vyberte počítače nebo skupiny tooOMS tooonboard, klikněte na **přidat**a potom klikněte na **OK**.

Můžete zobrazit počítače a skupiny konfigurované toocollect data z hello spravované počítače uzlu v rámci služby Operations Management Suite v hello **správy** prostoru hello Operations Console.  Tady můžete přidat nebo odebrat počítače a skupiny podle potřeby.

### <a name="configure-oms-proxy-settings-in-hello-operations-console"></a>Konfigurace nastavení proxy serveru OMS v hello Operations console
Proveďte následující kroky, pokud je interní proxy server mezi hello skupiny pro správu a webová služba OMS hello.  Tato nastavení jsou centrálně spravovány ze skupiny pro správu hello a distribuované tooagent spravované systémy, které jsou součástí data toocollect hello oboru pro OMS.  To je výhodné pro při určité řešení vynechat hello správy serveru a odesílání dat přímo tooOMS webové služby.

1. Otevřete hello Konzola nástroje Operations Manager a vyberte hello **správy** prostoru.
2. Rozbalte Operations Management Suite a pak klikněte na tlačítko **připojení**.
3. V hello OMS připojení zobrazení, klikněte na **nakonfigurovat Proxy Server**.
4. Na **Průvodce Operations Management Suite: Proxy Server** vyberte **použít proxy server tooaccess hello Operations Management Suite**, a potom zadejte adresu URL hello s hello číslo portu, například http:// corpproxy:80 a pak klikněte na tlačítko **Dokončit**.

Pokud proxy server vyžaduje ověřování, proveďte následující hello kroky tooconfigure přihlašovací údaje a nastavení, které je třeba toopropagate toomanaged počítače, které hlásí tooOMS ve skupině pro správu hello.

1. Otevřete hello Konzola nástroje Operations Manager a vyberte hello **správy** prostoru.
2. V části **Konfigurace RunAs** vyberte **Profily**.
3. Otevřete hello **System Center Advisor Proxy profilu spustit jako** profilu.
4. V hello Průvodce profilem spustit jako klikněte na tlačítko Přidat toouse účet Spustit jako. Můžete vytvořit [účet Spustit jako](https://technet.microsoft.com/library/hh321655.aspx) nebo použít existující účet. Tento účet musí toohave dostatečná oprávnění toopass pomocí hello proxy serveru.
5. tooset hello toomanage účtu, vyberte **vybranou třídu, skupinu nebo objekt**, klikněte na tlačítko **vyberte...** a pak klikněte na tlačítko **skupiny...** tooopen hello **skupiny vyhledávání** pole.
6. Vyhledejte a pak vyberte **monitorování skupiny serveru serveru Microsoft System Center Advisor**.  Klikněte na tlačítko **OK** po výběru hello skupiny tooclose hello **skupiny vyhledávání** pole.
7. Klikněte na tlačítko **OK** tooclose hello **přidat účet Spustit jako** pole.
8. Klikněte na tlačítko **Uložit** toocomplete hello průvodce a uložte změny.

Po vytvoření připojení hello a nakonfigurujete, jaké agenty bude shromažďování a vytváření sestav dat tooOMS, hello následující konfigurace platí ve skupině pro správu hello, nemusí v pořadí:

* Hello účet Spustit jako **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** je vytvořena.  Je spojena s profilem spustit jako hello **Microsoft System Center Advisor spustit jako profil Blob** a je zacílený na dvě třídy - **Server kolekce** a **skupina správy nástroje Operations Manager** .
* Jsou vytvořeny dva konektory.  nejprve názvem Hello **Microsoft.SystemCenter.Advisor.DataConnector** a je automaticky nakonfigurovaný pomocí odběr, který předává všechny výstrahy generované z instancí všechny třídy v hello správy skupiny tooOMS protokolu Analýzy. je druhý konektor Hello **konektor služby Advisor**, který zodpovídá za komunikaci s webovou službou OMS a sdílení dat.
* Agenty a skupiny, že jste vybrali toocollect dat ve skupině pro správu hello je přidána toohello **monitorování skupiny serveru serveru Microsoft System Center Advisor**.

## <a name="management-pack-updates"></a>Aktualizace sad Management pack
Po dokončení konfigurace skupiny pro správu nástroje Operations Manager hello naváže připojení se hello služby OMS.  server pro správu Hello synchronizuje s hello webové služby a získat informace, aktualizovanou konfiguraci v podobě hello sad management Pack pro hello řešení, které jste povolili, které se integrují s nástrojem Operations Manager.   Nástroj Operations Manager kontroluje aktualizace těchto sad management Pack a automaticky stáhnout a importuje je, pokud jsou k dispozici.  Existují dvě pravidla na konkrétní kterého ovládacího prvku toto chování:

* **Microsoft.SystemCenter.Advisor.MPUpdate** -aktualizace hello základní OMS management Pack. Ve výchozím nastavení spouští každých 12 hodin.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** -aktualizace řešení management Pack, která je povolena v pracovním prostoru. Ve výchozím nastavení spouští každých pět (5) minut.

Můžete přepsat tyto dvě pravidla tooeither zabránit automatické stahování zakázáním je nebo upravit frekvenci hello jak často hello serveru pro správu synchronizuje s OMS toodetermine Pokud je k dispozici nové sady management pack a by měl být stažen.  Postupujte podle kroků hello [jak tooOverride pravidlo nebo monitorování](https://technet.microsoft.com/library/hh212869.aspx) toomodify hello **frekvence** parametr s hodnotou v sekundách toochange hello plán synchronizace, nebo upravit hello **povoleno**  parametr toodisable hello pravidla.  Cíl hello přepíše tooall objekty třídy skupina správy nástroje Operations Manager.

Pokud chcete toocontinue následující stávajícího procesu řízení změn pro řízení verzí management pack v provozní skupina pro správu, můžete zakázat hello pravidla a povolit v určité době, kdy jsou povoleny aktualizace. Pokud máte na vývoj nebo skupiny pro správu QA ve vašem prostředí a má toohello připojení k Internetu, můžete nakonfigurovat tuto skupinu pro správu s toosupport pracovní prostor OMS tento scénář.  To vám umožní tooreview a vyhodnotit hello iterativní verzích sady management Pack hello OMS před uvolněním je do provozní skupina pro správu.

## <a name="switch-an-operations-manager-group-tooa-new-oms-workspace"></a>Přepnout tooa skupinu nástroje Operations Manager nový pracovní prostor OMS
1. Přihlaste se tooyour OMS odběru a vytvořit pracovní prostor v [Microsoft Operations Management Suite](http://oms.microsoft.com/).
2. Konzola nástroje Operations Manager otevřete hello pomocí účtu, který je členem role Správci nástroje Operations Manager hello a vyberte hello **správy** prostoru.
3. Rozbalte Operations Management Suite a vyberte **připojení**.
4. Vyberte hello **znovu nakonfigurovat operace Management Suite** odkaz na hello straně střední hello podokna.
5. Postupujte podle hello **Průvodce registrací ve službě Operations Management Suite** a zadejte hello e-mailovou adresu nebo telefonní číslo a heslo účtu správce hello, který je přidružen nový pracovní prostor OMS.
   
   > [!NOTE]
   > Hello **Průvodce registrací ve službě Operations Management Suite: Vyberte pracovní prostor** stránky uvede existujícímu pracovnímu prostoru hello, která je používána.
   > 
   > 

## <a name="validate-operations-manager-integration-with-oms"></a>Ověření integrace nástroje Operations Manager s OMS
Existuje několik různých způsobů, můžete ověřit, že vaše OMS tooOperations integrace Manager byla úspěšně dokončena.

### <a name="tooconfirm-integration-from-hello-oms-portal"></a>integrace tooconfirm z portálu OMS hello
1. Na portálu OMS hello, klikněte na tlačítko hello **nastavení** dlaždici
2. Vyberte **připojené zdroje**.
3. V tabulce hello pod hello části System Center Operations Manager měli byste vidět hello název skupiny pro správu hello označené hello počet agentů a stavu, pokud byl naposled přijal data.
   
   ![OMS. nastavení connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)
4. Poznámka: hello **ID pracovního prostoru** hodnotu hello levé straně stránky nastavení hello.  Ověření proti níže skupiny pro správu nástroje Operations Manager.  

### <a name="tooconfirm-integration-from-hello-operations-console"></a>integrace tooconfirm z konzole Operations console hello
1. Otevřete hello Konzola nástroje Operations Manager a vyberte hello **správy** prostoru.
2. Vyberte **sady Management Pack** a v hello **vyhledejte:** textového pole zadejte **Advisor** nebo **Intelligence**.
3. V závislosti na hello řešení, které jste povolili zobrazí odpovídající sada management pack uvedené ve výsledcích hledání hello.  Například pokud jste povolili hello řešení pro správu výstrah, hello management pack Microsoft System Center Advisor výstrahy Management je v seznamu hello.
4. Z hello **monitorování** zobrazit, přejděte toohello **Operations Management Suite\Health stavu** zobrazení.  Vyberte server pro správu v části hello **stav serveru pro správu** podokně a v hello **podrobné zobrazení** podokně potvrďte hello hodnotu pro vlastnost **identifikátor URI ověřovací služby** odpovídá ID hello pracovním prostorem OMS.
   
   ![OMS-OpsMgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-oms"></a>Odebrat integraci s OMS
Když už nepotřebujete integrace mezi vaší skupiny pro správu nástroje Operations Manager a pracovní prostor OMS, je několik kroků potřebných tooproperly odebrat hello připojení a konfigurace ve skupině pro správu hello. Hello následující postup se aktualizovat pracovní prostor OMS odstraněním hello odkaz skupiny pro správu, odstraňte hello OMS konektory a pak odstraňte sady management Pack, podpůrné OMS.   

Sady Management Pack pro hello řešení, které jste povolili, které se integrují s nástrojem Operations Manager a hello management Pack požadované toosupport integrace s hello OMS služby nelze snadno odstranit ze skupiny pro správu hello.  Je to proto, že některé sady management Pack hello OMS mají závislosti na jiných sadách pro správu související.  toodelete sady management Pack se závislostí na jiných sadách management Pack stáhnout skript hello [odebrat sadu management pack se závislostmi](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) z centra skriptů TechNet.  

1. Otevřete hello příkazové prostředí nástroje Operations Manager pomocí účtu, který je členem role Operations Manager Administrators hello.
   
    > [!WARNING]
    > Zkontrolujte, že nemáte žádné vlastní sady management Pack s hello slovo Advisor nebo IntelligencePack v názvu hello než budete pokračovat, jinak hello postupem je odstranit ze skupiny pro správu hello.
    > 

2. Hello prostředí příkazového řádku zadejte`Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
3. Další typ`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
4. tooremove žádné sady management Pack zbývající, které jsou závislé na dalších System Center Advisor management Pack, použijte skript hello *RecursiveRemove.ps1* jste si stáhli z centra skriptů TechNet hello dříve.  
 
    > [!NOTE]
    > Neodstraňujte hello Microsoft System Center Advisor nebo Microsoft System Center Advisor interní sad management Pack.  
    >  

5. Otevřete hello Operations Manager Operations console pomocí účtu, který je členem role Operations Manager Administrators hello.
6. V části **správy**, vyberte hello **sady Management Pack** uzel a v hello **vyhledejte:** zadejte **Advisor** a ověřte hello následující sady management Pack jsou stále ve vaší skupině pro správu importovány:
   
   * Microsoft System Center Advisor
   * Microsoft System Center Advisor interní
7. Na portálu OMS hello, klikněte na tlačítko hello **nastavení** dlaždici.
8. Vyberte **připojené zdroje**.
9. V tabulce hello pod hello části System Center Operations Manager, měli byste vidět hello název skupiny pro správu hello chcete tooremove z pracovního prostoru hello.  V části hello sloupec **poslední Data**, klikněte na tlačítko **odebrat**.  
   
    > [!NOTE]
    > Hello **odebrat** odkaz nebudete mít k dispozici až po dobu 14 dnů. Pokud nebude žádná aktivita nalezena, od hello připojené skupiny pro správu.  
    > 

10. Otevře se okno s dotazem, tooconfirm, které chcete tooproceed s odebrání hello.  Klikněte na tlačítko **Ano** tooproceed. 

toodelete hello dva konektory - Microsoft.SystemCenter.Advisor.DataConnector a konektor služby Advisor, uložení skriptu prostředí PowerShell hello níže tooyour počítači a spuštění pomocí hello následující příklady:

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> Spusťte tento skript z počítače Hello, pokud není serverem pro správu, musí mít hello nástroje Operations Manager příkazové prostředí nainstalován v závislosti na verzi hello skupiny pro správu.
> 
> 

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with hello specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with hello specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

Hello budoucí Pokud plánujete připojení vaší správy skupiny tooan pracovním prostorem OMS je nutné importovat toore hello `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` soubor sady management pack z nejnovější kumulativní aktualizace hello použít tooyour skupiny pro správu.  Tento soubor můžete najít v hello `%ProgramFiles%\Microsoft System Center 2012` nebo hello `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` složky.

## <a name="next-steps"></a>Další kroky
Funkce tooadd a shromáždění dat, najdete v části [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).


