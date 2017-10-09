---
title: "aaaAzure řešení sítě analýzy v Log Analytics | Microsoft Docs"
description: "Můžete použít hello řešení Azure sítě analýzy protokolů skupiny zabezpečení sítě Azure tooreview analýzy protokolů a protokoly Azure Application Gateway."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 3674189786bacccc82e6708e78f14c92178e6676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a>Monitorování řešení v analýzy protokolů Azure sítě

Analýzy protokolů nabízí hello následující řešení pro monitorování vaší sítí:
* Sledování výkonu sítě (NPM) na
 * Sledování stavu hello vaší sítě
* Tooreview analytics Azure Application Gateway
 * Protokoly služby Azure Application Gateway
 * Metriky Azure Application Gateway
* Skupina zabezpečení sítě Azure analytics tooreview
 * Protokoly skupinu zabezpečení sítě Azure

## <a name="network-performance-monitor-npm"></a>Sledování výkonu sítě (NPM)

Hello [sledování výkonu sítě](log-analytics-network-performance-monitor.md) řešení správy je monitorování řešení, která sleduje stav hello, dostupnosti a dostupnosti sítě sítě.  Jedná se o použitých toomonitor připojení mezi:

* veřejný cloud a místní
* datových center a umístění uživatele (firemních pobočkách)
* podsítě hostování různé úrovně víceúrovňových aplikací.

Další informace najdete v tématu [sledování výkonu sítě](log-analytics-network-performance-monitor.md).

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Analýza Azure Application Gateway a skupinu zabezpečení sítě
toouse hello řešení:
1. Přidat hello správu řešení tooLog analýzy, a
2. Povolte diagnostiku toodirect hello diagnostiky tooa pracovní prostor analýzy protokolů. Není nutné toowrite hello protokoly tooAzure Blob storage.

Diagnostika a řešení odpovídající hello můžete povolit pro jednoho nebo obou aplikační brány a skupiny zabezpečení sítě.

Pokud jste nepovolujte protokolování diagnostiky pro konkrétní typ prostředku, ale instalaci hello řešení, hello řídicí panel oken pro tento prostředek jsou prázdné a zobrazí se chybová zpráva.

