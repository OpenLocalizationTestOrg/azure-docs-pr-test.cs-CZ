---
title: "řešení pro správu Azure Log Analytics aaaAdd | Microsoft Docs"
description: "Operations Management Suite (OMS) / řešení pro správu analýzy protokolů jsou kolekce logiku, vizualizace a data pořízení pravidel, které poskytují metriky seskupit kolem oblasti konkrétní problém."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f5a232d20e144b800387b09adb5248505d801944
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-log-analytics-management-solutions-tooyour-workspace"></a>Přidat pracovní prostor analýzy protokolů Azure management řešení tooyour

Řešení pro správu protokolu Analytics jsou kolekce **logiku**, **vizualizace**, a **pravidla získávání dat** které poskytují metriky seskupit kolem konkrétní problém oblasti. Tento článek obsahuje seznam řešení pro správu nepodporuje analýzy protokolů a ukazuje, jak hello tooadd a odebrat pro pracovní prostor pomocí portálu Azure. Můžete také přidat řešení na portálu OMS hello pomocí Galerie řešení hello.

Řešení pro správu povolit podrobnější přehled na:

* Pomohou prozkoumat a vyřešit provozní problémy rychlejší
* Shromáždit a korelovat různé typy dat počítače
* Abyste mohli proaktivní s aktivitami, které zpřístupňuje hello řešení.

> [!NOTE]
> Analýzy protokolů zahrnuje funkce hledání protokolů, takže není nutné tooinstall tooenable řešení správy ho. Však získat vizualizaci dat, návrhy vyhledávání a insights přidáním prostoru tooyour řešení pro správu.

Pomocí tohoto článku, přidáte tooa prostoru řešení pro správu pomocí hello portál Azure Marketplace. Po přidání řešení, data se budou shromažďovat ze serverů hello ve vaší infrastruktuře a odeslat toohello OMS služby. Zpracování hello obvykle trvá několik minut tooan hodina OMS služby. Po hello služba zpracovává hello data, můžete ji zobrazit v OMS.

Můžete snadno odebrat řešení pro správu, když už ho nepotřebují. Když odeberete řešení pro správu, jeho data neodesílají tooOMS. Pokud jste na hello volná cenová úroveň, může snížit hello množství dat použít, což pomáhá dodrželi denní kvóta dat po odebrání řešení.

## <a name="view-available-management-solutions"></a>Řešení správy dostupných zobrazení

