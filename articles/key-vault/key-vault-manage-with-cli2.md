---
title: "aaaManage Azure Key Vault, pomocí rozhraní příkazového řádku | Microsoft Docs"
description: "Použijte tento kurz tooautomate běžné úlohy v Key Vault pomocí hello 2.0 rozhraní příkazového řádku"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ambapat
ms.openlocfilehash: 76855c0ea09b6b307159468382a6a63627205556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli-20"></a>Spravovat pomocí rozhraní příkazového řádku 2.0 Key Vault
Azure Key Vault je dostupný ve většině oblastí. Další informace najdete v tématu hello [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod
Použijte tento kurz toohelp získáte začít s Azure Key Vault toocreate zesílený kontejner (trezor) v Azure, toostore a spravovat kryptografické klíče a tajné klíče v Azure. Provede vás procesem hello pomocí rozhraní příkazového řádku pro různé platformy Azure toocreate trezoru, který obsahuje klíč nebo heslo, které pak můžete použít s aplikací Azure. Poté vám ukáže, jak aplikaci můžete potom použít tento klíč nebo heslo.

**Odhadovaný čas toocomplete:** 20 minut

> [!NOTE]
> Tento kurz neobsahuje pokyny, jak toowrite hello Azure aplikace, která obsahuje jeden z hello kroků, které ukazuje, jak tooauthorize aplikaci toouse klíč nebo tajný klíč v hello klíč trezoru.
>
> Tento kurz používá hello nejnovější 2.0 rozhraní příkazového řádku Azure.
>
>

Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).

## <a name="prerequisites"></a>Požadavky
toocomplete tento kurz, musíte mít hello následující:

* TooMicrosoft předplatné Azure. Pokud jeden nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial).
* Rozhraní příkazového řádku verze 2.0 nebo novější. tooinstall hello nejnovější verzi a připojte tooyour předplatné Azure, najdete v části [instalace a konfigurace hello 2.0 rozhraní příkazového řádku pro různé platformy Azure](/cli/azure/install-azure-cli).
* Aplikace, které budou nakonfigurované toouse hello klíč nebo heslo, které vytvoříte v tomto kurzu. Vzorová aplikace je k dispozici z hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343). Pokyny najdete v tématu hello doplňujícími souboru Readme.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Získání nápovědy pomocí rozhraní příkazového řádku Azure a platformy
V tomto kurzu se předpokládá, že jste obeznámeni s hello rozhraní příkazového řádku (Bash, terminálu, příkazového řádku)

Hello – Nápověda nebo -h parametr může být použit tooview nápovědy pro určité příkazy. Alternativně hello azure nápovědy [příkaz] [možnosti], že formát lze také použít tooreturn hello stejné informace. Například následující hello příkazy všechny návratové hello stejné informace:

```
az account set --help
az account set -h
```

Pokud máte pochybnosti o parametrech hello vyžaduje příkaz, naleznete v toohelp pomocí – Nápověda, – h nebo az help [příkaz].

Také si můžete přečíst následující kurzy tooget obeznámeni s Azure Resource Manager v rozhraní příkazového řádku a platformy Azure hello:

* [Instalace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli)
* [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli)

## <a name="connect-tooyour-subscriptions"></a>Připojit tooyour odběrů
toolog pomocí účtu organizace, hello použijte následující příkaz:

```
az login -u username@domain.com -p password
```

nebo pokud chcete toolog zadáním interaktivně

```
az login
```

Pokud máte více předplatných a chcete toospecify konkrétní jeden toouse pro Azure Key Vault, zadejte hello následující toosee hello předplatných pro váš účet:

```
az account list
```

Potom toospecify hello předplatné toouse, zadejte:

```
az account set --subscription <subscription name or ID>
```

