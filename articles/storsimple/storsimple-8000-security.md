---
title: "zabezpečení řady aaaStorSimple 8000 | Microsoft Docs"
description: "Popisuje hello zabezpečení a ochrany osobních údajů funkce, které chránit služby StorSimple, zařízení a data místně a v cloudu hello."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 48dd449d2908c21fe05d0ed37a4dc6f3e306e43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-security-and-data-protection"></a>StorSimple zabezpečení a ochranu dat.

## <a name="overview"></a>Přehled

Zabezpečení představuje závažný problém pro každého, kdo používá novou technologii, zejména v případě, že technologie hello používá s důvěrných nebo vlastnických dat.. Jak můžete vyhodnotit různých technologií, musíte zvážit, zvýšená rizika a náklady na ochranu dat. Microsoft Azure StorSimple poskytuje zabezpečení a ochrany osobních údajů řešení pro ochranu dat, pomáhá tooensure:

* **Důvěrnost** – pouze autorizovaný entity můžete zobrazit vaše data.
* **Integrita** – můžete upravit nebo odstranit data jenom autorizovaní entity.

Hello řešení Microsoft Azure StorSimple zahrnuje čtyři hlavní součásti, které spolupracují mezi sebou:

* **Správce zařízení StorSimple služby hostované v Microsoft Azure** – použijete tooconfigure a zřízení služby správy hello hello zařízení StorSimple.
* **Zařízení StorSimple** – fyzických zařízení nainstalované ve vašem datovém centru. Všechny hostitele a klienty, které generují data připojení zařízení StorSimple toohello a hello zařízení spravuje hello data a přesouvá ji toohello cloudu Azure podle potřeby.
* **Klienti nebo hostitelů připojené zařízení toohello** – hello klientů ve vaší infrastruktuře, které se připojují zařízení StorSimple toohello a vygenerovat data, která potřebují toobe chráněný.
* **Cloudového úložiště** – hello umístění v cloudu Azure, které jsou uložená data hello.

Hello následující části popisují funkce hello StorSimple zabezpečení, které pomáhají chránit všechny tyto součásti a hello data uložená na ně. Zahrnuje také seznam dotazů, které by mohly mít o zabezpečení Microsoft Azure StorSimple a příslušných odpovědí hello.

## <a name="storsimple-device-manager-service-protection"></a>Ochrana služby StorSimple Manager zařízení

Hello služby StorSimple Manager zařízení se službou správy hostované ve službě Microsoft Azure a použít toomanage všechna zařízení StorSimple, které vaše organizace má si opatřili. Hello služby StorSimple Manager zařízení můžete přístup k pomocí vaší firemní přihlašovací údaje toolog toohello portál Azure prostřednictvím webového prohlížeče.

Přístup k toohello služby StorSimple Manager zařízení vyžaduje, aby bylo vaše organizace předplatné Azure, která zahrnuje StorSimple. Vaše předplatné řídí funkce hello, kterým můžete přistupovat v hello portálu Azure. Pokud vaše organizace nemá předplatné služby Azure a chcete další informace o jejich toolearn, přečtěte si téma [registraci do Azure jako organizace](../active-directory/sign-up-organization.md).

Protože hello služby StorSimple Manager zařízení je hostované v Azure, je chráněn funkce Azure zabezpečení hello. Další informace o funkcích zabezpečení hello poskytované Microsoft Azure, přejděte toohello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>Ochrana zařízení StorSimple

zařízení StorSimple Hello je místní hybridní úložné zařízení, která obsahuje jednotek SSD (Solid-State Drive) a pevných disků (HDD), společně s redundantní řadiče a možnosti automatické převzetí služeb při selhání. Hello řadiče spravovat úložiště vrstvení, umístění aktuálně používá (nebo aktivní) data na místní úložiště (v hello zařízení StorSimple nebo místní servery), při přesouvání toohello méně často používaných dat v cloudu.

Pouze oprávnění StorSimple zařízení jsou povoleny toojoin hello služby StorSimple Manager zařízení, který jste vytvořili ve vašem předplatném Azure. tooauthorize zařízení, je nutné ji se zaregistrovat hello služby StorSimple Manager zařízení tím, že poskytuje registrační klíč služby hello. registrační klíč služby Hello je náhodný klíč 128-bit vygenerovaných hello portálu Azure.

