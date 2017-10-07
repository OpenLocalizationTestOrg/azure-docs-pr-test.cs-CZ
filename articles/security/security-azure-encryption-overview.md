---
title: "Přehled šifrování aaaAzure | Microsoft Docs"
description: "Další informace o různých možností šifrování v Azure"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/18/2017
ms.author: barclayn
ms.openlocfilehash: ef9ab46de32b857e99e8fe628a61386b95cf197f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-overview"></a>Přehled Azure šifrování

Tento článek obsahuje přehled použití šifrování v Microsoft Azure. Pokrývá hello hlavními oblastmi šifrování, včetně šifrování v klidovém stavu, šifrování v cestě a správu klíčů s Key Vault. Každá část obsahuje odkazy na podrobnější informace.

## <a name="encryption-of-data-at-rest"></a>Šifrování neaktivních uložených dat

Data v klidovém stavu zahrnují informace, které se nachází v trvalé úložiště na fyzickém médiu v libovolném digitální formátu. To zahrnuje soubory na magnetické nebo optické média, Archivovaná data a data zálohování. Microsoft Azure nabízí celou řadu toomeet řešení úložiště dat různé potřeby, včetně souborů, disku, objektů blob a tabulka úložiště. Společnost Microsoft poskytuje také šifrování tooprotect [Azure SQL Database](../sql-database/sql-database-technical-overview.md), [CosmosDB](../cosmos-db/introduction.md)a Azure Data Lake.

Šifrování dat v klidovém stavu je k dispozici pro služby napříč hello Azure softwaru jako služba (SaaS), platformy jako služba (PaaS) a modely cloudové infrastruktury jako služba (IaaS). Tento dokument shrnuje a poskytuje prostředky toohelp použijete možnosti šifrování Azure.

Podrobné informace o tom, jak je šifrované data umístěná v Azure, naleznete v dokumentu hello s názvem [šifrování na Rest Azure dat](azure-security-encryption-atrest.md)

## <a name="azure-encryption-models"></a>Modely Azure šifrování

Azure podporuje různé modely šifrování, včetně šifrování na straně serveru pomocí službu spravovat klíče, pomocí spravovaného zákazníkem klíčů v Azure Key Vault nebo pomocí klíče spravovaného zákazníkem na hardwaru řízenou zákazníka. Šifrování na straně klienta umožňuje vám toomanage, úložiště klíčů místně nebo v jiném zabezpečené umístění.

### <a name="client-side-encryption"></a>Šifrování na straně klienta

Šifrování na straně klienta se provádí mimo Azure. Šifrování na straně klienta zahrnuje:

- Data šifrovat aplikace, která běží v datovém centru hello zákazníka nebo aplikace služby
- Data, která už je šifrovaný při obdržení Azure.

Pomocí šifrování na straně klienta hello poskytovatele cloudové služby nemá přístup k toohello šifrovacích klíčů a nemůže data dešifrovat. Je-li zachovat úplnou kontrolu nad hello klíče.

### <a name="server-side-encryption"></a>Šifrování na straně serveru

Hello tři modely šifrování na straně serveru nabízejí různé správy klíčů charakteristiky, které může být zvolena podle požadavků.

- **Službu spravovat klíče** poskytovat kombinaci řízení a usnadnění práce s nízkou režii

- **Spravované zákazníkem klíče** udělení řízení nad klíči hello, včetně hello možnost toobring vlastní klíče (BYOK) nebo toogenerate nové.

- **Službu spravovat klíče v ccustomer controlledhardware** vám umožní toomanage klíče ve vaší vlastní úložiště, které je mimo kontrolu společnosti Microsoft. Tomu se říká hostitele si vlastní klíč (HYOK). Však konfigurace je složitá a nejvíce Azure services nepodporují tento model.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Virtuální počítače Windows a Linux se dají chránit pomocí [Azure Disk Encryption](azure-security-disk-encryption.md), hello, který používá [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) technologie a Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) tooprotect disky operačního systému a datové disky s šifrování celého svazku.

