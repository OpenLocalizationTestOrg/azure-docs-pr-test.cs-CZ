---
title: Instalace agenta Connect Health aaaAzure AD | Microsoft Docs
description: "Toto je stránka Azure AD Connect Health hello, která popisuje hello instalace agenta pro služby AD FS a synchronizaci."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 1cc8ae90-607d-4925-9c30-6770a4bd1b4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 9019628ec1d4c477496e08910cfd668ed1933a62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-health-agent-installation"></a>Instalace agenta služby Azure AD Connect Health
Tento dokument vás provede instalací a konfigurací agentů hello Azure AD Connect Health provede. Můžete si stáhnout hello agenty z [zde](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

## <a name="requirements"></a>Požadavky
Následující tabulka Hello je seznam požadavků pro používání Azure AD Connect Health.

| Požadavek | Popis |
| --- | --- |
| Azure AD Premium |Azure AD Connect Health je funkcí služby Azure AD Premium a vyžaduje Azure AD Premium. </br></br>Další informace najdete v článku [Začínáme s Azure AD Premium](../active-directory-get-started-premium.md). </br>toostart bezplatné 30denní zkušební verze, najdete v části [spuštění zkušební verze.](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Musíte být globální správce vaší služby Azure AD tooget začít s Azure AD Connect Health |Ve výchozím nastavení jenom globální správci hello můžete nainstalovat a nakonfigurovat hello stavu agentů tooget spustili, portál hello přístup a provádět žádné operace v rámci Azure AD Connect Health. Další informace najdete v článku o [správě adresáře Azure AD](../active-directory-administer.md). <br><br> Pomocí řízení přístupu na základě Role můžete povolit přístup tooAzure AD Connect Health tooother uživatelé ve vaší organizaci. Další informace najdete v článku o [řízení přístupu k Azure AD Connect Health na základě rolí](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control). </br></br>**Důležité:** hello účet použitý při instalaci agentů hello musí být pracovní nebo školní účet. Nemůže to být účet Microsoft. Další informace najdete v článku o [registraci do Azure jako organizace](../sign-up-organization.md). |
| Agent Azure AD Connect Health je nainstalovaný na každém cílovém serveru | Azure AD Connect Health vyžaduje hello stavu agentů toobe nainstalované a nakonfigurované na cílových serverech tooreceive hello dat a nabízí možnosti sledování a analýza hello </br></br>Například tooget data z infrastruktury služby AD FS, hello agenta musí být nainstalován na hello služby AD FS a Proxy serverech webových aplikací. Podobně tooget data na místní infrastrukturu služby AD DS, musí být nainstalovaný hello agent na řadičích domény hello. </br></br> |
| Koncové body služby Azure toohello odchozí připojení | Během instalace a za běhu hello agent vyžaduje koncové body služby připojení tooAzure AD Connect Health. Pokud odchozí připojení je blokovaný, pomocí brány firewall, ujistěte se, že hello následující koncové body se přidají toohello seznamu povolených aplikací: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net – Port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Odchozí připojení na základě IP adres | IP adresy na základě filtrování na brány firewall, najdete v části toohello [rozsahy IP adres Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653).|
| Kontrola protokolu SSL je pro odchozí připojení filtrovaná nebo zakázaná | Hello agenta registrace krok nebo data nahrávání operace mohou selhat, pokud je kontrolu SSL nebo ukončení pro odchozí pakety hello síťové vrstvy. |
| Porty brány firewall, na kterém je spuštěn hello agent server hello. |Hello agent vyžaduje následující porty brány firewall toobe otevřete v pořadí pro hello agenta toocommunicate s koncovými body služby Azure AD Health hello hello.</br></br><li>Port 443 protokolu TCP</li><li>Port 5671 protokolu TCP</li> |
| Povolit hello následující weby, pokud je povoleno rozšířené zabezpečení Internet Exploreru |Pokud je povoleno rozšířené zabezpečení Internet Exploreru, pak hello následující weby musí být povoleno na hello serveru, který je nainstalován agent pro probíhající toohave hello.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Hello federační server ve vaší organizaci, Azure Active Directory důvěryhodný. Příklad: https://sts.contoso.com</li> |
|Zákaz FIPS|Agenti Azure AD Connect Health nepodporují FIPS.|

## <a name="installing-hello-azure-ad-connect-health-agent-for-ad-fs"></a>Instalace hello agenta Azure AD Connect Health pro službu AD FS
toostart hello instalace agenta, poklikejte na stažený soubor .exe hello. Na první obrazovce hello kliknutím na tlačítko Instalovat.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install1.png)

Po dokončení instalace hello klikněte na tlačítko Konfigurovat.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install2.png)

