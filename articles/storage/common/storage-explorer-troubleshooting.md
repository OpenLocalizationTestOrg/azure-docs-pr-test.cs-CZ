---
title: "Průvodci odstraňováním potíží Storage Explorer aaaAzure | Microsoft Docs"
description: "Přehled ladění funkce hello dvě sady Azure"
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: delhan
ms.openlocfilehash: 5152f70418707d65c0a4bce9a916336829956219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Storage Explorer Průvodci odstraňováním potíží

## <a name="introduction"></a>Úvod

Microsoft Azure Storage Explorer (Preview) je samostatná aplikace, která vám umožní tooeasily práci s daty Azure Storage ve Windows, systému macOS a Linux. aplikace Hello se může připojit toStorage účty hostované na Azure, suverénní Cloudy a Azure zásobníku.

Tato příručka obsahuje souhrn řešení pro běžné problémy, které jsou vidět ve Storage Exploreru.

## <a name="sign-in-issues"></a>Přihlaste se problémy

Než budete pokračovat, zkuste restartovat aplikaci a zjistit, zda lze dlouhodobý hello problémy.

### <a name="error-self-signed-certificate-in-certificate-chain"></a>Chyba: Certifikát podepsaný svým držitelem v řetězu certifikátů

Tady je několik důvodů, proč této chybě může dojít, a nejběžnější dva důvody hello jsou následující:

1. aplikace Hello je připojená přes "transparentní proxy", což znamená, brání komunikaci přes protokol HTTPS, dešifrování ho a pak šifrování pomocí certifikát podepsaný svým držitelem serveru (například serveru vaší společnosti).

2. Běží aplikace, jako je antivirový software, který je podepsaný certifikát SSL vložení do hello zpráv protokolu HTTPS, které jste dostali.

Když Storage Explorer dojde jedním z problémů hello, můžete již nebude vědět, jestli je úmyslně hello přijata zpráva protokolu HTTPS. Pokud máte kopii hello certifikát podepsaný svým držitelem, můžete je nechat Storage Explorer důvěřujete mu. Pokud si nejste jisti kdo je vložení hello certifikát, postupujte podle těchto kroků toofind ho:

1. Instalace otevřete SSL

    - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (jakékoli světla verze hello by mělo být dostatečné)

    - Mac a Linux: by měly být zahrnuty s operačním systémem

