---
title: "aaaSecuring PaaS aplikace pomocí Azure Storage | Microsoft Docs"
description: " Další informace o zabezpečení Azure Storage osvědčené postupy pro zabezpečení vašich PaaS webové a mobilní aplikace. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: TomShinder
ms.openlocfilehash: 3fed75cb121e7f32eb8b948ee12ca35fb25eca7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-storage"></a>Zabezpečení PaaS webové a mobilní aplikace pomocí Azure Storage
V tomto článku probereme kolekce Azure Storage osvědčené postupy zabezpečení pro zabezpečení vašich PaaS webové a mobilní aplikace. Tyto doporučené postupy jsou odvozeny od našich zkušeností s Azure a hello prostředí zákazníků, jako sami.

Hello [Průvodce zabezpečením Azure Storage](../storage/common/storage-security-guide.md) je skvělým zdrojem pro podrobné informace o Azure Storage a zabezpečení.  Tento článek řeší na vysoké úrovni některé koncepty hello najít v Průvodci zabezpečením hello a Průvodce zabezpečením toohello odkazy, jakož i jiných zdrojů, další informace.

## <a name="azure-storage"></a>Azure Storage
Azure je možné toodeploy a použití úložiště způsoby snadno dosažitelné místní. S Azure storage dosáhnout vysoké úrovně, škálovatelnost a dostupnost s relativně malým množstvím úsilí. Ne jenom je foundation hello úložiště Azure pro Windows a Linux Azure virtuální počítače, může také podporovat velkých distribuovaných aplikací.

Azure storage poskytuje hello následující čtyři služby: objekt Blob úložiště, úložiště Table, Queue storage a úložiště File. Další, najdete v části toolearn [tooMicrosoft Úvod Azure Storage](../storage/storage-introduction.md).

## <a name="best-practices"></a>Osvědčené postupy
Tento článek řeší hello následující osvědčené postupy:

- NAP:
   - Sdílené přístupové podpisy (SAS)
   - Spravovaný disk
   - Řízení přístupu na základě role (RBAC)

- Šifrování úložiště:
   - Šifrování na straně klienta pro cenných dat
   - Azure Disk Encryption pro virtuální počítače (VM)
   - Storage Service Encryption

## <a name="access-protection"></a>NAP
### <a name="use-shared-access-signature-instead-of-a-storage-account-key"></a>Použití sdíleného přístupového podpisu místo klíče účtu úložiště.

