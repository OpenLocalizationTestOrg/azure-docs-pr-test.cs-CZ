---
title: aaaAzure operace poskytovatele Resource Manager | Microsoft Docs
description: "Podrobnosti o hello operací dostupných na hello zprostředkovatelé prostředků Microsoft Azure Resource Manager"
services: active-directory
documentationcenter: 
author: jboeshart
manager: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: jaboes
ms.openlocfilehash: 2d2f912ecbade335667d68fdc42ce03a2930a0eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Operace v Azure Resource Manager poskytovatele prostředků

Tento dokument uvádí hello operací dostupných pro každý poskytovatel prostředků Microsoft Azure Resource Manager. Ty lze použít ve vlastní role tooprovide granulární řízení přístupu na základě Role (RBAC) tooresources oprávnění v Azure. Poznámka: Toto není kompletní seznam a operace může přidat nebo odebrat, protože každý poskytovatel je aktualizovat. Operace řetězce podle hello formát `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Pro komplexní a aktuální seznam použijte `Get-AzureRmProviderOperation` (v prostředí PowerShell) nebo `azure provider operations show` (v Azure CLI) toolist operations poskytovatelů prostředků Azure.

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Operace | Popis |
|---|---|
|/ configuration/akce|Konfigurace aktualizací klienta.|
|/ services/akce|Aktualizace instance služby v klientovi hello.|
|/ configuration/zápisu|Vytvoří konfiguraci klienta.|
|/Configuration/Read|Přečte hello konfiguraci klienta.|
|/ services/zápisu|Vytvoří instanci služby v klientovi hello.|
|/Services/Read|Přečte hello instance služby v klientovi hello.|
|/Services/DELETE|Odstraní instanci služby v klientovi hello.|
|/Services/servicemembers/Action|Vytvoří instanci člen služby ve službě hello.|
|/Services/servicemembers/Read|Přečte instanci člen hello služby ve službě hello.|
|/Services/servicemembers/DELETE|Odstraní instanci člen služby ve službě hello.|
|/Services/servicemembers/Alerts/Read|Přečte hello výstrahy pro člena služby.|
|/Services/Alerts/Read|Přečte hello výstrahy pro službu.|
|/Services/Alerts/Read|Přečte hello výstrahy pro službu.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Operace | Popis |
|---|---|
|/ generateRecommendations nebo akce|Vygeneruje doporučení|
|/ suppressions nebo akce|Vytvoření nebo aktualizace suppressions|
|/ registrace nebo akce|Zaregistruje hello předplatné pro Microsoft Advisor hello|
|/generateRecommendations/Read|Získá generovat stav doporučení|
|/Recommendations/Read|Přečte doporučení|
|/suppressions/Read|Získá suppressions|
|/suppressions/DELETE|Odstraní potlačení|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Operace | Popis |
|---|---|
|/Servers/Read|Načte informace hello hello zadat Analysis serveru.|
|/ servery a zápis|Vytvoří nebo aktualizuje hello zadaný Analysis serveru.|
|/Servers/DELETE|Odstranění hello Analysis serveru.|
|/Servers/suspend/Action|Pozastaví hello Analysis serveru.|
|/Servers/Resume/Action|Obnoví hello Analysis serveru.|
|/ servery/checkNameAvailability<br>/action|Kontroluje, zda zadaný Analysis Server je platný název a ne v používání.|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Operace | Popis |
|---|---|
|/ checkNameAvailability nebo akce|Ověří, zda za předpokladu, že název služby je k dispozici|
|/ registrace nebo akce|Registrace předplatného pro poskytovatele prostředků Microsoft.ApiManagement|
|nebo zrušit registraci nebo akce|Zrušení registrace předplatného pro poskytovatele prostředků Microsoft.ApiManagement|
|/ service/zápisu|Vytvoření nové instance služby API Management|
|/Service/Read|Číst metadata pro instanci služby API Management|
|/Service/DELETE|Odstranění instance služby API Management|
|/Service/updatehostname/Action|Instalační program, aktualizovat nebo odebrat vlastní názvy domén pro služby API Management|
|/Service/uploadcertificate/Action|Nahrajte certifikát SSL pro službu správy rozhraní API|
|/Service/Backup/Action|Zálohování služby API Management toohello zadaný kontejner v uživatel zadaný účet úložiště|
|/Service/Restore/Action|Obnovení služby API Management ze zadaného kontejneru hello v uživatel zadaný účet úložiště|
|/Service/managedeployments/Action|Změnit SKU nebo jednotky, přidat nebo odebrat místní nasazení služby API Management|
|/Service/getssotoken/Action|Získá token jednotného přihlašování, které můžou být použité toologin starší verze služby API Management portálu jako správce|
|/Service/applynetworkconfigurationupdates/Action|Aktualizace hello Microsoft.ApiManagement prostředky spuštěna ve virtuální síti toopick aktualizovat nastavení sítě.|
|/Service/operationresults/Read|Získá aktuální stav operace probíhající dlouhou dobu|
|/Service/networkStatus/Read|Získá stav přístupu hello síťových prostředků.|
|/Service/loggers/Read|Získání seznamu protokolovačů nebo získat podrobnosti o protokolovacího nástroje|
|/Service/loggers/Write|Přidat nové protokoly nebo aktualizovat existující podrobnosti protokolovacího nástroje|
|/Service/loggers/DELETE|Odeberte existující protokolovacího nástroje.|
|/Service/Users/Read|Získat seznam registrovaní uživatelé nebo získat podrobnosti o účtu uživatele|
|/Service/Users/Write|Zaregistrovat nového uživatele nebo účet podrobnosti aktualizace stávajícího uživatele|
|/Service/Users/DELETE|Odebrat uživatelský účet|
|/Service/Users/generateSsoUrl/Action|Generování adresy URL jednotné přihlašování. Adresa URL Hello může být použité tooaccess portál pro správu|
|/Service/Users/Subscriptions/Read|Získat seznam uživatelů odběrů|
|/Service/Users/Keys/Read|Získat seznam klíčů uživatele|
|/Service/Users/groups/Read|Získání seznamu skupin uživatelů|
|/Service/tenant/operationResults/Read|Získejte seznam výsledky operace nebo získat výsledek určité operace|
|/Service/tenant/Policy/Read|Získání zásad konfigurace pro klienta hello|
|/Service/tenant/Policy/Write|Nastavení zásad konfigurace pro klienta hello|
|/Service/tenant/Policy/DELETE|Odebrat konfiguraci zásad pro klienta hello|
|/Service/tenant/Configuration/Save/Action|Vytvoří potvrzení se konfigurace snímku toohello zadaný větve v úložišti hello|
|/Service/tenant/Configuration/Deploy/Action|Spustí úlohu nasazení tooapply změny z hello zadaný git větve toohello konfigurace v databázi.|
|/Service/tenant/Configuration/Validate/Action|Ověří změny z větve zadaný git hello|
|/Service/tenant/Configuration/operationResults/Read|Získejte seznam výsledky operace nebo získat výsledek určité operace|
|/Service/tenant/Configuration/syncState/Read|Získat stav poslední synchronizace git|
|/Service/tenant/Access/Read|Získat podrobné informace o klientovi přístup|
|/Service/tenant/Access/Write|Aktualizovat podrobnosti o informace o přístupu klienta|
|/Service/tenant/Access/regeneratePrimaryKey/Action|Znova vygenerovat primární přístupový klíč|
|/Service/tenant/Access/regenerateSecondaryKey/Action|Znova vygenerovat sekundární přístupový klíč|
|/Service/identityProviders/Read|Získání seznamu zprostředkovatelů Identity nebo získáte podrobnosti zprostředkovatele Identity|
|/Service/identityProviders/Write|Vytvořit nový zprostředkovatel Identity nebo aktualizace podrobnosti existujícího poskytovatele Identity|
|/Service/identityProviders/DELETE|Odeberte existující zprostředkovatele Identity|
|/Service/Subscriptions/Read|Získat seznam předplatných produktů nebo získat podrobnosti o odběru produktu|
|/Service/Subscriptions/Write|Přihlášení k odběru existující uživatele tooan existující produkt nebo aktualizovat existující podrobnosti o předplatném. Tato operace může být použité toorenew předplatného|
|/Service/Subscriptions/DELETE|Odstraňte odběr. Tato operace může být použité toodelete předplatného|
|/Service/Subscriptions/regeneratePrimaryKey/Action|Znova vygenerovat primární klíč předplatného|
|/Service/Subscriptions/regenerateSecondaryKey/Action|Znova vygenerovat sekundární klíč předplatného|
|/Service/backends/Read|Získání seznamu back-EndY nebo získat podrobnosti o back-end|
|/Service/backends/Write|Přidat nový back-end nebo aktualizovat existující podrobnosti back-end|
|/Service/backends/DELETE|Odeberte existující back-end|
|/Service/APIs/Read|Získání seznamu všech registrované rozhraní API nebo Get podrobné informace o rozhraní API|
|/Service/APIs/Write|Nové rozhraní API umožňuje vytvořit nebo aktualizovat existující podrobnosti o rozhraní API|
|/Service/APIs/DELETE|Odeberte existující rozhraní API|
|/Service/APIs/Policy/Read|Získat podrobnosti o konfiguraci zásad pro rozhraní API|
|/Service/APIs/Policy/Write|Podrobnosti o konfiguraci zásad nastavení pro rozhraní API|
|/Service/APIs/Policy/DELETE|Odebrat konfiguraci zásad z rozhraní API|
|/Service/APIs/Operations/Read|Získání seznamu existující operace rozhraní API nebo získat podrobnosti operace rozhraní API|
|/Service/APIs/Operations/Write|Operaci nového rozhraní API umožňuje vytvořit nebo aktualizovat existující operace rozhraní API|
|/Service/APIs/Operations/DELETE|Odeberte existující operace rozhraní API|
|/Service/APIs/Operations/Policy/Read|Získat podrobnosti o konfiguraci zásad pro operaci|
|/Service/APIs/Operations/Policy/Write|Nastavení zásad konfigurace podrobnosti operace|
|/Service/APIs/Operations/Policy/DELETE|Odebrat konfiguraci zásad z operace|
|/Service/Products/Read|Získat seznam produktů nebo získat podrobnosti o produktu|
|/Service/Products/Write|Vytvořit nového produktu nebo aktualizovat existující podrobnosti o produktu|
|/Service/Products/DELETE|Odeberte existující produktu|
|/Service/Products/Subscriptions/Read|Získání seznamu předplatných produktu|
|/Service/Products/APIs/Read|Získání seznamu rozhraní API přidat tooexisting produktu|
|/Service/Products/APIs/Write|Přidat existující produkt tooexisting rozhraní API|
|/Service/Products/APIs/DELETE|Odeberte existující rozhraní API z existující produktu|
|/Service/Products/Policy/Read|Získání konfigurace zásad existující produktu|
|/Service/Products/Policy/Write|Nastavení zásad konfigurace pro stávající produkt|
|/Service/Products/Policy/DELETE|Odebrat konfiguraci zásad z existující produktu|
|/Service/Products/groups/Read|Získání seznamu skupin vývojáře spojené s produktem|
|/Service/Products/groups/Write|Existující produkt přidružit existující skupinu pro vývojáře|
|/Service/Products/groups/DELETE|Odstraňte přidružení existující produkt existující skupinu pro vývojáře|
|/Service/openidConnectProviders/Read|Získat seznam poskytovatelů OpenID Connect nebo získáte podrobnosti OpenID Connect zprostředkovatele|
|/Service/openidConnectProviders/Write|Vytvořit nové podrobnosti OpenID Connect zprostředkovatele nebo aktualizaci existujícího poskytovatele připojení OpenID|
|/Service/openidConnectProviders/DELETE|Odebrat existující OpenID Connect zprostředkovatele|
|/Service/Certificates/Read|Získání seznamu certifikátů nebo získat podrobnosti o certifikát|
|/Service/Certificates/Write|Přidat nový certifikát|
|/Service/Certificates/DELETE|Odebrat existující certifikát|
|/Service/Properties/Read|Získá seznam všech vlastností nebo získá podrobnosti o zadanou vlastnost|
|/Service/Properties/Write|Vytvoří novou vlastnost nebo aktualizuje hodnotu pro zadanou vlastnost|
|/Service/Properties/DELETE|Odebere existující vlastnost|
|/Service/groups/Read|Získání seznamu skupin nebo získá podrobnosti skupiny|
|/Service/groups/Write|Vytvořit novou skupinu nebo aktualizovat existující skupiny podrobnosti|
|/Service/groups/DELETE|Odeberte existující skupinu|
|/Service/groups/Users/Read|Získání seznamu skupin uživatelů|
|/Service/groups/Users/Write|Přidat existující tooexisting skupinu uživatelů|
|/Service/groups/Users/DELETE|Odebrat existující uživatele z existující skupiny|
|/Service/authorizationServers/Read|Získat seznam serverů autorizace nebo získat podrobnosti o serveru ověřování|
|/Service/authorizationServers/Write|Vytvořit nový server autorizace nebo podrobné informace o aktualizaci existujícího serveru autorizace|
|/Service/authorizationServers/DELETE|Odeberte existující serveru ověřování|
|/Service/Reports/bySubscription/Read|Získáte sestavy agregovat tímto odběrem.|
|/Service/Reports/byRequest/Read|Získání žádostí o data pro vytváření sestav|
|/Service/Reports/byOperation/Read|Získat sestavy agregován pomocí operací|
|/Service/Reports/byGeo/Read|Získat sestavy agregovat v zeměpisné oblasti|
|/Service/Reports/byUser/Read|Získáte sestavy agregovat vývojáři.|
|/Service/Reports/byTime/Read|Získat sestavy agregovat v časových období|
|/Service/Reports/byApi/Read|Získat sestavy agregovat v rozhraní API|
|/Service/Reports/byProduct/Read|Získáte sestavy agregaci produkty.|

## <a name="microsoftappservice"></a>Microsoft.AppService

| Operace | Popis |
|---|---|
|/ appidentities/čtení|Vrátí hello prostředků (webový server) zaregistrována hello brány.|
|/ appidentities/zápisu|Vytvoří novou identitu aplikace.|
|/ appidentities/odstranit|Odstraní existující Identity aplikace.|
|/deploymenttemplates/listMetadata/Action|Uvádí uživatelského rozhraní Metadata spojená s hello balíček aplikace API.|
|/deploymenttemplates/Generate/Action|Vrátí tooprovision šablonu nasazení aplikace API instance.|
|/ brány/čtení|Vrátí instanci brány hello.|
|/ brány/zápisu|Vytvoří novou bránu nebo aktualizuje existující.|
|/ brány/odstranit|Odstraní existující instanci brány.|
|/gateways/listLoginUris/Action|Naplní úložiště tokenů a vrátí přihlášení OAuth. identifikátory URI.|
|/gateways/listKeys/Action|Vrátí tajné klíče brány.|
|/gateways/tokens/Write|Vytvoří nový záhlaví Zumo Token s daným názvem hello.|
|/gateways/Registrations/Read|Vrátí hello prostředků (webový server) zaregistrována hello brány.|
|/gateways/Registrations/Write|Zaregistruje prostředku (webový server) se hello brány.|
|/gateways/Registrations/DELETE|Zruší registraci prostředku (webový server) z hello brány.|
|/ apiapps/čtení|Vrátí hello instanci aplikace API.|
|/ apiapps/zápisu|Vytvoří novou aplikaci API nebo aktualizuje existující.|
|/ apiapps/odstranit|Odstraní existující instanci aplikace API.|
|/apiapps/listStatus/Action|Vrátí stav aplikace API.|
|/apiapps/listKeys/Action|Vrátí aplikace API tajných klíčů.|
|/apiapps/apidefinitions/Read|Vrátí definice rozhraní API aplikace API.|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Operace | Popis |
|---|---|
|/ elevateAccess nebo akce|Uděluje hello volající přístup správce přístupu uživatelů na obor klienta hello|
|/classicAdministrators/Read|Přečte hello správce pro předplatné hello.|
|/ classicAdministrators/zápisu|Přidat nebo změnit správce předplatného tooa.|
|/classicAdministrators/DELETE|Odebere hello správce z předplatného hello.|
|/Locks/Read|Získá zámky v hello zadaný obor.|
|/ zámky a zápis|Umožňuje přidat zámky v hello zadaný obor.|
|/Locks/DELETE|Odstranit zámky v hello zadaný obor.|
|/policyAssignments/Read|Získání informací o přiřazení zásady|
|/ policyAssignments/zápisu|Vytvoření zásady přiřazení v hello zadaný obor.|
|/policyAssignments/DELETE|Odstraní přiřazení zásady v hello zadaný obor.|
|/Permissions/Read|Seznam všech hello oprávnění, která má volající hello v daném oboru.|
|/roleDefinitions/Read|Umožňuje načíst informace o definici role.|
|/ roleDefinitions/zápisu|Odstraňte nebo aktualizujte definice vlastních rolí s určenými oprávněními a přiřaditelnými obory.|
|/roleDefinitions/DELETE|Odstranit hello zadané definice vlastních rolí.|
|/providerOperations/Read|Získejte operace pro všechny poskytovatele prostředků, které je možné použít v definicích rolí.|
|/policyDefinitions/Read|Získání informací o definici zásady|
|/ policyDefinitions/zápisu|Vytvoření vlastních zásad pro definice.|
|/policyDefinitions/DELETE|Odstraní definici zásady.|
|/roleAssignments/Read|Umožňuje načíst informace o přiřazení role.|
|/ roleAssignments/zápisu|Umožňuje vytvořit roli přiřazení v hello zadaný obor.|
|/roleAssignments/DELETE|Odstranit přiřazení role v hello zadaný obor.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Operace | Popis |
|---|---|
|/automationAccounts/Read|Získá účet automatizace Azure|
|/ automationAccounts/zápisu|Vytvoří nebo aktualizuje účet Azure Automation.|
|/automationAccounts/DELETE|Odstraní účet Azure Automation|
|/automationAccounts/Configurations/readContent/Action|Získá obsah DSC Azure Automation.|
|/automationAccounts/hybridRunbookWorkerGroups/Read|Čte prostředky Hybrid Runbook Worker|
|/automationAccounts/hybridRunbookWorkerGroups/DELETE|Odstraní prostředky Hybrid Runbook Worker|
|/automationAccounts/jobSchedules/Read|Získá plán úloh Azure Automation.|
|/automationAccounts/jobSchedules/Write|Vytvoří plán úloh Azure Automation.|
|/automationAccounts/jobSchedules/DELETE|Odstraní plán úloh Azure Automation.|
|/automationAccounts/connectionTypes/Read|Získá prostředek typu připojení automatizace Azure.|
|/automationAccounts/connectionTypes/Write|Vytvoří prostředek typu připojení automatizace Azure.|
|/automationAccounts/connectionTypes/DELETE|Odstraní prostředek typu připojení automatizace Azure.|
|/automationAccounts/Modules/Read|Získá modul automatizace Azure|
|/automationAccounts/Modules/Write|Vytvoří nebo aktualizuje modul Azure Automation|
|/automationAccounts/Modules/DELETE|Odstraní modul Azure Automation|
|/automationAccounts/credentials/Read|Získá prostředek přihlašovacích údajů Azure Automation.|
|/automationAccounts/credentials/Write|Vytvoří nebo aktualizuje prostředek přihlašovacích údajů Azure Automation.|
|/automationAccounts/credentials/DELETE|Odstraní prostředek přihlašovacích údajů Azure Automation.|
|/automationAccounts/Certificates/Read|Načte prostředek certifikátu automatizace Azure.|
|/automationAccounts/Certificates/Write|Vytvoří nebo aktualizuje prostředek certifikátu automatizace Azure|
|/automationAccounts/Certificates/DELETE|Odstraní prostředek certifikátu automatizace Azure|
|/automationAccounts/Schedules/Read|Získá prostředek plánování Azure Automation.|
|/automationAccounts/Schedules/Write|Vytvoří nebo aktualizuje prostředek plánování Azure Automation.|
|/automationAccounts/Schedules/DELETE|Odstraní prostředek plánování Azure Automation.|
|/automationAccounts/Jobs/Read|Získá úlohu automatizace Azure|
|/automationAccounts/Jobs/Write|Vytvoří úlohu Azure Automation|
|/automationAccounts/Jobs/stop/Action|Zastaví úlohu Azure Automation|
|/automationAccounts/Jobs/suspend/Action|Pozastaví úlohu Azure Automation|
|/automationAccounts/Jobs/Resume/Action|Obnoví úlohu automatizace Azure|
|/automationAccounts/Jobs/runbookContent/Action|Získá obsah hello hello Azure Automation runbook v době provádění úlohy hello hello|
|/automationAccounts/Jobs/Output/Action|Získá hello výstup úlohy|
|/automationAccounts/Jobs/Read|Získá úlohu automatizace Azure|
|/automationAccounts/Jobs/Write|Vytvoří úlohu Azure Automation|
|/automationAccounts/Jobs/stop/Action|Zastaví úlohu Azure Automation|
|/automationAccounts/Jobs/suspend/Action|Pozastaví úlohu Azure Automation|
|/automationAccounts/Jobs/Resume/Action|Obnoví úlohu automatizace Azure|
|/automationAccounts/Jobs/Streams/Read|Získá stream úloh Azure Automation|
|/automationAccounts/Connections/Read|Načte prostředek připojení Azure Automation.|
|/automationAccounts/Connections/Write|Vytvoří nebo aktualizuje prostředek připojení Azure Automation.|
|/automationAccounts/Connections/DELETE|Odstraní prostředek připojení Azure Automation.|
|/automationAccounts/Variables/Read|Přečte variabilní prostředek Azure Automation.|
|/automationAccounts/Variables/Write|Vytvoří nebo aktualizuje variabilní prostředek Azure Automation.|
|/automationAccounts/Variables/DELETE|Odstraní variabilní prostředek Azure Automation.|
|/automationAccounts/runbooks/readContent/Action|Získá hello obsah runbooku automatizace Azure|
|/automationAccounts/runbooks/Read|Načte runbook služby automatizace Azure|
|/automationAccounts/runbooks/Write|Vytvoří nebo aktualizuje runbook služby automatizace Azure|
|/automationAccounts/runbooks/DELETE|Odstraní runbook služby automatizace Azure|
|/automationAccounts/runbooks/draft/readContent/Action|Získá hello obsah konceptu runbooku služby Azure Automation.|
|/automationAccounts/runbooks/draft/writeContent/Action|Vytvoří hello obsah konceptu runbooku služby Azure Automation.|
|/automationAccounts/runbooks/draft/Read|Získá koncept runbooku služby Azure Automation.|
|/automationAccounts/runbooks/draft/Publish/Action|Publikuje koncept runbooku služby Azure Automation.|
|/automationAccounts/runbooks/draft/undoEdit/Action|Vrátit zpět koncept runbooku automatizace Azure tooan úpravy|
|/automationAccounts/runbooks/draft/testJob/Read|Získá testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/runbooks/draft/testJob/Write|Vytvoří testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/runbooks/draft/testJob/stop/Action|Zastaví testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/runbooks/draft/testJob/suspend/Action|Pozastaví testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/runbooks/draft/testJob/Resume/Action|Obnoví testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/webhooks/Read|Přečte webhook automatizace Azure|
|/automationAccounts/webhooks/Write|Vytvoří nebo aktualizuje webhook automatizace Azure|
|/automationAccounts/webhooks/DELETE|Odstraní webhook automatizace Azure |
|/automationAccounts/webhooks/generateUri/Action|Vygeneruje URI pro webhook automatizace Azure|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

Tento zprostředkovatel není úplná zprostředkovatele ARM a neposkytuje žádné operace ARM.

## <a name="microsoftbatch"></a>Microsoft.Batch

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné hello hello poskytovatele prostředků Batch a umožňuje vytvoření hello účty Batch|
|/ batchAccounts/zápisu|Vytvoří nový účet Batch nebo aktualizuje existující účet Batch|
|/batchAccounts/Read|Zobrazí seznam účtů Batch nebo získá hello vlastnosti účtu Batch|
|/batchAccounts/DELETE|Odstraní účet Batch|
|/batchAccounts/listkeys/Action|Seznamy přístupové klíče pro účet Batch|
|/batchAccounts/regeneratekeys/Action|Obnoví přístupové klíče pro účet Batch|
|/batchAccounts/syncAutoStorageKeys/Action|Synchronizuje přístupové klíče pro účet úložiště automaticky hello nakonfigurované pro účet Batch|
|/batchAccounts/Applications/Read|Seznam aplikací, nebo získá hello vlastností aplikace|
|/batchAccounts/Applications/Write|Vytvoří novou aplikaci nebo aktualizuje existující aplikace|
|/batchAccounts/Applications/DELETE|Odstraní aplikaci|
|/batchAccounts/Applications/versions/Read|Získá vlastnosti hello balíček aplikace|
|/batchAccounts/Applications/versions/Write|Vytvoří nový balíček aplikace nebo aktualizuje existující balíček aplikace|
|/batchAccounts/Applications/versions/Activate/Action|Aktivuje balíček aplikace|
|/batchAccounts/Applications/versions/DELETE|Odstraní balíček aplikace|
|/Locations/Quotas/Read|Získá dávkové kvóty Dobrý den zadaný odběr v zadané oblasti Azure hello|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Operace | Popis |
|---|---|
|/Invoices/Read|K dispozici faktury seznamy|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Operace | Popis |
|---|---|
|/ mapApis/čtení|Operace čtení|
|/ mapApis/zápisu|Operace zápisu|
|/ mapApis/odstranit|Operace odstranění|
|/mapApis/regenerateKey/Action|Regeneruje hello klíč|
|/mapApis/listSecrets/Action|Seznam hello tajné klíče|
|/mapApis/listSingleSignOnToken/Action|Čtení jednoho přihlášení na autorizační Token pro hello prostředků|
|/ Operací/čtení|Popis hello operace.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Operace | Popis |
|---|---|
|/ checknameavailability nebo akce|Ověří, zda název je k dispozici pro použití s novou mezipaměť Redis|
|/ registrace nebo akce|Registruje zprostředkovatele prostředků 'Microsoft.Cache' hello s předplatným|
|nebo zrušit registraci nebo akce|Zrušení registrace poskytovatele prostředků 'Microsoft.Cache' hello s předplatným|
|/ redis a zápis|Upravit hello na nastavení a konfiguraci Redis Cache na portálu pro správu hello|
|/redis/Read|Zobrazit nastavení a konfiguraci hello Redis Cache na portálu správy hello|
|/redis/DELETE|Odstranění hello celý Redis Cache|
|/redis/listKeys/Action|Zobrazení hello hodnotu přístupové klíče služby Redis Cache na portálu pro správu hello|
|/redis/regenerateKey/Action|Změnit hodnotu hello přístupové klíče služby Redis Cache na portálu pro správu hello|
|/redis/import/Action|Import dat určeného formátu z víc objektů blob do Redis|
|/redis/export/Action|Export Redis objektů BLOB storage tooprefixed dat v zadaném formátu.|
|/redis/forceReboot/Action|Vynuťte restartování instance mezipaměti, což ale může mít za následek ztrátu dat.|
|/redis/stop/Action|Zastavte instance mezipaměti.|
|/redis/Start/Action|Spuštění instance mezipaměti.|
|/redis/metricDefinitions/Read|Získá hello dostupné metriky pro Redis Cache|
|/redis/firewallRules/Read|Získat hello IP brány firewall pravidla mezipaměti Redis|
|/redis/firewallRules/Write|Úprava pravidla brány firewall IP hello mezipaměti Redis|
|/redis/firewallRules/DELETE|Odstranit pravidla firewallu IP mezipaměti Redis|
|/redis/listUpgradeNotifications/Read|Seznam hello nejnovějších upozornění na upgrady pro klienta mezipaměti hello.|
|/redis/linkedservers/Read|Získáte odkazované servery přidružené k mezipaměti redis.|
|/redis/linkedservers/Write|Přidat odkazovaný Server tooa Redis Cache|
|/redis/linkedservers/DELETE|Odstranit odkazovaný Server z mezipaměti Redis|
|/redis/patchSchedules/Read|Získá hello opravy plán Redis Cache|
|/redis/patchSchedules/Write|Upravit hello opravy plán Redis Cache|
|/redis/patchSchedules/DELETE|Odstranit plán oprava hello mezipaměti Redis|

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Operace | Popis |
|---|---|
|/ provisionGlobalAppServicePrincipalInUserTenant nebo akce|Zřízení instančního objektu pro objekt zabezpečení služby aplikace|
|/ validateCertificateRegistrationInformation nebo akce|Ověření objektu nákupu certifikát bez odeslání|
|/ registrace nebo akce|Registrace poskytovatele prostředků hello Certificates společnosti Microsoft pro předplatné hello|
|/ certificateOrders/zápisu|Přidat nové certificateOrder nebo aktualizovat stávající|
|/ certificateOrders/odstranit|Odstraňte existující AppServiceCertificate|
|/ certificateOrders/čtení|Získání seznamu hello CertificateOrders|
|/certificateOrders/reissue/Action|Opakujte existující certificateorder|
|/certificateOrders/renew/Action|Obnovit existující certificateorder|
|/certificateOrders/retrieveCertificateActions/Action|Načtení seznamu hello akce certifikátu|
|/certificateOrders/retrieveEmailHistory/Action|Načtení historie certifikát e-mailu|
|/certificateOrders/resendEmail/Action|Znovu odeslat e-mail certifikátu|
|/certificateOrders/verifyDomainOwnership/Action|Ověřit vlastnictví domény|
|/certificateOrders/resendRequestEmails/Action|Znovu odeslat žádost o e-mailů tooanother e-mailová adresa|
|/certificateOrders/resendRequestEmails/Action|Načtení lokality zapečetění vydaného certifikátu služby aplikace|
|/certificateOrders/Certificates/Write|Přidat nový certifikát nebo aktualizovat stávající|
|/certificateOrders/Certificates/DELETE|Odstraňte existující certifikát|
|/certificateOrders/Certificates/Read|Získání seznamu hello certifikátů|

## <a name="microsoftclassiccompute"></a>Microsoft.Network

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistrovat tooClassic výpočetní|
|/ checkDomainNameAvailability nebo akce|Zkontroluje dostupnost hello zadaného názvu domény.|
|/ moveSubscriptionResources nebo akce|Přesune všechny klasické prostředky tooa jiné předplatné.|
|/ validateSubscriptionMoveAvailability nebo akce|Ověřte dostupnost hello předplatného pro operaci přesunutí classic.|
|/operatingSystemFamilies/Read|Uvádí hello hostovaného operačního systému rodiny dostupné v Microsoft Azure a také uvádí verze operačního systému hello k dispozici pro každý f
|/Capabilities/Read|Ukazuje možnosti hello|
|/operatingSystems/Read|Obsahuje seznam verzí hello hello hostovaného operačního systému, které jsou aktuálně k dispozici v Microsoft Azure.|
|/resourceTypes/skus/Read|Získá seznam hello Sku pro typy podporovaných prostředků.|
|/domainNames/Read|Vrátí hello názvy domén pro prostředky.|
|/ domainNames/zápisu|Přidat nebo změnit hello názvy domén pro prostředky.|
|/domainNames/DELETE|Odeberte hello názvy domén pro prostředky.|
|/domainNames/swap/Action|Zamění hello pracovní pozici toohello produkční slot.|
|/domainNames/serviceCertificates/Read|Vrátí hello používané certifikáty služeb.|
|/domainNames/serviceCertificates/Write|Přidat nebo změnit používané certifikáty služeb hello.|
|/domainNames/serviceCertificates/DELETE|Odstraňte používané certifikáty služeb hello.|
|/domainNames/serviceCertificates/operationStatuses/Read|Přečte stav operace hello pro certifikáty služeb názvů domény hello.|
|/domainNames/Capabilities/Read|Ukazuje možnosti názvu domény hello|
|/domainNames/Extensions/Read|Vrátí hello rozšíření názvu domény.|
|/domainNames/Extensions/Write|Přidejte rozšíření názvu domény hello.|
|/domainNames/Extensions/DELETE|Odeberte rozšíření názvu domény hello.|
|/domainNames/Extensions/operationStatuses/Read|Přečte stav operace hello pro rozšíření názvů domény hello.|
|/domainNames/Active/Write|Nastaví název domény služby active hello.|
|/domainNames/slots/Read|Zobrazuje hello nasazovací sloty.|
|/domainNames/slots/Write|Umožňuje vytvořit nebo aktualizovat hello nasazení.|
|/domainNames/slots/DELETE|Umožňuje odstranit zadaný slot pro nasazení.|
|/domainNames/slots/Start/Action|Umožňuje spustit slot pro nasazení.|
|/domainNames/slots/stop/Action|Pozastaví hello nasazovací slot.|
|/domainNames/slots/operationStatuses/Read|Přečte hello stav operace pro sloty názvů domény hello.|
|/domainNames/slots/Roles/Read|Získáte hello role pro hello nasazovací slot.|
|/domainNames/slots/Roles/extensionReferences/Read|Vrátí hello odkaz na rozšíření u role slotu nasazení hello.|
|/domainNames/slots/Roles/extensionReferences/Write|Přidat nebo změnit hello odkaz na rozšíření u role slotu nasazení hello.|
|/domainNames/slots/Roles/extensionReferences/DELETE|Odeberte hello odkaz na rozšíření u role slotu nasazení hello.|
|/domainNames/slots/Roles/extensionReferences/operationStatuses/Read|Přečte stav operace hello pro hello domény názvy přihrádek reference na rozšíření rolí.|
|/domainNames/slots/Roles/roleInstances/Read|Získá instanci role hello.|
|/domainNames/slots/Roles/roleInstances/restart/Action|Restartování instance role.|
|/domainNames/slots/Roles/roleInstances/reimage/Action|Reimages hello role instance.|
|/domainNames/slots/Roles/roleInstances/operationStatuses/Read|Přečte stav operace hello pro instance rolí u hello domény názvy přihrádek rolí.|
|/domainNames/slots/State/Start/Write|Změny hello toostopped Stav slotu nasazení.|
|/domainNames/slots/State/stop/Write|Změny hello toostarted Stav slotu nasazení.|
|/domainNames/slots/upgradeDomain/Write|Provede upgrade hello domény.|
|/domainNames/internalLoadBalancers/Read|Získá hello interní služby load balancer.|
|/domainNames/internalLoadBalancers/Write|Umožňuje vytvořit nový interní nástroj pro vyrovnávání zatížení.|
|/domainNames/internalLoadBalancers/DELETE|Umožňuje odebrat interní nástroj pro vyrovnávání zatížení.|
|/domainNames/internalLoadBalancers/operationStatuses/Read|Přečte stav operace hello pro interní služby load balancer názvů domén hello.|
|/domainNames/loadBalancedEndpointSets/Read|Zobrazuje hello sady koncových bodů s vyrovnáváním zatížení|
|/domainNames/loadBalancedEndpointSets/operationStatuses/Read|Přečte stav operace hello pro sady koncových bodů s vyrovnáváním zatížení názvů domén hello.|
|/domainNames/availabilitySets/Read|Zobrazit hello sadu dostupnosti pro prostředek hello.|
|/Quotas/Read|Získáte hello kvót pro předplatné hello.|
|/virtualMachines/Read|Umožňuje načíst seznam virtuálních počítačů.|
|/ virtuálních počítačů a zápis|Umožňuje přidat nebo upravit virtuální počítače.|
|/virtualMachines/DELETE|Odebere virtuální počítače.|
|/virtualMachines/Start/Action|Spusťte virtuální počítač hello.|
|/virtualMachines/redeploy/Action|Opětovně nasadí hello virtuálního počítače.|
|/virtualMachines/restart/Action|Restartuje virtuální počítače.|
|/virtualMachines/stop/Action|Zastaví hello virtuálního počítače.|
|/virtualMachines/Shutdown/Action|Vypnutí hello virtuálního počítače.|
|/virtualMachines/attachDisk/Action|Připojí data disku tooa virtuálního počítače.|
|/virtualMachines/detachDisk/Action|Umožňuje odpojit datový disk od virtuálního počítače.|
|/virtualMachines/downloadRemoteDesktopConnectionFile/Action|Stáhne hello soubor RDP pro virtuální počítač.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/čtení|Načte skupinu zabezpečení sítě hello přidružené hello síťové rozhraní.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups a zápis|Přidá skupinu zabezpečení sítě spojenou s hello síťové rozhraní.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups nebo odstranění|Odstraní skupinu zabezpečení sítě hello spojenou s hello síťové rozhraní.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/operationStatuses/čtení|Přečte stav operace hello hello virtuálních počítačů spojené skupiny zabezpečení sítě.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/Read|Získá hello Definice metrik.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/Read|Získáte nastavení diagnostiky hello.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/Write|Umožňuje přidat nebo změnit nastavení diagnostiky.|
|/virtualMachines/Metrics/Read|Získá metriky hello.|
|/virtualMachines/operationStatuses/Read|Přečte stav operace hello hello virtuálních počítačů.|
|/virtualMachines/Extensions/Read|Získá hello rozšíření virtuálního počítače.|
|/virtualMachines/Extensions/Write|Vloží hello rozšíření virtuálního počítače.|
|/virtualMachines/Extensions/operationStatuses/Read|Přečte stav operace hello pro rozšíření virtuálních počítačů hello.|
|/virtualMachines/asyncOperations/Read|Získá hello možných asynchronních operací.|
|/virtualMachines/Disks/Read|Umožňuje načíst seznam datových disků.|
|/virtualMachines/associatedNetworkSecurityGroups/Read|Načte skupinu zabezpečení sítě hello spojenou s virtuálním počítačem hello.|
|/virtualMachines/associatedNetworkSecurityGroups/Write|Přidá skupinu zabezpečení sítě spojenou s hello virtuálního počítače.|
|/virtualMachines/associatedNetworkSecurityGroups/DELETE|Odstraní skupinu zabezpečení sítě hello spojenou s virtuálním počítačem hello.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/Read|Přečte stav operace hello hello virtuálních počítačů spojené skupiny zabezpečení sítě.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistrovat tooClassic sítě|
|/gatewaySupportedDevices/Read|Načte hello seznam podporovaných zařízení.|
|/reservedIps/Read|Získá hello vyhrazené IP adresy|
|/ rezervovaných adres IP a zápis|Umožňuje přidat novou vyhrazenou IP adresu.|
|/reservedIps/DELETE|Umožňuje odstranit vyhrazenou IP adresu.|
|/reservedIps/Link/Action|Odkaz na rezervovanou IP adresu|
|/reservedIps/JOIN/Action|Připojit se k rezervované IP adrese|
|/reservedIps/operationStatuses/Read|Přečte stav operace hello pro hello vyhrazené IP adresy.|
|/virtualNetworks/Read|Získáte hello virtuální sítě.|
|/ virtualNetworks/zápisu|Umožňuje přidat novou virtuální síť.|
|/virtualNetworks/DELETE|Odstraní hello virtuální sítě.|
|/virtualNetworks/peer/Action|Vytvoří partnerskou virtuální síť s jinou virtuální sítí.|
|/virtualNetworks/JOIN/Action|Spojí hello virtuální sítě.|
|/virtualNetworks/checkIPAddressAvailability/Action|Zkontroluje dostupnost dané IP adresy ve virtuální síti hello.|
|/virtualNetworks/Capabilities/Read|Ukazuje možnosti hello|
|/virtualNetworks/podsítě /<br>associatedNetworkSecurityGroups/čtení|Načte skupinu zabezpečení sítě hello spojenou s podsítí hello.|
|/virtualNetworks/podsítě /<br>associatedNetworkSecurityGroups a zápis|Přidá skupinu zabezpečení sítě spojenou s podsítí hello.|
|/virtualNetworks/podsítě /<br>associatedNetworkSecurityGroups nebo odstranění|Odstraní skupinu zabezpečení sítě hello spojenou s podsítí hello.|
|/virtualNetworks/podsítě /<br>associatedNetworkSecurityGroups/operationStatuses/čtení|Přečte stav operace hello pro skupinu zabezpečení sítě spojené s podsítí hello virtuální sítě.|
|/virtualNetworks/operationStatuses/Read|Přečte stav operace hello hello virtuálních sítí.|
|/virtualNetworks/gateways/Read|Získá hello brány virtuální sítě.|
|/virtualNetworks/gateways/Write|Umožňuje přidat bránu virtuální sítě.|
|/virtualNetworks/gateways/DELETE|Odstraní hello brány virtuální sítě.|
|/virtualNetworks/gateways/startDiagnostics/Action|Umožňuje spustit diagnostiku pro bránu virtuální sítě hello.|
|/virtualNetworks/gateways/stopDiagnostics/Action|Zastaví hello diagnostiku pro bránu virtuální sítě hello.|
|/virtualNetworks/gateways/downloadDiagnostics/Action|Stáhne diagnostiku brány hello.|
|/virtualNetworks/gateways/listCircuitServiceKey/Action|Načte klíč služby okruhů hello.|
|/virtualNetworks/gateways/downloadDeviceConfigurationScript/Action|Stáhne skript pro konfiguraci zařízení hello.|
|/virtualNetworks/gateways/listPackage/Action|Uvádí hello balíček bran virtuální sítě.|
|/virtualNetworks/gateways/operationStatuses/Read|Přečte stav operace hello hello bran virtuálních sítí.|
|/virtualNetworks/gateways/Packages/Read|Získá hello balíček bran virtuální sítě.|
|/virtualNetworks/gateways/Connections/Read|Načte hello seznam připojení.|
|/virtualNetworks/gateways/Connections/Connect/Action|Připojí připojení site toosite brány.|
|/virtualNetworks/gateways/Connections/Disconnect/Action|Odpojí připojení site toosite brány.|
|/virtualNetworks/gateways/Connections/test/Action|Otestuje připojení site toosite brány.|
|/virtualNetworks/gateways/clientRevokedCertificates/Read|Čtení hello odvolané klientské certifikáty.|
|/virtualNetworks/gateways/clientRevokedCertificates/Write|Umožňuje odvolat klientský certifikát.|
|/virtualNetworks/gateways/clientRevokedCertificates/DELETE|Umožňuje zrušit odvolání klientského certifikátu.|
|/virtualNetworks/gateways/clientRootCertificates/Read|Najde hello klientské kořenové certifikáty.|
|/virtualNetworks/gateways/clientRootCertificates/Write|Umožňuje nahrát nový klientský kořenový certifikát.|
|/virtualNetworks/gateways/clientRootCertificates/DELETE|Odstraní klientský certifikát brány virtuální sítě hello.|
|/virtualNetworks/gateways/clientRootCertificates/download/Action|Umožňuje stáhnout certifikát podle kryptografického otisku.|
|/virtualNetworks/gateways/clientRootCertificates/listPackage/Action|Uvádí hello balíček certifikátů bran virtuální sítě.|
|/networkSecurityGroups/Read|Načte skupinu zabezpečení sítě hello.|
|/ skupin zabezpečení sítě a zápis|Přidá novou skupinu zabezpečení sítě.|
|/networkSecurityGroups/DELETE|Odstraní skupinu zabezpečení sítě hello.|
|/networkSecurityGroups/operationStatuses/Read|Přečte stav operace hello pro skupinu zabezpečení sítě hello.|
|/networkSecurityGroups/securityRules/Read|Získá hello pravidlo zabezpečení.|
|/networkSecurityGroups/securityRules/Write|Přidá nebo aktualizuje pravidlo zabezpečení.|
|/networkSecurityGroups/securityRules/DELETE|Odstraní pravidlo zabezpečení hello.|
|/networkSecurityGroups/securityRules/operationStatuses/Read|Přečte stav operace hello pro pravidla zabezpečení skupiny zabezpečení sítě hello.|
|/Quotas/Read|Získáte hello kvót pro předplatné hello.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistrovat tooClassic úložiště|
|/ checkStorageAccountAvailability nebo akce|Zkontroluje dostupnost hello účtu úložiště.|
|/Capabilities/Read|Ukazuje možnosti hello|
|/publicImages/Read|Načte obrázek hello veřejné virtuálního počítače.|
|/Images/Read|Vrátí obrázek hello.|
|/storageAccounts/Read|Vrátí hello účet úložiště s hello zadaný účet.|
|/ storageAccounts/zápisu|Umožňuje přidat nový účet úložiště.|
|/storageAccounts/DELETE|Odstraňte účet úložiště hello.|
|/storageAccounts/listKeys/Action|Uvádí hello přístupových klíčů pro účty úložiště hello.|
|/storageAccounts/regenerateKey/Action|Regeneruje hello existující přístupové klíče pro účet úložiště hello.|
|/storageAccounts/operationStatuses/Read|Přečte stav operace hello hello prostředku.|
|/storageAccounts/Images/Read|Vrátí hello image účtu storage.|
|/storageAccounts/Images/DELETE|Odstraní danou image účtu Storage.|
|/storageAccounts/Disks/Read|Vrátí hello disku účet úložiště.|
|/storageAccounts/Disks/Write|Přidá disk účtu úložiště.|
|/storageAccounts/Disks/DELETE|Odstraní daný disk účtu úložiště.|
|/storageAccounts/Disks/operationStatuses/Read|Přečte stav operace hello hello prostředku.|
|/storageAccounts/osImages/Read|Vrátí hello image operačního systému pro účet storage.|
|/storageAccounts/osImages/DELETE|Odstraní danou image operačního systému pro účet Storage.|
|/storageAccounts/Services/Read|Získáte služby k dispozici hello.|
|/storageAccounts/Services/metricDefinitions/Read|Získá hello Definice metrik.|
|/storageAccounts/Services/Metrics/Read|Získá metriky hello.|
|/storageAccounts/Services/diagnosticSettings/Read|Získáte nastavení diagnostiky hello.|
|/storageAccounts/Services/diagnosticSettings/Write|Umožňuje přidat nebo změnit nastavení diagnostiky.|
|/Disks/Read|Vrátí hello disku účet úložiště.|
|/osImages/Read|Vrátí image operačního systému hello.|
|/Quotas/Read|Získáte hello kvót pro předplatné hello.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Operace | Popis |
|---|---|
|/Accounts/Read|Načte účty rozhraní API.|
|/ účty a zápis|Zapíše účty rozhraní API.|
|/Accounts/DELETE|Odstraní účty rozhraní API|
|/Accounts/listKeys/Action|Seznam klíčů|
|/Accounts/regenerateKey/Action|Znovu vygenerovat klíč|
|/Accounts/skus/Read|Načte dostupné SKU pro existující prostředek.|
|/Accounts/usages/Read|Získáte hello kvóty využití pro existující prostředek.|
|/ Operací/čtení|Popis hello operace.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Operace | Popis |
|---|---|
|/ RateCard/čtení|Vrátí nabízejí dat, prostředku nebo měření metadata a sazby za hello zadané předplatné.|
|/ UsageAggregates/čtení|Získá předplatné Microsoft Azure spotřeby. Hello výsledek obsahuje agreguje data o využití, předplatné a prostředků související informace, na konkrétní časové rozmezí.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Umožňuje registrovat předplatné zprostředkovatele prostředků Microsoft.Compute.|
|/restorePointCollections/Read|Získat vlastnosti hello kolekce bodu obnovení|
|/ restorePointCollections/zápisu|Vytvoří novou kolekci bodu obnovení nebo aktualizuje existující|
|/restorePointCollections/DELETE|Odstranění hello kolekce bod obnovení a obsažené body obnovení|
|/restorePointCollections/restorePoints/Read|Získat vlastnosti hello bodu obnovení|
|/restorePointCollections/restorePoints/Write|Vytvoří nový bod obnovení|
|/restorePointCollections/restorePoints/DELETE|Odstraní bod obnovení hello|
|/restorePointCollections/restorePoints/retrieveSasUris/Action|Získat vlastnosti hello bodu obnovení společně s identifikátory URI SAS objektu blob|
|/virtualMachineScaleSets/Read|Získat vlastnosti hello sady škálování virtuálního počítače|
|/ virtualMachineScaleSets/zápisu|Umožňuje vytvořit novou nebo aktualizovat stávající škálovací sadu virtuálních počítačů.|
|/virtualMachineScaleSets/DELETE|Odstraní škálovací sadu virtuálních počítačů hello|
|/virtualMachineScaleSets/Start/Action|Spustí hello instance škálovací sady virtuálního počítače hello|
|/virtualMachineScaleSets/powerOff/Action|Umožňuje vypnout instance škálovací sadu virtuálních počítačů hello hello|
|/virtualMachineScaleSets/restart/Action|Restartuje hello instance škálovací sady virtuálního počítače hello|
|/virtualMachineScaleSets/deallocate/Action|Umožňuje vypnout a verze hello výpočetní prostředky pro hello instance škálovací sady virtuálního počítače hello |
|/virtualMachineScaleSets/manualUpgrade/Action|Umožňuje ručně aktualizovat instance toolatest model škálovací sadu virtuálních počítačů hello|
|/virtualMachineScaleSets/Scale/Action|Škálování v / set horizontální navýšení kapacity počet instancí existující škálování virtuálních počítačů|
|/virtualMachineScaleSets/instanceView/Read|Získá hello zobrazení instance škálovací sadu virtuálních počítačů hello|
|/virtualMachineScaleSets/skus/Read|Seznamy hello platných SKU pro existující škálovací sadu virtuálních počítačů|
|/virtualMachineScaleSets/virtualMachines/Read|Načte hello vlastnosti virtuálního počítače ve Škálovací sadě virtuálních počítačů|
|/virtualMachineScaleSets/virtualMachines/DELETE|Umožňuje odstranit konkrétní virtuální počítač ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/Start/Action|Umožňuje spustit instanci virtuálního počítače ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/powerOff/Action|Umožňuje vypnout instanci virtuálního počítače ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/restart/Action|Umožňuje restartovat instanci virtuálního počítače ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/deallocate/Action|Umožňuje vypnout a verze hello výpočetní prostředky pro virtuální počítač ve Škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/instanceView/Read|Načte hello zobrazení instance virtuálního počítače ve Škálovací sadě virtuálních počítačů.|
|/Images/Read|Získat vlastnosti hello hello bitové kopie|
|/ bitové kopie a zápis|Vytvoří novou bitovou kopii nebo aktualizuje existující|
|/Images/DELETE|Odstraní hello image|
|/Operations/Read|Umožňuje zobrazit seznam operací dostupných na zprostředkovateli prostředků Microsoft.Compute.|
|/Disks/Read|Získat vlastnosti hello disku|
|/ disků a zápis|Vytvoří nový disk nebo aktualizuje stávající.|
|/Disks/DELETE|Odstranění hello disku|
|/Disks/beginGetAccess/Action|Získat hello URI SAS hello disku pro přístup k objektu blob|
|/Disks/endGetAccess/Action|Odvolat hello URI SAS disku hello|
|/snapshots/Read|Získat vlastnosti hello snímku|
|/ snímky a zápis|Vytvoří nový snímek nebo aktualizuje stávající.|
|/snapshots/DELETE|Odstranit snímek|
|/availabilitySets/Read|Získat hello vlastnosti sady dostupnosti.|
|/ availabilitySets/zápisu|Umožňuje vytvořit novou nebo aktualizovat stávající sadu dostupnosti.|
|/availabilitySets/DELETE|Odstraní skupinu dostupnosti hello|
|/availabilitySets/vmSizes/Read|Seznam dostupných velikostí pro vytváření nebo aktualizaci virtuálního počítače v sadě dostupnosti hello|
|/virtualMachines/Read|Získat vlastnosti hello virtuálního počítače|
|/ virtuálních počítačů a zápis|Umožňuje vytvořit nový nebo aktualizovat stávající virtuální počítač.|
|/virtualMachines/DELETE|Odstraní hello virtuální počítač|
|/virtualMachines/Start/Action|Virtuální počítač spustí hello|
|/virtualMachines/powerOff/Action|Umožňuje vypnout virtuální počítač hello. Všimněte si, že hello virtuální počítač bude toobe účtují.|
|/virtualMachines/redeploy/Action|Opětovně nasadí virtuální počítač|
|/virtualMachines/restart/Action|Restartuje hello virtuálního počítače|
|/virtualMachines/deallocate/Action|Umožňuje vypnout hello virtuální počítač a uvolnit hello výpočetní prostředky|
|/virtualMachines/generalize/Action|Nastaví tooGeneralized stavu hello virtuálního počítače a připraví na zachycení hello virtuálního počítače|
|/virtualMachines/Capture/Action|Zaznamená hello virtuální počítač zkopírováním virtuálních pevných disků a vygenerováním šablony, kterou lze použít toocreate podobných virtuálních počítačů.|
|/virtualMachines/convertToManagedDisks/Action|Převede hello objektů blob na základě disky disky toomanaged hello virtuálního počítače|
|/virtualMachines/vmSizes/Read|Seznam dostupných velikostí, které budou aktualizovány hello virtuálního počítače|
|/virtualMachines/instanceView/Read|Získá hello podrobný stav runtime hello virtuálního počítače a jeho prostředků|
|/virtualMachines/Extensions/Read|Získat vlastnosti hello rozšíření virtuálního počítače.|
|/virtualMachines/Extensions/Write|Umožňuje vytvořit nové nebo aktualizovat stávající rozšíření virtuálního počítače.|
|/virtualMachines/Extensions/DELETE|Odstraní hello rozšíření virtuálního počítače.|
|/Locations/vmSizes/Read|Umožňuje zobrazit seznam dostupných velikostí virtuálních počítačů v daném umístění.|
|/Locations/usages/Read|Získá limity služby a aktuální využívaná množství výpočetních prostředků předplatného hello v umístění|
|/Locations/Operations/Read|Získá hello stav asynchronní operace|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje hello předplatného pro poskytovatele prostředků registru hello kontejneru a umožňuje hello vytvoření kontejneru registrech.|
|/checknameavailability/Read|Ověří, že název registru je platný a není používán.|
|/registries/Read|Vrátí hello seznam registrech kontejneru nebo získá hello vlastnosti pro zadaný kontejner registru hello.|
|/ registrech a zápis|Vytvoří kontejner registr s využitím zadaných parametrů hello nebo aktualizovat hello vlastnosti a značky pro zadaný kontejner registru hello.|
|/registries/DELETE|Odstraní existujícího registru kontejneru.|
|/registries/listCredentials/Action|Uvádí hello přihlašovací údaje pro zadaný kontejner registru hello.|
|/registries/regenerateCredential/Action|Regeneruje hello přihlašovací údaje pro zadaný kontejner registru hello.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Operace | Popis |
|---|---|
|/containerServices/Subscriptions/Read|Get hello určené služby kontejneru na základě předplatného.|
|/containerServices/resourceGroups/Read|Get hello určené služby kontejneru na základě skupiny prostředků|
|/containerServices/resourceGroups/ContainerServiceName/Read|Získá hello zadaný kontejner služby|
|/containerServices/resourceGroups/ContainerServiceName/Write|PUT nebo aktualizace hello zadaný kontejner služby|
|/containerServices/resourceGroups/ContainerServiceName/DELETE|Odstranění hello zadaný kontejner služby|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Operace | Popis |
|---|---|
|/ updateCommunicationPreference nebo akce|Předvolby komunikace aktualizace|
|/ listCommunicationPreference nebo akce|Seznam komunikace předvoleb|
|/Applications/Read|Operace čtení|
|/ aplikace/zápisu|Operace zápisu|
|/ aplikace/zápisu|Operace zápisu|
|/Applications/DELETE|Operace odstranění|
|/Applications/listSecrets/Action|Výpis tajných klíčů|
|/Applications/listSingleSignOnToken/Action|Přečíst jednotného přihlašování tokeny|
|/Operations/Read|operace čtení|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Operace | Popis |
|---|---|
|/hubs/Read|Číst všechny statistiky centra Azure zákazníků|
|/ hubs a zápis|Vytvořit nebo aktualizovat všechny statistiky centra Azure zákazníků|
|/hubs/DELETE|Odstranit všechny statistiky Centrum Azure zákazníků|
|/hubs/providers/Microsoft.Insights/metricDefinitions/Read|Získá hello dostupné metriky pro prostředek|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/Read|Získá hello nastavení diagnostiky pro prostředek hello|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/Write|Vytvoří nebo aktualizuje hello nastavení diagnostiky pro prostředek hello|
|/hubs/providers/Microsoft.Insights/logDefinitions/Read|Získá dostupné protokoly hello pro prostředek|
|/hubs/authorizationPolicies/Read|Přečtěte si, že žádné přehledy Azure zákazníků sdílené zásady přístupu podpis|
|/hubs/authorizationPolicies/Write|Vytvořit nebo aktualizovat žádné zásady Azure zákazníků Statistika sdílený přístupový podpis|
|/hubs/authorizationPolicies/DELETE|Odstranit všechny zásady Azure zákazníků Statistika sdílený přístupový podpis|
|/hubs/authorizationPolicies/regeneratePrimaryKey/Action|Znova vygenerovat primární klíč Azure zákazníka Statistika zásada sdíleného přístupu podpis|
|/hubs/authorizationPolicies/regenerateSecondaryKey/Action|Znova vygenerovat sekundární klíč Azure zákazníka Statistika zásada sdíleného přístupu podpis|
|/hubs/Profiles/Read|Číst všechny statistiky profil Azure zákazníků|
|/hubs/Profiles/Write|Zápis žádný profil Statistika Azure zákazníků|
|/hubs/KPI/Read|Číst všechny Azure zákazníků Statistika klíčového ukazatele výkonu|
|/hubs/KPI/Write|Vytvořit nebo aktualizovat všechny Azure zákazníků Statistika klíčového ukazatele výkonu|
|/hubs/KPI/DELETE|Odstranit všechny Azure zákazníků Statistika klíčového ukazatele výkonu|
|/hubs/views/Read|Přečtěte si všechna zobrazení statistiky aplikace Azure zákazníků|
|/hubs/views/Write|Vytvořit nebo aktualizovat všechna zobrazení statistiky aplikace Azure zákazníků|
|/hubs/views/DELETE|Odstranit všechna zobrazení statistiky aplikace Azure zákazníků|
|/hubs/interactions/Read|Číst interakce Statistika Azure zákazníků|
|/hubs/interactions/Write|Vytvořit nebo aktualizovat interakce Statistika Azure zákazníků|
|/hubs/roleAssignments/Read|Číst všechny Azure zákazníků Statistika Rbac přiřazení|
|/hubs/roleAssignments/Write|Vytvořit nebo aktualizovat všechny Azure zákazníků Statistika Rbac přiřazení|
|/hubs/roleAssignments/DELETE|Odstranit všechny Azure zákazníků Statistika Rbac přiřazení|
|/hubs/Connectors/Read|Číst všechny konektor služby Statistika Azure zákazníků|
|/hubs/Connectors/Write|Vytvořit nebo aktualizovat všechny konektor služby Statistika Azure zákazníků|
|/hubs/Connectors/DELETE|Odstranit všechny konektory Statistika Azure zákazníků|
|/hubs/Connectors/Mappings/Read|Číst všechny Azure zákazníků Statistika konektor mapování|
|/hubs/Connectors/Mappings/Write|Vytvořit nebo aktualizovat všechny Azure zákazníků Statistika konektor mapování|
|/hubs/Connectors/Mappings/DELETE|Odstranit všechny Azure zákazníků Statistika konektor mapování|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Operace | Popis |
|---|---|
|/ checkNameAvailability nebo akce|Zkontroluje dostupnost názvu katalogu pro klienta.|
|/catalogs/Read|Načíst vlastnosti pro katalogu nebo katalogů pod předplatné nebo skupinu prostředků.|
|/ katalogů a zápis|Vytvoří katalogu nebo aktualizace hello značek a vlastnosti pro katalog hello.|
|/catalogs/DELETE|Odstraní hello katalogu.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Operace | Popis |
|---|---|
|/datafactories/Read|Čte Data Factory.|
|/ datafactories/zápisu|Vytvořit nebo aktualizovat objekt pro vytváření dat|
|/datafactories/DELETE|Odstraní Data Factory.|
|/datafactories/datapipelines/Read|Přečte kanálu.|
|/datafactories/datapipelines/DELETE|Odstraní kanálu.|
|/datafactories/datapipelines/pause/Action|Pozastaví kanálu.|
|/datafactories/datapipelines/Resume/Action|Obnoví kanálu.|
|/datafactories/datapipelines/Update/Action|Aktualizace kanálu.|
|/datafactories/datapipelines/Write|Vytvořit nebo aktualizovat kanál|
|/datafactories/linkedServices/Read|Přečte propojenou službu.|
|/datafactories/linkedServices/DELETE|Odstraní propojenou službu.|
|/datafactories/linkedServices/Write|Vytvoření nebo aktualizace propojené služby|
|/datafactories/Tables/Read|Čtení tabulky.|
|/datafactories/Tables/DELETE|Odstraní tabulku.|
|/datafactories/Tables/Write|Vytvořit nebo aktualizovat tabulky|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Operace | Popis |
|---|---|
|/Accounts/Read|Získání informací o hello DataLakeAnalytics účtu.|
|/ účty a zápis|Vytvořit nebo aktualizovat účet DataLakeAnalytics hello.|
|/Accounts/DELETE|Odstranění účtu DataLakeAnalytics hello.|
|/Accounts/firewallRules/Read|Získání informací o pravidlo brány firewall.|
|/Accounts/firewallRules/Write|Vytvořit nebo aktualizovat pravidlo brány firewall.|
|/Accounts/firewallRules/DELETE|Odstraňte pravidlo brány firewall.|
|/Accounts/storageAccounts/Read|Získáte propojený účet úložiště pro hello DataLakeAnalytics účtu.|
|/Accounts/storageAccounts/Write|Odkaz toohello účet úložiště DataLakeAnalytics účtu.|
|/Accounts/storageAccounts/DELETE|Zrušit propojení účtu úložiště z hello DataLakeAnalytics účtu.|
|/Accounts/storageAccounts/Containers/Read|Získat kontejnery v účtu úložiště hello|
|/Accounts/storageAccounts/Containers/listSasTokens/Action|Seznam tokeny SAS pro kontejner úložiště hello|
|/Accounts/dataLakeStoreAccounts/Read|Získáte účet propojené DataLakeStore pro hello DataLakeAnalytics účtu.|
|/Accounts/dataLakeStoreAccounts/Write|Odkaz DataLakeStore účet toohello DataLakeAnalytics účtu.|
|/Accounts/dataLakeStoreAccounts/DELETE|Zrušit propojení účtu DataLakeStore z hello DataLakeAnalytics účtu.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Operace | Popis |
|---|---|
|/Accounts/Read|Získání informací o účtu DataLakeStore byl vytvořen.|
|/ účty a zápis|Vytvořit nový účet DataLakeStore nebo aktualizovat účet byl vytvořen DataLakeStore.|
|/Accounts/DELETE|Odstraňte účet byl vytvořen DataLakeStore.|
|/Accounts/firewallRules/Read|Získání informací o pravidlo brány firewall.|
|/Accounts/firewallRules/Write|Vytvořit nebo aktualizovat pravidlo brány firewall.|
|/Accounts/firewallRules/DELETE|Odstraňte pravidlo brány firewall.|
|/Accounts/trustedIdProviders/Read|Získáte informace o zprostředkovateli důvěryhodné identity.|
|/Accounts/trustedIdProviders/Write|Vytvořit nebo aktualizovat zprostředkovatele důvěryhodné identity.|
|/Accounts/trustedIdProviders/DELETE|Odstraňte zprostředkovatele důvěryhodné identity.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Registrovat předplatné hello pro hello IotHub prostředků zprostředkovatele a umožňuje hello vytváření prostředků IotHub|
|/ checkNameAvailability nebo akce|Zkontrolujte, zda je název IotHub-li k dispozici|
|/ použití nebo pro čtení|Získáte podrobnosti o použití předplatné pro tohoto zprostředkovatele.|
|/ operací/čtení|Získat všechny operace ResourceProvider|
|/ iotHubs/čtení|Získá prostředky IotHub hello|
|/ iotHubs/zápisu|Vytvořit nebo aktualizovat prostředek IotHub|
|/ iotHubs/odstranit|Odstranit prostředek IotHub|
|/iotHubs/listkeys/Action|Získat všechny klíče IotHub|
|/iotHubs/exportDevices/Action|Export zařízení|
|/iotHubs/importDevices/Action|Import zařízení|
|/ IotHubs/metricDefinitions/čtení|Získá hello dostupné metriky pro hello služby IotHub|
|/iotHubs/iotHubKeys/listkeys/Action|Získat klíč IotHub pro hello křestní jméno|
|/iotHubs/iotHubStats/Read|Získat statistiku IotHub|
|/iotHubs/quotaMetrics/Read|Získat metriky kvóty|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|Vytvoření skupiny příjemců EventHub|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|Získat EventHub příjemce skupiny|
|/iotHubs/eventHubEndpoints/consumerGroups/DELETE|Odstranění skupiny příjemců EventHub|
|/iotHubs/Routing/Routes/$ testall nebo akce|Testovací zpráva proti všechny existující trasy|
|/iotHubs/Routing/Routes/$ testnew nebo akce|Testovací zpráva proti testu zadané trasy|
|/ IotHubs/diagnosticSettings/čtení|Získá hello nastavení diagnostiky pro prostředek hello|
|/ IotHubs/diagnosticSettings/zápis|Vytvoří nebo aktualizuje hello nastavení diagnostiky pro prostředek hello|
|/iotHubs/skus/Read|Získání platných SKU IotHub|
|/iotHubs/Jobs/Read|Získání úloh Podrobnosti odeslaného na daný IotHub|
|/iotHubs/routingEndpointsHealth/Read|Získá stav hello všechny koncové body směrování pro platformou IotHub|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Operace | Popis |
|---|---|
|/ Předplatné nebo registrace nebo akce|Zaregistruje předplatné hello|
|/Labs/DELETE|Odstraňte laboratoře.|
|/Labs/Read|Přečtěte si laboratoře.|
|/ labs a zápis|Přidat nebo změnit laboratoře.|
|/Labs/ListVhds/Action|Zobrazí seznam bitových kopií disku k dispozici pro vytvoření vlastní image.|
|/Labs/GenerateUploadUri/Action|Generovat identifikátor URI pro nahrávání tooa bitové kopie disku vlastní testovacího prostředí.|
|/Labs/CreateEnvironment/Action|Vytvoření virtuálních počítačů v testovacím prostředí.|
|/Labs/ClaimAnyVm/Action|Deklarace identity náhodných vymahatelných virtuálního počítače v testovacím prostředí hello.|
|/Labs/ExportResourceUsage/Action|Exportuje hello využití prostředků testovacího prostředí do účtu úložiště|
|/Labs/Users/DELETE|Odstraňte profily uživatelů.|
|/Labs/Users/Read|Číst profily uživatelů.|
|/Labs/Users/Write|Přidat nebo upravit profily uživatelů.|
|/Labs/Users/secrets/DELETE|Odstraňte tajných klíčů.|
|/Labs/Users/secrets/Read|Čtení tajných klíčů.|
|/Labs/Users/secrets/Write|Přidat nebo změnit tajných klíčů.|
|/Labs/Users/Environments/DELETE|Odstraňte prostředí.|
|/Labs/Users/Environments/Read|Přečtěte si prostředí.|
|/Labs/Users/Environments/Write|Přidat nebo změnit prostředí.|
|/Labs/Users/Disks/DELETE|Odstraňte disky.|
|/Labs/Users/Disks/Read|Přečtěte si disky.|
|/Labs/Users/Disks/Write|Přidat nebo změnit disky.|
|/Labs/Users/Disks/Attach/Action|Připojit a vytvořit hello zapůjčení hello disku toohello virtuálního počítače.|
|/Labs/Users/Disks/Detach/Action|Odpojte a zalomení hello zapůjčení hello disku připojen toohello virtuálního počítače.|
|/Labs/customImages/DELETE|Odstraňte vlastní Image.|
|/Labs/customImages/Read|Přečtěte si vlastních bitových kopií.|
|/Labs/customImages/Write|Přidat nebo upravit vlastní Image.|
|/Labs/serviceRunners/DELETE|Odstraňte závodníky služby.|
|/Labs/serviceRunners/Read|Přečtěte si závodníky služby.|
|/Labs/serviceRunners/Write|Přidat nebo změnit závodníky služby.|
|/Labs/artifactSources/DELETE|Odstraňte artefaktů zdroje.|
|/Labs/artifactSources/Read|Přečtěte si artefaktů zdroje.|
|/Labs/artifactSources/Write|Přidat nebo změnit artefaktů zdroje.|
|/Labs/artifactSources/artifacts/Read|Přečtěte si artefakty.|
|/Labs/artifactSources/artifacts/GenerateArmTemplate/Action|Generuje šablony ARM pro hello zadané artefaktů, odešle účet úložiště tooa hello požadované soubory a ověří hello generované artefaktů.|
|/Labs/artifactSources/armTemplates/Read|Přečtěte si šablony správce prostředků azure.|
|/Labs/costs/Read|Přečtěte si náklady.|
|/Labs/costs/Write|Přidat nebo změnit náklady.|
|/Labs/virtualNetworks/DELETE|Odstraňte virtuální sítě.|
|/Labs/virtualNetworks/Read|Čtení virtuální sítě.|
|/Labs/virtualNetworks/Write|Přidat nebo upravit virtuální sítě.|
|/Labs/Formulas/DELETE|Odstraňte vzorce.|
|/Labs/Formulas/Read|Přečtěte si vzorce.|
|/Labs/Formulas/Write|Přidat nebo upravit vzorce.|
|/Labs/Schedules/DELETE|Odstraňte plány.|
|/Labs/Schedules/Read|Plánuje pro čtení.|
|/Labs/Schedules/Write|Přidat nebo změnit plány.|
|/Labs/Schedules/Execute/Action|Spuštění plánu.|
|/Labs/Schedules/ListApplicable/Action|Zobrazí všechny příslušné plány|
|/Labs/galleryImages/Read|Přečtěte si Galerie obrázků.|
|/Labs/policySets/EvaluatePolicies/Action|Vyhodnotí zásady testovacího prostředí.|
|/Labs/policySets/Policies/DELETE|Odstraňte zásady.|
|/Labs/policySets/Policies/Read|Přečtěte si zásady.|
|/Labs/policySets/Policies/Write|Přidat nebo upravit zásady.|
|/Labs/virtualMachines/DELETE|Odstraňte virtuální počítače.|
|/Labs/virtualMachines/Read|Přečtěte si virtuálních počítačů.|
|/Labs/virtualMachines/Write|Umožňuje přidat nebo upravit virtuální počítače.|
|/Labs/virtualMachines/Start/Action|Spuštění virtuálního počítače.|
|/Labs/virtualMachines/stop/Action|Zastavit virtuální počítač|
|/Labs/virtualMachines/ApplyArtifacts/Action|Použijte počítač toovirtual artefakty.|
|/Labs/virtualMachines/AddDataDisk/Action|Připojte počítač toovirtual disku s nový nebo existující data.|
|/Labs/virtualMachines/DetachDataDisk/Action|Odpojte hello zadaný disk z virtuálního počítače hello.|
|/Labs/virtualMachines/Claim/Action|Převzetí vlastnictví existujícího virtuálního počítače|
|/Labs/virtualMachines/ListApplicableSchedules/Action|Zobrazí všechny příslušné plány|
|/Labs/virtualMachines/Schedules/DELETE|Odstraňte plány.|
|/Labs/virtualMachines/Schedules/Read|Plánuje pro čtení.|
|/Labs/virtualMachines/Schedules/Write|Přidat nebo změnit plány.|
|/Labs/virtualMachines/Schedules/Execute/Action|Spuštění plánu.|
|/Labs/notificationChannels/DELETE|Odstraňte notificationchannels.|
|/Labs/notificationChannels/Read|Přečtěte si notificationchannels.|
|/Labs/notificationChannels/Write|Přidat nebo změnit notificationchannels.|
|/Labs/notificationChannels/Notify/Action|Odešlete tooprovided kanál oznámení.|
|/Schedules/DELETE|Odstraňte plány.|
|/Schedules/Read|Plánuje pro čtení.|
|/ plány a zápis|Přidat nebo změnit plány.|
|/Schedules/Execute/Action|Spuštění plánu.|
|/Schedules/Retarget/Action|Aktualizace plán cílový prostředek ID.|
|/Locations/Operations/Read|Operace čtení.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Operace | Popis |
|---|---|
|/databaseAccountNames/Read|Zkontroluje dostupnost názvu.|
|/databaseAccounts/Read|Přečte databázového účtu.|
|/ databaseAccounts/zápisu|Aktualizace databáze účtů.|
|/databaseAccounts/listKeys/Action|Seznam klíčů účtu databáze|
|/databaseAccounts/regenerateKey/Action|Otočit klíče účtu databáze|
|/databaseAccounts/listConnectionStrings/Action|Získat hello připojovací řetězce pro databázový účet|
|/databaseAccounts/changeResourceGroup/Action|Změnit skupinu prostředků účtu databáze|
|/databaseAccounts/failoverPriorityChange/Action|Změnit priorit převzetí služeb při selhání oblastí databázového účtu. Toto je použité tooperform ruční převzetí služeb při selhání operace|
|/databaseAccounts/DELETE|Odstraní hello databáze účtů.|
|/databaseAccounts/metricDefinitions/Read|Přečte hello databázový účet definice metrik.|
|/databaseAccounts/Metrics/Read|Přečte metriky účtu databáze hello.|
|/databaseAccounts/usages/Read|Přečte použití účtu databáze hello.|
|/databaseAccounts/Databases/Collections/metricDefinitions/Read|Načte kolekci hello Definice metrik.|
|/databaseAccounts/Databases/Collections/Metrics/Read|Přečte hello kolekce metriky.|
|/databaseAccounts/Databases/Collections/usages/Read|Načte kolekci použití hello.|
|/databaseAccounts/Databases/metricDefinitions/Read|Načte definice metrik hello databáze|
|/databaseAccounts/Databases/Metrics/Read|Přečte metriky hello databáze.|
|/databaseAccounts/Databases/usages/Read|Přečte použití databáze hello.|
|/databaseAccounts/readonlykeys/Read|Přečte hello databázového účtu jen pro čtení klíče.|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Operace | Popis |
|---|---|
|/ generateSsoRequest nebo akce|Generovat žádost o přihlášení k doméně řídicí centrum.|
|/ validateDomainRegistrationInformation nebo akce|Ověření objektu nákupu domény bez odeslání|
|/ checkDomainAvailability nebo akce|Zkontrolujte, jestli je možné zakoupit domény|
|/ listDomainRecommendations nebo akce|Načtení hello seznamu domény doporučení na základě klíčových slov|
|/ registrace nebo akce|Registrace poskytovatele prostředků Domains Microsoft hello hello předplatného|
|/ domény/čtení|Získání seznamu hello domén|
|/ domény/zápisu|Přidat novou doménu nebo aktualizovat stávající|
|/ domény/odstranit|Odstraňte existující domény.|
|/Domains/operationresults/Read|Získat operace domény|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Operace | Popis |
|---|---|
|/lcsprojects/Read|Zobrazení služby životního cyklu aplikace Microsoft Dynamics projektů, které patří tooa uživatele|
|/ lcsprojects/zápisu|Vytvářet a aktualizovat projekty služby Microsoft Dynamics životního cyklu, které patří toohello uživatele. Lze aktualizovat pouze vlastnosti hello název a popis. Hello předplatném a umístění přidružené hello projektu nelze aktualizovat, po vytvoření|
|/lcsprojects/DELETE|Odstranění služby životního cyklu aplikace Microsoft Dynamics projekty, které patří toohello uživatele|
|/lcsprojects/clouddeployments/Read|Zobrazit Microsoft Dynamics AX 2012 R3 vyhodnocení nasazení v projektu služby životního cyklu aplikace Microsoft Dynamics, které patří tooa uživatele|
|/lcsprojects/clouddeployments/Write|Vytvořte Microsoft Dynamics AX 2012 R3 vyhodnocení nasazení v projektu služby životního cyklu aplikace Microsoft Dynamics, které patří tooa uživatele. Nasazení lze provádět z portálu správy Azure|
|/lcsprojects/Connectors/Read|Přečtěte si konektory, které patří tooa služby Microsoft Dynamics životního cyklu projektu|
|/lcsprojects/Connectors/Write|Vytváření a aktualizaci konektory, které patří tooa služby Microsoft Dynamics životního cyklu projektu|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Operace | Popis |
|---|---|
|/ checkNameAvailability nebo akce|Zkontroluje dostupnost oboru názvů v daném předplatném.|
|/ registrace nebo akce|Zaregistruje hello předplatného pro poskytovatele prostředků EventHub hello a umožňuje vytvoření hello EventHub prostředků|
|/ obory názvů a zápis|Vytvořte prostředek Namespace a aktualizujte jeho vlastnosti. Značky a stav hello Namespace jsou hello vlastností, které lze aktualizovat.|
|/namespaces/Read|Získání seznamu hello Namespace prostředků popisu|
|/ obory názvů/odstranit|Odstranit prostředek Namespace|
|/namespaces/metricDefinitions/Read|Získání seznamu Namespace metrik popisy prostředků|
|/namespaces/authorizationRules/Read|Získání seznamu hello popisu obory názvů autorizačních pravidel.|
|/namespaces/authorizationRules/Write|Autorizační pravidla úrovni Namespace vytvořit a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/namespaces/authorizationRules/DELETE|Odstraňte Namespace autorizační pravidlo. Hello výchozí Namespace autorizační pravidlo nelze odstranit. |
|/namespaces/authorizationRules/listkeys/Action|Získat připojovací řetězec toohello hello Namespace|
|/namespaces/authorizationRules/regenerateKeys/Action|Znovu vygenerovat hello primární nebo sekundární klíče toohello prostředků|
|/namespaces/eventhubs/Write|Vytvoření nebo aktualizace EventHub vlastnosti.|
|/namespaces/eventhubs/Read|Získání seznamu popisů EventHub prostředků|
|/namespaces/eventhubs/DELETE|Operace toodelete EventHub prostředků|
|/namespaces/eventHubs/consumergroups/Write|Vytvoření nebo aktualizace ConsumerGroup vlastnosti.|
|/namespaces/eventHubs/consumergroups/Read|Získání seznamu popisů ConsumerGroup prostředků|
|/namespaces/eventHubs/consumergroups/DELETE|Operace toodelete ConsumerGroup prostředků|
|/namespaces/eventhubs/authorizationRules/Read| Získání seznamu hello EventHub autorizačních pravidel|
|/namespaces/eventhubs/authorizationRules/Write|Centrum EventHub autorizační pravidla vytvořit a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/namespaces/eventhubs/authorizationRules/DELETE|Operace toodelete EventHub autorizační pravidla|
|/namespaces/eventhubs/authorizationRules/listkeys/Action|Získat připojovací řetězec tooEventHub hello|
|/namespaces/eventhubs/authorizationRules/regenerateKeys/Action|Znovu vygenerovat hello primární nebo sekundární klíče toohello prostředků|
|/namespaces/diagnosticSettings/Read|Získání seznamu popisů Namespace nastavení pro diagnostiku prostředků|
|/namespaces/diagnosticSettings/Write|Získání seznamu popisů Namespace nastavení pro diagnostiku prostředků|
|/namespaces/logDefinitions/Read|Získání seznamu protokolů Namespace popisy prostředků|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Operace | Popis |
|---|---|
|/providers/features/Read|Získá hello funkci předplatného v daném zprostředkovateli prostředků.|
|/providers/features/Register/Action|Registruje hello funkci pro předplatné v daném zprostředkovateli prostředků.|
|/features/Read|Získá funkce předplatného hello.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Operace | Popis |
|---|---|
|/ clustery a zápis|Vytvořit nebo aktualizovat HDInsight Cluster|
|/Clusters/Read|Získejte podrobnosti o clusteru HDInsight|
|/Clusters/DELETE|Odstranění clusteru HDInsight|
|/Clusters/changerdpsetting/Action|Změna nastavení protokolu RDP pro HDInsight Cluster|
|/Clusters/Configurations/Action|Aktualizace konfigurace clusteru HDInsight|
|/Clusters/Configurations/Read|Získání konfigurace clusteru HDInsight|
|/Clusters/Roles/Resize/Action|Změnit velikost clusteru HDInsight|
|/Locations/Capabilities/Read|Získat možnosti předplatného|
|/Locations/checkNameAvailability/Read|Zkontrolujte dostupnost názvu|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje hello předplatného pro poskytovatele prostředků hello importu a exportu a umožňuje hello vytvoření úlohy importu a exportu.|
|/ úlohy/zápisu|Vytvoří úlohu s hello zadané parametry nebo aktualizovat vlastnosti hello nebo značky pro zadanou úlohu hello.|
|/Jobs/Read|Získá hello vlastnosti pro zadanou úlohu hello nebo vrátí hello seznam úloh.|
|/Jobs/listBitLockerKeys/Action|Získá hello klíče nástroje BitLocker pro zadanou úlohu hello.|
|/Jobs/DELETE|Odstraní stávající úloze.|
|/Locations/Read|Získá vlastnosti hello hello zadaného umístění nebo vrátí hello seznam umístění.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| Operace | Popis |
|---|---|
|/ Registrace nebo akce|Registrace zprostředkovatele Statistika microsoft hello|
|/ AlertRules/zápis|Zápis tooan pravidlo upozornění konfigurace|
|AlertRules nebo odstranění|Odstraňuje se konfigurace pravidla výstrahy.|
|/ AlertRules/čtení|Čte se konfigurace pravidla výstrahy.|
|/ AlertRules nebo aktivovat nebo akce|Pravidlo výstrahy, které jsou aktivované|
|/ AlertRules/vyřešit nebo akce|Pravidlo výstrahy vyřešil|
|/ AlertRules/omezeny nebo akce|Pravidlo výstrahy je omezen.|
|/ AlertRules/incidenty/čtení|Čte se konfigurace incidentu pravidla výstrahy.|
|/ MetricDefinitions/čtení|Číst definice metrik|
|/eventtypes/Values/Read|Číst hodnoty typů událostí správy|
|/eventtypes/digestevents/Read|Číst výtah typů událostí správy|
|/ Metriky/čtení|Číst metriky|
|/ LogProfiles/zápis|Zápis konfiguraci profilu protokolu tooa|
|LogProfiles nebo odstranění|Odstranit konfiguraci profilů protokolu|
|/ LogProfiles/čtení|Profily protokolu pro čtení|
|/ AutoscaleSettings/zápis|Zápis tooan škálování nastavení konfigurace|
|AutoscaleSettings nebo odstranění|Odstraňuje se konfigurace nastavení automatického škálování.|
|/ AutoscaleSettings/čtení|Čte se konfigurace nastavení automatického škálování.|
|/ AutoscaleSettings/Scaleup nebo akce|Operace automatického vertikálního navýšení kapacity|
|/ AutoscaleSettings/Scaledown nebo akce|Škálování vertikálně operace|
|/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read|Číst definice metrik|
|/ ActivityLogAlerts nebo aktivovat nebo akce|Spouštěná hello aktivity protokolu výstrah|
|/ DiagnosticSettings/zápis|Konfigurace nastavení toodiagnostic zápis|
|DiagnosticSettings nebo odstranění|Odstraňuje se konfigurace nastavení pro diagnostiku|
|/ DiagnosticSettings/čtení|Čte se konfigurační nastavení diagnostiky.|
|/ LogDefinitions/čtení|Číst definice protokolu|
|/ ExtendedDiagnosticSettings/zápis|Konfigurace nastavení pro diagnostiku tooextended zápis|
|ExtendedDiagnosticSettings nebo odstranění|Odstraňuje se konfigurace rozšířených nastavení diagnostiky|
|/ ExtendedDiagnosticSettings/čtení|Čtení konfigurace rozšířených nastavení diagnostiky|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné|
|/checkNameAvailability/Read|Zkontroluje, jestli je název trezoru klíčů platný a jestli se nepoužívá.|
|/vaults/Read|Zobrazení vlastností hello trezoru klíčů|
|/ trezory a zápis|Vytvoření nového klíče trezoru nebo aktualizace hello vlastnosti existující trezor klíčů|
|/vaults/DELETE|Odstranění trezoru klíčů|
|/vaults/Deploy/Action|Umožňuje přístup k toosecrets v trezoru klíčů při nasazování prostředků Azure|
|/vaults/secrets/Read|Zobrazit vlastnosti hello tajný klíč, ale nikoli jeho hodnotu|
|/vaults/secrets/Write|Vytvořte novou hodnotu hello tajný klíč nebo aktualizaci existujícího tajného klíče|
|/vaults/accessPolicies/Write|Aktualizovat existující zásady přístupu slučování nebo výměna nebo přidat nový trezor tooa zásad přístupu.|
|/deletedVaults/Read|Zobrazení vlastností hello soft odstranit trezorů klíčů|
|/Locations/operationResults/Read|Výsledek kontroly hello dlouho spuštění operace|
|/Locations/deletedVaults/Read|Zobrazení vlastností hello obnovitelného odstranit trezor klíčů|
|/Locations/deletedVaults/PURGE/Action|Vyprázdnění logicky odstraněné trezoru klíčů|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Operace | Popis |
|---|---|
|/workflows/Read|Přečte hello pracovního postupu.|
|/ pracovní postupy a zápis|Vytvoří nebo aktualizuje hello pracovního postupu.|
|/workflows/DELETE|Odstraní pracovní postup hello.|
|/workflows/Run/Action|Spustí spustit hello pracovního postupu.|
|/workflows/disable/Action|Zakáže hello pracovního postupu.|
|/workflows/enable/Action|Umožňuje hello pracovního postupu.|
|/workflows/Validate/Action|Ověří hello pracovního postupu.|
|/workflows/Move/Action|Přesune pracovního postupu z jeho existující skupinu prostředků id předplatného, skupinu prostředků nebo id jiného předplatného tooa názvu, nebo název.|
|/workflows/listSwagger/Action|Získá swagger definice pracovního postupu hello.|
|/workflows/regenerateAccessKey/Action|Regeneruje klíče tajné klíče hello přístup.|
|/workflows/listCallbackUrl/Action|Získá adresu URL hello zpětného volání pro pracovní postup.|
|/workflows/versions/Read|Přečte verze hello pracovního postupu.|
|/workflows/versions/Triggers/listCallbackUrl/Action|Získá adresu URL hello zpětného volání pro aktivační událost.|
|/ pracovních/spouští/číst|Přečte hello workflow spustit.|
|/workflows/runs/Cancel/Action|Zruší hello spuštění pracovního postupu.|
|/workflows/runs/Actions/Read|Přečte hello pracovní postup spustit akci.|
|/workflows/runs/Operations/Read|Přečte hello workflow běžet stav operace.|
|/workflows/Triggers/Read|Přečte hello aktivační události.|
|/workflows/Triggers/Run/Action|Provede hello aktivační události.|
|/workflows/Triggers/listCallbackUrl/Action|Získá adresu URL hello zpětného volání pro aktivační událost.|
|/workflows/Triggers/histories/Read|Přečte historií hello aktivační události.|
|/workflows/Triggers/histories/resubmit/Action|Znovu odešle aktivační událost pracovního postupu hello.|
|/workflows/accessKeys/Read|Přečte hello přístupový klíč.|
|/workflows/accessKeys/Write|Vytvoří nebo aktualizuje hello přístupový klíč.|
|/workflows/accessKeys/DELETE|Odstraní hello přístupový klíč.|
|/workflows/accessKeys/list/Action|Obsahuje seznam klíčů tajné klíče hello přístup.|
|/workflows/accessKeys/regenerate/Action|Regeneruje klíče tajné klíče hello přístup.|
|/Locations/workflows/Validate/Action|Ověří hello pracovního postupu.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné hello hello strojového učení zprostředkovatel prostředků webové služby a umožňuje vytvoření hello webových služeb.|
|/ webovým službám nebo akce|Vytvořit místní vlastnosti webové služby pro podporované oblasti|
|/commitmentPlans/Read|Číst všechny strojového učení závazků plán|
|/ commitmentPlans/zápisu|Vytvořit nebo aktualizovat každého Machine Learning závazků plánu|
|/commitmentPlans/DELETE|Odstranit všechny strojového učení závazků plán|
|/commitmentPlans/JOIN/Action|Připojte všechny strojového učení závazků plán|
|/commitmentPlans/commitmentAssociations/Read|Číst všechny strojového učení závazků přidružení plánu|
|/commitmentPlans/commitmentAssociations/Move/Action|Přesunout všechny strojového učení závazků přidružení plánu|
|/ Pracovních prostorů/čtení|Číst všechny strojového učení pracovního prostoru|
|A pracovní prostory/zápis|Vytvořit nebo aktualizovat všechny strojového učení pracovního prostoru|
|Pracovních prostorů a odstranění|Odstranit všechny strojového učení pracovního prostoru|
|/ Pracovních prostorů nebo listworkspacekeys nebo akce|Seznam klíčů pro pracovní prostor Machine Learning|
|/ Pracovních prostorů nebo resyncstoragekeys nebo akce|Nové synchronizace klíče účtu úložiště, které jsou nakonfigurované pro pracovní prostor Machine Learning|
|/webServices/Read|Číst všechny webová služba Machine Learning|
|/ webovým službám a zápis|Vytvořit nebo aktualizovat žádné webové služby Machine Learning|
|/webServices/DELETE|Odstranit všechny webová služba Machine Learning|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Operace | Popis |
|---|---|
|/agreements/OFFERS/plans/Read|Vrátí smlouvu pro položku daného webu marketplace|
|/agreements/OFFERS/plans/Sign/Action|Podepsat smlouvu pro položku daného webu marketplace|
|/agreements/OFFERS/plans/Cancel/Action|Zrušit smlouvu pro položku daného webu marketplace|

## <a name="microsoftmedia"></a>Microsoft.Media

| Operace | Popis |
|---|---|
|/mediaservices/Read||
|/ mediaservices/zápisu||
|/mediaservices/DELETE||
|/mediaservices/regenerateKey/Action||
|/mediaservices/listKeys/Action||
|/mediaservices/syncStorageKeys/Action||

## <a name="microsoftnetwork"></a>Microsoft.Network

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné hello|
|nebo zrušit registraci nebo akce|Zruší registraci předplatného hello|
|/ checkTrafficManagerNameAvailability nebo akce|Zkontroluje dostupnost hello názvu DNS relativní Traffic Manageru.|
|/dnszones/Read|Získáte hello zóny DNS, ve formátu JSON. Hello vlastnosti zóny zahrnují značky, etag, numberOfRecordSets a maxNumberOfRecordSets. Všimněte si, že tento příkaz nenačítá sady záznamů hello obsažených v zóně hello.|
|/ dnszones/zápisu|Vytvořit nebo aktualizovat zónu DNS v rámci skupiny prostředků.  Použít tooupdate hello značek u prostředku zóny DNS. Všimněte si, že tento příkaz nelze použít toocreate nebo aktualizace sady záznamů v zóně hello.|
|/dnszones/DELETE|Odstraňte zónu DNS hello, ve formátu JSON. Hello vlastnosti zóny zahrnují značky, etag, numberOfRecordSets a maxNumberOfRecordSets.|
|/dnszones/MX/Read|Získáte hello sadu záznamů typu "MX", ve formátu JSON. Hello sada záznamů obsahuje seznam záznamů, jakož i hello TTL, značky a etag.|
|/dnszones/MX/Write|Vytvořit nebo aktualizovat sadu záznamů typu "MX" v rámci zóny DNS. zaznamenává Hello zadaný nahradí hello aktuální záznamy v sadě záznamů hello.|
|/dnszones/MX/DELETE|Odebrat hello sadu záznamů daného názvu a typu "MX" ze zóny DNS.|
|/dnszones/NS/Read|Získá DNS sadu záznamů typu NS|
|/dnszones/NS/Write|Vytvoří nebo aktualizuje DNS sadu záznamů typu NS|
|/dnszones/NS/DELETE|Odstraní hello DNS sadu záznamů typu NS|
|/dnszones/AAAA/Read|Získáte hello sadu záznamů typu 'AAAA, ve formátu JSON. Hello sada záznamů obsahuje seznam záznamů, jakož i hello TTL, značky a etag.|
|/dnszones/AAAA/Write|Vytvořit nebo aktualizovat sadu záznamů typu 'AAAA, v rámci zóny DNS. zaznamenává Hello zadaný nahradí hello aktuální záznamy v sadě záznamů hello.|
|/dnszones/AAAA/DELETE|Odebrat hello sadu záznamů daného názvu a typu 'AAAA, umožňuje ze zóny DNS.|
|/dnszones/CNAME/Read|Získáte hello sadu záznamů typu 'CNAME' ve formátu JSON. sady záznamů Hello obsahuje hello TTL, značky a etag.|
|/dnszones/CNAME/Write|Vytvořit nebo aktualizovat sadu záznamů typu, CNAME, v rámci zóny DNS. zaznamenává Hello zadaný nahradí hello aktuální záznamy v sadě záznamů hello.|
|/dnszones/CNAME/DELETE|Odebrat hello sadu záznamů daného názvu a typu, CNAME, umožňuje ze zóny DNS.|
|/dnszones/SOA/Read|Získá sadu záznamů DNS typu SOA|
|/dnszones/SOA/Write|Vytvoří nebo aktualizuje DNS sadu záznamů typu SOA|
|/dnszones/SRV/Read|Získáte hello sadu záznamů typu 'SRV' ve formátu JSON. Hello sada záznamů obsahuje seznam záznamů, jakož i hello TTL, značky a etag.|
|/dnszones/SRV/Write|Vytvořit nebo aktualizovat sadu záznamů typu SRV|
|/dnszones/SRV/DELETE|Odebrat hello sadu záznamů daného názvu a typu SRV "služby" ze zóny DNS.|
|/dnszones/PTR/Read|Získáte hello sadu záznamů typu 'PTR, ve formátu JSON. Hello sada záznamů obsahuje seznam záznamů, jakož i hello TTL, značky a etag.|
|/dnszones/PTR/Write|Vytvořit nebo aktualizovat sadu záznamů typu 'PTR, v rámci zóny DNS. zaznamenává Hello zadaný nahradí hello aktuální záznamy v sadě záznamů hello.|
|/dnszones/PTR/DELETE|Odebrat hello sadu záznamů daného názvu a typu 'PTR, umožňuje ze zóny DNS.|
|/dnszones/A/Read|Získáte hello sadu záznamů typu "A", ve formátu JSON. Hello sada záznamů obsahuje seznam záznamů, jakož i hello TTL, značky a etag.|
|/dnszones/A/Write|Vytvořit nebo aktualizovat sadu záznamů typu "A" v rámci zóny DNS. zaznamenává Hello zadaný nahradí hello aktuální záznamy v sadě záznamů hello.|
|/dnszones/A/DELETE|Odebrat hello sadu záznamů daného názvu a typu "A" ze zóny DNS.|
|/dnszones/txt/Read|Získáte hello sadu záznamů typu 'TXT, ve formátu JSON. Hello sada záznamů obsahuje seznam záznamů, jakož i hello TTL, značky a etag.|
|/dnszones/txt/Write|Vytvořit nebo aktualizovat sadu záznamů typu 'TXT, v rámci zóny DNS. zaznamenává Hello zadaný nahradí hello aktuální záznamy v sadě záznamů hello.|
|/dnszones/txt/DELETE|Odebrat hello sadu záznamů daného názvu a typu 'TXT, umožňuje ze zóny DNS.|
|/dnszones/recordsets/Read|Získá sady záznamů DNS mezi typy|
|/networkInterfaces/Read|Získá definici rozhraní sítě. |
|/ networkInterfaces/zápisu|Vytvoří rozhraní sítě nebo aktualizuje existující rozhraní sítě. |
|/networkInterfaces/JOIN/Action|Spojí tooa síťové rozhraní virtuálního počítače|
|/networkInterfaces/DELETE|Odstraní rozhraní sítě|
|/networkInterfaces/effectiveRouteTable/Action|Získejte tabulku směrování nakonfigurovaný v síťovém rozhraní hello virtuálních počítačů|
|/networkInterfaces/effectiveNetworkSecurityGroups/Action|Získat skupiny zabezpečení sítě nakonfigurované na síťové rozhraní z hello virtuálních počítačů|
|/networkInterfaces/loadBalancers/Read|Získá všechny hello Vyrovnávání zatížení, které hello síťové rozhraní je součástí|
|/networkInterfaces/ipconfigurations/Read|Získá definici konfigurace ip rozhraní sítě. |
|/publicIPAddresses/Read|Získá definici veřejné ip adresy.|
|/ publicipaddresses, na které a zápis|Vytvoří veřejnou Ip adresu nebo aktualizuje existující veřejnou Ip adresu. |
|/publicIPAddresses/DELETE|Odstraní veřejnou Ip adresu.|
|/publicIPAddresses/JOIN/Action|Spojí veřejnou ip adresu.|
|/routeFilters/Read|Získá definici filtru trasy|
|/routeFilters/JOIN/Action|Spojí filtr trasy|
|/routeFilters/DELETE|Odstraní definici směrovací filtru|
|/ routeFilters/zápisu|Vytvoří filtr trasu nebo aktualizuje existující směrovací filtr|
|/routeFilters/Rules/Read|Získá definici pravidla filtru trasy|
|/routeFilters/Rules/Write|Vytvoří pravidlo filtru trasu nebo aktualizuje existující pravidlo filtru trasy|
|/routeFilters/Rules/DELETE|Odstraní definici směrovací filtr pravidlo|
|/networkWatchers/Read|Získat definici sledovací proces sítě hello|
|/ networkWatchers/zápisu|Sledovací proces sítě vytvoří nebo aktualizuje existující sledovací proces sítě|
|/networkWatchers/DELETE|Odstraní sledovací proces sítě|
|/networkWatchers/configureFlowLog/Action|Nakonfiguruje toku protokolování pro cílový prostředek.|
|/networkWatchers/ipFlowVerify/Action|Vrátí, zda hello paket povolený nebo zakázaný tooor z konkrétní cíl.|
|/networkWatchers/nextHop/Action|U zadaného cíle a cílové IP adresy vrátí typ dalšího směrování hello a další naděje IP adresu.|
|/networkWatchers/queryFlowLogStatus/Action|Získá stav hello toku protokolování na prostředku.|
|/networkWatchers/queryTroubleshootResult/Action|Získá hello řešení potíží s výsledek z již dříve spuštěna hello nebo řešení potíží s operace spuštěna.|
|/networkWatchers/securityGroupView/Action|Zobrazit hello nakonfigurované a pravidel skupiny zabezpečení sítě efektivní použita na virtuálním počítači.|
|/networkWatchers/Topology/Action|Získá úrovně zobrazení síťových prostředků a jejich vztahů ve skupině prostředků.|
|/networkWatchers/troubleshoot/Action|Spustí se Poradce při potížích s na prostředku sítě v Azure.|
|/networkWatchers/packetCaptures/queryStatus/Action|Získá informace o vlastnostech a stav prostředku zachytávání paketů.|
|/networkWatchers/packetCaptures/stop/Action|Zastavte hello spuštění relace zachytávání paketů.|
|/networkWatchers/packetCaptures/Read|Načtení definice zachytávání paketů hello|
|/networkWatchers/packetCaptures/Write|Vytvoří zachytávání paketů|
|/networkWatchers/packetCaptures/DELETE|Odstraní zachytávání paketů|
|/loadBalancers/Read|Získá definici nástroje pro vyrovnávání zatížení.|
|/ loadBalancers/zápisu|Vytvoří nástroj pro vyrovnávání zatížení nebo aktualizuje existující pro vyrovnávání zatížení|
|/loadBalancers/DELETE|Odstraní nástroj pro vyrovnávání zatížení|
|/loadBalancers/networkInterfaces/Read|Získá odkazy tooall hello rozhraní sítě pod nástrojem pro vyrovnávání zatížení|
|/loadBalancers/loadBalancingRules/Read|Získá zatížení nástroje pro vyrovnávání zatížení vyrovnávání definici pravidla|
|/loadBalancers/backendAddressPools/Read|Získá definici fondu adres back-end zatížení nástroje pro vyrovnávání|
|/loadBalancers/backendAddressPools/JOIN/Action|Spojí fond back-end adresy Vyrovnávání zatížení.|
|/loadBalancers/inboundNatPools/Read|Získá Vyrovnávání zatížení příchozí nat definici fondu|
|/loadBalancers/inboundNatPools/JOIN/Action|Spojí Vyrovnávání zatížení fond příchozích pravidel nat|
|/loadBalancers/inboundNatRules/Read|Nástroj pro vyrovnávání zatížení získá definici příchozího pravidla nat|
|/loadBalancers/inboundNatRules/Write|Vytvoří pravidlo příchozí nat nástroje pro vyrovnávání zatížení nebo aktualizuje existující pravidlo příchozí nat nástroje pro vyrovnávání zatížení|
|/loadBalancers/inboundNatRules/DELETE|Odstraní pravidlo příchozí nat nástroje pro vyrovnávání zatížení.|
|/loadBalancers/inboundNatRules/JOIN/Action|Spojí pravidlo příchozí nat nástroje pro vyrovnávání zatížení.|
|/loadBalancers/outboundNatRules/Read|Získá definici pravidla odchozí nat nástroje pro vyrovnávání zatížení|
|/loadBalancers/probes/Read|Získá sondu nástroje pro vyrovnávání zatížení|
|/loadBalancers/virtualMachines/Read|Získá odkazy tooall hello virtuální počítače pod nástrojem pro vyrovnávání zatížení|
|/loadBalancers/frontendIPConfigurations/Read|Získá definice konfigurace IP front-endu nástroje pro vyrovnávání zatížení|
|/trafficManagerGeographicHierarchies/Read|Získá hello Traffic Manager Geographic hierarchie obsahující oblastí, které se dají použít s hello metodu směrování provozu geografické|
|/bgpServiceCommunities/Read|Získat komunit protokolu Bgp služby|
|/applicationGatewayAvailableWafRuleSets/Read|Získá aplikační bránu, kterou sad pravidel k dispozici firewall webových aplikací|
|/virtualNetworks/Read|Získat definici virtuální sítě hello|
|/ virtualNetworks/zápisu|Vytvoří virtuální síť nebo aktualizuje existující virtuální síť|
|/virtualNetworks/DELETE|Odstraní virtuální sítě|
|/virtualNetworks/peer/Action|Partnerský vztah virtuální síť s jinou virtuální sítí|
|/virtualNetworks/virtualNetworkPeerings/Read|Získá definici partnerského vztahu virtuální sítě|
|/virtualNetworks/virtualNetworkPeerings/Write|Vytvoří partnerský vztah virtuální sítě nebo aktualizuje existující virtuální síť partnerský vztah|
|/virtualNetworks/virtualNetworkPeerings/DELETE|Odstraní virtuální sítě partnerského vztahu|
|/virtualNetworks/subnets/Read|Získá definici podsítě virtuální sítě|
|/virtualNetworks/subnets/Write|Vytvoří podsíť virtuální sítě nebo aktualizuje existující podsíť virtuální sítě|
|/virtualNetworks/subnets/DELETE|Odstraní podsíť virtuální sítě|
|/virtualNetworks/subnets/JOIN/Action|Připojí virtuální sítě|
|/virtualNetworks/subnets/joinViaServiceTunnel/Action|Spojí prostředků, jako je účet úložiště nebo SQL databáze tooa služby tunelové propojení povoleno podsítě.|
|/virtualNetworks/subnets/virtualMachines/Read|Získá odkazy tooall hello virtuální počítače v podsíti virtuální sítě|
|/virtualNetworks/checkIpAddressAvailability/Read|Zkontrolujte, jestli je k dispozici v zadané virtuální síti hello Ip adresu|
|/virtualNetworks/virtualMachines/Read|Získá odkazy tooall hello virtuální počítače ve virtuální síti|
|/expressRouteServiceProviders/Read|Získá poskytovatele služeb Expressroute|
|/dnsoperationresults/Read|Získá výsledky operace DNS|
|/localnetworkgateways/Read|Získá LocalNetworkGateway|
|/ localnetworkgateways/zápisu|Vytvoří nebo aktualizuje existující LocalNetworkGateway|
|/localnetworkgateways/DELETE|Odstraní LocalNetworkGateway|
|/trafficManagerProfiles/Read|Získání konfigurace profilu Správce provozu hello. To zahrnuje nastavení DNS, nastavení směrování provozu, nastavení monitorování koncového bodu a hello seznam koncových bodů směrovaných tímto profilem Traffic Manager.|
|/ trafficManagerProfiles/zápisu|Vytvoření profilu Správce provozu nebo změnit konfiguraci hello existující profil Traffic Manageru. To zahrnuje zapnutí nebo vypnutí profilu a změna nastavení DNS, nastavení směrování provozu nebo nastavení monitorování koncového bodu. Koncových bodů směrovaných tímto profil služby Traffic Manager hello můžete přidat, odebrat, povolit nebo zakázat.|
|/trafficManagerProfiles/DELETE|Odstraňte profil služby Traffic Manager hello. Všechna nastavení spojená s hello profil služby Traffic Manager se ztratí a hello profil už se nedá použít tooroute provoz.|
|/dnsoperationstatuses/Read|Získá stav operace DNS |
|/Operations/Read|Získat dostupné operace|
|/expressRouteCircuits/Read|Získat ExpressRouteCircuit|
|/ expressRouteCircuits/zápisu|Vytvoří nebo aktualizuje existující ExpressRouteCircuit|
|/expressRouteCircuits/DELETE|Odstraní ExpressRouteCircuit|
|/expressRouteCircuits/stats/Read|Získá ExpressRouteCircuit Stat|
|/expressRouteCircuits/peerings/Read|Získá partnerský vztah ExpressRouteCircuit|
|/expressRouteCircuits/peerings/Write|Vytvoří nebo aktualizuje existující ExpressRouteCircuit partnerský vztah|
|/expressRouteCircuits/peerings/DELETE|Odstraní partnerský vztah ExpressRouteCircuit|
|/expressRouteCircuits/peerings/arpTables/Action|Získá ArpTable ExpressRouteCircuit partnerského vztahu|
|/expressRouteCircuits/peerings/routeTables/Action|Získá RouteTable ExpressRouteCircuit partnerského vztahu|
|/expressRouteCircuits/partnerských vztahů /<br>routeTablesSummary nebo akce|Získá ExpressRouteCircuit partnerský vztah RouteTable souhrn|
|/expressRouteCircuits/peerings/stats/Read|Získá ExpressRouteCircuit partnerský vztah Statistika|
|/expressRouteCircuits/authorizations/Read|Získá povolení ExpressRouteCircuit|
|/expressRouteCircuits/authorizations/Write|Vytvoří nebo aktualizuje existující povolení ExpressRouteCircuit|
|/expressRouteCircuits/authorizations/DELETE|Odstraní ExpressRouteCircuit povolení|
|/Connections/Read|Získá VirtualNetworkGatewayConnection|
|/ připojení a zápis|Vytvoří nebo aktualizuje existující VirtualNetworkGatewayConnection|
|/Connections/DELETE|Odstraní VirtualNetworkGatewayConnection|
|/Connections/sharedKey/Read|Získá VirtualNetworkGatewayConnection SharedKey|
|/Connections/sharedKey/Write|Vytvoří nebo aktualizuje existující VirtualNetworkGatewayConnection SharedKey|
|/networkSecurityGroups/Read|Získá definici skupiny zabezpečení sítě|
|/ skupin zabezpečení sítě a zápis|Vytvoří skupinu zabezpečení sítě nebo aktualizuje existující skupinu zabezpečení sítě|
|/networkSecurityGroups/DELETE|Odstraní skupinu zabezpečení sítě|
|/networkSecurityGroups/JOIN/Action|Spojí skupinu zabezpečení sítě|
|/networkSecurityGroups/defaultSecurityRules/Read|Získá definici pravidla zabezpečení výchozí|
|/networkSecurityGroups/securityRules/Read|Získá definici pravidla zabezpečení|
|/networkSecurityGroups/securityRules/Write|Vytvoří pravidlo zabezpečení nebo aktualizuje existující pravidlo zabezpečení.|
|/networkSecurityGroups/securityRules/DELETE|Odstraní pravidlo zabezpečení|
|/applicationGateways/Read|Získá aplikační bránu.|
|/ applicationGateways/zápisu|Služby application gateway vytvoří nebo aktualizuje aplikační bránu.|
|/applicationGateways/DELETE|Odstraní aplikační bránu.|
|/applicationGateways/backendhealth/Action|Získá stavu back-end brány aplikaci|
|/applicationGateways/Start/Action|Spuštění služby application gateway|
|/applicationGateways/stop/Action|Zastavení služby application gateway|
|/applicationGateways/backendAddressPools/JOIN/Action|Spojí fond adres back-end brány aplikace|
|/routeTables/Read|Získá definici směrovací tabulky|
|/ routeTables/zápisu|Vytvoří směrovací tabulku nebo aktualizuje existující směrovací tabulku.|
|/routeTables/DELETE|Odstraní definici směrovací tabulky|
|/routeTables/JOIN/Action|Spojí směrovací tabulku|
|/routeTables/Routes/Read|Získá definici trasy|
|/routeTables/Routes/Write|Vytvoří trasu nebo aktualizuje existující trasu.|
|/routeTables/Routes/DELETE|Odstraní definici směrovací|
|/Locations/operationResults/Read|Získá výsledek operace asynchronní POST nebo operace odstranění|
|/Locations/checkDnsNameAvailability/Read|Ověří, zda je název dns je k dispozici na hello zadané umístění|
|/Locations/usages/Read|Získá metriky využití prostředků hello|
|/Locations/Operations/Read|Získá prostředek operace, který reprezentuje stav asynchronní operace|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje hello předplatného pro poskytovatele prostředků NotifciationHubs hello a umožní hello vytváření oborů názvů a NotificationHubs|
|/ CheckNamespaceAvailability nebo akce|Kontroluje, zda zadaný název prostředku Namespace je k dispozici v rámci hello NotificationHub služby.|
|A obory názvů/zápis|Vytvořte prostředek Namespace a aktualizujte jeho vlastnosti. Značky a stav hello Namespace jsou hello vlastností, které lze aktualizovat.|
|/ Obory názvů/čtení|Získání seznamu hello Namespace prostředků popisu|
|Obory názvů nebo odstranění|Odstranit prostředek Namespace|
|/ Obory názvů nebo authorizationRules nebo akce|Získání seznamu hello popisu obory názvů autorizačních pravidel.|
|/ Obory názvů nebo CheckNotificationHubAvailability nebo akce|Zkontroluje, jestli je v oboru názvů k dispozici název NotificationHub, nebo ne.|
|A obory názvů nebo authorizationRules/zápis|Autorizační pravidla úrovni Namespace vytvořit a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/ Obory názvů nebo authorizationRules/čtení|Získání seznamu hello popisu obory názvů autorizačních pravidel.|
|Obory názvů, authorizationRules nebo odstranění|Odstraňte Namespace autorizační pravidlo. Hello výchozí Namespace autorizační pravidlo nelze odstranit. |
|/ Obory názvů nebo authorizationRules/listkeys nebo akce|Získat připojovací řetězec toohello hello Namespace|
|/ Obory názvů nebo authorizationRules/regenerateKeys nebo akce|Namespace autorizační pravidlo znovu vygenerovat primární nebo sekundární klíč, zadejte hello klíč, který potřebuje toobe znovu vygeneroval.|
|A obory názvů nebo NotificationHubs/zápis|Vytvoření centra oznámení a aktualizujte jeho vlastnosti. Jeho vlastnosti především zahrnovat přihlašovací údaje systému PNS. Autorizační pravidla a hodnota TTL|
|/ Obory názvů nebo NotificationHubs/čtení|Získá seznam popisů prostředků Centra oznámení.|
|Obory názvů, NotificationHubs nebo odstranění|Odstranit prostředek centra oznámení|
|/ Obory názvů nebo NotificationHubs/authorizationRules nebo akce|Získání seznamu hello oznámení centra autorizačních pravidel|
|/ Obory názvů nebo NotificationHubs/pnsCredentials nebo akce|Získáte všechna pověření systému PNS centra oznámení. To zahrnuje, přihlašovací údaje WNS, MPNS, APNS, GCM a Baidu|
|/ Obory názvů nebo NotificationHubs/debugSend nebo akce|Poslat testovací nabízené oznámení|
|/ Obory názvů nebo NotificationHubs/metricDefinitions/čtení|Získání seznamu Namespace metrik popisy prostředků|
|/Namespaces/NotificationHubs /<br>authorizationRules a zápis|Vytvoření pravidla autorizace centra oznámení a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/Namespaces/NotificationHubs /<br>authorizationRules/čtení|Získání seznamu hello oznámení centra autorizačních pravidel|
|/Namespaces/NotificationHubs /<br>authorizationRules nebo odstranění|Odstranit autorizační pravidla Centra oznámení|
|/Namespaces/NotificationHubs /<br>authorizationRules/listkeys nebo akce|Získat toohello hello připojovací řetězec centra oznámení|
|/Namespaces/NotificationHubs /<br>authorizationRules/regenerateKeys nebo akce|Oznámení centra autorizační pravidlo znovu vygenerovat primární nebo sekundární klíč, zadejte hello klíč, který potřebuje toobe znovu vygeneroval.|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Registrace poskytovatele prostředků tooa předplatné.|
|/linkTargets/Read|Zobrazí seznam stávajících účtů, které nejsou přidružené předplatné Azure. toolink tento pracovní prostor tooa předplatné Azure, použijte id zákazníka vrácená touto operací ve vlastnosti id zákazníka hello hello operaci vytvořit pracovní prostor.|
|/ pracovních prostorů a zápis|Vytvoří nový pracovní prostor nebo existujícímu pracovnímu prostoru tooan odkazy tím, že poskytuje id zákazníka hello z hello existujícímu pracovnímu prostoru.|
|/workspaces/Read|Získá existujícímu pracovnímu prostoru|
|/workspaces/DELETE|Odstraní pracovního prostoru. Pokud bylo propojeno hello prostoru hello existujícímu pracovnímu prostoru tooan pak při vytvoření pracovního prostoru, který byl odkazovaný toois nebyl odstraněn.|
|/workspaces/generateregistrationcertificate/Action|Vygeneruje certifikát o registraci pro hello prostoru. Tento certifikát je použité tooconnect Microsoft System Center Operation Manager toohello prostoru.|
|/workspaces/sharedKeys/Action|Načte hello sdíleného klíče pro pracovní prostor hello. Tyto klíče jsou použité tooconnect statistice provozu Microsoft agenty toohello prostoru.|
|/workspaces/Search/Action|Provede vyhledávací dotaz.|
|/workspaces/Datasources/Read|Získáte zdrojů dat v pracovním prostoru.|
|/workspaces/Datasources/Write|Vytvořit nebo aktualizovat zdroje dat v pracovním prostoru.|
|/workspaces/Datasources/DELETE|Odstraňte zdrojů dat v pracovním prostoru.|
|/workspaces/managementGroups/Read|Získá hello názvy a metadat pro System Center Operations Manager pracovního prostoru toothis připojené skupiny správy.|
|/workspaces/Schema/Read|Získá hello vyhledávání schématu pro hello prostoru.  Hledání schématu zahrnuje hello zveřejněné pole a jejich typy.|
|/workspaces/usages/Read|Získá data o využití pro pracovní prostor, včetně hello množství dat číst hello prostoru.|
|/workspaces/intelligencepacks/Read|Obsahuje seznam všech intelligence Pack, které jsou viditelné pro danou worksapce a také uvádí, jestli je povolený nebo zakázaný pracovního prostoru hello pack.|
|/workspaces/intelligencepacks/enable/Action|Umožňuje intelligence pack pro daný pracovní prostor.|
|/workspaces/intelligencepacks/disable/Action|Zakáže intelligence pack pro daný pracovní prostor.|
|/workspaces/sharedKeys/Read|Načte hello sdíleného klíče pro pracovní prostor hello. Tyto klíče jsou použité tooconnect statistice provozu Microsoft agenty toohello prostoru.|
|/workspaces/savedSearches/Read|Získá uložené vyhledávací dotaz.|
|/workspaces/savedSearches/Write|Vytvoří uložené vyhledávací dotaz.|
|/workspaces/savedSearches/DELETE|Odstraní uložené vyhledávací dotaz.|
|/workspaces/storageinsightconfigs/Write|Vytvoří novou konfiguraci úložiště. Tyto konfigurace jsou použité toopull data z umístění, do stávající účet úložiště.|
|/workspaces/storageinsightconfigs/Read|Získá konfiguraci úložiště.|
|/workspaces/storageinsightconfigs/DELETE|Odstraní konfigurace úložiště. To se zastaví statistice provozu Microsoft z čtení dat z účtu úložiště hello.|
|/workspaces/configurationScopes/Read|Získání konfigurace oboru|
|/workspaces/configurationScopes/Write|Nastavení konfigurace oboru|
|/workspaces/configurationScopes/DELETE|Odstranit konfigurace oboru|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Registrace poskytovatele prostředků tooa předplatné.|
|/ řešení a zápis|Vytvořte nové řešení OMS|
|/Solutions/Read|Získat ukončení OMS řešení|
|/Solutions/DELETE|Odstraňte existující řešení OMS|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Operace | Popis |
|---|---|
|/ Trezory/backupJobsExport nebo akce|Export úloh|
|A trezory/zápis|Operace Vytvořit trezor vytvoří prostředek Azure typu trezor.|
|/ Trezory/čtení|Hello operace získání trezoru získá objekt, který reprezentuje hello prostředků Azure typu 'trezoru.|
|Trezory nebo odstranění|Hello odstranit trezor operaci odstranění hello zadaný Azure prostředek typu 'trezoru.|
|/ Trezory/refreshContainers/čtení|Aktualizuje seznam kontejneru hello|
|/ Trezory/backupJobsExport/operationResults/čtení|Vrátí hello výsledek exportovat úlohy operací.|
|/ Trezory/backupOperationResults/čtení|Vrátí výsledek operace zálohování trezoru Recovery Services.|
|/ Trezory/monitoringAlerts/čtení|Získá hello výstrahy pro trezor služeb zotavení hello.|
|/Vaults/monitoringAlerts / {uniqueAlertId} / číst|Získá hello podrobnosti výstrahy hello.|
|/ Trezory/backupSecurityPIN/čtení|Vrátí zabezpečení PIN kódu informace pro obnovení služeb trezoru.|
|/vaults/replicationEvents/Read|Číst všechny události|
|/ Trezory/backupProtectableItems/čtení|Vrátí seznam chránitelných položek.|
|/vaults/replicationFabrics/Read|Číst všechny prostředky infrastruktury|
|/vaults/replicationFabrics/Write|Vytvořit nebo aktualizovat všechny prostředky infrastruktury|
|/vaults/replicationFabrics/Remove/Action|Odebrání infrastruktury|
|/vaults/replicationFabrics/checkConsistency/Action|Kontroly konzistence hello prostředků infrastruktury|
|/vaults/replicationFabrics/DELETE|Odstranit všechny prostředky infrastruktury|
|/vaults/replicationFabrics/renewcertificate/Action||
|/vaults/replicationFabrics/deployProcessServerImage/Action|Nasadit Image procesového serveru|
|/vaults/replicationFabrics/reassociateGateway/Action|Přidružení brány|
|/ trezory/replicationFabrics/replicationRecoveryServicesProviders /<br>Pro čtení|Přečtěte si zprostředkovatelů služby obnovení|
|/ trezory/replicationFabrics/replicationRecoveryServicesProviders /<br>odebrat nebo akce|Odebrat zprostředkovatele služeb zotavení|
|/ trezory/replicationFabrics/replicationRecoveryServicesProviders /<br>Odstranit|Odstranit zprostředkovatelů služby obnovení|
|/ trezory/replicationFabrics/replicationRecoveryServicesProviders /<br>refreshProvider nebo akce|Aktualizujte zprostředkovatele|
|/vaults/replicationFabrics/replicationStorageClassifications/Read|Číst všechny klasifikace úložiště|
|/ trezory/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/čtení|Číst veškerá jeho mapování klasifikace úložiště|
|/ trezory/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings a zápis|Vytvořit nebo aktualizovat veškerá jeho mapování klasifikace úložiště|
|/ trezory/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings nebo odstranění|Odstranit veškerá jeho mapování klasifikace úložiště|
|/vaults/replicationFabrics/replicationvCenters/Read|Číst všechny úlohy|
|/vaults/replicationFabrics/replicationvCenters/Write|Vytvořit nebo aktualizovat všechny úlohy|
|/vaults/replicationFabrics/replicationvCenters/DELETE|Odstranit všechny úlohy|
|/vaults/replicationFabrics/replicationNetworks/Read|Přečtěte si žádné sítě.|
|/ trezory/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/čtení|Číst veškerá jeho mapování sítě|
|/ trezory/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings a zápis|Vytvořit nebo aktualizovat veškerá jeho mapování sítě|
|/ trezory/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings nebo odstranění|Odstranit všechny mapování sítě|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>Pro čtení|Číst všechny kontejnery ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>discoverProtectableItem nebo akce|Zjistit Chránitelná položka|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>zápis|Vytvořit nebo aktualizovat všechny kontejnery ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>odebrat nebo akce|Kontejner ochrany odebrat.|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>switchprotection nebo akce|Kontejner ochrany přepínače|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectableItems/čtení|Číst všechny položky, které jsou předmětem ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/čtení|Číst veškerá jeho mapování kontejnerů ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings a zápis|Vytvořit nebo aktualizovat veškerá jeho mapování kontejnerů ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings, odebrat nebo akce|Odebrat mapování kontejnerů ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings nebo odstranění|Odstranit veškerá jeho mapování kontejnerů ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/čtení|Číst všechny chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems a zápis|Vytvořit nebo aktualizovat všechny chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems nebo odstranění|Odstranit všechny chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems, odebrat nebo akce|Odebrat chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/plannedFailover nebo akce|Plánované převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/unplannedFailover nebo akce|Převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/testFailover nebo akce|Testovací převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/testFailoverCleanup nebo akce|Vyčistit testovací převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/failoverCommit nebo akce|Potvrzení převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems nebo opětovné ochrany nebo akce|Znovu aktivujte ochranu chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/updateMobilityService nebo akce|Aktualizace služby Mobility|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/repairReplication nebo akce|Oprava replikace|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/applyRecoveryPoint nebo akce|Použít bod obnovení|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/recoveryPoints/čtení|Číst všechny body obnovení replikace|
|/vaults/replicationPolicies/Read|Číst všechny zásady|
|/vaults/replicationPolicies/Write|Vytvořit nebo aktualizovat všechny zásady|
|/vaults/replicationPolicies/DELETE|Odstranit všechny zásady|
|/vaults/replicationRecoveryPlans/Read|Číst všechny plány obnovení|
|/vaults/replicationRecoveryPlans/Write|Vytvořit nebo aktualizovat všechny plány obnovení|
|/vaults/replicationRecoveryPlans/DELETE|Odstranit všechny plány obnovení|
|/vaults/replicationRecoveryPlans/plannedFailover/Action|Plán obnovení plánované převzetí služeb při selhání|
|/vaults/replicationRecoveryPlans/unplannedFailover/Action|Plán obnovení převzetí služeb při selhání|
|/vaults/replicationRecoveryPlans/testFailover/Action|Testovací převzetí služeb při selhání obnovení plán|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/Action|Plán obnovení testovací převzetí služeb při selhání čištění|
|/vaults/replicationRecoveryPlans/failoverCommit/Action|Plán obnovení potvrzení převzetí služeb při selhání|
|/vaults/replicationRecoveryPlans/reProtect/Action|Znovu nastavte ochranu plánu obnovení|
|/ Trezory/extendedInformation/čtení|Získá Hello získat rozšířené informace o operaci představující hello prostředků Azure typu informace o rozšířených objektu? trezoru?|
|A trezory/extendedInformation/zápis|Získá Hello získat rozšířené informace o operaci představující hello prostředků Azure typu informace o rozšířených objektu? trezoru?|
|Trezory, extendedInformation nebo odstranění|Získá Hello získat rozšířené informace o operaci představující hello prostředků Azure typu informace o rozšířených objektu? trezoru?|
|/ Trezory/backupManagementMetaData/čtení|Vrátí metadata správy zálohování trezoru Recovery Services.|
|/ Trezory/backupProtectionContainers/čtení|Vrátí všechny kontejnery patřící toohello předplatného|
|/ Trezory/backupFabrics/operationResults/čtení|Vrátí stav operace hello|
|/ Trezory/backupFabrics/protectionContainers/čtení|Vrátí všechny registrované kontejnery|
|/ Trezory/backupFabrics/protectionContainers /<br>operationResults/čtení|Načte výsledky operace provedené na kontejneru ochrany.|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/čtení|Vrátí objekt podrobnosti hello chráněné položce|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems a zápis|Vytvoření zálohy chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems nebo odstranění|Odstranění chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/zálohování nebo akce|Provede zálohování chráněné položky.|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/operationResults/čtení|Načte výsledky operace provedené na chráněných položkách.|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/operationStatus/čtení|Vrátí stav hello operace provedené na chráněné položky.|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints/čtení|Načíst body obnovení pro chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>obnovení nebo akce|Obnoví body obnovení pro chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>provisionInstantItemRecovery nebo akce|Zřízení rychlých položky obnovení pro chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>revokeInstantItemRecovery nebo akce|Odvolat rychlých položky obnovení pro chráněné položky|
|/ Trezory nebo použití/čtení|Vrátí podrobnosti využití trezoru Recovery Services.|
|/vaults/usages/Read|Číst všechny použití trezoru|
|A trezory/certifikátů/zápis|Hello operace aktualizace prostředek certifikátu aktualizuje certifikát přihlašovacích údajů hello prostředků nebo trezoru.|
|/ Trezory/tokenInfo/čtení|Vrátí token informace o trezoru služeb zotavení.|
|/vaults/replicationAlertSettings/Read|Číst všechny nastavení výstrah|
|/vaults/replicationAlertSettings/Write|Vytvořit nebo aktualizovat nastavení výstrah|
|/ Trezory/backupOperations/čtení|Operace zálohování vrátí stav pro obnovení služeb trezoru.|
|/ Trezory/storageConfig/čtení|Vrátí konfiguraci úložiště pro obnovení služeb trezoru.|
|A trezory/storageConfig/zápis|Aktualizace konfiguraci úložiště pro obnovení služeb trezoru.|
|/ Trezory/backupUsageSummaries/čtení|Vrátí souhrny pro chráněné položky a chráněné servery pro služeb zotavení.|
|/ Trezory/backupProtectedItems/čtení|Vrátí seznam hello všechny chráněné položky.|
|/ Trezory/backupconfig/vaultconfig/čtení|Vrátí konfiguraci pro obnovení služeb trezoru.|
|A trezory/backupconfig/vaultconfig/zápis|Konfigurace aktualizací pro obnovení služeb trezoru.|
|A trezory/registeredIdentities/zápis|Hello operaci zaregistrovat kontejneru služby lze použít tooregister kontejner službou obnovení.|
|/ Trezory/registeredIdentities/čtení|Získat kontejnery Hello, lze operace získání hello kontejnery zaregistrovat pro prostředek.|
|Trezory, registeredIdentities nebo odstranění|Hello operaci zrušit registraci kontejneru se dá použít toounregister kontejner.|
|/ Trezory/registeredIdentities/operationResults/čtení|operaci odeslání Hello získat operace lze použít získat stav operace hello a vést u hello asynchronně výsledky operace|
|/vaults/replicationJobs/Read|Číst všechny úlohy|
|/vaults/replicationJobs/Cancel/Action|Zrušení úlohy|
|/vaults/replicationJobs/restart/Action|Restartujte úlohu|
|/vaults/replicationJobs/Resume/Action|Obnovení úlohy|
|/ Trezory/backupPolicies/čtení|Vrátí všechny zásady ochrany|
|A trezory/backupPolicies/zápis|Vytvoří zásadu ochrany|
|Trezory, backupPolicies nebo odstranění|Odstranění zásady ochrany|
|/ Trezory/backupPolicies/operationResults/čtení|Načte výsledky operace zásad.|
|/ Trezory/backupPolicies/operationStatus/čtení|Načíst stav operace zásad.|
|/ Trezory/vaultTokens/čtení|Hello trezoru tokenu operace může být použité tooget trezoru tokenu pro operace trezoru úrovně back-end.|
|/ Trezory/monitoringConfigurations/notificationConfiguration/čtení|Získá konfigurace oznámení trezoru služeb zotavení hello.|
|/ Trezory/backupJobs/čtení|Vrátí všechny objekty úlohy|
|/ Trezory/backupJobs nebo zrušit nebo akce|Zrušit hello úlohy|
|/ Trezory/backupJobs/operationResults/čtení|Vrátí hello výsledek operace úlohy.|
|/Locations/allocateStamp/Action|AllocateStamp je interní operace, které používá služba|
|/Locations/allocatedStamp/Read|GetAllocatedStamp je interní operace, kterou používá služba|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Operace | Popis |
|---|---|
|/ checkNamespaceAvailability nebo akce|Zkontroluje dostupnost oboru názvů v daném předplatném.|
|/ registrace nebo akce|Zaregistruje hello předplatného pro poskytovatele prostředků hello předávání a umožňuje vytvoření hello předávání prostředků|
|/ obory názvů a zápis|Vytvořte prostředek Namespace a aktualizujte jeho vlastnosti. Značky a stav hello Namespace jsou hello vlastností, které lze aktualizovat.|
|/namespaces/Read|Získání seznamu hello Namespace prostředků popisu|
|/ obory názvů/odstranit|Odstranit prostředek Namespace|
|/namespaces/authorizationRules/Write|Autorizační pravidla úrovni Namespace vytvořit a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/namespaces/authorizationRules/DELETE|Odstraňte Namespace autorizační pravidlo. Hello výchozí Namespace autorizační pravidlo nelze odstranit. |
|/namespaces/authorizationRules/listkeys/Action|Získat připojovací řetězec toohello hello Namespace|
|/namespaces/HybridConnections/Write|Vytvoření nebo aktualizace HybridConnection vlastnosti.|
|/namespaces/HybridConnections/Read|Získání seznamu popisů HybridConnection prostředků|
|/namespaces/HybridConnections/DELETE|Operace toodelete HybridConnection prostředků|
|/namespaces/HybridConnections/authorizationRules/Write|Vytvořte HybridConnection autorizační pravidla a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/namespaces/HybridConnections/authorizationRules/DELETE|Operace toodelete HybridConnection autorizační pravidla|
|/namespaces/HybridConnections/authorizationRules/listkeys/Action|Získat připojovací řetězec tooHybridConnection hello|
|/namespaces/WcfRelays/Write|Vytvoření nebo aktualizace WcfRelay vlastnosti.|
|/namespaces/WcfRelays/Read|Získání seznamu popisů WcfRelay prostředků|
|/namespaces/WcfRelays/DELETE|Operace toodelete WcfRelay prostředků|
|/namespaces/WcfRelays/authorizationRules/Write|Vytvořte WcfRelay autorizační pravidla a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/namespaces/WcfRelays/authorizationRules/DELETE|Operace toodelete WcfRelay autorizační pravidla|
|/namespaces/WcfRelays/authorizationRules/listkeys/Action|Získat připojovací řetězec tooWcfRelay hello|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Operace | Popis |
|---|---|
|/ AvailabilityStatuses/čtení|Získá hello dostupnosti stavy pro všechny prostředky v hello zadaný obor|
|/ AvailabilityStatuses/aktuální/čtení|Získá stav dostupnosti hello hello zadaný prostředek|

## <a name="microsoftresources"></a>Microsoft.Resources

| Operace | Popis |
|---|---|
|/ checkResourceName nebo akce|Zkontrolujte správnost názvu prostředku hello.|
|/providers/Read|Získání seznamu hello zprostředkovatelů.|
|/subscriptions/Read|Získá seznam hello předplatných.|
|/subscriptions/operationresults/Read|Pořiďte si předplatné hello výsledky operace.|
|/subscriptions/providers/Read|Načte nebo vypíše zprostředkovatele prostředků.|
|/subscriptions/tagNames/Read|Načte nebo vypíše značky předplatného.|
|/subscriptions/tagNames/Write|Přidá značku předplatného.|
|/subscriptions/tagNames/DELETE|Odstraní značku předplatného.|
|/subscriptions/tagNames/tagValues/Read|Načte nebo vypíše hodnoty značky předplatného.|
|/subscriptions/tagNames/tagValues/Write|Přidá hodnotu značky předplatného.|
|/subscriptions/tagNames/tagValues/DELETE|Odstraní hodnotu značky předplatného.|
|/subscriptions/Resources/Read|Získá prostředky předplatného.|
|/subscriptions/resourceGroups/Read|Načte nebo vypíše skupinu prostředků.|
|/subscriptions/resourceGroups/Write|Vytvoří nebo aktualizuje skupinu prostředků.|
|/subscriptions/resourceGroups/DELETE|Odstraní skupinu prostředků a všechny její prostředky.|
|/subscriptions/resourceGroups/moveResources/Action|Přesune prostředky z tooanother skupiny jeden prostředek.|
|/subscriptions/resourceGroups/validateMoveResources/Action|Ověřte přesun prostředků z tooanother skupiny jeden prostředek.|
|/subscriptions/resourcegroups/Resources/Read|Získá hello prostředky pro skupinu prostředků hello.|
|/subscriptions/resourcegroups/Deployments/Read|Načte nebo vypíše nasazení.|
|/subscriptions/resourcegroups/Deployments/Write|Vytvoří nebo aktualizuje nasazení.|
|/subscriptions/resourcegroups/Deployments/operationstatuses/Read|Načte nebo vypíše stavy operace nasazení.|
|/subscriptions/resourcegroups/Deployments/Operations/Read|Načte nebo vypíše operace nasazení.|
|/subscriptions/Locations/Read|Získá hello seznam podporovaných umístění.|
|/Links/Read|Načte nebo vypíše odkazy na prostředek.|
|/ odkazy a zápis|Vytvoří nebo aktualizuje odkaz na prostředek.|
|/Links/DELETE|Odstraní odkaz na prostředek.|
|/tenants/Read|Získá seznam hello klientům.|
|/Resources/Read|Získejte hello seznam prostředků na základě filtrů.|
|/Deployments/Read|Načte nebo vypíše nasazení.|
|/ nasazení a zápis|Vytvoří nebo aktualizuje nasazení.|
|/Deployments/DELETE|Odstraní nasazení.|
|/Deployments/Cancel/Action|Zruší nasazení.|
|/Deployments/Validate/Action|Ověří nasazení.|
|/Deployments/Operations/Read|Načte nebo vypíše operace nasazení.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Operace | Popis |
|---|---|
|/jobcollections/Read|Získání kolekce úloh|
|/ kolekce a zápis|Vytvoří nebo aktualizuje kolekci úloh.|
|/jobcollections/DELETE|Kolekce úloh odstranění.|
|/jobcollections/enable/Action|Kolekce úloh umožňuje.|
|/jobcollections/disable/Action|Zakáže kolekce úloh.|
|/jobcollections/Jobs/Read|Získá úlohy.|
|/jobcollections/Jobs/Write|Vytvoří nebo aktualizuje úlohu.|
|/jobcollections/Jobs/DELETE|Odstraní úlohu.|
|/jobcollections/Jobs/Run/Action|Úloha se spustí.|
|/jobcollections/Jobs/generateLogicAppDefinition/Action|Vygeneruje definici Aplikace logiky na základě úlohy Scheduleru.|
|/jobcollections/Jobs/jobhistories/Read|Získá historie úlohy.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje hello předplatného pro poskytovatele prostředků hello vyhledávání a umožňuje hello vytvoření služby vyhledávání.|
|/ checkNameAvailability nebo akce|Zkontroluje dostupnost hello název služby.|
|/ searchServices/zápisu|Vytvoří nebo aktualizuje hello službu vyhledávání.|
|/searchServices/Read|Přečte hello službu vyhledávání.|
|/searchServices/DELETE|Odstraní hello službu vyhledávání.|
|/searchServices/Start/Action|Spustí službu vyhledávání hello.|
|/searchServices/stop/Action|Zastaví službu vyhledávání hello.|
|/searchServices/listAdminKeys/Action|Přečte hello klíče správce.|
|/searchServices/regenerateAdminKey/Action|Znovu vygeneruje klíč správce hello.|
|/searchServices/createQueryKey/Action|Vytvoří hello klíč dotazu.|
|/searchServices/queryKey/Read|Přečte hello klíče dotazu.|
|/searchServices/queryKey/DELETE|Odstraní hello klíč dotazu.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Operace | Popis |
|---|---|
|/jitNetworkAccessPolicies/Read|Získá zásad přístupu k síti za běhu hello|
|/ jitNetworkAccessPolicies/zápisu|Vytvoří nové zásady přístupu k síti za běhu nebo aktualizuje existující|
|/jitNetworkAccessPolicies/initiate/Action|Inicializuje zásady přístupu k síti za běhu|
|/securitySolutionsReferenceData/Read|Získá řešení zabezpečení hello referenční data|
|/securityStatuses/Read|Získá stav stavy hello zabezpečení pro prostředky Azure|
|/webApplicationFirewalls/Read|Získá hello webové aplikace brány firewall|
|/ webApplicationFirewalls/zápisu|Vytvoří nové brány firewall webových aplikací nebo aktualizuje existující|
|/webApplicationFirewalls/DELETE|Odstraní brány firewall webových aplikací|
|/securitySolutions/Read|Získá řešení zabezpečení hello|
|/ securitySolutions/zápisu|Vytvoří nové řešení zabezpečení nebo aktualizuje existující|
|/securitySolutions/DELETE|Odstraní zabezpečení řešení|
|/Tasks/Read|Získá všechny dostupné zabezpečení doporučení|
|/Tasks/dismiss/Action|Zavření doporučení zabezpečení|
|/Tasks/Activate/Action|Aktivovat doporučení zabezpečení|
|/Policies/Read|Získá zásady zabezpečení hello|
|/ Zásady/zápisu|Aktualizace hello zásady zabezpečení|
|/applicationWhitelistings/Read|Získá whitelistings aplikace hello|
|/ applicationWhitelistings/zápisu|Vytvoří nové vytvoření seznamu povolených aplikací nebo aktualizuje existující|

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

| Operace | Popis |
|---|---|
|/ subscriptions/zápisu|Vytvoří nebo aktualizuje předplatné|
|/ brány a zápis|Vytvoří nebo aktualizuje brány|
|/gateways/DELETE|Odstraní brány|
|/gateways/Read|Získá brány|
|/gateways/regenerateprofile/Action|Regeneruje profil brány hello|
|/gateways/upgradetolatest/Action|Upgrady hello brány toohello nejnovější verzi|
|/ uzly a zápis|Vytvoří nebo aktualizuje uzlu|
|/Nodes/DELETE|Odstraní uzel|
|/Nodes/Read|Získá uzel|
|/ relací a zápis|Vytvoří nebo aktualizuje relaci|
|/Sessions/Read|Získá relaci|
|/Sessions/DELETE|Odstraní relaci.|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Operace | Popis |
|---|---|
|/ checkNameAvailability nebo akce|Zkontroluje dostupnost oboru názvů v daném předplatném.|
|/ registrace nebo akce|Zaregistruje hello předplatného pro poskytovatele prostředků sběrnice hello a umožňuje vytvoření hello sběrnice prostředků|
|/ obory názvů a zápis|Vytvořte prostředek Namespace a aktualizujte jeho vlastnosti. Značky a stav hello Namespace jsou hello vlastností, které lze aktualizovat.|
|/namespaces/Read|Získání seznamu hello Namespace prostředků popisu|
|/ obory názvů/odstranit|Odstranit prostředek Namespace|
|/namespaces/metricDefinitions/Read|Získání seznamu Namespace metrik popisy prostředků|
|/namespaces/authorizationRules/Write|Autorizační pravidla úrovni Namespace vytvořit a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/namespaces/authorizationRules/Read|Získání seznamu hello popisu obory názvů autorizačních pravidel.|
|/namespaces/authorizationRules/DELETE|Odstraňte Namespace autorizační pravidlo. Hello výchozí Namespace autorizační pravidlo nelze odstranit. |
|/namespaces/authorizationRules/listkeys/Action|Získat připojovací řetězec toohello hello Namespace|
|/namespaces/authorizationRules/regenerateKeys/Action|Znovu vygenerovat hello primární nebo sekundární klíče toohello prostředků|
|/namespaces/diagnosticSettings/Read|Získání seznamu popisů Namespace nastavení pro diagnostiku prostředků|
|/namespaces/diagnosticSettings/Write|Získání seznamu popisů Namespace nastavení pro diagnostiku prostředků|
|/namespaces/Queues/Write|Vytvoření nebo aktualizace fronty vlastnosti.|
|/namespaces/Queues/Read|Získání seznamu popisů fronty prostředků|
|/namespaces/Queues/DELETE|Operace toodelete fronty prostředků|
|/namespaces/Queues/authorizationRules/Write|Vytvoření fronty autorizační pravidla a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/namespaces/Queues/authorizationRules/Read| Získání seznamu hello fronty autorizačních pravidel|
|/namespaces/Queues/authorizationRules/DELETE|Operace toodelete fronty autorizační pravidla|
|/namespaces/Queues/authorizationRules/listkeys/Action|Získat připojovací řetězec tooQueue hello|
|/namespaces/Queues/authorizationRules/regenerateKeys/Action|Znovu vygenerovat hello primární nebo sekundární klíče toohello prostředků|
|/namespaces/logDefinitions/Read|Získání seznamu protokolů Namespace popisy prostředků|
|/namespaces/topics/Write|Vytvoření nebo aktualizace tématu vlastnosti.|
|/namespaces/topics/Read|Získání seznamu popisů tématu prostředků|
|/namespaces/topics/DELETE|Operace toodelete tématu prostředků|
|/namespaces/topics/authorizationRules/Write|Vytvoření tématu autorizační pravidla a aktualizujte jeho vlastnosti. mohou být aktualizovány Hello autorizační pravidla přístupová práva, hello primární a sekundární klíče.|
|/namespaces/topics/authorizationRules/Read| Získání seznamu hello tématu autorizačních pravidel|
|/namespaces/topics/authorizationRules/DELETE|Operace toodelete tématu autorizační pravidla|
|/namespaces/topics/authorizationRules/listkeys/Action|Získat připojovací řetězec tooTopic hello|
|/namespaces/topics/authorizationRules/regenerateKeys/Action|Znovu vygenerovat hello primární nebo sekundární klíče toohello prostředků|
|/namespaces/topics/Subscriptions/Write|Vytvoření nebo aktualizace TopicSubscription vlastnosti.|
|/namespaces/topics/Subscriptions/Read|Získání seznamu popisů TopicSubscription prostředků|
|/namespaces/topics/Subscriptions/DELETE|Operace toodelete TopicSubscription prostředků|
|/namespaces/topics/Subscriptions/Rules/Write|Vytvořit nebo aktualizovat pravidlo vlastnosti.|
|/namespaces/topics/Subscriptions/Rules/Read|Získání seznamu popisů pravidlo prostředků|
|/namespaces/topics/Subscriptions/Rules/DELETE|Operace toodelete pravidlo prostředků|

## <a name="microsoftsql"></a>Microsoft.Sql

| Operace | Popis |
|---|---|
|/Servers/Read|Vrátí seznam serverů ve skupině prostředků na předplatné|
|/ servery a zápis|Vytvořit nový server nebo upravit vlastnosti stávajícího serveru ve skupině prostředků na předplatné|
|/Servers/DELETE|Odstraňte server a všechny obsažené databáze i elastické fondy|
|/Servers/import/Action|Vytvoření nové databáze na serveru hello a nasadit schéma a data z balíčku DacPac|
|/Servers/upgrade/Action|Nové funkce, které jsou k dispozici v nejnovější verzi serveru hello povolíte a zadáte databáze edice převod mapy|
|/Servers/VulnerabilityAssessmentScans/Action|Spuštění kontroly serveru assessment ohrožení zabezpečení|
|/Servers/operationResults/Read|Operace se používá tootrack postup upgradu serveru z nižší verze toohigher|
|/Servers/operationResults/DELETE|Zrušení upgrade verze serveru v průběhu|
|/Servers/securityAlertPolicies/Read|Načtení podrobností hello serveru threat detekce zásady nakonfigurované na daném serveru|
|/Servers/securityAlertPolicies/Write|Změnit server hello služba detekce hrozeb pro daný server|
|/Servers/securityAlertPolicies/operationResults/Read|Načíst výsledky hello serveru zásad detekce hrozeb operace nastavení|
|/Servers/Administrators/Read|Načíst podrobnosti o správce serveru|
|/Servers/Administrators/Write|Vytvořit nebo aktualizovat Správce serveru|
|/Servers/Administrators/DELETE|Odstranit správce serveru z hello server|
|/Servers/recoverableDatabases/Read|Tato operace se používá pro zotavení po havárii online databáze toorestore databáze toolast známé dobré bodu zálohy. Vrací informace o poslední správné zálohy hello, ale nepodporuje obnovení ve skutečnosti hello databáze.|
|/Servers/serviceObjectives/Read|Načtení seznamu cílů na úrovni služby (také označované jako úrovně výkonu) k dispozici na daném serveru|
|/Servers/firewallRules/Read|Načíst podrobnosti o pravidlo brány firewall serveru|
|/Servers/firewallRules/Write|Vytvořit nebo aktualizovat pravidlo brány firewall serveru, který určuje rozsah IP adres povoleny tooconnect toohello serveru|
|/Servers/firewallRules/DELETE|Odstranit pravidlo brány firewall ze serveru hello|
|/Servers/administratorOperationResults/Read|Načíst výsledky operace správce serveru|
|/Servers/recommendedElasticPools/Read|Načíst doporučení pro náklady tooreduce fondy elastické databáze nebo zlepšení výkonu na základě využití prostředků historica|
|/Servers/recommendedElasticPools/Metrics/Read|Načtení metriky pro fondy doporučené elastické databáze pro daný server|
|/Servers/recommendedElasticPools/Databases/Read|Načtení databází, které mají být přidány do doporučené fondy elastických databází pro daný server|
|/Servers/elasticPools/Read|Načíst podrobnosti o fondu elastické databáze na daném serveru|
|/Servers/elasticPools/Write|Vytvořit novou nebo změnit vlastnosti existující fond elastické databáze|
|/Servers/elasticPools/DELETE|Odstraňte existující fond elastické databáze|
|/Servers/elasticPools/operationResults/Read|Načíst podrobnosti o fondu operace dané elastické databáze|
|/Servers/elasticPools/providers/Microsoft.Insights/<br>metricDefinitions/čtení|Návratové typy metriky, které jsou k dispozici pro fondy elastické databáze|
|/Servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings/čtení|Získá hello nastavení diagnostiky pro prostředek hello|
|/Servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings a zápis|Vytvoří nebo aktualizuje hello nastavení diagnostiky pro prostředek hello|
|/Servers/elasticPools/Metrics/Read|Vrátí metriky využití prostředků fondu elastické databáze|
|/Servers/elasticPools/elasticPoolDatabaseActivity/Read|Načtení činnosti a údaje na danou databázi, která je součástí fondu elastické databáze|
|/Servers/elasticPools/advisors/Read|Vrátí seznam poradci, které jsou k dispozici pro elastický fond hello|
|/Servers/elasticPools/advisors/Write|Aktualizace automatického – spuštění stav advisor na úrovni elastického fondu.|
|/Servers/elasticPools/advisors/recommendedActions/Read|Vrátí seznam doporučené akce zadané pro elastický fond hello Poradce|
|/Servers/elasticPools/advisors/recommendedActions/Write|Použít hello Doporučená akce na hello elastického fondu|
|/Servers/elasticPools/elasticPoolActivity/Read|Načtení činnosti a údaje na danou databázi elastického fondu|
|/Servers/elasticPools/Databases/Read|Načtení seznamu a podrobnosti databáze, které jsou součástí fondu elastické databáze na daném serveru|
|/Servers/auditingPolicies/Read|Načíst podrobnosti o hello výchozí server tabulky auditování zásady nakonfigurované na daném serveru|
|/Servers/auditingPolicies/Write|Změnit hello výchozí server tabulka auditování pro daný server|
|/Servers/disasterRecoveryConfiguration/operationResults/Read|Získat výsledky operace konfigurace obnovení po havárii|
|/Servers/advisors/Read|Vrátí seznam poradci, které jsou k dispozici pro hello server|
|/Servers/advisors/Write|Aktualizace automatického – spuštění stav advisor na úrovni serveru.|
|/Servers/advisors/recommendedActions/Read|Vrátí seznam doporučených akcí zadaný advisor pro hello server|
|/Servers/advisors/recommendedActions/Write|Použít hello Doporučená akce na hello server|
|/Servers/usages/Read|Vrátí kvóty DTU serveru a aktuální consuption DTU všemi databázemi v rámci hello server|
|/Servers/elasticPoolEstimates/Read|Vrátí seznam odhad elastického fondu vytvořeny již pro tento server|
|/Servers/elasticPoolEstimates/Write|Vytvoří nový odhad elastického fondu pro seznam databází poskytuje|
|/Servers/auditingSettings/Read|Načíst podrobnosti o objektu blob server hello auditování zásady nakonfigurované na daném serveru|
|/Servers/auditingSettings/Write|Změnit hello auditování objektů blob serveru pro daný server|
|/Servers/auditingSettings/operationResults/Read|Načíst výsledek objektu blob server hello operace nastavení zásad auditování|
|/Servers/backupLongTermRetentionVaults/Read|Tato operace je použité tooget trezoru zálohování dlouhodobé uchovávání. Vrátí informace o serveru registrované toothis hello trezoru.|
|/Servers/backupLongTermRetentionVaults/Write|Zaregistrovat k trezoru zálohování dlouhodobé uchovávání|
|/Servers/restorableDroppedDatabases/Read|Načíst seznam databází, které byly vyřazeny na daném serveru, které jsou stále v rámci zásady uchovávání informací. Tato operace vrátí seznam databází a související metadata, jako je datum odstranění.|
|/Servers/Databases/Read|Vrátí seznam serverů ve skupině prostředků na předplatné|
|/Servers/Databases/Write|Vytvořit nový server nebo upravit vlastnosti stávajícího serveru ve skupině prostředků na předplatné|
|/Servers/Databases/DELETE|Odstraňte server a všechny obsažené databáze i elastické fondy|
|/Servers/Databases/export/Action|Vytvoření nové databáze na serveru hello a nasadit schéma a data z balíčku DacPac|
|/Servers/Databases/VulnerabilityAssessmentScans/Action|Spusťte kontrolu databáze assessment ohrožení zabezpečení.|
|/Servers/Databases/pause/Action|Pozastavení edici databáze datového skladu|
|/Servers/Databases/Resume/Action|Obnovit databázi datového skladu edition|
|/Servers/Databases/operationResults/Read|Operace se používá tootrack průběh operace probíhající dlouhou dobu databáze, jako je například škálování.|
|/Servers/Databases/replicationLinks/Read|Návratový podrobnosti o propojení replikace navázána pro konkrétní databázi|
|/Servers/Databases/replicationLinks/DELETE|Ukončení relace replikace hello vynuceně a potenciální ztráty dat.|
|/Servers/Databases/replicationLinks/Unlink/Action|Ukončení relace replikace hello vynuceně nebo po synchronizaci s partnerem hello|
|/Servers/Databases/replicationLinks/Failover/Action|Převzetí služeb při selhání po synchronizaci všech změn z hello primární, provedení této databáze do vztahu replikace hello hello primární a provedení vzdálené primární do sekundárního|
|/Servers/Databases/replicationLinks/forceFailoverAllowDataLoss/Action|Převzetí služeb při selhání okamžitě potenciální ztrátě dat, provedení této databáze do vztahu replikace hello hello primární a provedení vzdálené primární do sekundárního|
|/Servers/Databases/replicationLinks/updateReplicationMode/Action|Aktualizovat režim replikace pro odkaz toosynchronous nebo asynchronním režimu|
|/Servers/Databases/replicationLinks/operationResults/Read|Načíst stav operace dlouho běžící na odkazy replikace databáze|
|/Servers/Databases/dataMaskingPolicies/Read|Načtení podrobností maskování zásady nakonfigurované na danou databázi hello dat|
|/Servers/Databases/dataMaskingPolicies/Write|Změnit zásady pro danou databázi maskování dat.|
|/Servers/Databases/dataMaskingPolicies/Rules/Read|Načtení podrobností maskování pravidlo zásad, které jsou nakonfigurované na danou databázi hello dat|
|/Servers/Databases/dataMaskingPolicies/Rules/Write|Změňte pravidla zásad pro danou databázi maskování dat.|
|/Servers/Databases/securityAlertPolicies/Read|Načtení podrobností zásady detekce hrozeb hello nakonfigurované na danou databázi|
|/Servers/Databases/securityAlertPolicies/Write|Změnit hello zásady detekce hrozeb pro danou databázi|
|/Servers/Databases/providers/Microsoft.Insights/<br>metricDefinitions/čtení|Návratové typy metriky, které jsou k dispozici pro databáze|
|/Servers/Databases/providers/Microsoft.Insights/<br>diagnosticSettings/čtení|Získá hello nastavení diagnostiky pro prostředek hello|
|/Servers/Databases/providers/Microsoft.Insights/<br>diagnosticSettings a zápis|Vytvoří nebo aktualizuje hello nastavení diagnostiky pro prostředek hello|
|/Servers/Databases/providers/Microsoft.Insights/<br>logDefinitions/čtení|Získá dostupné protokoly hello pro databáze|
|/Servers/Databases/topQueries/Read|Vrátí statistiku modulu runtime pro vybraný dotaz agregovat do vybrané časové období|
|/Servers/Databases/topQueries/queryText/Read|Vrátí text hello Transact-SQL pro ID vybraný dotaz|
|/Servers/Databases/topQueries/Statistics/Read|Vrátí statistiku modulu runtime pro vybraný dotaz agregovat do vybrané časové období|
|/Servers/Databases/connectionPolicies/Read|Načtení podrobností hello připojení zásady nakonfigurované na danou databázi|
|/Servers/Databases/connectionPolicies/Write|Změnit zásady připojení pro danou databázi|
|/Servers/Databases/Metrics/Read|Vrátí metriky využití prostředků databáze|
|/Servers/Databases/auditRecords/Read|Načtení záznamů auditu blob hello databáze|
|/Servers/Databases/transparentDataEncryption/Read|Načíst stav a podrobnosti o funkce zabezpečení dat transparentní šifrování pro danou databázi|
|/Servers/Databases/transparentDataEncryption/Write|Povolit nebo zakázat transparentní šifrování dat pro danou databázi|
|/Servers/Databases/transparentDataEncryption/operationResults/Read|Načíst stav a podrobnosti o funkce zabezpečení dat transparentní šifrování pro danou databázi|
|/Servers/Databases/auditingPolicies/Read|Načtení podrobností hello tabulky auditování zásady nakonfigurované na danou databázi|
|/Servers/Databases/auditingPolicies/Write|Změna zásad auditování hello tabulky pro danou databázi|
|/Servers/Databases/dataWarehouseQueries/Read|Vrátí informace o hello datovém skladu distribuční dotazu pro vybraný dotaz ID|
|/ servery/databáze/dataWarehouseQueries /<br>dataWarehouseQuerySteps/čtení|Vrátí hello distribuovaných dotazů krok informace o dotazu datový sklad pro vybraný krok ID|
|/Servers/Databases/serviceTierAdvisors/Read|Vrátí návrhu o škálování databáze nahoru nebo dolů na základě výkonu tooimprove statistik provádění dotazů nebo snížení nákladů|
|/Servers/Databases/advisors/Read|Vrátí seznam poradci, které jsou k dispozici pro databázi hello|
|/Servers/Databases/advisors/Write|Aktualizace automatického – spuštění stav advisor na úrovni databáze.|
|/Servers/Databases/advisors/recommendedActions/Read|Vrátí seznam doporučených akcí zadaný advisor pro databázi hello|
|/Servers/Databases/advisors/recommendedActions/Write|Použít hello Doporučená akce v databázi hello|
|/Servers/Databases/usages/Read|Vrátí maximální velikost databáze, které lze přejít a aktuální velikost obsazena dat|
|/Servers/Databases/queryStore/Read|Vrátí aktuální hodnoty nastavení úložiště dotazů pro databázi hello|
|/Servers/Databases/queryStore/Write|Aktualizace nastavení úložiště dotazů pro databázi hello|
|/Servers/Databases/auditingSettings/Read|Načtení podrobností hello blob auditování zásady nakonfigurované na danou databázi|
|/Servers/Databases/auditingSettings/Write|Změna zásad auditování hello objektů blob pro danou databázi|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Read|Načíst seznam doporučení indexu v databázi|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Write|Použít doporučení indexu|
|/Servers/Databases/schemas/Tables/Columns/Read|Načíst seznam sloupců tabulky.|
|/Servers/Databases/missingindexes/Read|Vrátí návrhy o toocreate indexů databáze, upravit nebo odstranit v výkon dotazů tooimprove pořadí|
|/Servers/Databases/missingindexes/Write|Použít databázi doporučení indexu v konkrétní databázi|
|/Servers/Databases/importExportOperationResults/Read|Vrátí podrobnosti o import databáze nebo operace exportu z DacPac umístěný v účtu úložiště|
|/Servers/importExportOperationResults/Read|Vrátí seznam hello s podrobnostmi o databázové operace importu z účtu úložiště na daném serveru|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje hello předplatného pro poskytovatele prostředků úložiště hello a umožňuje hello vytvoření účtů úložiště.|
|/checknameavailability/Read|Zkontroluje, že název účtu je platný a nepoužívá se.|
|/ storageAccounts/zápisu|Vytvoří účet úložiště s hello zadané parametry nebo aktualizace hello vlastnosti a značky nebo přidá vlastní doménu pro hello zadaný účet úložiště.|
|/storageAccounts/DELETE|Odstraní existující účet úložiště.|
|/storageAccounts/listkeys/Action|Vrátí hello přístupové klíče pro hello zadaný účet úložiště.|
|/storageAccounts/regeneratekey/Action|Regeneruje hello přístupové klíče pro hello zadaný účet úložiště.|
|/storageAccounts/Read|Vrátí hello seznam účtů úložiště nebo získá hello vlastnosti hello zadaný účet úložiště.|
|/storageAccounts/listAccountSas/Action|Vrátí token SAS účtu hello hello zadaný účet úložiště.|
|/storageAccounts/listServiceSas/Action|Token SAS služby úložiště|
|/storageAccounts/Services/diagnosticSettings/Write|Vytvořit nebo aktualizovat nastavení diagnostiky účet úložiště.|
|/skus/Read|Uvádí hello SKU nepodporuje Microsoft.Storage.|
|/usages/Read|Vrátí hello limit a hello aktuální počet použití pro prostředky v hello zadané předplatné|
|/Operations/Read|Hlasování hello stav asynchronní operace.|
|/Locations/deleteVirtualNetworkOrSubnets/Action|Microsoft.Storage upozorní, že virtuální síť nebo podsíť, Probíhá odstranění|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Operace | Popis |
|---|---|
|/Managers/clearAlerts/Action|Vymažte všechny výstrahy hello přidružená ke Správci zařízení hello.|
|/Managers/getActivationKey/Action|Získáte aktivační klíč pro správce zařízení hello.|
|/Managers/regenerateActivationKey/Action|Znovu vygenerujte aktivační klíč pro správce zařízení hello.|
|/Managers/regenarateRegistationCertificate/Action|Znovu vygenerujte registrační certifikát pro hello Správci zařízení.|
|/Managers/getEncryptionKey/Action|Získání šifrovacího klíče pro správce zařízení hello.|
|/Managers/Read|Uvádí nebo získá hello Správce zařízení.|
|/Managers/DELETE|Odstraní hello Správce zařízení.|
|/ Správci a zápis|Vytvořit nebo aktualizovat hello Správce zařízení.|
|/Managers/configureDevice/Action|Konfiguruje zařízení|
|/Managers/listActivationKey/Action|Získá klíč aktivace hello hello Správce zařízení StorSimple.|
|/Managers/listPublicEncryptionKey/Action|Seznam veřejný šifrovací klíče nástroje Správce zařízení StorSimple.|
|/Managers/listPrivateEncryptionKey/Action|Získá privátní šifrovací klíč pro správce zařízení StorSimple.|
|/Managers/provisionCloudAppliance/Action|Vytvořte nové zařízení cloudu.|
|A správci/zápis|Operace Vytvořit trezor vytvoří prostředek Azure typu trezor.|
|/ Správci/čtení|Hello operace získání trezoru získá objekt, který reprezentuje hello prostředků Azure typu 'trezoru.|
|Správci nebo odstranění|Hello odstranit trezor operaci odstranění hello zadaný Azure prostředek typu 'trezoru.|
|/Managers/storageAccountCredentials/Write|Vytvořit nebo aktualizovat přihlašovací údaje účtu úložiště hello|
|/Managers/storageAccountCredentials/Read|Uvádí nebo získá přihlašovací údaje účtu úložiště hello|
|/Managers/storageAccountCredentials/DELETE|Odstraní hello přihlašovacích údajů účtu úložiště|
|/Managers/storageAccountCredentials/listAccessKey/Action|Seznam přístupových klíčů přihlašovacích údajů účtu úložiště|
|/Managers/accessControlRecords/Read|Uvádí nebo získá hello záznamy řízení přístupu|
|/Managers/accessControlRecords/Write|Vytvořit nebo aktualizovat hello záznamy řízení přístupu|
|/Managers/accessControlRecords/DELETE|Odstraní záznamy řízení přístupu hello|
|/Managers/Metrics/Read|Uvádí nebo získá hello metriky|
|/Managers/bandwidthSettings/Read|Seznam nastavení šířky pásma hello (pouze řady 8000)|
|/Managers/bandwidthSettings/Write|Vytvoří novou nebo aktualizuje nastavení šířky pásma (pouze řady 8000)|
|/Managers/bandwidthSettings/DELETE|Odstraní existující nastavení šířky pásma (pouze řady 8000)|
|/ Správci/extendedInformation/čtení|Získá Hello získat rozšířené informace o operaci představující hello prostředků Azure typu informace o rozšířených objektu? trezoru?|
|A správci/extendedInformation/zápis|Získá Hello získat rozšířené informace o operaci představující hello prostředků Azure typu informace o rozšířených objektu? trezoru?|
|Správci, extendedInformation nebo odstranění|Získá Hello získat rozšířené informace o operaci představující hello prostředků Azure typu informace o rozšířených objektu? trezoru?|
|/Managers/Alerts/Read|Uvádí nebo získá hello výstrahy|
|/Managers/storageDomains/Read|Uvádí nebo získá hello úložiště domén|
|/Managers/storageDomains/Write|Vytvořit nebo aktualizovat hello úložiště domén|
|/Managers/storageDomains/DELETE|Odstraní hello úložiště domén|
|/Managers/Devices/scanForUpdates/Action|Vyhledávání aktualizací v zařízení.|
|/Managers/Devices/download/Action|Stažení aktualizací pro zařízení.|
|/Managers/Devices/Install/Action|Instalaci aktualizací na zařízení.|
|/Managers/Devices/Read|Uvádí nebo získá hello zařízení|
|/Managers/Devices/Write|Vytvořit nebo aktualizovat hello zařízení|
|/Managers/Devices/DELETE|Vymaže zařízení hello|
|/Managers/Devices/Deactivate/Action|Deaktivuje zařízení.|
|/Managers/Devices/publishSupportPackage/Action|Publikujte balíček pro podporu zařízení pro řešení potíží s Microsoft Support.|
|/Managers/Devices/Failover/Action|Převzetí služeb při selhání hello zařízení.|
|/Managers/Devices/sendTestAlertEmail/Action|Odesílání výstrah e-mailů testovací tooconfigured příjemců e-mailu.|
|/Managers/Devices/installUpdates/Action|Nainstaluje aktualizace hello zařízení|
|/Managers/Devices/listFailoverSets/Action|Seznam hello převzetí služeb při selhání se nastaví pro ze stávajících zařízení.|
|/Managers/Devices/listFailoverTargets/Action|Seznam cílů převzetí služeb při selhání hello zařízení|
|/Managers/Devices/publicEncryptionKey/Action|Seznam veřejný šifrovací klíč Správce zařízení hello|
|/ Správci/zařízení nebo hardwareComponentGroups /<br>Pro čtení|Seznam hello hardwaru skupiny součástí|
|/ Správci/zařízení nebo hardwareComponentGroups /<br>changeControllerPowerState nebo akce|Změnit stav napájení řadiče hardwaru skupiny součástí|
|/Managers/Devices/Metrics/Read|Uvádí nebo získá hello metriky|
|/Managers/Devices/chapSettings/Write|Vytvořit nebo aktualizovat nastavení Chap hello|
|/Managers/Devices/chapSettings/Read|Uvádí nebo získá hello Chap nastavení|
|/Managers/Devices/chapSettings/DELETE|Odstraní nastavení Chap hello|
|/Managers/Devices/backupScheduleGroups/Read|Uvádí nebo získá hello skupiny plánu zálohování|
|/Managers/Devices/backupScheduleGroups/Write|Vytvořit nebo aktualizovat skupiny plánu zálohování hello|
|/Managers/Devices/backupScheduleGroups/DELETE|Odstraní hello skupiny plánu zálohování|
|/Managers/Devices/updateSummary/Read|Uvádí nebo získá hello souhrn aktualizace|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>Import nebo akce|Import konfigurace zdroje pro migraci.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>startMigrationEstimate nebo akce|Spusťte úlohu tooestimate hello dobu trvání procesu migrace hello.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>startMigration nebo akce|Spustit migraci pomocí zdroj konfigurace|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>confirmMigration nebo akce|Potvrzuje úspěšné migrace a potvrďte ho.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>fetchMigrationEstimate nebo akce|Načtěte stav hello pro úlohu odhad migrace hello.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>fetchMigrationStatus nebo akce|Načtěte stav hello hello migrace.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>fetchConfirmMigrationStatus nebo akce|Načtení hello potvrdit stav migrace.|
|/Managers/Devices/alertSettings/Read|Uvádí nebo získá hello nastavení výstrah|
|/Managers/Devices/alertSettings/Write|Vytvořit nebo aktualizovat nastavení výstrah hello|
|/Managers/Devices/networkSettings/Read|Uvádí nebo získá hello nastavení sítě|
|/Managers/Devices/networkSettings/Write|Vytvoří novou nebo aktualizovat nastavení sítě|
|/Managers/Devices/Jobs/Read|Uvádí nebo získá hello úlohy|
|/Managers/Devices/Jobs/Cancel/Action|Zrušení spuštěné úlohy|
|/Managers/Devices/metricsDefinitions/Read|Uvádí nebo získá definice metrik hello|
|/Managers/Devices/volumeContainers/Write|Vytvoří novou nebo aktualizuje kontejnery svazků (pouze řady 8000)|
|/Managers/Devices/volumeContainers/Read|Seznam kontejnery svazků hello (pouze řady 8000)|
|/Managers/Devices/volumeContainers/DELETE|Odstraní existující kontejnery svazků (pouze řady 8000)|
|/Managers/Devices/volumeContainers/listEncryptionKeys/Action|Seznam šifrování klíče kontejnery svazků|
|/Managers/Devices/volumeContainers/rolloverEncryptionKey/Action|Šifrování klíče výměny kontejnery svazků|
|/Managers/Devices/volumeContainers/Metrics/Read|Seznam hello metriky|
|/Managers/Devices/volumeContainers/Volumes/Read|Hello seznamu svazků|
|/Managers/Devices/volumeContainers/Volumes/Write|Vytvoří novou nebo aktualizuje svazky|
|/Managers/Devices/volumeContainers/Volumes/DELETE|Odstraní existující svazky|
|/Managers/Devices/volumeContainers/Volumes/Metrics/Read|Seznam hello metriky|
|/Managers/Devices/volumeContainers/Volumes/metricsDefinitions/Read|Seznam hello Definice metrik|
|/Managers/Devices/volumeContainers/metricsDefinitions/Read|Seznam hello Definice metrik|
|/Managers/Devices/iscsiservers/Read|Uvádí nebo získá hello iSCSI servery|
|/Managers/Devices/iscsiservers/Write|Vytvořit nebo aktualizovat hello iSCSI servery|
|/Managers/Devices/iscsiservers/DELETE|Odstraní hello iSCSI servery|
|/Managers/Devices/iscsiservers/Backup/Action|Proveďte zálohování serveru iSCSI.|
|/Managers/Devices/iscsiservers/Metrics/Read|Uvádí nebo získá hello metriky|
|/Managers/Devices/iscsiservers/Disks/Read|Uvádí nebo získá hello disky|
|/Managers/Devices/iscsiservers/Disks/Write|Vytvořit nebo aktualizovat hello disky|
|/Managers/Devices/iscsiservers/Disks/DELETE|Odstraní hello disky|
|/Managers/Devices/iscsiservers/Disks/Metrics/Read|Uvádí nebo získá hello metriky|
|/Managers/Devices/iscsiservers/Disks/metricsDefinitions/Read|Uvádí nebo získá definice metrik hello|
|/Managers/Devices/iscsiservers/metricsDefinitions/Read|Uvádí nebo získá definice metrik hello|
|/Managers/Devices/backups/Read|Uvádí nebo získá hello zálohovacího skladu|
|/Managers/Devices/backups/DELETE|Odstranění hello zálohovacího skladu|
|/Managers/Devices/backups/Restore/Action|Obnovte všechny svazky hello ze zálohovacího skladu.|
|/Managers/Devices/backups/Elements/clone/Action|Klonování sdílené složky nebo svazku pomocí zálohování elementu.|
|/Managers/Devices/backupPolicies/Write|Vytvoří novou nebo aktualizovat zásady zálohování (pouze řady 8000)|
|/Managers/Devices/backupPolicies/Read|Seznam hello zásady zálohování (pouze řady 8000)|
|/Managers/Devices/backupPolicies/DELETE|Odstraní existující zásady zálohování (pouze řady 8000)|
|/Managers/Devices/backupPolicies/Backup/Action|Proveďte zálohování svazků hello je chrání zásady hello toocreate ruční zálohování na vyžádání.|
|/Managers/Devices/backupPolicies/Schedules/Write|Vytvoří novou nebo aktualizovat plány|
|/Managers/Devices/backupPolicies/Schedules/Read|Seznam hello plány|
|/Managers/Devices/backupPolicies/Schedules/DELETE|Odstraní existující plán|
|/Managers/Devices/securitySettings/Update/Action|Aktualizujte nastavení zabezpečení hello.|
|/Managers/Devices/securitySettings/Read|Seznam hello nastavení zabezpečení|
|/ Správci/zařízení nebo securitySettings /<br>syncRemoteManagementCertificate nebo akce|Synchronizujte hello certifikát vzdálené správy pro zařízení.|
|/Managers/Devices/securitySettings/Write|Vytvoří novou nebo aktualizuje nastavení zabezpečení|
|/Managers/Devices/fileservers/Read|Uvádí nebo získá hello souborové servery|
|/Managers/Devices/fileservers/Write|Vytvořit nebo aktualizovat hello souborové servery|
|/Managers/Devices/fileservers/DELETE|Odstraní hello souborové servery|
|/Managers/Devices/fileservers/Backup/Action|Proveďte zálohování souborového serveru.|
|/Managers/Devices/fileservers/Metrics/Read|Uvádí nebo získá hello metriky|
|/Managers/Devices/fileservers/shares/Write|Vytvořit nebo aktualizovat hello sdílené složky|
|/Managers/Devices/fileservers/shares/Read|Uvádí nebo získá hello sdílené složky|
|/Managers/Devices/fileservers/shares/DELETE|Odstraní hello sdílené složky|
|/Managers/Devices/fileservers/shares/Metrics/Read|Uvádí nebo získá hello metriky|
|/Managers/Devices/fileservers/shares/metricsDefinitions/Read|Uvádí nebo získá definice metrik hello|
|/Managers/Devices/fileservers/metricsDefinitions/Read|Uvádí nebo získá definice metrik hello|
|/Managers/Devices/timeSettings/Read|Uvádí nebo získá nastavení času hello|
|/Managers/Devices/timeSettings/Write|Vytvoří novou nebo aktualizuje nastavení času|
|A správci/certifikátů/zápis|Hello operace aktualizace prostředek certifikátu aktualizuje certifikát přihlašovacích údajů hello prostředků nebo trezoru.|
|/Managers/cloudApplianceConfigurations/Read|Seznam hello cloudu zařízení podporované konfigurace|
|/Managers/metricsDefinitions/Read|Uvádí nebo získá definice metrik hello|
|/Managers/encryptionSettings/Read|Uvádí nebo získá hello nastavení šifrování|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Operace | Popis |
|---|---|
|/streamingjobs/Start/Action|Spuštění úlohy Stream Analytics|
|/streamingjobs/stop/Action|Zastavení úlohy Stream Analytics|
|/ streamingjobs/čtení|Úlohy Stream Analytics pro čtení|
|/ streamingjobs/zápisu|Zápis úlohy Stream Analytics|
|/ streamingjobs/odstranit|Odstranit úlohy Stream Analytics|
|/streamingjobs/providers/Microsoft.Insights/metricDefinitions/Read|Získá hello dostupné metriky pro streamingjobs|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/Read|Nastavení diagnostiky pro čtení.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/Write|Zápis nastavení diagnostiky.|
|/streamingjobs/providers/Microsoft.Insights/logDefinitions/Read|Získá dostupné protokoly hello pro streamingjobs|
|/streamingjobs/Transformations/Read|Číst Stream Analytics úlohy transformace|
|/streamingjobs/Transformations/Write|Zápis transformace úlohy Stream Analytics|
|/streamingjobs/Transformations/DELETE|Odstranit transformaci úlohy Stream Analytics|
|/streamingjobs/inputs/Read|Číst Stream Analytics úlohy vstup|
|/streamingjobs/inputs/Write|Zápis vstupu úlohy Stream Analytics|
|/streamingjobs/inputs/DELETE|Odstranit vstupu úlohy Stream Analytics|
|/streamingjobs/Outputs/Read|Výstup úlohy analýzy datového proudu pro čtení|
|/streamingjobs/Outputs/Write|Zapisovat výstup úlohy Stream Analytics|
|/streamingjobs/Outputs/DELETE|Odstranit výstup úlohy Stream Analytics|

## <a name="microsoftsupport"></a>Microsoft.Support

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje tooSupport poskytovatele prostředků|
|/supportTickets/Read|Získat podrobnosti lístku podpory (včetně stavu, závažnosti, podrobnosti kontaktu a komunikací) nebo získá hello seznam lístky žádostí o podporu ve předplatných.|
|/ supportTickets/zápisu|Vytvoří nebo aktualizuje lístek podpory. Můžete vytvořit lístek podpory pro Technical, fakturace, kvóty nebo správy předplatného související problémy. Můžete aktualizovat závažnost, podrobnosti kontaktu a komunikace pro existující lístky žádostí o podporu.|

## <a name="microsoftweb"></a>Microsoft.Web

| Operace | Popis |
|---|---|
|nebo zrušit registraci nebo akce|Zrušte registraci poskytovatele prostředků Microsoft.Web pro předplatné hello.|
|/ ověření nebo akce|Ověření.|
|/ registrace nebo akce|Registrace poskytovatele prostředků Microsoft.Web pro předplatné hello.|
|/ hostingEnvironments/čtení|Získat vlastnosti hello služby App Service Environment|
|/ hostingEnvironments/zápisu|Vytvoření nové služby App Service Environment nebo aktualizovat existující|
|/ hostingEnvironments/odstranit|Odstranění služby App Service Environment|
|/hostingEnvironments/reboot/Action|Restartujte všechny počítače ve službě App Service Environment|
|/hostingenvironments/Resume/Action|Obnovte hostitelská prostředí.|
|/hostingenvironments/suspend/Action|Pozastavte hostitelská prostředí.|
|/hostingenvironments/metricdefinitions/Read|Získat hostování prostředí definice metrik.|
|/hostingEnvironments/workerPools/Read|Získat vlastnosti hello fondu pracovních procesů ve službě App Service Environment|
|/hostingEnvironments/workerPools/Write|Vytvořit nový fond pracovních procesů ve službě App Service Environment nebo aktualizovat stávající|
|/hostingenvironments/workerpools/metricdefinitions/Read|Získat hostování prostředí Workerpools metrika definice.|
|/hostingenvironments/workerpools/Metrics/Read|Získat hostování prostředí Workerpools metriky.|
|/hostingenvironments/workerpools/skus/Read|Získat hostování prostředí Workerpools SKU.|
|/hostingenvironments/workerpools/usages/Read|Získat hostování prostředí Workerpools použití.|
|/hostingenvironments/Sites/Read|Získáte hostování prostředí webové aplikace.|
|/hostingenvironments/serverfarms/Read|Získáte hostování prostředí plány služby App Service.|
|/hostingenvironments/usages/Read|Získat hostování použití prostředí.|
|/hostingenvironments/capacities/Read|Získat hostování prostředí kapacity.|
|/hostingenvironments/Operations/Read|Získat hostování prostředí operace.|
|/hostingEnvironments/multiRolePools/Read|Získat vlastnosti hello front-endu fondu ve službě App Service Environment|
|/hostingEnvironments/multiRolePools/Write|Vytvořit nový fond front-endové služby App Service Environment, nebo aktualizaci existující|
|/hostingenvironments/multirolepools/metricdefinitions/Read|Získat hostování definice metrik u fondy prostředí.|
|/hostingenvironments/multirolepools/Metrics/Read|Získat hostování prostředí u fondy metriky.|
|/hostingenvironments/multirolepools/skus/Read|Získat hostování prostředí u fondy SKU.|
|/hostingenvironments/multirolepools/usages/Read|Získat hostování prostředí u fondy použití.|
|/hostingenvironments/Diagnostics/Read|Získat hostování prostředí diagnostiky.|
|/publishingusers/Read|Uživatelé získat publikování.|
|/ publishingusers/zápisu|Aktualizace publikování uživatelů.|
|/checknameavailability/Read|Zkontrolujte, zda název prostředku je k dispozici.|
|/ geoRegions/čtení|Získejte hello seznam geografických oblastech.|
|/ weby/čtení|Získat vlastnosti hello webové aplikace|
|/ weby/zápisu|Vytvořit novou webovou aplikaci nebo aktualizovat stávající|
|/ weby/odstranit|Odstranit stávající webovou aplikaci|
|/Sites/Backup/Action|Vytvoří novou zálohu webové aplikace|
|/Sites/publishxml/Action|Získat publikování profil xml pro webovou aplikaci|
|/Sites/Publish/Action|Publikování webové aplikace|
|/Sites/restart/Action|Restartování webové aplikace|
|/Sites/Start/Action|Spustí webovou aplikaci|
|/Sites/stop/Action|Zastavení webové aplikace|
|/Sites/slotsswap/Action|Prohodit sloty nasazení webové aplikace|
|/Sites/slotsdiffs/Action|Získat rozdíly v konfiguraci mezi sloty a webové aplikace|
|/Sites/applySlotConfig/Action|Aplikovat konfiguraci slot webové aplikace z cílového slotu toohello aktuální webové aplikace|
|/Sites/resetSlotConfig/Action|Resetovat konfiguraci webové aplikace|
|/Sites/Functions/Action|Funkce Web Apps.|
|/Sites/listsyncfunctiontriggerstatus/Action|Seznam synchronizace funkce aktivační událost stavu webových aplikací.|
|/Sites/networktrace/Action|Webové aplikace, síťové trasování.|
|/Sites/newpassword/Action|Nové_heslo webové aplikace.|
|/Sites/Sync/Action|Synchronizace webové aplikace.|
|/Sites/operationresults/Read|Získáte výsledky operace webové aplikace.|
|/Sites/webjobs/Read|Načtení úloh Webjob webové aplikace.|
|/ weby/zálohování/čtení|Získáte zálohování webové aplikace.|
|/Sites/Backup/Write|Aktualizace webové aplikace zálohování.|
|/Sites/metricdefinitions/Read|Získáte webové aplikace metrika definice.|
|/Sites/Metrics/Read|Získáte metriky webové aplikace.|
|/Sites/continuouswebjobs/DELETE|Odstranění webové aplikace nepřetržité webové úlohy.|
|/Sites/continuouswebjobs/Read|Získáte webové aplikace nepřetržité webové úlohy.|
|/Sites/continuouswebjobs/Start/Action|Spuštění webové aplikace nepřetržité webové úlohy.|
|/Sites/continuouswebjobs/stop/Action|Zastavení webové aplikace nepřetržité webové úlohy.|
|/Sites/domainownershipidentifiers/Read|Získání identifikátorů vlastnictví webové aplikace domény.|
|/Sites/domainownershipidentifiers/Write|Aktualizace webové aplikace domény vlastnictví identifikátory.|
|/Sites/premieraddons/DELETE|Odstraňte rozšíření Premier webové aplikace.|
|/Sites/premieraddons/Read|Získáte rozšíření Premier webové aplikace.|
|/Sites/premieraddons/Write|Aktualizace doplňků Premier webové aplikace.|
|/Sites/triggeredwebjobs/DELETE|Odstraňte spouštěná WebJobs webové aplikace.|
|/Sites/triggeredwebjobs/Read|Získáte spouštěná WebJobs webové aplikace.|
|/Sites/triggeredwebjobs/Run/Action|Spuštění webové aplikace aktivované webové úlohy.|
|/Sites/hostnamebindings/DELETE|Odstranění webové aplikace Hostname vazby.|
|/Sites/hostnamebindings/Read|Získáte webové aplikace Hostname vazby.|
|/Sites/hostnamebindings/Write|Aktualizujte vazby názvů hostitelů aplikací Web.|
|/Sites/virtualnetworkconnections/DELETE|Odstraňte připojení virtuální sítě webové aplikace.|
|/Sites/virtualnetworkconnections/Read|Získání připojení virtuální sítě webové aplikace.|
|/Sites/virtualnetworkconnections/Write|Aktualizujte připojení virtuální sítě webové aplikace.|
|/Sites/virtualnetworkconnections/gateways/Read|Získáte brány připojení virtuální sítě webové aplikace.|
|/Sites/virtualnetworkconnections/gateways/Write|Aktualizace brány připojení virtuální sítě webové aplikace.|
|/Sites/publishxml/Read|Získat publikování webové aplikace XML.|
|/Sites/hybridconnectionrelays/Read|Získáte webové aplikace hybridní připojení předávání.|
|/Sites/perfcounters/Read|Získáte čítače výkonu webové aplikace.|
|/Sites/usages/Read|Získáte použití webové aplikace.|
|/Sites/slots/Write|Vytvořit nový Slot webové aplikace nebo aktualizovat stávající|
|/Sites/slots/DELETE|Odstraňte existující Slot webové aplikace|
|/Sites/slots/Backup/Action|Vytvoří novou zálohu Slot webové aplikace.|
|/Sites/slots/publishxml/Action|Získat publikování profil xml pro Slot webové aplikace|
|/Sites/slots/Publish/Action|Publikování Slot webové aplikace|
|/Sites/slots/restart/Action|Restartujte Slot webové aplikace|
|/Sites/slots/Start/Action|Spustit Slot webové aplikace|
|/Sites/slots/stop/Action|Zastavit Slot webové aplikace|
|/Sites/slots/slotsswap/Action|Prohodit sloty nasazení webové aplikace|
|/Sites/slots/slotsdiffs/Action|Získat rozdíly v konfiguraci mezi sloty a webové aplikace|
|/Sites/slots/applySlotConfig/Action|Aplikovat konfiguraci slot webové aplikace z cílového slotu toohello aktuální pozici.|
|/Sites/slots/resetSlotConfig/Action|Resetovat konfiguraci slot webové aplikace|
|/Sites/slots/Read|Získat vlastnosti hello slot nasazení webové aplikace|
|/Sites/slots/newpassword/Action|Sloty nové_heslo webové aplikace.|
|/Sites/slots/Sync/Action|Sloty webové aplikace synchronizace.|
|/Sites/slots/operationresults/Read|Získáte výsledky operace sloty webové aplikace.|
|/Sites/slots/webjobs/Read|Načtení úloh Webjob sloty webové aplikace.|
|/Sites/slots/Backup/Write|Aktualizujte zálohování sloty webové aplikace.|
|/Sites/slots/metricdefinitions/Read|Získáte definice metrik sloty webové aplikace.|
|/Sites/slots/Metrics/Read|Získáte metriky sloty webové aplikace.|
|/Sites/slots/continuouswebjobs/DELETE|Odstranění webové aplikace sloty nepřetržité webové úlohy.|
|/Sites/slots/continuouswebjobs/Read|Získáte webové aplikace sloty nepřetržité webové úlohy.|
|/Sites/slots/continuouswebjobs/Start/Action|Spuštění webové aplikace sloty nepřetržité webové úlohy.|
|/Sites/slots/continuouswebjobs/stop/Action|Zastavení webové aplikace sloty nepřetržité webové úlohy.|
|/Sites/slots/premieraddons/DELETE|Odstranění webové aplikace sloty Premier rozšíření.|
|/Sites/slots/premieraddons/Read|Získáte webové aplikace sloty Premier rozšíření.|
|/Sites/slots/premieraddons/Write|Aktualizace webové aplikace sloty Premier doplňků.|
|/Sites/slots/triggeredwebjobs/DELETE|Odstraňte spouštěná WebJobs sloty webové aplikace.|
|/Sites/slots/triggeredwebjobs/Read|Získáte spouštěná WebJobs sloty webové aplikace.|
|/Sites/slots/triggeredwebjobs/Run/Action|Sloty spuštění webové aplikace aktivované webové úlohy.|
|/Sites/slots/hostnamebindings/DELETE|Odstraňte vazby názvů hostitelů sloty webové aplikace.|
|/Sites/slots/hostnamebindings/Read|Získáte vazby názvů hostitelů sloty webové aplikace.|
|/Sites/slots/hostnamebindings/Write|Aktualizujte vazby názvů hostitelů sloty webové aplikace.|
|/Sites/slots/phplogging/Read|Získáte Phplogging sloty webové aplikace.|
|/Sites/slots/virtualnetworkconnections/DELETE|Odstraňte připojení virtuální sítě sloty webové aplikace.|
|/Sites/slots/virtualnetworkconnections/Read|Získání připojení virtuální sítě sloty webové aplikace.|
|/Sites/slots/virtualnetworkconnections/Write|Aktualizujte připojení virtuální sítě sloty webové aplikace.|
|/Sites/slots/virtualnetworkconnections/gateways/Write|Aktualizace webové aplikace sloty virtuální síť připojení brány.|
|/Sites/slots/usages/Read|Získáte použití sloty webové aplikace.|
|/Sites/slots/hybridconnection/DELETE|Odstranění webové aplikace sloty hybridní připojení.|
|/Sites/slots/hybridconnection/Read|Získáte webové aplikace sloty hybridní připojení.|
|/Sites/slots/hybridconnection/Write|Aktualizace webové aplikace sloty hybridní připojení.|
|/Sites/slots/config/Read|Získat nastavení konfigurace Slot webové aplikace|
|/Sites/slots/config/list/Action|Seznam Slot webové aplikace citlivé nastavení zabezpečení, jako je například publikování přihlašovací údaje, nastavení aplikace a připojovacích řetězců|
|/Sites/slots/config/Write|Aktualizovat nastavení konfigurace Slot webové aplikace|
|/Sites/slots/config/DELETE|Odstraňte konfigurační sloty webové aplikace.|
|/Sites/slots/instances/Read|Získá instance sloty webové aplikace.|
|/Sites/slots/instances/Processes/Read|Získáte webové aplikace sloty instancí procesy.|
|/Sites/slots/instances/Deployments/Read|Získá webové aplikace sloty instancí nasazení.|
|/Sites/slots/sourcecontrols/Read|Získat nastavení konfigurace Slot webové aplikace zdrojového kódu|
|/Sites/slots/sourcecontrols/Write|Aktualizovat nastavení konfigurace správy zdrojů Slot webové aplikace|
|/Sites/slots/sourcecontrols/DELETE|Odstranění nastavení konfigurace správy zdrojů Slot webové aplikace|
|/Sites/slots/Restore/Read|Získáte obnovení sloty webové aplikace.|
|/Sites/slots/analyzecustomhostname/Read|Získat webové aplikace sloty analyzovat vlastní název hostitele.|
|/Sites/slots/backups/Read|Získat vlastnosti hello zálohy se sloty webové aplikace|
|/Sites/slots/backups/list/Action|Seznam webových aplikací sloty zálohování.|
|/Sites/slots/backups/Restore/Action|Obnovte zálohy sloty webové aplikace.|
|/Sites/slots/Deployments/DELETE|Odstraňte nasazení sloty webové aplikace.|
|/Sites/slots/Deployments/Read|Získá nasazení sloty webové aplikace.|
|/Sites/slots/Deployments/Write|Aktualizace nasazení sloty webové aplikace.|
|/Sites/slots/Deployments/log/Read|Získáte webové aplikace sloty nasazení protokolu.|
|/Sites/hybridconnection/DELETE|Odstranění webové aplikace hybridní připojení.|
|/Sites/hybridconnection/Read|Získáte webové aplikace hybridní připojení.|
|/Sites/hybridconnection/Write|Aktualizace webové aplikace hybridní připojení.|
|/Sites/recommendationhistory/Read|Zobrazit historii doporučení webové aplikace.|
|/Sites/Recommendations/Read|Získejte hello seznam doporučení pro webovou aplikaci.|
|/Sites/Recommendations/disable/Action|Zakážete doporučení webové aplikace.|
|/Sites/config/Read|Získat nastavení konfigurace webové aplikace|
|/Sites/config/list/Action|Seznam webové aplikace citlivé nastavení zabezpečení, jako je například publikování přihlašovací údaje, nastavení aplikace a připojovacích řetězců|
|/Sites/config/Write|Aktualizovat nastavení konfigurace webové aplikace|
|/Sites/config/DELETE|Odstraňte konfigurační webové aplikace.|
|/Sites/instances/Read|Získá instance webové aplikace.|
|/Sites/instances/Processes/DELETE|Odstraňte procesy instance webové aplikace.|
|/Sites/instances/Processes/Read|Získáte procesy instance webové aplikace.|
|/Sites/instances/Deployments/Read|Získá nasazení instance webové aplikace.|
|/Sites/sourcecontrols/Read|Získat nastavení konfigurace zdrojového kódu webové aplikace|
|/Sites/sourcecontrols/Write|Aktualizovat nastavení konfigurace správy zdrojů webové aplikace|
|/Sites/sourcecontrols/DELETE|Odstranění nastavení konfigurace správy zdrojů webové aplikace|
|/Sites/Restore/Read|Získáte obnovení webové aplikace.|
|/Sites/analyzecustomhostname/Read|Analýza vlastní název hostitele.|
|/Sites/backups/Read|Získat vlastnosti hello zálohování webové aplikace|
|/Sites/backups/list/Action|Seznam webových aplikací zálohování.|
|/Sites/backups/Restore/Action|Obnovte zálohy webové aplikace.|
|/Sites/snapshots/Read|Získáte snímky webové aplikace.|
|/Sites/Functions/DELETE|Odstraňte funkce webové aplikace.|
|/Sites/Functions/listsecrets/Action|Funkce, webové aplikace seznamu tajných klíčů.|
|/Sites/Functions/Read|Získáte funkce Web Apps.|
|/Sites/Functions/Write|Aktualizace funkcí webové aplikace.|
|/Sites/Deployments/DELETE|Odstraňte nasazení webové aplikace.|
|/Sites/Deployments/Read|Získá nasazení webové aplikace.|
|/Sites/Deployments/Write|Aktualizace nasazení webové aplikace.|
|/Sites/Deployments/log/Read|Získání protokolu nasazení webové aplikace.|
|/Sites/Diagnostics/Read|Získání diagnostiky webové aplikace.|
|/Sites/Diagnostics/workerprocessrecycle/Read|Získáte recyklace procesu Worker webové aplikace diagnostiky.|
|/Sites/Diagnostics/workeravailability/Read|Získáte Workeravailability webové aplikace diagnostiky.|
|/Sites/Diagnostics/runtimeavailability/Read|Získáte dostupnosti webové aplikace diagnostiky modulu Runtime.|
|/Sites/Diagnostics/cpuanalysis/Read|Získáte Cpuanalysis webové aplikace diagnostiky.|
|/Sites/Diagnostics/servicehealth/Read|Získání stavu služby webových aplikací diagnostiky.|
|/Sites/Diagnostics/frebanalysis/Read|Získáte webové analýzy FREB diagnostiky aplikace.|
|/availablestacks/Read|Získáte zásobníky k dispozici.|
|/isusernameavailable/Read|Zkontrolujte, zda uživatelské jméno je k dispozici.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API/čtení|Získání seznamu hello rozhraní API.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API a zápis|Přidání nového rozhraní Api nebo aktualizovat existující.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API nebo odstranění|Odstranění existujícího rozhraní Api.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API nebo připojení/čtení|Získejte hello seznam připojení.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API a připojení/zápis|Uložení nové připojení nebo aktualizuje stávající.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API, připojení nebo odstranění|Odstraňte existující připojení.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API nebo připojení/connectionAcls nebo pro čtení|ConnectionAcls pro čtení|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API a připojení nebo connectionAcls/zápis|Přidat nebo aktualizovat ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API, připojení, connectionAcls nebo odstranění|Odstranit ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API/connectionAcls/čtení|Čtení ConnectionAcls pro rozhraní Api|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API/apiAcls/čtení|ConnectionAcls pro čtení|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API a apiAcls/zápis|Vytvořit nebo aktualizovat seznamy ACL rozhraní Api|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API, apiAcls nebo odstranění|Odstranit rozhraní Api seznamy řízení přístupu|
|/ serverfarms/čtení|Získat vlastnosti hello na plán služby App Service|
|/ serverfarms/zápisu|Nový plán služby App Service umožňuje vytvořit nebo aktualizovat stávající|
|/ serverfarms/odstranit|Odstranit existující plán služby App Service|
|/serverfarms/restartSites/Action|Restartujte všechny webové aplikace v plán služby App Service|
|/serverfarms/operationresults/Read|Získáte výsledky operace plány služby App Service.|
|/serverfarms/Capabilities/Read|Získáte možnosti plány služby App Service.|
|/serverfarms/metricdefinitions/Read|Získáte definice metrik plány služby App Service.|
|/serverfarms/Metrics/Read|Získáte metriky plány služby App Service.|
|/serverfarms/hybridconnectionplanlimits/Read|Získáte limity plán služby App Service plány hybridní připojení.|
|/serverfarms/virtualnetworkconnections/Read|Získání připojení virtuální sítě plány služby App Service.|
|/serverfarms/virtualnetworkconnections/Routes/DELETE|Odstraňte směrování připojení virtuální sítě plány služby App Service.|
|/serverfarms/virtualnetworkconnections/Routes/Read|Získáte trasy připojení virtuální sítě plány služby App Service.|
|/serverfarms/virtualnetworkconnections/Routes/Write|Aktualizace tras připojení virtuální sítě plány služby App Service.|
|/serverfarms/virtualnetworkconnections/gateways/Write|Aktualizace brány připojení virtuální sítě plány služby App Service.|
|/serverfarms/firstpartyapps/Settings/DELETE|Odstraňte první strany aplikace nastavení plánů služby App Service.|
|/serverfarms/firstpartyapps/Settings/Read|Získáte nastavení první strany aplikací plány služby App Service.|
|/serverfarms/firstpartyapps/Settings/Write|Aktualizujte nastavení první strany aplikací plány služby App Service.|
|/serverfarms/Sites/Read|Získáte webové aplikace plány aplikační služby.|
|/serverfarms/workers/reboot/Action|Restartování pracovních procesů plány služby App Service.|
|/serverfarms/hybridconnectionrelays/Read|Získáte předávací připojení hybridní plány služby App Service.|
|/serverfarms/skus/Read|Získáte SKU plány služby App Service.|
|/serverfarms/usages/Read|Získáte použití plány služby App Service.|
|/serverfarms/hybridconnectionnamespaces/relays/Sites/Read|Získáte plány služby App Service hybridní připojení obory názvů předávací webové aplikace.|
|/ishostnameavailable/Read|Zkontrolujte, zda název hostitele je k dispozici.|
|/ connectionGateways/čtení|Získejte seznam hello připojení brány.|
|/ connectionGateways/zápisu|Vytvoří nebo aktualizuje připojení brány.|
|/ connectionGateways/odstranit|Odstraní připojení brány.|
|/connectionGateways/JOIN/Action|Spojí připojení brány.|
|/classicmobileservices/Read|Získáte Classic mobilních služeb.|
|/skus/Read|Získáte SKU.|
|/ certifikátů/čtení|Získání seznamu hello certifikátů.|
|/ certifikátů/zápisu|Přidat nový certifikát nebo aktualizovat existující.|
|/ certifikátů/odstranit|Odstraňte existující certifikát.|
|/Operations/Read|Získejte operace.|
|/ doporučení/čtení|Získejte hello seznam doporučení pro předplatné.|
|/ishostingenvironmentnameavailable/Read|Získáte, pokud je k dispozici hostování název prostředí.|
|/ apiManagementAccounts/čtení|Získejte seznam hello ApiManagementAccounts.|
|/ apiManagementAccounts/zápisu|Přidat nové ApiManagementAccount nebo aktualizovat stávající|
|/ apiManagementAccounts/odstranit|Odstraňte existující ApiManagementAccount|
|/apiManagementAccounts/connectionAcls/Read|Získejte hello seznam ACL připojení.|
|/apiManagementAccounts/apiAcls/Read|ConnectionAcls pro čtení|
|/ připojení/čtení|Získejte hello seznam připojení.|
|/ připojení/zápisu|Vytvoří nebo aktualizuje připojení.|
|/ připojení/odstranit|Odstraní připojení.|
|/Connections/JOIN/Action|Spojí připojení.|
|/Connections/confirmconsentcode/Action|Potvrďte souhlas kód připojení.|
|/Connections/listconsentlinks/Action|Seznam odkazů souhlasu pro připojení.|
|/deploymentlocations/Read|Získáte umístění nasazení.|
|/sourcecontrols/Read|Získáte ovládací prvky zdroje.|
|/ sourcecontrols/zápisu|Aktualizace ovládacích prvků zdroje.|
|/managedhostingenvironments/Read|Získat spravované hostitelská prostředí.|
|/managedhostingenvironments/Sites/Read|Získat spravované hostování prostředí webové aplikace.|
|/managedhostingenvironments/serverfarms/Read|Získat spravované hostování prostředí plány služby App Service.|
|/Locations/managedapis/Read|Získáte umístění spravovaná rozhraní API.|
|/Locations/apioperations/Read|Získejte operace umístění rozhraní API.|
|/Locations/connectiongatewayinstallations/Read|Získáte umístění připojení brány instalace.|
|/ listSitesAssignedToHostName/čtení|Získáte názvy lokalit, které jsou přiřazené toohostname.|

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak příliš[vytvořit vlastní roli](role-based-access-control-custom-roles.md).

- Zkontrolujte hello [integrovaným rolím RBAC](role-based-access-built-in-roles.md).

- Zjistěte, jak toomanage přistupovat k přiřazení [uživatelem](role-based-access-control-manage-assignments.md) nebo [prostředkem](role-based-access-control-configure.md) 
