---
title: "aaaConfigure spustit jako účet Azure | Microsoft Docs"
description: "Tento kurz vás provede procesem vytváření, testování a ukázkovým použitím hello objekt zabezpečení ověřování ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
keywords: "název objektu služby, název instančního objektu, setspn, ověřování azure"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: magoedte
ROBOTS: NOINDEX
redirect_url: /azure/automation/
redirect_document_id: True
ms.openlocfilehash: 06675d2f6b9d8e7260ffaead4f2b2f61c2b98d13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a>Ověření runbooků pomocí účtu Spustit jako pro Azure
Tento článek ukazuje, jak tooconfigure Azure Automation účet v hello portálu Azure. toodo tedy můžete používat hello spustit jako účet funkce tooauthenticate runbooky správu prostředků v Azure Resource Manageru nebo Azure Service Management.

Když vytvoříte účet služby Automation v hello portálu Azure, můžete automaticky vytvořit dva účty:

* Účet Spustit jako. Tento účet vytvoří instanční objekt ve službě Azure Active Directory (Azure AD) a certifikát. Také přiřadí hello Přispěvatel přístupu na základě role řízení (RBAC), která spravuje prostředky Resource Manageru pomocí sad runbook.
* Účet Spustit jako pro Azure Classic. Tento účet odešle certifikát správy, který je použit toomanage Service Management nebo klasické prostředky pomocí sad runbook.

Vytváření Automation účet zjednodušuje proces hello a pomáhá rychle začít vytvářet a nasazovat runbooky toosupport vaše automation potřebuje.

Pomocí účtu Spustit jako a účtu Spustit jako pro Azure Classic můžete:

* Poskytovat standardizovaného způsobu tooauthenticate s Azure při správě správce prostředků nebo služby správy prostředků ze sady runbook v hello portálu Azure.
* Automatizovat hello používání globálních runbooků, které můžete nakonfigurovat ve výstrahách Azure.

> [!NOTE]
> Hello [Azure výstrah funkce integrace](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) s automatizace globálních runbooků vyžaduje účet Automation, který je nakonfigurovaný s účet Spustit jako a účet Classic spustit jako. Můžete vybrat účet Automation, který již má definovaný účty spustit jako a Classic spustit jako, nebo můžete toocreate nového účtu Automation.
>  

Tento článek ukazuje, jak toocreate účtu Automation na portálu Azure, hello aktualizace účtu Automation pomocí prostředí Azure PowerShell, spravovat konfiguraci účtu hello a ověřit svoje runbooky.

Než začnete, vytvoření účtu Automation, je vhodné toounderstand a zvažte následující hello:

* Vytvoření účtu Automation nemá vliv na účty služby Automation, které mohou již jste vytvořili v hello classic nebo modelu nasazení Resource Manager.
* proces Hello funguje pouze pro účty Automation, které vytvoříte v hello portálu Azure. Probíhá pokus toocreate účtu hello portál Azure classic se nereplikuje konfigurace účtu spustit jako hello.
* Pokud už máte runbooky a prostředky (například plány nebo proměnné) v místní toomanage klasické prostředky, a chcete tooauthenticate sady runbook s hello nový Classic účet Spustit jako, proveďte jednu z následujících hello:

  * toocreate účet Classic spustit jako, postupujte podle pokynů hello v části "Správa vašeho účet Spustit jako" hello. 
  * tooupdate existující účet, použijte hello prostředí PowerShell skript v části "Aktualizace účtu Automation pomocí prostředí PowerShell" hello.
