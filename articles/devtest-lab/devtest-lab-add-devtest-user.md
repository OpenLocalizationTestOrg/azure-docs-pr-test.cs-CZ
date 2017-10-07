---
title: "aaaAdd vlastníků a uživatele v Azure DevTest Labs | Microsoft Docs"
description: "Přidat vlastníků a uživatelé v Azure DevTest Labs pomocí hello portál Azure nebo prostředí PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: 2a98f5fe1efbd7c23e0d97f58f47c37462aed3b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Přidat vlastníků a uživatelé v Azure DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Přístup v Azure DevTest Labs řídí [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-control-what-is.md). Pomocí RBAC, můžete oddělit povinností v rámci týmu do *role* kde byste udělit pouze hello množství přístup nezbytné toousers tooperform svou práci. Jsou tři tyto role RBAC *vlastníka*, *uživatel DevTest Labs*, a *Přispěvatel*. V tomto článku se dozvíte, jaké akce lze provést v každé role RBAC tři hlavní hello. Odtud, zjistíte, jak se tooadd uživatelé tooa laboratoř - i prostřednictvím hello portálu a pomocí skriptu prostředí PowerShell a jak hello tooadd uživatele na úrovni předplatného.

## <a name="actions-that-can-be-performed-in-each-role"></a>Akce, které mohou být prováděny v každé role
Existují tři hlavní role, můžete přiřadit uživatele:

* Vlastník
* Uživatel DevTest Labs
* Přispěvatel

Hello následující tabulka znázorňuje hello akce, které lze provést pomocí uživatelů v každé z těchto rolí:

| **Můžete provést akce, které uživatelé v této roli** | **Uživatel DevTest Labs** | **Vlastník** | **Přispěvatel** |
| --- | --- | --- | --- |
| **Úlohy testovacího prostředí** | | | |
| Přidat tooa lab uživatelů |Ne |Ano |Ne |
| Aktualizovat nastavení náklady |Ne |Ano |Ano |
| **Základní úkoly virtuálních počítačů** | | | |
| Přidání a odebrání vlastních bitových kopií |Ne |Ano |Ano |
| Přidat, aktualizovat a odstranit vzorce |Ano |Ano |Ano |
| Seznam povolených adres Azure Marketplace obrázků |Ne |Ano |Ano |
| **Úkoly virtuálních počítačů** | | | |
| Vytvoření virtuálních počítačů |Ano |Ano |Ano |
| Spuštění, zastavení a odstranění virtuální počítače |Pouze virtuální počítače vytvořené uživatelem hello |Ano |Ano |
| Aktualizovat zásady virtuálních počítačů |Ne |Ano |Ano |
| Přidání nebo odebrání datových disků z virtuálních počítačů |Pouze virtuální počítače vytvořené uživatelem hello |Ano |Ano |
| **Artefaktů úlohy** | | | |
| Přidání a odebrání úložiště artefaktů |Ne |Ano |Ano |
| Použít artefaktů |Ano |Ano |Ano |

> [!NOTE]
> Pokud uživatel vytvoří virtuální počítač, tento uživatel bude automaticky přiřazen toohello **vlastníka** role hello vytvoření virtuálního počítače.
> 
> 

