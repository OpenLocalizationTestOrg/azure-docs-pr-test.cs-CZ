---
title: Synchronizace hesel aaaTroubleshoot s Azure AD Connect sync | Microsoft Docs
description: "Tento článek obsahuje informace o tom, tootroubleshoot heslo synchronizace problémy."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 390eafec792cb39251627c14cb754f8bb30035b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a>Řešení potíží s synchronizace hesel s synchronizace Azure AD Connect
Toto téma popisuje kroky pro jak tootroubleshoot problémy se synchronizace hesel. Pokud hesla se nesynchronizují podle očekávání, může být buď pro podmnožinu uživatelů, nebo pro všechny uživatele. Pro Azure Active Directory (Azure AD) připojit nasazení s verzí 1.1.524.0 nebo novější, je nyní k dispozici diagnostické rutina, které můžete použít tootroubleshoot heslo synchronizace problémy:

* Pokud máte potíže, kde jsou synchronizovány žádná hesla, přečtěte si téma toohello [bez hesla se synchronizují: řešení potíží pomocí diagnostiky rutiny hello](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) části.

* Pokud máte potíže s jednotlivé objekty, přečtěte si téma toohello [jeden objekt není synchronizace hesel: řešení potíží pomocí diagnostiky rutiny hello](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) části.

Pro starší verze aplikace Azure AD Connect nasazení:

* Pokud máte potíže, kde jsou synchronizovány žádná hesla, přečtěte si téma toohello [bez hesla se synchronizují: Ruční postup řešení potíží](#no-passwords-are-synchronized-manual-troubleshooting-steps) části.

* Pokud máte potíže s jednotlivé objekty, přečtěte si téma toohello [jeden objekt není synchronizace hesel: Ruční postup řešení potíží](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) části.

## <a name="no-passwords-are-synchronized-troubleshoot-by-using-hello-diagnostic-cmdlet"></a>Žádná hesla se synchronizují: řešení potíží pomocí diagnostiky rutiny hello
Můžete použít hello `Invoke-ADSyncDiagnostics` toofigure rutiny se proč bez hesla se synchronizují.

> [!NOTE]
> Hello `Invoke-ADSyncDiagnostics` rutina je dostupné pouze pro verze služby Azure AD Connect 1.1.524.0 nebo novější.

### <a name="run-hello-diagnostics-cmdlet"></a>Spusťte rutinu diagnostiky hello
tootroubleshoot problémy, kde jsou synchronizovány žádná hesla:

1. Otevřete novou relaci prostředí Windows PowerShell na serveru Azure AD Connect s hello **spustit jako správce** možnost.

2. Spustit `Set-ExecutionPolicy RemoteSigned` nebo `Set-ExecutionPolicy Unrestricted`.

3. Spusťte `Import-Module ADSyncDiagnostics`.

4. Spusťte `Invoke-ADSyncDiagnostics -PasswordSync`.

### <a name="understand-hello-results-of-hello-cmdlet"></a>Pochopení hello výsledky rutiny hello
diagnostické rutiny Hello provádí hello následující kontroly:

* Ověří, že hello funkce Synchronizace hesla je povolený pro vašeho klienta Azure AD.

* Ověří, že hello Azure AD Connect server není v pracovním režimu.

* Pro každý existující místní konektor služby Active Directory (který odpovídá tooan existující doménové struktury služby Active Directory):

   * Ověří, že hello je povolena funkce Synchronizace hesla.
   
   * Vyhledá události prezenční signál synchronizace hesel v hello v protokolech událostí aplikace systému Windows.

   * Pro každou doménu služby Active Directory v části konektor služby Active Directory v místě hello:

      * Ověří, že hello doména je dostupný ze serveru hello Azure AD Connect.

      * Ověří, zda má účty služby Active Directory Domain Services (AD DS) hello používá konektor služby Active Directory v místě hello hello správné uživatelské jméno, heslo a oprávněních pro synchronizaci hesel.

Hello následující diagram znázorňuje hello výsledky hello rutiny pro topologie s jednou doménou, místní služby Active Directory:

![Výstup diagnostiky pro synchronizace hesel](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

Hello zbývající část tohoto oddílu popisuje konkrétní výsledky, které se vrátí pomocí rutiny hello a odpovídající problémy.

#### <a name="password-synchronization-feature-isnt-enabled"></a>Funkce Synchronizace hesla není povoleno.
Pokud jste nepovolili synchronizace hesel pomocí Průvodce hello Azure AD Connect, je vrácena hello následující chybě:

![Synchronizace hesla není povoleno.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a>Server Azure AD Connect je v pracovním režimu
Pokud server hello Azure AD Connect v pracovním režimu, synchronizace hesel je dočasně zakázána a hello vrácena následující chyba je:

![Server Azure AD Connect je v pracovním režimu](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a>Žádné události prezenční signál synchronizace hesel
Každý konektor služby Active Directory v místě má svou vlastní heslo synchronizačního kanálu. Při kanál synchronizace hesla hello je vytvořen a nejsou k dispozici žádné toobe změny hesla synchronizován, prezenční signál události (EventId 654) generuje každých 30 minut v protokolu událostí aplikace systému Windows hello. Pro každý konektor služby Active Directory v místě hello rutina vyhledá odpovídající události prezenčního signálu v hello posledních tří hodin. Pokud se nenajde žádný prezenční signál události, hello vrácena následující chyba je:

![Žádné vysílat synchronizace hesla porazit událostí](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a>AD DS účet nemá správná oprávnění
Pokud účet hello služby AD DS, který je používán hello místně hodnot hash hesel služby Active Directory connector toosynchronize nemá příslušná oprávnění hello hello vrácena následující chyba je:

![Nesprávná pověření](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a>Nesprávné uživatelské jméno účtu služby AD DS nebo heslo
Pokud hello účet služby AD DS používá má nesprávné uživatelské jméno nebo heslo zprostředkovatele hello místní služby Active Directory connector toosynchronize hodnot hash hesel, hello vrácena následující chyba je:

![Nesprávná pověření](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)

## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-hello-diagnostic-cmdlet"></a>Jeden objekt není synchronizace hesel: řešení potíží pomocí diagnostiky rutiny hello
Můžete použít hello `Invoke-ADSyncDiagnostics` rutiny toodetermine Proč není jeden objekt synchronizace hesel.

> [!NOTE]
> Hello `Invoke-ADSyncDiagnostics` rutina je dostupné pouze pro verze služby Azure AD Connect 1.1.524.0 nebo novější.

### <a name="run-hello-diagnostics-cmdlet"></a>Spusťte rutinu diagnostiky hello
tootroubleshoot problémy, kde jsou synchronizovány žádná hesla:

1. Otevřete novou relaci prostředí Windows PowerShell na serveru Azure AD Connect s hello **spustit jako správce** možnost.

2. Spustit `Set-ExecutionPolicy RemoteSigned` nebo `Set-ExecutionPolicy Unrestricted`.

3. Spusťte `Import-Module ADSyncDiagnostics`.

4. Spusťte následující rutinu hello:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   Například:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```

### <a name="understand-hello-results-of-hello-cmdlet"></a>Pochopení hello výsledky rutiny hello
diagnostické rutiny Hello provádí hello následující kontroly:

* Prozkoumá hello stav objektu služby Active Directory hello hello prostoru konektoru služby Active Directory, úložiště Metaverse a Azure AD prostoru konektoru.

* Ověří, že jsou synchronizační pravidla, jejichž synchronizace hesel povolena a použít toohello objektu služby Active Directory.

* Pokusy o tooretrieve a zobrazení výsledků hello hello poslední pokus o toosynchronize hello hesla pro objekt hello.

Hello následující diagram znázorňuje hello výsledky rutiny hello při řešení potíží s synchronizace hesel pro jediný objekt:

![Výstup diagnostiky pro synchronizaci hesel - jednoho objektu.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

Hello zbývající část tohoto oddílu popisuje konkrétní výsledků vrácených rutinou hello a odpovídající problémy.

#### <a name="hello-active-directory-object-isnt-exported-tooazure-ad"></a>Hello objektu služby Active Directory není exportovaný tooAzure AD
Synchronizace hesel pro tento účet místní služby Active Directory se nezdaří, protože neexistuje žádný odpovídající objektů v klientovi Azure AD hello. se vrátí Hello následující chybě:

![Chybí objekt Azure AD](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a>Uživatel má dočasné heslo.
Azure AD Connect v současné době nepodporuje synchronizaci dočasná hesla s Azure AD. Heslo je považován za toobe dočasné Pokud hello **při dalším přihlášení změnit heslo** je možnost nastavena na uživatele služby Active Directory v místě hello. se vrátí Hello následující chybě:

![Dočasné heslo není exportovali.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-toosynchronize-password-arent-available"></a>Nejsou k dispozici, výsledky poslední pokus o toosynchronize heslo
Ve výchozím nastavení ukládá Azure AD Connect hello výsledky pokusů o zadání synchronizace hesla sedm dní. Pokud nejsou k dispozici pro objekt služby Active Directory hello vybrané žádné výsledky, je vrácena hello následující upozornění:

![Výstup diagnostiky pro jediný objekt - žádná historie synchronizace hesla](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)


## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a>Žádná hesla se synchronizují: Ruční postup řešení potíží
Postupujte podle těchto kroků toodetermine, proč jsou synchronizovány žádná hesla:

1. Je server hello Connect v [pracovním režimu](active-directory-aadconnectsync-operations.md#staging-mode)? Server v pracovní režimu nebyla synchronizována všechna hesla.

2. Spusťte skript hello hello [získat stav hello nastavení synchronizace hesla](#get-the-status-of-password-sync-settings) části. Poskytuje přehled o konfiguraci synchronizace hesel hello.  

    ![Výstup skriptu prostředí PowerShell z nastavení synchronizace hesla](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. Pokud není povolená funkce hello ve službě Azure AD, nebo pokud není povolené stav kanál synchronizace hello, spusťte Průvodce instalací Connect hello. Vyberte **přizpůsobit možnosti synchronizace**a zrušte výběr synchronizaci hesel. Tato změna dočasně zakáže funkci hello. Potom spusťte znovu Průvodce hello a znovu povolte synchronizaci hesla. Spusťte skript hello znovu tooverify, který hello konfigurace je správná.

4. Vyhledejte v protokolu událostí hello chyby. Podívejte se na následující události, které by mohly ukazovat na problém hello:
    * Zdroje: ID "Synchronizace adresáře": 0, 611, 652, 655 těchto událostí došlo k potížím. připojení. zprávy protokolu událostí Hello obsahuje informace doménové struktury, kde došlo k potížím. Další informace najdete v tématu [problém s připojením k](#connectivity problem).

5. Pokud se neobjevil žádný prezenční signál nebo nic jiného fungovala, spusťte [spustit úplnou synchronizaci hesel všech](#trigger-a-full-sync-of-all-passwords). Spusťte skript hello pouze jednou.

6. V tématu hello [řešení potíží s jeden objekt, který není synchronizace hesel](#one-object-is-not-synchronizing-passwords) části.

### <a name="connectivity-problems"></a>Potíže s připojením k

Máte připojení k Azure AD?

Účet hello mají požadovaná oprávnění tooread hello hodnot hash hesel ve všech doménách? Pokud jste nainstalovali Connect pomocí expresního nastavení, musí být hello oprávnění již správné. 

Pokud jste použili vlastní instalaci, nastavte oprávnění hello ručně pomocí tohoto postupu hello následující:
    
1. toofind hello účet používaný službou hello konektor služby Active Directory, spuštění **Synchronization Service Manager**. 
 
2. Přejděte příliš**konektory**a poté vyhledejte místní doménové struktuře služby Active Directory hello řešení potíží. 
 
3. Vyberte konektor hello a pak klikněte na **vlastnosti**. 
 
4. Přejděte příliš**připojit doménové struktury Directory tooActive**.  
    
    ![Účet používaný službou konektoru služby Active Directory](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    Všimněte si hello uživatelské jméno a hello domény, kde je umístěn účet hello.
    
5. Spustit **Active Directory Users and Computers**a potom ověřte, zda má účet hello jste našli dříve hello postupujte podle oprávněními nastavenými v kořenovém adresáři hello všechny domény v doménové struktuře:
    * Replikovat změny adresáře
    * Replikace adresáře všechny změny

6. Jsou hello řadiče domény dostupný přes Azure AD Connect? Pokud hello připojit server se nemůže připojit tooall řadiče domény, nakonfigurujte **používat jenom upřednostňovaného řadiče domény**.  
    
    ![Řadič domény používané konektorem služby Active Directory](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. Přejděte zpět příliš**Synchronization Service Manager** a **konfigurace oddílu adresáře**. 
 
8. Vyberte doménu v **Vybrat oddíly adresářů**, vyberte hello **používat jenom řadiče domény upřednostňované** zaškrtněte políčko a potom klikněte na **konfigurace**. 

9. V seznamu hello zadejte hello řadiče domény, které by měly používat Connect pro synchronizaci hesel. Hello stejný seznam se používá pro import a export také. Proveďte tyto kroky pro všechny domény.

10. Pokud hello skript je ukázkou, že neexistuje žádný prezenční signál, spusťte skript hello [spustit úplnou synchronizaci hesel všech](#trigger-a-full-sync-of-all-passwords).

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a>Jeden objekt není synchronizace hesel: Ruční postup řešení potíží
Můžete snadno potíže heslo synchronizace kontrolou hello stavem objektu.

1. V **Active Directory Users and Computers**, vyhledejte hello uživatele a potom ověřte, že hello **musí uživatel změnit heslo při příštím přihlášení** zaškrtnutí políčka.  

    ![Produktivní hesla služby Active Directory](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    Pokud je zaškrtnuto políčko hello, požádejte uživatele toosign hello v a změnit heslo hello. Dočasná hesla nejsou synchronizovány s Azure AD.

2. Pokud správná hello hesla ve službě Active Directory, postupujte podle hello uživatele v synchronizační modul hello. Podle následující hello uživatele z místní služby Active Directory tooAzure AD se zobrazí, zda je u objektu hello popisný chyba.

    a. Spustit hello [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).

    b. Klikněte na tlačítko **konektory**.

    c. Vyberte hello **konektor služby Active Directory** kde se nachází hello uživatele.

    d. Vyberte **hledání prostoru konektoru**.

    e. V hello **oboru** vyberte **rozlišující název nebo ukotvení**a pak zadejte úplný název DN hello hello uživatele řešení potíží.

    ![Vyhledejte uživatele v prostoru konektoru s rozlišujícím názvem](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    f. Vyhledejte uživatele hello hledáte a potom klikněte na **vlastnosti** toosee všechny hello atributy. Pokud není uživatel hello ve výsledku hledání hello, ověřte vaší [pravidla filtrování](active-directory-aadconnectsync-configure-filtering.md) a ujistěte se, že spustíte [použít a ověřte změny](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) pro uživatele tooappear hello v připojení.

    g. Klikněte na tlačítko toosee hello heslo synchronizace Podrobnosti objektu hello pro hello minulého týdne **protokolu**.  

    ![Podrobnosti protokolu objektu](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    Pokud objekt protokolu hello je prázdná, Azure AD Connect byla hodnoty hash hesla hello nelze tooread ze služby Active Directory. Pokračovat v odstraňování problémů s [k chybám připojení](#connectivity-errors). Pokud se zobrazí jakoukoli jinou hodnotu než **úspěch**, najdete v tabulce toohello v [protokolu synchronizace hesla](#password-sync-log).

    h. Vyberte hello **rodokmenu** kartě a ujistěte se, že minimálně jeden synchronizační pravidlo v hello **PasswordSync** sloupec je **True**. Ve výchozí konfiguraci hello hello název pravidla synchronizace hello je **v ze služby Active Directory - uživatele AccountEnabled**.  

    ![Rodokmenu informací o uživateli](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    i. Klikněte na tlačítko **vlastnosti objektu úložiště Metaverse** toodisplay seznam atributů uživatele.  

    ![Informace o úložiště Metaverse](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    Ověřte, zda je žádné **cloudFiltered** atribut přítomen. Ujistěte se, že hello domény atributy (domainFQDN a domainNetBios) obsahují hello očekávaných hodnot.

    j. Klikněte na tlačítko hello **konektory** kartě. Ujistěte se, že vidíte konektory tooboth místní služby Active Directory a Azure AD.

    ![Informace o úložiště Metaverse](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    kB. Vyberte hello řádek, který představuje Azure AD, klikněte na tlačítko **vlastnosti**a potom klikněte na hello **rodokmenu** objektu prostoru konektoru hello kartu. měl by tam mít odchozí pravidlo hello **PasswordSync** sloupec nastavit příliš**True**. Ve výchozí konfiguraci hello hello název pravidla synchronizace hello je **Out tooAAD - připojení uživatele k**.  

    ![Dialogové okno Vlastnosti objektu prostoru konektoru](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a>Protokol synchronizace hesla
ve sloupci Stav Hello může mít hello následující hodnoty:

| Status | Popis |
| --- | --- |
| Úspěch |Heslo se úspěšně synchronizoval. |
| FilteredByTarget |Heslo se nastavuje příliš**musí uživatel změnit heslo při příštím přihlášení**. Heslo nebyl synchronizován. |
| NoTargetConnection |Žádný objekt v úložišti metaverse hello nebo v prostoru konektoru hello Azure AD. |
| SourceConnectorNotPresent |Žádný objekt v prostoru konektoru služby Active Directory v místě hello nalezen. |
| TargetNotExportedToDirectory |nebyl ještě exportoval Hello objekt v hello prostoru konektoru služby Azure AD. |
| MigratedCheckDetailsForMoreInfo |Položka protokolu byl vytvořen ještě před sestavení 1.0.9125.0 a je zobrazen v stav starší verze. |
| Chyba |Služba vrátila neznámou chybu. |
| Neznámý |Došlo k chybě při pokusu o tooprocess dávky hodnot hash hesel.  |
| MissingAttribute |Konkrétní atributy (například hodnota hash protokolu Kerberos) vyžaduje Azure AD Domain Services nejsou k dispozici. |
| RetryRequestedByTarget |Konkrétní atributy (například hodnota hash protokolu Kerberos) vyžaduje Azure AD Domain Services nebyly k dispozici dříve. Hodnota hash hesla pokusu o tooresynchronize hello uživatele Přišla žádost. |

## <a name="scripts-toohelp-troubleshooting"></a>Řešení potíží s toohelp skriptů

### <a name="get-hello-status-of-password-sync-settings"></a>Získat stav hello nastavení synchronizace hesla
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update hello script toouse hello appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Spustit úplnou synchronizaci všech hesel
> [!NOTE]
> Tento skript spusťte jenom jednou. Pokud potřebujete toorun je více než jednou, jiný problém hello je. tootroubleshoot hello problému, kontaktujte podporu společnosti Microsoft.

Úplnou synchronizaci všech hesel můžete aktivovat pomocí hello následující skript:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Další kroky
* [Implementace synchronizace hesel s synchronizace Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md)
* [Azure AD Connect Sync: Přizpůsobení možnosti synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
