---
title: "aaaHow toogenerate a přenos klíčů chráněných pomocí HSM pro Azure Key Vault | Microsoft Docs"
description: "Použijte tento článek toohelp plánování, generovat a potom přeneste vlastní toouse klíčů chráněných pomocí HSM s Azure Key Vault. Taky označovaný jako BYOK nebo přineste si vlastní klíč."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 51abafa1-812b-460f-a129-d714fdc391da
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: ambapat
ms.openlocfilehash: 3bb234bd1c4b81770542ccf7110e256385ca3309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogenerate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Jak toogenerate a přenos klíčů chráněných pomocí HSM pro Azure Key Vault
## <a name="introduction"></a>Úvod
Pro lepší kontrolu Pokud používáte Azure Key Vault, můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM hello. Tento scénář je často označují tooas *přineste si vlastní klíč*, nebo BYOK. Hello moduly hardwarového zabezpečení jsou FIPS 140-2 Level 2 ověřit. Azure Key Vault používá klíče Thales nShield řadu tooprotect moduly hardwarového zabezpečení.

Použijte hello informace v této toohelp téma plánování, generovat a potom přeneste vlastní toouse klíčů chráněných pomocí HSM s Azure Key Vault.

Tato funkce není dostupná pro Azure China.

> [!NOTE]
> Další informace o Azure Key Vault najdete v tématu [co je Azure Key Vault?](key-vault-whatis.md)  
>
> Úvodní kurz, který zahrnuje vytvoření trezoru klíčů pro klíčů chráněných pomocí HSM, najdete v části [Začínáme s Azure Key Vault](key-vault-get-started.md).
>
>

Další informace o generování a přenos klíč chráněný HSM pomocí přes hello Internetu:

* Vygenerování klíče hello z offline pracovní stanici, což snižuje prostor pro útoky hello.
* Hello je klíč zašifrovaný pomocí klíč výměny klíčů (KEK), který zůstává zašifrovaný, dokud nebude přenášená toohello Azure Key Vault moduly hardwarového zabezpečení. Pouze hello zašifrovaná verze vašeho klíče ponechá původní pracovní stanici hello.
* Sada nástrojů Hello nastaví vlastnosti pro klíč klienta, která se sváže vaše klíče toohello Azure Key Vault architektury security world. Proto po hello Azure Key Vault moduly hardwarového zabezpečení přijmou a dešifrují váš klíč, pouze tyto moduly hardwarového zabezpečení můžete použít. Klíč se nedá exportovat. Tuto vazbu vynucují moduly HSM Thales hello.
* Hello klíč výměny klíčů (KEK), je použít tooencrypt váš klíč se generuje uvnitř hello Azure Key Vault moduly hardwarového zabezpečení a není exportovatelný. Moduly hardwarového zabezpečení Hello vynutit, aby může existovat žádná čitelná verze hello KEK mimo hello moduly hardwarového zabezpečení. Kromě toho hello nástrojů obsahuje záruku od společnosti Thales, že hello KEK nedá exportovat a byl vygenerovaný v originálním modulu HSM vyrobeným společností Thales.
* Hello sada nástrojů obsahuje záruku od společnosti Thales, že hello Azure Key Vault architektury security world také vygenerovaná na originálním modulu HSM vyrobeném společností Thales. Toto ověření prokáže, že Microsoft používá originální hardware tooyou.
* Společnost Microsoft používá jiné klíče Kek a jiné architektury Security World v každé geografické oblasti. Toto oddělení zajišťuje, že váš klíč lze použít pouze v datových centrech v hello oblast, ve kterém jste ho zašifrovali. Například klíč od Evropského zákazníka nelze použít v datových centrech v Severní Americe nebo Asii.

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a>Další informace o modulech HSM Thales a služby Microsoft
Thales e-Security je přední globální poskytovatel šifrování dat a kybernetického zabezpečení řešení toohello finančních služeb, špičkové technologie, výroby, government a technologie. S 40-let chrání firemní i vládní informace řešení společnosti Thales využívají je čtyři hello pěti největších energetických a letecký společností. Svá řešení jsou také používány 22 členských zemí NATO a zabezpečení více než 80 % po celém světě platebních transakcí.