![Registrační klíč služby](./media/storsimple-security/ServiceRegistrationKey.png)

toolearn příliš jak získat klíče, přejděte služby registrace[krok 2: registrační klíč služby hello Get](storsimple-8000-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).

registrační klíč služby Hello je dlouhá klíč, který obsahuje 100 + znaky. Můžete zkopírovat hello klíč a uložit ho do textového souboru v zabezpečeném umístění, abyste použitím tooauthorize další zařízení podle potřeby. Pokud registrační klíč služby hello dojde ke ztrátě po registraci zařízení s prvním, můžete z hello služby StorSimple Manager zařízení vygenerovat nový klíč. To nebude mít vliv na operace hello stávajících zařízení.

Po registraci zařízení používá tokeny toocommunicate s Microsoft Azure. registrační klíč služby Hello nepoužívá po registraci zařízení.

> [!NOTE]
> Doporučujeme, abyste po každé použití znovu vygenerovat registrační klíč služby hello.


## <a name="protect-your-storsimple-solution-via-passwords"></a>Ochrana vašeho řešení StorSimple pomocí hesla

Hesla jsou důležitým aspektem zabezpečení počítače a jsou používány v řešení StorSimple hello toohelp zajistěte, aby vaše data jenom přístupné tooauthorized uživatele. StorSimple umožňuje tooconfigure hello následující hesla:

* Heslo správce zařízení StorSimple
* Výzvu, hesla a iniciátor ověřování protokol CHAP (Handshake)
* Heslo správce Snapshot Manageru zařízení StorSimple

### <a name="windows-powershell-for-storsimple-and-hello-storsimple-device-administrator-password"></a>Prostředí Windows PowerShell pro zařízení StorSimple a hello hesla správce zařízení StorSimple

Prostředí Windows PowerShell pro StorSimple je rozhraní příkazového řádku, které můžete použít zařízení StorSimple toomanage hello. Prostředí Windows PowerShell pro StorSimple je funkce, které vám umožňují tooregister zařízení, nakonfigurujte rozhraní sítě hello na vašem zařízení, instalace určité typy aktualizací, řešení potíží s zařízení s přístupem k relaci podpory hello a změnit stav zařízení hello . Prostředí Windows PowerShell pro StorSimple můžete přistupovat pomocí připojování konzoly sériového portu toohello na hello zařízení nebo pomocí vzdálené komunikace Windows Powershellu.

Vzdálená komunikace prostředí PowerShell lze provést prostřednictvím protokolu HTTPS nebo HTTP. Pokud je povoleno vzdálené správy přes protokol HTTPS, budete potřebovat toodownload hello vzdálené správy certifikátů z hello zařízení a jejich instalace vzdáleného klienta hello. Další informace o vzdálenou komunikaci prostředí PowerShell přejděte příliš[vzdáleně připojit zařízení StorSimple tooyour](storsimple-8000-remote-connect.md).

Po použití prostředí Windows PowerShell pro zařízení StorSimple tooconnect toohello, budete potřebovat toolog heslo správce zařízení pro tooprovide hello na toohello zařízení.

![Heslo správce zařízení](./media/storsimple-security/DeviceAdminPW.png)

Udržovat hello následující osvědčené postupy v paměti:

* Vzdálená správa je ve výchozím nastavení vypnutý. Můžete použít tooenable služby StorSimple Manager zařízení hello ho. Z hlediska zabezpečení je nejlepší vzdáleným by měly být povoleny pouze během hello časové období, která je skutečně potřeba.
* Pokud změníte heslo hello, být jisti toonotify všichni uživatelé vzdáleného přístupu tak, že nedochází ztrátě neočekávané připojení.
* Hello služby StorSimple Manager zařízení nelze načíst existující hesla: ho můžete pouze resetovat. Doporučujeme, abyste tak, že nemáte tooreset heslo je zapomenete-li uložit všechna hesla na bezpečném místě. Pokud potřebujete tooreset heslo, být jisti toonotify všichni uživatelé předtím, než ho resetovat.

