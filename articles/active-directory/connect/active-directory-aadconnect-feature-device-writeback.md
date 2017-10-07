---
title: "Azure AD Connect: Povolení zpětného zápisu zařízení | Microsoft Docs"
description: "Tato dokumentu podrobnosti o tom, jak pomocí Azure AD Connect tooenable zpětný zápis zařízení."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2566a514137fed85b21929207cf3230e6878ebbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Povolení zpětného zápisu zařízení
> [!NOTE]
> Předplatné tooAzure AD Premium je vyžadován pro zpětný zápis zařízení.
> 
> 

Hello následující dokumentace obsahuje informace o tom, jak funkce zpětný zápis zařízení hello tooenable v Azure AD Connect. Zpětný zápis zařízení se používá v hello následující scénáře:

* Povolení podmíněného přístupu podle zařízení tooADFS (2012 R2 nebo vyšší) chráněné aplikace (vztahy důvěryhodnosti předávající strany).

To poskytuje dodatečné zabezpečení a záruku toho, že přístup k tooapplications je povolen pouze tootrusted zařízení. Další informace o podmíněného přístupu najdete v tématu [řízením rizik pomocí podmíněného přístupu](../active-directory-conditional-access.md) a [nastavení místního podmíněného přístupu pomocí Azure Active Directory Device Registration](../active-directory-conditional-access-automatic-device-registration-setup.md).

> [!IMPORTANT]
> <li>Zařízení musí být umístěný ve stejné doménové struktuře jako uživatelé hello hello. Vzhledem k tomu, že zařízení, musí být napsaná zpět tooa jedné doménové struktury, tato funkce nepodporuje aktuálně nasazení s více doménovými strukturami uživatele.</li>
> <li>Objekt konfigurace pouze jedno zařízení registrace lze přidat toohello místní služby Active Directory doménové struktury. Tato funkce není kompatibilní s topologií kde hello místního adresáře služby Azure AD synchronizované toomultiple je služba Active Directory.</li>> 

## <a name="part-1-install-azure-ad-connect"></a>Část 1: Instalace služby Azure AD Connect
1. Nainstalujte Azure AD Connect s použitím vlastní nebo expresní nastavení. Společnost Microsoft doporučuje toostart s všichni uživatelé a skupiny bylo úspěšně synchronizováno před povolením zpětný zápis zařízení.

## <a name="part-2-prepare-active-directory"></a>Část 2: Příprava služby Active Directory
Použijte následující kroky tooprepare pro používání zpětný zápis zařízení hello.

1. Z počítače hello nainstalovanou Azure AD Connect spusťte prostředí PowerShell v režimu zvýšených oprávnění.
2. Pokud modul Active Directory PowerShell hello nainstalovaná není, nainstalujte ji pomocí hello následující příkaz:
   
   `Add-WindowsFeature RSAT-AD-PowerShell`