Další informace o konfiguraci rozhraní příkazového řádku pro různé platformy Azure najdete v tématu [nainstalovat rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Vytvoření nové skupiny prostředků
Pokud používáte Azure Resource Manager, všechny související prostředky vytváří v uvnitř skupiny prostředků. Pro účely tohoto kurzu vytvoříme novou skupinu prostředků, ContosoResourceGroup'.

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

první parametr Hello je název skupiny prostředků a druhý parametr hello je hello umístění. Pro umístění, použijte příkaz hello `az account list-locations` tooidentify jak toospecify alternativní umístění toohello jednu v tomto příkladu. Pokud potřebujete více informací, zadejte: `az account list-locations -h`.

## <a name="register-hello-key-vault-resource-provider"></a>Registrace poskytovatele prostředků hello Key Vault
Ujistěte se, že poskytovatele prostředků Key Vault je zaregistrován v rámci vašeho předplatného:

```
az provider register -n Microsoft.KeyVault
```

Stačí to toobe provádí jednou za předplatné.

## <a name="create-a-key-vault"></a>Vytvořte trezor klíčů
Použití hello `az keyvault create` příkaz toocreate trezoru klíčů. Tento skript má tři povinné parametry: název skupiny prostředků, název trezoru klíčů a hello zeměpisné umístění.

Například pokud použijete název trezoru hello ContosoKeyVault, název skupiny prostředků hello ContosoResourceGroup a hello umístění východní Asie, zadejte:
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Hello výstup tohoto příkazu zobrazuje vlastnosti trezoru klíčů hello, který jste právě vytvořili. nejdůležitější vlastnosti Hello dva jsou:

* **název**: V příkladu hello je to ContosoKeyVault. Tento název pro jiné příkazy Key Vault budete používat.
* **vaultUri**: V příkladu hello je to https://contosokeyvault.vault.azure.net. Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.

Váš účet Azure je autorizovaný tooperform žádné operace pro tento klíč trezoru. Zatím k tomu není oprávněn nikdo jiný.

## <a name="add-a-key-or-secret-toohello-key-vault"></a>Přidat klíč nebo tajný toohello trezoru klíčů
Pokud chcete Azure Key Vault toocreate klíč chráněný softwarem pro vás, použijte hello `az key create` příkaz a zadejte následující hello:
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
Nicméně pokud už máte existující klíč v soubor .pem uložený jako místního souboru do souboru s názvem softkey.pem, které chcete tooupload tooAzure Key Vault, zadejte hello následujících tooimport hello klíč z hello. Soubor PEM, který chrání klíč hello softwarem hello služby Key Vault:
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
Nyní můžete odkazovat hello klíče vytvořené nebo odeslán tooAzure Key Vault, pomocí jeho identifikátoru URI. Použít **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways získat aktuální verzi hello a používat **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget konkrétní verzi.

tooadd tajný toohello úložiště, který je hesla s názvem SQLPassword a hodnotou Pa$ $w0rd tooAzure Key Vault, typ hello následující hello:
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
Nyní si můžete odkazy Toto heslo přidali tooAzure Key Vault, a to pomocí jeho identifikátoru URI. Použít **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways získat aktuální verzi hello a používat **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget konkrétní verzi.

Podívejme se, zda text hello klíč nebo tajný klíč, který jste právě vytvořili:

* tooview vaše, zadejte:`az keyvault key list --vault-name 'ContosoKeyVault'`
* tooview váš tajný, typ:`az keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Registrujte aplikaci s Azure Active Directory
Tento krok obvykle provádí vývojář na samostatném počítači. Není konkrétní tooAzure Key Vault, ale je zahrnuté pro úplnost.

> [!IMPORTANT]
> toocomplete hello kurzu, váš účet, hello trezoru a hello aplikace, která zaregistrujete v tomto kroku musí být ve hello stejném adresáři Azure.
>
>

Aplikace, které používají trezor klíčů, se musí ověřit pomocí tokenu z Azure Active Directory. toodo se hello vlastníka aplikace hello musí hello aplikaci nejprve zaregistrovat v Azure Active Directory. Na hello konci registrace obdrží majitel aplikace hello hello následující hodnoty:

* **ID aplikace** (také označované jako ID klienta) a **ověřovací klíč** (také označované jako hello sdílený tajný klíč). Hello aplikace musí být obě tyto hodnoty tooAzure služby Active Directory, tooget token. Jak aplikace hello je nakonfigurován toodo to závisí na aplikaci hello. Pro hello ukázkovou aplikaci Key Vault nastavuje majitel aplikace hello tyto hodnoty v souboru app.config hello.

tooregister hello aplikace v Azure Active Directory:

1. Přihlaste se toohello portálu Azure.
2. Na levé straně hello, klikněte na tlačítko **Azure Active Directory**a potom vyberte hello adresář, ve kterém zaregistrujete vaší aplikace. <br> <br> 

> [!Note] 
> Je nutné vybrat hello hello stejný adresář, který obsahuje předplatné Azure, který jste použili pro vytvoření trezoru klíčů. Pokud si nejste jisti který adresář to je, klikněte na tlačítko **nastavení**, určete hello předplatné, které jste použili pro vytvoření trezoru klíčů, a Poznámka hello název adresáře hello zobrazí v posledním sloupci hello.

3. Klikněte na **Aplikace**. Pokud nebyly přidané žádné aplikace tooyour directory, tato stránka se zobrazí pouze hello **přidat aplikaci** odkaz. Kliknutím na odkaz hello nebo, případně můžete kliknutím na tlačítko hello **přidat** na panelu příkazů hello.
4. V hello **přidat aplikaci** na hello **co chcete toodo?** klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.
5. Na hello **Řekněte nám o své aplikaci** , zadejte název pro vaši aplikaci a vyberte **webové aplikace nebo webové rozhraní API** (hello výchozí). Kliknutím na ikonu další hello.
6. Na hello **vlastností aplikace** zadejte hello **adresa URL přihlašování** a **identifikátor ID URI aplikace** pro webovou aplikaci. Pokud vaše aplikace tyto hodnoty neobsahuje, v tomto kroku si je můžete vymyslet (například můžete do obou polí zadat http://test1.contoso.com). Není důležité, pokud tyto stránky existují; Co je důležité je, že tuto aplikaci hello identifikátor ID URI pro každou aplikaci se liší pro každou aplikaci ve vašem adresáři. Hello directory používá tento řetězec tooidentify vaší aplikace.
7. Klikněte na tlačítko hello dokončení ikonu toosave změny v Průvodci hello.
8. Na stránce hello rychlý Start, klikněte na **konfigurace**.
9. Posuňte se toohello **klíče** vyberte dobu trvání hello a pak klikněte na tlačítko **Uložit**. stránku Hello se obnoví a zobrazí hodnotu klíče. Vaši aplikaci je nutné nakonfigurovat s touto hodnotou klíče a hello **ID klienta** hodnotu. (Pokyny k této konfiguraci se budou lišit podle aplikace.)
10. Zkopírujte hodnotu ID klienta hello z této stránky, které budete používat v hello další krok tooset oprávnění pro váš trezoru.

## <a name="authorize-hello-application-toouse-hello-key-or-secret"></a>Autorizovat hello aplikace toouse hello klíče nebo tajného klíče
tooaccess aplikace hello tooauthorize hello klíče nebo tajného klíče v trezoru hello, použijte hello `az keyvault set-policy` příkaz.

Například pokud je název vaší trezoru ContosoKeyVault a hello aplikace, které chcete tooauthorize má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed ID klienta a chcete toodecrypt aplikace hello tooauthorize a přihlaste s klíči v trezoru, spusťte hello následující:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Pokud chcete, tooauthorize Tento stejný tooread tajné klíče aplikace ve vašem trezoru, spusťte hello následující:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a>Pokud chcete, aby toouse modul hardwarového zabezpečení (HSM)
Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM hello. Hello moduly hardwarového zabezpečení jsou FIPS 140-2 Level 2 ověřit. Pokud tento požadavek netýká tooyou, tuto část přeskočte a přejděte příliš[odstranit hello trezoru klíčů, přidružených klíčů a tajných klíčů](#delete-the-key-vault-and-associated-keys-and-secrets).

toocreate těchto klíčů chráněných pomocí HSM musíte mít předplatné trezoru s podporou klíčů chráněných pomocí HSM.

Když vytvoříte hello keyvault, přidejte parametr "sku" hello:

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
Můžete přidat klíče chráněné softwarem (jak je uvedeno výše) a trezoru klíčů chráněných pomocí HSM toothis. toocreate klíč chráněný HSM, sada hello cílový parametr too'HSM':

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

Můžete použít následující příkaz tooimport klíč ze souboru .pem ve vašem počítači hello. Tento příkaz importuje hello klíč do HSM ve hello služby Key Vault:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
Hello další příkaz importuje "přineste si vlastní klíč" (BYOK) balíčku. To umožňuje vygenerovat klíč v místním HSM a jeho přenesení tooHSMs v hello služby Key Vault, bez hello klíč opustil hranice HSM hello:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
Podrobnější pokyny, jak toogenerate tento balíček BYOK najdete v části [jak toouse HSM-Protected klíčů s Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a>Odstranit hello trezoru klíčů, přidružených klíčů a tajných klíčů
Pokud již nepotřebujete trezor klíčů hello a hello klíč nebo tajný klíč, který obsahuje, můžete hello trezor klíčů odstranit pomocí hello `az keyvault delete` příkaz:

```
az keyvault delete --name 'ContosoKeyVault'
```

Nebo můžete odstranit skupinu prostředků celý Azure, která zahrnuje trezor klíčů hello a další prostředky, které jste zahrnuli do této skupiny:

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Ostatní příkazy rozhraní příkazového řádku Azure a platformy
Další příkazy, že může využít ke správě Azure Key Vault.

Tento příkaz vypíše tabulkové zobrazení všech klíčů a vybraných vlastností:

AZ seznam klíčů keyvault – název trezoru ContosoKeyVault

Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč hello:

Zobrazit klíč keyvault az--název trezoru ContosoKeyVault – název 'ContosoFirstKey.

Tento příkaz vypíše tabulkové zobrazení všech názvů tajných klíčů a vybraných vlastností:

AZ keyvault tajný seznamu – název trezoru ContosoKeyVault

Tady je příklad toho, jak tooremove konkrétního klíče:

odstranění klíče keyvault az--název trezoru ContosoKeyVault – název 'ContosoFirstKey.

Tady je příklad toho, jak tooremove určitého tajného klíče:

Odstranit tajný klíč keyvault az--název trezoru ContosoKeyVault – název 'SQLPassword.


## <a name="next-steps"></a>Další kroky
Úplný referenční rozhraní příkazového řádku Azure pro příkazy trezoru klíčů, najdete v části [odkaz klíč trezoru rozhraní příkazového řádku](/cli/azure/keyvault)

Programátorské reference najdete v části [hello Příručka pro vývojáře Azure Key Vault](key-vault-developers-guide.md).