Hello Azure marketplace obsahuje seznam hello [řešení pro správu pro analýzy protokolů](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Řešení pro správu můžete nainstalovat z Azure marketplace kliknutím hello **ho získat** odkaz dole hello v jednotlivých řešení.

## <a name="add-a-management-solution"></a>Přidat řešení pro správu
1. Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí svého předplatného Azure.
2. V hello **nový** okno pod **Marketplace**, vyberte **monitorování + správu**.
3. V hello **monitorování + správu** okně klikněte na tlačítko **zobrazit všechny**.  
    ![Monitorování + okna Správa](./media/log-analytics-add-solutions/monitoring-management-blade.png)  
4. toohello napravo od **řešení pro správu**, klikněte na tlačítko **Další**.
5. V hello **řešení pro správu** okně vybrat řešení pro správu, které chcete tooadd tooa prostoru.  
    ![Monitorování + okna Správa](./media/log-analytics-add-solutions/management-solutions.png)  
6. Okna Správa řešení hello, přečtěte si informace o řešení pro správu hello a pak klikněte na **vytvořit**.
7. V hello *název řešení pro správu* okně vyberte pracovní prostor, které chcete tooassociate hello řešení pro správu.
8. Volitelně můžete změňte nastavení pracovního prostoru pro hello předplatné, skupinu prostředků a umístění. Můžete také **Možnosti automatizace**. Klikněte na možnost **Vytvořit**.  
    ![pracovní prostor řešení](./media/log-analytics-add-solutions/solution-workspace.png)  
9. nasazení řešení pro správu hello tooyour pracovní prostor, přidané toostart přejděte příliš**analýzy protokolů** > **odběry** > ***název pracovního prostoru ***  >  **Přehled**. Zobrazí se nová dlaždice pro vaše řešení pro správu. Klikněte na položku hello dlaždice tooopen ho a začít používat řešení hello po shromáždění dat pro řešení hello.

## <a name="remove-a-management-solution"></a>Odebrat řešení pro správu

1. V hello [portál Azure](https://portal.azure.com), přejděte příliš**analýzy protokolů** > **odběry** > ***název pracovního prostoru*** a pak v hello ***název pracovního prostoru*** okně klikněte na tlačítko **řešení**.
2. V seznamu hello řešení pro správu vyberte hello řešení, které chcete tooremove.
3. V okně řešení hello vašeho pracovního prostoru, klikněte na tlačítko **odstranit**.  
    ![Odstranit řešení](./media/log-analytics-add-solutions/solution-delete.png)  
4. V dialogovém okně hello potvrzení, klikněte na tlačítko **Ano**.

## <a name="offers-and-pricing-tiers"></a>Nabídky a cenové úrovně

Následující tabulka Hello identifikuje, které řešení pro správu patří nabídka tooeach Operations Management Suite.
Hello tabulka také určuje hello cenové úrovně, které jsou dostupné pro každý řešení pro správu.
Všechna řešení v následující tabulce hello jsou dostupné v rámci hello Azure portál a hello Galerie řešení portálu Analýza protokolů hello.

| Řešení pro správu                                                                       | Nabídka                                                                     | Cenové úrovně<sup>1</sup>                                                 | Poznámky |
| ---                                                                                       | ---                                                                       | ---                                                                                                       | ---   |
| [Activity Log Analytics](log-analytics-activity.md)                                                                   | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | 90 dnů dat jsou k dispozici zdarma<br>Data nejsou předmětem toohello úroveň Free zakončení |
| [Posouzení AD](log-analytics-ad-assessment.md)                                           | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Stav replikace AD](log-analytics-ad-replication-status.md)                           | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Není k dispozici tooadd z portálu Azure nebo marketplace. |
| [Stav agenta](../operations-management-suite/oms-solution-agenthealth.md)                                                                                | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Data nejsou předmětem toohello úroveň Free zakončení<br> Není k dispozici tooadd z portálu Azure nebo marketplace. |
| [Správa výstrah](log-analytics-solution-alert-management.md)                            | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Není k dispozici tooadd z portálu Azure nebo marketplace. |
| [Konektor služby Statistika aplikace (Preview)](log-analytics-app-insights-connector.md)                                               | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Automatizace hybridní pracovní proces](../automation/automation-hybrid-runbook-worker.md)                                                                     | <ul><li>Automatizace a řízení</li></ul>                                  | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | Vyžaduje vaše analýzy protokolů prostoru toobe propojené tooan účet Automation. |
| [Analýza brány Azure aplikace](log-analytics-azure-networking-analytics.md)    | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Skupina zabezpečení sítě Azure Analytics](log-analytics-azure-networking-analytics.md)     | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Analýza Azure SQL (Preview)](log-analytics-azure-sql.md)                                                       | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br>Za&nbsp;uzlu&nbsp;(OMS)                                                                          | Vyžaduje vaše analýzy protokolů prostoru toobe propojené tooan účet Automation.|
| [Azure Web Apps Analytics](log-analytics-azure-web-apps-analytics.md)     | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
|[Backup](../backup/backup-introduction-to-azure-backup.md)                                                                                 | <ul><li>Statistiky a analýza</li></ul>                                   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                       | Vyžaduje classic úložiště záloh.<br> Není k dispozici tooadd z portálu Azure nebo marketplace. |
| [Kapacitu a výkon (Preview)](log-analytics-capacity.md)                                                   | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Sledování změn](log-analytics-change-tracking.md)                                       | <ul><li>Automatizace a řízení</li></ul>                                  | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | Vyžaduje vaše analýzy protokolů prostoru toobe propojené tooan účet Automation. |
| [Kontejnery](log-analytics-containers.md)                                                 | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [IT služby konektoru Management (Preview)](log-analytics-itsmc-overview.md)                                              | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)     | |
| HDInsight HBase monitorování <br>(Preview)                                                  | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Analýza služby Key Vault](log-analytics-azure-key-vault.md)                   | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Logiku aplikace B2B](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)                    | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Není k dispozici tooadd z portálu Azure nebo marketplace. |
| [Posouzení malwaru](log-analytics-malware.md)                                            | <ul><li>Zabezpečení a dodržování předpisů</li></ul>                                 | Free<br> Standalone<br>Za&nbsp;uzlu&nbsp;(OMS)                                                                           | Pokud po 19 června 2017 přidáte řešení zabezpečení a dodržování předpisů hello [fakturuje se podle uzlu](https://azure.microsoft.com/pricing/details/security-compliance/), bez ohledu na to pracovního prostoru hello cenová úroveň. Hello prvních 60 dní jsou volné.  |
| [Sledování výkonu sítě](log-analytics-network-performance-monitor.md) <br>  | <ul><li>Statistiky a analýza</li></ul>                                   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | |
| [Analýza Office 365 (Preview)](../operations-management-suite/oms-solution-office-365.md)                                                       | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Zabezpečení a audit](../operations-management-suite/oms-security-getting-started.md)      | <ul><li>Zabezpečení&nbsp;a&nbsp;dodržování předpisů</li></ul>                       | Free<br> Standalone<br>Za&nbsp;uzlu&nbsp;(OMS)                                                                           | Shromažďování protokolů událostí zabezpečení vyžaduje toto řešení<br>Pokud po 19 června 2017 přidáte řešení zabezpečení a dodržování předpisů hello [fakturuje se podle uzlu](https://azure.microsoft.com/pricing/details/security-compliance/), bez ohledu na to pracovního prostoru hello cenová úroveň. Hello prvních 60 dní jsou volné. |
| [Služba Fabric Analytics (Preview)](log-analytics-service-fabric.md)                     | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Mapa služeb (Preview)](../operations-management-suite/operations-management-suite-service-map.md) | <ul><li>Statistiky a analýza</li></ul>                      | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | K dispozici ve východní USA, západní Evropa a střed USA – západ    |
| [Site Recovery](../site-recovery/site-recovery-overview.md)                                                                               | <ul><li>Statistiky a analýza</li></ul>                                   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                       | Vyžaduje classic trezoru Site Recovery.<br> Není k dispozici tooadd z portálu Azure nebo marketplace. |
| [Posouzení SQL](log-analytics-sql-assessment.md)                                         | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| Spuštění/zastavení virtuálních počítačů mimo špičku<br>(Preview)                                              | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | Vyžaduje vaše analýzy protokolů prostoru toobe propojené tooan účet Automation. |
| [SurfaceHub](log-analytics-surface-hubs.md)                                               | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Není k dispozici tooadd z portálu Azure nebo marketplace. |
| [System Center Operations Manager Assessment (Preview)](log-analytics-scom-assessment.md)  | <ul><li>Statistiky a analýza</li><li>Log Analytics</li></ul>        | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Správa aktualizací](../operations-management-suite/oms-solution-update-management.md)                                                                         | <ul><li>Automatizace a řízení</li></ul>                                  | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | Vyžaduje vaše analýzy protokolů prostoru toobe propojené tooan účet Automation. |
| [Dodržování předpisů pro aktualizaci (Preview)](https://docs.microsoft.com/windows/deployment/update/update-compliance-get-started)                                                             | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Bez poplatků dat nebo uzly<br>Data nejsou předmětem toohello úroveň Free zakončení.<br> Není k dispozici tooadd z portálu Azure nebo marketplace. |
| [Upgrade Readiness](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started)                                                          | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Bez poplatků dat nebo uzly<br>Data nejsou předmětem toohello úroveň Free zakončení.<br> Není k dispozici tooadd z portálu Azure nebo marketplace. |
| [VMware monitorování (Preview)](log-analytics-vmware.md)                                | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Za&nbsp;GB&nbsp;(samostatně)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Data kabelové sítě 2.0 (Preview)](log-analytics-wire-data.md)                                                                 | <ul><li>Statistiky a analýza</li></ul>                                   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | K dispozici ve východní USA, západní Evropa a střed USA – západ |

