---
title: "problémy při nasazení zařízení StorSimple aaaTroubleshoot | Microsoft Docs"
description: "Popisuje, jak toodiagnose a opravte chyby, ke kterým dochází při prvním nasazení zařízení StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f8352eaa-193c-42d1-8818-0b8e02d8485d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: 9a31a3cfb3be577500b226c2bc8172dd8664bad9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Řešení problémů nasazení zařízení StorSimple
## <a name="overview"></a>Přehled
Tento článek obsahuje užitečné pokyny k odstraňování potíží pro vaše nasazení Microsoft Azure StorSimple. Popisuje běžné problémy, možné příčiny a doporučené kroky toohelp vyřešit problémy, které se můžete setkat při konfiguraci zařízení StorSimple. Tyto informace platí fyzického zařízení tooboth hello StorSimple místní a virtuální zařízení StorSimple hello.

> [!NOTE]
> Související s konfigurací problémy zařízení, které může být vystaven může dojít, když nasazujete hello zařízení pro hello poprvé, nebo k nim můžete dojde později, když je zařízení hello provozní. Tento článek se zaměřuje na řešení potíží s prvním nasazení. tootroubleshoot provozní zařízení, přejděte příliš[potíží provozní zařízení](storsimple-troubleshoot-operational-device.md).
> 
> 

Tento článek také popisuje hello nástroje pro řešení potíží s nasazením zařízení StorSimple a poskytuje podrobný příklad řešení potíží.

## <a name="first-time-deployment-issues"></a>Problémy při prvním nasazení
Pokud narazíte na problém při nasazování zařízení pro hello poprvé, zvažte následující hello:

* Pokud řešíte fyzické zařízení, ujistěte se, že hello hardware má byla nainstalována a nakonfigurována, jak je popsáno v [instalace zařízení StorSimple 8100](storsimple-8100-hardware-installation.md) nebo [instalace zařízení StorSimple 8600](storsimple-8600-hardware-installation.md).
* Zkontrolujte požadavky na nasazení. Ujistěte se, že máte všechny hello informace popsané v hello [kontrolní seznam konfigurace nasazení](storsimple-deployment-walkthrough.md#deployment-configuration-checklist).
* Zkontrolujte toosee hello poznámky k verzi zařízení StorSimple, pokud je popsán hello problém. Poznámky k verzi Hello obsahovat alternativní řešení známých instalace problémy. 

Během nasazování zařízení hello nejběžnější problémy kterou můžou uživatelé setkávají dojde k jejich spuštění Průvodce instalací hello a při jejich registrace zařízení hello pomocí prostředí Windows PowerShell pro StorSimple. (Použijete Windows Powershellu pro StorSimple tooregister a konfigurace zařízení StorSimple. Další informace o registraci zařízení najdete v tématu [krok 3: Konfigurace a registrace zařízení pomocí Windows Powershellu pro StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

Hello následující části vám může pomoct vyřešit problémy, které nastat při konfiguraci zařízení StorSimple hello hello poprvé.

## <a name="first-time-setup-wizard-process"></a>Průvodce instalací prvního
Hello následující kroky shrnují hello Průvodce instalací. Instalační program podrobné informace najdete v tématu [nasazení místního zařízení StorSimple](storsimple-deployment-walkthrough.md).

1. Spustit hello [Invoke-HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) rutiny toostart hello instalace Průvodce, který vás provede hello zbývající kroky. 
2. Nakonfigurujte síť hello: hello Průvodce instalací vám umožní nakonfigurovat nastavení sítě pro hello síťového rozhraní DATA 0 v zařízení StorSimple. Tato nastavení zahrnují hello následující:
   * Virtuální IP (VIP), maska podsítě a bránu – hello [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) rutiny se spouštějí v pozadí hello. Nakonfiguruje hello IP adresu, masku podsítě a bránu pro hello síťového rozhraní DATA 0 v zařízení StorSimple.
   * Primární server DNS – hello [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) rutiny se spouštějí v pozadí hello. Konfiguruje nastavení DNS hello vašeho řešení StorSimple.
   * NTP server – hello [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) rutiny se spouštějí v pozadí hello. Konfiguruje nastavení serveru NTP hello vašeho řešení StorSimple.
   * Volitelné webový proxy server – hello [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) rutiny se spouštějí v pozadí hello. Nastaví a umožňuje hello konfigurace webového proxy serveru pro vaše řešení StorSimple.
3. Nastavit hesla hello: hello dalším krokem je tooset Správce zařízení a hesla Snapshot Manageru zařízení StorSimple. Pokud používáte Update 1, nebude požadované tooset si heslo Snapshot Manager zařízení StorSimple hello.
   
   * heslo správce zařízení Hello je použité toolog na tooyour zařízení. výchozí heslo zařízení Hello je **Heslo1**.
   * Při konfiguraci toouse zařízení StorSimple Snapshot Manager je vyžadováno heslo Snapshot Manager zařízení StorSimple Hello. Je třeba toofirst nastavit hello heslo v Průvodci instalací hello, a pak můžete nastavit a změnit z hello služby StorSimple Manager. Toto heslo ověřuje hello zařízení s StorSimple Snapshot Manager.
     
     > [!IMPORTANT]
     > Hesla jsou shromažďovány před zaregistrováním, ale použijí pouze po úspěšně registroval zařízení hello. Pokud dojde k selhání tooapply heslo, bude heslo hello výzvami toosupply znovu až hello požadovaných se shromažďují hesla (které splňují požadavky na složitost hello).
     > 
     > 
4. Registrace zařízení hello: hello posledním krokem je tooregister hello zařízení službě StorSimple Manager hello běžící v Microsoft Azure. registrace Hello vyžaduje příliš[získat registrační klíč služby hello](storsimple-manage-service.md#get-the-service-registration-key) z hello portál Azure classic a poskytnout ho v Průvodci instalací hello. Po úspěšném zaregistrování hello zařízení, je k dispozici tooyou šifrovacího klíče dat služby. Ujistěte se, že tookeep toto šifrování klíče do bezpečného umístění, protože je vyžadováno tooregister všechny následné zařízení pomocí služby hello.

## <a name="common-errors-during-device-deployment"></a>Běžné chyby během nasazování zařízení
Hello následující tabulky seznam hello běžných chyb, můžete setkat, když jste:

* Nakonfigurujte nastavení sítě hello vyžaduje.
* Nakonfigurujte nastavení hello volitelné webového proxy serveru.
* Nastavení Správce zařízení hello a hesla Snapshot Manageru zařízení StorSimple. 
* Registrace zařízení hello. 

## <a name="errors-during-hello-required-network-settings"></a>Chyby při hello požadované nastavení sítě
| Ne. | Chybová zpráva | Možné příčiny | Doporučená akce |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Tento příkaz lze spustit jen u aktivního řadiče hello. |Konfigurace se provádí na pasivní řadiče hello. |Tento příkaz lze spustíte z aktivního řadiče hello. Další informace najdete v tématu [identifikovat v zařízení aktivní řadič](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| 2 |Invoke-HcsSetupWizard: Zařízení není připraveno. |Nedochází k potížím s připojením k síti hello na DATA 0. |Zkontrolujte připojení k fyzické síti hello na DATA 0. |
| 3 |Invoke-HcsSetupWizard: Je konflikt IP adres s jiným systémem v síti hello (výjimky z HRESULT: 0x80070263). |Hello IP zadaná pro DATA 0 byl již používán jiným systémem. |Zadejte novou IP, které nejsou používána. |
| 4 |Invoke-HcsSetupWizard: Prostředku clusteru se nezdařilo. (Výjimka z HRESULT: 0x800713AE). |Duplicitní VIP. Hello zadané IP je již používán. |Zadejte novou IP, které nejsou používána. |
| 5 |Invoke-HcsSetupWizard: Neplatný IPv4 adresa. |Hello IP adresy se poskytuje v nesprávném formátu. |Zkontrolujte formát hello a znovu zadejte IP adresu. Další informace najdete v tématu [adresování protokolu Ipv4][1]. |
| 6 |Invoke-HcsSetupWizard: Neplatný IPv6 adresa. |Hello IP adresy se poskytuje v nesprávném formátu. |Zkontrolujte formát hello a znovu zadejte IP adresu. Další informace najdete v tématu [adresách Ipv6][2]. |
| 7 |Invoke-HcsSetupWizard: Další koncové body nejsou k dispozici z hello mapovač koncových bodů. (Výjimka z HRESULT: 0x800706D9) |fungování clusteru Hello nefunguje. |[Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) pro další kroky. |

## <a name="errors-during-hello-optional-web-proxy-settings"></a>Chyby při nastavení hello volitelné webového proxy serveru
| Ne. | Chybová zpráva | Možné příčiny | Doporučená akce |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Neplatný parametr (výjimky z HRESULT: 0x80070057) |Jeden z parametrů hello zadaných pro hello nastavení proxy serveru není platný. |Hello identifikátor URI není k dispozici ve správném formátu hello. Hello použijte následující formát: http://*<IP address or FQDN of hello web proxy server>*:*<TCP port number>* |
| 2 |Invoke-HcsSetupWizard: Server RPC není k dispozici (výjimky z HRESULT: 0x800706ba) |Hello hlavní příčinou je jedna z následujících hello:<ol><li>Hello cluster není nahoru.</li><li>Hello pasivní řadiče nemůže komunikovat s hello aktivního řadiče a hello příkaz spuštěn z pasivní řadiče.</li></ol> |V závislosti na hello příčiny:<ol><li>[Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) toomake, zda je daný cluster hello.</li><li>Spusťte příkaz hello z aktivního řadiče hello. Pokud chcete, aby příkaz hello toorun z hello pasivní řadiče, budete potřebovat tooensure pasivní kontroleru hello může komunikovat se službou active řadiče hello. Budete potřebovat příliš[kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) Pokud toto připojení je přerušené.</li></ol> |
| 3 |Invoke-HcsSetupWizard: Volání vzdáleného volání Procedur se nezdařilo (výjimky z HRESULT: 0x800706be) |Clusteru je mimo provoz. |[Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) toomake, zda je daný cluster hello. |
| 4 |Invoke-HcsSetupWizard: Clusteru prostředek nebyl nalezen (výjimky z HRESULT: 0x8007138f) |prostředek clusteru Hello nebyl nalezen. To může nastat při instalaci hello nebyl zadán správně. |Může být nutné tooreset hello zařízení toohello tovární nastavení. [Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) toocreate prostředku clusteru. |
| 5 |Invoke-HcsSetupWizard: Clusteru zdroj není online (výjimky z HRESULT: 0x8007138c) |Prostředky clusteru nejsou online. |[Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) pro další kroky. |

## <a name="errors-related-toodevice-administrator-and-storsimple-snapshot-manager-passwords"></a>Chyby související s toodevice správce a hesla Snapshot Manageru zařízení StorSimple
Hello výchozí heslo správce zařízení je **Heslo1**. Toto heslo vyprší po prvním přihlášení hello; Proto je nutné toouse hello instalace Průvodce toochange ho. Při registraci zařízení hello hello poprvé, je nutné zadat nové heslo správce zařízení. 

Pokud používáte software Snapshot Manager zařízení StorSimple hello spuštěné na hello systému Windows Server hostitele toomanage hello zařízení, pak musí také poskytnete heslo Snapshot Manager zařízení StorSimple při první registraci. 

Ujistěte se, že hesla splňují hello následující požadavky:

* Heslo správce zařízení musí mít délku 8 až 15 znaků.
* Heslo Snapshot Manager zařízení StorSimple by mělo být tvořeno 14 až 15 znaků.
* Hesla musí obsahovat 3 hello následující 4 typů znaků: malá písmena, velká písmena, číselné a speciální. 
* Heslo nemůže být hello stejné jako hello posledních 24 hesla.

Kromě toho mějte na paměti, že každý rok po vypršení platnosti hesla a lze změnit pouze po úspěšné registraci zařízení hello. Pokud z nějakého důvodu selže hello registrace, hello hesla se nezmění. Další informace o zařízení správce a hesla Snapshot Manageru zařízení StorSimple, přejděte příliš[použití hello toochange služby StorSimple Manager hesla služby StorSimple](storsimple-change-passwords.md).

Jeden nebo více hello následujícím chybám při nastavování správce zařízení hello a hesla Snapshot Manageru zařízení StorSimple se může vyskytnout.

| Ne. | Chybová zpráva | Doporučená akce |
| --- | --- | --- |
| 1 |heslo Hello překračuje maximální délku hello. |Použijte heslo, které splňuje tyto požadavky:<ul><li>Heslo správce zařízení musí být mezi 8 až 15 znaků.</li><li>Heslo Snapshot Manager zařízení StorSimple musí být tvořeno 14 až 15 znaků.</li></ul> |
| 2 |Hello heslo nesplňuje hello požadované délka. |Použijte heslo, které splňuje tyto požadavky:<ul><li>Heslo správce zařízení musí být mezi 8 až 15 znaků.</li><li>Heslo Snapshot Manager zařízení StorSimple musí být tvořeno 14 až 15 znaků.</lu></ul> |
| 3 |Hello heslo musí obsahovat malá písmena. |Hesla musí obsahovat 3 z hello následující 4 typů znaků: malá písmena, velká písmena, číselné a speciální. Ujistěte se, že vaše heslo splňuje tyto požadavky. |
| 4 |Hello heslo musí obsahovat znaky. |Hesla musí obsahovat 3 z hello následující 4 typů znaků: malá písmena, velká písmena, číselné a speciální. Ujistěte se, že vaše heslo splňuje tyto požadavky. |
| 5 |Hello heslo musí obsahovat speciální znaky. |Hesla musí obsahovat 3 z hello následující 4 typů znaků: malá písmena, velká písmena, číselné a speciální. Ujistěte se, že vaše heslo splňuje tyto požadavky. |
| 6 |Hello heslo musí obsahovat 3 z hello následující 4 typů znaků: velká písmena, malá písmena, číselné a speciální. |Heslo neobsahuje hello požadované typy znaků. Ujistěte se, že vaše heslo splňuje tyto požadavky. |
| 7 |Parametr neodpovídá potvrzení. |Ujistěte se, že vaše heslo splňuje všechny požadavky a že jste ji zadali správně. |
| 8 |Vaše heslo se nemůže shodovat výchozí hello. |výchozí heslo Hello je *Heslo1*. Je nutné toochange toto heslo po přihlášení pro hello poprvé. |
| 9 |Hello heslo, které jste zadali, neodpovídá hello heslo zařízení. Zadejte znovu heslo hello. |Zkontrolujte heslo hello a zadejte jej znovu. |

Hesla se shromažďují před hello zařízení je registrované, ale použijí se jenom po úspěšné registraci. pracovní postup obnovení hesla Hello vyžaduje toobe hello zařízení registrované. 

> [!IMPORTANT]
> Obecně platí Pokud pokusu o tooapply heslo nezdaří, pak hello softwaru opakovaně pokusí toocollect hello heslo, dokud nebude úspěšná. Ve výjimečných případech nelze použít heslo hello. V takovém případě můžete zaregistrovat zařízení hello a pokračovat, ale nebudou změněny hello hesla. Zobrazí se žádná informace jako toowhich heslo nebylo změněno – hello hesla správce zařízení nebo heslo Snapshot Manager zařízení StorSimple hello. Pokud k této situaci dochází, doporučujeme, abyste změnili obě hesla.
> 
> 

Můžete resetovat hesla hello v hello portál Azure classic prostřednictvím hello služby StorSimple Manager. Další informace najdete v tématu: 

* [Heslo správce zařízení hello změnu](storsimple-change-passwords.md#change-the-device-administrator-password).
* [Změna hesla Snapshot Manageru zařízení StorSimple hello](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Chyby při registraci zařízení
Použití služby StorSimple Manager hello spuštěné v Microsoft Azure tooregister hello zařízení. Může dojít jeden nebo více hello následující problémy při registraci zařízení.

| Ne. | Chybová zpráva | Možné příčiny | Doporučená akce |
| --- | --- | --- | --- |
| 1 |Chyba 350027: Nepodařilo tooregister hello zařízení s hello StorSimple Manager. | |Počkejte několik minut a hello operaci opakujte. Pokud hello problém přetrvá, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md). |
| 2 |Chyba 350013: Došlo k chybě při registraci zařízení hello. Může to být kvůli registrační klíč služby tooincorrect. | |Zaregistrujte zařízení hello znovu s hello správné služby registrační klíč. Další informace najdete v tématu [registrační klíč služby hello Get.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 |Chyba 350063: Ověřování tooStorSimple Manager service předán ale registrace se nezdařila. Opakujte operaci hello po určité době. |Tato chyba znamená, že uplynutí ověřování pomocí služby ACS, ale hello registrace volání toohello služby se nezdařilo. To může být výsledek chybě ojediněle sítě. |Pokud hello problém přetrvá, prosím [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md). |
| 4 |Chyba 350049: hello nelze získat přístup ke službě během registrace. |Při volání hello toohello služby, byl přijat webové výjimka. V některých případech to může získat stanovena s opakováním hello operaci později. |Zkontrolujte, zda vaše IP adresa a název DNS a zopakujte operaci hello. Pokud hello problém přetrvává, [kontaktovat Microsoft Support.](storsimple-contact-microsoft-support.md) |
| 5 |Chyba 350031: hello zařízení už je zaregistrovaný. | |Není potřebná žádná akce. |
| 6 |Chyba 350016: Registrace zařízení se nezdařila. | |Zkontrolujte prosím, že je správný hello registrační klíč. |
| 7 |Invoke-HcsSetupWizard: Došlo k chybě při registraci zařízení; může to být kvůli tooincorrect IP adresu nebo název DNS. Zkontrolujte síťové nastavení a zkuste to znovu. Pokud hello problém přetrvává, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md). (Chyba 350050) |Ujistěte se, že zařízení může odeslat příkaz ping hello mimo síť. Pokud nemáte připojení toooutside sítě, mohou selhat hello registrace k této chybě. Tato chyba může být kombinací nejméně jeden z následujících hello:<ul><li>Nesprávná IP</li><li>Nesprávný podsítě</li><li>Nesprávný brány</li><li>Nesprávné nastavení DNS</li></ul> |V tématu hello kroky hello [podrobný příklad řešení potíží](#step-by-step-storsimple-troubleshooting-example). |
| 8 |Invoke-HcsSetupWizard: hello aktuální operace se nezdařila z důvodu tooan vnitřní chybě služby [0x1FBE2]. Opakujte operaci hello po určité době. Pokud hello potíže potrvají, kontaktujte prosím Microsoft Support. |Toto je obecná chyba vyvolána pro všechny uživatele neviditelná chyby ze služby nebo agenta. Hello nejběžnějším Důvodem může být, že hello ACS ověřování se nezdařilo. Možnou příčinou selhání hello je, že dochází k potížím s konfigurací serveru NTP hello a čas na hello zařízení není správně nastaven. |Opravte hello čas (v případě problémů) a potom opakujte operaci registrace hello. Pokud používáte hello Set HcsSystem - časové pásmo příkaz tooadjust hello časové pásmo, převedení na velká písmena jednotlivých slov v časovém pásmu hello (například "Tichomoří (běžný čas)").  Pokud potíže potrvají, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) pro další kroky. |
| 9 |Upozornění: Nebylo možné aktivovat hello zařízení. Správce zařízení a hesla Snapshot Manageru zařízení StorSimple nebyly změněny. |V případě selhání registrace hello hello Správce zařízení a hesla Snapshot Manageru zařízení StorSimple, nebudou změněny. | |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Nástroje pro řešení potíží s nasazením zařízení StorSimple
StorSimple obsahuje několik nástrojů, které můžete použít tootroubleshoot vašeho řešení StorSimple. Mezi ně patří:

* Podpora balíčky a protokolů zařízení 
* Rutiny, které jsou vytvořené speciálně pro řešení potíží 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Podporuje balíčky a protokolů zařízení, které jsou k dispozici pro řešení potíží
Balíček pro podporu obsahuje všechny relevantní protokoly hello, které mohou pomoci týmu Microsoft Support hello s potíží zařízení. Prostředí Windows PowerShell můžete použít pro StorSimple toogenerate balíček šifrované podpory, které pak můžete sdílet s pracovníky podpory.

### <a name="tooview-hello-logs-or-hello-contents-of-hello-support-package"></a>balíček pro podporu tooview hello protokoly nebo hello obsah hello
1. Pomocí prostředí Windows PowerShell pro StorSimple toogenerate balíčku pro podporu, jak je popsáno v [vytvoření a Správa balíčku pro podporu](storsimple-create-manage-support-package.md).
2. Stáhnout hello [dešifrování skriptu](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) místně na klientském počítači.
3. Použít [podrobný postup](storsimple-create-manage-support-package.md#edit-a-support-package) tooopen a dešifrování balíček pro podporu hello.
4. Hello dešifrovat balíček pro podporu, které protokoly jsou ve formátu trasování událostí pro Windows nebo etvx. Tyto soubory můžete provést následující kroky tooview hello v prohlížeči událostí systému Windows:
   
   1. Spustit hello **eventvwr** příkazů na vašeho klienta Windows. Tato akce spustí hello Prohlížeč událostí.
   2. V hello **akce** podokně klikněte na tlačítko **otevřít uložený protokol** a soubory protokolů bodu toohello ve formátu etvx nebo trasování událostí pro Windows (podpora balíček hello). Teď si můžete zobrazit soubor hello. Po otevření souboru hello kliknete pravým tlačítkem a uložte soubor hello jako text.
      
      > [!IMPORTANT]
      > Můžete taky hello **Get-WinEvent** tooopen rutiny tyto soubory v prostředí Windows PowerShell. Další informace najdete v tématu [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) v hello referenční dokumentaci rutiny prostředí Windows PowerShell.
      > 
      > 
5. Při otevření hello protokoly v prohlížeči událostí, podívejte se na hello následující protokoly, které obsahují problémy související s toohello konfigurace zařízení:
   
   * hcs_pfconfig/Operational protokolu
   * hcs_pfconfig nebo konfigurace
6. V souborech protokolů hello hledat řetězce rutiny související toohello pomocí Průvodce instalací hello. V tématu [Průvodce instalací prvního](#first-time-setup-wizard-process) seznam tyto rutiny. 
7. Pokud si nejste možné toofigure out hello příčinu problému hello, můžete [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) pro další kroky. Hello použijte kroky [vytvořit žádost o podporu](storsimple-contact-microsoft-support.md#create-a-support-request) při požádejte Microsoft Support o pomoc.

## <a name="cmdlets-available-for-troubleshooting"></a>Rutiny, které jsou k dispozici pro řešení potíží
Pomocí následující rutiny prostředí Windows PowerShell k toodetect chybám připojení hello.

* `Get-NetAdapter`: Pomocí této rutiny toodetect hello stavu síťových rozhraní. 
* `Test-Connection`: Pomocí této rutiny toocheck hello připojení k síti uvnitř i vně hello sítě.
* `Test-HcsmConnection`: Pomocí této rutiny toocheck hello připojení úspěšně registrovaná zařízení.

Pokud používáte zařízení StorSimple Update 1, jsou k dispozici také následující diagnostiky rutiny hello.

* `Sync-HcsTime`: Pomocí této rutiny toodisplay zařízení doby a vynutit synchronizace času s serveru NTP hello.
* `Enable-HcsPing`a `Disable-HcsPing`: použijte tyto rutiny tooallow hello hostitelů tooping hello síťových rozhraní zařízení StorSimple. Ve výchozím nastavení hello StorSimple síťových rozhraní neodpoví tooping požadavky.
* `Trace-HcsRoute`: Pomocí této rutiny jako nástroj pro sledování tras. Odešle směrovač tooeach pakety hello způsob tooa konečného cíle přes v časovém intervalu a pak vypočítá výsledky podle pakety hello vrácená z každého směrování. Vzhledem k tomu `Trace-HcsRoute` zobrazuje hello stupeň ztráta paketů v jakékoli dané směrovače nebo odkaz, lze přesně určit které směrovače nebo odkazy může být příčinou problémů se sítí. 
* `Get-HcsRoutingTable`: Pomocí této rutiny toodisplay hello místní směrovací tabulky protokolu IP.

## <a name="troubleshoot-with-hello-get-netadapter-cmdlet"></a>Řešení potíží s pomocí rutiny Get-NetAdapter hello
Při konfiguraci síťových rozhraní pro nasazení prvního zařízení, stav hardwaru hello není k dispozici v hello služby StorSimple Manager uživatelského rozhraní, protože hello zařízení ještě není registrována službou hello. Kromě toho hello hardwaru stavové stránce nemusí odpovídat vždy správně hello stavu hello zařízení, zejména v případě, že jsou problémy, které mají vliv synchronizace služby. V těchto situacích můžete použít hello `Get-NetAdapter` rutiny toodetermine hello stav síťových rozhraní.

### <a name="toosee-a-list-of-all-hello-network-adapters-on-your-device"></a>toosee seznam všechny síťové adaptéry hello na zařízení
1. Spusťte prostředí Windows PowerShell pro StorSimple a pak zadejte `Get-NetAdapter`. 
2. Použít hello výstup hello `Get-NetAdapter` rutiny a hello následující pokyny toounderstand hello stav síťového rozhraní.
   
   * Pokud hello rozhraní je v pořádku a povolená, hello **ifIndex** stav se zobrazuje jako **až**.
   * Pokud hello rozhraní je v pořádku, ale není fyzicky (podle síťový kabel), hello **ifIndex** se zobrazí jako **zakázané**.
   * Pokud hello rozhraní je v pořádku, ale není povoleno, hello **ifIndex** stav se zobrazuje jako **NotPresent**.
   * Pokud hello rozhraní neexistuje, nezobrazí se v tomto seznamu. Hello služby StorSimple Manager uživatelského rozhraní, bude mít toto rozhraní ve stavu selhání.

Další informace o tom, toouse Tato rutina přejděte příliš[GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) v hello referenční informace o rutinách prostředí Windows PowerShell. 

Hello následující části vysvětlují, ukázky výstup hello `Get-NetAdapter` rutiny. 

 Tyto ukázky řadič 0 byl hello pasivní řadiče a bylo nakonfigurováno takto:

* DATA 0, 1 dat, DATA 2 a DATA 3 síťové rozhraní byly na hello zařízení.
* DATA 4 a 5 dat síťových karet nebyly nalezeny; proto nejsou uvedené ve výstupu hello.
* DATA 0 bylo povoleno.

Řadič 1 byla aktivní řadiče hello a bylo nakonfigurováno takto:

* DATA 0, DATA 1, 2 dat, DATA 3, DATA 4 a DATA 5 síťové rozhraní byly na hello zařízení.
* DATA 0 bylo povoleno.

**Ukázkový výstup – řadič 0**

Hello následuje výstup hello řadič 0 (hello pasivní řadič). DATA 1, DATA 2 a DATA 3 nejsou připojené. DATA 4 a 5 dat nejsou zobrazeny, protože se nenachází na hello zařízení. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Ukázkový výstup – řadič 1**

Hello následuje výstup hello řadiči 1 (hello active řadič). Pouze hello DATA 0 síťové rozhraní na zařízení hello je nakonfigurován a funguje.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent


## <a name="troubleshoot-with-hello-test-connection-cmdlet"></a>Poradce při potížích s rutiny Test-Connection hello
Můžete použít hello `Test-Connection` rutiny toodetermine jestli může zařízení StorSimple připojit toohello mimo síť. Pokud všechny hello síťové parametry, včetně hello DNS, jsou v Průvodci instalací hello správně nakonfigurovaný, můžete použít hello `Test-Connection` rutiny tooping známou adresu mimo hello sítě, jako je outlook.com. 

Pokud je zakázána ping, měli byste povolit problémy s připojením k tootroubleshoot ping pomocí této rutiny.

Najdete v následujících ukázky výstupu z hello hello `Test-Connection` rutiny. 

> [!NOTE]
> V první ukázce hello hello zařízení nakonfigurované s nesprávnou DNS. V ukázce druhý hello hello DNS je správná.
> 
> 

**Ukázkový výstup – nesprávný DNS**

V následující ukázka hello neexistuje žádný výstup pro adresy IPV4 a IPV6 hello, což znamená, že hello DNS nevyřeší. To znamená, že neexistuje žádná připojení toohello mimo síť a správné DNS musí toobe zadaný. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Ukázkový výstup – správné DNS**

V následující ukázka hello hello DNS vrátí hello IPV4 adresu, která určuje, že hello, které je server DNS správně nakonfigurovaný. To potvrdí, že existuje připojení toohello mimo síť. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-hello-test-hcsmconnection-cmdlet"></a>Poradce při potížích s rutiny Test-HcsmConnection hello
Použití hello `Test-HcsmConnection` rutiny pro zařízení, které je již připojen tooand zaregistrována služby StorSimple Manager. Tato rutina vám pomůže zkontrolovat hello připojení mezi registrované zařízení a hello odpovídající služby StorSimple Manager. Tento příkaz lze spustit v prostředí Windows PowerShell pro StorSimple. 

### <a name="toorun-hello-test-hcsmconnection-cmdlet"></a>rutina Test-HcsmConnection hello toorun
1. Zkontrolujte, zda že je zaregistrován hello zařízení.
2. Zkontrolujte stav zařízení hello. Pokud je deaktivována hello zařízení, v režimu údržby, nebo v režimu offline, může se zobrazit hello následujícím chybám: 
   
   * ErrorCode.CiSDeviceDecommissioned – to znamená, že toto zařízení hello je deaktivována.
   * ErrorCode.DeviceNotReady – to znamená, že toto zařízení hello je v režimu údržby.
   * ErrorCode.DeviceNotReady – to znamená, že toto zařízení hello není online.
3. Ověřte, že hello služby StorSimple Manager je spuštěna (použijte hello [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) rutiny). Pokud hello služba neběží, může se zobrazit hello následujícím chybám:
   
   * ErrorCode.CiSApplianceAgentNotOnline
   * ErrorCode.CisPowershellScriptHcsError – to znamená, že při spuštění Get-ClusterResource došlo k výjimce.
4. Zkontrolujte hello token služby Řízení přístupu (ACS). Pokud se vyhodí výjimku web, může to být výsledek hello problém brány, chybějící ověření proxy serveru, nesprávný DNS nebo selhání ověřování. Můžete se setkat hello následujícím chybám:
   
   * ErrorCode.CiSApplianceGateway – to znamená výjimku HttpStatusCode.BadGateway: hello název překladače služby nešlo přeložit název hostitele hello. 
   * ErrorCode.CiSApplianceProxy – to znamená výjimku HttpStatusCode.ProxyAuthenticationRequired (kód stavu HTTP 407): hello klienta nelze ověřit pomocí hello proxy serveru. 
   * ErrorCode.CiSApplianceDNSError – to znamená WebExceptionStatus.NameResolutionFailure výjimka: hello název překladače služby nešlo přeložit název hostitele hello.
   * ErrorCode.CiSApplianceACSError –, to znamená, že hello služba vrátila chybu ověřování, ale je k dispozici připojení.
     
     Pokud nevyvolá výjimku webové, zkontrolujte ErrorCode.CiSApplianceFailure. To znamená, že hello zařízení se nezdařila.
5. Zkontrolujte připojení hello cloudové služby. Pokud služba hello vyhodí výjimku web, může se zobrazit hello následujícím chybám:
   
   * ErrorCode.CiSApplianceGateway – to znamená výjimku HttpStatusCode.BadGateway: zprostředkujícího serveru proxy přijal neplatný požadavek z jiné proxy serveru nebo z původního serveru hello.
   * ErrorCode.CiSApplianceProxy – to znamená výjimku HttpStatusCode.ProxyAuthenticationRequired (kód stavu HTTP 407): hello klienta nelze ověřit pomocí hello proxy serveru. 
   * ErrorCode.CiSApplianceDNSError – to znamená WebExceptionStatus.NameResolutionFailure výjimka: hello název překladače služby nešlo přeložit název hostitele hello.
   * ErrorCode.CiSApplianceACSError –, to znamená, že hello služba vrátila chybu ověřování, ale je k dispozici připojení.
     
     Pokud nevyvolá výjimku webové, zkontrolujte ErrorCode.CiSApplianceSaasServiceError. To znamená problém s hello služby StorSimple Manager.
6. Zkontrolujte připojení k Azure Service Bus. ErrorCode.CiSApplianceServiceBusError označuje, že toto zařízení hello se nemůže připojit toohello Service Bus.

Hello soubory protokolu CiSCommandletLog0Curr.errlog a CiSAgentsvc0Curr.errlog bude obsahovat další informace, jako je například Podrobnosti o výjimce. 

Další informace o tom, jak toouse hello rutiny, přejděte příliš[Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) v prostředí Windows PowerShell hello referenční dokumentace.

> [!IMPORTANT]
> Spuštěním této rutiny pro hello aktivní a pasivní řadiče hello. 
> 
> 

Najdete v následujících ukázky výstupu z hello hello `Test-HcsmConnection` rutiny. 

**Ukázkový výstup – úspěšně registrovaná zařízení se systémem verze zařízení StorSimple (červenec 2014)**

první ukázka Hello je ze zařízení, které úspěšně registrovaná s hello služby StorSimple Manager a nemá žádné problémy s připojením. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes toocomplete....
     Checking device authentication.  ... Success.
     Checking connectivity from device tooStorSimple Manager service.  ... This operation will take few minutes toocomplete....
     Checking connectivity from device tooStorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service tooStorSimple device. .... Success.
     Controller1>

**Ukázkový výstup – úspěšně registrovaná zařízení se systémem StorSimple Update 1**

Pokud používáte zařízení StorSimple Update 1, nebude nutné toorun ho s přepínačem podrobné hello.

      Controller1>Test-HcsmConnection

      Checking device registration state  ... Success
      Device registered successfully

      Checking primary NTP server [time.windows.com] ... Success

      Checking web proxy  ... NOT SET

      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET

      Checking device online  ... Success

      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success

      Checking connectivity from device tooservice  ... This will take a few minutes.

      Checking connectivity from device tooservice  ... Success

      Checking connectivity from service toodevice  ... Success

      Checking connectivity tooMicrosoft Update servers  ... Success
      Controller1>

**Ukázkový výstup – offline zařízení StorSimple verzí (červenec 2014)**

Tato ukázka je ze zařízení, které je ve stavu **Offline** v hello portál Azure classic.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device tooSaaS.. Failure

Hello zařízení nelze připojit pomocí hello aktuální konfigurace webového proxy serveru. Může jít o problém s hello konfigurace webového proxy serveru nebo problém se síťovým připojením. V takovém případě by měl Ujistěte se, že vaše nastavení proxy serveru webové správné a webových proxy serverů jsou online a dostupný. 

## <a name="troubleshoot-with-hello-sync-hcstime-cmdlet"></a>Poradce při potížích s hello synchronizace HcsTime rutiny
Pomocí této rutiny toodisplay hello zařízení doby. Pokud čas zařízení hello posun s serveru NTP hello, pak můžete použít tuto rutinu tooforce-synchronizovat čas hello s serveru NTP. Pokud hello posun mezi hello zařízení a serveru NTP je větší než 5 minut, zobrazí se upozornění. Posun hello překročí 15 minut, hello zařízení bude přejděte do režimu offline. Můžete dál používat tuto rutinu tooforce synchronizace času. Překročí-li hello posun po dobu 15 hodin, pak nebude možné tooforce synchronizace hello čas a zobrazí chybovou zprávu.

**Ukázkový výstup – synchronizace vynucené času pomocí HcsTime synchronizace**

     Controller0>Sync-HcsTime
     hello current device time is 4/24/2015 4:05:40 PM UTC.

     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want tooresync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-hello-enable-hcsping-and-disable-hcsping-cmdlets"></a>Poradce při potížích s rutiny HcsPing povolit a zakázat HcsPing hello
Použijte tyto rutiny tooensure, že hello síťových rozhraní na vašem zařízení reagovat tooICMP žádosti příkazu ping. Ve výchozím nastavení hello StorSimple síťových rozhraní neodpoví tooping požadavky. Použití této rutiny je nejjednodušší způsob, jak tooknow hello, pokud vaše zařízení je online a dostupný.  

**Ukázkový výstup – HcsPing povolit a zakázat HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-hello-trace-hcsroute-cmdlet"></a>Poradce při potížích s hello trasování HcsRoute rutiny
Použijte tuto rutinu jako nástroj pro sledování tras. Odešle směrovač tooeach pakety hello způsob tooa konečného cíle přes v časovém intervalu a pak vypočítá výsledky podle pakety hello vrácená z každého směrování. Protože rutiny hello ukazuje hello stupeň ztráta paketů v každém směrovači nebo odkaz, lze přesně určit které směrovače nebo odkazy může být příčinou problémů se sítí.

**Ukázkový výstup znázorňující, jak tootrace hello trasy paket se HcsRoute trasování**

     Controller0>Trace-HcsRoute -Target 10.126.174.25

     Tracing route toocontoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]

     Computing statistics for 25 seconds...
                 Source tooHere   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]

     Trace complete.

## <a name="troubleshoot-with-hello-get-hcsroutingtable-cmdlet"></a>Řešení potíží s pomocí rutiny Get-HcsRoutingTable hello
Pomocí této směrovací tabulky rutiny tooview hello pro zařízení StorSimple. Směrovací tabulka je sada pravidel, které vám mohou pomoci určit, kde budou přesměrováni datových paketů cestě přes síť Internet Protocol (IP). 

Hello směrovací tabulka zobrazuje hello rozhraní a hello že trasy hello data toohello zadána brána sítě. Dává také hello směrování metriku, což je hello rozhodovací pravomocí pro hello trasu tooreach určitý cíl. Hello nižší hello směrování metriky, hello vyšší hello předvoleb. 

Například, pokud máte 2 síťová rozhraní DATA 2 a DATA 3 připojené toohello Internetu. Pokud hello směrování metriky pro DATA 2 a DATA 3 jsou 15 a 261, DATA 2 s nižší směrování metrika hello je, že tooreach hello Internetu použít upřednostňované rozhraní hello.

Pokud používáte zařízení StorSimple Update 1, má vaše síťového rozhraní DATA 0 hello nejvyšší předvoleb pro provoz cloudové hello. To znamená, že i když existují další rozhraní povolenou podporu cloudu, by provoz cloudu hello směrován přes DATA 0. 

Pokud spustíte hello `Get-HcsRoutingTable` rutiny bez zadání všech parametrů (jako hello následující příklad ukazuje), hello rutiny výstup směrovací tabulky IPv4 i IPv6. Alternativně můžete zadat `Get-HcsRoutingTable -IPv4` nebo `Get-HcsRoutingTable -IPv6` tooget relevantní směrovací tabulky.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================

      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================

      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None

      Controller0>

## <a name="step-by-step-storsimple-troubleshooting-example"></a>Podrobný příklad řešení potíží StorSimple
Hello následující příklad ukazuje podrobné řešení potíží s nasazení zařízení StorSimple. V ukázkovém scénáři hello registrace zařízení nezdaří a zobrazí se chybová zpráva oznamující, že nastavení sítě hello nebo název DNS hello je nesprávná.

je vrácen Hello chybová zpráva:

     Invoke-HcsSetupWizard: An error has occurred while registering hello device. This could be due tooincorrect IP address or DNS name. Please check your network settings and try again. If hello problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Hello chyba může být způsobena kterékoli z následujících hello:

* Instalace nesprávný hardwaru
* Vadné síti alespoň jedno rozhraní
* Nesprávnou adresu IP, masky podsítě, brány, primární server DNS nebo webového proxy serveru
* Nesprávný registrační klíč
* Nastavení brány firewall nesprávný

### <a name="toolocate-and-fix-hello-device-registration-problem"></a>toolocate a opravte problém registrace zařízení hello
1. Zkontrolujte konfiguraci zařízení: na řadič active hello spusťte `Invoke-HcsSetupWizard`.
   
   > [!NOTE]
   > Průvodce instalací Hello musí spustit na řadiči active hello. tooverify, které jsou připojené toohello aktivní řadiče, podívejte se na hello banner uvedené v hello konzoly sériového portu. Hlavička Hello Určuje, jestli jsou připojené toocontroller 0 nebo řadič 1, a zda je řadič hello aktivní nebo pasivní. Další informace, přejděte příliš[identifikovat v zařízení aktivní řadič](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
   > 
   > 
2. Ujistěte se, že hello zařízení je správně zapojena jeho: Zkontrolujte kabelů na zařízení hello zpět ploché sítě hello. kabeláž Hello je konkrétní toohello model zařízení. Další informace, přejděte příliš[instalace zařízení StorSimple 8100](storsimple-8100-hardware-installation.md) nebo [instalace zařízení StorSimple 8600](storsimple-8600-hardware-installation.md).
   
   > [!NOTE]
   > Pokud používáte 10 GbE síťové porty, je nutné zadat QSFP SFP adaptéry a kabely SFP hello toouse. Další informace najdete v tématu hello [seznam kabely, přepínače a vysílače doporučeno pro hello 10 GbE porty](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
   > 
   > 
3. Ověřte stav hello hello síťové rozhraní:
   
   * Pomocí rutiny Get-NetAdapter hello toodetect hello stav hello síťová rozhraní data 0. 
   * Pokud není funkce hello odkaz, hello **ifindex** stav označí, že rozhraní hello je mimo provoz. Pak musíte toocheck hello síťové připojení hello port toohello zařízení a toohello přepínače. Budete také potřebovat toorule out chybný kabely. 
   * Pokud máte podezření, že hello DATA 0 na řadič active hello port se nezdařila, můžete to ověřit připojením toohello DATA 0 portu na řadiči 1. tooconfirm, odpojte hello síťový kabel od hello zadní hello zařízení z řadič 0, připojte kabel toocontroller hello 1 a poté znovu spusťte rutinu Get-NetAdapter hello. 
     Pokud hello DATA 0 port na řadiči selže, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) pro další kroky. V systému může být nutné tooreplace hello řadiče.
4. Ověřte hello připojení toohello přepínače:
   
   * Ujistěte se, že jsou DATA 0 síťová rozhraní řadič 0 a řadič 1 ve vaší primární skříň na hello stejné podsíti. 
   * Zkontrolujte hello rozbočovače nebo směrovač. Obvykle by měl připojit oba řadiče toohello stejné rozbočovače nebo směrovač. 
   * Ověření, zda používáte pro připojení hello přepínače hello DATA 0 pro oba řadiče v hello stejnou síť vLAN.
5. Vyloučit všechny chyby uživatele:
   
   * Spusťte znovu Průvodce instalací hello (Spustit **Invoke-HcsSetupWizard**) a zadejte hodnoty hello znovu toomake se, že nejsou žádné chyby. 
   * Ověření registrace hello klíč používaný. Hello stejné registrační klíč může být použité tooconnect více tooa zařízení služby StorSimple Manager. Pomocí postupu hello v [registrační klíč služby hello Get](storsimple-manage-service.md#get-the-service-registration-key) tooensure, který používáte hello správné registrační klíč.
     
     > [!IMPORTANT]
     > Pokud máte více služby spuštěné, budete potřebovat tooensure této hello registrační klíč pro příslušnou službu hello je použité tooregister hello zařízení. Pokud zařízení jste zaregistrovali hello nesprávný služby StorSimple Manager, budete potřebovat příliš[kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) pro další kroky. Můžete mít tooperform obnovení továrního nastavení zařízení hello, (což by mohlo vést ke ztrátě dat) toothen připojte toohello určený služby.
     > 
     > 
6. Použijte tooverify rutiny hello Test-Connection, abyste měli toohello připojení mimo síť. Další informace, přejděte příliš[Poradce při potížích s rutiny Test-Connection hello](#troubleshoot-with-the-test-connection-cmdlet).
7. Zkontrolujte, zda brána firewall narušení. Pokud si ověříte, že hello virtuální IP (VIP), podsíť, bránu a DNS nastavení jsou všechny správné a stále vidět problémy s připojením, pak je možné, že brána firewall blokuje komunikaci mezi zařízením a hello mimo síť. Je nutné tooensure, že porty 80 a 443 jsou k dispozici v zařízení StorSimple pro odchozí komunikaci. Další informace najdete v tématu [požadavcích sítě pro zařízení StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
8. Podívejte se na protokoly hello. Přejděte příliš[podporuje balíčky a protokolů zařízení, které jsou k dispozici pro řešení potíží s](#support-packages-and-device-logs-available-for-troubleshooting).
9. V případě hello předchozí kroky není hello problém vyřešit, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) o pomoc.

## <a name="next-steps"></a>Další kroky
[Zjistěte, jak tootroubleshoot provozní zařízení](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
