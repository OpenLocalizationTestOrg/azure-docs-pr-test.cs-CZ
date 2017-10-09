---
title: "aaaAzure Active Directory Connect stavu časté otázky – Azure | Microsoft Docs"
description: "Tyto nejčastější dotazy odpovědi na otázky o Azure AD Connect Health. Tyto nejčastější dotazy obsahuje otázky týkající se používání hello služby, včetně hello fakturační model, schopností, omezení a podpory."
services: active-directory
documentationcenter: 
author: billmath
manager: samueld
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 3f15d9e9b557b09ef74ceff85806579dd51f2412
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-health-frequently-asked-questions"></a>Nejčastější dotazy ke službě Azure AD Connect Health
Tento článek obsahuje odpovědi toofrequently kladené dotazy (FAQ) o stavu připojení služby Azure Active Directory (Azure AD). Tyto nejčastější dotazy se týkají dotazy o tom, jak toouse hello service, která zahrnuje hello fakturační model, schopností, omezení a podpory.

## <a name="general-questions"></a>Obecné otázky
**Otázka: lze spravovat několik adresářů Azure AD. Jak lze přepnout toohello, ten, který má Azure Active Directory Premium?**

tooswitch mezi různými klienty Azure AD, vyberte hello aktuálně přihlášeného **uživatelské jméno** na hello pravém horním rohu a potom vyberte odpovídající účet hello. Pokud zde není uveden hello účtu, vyberte **Odhlásit se**, a pak použijte přihlašovací údaje globálního správce hello hello adresáře, který má Azure Active Directory Premium povolené toosign v.

**Otázka: jaký verzi role identity podporuje Azure AD Connect Health?**

Hello následující tabulka obsahuje seznam rolí hello a podporované verze operačního systému.

