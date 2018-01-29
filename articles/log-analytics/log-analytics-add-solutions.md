---
title: "Přidat řešení pro správu Azure Log Analytics | Microsoft Docs"
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
ms.date: 01/23/2018
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d951387882a5a8f5e0ebdc01841bb8384e4848ee
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="add-azure-log-analytics-management-solutions-to-your-workspace"></a>Přidat řešení pro správu Azure Log Analytics do pracovního prostoru

Řešení pro správu protokolu Analytics jsou kolekce **logiku**, **vizualizace**, a **pravidla získávání dat** které poskytují metriky seskupit kolem konkrétní problém oblasti. Tento článek obsahuje seznam řešení pro správu nepodporuje analýzy protokolů a ukazuje, jak přidávat a odebírat pro pracovní pomocí portálu Azure. Můžete také přidat řešení na portálu OMS pomocí Galerie řešení.

Řešení pro správu povolit podrobnější přehled na:

* Pomohou prozkoumat a vyřešit provozní problémy rychlejší
* Shromáždit a korelovat různé typy dat počítače
* Abyste mohli proaktivní s aktivitami, které zpřístupňuje řešení.

> [!NOTE]
> Analýzy protokolů zahrnuje funkce hledání protokolů, takže není nutné k instalaci řešení pro správu povolit. Však získat vizualizaci dat, návrhy vyhledávání a insights přidáním řešení pro správu do pracovního prostoru.

Pomocí tohoto článku, přidat do pracovního prostoru pomocí portálu Azure Marketplace řešení pro správu. Po přidání řešení, data se budou shromažďovat ze serverů ve vaší infrastruktuře a odešle do služby OMS. Zpracování OMS služby obvykle trvá několik minut za hodinu. Po služba zpracovává data, můžete ji zobrazit v OMS.

Můžete snadno odebrat řešení pro správu, když už ho nepotřebují. Když odeberete řešení pro správu, jeho data neposílají se do OMS. Pokud jste na cenové úrovně Free, můžete snížit množství dat použít, což pomáhá dodrželi denní kvóta dat po odebrání řešení.

## <a name="view-available-management-solutions"></a>Řešení správy dostupných zobrazení

