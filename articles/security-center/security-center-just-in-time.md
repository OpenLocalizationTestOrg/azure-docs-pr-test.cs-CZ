---
title: "přístup k aaaJust čas virtuální počítač v Azure Security Center | Microsoft Docs"
description: "Tento dokument vás provede procesem jak jenom na dobu přístup virtuálních počítačů v Azure Security Center pomáhá řídit přístup k tooyour Azure virtuální počítače."
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
ms.openlocfilehash: e6b58dd2c686cb227392b294e85914df5a546016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Spravovat přístup k virtuálním počítačům pomocí právě v čase

Právě v čas virtuální počítač (VM) může být přístup používané toolock dolů tooyour příchozí přenosy virtuálních počítačích Azure, snižuje tooattacks ohrožení a poskytuje snadný přístup tooconnect tooVMs v případě potřeby.

> [!NOTE]
> Hello pouze ve funkci čas je ve verzi preview a je k dispozici na hello úrovně Standard služby Security Center.  V tématu [cenová](security-center-pricing.md) toolearn Další informace o Security Center je cenové úrovně.
>
>

## <a name="attack-scenario"></a>Scénář útoku

Útok hrubou silou útokům běžně cílové porty správy jako tooa přístup toogain prostředky virtuálních počítačů. V případě úspěšného útočník může převzít kontrolu nad hello virtuálních počítačů a vytvořit dostane do vašeho prostředí.

Útoku hrubou silou tooa ohrožení jedním ze způsobů tooreduce je toolimit hello množství času, který je otevřený port. Porty pro správu to není nutné toobe otevřete za všech okolností. Potřebují toobe otevřete při jsou připojené toohello virtuálních počítačů, například tooperform úkolů údržby nebo správy. Pokud právě v čase je povoleno, Security Center používá [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) pravidla (NSG), které omezují přístup toomanagement porty, takže nemůžou být cílem útočníků.

![Jenom v případě čas][1]

## <a name="how-does-just-in-time-access-work"></a>Jak jenom při přístup k časovému funguje?

Pokud právě v čase je povoleno, Security Center zamyká tooyour příchozí přenosy virtuálních počítačů Azure ve vytvoření pravidla NSG. Vybrat hello porty na toowhich hello virtuálních počítačů bude uzamčené příchozí přenosy. Tyto porty jsou řízeny hello pouze v době řešení.

Když si uživatel vyžádá tooa přístup virtuálních počítačů, Security Center kontroluje má tento uživatel hello [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) oprávnění, které poskytují přístup pro zápis pro hello prostředků Azure. Pokud mají oprávnění k zápisu, hello žádost se schválí a Security Center automaticky nakonfiguruje hello skupiny zabezpečení sítě (Nsg) tooallow příchozí provoz toohello správu porty pro hello množství času, které zadáte. Po vypršení doby hello Security Center obnoví předchozí stavy tootheir hello skupiny Nsg.

