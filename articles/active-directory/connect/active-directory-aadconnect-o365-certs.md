---
title: "aaaCertificate obnovení pro uživatele služeb Office 365 a Azure AD | Microsoft Docs"
description: "Tento článek vysvětluje tooOffice 365 uživatelé jak tooresolve problémy s e-mailů, které je informovat o obnovení certifikátu."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 543b7dc1-ccc9-407f-85a1-a9944c0ba1be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b9b309e06949dc5488cd628650be413f366ed347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Obnovení federačních certifikátů pro Office 365 a Azure Active Directory
## <a name="overview"></a>Přehled
Pro úspěšné federaci mezi Azure Active Directory (Azure AD) a služby Active Directory Federation Services (AD FS) by měl odpovídat hello certifikátů používaných službou AD FS toosign zabezpečení tokeny tooAzure AD, co je nakonfigurováno ve službě Azure AD. Jakákoli neshoda může způsobit toobroken vztah důvěryhodnosti. Azure AD zajistí, že tyto informace jsou uchovávány v synchronizaci při nasazení služby AD FS a Proxy webových aplikací (pro přístup z extranetu).

Tento článek obsahuje další informace o toomanage podpisové certifikáty tokenu a udržovat synchronizované s Azure AD, v následujících případech hello:

* Nenasazujete hello Proxy webových aplikací, a proto není k dispozici v extranetu hello hello federačních metadat.
* Nepoužíváte hello výchozí konfiguraci služby AD FS pro podpisových certifikátů tokenu.
* Používáte poskytovatel identity jiného výrobce.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Výchozí konfiguraci služby AD FS pro podpisových certifikátů tokenu
Hello podpisové a dešifrovací certifikáty tokenu jsou obvykle certifikáty podepsané svým držitelem a jsou vhodné pro jeden rok. Ve výchozím nastavení, služba AD FS obsahuje procesu automatického obnovení označovaného jako **AutoCertificateRollover**. Pokud používáte službu AD FS 2.0 nebo novější, Office 365 a Azure AD automaticky aktualizujte certifikát, než vyprší její platnost.