Rozhraní Windows PowerShell hello dostanete pomocí zařízení toohello sériové připojení. Můžete také k němu přístup vzdáleně pomocí protokolu HTTP nebo HTTPS, který zvýšit zabezpečení. Protokol HTTPS nabízí vyšší úroveň zabezpečení než buď sériové nebo připojení HTTP. Ale toouse HTTPS, musíte nejdřív nainstalujete certifikát na hello klientský počítač, který bude přistupovat k zařízení hello. Certifikát pro vzdálený přístup hello můžete stáhnout ze stránky konfigurace zařízení hello hello služby StorSimple Manager zařízení. Pokud dojde ke ztrátě hello certifikátu pro vzdálený přístup, je nutné stáhnout nový certifikát a rozšířit ji tooall klienty, kteří jsou autorizovaní toouse vzdálené správy.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Výzvu, hesla a iniciátor ověřování protokol CHAP (Handshake)

Protokol CHAP je příslušné schéma ověřování používá hello zařízení StorSimple toovalidate hello identitu vzdálených klientů. ověření Hello je založena na sdílené heslo. CHAP může být jednosměrná (Jednosměrný) nebo vzájemné (obousměrné). Cíl hello (zařízení StorSimple hello) s jednosměrný CHAP, ověřuje iniciátor (hostitel). Vzájemné nebo zpětného CHAP vyžaduje, aby hello cíl ověření hello iniciátor a pak hello iniciátor ověření hello cíl. Vaše zařízení StorSimple může být nakonfigurované toouse metody.

Mějte na paměti, který následuje hello při konfiguraci CHAP:

* Hello CHAP uživatelské jméno musí obsahovat méně než 233 znaků.
* Hello CHAP heslo musí být 12 až 16 znaků. Pokus toouse delší uživatelské jméno nebo heslo povede k chybě ověřování na hostitele Windows hello.
* Nemůžete použít hello stejné heslo pro iniciátoru CHAP hello i cíle CHAP hello.
* Po nastavení hello heslo, můžete změnit ale ho nelze načíst. Pokud se změní heslo hello, být zda toonotify všichni uživatelé vzdáleného přístupu, aby se můžete úspěšně připojit zařízení StorSimple toohello.

