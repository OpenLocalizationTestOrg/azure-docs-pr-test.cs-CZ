---
title: "aaaUpgrade trezoru služeb zotavení tooa trezoru zálohování (Preview) | Microsoft Docs"
description: "Pokyny a informace tooupgrade podporu trezoru zálohování Azure tooa, které trezor služeb zotavení."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
ms.assetid: 228fef19-2f6b-4067-acc3-fb6e501afb88
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/03/2017
ms.author: sogup;markgal;arunak
ms.openlocfilehash: 49062ca4556a009c82f143bb3a60ec71748bed01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-backup-vault-tooa-recovery-services-vault"></a>Upgrade trezoru zálohování tooa trezoru služeb zotavení

Tento článek vysvětluje, jak tooupgrade úložiště záloh tooa trezor služeb zotavení. proces upgradu Hello nebude mít vliv na všechny úlohy spuštění zálohování a dojde ke ztrátě žádné zálohování dat. Hello primární důvodů, proč tooupgrade tooa trezor zálohování, které trezor služeb zotavení:
- Všechny funkce úložiště záloh jsou uchovány v trezoru služeb zotavení.
- Trezory služeb zotavení mají více funkcí než trezory Backup, včetně: lepší zabezpečení, integrované monitorování, rychlejší zálohování a obnovení na úrovni položek.
- Spravujte položky zálohování z vylepšené a zjednodušená portálu.
- Nové funkce se projeví pouze tooRecovery trezory služeb.

## <a name="impact-toooperations-during-upgrade"></a>Dopad toooperations během upgradu

Při upgradu trezoru zálohování tooa trezoru služeb zotavení, neexistuje žádný vliv tooyour datové roviny operace. Všechny úlohy zálohování pokračovat, protože normální a všechny úlohy active obnovení pokračovat bez přerušení. Během upgradu hello operace správy zpoplatněná krátký výpadek a nelze chránit nové položky nebo vytvořte ad hoc úlohy zálohování. Úlohy obnovení pro virtuální počítače IaaS neběží během upgradu hello. Hello trezoru upgradu trvá v části toocomplete hodinu. Po dokončení trezoru služeb zotavení nahrazuje hello úložiště záloh.

## <a name="changes-tooyour-automation-and-tool-after-upgrading"></a>Nástroj po upgradu a změny tooyour automatizace

Během přípravy infrastruktury hello trezoru upgradu, musíte aktualizovat existující automatizace nebo tooling tooensure to pokračuje toowork po upgradu hello.
Najdete odkazy na rutiny prostředí PowerShell hello hello [modelu nasazení portálu Service Manager](backup-client-automation-classic.md) a hello [modelu nasazení Resource Manager](backup-client-automation.md).


## <a name="before-you-upgrade"></a>Před upgradem

Zkontrolujte, zda hello následující problémy před upgradem vaší trezory Backup tooRecovery trezory služby.

- **Verze agenta minimální**: tooupgrade trezoru, ujistěte se, hello Microsoft Azure Recovery Services (MARS) agenta je minimálně verze 2.0.9070.0. Pokud agenta MARS hello je starší než 2.0.9070.0, aktualizujte agenta hello před zahájením procesu upgradu hello.
- **Model fakturace na základě instance**: trezory služby zotavení podporují pouze model fakturace na základě Instance hello. Pokud máte úložiště záloh, které používá hello starší na základě úložiště fakturační model, převeďte fakturační model hello během upgradu.
- **Žádné konfigurace zálohy průběžné operace**: během upgradu je přístup toohello správu rovině s omezeným přístupem. Dokončete všechny roviny akce správy a pak spusťte hello upgrade.

## <a name="using-powershell-scripts-tooupgrade-your-vaults"></a>Pomocí vašeho trezory tooupgrade skripty prostředí PowerShell

Můžete použít tooupgrade skripty prostředí PowerShell zálohování trezory tooRecovery trezory služeb. Zkontrolujte, zda mají hello vyžaduje prostředí PowerShell součásti tootrigger hello trezoru upgradu. Skripty prostředí PowerShell pro trezory Backup nefungují pro trezory služeb zotavení. Příprava prostředí tooupgrade hello trezory:

