---
title: "aaaUse Azure DevTest Labs pro virtuální počítač a PaaS testovací prostředí | Microsoft Docs"
description: "Zjistěte, jak toouse Azure DevTest Labs pro virtuální počítač a PaaS scénáře prostředí otestovat."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: d4e2c334-643a-40c9-9051-625b8f39fc86
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: tarcher
ms.openlocfilehash: 9285090da768491e1275942318b094fae89e3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a>Použití Azure DevTest Labs pro virtuální počítač a PaaS testovací prostředí

Můžete použít Azure DevTest Labs tooimplement mnoho klíčových scénářů, ale primární scénář vyžaduje, abyste použili počítače toohost DevTest Labs pro testerům, sada. 

V tomto scénáři DevTest Labs poskytuje následující výhody:

- Testery můžete otestovat hello nejnovější verzi jejich aplikace rychle zřizování prostředí systému Windows a Linux pomocí opakovaně použitelné šablony a artefaktů.
- Testery můžete postupně škálovat jejich zatížení testování zřizování více testovacích agentů.
- Správci můžou řídit náklady, přičemž zajistí, aby:
  - Testery nelze získat více virtuálních počítačů, než potřebují.
  - Virtuální počítače jsou vypnutí při není používán.

![Používá DevTest Labs pro školení](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

V tomto článku Další informace o různých Azure DevTest Labs funkce, které používá toomeet tester požadavcích a hello podrobné kroky toofollow tooset laboratoře.

## <a name="implementing-test-environments-with-azure-devtest-labs"></a>Implementace testovacích prostředích s Azure DevTest Labs
1. **Vytvoření testovacího prostředí hello** 
   
    Labs jsou výchozí bod v Azure DevTest Labs hello. Po vytvoření testovacího prostředí, můžete provádět úlohy, jako je například přidávání toohello lab uživatelů (testerům, sada), nastavení zásady toocontrol náklady, definování Image virtuálních počítačů, které můžete rychle vytvořit a další.  
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Vytvoření testovacího prostředí v Azure DevTest Labs](devtest-lab-create-lab.md) |Zjistěte, jak hello toocreate a testovacího prostředí v Azure DevTest Labs v portálu Azure. |
2. **Vytvořit virtuální počítače během pár minut pomocí připravených marketplace Image a vlastních bitových kopií** 
   
    Můžete vybrat připravených bitové kopie z široké škály bitové kopie v Azure Marketplace hello a zpřístupnit v testovacím prostředí hello. Pokud připravených Image hello nevyhovují vašim požadavkům, můžete vytvořit vlastní image vytvořením virtuálního počítače pomocí připravených bitovou kopii z Azure Marketplace, instalace všech hello softwaru, které potřebujete, a ukládá hello virtuálního počítače jako vlastní image v testovacím prostředí hello testovacím prostředí.

    Pokud budete používat vlastní Image, zvažte použití toocreate objekt pro vytváření bitové kopie a distribuci bitové kopie. Objekt pro vytváření bitové kopie je jako kód konfigurace řešení, které pravidelně sestavení a automaticky distribuuje nakonfigurované obrázků. Tato uloží hello čas potřebný toomanually konfiguraci systému hello po vytvoření virtuálního počítače s hello základní operační systém.
  
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Konfigurace Azure Marketplace obrázků](devtest-lab-configure-marketplace-images.md) |Přečtěte si, jak povolených Image Azure Marketplace, zpřístupnění pro výběr pouze hello bitové kopie, kterou chcete použít pro testery hello.|
   | [Vytvořit vlastní image](devtest-lab-create-template.md) |Vytvořte vlastní image před instalací hello softwaru, které potřebujete, aby testery dokáže rychle vytvořit virtuální počítač pomocí vlastní image hello.|
   | [Další informace o vytváření bitové kopie](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Přehrát video, které popisuje, jak tooset až a použít objekt pro vytváření bitové kopie.|

3. **Vytvoření šablon opakovaně použitelné pro testovacích počítačů** 
   
    Vzorec v Azure DevTest Labs je, že seznam výchozí hodnoty vlastností použít toocreate virtuálního počítače. Vzorce můžete vytvořit v testovacím prostředí hello výběrem obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě. Každý tester můžete najdete v části hello vzorec v testovacím prostředí hello a použít ho toocreate virtuálního počítače. 
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Spravovat DevTest Labs vzorce toocreate virtuální počítače](devtest-lab-manage-formulas.md) |Zjistěte, jak můžete vytvořit vzorec podle vyzvednutí obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.|

3. **Vytvoření více virtuálních počítačů testovací prostředí** 
   
    Můžete použít infrastrukturu hello toodefine šablony Azure Resource Manager a konfigurace řešení Azure a opakovaně nasadit více testovací virtuální počítače v konzistentním stavu.

    Prostředky Azure PaaS může být zřízen v prostředí z šablony Resource Manageru a zobrazí v sledování nákladů. Vypnutí virtuálního počítače automaticky nevztahuje tooPaaS prostředky.

    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Vytvoření prostředí více virtuálních počítačů a prostředků PaaS pomocí šablony Azure Resource Manageru](devtest-lab-create-environment-from-arm.md) |Zjistěte, jak můžete nasadit víc virtuálních počítačů v konzistentním stavu pro testovací prostředí.|