Azure marketplace obsahuje seznam [řešení pro správu pro analýzy protokolů](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Řešení pro správu můžete nainstalovat z Azure marketplace kliknutím **ho získat** odkaz na konci každé řešení.

## <a name="add-a-management-solution"></a>Přidat řešení pro správu
1. Pokud jste to ještě neudělali, přihlaste se na webu [Azure Portal](https://portal.azure.com) pomocí svého předplatného Azure.
2. V **nový** okno pod **Marketplace**, vyberte **monitorování + správu**.
3. V **monitorování + správu** okně klikněte na tlačítko **zobrazit všechny**.  
    ![Monitorování + okna Správa](./media/log-analytics-add-solutions/monitoring-management-blade.png)  
4. Napravo od **řešení pro správu**, klikněte na tlačítko **Další**.
5. V **řešení pro správu** okně vybrat řešení pro správu, který chcete přidat do pracovního prostoru.  
    ![Monitorování + okna Správa](./media/log-analytics-add-solutions/management-solutions.png)  
6. V okně řešení správy zkontrolovat informace o řešení pro správu a pak klikněte na tlačítko **vytvořit**.
7. V okně *název řešení pro správu* vyberte pracovní prostor, který chcete přidružit k příslušnému řešení pro správu.
8. Volitelně můžete změňte nastavení pracovního prostoru pro předplatné, skupinu prostředků a umístění. Můžete také **Možnosti automatizace**. Klikněte na možnost **Vytvořit**.  
    ![pracovní prostor řešení](./media/log-analytics-add-solutions/solution-workspace.png)  
9. Chcete-li začít používat řešení pro správu, kterou jste přidali do pracovního prostoru, přejděte na **analýzy protokolů** > **odběry** > ***název pracovního prostoru***  >  **Přehled**. Zobrazí se nová dlaždice pro vaše řešení pro správu. Kliknutím na dlaždici otevřít a začít používat řešení po shromáždění dat pro řešení.

## <a name="remove-a-management-solution"></a>Odebrat řešení pro správu

1. V [portál Azure](https://portal.azure.com), přejděte na **analýzy protokolů** > **odběry** > ***název pracovního prostoru*** a potom v ***název pracovního prostoru*** okně klikněte na tlačítko **řešení**.
2. V seznamu řešení pro správu vyberte řešení, které chcete odebrat.
3. V okně řešení vašeho pracovního prostoru klikněte na tlačítko **odstranit**.  
    ![Odstranit řešení](./media/log-analytics-add-solutions/solution-delete.png)  
4. V potvrzovacím dialogovém okně klikněte na tlačítko **Ano**.

## <a name="offers-and-pricing-tiers"></a>Nabídky a cenové úrovně

Následující tabulka uvádí, které patří řešení pro správu na každou nabídku Operations Management Suite.
V tabulce jsou uvedeny také cenové úrovně, které jsou dostupné pro každý řešení pro správu.
Všechna řešení v následující tabulce jsou dostupné v rámci portálu Azure a galerii řešení portálu Analýza protokolů.

| Řešení pro správu                                                                       | Nabídka                                                                     | Cenové úrovně<sup>1</sup>                                                 | Poznámky |
| ---                                                                                       | ---                                                                       | ---                                                                                                       | ---   |
| [Activity Log Analytics](log-analytics-activity.md)                                                                   | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | 90 dnů dat jsou k dispozici zdarma<br>Data nejsou předmětem krytky úroveň Free |
| [Posouzení AD](log-analytics-ad-assessment.md)                                           | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Stav replikace AD](log-analytics-ad-replication-status.md)                           | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Chcete-li přidat z portálu Azure nebo marketplace není k dispozici. |
| [Stav agenta](../operations-management-suite/oms-solution-agenthealth.md)                                                                                | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Data nejsou předmětem krytky úroveň Free<br> Chcete-li přidat z portálu Azure nebo marketplace není k dispozici. |
| [Správa výstrah](log-analytics-solution-alert-management.md)                            | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Chcete-li přidat z portálu Azure nebo marketplace není k dispozici. |
| [Konektor služby Statistika aplikace (Preview)](log-analytics-app-insights-connector.md)                                               | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Automatizace hybridní pracovní proces](../automation/automation-hybrid-runbook-worker.md)                                                                     | <ul><li>Automatizace a řízení</li></ul>                                  | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | Vyžaduje pracovní prostor analýzy protokolů propojení na účet služby Automation. |
| [Analýza brány Azure aplikace](log-analytics-azure-networking-analytics.md)    | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Skupina zabezpečení sítě Azure Analytics](log-analytics-azure-networking-analytics.md)     | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Analýza Azure SQL (Preview)](log-analytics-azure-sql.md)                                                       | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br>Za&nbsp;uzlu&nbsp;(OMS)                                                                          | Vyžaduje pracovní prostor analýzy protokolů propojení na účet služby Automation.|
| [Azure Web Apps Analytics](log-analytics-azure-web-apps-analytics.md)     | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
|[Backup](../backup/backup-introduction-to-azure-backup.md)                                                                                 | <ul><li>Statistiky a analýza</li></ul>                                   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                       | Vyžaduje classic úložiště záloh.<br> Chcete-li přidat z portálu Azure nebo marketplace není k dispozici. |
| [Kapacitu a výkon (Preview)](log-analytics-capacity.md)                                                   | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Sledování změn](log-analytics-change-tracking.md)                                       | <ul><li>Automatizace a řízení</li></ul>                                  | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | Vyžaduje pracovní prostor analýzy protokolů propojení na účet služby Automation. |
| [Kontejnery](log-analytics-containers.md)                                                 | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [IT Service Management Connector](log-analytics-itsmc-overview.md)                                                | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)     | |
| HDInsight HBase monitorování <br>(Preview)                                                  | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Analýza služby Key Vault](log-analytics-azure-key-vault.md)                   | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Logiku aplikace B2B](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)                    | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Chcete-li přidat z portálu Azure nebo marketplace není k dispozici. |
| [Posouzení malwaru](log-analytics-malware.md)                                            | <ul><li>Zabezpečení a dodržování předpisů</li></ul>                                 | Free<br> Standalone<br>Za&nbsp;uzlu&nbsp;(OMS)                                                                           | Pokud přidáte řešení zabezpečení a dodržování předpisů po 19 června 2017 [fakturuje se podle uzlu](https://azure.microsoft.com/pricing/details/security-compliance/), bez ohledu na to pracovního prostoru cenová úroveň. Prvních 60 dní jsou volné.  |
| [Sledování výkonu sítě](log-analytics-network-performance-monitor.md) <br>  | <ul><li>Statistiky a analýza</li></ul>                                   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | |
| [Analýza Office 365 (Preview)](../operations-management-suite/oms-solution-office-365.md)                                                       | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Zabezpečení a audit](../operations-management-suite/oms-security-getting-started.md)      | <ul><li>Zabezpečení&nbsp;a&nbsp;dodržování předpisů</li></ul>                       | Free<br> Standalone<br>Za&nbsp;uzlu&nbsp;(OMS)                                                                           | Shromažďování protokolů událostí zabezpečení vyžaduje toto řešení<br>Pokud přidáte řešení zabezpečení a dodržování předpisů po 19 června 2017 [fakturuje se podle uzlu](https://azure.microsoft.com/pricing/details/security-compliance/), bez ohledu na to pracovního prostoru cenová úroveň. Prvních 60 dní jsou volné. |
| [Služba Fabric Analytics (Preview)](log-analytics-service-fabric.md)                     | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Mapa služeb (Preview)](../operations-management-suite/operations-management-suite-service-map.md) | <ul><li>Statistiky a analýza</li></ul>                      | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | K dispozici ve východní USA, západní Evropa a střed USA – západ    |
| [Site Recovery](../site-recovery/site-recovery-overview.md)                                                                               | <ul><li>Statistiky a analýza</li></ul>                                   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                       | Vyžaduje classic trezoru Site Recovery.<br> Chcete-li přidat z portálu Azure nebo marketplace není k dispozici. |
| [Posouzení SQL](log-analytics-sql-assessment.md)                                         | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| Spuštění/zastavení virtuálních počítačů mimo špičku<br>(Preview)                                              | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | Vyžaduje pracovní prostor analýzy protokolů propojení na účet služby Automation. |
| [SurfaceHub](log-analytics-surface-hubs.md)                                               | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Chcete-li přidat z portálu Azure nebo marketplace není k dispozici. |
| [System Center Operations Manager Assessment (Preview)](log-analytics-scom-assessment.md)  | <ul><li>Statistiky a analýza</li><li>Log Analytics</li></ul>        | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Správa aktualizací](../operations-management-suite/oms-solution-update-management.md)                                                                         | <ul><li>Automatizace a řízení</li></ul>                                  | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | Vyžaduje pracovní prostor analýzy protokolů propojení na účet služby Automation. |
| [Dodržování předpisů pro aktualizaci (Preview)](https://docs.microsoft.com/windows/deployment/update/update-compliance-get-started)                                                             | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Bez poplatků dat nebo uzly<br>Data, která není předmětem krytky úroveň Free.<br> Chcete-li přidat z portálu Azure nebo marketplace není k dispozici. |
| [Upgrade Readiness](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started)                                                          | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | Bez poplatků dat nebo uzly<br>Data, která není předmětem krytky úroveň Free.<br> Chcete-li přidat z portálu Azure nebo marketplace není k dispozici. |
| [VMware monitorování (Preview)](log-analytics-vmware.md)                                | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(Standalone)<br> Za&nbsp;uzlu&nbsp;(OMS)   | |
| [Wire Data 2.0 (Preview)](log-analytics-wire-data.md)                                                                 | <ul><li>Statistiky a analýza</li></ul>                                   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)                                                                         | K dispozici ve východní USA, západní Evropa a střed USA – západ |

