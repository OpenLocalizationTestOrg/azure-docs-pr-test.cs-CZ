---
title: "Synchronizace Azure AD Connect: provozní úlohy a důležité informace | Microsoft Docs"
description: "Toto téma popisuje provozní úlohy pro synchronizaci Azure AD Connect a jak tooprepare pro provoz této součásti."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e6b21262e0924785808888d66f08a04a56581edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Synchronizace Azure AD Connect: provozní úlohy a zvážení
Hello cílem tohoto tématu je toodescribe provozní úlohy pro synchronizaci Azure AD Connect.

## <a name="staging-mode"></a>Pracovní režim
Pracovní režim lze použít pro několik scénářů, včetně:

* Vysoká dostupnost.
* Testování a nasazení nové změny konfigurace.
* Zavedení nového serveru a vyřadit z provozu původní hello.

Se serverem v pracovním režimu můžete provádět změny toohello konfigurace a preview hello změny před prováděním active hello server. Můžete taky toorun úplný import a úplnou synchronizaci tooverify všechny změny se očekává, než provedete tyto změny do vašeho provozního prostředí.

Během instalace, můžete vybrat toobe server hello v **pracovním režimu**. Tato akce aktivuje hello serveru pro import a synchronizaci, ale nespustí žádné export. Server v pracovní režimu není spuštěna synchronizace hesel nebo zpětný zápis hesla, i v případě, že jste vybrali tyto funkce během instalace. Pokud zakážete pracovní režim, hello server spustí export, umožňuje synchronizaci hesel a umožňuje zpětný zápis hesla.

Můžete vynutit exportu pomocí Správce služby synchronizace hello.

Server v pracovní režimu pokračuje tooreceive změny ze služby Active Directory a Azure AD. Má vždy kopii hello poslední změny a může trvat velmi rychlé přes hello odpovědnosti jiného serveru. Pokud provedete konfiguraci změny tooyour primární server, je vaše odpovědnosti toomake hello stejný server toohello změny v pracovním režimu.

Pro těch, které můžete s znalostmi starší technologie synchronizace se liší hello pracovním režimu, protože hello server má svou vlastní databázi SQL. Tato architektura umožňuje hello pracovní režim serveru toobe umístěný v jiném datovém centru.

### <a name="verify-hello-configuration-of-a-server"></a>Ověření konfigurace hello serveru
tooapply tuto metodu, postupujte takto:

1. [Příprava](#prepare)
2. [Konfigurace](#configuration)
3. [Import a synchronizaci](#import-and-synchronize)
4. [Ověření](#verify)
5. [Přepínač aktivní server](#switch-active-server)

#### <a name="prepare"></a>Příprava
1. Nainstalujte Azure AD Connect, vyberte **pracovním režimu**a zrušte výběr **spustit synchronizaci** na poslední stránce Průvodce instalací hello hello. Tento režim vám umožní toorun hello synchronizační modul ručně.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Vypnout nebo přihlášení a ze hello spusťte nabídky vyberte možnost přihlášení **synchronizační služba**.

#### <a name="configuration"></a>Konfigurace
Pokud primární server toohello vlastní změny a chcete toocompare hello konfigurace provedli s hello pracovní server, použijte [nástroje Azure AD Connect konfigurace dokumentace](https://github.com/Microsoft/AADConnectConfigDocumenter).

#### <a name="import-and-synchronize"></a>Import a synchronizaci
1. Vyberte **konektory**, a vyberte hello první konektor s typem hello **Active Directory Domain Services**. Klikněte na tlačítko **spustit**, vyberte **úplný import**, a **OK**. Proveďte tyto kroky pro všechny konektory tohoto typu.
2. Vyberte hello konektor s typem **Azure Active Directory (Microsoft)**. Klikněte na tlačítko **spustit**, vyberte **úplný import**, a **OK**.
3. Ujistěte se, že je stále vybraná karta hello konektory. Pro každý konektor s typem **Active Directory Domain Services**, klikněte na tlačítko **spustit**, vyberte **rozdílová synchronizace**, a **OK**.
4. Vyberte hello konektor s typem **Azure Active Directory (Microsoft)**. Klikněte na tlačítko **spustit**, vyberte **rozdílová synchronizace**, a **OK**.

Máte nyní dvoufázové instalace export změní tooAzure AD a místní AD (Pokud používáte hybridní nasazení systému Exchange). Další kroky Hello povolit tooinspect novinky o toochange před zahájením ve skutečnosti hello export toohello adresáře.

#### <a name="verify"></a>Ověřit
1. Spusťte příkazový řádek a přejděte příliš`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Spustit: `csexport "Name of Connector" %temp%\export.xml /f:x` hello název hello konektoru najdete v synchronizační služba. Obsahuje název podobné too"contoso.com – AAD" pro Azure AD.
3. Zkopírujte skript prostředí PowerShell hello z oddílu hello [CSAnalyzer](#appendix-csanalyzer) tooa soubor s názvem `csanalyzer.ps1`.
4. Otevřete okno prostředí PowerShell a procházet toohello složku, kde můžete vytvořit skript prostředí PowerShell hello.
5. Spustit: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.
6. Nyní máte soubor s názvem **processedusers1.csv** , může být prověřen v aplikaci Microsoft Excel. Všechny změny dvoufázové instalace toobe exportovat tooAzure AD se nacházejí v tomto souboru.
7. Proveďte potřebné změny toohello data nebo konfigurace a spusťte tyto kroky opakujte (Import a synchronizaci a ověřte, zda) až hello změny, které jsou o toobe exportovat se očekává.

#### <a name="switch-active-server"></a>Přepínač aktivní server
1. Na aktuálně aktivní server hello vypnout server hello (DirSync nebo FIM nebo Azure AD Sync), takže ji není export tooAzure AD nebo ho nastavit v pracovním režimu (Azure AD Connect).
2. Spustit Průvodce instalací hello na serveru hello v **pracovním režimu** a zakázat **pracovním režimu**.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Zotavení po havárii
V případě, že dojde k havárii, kde ztratíte hello synchronizační server, je součástí návrhu implementace hello tooplan pro jaké toodo. Existují různé modely toouse a které jeden toouse závisí na několika faktorech včetně:

* Co je tolerance, nebude možné zkontrolujte změny tooobjects ve službě Azure AD během hello výpadku?
* Pokud používáte synchronizace hesel, hello uživatelé přijmout, ke kterým mají toouse hello staré heslo ve službě Azure AD v případě, že se změní ho místně?
* Máte závislost na v reálném čase operace, jako je zpětný zápis hesla?

V závislosti na hello odpovědi toothese otázky a zásad vaší organizace může být implementováno jednu z následujících strategií hello:

* Znovu sestavte v případě potřeby.
* K výměně za chodu pohotovostní server, označuje jako **pracovním režimu**.
* Virtuální počítače použijte.

Pokud nepoužijete hello integrovanou databází SQL Express, tak také byste měli revidovat hello [vysoké dostupnosti SQL](#sql-high-availability) části.

### <a name="rebuild-when-needed"></a>Opětovné sestavení v případě potřeby
Vhodným strategie je tooplan pro opětovném sestavení serveru v případě potřeby. Obvykle instalaci hello synchronizační modul a hello počáteční import a synchronizace může být dokončeny v rámci několik hodin. Pokud není k dispozici náhradní server, je možné tootemporarily použití synchronizační modul domény Řadič toohost hello.

Hello synchronizační modul server neukládá jakýkoli stav o objektech hello, takže hello databáze může být znovu sestavit z hello dat ve službě Active Directory a Azure AD. Hello **sourceAnchor** atribut je použité toojoin hello objekty z místního a hello cloudu. Pokud znovu vytvoříte místní server hello s existující objekty a hello cloudu a potom hello synchronizaci modul odpovídá tyto objekty společně na přeinstalování znova. Hello potřebujete toodocument a uložte si hello změny konfigurace provedené toohello serveru, například pravidla filtrování a synchronizace. Před zahájením synchronizace ho znovu použít tyto vlastní konfigurace.

### <a name="have-a-spare-standby-server---staging-mode"></a>K výměně za chodu pohotovostní server - pracovní režim
Pokud máte složitější prostředí, pak má jeden nebo více pohotovostní serverů doporučujeme. Během instalace, můžete povolit serveru toobe v **pracovním režimu**.

Další informace najdete v tématu [pracovním režimu](#staging-mode).

### <a name="use-virtual-machines"></a>Použijte virtuální počítače
Je metoda běžných a podporovanou toorun hello synchronizační modul ve virtuálním počítači. V případě, že má hostitel hello problém, může být hello bitové kopie s hello synchronizační modul server migrované tooanother server.

### <a name="sql-high-availability"></a>Vysoká dostupnost SQL
Pokud nepoužíváte hello SQL Server Express, která se dodává s Azure AD Connect, pak vysoká dostupnost pro SQL Server měli byste také zvážit. řešení s vysokou dostupností Hello podporované zahrnují clusteringu SQL a AOA (skupiny dostupnosti Always On). Nepodporované řešení zahrnují zrcadlení.

Přidala se podpora pro SQL AOA tooAzure AD Connect ve verzi 1.1.524.0. Před instalací Azure AD Connect je nutné povolit SQL AOA. Při instalaci Azure AD Connect zjistí, zda zadaná instance SQL hello je povoleno pro SQL AOA, nebo ne. Pokud je povoleno SQL AOA, Azure AD Connect další hodnoty, pokud SQL AOA je nakonfigurované toouse replikace synchronní nebo asynchronní replikaci. Při nastavování hello naslouchací proces skupiny dostupnosti, se doporučuje, abyste nastavili too0 vlastnost RegisterAllProvidersIP hello. Je to proto, že Azure AD Connect v současné době používá tooSQL tooconnect Nativní klient systému SQL a SQL Native Client hello použití MultiSubNetFailover vlastnosti nepodporuje.

## <a name="appendix-csanalyzer"></a>Příloha CSAnalyzer
Části hello [ověřte](#verify) o toouse tento skript.

```
Param(
    [Parameter(Mandatory=$true, HelpMessage="Must be a file generated using csexport 'Name of Connector' export.xml /f:x)")]
    [string]$xmltoimport="%temp%\exportedStage1a.xml",
    [Parameter(Mandatory=$false, HelpMessage="Maximum number of users per output file")][int]$batchsize=1000,
    [Parameter(Mandatory=$false, HelpMessage="Show console output")][bool]$showOutput=$false
)

#LINQ isn't loaded automatically, so force it
[Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089") | Out-Null

[int]$count=1
[int]$outputfilecount=1
[array]$objOutputUsers=@()

#XML must be generated using "csexport "Name of Connector" export.xml /f:x"
write-host "Importing XML" -ForegroundColor Yellow

#XmlReader.Create won't properly resolve hello file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader toodeal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create hello object placeholder
    #adding them up here means we can enforce consistency
    $objOutputUser=New-Object psobject
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name ID -Value ""
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name Type -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name DN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name operation -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name UPN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name displayName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name sourceAnchor -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name alias -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name primarySMTP -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name onPremisesSamAccountName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name mail -Value ""

    $user = [System.Xml.Linq.XElement]::ReadFrom($reader)
    if ($showOutput) {Write-Host Found an exported object... -ForegroundColor Green}

    #object id
    $outID=$user.Attribute('id').Value
    if ($showOutput) {Write-Host ID: $outID}
    $objOutputUser.ID=$outID

    #object type
    $outType=$user.Attribute('object-type').Value
    if ($showOutput) {Write-Host Type: $outType}
    $objOutputUser.Type=$outType

    #dn
    $outDN= $user.Element('unapplied-export').Element('delta').Attribute('dn').Value
    if ($showOutput) {Write-Host DN: $outDN}
    $objOutputUser.DN=$outDN

    #operation
    $outOperation= $user.Element('unapplied-export').Element('delta').Attribute('operation').Value
    if ($showOutput) {Write-Host Operation: $outOperation}
    $objOutputUser.operation=$outOperation

    #now that we have hello basics, go get hello details

    foreach ($attr in $user.Element('unapplied-export-hologram').Element('entry').Elements("attr"))
    {
        $attrvalue=$attr.Attribute('name').Value
        $internalvalue= $attr.Element('value').Value

        switch ($attrvalue)
        {
            "userPrincipalName"
            {
                if ($showOutput) {Write-Host UPN: $internalvalue}
                $objOutputUser.UPN=$internalvalue
            }
            "displayName"
            {
                if ($showOutput) {Write-Host displayName: $internalvalue}
                $objOutputUser.displayName=$internalvalue
            }
            "sourceAnchor"
            {
                if ($showOutput) {Write-Host sourceAnchor: $internalvalue}
                $objOutputUser.sourceAnchor=$internalvalue
            }
            "alias"
            {
                if ($showOutput) {Write-Host alias: $internalvalue}
                $objOutputUser.alias=$internalvalue
            }
            "proxyAddresses"
            {
                if ($showOutput) {Write-Host primarySMTP: ($internalvalue -replace "SMTP:","")}
                $objOutputUser.primarySMTP=$internalvalue -replace "SMTP:",""
            }
        }
    }

    $objOutputUsers += $objOutputUser

    Write-Progress -activity "Processing ${xmltoimport} in batches of ${batchsize}" -status "Batch ${outputfilecount}: " -percentComplete (($objOutputUsers.Count / $batchsize) * 100)

    #every so often, dump hello processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit hello maximum users processed without completion... -ForegroundColor Yellow

        #export hello collection of users as as CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment hello output file counter
        $outputfilecount+=1

        #reset hello collection and hello user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need toobail out of hello loop if no more users tooprocess
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need toowrite out any users that didn't get picked up in a batch of 1000
#export hello collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a>Další kroky
**Témata s přehledem**  

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)  
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)  