## <a name="add-an-owner-or-user-at-hello-lab-level"></a>Přidat vlastníkem nebo uživateli na úrovni hello testovacího prostředí
Vlastníci a uživatelé mohou být přidány na úrovni testovacím hello prostřednictvím hello portálu Azure. To zahrnuje externí uživatele s platnou [účet Microsoft (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).
Hello následující kroky vás provedou hello proces přidávání tooa laboratoře protokolu vlastníka nebo uživatele v Azure DevTest Labs:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.
3. Ze seznamu hello labs vyberte požadované prostředí hello.
4. V okně prostředí hello vyberte **konfigurace**. 
5. Na hello **konfigurace** vyberte **uživatelé**.
6. Na hello **uživatelé** vyberte **+ přidat**.
   
    ![Přidání uživatele](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. Na hello **vyberte roli** okně, vyberte hello požadované role. Hello části [akce, které mohou být prováděny v každé role](#actions-that-can-be-performed-in-each-role) seznamy hello různé akce, které lze provést pomocí uživatelé v rolích hello vlastník, uživatel DevTest a Přispěvatel.
8. Na hello **přidat uživatele** okno, zadejte hello e-mailovou adresu nebo název hello uživatele chcete tooadd hello role, které jste zadali. Pokud uživatel hello nelze nalézt, chybová zpráva vysvětluje hello problém. Pokud je nalezen hello uživatele, tento uživatel je uvedena v seznamu a vybrané. 
9. Vyberte **vyberte**.
10. Vyberte **OK** tooclose hello **přidat přístup** okno.
11. Když se vrátíte toohello **uživatelé** okně hello uživatel přidaný.  

## <a name="add-an-external-user-tooa-lab-using-powershell"></a>Přidat testovacím tooa externího uživatele pomocí prostředí PowerShell
Kromě toho tooadding uživatele v hello portálu Azure, můžete přidat prostředí tooyour externího uživatele pomocí skriptu prostředí PowerShell. V hello následující ukázka, stačí upravit hodnoty parametrů hello pod hello **toochange hodnoty** komentář.
Můžete načíst hello `subscriptionId`, `labResourceGroup`, a `labName` hodnoty v okně prostředí hello v hello portálu Azure.

> [!NOTE]
> Hello ukázkový skript předpokládá, že tento hello zadaný uživatel byl přidán jako hosta toohello služby Active Directory a se nezdaří, pokud to není hello případ. tooadd uživatel není v hello tooa testovací prostředí služby Active Directory, pomocí hello Azure portálu tooassign hello uživatele tooa role podle pokynů v části hello [přidat vlastníkem nebo uživateli na úrovni testovacím hello](#add-an-owner-or-user-at-the-lab-level).   
> 
> 

    # Add an external user in DevTest Labs user role tooa lab
    # Ensure that guest users can be added toohello Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values toochange
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select hello Azure subscription that contains hello lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve hello user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create hello role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-hello-subscription-level"></a>Přidat vlastníkem nebo uživateli na úrovni předplatného hello
Azure oprávnění rozšířeny z nadřazeného oboru toochild oboru v Azure. Proto vlastníci předplatné Azure, který obsahuje labs jsou automaticky vlastníků těchto laboratoře. Vlastní také hello virtuální počítače a další prostředky vytvořené hello lab uživatelů a hello Azure DevTest Labs služby. 

Můžete přidat další vlastníky tooa testovacím prostřednictvím hello testovacím okno v hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). Ale hello přidat obor vlastníka správy je užším než vlastník předplatného hello oboru. Například hello přidat vlastníky nemají úplný přístup toosome hello prostředků, které vytvářejí v rámci předplatného hello hello DevTest Labs služby. 

tooadd tooan vlastníka předplatného Azure, postupujte takto:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **odběry** hello seznamu.
3. Vyberte předplatné hello potřeby.
4. Vyberte **přístup** ikonu. 
   
    ![Uživatelé přístup](./media/devtest-lab-add-devtest-user/access-users.png)
5. Na hello **uživatelé** vyberte **přidat**.
   
    ![Přidání uživatele](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. Na hello **vyberte roli** okně vyberte **vlastníka**.
7. Na hello **přidat uživatele** okno, zadejte hello e-mailovou adresu nebo název hello uživatele chcete tooadd jako vlastníka. Pokud hello uživatele nelze nalézt, zobrazí chybovou zprávu vysvětlením hello problém. Pokud uživatel hello je nalezen, je tento uživatel uveden v části hello **uživatele** textové pole.
8. Vyberte hello nachází uživatelské jméno.
9. Vyberte **vyberte**.
10. Vyberte **OK** tooclose hello **přidat přístup** okno.
11. Když se vrátíte toohello **uživatelé** okně hello uživatel přidaný jako vlastníka. Tento uživatel je teď vlastníkem žádné labs vytvořil v rámci tohoto předplatného a proto být schopný tooperform vlastník úlohy. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