### <a name="renewal-notification-from-hello-office-365-portal-or-an-email"></a>Obnovení oznámení z portálu hello Office 365 nebo e-mailu
> [!NOTE]
> Pokud jste dostali e-mailu nebo portálu oznámení s dotazem, toorenew svůj certifikát pro Office, najdete v [Správa změny tootoken podpisové certifikáty](#managecerts) toocheck, pokud potřebujete tootake žádnou akci. Microsoft má informace o potenciální problém, který může vést toonotifications pro obnovení certifikátu, které jsou odesílány, i když není vyžadována žádná akce.
>
>

Azure AD pokusí toomonitor hello federačních metadat a podpisových certifikátů, jak tato metadata aktualizace hello tokenu. 30 dní před vypršením platnosti hello hello tokenu podpisové certifikáty, Azure AD ověří, pokud nové certifikáty jsou k dispozici pomocí cyklického dotazování federačních metadat hello.

* Pokud můžete úspěšně dotazovat hello federačních metadat a načíst hello nové certifikáty, je vydána toohello uživatele žádné e-mailových oznámení nebo upozornění portálu hello Office 365.
* Pokud ho nelze načíst hello nové podpisových certifikátů tokenu, buď protože hello federačních metadat je nedostupná nebo není povoleno automatické certifikáty vyměnit, Azure AD vydá e-mailových oznámení a upozornění portálu hello Office 365.

![Oznámení portálu Office 365](./media/active-directory-aadconnect-o365-certs/notification.png)

> [!IMPORTANT]
> Pokud používáte službu AD FS, tooensure provozní kontinuitu, ověřte, že vaše servery mají hello následující aktualizace, takže nedojde k selhání ověřování známých problémů. To snižuje známé problémy serveru proxy služby AD FS pro toto období budoucí obnovení a obnovení:
>
> Server 2012 R2 - [systému Windows Server kumulativní může 2014](http://support.microsoft.com/kb/2955164)
>
> Server 2008 R2 a 2012 – [nezdaří ověřování prostřednictvím proxy serveru v systému Windows Server 2012 nebo Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)
>
>

## Zkontrolujte, pokud certifikáty hello potřebovat toobe aktualizovat<a name="managecerts"></a>
### <a name="step-1-check-hello-autocertificaterollover-state"></a>Krok 1: Kontrola stavu AutoCertificateRollover hello
Na serveru služby AD FS otevřete prostředí PowerShell. Zkontrolujte, zda je nastavená hodnota AutoCertificateRollover hello tooTrue.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

>[!NOTE] 
>Pokud používáte službu AD FS 2.0, spusťte nejprve přidat-Pssnapin Microsoft.Adfs.Powershell.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Krok 2: Ověřte, jestli jsou synchronizované služby AD FS a Azure AD
Na serveru služby AD FS otevřete příkazového řádku hello Azure AD PowerShell a připojte tooAzure AD.

> [!NOTE]
> Můžete si stáhnout Azure AD PowerShell [zde](https://technet.microsoft.com/library/jj151815.aspx).
>
>

    Connect-MsolService

Zkontrolujte hello certifikáty konfigurovány ve službě AD FS a vlastnosti vztahu důvěryhodnosti služby Azure AD pro hello zadané domény.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Pokud kryptografické otisky hello v obou hello výstupy shodu, jsou certifikáty synchronizované s Azure AD.

### <a name="step-3-check-if-your-certificate-is-about-tooexpire"></a>Krok 3: Kontrola, zda váš certifikát je o tooexpire
Ve výstupu hello Get-MsolFederationProperty nebo Get-AdfsCertificate zkontrolujte datum hello v části "Není po." Pokud je datum hello rychle méně než 30 dní, můžete provést akce.

| AutoCertificateRollover | Certifikáty, které jsou synchronizované s Azure AD | Federační metadata jsou veřejně přístupné | Platnosti | Akce |
|:---:|:---:|:---:|:---:|:---:|
| Ano |Ano |Ano |- |Není vyžadována žádná akce. V tématu [podepisování obnovit certifikát automaticky](#autorenew). |
| Ano |Ne |- |Méně než 15 dní |Obnovte okamžitě. V tématu [podepisování obnovit certifikát ručně](#manualrenew). |
| Ne |- |- |Méně než 30 dní |Obnovte okamžitě. V tématu [podepisování obnovit certifikát ručně](#manualrenew). |

\[-] Nezáleží.

## Obnovit automaticky podpisový certifikát tokenu hello (doporučeno)<a name="autorenew"></a>
Tooperform není třeba žádné ruční kroky, pokud jsou splněny obě následující hello:

* Nasazení Proxy webových aplikací, které můžete povolit přístup toohello metadatům federace z extranetu hello.
* Používáte výchozí konfigurace hello služby AD FS (AutoCertificateRollover je povoleno).

Zkontrolujte, zda lze automaticky aktualizovat hello následující tooconfirm, který hello certifikátu.

**1. hello služby AD FS vlastnost AutoCertificateRollover musí být nastavena tooTrue.** To znamená, že služby AD FS automaticky vygeneruje nový token podepisování a dešifrování tokenů, certifikáty, před hello staré těch, které jsou vyprší.

**2. hello AD FS federačních metadat je veřejně přístupná.** Zkontrolujte, jestli federačních metadat veřejně přístupná přechodem toohello následující adresu URL z počítače hello veřejného Internetu (z podnikové sítě hello):

/federationmetadata/2007-06/federationmetadata.xml https:// (your_FS_name)

kde `(your_FS_name) `je nahrazena hello hostitele název služby federation service vaše organizace používá, třeba fs.contoso.com.  Pokud jsou možné tooverify z těchto nastavení úspěšně, není nutné toodo cokoliv jiného.  

Příklad: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Obnovte ručně podpisový certifikát tokenu hello<a name="manualrenew"></a>
Můžete se rozhodnout toorenew hello podpisových certifikátů tokenu ručně. Například hello následující scénáře mohou fungovat lépe pro ruční obnovení:

* Token, podpisové certifikáty jsou certifikáty podepsané držitelem. Hello Nejčastější příčinou pro tuto je, že vaše organizace spravuje certifikáty služby AD FS zapsané od organizace certifikační autority.
* Zabezpečení sítě neumožňuje hello federační metadata toobe veřejně dostupné.

V těchto scénářích při každé aktualizaci hello token podpisové certifikáty, je nutné také aktualizovat doménu Office 365 pomocí příkazu prostředí PowerShell text hello, MsolFederatedDomain aktualizace.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Krok 1: Zajistěte, aby služba AD FS nové podpisových certifikátů tokenu
**Jiné než výchozí konfigurace**

Pokud používáte jiné než výchozí konfiguraci služby AD FS (kde **AutoCertificateRollover** je nastaven příliš**False**), pravděpodobně používáte vlastní certifikáty (není podepsaný). Další informace o tom, jak toorenew hello AD FS podpisové certifikáty najdete v tématu [pokyny pro zákazníky bez použití služby AD FS certifikátů podepsaných svým držitelem](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Federační metadata nejsou veřejně dostupné**

Na druhé straně hello, pokud **AutoCertificateRollover** je nastaven příliš**True**, ale není veřejně přístupná federačních metadat, nejprve se ujistěte, že byla vygenerována nové podpisové certifikáty tokenu AD SLUŽBY FS. Potvrďte, že máte nový token podpisové certifikáty podle trvá hello následující kroky:

1. Ověřte, že jste přihlášeni serveru toohello primární AD FS.
2. Zkontrolujte aktuální podpisové certifikáty hello ve službě AD FS Otevřete příkazové okno prostředí PowerShell a spuštěním hello následující příkaz:

    PS C:\>Get-ADFSCertificate – CertificateType podpisové

   > [!NOTE]
   > Pokud používáte službu AD FS 2.0, byste měli nejdřív spustit Add-Pssnapin Microsoft.Adfs.Powershell.
   >
   >
3. Podívejte se na výstup příkazu hello na všechny certifikáty uvedené. Pokud službu AD FS vygeneruje nový certifikát, měli byste vidět dva certifikáty ve výstupu hello: jeden pro které hello **IsPrimary** hodnota je **True** a hello **neplatí po** je datum v rámci 5 dní a jeden pro kterou **IsPrimary** je **False** a **neplatí po** spočívá v roce v budoucnu hello.
4. Pokud jste pouze se jeden certifikát a hello **neplatí po** datum je do 5 dnů, je třeba toogenerate nový certifikát.
5. toogenerate nový certifikát, spustit následující příkaz na příkazovém řádku prostředí PowerShell hello: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.
6. Ověřte hello aktualizace tak, že spustíte následující příkaz znovu hello: PS C:\>Get-ADFSCertificate – CertificateType podpisové

Teď by měl být uvedený dva certifikáty, z nichž jeden je **neplatí po** datum přibližně jeden rok v hello budoucí a pro které hello **IsPrimary** hodnota je **False**.

### <a name="step-2-update-hello-new-token-signing-certificates-for-hello-office-365-trust"></a>Krok 2: Aktualizace hello nový token podpisové certifikáty pro vztah důvěryhodnosti hello Office 365
Aktualizace Office 365 s hello nový token podpisový certifikáty toobe používá hello vztahu důvěryhodnosti, následujícím způsobem.

1. Otevřete hello Microsoft Azure Active Directory modul pro prostředí Windows PowerShell.
2. Spustit $cred = Get-Credential. Když tato rutina vás vyzve k zadání přihlašovacích údajů, zadejte přihlašovací údaje účtu správce cloudové služby.
3. Spustit Connect-MsolService – $cred na přihlašovací údaje. Tato rutina připojí toohello cloudové služby. Vytváření kontextu, který vás spojí toohello cloudové služby se vyžaduje před spuštěním další rutiny hello nainstalován nástrojem hello.
4. Pokud spustíte tyto příkazy v počítači, který není primární federační server hello služby AD FS, spusťte sadu MSOLAdfscontext-počítače <AD FS primary server>, kde <AD FS primary server> je hello interní název plně kvalifikovaný název domény serveru hello primární AD FS. Tato rutina vytvoří kontext, který vás spojí tooAD služby FS.
5. Spuštění aktualizace MSOLFederatedDomain – DomainName <domain>. Tato rutina aktualizuje nastavení hello ze služby AD FS do hello cloudové služby a nakonfiguruje hello vztah důvěryhodnosti mezi dvěma hello.

> [!NOTE]
> Pokud potřebujete toosupport víc domén nejvyšší úrovně, jako je například contoso.com a fabrikam.com, musíte použít hello **SupportMultipleDomain** přepínač s rutiny. Další informace najdete v tématu [podpory pro víc domén nejvyšší úrovně](active-directory-aadconnect-multiple-domains.md).
>
>

## Opravte vztah důvěryhodnosti služby Azure AD pomocí Azure AD Connect<a name="connectrenew"></a>
Pokud jste nakonfigurovali farma služby AD FS a vztah důvěryhodnosti služby Azure AD pomocí Azure AD Connect, můžete použít Azure AD Connect toodetect, pokud potřebujete žádnou akci pro tootake podpisové certifikáty tokenu. Pokud budete potřebovat certifikáty hello toorenew, můžete použít Azure AD Connect toodo tak.

Další informace najdete v tématu [opravu vztahu důvěryhodnosti hello](active-directory-aadconnect-federation-management.md).
