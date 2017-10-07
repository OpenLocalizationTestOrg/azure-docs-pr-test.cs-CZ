---
title: "aaaAzure DevTest Labs – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi toocommon Azure DevTest Labs otázky"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: tarcher
ms.openlocfilehash: 07d4c870eca21856750a472ed503de4a2734a438
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-devtest-labs-faq"></a>Nejčastější dotazy k Azure DevTest Labs
Tento článek obsahuje odpovědi na některé z hello nejčastější dotazy týkající se Azure DevTest Labs.

**Obecné**
## <a name="what-if-my-question-isnt-answered-here"></a>Co když není zde odpovědi můj dotaz?
Pokud váš dotaz není zde uvedeno, dejte nám vědět, pomůžeme vám najít odpověď.

* Odeslat dotaz hello [vlákna služby Disqus](#comments) na hello konci tohoto článku a zapojit tým Azure Cache hello a ostatními členy komunity o tomto článku.
* tooreach širší cílovou skupinu, odeslat dotaz na hello [fórum Azure DevTest Labs MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)a zapojit týmu Azure DevTest Labs hello i ostatním členům komunity hello.
* toomake žádosti o funkci, odeslání vaší žádosti a nápady toohello [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Proč používat Azure DevTest Labs?
Azure DevTest Labs můžete uložit team čas a peníze. Vývojáři můžou vytvořit svoje vlastní prostředí pomocí několika různých základů a použít artefakty tooquickly nasazení a konfigurace aplikací. Pomocí vlastních bitových kopií a vzorce, virtuální počítače lze uložit jako šablona a snadno reprodukovat. Kromě toho labs nabízí několik konfigurovat zásady, které správci testovacím tooreduce odpady a spravovat prostředí týmových. Tyto zásady obsahují auto vypnutí, prahová hodnota náklady, maximální virtuálních počítačů na uživatele a maximální velikosti virtuálních počítačů. Podrobnější vysvětlení Azure DevTest Labs, přečtěte si hello [přehled](devtest-lab-overview.md) nebo sledovat hello [úvodní video](/documentation/videos/videos/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Co znamená "bez obav, samoobslužné služby"?
"Bez obav, samoobslužných" znamená, že vývojáři a testerům, sada vytvořit svoje vlastní prostředí podle potřeby a správci mají hello jistota, že Azure DevTest Labs pomáhá minimalizovat odpady a řídit náklady. Správci mohou určit, které velikosti virtuálních počítačů je povoleno, hello maximální počet virtuálních počítačů, a když jsou virtuální počítače spuštění a vypnutí. Azure DevTest Labs také umožňuje snadno toomonitor náklady a sada výstrahy toostay vědět, jak jsou používány prostředky v testovacím prostředí hello.

## <a name="how-can-i-use-azure-devtest-labs"></a>Jak můžete použít Azure DevTest Labs?
Azure DevTest Labs je užitečná, když vyžadují dev nebo testovací prostředí a chcete, aby tooreproduce je rychle nebo spravovat s náklady na ukládání zásad.

Tady je několik scénářů, které používají Azure DevTest Labs pro naše zákazníky:

* Správa prostředí vývoje a testování na jednom místě, využívá zásady tooreduce náklady a vlastních bitových kopií tooshare sestavení napříč hello týmu.
* Vývoj aplikace pomocí vlastních bitových kopií toosave hello stav disku v průběhu fáze vývoj hello.
* Sledování hello náklady v tooprogress vztah.
* Vytváření velkokapacitního testovací prostředí pro testování záruku kvality.
* Pomocí artefaktů a vzorce tooeasily konfigurace a reprodukujte aplikaci v různých prostředích.
* Distribuci virtuálních počítačů pro hackathons (pracovní spolupráce pro vývoj nebo testování) a pak snadno zrušte zřizování je při ukončení hello událostí.

## <a name="how-am-i-billed-for-azure-devtest-labs"></a>Jak se fakturuje pro Azure DevTest Labs?
Azure DevTest Labs je bezplatná služba, tzn. vytváření labs a konfiguraci zásad hello, šablony a artefaktů je bezplatná. Platíte jenom za hello prostředků Azure používá v rámci vašeho prostředí, například virtuální počítače, účty úložiště a virtuální sítě. Další informace o hello náklady prostředky testovacího prostředí, přečtěte si informace o [ceny Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Zabezpečení**
## <a name="what-are-hello-different-security-levels-in-azure-devtest-labs"></a>Jaké jsou hello různé úrovně zabezpečení v Azure DevTest Labs?
Zabezpečení přístupu je dáno [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-built-in-roles.md). toounderstand přístupu funguje, jak jsou definovány pomocí RBAC pomáhá toounderstand hello rozdíly mezi oprávnění, role a obor.

* **Oprávnění** -oprávnění je určitá akce tooa definované přístup. Oprávnění může být například přístup pro čtení tooall virtuálních počítačů.
* **Role** -role je sadu oprávnění, která může být seskupené a přiřazení tooa uživatele. Například "vlastník předplatného" má přístup k prostředkům tooall v rámci předplatného.
* **Obor** -obor je úrovně uvnitř hierarchie hello prostředků Azure. Obor může být například skupinu prostředků nebo v rámci jednoho testovacího prostředí nebo hello celý předplatného.

V rámci oboru hello Azure DevTest Labs, existují dva typy rolí toodefine uživatelských oprávnění: vlastníka testovacího prostředí a testovacího prostředí uživatele.

* **Vlastník testovacím** -vlastníka testovacího prostředí má hello přístup k prostředkům tooany v testovacím prostředí hello. Proto se můžete upravit zásady, čtení a zápis všech virtuálních počítačů, změnit hello virtuální sítě a tak dále.
* **Uživatel Lab** – uživatel testovacího prostředí můžete zobrazit všechny prostředky testovacího prostředí, jako jsou virtuální počítače, zásady a virtuální sítě, ale nemohou upravovat zásady nebo všechny virtuální počítače vytvořené jinými uživateli. Je také možné toocreate vlastní role v Azure DevTest Labs, a dozvíte, jak toodo v článku hello [udělení uživatelských oprávnění zásad testovacího toospecific](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Vzhledem k tomu, že obory jsou hierarchické, když má uživatel oprávnění u určité oboru, jsou automaticky přiděleno těchto oprávnění v každé nižší úrovni oboru zahrnut. Například pokud uživatel přiřazený toohello roli vlastník předplatného, pak mají přístup k prostředkům tooall v předplatném. Tyto prostředky zahrnují všechny virtuální počítače, všechny virtuální sítě a všechny laboratoře. Proto vlastník předplatného automaticky zdědí hello roli vlastníka testovacího prostředí. Hello opačné však není pravda. Vlastník testovacího prostředí má laboratoř tooa přístupu, které je obor nižší než úroveň předplatného hello. Proto vlastník testovacího prostředí není možné toosee virtuální počítače nebo virtuální sítě nebo všechny prostředky, které jsou mimo hello testovacího prostředí.

## <a name="how-do-i-create-a-role-tooallow-users-tooperform-a-specific-task"></a>Vytvoření role tooallow uživatelé tooperform konkrétní úlohu?
Komplexní článek o tom, jak toocreate vlastní role a přiřaďte oprávnění toothat role je zde uveden. Tady je příklad skriptu, který vytvoří roli hello "DevTest Labs Advanced uživatel", který má oprávnění toostart a zastavení všech virtuálních počítačů v testovacím prostředí hello:

    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advance User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  


**Integrace CI/CD & automatizace**
## <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Umožňuje Azure DevTest Labs integraci s Moje CI/CD nástrojů?
Pokud používáte služby VSTS, je [rozšíření Azure DevTest Labs úloh](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , což vám umožní tooautomate vaší verze kanál v Azure DevTest Labs. Mezi hello používá tohoto rozšíření patří:

* Vytvoření a nasazení virtuálního počítače automaticky a konfigurace s nejnovější build hello pomocí Azure File Copy nebo prostředí PowerShell služby VSTS úloh.
* Automaticky zaznamenávání hello stavu virtuálního počítače po testování tooreproduce chyby v hello stejného virtuálního počítače pro další šetření.
* Odstraňování hello virtuálních počítačů na konci hello hello verze kanálu když už ho nepotřebují.

Hello těchto příspěvcích na blogu poskytnout pokyny a informace o použití rozšíření služby VSTS hello:

* [Azure DevTest Labs – rozšíření služby VSTS](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Nasazení nového virtuálního počítače v existující AzureDevTestLab ze služby VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [Pomocí služby VSTS Release Management pro tooAzureDevTestLabs průběžné nasazení](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Pro ostatní toolchains CI/CD všechny hello výše scénáře, které lze provádět pomocí hello rozšíření služby VSTS úloh může být podobně dosaženo pomocí nasazení [šablon Azure Resource Manageru](https://aka.ms/dtlquickstarttemplate) pomocí [ Rutiny Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) a [sady .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Můžete také použít [rozhraní REST API pro DevTest Labs](http://aka.ms/dtlrestapis) toointegrate s vaší nástrojů.  


**Virtual Machines**
## <a name="why-cant-i-see-certain-vms-in-hello-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Proč nevidím některé virtuální počítače v okně hello virtuální počítače Azure, která je v Azure DevTest Labs?
Při vytváření virtuálního počítače v Azure DevTest Labs oprávnění je zadána tooaccess tohoto virtuálního počítače. Je možné tooview ho v okně labs hello i hello **virtuální počítače** okno. Uživatelé v roli DevTest Labs hello mohou vidět všechny virtuální počítače vytvořené v testovacím prostředí hello prostřednictvím hello testovacím **všechny virtuální počítače** okno. Uživatelé v roli DevTest Labs hello však nejsou automaticky udělí přístup pro čtení tooVM prostředky, které ostatní jste vytvořili. Proto tyto virtuální počítače se nezobrazí v hello **virtuální počítače** okno.

## <a name="what-is-hello-difference-between-custom-images-and-formulas"></a>Jaký je rozdíl hello vlastních bitových kopií a vzorce?
Vlastní image je virtuální pevný disk (virtuální pevný disk), zatímco vzorec je obrázek, který můžete nakonfigurovat další nastavení, která můžete uložit a znovu vyvolali. Vlastní image může být vhodnější, pokud chcete vytvořit tooquickly několika prostředích s hello stejnou základní bitovou kopii neměnné. Vzorec je lepší, pokud chcete, aby tooreproduce hello konfiguraci virtuálního počítače pomocí nejnovější bits hello, virtuální sítě a podsítě nebo jen určitá část. Podrobnější vysvětlení najdete v článku hello, [porovnání vlastních bitových kopií a vzorce v DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-hello-same-template-at-once"></a>Jak vytvořit víc virtuálních počítačů z hello najednou stejné šablony?
Můžete použít hello [služby VSTS úloh rozšíření](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) nebo [Generovat šablonu Azure Resource Manager](devtest-lab-add-vm.md#save-azure-resource-manager-template) při vytváření virtuálního počítače a [nasazení šablony Azure Resource Manager hello z prostředí Windows PowerShell ](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Jak přesunout mé existující virtuální počítače Azure do mé testovacího prostředí Azure DevTest Labs?
Postupujte podle kroků hello toocopy existující virtuální počítače tooAzure DevTest Labs:

1. Kopírování souboru VHD hello vašeho stávajícího virtuálního počítače pomocí této [skript prostředí Windows PowerShell](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1)
2. [Vytvořit vlastní image hello](devtest-lab-create-template.md) uvnitř testovacího prostředí Azure DevTest Labs.
3. Vytvoření virtuálního počítače v testovacím prostředí hello z vlastní image

## <a name="can-i-attach-multiple-disks-toomy-vms"></a>Můžete připojit více toomy disky virtuálních počítačů?
Připojení více disků tooVMs je podporována.  

## <a name="if-i-want-toouse-a-windows-os-image-for-my-testing-do-i-have-toopurchase-an-msdn-subscription"></a>Pokud chcete toouse bitovou kopii operačního systému Windows pro moje testování, je nutné provést toopurchase předplatné MSDN?
Pokud budete potřebovat Image klientského operačního systému Windows toouse (Windows 7 nebo novější) pro vaše vývoj nebo testování v Azure, pak Ano, musíte buď:

- [Kupte si předplatné MSDN](https://www.visualstudio.com/products/how-to-buy-vs).
- Pokud máte smlouvu Enterprise Agreement, vytvořte si předplatné Azure s hello [Enterprise pro vývoj/testování nabídka](https://azure.microsoft.com/en-us/offers/ms-azr-0148p).

Další informace o hello kredity Azure pro každý nabídky MSDN, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-hello-process-of-uploading-vhd-files-toocreate-custom-images"></a>Jak mohu automatizovat proces hello odesílání virtuálního pevného disku soubory toocreate vlastní Image?
Existují dvě možnosti:

* [Azure AzCopy](../storage/common/storage-use-azcopy.md#blob-upload) lze použít toocopy nebo nahrání virtuálního pevného disku soubory toohello účtu úložiště přidruženého k testovacím hello.
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, která běží na systému Windows, na OSX a Linux.   

toofind hello cílový účet úložiště přidružené testovacího prostředí, postupujte takto:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **skupiny prostředků** z hello levém panelu.
3. Vyhledejte a vyberte skupinu prostředků hello přidružené testovacího prostředí.
4. Na hello **přehled** okně, vyberte jeden z účtů úložiště hello.
5. Vyberte **objekty BLOB**.
6. Podívejte se na odeslání v seznamu hello. Pokud žádný neexistuje, vraťte tooStep #4 a zkuste to jiný účet úložiště.
7. Použití hello **URL** jako cíl v příkazu AzCopy.

## <a name="how-can-i-automate-hello-process-of-deleting-all-hello-vms-in-my-lab"></a>Jak můžete automatizovat proces hello odstranění hello všechny virtuální počítače v mé prostředí?
Kromě toho toodeleting virtuální počítače z testovacího prostředí v hello portálu Azure, můžete odstranit všechny virtuální počítače hello ve svém testovacím prostředí pomocí skriptu prostředí PowerShell. V hello následující ukázka, upravte hodnoty parametrů hello pod hello **toochange hodnoty** komentář. Můžete načíst hello `subscriptionId`, `labResourceGroup`, a `labName` hodnoty v okně prostředí hello v hello portálu Azure.

    # Delete all hello VMs in a lab

    # Values toochange
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login tooyour Azure account
    Login-AzureRmAccount

    # Select hello Azure subscription that contains hello lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get hello lab that contains hello VMs toodelete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get hello VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete hello VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }

**Artefakty**
## <a name="what-are-artifacts"></a>Jaké jsou artefakty?
Artefakty je přizpůsobitelné prvky, které se dají použít toodeploy vaše nejnovější bits nebo vaše dev nástroje do virtuálního počítače. Během vytváření pomocí několika jednoduchých kliknutí jsou připojené tooyour virtuálních počítačů, a po zřízení virtuálního počítače hello hello artefakty nasazení a konfigurace virtuálního počítače. Existují různé existující artefakty v našem [veřejného úložiště GitHub](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), ale můžete také snadno [vytvářet vlastní artefakty](devtest-lab-artifact-author.md).


**Konfiguraci testovacího prostředí**
## <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Vytvoření testovacího prostředí z šablony Azure Resource Manager
Uvádíme [úložiště GitHub šablon Azure Resource Manager testovacím](https://aka.ms/dtlquickstarttemplate) , kterou můžete nasadit jako-je nebo upravit toocreate vlastních šablon pro vaše prostředí. Každá z těchto šablon je odkaz, který můžete kliknout na testovacím hello toodeploy jako-je v rámci předplatného Azure, nebo můžete přizpůsobit šablonu hello a [nasazení pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Proč se vytvoří virtuálních počítačů v různých skupinách prostředků s libovolné názvy? Můžete přejmenovat nebo upravit tyto skupiny prostředků?
Skupiny prostředků jsou vytvořené tímto způsobem, aby Azure DevTest Labs toomanage hello uživatelská oprávnění a přístupu toovirtual počítače. Když přesunete skupinu prostředků tooanother hello virtuální počítač s požadovaným názvem se nedoporučuje proto to. Pracujeme na vylepšení toto prostředí tooallow větší flexibilitu.   

## <a name="how-many-labs-can-i-create-under-hello-same-subscription"></a>Kolik labs je možné vytvořit v části hello stejného předplatného?
Neexistuje žádné konkrétní omezení počtu hello laboratořích, které lze vytvořit jedno předplatné. Hello prostředky využívané jsou však omezená na jedno předplatné. Další informace o hello [kvót a omezení vynucená pro předplatná Azure](../azure-subscription-service-limits.md) a [jak tooincrease tyto limity](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Kolik virtuálních počítačů můžete vytvořit za testovacího prostředí
Neexistuje žádné konkrétní omezení na hello počet virtuálních počítačů, které lze vytvořit za testovacího prostředí. Hello prostředky využívané jsou však omezená na jedno předplatné (např. virtuální počítač jader a veřejné IP adresy, atd.). Další informace o hello [kvót a omezení vynucená pro předplatná Azure](../azure-subscription-service-limits.md) a [jak tooincrease tyto limity](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-toomy-lab"></a>Jak sdílet testovacího prostředí toomy přímý odkaz?
tooshare přímý odkaz tooyour lab uživatelů, můžete provést hello následující postup:

1. Procházejte toohello laboratoře v hello portálu Azure.
2. Zkopírujte URL hello testovacího prostředí z prohlížeče a sdílet je s uživateli testovacího prostředí.

> [!NOTE]
> Pokud jsou externí uživatelé s vaši uživatelé testovacího prostředí [účtu Microsoft](#what-is-a-microsoft-account) a jejich nepatří Active directory tooyour společnosti, se může zobrazit chyba při navigaci toohello zadaný odkaz. Pokud se zobrazí chyba, upozorněte je, tooclick jména v pravém horním rohu hello hello portál Azure a vyberte hello adresáře, kde existuje hello testovacím z hello **Directory** hello nabídky.
>
>

## <a name="what-is-a-microsoft-account"></a>Co je účet Microsoft?
Účet Microsoft je použít pro téměř vše, co se děje s Microsoft zařízení a služeb. Je e-mailovou adresu a heslo, které použijete toosign tooSkype, Outlook.com, OneDrive, Windows Phone a Xbox LIVE – a jedná se o soubory, fotografie, kontakty a nastavení můžete podle tooany zařízení.

> [!NOTE]
> Účet Microsoft používá toobe názvem "Windows Live ID".
>
>


**Řešení potíží**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Moje artefaktů došlo k chybě během vytváření virtuálních počítačů. Jak odstranit ji?
Odkazovat příliš[jak toodiagnose artefaktů selhání v DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md) toolearn jak tooobtain protokoly týkající se vaší selhání artefaktů.

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Není proč můj stávající virtuální sítě ukládání správně?
Jednou z možností je, že název virtuální sítě obsahuje tečky. Pokud ano, zkuste odebrání hello tečky a jejich náhradu pomlčky a opakujte uložení znovu hello virtuální sítě.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-provisioning-a-vm-from-powershell"></a>Proč se při zřizování virtuálních počítačů z prostředí PowerShell zobrazí chybu "Nadřazený prostředek nebyl nalezen"?
Pokud jeden prostředek je prostředek tooanother nadřazené, hello nadřazený prostředek musí existovat před vytvořením hello podřízených prostředků. Pokud neexistuje, zobrazí se **ParentResourceNotFound** chyba. Pokud nezadáte závislost na nadřazeném prostředku hello, před hello nadřazené může být nasazený hello podřízených prostředků.

Virtuální počítače jsou podřízené prostředky v testovacím prostředí ve skupině prostředků. Pokud používáte toodeploy šablony Azure Resource Manager pomocí prostředí PowerShell, název skupiny prostředků hello součástí hello skript prostředí PowerShell musí být název skupiny prostředků hello hello prostředí. Další informace najdete v tématu [odstraňování běžných chyb nasazení Azure ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Kde můžete najít další informace o chybě, pokud se nezdaří nasazení virtuálního počítače?
Chyby nasazení virtuálního počítače zachyceny v protokoly aktivity hello. Testovacího prostředí můžete najít protokoly aktivity virtuálních počítačů prostřednictvím hello **protokoly auditu** nebo **diagnostiky virtuálního počítače** v nabídce hello prostředků v okně prostředí hello virtuálních počítačů (hello okno se zobrazí po výběru hello virtuálních počítačů z **Můj virtuální počítače** seznamu).

V některých případech hello nasazení dojde k chybě před zahájením hello nasazení virtuálního počítače – například když dojde k překročení hello limitu předplatného pro prostředek vytvořené pomocí hello virtuálních počítačů. V takovém případě podrobnosti o chybě hello zachyceny v úrovni testovacím hello **protokoly aktivity** který lze najít v hello dolní části hello **konfiguraci a zásady** nastavení. Další informace o používání aktivita přihlásí Azure, najdete v části [zobrazení aktivity protokoluje akce tooaudit na prostředcích](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