Šifrování klíče a tajné klíče jsou chráněné v vaše [Azure Key Vault](../key-vault/key-vault-whatis.md) předplatné. Můžete zálohovat a obnovit šifrované virtuálních počítačů, které jsou zašifrované s konfigurací KEK hello pomocí služby zálohování Azure hello.

### <a name="azure-storage-service-encryption"></a>Azure šifrování služby úložiště

Ve scénářích serverové a klientské se můžou šifrovat data umístěná v úložišti Azure (objektů Blob a souboru).

[Azure šifrování služby úložiště](../storage/storage-service-encryption.md) (SSE) automaticky data můžete šifrovat před uloženo a automaticky je dešifruje při načtení, což hello zpracovat uživatele zcela transparentní. Šifrování služby úložiště používá 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), který je jedním z hello nejsilnější šifry bloku k dispozici a zpracovává šifrování, dešifrování a správu klíčů transparentním způsobem.

### <a name="client-side-encryption-of-azure-blobs"></a>Šifrování na straně klienta objektů BLOB Azure

Šifrování na straně klienta objektů BLOB Azure lze provést různými způsoby.

Hello Klientská knihovna pro úložiště Azure pro data tooencrypt balíčku NuGet pro rozhraní .NET můžete použít v rámci vaší klientské aplikace před toouploading ho tooAzure úložiště.

Další informace o toolearn a stažení hello Klientská knihovna pro úložiště Azure pro balíček NuGet pro rozhraní .NET, naleznete v dokumentu hello s názvem [Windows Azure Storage 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage)

Při použití šifrování na straně klienta s Azure Key Vault, vaše data se šifrují pomocí jednorázové symetrického obsahu šifrovací klíč (CEK) generovaný klienta služby Azure Storage hello SDK. Hello CEK se šifrují pomocí klíč šifrovacích klíčů (KEK), které mohou být buď symetrický klíč nebo pár asymetrických klíčů. Můžete ho spravovat místně nebo uložit do Azure Key Vault. Hello šifrovaná data se pak nahrán tooAzure služby úložiště.

Další informace o šifrování na straně klienta s Azure Key Vault toolearn a začít pracovat s postupy tooinstructions, naleznete v dokumentu hello s názvem [kurz: šifrování a dešifrování objektů BLOB v úložišti Microsoft Azure pomocí Azure Key Vault](../storage/storage-encrypt-decrypt-blobs-key-vault.md)

Nakonec můžete také použít hello Klientská knihovna pro úložiště Azure pro šifrování na straně klienta tooperform Java před nahráním dat tooAzure úložiště a toodecrypt hello dat při stahování ho toohello klienta. Tato knihovna také podporuje integraci s [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) pro správu klíčů účtu úložiště.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Šifrování dat v klidovém stavu s databází Azure SQL

[Azure SQL Database](../sql-database/sql-database-technical-overview.md) je služba pro obecné účely relační databáze v Microsoft Azure, který podporuje struktury například relačních dat, formát JSON, prostorových a XML. Azure SQL podporuje šifrování na straně serveru pomocí funkce hello transparentní šifrování šifrování dat (TDE) i šifrování na straně klienta pomocí funkce Always Encrypted hello.

#### <a name="transparent-data-encryption"></a>Transparentní šifrování dat

[Šifrování TDE transparentní šifrování dat](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) je použité tooencrypt [systému SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL Database](../sql-database/sql-database-technical-overview.md), a [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) datových souborů v reálném čase pomocí databáze šifrovací klíč (DEK), který je uložený v záznamu spouštěcí hello databáze dostupnosti během obnovení.

Šifrování TDE chrání data a soubory protokolu, pomocí algoritmy šifrování AES a 3DES. Šifrování hello soubor databáze se provádí na úrovni stránky hello; Hello stránky v šifrované databáze zašifrována předtím, než jsou zapsané toodisk a jsou dešifrovat, pokud jste načtení do paměti. Ve výchozím nastavení na nově vytvořené databáze Azure SQL je teď povolené šifrování TDE.

#### <a name="always-encrypted"></a>Funkce Always Encrypted