|Role| Operační systém nebo verzi|
|--|--|
|AD FS (Active Directory Federation Services)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | Verze 1.0.9125 nebo vyšší|
|Active Directory Domain Services (AD DS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

Všimněte si, že hello funkce poskytované službou hello se můžou lišit podle hello role a hello operačního systému. Jinými slovy všechny funkce hello nemusí být dostupné pro všechny verze operačního systému. Najdete popis funkcí hello podrobnosti.

**Otázka: jak velký počet licencí provést potřebuji toomonitor Moje infrastruktury?**

* Hello první Agent Connect Health vyžaduje alespoň jednu licenci Azure AD Premium.
* Každý další registrované agent vyžaduje 25 další licence Azure AD Premium.
* Počet agentů je ekvivalentní toohello celkový počet agentů, které jsou zaregistrovány v rámci všech monitorovaných rolí (služby AD FS, Azure AD Connect a služby AD DS).

Informace o licencích také nachází na hello [stránky Azure AD cenová](https://aka.ms/aadpricing).

Příklad:

| Registrovaný agentů | Licence potřeby | Příklad konfigurace monitorování |
| ------ | --------------- | --- |
| 1 | 1 | 1 server azure AD Connect |
| 2 | 26| 1 server azure AD Connect a řadič domény 1 |
| 3 | 51 | 1 server active Directory Federation Services (AD FS), proxy 1 AD FS a řadič domény 1 |
| 4 | 76 | Server služby 1 AD FS, proxy 1 AD FS a 2 řadiče domény |
| 5 | 101 | 1 server azure AD Connect, server 1 AD FS, proxy 1 AD FS a 2 řadiče domény |


## <a name="installation-questions"></a>Instalace otázky

**Otázka: co je hello dopad instalace hello agenta Azure AD Connect Health na jednotlivých serverech?**

dopad Hello instalace proxy servery webových aplikací hello Microsoft Azure agenta AD Connect Health, služby AD FS, řadiče domény, servery Azure AD Connect (sync) je minimální s ohledem toohello procesoru, využití paměti, šířky pásma sítě a úložiště.

následující čísla Hello jsou sblížení:

* Spotřeba CPU: zvýšení ~ 1-5 %.
* Využití paměti: % too10 hello celkový systémové paměti.

> [!NOTE]
> Pokud hello agent nemůže komunikovat s Azure, hello agent ukládá hello data místně pro definovaný maximální limit. Hello agent přepíše hello "v mezipaměti" data na základě "nedávno nejméně servis".
>
>

* Vyrovnávací paměť místní úložiště pro agenty Azure AD Connect Health: ~ 20 MB.
* Pro servery služby AD FS doporučujeme, že zřizovat místa 1 024 MB (1 GB) pro kanál auditu hello služby AD FS pro agenty Azure AD Connect Health tooprocess všechna data auditu hello předtím, než se přepíše.

**Otázka: může dojít k tooreboot Moje servery během instalace hello agenty hello Azure AD Connect Health?**

Ne. instalace Hello hello agentů nebude vyžadovat jste tooreboot hello serveru. Instalace některé z požadovaných předchozích kroků však může vyžadovat restartování serveru hello.

Například na Windows Server 2008 R2, vyžaduje instalaci rozhraní .NET Framework 4.5 restartování serveru.

**Otázka: funguje Azure AD Connect Health prostřednictvím průchozí proxy serveru HTTP?**

Ano. Pro probíhající operace můžete nakonfigurovat agenta stavu toouse hello HTTP proxy tooforward odchozí požadavky HTTP.
Další informace o [konfiguraci proxy serveru HTTP pro agenty stavu](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy).

Pokud potřebujete tooconfigure proxy server při registraci agenta, bude pravděpodobně nutné toomodify vaše nastavení proxy serveru Internet Explorer předem.

1. Otevřete Internet Explorer > **nastavení** > **Možnosti Internetu** > **připojení** > **nastavení místní sítě**.
2. Vyberte **použít Proxy Server pro síť LAN**.
3. Vyberte **Upřesnit** Pokud máte jiný proxy portů pro protokol HTTP a HTTPS/Secure.

**Otázka: podporuje základní ověřování Azure AD Connect Health podporu při připojování tooHTTP proxy?**

Ne. Mechanismus toospecify libovolné uživatelské jméno a heslo pro základní ověřování není aktuálně podporován.

**Otázka: co dělat porty brány firewall pro připojení agenta stavu toowork hello Azure AD potřebuji tooopen?**

V tématu hello [v části požadavky na](active-directory-aadconnect-health-agent-install.md#requirements) hello seznam porty brány firewall a další požadavky na připojení.

**Otázka: Proč vidí dva servery s hello stejný název v portálu hello Azure AD Connect Health?**

Když odeberete agenta ze serveru, hello server automaticky se neodebere z portálu Azure AD Connect Health hello. Pokud ručně odebrat agenta ze serveru nebo odeberte samotný server hello, je třeba položku serveru hello toomanually odstranit z portálu hello Azure AD Connect Health.

Můžete obnovit z Image serveru a vytvořit nový server s hello stejné údaje (například název počítače). Pokud už zaregistrovaný server hello není odebrat z portálu hello Azure AD Connect Health a jste nainstalovali agenta hello na nový server hello, může se zobrazit dvě položky s hello stejný název.

V takovém případě odstraňte ručně hello položku, která patří toohello starším serveru. Hello dat pro tento server by měl být aktuální.

## <a name="health-agent-registration-and-data-freshness"></a>Registrace a data aktuálnosti agenta stavu

**Otázka: co jsou běžné příčiny chyb registrace agenta stavu hello a jak odstranit problémy?**

Hello agenta stavu může selhat z důvodu následující toohello tooregister možné důvody:

* Hello agent nemůže komunikovat s hello požadované koncové body, protože brána firewall neblokuje přenosy. To je zvlášť běžné na proxy servery webových aplikací. Ujistěte se, že se povolí odchozí komunikaci toohello požadované koncové body a porty. V tématu hello [v části požadavky na](active-directory-aadconnect-health-agent-install.md#requirements) podrobnosti.
* Odchozí komunikace je provedeno tooan kontrolu SSL vrstvou hello sítě. To způsobí, že certifikát hello hello agent používá toobe nahrazuje hello kontroly serveru nebo entity, a hello kroky toocomplete hello agenta registrace nezdaří.
* Hello uživatel nemá přístup k tooperform hello registrace agenta hello. Globální Správci mají přístup ve výchozím nastavení. Můžete použít [řízení přístupu na základě Role](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) toodelegate přístup tooother uživatele.

**Otázka: I mě získávání upozornění, že "data služby stavu není až toodate." Jak odstranit hello problém?**

Azure AD Connect Health vygeneruje výstrahu hello, pokud neobdrží všechny datové body hello ze serveru hello v hello poslední dvě hodiny. Může být několik důvodů pro tuto výstrahu.

* Hello agent nemůže komunikovat s hello požadované koncové body, protože brána firewall neblokuje přenosy. To je zvlášť běžné na proxy servery webových aplikací. Ujistěte se, že se povolí odchozí komunikaci toohello požadované koncové body a porty. V tématu hello [v části požadavky na](active-directory-aadconnect-health-agent-install.md#requirements) podrobnosti.
* Odchozí komunikace je provedeno tooan kontrolu SSL vrstvou hello sítě. To způsobí, že hello certifikátu, hello agent používá toobe nahrazuje hello kontroly serveru nebo entity a hello dojde k selhání tooupload data toohello Azure AD Connect Health service.
* Můžete použít příkaz připojení hello součástí hello agenta. [Přečtěte si další informace](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service).
* agenti Hello také podporují odchozí připojení přes neověřené server Proxy protokolu HTTP. [Přečtěte si další informace](active-directory-aadconnect-health-agent-install.md##configure-azure-ad-connect-health-agents-to-use-http-proxy).

## <a name="operations-questions"></a>Operace otázky
**Otázka: je nutné tooenable auditování na proxy servery webových aplikací hello?**

Ne, auditování nemusí toobe zapnuta hello proxy servery webových aplikací.

**Otázka: Jak získat Azure AD Connect Health výstrahy vyřešit?**

Azure AD Connect Health výstrahy získat přeložit na podmínka pro úspěch. Agenty Azure AD Connect Health zjistit a pravidelně zprávy hello úspěch podmínky toohello služby. Pro několik výstrah potlačení hello je založené na čase. Jinými slovy Pokud hello stejné chybový stav není v rámci 72 hodin od generování výstrah, hello výstraha je automaticky vyřešena.

**Otázka: I mě získávání upozornění, že "žádosti o testovací ověření (syntetické transakci) se nezdařilo tooobtain token." Jak odstranit hello problém?**

Azure AD Connect Health pro službu AD FS generuje tuto výstrahu v případě hello Agent stavu, které jsou nainstalovány na serveru služby AD FS chyby tooobtain token jako součást syntetické transakce iniciovaná hello agenta stavu. agent stavu Hello používá hello kontextu místního systému a pokusů o zadání tooget token pro vlastní předávající strany. Toto je tooensure catch-all test, který služby AD FS je ve stavu vystavování tokenů.

Nejčastěji tento test se nezdaří, protože hello stavu agenta je název farmy služby nemůže tooresolve hello služby AD FS. To může dojít, pokud jsou servery hello AD FS za nástroje pro vyrovnávání zatížení sítě a požadavek hello získá inicioval z uzlu, který je za nástroj pro vyrovnávání zatížení hello (jako názvem na rozdíl od tooa regulární klient, který je před hello nástroj pro vyrovnávání zatížení). To může být opraven aktualizací soubor hello "hosts" umístěný v části "C:\Windows\System32\drivers\etc" tooinclude hello IP adresa serveru hello služby AD FS nebo IP adresou zpětné smyčky (127.0.0.1) pro název farmy služby AD FS hello (např. sts.contoso.com). Přidání souboru hostitele hello bude krátká smyčka hello síťové volání, což umožňuje hello agenta stavu tooget hello token.

**Otázka: chybové e-mailu, což značí, že mých počítačů není nainstalována oprava hello poslední ransomeware útokům. Proč zobrazila tato e-mailu?**

Služba Azure AD Connect Health zkontrolovat všechny hello, které byly nainstalovány počítače sleduje tooensure hello požadované opravy. e-mailu Hello byla odeslána správci klientů toohello, pokud alespoň jeden počítač neměl hello důležité opravy. Hello následující logiku byl použité toomake toto rozhodnutí.
1. Najít všechny opravy hotfix hello nainstalovat na počítač hello.
2. Zkontrolujte, zda alespoň jeden z hello opravy hotfix z hello definovaný seznam není k dispozici.
3. Pokud ano, počítač hello je chráněný. Pokud ne, hello počítač jsou v ohrožení hello útoku.

Můžete použít následující tooperform skript prostředí PowerShell hello tato kontrola ručně. Implementuje hello výše logiku.

```
Function CheckForMS17-010 ()
{
    $hotfixes = "KB3205409", "KB3210720", "KB3210721", "KB3212646", "KB3213986", "KB4012212", "KB4012213", "KB4012214", "KB4012215", "KB4012216", "KB4012217", "KB4012218", "KB4012220", "KB4012598", "KB4012606", "KB4013198", "KB4013389", "KB4013429", "KB4015217", "KB4015438", "KB4015546", "KB4015547", "KB4015548", "KB4015549", "KB4015550", "KB4015551", "KB4015552", "KB4015553", "KB4015554", "KB4016635", "KB4019213", "KB4019214", "KB4019215", "KB4019216", "KB4019263", "KB4019264", "KB4019472", "KB4015221", "KB4019474", "KB4015219", "KB4019473"

    #checks hello computer it's run on if any of hello listed hotfixes are present
    $hotfix = Get-HotFix -ComputerName $env:computername | Where-Object {$hotfixes -contains $_.HotfixID} | Select-Object -property "HotFixID"

    #confirms whether hotfix is found or not
    if (Get-HotFix | Where-Object {$hotfixes -contains $_.HotfixID})
    {
        "Found HotFix: " + $hotfix.HotFixID
    } else {
        "Didn't Find HotFix"
    }
}

CheckForMS17-010

```



## <a name="related-links"></a>Související odkazy
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
