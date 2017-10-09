---
title: "zabezpečení aaaDatabase - Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak Azure Cosmos DB poskytuje databáze ochrany a data zabezpečení pro vaše data."
keywords: "databáze nosql zabezpečení, informace o zabezpečení, zabezpečení dat, šifrování databáze, ochrana databáze, zásady zabezpečení, zabezpečení testování"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: a02a6a82-3baf-405c-9355-7a00aaa1a816
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: mimig
ms.openlocfilehash: 85ffa62f611bfad00bf3fc5dbe536f91f97f1113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-database-security"></a>Zabezpečení databáze Azure Cosmos DB

Tento článek popisuje osvědčené postupy zabezpečení databáze a klíčové funkce nabízené sítěmi Azure Cosmos DB toohelp zabránit, zjistit a reagovat toodatabase narušení.
 
## <a name="whats-new-in-azure-cosmos-db-security"></a>Co je nového v Azure Cosmos DB zabezpečení?

Šifrování v klidovém stavu je nyní k dispozici pro dokumenty a záloh uložených v Azure DB Cosmos ve všech oblastech Azure. Šifrování v klidovém stavu se automaticky použije nové i stávající zákazníků v těchto oblastech. Neexistuje žádné tooconfigure nutné nic; a získáte hello stejnou skvělé latenci, propustnost, dostupnost a funkce jako před s výhodou hello zároveň budete vědět, vaše data jsou bezpečné a zabezpečení pomocí šifrování v klidovém stavu.

## <a name="how-do-i-secure-my-database"></a>Jak zabezpečit databáze? 

