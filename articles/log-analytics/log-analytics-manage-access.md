---
title: "pracovní prostory aaaManage na portálu Azure Log Analytics a hello OMS | Microsoft Docs"
description: "Můžete spravovat pracovních prostorů Azure Log Analytics a hello portálu OMS pomocí různých úloh správy uživatelů, účty, pracovních prostorů a účtů Azure."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/06/2017
ms.author: magoedte
ms.openlocfilehash: 570e6c1f13ad28f120ecd65052d00c4ca986ac98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-workspaces"></a>Správa pracovních prostorů

toomanage přístup tooLog analýzy, provádět různé úlohy správy související tooworkspaces. Tento článek obsahuje osvědčený postup postupů a Rady, jak toomanage pracovních prostorů. Pracovní prostor je v podstatě kontejner, který obsahuje informace o účtu a jednoduchý konfigurační informace pro účet hello. Můžete nebo jinými členy vaší organizace může použít několik pracovních prostorů toomanage různé sady dat, která se shromažďují ze všech nebo části vaši IT infrastrukturu.

toocreate pracovního prostoru, budete muset:

1. Mít předplatné Azure.
2. Zvolit název pracovního prostoru.
3. Pracovní prostor hello přidružte ke svému předplatnému.
4. Vybrat zeměpisné umístění.

## <a name="determine-hello-number-of-workspaces-you-need"></a>Určit počet hello pracovní prostory, které potřebujete
Pracovní prostor je prostředek služby Azure a je kontejner, kde se data shromažďují, agregován, analyzovat a zobrazovat v hello portálu Azure.

Může mít několik pracovních prostorů za předplatné a můžou mít přístup toomore než jednoho pracovního prostoru. Minimalizovat počet hello pracovních prostorů můžete tooquery a korelovat napříč hello většinu dat, protože to není možné tooquery napříč více pracovní prostory. Tato část popisuje, kdy může být užitečné toocreate více než jednoho pracovního prostoru.

V současné době pracovní prostor nabízí:

* Zeměpisné umístění úložiště dat
* Členitost fakturace
* Izolaci dat
* Obor pro konfiguraci

Podle hello předcházející charakteristiky, je vhodné toocreate několik pracovních prostorů pokud:

* Jste globální společnost a potřebujete ukládat data v různých oblastech z důvodů suverenity dat nebo dodržování předpisů.
* Používáte Azure a chcete, aby tooavoid odchozí přenosy dat poplatky tak, že pracovním prostoru hello stejné oblasti jako hello spravuje prostředky Azure.
* Chcete tooallocate poplatky toodifferent oddělení nebo obchodní skupiny na základě jejich využití. Při vytváření pracovního prostoru pro každé oddělení nebo obchodní skupiny zobrazuje Azure faktury a použití příkazu hello poplatky za každý pracovní prostor samostatně.
* Jsou poskytovatel spravované služby a nutnost tookeep hello protokolu analytická data každého zákazníka a to, které spravujete. jsou izolovány od jiných zákaznická data.
* Spravovat víc zákazníků a chcete, aby každý zákazník nebo oddělení / obchodní skupiny toosee svá vlastní data, ale ne hello data pro ostatní.

Pokud používáte agenty toocollect data, můžete [konfigurace tooone tooreport každého agenta nebo více pracovních prostorů](log-analytics-windows-agents.md).

Pokud používáte System Center Operations Manager, můžete připojit každou skupinu nástroje Operations Manager jen do jednoho pracovního prostoru. Můžete nainstalovat hello agenta Microsoft Monitoring Agent do počítačů spravovaných nástrojem Operations Manager a mají hello agenta sestavy tooboth nástroje Operations Manager a jiný pracovní prostor analýzy protokolů.

### <a name="workspace-information"></a>Informace o pracovním prostoru

Zobrazení podrobností o pracovního prostoru v hello portálu Azure. Podrobnosti lze také zobrazit na portálu OMS hello.

#### <a name="view-workspace-information-in-hello-azure-portal"></a>Zobrazení informací o prostoru v hello portálu Azure

1. Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí svého předplatného Azure.
2. Na hello **rozbočovače** nabídky, klikněte na tlačítko **další služby** a v hello seznamu prostředků zadejte **analýzy protokolů**. Během zadávání hello seznamu filtrů podle vašeho zadání. Klikněte na **Log Analytics**.  
    ![Centrum Azure](./media/log-analytics-manage-access/hub.png)  
3. V okně odběry analýzy protokolů hello vyberte pracovní prostor.
4. okno prostoru Hello zobrazí podrobnosti o hello prostoru a odkazy na další informace.  
    ![Podrobnosti o pracovním prostoru](./media/log-analytics-manage-access/workspace-details.png)  


## <a name="manage-accounts-and-users"></a>Správa účtů a uživatelů
Každém pracovním prostoru můžete mít přidruženo několik účtů a každý účet (účtu Microsoft nebo účtu organizace) můžou mít přístup toomultiple pracovních prostorů.

Ve výchozím nastavení stane hello správce pracovního prostoru hello hello účtu Microsoft nebo účtu organizace, který vytvoří hello prostoru.

Existují dva modely oprávnění, která řídí přístup k pracovní prostor analýzy protokolů tooa:

1. Starší uživatelské role Log Analytics
2. [Přístup na základě rolí Azure](../active-directory/role-based-access-control-configure.md)

Hello následující tabulka shrnuje hello přístup, který se dá nastavit pomocí každý model oprávnění:

|                          | Portál Log Analytics | portál Azure | Rozhraní API (včetně PowerShellu) |
|--------------------------|----------------------|--------------|----------------------------|
| Uživatelské role Log Analytics | Ano                  | Ne           | Ne                         |
| Přístup na základě rolí Azure  | Ano                  | Ano          | Ano                        |

> [!NOTE]
> Analýzy protokolů přechází toouse přístupu na základě role Azure jako model oprávnění hello, nahraďte hello analýzy protokolů uživatelské role.
>
>

