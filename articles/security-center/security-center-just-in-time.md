---
title: "Pouze ve virtuálním počítači čas přístup v Azure Security Center | Microsoft Docs"
description: "Tento dokument nevystavíte slabé stránky zabezpečení prostřednictvím jak jenom na dobu virtuálních počítačů přístup v Azure Security Center pomáhá můžete řídit přístup k virtuální počítače Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: terrylan
ms.openlocfilehash: 5bb87488dcfc79ed4baa1dbd81dc4e1174f84e4b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Spravovat přístup k virtuálním počítačům pomocí právě v čase

Právě v čas virtuální počítač (VM) přístupu slouží zamknout příchozí přenosy na virtuální počítače Azure, snižuje riziko napadení a snadného přístupu pro připojení k virtuálním počítačům v případě potřeby.

> [!NOTE]
> Právě v čase je funkce ve verzi preview a je k dispozici ve standardní vrstvě služby Security Center.  V tématu [cenová](security-center-pricing.md) Další informace o službě Security Center je cenové úrovně.
>
>

## <a name="attack-scenario"></a>Scénář útoku

Útok hrubou silou útokům běžně cílové porty správy jako prostředek k získání přístupu k virtuálnímu počítači. V případě úspěšného útočník může převzít kontrolu nad virtuálního počítače a vytvořit dostane do vašeho prostředí.

Chcete-li omezit množství času, který je otevřený port je jedním ze způsobů, aby se snížila zranitelnost vůči útoku hrubou silou. Porty pro správu se nemusíte být otevřený za všech okolností. Pouze musí být otevřený, pokud jsou připojeny k virtuálnímu počítači, například k provádění úkolů údržby nebo správy. Pokud právě v čase je povoleno, Security Center používá [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) (NSG) pravidla, která omezit přístup k portům správy, takže nemůžou být cílem útočníků.

![Jenom v případě čas][1]

## <a name="how-does-just-in-time-access-work"></a>Jak jenom při přístup k časovému funguje?

Pokud je povolený přístup JIT (právě včas), Security Center uzamkne příchozí provoz do vašich virtuálních počítačů Azure vytvořením pravidla NSG. Můžete vybrat porty na virtuálním počítači, do které se uzamkne příchozí provoz směrem dolů. Tyto porty jsou řízeny jenom v době řešení.

Když uživatel požaduje přístup k virtuálnímu počítači, Security Center zkontroluje, zda má uživatel [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) oprávnění, které poskytují přístup pro zápis pro prostředků Azure. Pokud mají oprávnění k zápisu, jeho žádost se schválí a Security Center automaticky nakonfiguruje skupin zabezpečení sítě (Nsg) chcete povolit příchozí přenosy na správu porty pro množství času, který jste zadali. Po dobu vypršení platnosti, Security Center obnoví skupin Nsg do předchozího stavu.

