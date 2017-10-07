---
title: "aaaSettings a datový roaming – nejčastější dotazy | Microsoft Docs"
description: "Poskytuje odpovědi toosome otázky správci IT můžou mít o nastavení a synchronizaci dat aplikací."
services: active-directory
keywords: "Enterprise stavu nastavení roamingu windows cloudu, nejčastější dotazy na enterprise stavu roaming"
documentationcenter: 
author: tanning
manager: swadhwa
editor: curtand
ms.assetid: c0824f5c-129b-4240-969f-921f6a64eae7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 4d6d6619b3a5fbd1d274603808d89b73ed942cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="settings-and-data-roaming-faq"></a>Nejčastější dotazy k nastavení a datovému roamingu
Toto téma odpovědi na některé dotazy, které správci IT mohou mít o nastavení a synchronizaci dat aplikací.

## <a name="what-data-roams"></a>Jaká data přemístí?
**Nastavení systému Windows**: hello nastavení počítače, které jsou součástí operačního systému Windows hello. Obecně platí jsou to nastavení, které přizpůsobení počítače a patří mezi ně hello následující rozsáhlé kategorie:

* *Motiv*, což zahrnuje funkce, jako je nastavení plochy motiv a na hlavním panelu.
* *Nastavení aplikace Internet Explorer*, včetně posledních otevřených karet a Oblíbené položky.
* *Okraj nastavení prohlížeče*, jako jsou například oblíbených položek a čtení seznamu.
* *Hesla*, včetně Internetu hesla, profily Wi-Fi a další.
* *Jazykové předvolby*, což zahrnuje nastavení pro rozložení klávesnice, jazykem, datum a čas a další.
* *Snadné získat přístup k funkcím*, jako jsou například motiv s vysokým kontrastem a Předčítání, Lupa.
* *Další nastavení Windows*, jako jsou nastavení příkazového řádku a seznam aplikací.

