---
title: "Nástroj hodnocení řešení Intelligence aaaCortana | Microsoft Docs"
description: "Jako Partner společnosti Microsoft tady jsou všechny kroky hello potřebujete toofollow toopublish vaše řešení tooAppSource Cortana Intelligence."
services: machine-learning
documentationcenter: 
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: anupams;v-bruham;garye
ms.openlocfilehash: 76cde4e2090c121683b7026f3d80f90f64566607
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-solution-evaluation-tool"></a>Nástroj pro vyhodnocení řešení Cortana Intelligence
## <a name="overview"></a>Přehled
Pokročilá analytická řešení hello Cortana Intelligence řešení vyhodnocení nástroj tooassess můžete použít pro s doporučuje Microsoft doporučenými postupy. Společnost Microsoft se nadšení toowork s našimi partnery (nezávislí dodavatelé softwaru / SIs) tooprovide vysoce kvalitní řešení pro zákazníky, prodejce a implementace. Tento průvodce provede hello proces hello se nástroj hodnocení řešení pomocí řešení a popisují konkrétní osvědčené postupy v kontroluje hello.

## <a name="getting-started"></a>Začínáme
Prosím [Stáhnout](https://aka.ms/aa-evaluation-tool-download) a nainstalujte nástroj hodnocení řešení Cortana Intelligence hello.

Požadavky:
- Windows 10: [oficiální web pro Windows 10](https://www.microsoft.com/en-us/windows)
- Azure Powershell: [nainstalovat a nakonfigurovat Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).

## <a name="identifying-your-app"></a>Identifikace vaší aplikace
Po dokončení instalace, otevřete nástroj hello a proces první hodnocení.

![Nástroj otevřené vyhodnocování](./media/cortana-intelligence-appsource-evaluation-tool/1-open-evaluation-tool.png)

Zadejte identifikační informace o řešení.

![Připojte předplatné Azure](./media/cortana-intelligence-appsource-evaluation-tool/2-connect-azure-subscription.png)

Připojte tooyour předplatného Azure a zadejte hello skupinu prostředků obsahující aplikaci.

![Vyberte prostředky](./media/cortana-intelligence-appsource-evaluation-tool/3-select-resources.png)

Jakmile se načetl hello skupinu prostředků, vyberte hello prostředky, které jsou součástí vašeho řešení a identifikovat hello usnadnění všech zdrojů dat jako:
- Přijímání
- Využití
- Interní

Tyto informace používáme toobetter pochopit, jak je řešení využívá různé součásti a tooensure uživatelsky orientovaný součásti jsou v souladu s osvědčenými postupy.

### <a name="ingestion"></a>Přijímání
Přijímání v tomto případě znamená žádné zdroje dat, které jsou používané toopull v datech z řešení mimo hello nebo že žádné služby mimo hello řešení použít toopush data do ní.

### <a name="consumption"></a>Využití
Spotřeba v tomto případě znamená žádné datové sady, které jsou používané toopush uživatelé tooend dat, buď přímo nebo nepřímo. Například:
- Datové sady použité v přímý dotaz z PowerBI.
- Datové sady v WebApp dotazována.

>[!NOTE]
Pokud konkrétní prostředek slouží k přijímání a využití, zvolte **spotřeba**.

### <a name="internal"></a>Interní
Použití interní pro zdroje dat, které se používají pouze pro interní aplikační zpracováním.

Dále budete výzvami tooprovide platné přihlašovací údaje pro všechny databáze zadali v předchozím kroku hello:

![Požadavky testovací sady](./media/cortana-intelligence-appsource-evaluation-tool/4-set-test-prerequisites.png)

## <a name="solution-test-cases"></a>Řešení testovacích případů
Nástroj řešení Hello provádí kolekce automatizovaných testů na vaše řešení.

![Spuštění testovací sady](./media/cortana-intelligence-appsource-evaluation-tool/5-set-test-execution.png)

Po dokončení testů hello budete vyzváni tooprovide vysvětlení nebo její odůvodnění proč řešení není v souladu s požadavky hello.

![Uveďte její odůvodnění firmy](./media/cortana-intelligence-appsource-evaluation-tool/6-provide-business-justification.png)

Například pokud vaše řešení publikuje tooAzure SQL DW, hello vyhodnocení, testy potřeba tooalso můžete publikovat tooAzure Analysis Services. 

Řešení může používat virtuální počítače IaaS systémem Sql Server Analysis Services místo Azure Analysis Services. To může být přijatelné příčinu selhání testu hello.
## <a name="packaging-your-evaluation-results"></a>Balení výsledky vyhodnocení
Po dokončení hello testovacích případů se váš balíček vyhodnocení bude tooa exportovaný soubor zip a zobrazí se výzva tooprovide zpětnou vazbu na hello se nástroj hodnocení. 

Je nutné tento test výsledků souboru zip se společností Microsoft pro vaše řešení toobe vyhodnotit před získáním schválení toobe přidat tooshare tooAppSource

![Nástroj pro vyhodnocení úrovni](./media/cortana-intelligence-appsource-evaluation-tool/7-grade-evaluation-tool.png)

Výše části tohoto článku popisuje různé funkce nástroje hello, teď dejte nám zkontrolujte typy osvědčených postupů, které vyhodnotí tento nástroj.

## <a name="security-evaluation-considerations"></a>Vyhodnocení aspekty zabezpečení
### <a name="databases-should-use-azure-active-directory-authentication"></a>Databáze by měla mít ověřování Azure Active Directory
Všechny prostředky Azure SQL nebo Azure SQL DW v hello sloution by měl povolit ověřování Azure Active Directory (AAD). AAD poskytuje jednotné místo toomanage všechny identity a rolí.

| Další informace o | Najdete v tomto článku |
| --- | --- |
| AAD s SQL Database a SQL Data Warehouse | [Pomocí ověřování Azure Active Directory k ověřování připojení k SQL Database nebo SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication) |
| Konfigurovat a spravovat AAD | [Konfigurovat a spravovat ověřování Azure Active Directory s SQL Database nebo SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) |
| Azure WebApps ověřování | [Ověřování a autorizace ve službě Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-authentication-overview) |
| Konfigurace WebApps v AAD | [Jak tooconfigure přihlášení vaší služby App Service toouse aplikace Azure Active Directory](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication)|

### <a name="datasets-accessible-tooend-users-should-support-role-based-access-control"></a>Datové sady dostupné tooend – uživatelé musí podporovat řízení přístupu na základě rolí
Při provádění hello se nástroj hodnocení, budete vyzváni toospecify žádné vytváření sestav, nebo publikování prostředků. Předpokládá se, že tyto prostředky jsou určeny pro přístup koncových uživatelů, nikoli pro vývojáře. Tyto prostředky by měl být poskytovat řízení přístupu na základě role (RBAC) v pořadí tooensure, aby koncoví uživatelé se, že pouze možnost tooaccess oprávnění data.

Konkrétně žádné hello následující prostředky Azure se dá nakonfigurovat s RBAC a jsou považovány za přijatelné:
- Zabezpečení HDInsight naleznete v tématu [zabezpečení služby Úvod tooHadoop s clustery HDInsight připojený k doméně](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-domain-joined-introduction)
- Azure SQL, najdete v části [AAD ověřování s Azure SQL]( https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication)
- Azure Analysis Services, najdete v části [spravovat role databáze a uživatelů pro Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-database-users)
- Azure SQL Data Warehouse (mějte na paměti, protože SQL DW podporuje RBAC, není doporučeno pro koncového uživatele přímý přístup.)

Pokud používáte jiný typ prostředku podporující RBAC, zadejte, v hello testovacího případu zarovnání do bloku.

### <a name="azure-data-lake-store-should-use-at-rest-encryption"></a>Azure Data Lake Store měli používat šifrování na rest
Azure Data Lake Store (ADLS) podporuje šifrování na rest ve výchozím nastavení pomocí ADLS spravované šifrovací klíče. Můžete také nakonfigurovat šifrování pomocí Azure Key Vault.

Informace o zadání nastavení šifrování ADLS [vytvoření účtu Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal#create-an-azure-data-lake-store-account).

### <a name="azure-sql-and-azure-sql-data-warehouse-should-use-encryption"></a>Azure SQL a Azure SQL Data Warehouse měli použít šifrování
Azure SQL a Azure SQL DW podporují transparentní dat šifrování (TDE), který poskytuje v reálném čase šifrování a dešifrování souborů protokolu a data.

| Další informace o | Najdete v tomto článku |
| --- | --- |
| Transparentní šifrování dat (šifrování TDE) | [Transparentní šifrování dat](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Azure SQL Data Warehouse pomocí šifrování TDE | [SQL datového skladu Encrption TDE TSQL](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql) |
| Konfigurace Azure SQL pomocí šifrování TDE | [Transparentní šifrování dat s databází Azure SQL](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database) |
| Konfigurace SQL Azure s vždycky šifrovaná. | [Databáze SQL Always Encrypted Azure Key Vault](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault)|

Kromě toho tooTDE, Azure SQL podporuje také vždy šifrována, nové technologie šifrování dat, která zajišťuje, že data se šifrují nejen na rest a během pohybu mezi klientem a serverem, ale také při dat se používá při provádění příkazů na serveru hello.

### <a name="any-virtual-machines-must-be-deployed-from-hello-azure-marketplace"></a>Všechny virtuální počítače musí být nasazený z hello Azure Marketplace
V pořadí tooprovide konzistentní úroveň zabezpečení napříč AppSource jsme vyžadovat, aby žádné virtuální počítače nasazené jako součást řešení Cortana Intelligence certifikaci a publikované na hello Azure Marketplace.

toosearch hello aktuální seznam Azure Marketplace Image, najdete v části [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute).

Informace o tom, jak toopublish virtuálního počítače obrázků pro Azure Marketplace naleznete v tématu [Průvodce toocreate bitovou kopii virtuálního počítače pro hello Azure Marketplace](https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation).

## <a name="scalability-evaluation-considerations"></a>Důležité informace ke škálovatelnosti vyhodnocení
### <a name="cortana-intelligence-solutions-should-include-a-scalable-big-data-platform"></a>Řešení Cortana Intelligence by měla obsahovat platformu škálovatelné velkých objemů dat
Řešení Cortana Intelligence by měl škálovat toovery velkých objemů dat velikosti. V Azure to znamená, že se má použít jeden z hello dvou škálování Petabajty dat platforem:
- Azure Data Lake Store
- Azure SQL Data Warehouse

Pokud vaše řešení nevyžaduje podporu pro tyto velikosti dat nebo pokud používáte alternativní datová platforma, vysvětlete to v hello testovacího případu zarovnání do bloku.
### <a name="cortana-intelligence-solutions-should-include-dedicated-ingestion-data-environments"></a>Řešení Cortana Intelligence by měla obsahovat vyhrazený přijímání dat prostředí
Řešení Cortana Intelligence byste obvykle neměli přímo vložení dat do zdroje relačních dat. Nezpracovaná data místo toho by měly být uložené v prostředí nestrukturovaných s idempotent vložení nebo aktualizace do všech relační úložiště pomocí Azure Data Factory.

Další informace o kopírování dat pomocí Azure Data Factory [kurz: vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-copy-activity-tutorial-using-visual-studio).

### <a name="azure-sql-data-warehouse-should-use-polybase-for-data-ingestion"></a>Azure SQL Data Warehouse by měly používat PolyBase pro přijímání dat
Azure SQL DW podporuje pro přijímání dat vysoce škálovatelnou a paralelní PolyBase. PolyBase umožňuje toouse Azure SQL DW tooissue dotazů vůči externích datových sad, které jsou uložené v Azure Blob Storage nebo Azure Data Lake Store. To poskytuje vysoce výkonné tooalternative metody hromadné aktualizace.

Pokyny Začínáme s funkcí PolyBase a Azure SQL DW, najdete v části [načtení dat pomocí funkce PolyBase v SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-get-started-load-with-polybase).

Další informace o osvědčených postupech s PolyBase a datovým Skladem SQL Azure najdete v tématu [Průvodce pro používání funkce PolyBase v SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide).

## <a name="availability-evaluation-considerations"></a>Důležité informace o vyhodnocení dostupnosti

### <a name="datasets-accessible-tooend-users-should-support-a-large-volume-of-concurrent-users"></a>Datové sady dostupné tooend uživatele by měly podporovat velký objem souběžných uživatelů
Při provádění hello se nástroj hodnocení, budete vyzváni toospecify žádné vytváření sestav, nebo publikování prostředků. Předpokládá se, že tyto prostředky jsou určeny pro přístup koncových uživatelů, nikoli pro vývojáře. Tyto prostředky by měly podporovat středně velký počet souběžných uživatelů.

Konkrétně Azure SQL Data Warehouse by neměl být hello jedinou datového zdroje k dispozici tooend uživatelé. Pokud Azure SQL DW je zadaný jako prostředek pro Power Users, Azure Analysis Services je třeba k dispozici tootypical uživatele.

Další informace o omezeních souběžnosti Azure SQL DW najdete v tématu [souběžnosti a úlohy správy v SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-develop-concurrency).

Další informace o Azure Analysis Services najdete v tématu [Přehled služby Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview).

### <a name="azure-sql-resources-should-have-a-read-only-replica-for-failover"></a>Prostředky Azure SQL by měly mít repliku jen pro čtení pro převzetí služeb při selhání
Databáze Azure SQL podporují geografická replikace tooa sekundární instance. Tato instance pak slouží jako převzetí služeb při selhání instance tooprovide vysokou dostupnost aplikací.

Další informace o geografická replikace pro databáze Azure SQL najdete v tématu [Geografickou replikaci databáze SQL přehled](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview).

Pokyny najdete v části tooconfigure geografická replikace pro server Azure SQL, [konfigurace aktivní geografickou replikací pro Azure SQL Database pomocí jazyka Transact-SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-transact-sql).

### <a name="azure-sql-data-warehouse-should-have-geo-redundant-backups-enabled"></a>Azure SQL Data Warehouse by měl mít geograficky redundantní zálohy povoleno
Azure SQL DW podporuje denní zálohy toogeo redundantní úložiště. Geografická replikace zajistí, že můžete obnovit hello datového skladu i v situacích, kdy nelze získat přístup snímky uložené ve vaší primární oblasti. Tato funkce je ve výchozím a nesmí být zakázat Cortana Intelligence řešení.

Další informace o Azure SQL DW zálohování a obnovení, zde [SQL Data Warehouse zálohy](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-backups).

### <a name="virtual-machines-should-be-configured-with-availability-sets"></a>Virtuální počítače by měly být nakonfigurované skupiny dostupnosti
Virtuální počítače Azure musí být nakonfigurovaný v nastavení dostupnosti v pořadí toominimize hello dopad plánovaných a neplánovaných údržby události.

Další informace o dostupnosti virtuálního počítače Azure najdete v tématu [spravovat hello dostupnost virtuální počítače s Windows v Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability).

## <a name="other-evaluation-considerations"></a>Další důležité informace o vyhodnocení
### <a name="cortana-intelligence-apps-should-use-a-centralized-tool-for-data-orchestration"></a>Cortana Intelligence aplikace by měl použít centrální nástroj pro Orchestrace dat
Pomocí jediného nástroje pro správu a plánování přesun dat a transformaci zajišťuje konzistenci kolem důležitá data. Umožňuje vymazat logiku kolem logika opakovaných pokusů, správě závislosti, výstrahy nebo protokolování, atd. Doporučujeme používat hello [Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-introduction) pro Orchestrace dat v Azure.

Pokud používáte nástroj pro Orchestrace dat než Azure Data Factory, popište prosím, které nástroje nebo nástroje, které používáte.
### <a name="azure-machine-learning-models-should-be-retrained-using-azure-data-factory"></a>Modely Azure Machine Learning by měla být retrained pomocí Azure Data Factory
Azure Machine Learning (AzureML) poskytuje snadno toouse nástroje pro hello vytvoření a nasazení prediktivního modelování a strojového učení kanály. Ale je důležité, že není založena na jednu datovou sadu s pevnou nasazení v produkčním prostředí těchto modelů AzureML, ale místo toho přizpůsobuje toohello dynamics reálného jevů s posunem.

Další informace o vytváření retraining webové služby v AzureML najdete v tématu [Machine Learning Přeučování modelů prostřednictvím kódu programu](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-retrain-models-programmatically).

Další informace o automatizaci procesu školení modelu hello pomocí Azure Data Factory najdete v tématu [aktualizace Azure Machine Learning modelů pomocí aktivita prostředku aktualizace](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-azure-ml-update-resource-activity).

## <a name="existing-documentation"></a>Stávající dokumentaci
[Microsoft Azure certifikované toogrow vaší firmě cloudu](https://azure.microsoft.com/en-us/marketplace/programs/certified/)

[Certifikované pro Cortana Intellignece Microsoft Azure](https://azure.microsoft.com/en-us/marketplace/programs/certified/cortana/)

