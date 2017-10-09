---
title: "aaaAzure klíč trezoru – Příručka vývojáře"
description: "Vývojáři mohou pomocí Azure Key Vault toomanage kryptografické klíče v rámci prostředí Microsoft Azure hello."
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 631cea1315964cd0b97e8b2cf3311754230fb801
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-developers-guide"></a>Příručka pro vývojáře Azure Key Vault

Key Vault umožňuje toosecurely přístup k citlivé informace z v rámci aplikace:

- Klíče a tajné klíče jsou chráněné bez nutnosti toowrite hello kód sami a jsou snadno možné toouse je z vašich aplikací.
- Jsou možné toohave vlastní zákazníkům a spravovat vlastní klíče, aby vám umožní soustředit se na poskytnutí hello základní softwarové funkce. Tímto způsobem nebude vlastní aplikace hello odpovědnost nebo potenciální odpovědnosti pro vaše zákazníky klientské klíče a tajné klíče.
- Aplikace může používat klíče pro podepisování a šifrování ještě udržuje správy klíčů hello externí z vaší aplikace. Díky tomu vaše řešení toobe vhodný jako geograficky distribuované aplikace.
- Od září 2016 vydání hello služby Key Vault se vaše aplikace teď můžete použít Key Vault [certifikáty](https://docs.microsoft.com/rest/api/keyvault/certificate-operations). Další informace najdete v tématu [informace o klíčích, tajných klíčů a certifikátů](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates).

Další obecné informace o Azure Key Vault najdete v tématu [co je Key Vault](key-vault-whatis.md).

## <a name="public-previews"></a>Veřejné verze Preview

Pravidelně jsme verze public preview systému novou funkci Key Vault. Zkuste to prosím na tyto a dejte nám vědět, co si myslíte prostřednictvím azurekeyvault@microsoft.com, naše zpětnou vazbu e-mailovou adresu.

### <a name="storage-account-keys---july-10-2017"></a>Klíče účtu úložiště - 10 července 2017

>[!NOTE]
>Pro tuto aktualizaci pouze hello Azure Key Vault **klíče účtu úložiště** funkce je ve verzi preview.

Tato verze preview zahrnuje naše nové klíče účtu úložiště funkce k dispozici prostřednictvím těchto rozhraní; [.NET / C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault/), [REST](https://docs.microsoft.com/rest/api/keyvault/) a [prostředí PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/). 

Další informace o nové funkci klíče účtu úložiště hello najdete v tématu [přehled klíče účtu úložiště Azure Key Vault](key-vault-ovw-storage-keys.md).

## <a name="videos"></a>Videa

Toto video ukazuje, jak toocreate si vlastní klíč trezoru a toouse z hello ukázkovou aplikaci, Key Vault Hello".

- [Vývojáře Key Vault – úvodní příručka](https://channel9.msdn.com/Blogs/Azure/Azure-Key-Vault-Developer-Quick-Start/player)

Prostředky v zmíněné video:

- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure Key Vault ukázkový kód](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

## <a name="creating-and-managing-key-vaults"></a>Vytváření a správa trezorů klíčů

Před zahájením práce s Azure Key Vault ve vašem kódu, můžete vytvořit a spravovat trezory prostřednictvím REST, šablony Resource Manageru, prostředí PowerShell nebo rozhraní příkazového řádku, jak je popsáno v hello následující články:

- [Vytvoření a správa trezorů klíčů s REST](https://docs.microsoft.com/rest/api/keyvault/)
- [Vytvoření a správa trezorů klíčů pomocí prostředí PowerShell](key-vault-get-started.md)
- [Vytvoření a správa trezorů klíčů pomocí rozhraní příkazového řádku](key-vault-manage-with-cli2.md)
- [Vytvoření trezoru klíčů a přidat tajný klíč prostřednictvím šablonu Azure Resource Manager](../azure-resource-manager/resource-manager-template-keyvault.md)

> [!NOTE]
> Operace u trezorů klíčů jsou ověřit prostřednictvím AAD a oprávnění pomocí zásad přístupu Key Vault pro vlastní, definují pro jednotlivé trezoru.

## <a name="coding-with-key-vault"></a>Psaní kódu s použitím trezoru klíčů

Hello systém správy Key Vault pro programátory v jazyce se skládá z několika rozhraní, se zbytkem jako hello foundation. Prostřednictvím rozhraní REST hello všechny vaše prostředky trezorů klíčů jsou přístupné; klíčů, tajných klíčů a certifikátů. [Referenční dokumentace rozhraní API REST trezoru klíčů](https://docs.microsoft.com/rest/api/keyvault/). 

### <a name="supported-programming-languages"></a>Podporované programovací jazyky

#### <a name="net"></a>.NET

- [Referenční .NET API pro Key Vault](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault) 

Další informace o hello verze 2.x hello .NET SDK najdete v tématu hello [poznámky k verzi](key-vault-dotnet2api-release-notes.md).

#### <a name="java"></a>Java

- [Java SDK pro trezor klíčů](https://docs.microsoft.com/java/api/com.microsoft.azure.keyvault)

#### <a name="nodejs"></a>Node.js

V Node.js jsou oddělené trezoru hello rozhraní API pro správu a hello trezoru objekt rozhraní API. Správa trezoru klíčů umožňuje vytvářet a aktualizovat váš trezor klíčů. Klíč trezoru operace rozhraní API je pro práci s objekty trezoru jako; klíčů, tajných klíčů a certifikátů. 

- [Referenční dokumentace rozhraní API Node.js pro správy trezoru klíčů](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest/)
- [Referenční dokumentace rozhraní API Node.js pro klíč trezoru operace](http://azure.github.io/azure-sdk-for-node/azure-keyvault/latest/) 

### <a name="quick-start"></a>Rychlý start

- [Vytvoření trezoru klíčů](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
- [Začínáme s Key Vault v Node.js](https://azure.microsoft.com/en-us/resources/samples/key-vault-node-getting-started/)

### <a name="code-examples"></a>Příklady kódu

Pomocí Key Vault s vašimi aplikacemi dokončení příklady najdete v tématu:

- [Ukázky kódu Azure Key Vault](http://www.microsoft.com/download/details.aspx?id=45343) – ukázková aplikace .NET *HelloKeyVault* a příklad Azure webové služby. 
- [Použití Azure Key Vault z webové aplikace](key-vault-use-from-web-application.md) – kurz toohelp zjistíte, jak toouse Azure Key Vault z webové aplikace v Azure. 

## <a name="how-tos"></a>Postupy

Hello následující články a scénáře obsahují pokyny úlohy specifické pro práci s Azure Key Vault:

- [ID klienta trezoru klíčů změnu po předplatné přesunout](key-vault-subscription-move-fix.md) – když přesunete vašeho předplatného Azure z klienta tootenant B, vaše stávající trezorů klíčů jsou nedostupné objekty hello zabezpečení (uživatele a aplikace) v klientovi B. oprava pomocí tohoto průvodce.
- [Přístup k Key Vault za bránou firewall](key-vault-access-behind-firewall.md) -tooaccess klíč trezoru váš trezor klíčů klienta aplikace potřebám toobe možné tooaccess více koncových bodů pro různé funkce.
- [Jak tooGenerate a Transfer HSM-Protected klíče pro Azure Key Vault](key-vault-hsm-protected-keys.md) – to vám pomůže naplánovat, generovat a potom přeneste vlastní toouse klíčů chráněných pomocí HSM s Azure Key Vault.
- [Jak zabezpečit toopass hodnoty (například hesla) během nasazení](../azure-resource-manager/resource-manager-keyvault-parameter.md) – Pokud je třeba toopass zabezpečenou hodnotu (jako jsou hesla) jako parametr během nasazení, tuto hodnotu můžete uložit jako tajný klíč v hodnotě hello Azure Key Vault a referenční informace v dalších Šablony Resource Manageru.
- [Jak toouse Key Vault pro ekm s SQL serverem](https://msdn.microsoft.com/library/dn198405.aspx) -hello konektor služby serveru SQL pro Azure Key Vault umožňuje systému SQL Server a SQL v VM tooleverage hello Azure Key Vault služby jako zprostředkovatel Extensible Key Management (EKM) tooprotect jeho šifrovací klíče pro odkaz aplikace; Transparentní šifrování dat, zálohování šifrování a šifrování na úrovni sloupce.
- [Jak toodeploy tooVMs certifikáty z Key Vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) – cloudových aplikací spuštěných ve virtuálním počítači na Azure potřebám certifikát. Jak můžete získat tento certifikát do tohoto virtuálního počítače dnes?
- [Jak tooset až Key Vault s end tooend klíčů otočení a auditování](key-vault-key-rotation-log-monitoring.md) – to provede jak tooset střídání klíče a auditování s Azure Key Vault.
- [Nasazení certifikát webové aplikace Azure prostřednictvím Key Vault]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) poskytuje podrobné pokyny pro nasazení certifikátů uložené v Key Vault jako součást [certifikát služby aplikace](https://azure.microsoft.com/blog/internals-of-app-service-certificate/) nabídky.
- [Udělení oprávnění toomany aplikace tooaccess trezoru klíčů](key-vault-group-permissions-for-apps.md) zásad řízení přístupu Key Vault podporuje pouze 16 položky. Ale můžete vytvořit skupiny zabezpečení služby Azure Active Directory. Přidejte všechny hello přidružené skupiny zabezpečení toothis objekty služby, pak udělují přístup toothis zabezpečení skupiny tooKey trezoru.
- Další pokyny specifických úkolů na integraci a trezory klíč pomocí Azure najdete v tématu [příklady šablony Azure Resource Manager Ryan Petr pro Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
- [Jak toouse Key Vault obnovitelného odstranění pomocí rozhraní příkazového řádku](key-vault-soft-delete-cli.md) vás provede hello použití a životního cyklu trezoru klíčů a různých objektů trezor klíčů s povoleno obnovitelného odstranění.
- [Jak toouse Key Vault obnovitelného odstranění pomocí prostředí PowerShell](key-vault-soft-delete-powershell.md) vás provede hello použití a životního cyklu trezoru klíčů a různých objektů trezor klíčů s povoleno obnovitelného odstranění.

## <a name="integrated-with-key-vault"></a>Integrovaná se trezor klíčů

Tyto články jsou o další scénáře a službách, které používají nebo integrovat Key Vault.

- [Azure Disk Encryption](../security/azure-security-disk-encryption.md) využívá hello standardní [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce systému Windows a hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funkce šifrování svazku tooprovide Linux hello operačního systému a dat hello disky. Hello řešení jsou integrované s Azure Key Vault toohelp řídit a spravovat hello disku šifrovacích klíčů a tajných klíčů v trezoru klíčů předplatného, a zajistit, že jsou všechna data v disky virtuálního počítače hello zašifrovaná přinejmenším ve službě Azure storage.
- [Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md) poskytuje možnost pro šifrování dat, který je uložený v účtu hello. Pro správu klíčů Data Lake Store poskytuje dva režimy pro správu hlavní šifrovacích klíčů (MEKs), které jsou požadovány pro dešifrování žádná data, která je uložená v hello Data Lake Store. Můžete je nechat buď Data Lake Store můžete spravovat hello MEKs, nebo zvolte tooretain vlastnictví hello MEKs pomocí účtu Azure Key Vault. Zadáte hello režimu správy klíčů při vytváření účtu Data Lake Store. 
- [Azure Information Protection](/information-protection/plan-design/plan-implement-tenant-key) vám umožní toomanager klíč klienta. Například místo Microsoft správě klíče klienta (hello výchozí nastavení), můžete spravovat vlastní klíč toocomply klienta s konkrétním předpisům platným tooyour organizace. Správa vlastní klíč tenanta se také označují tooas přineste si vlastní klíč neboli BYOK.

## <a name="key-vault-overviews-and-concepts"></a>Přehledy Key Vault a koncepty

- [Chování konfigurace soft odstranění Key Vault](key-vault-ovw-soft-delete.md) popisuje funkce, která umožňuje obnovení odstraněných objektů, zda text hello, co se náhodnému nebo záměrnému.
- [Omezení klienta Key Vault](key-vault-ovw-throttling.md) vás seznámí s toohello základní koncepty omezování a nabízí přístup pro vaši aplikaci.
- [Přehled klíčů účtu úložiště Key Vault](key-vault-ovw-storage-keys.md) popisuje hello Key Vault integrace účtech úložiště Azure klíče.
- [Rozdílné architektury security World Key Vault](key-vault-ovw-security-worlds.md) popisuje hello vztahy mezi regiony a oblasti zabezpečení.

## <a name="social"></a>Sociální

- [Blog trezoru klíčů](http://aka.ms/kvblog)
- [Fórum trezoru klíčů](http://aka.ms/kvforum)

## <a name="supporting-libraries"></a>Podpora knihovny

- [Microsoft Azure Key Vault základní knihovna](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) poskytuje **IKey** a **IKeyResolver** rozhraní pro vyhledání klíče z identifikátory a provádění operací s klíči.
- [Rozšíření Microsoft Azure Key Vault](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) poskytuje rozšířené možnosti pro Azure Key Vault.