2. Spustit otevřete SSL

    - Windows: Otevřete hello instalační adresář, klikněte na **/bin/**a potom dvakrát klikněte na **openssl.exe**.
    - Mac a Linux: Spusťte **openssl** z terminálu.

3. Spuštění s_client - showcerts-připojení microsoft.com:443

4. Hledejte certifikáty podepsané svým držitelem. Pokud si nejste jistí, které jsou podepsané svým držitelem, vyhledejte kdekoli hello subjektu ("s:") a vystavitele ("i") jsou hello stejné.

5. Po nalezení všechny certifikáty podepsané svým držitelem pro každé z nich, zkopírujte a vložte všechno z a to včetně **---BEGIN CERTIFICATE---** příliš**---END CERTIFICATE---** tooa nový soubor .cer.

6. Otevřete Storage Explorer, klikněte na **upravit** > **certifikáty SSL** > **importu certifikátů**a pak použijte hello souboru výběr toofind, vyberte možnost, a otevřete hello .cer soubory, které jste vytvořili.

Pokud nenajdete žádné certifikáty podepsané svým držitelem pomocí hello výše uvedené kroky, kontaktujte nás pomocí nástroje hello zpětnou vazbu o další pomoc.

### <a name="unable-tooretrieve-subscriptions"></a>Odběry nelze tooretrieve

Pokud jste nelze tooretrieve vašich předplatných po můžete úspěšně přihlásit, postupujte tento problém tootroubleshoot tyto kroky:

- Ověřte, zda má váš účet přístup toohello odběry po přihlášení k portálu Azure hello.

- Ujistěte se, že jste se přihlásili pomocí správné prostředí hello (Azure, Azure China, Azure v Německu, Azure US Government nebo vlastní prostředí nebo Azure zásobníku).

- Pokud se nacházíte za proxy, ujistěte se, které jste nakonfigurovali proxy hello Storage Explorer správně.

- Zkuste odebrat a nové přidání účtu hello.

- Zkuste odstranit hello následující soubory z kořenového adresáře (tedy C:\Users\ContosoUser) a potom znovu přidat účet hello:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Sledování konzoly vývojářských nástrojů hello (stisknutím klávesy F12) Pokud se přihlašujete všechny chybové zprávy:

![Nástroje pro vývojáře](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-toosee-hello-authentication-page"></a>Stránku nelze toosee hello ověřování

Pokud jste nelze toosee hello ověřovací stránku, postupujte tento problém tootroubleshoot tyto kroky:

- V závislosti na rychlosti hello připojení může chvíli trvat, než tooload přihlašovací stránku hello, počkejte před jeho zavřením dialogové okno ověřování hello alespoň jednu minutu.

- Pokud se nacházíte za proxy, ujistěte se, které jste nakonfigurovali proxy hello Storage Explorer správně.

- Zobrazení hello vývojářské konzole stisknutím klávesy F12 hello. Sledovat hello odpovědí z vývojářské konzole hello a zjistit, jestli můžete najít všechny potvrzením pro důvod, proč ověřování nepracuje.

### <a name="cannot-remove-account"></a>Nelze odebrat účet

Pokud jste nelze tooremove účet nebo pokud hello znovu ověřit propojení nemá žádný, postupujte podle těchto kroků tootroubleshoot tento problém:

- Zkuste odstranit hello následující soubory z kořenového adresáře a pak nové přidání účtu hello:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Pokud chcete, aby byl tooremove SAS připojen prostředky úložiště, odstraňte hello následující soubory:

    - %AppData%/StorageExplorer složky pro Windows

    - /Users/ < vaše_jméno >/knihovny/Express podporu nebo StorageExplorer pro Mac

    - ~/.config/StorageExplorer pro Linux

> [!NOTE]
>  Tooreenter bude mít všechny svoje přihlašovací údaje, pokud odstraníte tyto soubory.

## <a name="proxy-issues"></a>Problémy s proxy

Nejprve zkontrolujte, zda jsou všechny správné této hello následující informace, které jste zadali:

- Hello adresa URL proxy serveru a port číslo

- Uživatelské jméno a heslo, pokud to vyžaduje proxy server hello

### <a name="common-solutions"></a>Běžná řešení

Pokud pořád dochází k problémům, postupujte podle těchto kroků tootroubleshoot je:

- Pokud se můžete připojit toohello Internetu bez použití proxy, ověřte, že Storage Explorer funguje bez povolené nastavení proxy serveru. Pokud je to hello případ, může být problém se vaše nastavení proxy serveru. Spolupracovat s proxy správce tooidentify hello problémů.

- Ověřte, zda jiné aplikace pomocí serveru proxy hello fungují podle očekávání.

- Ověřte, zda se můžete připojit toohello portálu Microsoft Azure pomocí webového prohlížeče

- Ověřte, zda se zobrazila odpovědí z koncových bodů služby. Zadejte jednu z váš koncový bod adresy URL do prohlížeče. Pokud se můžete připojit, měli byste obdržet InvalidQueryParameterValue nebo podobné odpovědi ve formátu XML.

- Pokud někdo jiný používá také Storage Explorer proxy serveru, ověřte, zda se můžete připojit. Pokud se mohou připojovat, bude pravděpodobně toocontact správce proxy serveru.

### <a name="tools-for-diagnosing-issues"></a>Nástroje pro diagnostiku problémů

Pokud máte síťové nástroje, například aplikaci Fiddler pro Windows, může být schopný toodiagnose hello problémy následujícím způsobem:

- Pokud máte toowork prostřednictvím proxy, můžete mít tooconfigure vaší sítě tooconnect nástroj přes proxy server hello.

- Zkontrolujte číslo portu hello používané nástrojem pro vaší sítě.

- Zadejte adresu URL místního hostitele hello a hello sítě číslo portu je nástroj jako nastavení proxy serveru v Storage Explorer. Pokud tento isdone správně, vaší sítě nástroj spustí protokolování provedené koncové body toomanagement a služby Storage Explorer síťové požadavky. Zadejte například https://cawablobgrs.blob.core.windows.net/ pro koncový bod služby objektů blob v prohlížeči a zobrazí se odpověď se podobá následující text hello, který naznačuje hello prostředek existuje, i když nelze k němu přístup.

![Ukázka kódu](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Obraťte se na správce serveru proxy

Pokud vaše nastavení proxy serveru jsou správné, bude pravděpodobně toocontact správce serveru proxy a

- Ujistěte se, že proxy neblokuje přenosy tooAzure správy nebo prostředků koncových bodů.

- Zkontrolujte protokol ověřování hello používá proxy server. Storage Explorer v současné době nepodporuje proxy protokolu NTLM.

## <a name="unable-tooretrieve-children-error-message"></a>Chybová zpráva "Nelze tooRetrieve podřízené objekty"

Pokud jste připojené tooAzure prostřednictvím proxy serveru, ověřte správnost nastavení proxy serveru. Pokud byla udělena prostředků tooa přístup od vlastníka hello hello předplatné nebo účet, ověřte, zda přečetli nebo seznamu oprávnění pro tento prostředek.

### <a name="issues-with-sas-url"></a>Problémy s adresou URL SAS
Pokud se připojujete tooa služby pomocí SAS adresa URL a hlásí tuto chybu:

- Ověřte, že adresa URL hello poskytuje hello potřebná oprávnění tooread nebo seznamu prostředků.

- Ověřte, že hello nevypršela platnost adresy URL.

- Pokud hello SAS adresa URL je založená na zásadách přístupu, ověřte nebyl odvolaný hello zásady přístupu.

## <a name="next-steps"></a>Další kroky

Pokud žádná z hello řešení fungovat pro vás, odešlete svůj problém pomocí nástroje hello zpětnou vazbu s e-mailu a tolik podrobnosti o problému hello zahrnuty jako je možné, tak, aby budeme vás moc kontaktovat o nápravě problému hello.

toodo tento, klikněte na tlačítko **pomoci** nabídce a pak klikněte na tlačítko **odeslat zpětnou vazbu**.

![Váš názor](./media/storage-explorer-troubleshooting/4022503_en_1.png)
