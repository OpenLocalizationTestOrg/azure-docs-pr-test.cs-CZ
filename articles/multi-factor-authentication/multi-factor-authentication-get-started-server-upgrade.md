---
title: aaaUpgrade PhoneFactor tooAzure MFA serveru | Microsoft Docs
description: "Začínáme s Azure MFA serveru po upgradu ze staršího agenta phonefactor hello."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 42838ff7-bdf2-4d06-bacc-b3839a00cd76
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.openlocfilehash: 15b7b8517929c30f66e6a39cd44c69d12d25c6d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-hello-phonefactor-agent-tooazure-multi-factor-authentication-server"></a>Upgrade agenta PhoneFactor tooAzure aplikace Multi-Factor Authentication Server hello
tooupgrade hello PhoneFactor agenta v5.x nebo starší tooAzure aplikace Multi-Factor Authentication Server, odinstalujte hello agenta PhoneFactor a nejprve spojit součásti. Potom hello aplikace Multi-Factor Authentication Server a jeho přidružených součásti lze nainstalovat.

## <a name="uninstall-hello-phonefactor-agent"></a>Odinstalujte agenta PhoneFactor hello

1. Nejprve zálohujte datový soubor PhoneFactor hello. Hello výchozí umístění instalace je C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.

2. Pokud je nainstalovaná hello uživatelského portálu:
  1. Přejděte toohello složky instalace a zálohujte soubor web.config hello. Hello výchozí umístění instalace je C:\inetpub\wwwroot\PhoneFactor.

  2. Pokud jste přidali vlastní motivy toohello portál, zálohujte vlastní složky pod adresářem C:\inetpub\wwwroot\PhoneFactor\App_Themes hello.

  3. Odinstalujte hello portálu User Portal, buď prostřednictvím hello agenta PhoneFactor (dostupné, pouze pokud je nainstalován na hello stejný server jako hello agenta PhoneFactor) nebo prostřednictvím Windows programy a funkce.

3. Pokud je nainstalovaná hello webové služby mobilní aplikace:

  1. Přejděte toohello složky instalace a zálohujte soubor web.config hello. Hello výchozí umístění instalace je C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.

  2. Odinstalujte hello webové služby mobilní aplikace prostřednictvím Windows programy a funkce.

4. Pokud je nainstalovaná hello Web Service SDK, odinstalujte ji prostřednictvím agenta PhoneFactor hello nebo prostřednictvím Windows programy a funkce.

5. Odinstalujte hello PhoneFactor Agent prostřednictvím programů Windows a funkce.

## <a name="install-hello-multi-factor-authentication-server"></a>Instalace aplikace Multi-Factor Authentication Server hello

Hello cesta instalace je převzata z hello registru z předchozí instalace agenta PhoneFactor hello, takže by se měla nainstalovat v hello stejné umístění (například C:\Program Files\PhoneFactor). Nové instalace mají různé výchozí instalační cesty (například C:\Program Files\Multi-Factor Authentication Server). Hello datový soubor zanechaný předchozí hello agenta PhoneFactor musí být upgradován během instalace, takže nastavení uživatelů a měli byste být, že existuje po instalaci hello nové aplikace Multi-Factor Authentication Server.

1. Pokud se zobrazí výzva, aktivovat hello aplikace Multi-Factor Authentication Server a ujistěte se, že je přiřazen toohello správné replikační skupině.

2. Pokud byla hello Web Service SDK dříve nainstalována, nainstalujte hello nový Web Service SDK prostřednictvím hello uživatelského rozhraní serveru Multi-Factor Authentication.

  Hello výchozí název virtuálního adresáře je nyní **MultiFactorAuthWebServiceSdk** místo **PhoneFactorWebServiceSdk**. Pokud chcete toouse hello předchozí název, musíte změnit název hello hello virtuálního adresáře během instalace. Jinak Pokud povolíte hello instalace toouse hello nového výchozího názvu, máte toochange hello URL ve všech aplikacích, tento odkaz hello toopoint sady SDK webové služby (např. hello portálu User Portal a webová služba mobilní aplikace) na správné místo hello.

