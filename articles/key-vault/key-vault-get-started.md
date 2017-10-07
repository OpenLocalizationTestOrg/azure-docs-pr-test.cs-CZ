---
title: "aaaGet začít s Azure Key Vault | Microsoft Docs"
description: "Použijte tento kurz toohelp získáte začít s Azure Key Vault toocreate zesílený kontejner v Azure, toostore a spravovat kryptografické klíče a tajné klíče v Azure."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 865853b778dec5fca5c7db0d060627554c0a9cb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-key-vault"></a>Začínáme s Azure Key Vault
Azure Key Vault je dostupný ve většině oblastí. Další informace najdete v tématu hello [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod
Použijte tento kurz toohelp získáte začít s Azure Key Vault toocreate zesílený kontejner (trezor) v Azure, toostore a spravovat kryptografické klíče a tajné klíče v Azure. Provede vás procesem hello použití prostředí Azure PowerShell toocreate trezoru, který obsahuje klíč nebo heslo, které pak můžete použít s aplikací Azure. Poté vám ukáže, jak aplikace může tento klíč nebo heslo použít.

**Odhadovaný čas toocomplete:** 20 minut

> [!NOTE]
> Tento kurz neobsahuje pokyny, jak toowrite hello aplikaci Azure, která obsahuje jeden z kroků hello, a to jak tooauthorize aplikaci toouse klíč nebo tajný klíč v hello klíč trezoru.
>
> Tento kurz používá prostředí Azure PowerShell. Pokyny pro rozhraní příkazového řádku pro různé platformy najdete v [tomto ekvivalentním kurzu](key-vault-manage-with-cli2.md).
>
>

Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).

## <a name="prerequisites"></a>Požadavky
toocomplete tento kurz, musíte mít hello následující:

