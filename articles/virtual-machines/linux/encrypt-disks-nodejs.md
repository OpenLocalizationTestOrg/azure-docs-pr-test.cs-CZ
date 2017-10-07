---
title: "aaaEncrypt disky na virtuální počítač s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak hello tooencrypt disky na virtuální počítač s Linuxem pomocí Azure CLI 1.0 a modelu nasazení Resource Manager hello"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2017
ms.author: iainfou
ms.openlocfilehash: 68a0394d366c3c6941e2c6db0d4263123f951946
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-hello-azure-cli-10"></a>Šifrování disky na virtuální počítač s Linuxem pomocí hello Azure CLI 1.0
Pro lepší virtuální počítač (VM) zabezpečení a dodržování předpisů je možné zašifrovat virtuální disky v Azure v klidovém stavu. Disky jsou šifrované pomocí kryptografických klíčů, které jsou zabezpečené v Azure Key Vault. Řízení těchto kryptografické klíče a můžete auditovat jejich použití. Tento článek podrobně popisuje, jak hello tooencrypt virtuální disky na virtuální počítač s Linuxem pomocí Azure CLI 1.0 a modelu nasazení Resource Manager hello.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="quick-commands"></a>Rychlé příkazy
Pokud je třeba tooquickly dosáhnout hello, hello následující části Podrobnosti hello základní příkazy tooencrypt virtuální disky na virtuální počítač. Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#overview-of-disk-encryption).

Je třeba hello [nejnovější Azure CLI 1.0](../../xplat-cli-install.md) nainstalován a přihlášení pomocí režimu Resource Manager hello takto:

```azurecli
azure config mode arm
```

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad `myResourceGroup`, `myKeyVault`, a `myVM`.