**Data aplikací**: univerzální aplikace pro Windows může zapisovat tooa dat nastavení roamingu složky, a jakákoliv data zapsána toothis složky se budou automaticky synchronizovat. Je to toohello jednotlivých aplikací vývojáře toodesign tuto funkci využít tootake aplikace. Další informace o tom, jak toodevelop aplikace Universal Windows, který používá roaming, najdete v části hello [úložiště data aplikací API](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) a hello [data aplikací Windows 8 roaming blogu pro vývojáře](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Který účet se používá pro nastavení synchronizace?
V systému Windows 8 a Windows 8.1 nastavení synchronizace vždycky používají účty Microsoft příjemce. Uživatelé Enterprise měl hello možnost tooconnect tootheir účet Microsoft synchronizace toosettings přístup toogain účtu pro domény služby Active Directory. Ve Windows 10 toto připojení účtu Microsoft, že funkce je nahrazován framework primární a sekundární účet.

primární účet Hello je definován jako účet hello používá toosign v tooWindows. To může být účet Microsoft, účet služby Azure Active Directory (Azure AD), účet místní služby Active Directory nebo místní účet. Přidání toohello primární účet uživatelé Windows 10 můžete přidat jeden nebo více sekundárních cloud účty tootheir zařízení. Sekundární účet je obvykle účet Microsoft, účet Azure AD nebo jiný účet třeba z Gmailu nebo Facebook. Tyto účty sekundární poskytovat přístup tooadditional služby, jako je například jednotné přihlašování a hello Windows Store, ale nejsou schopná pohánějící nastavení synchronizace.

Ve Windows 10, lze použít pouze hello primární účet pro hello zařízení pro nastavení synchronizace (viz [jak je upgrade z Microsoft synchronizaci nastavení účtu v systému Windows 8 tooAzure AD nastavení synchronizace ve Windows 10?](active-directory-windows-enterprise-state-roaming-faqs.md#how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10)).

Data se nikdy kombinované mezi hello různé uživatelské účty na hello zařízení. Existují dvě pravidla pro synchronizaci nastavení:

* Nastavení systému Windows bude vždy spolu s primární účet hello.
* Data aplikací budou označené hello účet používaný tooacquire hello aplikace. Jenom aplikace, které jsou označené hello primární účet bude synchronizovat. Označování vlastnictví aplikace je určen, když je aplikace zkušebně načtených prostřednictvím úložiště Windows hello nebo Správa mobilních zařízení (MDM).

Pokud aplikace vlastníka nelze identifikovat, bude přenášet primární účtem hello. Pokud je zařízení upgradovali z Windows 8 nebo Windows 8.1 tooWindows 10, všechny aplikace hello označí jako získávají pomocí účtu Microsoft hello. Je to proto, že většina uživatelů získat aplikací prostřednictvím hello Windows Store a se žádná podpora pro Windows Store pro Azure AD účty předchozí tooWindows 10. Pokud je aplikace nainstalována licenci offline, aplikace hello označené pomocí hello primární účtu na zařízení hello.

> [!NOTE]
> Zařízení s Windows 10, které jsou ve vlastnictví společnosti a jsou připojené tooAzure AD již nemůže připojit svůj účet Microsoft tooa účty domény. Hello tooconnect možnost účet Microsoft tooa účet domény a mít všechny hello uživatelské synchronizaci dat z se odebere toohello účet Microsoft (tedy hello účtu Microsoft roaming prostřednictvím účtu Microsoft hello připojené a funkce služby Active Directory) Zařízení s Windows 10, které jsou připojené k tooa připojené služby Active Directory nebo Azure AD prostředí.
>
>

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-tooazure-ad-settings-sync-in-windows-10"></a>Jak upgradovat z Microsoft synchronizaci nastavení účtu v systému Windows 8 tooAzure AD nastavení synchronizace ve Windows 10?
Pokud jste toohello připojené k doméně služby Active Directory systémem Windows 8 nebo Windows 8.1 připojených účtem Microsoft, bude synchronizovat nastavení prostřednictvím účtu Microsoft. Po upgradu tooWindows 10, bude pokračovat toosync nastavení uživatele prostřednictvím účtu Microsoft, jak dlouho, dokud je uživatel připojený k doméně a domény služby Active Directory hello se nemůže spojit se Azure AD.

Pokud hello místní domény služby Active Directory připojit se s Azure AD, zařízení se pokusí nastavení toosync pomocí účtu Azure AD hello připojen. Pokud správce hello Azure AD není povolen Enterprise State Roaming, vaše připojené účet Azure AD se zastaví synchronizaci nastavení. Pokud jste uživatelem systému Windows 10 a přihlásit Azure AD identity, se spustí, jakmile správce umožní synchronizace nastavení přes Azure AD synchronizovat nastavení systému windows.

Pokud jste uložili všechna osobní data v podnikových zařízení, byste měli vědět, že operačního systému Windows a data aplikací bude zahájena synchronizace tooAzure AD. Tato akce nemá hello následující důsledky:

* Nastavení svého osobního účtu Microsoft se soubor kromě nastavení hello na váš pracovní nebo školní účty Azure AD. Důvodem je, že účet Microsoft hello a nastavení služby Azure AD synchronizovat nyní používáte samostatné účty.
* Osobní data, jako jsou hesla Wi-Fi, webové pověření a Oblíbené položky aplikace Internet Explorer, dříve synchronizované prostřednictvím připojeného účtu Microsoft se budou synchronizovat přes Azure AD.

## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Jak se účet Microsoft a pracovní vzájemná funkční spolupráce Azure AD Enterprise stavu Roaming?
V hello. listopadu 2015 nebo pozdějších verzích Windows 10 Enterprise State Roaming je podporována pouze pro jeden účet najednou. Pokud tooWindows přihlásit pomocí pracovního nebo školního účtu Azure AD, všechna data synchronizují přes Azure AD. Pokud tooWindows přihlásíte pomocí osobního účtu Microsoft, všechna data se budou synchronizovat prostřednictvím hello účtu Microsoft. Univerzální data aplikací bude přecházet pomocí pouze hello primární přihlášení účtu na zařízení hello a bude mohla používat pouze v případě, že primární účet hello vlastní licenci aplikace hello. Nebudou synchronizovat data univerzálních aplikací pro aplikace hello vlastníkem všechny sekundární účty.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Nastavení synchronizace pro účty Azure AD od víc klientů?
Když více Azure AD účty z různých klientů Azure AD jsou na hello stejného zařízení, musíte aktualizovat toocommunicate registru hello zařízení pomocí Azure Rights Management (Azure RMS) pro každý klient Azure AD.  

1. Najde hello identifikátor GUID pro každého klienta Azure AD. Otevřete hello portál Azure classic a vyberte klienta Azure AD. Hello identifikátor GUID pro klienta hello je v adrese URL hello hello adresního řádku prohlížeče. Příklad: `https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. Až budete mít hello identifikátor GUID, budete potřebovat klíč registru hello tooadd **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<klienta ID GUID >**.
   Z hello **klienta ID GUID** klíče, vytvořte novou víceřetězcovou hodnotu (REG SZ více) s názvem **AllowedRMSServerUrls**. Pro svá data zadejte hello licencování distribuční bod adresy URL hello jiných klientů Azure, které hello přístupů zařízení.
3. Můžete najít hello licencování adres URL distribučních bodů tak, že spustíte hello **Get-AadrmConfiguration** rutiny. Pokud hello hodnoty pro hello **LicensingIntranetDistributionPointUrl** a **LicensingExtranetDistributionPointUrl** se liší, zadat obě hodnoty. Pokud hello hodnoty jsou hello stejné, zadejte hodnotu hello pouze jednou.

## <a name="what-are-hello-roaming-settings-options-for-existing-windows-desktop-applications"></a>Jaké jsou možnosti hello cestovní nastavení pro existující aplikace pracovní plochy Windows?
Roaming funguje pouze pro univerzální aplikace pro Windows. K dispozici pro povolení roamingu na existující aplikaci plochy Windows existují dvě možnosti:

* Hello [plochy most](http://aka.ms/desktopbridge) umožňuje uvést vaše stávající toohello aplikace klasické pracovní plochy Windows univerzální platformu Windows. Z tohoto místa vyžaduje minimální kód, který změny budou výhod tootake roaming dat aplikací Azure AD. Hello most plocha poskytuje aplikace s identitou aplikace, které je potřeba tooenable aplikace datový roaming existující aplikací klasické pracovní plochy.
* [Virtualizace uživatelského prostředí (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) vám pomůže vytvořit šablonu vlastní nastavení pro existující aplikacích klasické pracovní plochy Windows a povolit roaming pro aplikace Win32. Tato možnost nevyžaduje hello aplikace vývojáře toochange kód aplikace hello. UE-V je omezená tooon místní službě Active Directory roamingu pro zákazníky, kteří si koupili hello Microsoft Desktop Optimization Pack.

Správci data aplikace na ploše Windows tooroam UE-V můžete nakonfigurovat tak, že změníte cestovní nastavení operačního systému Windows a univerzální aplikace pro data prostřednictvím [UE-V zásady skupiny](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), včetně:

* Mohla používat zásady skupiny nastavení systému Windows
* Nesynchronizovat zásad skupiny aplikací pro Windows
* Roaming v části hello aplikace Internet Explorer

V budoucích hello může společnost Microsoft prozkoumat toomake způsoby, které UE-V se úzce integruje do systému Windows a rozšířit nastavení tooroam UE-V prostřednictvím hello cloudové služby Azure AD.

## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Může ukládat synchronizovaná nastavení a data místně?
Enterprise State Roaming ukládá všechna synchronizovaná data v hello cloudu Azure. UE-V nabízí místní řešení roamingu.

## <a name="who-owns-hello-data-thats-being-roamed"></a>Kdo je vlastníkem hello data, která je právě roamované?
podniky Hello vlastní hello dat roamované prostřednictvím Enterprise State Roaming. Data jsou uložena v datovém centru Azure. Všechna data se šifrují během přenosu a uložená v cloudu hello pomocí služby Azure RMS. Toto je zlepšování porovnání tooMicrosoft nastavení založeného na účtu synchronizace, který šifruje jenom určité citlivých dat jako pověření uživatele před opuštěním hello zařízení.

Společnost Microsoft se potvrdí toosafeguarding zákaznická data. Uživatel s enterprise nastavení data se šifrují automaticky službou Azure RMS, než opustí zařízením s Windows 10, aby žádný jiný uživatel může číst tato data. Pokud má vaše organizace na placené předplatné pro Azure RMS, můžete použít jiné funkce Azure RMS, jako je například sledovat a odvolat dokumenty, automaticky chránit e-mailů, které obsahují citlivé informace a spravovat vlastní klíče (hello "přineste si vlastní klíč" řešení také označuje jako BYOK). Další informace o těchto funkcích a o tom, jak funguje Azure RMS najdete v tématu [co je Azure Rights Management](https://technet.microsoft.com/jj585026.aspx).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Můžete spravovat synchronizace pro konkrétní aplikaci nebo nastavení?
V systému Windows 10 je nastavení toodisable žádné MDM nebo zásady skupiny roamingu pro jednotlivé aplikace. Správci klienta můžete zakázat synchronizaci data aplikací pro všechny aplikace na spravované zařízení, ale neexistuje žádné jemnějšího ovládání na úrovni pro aplikaci nebo v rámci aplikace.

## <a name="how-can-i-enable-or-disable-roaming"></a>Jak můžete povolit nebo zakázat roaming?
V hello **nastavení** aplikace, přejděte příliš**účty** > **synchronizace nastavení**. Z této stránky se zobrazí, který účet se nastavení používané tooroam a můžete povolit nebo zakázat jednotlivé skupiny nastavení toobe roamované.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Co je doporučením společnosti Microsoft pro povolení roaming v systému Windows 10?
Microsoft má několik různých nastavení roamingu řešení, které jsou k dispozici, včetně cestovních profilů uživatelů, UE-V a Enterprise stavu roamingu.  Společnost Microsoft se potvrdí toomaking investice do Enterprise State Roaming v budoucích verzích systému Windows. Pokud vaše organizace není připraveno nebo možnost s toohello přesunutí dat v cloudu, pak doporučujeme použít UE-V jako primární roamingu technologie. Pokud vaše organizace vyžaduje roaming podporu pro existující aplikace pracovní plochy Windows, ale je přes toomove toohello cloudu, doporučujeme, abyste použili Enterprise stavu roamingu a UE-V. I když jsou velmi podobné technologie UE-V a Enterprise State Roaming, nejsou vzájemně vylučují. Doplňují vzájemně toohelp Ujistěte se, že vaše organizace poskytuje hello roaming služby, které uživatelé potřebují.  

Při použití Enterprise State Roaming a UE-V, platí následující pravidla hello:

* Enterprise State Roaming je primární roamingu agent hello na hello zařízení. UE-V se použité toosupplement hello "Win32 mezery."
* Cestovní nastavení systému Windows a moderní data aplikací UWP UE-V by mělo být zakázáno, když pomocí skupiny hello UE-V zásady. Tyto jsou již vztahuje Enterprise State Roaming.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Jak Enterprise State Roaming podporuje infrastruktury virtuálních klientských (počítačů VDI)?
Enterprise State Roaming je podporována v klientovi Windows 10 SKU, ale není na serveru SKU. Pokud klient virtuálního počítače je hostované na hypervisoru počítače a vzdáleně přihlaste toohello virtuálního počítače, bude přenášet data. Pokud se více uživateli sdílet hello stejné operačního systému a uživatelé vzdáleně přihlašují tooa server pro úplné desktopové prostředí, roaming nemusí fungovat. scénář Hello pozdější na bázi relace není oficiálně podporován.

## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Co se stane, když Moje organizace zakoupí Azure RMS po použití roamingu?
Pokud vaše organizace už používá roaming v systému Windows 10 s hello bezplatné předplatné Azure RMS omezeného použití, zakoupení placené předplatné Azure RMS nebude mít žádný vliv na funkci hello hello roamingu funkce a žádné změny konfigurace budou vyžaduje váš správce IT.

## <a name="known-issues"></a>Známé problémy
Naleznete v dokumentaci hello v hello [řešení potíží s](active-directory-windows-enterprise-state-roaming-troubleshooting.md) části Seznam známých problémů. 

## <a name="related-topics"></a>Související témata
* [Přehled roamingu stavu Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
* [Povolit stav enterprise roaming v Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
* [Nastavení MDM pro synchronizaci nastavení zásad skupiny a](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Odkaz nastavení roamingu Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Řešení potíží](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