Hello [vždy šifrována](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) funkce v Azure SQL umožňuje tooencrypt dat v rámci klientské aplikace před toostoring ve službě Azure SQL Database a umožňuje vám tooenable delegování místní databáze správy toothird strany a udržovat oddělení od uživatelů, kteří vlastní a můžete zobrazit hello dat a uživatelů, kteří ho spravovat, ale by neměl mít tooit přístup.

#### <a name="cellcolumn-level-encryption"></a>Šifrování na úrovni buněk nebo sloupce

Databáze SQL Azure umožňuje tooapply symetrického šifrování tooa sloupec dat pomocí jazyka Transact-SQL. To se označuje jako [buňky šifrování na úrovni nebo šifrování na úrovni sloupce](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data) (Vymazat), protože můžete ho tooencrypt určité sloupce nebo i konkrétní buněk dat s různými šifrovacími klíči. To vám dává podrobnější možnosti šifrování než šifrování TDE, který šifruje data stránky.

VY obsahuje integrované funkce, které můžete použít tooencrypt dat pomocí buď symetrický, nebo asymetrické klíče s hello veřejný klíč certifikátu, nebo přístupové heslo pomocí 3DES.

### <a name="cosmos-db-database-encryption"></a>Šifrování databáze cosmos DB

[Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) je globálně distribuované a více modelech databáze společnosti Microsoft. Data uloženy v databázi Cosmos ve stálé úložiště (SSD disky) se šifrují ve výchozím nastavení; neexistují žádné ovládací prvky tooturn ho nebo vypnout. Šifrování v klidovém stavu je implementována pomocí několika technologiemi zabezpečení, včetně systémů zabezpečeného úložiště klíčů, šifrované sítí a kryptografické rozhraní API. Šifrovací klíče se spravují přes Microsoft a otáčejí na interní pokyny společnosti Microsoft.

### <a name="at-rest-encryption-in-azure-data-lake"></a>Šifrování na rest v Azure Data Lake

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) je úložiště celopodnikové každý typ dat shromážděných v předchozí tooany jednotné místo formální definici požadavků nebo schéma. Azure Data Lake Store podporuje "na standardně" transparentní šifrování dat v klidovém stavu, která se nastavuje během vytváření hello vašeho účtu. Ve výchozím nastavení, Data Lake Store spravuje hello klíče pro vás, ale máte možnost toomanage hello je sami.

Tři typy klíčů se používají v šifrování a dešifrování dat: hello hlavní šifrovací klíč (MEK), datový šifrovací klíč (DEK) a bloku šifrovací klíč (BEK). Hello MEK je použité tooencrypt hello klíč DEK, který je uložený na trvalé média, a hello BEK je odvozená od hello klíč DEK a hello datového bloku. Pokud spravujete vlastní klíče, je možné otočit hello MEK.

## <a name="encryption-of-data-in-transit"></a>Šifrování dat při přenosu

Azure nabízí mnoho mechanismy pro zachování dat při jejich přesunu z jednoho umístění tooanother privátní.

### <a name="tlsssl-encryption-in-azure"></a>Protokol TLS/SSL šifrování v Azure

Společnost Microsoft používá hello [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) protokolu tooprotect data cestách mezi hello cloudové služby a zákazníků. Datových centrech společnosti Microsoft vyjednávání TLS připojení s klientské systémy, které se připojují tooAzure služby. Protokol TLS poskytuje silné ověřování, ochrana soukromí zpráv a integritu (povolení detekce zpráva manipulaci, zachycení a padělání), vzájemná funkční spolupráce, flexibilita algoritmu, snadné nasazení a používání.

[Ideální Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) je chrání připojení mezi klientské systémy zákazníků a cloudové služby společnosti Microsoft podle jedinečné klíče. Připojení pomocí také délek klíčů pro šifrování RSA podle 2 048 bitů. Tato kombinace znesnadňuje někomu toointercept a přístup data, která je na cestě.

### <a name="azure-storage-transactions"></a>Azure transakcí úložiště

