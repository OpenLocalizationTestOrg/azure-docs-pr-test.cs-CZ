---
title: "Azure Active Directory Domain Services: Připojení k spravované doméně tooa virtuálního počítače Windows serveru | Microsoft Docs"
description: "Připojení k virtuálnímu počítači s Windows serverem tooAzure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 74dbdb33-05db-4d47-badc-0d7bb6d0c8cb
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1e85833b42bd51f3b3df067d6c5b69253459bec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-windows-server-virtual-machine-tooa-managed-domain"></a>Připojení k spravované doméně systému Windows Server virtuálního počítače tooa
> [!div class="op_single_selector"]
> * [Portál Azure classic – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell – Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

Tento článek ukazuje, jak toojoin virtuální počítač spuštěný Windows Server 2012 R2 tooan Azure AD Domain Services spravované doméně pomocí hello portál Azure classic.

## <a name="step-1-create-hello-windows-server-virtual-machine"></a>Krok 1: Vytvoření virtuálního počítače Windows serveru hello
Postupujte podle pokynů hello uvedených v hello [vytvoření virtuálního počítače se systémem Windows v hello portál Azure classic](../virtual-machines/windows/classic/tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) kurzu. Je důležité tooensure, který tento nově vytvořený virtuální počítač je připojený k toohello stejnou virtuální síť, ve kterém jste povolili službu Azure AD Domain Services. Hello možnost 'Vytvořit' neumožňuje toojoin hello virtuálního počítače tooa virtuální sítě. Proto musíte toouse hello z Galerie možnost toocreate hello virtuálního počítače.

Proveďte následující kroky toocreate hello Windows virtuální počítač připojený k toohello virtuální sítě ve kterém jste povolili Azure AD Domain Services.

1. V hello portál Azure classic, na panelu příkazů hello v hello dolní části okna hello, klikněte na **nový**.
2. V části **výpočetní**, klikněte na tlačítko **virtuálního počítače**, pak klikněte na tlačítko **z Galerie**.
3. úvodní obrazovka první umožňuje **zvolí obrázek** pro virtuální počítač ze seznamu dostupných imagí hello. Vyberte příslušné bitové kopie hello.

    ![Vyberte bitovou kopii](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. úvodní obrazovka druhý umožňuje vyberte název počítače, velikost a správu uživatelské jméno a heslo. Použití hello vrstvy a velikost vyžaduje toorun aplikace nebo zatížení. Hello uživatelské jméno, které tady vyberete je uživatel místní správce na počítači hello. Nezadávejte zde přihlašovací údaje pro účet uživatele domény.

    ![Konfigurace virtuálního počítače](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)
5. úvodní obrazovka třetí umožňuje nakonfigurovat prostředky sítě, úložiště a dostupnosti. Ujistěte se, vyberete hello virtuální síť, ve kterém jste povolili službu Azure AD Domain Services z hello **oblasti nebo vztahů skupiny nebo virtuální síť** rozevíracího seznamu. Zadejte **název cloudové služby DNS** jako vhodný pro virtuální počítač hello.

    ![Vyberte virtuální síť pro virtuální počítač](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

   > [!WARNING]
   > Zkontrolujte připojení k toohello hello virtuální počítač stejnou virtuální síť, ve kterém jste povolili Azure AD Domain Services. V důsledku toho můžete hello virtuální počítač "viz" hello domény a provádět úlohy, jako je například přidávání hello domény. Pokud si zvolíte toocreate hello virtuálního počítače v jinou virtuální síť, připojte virtuální síť této virtuální sítě toohello, ve kterém jste povolili Azure AD Domain Services.
   >
   >
6. úvodní obrazovka čtvrtý umožňuje nainstalovat hello agenta virtuálního počítače a nakonfigurovat některé z dostupných rozšíření hello.

    ![Hotovo](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
7. Po vytvoření virtuálního počítače hello portálu classic hello uvádí hello nového virtuálního počítače v části hello **virtuální počítače** uzlu. Automaticky spustit hello virtuálního počítače a cloudové služby a jejich stav je uveden jako **systémem**.

    ![Virtuální počítač je spuštěný a funkční](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)

## <a name="step-2-connect-toohello-windows-server-virtual-machine-using-hello-local-administrator-account"></a>Krok 2: Připojení pomocí účtu místního správce hello toohello virtuálního počítače Windows serveru
Nyní se nám připojit toohello nově vytvořený virtuální počítač Windows serveru, toojoin ho toohello domény. Použití hello přihlašovací údaje místního správce, který jste zadali při vytváření hello virtuálního počítače, tooconnect tooit.

Proveďte následující kroky tooconnect toohello virtuální počítač hello.

1. Přejděte příliš**virtuální počítače** uzlu portálu classic hello. Vyberte virtuální počítač hello jste vytvořili v kroku 1 a klikněte na tlačítko **Connect** na panelu příkazů hello v hello dolní části okna hello.

    ![Připojit tooWindows virtuálního počítače](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. portál classic Hello vás vyzve k tooopen nebo uložit soubor s příponou '.rdp', což je použité tooconnect toohello virtuálního počítače. Po dokončení stahování, klikněte na tlačítko tooopen hello souboru.
3. Na příkazovém řádku hello přihlášení zadejte vaše **přihlašovací údaje místního správce**, který jste zadali při vytváření hello virtuálního počítače. Například jsme v tomto příkladu jste použili 'localhost\mahesh'.

V tomto okamžiku jste mají být protokolovány v toohello nově vytvořený virtuální počítač s Windows pomocí přihlašovacích údajů místního správce. dalším krokem Hello je toojoin hello virtuálního počítače toohello domény.

## <a name="step-3-join-hello-windows-server-virtual-machine-toohello-aad-ds-managed-domain"></a>Krok 3: Připojení hello systému Windows Server virtuálního počítače toohello AAD DS spravované doméně
Proveďte následující kroky toojoin hello systému Windows Server virtuálního počítače toohello AAD DS spravované doméně hello.

1. Připojte toohello Windows serveru, jak je znázorněno v kroku 2. Hello úvodní obrazovce otevřete **správce serveru**.
2. Klikněte na tlačítko **místní Server** v levém podokně okna Správce serveru hello hello.

    ![Spusťte správce serveru na virtuálním počítači](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Klikněte na tlačítko **pracovní skupiny** pod hello **vlastnosti** části. V hello **vlastnosti systému** stránce vlastností, klikněte na tlačítko **změnu** toojoin hello domény.

    ![Stránka vlastností systému](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Zadejte název domény hello vaší spravované domény služby Azure AD Domain Services v hello **domény** textové pole a klikněte na tlačítko **OK**.

    ![Zadejte, připojený k doméně toobe hello](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. Můžete se výzvami tooenter doménu hello toojoin přihlašovací údaje. Ujistěte se, které jste **a zadat přihlašovací údaje uživatel patřící toohello AAD řadič domény správci hello** skupiny. Pouze členové této skupiny mají oprávnění toojoin počítače toohello spravované domény.

    ![Zadejte pověření pro připojení k doméně](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Můžete zadat přihlašovací údaje v některém z následujících způsobů hello:

   * Formát UPN: Zadejte hello příponu UPN pro hello uživatelský účet, jak nakonfigurovat ve službě Azure AD. V tomto příkladu je přípona UPN hello hello uživatele "bob",bob@domainservicespreview.onmicrosoft.com'.
   * Formát SAMAccountName: můžete zadat název účtu hello ve formátu SAMAccountName hello. V tomto příkladu hello "bob" by stačit, když uživatel tooenter 'CONTOSO100\bob'.

     > [!NOTE]
     > **Doporučujeme používat hello UPN formátu toospecify pověření.** Hello SAMAccountName může být automaticky generovaný pokud předpona uživatele (UPN) je příliš dlouhý (například "joereallylongnameuser"). Pokud máte více uživatelů hello stejnou předponou hlavní název uživatele (například "bob") v klientovi služby Azure AD, jejich SAMAccountName formátu může být automaticky generované službou hello. V těchto případech můžete spolehlivě použít formát UPN hello toolog v doméně toohello.
     >
     >
7. Po připojení k doméně je úspěšné, zobrazí hello následující zprávou, které vás vítá toohello domény. Restartujte hello virtuálního počítače pro toocomplete operaci připojení k doméně hello.

    ![Vítejte toohello domény](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

## <a name="troubleshooting-domain-join"></a>Řešení potíží s připojení k doméně
### <a name="connectivity-issues"></a>Problémy s připojením
Pokud je hello virtuální počítač nelze toofind hello domény, podívejte se na toohello následující kroky řešení potíží:

* Zajistěte, aby hello virtuální počítač je připojený toohello stejné virtuální síti, který jste povolili Domain Services. Pokud ne, hello virtuální počítač nelze tooconnect toohello domény a je proto nelze toojoin hello domény.
* Pokud je virtuální počítač hello tooanother připojené virtuální sítě, zajistěte, aby byl této virtuální sítě připojený toohello virtuální síť, ve kterém jste povolili Domain Services.
* Zkuste tooping hello domény pomocí názvu domény hello hello spravované domény (například "ping contoso100.com"). Pokud jste nelze toodo tak, zkuste tooping hello IP adresy pro doménu hello zobrazí na stránce hello, kde jste povolili službu Azure AD Domain Services (například "ping 10.0.0.4'). Pokud budete mít tooping hello IP adresa, ale není hello domény, DNS je pravděpodobně nesprávně nakonfigurována. Nemusí nakonfigurujete hello IP adresy hello domény jako servery DNS pro virtuální síť hello.
* Zkuste, abyste vyprázdnili hello mezipaměť Překladač DNS na virtuálním počítači hello ("ipconfig/flushdns").

Pokud se zobrazí toohello dialogové okno s dotazem, pro přihlašovací údaje toojoin hello domény, nemáte problémy s připojením.

### <a name="credentials-related-issues"></a>Problémy související s přihlašovací údaje
Naleznete toohello následující kroky, pokud máte problémy s přihlašovacími údaji a jsou nelze toojoin hello domény.

* Zkuste použít hello UPN formátu toospecify pověření. Hello SAMAccountName pro váš účet může být automaticky generovaný Pokud více uživatelů s hello stejné UPN předpony ve vašem klientovi nebo pokud vaši předponu UPN je příliš dlouhé. Proto hello SAMAccountName formát pro váš účet může lišit od co můžete očekávat, nebo použít v místní doméně.
* Zkuste toouse hello přihlašovací údaje uživatelského účtu, který patří toohello 'AAD řadič domény správci' skupiny toojoin počítače toohello spravované domény.
* Ujistěte se, že máte [povolena synchronizace hesel](active-directory-ds-getting-started-password-sync.md) v souladu s hello kroků uvedených v příručce Začínáme hello.
* Zkontrolujte, jestli hello UPN hello uživatele používají jako nakonfigurovaný v Azure AD (například "bob@domainservicespreview.onmicrosoft.com') toosign v.
* Ujistěte se, že jste čekali dost dlouho pro toocomplete synchronizace heslo zadané v příručce Začínáme hello.

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