Další informace o CHAP a jak tooconfigure pro vašeho řešení StorSimple přejděte příliš[konfigurace CHAP pro vaše zařízení StorSimple](storsimple-8000-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>Heslo správce Snapshot Manageru zařízení StorSimple

Snapshot Manager zařízení StorSimple je modul snap-in konzoly Microsoft Management Console (MMC), využívající skupiny svazku a zálohování konzistentní s aplikací toogenerate služby Stínová kopie svazku Windows hello. Kromě toho můžete použít StorSimple Snapshot Manager toocreate plánů zálohování a klonování nebo obnovit svazky.

Když konfigurujete toouse zařízení StorSimple Snapshot Manager, bude heslo Snapshot Manager zařízení StorSimple požadované tooprovide hello. Toto heslo je nejdřív nastavit ve Windows Powershellu pro StorSimple během registrace. Hello heslo můžete také nastavit a změnit z hello služby StorSimple Manager zařízení. Toto heslo ověřuje hello zařízení s StorSimple Snapshot Manager.

![Heslo správce Snapshot Manageru zařízení StorSimple](./media/storsimple-security/SnapshotMgrPassword.png)

heslo Snapshot Manager zařízení StorSimple Hello musí být 14 too15 znaků a musí obsahovat 3 nebo více z kombinace velká písmena, malá písmena, číselné a speciální znaky. Po nastavení hesla Snapshot Manageru zařízení StorSimple hello lze změnit ale ho nelze načíst. Pokud změníte heslo hello, být toonotify se všichni vzdálení uživatelé.

Další informace o Snapshot Manager zařízení StorSimple, přejděte příliš[co je StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Osvědčené postupy heslo

Doporučujeme použít následující hello pokyny toohelp zajistěte, aby hesel zařízení StorSimple silné a dobře chráněný:

* Změna hesla každé tři měsíce. Změna hesel hello se vynucuje ročně.
* Používejte silná hesla. Další informace, přejděte příliš[vytvářet silnější hesla a chránit je](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
* Vždy používejte různá hesla pro jiný přístup mechanismy; Každý hello hesel, které zadáte, musí být jedinečné.
* Nemáte sdílet hesla s každý, kdo není autorizovaný tooaccess zařízení StorSimple hello.
* Mluvit o zadání hesla před ostatní nebo pomocného parametru v hello formát hesla.
* Pokud máte podezření, že účtu nebo hesla došlo k ohrožení zabezpečení, sestavy hello incidentu tooyour informace zabezpečení oddělení.
* Všechna hesla považovat za důvěrné, citlivé informace. 

## <a name="storsimple-data-protection"></a>Ochrana dat StorSimple

Tato část popisuje hello StorSimple zabezpečení funkce, které chrání data při přenosu a uložená data.

Jak je popsáno v dalších částech, jsou použité tooauthorize hesla a ověřování uživatelů, než získají přístup k řešení StorSimple tooyour. Zabezpečení zamyslet je ochrana dat před neoprávněnými uživateli, zatímco probíhá přenosu mezi úložných systémů, a když je uložená. Hello následující části popisují funkce ochrany dat hello součástí StorSimple.

> [!NOTE]
> Odstranění duplicitních dat zajišťuje zvýšenou ochranu pro data uložená na zařízení StorSimple hello a v úložišti Microsoft Azure. Když se odstranění duplicit dat, hello datové objekty jsou uloženy odděleně od hello metadata používaná toomap a přistupovat k nim: nejsou žádná data hello tooreconstruct dostupné kontextu úroveň úložiště na základě strukturu svazku, systém souborů nebo název souboru.


## <a name="protect-data-flowing-through-hello-service"></a>Ochrana dat předávaných mezi hello služby

primárním účelem Hello hello služby StorSimple Manager zařízení je toomanage a konfigurace zařízení StorSimple hello. Hello služby StorSimple Manager zařízení běží v Microsoft Azure. Použijete hello Azure portálu tooenter zařízení konfigurační data a použije toohello zařízení data toosend hello hello Správce zařízení StorSimple služby Microsoft Azure. StorSimple používá systém asymetrického klíče dvojice toohelp Ujistěte se, že ohrožení hello služby Azure nebudou mít za následek ohrožení uložené informace.

![Šifrování dat v cestě](./media/storsimple-security/DataEncryption.png)

asymetrické klíče systému Hello data chráněna v hello přenášeného přes hello služby následujícím způsobem:

1. Certifikát šifrování dat, který používá veřejné a privátní pár asymetrických klíčů se generuje na hello zařízení a je použité tooprotect hello data. Hello klíče jsou generovány, pokud hello první zařízení je zaregistrované.
2. šifrovací certifikát klíče Hello dat jsou exportovány do souboru Personal Information Exchange (.pfx), který je chráněn hello šifrovacího klíče dat služby, což je silné 128 bitů klíč, který je náhodně generované hello první zařízení během registrace.
3. Hello veřejný klíč certifikátu hello bezpečně přišla k dispozici toohello služby StorSimple Manager zařízení se u zařízení hello zůstane hello privátní klíč.
4. Data, která vstup hello služby jsou šifrována pomocí hello veřejný klíč a dešifrovat pomocí hello privátní klíč uložený na hello zařízení, zajistit, že hello služba Azure nelze dešifrovat hello dat odesílaných toohello zařízení.

šifrovací klíč dat služby Hello se vygeneruje jenom hello první zařízení registrovaný ve službe hello. Všechny následné zařízení, které jsou registrovány hello služby musíte použít hello stejné šifrovacího klíče dat služby.

> [!IMPORTANT]
> Je velmi důležité toomake kopii šifrovacího klíče dat služby hello a uložit na bezpečném místě. Kopie šifrovacího klíče dat služby hello by měly být uložené tak, že budou mít přístup oprávněné osoby a může být snadno informovat toohello Správce zařízení.
> 
> Pokud dojde ke ztrátě šifrovacího klíče dat služby hello, pracovníci technické podpory společnosti Microsoft vám může pomoct tooretrieve ji zadat, že máte alespoň jedno zařízení ve stavu online. Doporučujeme změnit šifrovacího klíče dat služby hello ho načte. Pokyny najdete příliš[šifrovacího klíče dat služby změnu hello](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Můžete změnit šifrovacího klíče dat služby hello a hello odpovídající certifikát datového šifrovacího výběrem hello **šifrovacího klíče dat služby změnu** možnost na řídicím panelu služby hello. tooensure, že nedojde k ohrožení zabezpečení dat, je nutné použít fyzického zařízení StorSimple zařízení toochange hello šifrovacího klíče dat služby. Změna hello šifrovací klíče vyžaduje, aby všechna zařízení aktualizovat pomocí nového klíče hello. Doporučujeme proto, že změníte klíč hello všechna zařízení jsou online. Pokud zařízení offline, jejich klíče lze změnit v jinou dobu. zařízení Hello zastaralé klíče budou mít pořád moct toorun zálohování, ale nebudou ji moct toorestore data až po aktualizaci klíče hello. Další informace, přejděte příliš[použití řídicího panelu služby StorSimple Manager zařízení hello](storsimple-8000-service-dashboard.md).

šifrovací klíč dat služby Hello a certifikát pro šifrování dat hello nevyprší platnost. Doporučujeme však změnit šifrování dat služby hello klíče ročně toohelp zabránit ohrožení zabezpečení klíčů.

## <a name="protect-data-at-rest"></a>Ochrana dat v klidovém stavu

zařízení StorSimple Hello spravuje data jejich uložením do vrstvy místně a v cloudu hello, v závislosti na četnost použití. Všechny hostitele počítače, které jsou připojené toohello zařízení odesílat data toohello zařízení pak se posouvá toohello dat v cloudu, podle potřeby. Data se přenáší z cloudu toohello zařízení hello bezpečně prostřednictvím Internetu hello. Každé zařízení má jeden cíl iSCSI, která vyvolá všechny sdílené svazky na daném zařízení. Všechna data se šifrují před odesláním toocloud úložiště. 

![Šifrovací klíč cloudového úložiště](./media/storsimple-security/CloudStorageEncryption.png)

toohelp zajistit zabezpečení hello a integritu dat přesunout toohello cloudu, StorSimple vám umožní toodefine cloudové úložiště šifrovacích klíčů následujícím způsobem:

* Když vytvoříte kontejner svazků zadáte hello šifrovací klíč cloudového úložiště. klíč Hello nelze změnit ani přidat později.
* Všechny svazky ve sdílené složce kontejneru svazku hello stejný šifrovací klíč. Pokud chcete různé typy šifrování pro konkrétní svazek, doporučujeme vytvořit nové toohost kontejneru svazku tohoto svazku.
* Když zadáte v hello služby StorSimple Manager zařízení hello šifrovací klíč cloudového úložiště, hello je klíč zašifrovaný pomocí hello veřejnou část šifrovacího klíče dat služby hello a pak se odešle toohello zařízení.
* šifrovací klíč cloudového úložiště Hello není uložit kdekoli v hello služby a je známý jenom toohello zařízení.
* Určení šifrovací klíč cloudového úložiště je volitelné. Může odesílat data, která byla dříve zašifrována na zařízení toohello hello hostitele.

### <a name="additional-security-best-practices"></a>Osvědčené postupy zabezpečení další

* Rozdělit provoz: nasazení zcela oddělené sítě a pomocí sítí VLAN, kde fyzické izolace není možné izolovat vaše síť SAN iSCSI z provozu generovaného uživateli v podnikové síti LAN. Síť vyhrazený pro úložiště iSCSI bude zaručit hello zabezpečení a výkonu vaše důležitá obchodní data. Kombinování přenosů dat úložiště a uživatelů v podnikových sítích LAN se nedoporučuje a můžete zvýšit latence a způsobit selhání sítě.
* Pro zabezpečení sítě na straně hostitele použijte síťových rozhraní, které podporují TCP/IP Offload Engine (TOE). TOE snižuje zatížení procesoru na síťový adaptér hello zpracováním TCP.

## <a name="protect-data-via-storage-accounts"></a>Ochrana dat pomocí účtů úložiště

Každé předplatné Microsoft Azure můžete vytvořit jeden nebo více účtů úložiště. (Účet úložiště poskytuje jedinečný obor názvů pro práci s daty uloženými v hello cloudu Azure.) Účet pro přístup k tooa úložiště je řízena hello předplatného a přístupových klíčů přidružené k tomuto účtu úložiště.

Při vytváření účtu úložiště vygeneruje Microsoft Azure dva 512bitové přístupové klíče k úložišti, z nichž jeden slouží k ověřování, když zařízení StorSimple hello přistupuje k účtu úložiště hello. Všimněte si, že pouze jeden z těchto klíčů se používá. Hello Další klíč je uchovávat v rezervy, umožní vám toorotate hello klíče pravidelně. toorotate klíče, provedete hello sekundární klíč aktivní a pak odstranit hello primární klíč. Potom můžete vytvořit nový klíč pro použití při další otočení hello. (Z bezpečnostních důvodů vyžadují mnoho datových centrech střídání klíčů.)

Doporučujeme, postupujte podle těchto osvědčené postupy pro střídání klíčů:

* Klíče účtu úložiště pravidelně toohelp Ujistěte se, že váš účet úložiště není přístup neoprávnění uživatelé by měl otočit.
* Správce Azure pravidelně by měla změnit nebo znovu vygenerovat primární nebo sekundární klíč hello pomocí hello úložiště v tématu hello účet úložiště hello Azure portálu toodirectly přístup.

## <a name="protect-data-via-encryption"></a>Chránit data pomocí šifrování

StorSimple používá hello následující šifrovací algoritmy, které tooprotect data uložená v nebo cestě mezi součástmi hello vašeho řešení StorSimple.

| Algoritmus | Délka klíče | Protokoly/aplikace/komentáře |
| --- | --- | --- |
| RSA |2 048 |Hello Azure portálu tooencrypt konfigurační data, která je odeslána toohello zařízení používá v1.5 RSA PKCS č. 1: například přihlašovací údaje, konfigurace zařízení StorSimple, účtu úložiště a cloudové úložiště šifrovacích klíčů. |
| AES |256 |AES s CBC je použité tooencrypt hello veřejnou část šifrovacího klíče dat služby hello před odesláním toohello portálu Azure ze zařízení StorSimple hello. Ji používá také data tooencrypt zařízení StorSimple hello před odesláním dat hello toohello účet cloudového úložiště. |

## <a name="storsimple-cloud-appliance-security"></a>Zabezpečení cloudu zařízení StorSimple

[!INCLUDE [storsimple Cloud Appliance security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Nejčastější dotazy

Hello Následují některé otázky a odpovědi týkající se zabezpečení a Microsoft Azure StorSimple.

**Otázka:** mé služby dojde k ohrožení bezpečnosti. Moje další kroky, které mají být?

**Odpověď:** by měl hned změnit hello šifrovacího klíče dat služby a klíče účtu úložiště hello hello účtu úložiště, který se používá pro vrstvení data. Pokyny najdete v tématu:

* [Změna šifrovacího klíče dat služby hello](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
* [Střídání klíče účtů úložiště](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Otázka:** mám nové zařízení StorSimple, které se žádostí o registrační klíč služby hello. Jak jej lze vyhledat?

**Odpověď:** tento klíč byl vytvořen při prvním vytvoření služby StorSimple Manager zařízení hello. Pokud použijete hello Správce zařízení StorSimple služby tooconnect toohello zařízení, můžete použít hello služby úvodní stránky tooview nebo registrační klíč služby znovu generovat hello. Generování nový registrační klíč služby nebude mít vliv na stávající registrovaná zařízení hello. Pokyny najdete v tématu:

* [Zobrazení nebo znovu vygenerovat registrační klíč služby hello](storsimple-8000-manage-service.md##regenerate-the-service-registration-key)

**Otázka:** ztrátou Moje šifrovacího klíče dat služby. Co mám udělat?

**Odpověď:** obraťte se na podporu společnosti Microsoft. Uživatel může přihlásit tooa podporu relace na zařízení a nápovědy načíst klíč hello (za předpokladu, že je online alespoň jedno zařízení). Okamžitě po získání šifrovacího klíče dat služby hello, měli byste ji změnit tooensure tento nový klíč hello se označuje pouze tooyou. Pokyny najdete v tématu:

* [Změna šifrovacího klíče dat služby hello](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Otázka:** I oprávnění zařízení pro změnu klíče šifrování dat služby, ale proces změny klíče hello se nespustila. Co bych měl/a dělat?

**Odpověď:** Pokud vypršela platnost hello vypršení časového limitu, budete potřebovat tooreauthorize hello zařízení pro změnu hello do služby data šifrování klíče a znovu spustit proces hello.

**Otázka:** po změně hello šifrovacího klíče dat služby, ale I nemohl tooupdate hello jiná zařízení do 4 hodin. Je nutné provést toostart znovu?

**Odpověď:** hello 4 hodin časové období se pouze za inicializaci hello změnu. Po spuštění procesu aktualizace hello na hello autorizovaný zařízení StorSimple, hello autorizace je platný, dokud nejsou aktualizovány všechna zařízení.

**Otázka:** naše StorSimple správce opustil společnost hello. Co bych měl/a dělat?

**Odpověď:** změny a resetování hesel, která umožňují přístup k zařízení StorSimple toohello a změňte hello služby dat šifrování klíče tooensure, informace o novém hello nezná toounauthorized pracovníky hello. Pokyny najdete v tématu:

* [Použít toochange služby StorSimple Manager zařízení hello hesla služby storsimple](storsimple-8000-change-passwords.md)
* [Změna šifrovacího klíče dat služby hello](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
* [Konfigurace protokolů CHAP pro vaše zařízení StorSimple](storsimple-8000-configure-chap.md)

**Otázka:** chci tooprovide hello StorSimple Snapshot Manager heslo tooa hostitele, který se připojuje toohello zařízení StorSimple, ale heslo hello není k dispozici. Co můžete dělat?

**Odpověď:** Pokud jste zapomněli heslo hello, měli byste vytvořit nový. Potom Ujistěte se, že tooinform všichni stávající uživatelé, kteří hello heslo se změnil a, že by měl aktualizovat jejich toouse klienti hello nové heslo. Pokyny najdete v tématu:

* [Změnit heslo Snapshot Manager zařízení StorSimple hello](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password)
* [Ověřování zařízení](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Otázka:** hello certifikát pro vzdálený přístup toohello prostředí Windows PowerShell pro zařízení StorSimple se změnil na hello zařízení. Jak aktualizovat Moje klienty vzdáleného přístupu?

**Odpověď:** můžete stáhnout hello nový certifikát z hello služby StorSimple Manager zařízení a pak ho zadat toobe nainstalovaný v úložišti certifikátů hello klienty vzdáleného přístupu. Pokyny najdete v tématu:

* [Rutina Import certifikátu](https://technet.microsoft.com/library/hh848630.aspx)

**Otázka:** je můj data chráněná, pokud dojde k ohrožení bezpečnosti hello služby StorSimple Manager zařízení?

**Odpověď:** konfiguračních dat služby je vždy šifrované svým veřejným klíčem, při zobrazení ve webovém prohlížeči. Protože služba hello nemá privátní klíč toohello přístup, bude hello služby není možné toosee se žádná data. Pokud dojde k narušení hello služby StorSimple Manager zařízení, neexistuje žádný vliv, protože neexistují žádné klíče uložené v hello služby StorSimple Manager zařízení.

**Otázka:** Pokud někdo získá přístup toohello data šifrovací certifikát, budou data dojít k ohrožení?

**Odpověď:** Microsoft Azure ukládá hello zákazníka datový šifrovací klíč (soubor .pfx) v zašifrovaném formátu. Protože je šifrovaný soubor .pfx hello a hello služby StorSimple nemá hello služby dat šifrování klíče toodecrypt hello soubor .pfx, jednoduše získávání soubor .pfx pro přístup k toohello nebude vystavit všech tajných klíčů.

**Otázka:** co se stane, když vládních entity Microsoft požádá o svá data?

**Odpověď:** vzhledem k tomu, že všechna hello data se šifrují hello služby a hello privátní klíč se uchovává se zařízení hello hello vládních entity musí zeptat hello zákazníka hello data.

## <a name="next-steps"></a>Další kroky

[Nasazení zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).