<sup>1</sup> hello *standardní* a *Premium (OMS)* cenové úrovně jsou dostupné pouze pro zákazníky, kteří vytvořili jejich analýzy protokolů prostoru předchozí tooSeptember 21, 2016.

### <a name="community-provided-management-solutions"></a>Komunita poskytuje řešení pro správu

Jsou k dispozici z hello komunity poskytuje řešení [šablony Azure Galerie](https://azure.microsoft.com/resources/templates/?term=Per&nbsp;Node&nbsp;(OMS)) a direct od autorů hello.

| Řešení pro správu               | Nabídka                                                                     | Cenové úrovně                         | Poznámky |
| ---                               | ---                                                                       | ---                                   | ---   |
| Všechna řešení pro zadaný komunity  | <ul><li>Přehled&nbsp;a&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)     |   Vyžaduje vaše analýzy protokolů prostoru toobe propojené tooan účet Automation. |




## <a name="data-collection-details"></a>Podrobnosti kolekce dat
Hello následující tabulky popisují metody shromažďování dat a další podrobnosti o tom, jak se údaje pro analýzy protokolů správy řešení a zdrojů dat. Hello tabulky jsou klasifikovány podle nabízí řešení, které označení rovnosti příliš[předplatné cenové úrovně](https://go.microsoft.com/fwlink/?linkid=827926). Hello analýzy protokolů aktivity řešení je k dispozici tooall cenové úrovně zdarma.

agent protokolu analýzy Windows Hello a hello System Center Operations Manager agent jsou v podstatě stejné. agent webu Windows Hello zahrnuje další funkce tooallow ho tooconnect toohello OMS pracovní prostor a směrovat přes proxy server. Pokud používáte agenta nástroje Operations Manager, musí být cílem jako toocommunicate agenta OMS s OMS. Agenti nástroje Operations Manager v této tabulce jsou OMS agentů, které jsou připojené tooOperations správce. V tématu [tooLog připojení nástroje Operations Manager Analytics](log-analytics-om-agents.md) informace o připojení vaše stávající prostředí tooOMS nástroje Operations Manager.

> [!NOTE]
> Typ Hello agenta, který používáte Určuje, jak se neposílají data tooOMS, hello následující podmínky:
> - Buď použijte agenta Windows hello nebo agenta nástroje Operations Manager připojené OMS.
> - Pokud Operations Manager je požadována, data agenta nástroje Operations Manager pro řešení hello je vždy odesílány tooOMS pomocí skupiny pro správu nástroje Operations Manager hello. Kromě toho pokud Operations Manager je požadována, pouze agenta nástroje Operations Manager hello používá hello řešení.
> - Při nástroje Operations Manager není vyžadován a hello tabulka ukazuje, že data agenta nástroje Operations Manager se neodesílají tooOMS pomocí hello skupiny pro správu a pak data agenta nástroje Operations Manager je vždy odesílány tooOMS pomocí skupin pro správu. Agenty se systémem Windows obejít hello skupiny pro správu a odesílají data přímo tooOMS.
> - Odeslání dat agenta nástroje Operations Manager není pomocí skupiny pro správu, hello data se pak odešlou přímo tooOMS – obcházení hello skupiny pro správu.

### <a name="insight--analytics--log-analytics"></a>Přehledy a analýzy / Log Analytics

| Řešení pro správu | Platforma | Agent monitorování Microsoft | Agent nástroje Operations Manager | Úložiště Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Activity Log Analytics | Azure |   |   |   |   |   | v oznámení |
| Posouzení AD |Windows |&#8226; |&#8226; |  |  |&#8226; |7 dní |
| Stav replikace AD |Windows |&#8226; |&#8226; |  |  |&#8226; |5 dní |
| Stav agenta | Systém Windows a Linux | &#8226; | &#8226; |   |   | &#8226; | 1 minuta |
| Správu výstrah (Nagios) |Linux |&#8226; |  |  |  |  |v případě přijetí |
| Správu výstrah (Zabbix) |Linux |&#8226; |  |  |  |  |1 minuta |
| Správu výstrah (Operations Manager) |Windows |  |&#8226; |  |&#8226; |&#8226; |3 minuty |
| Konektor služby Statistika aplikace (Preview) | Azure |   |   |   |   |   | v oznámení |
| Analýza brány Azure aplikace | Azure |   |   |   |   |   | v oznámení |
| Skupina zabezpečení sítě Azure Analytics | Azure |   |   |   |   |   | v oznámení |
| Analýza Azure SQL (Preview) |Windows |  |  |  |  |  | 10 minut |
| Správa kapacit |Windows |&#8226; |&#8226; |  |  |&#8226; |v případě přijetí |
| Kontejnery | Systém Windows a Linux | &#8226; | &#8226; |   |   |   | 3 minuty |
| Analýza trezoru klíčů |Windows |  |  |  |  |  |v oznámení |
| Sledování výkonu sítě | Windows | &#8226; | &#8226; |   |   |   | TCP metodou handshake každých 5 sekund, data odeslána každé 3 minuty |
| Analýza Office 365 (Preview) |Windows |  |  |  |  |  |v oznámení |
| Analýza Service Fabric |Windows |  |  |&#8226; |  |  |5 minut |
| Mapa služeb | Systém Windows a Linux | &#8226; | &#8226; |   |   |   | 15 sekund |
| Posouzení SQL |Windows |&#8226; |&#8226; |  |  |&#8226; |7 dní |
| SurfaceHub |Windows |&#8226; |  |  |  |  |v případě přijetí |
| System Center Operations Manager Assessment (Preview) | Windows | &#8226; | &#8226; |   |   | &#8226; | sedm dní |
| Upgrade Analytics (Preview) | Windows | &#8226; |   |   |   |   | 2 dny |
| VMware monitorování (Preview) | Linux | &#8226; |   |   |   |   | 3 minuty |
| Linková data |Windows (2012 R2 / 8.1 nebo novější) |&#8226; |&#8226; |  |  |  | 1 minuta |


### <a name="automation--control"></a>Automatizace a řízení

| Řešení pro správu | Platforma | Agent monitorování Microsoft | Agent nástroje Operations Manager | Úložiště Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Automatizace hybridní pracovní proces | Windows | &#8226; | &#8226; |   |   |   | neuvedeno |
| Sledování změn |Windows |&#8226; |&#8226; |  |  |&#8226; |každou hodinu |
| Sledování změn |Linux |&#8226; |  |  |  |  |každou hodinu |
| Update Management | Windows |&#8226; |&#8226; |  |  |&#8226; |aspoň 2 časy denně a 15 minut po instalaci aktualizace |

### <a name="security--compliance"></a>Zabezpečení a dodržování předpisů

| Řešení pro správu | Platforma | Agent monitorování Microsoft | Agent nástroje Operations Manager | Úložiště Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Vyhodnocení proti malwaru |Windows |&#8226; |&#8226; |  |  |&#8226; |každou hodinu |
| Zabezpečení a Audit<sup>1</sup> | Systém Windows a Linux | částečné | částečné | částečné |   | částečné | různé |

<sup>1</sup> hello zabezpečení a Audit řešení můžou shromažďovat protokoly z agentů systému Windows, Operations Manager a Linux. V tématu [zdroje dat](#data-sources) pro informace o shromažďování dat o:

- Syslog
- Protokoly událostí zabezpečení systému Windows
- Protokoly brány firewall systému Windows
- Protokoly událostí systému Windows



### <a name="protection--recovery"></a>Ochrana a obnovení

| Řešení pro správu | Platforma | Agent monitorování Microsoft | Agent nástroje Operations Manager | Úložiště Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Zálohování | Azure |   |   |   |   |   | neuvedeno |
| Azure Site Recovery | Azure |   |   |   |   |   | neuvedeno |


### <a name="data-sources"></a>Zdroje dat


| Zdroj dat | Platforma | Agent monitorování Microsoft | Agent nástroje Operations Manager | Úložiště Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Protokoly Azure aktivity |Windows |  |  |  |  |  |v oznámení |
| Azure diagnostických protokolů |Windows |  |  |  |  |  |v oznámení |
| Azure diagnostiky metriky |Windows |  |  |  |  |  |v oznámení |
| TRASOVÁNÍ UDÁLOSTÍ PRO WINDOWS |Windows |  |  |&#8226; |  |  |5 minut |
| Protokoly služby IIS |Windows |&#8226; |&#8226; |&#8226; |  |  |5 minut |
| Čítače výkonu |Windows |&#8226; |&#8226; |  |  |  |podle plánu, minimálně 10 sekund. |
| Čítače výkonu |Linux |&#8226; |  |  |  |  |podle plánu, minimálně 10 sekund. |
| Syslog |Linux |&#8226; |  |  |  |  |ze služby Azure storage: 10 minut; z agenta: na přijetí |
| Protokoly událostí zabezpečení systému Windows |Windows |&#8226; |&#8226; |&#8226; |  |  |pro úložiště Azure: 10 min; pro agenta hello: na přijetí |
| Protokoly brány firewall systému Windows |Windows |&#8226; |&#8226; |  |  |  |v případě přijetí |
| Protokoly událostí systému Windows |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; |pro úložiště Azure: 10 min; pro agenta hello: na přijetí |



## <a name="preview-management-solutions-and-features"></a>Řešení pro správu Preview a funkce
Spuštěním služby a následující postupy devops, se může toopartner s funkcí toodevelop zákazníků a řešení.

Během privátní Preview verzi jsme poskytnout malou skupinu zákazníků přístup tooan časná implementace hello funkce nebo řešení toogain připomínek a vylepšení. Tato časná implementace má minimální funkce a možnosti provozu.

Naším cílem je věcí tootry rychle, můžou se nám najít co funguje, a co nefunguje. Jsme iteraci v rámci tohoto procesu dokud hello zpětné vazby od zákazníků privátní Preview verzi hello nám informuje o tom, že je vše připraveno pro verzi public preview.

Během verzi public preview hello jsme zpřístupnit hello funkce nebo řešení pro všechny uživatele tooget další zpětnou vazbu a ověřit naše škálování a efektivitu. V průběhu této fáze:

* Funkce Preview se zobrazí na kartě nastavení hello a lze je povolit žádný uživatel.
* Náhled řešení jsou přidány prostřednictvím Galerie hello nebo pomocí skriptu.

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Co je třeba vědět o funkce verze Preview a řešení?
Nemohli jsme nadšení o nových funkcích a řešení pro správu a jsme rádi, práce s toodevelop vám je.

Funkce verze Preview a řešení nejsou správné pro všechny uživatele. Před výzvou toojoin privátní Preview verzi nebo povolení verzi public preview, zkontrolujte, zda že OK pracujete s něčím, co je ve vývoji.

Při povolování funkce Náhled přes portál hello, zobrazí se upozornění upozornění, že hello funkce je ve verzi preview.

#### <a name="for-both-private-and-public-preview"></a>Pro obě *privátní* a *veřejné* preview
Hello následující informace platí tooboth veřejné a privátní Preview:

* Věcí nemusí vždy fungovat správně.
  * Vydá rozsah nebudou menší obtěžování hlukem prostřednictvím toosomething nepracuje vůbec.
* Dojít ke hello preview toohave negativní dopad na vaše systémy nebo prostředí.
  * Pokusíme tooavoid záporné věcí dojít k situaci toohello systémů, které používáte s OMS, ale někdy neočekávané věcí.
* Ztráty dat / může dojít k poškození.
* Můžeme požádat můžete diagnostické protokoly toocollect nebo jiných dat toohelp řešení problémů.
* Funkce Hello nebo řešení může být odebrán (dočasně nebo trvale).
  * Podle našich learnings během hello preview jsme rozhodnout toonot verze hello funkce nebo řešení.
* Verze Preview nemusí fungovat nebo nemusí byly testovány s všechny konfigurace a můžeme omezit:
  * Hello operační systémy, které lze použít (například funkce pouze uplatnit tooLinux při ve verzi preview).
  * Hello typ agent (MMA, Operations Manager), které je možné (například funkce nemusí fungovat s nástrojem Operations Manager, zatímco ve verzi preview).  
* Řešení Preview a funkce nejsou předmětem hello smlouvy o úrovni služeb.
* Použití funkce verze preview způsobuje poplatky za používání.
* Funkcí nebo schopností, že je nutné pro funkci hello / řešení toobe užitečné pravděpodobně chybí nebo jsou neúplné.
* Funkce / řešení nemusí být k dispozici ve všech oblastech.
* Funkce / nemusí být lokalizovány řešení.
* Funkce / řešení může mít omezení počtu hello zákazníky nebo zařízení, které ho používají.
* Může být nutné toouse skripty tooperform konfigurace a tooenable hello řešení nebo funkce.
* Hello uživatelské rozhraní (UI) není úplný a může změnit z tooday den.
* Veřejné verze Preview nemusí být vhodné pro vaše produkční / důležité systémy.

#### <a name="for-private-preview"></a>Pro *privátní* preview
V uvedených položek toohello přidání je hello následující informace o konkrétní tooprivate Preview:

* Očekáváme, že jste tooprovide nám s zpětná vazba týkající se prostředí tak, aby bychom mohli hello funkce nebo řešení lépe.
* Můžeme vás kontaktovat zpětnou vazbu pomocí průzkumy, telefonních hovorů nebo e-mailu.
* Co není vždy fungovat správně.
* Nemůžeme může vyžadovat Non-zpřístupnění smlouvy (NDA) pro zapojení nebo může zahrnovat důvěrného obsahu.
  * Před blogu, počítačích nebo v opačném případě komunikaci s třetími stranami Zkontrolujte prosím se hello manažer programu, která je odpovědná za verzi preview toounderstand hello nějaká omezení na zpřístupnění.
* Nespouštějte u produkčních / důležité systémy.

### <a name="how-do-i-get-access-tooprivate-preview-features-and-solutions"></a>Jak získám přístup tooprivate preview funkcí a řešení?
Doporučujeme náhledy tooprivate zákazníkům prostřednictvím několika různými způsoby v závislosti na verzi preview hello.

* Odpoví hello měsíční přehled zákazníků a sdělte nám oprávnění toofollow s vámi zlepšuje pravděpodobnosti pozvané tooa privátní Preview verzi.
* Váš tým účet Microsoft můžete určit můžete.
* Můžete si zaregistrovat na základě podrobností odeslány na twitteru [msopsmgmt](https://twitter.com/msopsmgmt).
* Můžete si zaregistrovat na základě událostí sdílené komunity podrobnosti – podívejte se na splňují pro nás nepřerušitelný zdroj napájení, konferencí a v online komunit.

## <a name="next-steps"></a>Další kroky
* [V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné informace o shromažďovaných funkcí řešení pro správu.
