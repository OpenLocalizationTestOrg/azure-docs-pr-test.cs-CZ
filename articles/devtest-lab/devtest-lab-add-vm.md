---
title: "aaaAdd tooa testovacího prostředí virtuálních počítačů v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tooadd testovacího prostředí tooa virtuálního počítače v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: tarcher
ms.openlocfilehash: 82838e4349550db56de311264c188140b9556b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-vm-tooa-lab-in-azure-devtest-labs"></a>Přidání testovacího prostředí tooa virtuálních počítačů v Azure DevTest Labs
Pokud už máte [vytvoření vaší první virtuální počítač](devtest-lab-create-first-vm.md), pravděpodobně jste to v předem načtený [marketplace image](devtest-lab-configure-marketplace-images.md). Nyní, pokud chcete tooadd dalších virtuálních počítačů tooyour testovacího prostředí, můžete také *základní* to znamená buď [vlastní image](devtest-lab-create-template.md) nebo [vzorec](devtest-lab-manage-formulas.md). Tento kurz vás provede pomocí hello Azure portálu tooadd testovacího virtuálního počítače tooa prostředí v DevTest Labs.

Tento článek také ukazuje, jak toomanage hello artefaktů pro virtuální počítač ve svém testovacím prostředí.

## <a name="steps-tooadd-a-vm-tooa-lab-in-azure-devtest-labs"></a>Kroky tooadd tooa testovacího prostředí virtuálních počítačů v Azure DevTest Labs
1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
1. Ze seznamu hello labs vyberte hello testovacího prostředí, ve kterém chcete toocreate hello virtuálních počítačů.  
1. V testovacím hello **přehled** vyberte **+ přidat**.  

    ![Virtuální počítač tlačítko Přidat](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Na hello **zvolte na základní** okně vyberte základ pro hello virtuálních počítačů.
1. Na hello **virtuálního počítače** okno, zadejte název pro nový virtuální počítač hello v hello **název virtuálního počítače** textové pole.

    ![Okno prostředí virtuálních počítačů](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Zadejte **uživatelské jméno** , jsou udělena oprávnění správce na virtuálním počítači hello.  
1. Pokud chcete, aby toouse heslo uložené v vaše [tajný úložiště](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), vyberte **použít uložené tajný klíč**a zadejte hodnotu klíče, která odpovídá tooyour tajný klíč (heslo). Jinak, zadejte heslo do hello textové pole s názvem bez přípony **zadejte hodnotu**.
1. Hello **typ disku virtuálního počítače** Určuje, jaký typ úložiště disku je povoleno pro hello virtuálních počítačů v testovacím prostředí hello.
1. Vyberte **velikost virtuálního počítače** a vyberte jednu z hello předdefinované položky, které zadejte hello jader procesoru, velikost paměti RAM a velikost pevného disku hello toocreate hello virtuálních počítačů.
1. Vyberte **artefakty** a - z hello seznam artefakty - vyberte a nakonfigurujte hello artefakty, že chcete tooadd toohello základní image.
    **Poznámka:** nové Labs tooDevTest nebo toohello konfigurace artefaktů, najdete v [přidat existující artefaktů tooa virtuálních počítačů](#add-an-existing-artifact-to-a-vm) části a vraťte se sem po dokončení.
1. Vyberte **upřesňující nastavení** tooconfigure hello síť virtuálních počítačů na možnosti možnosti a vypršení platnosti. 

   tooset možnost vypršení platnosti, zvolte ikonu toospecify kalendáře hello, datum, na kterém hello virtuální počítač bude automaticky odstraněna.  Ve výchozím nastavení hello virtuálních počítačů nikdy nevyprší. 
1. Chcete-li tooview nebo zkopírujte hello šablony Azure Resource Manageru, najdete v toohello [šablony uložit Azure Resource Manageru](#save-azure-resource-manager-template) části a sem vraťte po dokončení.
1. Vyberte **vytvořit** tooadd hello zadaný testovacím toohello virtuálních počítačů.
1. Hello testovacím zobrazuje hello stav vytvoření hello VM - nejprve jako **vytváření**, pak jako **systémem** po hello virtuálního počítače byla spuštěna.

> [!NOTE]
> [Přidat virtuální počítač vymahatelných](devtest-lab-add-claimable-vm.md) ukazuje, jak toomake hello vymahatelných virtuálních počítačů, aby bylo k dispozici pro použití v testovacím prostředí hello žádný uživatel.
>
>

## <a name="add-an-existing-artifact-tooa-vm"></a>Přidat existující artefaktů tooa virtuálních počítačů
Při vytváření virtuálního počítače, můžete přidat existující artefakty. Každý laboratoř zahrnuje artefakty z hello veřejného úložiště artefaktů DevTest Labs i artefakty, že jste vytvořili a přidané tooyour vlastní úložiště artefaktů.

* Azure DevTest Labs *artefakty* umožňují zadat *akce* , provede, když je zřízený hello virtuálních počítačů, jako je například spouštění skriptů prostředí Windows PowerShell, Bash příkazy a instalaci softwaru.
* Artefaktů *parametry* umožňují přizpůsobit hello artefaktů pro konkrétní scénář

hello toodiscover jak toocreate artefaktů, najdete v článku [zjistěte, jak tooauthor vlastních artefaktů pro použití s DevTest Labs](devtest-lab-artifact-author.md).

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
1. Ze seznamu hello labs vyberte hello testovacím obsahující hello virtuálních počítačů, pro který chcete toowork.  
1. Vyberte **Můj virtuální počítače**.
1. Vyberte hello požadovaných virtuálních počítačů.
1. Vyberte **artefakty**. 
1. Vyberte **použít artefakty**.
1. Na hello **použít artefakty** okně, vyberte hello artefaktů chcete tooadd toohello virtuálních počítačů.
1. Na hello **přidat artefaktů** okno, zadejte hodnoty parametrů hello požadované a volitelné parametry, které potřebujete.  
1. Vyberte **přidat** tooadd hello artefaktů a vraťte se toohello **použít artefakty** okno.
1. Pokračujte v přidávání artefakty potřebné pro virtuální počítač.
1. Po přidání artefakty, můžete [změnit hello pořadí, ve které hello artefakty spouštějí](#change-the-order-in-which-artifacts-are-run). Můžete také přejít zpět příliš[zobrazit nebo upravit artefakt](#view-or-modify-an-artifact).
1. Po dokončení přidávání artefakty, vyberte **použít**

## <a name="change-hello-order-in-which-artifacts-are-run"></a>Změňte hello pořadí, ve kterém jsou spuštěné artefakty
Ve výchozím nastavení spouštění akcí hello hello artefaktů v hello pořadí, ve kterém byly přidány toohello virtuálních počítačů. Hello následující kroky popisují jak toochange hello pořadí, ve které hello jsou spuštěny artefakty.

1. Hello horní části hello **použít artefakty** okně, vyberte hello odkaz určující hello počet artefaktů, které byly přidány toohello virtuálních počítačů.
   
    ![Počet artefaktů přidat tooVM](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Na hello **vybrané artefakty** okno, přetáhněte hello artefakty do hello požadovaného pořadí. **Poznámka:** Pokud máte potíže při přetahování hello artefaktů, ujistěte se, že přetáhnete z levé straně hello artefaktů hello. 
1. Až to budete mít, vyberte **OK**.  

## <a name="view-or-modify-an-artifact"></a>Zobrazit nebo upravit artefakt
Hello následující kroky popisují jak tooview nebo upravte parametry hello artefakt:

1. Hello horní části hello **použít artefakty** okně, vyberte hello odkaz určující hello počet artefaktů, které byly přidány toohello virtuálních počítačů.
   
    ![Počet artefaktů přidat tooVM](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Na hello **vybrané artefakty** okně, vyberte hello artefaktu má tooview nebo upravit.  
1. Na hello **přidat artefaktů** okno, ujistěte se, všechny potřebné změny a vyberte **OK** tooclose hello **přidat artefaktů** okno.
1. Vyberte **OK** tooclose hello **vybrané artefakty** okno.

## <a name="save-azure-resource-manager-template"></a>Uložení šablony Azure Resource Manageru
Šablonu Azure Resource Manager nabízí deklarativní způsob toodefine opakovatelných nasazení. Hello následující kroky popisují, jak toosave hello šablony Azure Resource Manageru pro hello při vytváření virtuálního počítače.
Po uložení šablony Azure Resource Manageru hello můžete příliš[nasadit nové virtuální počítače v prostředí Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. Na hello **virtuálního počítače** vyberte **šablony ARM zobrazení**.
2. Na hello **šablony zobrazení Azure Resource Manageru** okně, vyberte hello text šablony.
3. Zkopírujte schránky toohello hello vybraný text.
4. Vyberte **OK** tooclose hello **šablony Azure Resource Manageru zobrazit okno**.
5. Otevřete textový editor.
6. Vložte text hello šablony ze schránky hello.
7. Uložte soubor hello pro pozdější použití.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Další kroky
* Jednou hello vytvořil virtuální počítač, můžete připojit toohello virtuálních počítačů tak, že vyberete **Connect** v okně hello Virtuálního počítače.
* Zjistěte, jak příliš[vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs](devtest-lab-artifact-author.md).
* Prozkoumejte hello [Galerie šablon DevTest Labs Azure Resource Manager QuickStart](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