Zabezpečení dat je sdílený odpovědnost mezi vy, hello zákazníka a váš poskytovatel databáze. V závislosti na hello poskytovatel databáze, který zvolíte se může lišit hello množství odpovědnosti, kterou jste provádění. Pokud si zvolíte možnost v případě místních řešení, je třeba tooprovide všechno koncový bod ochrany toophysical zabezpečení hardwaru – tzn. bez snadno úlohy. Pokud si zvolíte poskytovatele databáze PaaS cloudu například Azure Cosmos DB, zmenší se výrazně vaší oblasti zájmu. Následující obrázek, vždy pouze vypůjčí na společnosti Microsoft Hello [sdílené odpovědnosti pro Cloud Computing](https://aka.ms/sharedresponsibility) dokumentu white paper, ukazuje, jak vaše odpovědnosti snižuje s poskytovatelem PaaS jako Azure Cosmos DB.

![Odpovědnosti zákazníka a databáze zprostředkovatele](./media/database-security/nosql-database-security-responsibilities.png)

Hello předchozí diagram ukazuje vysoké úrovni cloudu zabezpečení součásti, ale položky, které potřebujete tooworry o speciálně pro vaše řešení databáze? A jak můžeme srovnávat řešení tooeach další? 

Doporučujeme hello následující kontrolní seznam požadavků na které toocompare databáze systémy:

- Zabezpečení sítě a nastavení brány firewall
- Ověření uživatele a podrobné uživatelské ovládací prvky
- Možnost tooreplicate data globálně pro místní selhání
- Možnost tooperform převzetí z jednoho datového centra tooanother
- Místní data replikace v rámci datového centra
- Zálohování dat
- Obnovení odstraněná data ze zálohy
- Chránit a izolovat citlivá data
- Monitorování útokům.
- Odpovídá tooattacks
- Možnost toogeo ochranná data tooadhere toodata zásad správného řízení omezení
- Fyzická ochrana servery v centrech chráněných dat.

A i když to nemusí připadat zřejmé, poslední [rozsáhlé databáze narušení](http://thehackernews.com/2017/01/mongodb-database-security.html) připomenout nám hello jednoduchý, ale kritické důležitosti hello následující požadavky:
- Opravenou servery, které jsou používány maximálně toodate
- HTTPS šifrováním výchozí/SSL
- Účty pro správu s silná hesla

## <a name="how-does-azure-cosmos-db-secure-my-database"></a>Jak Azure Cosmos DB zabezpečit databáze?

Podívejme se zpět na hello předcházející seznamu – kolik tyto požadavky na zabezpečení Azure Cosmos DB poskytuje? Každý jeden jedné.

Pojďme proniknout do každé z nich podrobně.

|Požadavek na zabezpečení|Způsob zabezpečení Azure Cosmos DB|
|---|---|---|
|Zabezpečení sítě|Použití IP brány firewall je hello první vrstvu ochrany toosecure vaší databáze. Azure Cosmos DB podporuje zásady řízené řízení přístupu na základě IP pro podporu brány firewall pro příchozí. řízení přístupu na základě IP Hello jsou podobné pravidla brány firewall toohello používané systémy tradiční databáze, ale jejich jsou rozšířit tak, aby účet Azure Cosmos DB databáze je k dispozici pouze z schválené sadu počítačů nebo cloudových služeb. <br><br>Azure Cosmos DB umožňuje vám tooenable konkrétní IP adresu (168.61.48.0), rozsah adres IP (168.61.48.0/8) a kombinace IP adresy a rozsahy adres. <br><br>Všechny požadavky z počítače mimo tento seznam povolených jsou blokována Azure Cosmos DB. Schválení žádosti z počítače a cloudové služby musí dokončete toobe proces ověřování hello zadané prostředky toohello řízení přístupu.<br><br>Další informace v [podporu brány firewall pro Azure Cosmos DB](firewall-support.md).|
|Autorizace|Azure Cosmos DB používá na základě hodnoty hash message authentication code (HMAC) pro ověřování. <br><br>Každý požadavek je zakódována pomocí hello účtu tajný klíč a hello následné kódování base-64 kódovaného hash je odeslána s každou volání tooAzure Cosmos DB. toovalidate hello žádost, používá služba Azure Cosmos DB hello hello správné tajný klíč a vlastnosti toogenerate hodnota hash a potom porovná hello hodnotu s hello, jeden v žádosti o hello. Pokud odpovídají hello dvě hodnoty, operace hello oprávněný úspěšně a zpracování žádosti hello, jinak dojde selhání autorizace a hello požadavek byl odmítnut.<br><br>Můžete použít buď [hlavní klíč](secure-access-to-data.md#master-keys), nebo [token prostředku](secure-access-to-data.md#resource-tokens) povolení podrobných přístup tooa prostředek například dokument.<br><br>Další informace v [zabezpečení přístupu k prostředkům Cosmos DB tooAzure](secure-access-to-data.md).|
|Uživatele a oprávnění|Pomocí hello [hlavní klíč](#master-key) pro hello účet, můžete vytvořit uživatele a oprávnění prostředků na databázi. A [token prostředku](#resource-token) souvisí s oprávnění v databázi a určuje, zda má uživatel hello přístup (pro čtení a zápis, jen pro čtení, nebo žádný přístup) tooan prostředků aplikace v databázi hello. Prostředky aplikace obsahují kolekcí, dokumentů, přílohy, uložené procedury, triggery a UDF. token prostředku Hello je pak používají při ověřování tooprovide nebo odepřít přístup toohello prostředků.<br><br>Další informace v [zabezpečení přístupu k prostředkům Cosmos DB tooAzure](secure-access-to-data.md).|
|Integrace služby Active directory (RBAC)| Účet databáze toohello přístupu pomocí řízení přístupu (IAM) v hello portál Azure, můžete zadat taky, jak je znázorněno na snímku obrazovky hello pod touto tabulkou. IAM poskytuje řízení přístupu na základě rolí a integruje se službou Active Directory. Můžete použít integrované role nebo vlastní role pro jednotlivce a skupiny, jak ukazuje následující obrázek hello.|
|Globální replikace|Azure Cosmos DB nabízí to globální distribuční, což vám umožní tooreplicate vaše data tooany jedním datových center Azure na celém světě s hello kliknutím tlačítko. Globální replikace umožňuje globálně škálovat a poskytují přístup s nízkou latencí tooyour data kolem hello, world.<br><br>V kontextu hello zabezpečení globální replikace tomu se budou data ochrany proti selhání místní.<br><br>Další informace v [distribuci dat globálně](distribute-data-globally.md).|
|Místní převzetí služeb při selhání|Pokud mají replikovat data do více než jednoho datového centra, Azure Cosmos DB automatické navyšování vaše operace by měla místního datového centra přechodu do offline režimu. Můžete vytvořit seznam oblastí převzetí služeb při selhání pomocí hello oblasti, ve kterých vaše data se replikují seřazený podle priority. <br><br>Další informace v [regionální převzetí služeb při selhání v Azure Cosmos DB](regional-failover.md).|
|Místní replikaci|I v rámci jednoho datového centra Azure Cosmos DB automaticky replikuje data pro vysokou dostupnost poskytnutí hello volbu [úrovně konzistence](consistency-levels.md). Zaručí se tím [99,99 % dostupnost smlouva SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) a dodává se s finanční jistotu - něco může poskytnout žádnou jinou službu databáze.|
|Automatizované zálohování online|Azure Cosmos DB databáze jsou pravidelně zálohovány a uloženy v úložišti georedundant. <br><br>Další informace v [automatické online zálohování a obnovení Azure Cosmos DB](online-backup-and-restore.md).|
|Obnovení odstraněných dat|Hello automatizované zálohování online může být dat použité toorecover jste omylem odstranili až příliš ~ 30 dní po hello události. <br><br>Další informace v [automatické online zálohování a obnovení databáze Cosmos Azure](online-backup-and-restore.md)|
|Chránit a izolovat citlivá data|Všechna data v oblastech hello uvedené v [co je nového?](#whats-new) je nyní v zašifrované podobě.<br><br>PII a jiné důvěrné údaje mohou být izolované toospecific kolekce a čtení a zápis, nebo jen pro čtení může být omezené toospecific uživatele.|
|Monitorování útokům.|Pomocí protokolování auditu a protokolů činnosti, můžete monitorovat váš účet pro běžné a neobvyklé aktivity. Můžete zobrazit, jaké operace provedených na vaše prostředky, kteří vyvoláno hello operaci hello operace došlo k chybě, stav hello hello operace a víc jak je uvedené v hello snímek za touto tabulkou.|
|Odpověď tooattacks|Jakmile máte kontaktovat podporu Azure tooreport potenciální útoky, proces reakcí na incidenty krok 5 je spuštěna.. Hello cílem 5 krocích hello je zabezpečení toorestore normální služby a operace co nejdříve po došlo k potížím se zamykáním a jestli je spuštěná šetření.<br><br>Další informace v [odpověď zabezpečení společnosti Microsoft Azure v cloudu hello](https://aka.ms/securityresponsepaper).|
|Geografického vymezení|Azure Cosmos DB zajišťuje řízení dat a dodržování předpisů pro svrchovaných oblasti (například Německo, Čína, nám verze pro státní správu).|
|Chráněné pracoviště|Data v Azure Cosmos DB se ukládají na jednotkách SSD v chráněných datových centrech Azure.<br><br>Další informace v [globálních datových centrech společnosti Microsoft](https://www.microsoft.com/en-us/cloud-platform/global-datacenters)|
|Šifrování protokolu HTTPS, SSL/TLS|Všechny interakce klienta služby Azure Cosmos DB jsou vynucené SSL/TLS 1.2. Všechny uvnitř datového centra a mezi replikace datacenter je také SSL/TLS 1.2 vynucené.|
|Šifrování v klidovém stavu|Všechna data uložena do Azure Cosmos DB zašifrovaná přinejmenším. Další informace v [Azure Cosmos DB šifrování v klidovém stavu](.\database-encryption-at-rest.md)|
|Opravenou servery|Azure Cosmos DB jako spravované databáze eliminuje hello nutné toomanage a oprava serverů, které bylo dokončeno, automaticky.|
|Účty pro správu s silná hesla|Jeho pevný toobelieve, které jsme i potřebovat toomention tento požadavek, ale na rozdíl od některých naše konkurenci, je možné toohave účtu správce bez hesla v Azure Cosmos DB.<br><br> Zabezpečení prostřednictvím protokolu SSL a HMAC tajný ověřování založené na certifikaci je zaručená v ve výchozím nastavení.|
|Zabezpečení a data protection certifikace|Má Azure Cosmos DB [ISO 27001](https://www.microsoft.com/en-us/TrustCenter/Compliance/ISO-IEC-27001), [klauzule Evropského modelu (VVEU)](https://www.microsoft.com/en-us/TrustCenter/Compliance/EU-Model-Clauses), a [HIPAA](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) certifikace. Další certifikáty jsou v průběhu.|

Hello následující snímek obrazovky ukazuje integrace služby Active directory (RBAC) pomocí řízení přístupu (IAM) v hello portálu Azure: ![řízení (IAM) přístupu v hello portál Azure – ukázka zabezpečení databáze](./media/database-security/nosql-database-security-identity-access-management-iam-rbac.png)

Hello následující snímek obrazovky ukazuje, jak můžete pomocí protokolování auditu a aktivity protokoly toomonitor váš účet: ![aktivity protokoly pro Azure Cosmos DB](./media/database-security/nosql-database-security-application-logging.png)

## <a name="next-steps"></a>Další kroky

Další podrobnosti o hlavních klíčů a tokeny prostředků najdete v tématu [tooAzure přístup k zabezpečení dat databáze Cosmos](secure-access-to-data.md).

Další podrobnosti o certifikace společnosti Microsoft najdete v tématu [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/).