* tooauthenticate pomocí hello nový účet Spustit jako a Classic spustit jako automatizace účtu, je nutné toomodify vaší existující sady runbook s hello ukázkový kód, které jsou uvedeny v části hello [příklady ověřovacího kódu](#authentication-code-examples).

    >[!NOTE]
    >Hello účet Spustit jako je pro ověřování s prostředky Resource Manager pomocí hello založené na certifikátech instanční objekt. Hello Classic spustit jako účtu je pro ověřování proti zdroje informací pro správu služby pomocí certifikátu správy.

## <a name="create-an-automation-account-from-hello-azure-portal"></a>Vytvoření účtu Automation z hello portálu Azure
V této části vytvoříte účet služby Azure Automation z hello portál Azure, které pak vytvoří účet Spustit jako a účet Classic spustit jako.

>[!NOTE]
>toocreate účet Automation, musíte být členem role Správci služby hello správce nebo spolusprávce předplatného hello je udělení přístupu toohello předplatné. Je třeba přidat také jako instanci Active Directory výchozí předplatné toothat uživatele. účet Hello nemusí toobe přiřadit privilegované role.
>
>Pokud si nejste členem hello předplatné instanci Active Directory, než se přidají toohello role spolusprávcem předplatného hello, bude možné přidat tooActive Directory jako Host. V této instanci zobrazí se "Nemáte oprávnění toocreate..." upozornění na hello **přidat účet Automation** okno.
>
>Uživatelé, kteří byly přidány toohello spolusprávcem role nejprve lze odebrat z instance služby Active Directory hello předplatného a znova se přidal toomake je úplné uživatelské ve službě Active Directory. tooverify této situaci z hello **Azure Active Directory** poddokně na portálu Azure tak, že vyberete hello **uživatelů a skupin**, vyberete **všichni uživatelé** a po výběru Hello konkrétního uživatele, vyberete **profil**. Hello hodnotu hello **typ uživatele** atribut v profilu uživatele hello nesmí rovnat **hosta**.
>

1. Přihlaste se toohello portálu Azure pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.

2. Vyberte **Účty Automation**.

3. Na hello **účty Automation** okně klikněte na tlačítko **přidat**.
Hello **přidat účet Automation** otevře se okno.

 ![okno "Přidat účet Automation" Hello](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > Pokud váš účet není členem role Správci předplatného hello a spolusprávce předplatného hello, hello následující upozornění se zobrazí na hello **přidat účet Automation** okno:
   >
   >![Přidání upozornění k účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. Na hello **přidat účet Automation** okno, v hello **název** pole, zadejte název nového účtu Automation.

5. Pokud máte více než jedno předplatné, hello následující:

    a. V části **předplatné**, zadejte jednu pro nový účet hello.

    b. V části **Skupina prostředků** klikněte na **Vytvořit novou** nebo **Použít stávající**.

    c. V části **Umístění** zadejte datové centrum Azure.

6. V části **Vytvořit účet Spustit v Azure jako** vyberte **Ano** a potom klikněte na **Vytvořit**.

   > [!NOTE]
   > Pokud se rozhodnete není toocreate hello účet Spustit jako výběrem **ne**, se zobrazí zpráva s upozorněním hello **přidat účet Automation** okno. I když hello účet je vytvořen v hello portálu Azure, nemá odpovídající identitu ověřování v rámci vaší classic nebo Resource Manager předplatné adresářové služby. V důsledku toho hello účet nemá žádná tooresources přístup v rámci vašeho předplatného. Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky v těchto modelech nasazení.
   >
   > ![Upozornění v okně "Přidat účet Automation" hello](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > Kromě toho protože instanční objekt hello není vytvořen, není přiřazena role Přispěvatel hello.
   >

7. Zatímco Azure vytváří účet Automation hello, můžete sledovat průběh hello pod **oznámení** nabídce hello.

### <a name="resources"></a>Zdroje
Když hello účet Automation je úspěšně vytvořen, jsou automaticky vytvoří několik prostředků. Hello prostředky jsou shrnuty v následující dvě tabulky hello:

#### <a name="run-as-account-resources"></a>Prostředky účtu Spustit jako

| Prostředek | Popis |
| --- | --- |
| Runbook AzureAutomationTutorial | Ukázkový grafický runbook, který ukazuje, jak tooauthenticate pomocí hello účet Spustit jako a získá všechny prostředky Resource Manager hello. |
| Runbook AzureAutomationTutorialScript | Ukázkový runbook prostředí PowerShell, který ukazuje, jak tooauthenticate pomocí hello účet Spustit jako a získá všechny prostředky Resource Manager hello. |
| AzureRunAsCertificate | Hello asset certifikátu, který se automaticky vytvoří při vytvoření účtu Automation nebo použijte hello následující skript prostředí PowerShell pro existující účet. certifikát Hello umožňuje tooauthenticate s Azure, aby mohl spravovat prostředky Azure Resource Manager ze sady runbook. Hello certifikát má životnost jeden rok. |
| AzureRunAsConnection | Hello asset připojení, která se automaticky vytvoří při vytvoření účtu Automation, nebo pomocí skriptu prostředí PowerShell hello pro existující účet. |

#### <a name="classic-run-as-account-resources"></a>Prostředky účtu Spustit jako pro Classic

| Prostředek | Popis |
| --- | --- |
| Runbook AzureClassicAutomationTutorial | Ukázkový grafický runbook, který získá všechny hello virtuálních počítačů, které jsou vytvořené pomocí modelu nasazení classic hello v předplatném pomocí hello Classic účet Spustit jako (certifikátu) a pak zapíše hello název virtuálního počítače a stav. |
| Runbook se skriptem AzureClassicAutomationTutorial | Ukázkový runbook prostředí PowerShell, který získá všechny hello klasické virtuální počítače v předplatném pomocí hello Classic účet Spustit jako (certifikátu), a pak zápisy hello název virtuálního počítače a stav. |
| AzureClassicRunAsCertificate | asset certifikátu Hello automaticky vytvoří pomocí tooauthenticate s Azure, abyste mohli spravovat prostředky Azure classic ze sady runbook. Hello certifikát má životnost jeden rok. |
| AzureClassicRunAsConnection | prostředek Hello automaticky vytvořené připojení používat tooauthenticate službou Azure, abyste mohli spravovat prostředky Azure classic ze sady runbook. |

## <a name="verify-run-as-authentication"></a>Kontrola ověřování pomocí účtu Spustit jako
Provedení testu malých tooconfirm, který úspěšně, můžete ověřovat pomocí hello nový účet Spustit jako.

1. V hello portálu Azure otevřete účet Automation hello, kterou jste vytvořili dříve.

2. Klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.

3. Vyberte hello **AzureAutomationTutorialScript** runbook a potom klikněte na **spustit** toostart hello runbook. dojde k Hello následující události:
 * A [úlohy runbooku](automation-runbook-execution.md) vytvoření hello **úlohy** zobrazí se okno a v hello se zobrazí stav úlohy hello **Souhrn úlohy** dlaždici.
 * Stav úlohy Hello začne jako **zařazeno ve frontě**, což indikuje, že se čeká na pracovního procesu runbooku v toobecome cloudu hello k dispozici.
 * Stav Hello stane **počáteční** když pracovní proces úlohu hello.
 * Stav Hello stane **systémem** při spuštění sady runbook hello.
 * Po dokončení spuštění úlohy runbooku hello měli vidět stav **dokončeno**.

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. toosee hello podrobné výsledky hello sady runbook, klikněte na tlačítko hello **výstup** dlaždici.  
Hello **výstup** okno se zobrazí, zobrazující dané sady runbook hello úspěšně ověřen a vrátí seznam všech prostředků, které jsou k dispozici ve skupině prostředků hello.

5. Zavřít hello **výstup** okno tooreturn toohello **Souhrn úlohy** okno.

6. Zavřít hello **Souhrn úlohy** okno a odpovídající hello **AzureAutomationTutorialScript** okno sady runbook.

## <a name="verify-classic-run-as-authentication"></a>Kontrola ověřování pomocí účtu Spustit jako pro Azure Classic
Provedení podobné malá testování tooconfirm, který úspěšně, můžete ověřovat pomocí hello nový Classic účet Spustit jako.

1. V hello portálu Azure otevřete účet Automation hello, kterou jste vytvořili dříve.

2. Klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.

3. Vyberte hello **AzureClassicAutomationTutorialScript** runbook a potom klikněte na **spustit** příliš spuštění sady runbook hello. dojde k Hello následující události:

 * A [úlohy runbooku](automation-runbook-execution.md) vytvoření hello **úlohy** zobrazí se okno a v hello se zobrazí stav úlohy hello **Souhrn úlohy** dlaždici.
 * Stav úlohy Hello začne jako **zařazeno ve frontě**, což indikuje, že se čeká na pracovního procesu runbooku v toobecome cloudu hello k dispozici.
 * Stav Hello stane **počáteční** když pracovní proces úlohu hello.
 * Stav Hello stane **systémem** při spuštění sady runbook hello.
 * Po dokončení spuštění úlohy runbooku hello měli vidět stav **dokončeno**.

    ![Test runbooku objektu zabezpečení](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. toosee hello podrobné výsledky hello sady runbook, klikněte na tlačítko hello **výstup** dlaždici.  
Hello **výstup** okno se zobrazí, zobrazující dané sady runbook hello úspěšně ověřen a vrátí seznam všech klasické virtuální počítače v rámci předplatného hello.

5. Zavřít hello **výstup** okno tooreturn toohello **Souhrn úlohy** okno.

6. Zavřít hello **Souhrn úlohy** okno a odpovídající hello **AzureAutomationTutorialScript** okno sady runbook.

## <a name="managing-your-run-as-account"></a>Správa účtu Spustit jako
V určitém okamžiku před vypršením platnosti účtu Automation, musíte certifikát toorenew hello. Pokud se domníváte, že došlo k ohrožení zabezpečení tohoto hello účet Spustit jako, můžete odstranit a znovu vytvořit. Tato část pojednává o tom, jak tooperform těchto operací.

### <a name="self-signed-certificate-renewal"></a>Obnovení certifikátu podepsaného svým držitelem
certifikát podepsaný svým držitelem Hello, kterou jste vytvořili pro hello účet Spustit jako vyprší platnost jeden rok z hello data vytvoření. Před vypršením platnosti ho můžete kdykoli obnovit. Při obnovování, je aktuální platný certifikát hello udržených tooensure, že nejsou žádné sady runbook, které jsou zařazeny do fronty až nebo aktivně spuštěná, a který ověření pomocí hello účet Spustit jako, negativní vliv. certifikát Hello zůstává platná do data vypršení platnosti.

> [!NOTE]
> Pokud jste nakonfigurovali vaší spustit v Automation jako účet toouse certifikát vydaný certifikační autoritou rozlehlé sítě a chcete použít tuto možnost, hello podnikový certifikát budou nahrazeny certifikát podepsaný svým držitelem.

toorenew hello certifikátů, hello následující:

1. V hello portálu Azure otevřete účet Automation hello.

2. Na hello **účet Automation** okno, v hello **účet vlastnosti** podokně v části **nastavení účtu**, vyberte **účty spustit jako**.

    ![Podokno vlastností účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. Na hello **účty spustit jako** vlastnosti okně vyberte buď hello účet Spustit jako nebo hello Classic účet Spustit jako, které chcete toorenew hello certifikát pro.

4. Na hello **vlastnosti** okně hello vybraný účet, klikněte na tlačítko **obnovit certifikát**.

    ![Obnovení certifikátu pro účet Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. Při obnovení certifikátu hello se můžete sledovat průběh hello pod **oznámení** nabídce hello.

### <a name="delete-a-run-as-or-classic-run-as-account"></a>Odstranění účet Spustit jako nebo Spustit jako pro Classic
Tato část popisuje, jak toodelete a znovu vytvořit účet Spustit jako nebo Classic spustit jako. Když provedete tuto akci, se uchovávají hello účet Automation. Po odstranění účtu spustit jako nebo Classic spustit jako, znovu ji vytvořte hello portálu Azure.

1. V hello portálu Azure otevřete účet Automation hello.

2. Na hello **účet Automation** okno, v hello účet vlastnosti podokně, vyberte **účty spustit jako**.

3. Na hello **účty spustit jako** okno Vlastnosti, vyberte buď hello účet Spustit jako nebo Classic spustit jako účet, který má toodelete. Potom na hello **vlastnosti** okně hello vybraný účet, klikněte na tlačítko **odstranit**.

 ![Odstranění účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. Zatímco se odstraňuje hello účet, můžete sledovat průběh hello v části **oznámení** nabídce hello.

5. Po odstranění účtu hello můžete znovu vytvořit ji na hello **účty spustit jako** okno Vlastnosti výběrem hello vytvořit možnost **Azure účet Spustit jako**.

 ![Znovu vytvořit účet Automation spustit jako hello](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a>Chybná konfigurace
Některé položky konfigurace potřebné pro toofunction účet Spustit jako nebo Classic spustit jako hello správně může byla odstraněna nebo nesprávně vytvořený během počáteční instalace. položky Hello patří:

* Asset certifikátu
* Asset připojení
* Účet Spustit jako byl odebrán z role Přispěvatel hello
* Instanční objekt nebo aplikace v Azure AD

V hello předchozí a další instance chybné konfigurace, zjistí hello účet Automation hello změní a zobrazuje stav *nekompletní* na hello **účty spustit jako** okně Vlastnosti pro hello účet.

![Nedokončená konfigurace účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

Když vyberete účet Spustit jako hello, hello účet **vlastnosti** podokně zobrazí hello následující chybová zpráva:

![Zpráva upozornění o nedokončené konfiguraci účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).

Odstranit a znovu vytvořit účet hello můžete rychle vyřešit tyto problémy účet Spustit jako.

## <a name="update-your-automation-account-by-using-powershell"></a>Aktualizace účtu Automation pomocí PowerShellu
Tooupdate prostředí PowerShell můžete použít svůj existující účet Automation, pokud:

* Vytvoření účtu Automation, ale odmítnout toocreate hello účet Spustit jako.
* Už používáte prostředky Resource Manager toomanage účet Automation a chcete tooupdate hello tooinclude hello spustit jako účet pro ověřování sady runbook.
* Už používáte toomanage účet Automation pro klasické prostředky a chcete, aby tooupdate ho toouse hello Classic spustit jako účet, místo vytvoření nového účtu a migrace tooit vaše runbooky a prostředky.   
* Chcete toocreate spustit jako a Classic spustit jako účet pomocí certifikát vydaný certifikační autoritou (CA) rozlehlé sítě.

skript Hello má hello následující požadavky:

* skript Hello lze spustit pouze na systémy Windows 10 a Windows Server 2016 s moduly Azure Resource Manager 2.01 a novější. V předchozích verzích Windows není podporován.
* Azure PowerShell 1.0 nebo novější. Informace o verzi hello PowerShell 1.0 najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
* Účet služby Automation, který je odkazováno jako hodnota hello hello *-AutomationAccountName* a *- ApplicationDisplayName* parametry v hello následující skript prostředí PowerShell.

tooget hello hodnoty pro *SubscriptionID*, *ResourceGroup*, a *AutomationAccountName*, které jsou požadované parametry pro hello skripty, hello následující:
1. V hello portálu Azure, vyberte svůj účet Automation na hello **účet Automation** a pak vyberte **všechna nastavení**. 
2. Na hello **všechna nastavení** okno, v části **nastavení účtu**, vyberte **vlastnosti**. 
3. Všimněte si hodnot hello hello **vlastnosti** okno.

![okno "Vlastnosti" účet Automation Hello](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a>Vytvoření powershellového skriptu pro účet Spustit jako
Tento skript prostředí PowerShell zahrnuje podporu pro hello následující konfigurace:

* Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem
* Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem
* Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu
* Vytvořte účet Spustit jako a Classic spustit jako účet pomocí certifikát podepsaný svým držitelem v hello cloudu Azure Government.

V závislosti na hello možnost konfigurace, kterou vyberete vytvoří skript hello hello následující položky.

**Pro účty Spustit jako:**

* Vytvoří Azure AD application toobe exportovaný s buď hello podepsaného svým držitelem nebo enterprise veřejný klíč certifikátu, vytvoří hlavní účet služby pro aplikaci hello ve službě Azure AD a hello přiřadí role Přispěvatel pro účet hello ve vaší stávající předplatné. Můžete změnit tato nastavení tooOwner nebo jakákoli jiná role. Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).
* Vytvoří prostředek certifikátu automatizace s názvem *AzureRunAsCertificate* v hello určený účet Automation. asset certifikátu Hello obsahuje hello privátní klíč certifikátu používaného aplikací hello Azure AD.
* Vytvoří prostředek připojení automatizace s názvem *AzureRunAsConnection* v hello určený účet Automation. asset připojení Hello obsahuje hello applicationId, tenantId, subscriptionId a kryptografický otisk certifikátu.

**Pro účet Spustit jako pro Classic:**

* Vytvoří prostředek certifikátu automatizace s názvem *AzureClassicRunAsCertificate* v hello určený účet Automation. asset Hello certifikát obsahuje privátní klíč pro hello certifikátu používá certifikát pro správu hello.
* Vytvoří prostředek připojení automatizace s názvem *AzureClassicRunAsConnection* v hello určený účet Automation. asset připojení Hello obsahuje název odběru hello, subscriptionId a název certifikátu asset.

>[!NOTE]
> Pokud vyberete jednu z možností pro vytvoření účtu Classic spustit jako, po hello skript se spustí, nahrávání hello veřejné správy toohello (přípona názvu souboru .cer) úložiště certifikátů pro předplatné hello této hello účet Automation byla vytvořena v.
> 

tooexecute hello skriptu a nahrání certifikátu hello, hello následující:

1. Uložte následující skript v počítači hello. V tomto příkladu ho uložte pod názvem hello *New-RunAsAccount.ps1*.

        #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds tooallow hello service principal application toobecome active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate hello ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload hello .cer format of #CERT# toohello Management store by following hello steps below." + [Environment]::NewLine +
                    "Log in toohello Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload hello .cer format of #CERT#"

             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate hello ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Na svém počítači klikněte na **Start** a potom spusťte **Windows PowerShell** se zvýšenými uživatelskými oprávněními.

3. Z hello se zvýšenými oprávněními prostředí příkazového řádku prostředí PowerShell, přejděte toohello složku, která obsahuje hello skript, který jste vytvořili v kroku 1.

4. Spusťte skript hello pomocí hodnoty parametrů hello hello konfigurace, kterou požadujete.

    **Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Vytvořit účet Spustit jako a Classic spustit jako účet pomocí certifikát podepsaný svým držitelem v hello cloudu Azure Government.**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Po provedení hello skript bude výzvami tooauthenticate s Azure. Přihlaste se pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.
    >
    >

Po hello skript byl úspěšně proveden, vezměte na vědomí následující hello:
* Pokud jste vytvořili účet Classic spustit jako s certifikát podepsaný svým držitelem veřejné (soubor .cer), skript hello vytvoří a uloží jej toohello dočasné soubory složky v počítači v rámci profilu uživatele hello *%USERPROFILE%\AppData\Local\Temp*, který používá relaci prostředí PowerShell tooexecute hello.
* Pokud jste vytvořili účet Spustit jako pro Classic s využitím podnikového veřejného certifikátu (soubor .cer), použijte tento certifikát. Postupujte podle pokynů hello pro [odesílání toohello certifikátu rozhraní API pro správu portálu Azure classic](../azure-api-management-certs.md)a ověřit konfiguraci hello přihlašovacích údajů pomocí služby správy prostředků pomocí hello [ukázkový kód tooauthenticate s prostředky služby správy](#sample-code-to-authenticate-with-service-management-resources). 
* Pokud jste to udělali *není* vytvořit účet Classic spustit jako, ověření pomocí prostředků Resource Manageru a ověřit konfiguraci hello přihlašovacích údajů pomocí hello [ukázkový kód pro ověřování pomocí služby správy prostředky](#sample-code-to-authenticate-with-resource-manager-resources).

## <a name="sample-code-tooauthenticate-with-resource-manager-resources"></a>Ukázka kódu tooauthenticate pomocí prostředků Resource Manageru
Můžete použít následující hello aktualizovat ukázkový kód, provedených od hello *AzureAutomationTutorialScript* ukázkový runbook, tooauthenticate pomocí hello spustit jako účet toomanage prostředky Resource Manageru pomocí svých runbooků.

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get hello connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in tooAzure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context tooa specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }

toohelp tooeasily můžete pracovat s několika předplatnými, hello skript obsahuje dva další řádky kódu, které podporují odkazování na kontext předplatného. Proměnný asset s názvem *SubscriptionId* obsahuje ID hello hello předplatného. Po hello `Add-AzureRmAccount` příkazu rutiny hello [ `Set-AzureRmContext` ](/powershell/module/azurerm.profile/set-azurermcontext) rutiny je uvedená v sada parametrů hello *- SubscriptionId*. Pokud hello název proměnné příliš obecný, můžete zkontrolujte ji tooinclude předpony nebo pomocí jiné zásady vytváření názvů toomake je snazší tooidentify. Alternativně můžete použít sadu parametrů hello *- Název_předplatného* místo *- SubscriptionId* s odpovídajícím proměnným assetem.

Hello rutiny, který používáte pro ověřování v sadě runbook hello, `Add-AzureRmAccount`, používá hello *ServicePrincipalCertificate* sadu parametrů. Ověřuje pomocí hello certifikát hlavní služby, hello pověření uživatele.

## <a name="sample-code-tooauthenticate-with-service-management-resources"></a>Ukázka kódu tooauthenticate pomocí služby správy prostředků
Můžete použít následující aktualizovaný ukázkový kód, který je převzat ze hello hello *AzureClassicAutomationTutorialScript* ukázkový runbook, tooauthenticate pomocí hello Classic účet Spustit jako toomanage klasické prostředky s vaší sady runbook.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get hello connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate tooAzure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in hello Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting hello certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in hello Automation account."
    }

    Write-Verbose "Authenticating tooAzure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a>Další kroky
* [Aplikační a instanční objekty v Azure AD](../active-directory/active-directory-application-objects.md)
* [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md)
* [Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)