Nejprve povolte hello Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure a vytvořte skupinu prostředků. Hello následující příklad vytvoří název skupiny prostředků `myResourceGroup` v hello `WestUS` umístění:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Vytvoření Azure Key Vault. Hello následující příklad vytvoří Key Vault s názvem `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Vytvoření kryptografické klíče v Key Vault a povolit šifrování disku. Hello následující příklad vytvoří klíč s názvem `myKey`:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Vytvořte koncový bod pomocí Azure Active Directory pro zpracování hello ověřování a výměna kryptografických klíčů z Key Vault. Hello `--home-page` a `--identifier-uris` nemusí toobe skutečné směrovatelné adresu. Hello nejvyšší úroveň zabezpečení je třeba použít klienta tajné klíče místo hesla. Hello rozhraní příkazového řádku Azure nelze vygenerovat aktuálně tajné klíče klienta. Tajné klíče klienta může být generována pouze v hello portálu Azure. Hello následující příklad vytvoří koncový bod služby Azure Active Directory s názvem `myAADApp` a používá na heslo `myPassword`. Zadejte své heslo následujícím způsobem:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Poznámka: hello `applicationId` ukazuje výstup hello hello předcházející příkaz. Toto ID aplikace se používá v hello následující kroky:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Přidáte datového disku tooan existující virtuální počítač. Hello následující příklad přidá disk tooa data virtuálního počítače s názvem `myVM`:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Zkontrolujte podrobnosti hello Key Vault a hello klíče, kterou jste vytvořili. Třeba hello ID klíč trezoru, identifikátor URI a klíč adresu URL v posledním kroku hello. Hello následující příklad zkontroluje hello podrobnosti pro Key Vault s názvem `myKeyVault` a klíč s názvem `myKey`:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Šifrování vaše disky následujícím způsobem zadávání vlastní názvy parametrů v průběhu:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Hello rozhraní příkazového řádku Azure neposkytuje podrobné chyby během procesu šifrování hello. Další informace o řešení potíží, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Jako hello předcházející příkaz obsahuje mnoho proměnné a nelze získat mnohem indikace jako toowhy hello proces selže, například dokončení příkazu vypadat takto:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Nakonec zkontrolujte stav šifrování hello znovu tooconfirm, že virtuální disky mají nyní šifrována. Hello následující příklad ověří hello stav virtuálního počítače s názvem `myVM` v hello `myResourceGroup` skupiny prostředků:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Přehled šifrování disku
Virtuální disky na virtuální počítače s Linuxem se šifrují pomocí rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Není nijak zpoplatněn pro šifrování virtuálních disků v Azure. Kryptografické klíče ukládají v Azure Key Vault pomocí ochrany proti softwaru, nebo můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM) certifikované tooFIPS standardy úroveň 2 140-2. Uchování kontroly nad těchto kryptografické klíče a můžete auditovat jejich použití. Tyto kryptografické klíče jsou použité tooencrypt a dešifrování tooyour připojené virtuální disky virtuálních počítačů. Koncový bod služby Azure Active Directory poskytuje zabezpečené mechanismus pro vydávání tyto kryptografické klíče jako virtuální počítače jsou zapnuté zapnout a vypnout.

Hello proces šifrování virtuálního počítače je následujícím způsobem:

1. Vytvoření kryptografické klíče v Azure Key Vault.
2. Nakonfigurujte hello kryptografické klíče toobe použitelné pro šifrování disků.
3. tooread hello kryptografický klíč z hello Azure Key Vault, vytvořit koncový bod pomocí služby Azure Active Directory s příslušnými oprávněními hello.
4. Vydejte příkaz tooencrypt hello virtuální disky, zadání koncový bod služby Azure Active Directory hello a příslušné kryptografické klíče toobe použít.
5. koncový bod služby Azure Active Directory Hello požadavků hello požadované kryptografický klíč z Azure Key Vault.
6. Hello virtuální disky jsou šifrované pomocí hello zadaný kryptografický klíč.

## <a name="supporting-services-and-encryption-process"></a>Podpora služby a proces šifrování
Šifrování disku spoléhá na hello následující další součásti:

* **Azure Key Vault** -použít toosafeguard kryptografické klíče a tajné klíče používané pro proces šifrování/dešifrování hello disku.
  * Pokud ano, můžete použít existující Azure Key Vault. Nemáte toodedicate disky tooencrypting Key Vault.
  * hranicemi správy tooseparate a klíče viditelnost, můžete vytvořit vyhrazený Key Vault.
* **Azure Active Directory** – obslužné rutiny hello zabezpečené výměna požadované kryptografické klíče a ověřování pro požadované akce.
  * Pro uložení aplikace můžete obvykle použít existující instanci služby Azure Active Directory.
  * aplikace Hello koncový bod pro hello Key Vault a virtuální počítač služby toorequest a získat vydané hello odpovídající kryptografické klíče. Nevyvíjíte skutečné aplikace, která se integruje se službou Azure Active Directory.

## <a name="requirements-and-limitations"></a>Požadavky a omezení
Podporované scénáře a požadavky na šifrování disku:

* Hello následující server Linux SKU - Ubuntu, CentOS, SUSE a SUSE Linux Enterprise Server (SLES) a Red Hat Enterprise Linux.
* Všechny prostředky (například Key Vault, účet úložiště a virtuálních počítačů) musí být v hello stejné oblasti Azure a předplatné.
* Standardní A, D, DS, G a GS řada virtuálních počítačů.

Šifrování disku aktuálně nepodporuje hello následující scénáře:

* Úroveň Basic virtuálních počítačů.
* Virtuální počítače vytvořené pomocí modelu nasazení Classic hello.
* Zakázáním šifrování disku operačního systému na virtuální počítače s Linuxem.
* Aktualizace hello kryptografické klíče na virtuální počítač již šifrované Linux.

## <a name="create-hello-azure-key-vault-and-keys"></a>Vytvoření hello Azure Key Vault a klíče
toocomplete hello zbytek tohoto průvodce, je nutné hello [nejnovější Azure CLI 1.0](../../xplat-cli-install.md) nainstalován a přihlášení pomocí režimu Resource Manager hello takto:

```azurecli
azure config mode arm
```

V rámci hello příkladech nahraďte všechny parametry příklad vlastní názvy, umístění a hodnoty klíče. Hello následující příklady použití konvence `myResourceGroup`, `myKeyVault`, `myAADApp`atd.

prvním krokem Hello je toocreate Azure Key Vault toostore kryptografických klíčů. Azure Key Vault můžete ukládat klíče, tajné klíče, nebo jejich hesla, které vám umožňují toosecurely implementaci v vašim aplikacím a službám. Šifrování virtuálního disku použijte Key Vault toostore kryptografický klíč, který je použité tooencrypt nebo dešifrovat virtuální disky.

Povolit zprostředkovatele Azure Key Vault hello ve vašem předplatném Azure a pak vytvořte skupinu prostředků. Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `WestUS` umístění:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Hello Azure Key Vault obsahující hello kryptografické klíče a přidružených výpočetních, které prostředky, jako je například úložiště a hello virtuální počítač se musí nacházet v hello stejné oblasti. Hello následující příklad vytvoří Azure Key Vault s názvem `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Můžete uložit kryptografické klíče pomocí softwaru nebo ochrany modelu hardwarového zabezpečení (HSM). Použití modulu hardwarového zabezpečení vyžaduje premium Key Vault. Není další náklady toocreating premium Key Vault, nikoli standardní Key Vault, který ukládá klíče chráněné softwarem. Přidat premium Key Vault, v předchozím kroku hello toocreate `--sku Premium` toohello příkaz. Hello následující příklad používá klíče chráněné softwarem vzhledem k tomu, že jsme vytvořili standardní Key Vault.

