---
title: "aspekty aaaSecurity pro přesun dat v Azure Data Factory | Microsoft Docs"
description: "Další informace o zabezpečení přesun dat v Azure Data Factory."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 4bfce8884df14ad5b94e28ad3dfcf7025e2130a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory - důležité informace o zabezpečení pro přesun dat
## <a name="introduction"></a>Úvod
Tento článek popisuje základní zabezpečení infrastruktury pomocí služby pro přesun dat v Azure Data Factory toosecure dat. Správa prostředků Azure Data Factory jsou postaveny na infrastrukturu zabezpečení Azure a použít všechny možné bezpečnostní opatření, které nabízí Azure.

V řešení Data Factory vytváříte jeden nebo více datových [kanálů](data-factory-create-pipelines.md). Kanál je logické seskupení aktivit, které dohromady provádějí určitou úlohu. Tyto kanály nacházet v hello oblasti, kde byl vytvořen objekt pro vytváření dat hello. 

I když objekt pro vytváření dat je k dispozici v pouze **západní USA**, **východní USA**, a **Severní Evropa** oblasti, je k dispozici služba pro přesun dat hello [globálně v několik oblastí](data-factory-data-movement-activities.md#global). Objekt pro vytváření dat, které služby zajišťuje, že data nenechává zeměpisné oblasti / oblasti pokud explicitně pokyn hello služby toouse oblast alternativní Pokud není služba pro přesun dat hello ještě nasadit toothat oblast. 

Azure Data Factory, samotné neukládá všechna data s výjimkou pověření propojené služby pro cloudové úložiště dat, které jsou zašifrované pomocí certifikátů. Umožňuje vám vytvořit datové pracovních tooorchestrate přesouvání dat mezi [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a zpracování dat pomocí [výpočetní služby](data-factory-compute-linked-services.md) v jiných oblastech nebo v místní prostředí. Můžete taky příliš[sledování a správa pracovních](data-factory-monitor-manage-pipelines.md) pomocí prostřednictvím kódu programu a uživatelského prostředí.

Přesun dat pomocí Azure Data Factory byl **certifikované** pro:
-   [HIPAA NEBO HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA)  
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018) 
-   [CSA HVĚZDIČKOU](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)
     
Pokud vás zajímá Azure dodržování předpisů a jak Azure zabezpečuje svou vlastní infrastrukturu, navštivte hello [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

V tomto článku jsme projděte si informace o zabezpečení ve hello následující dva scénáře přesun dat: 

- **Scénář cloudu**– v tomto scénáři jsou veřejně přístupná prostřednictvím Internetu zdrojový i cílový. Mezi ně patří spravované cloudové úložiště služby Azure Storage, Azure SQL Data Warehouse, databáze SQL Azure, Azure Data Lake Store, Amazon S3, Amazon Redshift, služby SaaS, jako je například služby Salesforce a webové protokoly například protokoly FTP a OData. Úplný seznam podporovaných zdrojů dat můžete najít [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- **Scénář hybridního**– v tomto scénáři vaše zdrojové nebo cílové je za bránou firewall nebo uvnitř místní firemní sítě nebo hello data úložiště je v privátní síti / virtuální sítě (nejčastěji hello zdroje) a není veřejně přístupný . Databázové servery hostované ve virtuálních počítačích také spadají pod tento scénář.

## <a name="cloud-scenarios"></a>Scénáře cloudu
###<a name="securing-data-store-credentials"></a>Zabezpečení údaje k úložišti dat.
Azure Data Factory chrání přihlašovací údaje k úložišti dat pomocí **šifrování** je pomocí **certifikáty, které spravuje Microsoft**. Tyto certifikáty otáčejí každých **dva roky** (která zahrnuje obnovení certifikátu a migrace přihlašovacích údajů). Tyto zašifrované přihlašovací údaje jsou bezpečně uložené v **Azure Storage spravuje služby Azure Data Factory pro**. Další informace o zabezpečení Azure Storage, najdete v části [Přehled zabezpečení služby Azure Storage](../security/security-storage-overview.md).

### <a name="data-encryption-in-transit"></a>Šifrování dat při přenosu
Pokud hello cloudu data store podporuje protokol HTTPS nebo TLS, všech dat přenesených mezi služby pro přesun dat v datové továrně a cloudové úložiště dat jsou prostřednictvím zabezpečeného kanálu HTTPS nebo TLS.

> [!NOTE]
> Všechna připojení příliš**Azure SQL Database** a **Azure SQL Data Warehouse** vždy vyžadovat šifrování (SSL/TLS) se při přenosu tooand z databáze hello data. Při vytváření kanálu pomocí editoru JSON, přidejte hello **šifrování** vlastnost a nastavte ji příliš**true** v hello **připojovací řetězec**. Při použití hello [Průvodce kopírováním](data-factory-azure-copy-wizard.md), tato vlastnost nastaví hello Průvodce ve výchozím nastavení. Pro **Azure Storage**, můžete použít **HTTPS** hello připojovacího řetězce.

### <a name="data-encryption-at-rest"></a>Šifrování v klidovém stavu
Některá data ukládá podpora šifrování dat v klidovém stavu. Doporučujeme, abyste povolili mechanismus šifrování dat pro tyto datové úložiště. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transparentní dat šifrování (TDE) v Azure SQL Data Warehouse pomáhá s provedením v reálném čase šifrování a dešifrování dat v klidovém stavu ochrany proti ohrožení hello škodlivých aktivit. Toto chování je transparentní toohello klienta. Další informace najdete v tématu [zabezpečení databáze v SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL Database podporuje také transparentní šifrování dat (šifrování TDE), která pomáhá s ochrany proti ohrožení hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování dat hello bez nutnosti změny toohello aplikace. Toto chování je transparentní toohello klienta. Další informace najdete v tématu [transparentní šifrování dat s Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store taky zajišťuje šifrování dat uložených v účtu hello. Když je povolené, úložiště Data Lake store automaticky šifruje data před uložením a dešifruje před načtení, takže je transparentní toohello klientský přístup k datům hello. Další informace najdete v tématu [zabezpečení v Azure Data Lake Store](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Storage a Azure Table Storage
Úložiště Azure Blob Storage a Azure Table podporuje šifrování služby úložiště (SSE), který automaticky šifruje data, než zachování toostorage a dešifruje před načtení. Další informace najdete v tématu [šifrování služby úložiště Azure pro Data v klidovém stavu](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 podporuje klient i server šifrování dat v klidovém stavu. Další informace najdete v tématu [ochranu šifrování dat pomocí](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Objekt pro vytváření dat v současné době nepodporuje Amazon S3 uvnitř virtuální privátní cloud (VPC).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift podporuje šifrování clusteru pro neaktivní uložená data. Další informace najdete v tématu [šifrování databáze Redshift Amazon](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Objekt pro vytváření dat v současné době nepodporuje Amazon Redshift uvnitř VPC. 

#### <a name="salesforce"></a>Salesforce
Salesforce podporuje šifrování štítu platformy, která umožňuje šifrování všech souborů, přílohy, vlastní pole. Další informace najdete v tématu [Principy hello tok ověřování OAuth serveru webové](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-data-management-gateway"></a>Hybridní scénáře (s použitím brány pro správu dat)
Hybridní scénáře vyžadují toobe Brána pro správu dat, které jsou nainstalované v místní síti nebo uvnitř virtuální sítě (Azure) nebo virtuální privátní cloud (Amazon). Hello brány musí být schopný tooaccess hello místní datová úložiště. Další informace o bráně hello najdete v tématu [Brána pro správu dat](data-factory-data-management-gateway.md). 

![Datové kanály Brána pro správu](media/data-factory-data-movement-security-considerations/data-management-gateway-channels.png)

Hello **příkaz kanál** umožňuje komunikaci mezi služby pro přesun dat v datové továrně a Data Management Gateway. komunikace Hello obsahuje informace související s toohello aktivity. Hello datový kanál se používá pro přenos dat mezi místním úložištím dat a úložiště dat v cloudu.    

### <a name="on-premises-data-store-credentials"></a>Místní přihlašovací údaje k úložišti dat
Hello přihlašovací údaje pro vaše místní úložiště dat jsou uloženy místně (není v cloudu hello). Může být nastavena v třemi různými způsoby. 

- Pomocí **prostého textu** (méně bezpečné) přes HTTPS z portálu Azure nebo Průvodce kopírováním. přihlašovací údaje Hello předávána ve formátu prostého textu toohello místní brána.
- Pomocí **knihovny JavaScript kryptografie z Průvodce kopírováním**.
- Pomocí **klikněte na tlačítko-po aplikaci přihlašovací údaje správce na základě**. Klikněte na tlačítko Hello-Jakmile se aplikace spustí na hello na místním počítači, který má přístup toohello brány a nastaví pověření pro úložiště dat hello. Tuto možnost a hello další jeden jsou hello nejbezpečnější možnosti. Hello aplikaci správce přihlašovacích údajů, ve výchozím nastavení, používá hello port 8050 na hello počítači s bránou pro zabezpečenou komunikaci.  
- Použití [New-AzureRmDataFactoryEncryptValue](/powershell/module/azurerm.datafactories/New-AzureRmDataFactoryEncryptValue) pověření tooencrypt rutiny prostředí PowerShell. rutiny Hello používá hello certifikátu, že brána je nakonfigurovaná toouse tooencrypt hello pověření. Můžete použít přihlašovací údaje hello šifrované vrácená touto rutinou a přidat jej příliš**EncryptedCredential** element hello **connectionString** v souboru hello JSON, který používáte s hello [ Nový-AzureRmDataFactoryLinkedService](/powershell/module/azurerm.datafactories/new-azurermdatafactorylinkedservice) rutiny nebo ve fragmentu kódu JSON hello v hello editoru služby Data Factory hello portálu. Tuto možnost a hello kliknutím-Jakmile aplikace jsou nejbezpečnější možnosti, hello. 

#### <a name="javascript-cryptography-library-based-encryption"></a>Šifrování na základě knihovny JavaScript kryptografie
Můžete šifrovat přihlašovací údaje k úložišti dat pomocí [knihovny JavaScript kryptografie](https://www.microsoft.com/download/details.aspx?id=52439) z hello [Průvodce kopírováním](data-factory-copy-wizard.md). Když vyberete tuto možnost, hello Průvodce kopírováním načte hello veřejný klíč brány a používá je tooencrypt údaje k úložišti dat. hello. pověření musí být Hello dešifrovat počítač brány hello a chráněn systémem Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx).

**Podporované prohlížeče:** IE8, Internet Explorer 9, IE10, IE11, Microsoft Edge a nejnovější Firefox, Chrome, Opera prohlížeče Safari. 

#### <a name="click-once-credentials-manager-app"></a>Klikněte na tlačítko-jednou aplikaci přihlašovací údaje správce
Můžete spustit kliknutím hello-po na základě přihlašovacích údajů správce aplikace z Azure portal nebo zkopírování Průvodce při vytváření kanálů. Tuto aplikaci zajistí, že přihlašovací údaje nejsou přenosu v prostém textu hello přenosu. Ve výchozím nastavení, používá hello port **8050** na hello počítači s bránou pro zabezpečenou komunikaci. V případě potřeby můžete změnit tento port.  
  
![Port HTTPS pro brány hello](media/data-factory-data-movement-security-considerations/https-port-for-gateway.png)

Brána pro správu dat v současné době používá jeden **certifikát**. Tento certifikát je vytvořen během instalace brány hello (platí tooData Brána pro správu vytvořili po listopadu 2016 a verze 2.4.xxxx.x nebo novější). Tento certifikát můžete nahradit vlastním certifikátem SSL/TLS. Tento certifikát se používá kliknutím hello-po toosecurely aplikace Správce přihlašovacích údajů připojení počítače brány toohello pro nastavení přihlašovací údaje k úložišti dat. Ukládá data přihlašovací údaje k úložišti bezpečně místně pomocí hello Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) na hello počítači s bránou. 

> [!NOTE]
> Starší brány, které byly nainstalovány před listopadu 2016 nebo verze 2.3.xxxx.x pokračovat toouse pověření zašifrovány a uloženy v cloudu. I v případě upgradu nejnovější verze toohello hello brány hello pověření nejsou migrované tooan na místním počítači    
  
| Verze brány (během vytváření) | Přihlašovací údaje uložené | Přihlašovací údaje, šifrování nebo zabezpečení | 
| --------------------------------- | ------------------ | --------- |  
| < = 2.3.xxxx.x | V cloudu | Šifrovat pomocí certifikátu (jiný než hello používá aplikaci správce přihlašovacích údajů) | 
| > = 2.4.xxxx.x | Místní | Zabezpečené pomocí rozhraní DPAPI | 
  

### <a name="encryption-in-transit"></a>Šifrování během přenosu
Všechny přenosy dat jsou prostřednictvím zabezpečeného kanálu **HTTPS** a **TLS přes protokol TCP** útokům man-in-the-middle tooprevent při komunikaci se službami Azure.
 
Můžete také použít [IPSec pro síť VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) nebo [Express Route](../expressroute/expressroute-introduction.md) toofurther hello zabezpečený komunikační kanál mezi místní sítí a Azure.

Virtuální síť se znázornění logické sítě v cloudu hello. Je možné připojit místní síti tooyour virtuální sítě Azure (VNet) s nastavením IPSec pro síť VPN (site-to-site) nebo Express Route (soukromý partnerský vztah)       

Hello následující tabulka shrnuje hello sítě a brány konfigurace doporučení na základě různé kombinace zdrojového a cílového umístění pro přesun dat hybridní.

| Zdroj | Cíl | Konfigurace sítě | Instalační program brány |
| ------ | ----------- | --------------------- | ------------- | 
| Lokálně | Virtuální počítače a cloudové služby nasazené ve virtuálních sítích | IPSec pro síť VPN (point-to-site nebo site-to-site) | Brány lze nainstalovat místně nebo v Azure virtuální počítač (VM) ve virtuální síti | 
| Lokálně | Virtuální počítače a cloudové služby nasazené ve virtuálních sítích | ExpressRoute (soukromý partnerský vztah) | Brána může být nainstalované místně nebo na virtuální počítač Azure ve virtuální síti | 
| Lokálně | Služeb založených na Azure, které mají veřejný koncový bod | ExpressRoute (veřejný partnerský vztah) | Brány musí být nainstalované na místě | 

Hello následující obrázky znázorňují použití hello Brána pro správu dat pro přesun dat mezi místní databázi a službami Azure pomocí expressroute a VPN IPSec (s virtuální sítě):

**Express Route:**
 
![Použití Express Route s bránou](media/data-factory-data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec pro síť VPN:**

![IPSec pro síť VPN s bránou](media/data-factory-data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a>Konfigurace bran firewall a vytvoření seznamu povolených IP adresu brány

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Požadavky na brány firewall pro ve místní nebo soukromé síti  
V podniku **podniková brána firewall** běží na hello centrální směrovač hello organizace. A, **brány Windows firewall** spouští se jako démon na hello místního počítače, na které hello brána je nainstalovaná. 

Hello následující tabulka obsahuje **odchozí port** a požadavky domény pro hello **podniková brána firewall**.

| Názvy domén | Odchozí porty | Popis |
| ------------ | -------------- | ----------- | 
| `*.servicebus.windows.net` | 443, 80 | Vyžaduje služby pro přesun toodata tooconnect brány hello v datové továrně |
| `*.core.windows.net` | 443 | Používá hello brány tooconnect tooAzure účet úložiště při použití hello [připravený kopie](data-factory-copy-activity-performance.md#staged-copy) funkce. | 
| `*.frontend.clouddatahub.net` | 443 | Vyžaduje hello brány tooconnect toohello služby Azure Data Factory. | 
| `*.database.windows.net` | 1433   | (Volitelné) je nutné zadat cíl je Azure SQL Database / Azure SQL datového skladu. Použití hello připravený kopie funkce toocopy data tooAzure SQL databáze nebo Azure SQL Data Warehouse bez otevření hello port 1433. | 
| `*.azuredatalakestore.net` | 443 | (Volitelné) je nutné zadat cíl je Azure Data Lake store | 

> [!NOTE] 
> Mohou mít porty toomanage / vytvoření seznamu povolených domén v hello úroveň podniková brána firewall podle potřeby podle příslušné datové zdroje. Tato tabulka pouze používá databázi SQL Azure, Azure SQL Data Warehouse, Azure Data Lake Store jako příklady.   

Hello následující tabulka obsahuje **příchozí port** požadavky pro hello **brány windows firewall**.

| Příchozí porty | Popis | 
| ------------- | ----------- | 
| 8050 (TCP) | Vyžaduje hello přihlašovacích údajů správce aplikací toosecurely nastavení přihlašovacích údajů pro místní úložiště dat v bráně hello. | 

![Požadavky na porty brány](media\data-factory-data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-whitelisting-in-data-store"></a>Konfigurace protokolu IP / vytvoření seznamu povolených v datech úložiště
Některé datové úložiště v cloudu hello také vyžadují povolených IP adresu počítače hello k nim přistupovat. Ujistěte se, že hello IP adresu počítače brány hello je seznam povolených adres a správně nakonfigurována v bráně firewall.

Hello následující cloudové úložiště dat vyžaduje povolených IP adresu počítače brány hello. Některé z těchto dat úložiště, ve výchozím nastavení, ale nemusí vyžadovat vytvoření seznamu povolených hello IP adresy. 

- [Azure SQL Database](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md#create-a-server-level-firewall-rule-in-the-azure-portal)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../documentdb/documentdb-firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Otázka:** hello brány v lze sdílet různé datové továrny?
**Odpověď:** tato funkce ještě nepodporujeme. Aktivně pracujeme na něm.

**Otázka:** jaké jsou požadavky port hello hello brány toowork?
**Odpověď:** brány umožňuje připojení pomocí protokolu HTTP tooopen Internetu. Hello **Odchozí porty 443 a 80** musí být otevřen pro brány toomake toto připojení. Otevřete **příchozí Port 8050** jenom na úrovni počítače hello (ne na úrovni podniková brána firewall) pro aplikaci správce přihlašovacích údajů. Pokud se databáze SQL Azure nebo Azure SQL Data Warehouse používá jako zdroj / cíl a pak musí tooopen **1433** také portu. Další informace najdete v tématu [konfigurace a vytvoření seznamu povolených IP adresy brány Firewall](#firewall-configurations-and-whitelisting-ip-address-of gateway) části. 

**Otázka:** jaké jsou požadavky na certifikát pro bránu?
**Odpověď:** certifikát, který se používá v aplikaci správce přihlašovacích údajů hello bezpečně nastavit přihlašovací údaje k úložišti dat vyžaduje aktuální brány. Tento certifikát je certifikát podepsaný svým držitelem a konfigurují instalací hello brány. Můžete použít vlastní protokol TLS / SSL certifikát místo. Další informace najdete v tématu [klikněte na tlačítko-jednou přihlašovacích údajů správce aplikací](#click-once-credentials-manager-app) části. 

## <a name="next-steps"></a>Další kroky
Informace o výkonu aktivitě kopírování najdete v tématu [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md).

 
