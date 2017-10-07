---
title: "aaaAzure šifrování služby úložiště pomocí zákazníka spravovaného klíče v Azure Key Vault | Microsoft Docs"
description: "Použijte hello šifrování služby úložiště Azure funkce tooencrypt službě Azure Blob Storage na straně služby hello při ukládání dat hello a při načítání dat hello pomocí zákazníka spravovaných klíčů ho dešifrovat."
services: storage
documentationcenter: .net
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: lakasa
ms.openlocfilehash: eb2e0ad27df40e61f9c08b9db7ca7a7568adad9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Šifrování služby úložiště pomocí klíčů zákazníků spravované v Azure Key Vault

Microsoft Azure je potvrzená toohelping chránit a chrání vaše data toomeet zabezpečení organizace a dodržování předpisů závazky.  Jedním ze způsobů, budete moci chránit vaše data v klidovém stavu je toouse šifrování služby úložiště (SSE), který automaticky šifruje vaše data při zápisu ho toostorage a při získávání ji dešifruje data. Hello šifrování a dešifrování je automatická, naprosto transparentní a používá 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejsilnějších bloku hello šifer k dispozici.

Spravované společností Microsoft šifrovací klíče můžete použít s SSE nebo můžete použít vlastní šifrovací klíče. Tento článek popisuje hello pozdější. Další informace o používání klíčů spravovaný společností Microsoft nebo o SSE v obecné najdete v tématu [šifrování služby úložiště pro Data v klidovém stavu](../storage-service-encryption.md).

možnost toouse hello tooprovide vlastní šifrovací klíče, SSE pro úložiště objektů Blob je integrovaná se trezor službou klíč Azure (AZURE). Můžete vytvořit vlastní šifrovací klíče a uložit je do službou AZURE, nebo můžete použít službou AZURE na rozhraní API toogenerate šifrovací klíče. Nejen se službou AZURE umožňují toomanage a řídit klíče, taky umožňuje vám tooaudit vaše použití klíče. 

Proč byste se měli toocreate vlastní klíče? Poskytuje větší flexibilitu, umožní vám toocreate, otáčení, zakázat a definovat řízení přístupu. Umožňuje také, že jste tooaudit hello šifrovací klíče použili tooprotect vaše data.

## <a name="sse-with-customer-managed-keys-preview"></a>SSE se customer preview spravovaných klíčů

Tato funkce je aktuálně ve verzi Preview. toouse tuto funkci, budete potřebovat toocreate nový účet úložiště. Můžete buď vytvořit nový trezor klíčů a klíč nebo můžete použít existující trezor klíčů a klíč. účet úložiště Hello a hello trezoru klíčů musí být v hello stejné oblasti, ale může být v různých předplatných.

tooparticipate ve verzi preview hello, obraťte se na [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com). Poskytujeme tooparticipate speciálního odkazu ve verzi preview hello.

