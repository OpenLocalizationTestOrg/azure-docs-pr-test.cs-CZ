---
title: "aaaSecure klíč trezoru | Microsoft Docs"
description: "Spravujte přístupová oprávnění pro trezor klíčů pro správu trezorů, klíčů a tajných klíčů. Ověřování a autorizace model pro trezor klíčů a jak toosecure klíč trezoru"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: 84f5fc18142a1ad89babbd11f4f65eca105afc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-key-vault"></a>Zabezpečení trezoru klíčů
Azure Key Vault je cloudová služba, která chrání šifrovací klíče a tajné klíče (například certifikáty, připojovací řetězce a hesla) a pro vaše cloudové aplikace. Vzhledem k tomu, že tato data se velká a malá písmena a business kritické, mají trezorů klíčů tooyour toosecure přístup tak, aby pouze oprávnění aplikace a uživatelé mají přístup k trezoru klíčů. Tento článek obsahuje přehled model přístupu trezoru klíčů, vysvětluje, ověřování a autorizace a popisuje, jak toosecure přístup tookey trezoru pro cloudové aplikace se příklad.

## <a name="overview"></a>Přehled
Trezor klíčů tooa přístup je řízen pomocí dvou samostatných rozhraní: správy a rovinu data. Pro obě roviny správné ověřování a autorizace je vyžadována před volající (uživatele nebo aplikace) můžete získat přístup k tookey trezoru. Ověřování prokáže hello identitu volajícího hello při autorizaci Určuje, jaké operace hello volající smí tooperform.

K ověření využívají rovina správy i rovina dat službu Azure Active Directory. K autorizaci ale rovina správy používá řízení přístupu podle role (RBAC), zatímco rovina dat používá zásady přístupu trezoru klíčů.

Zde je stručný přehled hello témata:

[Ověřování pomocí služby Azure Active Directory](#authentication-using-azure-active-directory) – Tato část vysvětluje, jak volající ověřuje s Azure Active Directory tooaccess trezoru klíčů přes správu a rovinu data. 

[Rovina správy a rovina dat](#management-plane-and-data-plane): Rovina správy a rovina dat jsou dvě roviny přístupu, které se používají pro přístup k vašemu trezoru klíčů. Každá rovina podporuje určité operace. Tato část popisuje hello přístup koncových bodů, operace podporované a přístup k řízení metodu používanou pro každé plochy. 

[Správa řízení přístupu roviny](#management-plane-access-control) – v této části se podíváme povolením přístupu toomanagement roviny operace pomocí řízení přístupu na základě rolí.

[Ovládací prvek pro datové roviny přístup](#data-plane-access-control) – Tato část popisuje, jak data toocontrol zásad přístupu k trezoru klíčů toouse roviny přístup.

[Příklad](#example) – tento příklad popisuje, jak toosetup přistupovat k řízení pro váš trezor klíčů tooallow tři různé týmy (tým pro zabezpečení, vývojáři nebo operátory a auditoři) tooperform toodevelop konkrétní úlohy, spravovat a monitorovat aplikace v Azure .

## <a name="authentication-using-azure-active-directory"></a>Ověření pomocí služby Azure Active Directory
Při vytváření trezoru klíčů v předplatné Azure, se automaticky přidruží hello předplatného klienta Azure Active Directory. Všechny volající (uživatelé a aplikace) musí být zaregistrovaný v této klienta tooaccess tímto trezorem klíčů. Aplikace nebo uživatel musí provést ověření pomocí klíče trezoru tooaccess Azure Active Directory. To platí tooboth správy a datovou rovinu přístup. V obou případech může aplikace přistupovat k trezoru klíčů dvěma způsoby:

* **přístup uživatele a aplikace** – obvykle se používá pro aplikace, které přistupují k trezoru klíčů jménem přihlášeného uživatele. Příklady tohoto typu přístupu jsou Azure PowerShell a Azure Portal. Existují dva způsoby toogrant přístup toousers: jedním ze způsobů je toogrant přístup toousers tak získají přístup k trezoru klíčů ze všech aplikací a hello jiný způsob je toogrant trezoru tookey přístup uživatele jenom v případě, že používají konkrétní aplikaci (označují tooas složená identita). 
* **přístup jen aplikace** – pro aplikace, spouštění služeb démon, úlohy na pozadí identitu aplikace hello atd., jsou udělena přístup toohello klíče trezoru.

V obou typech aplikací, aplikace hello ověřuje s Azure Active Directory pomocí kteréhokoli hello [podporované metody ověřování](../active-directory/active-directory-authentication-scenarios.md) a získá token. Použitá metoda ověřování závisí na typu aplikace hello. Pak aplikace hello používá tento token a odešle trezoru tookey požadavku REST API. V případě správy roviny přístup hello požadavky jsou směrovány prostřednictvím koncového bodu Azure Resource Manager. Při přístupu k rovině data, aplikace hello komunikuje přímo koncový bod tooa trezoru klíčů. Další informace najdete na hello [tok ověřování celou](../active-directory/active-directory-protocols-oauth-code.md). 

název prostředku Hello, pro které aplikace hello požaduje token se liší v závislosti na tom, jestli aplikace hello je přístup k správu roviny nebo roviny data. Proto se název prostředku hello buď roviny nebo data roviny koncový bod správy popsané v tabulce hello v další části, v závislosti na hello prostředí Azure.

Jeden jeden mechanismus pro ověřování tooboth Správa a datové roviny má svou vlastní výhody:

* Organizace mohou centrálně řídit přístup tooall trezorů klíčů v organizaci
* Pokud uživatel odejde, se okamžitě ztratit přístup tooall trezorů klíčů v organizaci hello
* Organizace můžete přizpůsobit ověřování prostřednictvím možnosti hello v Azure Active Directory (například povolení služby Multi-Factor authentication pro zvýšení zabezpečení)

## <a name="management-plane-and-data-plane"></a>Rovina správy a rovina dat
Azure Key Vault je služba Azure, která je dostupná prostřednictvím modelu nasazení Azure Resource Manager. Když vytvoříte trezor klíčů, získáte virtuální kontejner, ve kterém můžete vytvářet další objekty, jako jsou klíče, tajné klíče a certifikáty. Pak máte přístup k trezoru klíčů pomocí roviny a datové roviny tooperform konkrétní operace správy. Rozhraní pro správu roviny je použité toomanage klíč trezoru samostatně, jako je například vytváření, odstraňování, aktualizaci atributů trezoru klíčů a nastavení zásad přístupu pro datové roviny. Rozhraní roviny dat je použité tooadd, odstranit, upravit a použít hello klíčů, tajných klíčů a certifikátů uložené v trezoru klíčů.

Hello roviny a datové roviny rozhraní pro správu jsou přístupné prostřednictvím různých koncových bodů (viz tabulka). Hello druhý sloupec v tabulce hello popisuje hello názvy DNS pro tyto koncové body v různých prostředích Azure. třetí sloupec Hello popisuje hello operace, které můžete provést z každé plochy přístup. Každá rovina přístupu má také vlastní mechanismus řízení přístupu: pro rovinu správy se řízení přístupu nastavuje pomocí služby řízení přístupu podle role (RBAC) služby Azure Resource Manager, pro rovinu dat se řízení přístupu nastavuje pomocí zásad přístupu trezoru klíčů.

| Rovina přístupu | Koncové body přístupu | Operace | Mechanismus řízení přístupu |
| --- | --- | --- | --- |
| Rovina správy |**Globální:**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> management.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> management.microsoftazure.de:443 |Vytvořit (create), číst (read), aktualizovat (update), odstranit (delete) trezor klíčů <br> Nastavit zásady přístupu pro trezor klíčů<br>Nastavit značky pro trezor klíčů |Řízení přístupu podle role (RBAC) služby Azure Resource Manager |
| Rovina dat |**Globální:**<br> &lt;název_trezoru&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;název_trezoru&gt;.vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;název_trezoru&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> &lt;název_trezoru&gt;.vault.microsoftazure.de:443 |Pro klíče: dešifrovat (decrypt), zašifrovat (encrypt), rozbalit (UnwrapKey), zabalit (WrapKey), ověřit (verify), podepsat (sign), získat (get), vypsat (list), aktualizovat (update), vytvořit (create), importovat (import), odstranit (delete), zálohovat (backup), obnovit (restore)<br><br> Pro tajné klíče: získat (get), vypsat (list), nastavit (set), odstranit (delete) |Zásady přístupu trezoru klíčů |

Hello správu roviny a datové roviny řízení přístupu fungovat nezávisle. Pokud chcete toogrant aplikace přístupových toouse klíčů v trezoru klíčů, stačí pouze toogrant datové roviny přístupová oprávnění pomocí zásad přístupu k trezoru klíčů a je potřeba žádné roviny přístup pro správu pro tuto aplikaci. A naopak, pokud chcete toobe uživatele možné tooread trezoru vlastnosti a značky, ale nemá žádné tookeys přístup, tajné klíče ani certifikáty, můžete udělit pro tohoto uživatele, je vyžadován přístup "read" pomocí RBAC a roviny toodata žádný přístup.

## <a name="management-plane-access-control"></a>Řízení přístupu roviny správy
Hello správu roviny se skládá z operace, které ovlivňují hello trezoru klíčů, sám sebe. Můžete například vytvořit nebo odstranit trezor klíčů. Můžete získat seznam trezorů klíčů v určitém předplatném. Můžete načíst vlastnosti trezoru klíčů (například SKU, značky) a nastavit zásady přístupu, které řídí hello uživatelé a aplikace, které mají přístup k klíčů a tajných klíčů v trezoru klíčů hello trezoru klíčů. Řízení přístupu roviny správy používá RBAC. Viz hello úplný seznam operací trezoru klíčů, které je možné provádět prostřednictvím správy roviny v tabulce hello v předchozím oddílu. 

### <a name="role-based-access-control-rbac"></a>Řízení přístupu podle role (RBAC)
Každé předplatné Azure zahrnuje službu (adresář) Azure Active Directory. Uživatelé, skupiny a aplikace z tohoto adresáře můžete udělit přístup k prostředkům toomanage v hello předplatné Azure, které používají model nasazení Azure Resource Manager hello. Tento typ řízení přístupu se označují tooas řízení přístupu na základě Role (RBAC). toomanage tento přístup, můžete použít hello [portál Azure](https://portal.azure.com/), hello [nástrojů příkazového řádku Azure](../cli-install-nodejs.md), [prostředí PowerShell](/powershell/azureps-cmdlets-docs), nebo hello [rozhraní REST APIAzureResourceManager](https://msdn.microsoft.com/library/azure/dn906885.aspx).

S hello modelu Azure Resource Manager je vytvoření trezoru klíčů v prostředku skupiny a řízení přístupu toohello správu rovině klíče trezoru pomocí služby Azure Active Directory. Například můžete udělit uživatelům nebo skupiny možnost toomanage trezorů klíčů v určité skupiny zdrojů.

Přiřazením příslušné role RBAC můžete udělit přístup toousers, skupin a aplikací na konkrétní obor. Například toogrant přístup tooa uživatele toomanage trezorů klíčů předdefinované role 'trezoru klíčů přispěvatele, bude přiřazen toothis uživatele v konkrétní obor. Hello oboru v takovém případě bude předplatné, skupinu prostředků nebo jenom určité trezoru klíčů. Role přiřazené na úrovni předplatného se vztahuje tooall skupiny prostředků a prostředky v rámci tohoto předplatného. Role přiřazené na úrovni skupiny prostředků se vztahuje tooall prostředky v příslušné skupině prostředků. Role přiřazené pro určitý prostředek se vztahuje pouze na toothat prostředků. Existuje několik předdefinovaných rolí (viz [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md)), a pokud hello předdefinované role nebudou vyhovovat vašim potřebám, je možné definovat také vlastní role.

> [!IMPORTANT]
> Všimněte si, že pokud má uživatel Přispěvatel oprávnění (RBAC) tooa trezoru klíčů správu rovině, Jana můžete udělit samu sebe roviny toodata přístup, nastavením zásad pro přístup k trezoru klíčů, který řídí roviny toodata přístup. Proto se doporučuje tootightly určit, kdo má 'Přispěvatel' přístup tooyour trezorů klíčů tooensure v pouze oprávněných osob můžete otvírat a spravovat trezorů klíčů, klíčů, tajných klíčů a certifikátů.
> 
> 

## <a name="data-plane-access-control"></a>Řízení přístupu roviny dat
Hello trezoru klíčů datové roviny se skládá z operace, které ovlivňují objekty hello v trezoru klíčů, jako je například klíčů, tajných klíčů a certifikátů.  To zahrnuje operace s klíči, jako je vytvoření, import, aktualizace, výpis, zálohování a obnova klíčů, kryptografické operace jako podepsání, ověření, zašifrování, dešifrování, zabalení a rozbalení a nastavení značek a dalších atributů pro klíče. Pro tajné klíče sem podobně patří operace získat, nastavit, vypsat a odstranit.

Přístup k rovině dat je udělován nastavením zásad přístupu pro trezor klíčů. Uživatele, skupiny nebo aplikace, musí mít oprávnění přispěvatele (RBAC) pro správu roviny pro trezoru klíčů toobe možné tooset zásady přístupu pro tento trezor klíčů. Uživatele, skupiny nebo aplikace můžete udělit přístup tooperform konkrétních operací pro klíčů nebo tajných klíčů v trezoru klíčů. Trezor klíčů podpora až too16 položek zásad přístupu pro trezoru klíčů. Vytvoření skupiny zabezpečení služby Azure Active Directory a přidání uživatelů toothat skupiny toogrant data roviny přístup tooseveral uživatelé tooa trezoru klíčů.

### <a name="key-vault-access-policies"></a>Zásady přístupu trezoru klíčů
Zásady přístupu k trezoru klíčů udělit oprávnění tookeys, tajných klíčů a certifikátů samostatně. Například můžete udělit klíče tooonly přístup uživatele, ale žádná oprávnění pro tajných klíčů. Oprávnění tooaccess klíčů nebo tajných klíčů nebo certifikáty, ale jsou na úrovni hello trezoru. Zásady přístupu trezoru klíčů tedy nepodporují oprávnění na úrovni objektu. Můžete použít [portál Azure](https://portal.azure.com/), hello [nástrojů příkazového řádku Azure](../cli-install-nodejs.md), [prostředí PowerShell](/powershell/azureps-cmdlets-docs), nebo hello [trezoru klíčů rozhraní REST API pro správu](https://msdn.microsoft.com/library/azure/mt620024.aspx) tooset zásady přístupu pro trezoru klíčů.

> [!IMPORTANT]
> Nezapomeňte, že zásady přístupu k trezoru klíčů platí na úrovni hello trezoru. Například když uživatel jsou udělena oprávnění toocreate a odstranění klíče, mohla provádět tyto operace na všechny klíče v tomto trezoru klíčů.
> 
> 

## <a name="example"></a>Příklad
Řekněme, že vyvíjíte aplikaci, která používá certifikát pro SSL, službu Azure Storage k ukládání dat a používá 2048bitový klíč RSA pro podpisové operace. A tato aplikace běží ve virtuálním počítači (nebo ve škálovací sadě virtuálních počítačů). Můžete použít trezoru klíčů toostore všechny hello tajné klíče aplikace a použití klíče trezoru toostore hello bootstrap certifikát, který je používán hello tooauthenticate aplikací s Azure Active Directory.

Ano zde je souhrn všech hello klíče a tajné klíče toobe uloženého v trezoru klíčů.

* **Certifikát SSL** – používá se pro SSL.
* **Klíč k úložišti** -použil tooget přístup tooStorage účet
* **2048bitový klíč RSA** – používá se pro podpisové operace.
* **Zavedení certifikátu** -použít tooauthenticate tooAzure služby Active Directory, tooget přístup tookey trezoru toofetch hello klíč úložiště a klíč RSA hello použít pro podepisování.

Nyní Pojďme splňovat hello lidé, kteří jsou správě, nasazení a auditování této aplikace. V tomto příkladu použijeme tři role.

* **Tým pro zabezpečení** – obvykle se jedná o pracovníky IT z hello "úřad hello CSO (ředitel zabezpečení)" nebo ekvivalentní, zodpovědná za hello správné udržován tajné klíče, jako certifikáty protokolu SSL, použít pro podepisování připojovací řetězce pro klíče RSA databáze, klíče účtu úložiště.
* **Vývojáři nebo operátory** – jedná se o hello zaměstnance, kteří vyvíjet tuto aplikaci a poté ji nasadit v Azure. Obvykle nejsou součástí týmu hello zabezpečení a proto by neměl mít přístup tooany citlivých dat, jako třeba certifikáty protokolu SSL, klíče RSA, ale aplikace hello nasadí by měl mít toothose přístup.
* **Auditory** – to je obvykle jinou sadu uživatelů, které jsou izolovány od vývojáře hello a obecné zaměstnanců IT. Zodpovídají je tooreview správné použití a správa certifikátů, klíče, atd. a dodržování standardů zabezpečení data. 

Je jeden další role, která je mimo rozsah hello této aplikace, ale příslušné sem toobe uvedených a který bude předplatné hello (nebo skupinu prostředků) správce. Nastaví počáteční přístupová oprávnění pro tým zabezpečení hello správce předplatného. Zde předpokládáme, že správce předplatného hello udělil přístup toohello team tooa prostředků skupiny zabezpečení ve které všechny prostředky potřebné k této aplikaci bydliště hello.

Nyní si ukážeme, jaké akce provede každou roli v kontextu hello této aplikace.

* **Bezpečnostní tým**
  * Vytváření trezorů klíčů
  * Zapnutí protokolování trezoru klíčů
  * Přidání klíčů / tajných klíčů
  * Vytvoření zálohy klíčů pro zotavení po havárii
  * Nastavení přístupu trezoru klíčů zásad toogrant oprávnění toousers a aplikace tooperform konkrétních operací
  * Pravidelná obměna klíčů / tajných klíčů
* **Vývojáři/operátoři**
  * Získat odkazy toobootstrap a certifikátů protokolu SSL (kryptografické otisky), klíč k úložišti (tajný klíč URI) a podpisový klíč (klíč URI) od týmu zabezpečení
  * Vývoj a nasazení aplikace, která přistupuje ke klíčům a tajným klíčům prostřednictvím programového kódu
* **Auditoři**
  * Zkontrolujte využití protokolů tooconfirm správné použití klíče nebo tajného klíče a dodržování standardů zabezpečení dat

Nyní si ukážeme, jaký přístup oprávnění tookey trezoru jsou vyžadovány všechny role (a aplikace hello) tooperform jejich přiřazené úlohy. 

| Role uživatele | Oprávnění k rovině správy | Oprávnění k rovině dat |
| --- | --- | --- |
| Bezpečnostní tým |Přispěvatel trezoru klíčů |Klíče: zálohovat (backup), vytvořit (create), odstranit (delete), získat (get), importovat (import), vypsat (list), obnovit (restore) <br> Tajné klíče: vše |
| Vývojáři/operátoři |Trezor klíčů nasazení oprávnění tak, aby virtuální počítače hello nasadí můžete načíst tajné klíče z trezoru klíčů hello |Žádný |
| Auditoři |Žádný |Klíče: vypsat (list)<br>Tajné klíče: vypsat (list) |
| Aplikace |Žádný |Klíče: podepsat (sign)<br>Tajné klíče: získat (get) |

> [!NOTE]
> Auditory potřebovat seznam oprávnění pro klíče a tajné klíče, navrhují atributy pro klíče a tajné klíče, které nejsou vygenerované v hello protokoly, jako je například značky, aktivace a datum vypršení platnosti.
> 
> 

Kromě oprávnění tookey trezoru všechny tři role také potřebovat přístup k tooother prostředkům. Například toobe možné toodeploy virtuální počítače (nebo webové aplikace atd.) Vývojáři nebo operátory také potřebovat typy prostředků toothose přístup, Přispěvatel'. Auditory potřebovat účet úložiště toohello přístup pro čtení, kde jsou uloženy protokoly trezoru klíčů hello.

Vzhledem k tomu, že hello fokus tohoto článku je zabezpečení přístupu k trezoru klíčů tooyour, jsme pouze ilustrovat hello příslušné části tématu toothis, která se týkají a Přeskočit podrobnosti týkající se nasazení certifikátů, přístup k klíče a tajné klíče prostřednictvím kódu programu atd. Tyto podrobnosti jsou popsány v jiných článcích. Nasazení certifikátů uložené v trezoru klíčů tooVMs, najdete v článku [příspěvku na blogu](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)a je [ukázkový kód](https://www.microsoft.com/download/details.aspx?id=45343) k dispozici, ukazuje, jak toouse zavedení certifikátu tooget tooauthenticate tooAzure AD Trezor tookey přístup.

Většina hello přístupová oprávnění lze udělit pomocí portálu Azure, ale může být nutné toouse prostředí Azure PowerShell (nebo Azure CLI) tooachieve hello oprávnění na podrobné úrovni toogrant požadovaného výsledku. 

Předpokládejme, Hello následující fragmenty prostředí PowerShell:

* Správce služby Azure Active Directory Hello vytvořil skupiny zabezpečení, které představují hello tři role, konkrétně tým pro zabezpečení společnosti Contoso, Devops aplikace Contoso, auditory aplikace Contoso. Hello správce má také přidat uživatele toohello skupiny, ke kterým patří.
* **ContosoAppRG** je skupina prostředků hello, kde jsou umístěny všechny prostředky hello. **contosologstorage** je, kde jsou uloženy protokoly hello. 
* Trezor klíčů **ContosoKeyVault** a účet úložiště pro protokoly trezoru klíčů **contosologstorage** musí být v hello stejného umístění Azure

První správce předplatného hello přiřadí 'klíče trezoru přispěvatele a tým zabezpečení toohello role správce přístupu uživatelů. To umožňuje hello zabezpečení team toomanage přístup tooother prostředků a spravovat trezorů klíčů ve skupině prostředků hello ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Hello následující skript ukazuje, jak můžete tým zabezpečení hello vytvoření trezoru klíčů, nastavení protokolování a nastavte přístupová oprávnění pro další role a aplikace hello. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role tooDev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

vlastní role Hello definován, je pouze přiřadit toohello předplatné, kde se má vytvořit skupinu prostředků ContosoAppRG hello. Pokud hello stejné vlastní role se použije pro další projekty v jiných předplatných, můžou mít její obor přidat více odběrů.

přiřazení vlastní role Hello hello vývojáři nebo operátorů oprávnění "nasazení nebo action" hello je vymezená toohello skupinu prostředků. Tímto způsobem pouze virtuální počítače hello vytvořit ve skupině prostředků hello 'ContosoAppRG' získají hello tajné klíče (certifikát SSL a zavedení certifikátu). Všechny virtuální počítače, které vytvoří člen týmu dev/ops v jiné skupině prostředků nebude možné tooget těchto tajných klíčů i v případě, že znal hello tajný klíč identifikátory URI.

Tento příklad znázorňuje jednoduchý scénář. Scénáře reálného života může být složitější a může být nutné tooadjust oprávnění tooyour trezoru klíčů na základě potřeb. Například v našem příkladu předpokládáme, že bude tento tým zabezpečení poskytovat hello klíče a tajné odkazy (identifikátory URI a kryptografické otisky), že vývojáři nebo operátory team nutné tooreference ve svých aplikacích. Proto nemusí toogrant vývojáři nebo operátory žádné přístup k datům roviny. Také připomínáme, že tento příklad se zaměřuje na zabezpečení trezoru klíčů. Podobně jako třeba zvážit toosecure [virtuální počítače](https://azure.microsoft.com/services/virtual-machines/security/), [účty úložiště](../storage/common/storage-security-guide.md) a dalším prostředkům služby Azure příliš.

> [!NOTE]
> Poznámka: Tento příklad ukazuje, jak bude přístup k trezoru klíčů uzamčen v produkčním prostředí. Vývojáři Hello by měl mít vlastní předplatné nebo resourcegroup které mají úplná oprávnění toomanage jejich trezory, virtuální počítače a účet úložiště kde vyvíjejí aplikace hello.
> 
> 

## <a name="resources"></a>Zdroje
* [Řízení přístupu na základě role v Azure Active Directory](../active-directory/role-based-access-control-configure.md)
  
  Tento článek vysvětluje hello řízení přístupu na základě Role v Azure Active Directory a jak to funguje.
* [RBAC: vestavěné role](../active-directory/role-based-access-built-in-roles.md)
  
  Tento článek podrobnosti všechny hello vestavěné role, které jsou k dispozici v RBAC.
* [Principy nasazení podle modelu Resource Manager a klasického nasazení](../azure-resource-manager/resource-manager-deployment-model.md)
  
  Tento článek popisuje nasazení Resource Manager hello a modely nasazení classic a vysvětluje hello výhody použití Resource Manager a prostředek skupiny hello
* [Správa řízení přístupu na základě role pomocí Azure PowerShellu](../active-directory/role-based-access-control-manage-access-powershell.md)
  
  Tento článek vysvětluje, jak toomanage na základě rolí přistupovat k řízení pomocí prostředí Azure PowerShell
* [Správa řízení přístupu na základě rolí pomocí rozhraní REST API hello](../active-directory/role-based-access-control-manage-access-rest.md)
  
  Tento článek ukazuje, jak toouse hello REST API toomanage RBAC.
* [Řízení přístupu na základě role pro Microsoft Azure z Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Toto je odkaz tooa, který je na webu Channel 9 video z konference Ignite 2015 MS hello. V této relaci se mluvit o přístup k správy a možnosti vytváření sestav v Azure a zkoumat osvědčených postupů zabezpečení přístupu tooAzure odběry pomocí služby Azure Active Directory.
* [Autorizace přístupu tooweb aplikací pomocí OAuth 2.0 a Azure Active Directory](../active-directory/active-directory-protocols-oauth-code.md)
  
  Tento článek popisuje úplný proces OAuth 2.0 pro ověřování v Azure Active Directory.
* [Rozhraní REST API správy trezoru klíčů](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Tento dokument je hello odkaz pro toomanage rozhraní REST API hello klíč trezoru prostřednictvím kódu programu, včetně nastavení zásad přístupu k trezoru klíčů.
* [Rozhraní REST API trezoru klíčů](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Odkaz tookey trezor referenční dokumentace rozhraní API REST.
* [Řízení přístupu ke klíčům](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Odkaz tooSecret přístup řízení referenční dokumentaci k nástroji.
* [Řízení přístupu k tajným klíčům](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Odkaz tooKey přístup řízení referenční dokumentaci k nástroji.
* [Nastavení](https://msdn.microsoft.com/library/mt603625.aspx) a [odebrání](https://msdn.microsoft.com/library/mt619427.aspx) zásady přístupu trezoru klíčů pomocí PowerShellu
  
  Odkazy tooreference dokumentace pro zásady přístupu trezoru klíčů toomanage rutiny prostředí PowerShell.

## <a name="next-steps"></a>Další kroky
Úvodní kurz pro správce najdete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md).

Další informace o protokolování využití trezoru klíčů najdete v tématu [Protokolování Azure Key Vault](key-vault-logging.md).

Další informace o používání klíčů a tajných klíčů se službou Azure Key Vault najdete v tématu [Informace o klíčích a tajných klíčích](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Pokud máte dotazy k trezoru klíčů, navštivte hello [Azure trezoru klíčů fóra](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