Pokud při práci s Azure Storage prostřednictvím hello portálu Azure, všechny transakce proběhnout přes protokol HTTPS. Můžete taky hello REST API úložiště přes protokol HTTPS toointeract s Azure Storage. Při volání metody hello rozhraní REST API tooaccess objekty v účtech úložiště, povolením bezpečnému přenosu požadované pro účet úložiště hello můžete vynutit hello použití protokolu HTTPS.

Sdílené přístupové podpisy ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), může být použité toodelegate přístup k objektům úložiště tooAzure, zahrnout toospecify možnost této pouze hello protokol HTTPS můžete použít, pokud se používá sdílené přístupové podpisy. Tím se zajistí, že kdokoliv odesílání se propojení s tokeny SAS používá správný protokol hello.

[Protokol SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption) použité tooaccess sdílené složky Azure podporuje šifrování a je k dispozici v systému Windows Server 2012 R2, Windows 8, Windows 8.1 a Windows 10, povolení přístupu mezi oblastmi a dokonce přejít na ploše hello.

Šifrování na straně klienta šifruje hello data před odeslal tooAzure úložiště, tak, aby při přenosu v síti hello je zašifrovaná.

### <a name="smb-encryption-over-azure-virtual-networks"></a>Šifrování paketů SMB přes virtuální sítě Azure 

[Protokol SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) útoky tooprotect proti manipulaci a odposlouchávání ve virtuálních počítačích Azure s Windows serverem 2012 a vyšší poskytuje hello možnost toomake datové přenosy zabezpečení tím, že šifrování dat při přenosu přes virtuálních sítí Azure. Správci mohou povolit šifrování protokolu SMB pro celý server hello nebo jenom určité sdílené složky.

Ve výchozím nastavení Jakmile se pro sdílenou složku nebo server, zapnuté šifrování protokolu SMB jsou pouze klienti SMB 3 povoleny tooaccess hello zašifrované složky.

## <a name="in-transit-encryption-in-azure-virtual-machines"></a>Šifrování během přenosu ve virtuálních počítačích Azure

Data při přenosu do, z a mezi virtuálními počítači Azure s Windows je zašifrovaná různými způsoby v závislosti na povaze hello hello připojení.

### <a name="rdp-sessions"></a>Relace RDP

Můžete se připojit a přihlásit tooan virtuálního počítače Azure pomocí hello [Remote Desktop Protocol](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) (RDP) z klientského počítače Windows nebo z Mac s klientem RDP nainstalována. Data při přenosu přes síť hello v relace RDP může být chráněna TLS.

Můžete také použít vzdálené plochy tooconnect tooa virtuálního počítače s Linuxem v Azure.

### <a name="secure-access-toolinux-vms-with-ssh"></a>Zabezpečení přístupu k tooLinux virtuálních počítačů pomocí protokolu SSH