Spustí se prostředí PowerShell okno tooinitiate hello agenta registračním procesem. Pokud budete vyzváni, přihlaste se pomocí účtu Azure AD, který má přístup k registraci agenta tooperform. Ve výchozím nastavení hello účet globálního správce má přístup.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install3.png)

Po přihlášení bude PowerShell pokračovat. Po jeho dokončení můžete PowerShell zavřít a hello konfigurace je hotová.

V tomto okamžiku hello agent služby by měla být spuštěn automaticky povolení hello agenta nahrávání hello vyžaduje data toohello cloudové služby Zabezpečené způsobem.

Pokud jste nesplnili všechny hello požadavky popsané v předchozích částech hello, upozornění se zobrazí v okně PowerShell hello. Se, zda text hello toocomplete [požadavky](active-directory-aadconnect-health-agent-install.md#requirements) před instalací agenta hello. Hello následující snímek obrazovky je příklad těchto chyb.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install4.png)

byl nainstalován tooverify hello agent, vyhledejte následující služby na serveru hello hello. Pokud jste dokončili hello konfigurace, že by již měl běžet. Jinak jsou zastaveny až po dokončení konfigurace hello.

* Diagnostické služby AD FS pro Azure AD Connect Health
* Služba analýz AD FS pro Azure AD Connect Health
* Služba monitorování AD FS pro Azure AD Connect Health

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install5.png)

### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Instalace agenta na servery se systémem Windows Server 2008 R2
Kroky pro servery se systémem Windows Server 2008 R2:

