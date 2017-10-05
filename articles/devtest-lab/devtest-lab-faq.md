---
title: "Nejčastější dotazy k Azure DevTest Labs | Microsoft Docs"
description: "Odpovědi na běžné otázky Azure DevTest Labs"
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
ms.openlocfilehash: 8ee4a0fd714027cdc77247ec8dc259fe62442564
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-devtest-labs-faq"></a>Nejčastější dotazy k Azure DevTest Labs
Tento článek obsahuje odpovědi na některé nejčastější dotazy o Azure DevTest Labs.

**Obecné**
## <a name="what-if-my-question-isnt-answered-here"></a>Co když není zde odpovědi můj dotaz?
Pokud váš dotaz není zde uvedeno, dejte nám vědět, pomůžeme vám najít odpověď.

* Odeslat dotaz v [vlákna služby Disqus](#comments) na konci tohoto článku a provozovat s týmem Azure Cache a ostatními členy komunity o tomto článku.
* K dosažení širší cílové skupiny, odeslat dotaz na [fórum Azure DevTest Labs MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)a spojte s týmem Azure DevTest Labs i ostatním členům komunity.
* Chcete-li žádosti o funkci, odeslání požadavků a nápady, jak [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Proč používat Azure DevTest Labs?
Azure DevTest Labs můžete uložit team čas a peníze. Vývojáři můžou vytvořit svoje vlastní prostředí pomocí několika různých základů a využívat artefakty k rychlému nasazení a konfigurace aplikací. Pomocí vlastních bitových kopií a vzorce, virtuální počítače lze uložit jako šablona a snadno reprodukovat. Kromě toho labs nabízí několik konfigurovat zásady, které umožňují správcům testovacím plýtvání a spravovat týmových prostředí. Tyto zásady obsahují auto vypnutí, prahová hodnota náklady, maximální virtuálních počítačů na uživatele a maximální velikosti virtuálních počítačů. Podrobnější vysvětlení Azure DevTest Labs, přečtěte si [přehled](devtest-lab-overview.md) nebo sledování [úvodní video](/documentation/videos/videos/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Co znamená "bez obav, samoobslužné služby"?
"Bez obav, samoobslužných" znamená, že vývojáři a testerům, sada vytvořit svoje vlastní prostředí podle potřeby a správci mají zabezpečení zároveň budete vědět, že Azure DevTest Labs pomáhá minimalizovat odpady a řídit náklady. Správci mohou určit, které velikosti virtuálních počítačů je povoleno, maximální počet virtuálních počítačů, a když jsou virtuální počítače spuštění a vypnutí. Azure DevTest Labs také umožňuje snadno monitorovat náklady a nastavit výstrahy zůstane vědět, jak jsou používány prostředky v testovacím prostředí.

## <a name="how-can-i-use-azure-devtest-labs"></a>Jak můžete použít Azure DevTest Labs?
Azure DevTest Labs je užitečná, když vyžadují dev nebo testovací prostředí, a chcete rychle reprodukovat nebo spravovat s náklady na ukládání zásad.

Tady je několik scénářů, které používají Azure DevTest Labs pro naše zákazníky:

* Správa prostředí vývoje a testování na jednom místě, použití zásad ke snížení nákladů a vlastní Image do sdílené složky sestavení napříč týmem.
* Vývoj aplikace pomocí vlastních bitových kopií pro uložení stavu disku v průběhu fáze vývoj.
* Sledování nákladů ve vztahu k průběh.
* Vytváření velkokapacitního testovací prostředí pro testování záruku kvality.
* Pomocí artefaktů a vzorce snadno konfigurovat a reprodukujte aplikaci v různých prostředích.
* Distribuci virtuálních počítačů pro hackathons (pracovní spolupráce pro vývoj nebo testování) a pak snadno zrušte zřizování je při ukončení události.

## <a name="how-am-i-billed-for-azure-devtest-labs"></a>Jak se fakturuje pro Azure DevTest Labs?
Azure DevTest Labs je bezplatná služba, tzn. vytváření labs a konfiguraci zásad, šablony a artefaktů je bezplatná. Platí pouze pro prostředky Azure používá v rámci vašeho prostředí, například virtuální počítače, účty úložiště a virtuální sítě. Další informace o náklady na prostředky testovacího prostředí, přečtěte si informace o [ceny Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Zabezpečení**
## <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Jaké jsou různé úrovně zabezpečení v Azure DevTest Labs?
Zabezpečení přístupu je dáno [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-built-in-roles.md). Abyste pochopili, jak přístup funguje, pomáhá pochopit rozdíly mezi oprávnění, role a obor podle definice RBAC.

* **Oprávnění** -oprávnění je definovaný přístup k určité akce. Oprávnění může být například přístup pro čtení pro všechny virtuální počítače.
* **Role** -role je sadu oprávnění, které mohou být seskupeny a přiřazen k uživateli. Například "vlastník předplatného" má přístup ke všem prostředkům v rámci předplatného.
* **Obor** -obor je úrovní v hierarchii prostředků Azure. Obor může být například skupinu prostředků nebo jednoho testovacího prostředí nebo celé předplatné.

V rámci oboru Azure DevTest Labs, existují dva typy rolí k definování oprávnění uživatele: vlastníka testovacího prostředí a testovacího prostředí uživatele.

* **Vlastník testovacím** -vlastníka testovacího prostředí má přístup k žádným prostředkům v testovacím prostředí. Proto se můžete upravit zásady, číst a zapisovat všechny virtuální počítače, změňte virtuální síť a tak dále.
* **Uživatel Lab** – uživatel testovacího prostředí můžete zobrazit všechny prostředky testovacího prostředí, jako jsou virtuální počítače, zásady a virtuální sítě, ale nemohou upravovat zásady nebo všechny virtuální počítače vytvořené jinými uživateli. Je také možné vytvářet vlastní role v Azure DevTest Labs a zjistěte, jak to provést v článku, [udělení uživatelských oprávnění k zásad konkrétní testovacího](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Vzhledem k tomu, že obory jsou hierarchické, když má uživatel oprávnění u určité oboru, jsou automaticky přiděleno těchto oprávnění v každé nižší úrovni oboru zahrnut. Například pokud uživatel je přiřazen k roli vlastník předplatného, pak mají přístup ke všem prostředkům v předplatném. Tyto prostředky zahrnují všechny virtuální počítače, všechny virtuální sítě a všechny laboratoře. Proto vlastník předplatného automaticky převezme roli vlastníka testovacího prostředí. Jako opak však není pravda. Vlastník testovacího prostředí má přístup k testovacím prostředí, které je obor nižší než úroveň předplatného. Vlastník testovacího prostředí je proto není moci zobrazit virtuální počítače nebo virtuální sítě nebo všechny prostředky, které jsou mimo testovací prostředí.

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Jak vytvořit roli, aby uživatelé mohli provádět konkrétní úlohu?
Komplexní článek o tom, jak vytvořit vlastní role a přiřaďte oprávnění k této roli je zde uveden. Tady je příklad skriptu, který vytvoří roli "DevTest Labs Advanced uživatel", který má oprávnění pro spuštění a zastavení všech virtuálních počítačů v testovacím prostředí:

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
Pokud používáte služby VSTS, je [rozšíření Azure DevTest Labs úloh](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , které umožňuje automatizovat svůj verze kanál v Azure DevTest Labs. Použití tohoto rozšíření patří:

* Vytvoření a nasazení virtuálního počítače automaticky a konfigurace s nejnovější build pomocí Azure File Copy nebo prostředí PowerShell služby VSTS úloh.
* Automaticky zaznamenávání stavu virtuálního počítače po testování pro reprodukci chyby na stejného virtuálního počítače pro další šetření.
* Virtuální počítač na konci kanálu verze odstranění, když už ho nepotřebují.

V následujících příspěvcích na blogu poskytnout pokyny a informace o používání služby VSTS rozšíření:

* [Azure DevTest Labs – rozšíření služby VSTS](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Nasazení nového virtuálního počítače v existující AzureDevTestLab ze služby VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [Pomocí správy verzí služby VSTS průběžné nasazení do AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Pro ostatní toolchains CI/CD může být podobně dosaženo všechny výše uvedené scénáře, které lze provádět pomocí rozšíření úloh služby VSTS prostřednictvím nasazení [šablon Azure Resource Manageru](https://aka.ms/dtlquickstarttemplate) pomocí [Azure Rutiny prostředí PowerShell](../azure-resource-manager/resource-group-template-deploy.md) a [sady .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Můžete také použít [rozhraní REST API pro DevTest Labs](http://aka.ms/dtlrestapis) integrovat s vaší nástrojů.  


**Virtual Machines**
## <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Proč nevidím některé virtuální počítače v okně virtuální počítače Azure, která je v Azure DevTest Labs?
Při vytváření virtuálního počítače v Azure DevTest Labs dostane oprávnění pro přístup k tohoto virtuálního počítače. Budete moci zobrazit i v okně prostředí a **virtuální počítače** okno. Uživatelé v roli DevTest Labs mohou vidět všechny virtuální počítače vytvořené v testovacím prostředí prostřednictvím tohoto prostředí **všechny virtuální počítače** okno. Uživatelé v roli DevTest Labs však nejsou automaticky udělit oprávnění ke čtení na prostředky virtuálních počítačů, které ostatní jste vytvořili. Proto nejsou tyto virtuální počítače zobrazují v **virtuální počítače** okno.

## <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Jaký je rozdíl mezi vlastních bitových kopií a vzorce?
Vlastní image je virtuální pevný disk (virtuální pevný disk), zatímco vzorec je obrázek, který můžete nakonfigurovat další nastavení, která můžete uložit a znovu vyvolali. Vlastní image může být vhodnější, pokud chcete rychle vytvořit více prostředích se stejnou základní, neměnné bitovou kopii. Vzorec je lepší, pokud chcete reprodukujte konfiguraci virtuálního počítače pomocí nejnovější bits, virtuální sítě a podsítě nebo jen určitá část. Podrobnější vysvětlení najdete v článku [porovnání vlastních bitových kopií a vzorce v DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Vytvoření víc virtuálních počítačů ze stejné šablony najednou?
Můžete použít [služby VSTS úloh rozšíření](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) nebo [Generovat šablonu Azure Resource Manager](devtest-lab-add-vm.md#save-azure-resource-manager-template) při vytváření virtuálního počítače a [nasazení šablony Azure Resource Manageru z prostředí Windows PowerShell](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Jak přesunout mé existující virtuální počítače Azure do mé testovacího prostředí Azure DevTest Labs?
Postupujte podle následujících kroků a zkopírovat stávající virtuální počítače do Azure DevTest Labs:

1. Zkopírujte soubor virtuálního pevného disku vašeho stávajícího virtuálního počítače pomocí této [skript prostředí Windows PowerShell](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1)
2. [Vytvořit vlastní image](devtest-lab-create-template.md) uvnitř testovacího prostředí Azure DevTest Labs.
3. Vytvoření virtuálního počítače v testovacím prostředí z vlastní image

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>Můžete připojit více disků do virtuálních počítačů?
Připojení více disků do virtuálních počítačů je podporováno.  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Pokud chcete použít bitovou kopii operačního systému Windows pro moje testování, je nutné zakoupit předplatné MSDN?
Pokud budete muset použít bitové kopie klientského operačního systému Windows (Windows 7 nebo novější) pro vaše vývoj nebo testování v Azure, pak Ano, musíte buď:

- [Kupte si předplatné MSDN](https://www.visualstudio.com/products/how-to-buy-vs).
- Pokud máte smlouvu Enterprise Agreement, vytvořte předplatné Azure s [Enterprise pro vývoj/testování nabídka](https://azure.microsoft.com/en-us/offers/ms-azr-0148p).

Další informace o kredity Azure pro každou nabídku MSDN najdete v tématu [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Jak mohu automatizovat proces odesílání soubory virtuálního pevného disku pro vytvoření vlastní Image?
Existují dvě možnosti:

* [Azure AzCopy](../storage/common/storage-use-azcopy.md#blob-upload) lze zkopírovat nebo nahrání souborů virtuálního pevného disku do účtu úložiště přidruženého k testovacím prostředí.
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, která běží na systému Windows, na OSX a Linux.   

Najít cílový účet úložiště přidružené testovacího prostředí, postupujte takto:

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **skupiny prostředků** v levém panelu.
3. Vyhledejte a vyberte skupinu prostředků, které jsou přidružené k testovacího prostředí.
4. Na **přehled** okně, vyberte jednu z účty úložiště.
5. Vyberte **objekty BLOB**.
6. Podívejte se na odeslání v seznamu. Pokud žádný neexistuje, vraťte ke kroku &#4; a zkuste to jiný účet úložiště.
7. Použití **URL** jako cíl v příkazu AzCopy.

## <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Jak můžete automatizovat proces odstranění všech virtuálních počítačů v mé prostředí?
Kromě odstraňte virtuální počítače z testovacího prostředí na portálu Azure, můžete odstranit všechny virtuální počítače ve vašem testovacím prostředí pomocí skriptu prostředí PowerShell. V následujícím příkladu upravte hodnoty parametru v části **hodnoty změnit** komentář. Můžete získat `subscriptionId`, `labResourceGroup`, a `labName` hodnoty v okně prostředí na portálu Azure.

    # Delete all the VMs in a lab

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount

    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }

**Artefakty**
## <a name="what-are-artifacts"></a>Jaké jsou artefakty?
Artefakty je přizpůsobitelné prvky, které lze použít k nasazení vaší nejnovější bits nebo vaše nástroje pro vývojáře na virtuální počítač. Během vytváření pomocí několika jednoduchých kliknutí jsou připojené k virtuálnímu počítači a po zřízení virtuálního počítače artefakty nasazení a konfigurace virtuálního počítače. Existují různé existující artefakty v našem [veřejného úložiště GitHub](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), ale můžete také snadno [vytvářet vlastní artefakty](devtest-lab-artifact-author.md).


**Konfiguraci testovacího prostředí**
## <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Vytvoření testovacího prostředí z šablony Azure Resource Manager
Uvádíme [úložiště GitHub šablon Azure Resource Manager testovacím](https://aka.ms/dtlquickstarttemplate) , kterou můžete nasadit jako-je nebo upravit můžete vytvořit vlastní šablony pro vaše prostředí. Každá z těchto šablon je odkaz, který můžete kliknout na nasazení testovacího prostředí jako-je v rámci předplatného Azure, nebo můžete upravit šablonu a [nasazení pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Proč se vytvoří virtuálních počítačů v různých skupinách prostředků s libovolné názvy? Můžete přejmenovat nebo upravit tyto skupiny prostředků?
Skupiny prostředků jsou vytvořené tímto způsobem, aby Azure DevTest Labs ke správě uživatelských oprávnění a přístup k virtuálním počítačům. Při virtuálního počítače můžete přesunout do jiné skupině prostředků s požadovaným názvem, se nedoporučuje proto to. Pracujeme na vylepšení toto prostředí umožňující větší flexibilitu.   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Kolik labs můžete vytvořit v rámci stejného předplatného?
Neexistuje žádné konkrétní omezení počtu laboratořích, které lze vytvořit jedno předplatné. Prostředky využívané jsou však omezená na jedno předplatné. Další informace o [kvót a omezení vynucená pro předplatná Azure](../azure-subscription-service-limits.md) a [postup tyto limity zvýšit](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Kolik virtuálních počítačů můžete vytvořit za testovacího prostředí
Neexistuje žádné konkrétní omezení na počet virtuálních počítačů, které lze vytvořit za testovacího prostředí. Prostředky využívané jsou však omezená na jedno předplatné (např. virtuální počítač jader a veřejné IP adresy, atd.). Další informace o [kvót a omezení vynucená pro předplatná Azure](../azure-subscription-service-limits.md) a [postup tyto limity zvýšit](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Jak sdílet přímý odkaz na můj testovacího prostředí?
Pokud chcete sdílet přímý odkaz na vaši uživatelé testovacího prostředí, můžete provádět následující postup:

1. Přejděte do testovacího prostředí na portálu Azure.
2. Zkopírujte adresu URL testovacího prostředí z prohlížeče a sdílet je s uživateli testovacího prostředí.

> [!NOTE]
> Pokud jsou externí uživatelé s vaši uživatelé testovacího prostředí [účtu Microsoft](#what-is-a-microsoft-account) a jejich nepatří do služby Active directory vaší společnosti, se může zobrazit chyba při navigaci na zadaný odkaz. Pokud se zobrazí chyba, upozorněte je, a klikněte na tlačítko jména v pravém horním rohu portálu Azure vyberte adresář, kde existuje testovací prostředí z **Directory** části nabídky.
>
>

## <a name="what-is-a-microsoft-account"></a>Co je účet Microsoft?
Účet Microsoft je použít pro téměř vše, co se děje s Microsoft zařízení a služeb. Je e-mailovou adresu a heslo, které používáte k přihlášení a Skype, Outlook.com, OneDrive, Windows Phone, Xbox LIVE – a znamená soubory, fotografie, kontakty a nastavení vám může používat pro všechna zařízení.

> [!NOTE]
> Účet Microsoft dřív říkalo "Windows Live ID".
>
>


**Řešení potíží**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Moje artefaktů došlo k chybě během vytváření virtuálních počítačů. Jak odstranit ji?
Odkazovat na [postup diagnostikování selhání artefaktů v DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md) se dozvíte, jak získat protokoly týkající se vaší selhání artefaktů.

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Není proč můj stávající virtuální sítě ukládání správně?
Jednou z možností je, že název virtuální sítě obsahuje tečky. Pokud ano, zkuste odebrání období nebo je nahraďte pomlčky a pak zkuste znovu uložit virtuální sítě.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-provisioning-a-vm-from-powershell"></a>Proč se při zřizování virtuálních počítačů z prostředí PowerShell zobrazí chybu "Nadřazený prostředek nebyl nalezen"?
Pokud jeden prostředek je nadřazený na jiný prostředek, musí existovat nadřazený prostředek před vytvořením podřízené prostředků. Pokud neexistuje, zobrazí se **ParentResourceNotFound** chyba. Pokud nezadáte závislost na nadřazeném prostředku, před nadřazený může být nasazený podřízených prostředků.

Virtuální počítače jsou podřízené prostředky v testovacím prostředí ve skupině prostředků. Při použití šablon Azure Resource Manager k nasazení pomocí prostředí PowerShell, název skupiny prostředků uvedené v skriptu prostředí PowerShell by měl být název skupiny prostředků v prostředí. Další informace najdete v tématu [odstraňování běžných chyb nasazení Azure ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Kde můžete najít další informace o chybě, pokud se nezdaří nasazení virtuálního počítače?
Chyby nasazení virtuálního počítače zachyceny v protokoly aktivity. Testovacího prostředí můžete najít protokoly aktivity virtuálních počítačů prostřednictvím **protokoly auditu** nebo **diagnostiky virtuálního počítače** v nabídce prostředků v okně v prostředí virtuálního počítače (v okně se zobrazí po výběru virtuálního počítače z **Moje virtuální počítače** seznamu).

V některých případech k chybě nasazení dojde před zahájením nasazování virtuálních počítačů – například když dojde k překročení limitu předplatného pro prostředek vytvořené pomocí virtuálních počítačů. V takovém případě se podrobnosti o chybě zachyceny v testovacím úroveň **protokoly aktivity** který lze najít v dolní části **konfiguraci a zásady** nastavení. Další informace o používání aktivita přihlásí Azure, najdete v části [zobrazit protokoly aktivity akce u prostředků](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