> [!NOTE]
> V ledna 2017 podporované hello způsob odesílání protokolů z tooLog aplikačních bran a skupiny zabezpečení sítě, analýza změnit. Pokud se zobrazí hello **Azure sítě Analytics (nepoužívané)** řešení, najdete příliš[migrace z řešení sítě analýzy staré hello](#migrating-from-the-old-networking-analytics-solution) kroky musíte toofollow.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Zkontrolujte podrobnosti kolekce dat sítě Azure
Hello Azure Application Gateway analýzy a řešení pro správu hello skupinu zabezpečení sítě analytics shromažďování protokolů diagnostiky přímo z Azure Application Gateway a skupiny zabezpečení sítě. Není nutné toowrite hello protokoly tooAzure úložiště objektů Blob a žádný agent je vyžadována pro shromažďování dat.

Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro Azure Application Gateway analýzy a analýzy hello skupinu zabezpečení sítě.

| Platforma | Přímé agenta | Agent systémy Center Operations Manager | Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |Při zaznamenání |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a>Analýza řešení služby Azure Application Gateway v analýzy protokolů

![Azure Application Gateway Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

Hello následující protokoly jsou podporovány pro Application Gateway:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog
* ApplicationGatewayFirewallLog

pro Application Gateway se podporují Hello následující metriky:

* propustnost 5 minut

### <a name="install-and-configure-hello-solution"></a>Instalace a konfigurace řešení hello
Použijte následující pokyny tooinstall hello a řešení analytics Azure Application Gateway hello nakonfigurovat:

1. Povolit řešení analýzy Azure Application Gateway hello z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).
2. Zapnutí protokolování diagnostiky hello [Application Gateway](../application-gateway/application-gateway-diagnostics.md) chcete toomonitor.

#### <a name="enable-azure-application-gateway-diagnostics-in-hello-portal"></a>Povolte diagnostiku Azure Application Gateway hello portálu

1. V hello portálu Azure přejděte toomonitor prostředků toohello Application Gateway
2. Vyberte *protokolů diagnostiky* tooopen hello následující stránky

   ![bitové kopie prostředku Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. Klikněte na tlačítko *zapněte diagnostiku* tooopen hello následující stránky

   ![bitové kopie prostředku Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. Klikněte na tlačítko tooturn na Diagnostika, *na* pod *stav*
5. Klikněte na políčko hello *odeslat tooLog Analytics*
6. Vyberte existující pracovní prostor analýzy protokolů, nebo vytvořit pracovní prostor
7. Klikněte na zaškrtávací políčko hello pod **protokolu** pro každou toocollect typy protokolu hello
8. Klikněte na tlačítko *Uložit* tooenable hello protokolování diagnostiky tooLog Analytics

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>Povolte diagnostiku sítě Azure pomocí prostředí PowerShell

Hello následující skript prostředí PowerShell představuje příklad, jak tooenable diagnostické protokolování pro application Gateway.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Použití Azure Application Gateway analytics
![Obrázek analytics dlaždici Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

Po kliknutí na tlačítko hello **Azure Application Gateway analytics** dlaždici na hello přehled, můžete zobrazení souhrnných informací o protokoly a potom přejít k podrobnostem toodetails pro hello následujících kategorií:

* Přístup k aplikaci brány protokolů
  * Chyby klienta a serveru pro službu Application Gateway přístup k protokolům
  * Počet požadavků za hodinu pro každý Application Gateway
  * Neúspěšné požadavky za hodinu pro každý Application Gateway
  * Chyby podle uživatelského agenta pro Application Gateway
* Výkon brány aplikace
  * Stav hostitele pro službu Application Gateway
  * Maximální a 95. percentil pro službu Application Gateway neúspěšné požadavky

![Obrázek panelu analýzy Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Obrázek panelu analýzy Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

Na hello **Azure Application Gateway analytics** řídicí panel, zkontrolujte souhrnné informace hello v jednom z okna hello a pak klikněte na jednu tooview podrobné informace na stránce vyhledávání protokolu hello.

Na žádném z hello protokolu hledání stránky můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu. Můžete také filtrovat podle výsledků hello toonarrow omezující vlastnosti.


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a>Skupina zabezpečení sítě Azure analytics řešení v analýzy protokolů

![Skupina zabezpečení sítě Azure Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

pro skupiny zabezpečení sítě jsou podporovány následující protokoly Hello:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

### <a name="install-and-configure-hello-solution"></a>Instalace a konfigurace řešení hello
Použijte následující pokyny tooinstall hello a nakonfigurujte řešení hello analýzy sítě Azure:

1. Povolit řešení analýzy hello skupinu zabezpečení sítě Azure z [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).
2. Zapnutí protokolování diagnostiky hello [skupinu zabezpečení sítě](../virtual-network/virtual-network-nsg-manage-log.md) zdroje, které má toomonitor.

### <a name="enable-azure-network-security-group-diagnostics-in-hello-portal"></a>Povolte diagnostiku skupiny zabezpečení sítě Azure hello portálu

1. V hello portálu Azure přejděte toomonitor prostředků toohello skupinu zabezpečení sítě
2. Vyberte *protokolů diagnostiky* tooopen hello následující stránky

   ![bitové kopie prostředku skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. Klikněte na tlačítko *zapněte diagnostiku* tooopen hello následující stránky

   ![bitové kopie prostředku skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. Klikněte na tlačítko tooturn na Diagnostika, *na* pod *stav*
5. Klikněte na políčko hello *odeslat tooLog Analytics*
6. Vyberte existující pracovní prostor analýzy protokolů, nebo vytvořit pracovní prostor
7. Klikněte na zaškrtávací políčko hello pod **protokolu** pro každou toocollect typy protokolu hello
8. Klikněte na tlačítko *Uložit* tooenable hello protokolování diagnostiky tooLog Analytics

### <a name="enable-azure-network-diagnostics-using-powershell"></a>Povolte diagnostiku sítě Azure pomocí prostředí PowerShell

Hello následující skript prostředí PowerShell představuje příklad, jak tooenable diagnostické protokolování pro skupiny zabezpečení sítě
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Použijte skupinu zabezpečení sítě Azure analytics
Po kliknutí na tlačítko hello **skupinu zabezpečení sítě Azure analytics** dlaždici na hello přehled, můžete zobrazení souhrnných informací o protokoly a potom přejít k podrobnostem toodetails pro hello následujících kategorií:

* Skupina zabezpečení sítě blokované toky
  * Pravidla skupiny zabezpečení sítě s blokované toky
  * Adresy MAC s blokované toky
* Skupina zabezpečení sítě povolené toky
  * Pravidla skupiny zabezpečení sítě s povolenou toky
  * Adresy MAC s povolenou toky

![Obrázek panelu analýzy skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Obrázek panelu analýzy skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

Na hello **skupinu zabezpečení sítě Azure analytics** řídicí panel, zkontrolujte souhrnné informace hello v jednom z okna hello a pak klikněte na jednu tooview podrobné informace na stránce vyhledávání protokolu hello.

Na žádném z hello protokolu hledání stránky můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu. Můžete také filtrovat podle výsledků hello toonarrow omezující vlastnosti.

## <a name="migrating-from-hello-old-networking-analytics-solution"></a>Migrace z řešení sítě analýzy staré hello
V ledna 2017 podporované hello způsob odesílání protokolů z Azure Application Gateway a skupiny zabezpečení sítě Azure tooLog Analytics se změnila. Tyto změny poskytují hello následující výhody:
+ Protokoly zapisují přímo tooLog Analytics bez hello potřebovat toouse účet úložiště
+ Menší latenci hello čase, kdy protokoly generované toothem, který je k dispozici v analýzy protokolů
+ Méně kroků konfigurace
+ Běžný formát pro všechny typy Azure diagnostics

toouse hello aktualizovat řešení:

1. [Konfigurace diagnostiky toobe odeslaných tooLog Analytics přímo z Azure Application Gateway](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [Konfigurace diagnostiky toobe odeslaný přímo tooLog Analytics skupin zabezpečení sítě Azure](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. Povolit hello *Azure Application Gateway Analytics* a hello *Analytics skupiny zabezpečení sítě Azure* řešení pomocí procesu hello popsané v [řešení přidat analýzy protokolů z Hello Galerie řešení](log-analytics-add-solutions.md)
3. Aktualizovat žádné uložené dotazy, řídicí panely nebo výstrahy toouse hello nový datový typ.
  + Typ je tooAzureDiagnostics. Můžete použít hello ResourceType toofilter tooAzure síťových protokolů.

    | Namísto: | Použití: |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + Pro každé pole, které má příponu \_s, \_d, nebo \_g v hello názvu, změna hello první znak toolower případu
   + Pro každé pole, které má příponu \_o název, hello dat je rozdělená do jednotlivých polí podle hello vnořené názvy polí.
4. Odebrat hello *Analytics sítě Azure (nepoužívané)* řešení.
  + Pokud používáte prostředí PowerShell, použijte`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`

Data jsou shromažďována před hello změn není zobrazená v nové řešení hello. Můžete dál tooquery pro tato data pomocí hello starého typu a názvy polí.

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Další kroky
* Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobné Azure diagnostická data.
