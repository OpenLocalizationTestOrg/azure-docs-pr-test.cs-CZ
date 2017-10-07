---
title: aaaAzure IoT Suite a Azure Active Directory | Microsoft Docs
description: "Popisuje, jak Azure IoT Suite využívá Azure Active Directory toomanage oprávnění."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: dobett
ms.openlocfilehash: 4768630f2de4bb431731fbd4e8929232bc80b9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-on-hello-azureiotsuitecom-site"></a>Oprávnění na webu azureiotsuite.com hello

## <a name="what-happens-when-you-sign-in"></a>Co se stane, když se přihlásíte

Hello poprvé, přihlaste se na [azureiotsuite.com][lnk-azureiotsuite], lokalita hello Určuje úrovně oprávnění hello založené na hello aktuálně vybrané klienta Azure Active Directory (AAD) a Azure předplatné.

1. Nejprve seznamu toopopulate hello klientům vidět další uživatelského jména tooyour hello lokality zjistí z Azure které AAD klienty patří do. V současné době hello lokality může obsahovat pouze tokeny uživatele pro jednoho klienta v čase. Proto při přepnutí klientů pomocí rozevíracího seznamu hello v pravém horním rohu hello hello lokality přihlásí jste toothat klienta tooobtain hello tokeny pro tohoto klienta.

2. V dalším kroku hello lokality zjistí z Azure, které odběry přidružený hello vybrané klienta. Při vytváření nové předkonfigurované řešení zobrazit hello dostupných předplatných.

3. Nakonec hello lokality načte všechny zdroje hello v hello předplatných a skupin prostředků označené jako předkonfigurovaná řešení a naplní hello dlaždice na domovskou stránku hello.

Hello následující oddíly popisují hello rolí, které řídí přístup toohello předkonfigurované řešení.

## <a name="aad-roles"></a>Role AAD

role AAD Hello hello možnost zřídit předkonfigurované řešení řídit a spravovat uživatele v předkonfigurované řešení.

Další informace o rolích správce můžete najít v AAD v [přiřazení rolí správce ve službě Azure AD][lnk-aad-admin]. Hello aktuální článek zaměřuje na hello **globálního správce** a hello **uživatele** role directory používaného hello předkonfigurovaných řešení.

### <a name="global-administrator"></a>Globální správce

Může být mnoho globální správci za klienta AAD:

* Při vytváření klienta služby AAD jste výchozí hello globální správce tohoto klienta.
* Globální správce Hello můžete zřídit předkonfigurované řešení a je mu přiřazená **správce** role pro aplikaci hello uvnitř jejich klienta AAD.
* Pokud jiný uživatel v hello stejné vytvoří klienta AAD aplikace, hello výchozí role uděleno toohello globální správce je **jen pro čtení**.
* Globální správce můžete přiřadit tooroles uživatelů pro aplikace pomocí hello [portál Azure][lnk-portal].

### <a name="domain-user"></a>Uživatel domény

Může být velký počet uživatelů domény na klienta AAD:

* Uživatel domény můžete zřídit předkonfigurované řešení prostřednictvím hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality. Ve výchozím nastavení, hello domény uživateli je udělen hello **správce** role v hello zřízený aplikace.
* Uživatel domény může vytvořit aplikaci pomocí skriptu build.cmd hello v hello [azure-iot-remote-monitoring][lnk-rm-github-repo], [azure-iot-predictive-maintenance] [ lnk-pm-github-repo], nebo [azure-iot připojené factory] [ lnk-cf-github-repo] úložiště. Ale udělena hello výchozí role uživatele domény toohello je **jen pro čtení**, protože uživatel domény nemá oprávnění tooassign role.
* Pokud jiný uživatel v tenantovi AAD hello vytvoří aplikace, uživatele domény hello je přiřazen hello **jen pro čtení** roli ve výchozím nastavení pro tuto aplikaci.
* Uživatel domény nelze přiřadit role pro aplikace; proto nelze uživatele domény přidat, uživatelé nebo role pro uživatele pro aplikaci i v případě, že se jeho zřízení.

### <a name="guest-user"></a>Uživatele Guest

Může být velký počet uživatelů typu Host za klienta AAD. Uživatele typu Host mají omezenou sadu oprávnění v tenantovi AAD hello. V důsledku toho uživatele typu Host nejde zřídit předkonfigurované řešení v tenantovi AAD hello.

Další informace týkající se uživatelů a rolí v AAD najdete v tématu hello následující prostředky:

* [Vytvořte uživatele ve službě Azure AD][lnk-create-edit-users]
* [Přiřazení uživatelů tooapps][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Role správce předplatného Azure

role správce Azure Hello řídit hello možnost toomap tooan AD klienta služby předplatného Azure.

Další informace o rolích správce Azure hello v článku hello [jak tooadd nebo změňte Azure Spolusprávcem, Správce služeb a správce účtu][lnk-admin-roles].

## <a name="application-roles"></a>Aplikační role

Hello aplikační role řízení přístupu toodevices v předkonfigurovaném řešení.

Existují dvě definované role a jedna implicitní role, které jsou definované v zřízené aplikaci:

* **Správce:** má úplné řízení tooadd, správu, odeberte zařízení a upravit nastavení.
* **Jen pro čtení:** můžete zobrazit zařízení, pravidla, akce, úlohy a telemetrie.

Můžete najít hello oprávnění přiřazenou roli tooeach v hello [RolePermissions.cs] [ lnk-resource-cs] zdrojový soubor.

### <a name="changing-application-roles-for-a-user"></a>Změna aplikační role pro uživatele

Můžete použít následující postup toomake uživatele v Active Directory správcem předkonfigurované řešení hello.

Musí být role Globální správce toochange AAD pro uživatele:

1. Přejděte toohello [portál Azure][lnk-portal].
2. Vyberte **Azure Active Directory**.
3. Ujistěte se, že používáte hello adresář, který jste zvolili na azureiotsuite.com při zřizování řešení. Pokud máte několik adresářů, které jsou spojené s vaším předplatným, můžete přepnout mezi nimi Pokud kliknete na název účtu v pravém horním hello hello portálu.
4. Klikněte na tlačítko **podnikové aplikace, které**, pak **všechny aplikace**.
4. Zobrazit **všechny aplikace** s **žádné** stavu. Poté vyhledejte aplikaci s názvem předkonfigurované řešení.
5. Klikněte na název hello hello aplikace, která odpovídá názvu vašeho předkonfigurované řešení.
6. Klikněte na tlačítko **uživatelů a skupin**.
7. Vyberte uživatele hello chcete tooswitch role.
8. Klikněte na tlačítko **přiřadit** a vyberte hello role (jako například **správce**) byste chtěli tooassign toohello uživatele, klikněte na tlačítko zaškrtnutí hello.

## <a name="faq"></a>Nejčastější dotazy

### <a name="im-a-service-administrator-and-id-like-toochange-hello-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Správce služby a v nastavit jako toochange hello directory mapování mezi Moje předplatné a konkrétní klienta AAD. Jak tento úkol provést?

1. Přejděte toohello [portál Azure classic][lnk-classic-portal], klikněte na tlačítko **nastavení** v seznamu služeb na levé straně hello hello.
2. Vyberte hello odběr, který chcete toochange hello directory mapování.
3. Klikněte na tlačítko **upravit adresář**.
4. Vyberte hello **Directory** chcete toouse v rozevírací nabídce hello. Klikněte na tlačítko Předat dál šipku hello.
5. Potvrďte mapování hello adresáře a spolusprávci vliv. Pokud přecházíte z jiného adresáře, budou odebrány všechny hello spolusprávci z původní adresáře hello.

### <a name="im-a-domain-usermember-on-hello-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Jsem uživatele nebo členem domény na hello AAD klienta a vytvořili jste předkonfigurované řešení. Jak I přiřazovány roli své aplikaci?

Požádejte toomake globálního správce globálního správce na hello AAD klienta a pak mu přiřaďte role toousers sami. Případně požádejte tooassign globální správce je role přímo. Pokud chcete klienta AAD hello toochange předkonfigurované řešení pro nasazení, najdete v části Další otázka hello.

### <a name="how-do-i-switch-hello-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Jak lze přepnout hello AAD klienta, které Moje předkonfigurovaného řešení vzdáleného monitorování a aplikace jsou přiřazeny k?

Můžete spustit nasazení cloudu z <https://github.com/Azure/azure-iot-remote-monitoring> a znovu nasaďte s nově vytvořený klienta AAD. Protože jsou ve výchozím globálním správcem při vytváření klienta služby AAD máte oprávnění tooadd uživatele a přiřadit role uživatele toothose.

1. Vytvoření adresáře služby AAD v hello [portál Azure][lnk-portal].
2. Přejděte příliš<https://github.com/Azure/azure-iot-remote-monitoring>.
3. Spustit `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (například `build.cmd cloud debug myRMSolution`)
4. Po zobrazení výzvy, nastavte hello **tenantid** toobe tenanta nově vytvořený místo předchozího klienta.

### <a name="i-want-toochange-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Chci, aby toochange služby správce nebo Spolusprávce při přihlášení k účtu organizační

Najdete v článku podpory hello [změna správce služeb a Spolusprávce při přihlášení k účtu organizační][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-hello-proper-permissions-toocreate-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Proč se zobrazuje tato chyba? "Váš účet nemá příslušná oprávnění toocreate hello řešení. "Svého účtu správce nebo zkuste to znovu s jiným účtem."

Podívejte se na následující diagram pokyny hello:

![][img-flowchart]

> [!NOTE]
> Pokud budete pokračovat toosee hello chybu po ověření jste globální správce v tenantovi AAD hello a spolusprávce předplatného hello, musí správce účtu odebrat hello uživatele a přiřadit potřebná oprávnění v tomto pořadí. Nejdřív přidejte uživatele hello jako globální správce a poté přidejte uživatele jako spolusprávce na hello předplatného Azure. Pokud problémy potrvají, obraťte se na [centru pro nápovědu a podporu][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-toocreate-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Proč se zobrazuje tato chyba, pokud předplatné Azure? "Předplatné Azure je řešení vyžaduje toocreate předem nakonfigurované. Bezplatný zkušební účet můžete vytvořit během několika minut."

Pokud jste si jisti, že máte předplatné Azure, ověření klienta hello mapování pro vaše předplatné a ujistěte se, že hello správné klienta je vybraný v rozevírací nabídce hello. Pokud jste ověřit hello potřeby správnost klienta, postupujte podle hello předchozím diagramu a ověřit mapování hello vaše předplatné a tohoto klienta AAD.

## <a name="next-steps"></a>Další kroky
toocontinue získávání informací o IoT Suite, najdete v části jak můžete [přizpůsobení předkonfigurovaného řešení][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