Microsoft spolupracoval se stavem hello tooenhance Thales obrázky pro moduly hardwarového zabezpečení. Tato vylepšení umožňují tooget hello typické výhod hostované služby bez vzdát kontrolu nad klíče. Konkrétně tato vylepšení umožňují Microsoftu spravovat hello moduly hardwarového zabezpečení, takže není nutné. Jako cloudové služby, Azure Key Vault škálování na krátké oznámení toomeet špičky využití vaší organizace. Na hello stejný čas, váš klíč je uvnitř modulů HSM Microsoftu chráněný: ponecháte si kontrolu nad hello životního cyklu u klíče, protože generování klíče hello a přenést na tooMicrosoft moduly hardwarového zabezpečení.

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Implementace funkce přineste si vlastní klíč (BYOK) pro Azure Key Vault
Hello použijte následující informace a postupy, pokud chcete vygenerovat si vlastní klíč chráněný HSM a pak ho přenést tooAzure Key Vault – hello přineste si vlastní klíč (BYOK) scénář.

## <a name="prerequisites-for-byok"></a>Předpoklady pro funkci BYOK
Viz následující tabulka obsahuje seznam požadavků pro hello přineste si vlastní klíč (BYOK) pro Azure Key Vault.

| Požadavek | Další informace |
| --- | --- |
| TooAzure předplatného |toocreate Azure Key Vault, budete potřebovat předplatné Azure: [zaregistrovat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) |
| Hello Azure Key Vault Premium služby vrstvě toosupport klíčů chráněných pomocí HSM |Další informace o úrovních služeb hello a možnosti pro Azure Key Vault najdete v tématu hello [Azure Key Vault ceny](https://azure.microsoft.com/pricing/details/key-vault/) webu. |
| Modulu HSM společnosti Thales, čipové karty a podpůrný software |Musíte mít přístup k tooa modulu hardwarového zabezpečení Thales a základní provozní znalosti o modulech HSM Thales. V tématu [modulu hardwarového zabezpečení Thales](https://www.thales-esecurity.com/msrms/buy) hello seznam kompatibilních modelů nebo toopurchase modul hardwarového zabezpečení, pokud nemáte jeden. |
| Hello následující hardware a software:<ol><li>Offline x64 pracovní stanice s minimální operační systém Windows Windows 7 a Thales software nshield od, který je minimálně verze 11.50.<br/><br/>Pokud tato pracovní stanice používá Windows 7, musíte [instalace rozhraní Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Pracovní stanice, která je připojená toohello Internet a má minimální operační systém Windows Windows 7 a [prostředí Azure PowerShell](/powershell/azure/overview) **minimální verzi 1.1.0** nainstalována.</li><li>USB Flash disk nebo jiné přenosné úložné zařízení, která obsahuje aspoň 16 MB volného místa.</li></ol> |Z bezpečnostních důvodů doporučujeme, abyste že tento hello první pracovní stanice není připojený tooa sítí. Toto doporučení se však nevynucuje prostřednictvím kódu programu.<br/><br/>Všimněte si, že v hello pokynech, této pracovní stanici odkazované tooas hello odpojená pracovní stanice.</p></blockquote><br/>Kromě toho pokud váš klíč tenanta je pro produkční síť, doporučujeme používat druhou, samostatnou pracovní stanici toodownload hello nástrojů a nahrání hello klíč tenanta. Ale pro účely testování můžete použít jako hello první hello pracovní stanici.<br/><br/>Upozorňujeme, že v hello pokynech se druhá pracovní stanice se stanice připojené k Internetu odkazované tooas hello.</p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-tooazure-key-vault-hsm"></a>Generování a přenos vašeho klíče tooAzure klíč trezoru modulu hardwarového zabezpečení
Budete používat hello následujících pět toogenerate kroky a přenos vašeho klíče tooan Azure Key Vault modulu hardwarového zabezpečení:

* [Krok 1: Příprava pracovní stanice připojené k Internetu](#step-1-prepare-your-internet-connected-workstation)
* [Krok 2: Příprava odpojené pracovní stanice](#step-2-prepare-your-disconnected-workstation)
* [Krok 3: Vygenerování klíče](#step-3-generate-your-key)
* [Krok 4: Příprava klíče pro přenos](#step-4-prepare-your-key-for-transfer)
* [Krok 5: Přenos vašeho klíče tooAzure Key Vault](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Krok 1: Příprava pracovní stanice připojené k Internetu
Pro tento první krok text hello následující postupy na pracovní stanici, která je připojená toohello Internetu.

### <a name="step-11-install-azure-powershell"></a>Krok 1.1: Nainstalování prostředí Azure PowerShell
Z hello stanice připojené k Internetu stáhněte a nainstalujte modul Azure PowerShell text hello, který obsahuje toomanage hello rutiny Azure Key Vault. To vyžaduje minimální verzi 0.8.13.

Pokyny k instalaci naleznete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

### <a name="step-12-get-your-azure-subscription-id"></a>Krok 1.2: Získání ID předplatného Azure
Spusťte relaci prostředí Azure PowerShell a přihlaste se pomocí hello následující příkaz tooyour účet Azure:

        Add-AzureAccount
V okně hello automaticky otevírané okno prohlížeče zadejte účet Azure uživatelské jméno a heslo. Poté použijte hello [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) příkaz:

        Get-AzureSubscription
Z výstupu hello vyhledejte ID hello hello předplatného, které chcete použít pro Azure Key Vault. Toto ID předplatného budete potřebovat později.

Nezavírejte okno Azure PowerShell hello.

### <a name="step-13-download-hello-byok-toolset-for-azure-key-vault"></a>Krok 1.3: Stažení sady nástrojů funkce BYOK hello pro Azure Key Vault
Přejděte toohello Microsoft Download Center a [stažení nástrojů Azure Key Vault BYOK hello](http://www.microsoft.com/download/details.aspx?id=45345) pro zeměpisné oblasti nebo instanci Azure. Použijte hello informace tooidentify hello balíčku název toodownload a jeho odpovídající SHA-256 balíčku hash:

- - -
**Spojené státy americké:**

KeyVault-BYOK-nástroje spojené States.zip

760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B

- - -
**Evropa:**

KeyVault BYOK nástroje Europe.zip

7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D

- - -
**Asii:**

KeyVault BYOK nástroje AsiaPacific.zip

813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8

- - -
**Latinská Amerika:**

KeyVault BYOK nástroje LatinAmerica.zip

3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0

- - -
**Japonsko:**

KeyVault BYOK nástroje Japan.zip

453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90

- - -
**Korea:**

KeyVault BYOK nástroje Korea.zip

C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A

- - -
**Austrálie:**

KeyVault BYOK nástroje Australia.zip

4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B

- - -
[**Azure Government:**](https://azure.microsoft.com/features/gov/)

KeyVault BYOK nástroje USGovCloud.zip

3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64

- - -
**Vláda USA DOD:**

KeyVault BYOK nástroje USGovernmentDoD.zip

A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D

- - -
**Kanada:**

KeyVault BYOK nástroje Canada.zip

30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7

- - -
**Německo:**

KeyVault BYOK nástroje Germany.zip

5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261

- - -
**Indie:**

KeyVault BYOK nástroje India.zip

136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7

- - -
**Spojené království:**

KeyVault BYOK nástroje UnitedKingdom.zip

ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268

- - -

toovalidate hello integritu vašeho stažené sady nástrojů funkce BYOK, z relace prostředí Azure PowerShell, použijte hello [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) rutiny.

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Sada nástrojů Hello zahrnuje hello následující:

* Balíček klíče výměny klíčů (KEK), který má název začíná **BYOK-KEK - pkg-.**
* Balíček architektury Security World, jehož název začíná **BYOK-SecurityWorld - pkg-.**
* Skript v jazyce python s názvem **verifykeypackage.py.**
* Spustitelný soubor příkazového řádku, s názvem **KeyTransferRemote.exe** a přidružené knihovny DLL.
* Visual C++ Redistributable balíčku, s názvem **vcredist_x64.exe.**

Zkopírujte hello balíček tooa USB Flash disku nebo jiného přenosného úložiště.

## <a name="step-2-prepare-your-disconnected-workstation"></a>Krok 2: Příprava odpojené pracovní stanice
Pro tento druhý krok text hello následující postupy na hello pracovní stanici, která není připojená tooa sítí (hello Internet nebo interní síti).

### <a name="step-21-prepare-hello-disconnected-workstation-with-thales-hsm"></a>Krok 2.1: Příprava hello odpojená pracovní stanice s modulu HSM společnosti Thales
Nainstalujte na počítač s Windows podpůrný software nCipher (Thales) hello a pak připojte počítač toothat modulu HSM společnosti Thales.

Ujistěte se, že hello nástroje Thales jsou ve své cestě (**%nfast_home%\bin**). Můžete například zadáte následující hello:

        set PATH=%PATH%;"%nfast_home%\bin"

Další informace najdete v tématu hello uživatelské příručce dodané s hello modulu HSM společnosti Thales.

### <a name="step-22-install-hello-byok-toolset-on-hello-disconnected-workstation"></a>Krok 2.2: Instalace hello sady nástrojů funkce BYOK na pracovní stanici hello odpojení
Zkopírujte balíček sady nástrojů funkce BYOK hello z hello USB Flash disku nebo jiného přenosného úložiště a pak hello následující:

1. Extrahujte soubory hello z hello stáhli balíčku do libovolné složky.
2. Z této složky spusťte program vcredist_x64.exe.
3. Postupujte podle pokynů hello toohello nainstalovat hello komponenty modulu runtime Visual C++ pro Visual Studio 2013.

## <a name="step-3-generate-your-key"></a>Krok 3: Vygenerování klíče
Pro tento třetí krok text hello následující postupy na pracovní stanici hello odpojen. toocomplete tento krok vašeho HSM musí být v režimu inicializace. 


### <a name="step-31-change-hello-hsm-mode-tooi"></a>Krok 3.1: Změnit too'I režimu hello HSM.
Pokud používáte Thales nShield okraji a toochange hello režim: 1. Použijte hello režimu tlačítko toohighlight hello požadovaný režim. 2. Během pár sekund stiskněte a podržte tlačítko Vymazat hello z několika sekund. Pokud se změní hello režimu, hello nový režim DIODU přestane blikat a zůstane po. Hello Indikátor stavu může nepravidelně flash na několik sekund a pak bliká pravidelně při hello zařízení je připraveno. V opačném lit hello zařízení zůstanou v aktuálním režimu hello s Indikátor příslušné režimu hello.

### <a name="step-32-create-a-security-world"></a>Krok 3.2: Vytvoření architektury security world
Otevřete příkazový řádek a spusťte program společnosti Thales nové architektury Security world hello.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Tento program vytvoří **architektury Security World** soubor v adresáři % NFAST_KMDATA%\local\world, který odpovídá toohello C:\ProgramData\nCipher\Key správu aplikací\Místní složky. Můžete použít jiné hodnoty pro hello kvorum, ale v našem příkladu jste výzvami tooenter tři prázdné karty a kódy PIN pro každé z nich. Jakékoli dvě karty potom poskytnout úplný přístup toohello security world. Tyto karty tvoří hello **Administrator Card Set** pro hello, world nové zabezpečení.

Potom hello následující:

* Zálohujte soubor hello world. Zabezpečení a ochraně soubor hello world, karty Správce hello a kódu PIN a ujistěte se, že jeden člověk měl přístup toomore než jedné karty.

### <a name="step-33-change-hello-hsm-mode-tooo"></a>Krok 3.3: Změnit too'O režimu hello HSM.
Pokud používáte Thales nShield okraji a toochange hello režim: 1. Použijte hello režimu tlačítko toohighlight hello požadovaný režim. 2. Během pár sekund stiskněte a podržte tlačítko Vymazat hello z několika sekund. Pokud se změní hello režimu, hello nový režim DIODU přestane blikat a zůstane po. Hello Indikátor stavu může nepravidelně flash na několik sekund a pak bliká pravidelně při hello zařízení je připraveno. V opačném lit hello zařízení zůstanou v aktuálním režimu hello s Indikátor příslušné režimu hello.


### <a name="step-34-validate-hello-downloaded-package"></a>Krok 3.4: Ověření hello Stáhnout balíček
Tento krok je volitelný, ale doporučujeme, aby mohli ověřit hello následující:

* Hello klíč výměny klíče, který je součástí sady nástrojů hello byl vygenerovaný na originálním modulu HSM společnosti Thales.
* Hodnota hash Hello hello World zabezpečení, která je součástí sady nástrojů hello byla vygenerovaná na originálním modulu HSM společnosti Thales.
* Hello klíč pro výměnu klíčů je neexportovatelného.

> [!NOTE]
> toovalidate hello stáhli balíček, hello modulu hardwarového zabezpečení musí být připojený, zapnutý a musí mít architektury security world v něm (například hello jeden, který jste právě vytvořili).
>
>

toovalidate hello Stáhnout balíček:

1. Spusťte skript verifykeypackage.py hello zadáním jedné z následujících hello, v závislosti na zeměpisné oblasti nebo instanci Azure:

   * Pro Severní Ameriku:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * Pro Evropu:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * Pro Asii:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * Pro Latinská Amerika:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * Pro Japonsko:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * Pro Koreu:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * Pro Austrálii:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * Pro [Azure Government](https://azure.microsoft.com/features/gov/), který používá hello US government instanci Azure:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * Pro US Government DOD:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * Pro Kanadu:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * Pro Německo:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * Pro Indii:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > Hello software společnosti Thales obsahuje python v %NFAST_HOME%\python\bin
     >
     >
2. Zkontrolujte, jestli hello následující příkaz, který znamená úspěšné ověření: **výsledek: Úspěch**

Tento skript ověřuje řetězec podepisujících hello až toohello kořenovému klíči Thales. ve skriptu hello vložené Hello hash tohoto kořenového klíče a jeho hodnota by měla být **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Tuto hodnotu můžete potvrdit samostatně návštěvou hello [webu společnosti Thales](http://www.thalesesec.com/).

Nyní jste připravené toocreate nový klíč.

### <a name="step-35-create-a-new-key"></a>3.5 krok: Vytvořte nový klíč
Generování klíče pomocí hello Thales **generatekey** program.

Spusťte následující příkaz toogenerate hello klíč hello:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Když spustíte tento příkaz, použijte tyto pokyny:

* Hello parametr *chránit* musí být nastavena hodnota toohello **modulu**, jak je vidět. Tím se vytvoří klíč chráněný modulem. Hello sady nástrojů funkce BYOK nepodporuje klíče chráněné OCS.
* Nahraďte hodnotu hello *contosokey* pro hello **ident** a **plainname** s libovolnou hodnotou řetězce. správní režie toominimize a snížit riziko hello chyb, doporučujeme použít stejnou hodnotu pro oba hello. Hello **ident** hodnota musí obsahovat jenom čísla, pomlčky a malá písmena.
* Hello parametr pubexp je prázdný (výchozí nastavení) v tomto příkladu, ale můžete zadat konkrétní hodnoty. Další informace najdete v tématu hello dokumentace společnosti Thales.

Tento příkaz vytvoří soubor Tokenizovaného klíče ve složce %NFAST_KMDATA%\local s názvem začínajícím textem **key_simple_**, za nímž následují hello **ident** zadaný v příkazu hello. Příklad: **key_simple_contosokey**. Tento soubor obsahuje šifrovaný klíč.

Zálohujte tento soubor Tokenizovaného klíče do bezpečného umístění.

> [!IMPORTANT]
> Pokud později přenést vaše klíče tooAzure Key Vault, Microsoft nelze exportovat tento klíč back tooyou tak bude velmi důležité, abyste zálohovali váš klíč a architekturu security world bezpečně. Pokyny a osvědčené postupy pro zálohování klíčů získáte od společnosti Thales.
>
>

Můžete je nyní připraven tootransfer vaše klíče tooAzure Key Vault.

## <a name="step-4-prepare-your-key-for-transfer"></a>Krok 4: Příprava klíče pro přenos
Pro tento čtvrtý krok text hello následující postupy na pracovní stanici hello odpojen.

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Krok 4.1: Vytvoření kopie klíče se sníženými oprávněními

Otevřete nový příkazový řádek a změňte hello aktuální toohello umístění adresáře kde jste rozbalené soubor zip BYOK hello. tooreduce hello oprávnění na váš klíč z příkazového řádku, spusťte jeden z následujících hello, v závislosti na zeměpisné oblasti nebo instanci Azure:

* Pro Severní Ameriku:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* Pro Evropu:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* Pro Asii:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* Pro Latinská Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* Pro Japonsko:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* Pro Koreu:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* Pro Austrálii:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* Pro [Azure Government](https://azure.microsoft.com/features/gov/), který používá hello US government instanci Azure:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* Pro US Government DOD:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* Pro Kanadu:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* Pro Německo:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* Pro Indii:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

Když spustíte tento příkaz, nahraďte *contosokey* s hello stejnou hodnotu, kterou jste zadali v **3.5 krok: Vytvořte nový klíč** z hello [vygenerování klíče](#step-3-generate-your-key) krok.

Zobrazí se výzva tooplug v karty Správce security world.

Pokud příkaz hello dokončení zobrazí **výsledek: Úspěch** a hello kopie vašeho klíče se sníženými oprávněními jsou v hello soubor s názvem key_xferacId_<contosokey>.

Může zkontroluje hello seznamy ACL pomocí následujících příkazů pomocí nástrojů Thales hello:

* aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  Při spuštění těchto příkazů, nahraďte contosokey hello stejnou hodnotu, kterou jste zadali v **3.5 krok: Vytvořte nový klíč** z hello [vygenerování klíče](#step-3-generate-your-key) krok.

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Krok 4.2: Zašifrování klíče pomocí klíč pro výměnu klíčů společnosti Microsoft
Spusťte jeden z následujících příkazů, v závislosti na zeměpisné oblasti nebo instanci Azure hello:

* Pro Severní Ameriku:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro Evropu:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro Asii:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro Latinská Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro Japonsko:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro Koreu:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro Austrálii:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro [Azure Government](https://azure.microsoft.com/features/gov/), který používá hello US government instanci Azure:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro US Government DOD:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro Kanadu:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro Německo:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Pro Indii:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

Když spustíte tento příkaz, použijte tyto pokyny:

* Nahraďte *contosokey* s hello identifikátor, který jste použili klíč hello toogenerate v **3.5 krok: Vytvořte nový klíč** z hello [vygenerování klíče](#step-3-generate-your-key) krok.
* Nahraďte *SubscriptionID* s ID hello hello předplatné Azure, která obsahuje váš trezor klíčů. Načíst tuto hodnotu dříve, v **krok 1.2: získání ID předplatného Azure** z hello [Příprava pracovní stanice připojené k Internetu](#step-1-prepare-your-internet-connected-workstation) krok.
* Nahraďte *ContosoFirstHSMKey* s popiskem, který se používá pro názvu výstupního souboru.

Po dokončení této akce úspěšně, zobrazí **výsledek: Úspěch** a nový soubor existuje v aktuální složce hello, který má následující název hello: KeyTransferPackage -*ContosoFirstHSMkey*.byok

### <a name="step-43-copy-your-key-transfer-package-toohello-internet-connected-workstation"></a>Krok 4.3: Kopírování vaše klíče přenosu balíčku toohello stanice připojené k Internetu
Použijte USB Flash disk nebo jiné přenosné úložné toocopy hello výstupní soubor z hello předchozího kroku (KeyTransferPackage-ContosoFirstHSMkey.byok) tooyour stanice připojené k Internetu.

## <a name="step-5-transfer-your-key-tooazure-key-vault"></a>Krok 5: Přenos vašeho klíče tooAzure Key Vault
Tento poslední krok, hello stanice připojené k Internetu, použít hello [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) rutiny tooupload hello přenos klíče balíček, který jste zkopírovali ze hello odpojená pracovní stanice toohello Azure Key Vault modulu hardwarového zabezpečení:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Pokud je hello nahrání úspěšné, zobrazí vlastnosti zobrazené hello hello klíče, který jste právě přidali.

## <a name="next-steps"></a>Další kroky
Teď můžete použít tento klíč chráněný HSM v trezoru klíčů. Další informace najdete v tématu hello **Pokud budete chtít toouse modul hardwarového zabezpečení (HSM)** část v hello [Začínáme s Azure Key Vault](key-vault-get-started.md) kurzu.
