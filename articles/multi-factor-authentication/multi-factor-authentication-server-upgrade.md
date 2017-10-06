---
title: upgrade serveru MFA aaaAzure | Microsoft Docs
description: "Kroky a pokyny tooupgrade hello Azure Multi-Factor Authentication Server tooa novější verze."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: aaa8d400e0e5f1c6be3a6d22cde6dd893ef4d546
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-toohello-latest-azure-multi-factor-authentication-server"></a>Upgrade toohello nejnovější Azure Multi-Factor Authentication Server

Tento článek vás provede procesem hello proces upgradu serveru Azure Multi-Factor Authentication (MFA) verze 6.0 nebo vyšší. Pokud potřebujete tooupgrade starší verzi hello agenta PhoneFactor, podívejte se příliš[hello upgradu agenta PhoneFactor tooAzure aplikace Multi-Factor Authentication Server](multi-factor-authentication-get-started-server-upgrade.md).

Pokud jste upgrade z verze 6.x nebo starší toov7.x nebo novější, změňte všechny součásti z rozhraní .NET 2.0 too.NET 4.5. Všechny součásti také vyžadují Microsoft Visual C++ 2015 Redistributable Update 1 nebo vyšší. Instalační program MFA Server Hello nainstaluje obě verze hello x86 a x64 všechny tyto komponenty, pokud již nejsou nainstalovány. Pokud hello portálu User Portal a webová služba mobilní aplikace spustit na samostatných serverech, musíte tooinstall tyto balíčky před provedením upgradu těchto součástí. Můžete vyhledat hello nejnovější aktualizace Microsoft Visual C++ 2015 Redistributable na hello [Microsoft Download Center](https://www.microsoft.com/en-us/download/). 

## <a name="install-hello-latest-version-of-azure-mfa-server"></a>Nainstalujte nejnovější verzi Azure MFA Server hello

1. Postupujte pokynů hello v [hello stažení serveru Azure Multi-Factor Authentication Server](multi-factor-authentication-get-started-server.md#download-the-azure-multi-factor-authentication-server) tooget hello nejnovější verzi hello Azure MFA serveru.
2. Vytvořte záložní kopii hello MFA Server datový soubor nacházející se v C:\Program Files\Multi-Factor Authentication Server\Data\PhoneFactor.pfdata (v případě hello výchozí umístění instalace) na hlavním serveru MFA.
3. Pokud spouštíte více serverů pro zajištění vysoké dostupnosti, změňte hello klientské systémy, které ověření toohello MFA Server tak, aby se zastavit odesílání provozu toohello servery, které provádíte upgrade. Pokud používáte nástroj pro vyrovnávání zatížení, odeberte MFA serveru z nástroje pro vyrovnávání zatížení hello, hello upgrade a pak přidejte hello server zpět do farmy hello.
4. Spuštění nového instalačního programu hello na každý Server MFA. Nejprve upgradujte podřízené servery, protože můžou přečíst hello starý datový soubor replikuje pomocí hlavního hello. 

  Není nutné toouninstall váš aktuální Server MFA před spuštěné hello Instalační služby. Hello instalační program provádí upgrade na místě. Hello cesta instalace je převzata z registru hello z předchozí instalace hello, takže se nainstaluje v hello stejné umístění (například C:\Program Files\Multi-Factor Authentication Server). 
  
5. Pokud jste balíček výzvami tooinstall Microsoft Visual C++ 2015 Redistributable aktualizace, přijetí výzvy hello. Obě verze hello x86 a x64 hello balíčku jsou nainstalovány.
5. Pokud používáte hello Web Service SDK, zobrazí se výzva tooinstall hello nové sady SDK webové služby. Při instalaci hello nové sady SDK webové služby, ujistěte se, že tento název virtuálního adresáře hello odpovídá hello z dříve nainstalovaných virtuálního adresáře (například MultiFactorAuthWebServiceSdk).
6. Opakujte kroky hello na všechny podřízené servery. Zvyšte úroveň jednoho hello podřízené toobe hello nový vzor, pak se upgrade hello starého hlavního serveru. 

## <a name="upgrade-hello-user-portal"></a>Upgrade hello portálu User Portal

1. Vytvořte záložní kopii hello souboru web.config, který je v hello virtuální adresář hello umístění instalace portálu User Portal (např. C:\inetpub\wwwroot\MultiFactorAuth). Pokud výchozí motiv toohello nebyly provedeny žádné změny, vytvořte záložní kopii hello App_Themes\Default také složky. Je lepší toocreate kopie hello výchozí složky a vytvořit nový motiv než toochange hello výchozí motiv.
2. Pokud hello portálu User Portal běží na stejném serveru jako ostatní součásti serveru MFA hello hello hello instalace MFA serveru vás vyzve k tooupdate hello portálu User Portal. Přijetí výzvy hello a nainstalujte aktualizaci hello portálu User Portal. Zkontrolujte, zda že tento název virtuálního adresáře hello odpovídá hello z dříve nainstalovaných virtuálního adresáře (například MultiFactorAuth).
3. Pokud hello portálu User Portal je na vlastním serveru kopírovat soubor MultiFactorAuthenticationUserPortalSetup64.msi hello z hello nainstalujte umístění hello vícefaktorového ověřování serverů a umístí jej do hello portálu User Portal webový server. Spusťte instalační program hello. 

  Pokud dojde k chybě stanovení, "Microsoft Visual C++ 2015 Redistributable Update 1 nebo vyšší je povinné," stáhnout a nainstalovat nejnovější balíček aktualizace hello z hello [Microsoft Download Center](https://www.microsoft.com/download/). Nainstalujte obě verze x86 a x64 hello.

4. Po aktualizaci User Portal je nainstalován software hello porovnejte hello web.config zálohování, které jste provedli v kroku 1 s hello nový soubor web.config. Pokud žádné nové atributy neexistují v hello nového souboru web.config, zkopírujte do hello virtuální adresář toooverwrite hello nový zálohování souboru web.config. Další možností je toocopy a vložte hello appSettings hodnoty a hello adresa URL webové služby sady SDK z hello záložní soubor do nového souboru web.config hello.

Pokud máte hello portálu User Portal na více serverech, opakujte instalaci hello na všechny z nich. 


## <a name="upgrade-hello-mobile-app-web-service"></a>Upgrade hello webové služby mobilní aplikace

1. Vytvořte záložní kopii hello souboru web.config, který je v hello virtuální adresář hello umístění instalace webové služby mobilní aplikace (například C:\inetpub\wwwroot\app nebo C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService).
2. Kopírovat soubor MultiFactorAuthenticationMobileAppWebServiceSetup64.msi hello z hello nainstalujte umístění hello vícefaktorového ověřování serverů a umístí jej do hello mobilní aplikace registrace webový server.
3. Spusťte instalační program hello. 

  Pokud dojde k chybě s oznámením, že Microsoft Visual C++ 2015 Redistributable Update 1 nebo vyšší není vyžadováno, stáhněte a nainstalujte nejnovější balíček aktualizace hello z hello [Microsoft Download Center](https://www.microsoft.com/download/). Nainstalujte obě verze x86 a x64 hello.

4. Po instalaci softwaru webové služby mobilní aplikace hello aktualizovat porovnejte hello souboru web.config, který byl zálohován v kroku 1 s hello nový soubor web.config. Pokud žádné nové atributy neexistují v hello nového souboru web.config, můžete zkopírovat uloženého souboru web.config zpět do virtuálního adresáře hello a přepsat hello nový. Další možností je toocopy a vložte hello appSettings hodnoty a hello adresa URL webové služby sady SDK z hello záložní soubor do nového souboru web.config hello.

Pokud máte hello webové služby mobilní aplikace na více serverech, opakujte instalaci hello na všechny z nich. 

## <a name="upgrade-hello-ad-fs-adapters"></a>Upgrade hello adaptéry AD FS


### <a name="if-mfa-runs-on-different-servers-than-ad-fs"></a>Pokud vícefaktorové ověřování běží na různých serverech než služba AD FS

Tyto pokyny platí pouze pro spuštění aplikace Multi-Factor Authentication Server samostatně z vašich serverů služby AD FS. Pokud obě služby spustili hello stejných serverů, tuto část přeskočte a přejděte toohello kroky instalace. 

1. Uložení kopie souboru MultiFactorAuthenticationAdfsAdapter.config hello, který byl zaregistrován ve službě AD FS nebo exportovat konfiguraci hello pomocí hello následující příkaz prostředí PowerShell: `Export-AdfsAuthenticationProviderConfigurationData -Name [adapter name] -FilePath [path tooconfig file]`. v závislosti na dříve nainstalované verzi hello je název adaptéru Hello "WindowsAzureMultiFactorAuthentication" nebo "AzureMfaServerAuthentication".
2. Zkopírujte následující soubory z hello MFA Server instalace umístění toohello AD FS serverů hello:

  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config

3. Upravte skript Register-MultiFactorAuthenticationAdfsAdapter.ps1 hello přidáním `-ConfigurationFilePath [path]` toohello konec hello `Register-AdfsAuthenticationProvider` příkaz. Nahraďte *[cesta]* s hello úplná cesta toohello MultiFactorAuthenticationAdfsAdapter.config soubor nebo soubor konfigurace hello exportovali v předchozím kroku hello. 

  Zkontrolujte hello atributů v nové toosee MultiFactorAuthenticationAdfsAdapter.config hello, pokud budou odpovídat hello původního konfiguračního souboru. Pokud žádné atributy přidané nebo odebrané v nové verzi hello, zkopírovat hodnoty atributu hello hello původní konfigurace souboru toohello nový nebo upravit hello toomatch souboru původní konfigurace.

### <a name="install-new-ad-fs-adapters"></a>Instalace nové adaptéry služby AD FS

> [!IMPORTANT] 
> Uživatelé nebudou požadované tooperform dvoustupňové ověřování během kroky 3-8 v této části. Pokud máte služby AD FS nakonfigurovaný v více clusterů, můžete odebrat, upgrade a obnovení farmy jednotlivé clustery v hello nezávisle na hello další clustery tooavoid výpadku.

1. Odeberte některé servery služby AD FS z farmy hello. Aktualizujte tyto servery při hello ponecháním ostatních v běhu.
2. Nainstalujte nový adaptér služby AD FS hello na každém serveru odebrány z farmy hello AD FS. Pokud hello MFA serveru je nainstalovaná na každém serveru služby AD FS, můžete aktualizovat pomocí Správce serveru MFA Server hello UX V opačném aktualizujte spuštěním soubory MultiFactorAuthenticationAdfsAdapterSetup64.msi. 

  Pokud dojde k chybě stanovení, "Microsoft Visual C++ 2015 Redistributable Update 1 nebo vyšší je povinné," stáhnout a nainstalovat nejnovější balíček aktualizace hello z hello [Microsoft Download Center](https://www.microsoft.com/download/). Nainstalujte obě verze x86 a x64 hello.

3. Přejděte příliš**služby AD FS** > **zásady ověřování** > **upravit globální zásady vícefaktorového ověřování**. Zrušte zaškrtnutí políčka **WindowsAzureMultiFactorAuthentication** nebo **AzureMFAServerAuthentication** (v závislosti na aktuální verzi hello nainstalovaná). 

  Po dokončení tohoto kroku dvoustupňové ověřování přes MFA Server není k dispozici v tomto clusteru služby AD FS, dokud nedokončíte krok 8.

4. Unregister hello starší verze hello AD FS adapteru pomocí skriptu prostředí PowerShell Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 hello. Ujistěte se, že hello *-název* hello název, který se zobrazí v kroku 3 odpovídá parametru ("WindowsAzureMultiFactorAuthentication" nebo "AzureMFAServerAuthentication"). Vzhledem k tomu, že je konfigurace centrálních platí tooall servery v clusteru hello stejnou AD FS.
5. Zaregistrujte nový adaptér služby AD FS hello spuštěním skriptu prostředí PowerShell Register-MultiFactorAuthenticationAdfsAdapter.ps1 hello. Vzhledem k tomu, že je konfigurace centrálních platí tooall servery v clusteru hello stejnou AD FS.
6. Hello restartování služby AD FS na každém serveru odebrat z hello farma služby AD FS.
7. Přidejte servery hello aktualizovat zpět farmy toohello AD FS a odebrat hello jiné servery z farmy hello.
8. Přejděte příliš**služby AD FS** > **zásady ověřování** > **upravit globální zásady vícefaktorového ověřování**. Zkontrolujte **AzureMfaServerAuthentication**.
9. Opakujte krok 2 tooupdate hello servery nyní odebrány z farmy hello AD FS a restartujte službu hello AD FS na těchto serverech.
10. Přidáte tyto servery zpátky do farmy hello služby AD FS.

## <a name="next-steps"></a>Další kroky

- Získat příklady [pokročilé scénáře s Azure Multi-Factor Authentication a sítě VPN třetí strany](multi-factor-authentication-advanced-vpn-configurations.md)

- [Synchronizace MFA serveru s Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)

- [Konfigurovat ověřování systému Windows](multi-factor-authentication-get-started-server-windows.md) pro vaše aplikace