4. **Vytvoření artefakty tooenable flexibilní přizpůsobení virtuálního počítače**

   Artefakty jsou použité toodeploy a nakonfigurovat svoji aplikaci po zřízení virtuálního počítače. Artefakty mohou být:

   - Nástroje, které chcete tooinstall na hello virtuálního počítače – například agentů, Fiddler a Visual Studio.
   - Akce, že chcete toorun na hello virtuálního počítače – například klonování úložišti.
   - Aplikace, které chcete tootest.

   Mnoho artefakty je již k dispozici out-of-the-box. Ale pokud chcete další přizpůsobení svých konkrétních potřeb, můžete vytvořit vlastní artefakty.

   Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs](devtest-lab-artifact-author.md) |Vytvoření vlastních artefaktů pro virtuální počítače hello ve svém testovacím prostředí.|
   | [Přidání vlastních artefaktů Git úložiště toostore a šablon Azure Resource Manageru pro použití v Azure DevTest Labs](devtest-lab-add-artifact-repo.md) |Zjistěte, jak toostore vlastní artefakty na vlastní privátní úložiště Git.|

5. **Náklady na ovládací prvek**
   
    Azure DevTest Labs umožňuje tooset zásadu hello testovacím toospecify hello maximálního povoleného počtu virtuálních počítačů, které lze vytvořit pomocí nástroje testování v testovacím prostředí hello. 
   
    Pokud má váš tým testovací sadu pracovní plán a chcete toostop všechny hello virtuální počítače v určitém čase hello dne a pak automaticky je restartovat hello následující den, můžete snadno dosáhnete pomocí nastavení automatické ukončení a automatického spouštění zásad v testovacím prostředí hello. 
   
    Nakonec po dokončení vývoj aplikací můžete odstranit všechny virtuální počítače hello najednou spuštěním jednoho skriptu prostředí PowerShell. 
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Definice zásad testovacího prostředí](devtest-lab-set-lab-policy.md) |Řízení nákladů pomocí nastavení zásad v testovacím prostředí hello. |
   | [Odstranit všechny hello prostředí virtuálních počítačů pomocí skriptu prostředí PowerShell](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Po dokončení testování, odstraňte všechny labs hello v rámci jedné operace.|

1. **Přidat virtuální síť tooa testovacího prostředí** 
   
    DevTest Labs vytvoří novou virtuální síť (VNET) vždy, když je vytvořena testovacím prostředí. Pokud jste nakonfigurovali vlastní virtuální síť – například s použitím ExpressRoute nebo VPN typu site-to-site – můžete přidat nastavení virtuální sítě v této laboratoři tooyour virtuální sítě tak, aby bylo dostupné při vytváření virtuálních počítačů.

    Kromě toho je Azure Active Directory domény spojení artefakt k dispozici, připojen do domény tooa virtuálních počítačů, když se vytváří hello virtuálních počítačů. 
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Konfigurace virtuální sítě v Azure DevTest Labs](devtest-lab-configure-vnet.md) |Zjistěte, jak hello tooconfigure virtuální sítě v Azure DevTest Labs pomocí portálu Azure.|

6. **Laboratoř hello sdílenou složku s každou tester**
   
    Labs lze přímo přistupovat pomocí odkazu, který sdílíte s vaší testerům, sada. I nemají toohave účet Azure, tak dlouho, dokud mají [účtu Microsoft](devtest-lab-faq.md#what-is-a-microsoft-account). Testery neuvidí vytvořit další testerům, sada virtuálních počítačů.  
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Přidání testovacího prostředí tooa tester v Azure DevTest Labs](devtest-lab-add-devtest-user.md) |Použijte hello Azure portálu tooadd testery tooyour testovacího prostředí.|
   | [Přidání prostředí toohello testery pomocí skriptu prostředí PowerShell](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Pomocí prostředí PowerShell tooautomate přidání testery tooyour testovacího prostředí. |
   | [Získání testovacího prostředí toohello odkaz](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Zjistěte, jak testery přímo přistupovat k testovacím prostředí pomocí hypertextový odkaz.|

7. **Automatizovat vytvoření testovacího prostředí pro více týmů** 
   
    Je možné automatizovat vytvoření testovacího prostředí, včetně vlastních nastavení, vytváření šablony Resource Manageru a použitím ho znovu a znovu toocreate identické laboratoře. 
   
    Další informace kliknutím na odkazy hello v hello následující tabulka:
   
   | Úkol | Co se naučíte |
   | --- | --- |
   | [Vytvoření testovacího prostředí pomocí šablony Resource Manageru](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |Vytvoření prostředí v Azure DevTest Labs pomocí šablony Resource Manageru. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

