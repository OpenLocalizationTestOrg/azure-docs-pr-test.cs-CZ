---
title: "aaaEncrypt disky na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Jak hello tooencrypt virtuální disky na virtuální počítač s Linuxem pro lepší zabezpečení pomocí Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: d6197742bc8562630e8395588c072093fc01d614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencrypt-virtual-disks-on-a-linux-vm"></a>Jak tooencrypt virtuální disky na virtuální počítač s Linuxem
Pro lepší virtuální počítač (VM) zabezpečení a dodržování předpisů je možné zašifrovat virtuální disky v Azure. Disky jsou šifrované pomocí kryptografických klíčů, které jsou zabezpečené v Azure Key Vault. Řízení těchto kryptografické klíče a můžete auditovat jejich použití. Tento článek podrobně popisuje, jak hello tooencrypt virtuální disky na virtuální počítač s Linuxem pomocí Azure CLI 2.0. Můžete také provést tyto kroky hello [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-commands"></a>Rychlé příkazy
Pokud je třeba tooquickly dosáhnout hello, hello následující části Podrobnosti hello základní příkazy tooencrypt virtuální disky na virtuální počítač. Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#overview-of-disk-encryption).

Je třeba hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login). Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad *myResourceGroup*, *myKey*, a *Můjvp*.

Nejprve povolit hello Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure s [registrace zprostředkovatele az](/cli/azure/provider#register) a vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří název skupiny prostředků *myResourceGroup* v hello *eastus* umístění:

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

Vytvoření Azure Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolte hello Key Vault pro použití s šifrování disku. Zadejte jedinečný název pro Key Vault *keyvault_name* následujícím způsobem:

```azurecli
keyvault_name=mykeyvaultikf
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Vytvoření kryptografické klíče v Key Vault s [vytvořit klíč keyvault az](/cli/azure/keyvault/key#create). Hello následující příklad vytvoří klíč s názvem *myKey*:

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```

Vytvořit objekt služby pomocí služby Azure Active Directory s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac). hlavní obslužných rutin služby Hello hello ověřování a výměnu kryptografických klíčů z Key Vault. Následující ukázka Hello čtení hodnoty hello hello instanční objekt Id a heslo pro použití v novější příkazy:

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

