---
title: "aaaCreate vlastní role řízení přístupu na základě Role a přiřaďte toointernal a externí uživatele v Azure | Microsoft Docs"
description: "Přiřadit vlastní role RBAC vytvořené pomocí prostředí PowerShell a rozhraní příkazového řádku pro interních a externích uživatelů"
services: active-directory
documentationcenter: 
author: andreicradu
manager: catadinu
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/10/2017
ms.author: a-crradu
ms.openlocfilehash: 26793a66d6ca2f771338eed87d10ce2b3b431841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="intro-on-role-based-access-control"></a>Úvod na řízení přístupu na základě rolí

Řízení přístupu na základě role je Azure portálu pouze funkce umožňuje hello vlastníky předplatného tooassign granulární role tooother uživatelům, kteří mohou spravovat konkrétní prostředek obory ve svém prostředí.

RBAC umožňuje lepší zabezpečení správy pro velké organizace a pro SMB práce s externími spolupracovníky, dodavatele nebo freelancers, které potřebují přístup k prostředkům toospecific v prostředí, ale nemusí nutně jít toohello celé infrastruktury nebo žádné související fakturace obory. RBAC umožňuje flexibilitu hello vlastnící jedno předplatné spravuje hello správce účtu (role Správce služby na úrovni předplatného) a mít více uživatelů pozvat toowork pod hello stejného předplatného. ale bez všechny správce práva pro něj. Ze správy a fakturace perspektivy funkce RBAC hello prokáže toobe možnost efektivní čas a správy pro používání Azure v různých situacích.

## <a name="prerequisites"></a>Požadavky
Používání RBAC v hello prostředí Azure vyžaduje:

* Nutnosti samostatné předplatné přiřazený uživatel toohello jako vlastník (předplatné role)
* Mít roli vlastníka hello hello předplatného Azure
* Mít přístup toohello [portálu Azure](https://portal.azure.com)
* Zajistěte, aby registroval toohave hello následující zprostředkovatelé prostředků pro předplatné uživatele hello: **Microsoft.Authorization**. Další informace o tom, jak tooregister hello zprostředkovatelé prostředků najdete v tématu [zprostředkovatelé Resource Manager, oblastí, verzí rozhraní API a schémat](/azure-resource-manager/resource-manager-supported-services.md).

> [!NOTE]
> Předplatná Office 365 nebo Azure Active Directory licence (například: přístup k tooAzure služby Active Directory) zajištěného z portálu není kvality pomocí RBAC hello O365.

## <a name="how-can-rbac-be-used"></a>RBAC použití
RBAC lze použít na tři různé rozsahy v Azure. Z hello nejvyšší oboru toohello nejnižší jeden ty jsou následující:

* Předplatné (nejvyšší)
* Skupina prostředků
* Prostředek oboru (hello nejnižší úroveň přístupu nabídky cílové oprávnění oboru tooan jednotlivých prostředků Azure)

## <a name="assign-rbac-roles-at-hello-subscription-scope"></a>Přiřadit role RBAC v oboru předplatné hello
Existují dvě běžných příkladů, když RBAC je použít (ale mimo jiné):

* Že externí uživatelé z organizací hello (není součástí klienta Azure Active Directory uživatele správce hello) pozvat toomanage určité prostředků nebo předplatného celou hello
* Práce s uživateli uvnitř hello organizace (jsou součástí klienta Azure Active Directory hello uživatele), ale součást různé týmy nebo skupin, které potřebují přístup granulární buď toohello celé předplatné nebo toocertain skupiny prostředků nebo prostředek rozsahy hello prostředí

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Udělení přístupu na úrovni předplatného pro uživatele mimo Azure Active Directory
Role RBAC lze udělit pouze systémem **vlastníky** hello předplatného proto hello správce musí být přihlášený uživatel s uživatelským jménem, které má tato role předběžně zařazená nebo vytvořil hello předplatného Azure.

Z hello portálu Azure po jste přihlášení jako správce, vyberte možnost "Odběry" a vyberte hello požadované jeden.
![okno odběru na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) ve výchozím nastavení, pokud uživatel s oprávněními správce hello zakoupila hello předplatné Azure, hello uživatele se zobrazí jako **správce účtu**, tato se hello předplatné role. Další informace o hello předplatného Azure rolí, najdete v části [přidání nebo změna role Správce služby Azure, které spravují předplatné hello nebo služby](/billing/billing-add-change-azure-subscription-administrator.md).

V tomto příkladu hello uživatele "alflanigan@outlook.com" je hello **vlastníka** z hello "Bezplatnou zkušební verzi" předplatné v hello AAD klienta "Výchozí klienta Azure". Vzhledem k tomu, že je tento uživatel hello Tvůrce hello předplatného Azure s hello počáteční Account Microsoft "Outlook" (Account Microsoft = Outlook, Live atd.) bude hello výchozí název domény pro všechny uživatele přidán do tohoto klienta **"@alflaniganuoutlook.onmicrosoft.com"**. Standardně je vytvořen hello syntaxe hello nové domény umístěním společně hello uživatelské jméno a doménu název hello uživatele, který vytvořil hello klienta a přidání rozšíření hello **". onmicrosoft.com"**.
Kromě toho uživatelé můžou přihlásit pomocí vlastního názvu domény v klientovi hello po přidání a ověření pro nového klienta hello. Další informace o tom, jak tooverify vlastního názvu domény v klienta služby Azure Active Directory najdete v části [přidat adresář tooyour název vlastní domény](/active-directory/active-directory-add-domain).

V tomto příkladu hello "výchozí klient Azure" adresář obsahuje pouze uživatelé s názvem domény hello "@alflanigan.onmicrosoft.com".

Po výběru předplatného hello, musíte kliknout na uživatel s oprávněními správce hello **řízení přístupu (IAM)** a potom **přidat novou roli**.





![Funkce IAM řízení přístupu na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![Přidání nového uživatele v IAM funkce řízení přístupu na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

dalším krokem Hello je tooselect hello role toobe přiřazené a hello uživatele, kterému se přiřadí hello RBAC role. V hello **Role** rozevírací nabídce hello správce uživateli se zobrazí pouze hello předdefinované RBAC role, které jsou k dispozici v Azure. Podrobné vysvětlení jednotlivých rolí a jejich přiřaditelnými obory, najdete v části [předdefinované role pro řízení přístupu](/active-directory/role-based-access-built-in-roles.md).

uživatel s oprávněními správce Hello pak musí tooadd hello e-mailovou adresu externího uživatele hello. Hello očekává, že je chování pro hello externího uživatele toonot se zobrazí v hello existující klienta. Po pozval hello externího uživatele, mohl se nebude zobrazovat v části **odběry > řízení přístupu (IAM)** se všemi uživateli aktuální hello, které jsou přiřazeny role RBAC v hello obor předplatného.





![Přidání role RBAC toonew oprávnění](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![seznam rolí RBAC na úrovni předplatného](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

uživatel Hello "chessercarlton@gmail.com" byl pozvané toobe **vlastníka** pro hello "Bezplatnou zkušební verzi" předplatného. Po odeslání pozvánky hello, obdrží hello externího uživatele potvrzení e-mailu s odkazem k aktivaci.
![e-mailová pozvánka pro RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)

Probíhá externí toohello organizace, hello nový uživatel nemá žádné existující atributy v adresáři "Výchozí klienta Azure" hello. Bude vytvořen po externího uživatele hello udělil souhlas toobe zaznamenaná v hello adresář, který je přidružen ke hello odběr, který byl přiřazen k roli.





![e-mailové zprávě pozvánky pro RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

Hello externího uživatele zobrazuje v hello klienta Azure Active Directory od této chvíle jako externí uživatel a tento lze zobrazit v hello portálu Azure i na portálu classic hello.





![uživatelé okno azure active directory portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![uživatelé okno azure active directory portálu Azure classic](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

V hello **uživatelé** rozpoznal zobrazení v obou portálů hello externích uživatelů:

* Zadejte jinou ikonu Hello hello portál Azure
* Hello různých sourcing bodu portálu classic hello

Ale udělení **vlastníka** nebo **Přispěvatel** přístup tooan externí uživatel v hello **předplatné** obor, nepovoluje hello přístup toohello správce adresáře uživatele, Pokud hello **globálního správce** to umožňuje. V hello vlastnosti uživatele, hello **typ uživatele** jehož dvě společné parametry, **člen** a **hosta** lze identifikovat. Člen je uživatel, která je registrována v adresáři hello při hosta toohello adresáře pozvat uživatele z externího zdroje. Další informace najdete v tématu [jak správci Azure Active Directory přidat uživatele spolupráce B2B](/active-directory/active-directory-b2b-admin-add-users).

> [!NOTE]
> Ujistěte se, že po zadání přihlašovacích údajů hello hello portálu, hello externí uživatel vybere hello toosign ve správném adresáři. Hello stejného uživatele můžete mít přístup toomultiple adresáře a můžete vybrat buď jeden z nich kliknutím hello uživatelské jméno v hello vpravo nahoře v hello portál Azure a pak vyberte příslušný adresář hello z rozevíracího seznamu hello.

Při se hostovaného v adresáři hello, hello externího uživatele můžete spravovat všechny prostředky pro hello předplatné Azure, ale hello adresáři nelze získat přístup.





![přístup k portálu Azure active directory s omezeným přístupem tooazure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

Azure Active Directory a předplatné Azure, nemají vztah nadřazený podřízený jako ostatní prostředky služby Azure (například: virtuálních počítačů, virtuálních sítí, webové aplikace, úložiště atd.) s předplatné Azure. Všechny pozdější hello je vytvořen, spravovat a účtují pod předplatným Azure při předplatné Azure použité toomanage hello přístup tooan Azure directory. Další podrobnosti najdete v tématu [jak Azure předplatného je související tooAzure AD](/active-directory/active-directory-how-subscriptions-associated-directory).

Ze všech hello předdefinované RBAC rolí **vlastníka** a **Přispěvatel** nabízejí úplné správy přístupu tooall prostředků v prostředí hello, hello rozdíl probíhá, který nelze vytvořit Přispěvatel a odstranění, nové Role RBAC. Hello jiné předdefinované role, jako **Přispěvatel virtuálních počítačů** nabízejí úplné správy přístup pouze prostředky toohello indikován hello název, bez ohledu na to hello **skupiny prostředků** během vytváření do.

Přiřazení hello předdefinované role RBAC z **Přispěvatel virtuálních počítačů** na úrovni předplatného, znamená dané role uživatele přiřazené hello hello:

* Můžete zobrazit všechny virtuální počítače bez ohledu na to jejich nasazení datum a hello skupin prostředků, které jsou součástí
* Má úplné správy přístupu toohello virtuální počítače v rámci předplatného hello
* Nelze zobrazit u jiných typů prostředků v předplatném hello
* Všechny změny z hlediska fakturační nemůže pracovat.

> [!NOTE]
> Probíhá Azure portálu pouze funkce RBAC, ho neuděluje portálu classic toohello přístup.

## <a name="assign-a-built-in-rbac-role-tooan-external-user"></a>Přiřazení předdefinované RBAC role tooan externího uživatele
Pro jiný scénář v tomto testu, hello externí uživatele "alflanigan@gmail.com" se přidá jako **Přispěvatel virtuálních počítačů**.




![předdefinované role Přispěvatel virtuálního počítače](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

Hello normální chování pro tento externí uživatele s tato předdefinovaná role je toosee a spravovat pouze virtuální počítače a jejich přiléhající Resource Manager pouze prostředky potřebné při nasazování. Návrh těchto omezené rolí nabízí přístup jenom příslušné prostředky tootheir vytvořené v hello portálu Azure, bez ohledu na to některé je stále možné nasadit v hello klasického portálu (například: virtuální počítače).





![Přehled role Přispěvatel virtuálních počítačů na portálu azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-hello-same-directory"></a>Udělení přístupu na úrovni předplatného pro uživatele v hello stejný adresář
tok procesu Hello je identické tooadding externího uživatele, jak z hello perspektivy poskytující hello RBAC role správce, jakož i udělení přístupu toohello role uživatele hello. Hello rozdíl je, že uživatel hello pozvat neobdrží žádné pozvánek e-mailu jako všechny obory hello prostředků v rámci předplatného hello bude k dispozici v řídicím panelu hello po přihlášení.

## <a name="assign-rbac-roles-at-hello-resource-group-scope"></a>Přiřazení role RBAC v oboru skupiny prostředků hello
Přiřazení role RBAC v **skupiny prostředků** oboru má identické proces pro přiřazení role hello na úrovni předplatného hello, pro oba typy uživatelů - externí nebo interní (součástí hello stejný adresář). Hello uživatelů, které jsou přiřazeny hello RBAC role je toosee ve svém prostředí pouze skupinu prostředků hello mají přiřazený přístup z hello **skupiny prostředků** ikonu v hello portálu Azure.

## <a name="assign-rbac-roles-at-hello-resource-scope"></a>Přiřadit role RBAC v oboru prostředků hello
Přiřazení role RBAC v oboru prostředků v Azure má identické proces pro přiřazení role hello na úrovni předplatného hello nebo na úrovni skupiny prostředků hello, následující hello stejný pracovní postup pro oba scénáře. Hello uživatelů, které jsou přiřazeny hello RBAC role znovu, uvidí jenom hello položky, že mají přiřazený přístup k buď v hello **všechny prostředky** kartě nebo přímo v jejich řídicího panelu.

Je důležitým aspektem pro RBAC jak v oboru skupiny prostředků nebo prostředek oboru pro hello uživatelé toomake toohello opravdu toosign ve správném adresáři.





![přihlášení Directory na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Přiřazení role RBAC pro skupinu služby Azure Active Directory
Všechny scénáře hello pomocí RBAC na tři různé rozsahy hello v nabídka Azure hello oprávnění správě, nasazení a správa různé prostředky jako uživatel s přiřazenou bez hello potřebovat spravovat osobní předplatné. Bez ohledu na to hello RBAC role je přiřazená pro předplatné, skupinu prostředků nebo prostředek oboru, všechny prostředky hello hello přiřazené uživatelé vytvořili na další se fakturují pod hello jedno předplatné v které hello uživatelé mají přístup k. Tímto způsobem hello uživatelé, kteří mají oprávnění správce pro toto předplatné celý Azure fakturace má úplný přehled o spotřebě hello, bez ohledu na to, který spravuje prostředky hello.

Pro větší organizace, můžete použít role RBAC v hello stejným způsobem jako pro skupiny Azure Active Directory s hello perspektivy tento uživatel správce hello chce toogrant hello granulární přístup pro týmy nebo celý oddělení, není jednotlivě pro každého uživatele, tedy vzhledem k tomu, ho jako velmi čas a správu efektivní možnost. tooillustrate tento příklad, hello **Přispěvatel** role přidala tooone hello skupin v klientovi hello na úrovni předplatného hello.





![Přidání role RBAC pro skupiny AAD](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

Tyto skupiny jsou skupiny zabezpečení, které jsou zřizovat a spravovat pouze v rámci Azure Active Directory.

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-powershell"></a>Vytvořit vlastní podporu tooopen role RBAC požadavky pomocí prostředí PowerShell
Hello předdefinované RBAC role, které jsou k dispozici v Azure zkontrolujte určité úrovně oprávnění na základě dostupných prostředků hello v prostředí hello. Pokud žádná z těchto rolí potřebám hello Správce uživatelů, je však hello možnost toolimit přístup i další vytvořením vlastní role RBAC.

Vytvoření vlastní role RBAC vyžaduje, aby tootake jeden předdefinovaná role, upravovat a importujte ji zpět do prostředí hello. stažení Hello a nahrání hello role se spravují pomocí prostředí PowerShell nebo rozhraní příkazového řádku.

Je důležité toounderstand požadavky hello vytváření vlastní roli, které můžete udělit granulární přístup na úrovni předplatného hello a také dát hello pozvat uživatele hello možnost otevření žádosti o podporu.

Pro tento příklad hello předdefinovaná role **čtečky** což umožňuje uživatelům přístup tooview prostředků všech hello rozsahy ale není tooedit je nebo vytvořit nové byl přizpůsobit tooallow hello uživatele hello možnost otevření žádosti o podporu.

Hello první akce exportu hello **čtečky** spustili toobe role musí dokončit v prostředí PowerShell se zvýšenými oprávněními jako správce.

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Snímek obrazovky prostředí PowerShell pro role RBAC čtečky](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

Pak musíte tooextract hello JSON šablony role hello.





![Šablona JSON pro vlastní role RBAC čtečky](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

Typické role RBAC se skládá ze tří hlavních částí **akce**, **NotActions** a **AssignableScopes**.

V hello **akce** části jsou uvedeny všechny hello povolených operací pro tuto roli. Je důležité toounderstand přiřazený každá akce od zprostředkovatele prostředků. V takovém případě pro vytvoření hello lístků podpory **Microsoft.Support** poskytovatele prostředků musí být uvedený.

možnost toosee toobe všechny hello zprostředkovatelé prostředků k dispozici a registrovaný v rámci vašeho předplatného, můžete použít prostředí PowerShell.
```
Get-AzureRMResourceProvider

```
Kromě toho můžete zobrazit v hello všech hello dostupné prostředí PowerShell rutiny toomanage hello poskytovatelů prostředků.
    ![Snímek obrazovky prostředí PowerShell pro správu zprostředkovatele prostředků](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)

všechny akce pro konkrétní role RBAC prostředků poskytovatelé jsou uvedené části hello hello toorestrict **NotActions**.
Poslední je povinný, že tato role RBAC hello obsahuje explicitní předplatné hello ID, kde se používá. Hello ID předplatného jsou uvedeny v části hello **AssignableScopes**, v opačném případě je nebudou povolena, tooimport hello role v rámci vašeho předplatného.

Po vytvoření a vlastní nastavení hello RBAC role, potřebuje toobe importované back hello prostředí.

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

V tomto příkladu je hello vlastní název této role RBAC "Čtečky podporu lístky úroveň přístupu" povolení hello uživatele tooview všechno, co v hello předplatné a také tooopen žádosti o podporu.

> [!NOTE]
> jsou technologie Hello pouze dvě předdefinované role RBAC povolení hello akce otevření žádosti o podporu **vlastníka** a **Přispěvatel**. Pro podporu požadavky možné tooopen toobe uživatel mohl musí mít přiřazenou RBAC pouze v oboru předplatné hello, všechny žádosti o podporu se vytváří podle předplatného Azure.

Tato nová vlastní role byla přiřazena tooan uživatele z hello stejný adresář.





![snímek obrazovky vlastní role RBAC naimportována hello portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![snímek obrazovky přiřazení vlastní importované toouser RBAC role v hello stejný adresář](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![snímek obrazovky oprávnění pro vlastní importované role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

Příklad Hello byl další omezení hello podrobné tooemphasize této vlastní role RBAC následujícím způsobem:
* Můžete vytvořit nové žádosti o podporu
* Nelze vytvořit nové obory prostředků (například: virtuálního počítače)
* Nelze vytvořit nové skupiny prostředků





![snímek obrazovky vytváření žádosti o podporu vlastní role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![snímek obrazovky vlastní role RBAC nebylo možné toocreate virtuální počítače](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![snímek obrazovky vlastní role RBAC nebylo možné toocreate nové RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-azure-cli"></a>Vytvořit vlastní podporu tooopen role RBAC požadavků pomocí Azure CLI
Spouštění v Macu a bez nutnosti přístupu tooPowerShell, rozhraní příkazového řádku Azure je způsob, jak toogo hello.

Hello kroky toocreate vlastní role jsou hello stejné, s jedinou výjimkou hello, pomocí rozhraní příkazového řádku hello role nelze stáhnout v šablonu JSON, ale lze zobrazit v hello rozhraní příkazového řádku.

V tomto příkladu I rozhodli hello předdefinované role **zálohování čtečky**.

```

azure role show "backup reader" --json

```





![Snímek obrazovky rozhraní příkazového řádku zálohování čtečky role zobrazit](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

Po zkopírování hello vlastnosti v šabloně JSON úpravy hello role v sadě Visual Studio, hello **Microsoft.Support** poskytovatele prostředků byla přidána do hello **akce** částech tak, aby tento uživatel může otevřít žádosti o podporu přitom dál toobe čtečku pro hello záloh. Znovu případě je nutné tooadd ID předplatného hello kde tato role se použije v hello **AssignableScopes** části.

```

azure role create --inputfile <path>

```





![Rozhraní příkazového řádku snímek obrazovky importování vlastní role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

Nová role Hello je teď dostupná v hello portál Azure a proces assignation hello je hello stejné jako v předchozích příkladech hello.





![Azure portálu snímek obrazovky vlastní role RBAC vytvořené pomocí rozhraní příkazového řádku 1.0](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

Od verze hello je všeobecně dostupná nejnovější 2017 sestavení, hello prostředí cloudu Azure. Prostředí Azure Cloud je doplňkovým tooIDE a hello portálu Azure. S touto službou můžete získat prostředí založené na prohlížeči, který je ověřen a je hostovaná v Azure a můžete ji použít místo rozhraní příkazového řádku v počítači nainstalován.





![Prostředí cloudu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