V řešení IaaS, obvykle systémem Windows Server nebo Linux virtuální počítače chráněné soubory z zpřístupnění a zneužitím ohrožení pomocí mechanismy řízení přístupu. V systému Windows byste použili [seznamy řízení (ACL) přístupu](../virtual-network/virtual-networks-acl.md) a v systému Linux, pravděpodobně použijete [chmod](https://en.wikipedia.org/wiki/Chmod). V podstatě jde přesně jaká by provést, pokud byly dnes Ochrana souborů na server v datovém centru.

PaaS se liší. Jeden z hello nejběžnější způsoby toostore souborů v Microsoft Azure je toouse [úložiště objektů Azure Blob](../storage/storage-dotnet-how-to-use-blobs.md). Rozdíl mezi úložiště objektů Blob a jiného úložiště souborů je hello souboru vstupně-výstupních operací a hello ochrany metody, které jsou součástí souboru vstupně-výstupní operace.

Řízení přístupu je velmi důležité. můžete řídit přístup k úložišti tooAzure toohelp, hello systém generuje dva klíče účtu úložiště 512 bitů (SAKs) Pokud jste [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md). úroveň Hello klíče redundance umožňuje vám tooavoid služby přerušení během běžné střídání klíče.

Přístupové klíče k úložišti jsou tajné klíče s vysokou prioritou a by měly být jenom přístupné toothose zodpovědná za řízení přístupu úložiště. Pokud hello nikdo získat přístup toothese klíče, můžou bude mít úplnou kontrolu nad úložiště a nahradit, odstranit nebo přidat toostorage soubory. To zahrnuje malwaru a jiné typy obsahu, který může potenciálně ohrozit vaše organizace nebo vašich zákazníků.

Je stále nutné tooobjects způsob tooprovide přístupu v úložišti. Podrobnější tooprovide přístup, můžete využít výhod [sdíleného přístupového podpisu](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). Hello SAS umožňuje vám tooshare určitých objektů v úložišti pro předem definovaný časový interval a se specifickými oprávněními. Podpis sdíleného přístupu vám umožní toodefine:

- interval Hello, přes které hello SAS platný, včetně hello počáteční čas a čas vypršení platnosti hello.
- Hello oprávnění udělují hello SAS. Například SAS na objekt blob může udělit uživatele pro čtení a zápisu objektů blob toothat oprávnění, ale oprávnění k odstranění.
- Volitelné IP adresu nebo rozsah IP adres, ze kterých Azure Storage přijímá hello SAS. Například můžete určit rozsah IP adres, které patří tooyour organizace. To poskytuje jiné míře zabezpečení pro vaše SAS.
- Hello protokol, přes který Azure Storage přijímá hello SAS. Můžete použít tento volitelný parametr tooclients přístup toorestrict pomocí protokolu HTTPS.

SAS vám umožní tooshare obsahu hello způsob tooshare bez udělení rychle klíče účtu úložiště. Vždy pomocí SAS v aplikaci je bezpečný tooshare svým prostředkům úložiště bez kompromisů klíče účtu úložiště.

Další, najdete v části toolearn [pomocí sdílené přístupové podpisy](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). Další informace o potenciální rizika a doporučení toomitigate toolearn těchto rizik, najdete v části [osvědčených postupů při použití SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="use-managed-disks-for-vms"></a>Používat spravované disky pro virtuální počítače

Pokud vyberete [Azure spravované disky](../storage/storage-managed-disks-overview.md), spravuje hello účty úložiště, které používáte pro disky virtuálních počítačů Azure. Stačí toodo je, vyberte typ hello disku (Standard nebo Premium) a velikost disku hello; Úložiště Azure provede hello rest. Nemáte tooworry o limitech škálovatelnosti, které mohou mít jinak vyžadovány účty úložiště toomultiple tooyou.

Další, najdete v části toolearn [– nejčastější dotazy o spravovaných a nespravovaných prémiové disky](../storage/storage-faq-for-disks.md).

### <a name="use-role-based-access-control"></a>Pomocí řízení přístupu na základě rolí

Výše jsme probrali používání sdíleného přístupového podpisu (SAS) toogrant omezený přístup tooobjects vaši klienti tooother účtu úložiště bez vystavení účet klíč účtu úložiště. Někdy hello rizika spojená s konkrétní operaci u vašeho účtu úložiště převáží nad hello výhod SAS. Někdy je jednodušší přístup toomanage jinými způsoby.

Přístup toomanage jiný způsob, jak je toouse [řízení přístupu](../active-directory/role-based-access-control-what-is.md) (RBAC). V RBAC zaměřit se na poskytnutí zaměstnanci hello přesný oprávnění, které potřebují, na základě hello nutné tooknow a principy zabezpečení nejnižší oprávnění. Příliš mnoho oprávnění můžou zpřístupnit tooattackers účtu. Příliš málo oprávnění znamená, že zaměstnanci nelze práci efektivně. AZURE pomůže vyřešit tento problém tak, že nabídka vyladění správy přístupu pro Azure. To je nutné pro organizace, které mají tooenforce zásady zabezpečení pro přístup k datům.

Můžete využít předdefinované role RBAC v Azure tooassign toousers oprávnění. Zvažte použití Přispěvatel účet úložiště pro cloudové operátory, které je třeba toomanage úložiště účty a účty úložiště classic toomanage role Classic Přispěvatel účet úložiště. Operátoři cloudu, které je třeba toomanage virtuálních počítačů, ale není hello virtuální sítě nebo toowhich účet úložiště, které jsou připojeny zvažte jejich přidáním toohello role Přispěvatel virtuálních počítačů.

Organizace, které nebudou vynucovat řízení přístupu dat s využitím funkcí, jako je RBAC může být poskytnutí další oprávnění, než je nezbytné pro své uživatele. To může způsobit ohrožení zabezpečení toodata tím, že některé toodata přístup uživatelů, který by neměl mít na prvním místě hello.

toolearn, které další informace o RBAC najdete v části:

- [Řízení přístupu Azure na základě rolí](../active-directory/role-based-access-control-configure.md)
- [Předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md)
- [Průvodce zabezpečením služby Azure Storage](../storage/common/storage-security-guide.md) pro podrobnosti o tom, jak toosecure úložiště účet s RBAC

## <a name="storage-encryption"></a>Šifrování úložiště
### <a name="use-client-side-encryption-for-high-value-data"></a>Používat šifrování na straně klienta dat vysoké hodnoty

Povoluje šifrování na straně klienta můžete tooprogrammatically šifrování přenášených dat před nahráním tooAzure úložiště a prostřednictvím kódu programu dešifrování dat při načítání z úložiště.  To zajišťuje šifrování dat během přenosu, ale také poskytuje šifrování dat v klidovém stavu.  Šifrování na straně klienta je nejbezpečnější metodou hello šifrování dat, ale vyžadují toomake programové změny tooyour aplikace a put procesy správy klíčů na místě.

Šifrování na straně klienta taky umožňuje toohave výhradní kontrolu nad šifrovacích klíčů.  Můžete vygenerovat a spravovat vlastní šifrovací klíče.  Šifrování na straně klienta používá metodu obálky, kde hello Klientská knihovna pro úložiště Azure vygeneruje obsahu šifrovací klíč (CEK) pak zabalený (šifrované) pomocí hello klíče šifrovacího klíče (KEK). Hello KEK je identifikovaná identifikátorem klíče a může být pár asymetrických klíčů nebo symetrického klíče a mohou být spravovaný místně nebo uloženy v [Azure Key Vault](../key-vault/key-vault-whatis.md).

Šifrování na straně klienta jsou součástí hello Java a knihovny klienta úložiště hello .NET.  V tématu [šifrování na straně klienta a Azure Key Vault pro Microsoft Azure Storage](../storage/storage-client-side-encryption.md) informace o šifrování dat v rámci klientské aplikace a generování a správu šifrovacích klíčů.

### <a name="azure-disk-encryption-for-vms"></a>Azure Disk Encryption pro virtuální počítače
Azure Disk Encryption je funkce, která pomáhá šifrování disky virtuálního počítače s Windows a Linux IaaS. Azure Disk Encryption využívá hello oborový standard BitLocker funkce systému Windows a DM-Crypt funkce hello Linux tooprovide svazku šifrování pro hello operačního systému a datové disky hello. Hello řešení jsou integrované s Azure Key Vault toohelp řídit a spravovat hello šifrování disku klíče a tajné klíče v rámci vašeho předplatného trezoru klíčů. Hello řešení také zajistí, že jsou všechna data na disky virtuálního počítače hello zašifrovaná přinejmenším ve službě Azure storage.

V tématu [Azure Disk Encryption pro systém Windows a virtuálních počítačů Linux IaaS](azure-security-disk-encryption.md).

### <a name="storage-service-encryption"></a>Storage Service Encryption
Když [šifrování služby úložiště](../storage/storage-service-encryption.md) pro úložiště souborů je povoleno, hello data se šifrují automaticky pomocí šifrování AES-256. Microsoft zpracovává všechny hello šifrování, dešifrování a správu klíčů. Tato funkce je dostupná pro typy LRS a GRS redundance.

## <a name="next-steps"></a>Další kroky
Tento článek se zavedl tooa kolekce Azure Storage osvědčené postupy zabezpečení pro zabezpečení vašich PaaS webové a mobilní aplikace. Další informace o zabezpečení vašeho nasazení PaaS toolearn najdete v části:

- [Zabezpečení nasazení PaaS](security-paas-deployments.md)
- [Zabezpečení PaaS webové a mobilní aplikace pomocí Azure App Services](security-paas-applications-using-app-services.md)
- [Zabezpečení databáze PaaS v Azure](security-paas-applications-using-sql.md)
