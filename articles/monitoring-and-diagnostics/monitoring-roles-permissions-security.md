---
title: "aaaGet začít s rolí, oprávnění a zabezpečení pomocí Azure monitorování | Microsoft Docs"
description: "Zjistěte, jak toouse Azure monitorování integrovaných rolí a oprávnění toorestrict přístup toomonitoring prostředky."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2686e53b-72f0-4312-bcd3-3dc1b4a9b912
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: johnkem
ms.openlocfilehash: 44fdf2a697050309480dfc164ebab51445b19bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Začínáme s rolemi, oprávnění a zabezpečení pomocí Azure monitorování
Mnoha týmy potřebovat toostrictly regulovat přístup k datům toomonitoring a nastavení. Například pokud máte členové týmu, kteří pracují výhradně na monitorování (pracovníci technické podpory, technici devops) nebo pokud používáte poskytovatel spravované služby, může být vhodné toogrant je přístup k tooonly monitorování dat při jejich toocreate možnost omezení, upravit, nebo Odstraňte prostředky. Tento článek ukazuje, jak tooquickly použít integrované monitorování RBAC role tooa uživatele v Azure nebo vytvořit vlastní vlastní role pro uživatele, který potřebuje monitorování omezenými oprávněními. Potom popisuje aspekty zabezpečení pro vaše prostředky související s monitorování Azure a jak můžete omezit přístup k datům toohello obsahují.

## <a name="built-in-monitoring-roles"></a>Vestavěné role, které monitorování
Předdefinované role Azure monitorování jsou navrženou toohelp omezení přístupu tooresources v rámci předplatného, ale ty zodpovědná za monitorování tooobtain infrastruktury a konfigurace hello dat, které potřebují. Monitorování Azure poskytuje dvě role se na pole: A monitorování Čtenář a Přispěvatel monitorování.

### <a name="monitoring-reader"></a>Monitorování čtečky
Lidé přiřadit role Čtenář monitorování hello můžete zobrazit všechna data monitorování v předplatném, ale nelze upravit žádný prostředek nebo upravit všechny prostředky související toomonitoring nastavení. Tato role je vhodný pro uživatele v organizaci, například podporu nebo operations engineers, kteří potřebují toobe moci:

* Zobrazit řídicí panely monitorování hello portálu a vytvořit vlastní privátního sledování řídicí panely.
* Dotaz pro metriky pomocí hello [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn931930.aspx), [rutiny prostředí PowerShell](insights-powershell-samples.md), nebo [rozhraní příkazového řádku a platformy](insights-cli-samples.md).
* Dotaz hello protokol aktivit pomocí portálu hello, REST API služby Azure monitorování, rutiny prostředí PowerShell nebo rozhraní příkazového řádku a platformy.
* Zobrazení hello [nastavení pro diagnostiku](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) pro prostředek.
* Zobrazení hello [protokolu profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) pro předplatné.
* Zobrazit nastavení automatického škálování.
* Zobrazení výstrah aktivity a nastavení.
* Přístup k datům Application Insights a zobrazení dat v AI Analytics.
* Vyhledávání dat pracovního prostoru analýzy protokolů (OMS) včetně data o využití pro pracovní prostor hello.
* Zobrazení skupiny pro správu analýzy protokolů (OMS).
* Načtení schématu vyhledávání hello analýzy protokolů (OMS).
* Zobrazí seznam intelligence Pack analýzy protokolů (OMS).
* Načtení a provedení analýzy protokolů (OMS) uložená hledání.
* Načtěte konfiguraci úložiště analýzy protokolů (OMS) hello.

