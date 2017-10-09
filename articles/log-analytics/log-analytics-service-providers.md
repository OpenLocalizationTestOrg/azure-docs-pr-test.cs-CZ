---
title: "aaaLog analýzy funkce pro poskytovatele služeb | Microsoft Docs"
description: "Analýzy protokolů může pomoci spravovat poskytovatelé služeb (MSPs), velké podniky nezávislí výrobci softwaru (ISV) a poskytovatele hostitelských služeb, spravovat a monitorovat servery v jeho místní nebo cloudovou infrastrukturu."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: richrund
ms.openlocfilehash: 3c0a93232293f90385c6c724be436cee1cf855f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-features-for-service-providers"></a>Funkce analýzy protokolů pro poskytovatele služeb
Analýzy protokolů může pomoct zprostředkovatelé spravované služby (MSPs), velké podniky, nezávislí dodavatelé softwaru (ISV) a poskytovatele hostitelských služeb spravovat a monitorovat servery v jeho místní nebo cloudovou infrastrukturu. 

Velké podniky sdílet mnoho společného s poskytovatelé služeb, zejména v případě, že je centralizované IT tým, který je zodpovědný za správu IT pro mnoho různých podnikových jednotek. Tento dokument používá pro jednoduchost, termín hello *poskytovatele služeb* ale hello stejné funkce je k dispozici také pro podniky a dalších zákazníků.

## <a name="cloud-solution-provider"></a>Cloud Solution Provider
Pro partnery a poskytovatelé, kteří jsou součástí hello [Cloud Solution Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programu, analýzy protokolů je jedním z hello služby Azure k dispozici na předplatného poskytovatele CSP. 

Pro analýzu protokolu hello následující funkce jsou povolené v *poskytovatele Cloud Solution Provider* odběry.

Jako *poskytovatele Cloud Solution Provider* můžete:

* Vytváření pracovních prostorů analýzy protokolů v rámci předplatného klienta (zákazníka).
* Pracovní prostory přístup vytvořené klienty. 
* Přidání a odebrání prostoru toohello přístupu uživatele pomocí správy Azure uživatelů. Když v pracovním prostoru klienta v hello OMS stránce správy portálu hello uživatele v části nastavení není k dispozici
  * Analýzy protokolů nepodporuje přístupu podle rolí ještě - poskytnutí uživatele `reader` oprávnění v hello portál Azure umožňuje jejich změny konfigurace toomake na portálu OMS hello

toolog v předplatném tooa klienta, budete potřebovat identifikátor klienta toospecify hello. identifikátor klienta Hello je často poslední část hello e-mailová adresa použitá toosign v.

* Na portálu OMS hello, přidejte `?tenant=contoso.com` v adrese URL hello hello portálu. Například `mms.microsoft.com/?tenant=contoso.com`.
* V prostředí PowerShell, použijte hello `-Tenant contoso.com` parametr při použití `Add-AzureRmAccount` rutiny
* identifikátor klienta Hello je automaticky přidáno, jakmile použijete hello `OMS portal` propojit z hello Azure portálu tooopen a přihlaste se na portálu OMS toohello pro hello vybrané pracovní prostor

Jako *zákazníka* z poskytovatele cloudových řešení, můžete:

* Vytvořit protokol analýzy pracovní prostory v předplatném zprostředkovatele kryptografických služeb
* Pracovní prostory přístup vytvořené hello zprostředkovatele kryptografických služeb
  * Použití hello `OMS portal` propojit z hello Azure portálu tooopen a přihlaste se na portálu OMS toohello pro hello vybrané pracovní prostor
* Zobrazení a používání hello stránce uživatele správu v části nastavení na portálu OMS hello

> [!NOTE]
> Hello zahrnuté zálohování a Site Recovery řešení pro analýzu protokolu nejsou možné tooconnect trezor služeb zotavení tooa a nejde nakonfigurovat v předplatného poskytovatele CSP. 
> 
> 

## <a name="managing-multiple-customers-using-log-analytics"></a>Správa více zákazníků pomocí analýzy protokolů
Doporučuje se vytvořit pracovní prostor analýzy protokolů pro každého zákazníka, které spravujete. Pracovní prostor analýzy protokolů poskytuje:

* Geografické umístění pro data toobe uložené. 
* Členitost fakturace 
* Izolaci dat 
* Jedinečné konfigurace

Vytvořením pracovního prostoru pro zákazníka, jsou možné tookeep data každého zákazníka samostatné a také sledovat využití hello každého zákazníka.

Další informace o kdy a proč toocreate několik pracovních prostorů je popsán v [Správa přístupu toolog analytics](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Vytvoření a konfigurace pracovních prostorů zákazníka je možné automatizovat pomocí [prostředí PowerShell](log-analytics-powershell-workspace-configuration.md), [šablony Resource Manageru](log-analytics-template-workspace-configuration.md), nebo pomocí hello [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

Hello použití šablony Resource Manageru pro konfiguraci pracovního prostoru vám umožní toohave hlavní konfigurace, které je možné použít toocreate a nakonfigurovat pracovní prostory. Můžete být jisti, jako pracovní prostory, které jsou vytvořeny pro zákazníky, které budou automaticky nakonfigurované tooyour požadavky. Při aktualizaci požadavků na hello šablona se aktualizuje a pak ho znovu použít stávající pracovní prostory hello. Tento proces zajišťuje, že i existující pracovní prostory splňují vaše standardy nové.    

Při správě více analýzy protokolů pracovních prostorů, doporučujeme integraci do stávajícího systému systém lístků podpory každém pracovním prostoru / konzole operations console pomocí hello [výstrahy](log-analytics-alerts.md) funkce. Díky integraci do stávajících systémů, můžete pracovníky technické podpory dál toofollow jejich známých procesy. Analýzy protokolů pravidelně kontroluje každý pracovní prostor proti hello upozornění zadaných kritérií a vygeneruje výstrahu, když akce je vyžadována.

Pro přizpůsobené zobrazení dat, použijte hello [řídicí panel](../azure-portal/azure-portal-dashboards.md) funkci hello portálu Azure.  

Pro executive úroveň sestavy, které shrnout data napříč pracovních prostorů můžete použít hello integrace mezi analýzy protokolů a [PowerBI](log-analytics-powerbi.md). Pokud potřebujete toointegrate s jiným systémem generování sestav, můžete použít hello rozhraní API služby Search (pomocí prostředí PowerShell nebo [REST](log-analytics-log-search-api.md)) toorun dotazy a export výsledků hledání.

## <a name="next-steps"></a>Další kroky
* Automatizovat vytvoření a konfigurace pomocí pracovních prostorů [šablony Resource Manageru](log-analytics-template-workspace-configuration.md)
* Automatizovat vytváření pracovních prostorů pomocí [prostředí PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Použití [výstrahy](log-analytics-alerts.md) toointegrate s existujícími systémy
* Souhrnné sestavy pomocí [PowerBI](log-analytics-powerbi.md)

