---
title: Koncepty aaaDevTest Labs | Microsoft Docs
description: "Další informace hello základními koncepty prostředí DevTest Labs a jak může být snadno toocreate, spravovat a monitorovat virtuální počítače Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: d9f1d948002c4d3121e5bdd4e65eb8b54cd91f9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="devtest-labs-concepts"></a>Koncepce DevTest Labs
## <a name="overview"></a>Přehled
Hello následující seznam obsahuje klíčové koncepty DevTest Labs a definice:

## <a name="labs"></a>Testovací prostředí
Testovacího prostředí je hello infrastrukturu, která zahrnuje skupinu prostředků, třeba virtuální počítače (VM), které vám umožní lépe spravovat tyto prostředky zadáním omezení a kvóty.

## <a name="virtual-machine"></a>Virtuální počítač
Virtuální počítač Azure je jedním z několika typů [na vyžádání, škálovatelných výpočetních prostředků](https://docs.microsoft.com/azure/app-service-web/choose-web-site-cloud-service-vm) Azure nabízí. Azure virtuální počítače udělení hello flexibilitu virtualizace bez nutnosti toobuy a udržovat hello fyzický hardware, který spouští, i když stále potřebujete toomaintain hello virtuálního počítače provedením určitých úloh, jako je například konfigurace, opravy a instalace softwaru hello která se spouští na něm.

[Přehled o virtuálních počítačích s Windows v Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) poskytuje informace o co byste měli zvážit před vytvořit virtuální počítač, jak vytvořit a jak spravovat.

## <a name="claimable-vm"></a>Vymahatelných virtuálních počítačů
Virtuální počítač Azure vymahatelných je virtuální počítač, který je k dispozici pro použití jakéhokoli testovacího prostředí uživatele s oprávněními. Správce testovacího prostředí můžete připravit virtuální počítače s konkrétním základní bitové kopie a artefaktů a uložíte tooa sdílený fond. Testovacího prostředí můžete pak deklarace identity uživatele funkční virtuální počítač z fondu hello když potřebují jednu s konfigurací pro tuto konkrétní.

Virtuální počítač, který je vymahatelných není přiřazen původně tooany určitého uživatele, ale se zobrazí v seznamu každého uživatele v části "Vymahatelných virtuální počítače". Po virtuálních počítačů je požadována uživatelem, je přesunout nahoru tootheir "Moje virtuální počítače" oblasti a nadále již není vymahatelných jiným uživatelem.

## <a name="environment"></a>Prostředí
Prostředí v DevTest Labs odkazuje tooa kolekce prostředků Azure v testovacím prostředí. [Tento příspěvek blogu](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) popisuje, jak toocreate prostředí více virtuálních počítačů z šablony Azure Resource Manager.

## <a name="base-images"></a>Základní bitové kopie
Základní Image jsou Image virtuálních počítačů se všemi hello nástroje a nastavení předinstalována a nakonfigurované tooquickly vytvoření virtuálního počítače. Virtuální počítač můžete zřídit výběr existující základní a přidáním tooinstall artefaktů agenta test agent. Vám může pak uložte hello zřízen virtuálního počítače jako na základní tak, aby hello základní lze použít bez tooreinstall hello test agent pro každý zřizování hello virtuálních počítačů.

## <a name="artifacts"></a>Artefakty
Artefakty jsou použité toodeploy a nakonfigurovat svoji aplikaci po zřízení virtuálního počítače. Artefakty mohou být:

* Nástroje, které chcete tooinstall na hello virtuálního počítače – například agentů, Fiddler a Visual Studio.
* Akce, že chcete toorun na hello virtuálního počítače – například klonování úložišti.
* Aplikace, které chcete tootest.

Artefakty je [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) soubory JSON, které obsahují pokyny tooperform nasazení a konfiguraci použít.

## <a name="artifact-repositories"></a>Úložiště artefaktů
Úložiště artefaktů jsou úložiště git, kde se změnami artefakty. Úložiště artefaktů lze přidat toomultiple labs ve vaší organizaci povolení opakované použití a sdílení.

## <a name="formulas"></a>Vzorce
Vzorce v přidání toobase Image, poskytují mechanismus pro rychlé zřizování virtuálních počítačů. Vzorec v DevTest Labs je seznam výchozí vlastnost hodnoty používané toocreate testovacího prostředí virtuálních počítačů.
Bez nutnosti toospecify tyto vlastnosti pokaždé, když se dají vytvořit pomocí vzorce, virtuálních počítačů pomocí stejné sady vlastností – například základní bitovou kopii, velikost virtuálního počítače, virtuální sítě a artefakty - hello. Při vytváření virtuálního počítače ze vzorce, hello výchozí hodnoty lze použít jako-je nebo upravit.

## <a name="policies"></a>Zásady
Zásady pomáhají v řízení nákladů ve svém testovacím prostředí. Například můžete vytvořit zásady tooautomatically, vypněte virtuální počítače podle definovaného plánu.

## <a name="caps"></a>CAP k vzdálené ploše
CAP k vzdálené ploše je plýtvání toominimize mechanismus ve svém testovacím prostředí. Například můžete nastavit několik hello toorestrict limitu virtuálních počítačů, které lze vytvořit na uživatele nebo v testovacím prostředí.

## <a name="security-levels"></a>Úrovně zabezpečení
Zabezpečení přístupu je určen podle řízení řízení přístupu (RBAC). toounderstand přístupu funguje, jak jsou definovány pomocí RBAC pomáhá toounderstand hello rozdíly mezi oprávnění, role a obor.

* Oprávnění – oprávnění je určitá akce tooa definované přístup (například přístup pro čtení tooall virtuálních počítačů).
* Role - role je sadu oprávnění, která může být seskupené a přiřazení tooa uživatele. Například hello *vlastník předplatného* má role přístup k prostředkům tooall v rámci předplatného.
* Rozsah - obor je úrovně uvnitř hierarchie hello prostředek služby Azure, například skupinu prostředků, jeden testovacího prostředí nebo hello celé předplatné.

V rámci oboru hello DevTest Labs, existují dva typy rolí toodefine uživatelských oprávnění: vlastníka testovacího prostředí a testovacího prostředí uživatele.

* Laboratoř vlastníka – vlastník testovacího prostředí má přístup k prostředkům tooany v testovacím prostředí hello. Proto vlastníka testovacího prostředí můžete upravit zásady, čtení a zápis všech virtuálních počítačů, změnit hello virtuální sítě a tak dále.
* Uživatel Lab - uživatel testovacího prostředí můžete zobrazit všechny prostředky testovacího prostředí, jako jsou virtuální počítače, zásady a virtuální sítě, ale nelze změnit zásady nebo všechny virtuální počítače vytvořené jinými uživateli.

toosee jak toocreate vlastní role v DevTest Labs, najdete v článku toohello [udělení uživatelských oprávnění zásad testovacího toospecific](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Vzhledem k tomu, že obory jsou hierarchické, když má uživatel oprávnění u určité oboru, jsou automaticky přiděleno těchto oprávnění v každé nižší úrovni oboru zahrnut. Například pokud uživatel přiřazený toohello roli vlastník předplatného, pak mají přístup k prostředkům tooall v jednom předplatném, včetně všech virtuálních počítačů, všechny virtuální sítě a všechny laboratoře. Proto vlastník předplatného automaticky zdědí hello roli vlastníka testovacího prostředí. Hello opačné však není pravda. Vlastník testovacího prostředí má laboratoř tooa přístupu, které je obor nižší než úroveň předplatného hello. Vlastník testovacího prostředí se proto nebudou mít toosee virtuální počítače nebo virtuální sítě nebo všechny prostředky, které jsou mimo prostředí hello.

## <a name="azure-resource-manager-templates"></a>Šablony Azure Resource Manageru
Všechny hello Principy probírané v tomto článku lze nakonfigurovat pomocí šablony Azure Resource Manager, které umožňují definovat hello infrastruktury nebo konfiguraci řešení Azure a opakovaně nasadit v konzistentním stavu.

[Pochopení syntaxi šablon Azure Resource Manager a struktura hello](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) popisuje strukturu hello Azure Resource Manager šablony a hello vlastnosti, které jsou k dispozici v různých částech hello šablony.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
[Vytvoření testovacího prostředí v DevTest Labs](devtest-lab-create-lab.md)