1. Instalace nebo upgrade [Windows Management Framework (WMF) tooversion 5](https://www.microsoft.com/download/details.aspx?id=50395) nebo vyšší.
2. [Nainstalovat Azure PowerShell MSI](https://github.com/Azure/azure-powershell/releases/download/v3.8.0-April2017/azure-powershell.3.8.0.msi).
3. Stáhnout hello [skript prostředí PowerShell](https://aka.ms/vaultupgradescript2) tooupgrade vaše trezorů.

### <a name="run-hello-powershell-script"></a>Spustit skript prostředí PowerShell hello

Použijte následující skript tooupgrade hello vaší trezorů. Následující ukázkový skript Hello obsahuje vysvětlení parametrů hello.

RecoveryServicesVaultUpgrade 1.0.2.ps1 **- SubscriptionID** `<subscriptionID>` **- VaultName** `<vaultname>` **-umístění** `<location>` **- ResourceType** `BackupVault` **- TargetResourceGroupName**`<rgname>`

**ID předplatného** -hello číslo ID předplatného hello úložiště, která se upgraduje.<br/>
**VaultName** – hello název hello úložiště záloh, která se upgraduje.<br/>
**Umístění** -umístění úložiště hello probíhá upgrade.<br/>
**Typ prostředku** -použít BackupVault.<br/>
**TargetResourceGroupName** – vzhledem k tomu, že upgradujete hello trezoru tooa Resource Manager nasazení na základě, zadejte skupinu prostředků. Můžete použít existující skupinu prostředků nebo vytvořit tím, že poskytuje nový název. Pokud píšete hello název skupiny prostředků, můžete vytvořit novou skupinu prostředků. toolearn více o skupinách prostředků, přečtěte si to [přehled o skupinách prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups).

>[!NOTE]
> Názvy skupin prostředků nastavit omezení. Být jisti toofollow hello pokyny; selhání toodo tak může způsobit toofail upgrady trezoru.
>
>

Hello následující fragment kódu je příkladem jaké příkazu prostředí PowerShell by měl vypadat jako:

```
RecoveryServicesVaultUpgrade.ps1 -SubscriptionID 53a3c692-5283-4f0a-baf6-49412f5ebefe -VaultName "TestVault" -Location "Australia East" -ResourceType BackupVault -TargetResourceGroupName "ContosoRG"
```

Můžete taky spustit hello skript bez parametrů a dotaz tooprovide vstupy pro všechny požadované parametry.

Hello skript prostředí PowerShell vyzve k zadání tooenter jste svoje přihlašovací údaje. Zadejte své přihlašovací údaje dvakrát: jednou pro účet portálu Service Manager hello a ještě jednou pro hello účet správce prostředků.

### <a name="pre-requisites-checking"></a>Předpoklady kontrola
Po zadání přihlašovacích údajů Azure, Azure kontroluje, zda vaše prostředí splňuje hello následující požadavky:

- **Verze agenta minimální** -trezory služeb tooRecovery trezory Backup upgrade vyžaduje toobe agenta MARS hello alespoň verze 2.0.9070. Pokud máte položky zaregistrován tooa trezoru zálohování s agentem starší než 2.0.9070, selže kontrola předpokladů hello. Pokud selže kontrola předpokladů hello, aktualizujte hello agenta a opakujte tooupgrade hello trezoru. Můžete stáhnout nejnovější verzi agenta hello z hello [http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe](http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).
- **Úlohy konfigurace průběžné**: Pokud někdo konfiguruje úlohy pro úložiště záloh, nastavte toobe upgradovat nebo položka registrace, hello Kontrola předpokladů selže. Dokončete konfiguraci hello nebo dokončit registraci hello položky a pak spusťte procesu upgradu hello trezoru.
- **Model fakturace na základě úložiště**: obnovení služeb trezory podporu hello založený na instancích model fakturace. Pokud spouštíte upgrade hello úložiště na úložiště záloh, používá hello model fakturace na základě úložiště, jsou výzvami tooupgrade model fakturace společně s hello trezoru. Jinak můžete aktualizovat model fakturace první, a poté spusťte upgrade hello trezoru.
- Identifikujte skupinu prostředků pro trezor služeb zotavení hello. tootake výhod hello funkce nasazení Resource Manager, musíte umístit do trezoru služeb zotavení ve skupině prostředků. Pokud si nejste jisti, který toouse skupinu prostředků, zadejte název a hello procesu upgradu pro vás vytvoří hello skupinu prostředků. Hello upgradu proces také přidruží hello trezoru s hello novou skupinu prostředků.

Po upgradu hello dokončí kontrolu hello předpoklady, vyzve hello procesu můžete toostart hello trezoru upgradu. Jakmile potvrdíte, procesu upgradu hello obvykle trvá přibližně toocomplete 15-20 minut v závislosti na velikosti hello svůj trezor. Pokud máte velké trezoru, upgrade může trvat až too90 minut.

## <a name="managing-your-recovery-services-vaults"></a>Správa vašeho trezory služeb zotavení

Hello následující obrazovky zobrazit nový trezor služeb zotavení, upgradovali z trezoru služby Backup, hello portálu Azure. První obrazovka Hello ukazuje řídicí panel hello trezoru, který zobrazí klíče entity pro hello trezoru.

![Příklad trezor služeb zotavení upgradovali z úložiště záloh](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

úvodní obrazovka druhý zobrazuje odkazy na nápovědu hello k dispozici toohelp Začínáme pomocí služeb zotavení hello trezoru.

![odkazy nápovědy v okně Rychlý Start hello](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Kroků po upgradu
Trezor služeb zotavení podporuje zadání informace o časovém pásmu v zásady zálohování. Po úspěšném upgradu trezoru přejděte tooBackup zásady z nabídky Nastavení trezoru a aktualizovat informace o časovém pásmu hello hello zásady nakonfigurované v trezoru hello. Tato obrazovka ukazuje již hello plán zálohování čas zadaný jako na místní časové pásmo použít při vytvoření zásady. 

## <a name="enhanced-security"></a>Rozšířené zabezpečení

Při upgradu úložiště záloh trezor služeb zotavení tooa, jsou automaticky zapnutá hello nastavení zabezpečení pro tento trezor. Při nastavení zabezpečení hello jsou u určité operace, třeba odstranění zálohy, nebo změna přístupového hesla o vyžadují [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) PIN kód. Další informace o hello rozšířené zabezpečení, najdete v článku hello [bezpečnostní funkce zálohování hybridní tooprotect](backup-azure-security-feature.md). 

Pokud je zapnuta hello rozšířené zabezpečení, mají být uchována data si too14 dnů po hello informace o bodu obnovení se odstranil z trezoru hello. Zákazníci se účtují úložiště těchto dat zabezpečení. Uchovávání dat zabezpečení platí toorecovery body pro agenta Azure Backup hello serveru Azure Backup a System Center Data Protection Manager. 

## <a name="gather-data-on-your-vault"></a>Shromažďování dat pro váš trezoru

Po upgradu trezor služeb zotavení tooa Konfigurace sestav pro zálohování Azure (pro virtuální počítače IaaS a Microsoft Azure Recovery Services (MARS)) a pomocí sestav hello tooaccess Power BI. Další informace o shromažďování dat, najdete v článku hello, [konfigurace služby Azure Backup sestavy](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Vliv plán upgradu hello Moje probíhající zálohování?**</br>
Ne. Probíhající zálohování pokračovat bez přerušení během a po provedení upgradu.

**Pokud nemáte plánem týkající se upgradu brzy, co se stane toomy trezory?**</br>
Vzhledem k tomu, že všechny nové funkce lze použít pouze tooRecovery trezory služeb, doporučujeme vám tooupgrade vaše trezorů. Microsoft nakonec začne portálu classic hello. Od 1. září 2017 Microsoft zahájíte upgrade automaticky trezory služeb tooRecovery úložiště záloh. 2017 1 listopadu Microsoft dokončí proces upgradu hello. Váš trezor lze automaticky upgradovat kdykoli během září nebo říjen. Společnost Microsoft doporučuje, že co nejdříve upgradovat svůj trezor.

**Jaké jsou znamená to upgradu pro moje existující nástrojů?**</br>
Aktualizace modelu nasazení Resource Manager toohello nástrojů. Obnovení služby, které byly vytvořeny trezory pro použití v modelu nasazení Resource Manager hello. Plánování modelu nasazení Resource Manager hello a monitorování účtů pro hello rozdíl ve vašem trezory je důležité. 

**Během upgradu hello je k dispozici mnoho výpadku?**</br>
To závisí na hello počet prostředků, které se upgraduje. Pro menší nasazení (několik desítkami chráněné instance) by měl celou upgrade hello trvat méně než 20 minut. U větších nasazení má trvat maximálně jednu hodinu.

**Můžete I vrátit po provedení upgradu?**</br>
Ne. Vrácení zpět není podporována po úspěšném upgradu hello prostředky.

**Můžete ověřit Moje předplatné nebo prostředky toosee, pokud jsou schopné upgrade?**</br>
Ano. Hello prvním krokem při upgradu ověří, zda jsou prostředky hello schopna upgradu. V případě, že se nezdaří ověření hello požadavky, zobrazí se zprávy důvodů hello, hello upgradu nelze dokončit.

**Uvedete, jaká oprávnění mají upgrade tootrigger trezoru?**</br>
tooperform hello trezoru upgradu je třeba přidat jako spolusprávce pro předplatné hello v hello portál Azure classic. To je potřeba, i když jsou již uveden jako vlastník v hello portálu Azure. Zkuste tooadd spolusprávce k předplatnému Azure classic portálu toofind out hello, pokud jste spolusprávcem předplatného hello. Pokud si nejste možné tooadd spolusprávce, obraťte se na správce služeb nebo spolusprávce hello odběru, který můžete přidat jako spolusprávce.

**Můžete upgradovat Moje úložiště záloh na základě CSP?**</br>
Ne. V současné době nelze upgradovat na základě CSP záloh. Přidáme podpora pro upgrade trezory Backup na základě CSP v hello další verze.

**Můžete zobrazit Moje classic trezoru po upgradu?**</br>
Ne. Nelze zobrazit nebo spravovat trezoru classic po upgradu. Bude pouze možnost toouse hello nový portál Azure pro všechny akce správy na hello trezoru.

**Moje upgrade se nezdařil, ale hello počítač, který uchovávat hello agenta, které vyžadují aktualizaci, už neexistuje. Co dělat v takovém případě?**</br>
Pokud potřebujete toouse hello úložiště, hello zálohy tohoto počítače pro dlouhodobé uchovávání a potom nebude možné tooupgrade hello trezoru. V budoucích verzích přidáme podpora pro upgrade takové trezoru.
Pokud není nutné toostore hello zálohy tohoto počítače už pak prosím zrušte registraci tento počítač z trezoru hello a opakujte hello upgrade.

**Proč nevidím hello úlohy informace pro místní zdroje po upgradu**</br>
Monitorování pro místní zálohování (MARS agent aplikace DPM a Azure Backup Server) je nová funkce, kterou dostanete při upgradu trezoru služby Backup trezoru tooRecovery služby. informace o sledování Hello zabírají too12 hodin toosync službou hello.

**Jak se nahlásit problém?**</br>
Pokud jakékoli její části hello trezoru upgrade selže, Poznámka hello OperationId uvedený v chybě hello. Microsoft Support bude fungovat proaktivně tooresolve hello problém. Můžete oslovení tooSupport nebo e-mailu nás na adrese rsvaultupgrade@service.microsoft.com s ID odběru, název trezoru a OperationId. Co nejrychleji pokusíme tooresolve hello problém. Nezkoušejte zopakovat operaci hello Pokud explicitně pokyn toodo Ano společností Microsoft.


## <a name="next-steps"></a>Další kroky
Použijte následující článek a hello:</br>
[Zálohování virtuálních počítačů IaaS](backup-azure-arm-vms-prepare.md)</br>
[Zálohování serveru Azure Backup](backup-azure-microsoft-azure-backup.md)</br>
[Zálohování serveru Windows](backup-configure-vault.md).