<sup>1</sup> *standardní* a *Premium (OMS)* cenové úrovně jsou dostupné pouze pro zákazníky, kteří vytvořili jejich pracovní prostor analýzy protokolů před 21 září 2016.

### <a name="community-provided-management-solutions"></a>Komunita poskytuje řešení pro správu

Jsou k dispozici z komunity poskytuje řešení [šablony Azure Galerie](https://azure.microsoft.com/resources/templates/?term=Per&nbsp;Node&nbsp;(OMS)) a direct od autorů.

| Řešení pro správu               | Nabídka                                                                     | Cenové úrovně                         | Poznámky |
| ---                               | ---                                                                       | ---                                   | ---   |
| Všechna řešení pro zadaný komunity  | <ul><li>Insight&nbsp;and&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Free<br> Za&nbsp;uzlu&nbsp;(OMS)     |   Vyžaduje pracovní prostor analýzy protokolů propojení na účet služby Automation. |




## <a name="data-collection-details"></a>Podrobnosti kolekce dat
Následující tabulky popisují metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují řešení pro správu analýzy protokolů a datových zdrojů. V tabulkách jsou zařazené do kategorie nabízí řešení, které rovnat [předplatné cenové úrovně](https://go.microsoft.com/fwlink/?linkid=827926). Analýzy protokolů aktivity řešení je k dispozici pro všechny cenové úrovně zdarma.

Agent služby Windows analýzy protokolů a agenta System Center Operations Manager jsou v podstatě stejné. Agent služby Windows zahrnuje další funkce tak, aby ji k připojení k pracovním prostorem OMS a směrovat přes proxy server. Pokud používáte agenta nástroje Operations Manager, musí být cílem jako agenta OMS ke komunikaci s OMS. Agenti nástroje Operations Manager v této tabulce jsou OMS agentů, které jsou připojené k nástroji Operations Manager. V tématu [připojení nástroje Operations Manager k analýze protokolů](log-analytics-om-agents.md) informace o připojení vaše stávající prostředí nástroje Operations Manager k OMS.

> [!NOTE]
> Typ agenta, který používáte Určuje, jak data se odešlou do OMS, s následující podmínky:
> - Můžete buď použít agent služby Windows nebo agenta nástroje Operations Manager připojit OMS.
> - Pokud Operations Manager je požadována, data agenta nástroje Operations Manager pro řešení vždy posílá OMS pomocí skupiny pro správu nástroje Operations Manager. Kromě toho pokud Operations Manager je požadována, pouze agenta nástroje Operations Manager používá řešení.
> - Pokud Operations Manager není vyžadován a v tabulce jsou uvedeny dat agenta nástroje Operations Manager posílá OMS pomocí skupiny pro správu a potom data agenta nástroje Operations Manager vždy posílá OMS pomocí skupin pro správu. Agenty se systémem Windows vynechat skupině pro správu a odesílají data přímo do OMS.
> - Odeslání dat agenta nástroje Operations Manager není pomocí skupiny pro správu, data se pak odešlou přímo do OMS – obcházení skupině pro správu.

### <a name="insight--analytics--log-analytics"></a>Přehledy a analýzy / Log Analytics

| Řešení pro správu | Platforma | Agent monitorování Microsoft | Agent nástroje Operations Manager | Úložiště Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Activity Log Analytics | Azure |   |   |   |   |   | v oznámení |
| Posouzení AD |Windows |&#8226; |&#8226; |  |  |&#8226; |7 dní |
| Stav replikace AD |Windows |&#8226; |&#8226; |  |  |&#8226; |5 dní |
| Stav agenta | Windows a Linux | &#8226; | &#8226; |   |   | &#8226; | 1 minuta |
| Správu výstrah (Nagios) |Linux |&#8226; |  |  |  |  |v případě přijetí |
| Správu výstrah (Zabbix) |Linux |&#8226; |  |  |  |  |1 minuta |
| Správu výstrah (Operations Manager) |Windows |  |&#8226; |  |&#8226; |&#8226; |3 minuty |
| Konektor služby Statistika aplikace (Preview) | Azure |   |   |   |   |   | v oznámení |
| Analýza brány Azure aplikace | Azure |   |   |   |   |   | v oznámení |
| Skupina zabezpečení sítě Azure Analytics | Azure |   |   |   |   |   | v oznámení |
| Analýza Azure SQL (Preview) |Windows |  |  |  |  |  | 10 minut |
| Správa kapacit |Windows |&#8226; |&#8226; |  |  |&#8226; |v případě přijetí |
| Kontejnery | Windows a Linux | &#8226; | &#8226; |   |   |   | 3 minuty |
| Analýza trezoru klíčů |Windows |  |  |  |  |  |v oznámení |
| Sledování výkonu sítě | Windows | &#8226; | &#8226; |   |   |   | TCP metodou handshake každých 5 sekund, data odeslána každé 3 minuty |
| Analýza Office 365 (Preview) |Windows |  |  |  |  |  |v oznámení |
| Analýza Service Fabric |Windows |  |  |&#8226; |  |  |5 minut |
| Mapa služeb | Windows a Linux | &#8226; | &#8226; |   |   |   | 15 sekund |
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
| Zabezpečení a Audit<sup>1</sup> | Windows a Linux | částečné | částečné | částečné |   | částečné | různé |

<sup>1</sup> řešení zabezpečení a Audit můžete shromažďovat protokoly ze systému Windows, Operations Manager a Linux agenty. V tématu [zdroje dat](#data-sources) pro informace o shromažďování dat o:

- Syslog
- Protokoly událostí zabezpečení systému Windows
- Protokoly brány firewall systému Windows
- Protokoly událostí systému Windows



### <a name="protection--recovery"></a>Ochrana a obnovení

| Řešení pro správu | Platforma | Agent monitorování Microsoft | Agent nástroje Operations Manager | Úložiště Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Backup | Azure |   |   |   |   |   | neuvedeno |
| Azure Site Recovery | Azure |   |   |   |   |   | neuvedeno |


### <a name="data-sources"></a>Zdroje dat


| Zdroj dat | Platforma | Agent monitorování Microsoft | Agent nástroje Operations Manager | Úložiště Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Protokoly Azure aktivity |Windows |  |  |  |  |  |v oznámení |
| Azure diagnostických protokolů |Windows |  |  |  |  |  |v oznámení |
| Azure diagnostiky metriky |Windows |  |  |  |  |  |v oznámení |
| ETW |Windows |  |  |&#8226; |  |  |5 minut |
| Protokoly služby IIS |Windows |&#8226; |&#8226; |&#8226; |  |  |5 minut |
| Čítače výkonu |Windows |&#8226; |&#8226; |  |  |  |podle plánu, minimálně 10 sekund. |
| Čítače výkonu |Linux |&#8226; |  |  |  |  |podle plánu, minimálně 10 sekund. |
| Syslog |Linux |&#8226; |  |  |  |  |ze služby Azure storage: 10 minut; z agenta: na přijetí |
| Protokoly událostí zabezpečení systému Windows |Windows |&#8226; |&#8226; |&#8226; |  |  |pro úložiště Azure: 10 min; pro agenta: na přijetí |
| Protokoly brány firewall systému Windows |Windows |&#8226; |&#8226; |  |  |  |v případě přijetí |
| Protokoly událostí systému Windows |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; |pro úložiště Azure: 10 min; pro agenta: na přijetí |



## <a name="preview-management-solutions-and-features"></a>Řešení pro správu Preview a funkce
Spuštěním služby a následující postupy devops, jsou možné u zákazníků k vývoji funkcí a řešení partnerů.

Během privátní Preview verzi jsme poskytnout malou skupinu zákazníkům přístup k časná implementace funkce nebo řešení pro získání zpětné vazby a vylepšení. Tato časná implementace má minimální funkce a možnosti provozu.

Naším cílem je vyzkoušet věcí rychle a můžete se nám najít co funguje, a co nefunguje. Jsme iteraci v rámci tohoto procesu dokud zpětné vazby od zákazníků privátní Preview verzi nám informuje o tom, že je vše připraveno pro verzi public preview.

Ve veřejné verzi Preview můžeme zpřístupnit funkce nebo řešení pro všechny uživatele na základě názorů a ověřit naše škálování a efektivitu. V průběhu této fáze:

* Funkce Preview se zobrazí na kartě nastavení a lze je povolit žádný uživatel.
* Náhled řešení jsou přidány prostřednictvím Galerie nebo pomocí skriptu.

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Co je třeba vědět o funkce verze Preview a řešení?
Nemohli jsme nadšení o nových funkcích a řešení pro správu a budeme rádi práci se můžete k jejich vývoji.

Funkce verze Preview a řešení nejsou správné pro všechny uživatele. Před požádá o připojení k privátní Preview verzi nebo povolení verzi public preview, zkontrolujte, zda OK práci s něčím, co je ve vývoji.

Při povolování funkce Náhled přes portál, zobrazí se upozornění připomenutím, že tato funkce je ve verzi preview.

#### <a name="for-both-private-and-public-preview"></a>Pro obě *privátní* a *veřejné* preview
Následující informace platí pro veřejné a privátní Preview:

* Věcí nemusí vždy fungovat správně.
  * Vydá rozsah nebudou menší obtěžování hlukem prostřednictvím něco nefunguje vůbec.
* Je možné mít negativní dopad na vaše systémy ve verzi Preview nebo prostředí.
  * Pokusíme se vyhnout záporné věcí, které používáte s OMS ale někdy neočekávané věcí nastat děje na systémy.
* Ztráty dat / může dojít k poškození.
* Možná vás požádáme o shromažďování diagnostických protokolů nebo jiná data při řešení problémů.
* Funkce nebo řešení může být odebrán (dočasně nebo trvale).
  * Podle našich learnings ve verzi Preview jsme rozhodnout pro vydané funkce nebo řešení.
* Verze Preview nemusí fungovat nebo nemusí byly testovány s všechny konfigurace a můžeme omezit:
  * Operační systémy, které je možné (například funkce může platit pouze pro Linux, zatímco ve verzi preview).
  * Typ agent (MMA, Operations Manager), které je možné (například funkce nemusí fungovat s nástrojem Operations Manager, zatímco ve verzi preview).  
* Řešení Preview a funkce nejsou předmětem smlouvy o úrovni služeb.
* Použití funkce verze preview způsobuje poplatky za používání.
* Funkcí nebo schopností, že je nutné pro funkci / řešení, aby byla užitečná pravděpodobně chybí nebo jsou neúplné.
* Funkce / řešení nemusí být k dispozici ve všech oblastech.
* Funkce / nemusí být lokalizovány řešení.
* Funkce / řešení může mít omezení počtu zákazníků nebo zařízení, které ho používají.
* Musíte použít skripty, chcete-li provést konfiguraci a povolit řešení nebo funkce.
* Uživatelské rozhraní (UI) není úplný a může změnit den.
* Veřejné verze Preview nemusí být vhodné pro vaše produkční / důležité systémy.

#### <a name="for-private-preview"></a>Pro *privátní* preview
Kromě výše uvedených položek je specifická pro privátní Preview následující informace:

* Očekáváme, abyste nám poskytnout zpětnou vazbu na prostředí, aby jsme funkce nebo řešení zajistit lepší.
* Můžeme vás kontaktovat zpětnou vazbu pomocí průzkumy, telefonních hovorů nebo e-mailu.
* Co není vždy fungovat správně.
* Nemůžeme může vyžadovat Non-zpřístupnění smlouvy (NDA) pro zapojení nebo může zahrnovat důvěrného obsahu.
  * Před blogu, počítačích nebo v opačném případě komunikaci s třetími stranami zkontrolujte pomocí programu Správce odpovědný za verzi preview pochopit nějaká omezení na zpřístupnění.
* Nespouštějte u produkčních / důležité systémy.

### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Jak lze získat přístup k privátní Preview verzi funkcí a řešení?
Doporučujeme zákazníkům privátní Preview prostřednictvím několika různými způsoby v závislosti na verzi preview.

* Odpoví měsíční přehled zákazníků a sdělte nám oprávnění pro následnou akci s vámi zvyšuje pravděpodobnost pozvat na privátní Preview verzi.
* Váš tým účet Microsoft můžete určit můžete.
* Můžete si zaregistrovat na základě podrobností odeslány na twitteru [msopsmgmt](https://twitter.com/msopsmgmt).
* Můžete si zaregistrovat na základě událostí sdílené komunity podrobnosti – podívejte se na splňují pro nás nepřerušitelný zdroj napájení, konferencí a v online komunit.

## <a name="next-steps"></a>Další postup
* [V protokolech Hledat](log-analytics-log-searches.md) k zobrazení podrobných informací shromažďovaných funkcí řešení pro správu.