toolearn odkazovat více, toohello [– nejčastější dotazy](#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).

> [!IMPORTANT]
> Musíte se přihlásit hello preview předchozí toofollowing hello kroky v tomto článku. Bez přístupu preview, není možné tooenable se tato funkce portálu hello.

## <a name="getting-started"></a>Začínáme
## <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Krok 1: [vytvořit nový účet úložiště](../storage-create-storage-account.md)

## <a name="step-2-enable-encryption"></a>Krok 2: Povolení šifrování
Můžete povolit SSE pro účet úložiště hello pomocí hello [portál Azure](https://portal.azure.com). V okně hello nastavení pro účet úložiště hello jak ukazuje následující obrázek a klikněte na možnost šifrování hello vyhledejte hello části služby objektů Blob.

![Možnost šifrování portálu snímek obrazovky zobrazující](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)
<br/>*Povolit SSE pro služby objektů Blob*

Chcete-li tooprogrammatically povolit nebo zakázat hello šifrování služby úložiště na účet úložiště, můžete použít hello [REST API služby Azure Storage prostředků zprostředkovatele](https://docs.microsoft.com/en-us/rest/api/storagerp/?redirectedfrom=MSDN), hello [knihovny klienta poskytovatele prostředků úložiště pro platformu .NET](https://docs.microsoft.com/en-us/dotnet/api/?redirectedfrom=MSDN), [prostředí Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-4.0.0), nebo hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli).

Na této obrazovce Pokud nevidíte políčko "použití vlastní klíč" hello, můžete nebylo schváleno hello preview. Odeslat e-mail příliš[ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) a požádat o schválení.

![Portál snímek obrazovky zobrazující šifrování Preview](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

Ve výchozím nastavení používá SSE spravovaný Microsoftem klíče. toouse vlastní klíče, zaškrtněte políčko hello. Potom můžete buď zadat klíč identifikátor URI, nebo vybrat klíč a Key Vault z hello výběr.

## <a name="step-3-select-your-key"></a>Krok 3: Vyberte klíč

![Portálu snímek obrazovky zobrazující šifrování použijte možnost vlastní klíče](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

![Portál snímek obrazovky s šifrování s, zadejte možnost klíče uri](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Pokud účet úložiště hello nemá přístup k toohello Key Vault, můžete spustit hello následující příkaz, pomocí prostředí Azure Powershell toogrant toohello úložiště účty pro přístup k požadované toohello trezoru klíčů.

![Portál snímek obrazovky ukazující, přístup k trezoru klíčů odepřen](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Můžete také udělit přístup přes hello portálu Azure tak, že budete toohello Azure Key Vault v hello portál Azure a udělení přístupu k účtu úložiště toohello.

## <a name="step-4-copy-data-toostorage-account"></a>Krok 4: Kopírování dat toostorage účtu
Pokud chcete tootransfer dat do nového účtu úložiště, tak, aby se šifrují, podívejte se příliš[krok 3 z Začínáme v šifrování služby úložiště pro Data v klidovém stavu](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-3-copy-data-to-storage-account).

## <a name="step-5-query-hello-status-of-hello-encrypted-data"></a>Krok 5: Stav hello dotazu hello šifrovaná data
Stav hello tooquery hello šifrovaná data, získáte příliš[krok 4 z Začínáme v šifrování služby úložiště pro Data v klidovém stavu](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-4-query-the-status-of-the-encrypted-data).

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Nejčastější dotazy o šifrování služby úložiště pro Data v klidovém stavu
**Otázka: používám storage úrovně Premium; můžete použít SSE pomocí klíčů zákazníků spravovat?**

Odpověď: Ano SSE s spravovaný společností Microsoft a k zákaznickým spravované klíče je podporována v úložiště úrovně Standard a Premium storage. 

**Otázka: je možné vytvořit nové účty úložiště s SSE pomocí klíčů zákazníků spravované povolit pomocí Azure PowerShell a rozhraní příkazového řádku Azure?**

Odpověď: Ano.

**Otázka: jak mnohem víc náklady na úložiště Azure nepodporuje když SSE s zákazníka spravované klíče je povolen?**

Odpověď: je s náklady související pro použití Azure Key Vault. Další podrobnosti najdete [klíč trezoru ceny](https://azure.microsoft.com/en-us/pricing/details/key-vault/). Není k dispozici pro použití SSE bez dalších nákladů.

**Otázka: je možné odvolat přístup toohello šifrovací klíče?**

A: Ano, můžete kdykoli odvolat přístup. Existuje několik způsobů toorevoke přístup tooyour klíče. Odkazovat příliš[Azure Key Vault PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.0.0) a [příkazového řádku Azure Key Vault](https://docs.microsoft.com/en-us/cli/azure/keyvault) další podrobnosti. Odvolání přístupu efektivně blokuje přístup k objektům BLOB tooall v účtu úložiště hello jako hello účet šifrovací klíč je pravděpodobně nepřístupný úložiště Azure.

**Otázka: je možné vytvořit účet úložiště a klíč v jiné oblasti?**

Odpověď: Ne, hello účet úložiště a klíč nebo klíč trezoru toobe nutné v hello stejné oblasti. 

**Otázka: je možné povolit SSE pomocí klíčů zákazníků spravované při vytváření účtu úložiště hello?**

Odpověď: Ne. Když povolíte SSE při vytváření účtu úložiště hello, můžete použít pouze spravovaný Microsoftem klíče. Pokud chcete toouse zákazníka spravované klíče, je nutné aktualizovat vlastnosti účtu úložiště hello. Můžete použít REST nebo jeden z tooprogrammatically knihovny klienta úložiště hello aktualizovat účet úložiště, nebo aktualizovat vlastnosti účtu úložiště hello pomocí portálu Azure hello po vytvoření účtu hello.

**Otázka: je možné zakázat šifrování, zatímco SSE pomocí zákazníka spravované klíče?**

A: šifrování nelze zakázat Ne, zatímco SSE pomocí zákazníka spravované klíče. toodisable šifrování, je nutné přepnout toousing spravovaný Microsoftem klíče. To provedete pomocí hello portál Azure nebo Powershellu.

**Otázka: je SSE ve výchozím nastavení povolené, po vytvoření nového účtu úložiště?**

Odpověď: SSE není povoleno ve výchozím nastavení; můžete použít hello Azure portálu tooenable ho. Můžete také prostřednictvím kódu programu povolit tuto funkci pomocí hello REST API poskytovatele prostředků úložiště. 

**Otázka: I nelze povolit šifrování na svůj účet úložiště.**

Odpověď: je účet správce prostředků úložiště? Klasické účty úložiště nejsou podporovány. SSE pomocí klíčů zákazníků spravovat lze také povolit pouze na nově vytvořené účty úložiště Resource Manager.

**Otázka: je SSE s zákazníka spravované klíče je povolena pouze v určitých oblastí?**

Odpověď: SSE je k dispozici v pouze určité oblasti pro úložiště objektů Blob pro tuto verzi Preview. E-mailu [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) toocheck dostupnosti a podrobnosti o verzi preview. 

**Otázka: jak I contact někdo po jakýchkoli problémů nebo slyšet tooprovide názor?**

Odpověď: kontaktní [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) pro všechny problémy související s tooStorage šifrování služby. 

## <a name="next-steps"></a>Další kroky

*   Další informace o komplexní sadu hello zabezpečení možnosti, které pomáhají vývojářům vytvářet aplikace, zabezpečení naleznete v tématu hello [Průvodce zabezpečením úložiště](https://docs.microsoft.com/en-us/azure/storage/storage-security-guide).
*   Souhrnné informace o Azure Key Vault naleznete v tématu [co je Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)?
*   Začínáme v Azure Key Vault, najdete v části [Začínáme s Azure Key Vault](../../key-vault/key-vault-get-started.md).