1. Zajistěte, aby že tento server hello běží na Service Pack 1 nebo vyšší.
2. Pro instalaci agenta vypněte rozšířené zabezpečení IE:
3. Na všech serverech hello před instalací hello agenta AD Health nainstalujte prostředí Windows PowerShell 4.0. tooinstall prostředí Windows PowerShell 4.0:
   * Nainstalujte [rozhraní Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) pomocí hello následující offline instalační program aplikace odkaz toodownload hello.
   * Instalace PowerShellu ISE (z funkcí systému Windows)
   * Nainstalujte hello [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
   * Nainstalovat aplikaci Internet Explorer verze 10 nebo vyšší na serveru hello. (Vyžadováno tooauthenticate hello služba Health Service, pomocí přihlašovacích údajů správce Azure.)
4. Další informace o instalaci prostředí Windows PowerShell 4.0 na Windows Server 2008 R2, najdete v článku wikiwebu hello [zde](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Povolení auditování služby AD FS
> [!NOTE]
> Tato část se týká pouze servery tooAD služby FS. Nemáte toofollow na Proxy servery webových aplikací hello tyto kroky.
>

Aby hello analýzy využití funkcí toogather a analyzovat data, hello agenta Azure AD Connect Health potřebuje hello informace z protokolů auditování hello AD FS. Tyto protokoly nejsou ve výchozím nastavení povolené. Protokoly, na server služby AD FS auditu hello použijte následující postupy tooenable AD FS auditování a toolocate hello služby AD FS.

#### <a name="tooenable-auditing-for-ad-fs-on-windows-server-2008-r2"></a>tooenable auditování služby AD FS v systému Windows Server 2008 R2
1. Klikněte na tlačítko **spustit**, bod příliš**programy**, bod příliš**nástroje pro správu**a potom klikněte na **místní zásady zabezpečení**.
2. Přejděte toohello **Security Settings\Local Policies\User Rights Management** složku a potom na položku Generovat audity zabezpečení.
3. Na hello **místní nastavení zabezpečení** ověřte, že je uvedený účet služby hello AD FS 2.0. Pokud není přítomen, klikněte na tlačítko **přidat uživatele nebo skupinu** , přidejte ho toohello seznamu a pak klikněte na tlačítko **OK**.
4. tooenable auditování, otevřete příkazový řádek se zvýšenými oprávněními a spusťte následující příkaz hello:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Zavřete místní zásady zabezpečení a potom otevřete snap-in Správa hello. hello tooopen modulu snap-in Správa, klikněte na tlačítko **spustit**, bodu příliš**programy**, bodu příliš**nástroje pro správu**a pak klikněte na AD FS 2.0 správy.
6. V podokně Akce hello klikněte na tlačítko Upravit vlastnosti federační služby.
7. V hello **vlastnosti služby FS** dialogovém okně klikněte na hello **události** kartě.
8. Vyberte hello **úspěšné audity** a **neúspěšné audity** zaškrtávací políčka.
9. Klikněte na **OK**.

#### <a name="tooenable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>tooenable auditování služby AD FS v systému Windows Server 2012 R2
1. Otevřete **místní zásady zabezpečení** otevřením **správce serveru** na hello úvodní obrazovce nebo správce serveru na hello panelu na ploše hello, pak klikněte na **nástroje/Místní zásady zabezpečení**.
2. Přejděte toohello **Security Settings\Local Policies\User Rights přiřazení** složku a pak dvojitým kliknutím **Generovat audity zabezpečení**.
3. Na hello **místní nastavení zabezpečení** ověřte, že je uvedený účet služby hello AD FS. Pokud není přítomen, klikněte na tlačítko **přidat uživatele nebo skupinu** , přidejte ho toohello seznamu a pak klikněte na tlačítko **OK**.
4. tooenable auditování, otevřete příkazový řádek se zvýšenými oprávněními a spusťte následující příkaz hello: ```auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable```.
5. Zavřít **místní zásady zabezpečení**a pak otevřete hello **Správa služby AD FS** modul snap-in (ve Správci serveru klikněte na nástroje a potom vyberte správu služby AD FS).
6. V podokně Akce hello, klikněte na **upravit vlastnosti federační služby**.
7. V dialogové okno Vlastnosti služby FS hello, klikněte na tlačítko hello **události** kartě.
8. Vyberte hello **úspěšné audity a neúspěšné audity** zaškrtněte políčka a pak klikněte na **OK**.

#### <a name="tooenable-auditing-for-ad-fs-on-windows-server-2016"></a>tooenable auditování služby AD FS v systému Windows Server 2016
1. Otevřete **místní zásady zabezpečení** otevřením **správce serveru** na hello úvodní obrazovce nebo správce serveru na hello panelu na ploše hello, pak klikněte na **nástroje/Místní zásady zabezpečení**.
2. Přejděte toohello **Security Settings\Local Policies\User Rights přiřazení** složku a pak dvojitým kliknutím **Generovat audity zabezpečení**.
3. Na hello **místní nastavení zabezpečení** ověřte, že je uvedený účet služby hello AD FS. Pokud není přítomen, klikněte na tlačítko **přidat uživatele nebo skupinu** , přidejte hello AD FS služby účet toohello seznamu a pak klikněte na tlačítko **OK**.
4. tooenable auditování, otevřete příkazový řádek se zvýšenými oprávněními a spusťte následující příkaz hello:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Zavřít **místní zásady zabezpečení**a pak otevřete hello **Správa služby AD FS** modul snap-in (ve Správci serveru klikněte na nástroje a potom vyberte správu služby AD FS).
6. V podokně Akce hello, klikněte na **upravit vlastnosti federační služby**.
7. V dialogové okno Vlastnosti služby FS hello, klikněte na tlačítko hello **události** kartě.
8. Vyberte hello **úspěšné audity a neúspěšné audity** zaškrtněte políčka a pak klikněte na **OK**. Tyto možnosti by měly být povolené ve výchozím nastavení.
9. Otevřete okno prostředí PowerShell a spusťte následující příkaz hello: ```Set-AdfsProperties -AuditLevel Verbose```.

Poznámka: Ve výchozím nastavení je povolena úroveň Basic. Další informace o hello [vylepšení auditu AD FS v systému Windows Server 2016](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/auditing-enhancements-to-ad-fs-in-windows-server-2016)


#### <a name="toolocate-hello-ad-fs-audit-logs"></a>protokoly auditu toolocate hello služby AD FS
1. Otevřete **Prohlížeč událostí**.
2. Přejděte tooWindows protokoly a vyberte možnost **zabezpečení**.
3. Na hello správné, klikněte na tlačítko **filtrovat aktuální protokoly**.
4. V části Zdroj události vyberte **Auditování služby AD FS**.

![Protokoly auditu služby AD FS](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [!WARNING]
> Zásady skupiny můžou zakázat auditování služby AD FS. Pokud je auditování služby AD FS zakázané, analýzy využití týkající se aktivit přihlašování nebudou dostupné. Zkontrolujte,že nepoužíváte zásady skupiny, které zakazují auditování služby AD FS.
>


## <a name="installing-hello-azure-ad-connect-health-agent-for-sync"></a>Instalace agenta hello Azure AD Connect Health pro synchronizaci
agent Hello Azure AD Connect Health pro synchronizaci se v hello posledním sestavení Azure AD Connect instaluje automaticky. toouse Azure AD Connect pro synchronizaci, potřebujete toodownload hello nejnovější verzi služby Azure AD Connect a nainstalujte ji. Si můžete stáhnout nejnovější verzi hello [zde](http://www.microsoft.com/download/details.aspx?id=47594).

byl nainstalován tooverify hello agent, vyhledejte následující služby na serveru hello hello. Pokud jste dokončili hello konfigurace, že by již měl běžet. Jinak jsou zastaveny až po dokončení konfigurace hello.

* Služba analýz synchronizace služby Azure AD Connect Health
* Služba monitorování synchronizace služby Azure AD Connect Health

![Ověření Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/services.png)

> [!NOTE]
> Nezapomeňte, že používání Azure AD Connect Health vyžaduje službu Azure AD Premium. Pokud nemáte Azure AD Premium, jste nelze toocomplete hello konfigurace v hello portálu Azure. Další informace najdete v tématu hello [požadavky na stránce](active-directory-aadconnect-health-agent-install.md#requirements).
>
>

## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Ruční registrace Azure AD Connect Health pro synchronizaci
Pokud hello Azure AD Connect Health pro synchronizaci registrace agenta selže po úspěšné instalaci Azure AD Connect, můžete použít následující příkaz prostředí PowerShell toomanually zaregistrujte agenta hello hello.

> [!IMPORTANT]
> Pomocí tohoto příkazu Powershellu je jenom potřeba, pokud neúspěšné registraci agenta hello po instalaci Azure AD Connect.
>
>

Hello následující PowerShell příkaz je potřeba pouze při hello neúspěšné registrace agenta stavu i po úspěšné instalaci a konfiguraci služby Azure AD Connect. služby Azure AD Connect Health Hello se spustí po hello agent byl úspěšně registrován.

Můžete ručně registrovat agenta hello Azure AD Connect Health pro synchronizaci pomocí hello následující příkaz prostředí PowerShell:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Hello příkaz přijímá následující parametry:

* AttributeFiltering: $true (výchozí) – Pokud Azure AD Connect nesynchronizuje nastavený výchozí atribut hello a byl přizpůsobené toouse filtrované atribut nastavit. V opačném případě $false.
* StagingMode: $false (výchozí) – Pokud server hello Azure AD Connect není v pracovním režimu, $true, pokud je hello server nakonfigurovat toobe v pracovním režimu.

Po zobrazení výzvy k ověření byste měli používat hello stejný účet globálního správce (například admin@domain.onmicrosoft.com), který se používá pro konfiguraci Azure AD Connect.

## <a name="installing-hello-azure-ad-connect-health-agent-for-ad-ds"></a>Instalace agenta Azure AD Connect Health pro službu AD DS hello
toostart hello instalace agenta, poklikejte na stažený soubor .exe hello. Na první obrazovce hello kliknutím na tlačítko Instalovat.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Po dokončení instalace hello klikněte na tlačítko Konfigurovat.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Spustí se příkazový řádek následovaný kódem PowerShellu, který provede Register-AzureADConnectHealthADDSAgent. Když výzvami toosign v tooAzure, pokračujte a přihlaste se.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Po přihlášení bude PowerShell pokračovat. Po jeho dokončení můžete PowerShell zavřít a hello konfigurace je hotová.

V tomto okamžiku hello služby by měl být spuštěn automaticky povolení hello agenta toomonitor a shromáždit data. Pokud jste nesplnili všechny hello požadavky popsané v předchozích částech hello, upozornění se zobrazí v okně PowerShell hello. Se, zda text hello toocomplete [požadavky](active-directory-aadconnect-health-agent-install.md#requirements) před instalací agenta hello. Hello následující snímek obrazovky je příklad těchto chyb.

![Ověření Azure AD Connect Health pro AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

byl nainstalován tooverify hello agent, vyhledejte následující služby na řadiči domény hello hello.

* Služba analýz AD DS pro Azure AD Connect Health
* Služba monitorování AD DS pro Azure AD Connect Health

Pokud jste dokončili hello konfigurace, by měl běžet již těchto služeb. Jinak jsou zastaveny až po dokončení konfigurace hello.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)


### <a name="agent-registration-using-powershell"></a>Registrace agenta pomocí PowerShellu
Po instalaci hello odpovídající agenta setup.exe, můžete provést kroku registrace agenta hello pomocí následujících příkazů prostředí PowerShell v závislosti na roli hello hello. Otevřete okno prostředí PowerShell a spusťte příslušný příkaz hello:

```
    Register-AzureADConnectHealthADFSAgent
    Register-AzureADConnectHealthADDSAgent
    Register-AzureADConnectHealthSyncAgent

```

Tyto příkazy přijmout "Pověření" jako parametr toocomplete hello registrace neinteraktivní způsobem nebo na počítači jádra serveru.
* Hello přihlašovací údaje se dají zachytit v proměnné prostředí PowerShell, který je předán jako parametr.
* Můžete zadat všechny Azure AD Identity, který má přístup tooregister hello agentů a nemá povolené ověřování MFA.
* Ve výchozím nastavení globální Správci mají přístup tooperform registraci agenta. Můžete povolit také jiné méně privilegované identity tooperform tento krok. Další informace o [Řízení přístupu na základě rolí](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control).

```
    $cred = Get-Credential
    Register-AzureADConnectHealthADFSAgent -Credential $cred

```

## <a name="configure-azure-ad-connect-health-agents-toouse-http-proxy"></a>Konfigurace agentů Azure AD Connect Health toouse server Proxy protokolu HTTP
Toowork agenty Azure AD Connect Health můžete nakonfigurovat proxy server HTTP.

> [!NOTE]
> * Použití "Netsh WinHttp set ProxyServerAddress" není podporováno, protože hello agent používá webových požadavků System.Net toomake místo služby Microsoft Windows HTTP Services.
> * Hello nakonfigurovaná adresa proxy serveru Http je použité toopass prostřednictvím šifrovaných zpráv protokolu Https.
> * Ověřené servery proxy (použití HTTPBasic) nejsou podporované.
>
>

### <a name="change-health-agent-proxy-configuration"></a>Změna konfigurace proxy serveru agenta stavu
Máte následující možnosti tooconfigure Azure AD Connect agenta stavu toouse proxy serveru HTTP hello.

> [!NOTE]
> Všechny služby agenta Azure AD Connect Health je třeba restartovat, aby toobe nastavení proxy serveru hello aktualizovat. Spusťte následující příkaz hello:<br>
> Restart-Service AdHealth*
>
>

#### <a name="import-existing-proxy-settings"></a>Import existujících nastavení proxy serveru
##### <a name="import-from-internet-explorer"></a>Import z Internet Exploreru
Nastavení proxy serveru HTTP z Internet Exploreru můžete importovat, toobe používané hello agenty Azure AD Connect Health. Na všech serverech hello běží agent stavu hello spusťte následující příkaz prostředí PowerShell hello:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Import ze služby WinHTTP
Nastavení proxy serveru WinHTTP můžete importovat, toobe používané hello agenty Azure AD Connect Health. Na všech serverech hello běží agent stavu hello spusťte následující příkaz prostředí PowerShell hello:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Ruční zadání adres proxy serverů
Na všech serverech hello systémem hello Agent stavu, tak, že spustíte následující příkaz prostředí PowerShell hello můžete ručně zadat proxy server:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Příklad: *Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress myproxyserver: 443*

* Adresa může být název serveru DNS, který lze přeložit, nebo adresa IPv4.
* Port můžete vynechat. Pokud tento parametr vynecháte, bude jako výchozí vybrán port 443.

#### <a name="clear-existing-proxy-configuration"></a>Vymazání existující konfigurace proxy serveru
Hello existující konfiguraci proxy serveru můžete vymazat spuštěním hello následující příkaz:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Čtení aktuálního nastavení proxy serveru
Můžete si přečíst hello aktuálně nakonfigurované nastavení proxy serveru tak, že spustíte následující příkaz hello:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-tooazure-ad-connect-health-service"></a>Testovací připojení tooAzure AD Connect Health Service
Je možné, že problémy mohou nastat způsobit agenta Azure AD Connect Health hello toolose připojení s hello Azure AD Connect Health service. Patří sem problémy se sítí, problémy s oprávněním a další příčiny.

Pokud hello agenta nelze toosend data toohello Azure AD Connect Health service déle než dvě hodiny, je vyznačené hello následující upozornění portálu hello: "data služby stavu není až toodate." Si můžete ověřit, pokud je agent Azure AD Connect Health hello vliv tak, že spustíte následující příkaz prostředí PowerShell hello možné tooupload data toohello Azure AD Connect Health service:

    Test-AzureADConnectHealthConnectivity -Role ADFS

parametr "role" Hello současnosti přijímá hello následující hodnoty:

* ADFS
* Sync
* PŘIDÁ

Příznak - ShowResults hello můžete použít v příkazu tooview hello podrobné protokoly. Použijte hello následující ukázka:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

> [!NOTE]
> Nástroje pro připojení hello toouse, je nutné první registraci agenta dokončení hello. Pokud si nejste registraci agenta může toocomplete hello, ujistěte se, že jste splnili všechny hello [požadavky](active-directory-aadconnect-health-agent-install.md#requirements) pro Azure AD Connect Health. Tento test připojení se ve výchozím nastavení provádí během registrace agenta.
>
>

## <a name="related-links"></a>Související odkazy
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