3. Pokud modul Azure Active Directory PowerShell hello nainstalovaná není, pak stáhněte a nainstalujte ji z [Azure Active Directory modul pro prostředí Windows PowerShell (64bitová verze)](http://go.microsoft.com/fwlink/p/?linkid=236297). Tato součást obsahuje závislost na hello Pomocníka pro přihlášení, která se instaluje s Azure AD Connect.
4. Pomocí přihlašovacích údajů správce podniku spusťte následující příkazy hello a poté ukončete prostředí PowerShell.
   
   `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`
   
   `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Vzhledem k tomu, že jsou potřebné změny toohello konfigurace oboru názvů jsou vyžadována pověření správce podniku. Správce domény, nebude mít dostatečná oprávnění.

![Prostředí PowerShell pro povolení zpětného zápisu zařízení](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Popis:

* Pokud by již neexistuje, vytvoří a nakonfiguruje nové kontejnery a objekty v rámci CN = Device Registration Configuration, CN = Services, CN = Configuration, [doménové struktury rozlišující název].
* Pokud by již neexistuje, vytvoří a nakonfiguruje nové kontejnery a objekty v rámci CN = RegisteredDevices, [rozlišující název domény]. Objekty zařízení budou vytvořeny v tomto kontejneru.
* Nastaví potřebná oprávnění hello účet konektoru služby Azure AD, toomanage zařízení na službě Active Directory.
* Stačí toorun na jednu doménovou strukturu, i v případě, že Azure AD Connect instalujete na několik doménových struktur.

Parametry:

* DomainName: Doména služby Active Directory kde bude vytvořen objekty zařízení. Poznámka: Všechna zařízení v dané doménové struktuře služby Active Directory bude vytvořen v jedné doméně.
* AdConnectorAccount: Účet služby Active Directory, který se použije v Azure AD Connect toomanage objekty v adresáři hello. Toto je hello účet používaný službou Azure AD Connect sync tooconnect tooAD. Pokud jste nainstalovali pomocí expresního nastavení, je účet hello MSOL_ s předponou.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Část 3: Zpětný zápis zařízení povolit ve službě Azure AD Connect
Použijte následující postup zpětný zápis zařízení tooenable v Azure AD Connect hello.

1. Znovu spusťte Průvodce instalací hello. Vyberte **přizpůsobit možnosti synchronizace** z hello další úkoly a klikněte na tlačítko **Další**.
   ![Vlastní instalace přizpůsobit možnosti synchronizace](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2. Na stránce volitelné funkce hello zpětný zápis zařízení už k dispozici. Upozorňujeme, že pokud hello nebyly dokončeny přípravný kroky Azure AD Connect zpětný zápis zařízení k dispozici odhlašování stránku hello volitelné funkce. Zaškrtněte políčko hello pro zpětný zápis zařízení a klikněte na tlačítko **Další**. Pokud políčko hello je stále zakázaná, najdete v části hello [řešení potíží v](#the-writeback-checkbox-is-still-disabled).
   ![Vlastní instalace volitelných funkcí zpětný zápis zařízení.](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3. Na stránce hello zpětný zápis uvidíte hello zadané domény jako hello výchozí zařízení zpětný zápis doménové struktury.
   ![Vlastní instalace zařízení zpětný zápis cílové doménové struktuře](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4. Dokončení instalace hello hello průvodce bez změny další konfigurace. V případě potřeby odkazovat příliš[vlastní instalace Azure AD Connect.](active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Povolení podmíněného přístupu
Tooenable podrobné pokyny jsou k dispozici v tomto scénáři [nastavení místního podmíněného přístupu pomocí Azure Active Directory Device Registration](../active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="verify-devices-are-synchronized-tooactive-directory"></a>Ověřte, zda zařízení synchronizované tooActive adresáře
Zpětný zápis zařízení by měl nyní pracuje správně. Upozorňujeme, že může trvat až too3 hodin pro zařízení objekty toobe zapsány zpět tooAD.  zařízení jsou se správně, synchronizovaný tooverify hello následující po dokončení synchronizace pravidla hello:

1. Spusťte Centrum správy služby Active Directory.
2. Rozbalte RegisteredDevices, v rámci domény, který je právě Federovaná hello.
   ![Centrum správy služby Active Directory zaregistrované zařízení](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3. Aktuální registrovaná zařízení se objeví existuje.
   ![Centrum správy služby Active Directory zaregistrován seznam zařízení](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Řešení potíží
### <a name="hello-writeback-checkbox-is-still-disabled"></a>zpětný zápis Hello zaškrtávací políčko je stále zakázáno.
Pokud políčko hello pro zpětný zápis zařízení není povolena, i když jste postupovali podle kroků hello výše hello následující kroky vám pomohou prostřednictvím jaké hello instalace Průvodce ověřuje předtím, než je povoleno hello pole.

Nejdůležitější první:

* Ujistěte se, že má aspoň jednu doménovou strukturu Windows Server 2012 R2. Typ objektu Hello zařízení musí být přítomen.
* Pokud Průvodce instalací hello je již spuštěna, nebudou zjištěna žádné změny. V takovém případě dokončete Průvodce instalací hello a potom ho spusťte znovu.
* Ujistěte se, že účet hello, které jsou uvedeny v skript inicializace hello je ve skutečnosti hello správné uživatelské používané hello konektor služby Active Directory. tooverify, postupujte takto:
  * Z nabídky start hello, otevřete **synchronizační služba**.
  * Otevřete hello **konektory** kartě.
  * Najde hello konektor s typem Active Directory Domain Services a vyberte jej.
  * V části **akce**, vyberte **vlastnosti**.
  * Přejděte příliš**připojit doménové struktury Directory tooActive**. Ověřte, že hello doména a uživatelské jméno zadané na této obrazovce shodu hello zadaný účet toohello skriptu.
    ![Účet konektoru ve Správci služby synchronizace](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Ověření konfigurace ve službě Active Directory:

* Ověřte, že hello Device Registration Service se nachází v umístění hello níže (CN DeviceRegistrationService, CN = = služby registrace zařízení, CN = Device Registration Configuration, CN = Services, CN = Configuration) v názvovém kontextu konfigurace.

![Řešení potíží s DeviceRegistrationService v oboru názvů configuration](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

* Ověřte, že existuje jenom jeden objekt konfigurace tak, že hello konfigurace oboru názvů. Pokud je více než jedna, odstraňte duplicitní hello.

![Řešení potíží, vyhledejte hello duplicitní objekty](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

* Na objekt hello Device Registration Service Přesvědčte se, zda hello atribut msDS-DeviceLocation je k dispozici a má hodnotu. Vyhledávání toto umístění a ujistěte se, že je k dispozici s hello objectType msDS-DeviceContainer.

![Řešení potíží s msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Řešení potíží s RegisteredDevices objektu třídy](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

* Ověřte hello účet používaný službou hello, že konektor služby Active Directory má požadovaná oprávnění na hello registrovaná zařízení kontejner nalezený v předchozím kroku hello. Toto je očekávaný hello oprávnění pro tento kontejner:

![Řešení potíží, ověřte oprávnění u kontejneru](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

* Ověřte hello účet služby Active Directory má oprávnění pro hello CN = Device Registration Configuration, CN = Services, CN = objekt konfigurace.

![Řešení potíží, ověřte oprávnění u konfigurace registrace zařízení](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Další informace
* [Řízení rizik pomocí podmíněného přístupu](../active-directory-conditional-access.md)
* [Nastavení místního podmíněného přístupu pomocí Azure Active Directory Device Registration](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