Hello starší verze role uživatele analýzy protokolů jenom řízení přístupu tooactivities prováděla hello [portálu Analýza protokolů](https://mms.microsoft.com).

Hello následující aktivity také vyžadovat Azure oprávnění:

| Akce                                                          | Potřebná oprávnění Azure | Poznámky |
|-----------------------------------------------------------------|--------------------------|-------|
| Přidání a odebrání řešení pro správu                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | |
| Změna hello cenové úrovně                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Zobrazení dat v hello *zálohování* a *Site Recovery* dlaždice řešení | Správce nebo spolusprávce | Prostředkům přistupuje nasadit pomocí modelu nasazení classic hello |
| Vytváření pracovního prostoru v hello portálu Azure                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-toolog-analytics-using-azure-permissions"></a>Správa přístupu tooLog Analytics pomocí Azure oprávnění
toogrant přístup toohello pracovní prostor analýzy protokolů pomocí Azure oprávnění, postupujte podle kroků hello v [používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](../active-directory/role-based-access-control-configure.md).

Azure má pro Log Analytics dvě předdefinované role uživatele:
- Čtenář Log Analytics
- Přispěvatel Log Analytics

Členové hello *Log Analytics Reader* role můžete:
- Zobrazení a prohledávání všech dat monitorování 
- Zobrazte nastavení monitorování, včetně zobrazení hello konfiguraci Azure diagnostics na všechny prostředky Azure.

| Typ    | Oprávnění | Popis |
| ------- | ---------- | ----------- |
| Akce | `*/read`   | Možnost tooview všech prostředků a konfigurace prostředků. To zahrnuje zobrazení: <br> Stavu rozšíření virtuálního počítače <br> Konfigurace diagnostiky Azure pro prostředky <br> Všech vlastnosti a nastavení všech prostředků |
| Akce | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Možnost tooperform hledání protokolů v2 dotazy |
| Akce | `Microsoft.OperationalInsights/workspaces/search/action` | Možnost tooperform hledání protokolů v1 dotazy |
| Akce | `Microsoft.Support/*` | Možnost případů podpory tooopen |
|Jiný než akce | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Brání čtení prostoru klíče požadované toouse hello dat kolekce rozhraní API a tooinstall agentů |


Členové hello *Log Analytics Přispěvatel* role můžete:
- Čtení všech dat monitorování 
- Vytváření a konfigurace účtů služby Automation
- Přidání a odebrání řešení pro správu
- Čtení klíčů účtu úložiště 
- Konfigurace shromažďování protokolů ze služby Azure Storage
- Úprava nastavení monitorování pro prostředky Azure, včetně
  - Přidání tooVMs rozšíření virtuálního počítače hello
  - Konfigurace diagnostiky Azure pro všechny prostředky Azure

> [!NOTE] 
> Můžete použít možnost tooadd hello virtuální počítač rozšíření tooa virtuální počítač toogain plnou kontrolu nad virtuálního počítače.

| Oprávnění | Popis |
| ---------- | ----------- |
| `*/read`     | Možnost tooview všech prostředků a konfigurace prostředků. To zahrnuje zobrazení: <br> Stavu rozšíření virtuálního počítače <br> Konfigurace diagnostiky Azure pro prostředky <br> Všech vlastnosti a nastavení všech prostředků |
| `Microsoft.Automation/automationAccounts/*` | Možnost toocreate a nakonfigurovat účty Azure Automation, včetně přidání a úpravy sady runbook |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Přidat, aktualizovat a odebírat rozšíření virtuálního počítače, včetně hello agenta Microsoft Monitoring Agent rozšíření a hello OMS agenta pro Linux rozšíření |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Zobrazení hello klíč účtu úložiště. Požadované protokoly tooread tooconfigure analýzy protokolů z účtů úložiště Azure |
| `Microsoft.Insights/alertRules/*` | Přidání, aktualizace a odebrání pravidel upozornění |
| `Microsoft.Insights/diagnosticSettings/*` | Přidání, aktualizace a odebrání nastavení diagnostiky pro prostředky Azure |
| `Microsoft.OperationalInsights/*` | Přidání, aktualizace a odebrání konfigurace pro pracovní prostory Log Analytics |
| `Microsoft.OperationsManagement/*` | Přidání a odebrání řešení pro správu |
| `Microsoft.Resources/deployments/*` | Vytvoření a odstranění nasazení. Požadováno pro přidávání a odebírání řešení, pracovních prostorů a účtů služby Automation |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Vytvoření a odstranění nasazení. Požadováno pro přidávání a odebírání řešení, pracovních prostorů a účtů služby Automation |

tooadd a odebrat uživatele tooa role uživatele, je nutné toohave `Microsoft.Authorization/*/Delete` a `Microsoft.Authorization/*/Write` oprávnění.

Použijte tyto role toogive uživatelům přístup na různých místech:
- Předplatné - pracovních prostorů tooall přístup v rámci předplatného hello
- Skupiny prostředků – přístup tooall prostoru ve skupině prostředků hello
- Prostředek - přístup tooonly hello zadán pracovního prostoru

Použití [vlastní role](../active-directory/role-based-access-control-custom-roles.md) toocreate rolí s hello konkrétní oprávnění potřebná.

### <a name="azure-user-roles-and-log-analytics-portal-user-roles"></a>Role uživatele Azure a role uživatele portálu Log Analytics
Pokud máte v Azure minimálně oprávnění ke čtení pro pracovní prostor analýzy protokolů hello, portálu Analýza protokolů hello můžete otevřít kliknutím hello **portálu OMS** úkolů při prohlížení pracovní prostor analýzy protokolů hello.

Při otevření portálu analýzy protokolů hello, přepnete toousing hello starší verze analýzy protokolů uživatelské role. Pokud nemáte přiřazení role v portálu Analýza protokolů hello, hello služby [kontroly hello Azure oprávnění, které máte v prostoru hello](https://docs.microsoft.com/rest/api/authorization/permissions#Permissions_ListForResource).
Přiřazení role v portálu Analýza protokolů hello se určuje pomocí následujícím způsobem:

| Podmínky                                                   | Přiřazená uživatelské role Log Analytics | Poznámky |
|--------------------------------------------------------------|----------------------------------|-------|
| Váš účet patří tooa role uživatele starší verze analýzy protokolů     | Hello zadaná uživatelská role analýzy protokolů | |
| Váš účet nepatří tooa role uživatele starší verze analýzy protokolů <br> Pracovní prostor toohello úplné Azure oprávnění (`*` oprávnění <sup>1</sup>) | Správce ||
| Váš účet nepatří tooa role uživatele starší verze analýzy protokolů <br> Pracovní prostor toohello úplné Azure oprávnění (`*` oprávnění <sup>1</sup>) <br> *ne akce*`Microsoft.Authorization/*/Delete` a `Microsoft.Authorization/*/Write` | Přispěvatel ||
| Váš účet nepatří tooa role uživatele starší verze analýzy protokolů <br> Oprávnění Azure ke čtení | Jen pro čtení ||
| Váš účet nepatří tooa role uživatele starší verze analýzy protokolů <br> Oprávnění Azure nejsou rozpoznána. | Jen pro čtení ||
| Pro předplatná spravovaná poskytovatelem Cloud Solution Provider (CSP) <br> Hello účet, který jste přihlášeného s je v prostoru toohello Azure Active Directory propojené hello | Správce | Obvykle hello zákazníka zprostředkovatele kryptografických služeb |
| Pro předplatná spravovaná poskytovatelem Cloud Solution Provider (CSP) <br> Hello účet, který jste přihlášeného s není v prostoru toohello Azure Active Directory propojené hello | Přispěvatel | Obvykle hello zprostředkovatele kryptografických služeb |

<sup>1</sup> odkazovat příliš[Azure oprávnění](../active-directory/role-based-access-control-custom-roles.md) Další informace o definice rolí. Při vyhodnocování role, akce `*` není ekvivalentní příliš`Microsoft.OperationalInsights/workspaces/*`.

Některé body tookeep nezapomeňte o hello portálu Azure:

* Když se přihlásíte na portálu OMS toohello pomocí http://mms.microsoft.com, uvidíte hello **vyberte pracovní prostor** seznamu. Tento seznam obsahuje pouze pracovní prostory, ve kterých máte uživatelskou roli Log Analytics. pracovní prostory hello toosee máte přístup k toowith Azure odběry, je nutné toospecify klienta v rámci adresy URL hello. Například `mms.microsoft.com/?tenant=contoso.com`. identifikátor klienta Hello je často, že poslední část hello e-mailová adresa, který používáte toosign v.
* Pokud chcete toonavigate přímo tooa portálu, ke které máte přístup k toousing Azure oprávnění a potom potřebovat toospecify hello prostředků v rámci adresy URL hello. Je možné tooget je tato adresa URL pomocí prostředí PowerShell.

  Například, `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Adresa URL Hello vypadá takto:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-hello-oms-portal"></a>Správa uživatelů na portálu OMS hello
Spravovat uživatele a skupiny na hello **spravovat uživatele** kartě pod hello **účty** kartě na stránce nastavení hello.   

![Správa uživatelů](./media/log-analytics-manage-access/setup-workspace-manage-users.png)


#### <a name="add-a-user-tooan-existing-workspace"></a>Přidání uživatel tooan existujícímu pracovnímu prostoru
Pomocí následujících kroků tooadd uživatel nebo skupina pracovního prostoru tooa hello.

1. Na portálu OMS hello, klikněte na tlačítko hello **nastavení** dlaždici.
2. Klikněte na tlačítko hello **účty** a pak klikněte hello **spravovat uživatele** kartě.
3. V hello **spravovat uživatele** zvolte tooadd typ účtu hello: **účet organizace**, **Account Microsoft**, **Microsoft Support**.

   * Pokud si zvolíte Account Microsoft, zadejte e-mailovou adresu uživatele hello přidruženého k hello Account Microsoft hello.
   * Pokud si zvolíte účet organizace, zadat část hello uživatele nebo skupiny alias názvu nebo e-mailu a seznam odpovídající uživatelé a skupiny se zobrazí v rozevírací pole. Vyberte uživatele nebo skupinu.
   * Použijte Microsoft Support toogive jako Microsoft Support engineer nebo jiných Microsoft zaměstnanec dočasný přístup tooyour prostoru toohelp při řešení problémů.

     > [!NOTE]
     > Pro zajištění nejlepšího výkonu hello omezit počet hello skupiny služby Active Directory, které jsou přidružené k jedné toothree účet OMS – jeden pro správce, jeden pro přispěvatele a jeden pro uživatele, jen pro čtení. Použití více skupin může mít dopad na výkon hello analýzy protokolů.
     >
     >
4. Zvolte typ hello uživatele nebo skupinu tooadd: **správce**, **Přispěvatel**, nebo **jen pro čtení uživatele**.  
5. Klikněte na tlačítko **Přidat**.

   Pokud přidáváte účet Microsoft, pracovní prostor služby pozvánku toojoin hello odešle toohello e-mailu, který jste zadali. Po hello uživatel bude postupovat hello pokyny v hello pozvánku toojoin OMS, uživatel hello přístup hello prostoru.
   Pokud přidáváte účet organizace, uživatel hello okamžitě přístup analýzy protokolů.  

#### <a name="edit-an-existing-user-type"></a>Úprava typu existujícího uživatele
Můžete změnit hello účet role pro uživatele přidružené k účtu OMS. Máte následující možnosti role hello:

* *Správce*: může spravovat uživatele, zobrazovat a reagovat na všechny výstrahy a přidávat a odebírat servery.
* *Přispěvatel*: může zobrazovat a reagovat na všechny výstrahy a přidávat a odebírat servery.
* *Jen pro čtení*: takto označení uživatelé nemůžou:

  1. Přidávat a odebírat řešení. Galerie řešení Hello je skrytý.
  2. Přidávat, upravovat nebo odebírat dlaždice na řídicím panelu **My Dashboard** (Můj řídicí panel).
  3. Zobrazení hello **nastavení** stránky. Hello stránky jsou skryté.
  4. V zobrazení vyhledávání hello, konfigurace PowerBI, uložená hledání a výstrahy jsou skryté úlohy.

#### <a name="tooedit-an-account"></a>tooedit účet
1. Na portálu OMS hello, klikněte na tlačítko hello **nastavení** dlaždici.
2. Klikněte na tlačítko hello **účty** a pak klikněte hello **spravovat uživatele** kartě.
3. Vyberte roli hello hello uživatele, které chcete toochange.
4. V dialogové okno potvrzení hello, klikněte na **Ano**.

### <a name="remove-a-user-from-a-workspace"></a>Odebrání uživatele z pracovního prostoru
Pomocí následujících kroků tooremove uživatele z pracovního prostoru hello. Odebírání hello uživatel nezavře hello prostoru. Místo toho se odeberou hello přidružení mezi tohoto uživatele a hello prostoru. Pokud uživatel je přidružen více pracovní prostory, tento uživatel může přihlásit tooOMS a jejich jiných pracovních prostorech najdete v části.

1. Na portálu OMS hello, klikněte na tlačítko hello **nastavení** dlaždici.
2. Klikněte na tlačítko hello **účty** a pak klikněte hello **spravovat uživatele** kartě.
3. Klikněte na tlačítko **odebrat** další toohello uživatelské jméno, které chcete tooremove.
4. V dialogové okno potvrzení hello, klikněte na **Ano**.

### <a name="add-a-group-tooan-existing-workspace"></a>Přidat skupiny tooan existujícímu pracovnímu prostoru
1. V předchozím oddílu "tooadd uživatele pracovního prostoru existující tooan" hello postupujte podle kroků 1 – 4.
2. V části **Choose User/Group** (Vyberte uživatele/skupinu) vyberte **Group** (Skupina).  
   ![Přidat skupiny tooan existujícímu pracovnímu prostoru](./media/log-analytics-manage-access/add-group.png)
3. Zadejte hello zobrazované jméno nebo e-mailovou adresu pro skupinu hello chcete tooadd.
4. Vyberte skupinu hello ve výsledcích hello seznamu a pak klikněte na tlačítko **přidat**.

## <a name="link-an-existing-workspace-tooan-azure-subscription"></a>Propojit existující prostoru tooan předplatného Azure
Všechny pracovní prostory, které jsou vytvořeny po 26 září 2016 musí být propojená tooan předplatného Azure v okamžiku vytvoření. Pracovní prostory vytvořené před tímto datem musí být propojená tooa prostoru při přihlášení. Při vytváření pracovního prostoru hello z hello portálu Azure, nebo když propojit váš pracovní prostor tooan předplatné Azure, Azure Active Directory je váš účet propojen jako váš účet organizace.

### <a name="toolink-a-workspace-tooan-azure-subscription-in-hello-oms-portal"></a>toolink prostoru tooan předplatného Azure na portálu OMS hello

- Když se přihlásíte na portál OMS hello, jste výzvami tooselect předplatné Azure. Vyberte předplatné hello má toolink tooyour prostoru a pak klikněte na **odkaz**.  
    ![propojení s předplatným Azure](./media/log-analytics-manage-access/required-link.png)

    > [!IMPORTANT]
    > toolink pracovního prostoru, Azure musí mít váš účet již prostoru toohello přístup byste chtěli toolink.  Jinými slovy, musí být účet hello tooaccess hello portál Azure používáte **hello stejné** jako hello účet, který používáte tooaccess hello prostoru. Pokud ne, najdete v části [přidat uživatele pracovního prostoru existující tooan](#add-a-user-to-an-existing-workspace).

### <a name="toolink-a-workspace-tooan-azure-subscription-in-hello-azure-portal"></a>toolink prostoru tooan předplatného Azure v hello portálu Azure
1. Přihlaste se k hello [portál Azure](http://portal.azure.com).
2. Přejděte na **Log Analytics** a vyberte tuto možnost.
3. Uvidíte svůj seznam existujících pracovních prostorů. Klikněte na tlačítko **Přidat**.  
   ![seznam pracovních prostorů](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. V oddílu **OMS Workspace** klikněte na možnost **Or link existing** (Nebo propojit existující).  
   ![propojení existujícího](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. Klikněte na **Configure required settings** (Konfigurovat požadovaná nastavení).  
   ![konfigurace požadovaných nastavení](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Uvidíte, že hello seznam pracovní prostory, které nejsou propojené ještě tooyour účet Azure. Vyberte pracovní prostor.  
   ![Výběr pracovního prostoru](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. V případě potřeby můžete změnit hodnoty pro hello následující položky:
   * Předplatné
   * Skupina prostředků
   * Umístění
   * Cenová úroveň  
     ![Změna hodnot](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. Klikněte na **OK**. pracovní prostor Hello je nyní propojené tooyour účet Azure.

> [!NOTE]
> Pokud nevidíte hello prostoru byste chtěli toolink a potom vašeho předplatného Azure nemá přístup toohello prostoru, které jste vytvořili pomocí portálu OMS hello.  účet pro přístup k toothis toogrant z portálu OMS hello, najdete v části [přidat uživatele pracovního prostoru existující tooan](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-tooa-paid-plan"></a>Upgrade pracovní prostor tooa placené plán
Ve službě OMS existují tři tarify pracovního prostoru: **Free**, **Standalone** a **Premium**.  Pokud jste na hello *volné* plánování, je omezeno na 500 MB dat za den odeslané tooLog Analytics.  Pokud toto množství, musíte toochange tooavoid plán tooa placené vašeho pracovního prostoru není shromažďování dat nad rámec tohoto limitu. Svůj tarif můžete kdykoli změnit.  Informace o cenách OMS najdete na stránce [podrobností o cenách](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Používání nároků z předplatného OMS
toouse hello oprávnění, které pocházejí z nákupu OMS E1, OMS E2 OMS nebo OMS rozšíření pro System Center, zvolte hello *OMS* plán analýzy protokolů OMS.

Pokud si koupíte předplatné OMS, se přidají hello oprávnění tooyour smlouvy Enterprise. Hello oprávnění můžete použít jakéhokoli předplatného Azure, který je vytvořen v rámci této dohody. Všechny pracovní prostory v těchto předplatných použijte hello OMS oprávnění.

tooensure, že použití pracovního prostoru je použité tooyour oprávnění z hello OMS předplatné, budete muset:

1. Vytváření pracovního prostoru v předplatné Azure, který je součástí hello smlouvy Enterprise, obsahující hello OMS předplatného
2. Vyberte hello *OMS* plánování hello prostoru

> [!NOTE]
> Pokud pracovní prostor byl vytvořen ještě před 26 září 2016 a je vaše analýzy protokolů ceny plán *Premium*, tento pracovní prostor se použije oprávnění z hello OMS rozšíření pro System Center. Můžete také použít vaše oprávnění změnou toohello *OMS* cenová úroveň.
>
>

Hello OMS předplatné oprávnění nejsou viditelné v hello Azure nebo portálu OMS. Můžete zobrazit oprávnění a využití v hello podnikový portál.  

Pokud potřebujete toochange hello předplatné Azure, propojené pracovního prostoru, můžete použít hello prostředí Azure PowerShell [přesunutí AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) rutiny.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Využití závazků Azure ze smlouvy Enterprise
Pokud nemáte předplatné OMS, platíte samostatně pro každou součást OMS a využití hello se zobrazí na vaší faktuře Azure.

Pokud máte Azure peněžních závazků na hello podnikové registrace toowhich předplatné Azure propojeno, využití analýzy protokolů bude automaticky MD proti hello zbývající peněžní potvrzení.

Pokud potřebujete toochange hello předplatného Azure, které hello prostoru souvisí, můžete použít hello prostředí Azure PowerShell [přesunutí AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) rutiny.  

### <a name="change-a-workspace-tooa-paid-pricing-tier-in-hello-azure-portal"></a>Změnit pracovní prostor tooa placené cenová úroveň v hello portálu Azure
1. Přihlaste se k hello [portál Azure](http://portal.azure.com).
2. Přejděte na **Log Analytics** a vyberte tuto možnost.
3. Uvidíte svůj seznam existujících pracovních prostorů. Vyberte pracovní prostor.  
4. V okně hello pracovního prostoru v části **Obecné**, klikněte na tlačítko **cenová úroveň**.  
5. V části **Cenová úroveň** vyberte cenovou úroveň a klikněte na **Vybrat**.  
    ![výběr tarifu](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. Při aktualizaci zobrazení v hello portálu Azure, uvidíte **cenová úroveň** aktualizované hello vrstvy, které jste vybrali.  
    ![aktualizovaný plán](./media/log-analytics-manage-access/manage-access-change-plan04.png)

> [!NOTE]
> Je-li pracovní prostor propojené tooan účet Automation, než můžete vybrat hello *samostatné (GB za)* cenová úroveň je nutné odstranit všechny **automatizace a řízení** řešení a zrušit propojení hello automatizace účet. V okně hello pracovního prostoru v části **Obecné**, klikněte na tlačítko **řešení** řešení toosee a odstranění. toounlink hello účet Automation, klikněte na název hello hello účtu Automation na hello **cenová úroveň** okno.
>
>

### <a name="change-a-workspace-tooa-paid-pricing-tier-in-hello-oms-portal"></a>Změnit pracovní prostor tooa placené cenovou úroveň na portálu OMS hello

toochange hello pomocí portálu OMS hello cenovou úroveň, musíte mít předplatné Azure.

1. Na portálu OMS hello, klikněte na tlačítko hello **nastavení** dlaždici.
2. Klikněte na tlačítko hello **účty** a pak klikněte hello **předplatné Azure & datový tarif** kartě.
3. Klikněte na tlačítko hello cenová úroveň chcete toouse.
4. Klikněte na **Uložit**.  
   ![Předplatné a datové tarify](./media/log-analytics-manage-access/subscription-tab.png)

Váš nový datový tarif se zobrazí v hello OMS portálu pásu karet v horní části hello webové stránky.

![Pás karet OMS](./media/log-analytics-manage-access/data-plan-changed.png)


## <a name="change-how-long-log-analytics-stores-data"></a>Změna doby, po kterou služba Log Analytics ukládá data

Na hello volná cenová úroveň analýzy protokolů je k dispozici hello posledních sedmi dnů dat.
Na hello standardní cenovou úroveň analýzy protokolů je k dispozici hello posledních 30 dní od data.
Na hello cenová úroveň Premium analýzy protokolů je k dispozici hello posledních 365 dnů dat.
Na hello samostatné a OMS cenové úrovně, ve výchozím nastavení analýzy protokolů je k dispozici hello posledních 31 dní dat.

Pokud používáte hello samostatné a OMS cenové úrovně, můžete udržovat too2 roky dat (730 dnů). Data uložená déle než výchozí hello 31 dní způsobuje zpoplatněny uchování data. Další informace o cenách najdete v tématu věnovaném [poplatkům za nadlimitní využití](https://azure.microsoft.com/pricing/details/log-analytics/).

Délka hello toochange uchovávání dat:

1. Přihlaste se k hello [portál Azure](http://portal.azure.com).
2. Přejděte na **Log Analytics** a vyberte tuto možnost.
3. Uvidíte svůj seznam existujících pracovních prostorů. Vyberte pracovní prostor.  
4. V okně prostoru hello pod **Obecné**, klikněte na tlačítko **uchování**.  
5. Pomocí posuvníku tooincrease hello nebo snížit hello počet dní uchovávání a potom klikněte na **Uložit**.  
    ![změna uchovávání](./media/log-analytics-manage-access/manage-access-change-retention01.png)

## <a name="change-an-azure-active-directory-organization-for-a-workspace"></a>Změna organizace Azure Active Directory pracovního prostoru

Můžete změnit organizaci Azure Active Directory pracovního prostoru. Změna hello Azure Active Directory organizace vám umožní tooadd uživatelé a skupiny z tohoto pracovního prostoru toohello adresáře.

### <a name="toochange-hello-azure-active-directory-organization-for-a-workspace"></a>toochange hello Azure Active Directory organizace pro pracovní prostor

1. Na stránce nastavení hello na portálu OMS hello, klikněte na tlačítko **účty** a pak klikněte na tlačítko hello **spravovat uživatele** kartě.  
2. Zkontrolujte hello informace o účty organizace a pak klikněte na tlačítko **změnu organizace**.  
    ![změna organizace](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Zadejte hello informace o identitě hello správce vaší domény Azure Active Directory. Potom zobrazí na potvrzení, že pracovní prostor není propojené tooyour domény Azure Active Directory.  
    ![Potvrzení o propojení pracovního prostoru](./media/log-analytics-manage-access/manage-access-add-adorg02.png)


## <a name="delete-a-log-analytics-workspace"></a>Odstranění pracovního prostoru Log Analytics
Pokud odstraníte pracovní prostor analýzy protokolů, všechna data související s tooyour prostoru se odstraní z hello OMS služby do 30 dní.

Pokud jste správce a více uživatelů, které jsou přidružené k hello prostoru, hello přidružení mezi tyto uživatele a hello prostoru se přeruší. Pokud uživatelé hello přidružené jiných pracovních prostorech, potom můžou pokračovat pomocí těchto jiných pracovních prostorech OMS. Ale pokud nejsou přidruženy jiných pracovních prostorech pak potřebují toocreate toouse pracovní prostor OMS.

### <a name="toodelete-a-workspace"></a>toodelete pracovního prostoru
1. Přihlaste se k hello [portál Azure](http://portal.azure.com).
2. Přejděte na **Log Analytics** a vyberte tuto možnost.
3. Uvidíte svůj seznam existujících pracovních prostorů. Vyberte pracovní prostor hello, které chcete toodelete.
4. V okně hello pracovního prostoru, klikněte na tlačítko **odstranit**.  
    ![odstranění](./media/log-analytics-manage-access/delete-workspace01.png)
5. V potvrzovacím dialogovém hello odstranit pracovní prostor, klikněte na tlačítko **Ano**.

## <a name="next-steps"></a>Další kroky
* V tématu [tooLog počítače připojit Windows Analytics](log-analytics-windows-agents.md) tooadd agentů a shromáždit data.
* [Přidat řešení pro analýzu protokolu z hello řešení Galerie](log-analytics-add-solutions.md) tooadd funkce a shromáždění dat.
* [Nakonfigurujte nastavení serveru proxy a brány firewall v analýzy protokolů](log-analytics-proxy-firewall.md) Pokud vaše organizace používá proxy serveru nebo brány firewall, aby agenti komunikovat s hello analýzy protokolů služby.