3. Pokud hello portálu User Portal dříve instalovaná na hello serveru agenta PhoneFactor, nainstalujte hello nové služby Multi-Factor Authentication User Portal prostřednictvím hello uživatelského rozhraní serveru Multi-Factor Authentication.

  Hello výchozí název virtuálního adresáře je nyní **MultiFactorAuth** místo **PhoneFactor**. Pokud chcete toouse hello předchozí název, musíte změnit název hello hello virtuálního adresáře během instalace. Jinak Pokud povolíte hello instalace toouse hello nového výchozího názvu, doporučujeme klikněte na ikonu portálu pro uživatele hello v hello aplikace Multi-Factor Authentication Server a aktualizovat hello URL uživatelského portálu na kartě nastavení hello.

4. Pokud hello portál pro uživatele nebo webové služby mobilní aplikace byla předtím nainstalovaná na jiném serveru než PhoneFactor Agent hello:

  1. Přejděte toohello umístění instalace (například C:\Program Files\PhoneFactor) a zkopírujte jeden nebo více instalačních programů toohello jiný server. Existují 32bitové a 64bitové verze instalačních programů pro hello portálu User Portal a webová služba mobilní aplikace. Nazývají se MultiFactorAuthenticationUserPortalSetupXX.msi a MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.

  2. tooinstall hello portálu User Portal na hello webový server, otevřete příkazový řádek jako správce a spusťte soubor MultiFactorAuthenticationUserPortalSetupXX.msi.

    Hello výchozí název virtuálního adresáře je nyní **MultiFactorAuth** místo **PhoneFactor**. Pokud chcete toouse hello předchozí název, musíte změnit název hello hello virtuálního adresáře během instalace. Jinak Pokud povolíte hello instalace toouse hello nového výchozího názvu, doporučujeme klikněte na ikonu portálu pro uživatele hello v hello aplikace Multi-Factor Authentication Server a aktualizovat hello URL uživatelského portálu na kartě nastavení hello. Stávající uživatelé potřebovat toobe informováni o nové adrese URL hello.

  3. Přejděte toohello umístění instalace portálu User Portal (např. C:\inetpub\wwwroot\MultiFactorAuth) a upravte soubor web.config hello. Zkopírujte hodnoty hello v hello appSettings a applicationSettings z původního souboru web.config, který byl zálohován před upgradem hello do nového souboru web.config hello. Pokud hello nový výchozí název virtuálního adresáře byl zachovány při instalaci hello sady SDK webové služby, změňte adresu URL hello hello applicationSettings části toopoint toohello správné místo. Pokud byly v hello předchozím soubor web.config změněny ostatní výchozí hodnoty, platí tyto stejné změny toohello nový soubor web.config.

  4. tooinstall hello webové služby mobilní aplikace na webovém serveru hello, otevřete příkazový řádek jako správce a spusťte soubor MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi hello.

    Hello výchozí název virtuálního adresáře je nyní **MultiFactorAuthMobileAppWebService** místo **PhoneFactorPhoneAppWebService**. Pokud chcete toouse hello předchozí název, musíte změnit název hello hello virtuálního adresáře během instalace. Může být vhodné toochoose kratší název toomake snadno tootype koncovým uživatelům v na mobilním zařízení. Jinak Pokud povolíte hello instalace toouse hello nového výchozího názvu, doporučujeme klikněte na ikonu mobilní aplikace hello hello aplikace Multi-Factor Authentication Server a aktualizace hello adresa URL webové služby mobilní aplikace.

  5. Přejděte toohello umístění instalace webové služby mobilní aplikace (např. C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) a upravte soubor web.config hello. Zkopírujte hodnoty hello v hello appSettings a applicationSettings z původního souboru web.config, který byl zálohován před upgradem hello do nového souboru web.config hello. Pokud hello nový výchozí název virtuálního adresáře byl zachovány při instalaci hello sady SDK webové služby, změňte adresu URL hello hello applicationSettings části toopoint toohello správné místo. Pokud byly v hello předchozím soubor web.config změněny ostatní výchozí hodnoty, platí tyto stejné změny toohello nový soubor web.config.

## <a name="next-steps"></a>Další kroky

- [Instalace portálu uživatelů hello](multi-factor-authentication-get-started-portal.md) pro hello Azure Multi-Factor Authentication Server.

- [Konfigurace ověřování systému Windows](multi-factor-authentication-get-started-server-windows.md) pro vaše aplikace 
