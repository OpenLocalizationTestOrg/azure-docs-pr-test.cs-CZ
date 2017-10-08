---
title: "aaaLog v tooAzure z hello rozhraní příkazového řádku | Microsoft Docs"
description: "Připojit tooyour předplatné Azure pro Mac, Linux a Windows z hello rozhraní příkazového řádku Azure (Azure CLI)"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 42682c00c8dea78b2c624e640379716d1d4d7a2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-in-tooazure-from-hello-azure-cli"></a>Přihlaste se tooAzure z hello rozhraní příkazového řádku Azure
Hello rozhraní příkazového řádku Azure je sada příkazů open source, a platformy pro práci s prostředky Azure. Tento článek popisuje různé způsoby tooprovide hello účet Azure pověření tooconnect hello rozhraní příkazového řádku Azure tooyour předplatné Azure:

* Spustit hello `azure login` rozhraní příkazového řádku příkaz tooauthenticate prostřednictvím Azure Active Directory. Tato metoda poskytuje přístup tooCLI příkazy v obou [příkaz režimy](#cli-command-modes). Když spustíte příkaz hello bez dalších možností, `azure login` vyzve vás toocontinue interaktivní přihlašování prostřednictvím webového portálu. Pro další `azure login` příkaz Možnosti najdete v tématu hello scénáře v tomto článku nebo typ `azure login --help`.
* Pokud potřebujete jenom toouse Azure Service Management režimu rozhraní příkazového řádku (nedoporučuje se většina nových nasazení), můžete stáhnout a nainstalovat soubor nastavení publikování ve vašem počítači.

Pokud jste ještě nenainstalovali hello CLI, přečtěte si téma [hello instalace rozhraní příkazového řádku Azure](cli-install-nodejs.md). Pokud nemáte předplatné Azure, můžete si [bezplatně vytvořit účet](http://azure.microsoft.com/free/) během několika minut.

Informace o jiný účet identity a předplatná Azure, najdete v části [asociování předplatných Azure se službou Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="scenario-1-azure-login-with-interactive-login"></a>Scénář 1: přihlášení k azure s interaktivní přihlášení
S určitým účtům hello rozhraní příkazového řádku, musíte toorun `azure login` a pokračujte v procesu přihlášení hello s webovým prohlížečem, přes webový portál, procesu označovaného jako *interaktivní přihlášení*. Obvyklým důvodem je, když máte pracovní nebo školní účet (také nazývané *účet organizace*) je nastavený toorequire vícefaktorového ověřování. Pokud chcete příkazy v režimu Resource Manager toouse také používejte interaktivní přihlášení pomocí účtu Microsoft.

Interaktivní přihlášení je snadné: typ `azure login` – bez jakékoli možnosti – jak ukazuje následující příklad hello:

```
azure login
```                                                                                             

Zobrazí se výstup Hello něco podobného jako hello následující:

```         
info:    Executing command login
info:    toosign in, use a web browser tooopen hello page http://aka.ms/devicelogin. Enter hello code XXXXXXXXX tooauthenticate.
```
Zkopírujte kód hello nabízí tooyou ve výstupu příkazu hello a otevřete prohlížeč toohttp://aka.ms/devicelogin nebo jinou stránku, pokud zadaný. (Můžete otevřít prohlížeč na hello stejný počítač, nebo na jiný počítač nebo zařízení.) Zadejte kód hello a pak jste výzvami tooenter hello uživatelské jméno a heslo pro identitu hello chcete toouse. Po dokončení tohoto procesu hello příkazové okno dokončení přihlášení hello. Ho může vypadat podobně jako:

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> S interaktivní přihlášení ověřování a autorizace se provádí pomocí služby Azure Active Directory. Pokud chcete použít identitu účtu Microsoft, přistupuje k procesu přihlášení hello výchozí doménu služby Azure Active Directory. (Pokud jste zaregistrovali bezplatný účet Azure, Azure Active Directory automaticky vytvoří výchozí doménu pro účet.)
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a>Scénář 2: azure přihlášení pomocí uživatelského jména a hesla
Použití hello `azure login` příkazu s uživatelským jménem hello (`-u`) tooauthenticate parametr, pokud chcete, aby toouse pracovní nebo školní účet, který nevyžaduje vícefaktorového ověřování. Zobrazí se výzva v příkazovém řádku hello hello hesla (nebo můžete volitelně předat hello heslo jako dodatečný parametr hello `azure login` příkaz). Hello následující příklad předá hello uživatelské jméno účtu organizace:

    azure login -u myUserName@contoso.onmicrosoft.com

Pak se zobrazí výzva tooenter heslo:

    info:    Executing command login
    Password: *********

potom dokončí proces přihlášení Hello.

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

Pokud je vaše první protokolování v čase se pomocí těchto přihlašovacích údajů, se zobrazí výzva tooverify chcete toocache ověřovací token. Tato výzva také nastat, pokud jste dříve používali hello `azure logout` příkazu (popsaný v článku hello později). spustit tuto výzvu pro scénáře automatizace toobypass `azure login` s hello `-q` parametr.

## <a name="scenario-3-azure-login-with-a-service-principal"></a>Scénář 3: přihlášení k azure pomocí objektu služby
Pokud vytvoříte objekt služby pro aplikaci služby Active Directory a objektu služby hello má oprávnění pro vaše předplatné, můžete použít hello `azure login` příkaz tooauthenticate hello instanční objekt. V závislosti na scénáři, je možné zadat přihlašovací údaje hello hello instanční objekt jako explicitní parametry hello `azure login` příkaz. Například hello následující příkaz předává hello hlavní název služby a služby Active Directory ID klienta:

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

Můžete se pak výzvami tooprovide hello heslo. Můžete také zadat přihlašovací údaje hello prostřednictvím rozhraní příkazového řádku skriptu nebo aplikace kódu nebo použít objekt certifikátu tooauthenticate hello služby neinteraktivně pro scénáře automatizace. Podrobnosti a příklady naleznete v tématu [ověřování hlavní název služby pomocí Azure Resource Manageru](resource-group-authenticate-service-principal-cli.md).

## <a name="scenario-4-use-a-publish-settings-file"></a>Scénář 4: Použijte soubor nastavení publikování
Pokud potřebujete jenom toouse hello Azure Service Management režimu rozhraní příkazového řádku (například toodeploy virtuální počítače Azure v modelu nasazení classic hello), se můžete připojit pomocí soubor nastavení publikování. Tato metoda nainstaluje certifikát v místním počítači, který umožňuje tooperform úlohy správy pro také hello předplatném a certifikátu hello jsou platné.

* **soubor nastavení publikování toodownload hello** pro váš účet, ujistěte se, že hello rozhraní příkazového řádku je v režimu správy služby zadáním `azure config mode asm`. Spusťte následující příkaz hello:

        azure account download

To otevře výchozí prohlížeč a zobrazí výzvu toosign v toohello [portál Azure classic](https://manage.windowsazure.com). Po přihlášení, `.publishsettings` stahování souborů. Poznamenejte si, kam se soubor uloží.

> [!NOTE]
> Pokud váš účet je spojen s více klienty Azure Active Directory, může být výzvami tooselect, který chcete toodownload nastavení publikování služby Active Directory v souboru.
>
>

Po výběru pomocí stránky pro stažení hello nebo návštěvou hello portál Azure classic, hello vybrané služby Active Directory změní výchozí hello používá hello klasický portál a stránky pro stažení. Po vytvoření výchozí, zobrazí se hello text,**kliknutím sem stránka pro výběr toohello tooreturn**' hello horní části stránky pro stažení hello. Použití hello zadat odkaz tooreturn toohello výběr stránky.

* **soubor nastavení publikování tooimport hello**spusťte hello následující příkaz:

        azure account import <path tooyour .publishsettings file>

> [!IMPORTANT]
> Po importu vaše nastavení publikování, měli byste odstranit hello `.publishsettings` souboru. Je už nevyžadovala hello rozhraní příkazového řádku Azure a představuje bezpečnostní riziko, může se použít toogain přístup tooyour předplatné.
>
>

## <a name="cli-command-modes"></a>Režimy rozhraní příkazového řádku příkaz
Hello rozhraní příkazového řádku Azure obsahuje dva režimy příkaz pro práci s prostředky Azure, se sadami jiný příkaz:

* **Režimu Resource Manager** – pro práci s prostředky Azure v modelu nasazení Resource Manager hello. tooset tento režim spuštění `azure config mode arm`.
* **Režim správy služby** – pro práci s prostředky Azure v modelu nasazení classic hello. tooset tento režim spuštění `azure config mode asm`.

Při první instalaci hello aktuální verzi hello rozhraní příkazového řádku je v režimu Resource Manager.

> [!NOTE]
> Hello režimu Resource Manager a režimu správy služby se vzájemně vylučují. To znamená, se nedají spravovat prostředky vytvořené v jednom režimu z hello jiných režimu.
>
>

## <a name="multiple-subscriptions"></a>Více předplatných
Pokud máte víc předplatných Azure, připojení tooAzure uděluje přístup tooall předplatná spojená s přihlašovacích údajů. Jedno předplatné, je vybrána jako výchozí hello a použít hello rozhraní příkazového řádku Azure při provádění operací. Můžete zobrazit hello předplatného, včetně hello aktuální výchozí předplatné, pomocí hello `azure account list` příkaz. Tento příkaz vrátí podobné toohello následující informace:

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

V předchozím seznamu hello, hello **aktuální** sloupec zobrazuje informace o hello aktuální výchozí předplatné jako Azure-sub-1. toochange hello výchozí předplatné, použijte hello `azure account set` příkaz a zadejte hello předplatné chcete toobe hello výchozí. Například:

    azure account set Azure-sub-2

Tato operace změní hello výchozí předplatné tooAzure-sub-2.

> [!NOTE]
> Změna výchozí předplatné hello se okamžitě projeví a globální změně; nové příkazy rozhraní příkazového řádku Azure, zda je spouštět z hello stejné instance příkazového řádku nebo jinou instanci, použijte nové předplatné výchozí hello.
>
>

Pokud chcete toouse jiné než výchozí předplatné s hello rozhraní příkazového řádku Azure, ale nechcete, aby toochange hello aktuální výchozí nastavení, můžete použít hello `--subscription` možnost pro příkaz hello a zadejte název hello hello předplatného chcete toouse pro operaci hello.

Až se připojené tooyour předplatné Azure, můžete začít používat toowork příkazy příkazového řádku Azure CLI hello s prostředky Azure.

## <a name="storage-of-cli-settings"></a>Ukládání nastavení rozhraní příkazového řádku
Jestli přihlásit hello `azure login` příkaz nebo importu nastavení publikování, profil rozhraní příkazového řádku a protokoly jsou uloženy v `.azure` adresář umístěný ve vaší `user` adresáře. Vaše `user` adresář je chráněn váš operační systém. Doporučujeme však, že je provést další kroky tooencrypt vaše `user` adresáře. Můžete tak učinit v hello následující způsoby:

* V systému Windows upravit vlastnosti directory hello nebo pomocí nástroje BitLocker.
* V systému Mac zapněte pro adresář hello FileVault.
* Na Ubuntu použijte hello šifrovaný domovské adresáře funkce. Další Linuxových distribucích nabízí podobné funkce.

## <a name="logging-out"></a>Protokolování se
toolog out hello použijte následující příkaz:

    azure logout -u <username>

Pokud hello odběry přidružený účet hello pouze ověření službou Active Directory, protokolování se odstraní informace o předplatném hello z místního profilu hello. Ale pokud soubor nastavení publikování byl importován také odběrů hello, protokolování jen odstranění služby Active Directory související informace z profilu místní hello.

## <a name="next-steps"></a>Další kroky
* toouse rozhraní příkazového řádku Azure, najdete v části [rozhraní příkazového řádku Azure v režimu Resource Manager](virtual-machines/azure-cli-arm-commands.md) a [rozhraní příkazového řádku Azure v režimu správy služby](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* toolearn Další informace o hello rozhraní příkazového řádku Azure, zdrojový kód, odesílat zprávy o problémech, nebo přispívat toohello projekt stáhnete na adrese hello [úložiště GitHub pro hello rozhraní příkazového řádku Azure](https://github.com/azure/azure-xplat-cli).
* Pokud narazíte na problémy pomocí hello rozhraní příkazového řádku Azure nebo Azure, navštivte hello [fóra Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).