> [!NOTE]
> Security Center, které jsou právě čas virtuálních počítačů přístup aktuálně podporuje pouze virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager. toolearn informace o modelu nasazení Resource Manager i hello classic najdete [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Použití pouze v přístup k časovému

Hello **těsně v čas virtuálních počítačů přístup** na hello dlaždici **Security Center** se zobrazí okno hello počet virtuálních počítačů, které jsou nakonfigurované pro právě v čas přístupu a hello číslo požadavků na přístup schválené provedené v hello minulého týdne.

Vyberte hello **těsně v čas virtuálních počítačů přístup** dlaždice a hello **těsně v čas virtuálních počítačů přístup** otevře se okno.

![Právě v čase virtuální počítač přístup k dlaždici][2]

Hello **těsně v čas virtuálních počítačů přístup** okno poskytuje informace o stavu hello virtuální počítače:

- **Nakonfigurované** – virtuální počítače, které byly nakonfigurované toosupport jenom při přístup k časovému virtuálních počítačů. Hello data uvedená pro hello minulého týdne a zahrnuje pro každý virtuální počítač hello číslo schválené žádosti, datum posledního přístupu a čas posledního uživatele.
- **Doporučená** -virtuálních počítačů, které může podporovat jenom v přístup k časovému virtuálních počítačů, ale nebyly nakonfigurovány na. Doporučujeme povolit jenom při řízení přístupu čas virtuálních počítačů pro tyto virtuální počítače. V tématu [Povolit jenom v čas virtuálních počítačů přístup](#enable-just-in-time-vm-access).
- **Žádná doporučení** -důvody, které můžou způsobit, že virtuální počítač není toobe doporučená jsou:
  - Chybí NSG - hello pouze v době řešení vyžaduje toobe NSG na místě.
  - Classic virtuálního počítače – Security Center, které jsou právě čas virtuálních počítačů přístup aktuálně podporuje pouze virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager. Nasazení classic nepodporuje hello pouze v době řešení.
  - Druhá - virtuální počítač je v této kategorii Pokud hello jenom na dobu, kterou řešení vypnutý v zásadách zabezpečení hello hello předplatné nebo skupinu prostředků hello nebo že hello virtuálních počítačů chybí veřejnou IP adresu a nemá skupinu NSG na místě.

## <a name="configuring-a-just-in-time-access-policy"></a>Konfigurace jenom v zásadách přístupu čas

tooselect hello virtuálních počítačů, které chcete tooenable:

1. Na hello **těsně v čas virtuálních počítačů přístup** okně, vyberte hello **doporučeno** kartě.

  ![Povolit přístup k časovému][3]

2. V části **virtuální počítače**, vyberte hello virtuálních počítačů, které chcete tooenable. To umístí další tooa zaškrtnutí virtuálních počítačů.
3. Vyberte **povolení JIT na virtuálních počítačích**.
4. Vyberte **Uložit**.

### <a name="default-ports"></a>Výchozí porty

Můžete zobrazit hello výchozí porty, které Security Center doporučuje povolení jenom v čase.

1. Na hello **těsně v čas virtuálních počítačů přístup** okně, vyberte hello **doporučeno** kartě.

  ![Zobrazit výchozí porty][6]

2. V části **virtuální počítače**, vyberte virtuální počítač. To umístí značku zaškrtnutí další toohello virtuálních počítačů a otevře se okno hello **JIT konfiguraci přístupu** okno. Toto okno zobrazuje hello výchozí porty.

### <a name="add-ports"></a>Přidat porty

Z hello **JIT konfiguraci přístupu** okno, můžete také přidávat a konfigurovat nový port, na kterém chcete tooenable hello pouze v době řešení.

1. Na hello **JIT konfiguraci přístupu** vyberte **přidat**. Tím se otevře hello **konfiguraci portů přidat** okno.

  ![Konfigurace portu][7]

2. Na **konfiguraci portů přidat** okně identifikovat hello port, protokol typ zdrojové adresy IP a maximální doba požadavku povolená.

  Povolené zdrojové adresy IP jsou hello IP rozsahů povolených tooget přístupu při schválené žádosti.

  Doba požadavku maximální je hello maximální časový interval, může být otevřena specifického portu.

3. Vyberte **OK**.

## <a name="requesting-access-tooa-vm"></a>Požaduje přístup tooa virtuálních počítačů

toorequest přístup tooa virtuálních počítačů:

1. Na hello **těsně v čas virtuálních počítačů přístup** okně, vyberte hello **konfigurovaná** kartě.
2. V části **virtuální počítače**, vyberte hello virtuální počítače, který má přístup tooenable. To umístí další tooa zaškrtnutí virtuálních počítačů.
3. Vyberte **požádat o přístup**. Tím se otevře hello **požádat o přístup** okno.

  ![Žádost o přístup tooa virtuálních počítačů][4]

4. Na hello **požádat o přístup** okně nakonfigurovat pro každý virtuální počítač hello porty tooopen společně s hello zdrojovou IP, který hello port je otevřít tooand hello časový interval, pro které hello je otevřen port. Může požádat o přístup pouze toohello porty, které jsou nakonfigurované v hello jenom v zásadách čas. Každý z portů je maximální povolená doba odvozené od hello jenom v zásadách čas.
5. Vyberte **otevřít porty**.

## <a name="editing-a-just-in-time-access-policy"></a>Úpravy jenom v zásadách přístupu čas

Virtuálního počítače existující jenom v zásadách čas přidání a konfigurace nové tooopen portu pro tento virtuální počítač, můžete změnit nebo změnou druhý parametr související tooan už chráněný portu.

V pořadí tooedit existující jenom v zásadách čas virtuálního počítače, hello **konfigurovaná** karta se používá:

1. V části **virtuální počítače**, vyberte virtuální počítač tooadd port tooby, kliknutím na hello tři tečky v rámci hello řádek tohoto virtuálního počítače. Tím otevřete nabídku.
2. Vyberte **upravit** v nabídce hello. Tím se otevře hello **JIT konfiguraci přístupu** okno.

  ![Upravit zásady][8]

3. Na hello **JIT konfiguraci přístupu** okně hello existující nastavení už chráněný portu můžete buď upravit kliknutím na její port, nebo můžete vybrat **přidat**. Tím se otevře hello **konfiguraci portů přidat** okno.

  ![Přidat na port][7]

4. Na **konfiguraci portů přidat** okno identifikovat hello port, typ protokolu, povolené zdrojové adresy IP a doba maximální požadavku.
5. Vyberte **OK**.
6. Vyberte **Uložit**.

## <a name="auditing-just-in-time-access-activity"></a>Auditování, právě aktivity přístup času.

Můžete získat přehledy aktivity virtuálního počítače pomocí protokolů hledání. tooview protokoly:

1. Na hello **těsně v čas virtuálních počítačů přístup** okně, vyberte hello **konfigurovaná** kartě.
2. V části **virtuální počítače**, vyberte virtuální počítač tooview informací o kliknutím na hello tři tečky v rámci hello řádek tohoto virtuálního počítače. Tím otevřete nabídku.
3. Vyberte **protokol aktivit** v nabídce hello. Tím se otevře hello **protokol aktivit** okno.

![Vyberte protokol aktivit][9]

Hello **protokol aktivit** okno poskytuje filtrované zobrazení předchozí operace pro tento virtuální počítač společně s čas, datum a předplatného.

![Protokol aktivit zobrazení][5]

Informace protokolu hello si můžete stáhnout tak, že vyberete **kliknutím sem toodownload všechny položky hello jako sdílený svazek clusteru**.

Upravit hello filtry a vyberte **použít** toocreate vyhledávání a protokolu.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Pomocí jenom na dobu přístup virtuálních počítačů pomocí prostředí PowerShell

V pořadí toouse hello právě v doba řešení pomocí prostředí PowerShell, zajistěte, aby byla hello [nejnovější](/powershell/azure/install-azurerm-ps) prostředí Azure PowerShell verze.
Až to uděláte, musíte tooinstall hello [nejnovější](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center z Galerie prostředí PowerShell hello.

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a>Konfigurace jenom v zásadách čas pro virtuální počítač

tooconfigure jenom v zásadách čas na konkrétním virtuálním počítači, je nutné toorun tento příkaz v relaci prostředí PowerShell: Set-ASCJITAccessPolicy.
Postupujte podle hello rutiny dokumentaci toolearn Další.

### <a name="requesting-access-tooa-vm"></a>Požaduje přístup tooa virtuálních počítačů

tooaccess konkrétní virtuální počítač, který je chráněný pomocí hello pouze v době řešení, musíte tento příkaz pro toorun v relaci prostředí PowerShell: vyvolání ASCJITAccess.
Postupujte podle hello rutiny dokumentaci toolearn Další.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli, jak jenom na dobu přístup virtuálních počítačů v Security Center pomáhá řídit přístup tooyour Azure virtuální počítače.

toolearn Další informace o Security Center, najdete v části hello následující:

- [Nastavení zásad zabezpečení](security-center-policies.md) – Další informace jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
- [Správa doporučení zabezpečení](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
- [Sledování stavu zabezpečení](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
- [Správa a zpracování výstrah toosecurity](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy.
- [Sledování partnerských řešení](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
- [Security Center – nejčastější dotazy](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
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