Při vytváření hello instanční objekt se nachází pouze výstup Hello heslo. V případě potřeby, zobrazení a záznam hello heslo (`echo $sp_password`). Můžete vytvořit seznam hlavních objektů s několika vaší služby [az ad sp seznamu](/cli/azure/ad/sp#list) a zobrazit další informace o konkrétní objekt s služby [az ad sp zobrazit](/cli/azure/ad/sp#show).

Nastavení oprávnění pro Key Vault s [az keyvault set-policy](/cli/azure/keyvault#set-policy). V následujícím příkladu hello hello ID objektu zabezpečení služby je dodávána hello předcházející příkaz:

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set
```

Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create) a připojit datový disk 5 Gb. Pouze některé bitové kopie marketplace podporují šifrování disku. Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí **CentOS 7.2n** bitové kopie:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH tooyour virtuální počítač pomocí hello `publicIpAddress` ukazuje výstup hello hello předcházející příkaz. Vytvořit oddíl a systému souborů a pak připojit datový disk hello. Další informace najdete v tématu [připojit tooa virtuálního počítače s Linuxem toomount hello nový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Ukončení relace SSH.

Šifrování virtuálního počítače s [povolit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#enable). Hello následující příklad používá hello `$sp_id` a `$sp_password` proměnné z předchozí hello `ad sp create-for-rbac` příkaz:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Toocomplete proces šifrování disku hello nějakou dobu trvá. Monitorování stavu hello hello proces s [zobrazit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#show):

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

Dobrý den zobrazí stav **EncryptionInProgress**. Počkat, až hello stav pro sestavy disku hello OS **VMRestartPending**, potom restartujte virtuální počítač s [restartování virtuálního počítače az](/cli/azure/vm#restart):

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

Hello proces šifrování disku je dokončen. během procesu spuštění hello, proto Počkejte několik minut před zaškrtnutím hello stav šifrování znovu s **zobrazit šifrování virtuálních počítačů az**:

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

Stav Hello by teď disku hello operačního systému i datový disk jako **šifrovaný**.

## <a name="overview-of-disk-encryption"></a>Přehled šifrování disku
Virtuální disky na virtuální počítače s Linuxem se šifrují pomocí rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Není nijak zpoplatněn pro šifrování virtuálních disků v Azure. Kryptografické klíče ukládají v Azure Key Vault pomocí ochrany proti softwaru, nebo můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM) certifikované tooFIPS standardy úroveň 2 140-2. Uchování kontroly nad těchto kryptografické klíče a můžete auditovat jejich použití. Tyto kryptografické klíče jsou použité tooencrypt a dešifrování tooyour připojené virtuální disky virtuálních počítačů. Hlavní služby Azure Active Directory poskytuje zabezpečené mechanismus pro vydávání tyto kryptografické klíče jako virtuální počítače jsou zapnuté zapnout a vypnout.

Hello proces šifrování virtuálního počítače je následujícím způsobem:

1. Vytvoření kryptografické klíče v Azure Key Vault.
2. Nakonfigurujte hello kryptografické klíče toobe použitelné pro šifrování disků.
3. tooread hello kryptografický klíč z hello Azure Key Vault, vytvoření služby Azure Active Directory objekt zabezpečení s příslušnými oprávněními hello.
4. Vydejte příkaz tooencrypt hello virtuální disky, zadáte hello objektu služby Azure Active Directory a příslušné kryptografické klíče toobe použít.
5. hlavní požadavky služby Azure Active Directory Hello hello požadované kryptografický klíč z Azure Key Vault.
6. Hello virtuální disky jsou šifrované pomocí hello zadaný kryptografický klíč.

## <a name="encryption-process"></a>Proces šifrování
Šifrování disku spoléhá na hello následující další součásti:

* **Azure Key Vault** -použít toosafeguard kryptografické klíče a tajné klíče používané pro proces šifrování/dešifrování hello disku.
  * Pokud ano, můžete použít existující Azure Key Vault. Nemáte toodedicate disky tooencrypting Key Vault.
  * hranicemi správy tooseparate a klíče viditelnost, můžete vytvořit vyhrazený Key Vault.
* **Azure Active Directory** – obslužné rutiny hello zabezpečené výměna požadované kryptografické klíče a ověřování pro požadované akce.
  * Pro uložení aplikace můžete obvykle použít existující instanci služby Azure Active Directory.
  * Hello instanční objekt poskytuje zabezpečené mechanismus toorequest a vydávají hello odpovídající kryptografické klíče. Nevyvíjíte skutečné aplikace, která se integruje se službou Azure Active Directory.

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

## <a name="create-azure-key-vault-and-keys"></a>Vytvoření Azure Key Vault a klíče
Je třeba hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login). Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad *myResourceGroup*, *myKey*, a *Můjvp*.

prvním krokem Hello je toocreate Azure Key Vault toostore kryptografických klíčů. Azure Key Vault můžete ukládat klíče, tajné klíče, nebo jejich hesla, které vám umožňují toosecurely implementaci v vašim aplikacím a službám. Šifrování virtuálního disku použijte Key Vault toostore kryptografický klíč, který je použité tooencrypt nebo dešifrovat virtuální disky.

Povolit hello Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure s [registrace zprostředkovatele az](/cli/azure/provider#register) a vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří název skupiny prostředků *myResourceGroup* v hello `eastus` umístění:

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

Hello Azure Key Vault obsahující hello kryptografické klíče a přidružených výpočetních, které prostředky, jako je například úložiště a hello virtuální počítač se musí nacházet v hello stejné oblasti. Vytvoření Azure Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolte hello Key Vault pro použití s šifrování disku. Zadejte jedinečný název pro Key Vault *keyvault_name* následujícím způsobem:

```azurecli
keyvault_name=myUniqueKeyVaultName
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Můžete uložit kryptografické klíče pomocí softwaru nebo ochrany modelu hardwarového zabezpečení (HSM). Použití modulu hardwarového zabezpečení vyžaduje premium Key Vault. Není další náklady toocreating premium Key Vault, nikoli standardní Key Vault, který ukládá klíče chráněné softwarem. Přidat premium Key Vault, v předchozím kroku hello toocreate `--sku Premium` toohello příkaz. Hello následující příklad používá klíče chráněné softwarem vzhledem k tomu, že jsme vytvořili standardní Key Vault.

U obou modelů ochranu se musí hello platformy Azure toobe udělit přístup toorequest hello kryptografické klíče, když hello virtuální počítač spustí toodecrypt hello virtuální disky. Vytvoření kryptografické klíče v Key Vault s [vytvořit klíč keyvault az](/cli/azure/keyvault/key#create). Hello následující příklad vytvoří klíč s názvem *myKey*:

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-hello-azure-active-directory-service-principal"></a>Vytvořit objekt služby Azure Active Directory hello
Když virtuální disky jsou zašifrovaná nebo dešifrovat, je třeba zadat ověření účtu toohandle hello a výměna kryptografických klíčů z Key Vault. Tento účet objektu zabezpečení služby Azure Active Directory umožňuje hello platformy Azure toorequest odpovídající kryptografické klíče hello jménem hello virtuálních počítačů. Výchozí instance služby Azure Active Directory je ve vašem předplatném dostupná, když máte mnoho organizací vyhrazené adresáře služby Azure Active Directory.

Vytvořit objekt služby pomocí služby Azure Active Directory s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac). Následující ukázka Hello čtení hodnoty hello hello instanční objekt Id a heslo pro použití v novější příkazy:

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

Hello hesla se zobrazí pouze při vytváření objektu služby hello. V případě potřeby, zobrazení a záznam hello heslo (`echo $sp_password`). Můžete vytvořit seznam hlavních objektů s několika vaší služby [az ad sp seznamu](/cli/azure/ad/sp#list) a zobrazit další informace o konkrétní objekt s služby [az ad sp zobrazit](/cli/azure/ad/sp#show).

toosuccessfully zašifrovat nebo dešifrovat virtuální disky, oprávnění hello kryptografického klíče uložené v Key Vault musí být sada toopermit hello Azure Active Directory service hlavní tooread hello klíče. Nastavení oprávnění pro Key Vault s [az keyvault set-policy](/cli/azure/keyvault#set-policy). V následujícím příkladu hello hello ID objektu zabezpečení služby je dodávána hello předcházející příkaz:

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače
tooactually šifrování některé virtuální disky, umožňuje vytvoření virtuálního počítače a přidat datový disk. Vytvoření virtuálního počítače tooencrypt s [vytvořit virtuální počítač az](/cli/azure/vm#create) a připojit datový disk 5 Gb. Pouze některé bitové kopie marketplace podporují šifrování disku. Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* pomocí **CentOS 7.2n** bitové kopie:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH tooyour virtuální počítač pomocí hello `publicIpAddress` ukazuje výstup hello hello předcházející příkaz. Vytvořit oddíl a systému souborů a pak připojit datový disk hello. Další informace najdete v tématu [připojit tooa virtuálního počítače s Linuxem toomount hello nový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Ukončení relace SSH.


## <a name="encrypt-virtual-machine"></a>Šifrování virtuálního počítače
tooencrypt hello virtuální disky, můžete seskupit všechny předchozí komponenty hello:

1. Zadejte hello objektu služby Azure Active Directory a heslo.
2. Zadejte hello Key Vault toostore hello metadata pro šifrované disky.
3. Zadejte hello toouse kryptografické klíče pro hello skutečné šifrování a dešifrování.
4. Zadejte, zda chcete disk tooencrypt hello operačního systému, hello datových disků nebo všechny.

Šifrování virtuálního počítače s [povolit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#enable). Hello následující příklad používá hello `$sp_id` a `$sp_password` proměnné z předchozí hello `ad sp create-for-rbac` příkaz:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Toocomplete proces šifrování disku hello nějakou dobu trvá. Monitorování stavu hello hello proces s [zobrazit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#show):

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

Hello výstup je podobné toohello následující zkrácený ukázka:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress",
]
```

Počkat, až hello stav pro sestavy disku hello OS **VMRestartPending**, potom restartujte virtuální počítač s [restartování virtuálního počítače az](/cli/azure/vm#restart):

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

Hello proces šifrování disku je dokončen. během procesu spuštění hello, proto Počkejte několik minut před zaškrtnutím hello stav šifrování znovu s **zobrazit šifrování virtuálních počítačů az**:

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

Stav Hello by teď disku hello operačního systému i datový disk jako **šifrovaný**.


## <a name="add-additional-data-disks"></a>Přidání dalších datových disků
Jakmile jste zašifrovali datových disků, můžete později přidat tooyour další virtuální disky virtuálních počítačů a také zašifrovat. Umožňuje například přidat druhý virtuální disk tooyour virtuální počítač následujícím způsobem:

```azurecli
az vm disk attach-new --resource-group myResourceGroup --vm-name myVM --size-in-gb 5
```

Znovu spusťte hello příkaz tooencrypt hello virtuální disky následujícím způsobem:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a>Další kroky
* Další informace o správě Azure Key Vault, včetně odstraňování kryptografické klíče a trezory, najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md).
* Další informace o šifrování disku, například při přípravě šifrované vlastní virtuální počítač tooupload tooAzure, najdete v části [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