Můžete použít [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (SSH) tooconnect tooLinux virtuální počítače běžící v Azure pro vzdálenou správu. SSH je protokol šifrované připojení, která umožňuje zabezpečený přihlášení přes nezabezpečený připojení. Je hello výchozí připojení protokolu pro virtuální počítače Linux hostované v Azure. Pomocí klíče SSH pro ověřování eliminovat potřebu hello toolog hesla v. Protokol SSH používá pár veřejného a privátního klíče (asymetrické šifrování) pro ověřování.

## <a name="azure-vpn-encryption"></a>Šifrování Azure VPN

TooAzure se můžete připojit přes virtuální privátní sítě, která vytvoří zabezpečené tunelové propojení tooprotect hello zásady ochrany osobních údajů hello se odesílají přes síť hello.

### <a name="azure-vpn-gateway"></a>Azure VPN Gateway

[Služba Azure VPN gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) lze použít toosend šifrovat provoz mezi virtuální sítí a vaše místní umístění veřejné připojení nebo toosend provoz mezi virtuálními sítěmi.

Site-to-site VPN používá [IPsec](https://en.wikipedia.org/wiki/IPsec) pro šifrování přenosu. Azure VPN Gateway pomocí sady výchozí návrhů. Můžete nakonfigurovat vlastní zásady protokolu IPsec/IKE toouse brány Azure VPN s konkrétní kryptografické algoritmy a klíče síly, místo hello sady Azure výchozí zásady.

### <a name="point-to-site-vpn"></a>Point-to-site VPN

Point-to-Site VPN povolit jednotlivé klientským počítačům přístup k tooan Azure Virtual Network. [Hello Secure Socket Tunneling Protocol](https://technet.microsoft.com/library/2007.06.cableguy.aspx) (SSTP) je tunelového připojení sítě VPN použít toocreate hello a mohou procházet brány firewall (hello tunelové propojení se zobrazí jako připojení HTTPS). Vlastní interní kořenové certifikační Autority infrastruktury veřejných KLÍČŮ můžete použít pro připojení point-to-site.

Můžete nakonfigurovat point-to-site VPN připojení tooa virtuální síť hello portálu Azure pomocí ověření certifikátu nebo prostředí PowerShell.

toolearn Další informace o tooAzure připojení VPN typu point-to-site virtuální sítě, najdete v části: [konfigurace tooa připojení Point-to-Site virtuální sítě pomocí ověřování certifikace: portál Azure](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) a

[Konfigurace tooa připojení Point-to-Site virtuální síť ověřování pomocí certifikátů: prostředí PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpn"></a>Site-to-site VPN 

Připojení k bráně VPN Site-to-Site je použité tooconnect místní sítě tooan virtuální síť Azure přes tunelové propojení VPN protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje sítě VPN zařízení nachází v místě které má zvenčí veřejnou IP adresu přiřazenou tooit.

Můžete nakonfigurovat site-to-site VPN připojení tooa virtuální sítě pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure (CLI).

Přečtěte si tyto další informace:

[Umožňuje vytvořit připojení Site-to-Site v hello portálu Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Umožňuje vytvořit připojení Site-to-Site](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí rozhraní příkazového řádku](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-azure-data-lake"></a>Šifrování během přenosu v Azure Data Lake

Přenášená data se ve službě Data Lake Store také vždy šifrují. Kromě toho tooencrypting data předchozí toostoring toopersistent média, hello je také vždy zabezpečená data při přenosu pomocí protokolu HTTPS. HTTPS je hello pouze protokol, který je podporován pro rozhraní Data Lake Store REST hello.

Další informace o šifrování dat během přenosu v Azure Data Lake toolearn naleznete v dokumentu hello s názvem [šifrování dat v Azure Data Lake Store.](../data-lake-store/data-lake-store-encryption.md)

## <a name="key-management-with-azure-key-vault"></a>Správa klíčů s Azure Key Vault

Bez správné ochrana a Správa klíčů hello je vykreslen šifrování nepoužitelné. Azure Key Vault je doporučené řešení společnosti Microsoft pro správu a řízení přístupu tooencryption klíčů používaných certifikačními cloudové služby. Klíče tooaccess oprávnění lze přiřadit tooservices nebo toousers pomocí účtů služby Azure Active Directory.

Azure Key Vault zbavuje kterou přináší nutnost organizace hello nutné tooconfigure, opravy a udržovat modulů hardwarového zabezpečení (HSM) a software pro správu klíčů. S Azure Key Vault Microsoft nikdy uvidí vaše klíče a aplikace nemají přímý přístup toothem; můžete zachovat kontrolu. Můžete také importovat nebo generovat klíče v modulech Hsm.

## <a name="next-steps"></a>Další kroky

- [Přehled zabezpečení Azure](security-get-started-overview.md)
- [Přehled zabezpečení sítě Azure](security-network-overview.md)
- [Přehled zabezpečení databáze Azure](azure-database-security-overview.md)
- [Přehled zabezpečení virtuální počítače Azure](security-virtual-machines-overview.md)
- [Šifrování dat v klidovém stavu](azure-security-encryption-atrest.md)
- [Osvědčené postupy šifrování a zabezpečení dat](azure-security-data-encryption-best-practices.md)
