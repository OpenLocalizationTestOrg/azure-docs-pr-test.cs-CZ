---
title: "aaaUse Azure DevTest Labs pro školení | Microsoft Docs"
description: "Zjistěte, jak toouse Azure DevTest Labs pro scénáře školení."
services: devtest-lab,virtual-machines
documentationcenter: na
author: steved0x
manager: douge
editor: 
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: sdanie
ms.openlocfilehash: 4a5f44a282d8f6a58849c730ff89237ccff39ca8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-training"></a>Použití Azure DevTest Labs pro školení
Azure DevTest Labs lze použít tooimplement mnoho klíčových scénářů v přidání toodev/testu. Jedním z těchto scénářů je tooset laboratoře pro školení. Azure DevTest Labs vám umožní toocreate testovacím prostředí, kde můžete zadat vlastní šablony, můžete každý praktikanta toocreate identické a izolované prostředí pro školení. Můžete použít zásady tooensure, školení prostředí jsou k dispozici tooeach praktikanta jenom v případě, že je potřebují a obsahovat dostatek prostředků – jako jsou virtuální počítače - požadované pro hello školení. Nakonec můžete snadno sdílet hello laboratoře s povolání, které mají přístup k jedním kliknutím.

![Používá DevTest Labs pro školení](./media/devtest-lab-training-lab/devtest-lab-training.png)

Azure DevTest Labs splňuje následující požadavky, které jsou požadované tooconduct školení v jakémkoli virtuální prostředí hello: 

* Povolání neuvidí virtuální počítače vytvořené jiných povolání
* Každý počítač školení by měly být shodné
* Povolání můžete rychle zřídit jejich prostředí školení
* Řízení nákladů tím zajistí, že povolání nelze získat více virtuálních počítačů, než potřebují pro hello školení a také vypnutí virtuálních počítačů, když nejsou na jejich používání
* Snadno sdílet s každou praktikanta hello školení testovacího prostředí
* Opakované použití hello školení testovacím znovu a znovu

V tomto článku můžete informace o různých funkcí Azure DevTest Labs, které lze použít, toomeet hello výše popsané požadavky na školení a podrobné pokyny vám pomůžou tooset laboratoře pro školení.  

## <a name="implementing-training-with-azure-devtest-labs"></a>Implementace školení s Azure DevTest Labs
1. **Vytvoření testovacího prostředí hello** 
   
    Labs jsou výchozí bod v Azure DevTest Labs hello. Po vytvoření testovacího prostředí, můžete provést tak, jak přidat uživatele (povolání) toohello prostředí, sadu zásad toocontrol náklady, definujte Image virtuálních počítačů, které lze rychle vytvořit a další úkoly.   
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Vytvoření testovacího prostředí v Azure DevTest Labs](devtest-lab-create-lab.md) |Zjistěte, jak hello toocreate a testovacího prostředí v Azure DevTest Labs v portálu Azure. |
2. **Vytvořit virtuální počítače školení během pár minut pomocí připravených marketplace Image a vlastních bitových kopií** 
   
    Můžete vybrat připravených bitové kopie z široké škály bitové kopie v hello Azure Marketplace a zpřístupnit na hello povolání v testovacím prostředí hello. Pokud připravených Image hello nevyhovují vašim požadavkům, můžete vytvořit vlastní image vytvořením virtuálního počítače pomocí bitové kopie připravených z Azure Marketplace, instalaci všech hello software, který budete potřebovat pro hello školení a ukládání hello virtuálního počítače jako vlastní image v testovacím prostředí hello testovacím prostředí. 
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Konfigurace Azure Marketplace obrázků](devtest-lab-configure-marketplace-images.md) |Přečtěte si, jak Image Azure Marketplace povolených; zpřístupnění pro výběr pouze hello Image chcete použít pro hello školení. |
   | [Vytvořit vlastní image](devtest-lab-create-template.md) |Vytvořte vlastní image před instalací hello softwaru, které potřebujete k hello školení tak, aby povolání dokáže rychle vytvořit virtuální počítač pomocí vlastní image hello. |
3. **Vytvořit opakovaně použitelný šablony pro školení počítače** 
   
    Vzorec v Azure DevTest Labs je, že seznam výchozí hodnoty vlastností použít toocreate virtuálního počítače. Vzorce můžete vytvořit v testovacím prostředí hello výběrem obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě. Každý praktikanta můžete najdete v části hello vzorec v testovacím prostředí hello a použít ho toocreate virtuálního počítače. 
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Spravovat DevTest Labs vzorce toocreate virtuální počítače](devtest-lab-manage-formulas.md) |Zjistěte, jak můžete vytvořit vzorec podle vyzvednutí obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě. |
4. **Náklady na ovládací prvek**
   
    Azure DevTest Labs umožňuje tooset zásadu hello testovacím toospecify hello maximálního povoleného počtu virtuálních počítačů, které lze vytvořit pomocí praktikanta v testovacím prostředí hello. 
   
    Pokud jsou provádění Vícedenní školení a chcete toostop všechny hello virtuální počítače v určitém čase hello den a poté automaticky je restartovat hello následující den, můžete snadno provést podle nastavení automatické ukončení a automatického spouštění zásad v testovacím prostředí hello. 
   
    Nakonec po dokončení školení můžete odstranit všechny virtuální počítače hello najednou spuštěním jednoho skriptu prostředí PowerShell. 
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Definice zásad testovacího prostředí](devtest-lab-set-lab-policy.md) |Řízení nákladů pomocí nastavení zásad v testovacím prostředí hello. |
   | [Odstranit všechny hello prostředí virtuálních počítačů pomocí skriptu prostředí PowerShell](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Po dokončení hello školení, odstraňte všechny labs hello v rámci jedné operace. |
5. **Sdílet s každou praktikanta hello testovacího prostředí**
   
    Labs lze přímo přistupovat pomocí odkazu, který sdílíte s vaší povolání. Vaše povolání dokonce ani nemusí toohave účet Azure, tak dlouho, dokud mají [účtu Microsoft](devtest-lab-faq.md#what-is-a-microsoft-account). Povolání neuvidí virtuální počítače vytvořené jiných povolání.  
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Přidání testovacího prostředí tooa praktikanta v Azure DevTest Labs](devtest-lab-add-devtest-user.md) |Použijte hello Azure portálu tooadd povolání tooyour školení prostředí. |
   | [Přidání prostředí toohello povolání pomocí skriptu prostředí PowerShell](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Pomocí prostředí PowerShell tooautomate přidání povolání tooyour školení testovacího prostředí. |
   | [Získání testovacího prostředí toohello odkaz](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Zjistěte, jak testovacího prostředí je přímo přístupný prostřednictvím hypertextový odkaz. |
6. **Opakované použití hello znovu a znovu prostředí** 
   
    Je možné automatizovat vytvoření testovacího prostředí, včetně vlastních nastavení, vytváření šablony Resource Manageru a použitím ho znovu a znovu toocreate identické laboratoře. 
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Vytvoření testovacího prostředí pomocí šablony Resource Manageru](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |Vytvoření prostředí v Azure DevTest Labs pomocí šablony Resource Manageru. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