U obou modelů ochranu se musí hello platformy Azure toobe udělit přístup toorequest hello kryptografické klíče, když hello virtuální počítač spustí toodecrypt hello virtuální disky. Vytvořit šifrovací klíč v Key Vault a pak ji povolit pro použití s šifrováním virtuálního disku. Hello následující příklad vytvoří klíč s názvem `myKey` a povolí ho šifrování disku:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-hello-azure-active-directory-application"></a>Vytvoření aplikace Azure Active Directory hello
Když virtuální disky jsou zašifrovaná nebo dešifrovat, použijte ověřování hello toohandle koncový bod a výměna kryptografických klíčů z Key Vault. Tento koncový bod, aplikaci Azure Active Directory umožňuje hello platformy Azure toorequest odpovídající kryptografické klíče hello jménem hello virtuálních počítačů. Výchozí instance služby Azure Active Directory je ve vašem předplatném dostupná, když máte mnoho organizací vyhrazené adresáře služby Azure Active Directory.

Jako úplné aplikace Azure Active Directory nejsou vytváření, hello `--home-page` a `--identifier-uris` parametry v hello následující ukázka nemusí toobe skutečné směrovatelné adresu. Hello následující příklad také určuje tajného klíče založené na heslech, nikoli generování klíčů z v hello portálu Azure. Jako tento čas generování klíčů nelze provést z hello rozhraní příkazového řádku Azure.

Vytvoření aplikace Azure Active Directory. Hello následující příklad vytvoří aplikaci s názvem `myAADApp` a používá na heslo `myPassword`. Zadejte své heslo následujícím způsobem:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Poznamenejte si hello `applicationId` , je vrácen ve výstupu hello z hello předcházející příkaz. Toto ID aplikace se používá v některé z hello zbývající kroky. Dále vytvořte hlavní název služby (SPN) tak, aby aplikace hello je dostupný ve vašem prostředí. toosuccessfully zašifrovat nebo dešifrovat virtuální disky, oprávnění hello kryptografického klíče uložené v Key Vault musí být sada toopermit hello Azure Active Directory aplikace tooread hello klíče.

Vytvořte hello hlavního názvu služby a nastavte příslušná oprávnění hello takto:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Přidat virtuální disk a zkontrolovat stav šifrování
tooactually šifrování některé virtuální disky, umožňuje přidat disk tooan, existující virtuální počítač. Přidejte tooan 5Gb dat disku existující virtuální počítač následujícím způsobem:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

aktuálně nejsou šifrovány Hello virtuální disky. Zkontrolujte aktuální stav šifrování hello vašeho virtuálního počítače takto:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Šifrování virtuálních disků
toonow šifrování hello virtuální disky, můžete seskupit všechny předchozí komponenty hello:

1. Zadejte hello aplikaci Azure Active Directory a heslo.
2. Zadejte hello Key Vault toostore hello metadata pro šifrované disky.
3. Zadejte hello toouse kryptografické klíče pro hello skutečné šifrování a dešifrování.
4. Zadejte, zda chcete disk tooencrypt hello operačního systému, hello datových disků nebo všechny.

Umožňuje, zkontrolujte podrobnosti hello Azure Key Vault a hello klíče, které jste vytvořili, potřebujete hello ID klíč trezoru, identifikátor URI, a potom klíče adresy URL v posledním kroku hello:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Zašifrovat virtuální disky pomocí hello výstup hello `azure keyvault show` a `azure keyvault key show` příkazy následujícím způsobem:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Jak hello předchozí příkaz má mnoho proměnné, hello následující příklad je hello dokončení příkazu pro referenci:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Hello rozhraní příkazového řádku Azure neposkytuje podrobné chyby během procesu šifrování hello. Další informace o řešení potíží, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` na hello šifrujete virtuálních počítačů.

Nakonec umožňuje zkontrolovat stav šifrování hello znovu tooconfirm, mají nyní šifrované virtuální disky:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Přidání dalších datových disků
Jakmile jste zašifrovali datových disků, můžete později přidat tooyour další virtuální disky virtuálních počítačů a také zašifrovat. Když spustíte hello `azure vm enable-disk-encryption` příkaz, přírůstek hello pořadí verzi pomocí hello `--sequence-version` parametr. Tento parametr pořadí verzi můžete tooperform operací s hello stejného virtuálního počítače.

Umožňuje například přidat druhý virtuální disk tooyour virtuální počítač následujícím způsobem:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Znovu spustit hello příkaz tooencrypt hello virtuální disky, tentokrát přidání hello `--sequence-version` parametr a narůstajícími hello hodnotu z našich prvního spustit takto:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Další kroky
* Další informace o správě Azure Key Vault, včetně odstraňování kryptografické klíče a trezory, najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md).
* Další informace o šifrování disku, například při přípravě šifrované vlastní virtuální počítač tooupload tooAzure, najdete v části [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