> [!NOTE]
> Tato role není poskytnuta data toolog přístup pro čtení, která byla centra událostí přenášené datovými proudy tooan nebo uložený v účtu úložiště. [Viz níže](#security-considerations-for-monitoring-data) informace o konfiguraci přístup k prostředkům toothese.
> 
> 

### <a name="monitoring-contributor"></a>Monitorování přispěvatele
Osoby, které jsou přiřazené hello monitorování Přispěvatel role můžete zobrazit všechna data monitorování v předplatném a vytvořit nebo upravit nastavení monitorování, ale nelze upravit žádné další prostředky. Tato role je nadmnožinou hello monitorování čtečky role a je vhodný pro členy týmu monitorování nebo poskytovatele spravované služby, kteří kromě výše uvedeného toohello oprávnění také potřebovat toobe moct organizace:

* Publikujte jako sdílené řídicího panelu monitorování řídicí panely.
* Nastavit [nastavení pro diagnostiku](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) pro resource.*
* Sada hello [protokolu profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) pro subscription.*
* Nastavit výstrahy aktivity a nastavení.
* Vytvoření služby Application Insights webové testy a součásti.
* Pracovní prostor analýzy protokolů (OMS) seznamu sdílených klíčů.
* Povolit nebo zakázat intelligence Pack analýzy protokolů (OMS).
* Vytvoření a odstranit a provedení analýzy protokolů (OMS) uložená hledání.
* Vytvořte a odstranit konfiguraci úložiště analýzy protokolů (OMS) hello.

* uživatele musí být taky samostatně přidělují hello cílového prostředku (úložiště účet nebo události rozbočovače obor názvů) tooset profil protokolu nebo nastavení diagnostiky ListKeys oprávnění.

> [!NOTE]
> Tato role není poskytnuta data toolog přístup pro čtení, která byla centra událostí přenášené datovými proudy tooan nebo uložený v účtu úložiště. [Viz níže](#security-considerations-for-monitoring-data) informace o konfiguraci přístup k prostředkům toothese.
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Monitorování oprávnění a vlastní role RBAC
Pokud hello výše vestavěné role, které nesplňují požadavky přesný hello týmu, můžete [vytvořit vlastní role RBAC](../active-directory/role-based-access-control-custom-roles.md) s podrobnější oprávnění. Níže jsou hello běžných operací RBAC monitorování Azure s jejich popisy.

| Operace | Popis |
| --- | --- |
| Microsoft.Insights/AlertRules/[Read, zápisu, odstraní] |Pravidla výstrah pro čtení, zápisu a odstranění. |
| Microsoft.Insights/AlertRules/Incidents/Read |Seznam incidentů (historie hello pravidlo výstrahy se aktivuje) pro pravidla výstrah. To se vztahuje pouze toohello portálu. |
| Microsoft.Insights/AutoscaleSettings/[Read, zápisu, odstraní] |Nastavení automatického škálování pro čtení, zápisu a odstranění. |
| Microsoft.Insights/DiagnosticSettings/[Read, zápisu, odstraní] |Nastavení diagnostiky pro čtení, zápisu a odstranění. |
| Microsoft.Insights/eventtypes/digestevents/Read |Toto oprávnění je nutné pro uživatele, kteří potřebují přístup k protokolům tooActivity přes portál hello. |
| Microsoft.Insights/eventtypes/values/Read |Zobrazí seznam aktivity protokolu události (události management) v předplatném. Toto oprávnění je použít tooboth programová a portálu přístup toohello protokol aktivit. |
| Microsoft.Insights/LogDefinitions/Read |Toto oprávnění je nutné pro uživatele, kteří potřebují přístup k protokolům tooActivity přes portál hello. |
| Microsoft.Insights/MetricDefinitions/Read |Číst definice metrik (seznamu dostupných typů metriky pro prostředek). |
| Microsoft.Insights/Metrics/Read |Číst metriky pro prostředek. |

> [!NOTE]
> Tooalerts přístupu, nastavení pro diagnostiku a metriky pro prostředek vyžaduje tento hello uživatel má přístup pro čtení toohello prostředků typu a rozsahu prostředku. Vytváření ("zápis") diagnostický profil nastavení nebo protokolu, archivy tooa úložiště účet nebo datové proudy tooevent centra vyžaduje hello uživatele tooalso oprávnění ListKeys hello cílovému prostředku.
> 
> 

Například pomocí hello výše tabulky můžete vytvořit vlastní role RBAC pro "Aktivity protokolu čtečku" takto:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Důležité informace o zabezpečení pro monitorování dat.
Data monitorování – zvlášť soubory protokolu – mohou obsahovat citlivé údaje, jako je například IP adresy nebo uživatelské jméno. Monitorování dat z Azure dodává ve formulářích tři základní:

1. Hello protokol aktivit, která popisuje všechny akce v rovině řízení na vaše předplatné Azure.
2. Diagnostické protokoly, které jsou protokoly vygenerované prostředkem.
3. Metriky, které jsou vysílaných prostředky.

Všechny tři z těchto typů dat může být uložený v účtu úložiště nebo prostřednictvím datového proudu tooEvent rozbočovače, které jsou pro obecné účely prostředků Azure. Privilegované operace obvykle vyhrazena pro správce je, protože se jedná pro obecné účely prostředky, vytváření, odstraňování a k nim přistupovat. Doporučujeme použít následující postupy pro monitorování související prostředky tooprevent zneužití hello:

* Použijte účet jeden vyhrazený úložiště pro data monitorování. Pokud potřebujete tooseparate monitorování data do více účtů úložiště, nikdy sdílet využití účtu úložiště mezi monitorování a data-monitoring, jako to může poskytnout nechtěně uživatelů, kteří potřebují pouze přístup k toomonitoring dat (např. třetí strany SIEM) přístup k datům monitorování toonon.
* Používejte jednu, vyhrazené sběrnice nebo Centrum událostí názvů ve všech nastavení pro diagnostiku pro hello stejného důvodu, jak je uvedeno výše.
* Omezit tím, že jim skupinu prostředků samostatné účty pro přístup k související toomonitoring úložiště nebo event hubs a [použít obor](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) na monitorování role toolimit přístup tooonly příslušné skupině prostředků.
* Když uživatel potřebuje pouze přístup k datům toomonitoring nikdy udělit hello ListKeys oprávnění pro účty úložiště nebo služby event hubs v oboru předplatné. Místo toho poskytnout tyto uživatele toohello oprávnění na prostředek nebo skupina prostředků (Pokud máte vyhrazené monitorování skupiny prostředků) oboru.

### <a name="limiting-access-toomonitoring-related-storage-accounts"></a>Omezení týkajícího se toomonitoring úložiště účty pro přístup
Pokud uživatele nebo aplikace potřebuje přístup k datům toomonitoring v účtu úložiště, měli byste [generovat SAS účtu](https://msdn.microsoft.com/library/azure/mt584140.aspx) hello účtu úložiště, který obsahuje data monitorování s úložištěm tooblob přístup jen pro čtení na úrovni služby. V prostředí PowerShell může vypadat například:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Pak můžete udělit hello tokenu toohello entita, která potřebuje tooread z daného účtu úložiště, a můžete zobrazit seznam a čtení ze všech objektů BLOB v daném účtu úložiště.

Případně pokud potřebujete tato oprávnění se RBAC toocontrol, můžete udělit této entity hello Microsoft.Storage/storageAccounts/listkeys/action oprávnění na tuto konkrétní účet úložiště. To je nezbytné pro uživatele, kteří potřebují mít tooset toobe diagnostické nastavení nebo protokolu profil tooarchive tooa účet úložiště. Můžete například vytvořit hello následující vlastní role RBAC pro uživatele nebo aplikace, které stačí tooread z jeden účet úložiště:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get hello storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [!WARNING]
> Hello ListKeys oprávnění umožňuje hello uživatele toolist hello primární a sekundární klíčů k účtu úložiště. Tyto klíče hello uživateli udělit všechny podepsané oprávnění (čtení, zápis, vytvořit objekty BLOB, odstranit objekty BLOB, atd.) napříč všemi podepsané služby (objekt blob, fronty, tabulce, soubor) v daném účtu úložiště. Doporučujeme používat SAS účtu popsané výše, pokud je to možné.
> 
> 

### <a name="limiting-access-toomonitoring-related-event-hubs"></a>Omezení přístupu související toomonitoring služba event hubs
Podobný princip platí službou event hubs, ale nejdřív je potřeba toocreate vyhrazené autorizační pravidlo naslouchání. Pokud chcete toogrant přístup tooan aplikace, která stačí centra událostí souvisejících s toomonitoring toolisten, hello následující:

1. Vytvořte zásady sdíleného přístupu na hello události rozbočovače, které byly vytvořeny pro streamování dat monitorování s jenom deklarace identity naslouchání. To lze provést hello portálu. Například můžete ho může volat "monitoringReadOnly." Pokud je to možné můžete toogive, který klíče přímo toohello příjemce a hello další krok přeskočit.
2. Pokud příjemce hello potřebuje toobe možné tooget hello klíče ad-hoc, udělte akce ListKeys hello hello uživatele pro tohoto centra událostí. To je nezbytné pro uživatele, kteří potřebují mít tooset toobe nastavení diagnostiky nebo protokolu profil toostream tooevent rozbočovače. Například může vytvořit pravidlo RBAC:
   
   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get hello key toolisten tooan event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```

## <a name="next-steps"></a>Další kroky
* [Přečtěte si o RBAC a oprávnění ve službě Správce prostředků](../active-directory/role-based-access-control-what-is.md)
* [Přečtěte si hello Přehled monitorování v Azure](monitoring-overview.md)