* TooMicrosoft předplatné Azure. Pokud žádné nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/)
* Azure PowerShell v **minimální verzi 1.1.0**. tooinstall prostředí Azure PowerShell a přidružit ho ke svému předplatnému Azure, najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Pokud jste již Azure PowerShell nainstalovali a neznáte hello verzi, z konzoly Azure PowerShell text hello, zadejte `(Get-Module azure -ListAvailable).Version`. Máte-li nainstalovaný Azure PowerShell ve verzi 0.9.1 až 0.9.8, stále můžete tento kurz použít, pouze s menšími změnami. Například musíte použít hello `Switch-AzureMode AzureResourceManager` příkaz a některé příkazy Azure Key Vault hello změnily. Seznam rutin Key Vault pro verze 0.9.1 až 0.9.8 hello najdete v tématu [rutiny Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).
* Aplikace, které budou nakonfigurované toouse hello klíč nebo heslo, které vytvoříte v tomto kurzu. Vzorová aplikace je k dispozici z hello [Microsoft Download Center](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Pokyny najdete v tématu hello doplňujícími souboru Readme.

Tento kurz je určen pro začátečníky v Azure Powershellu, ale předpokládá, že chápete základní koncepty hello, jako jsou moduly, rutiny a relace. Další informace naleznete v tématu [Začínáme s Microsoft PowerShellem](https://technet.microsoft.com/library/hh857337.aspx).

tooget podrobnou nápovědu k jakékoli rutině v tomto kurzu použijte hello **Get-Help** rutiny.

    Get-Help <cmdlet-name> -Detailed

Například tooget nápovědu pro hello **Login-AzureRmAccount** rutiny, zadejte:

    Get-Help Login-AzureRmAccount -Detailed

Také si můžete přečíst následující kurzy tooget obeznámeni s Azure Resource Manager v prostředí Azure PowerShell hello:

* [Jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview)
* [Použití Azure PowerShellu s Resource Managerem](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Připojit tooyour odběrů
Spusťte relaci prostředí Azure PowerShell a přihlaste tooyour účet Azure s hello následující příkaz:  

    Login-AzureRmAccount

Všimněte si, že pokud používáte konkrétní instanci Azure, například Azure Government, použijte hello - parametr prostředí pomocí tohoto příkazu. Příklad: `Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

V okně hello automaticky otevírané okno prohlížeče zadejte účet Azure uživatelské jméno a heslo. Prostředí Azure PowerShell získá všechna předplatná hello, které jsou spojeny s tímto účtem a ve výchozím nastavení, používá hello první z nich.

Pokud máte více předplatných a chcete toospecify konkrétní jeden toouse pro Azure Key Vault, zadejte hello následující toosee hello předplatných pro váš účet:

    Get-AzureRmSubscription

Potom toospecify hello předplatné toouse, zadejte:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Další informace o konfiguraci Azure Powershellu najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a id="resource"></a>Vytvoření nové skupiny prostředků
Používáte-li Azure Resource Manager, pak se všechny související prostředky vytváří v uvnitř skupiny prostředků. Pro účely tohoto kurzu vytvoříme novou skupinu prostředků s názvem **ContosoResourceGroup**:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Vytvoření trezoru klíčů
Použití hello [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) rutiny toocreate trezoru klíčů. Tato rutina má tři povinné parametry: **název skupiny prostředků**, **název trezoru klíčů**a hello **zeměpisné umístění**.

Například pokud použijete název trezoru hello **ContosoKeyVault**, název skupiny prostředků hello **ContosoResourceGroup**a umístění hello **východní Asie**, zadejte:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Hello výstup této rutiny zobrazuje vlastnosti trezoru klíčů hello, který jste právě vytvořili. nejdůležitější vlastnosti Hello dva jsou:

* **Název trezoru**: V příkladu hello je to **ContosoKeyVault**. Tento název budete používat pro další rutiny Key Vault.
* **Identifikátor URI trezoru**: V příkladu hello je to https://contosokeyvault.vault.azure.net/. Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.

Váš účet Azure je autorizovaný tooperform žádné operace pro tento klíč trezoru. Zatím k tomu není oprávněn nikdo jiný.

> [!NOTE]
> Pokud se zobrazí chyba hello **hello předplatné není registrované toouse oboru názvů "Microsoft.KeyVault"** když se pokusíte toocreate nového trezoru klíčů, spusťte `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` a poté znovu spusťte váš příkaz New-AzureRmKeyVault. Další informace najdete v tématu [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Přidat klíč nebo tajný toohello trezoru klíčů
Pokud chcete Azure Key Vault toocreate klíč chráněný softwarem pro vás, použijte hello [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) rutiny a zadejte následující hello:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Ale pokud máte existující softwarově chráněný klíč. Jednotka tooyour uložit soubor PFX C:\ v souboru s názvem softkey.pfx, které chcete tooupload tooAzure Key Vault, typ hello následující proměnné hello tooset **securepfxpwd** na heslo **123** pro hello. Soubor PFX:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Zadejte hello následujících tooimport hello klíč z hello. Soubor PFX, který chrání klíč hello softwarem hello služby Key Vault:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Tento klíč vytvořené nebo odeslaný pomocí jeho identifikátoru URI tooAzure Key Vault, můžete odkazovat. Použít **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways získat aktuální verzi hello a používat **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget konkrétní verzi.  

toodisplay hello identifikátor URI pro tento klíč, zadejte:

    $Key.key.kid

tooadd tajný toohello trezoru, která je hesla s názvem SQLPassword a hodnotou Pa$ $w0rd tooAzure Key Vault hello, nejprve převeďte hello hodnotu Pa$ $w0rd tooa zabezpečený řetězec zadáním hello následující:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Poté zadejte hello následující:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Nyní si můžete odkazy Toto heslo přidali tooAzure Key Vault, a to pomocí jeho identifikátoru URI. Použít **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways získat aktuální verzi hello a používat **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget konkrétní verzi.

toodisplay hello identifikátor URI tajného klíče, zadejte:

    $secret.Id

Podívejme se, zda text hello klíč nebo tajný klíč, který jste právě vytvořili:

* tooview vaše, zadejte:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
* tooview váš tajný, typ:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Nyní je připraven pro aplikace toouse trezoru klíčů a klíče nebo tajného klíče. Je nutné autorizovat toouse aplikace je.  

## <a id="register"></a>Registrace aplikace s Azure Active Directory
Tento krok obvykle provádí vývojář na samostatném počítači. Není konkrétní tooAzure Key Vault, ale jsou zde uvedeny pro úplnost.

> [!IMPORTANT]
> toocomplete hello kurzu, váš účet, hello trezoru a hello aplikace, která zaregistrujete v tomto kroku musí být ve hello stejném adresáři Azure.
>
>

Aplikace, které používají trezor klíčů, se musí ověřit pomocí tokenu z Azure Active Directory. toodo se hello vlastníka aplikace hello musí hello aplikaci nejprve zaregistrovat v Azure Active Directory. Na hello konci registrace obdrží majitel aplikace hello hello následující hodnoty:

* **ID aplikace** (také označované jako ID klienta) a **ověřovací klíč** (také označované jako hello sdílený tajný klíč). Hello aplikace musí být obě tyto hodnoty tooAzure služby Active Directory, tooget token. Jak aplikace hello je nakonfigurován toodo to závisí na aplikaci hello. Pro hello ukázkovou aplikaci Key Vault nastavuje majitel aplikace hello tyto hodnoty v souboru app.config hello.

tooregister hello aplikace v Azure Active Directory:

1. Přihlaste se toohello portál Azure classic.
2. Na levé straně hello, klikněte na tlačítko **služby Active Directory**a potom vyberte hello adresář, ve kterém zaregistrujete vaší aplikace. <br> <br> **Poznámka:** hello musíte vybrat stejný adresář, který obsahuje hello předplatné Azure, který jste použili pro vytvoření trezoru klíčů. Pokud si nejste jisti který adresář to je, klikněte na tlačítko **nastavení**, určete hello předplatné, které jste použili pro vytvoření trezoru klíčů, a Poznámka hello název adresáře hello zobrazí v posledním sloupci hello.
3. Klikněte na **Aplikace**. Pokud nebyly přidané žádné aplikace tooyour directory, tato stránka zobrazuje pouze hello **přidat aplikaci** odkaz. Kliknutím na odkaz hello nebo Alternativně můžete kliknout na **přidat** na panelu příkazů hello.
4. V hello **přidat aplikaci** na hello **co chcete toodo?** klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.
5. Na hello **Řekněte nám o své aplikaci** , zadejte název pro vaši aplikaci a pak vyberte **webové aplikace nebo webové rozhraní API** (hello výchozí). Klikněte na tlačítko hello **Další** ikonu.
6. Na hello **vlastností aplikace** zadejte hello **adresa URL přihlašování** a **identifikátor ID URI aplikace** pro webovou aplikaci. Pokud vaše aplikace tyto hodnoty neobsahuje, v tomto kroku si je můžete vymyslet (například můžete do obou polí zadat http://test1.contoso.com). Nezáleží na tom, zda tyto weby existují. Co je důležité je, že tuto aplikaci hello identifikátor ID URI pro každou aplikaci se liší pro každou aplikaci ve vašem adresáři. Hello directory používá tento řetězec tooidentify vaší aplikace.
7. Klikněte na tlačítko hello **Complete** ikonu toosave změny v Průvodci hello.
8. Na hello **rychlý Start** klikněte na tlačítko **konfigurace**.
9. Posuňte se toohello **klíče** vyberte dobu trvání hello a pak klikněte na tlačítko **Uložit**. stránku Hello se obnoví a zobrazí hodnotu klíče. Vaši aplikaci je nutné nakonfigurovat s touto hodnotou klíče a hello **ID klienta** hodnotu. (Pokyny k této konfiguraci se liší podle aplikace.)
10. Zkopírujte hodnotu ID klienta hello z této stránky, které budete používat v hello další krok tooset oprávnění pro váš trezoru.

## <a id="authorize"></a>Autorizovat hello aplikace toouse hello klíče nebo tajného klíče
tooaccess aplikace hello tooauthorize hello klíče nebo tajného klíče v trezoru hello, použijte [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) rutiny.

Například, pokud je název vaší trezoru **ContosoKeyVault** a aplikace hello chcete tooauthorize má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed ID klienta a chcete toodecrypt aplikace hello tooauthorize a přihlaste s klíči v vašem trezoru spuštěním hello následující:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Pokud chcete, tooauthorize Tento stejný tooread tajné klíče aplikace ve vašem trezoru, spusťte hello následující:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Pokud chcete, aby toouse modul hardwarového zabezpečení (HSM)
Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM hello. Hello moduly hardwarového zabezpečení jsou FIPS 140-2 Level 2 ověřit. Pokud tento požadavek netýká tooyou, tuto část přeskočte a přejděte příliš[odstranit hello trezoru klíčů, přidružených klíčů a tajných klíčů](#delete).

toocreate těchto klíčů chráněných pomocí HSM musíte použít hello [klíčů chráněných pomocí HSM toosupport vrstvy služby Azure Key Vault Premium](https://azure.microsoft.com/pricing/free-trial/). Kromě toho tato funkce není dostupná pro Azure China.

Při vytváření trezoru klíčů hello přidat hello **- SKU** parametr:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Můžete přidat klíče chráněné softwarem (jak jsme ukázali výše) a trezoru klíčů toothis klíčů chráněných pomocí HSM. toocreate klíč chráněný HSM, sada hello **-cílové** too'HSM parametr ':

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Můžete použít následující příkaz tooimport hello klíč z. Soubor PFX ve vašem počítači. Tento příkaz importuje hello klíč do HSM ve hello služby Key Vault:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Hello další příkaz importuje "přineste si vlastní klíč" (BYOK) balíčku. Tento scénář umožňuje vygenerovat klíč v místním HSM a jeho přenesení tooHSMs v hello služby Key Vault, bez hello klíč opustil hranice HSM hello:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Podrobnější pokyny, jak toogenerate tento balíček BYOK najdete v části [jak toogenerate a přenos klíčů chráněných pomocí HSM pro Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Odstranit hello trezoru klíčů, přidružených klíčů a tajných klíčů
Pokud již nepotřebujete trezor klíčů hello a hello klíč nebo tajný klíč, který obsahuje, můžete hello trezor klíčů odstranit pomocí hello [Remove-AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault) rutiny:

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Nebo můžete odstranit skupinu prostředků celý Azure, která zahrnuje trezor klíčů hello a další prostředky, které jste zahrnuli do této skupiny:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Další rutiny Azure PowerShellu
Další užitečné příkazy pro správu Azure Key Vault:

* `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Tento příkaz načte tabelární zobrazení všech klíčů a vybraných vlastnosti.
* `$Keys[0]`: Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč hello
* `Get-AzureKeyVaultSecret`: Tento příkaz vypíše tabelární zobrazení všech názvů tajných klíčů a vybraných vlastností.
* `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Příklad jak tooremove konkrétního klíče.
* `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Příklad jak tooremove určitého tajného klíče.

## <a id="next"></a>Další kroky
Chcete-li používat Azure Key Vault ve webové aplikaci, podívejte se na navazující kurz [Použití Azure Key Vault z webové aplikace](key-vault-use-from-web-application.md).

toosee způsobu trezoru klíčů se používá, najdete v [protokolování Azure Key Vault](key-vault-logging.md).

Seznam hello nejnovějších rutin Azure Powershellu pro Azure Key Vault najdete v tématu [rutiny Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).

Programátorské reference najdete v části [hello Příručka pro vývojáře Azure Key Vault](key-vault-developers-guide.md).