> [!NOTE]
> Security Center, které jsou právě čas virtuálních počítačů přístup aktuálně podporuje pouze virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager. Další informace o modelu nasazení Resource Manager i classic najdete [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Použití pouze v přístup k časovému

**Těsně v čas virtuálních počítačů přístup** na dlaždici **Security Center** okno zobrazuje počet virtuálních počítačů, které jsou nakonfigurované pro jenom v čas přístupu a počet požadavků schválené přístup provedených během posledního týdne.

Vyberte **těsně v čas virtuálních počítačů přístup** dlaždici a **těsně v čas virtuálních počítačů přístup** otevře se okno.

![Právě v čase virtuální počítač přístup k dlaždici][2]

**Těsně v čas virtuálních počítačů přístup** okno poskytuje informace o stavu virtuálních počítačů:

- **Nakonfigurované** -virtuálních počítačů, které jsou nakonfigurované pro podporu právě v přístup k časovému virtuálních počítačů. Data uvedená za poslední týden a zahrnuje počet schválené žádosti, datum posledního přístupu a čas a naposledy uživatele pro každý virtuální počítač.
- **Doporučená** -virtuálních počítačů, které může podporovat jenom v přístup k časovému virtuálních počítačů, ale nebyly nakonfigurovány na. Doporučujeme povolit jenom při řízení přístupu čas virtuálních počítačů pro tyto virtuální počítače. V tématu [Povolit jenom v čas virtuálních počítačů přístup](#enable-just-in-time-vm-access).
- **Žádná doporučení** -důvody, které můžou způsobit, že virtuální počítač tak, aby se doporučuje jsou:
  - Chybí NSG - právě v čase řešení vyžaduje skupinu NSG se.
  - Classic virtuálního počítače – Security Center, které jsou právě čas virtuálních počítačů přístup aktuálně podporuje pouze virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager. Nasazení classic není podporována pouze v době řešení.
  - Druhá - virtuální počítač je v této kategorii Pokud právě v čase řešení vypnutý v zásadách zabezpečení předplatné nebo skupinu prostředků nebo že virtuálního počítače chybí veřejnou IP adresu a nemá skupinu NSG na místě.

## <a name="configuring-a-just-in-time-access-policy"></a>Konfigurace jenom v zásadách přístupu čas

Vyberte virtuální počítače, které chcete povolit:

1. Na **těsně v čas virtuálních počítačů přístup** okně, vyberte **doporučeno** kartě.

  ![Povolit přístup k časovému][3]

2. V části **virtuální počítače**, vyberte virtuální počítače, které chcete povolit. To převádí zatržení vedle virtuálního počítače.
3. Vyberte **povolení JIT na virtuálních počítačích**.
4. Vyberte **Uložit**.

### <a name="default-ports"></a>Výchozí porty

Můžete zobrazit výchozí porty, které Security Center doporučuje povolení jenom v čase.

1. Na **těsně v čas virtuálních počítačů přístup** okně, vyberte **doporučeno** kartě.

  ![Zobrazit výchozí porty][6]

2. V části **virtuální počítače**, vyberte virtuální počítač. To vloží zatržení vedle virtuálního počítače a otevře **JIT konfiguraci přístupu** okno. Toto okno se zobrazí výchozí porty.

### <a name="add-ports"></a>Přidat porty

Z **JIT konfiguraci přístupu** okno, můžete také přidávat a konfigurovat nový port, na kterém chcete povolit právě v době řešení.

1. Na **JIT konfiguraci přístupu** vyberte **přidat**. Tím se otevře **konfiguraci portů přidat** okno.

  ![Konfigurace portu][7]

2. Na **konfiguraci portů přidat** okně identifikaci portů, typ protokolu zdrojové adresy IP a maximální doba požadavku povolena.

  Povolené zdrojové adresy IP jsou rozsahy IP moct získat přístup na schválené žádost.

  Doba požadavku maximální je maximální časový interval, může být otevřena specifického portu.

3. Vyberte **OK**.

## <a name="requesting-access-to-a-vm"></a>Žádají o přístup do virtuálního počítače

Chcete-li požádat o přístup k virtuálnímu počítači:

1. Na **těsně v čas virtuálních počítačů přístup** okně, vyberte **konfigurovaná** kartě.
2. V části **virtuální počítače**, vyberte virtuální počítače, které chcete povolit přístup. To převádí zatržení vedle virtuálního počítače.
3. Vyberte **požádat o přístup**. Tím se otevře **požádat o přístup** okno.

  ![Požádat o přístup k virtuálnímu počítači][4]

4. Na **požádat o přístup** okno, můžete nakonfigurovat pro každý virtuální počítač portech, které spolu s zdrojové IP adresy, který se otevře port pro a časový interval, pro kterou je otevřen port. Může vyžádat přístup pouze k porty, které jsou nakonfigurované v jenom v zásadách čas. Každý z portů je maximální povolená doba odvozené od jenom v zásadách čas.
5. Vyberte **otevřít porty**.

## <a name="editing-a-just-in-time-access-policy"></a>Úpravy jenom v zásadách přístupu čas

Můžete změnit Virtuálního počítače existující jenom v zásadách čas přidáním a konfigurace nový port otevřete tohoto virtuálního počítače nebo změnou druhý parametr, související s již chráněné portu.

Chcete-li upravit stávající jenom v zásadách čas virtuálního počítače, **konfigurovaná** karta se používá:

1. V části **virtuální počítače**, vyberte virtuální počítač port na přidáte kliknutím na tři tečky v řádku tohoto virtuálního počítače. Tím otevřete nabídku.
2. Vyberte **upravit** v nabídce. Tím se otevře **JIT konfiguraci přístupu** okno.

  ![Upravit zásady][8]

3. Na **JIT konfiguraci přístupu** okno, můžete buď upravit existující nastavení, již chráněné portu kliknutím na její port, nebo můžete vybrat **přidat**. Tím se otevře **konfiguraci portů přidat** okno.

  ![Přidat na port][7]

4. Na **konfiguraci portů přidat** okno identifikovat port, typ protokolu, povolené zdrojové adresy IP a doba maximální požadavku.
5. Vyberte **OK**.
6. Vyberte **Uložit**.

## <a name="auditing-just-in-time-access-activity"></a>Auditování, právě aktivity přístup času.

Můžete získat přehledy aktivity virtuálního počítače pomocí protokolů hledání. Pokud chcete zobrazit protokoly:

1. Na **těsně v čas virtuálních počítačů přístup** okně, vyberte **konfigurovaná** kartě.
2. V části **virtuální počítače**, vyberte virtuální počítač k zobrazení informací o kliknutím na tři tečky v řádku tohoto virtuálního počítače. Tím otevřete nabídku.
3. Vyberte **protokol aktivit** v nabídce. Tím se otevře **protokol aktivit** okno.

![Vyberte protokol aktivit][9]

**Protokol aktivit** okno poskytuje filtrované zobrazení předchozí operace pro tento virtuální počítač společně s čas, datum a předplatného.

![Protokol aktivit zobrazení][5]

Informace protokolu si můžete stáhnout tak, že vyberete **kliknutím sem stáhnete všechny položky jako sdílený svazek clusteru**.

Upravit filtry a vyberte **použít** vytvořit vyhledávání a protokolu.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Pomocí jenom na dobu přístup virtuálních počítačů pomocí prostředí PowerShell

Chcete-li použít jenom v době řešení pomocí prostředí PowerShell, zajistěte, aby byla [nejnovější](/powershell/azure/install-azurerm-ps) prostředí Azure PowerShell verze.
Až to uděláte, musíte nainstalovat [nejnovější](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center z Galerie prostředí PowerShell.

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a>Konfigurace jenom v zásadách čas pro virtuální počítač

Ke konfiguraci jenom v zásadách čas na konkrétní virtuální počítač, budete muset spustit tento příkaz v relaci prostředí PowerShell: Set-ASCJITAccessPolicy.
Postupujte podle dokumentace rutiny Další informace.

### <a name="requesting-access-to-a-vm"></a>Žádají o přístup do virtuálního počítače

Pro přístup k konkrétní virtuální počítač, který je chráněný právě v doba řešení, budete muset spustit tento příkaz v relaci prostředí PowerShell: vyvolání ASCJITAccess.
Postupujte podle dokumentace rutiny Další informace.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli, jak jenom na dobu přístup virtuálních počítačů v Security Center pomáhá, že můžete řídit přístup k virtuální počítače Azure.

Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

- [Nastavení zásad zabezpečení](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.
- [Správa doporučení zabezpečení](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
- [Sledování stavu zabezpečení](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
- [Správa a zpracování výstrah zabezpečení](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.
- [Sledování partnerských řešení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav vašich partnerských řešení.
- [Security Center – nejčastější dotazy](security-center-faq.md) – přečtěte si nejčastější dotazy o použití této služby.
- [Blog o zabezpečení Azure](https://blogs.msdn.microsoft.com/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
