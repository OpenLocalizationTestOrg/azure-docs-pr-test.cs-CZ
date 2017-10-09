---
title: "aaaAzure Security Center – nejčastější dotazy (FAQ) | Microsoft Docs"
description: "Tyto nejčastější dotazy odpovědi na otázky o Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: cd0c0f8bdf15cdaf5889f2da5ac3cadf6017a9e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Nejčastější dotazy ohledně Azure Security Center
Tyto nejčastější dotazy odpovědi na otázky o Azure Security Center, služba, která pomáhá zabránit, zjistit a reagovat toothreats nabízí lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Microsoft Azure.

> [!NOTE]
> Počínaje časná 2017 června, Security Center použije hello agenta Microsoft Monitoring Agent toocollect a ukládat data. Další, najdete v části toolearn [Azure Security Center platformy migrace](security-center-platform-migration.md). Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.
>
>

## <a name="general-questions"></a>Obecné otázky
### <a name="what-is-azure-security-center"></a>Co je Azure Security Center?
Azure Security Center pomáhá zabránit, zjistit a reagovat toothreats nabízí lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

### <a name="how-do-i-get-azure-security-center"></a>Jak získám Azure Security Center?
Azure Security Center je povolená s vaším předplatným Microsoft Azure a k němu přistupovat z hello [portál Azure](https://azure.microsoft.com/features/azure-portal/). ([Přihlášení portálu toohello](https://portal.azure.com), vyberte **Procházet**a posuňte se příliš**Security Center**).  

## <a name="billing"></a>Fakturace
### <a name="how-does-billing-work-for-azure-security-center"></a>Jak funguje fakturace pro Azure Security Center?
Security Center je k dispozici v dvěma vrstvami:

Hello **úroveň Free** poskytuje přehled o hello stav zabezpečení prostředků Azure, zásady základního zabezpečení, doporučení zabezpečení a integrace s zabezpečovací produkty a služby od partnerů.

Hello **úrovně Standard** přidá rozšířené hrozba možností detekce, včetně hrozby intelligence, analýzy chování, detekce anomálií, incidenty zabezpečení a hrozby uvedení sestavy. úroveň Standard Hello je zdarma hello prvních 60 dní. Si zvolíte toocontinue toouse hello služby za 60 dnů, můžeme automaticky spustit toocharge služby hello.  tooupgrade, vyberte cenová úroveň v hello [zásady zabezpečení](security-center-policies.md#set-security-policies). Další, najdete v části toolearn [Security Center ceny](security-center-pricing.md).

## <a name="permissions"></a>Oprávnění
Azure Security Center používá [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md), který poskytuje [předdefinované role](../active-directory/role-based-access-built-in-roles.md) , lze přiřadit toousers, skupiny a služby v Azure.

Security Center vyhodnocuje hello konfiguraci problémy se zabezpečením tooidentify prostředky a ohrožení zabezpečení. V Centru zabezpečení, uvidíte jenom informace související s tooa prostředků, když jsou přiřazeny hello roli vlastník, Přispěvatel nebo Čtenář pro předplatné nebo prostředek skupiny hello, která daný prostředek patří.

V tématu [oprávnění v Azure Security Center](security-center-permissions.md) toolearn více informací o rolí a povolených akcí v Centru zabezpečení.

## <a name="data-collection"></a>Shromažďování dat
Security Center shromažďuje data z vaší virtuální počítače tooassess jejich stavu zabezpečení, zadejte doporučení zabezpečení a výstrah toothreats. Pokud nejprve přístup k Security Center, shromažďování dat je povolené na všechny virtuální počítače ve vašem předplatném. Můžete také povolit shromažďování dat v hello zásad Security Center.

### <a name="how-do-i-disable-data-collection"></a>Jakým způsobem vypnout shromažďování dat?
Pokud používáte hello Azure Security Center volné vrstvy, můžete zakázat shromažďování dat z virtuálních počítačů v každém okamžiku. Shromažďování dat je vyžadován pro odběry ve standardní vrstvě hello. Shromažďování dat pro předplatné za hello zásady zabezpečení můžete zakázat. ([Přihlášení toohello portál Azure](https://portal.azure.com), vyberte **Procházet**, vyberte **Security Center**a vyberte **zásad**.)  Když vyberte předplatné, otevře nové okno a poskytuje hello tooturn možnost vypnout **shromažďování dat**.

### <a name="how-do-i-enable-data-collection"></a>Povolení shromažďování dat
Shromažďování dat můžete povolit pro vaše předplatné Azure v hello zásady zabezpečení. shromažďování dat tooenable. [Přihlaste se toohello portál Azure](https://portal.azure.com), vyberte **Procházet**, vyberte **Security Center**a vyberte **zásad**. Nastavit **shromažďování dat** příliš**na**.

### <a name="what-happens-when-data-collection-is-enabled"></a>Co se stane, pokud je povoleno shromažďování dat?
Při shromažďování dat je povolené, hello agenta Microsoft Monitoring Agent je automaticky zřízený na všechny stávající a nově podporované virtuální počítače, které jsou nasazeny v předplatném hello.

### <a name="does-hello-monitoring-agent-impact-hello-performance-of-my-servers"></a>Podporuje hello Monitoring Agent dopad hello výkon Moje serverů?
Hello agent využívá nominální množství systémové prostředky a musí mít malý vliv na výkon hello. Další informace o vlivu na výkon a hello agenta a rozšíření najdete v tématu hello [Průvodce plánováním a operace](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Kde jsou moje data uložená?
Data shromážděná z tohoto agenta je uložená v existující pracovní prostor analýzy protokolů spojené s vaším předplatným nebo nový pracovní prostor. Další informace najdete v tématu [zabezpečení dat](security-center-data-security.md).

## <a name="using-azure-security-center"></a>Pomocí Azure Security Center
### <a name="what-is-a-security-policy"></a>Co je zásady zabezpečení?
Zásady zabezpečení definuje hello sadu ovládacích prvků, které se doporučují pro prostředky v rámci hello zadaný odběr. V Azure Security Center určíte zásady pro vaše předplatná Azure podle tooyour požadavky na zabezpečení a společnosti hello typu aplikací nebo citlivosti dat hello v každém předplatném.

zásady zabezpečení Hello povolené v Azure Security Center doporučení zabezpečení jednotky a monitorování. toolearn informace o zásadách zabezpečení, najdete v části [sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Kdo může upravit zásady zabezpečení?
toomodify zásady zabezpečení, musíte být správce zabezpečení nebo roli vlastníka nebo přispěvatele daného předplatného.

jak zjistit, tooconfigure zásadu zabezpečení toolearn [nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Co je doporučeným zabezpečením?
Azure Security Center analyzuje stav zabezpečení hello vašich prostředků Azure. Když jsou identifikovány potenciální ohrožení zabezpečení, doporučení jsou vytvořeny. Hello doporučení Průvodce vás provede procesem konfigurace hello hello potřeby ovládacího prvku. Mezi příklady patří:

* Zřizování antimalwarových toohelp identifikovat a odebrat škodlivý software
* Konfigurace [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) a pravidla toocontrol provoz toovirtual počítače
* Zřizování webové aplikace brány firewall toohelp bránit proti útokům na cílení na vaše webové aplikace
* Nasazení chybějících aktualizací systému
* Adresování konfigurací operačního systému, které neodpovídají hello doporučená standardních hodnot

Zde se zobrazují pouze doporučení, které jsou povolené v zásadách zabezpečení.

### <a name="how-can-i-see-hello-current-security-state-of-my-azure-resources"></a>Jak lze zobrazit aktuální stav zabezpečení hello Moje prostředků Azure?
Hello **Přehled Centra zabezpečení** okno hello ukazuje celkové postavení zabezpečení prostředí podle výpočty, sítě, úložiště a datům a aplikacím. Každý typ prostředku má zobrazení indikátoru, pokud byly nalezeny všechny potenciální ohrožení zabezpečení. Kliknutím na každou dlaždici zobrazí seznam problémy se zabezpečením identifikovaný Security Center spolu s inventář hello prostředky ve vašem předplatném.

### <a name="what-triggers-a-security-alert"></a>Co se aktivuje výstraha zabezpečení?
Azure Security Center automaticky shromažďuje, analyzuje a fuses dat protokolu z vaše prostředky Azure, hello sítě a řešení partnerů, jako Antimalwarový program a brány firewall. Při zjištění ohrožení zabezpečení se vytvoří výstraha zabezpečení. Příklady zahrnují zjišťování následujících situací:

* Ohrožené virtuální počítače, které komunikují se známými IP adresami se zlými úmysly
* Pokročilý malware zjištěný pomocí zasílání zpráv o chybách systému Windows
* Útoky hrubou silou na virtuální počítače
* Výstrahy zabezpečení z integrovaných partnerských řešení zabezpečení například proti malwaru nebo brány firewall webových aplikací

### <a name="whats-hello-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Co je hello rozdíl mezi hrozeb zjistila a zobrazovat výstrahy na podle Microsoft Security Response Center versus Azure Security Center?
Hello Microsoft Security Response Center (MSRC) provádí, vyberte možnost zabezpečení monitorování hello síť Azure a infrastruktury a přijímá stížností intelligence a zneužití ohrožení od jiných výrobců. Když střediska MSRC zjistí, že data zákazníků přistupoval strana nezákonné nebo neoprávněným nebo tohoto zákazníka hello použití Azure není v souladu s podmínkami hello pro použít přípustné, upozorní incidentu správce zabezpečení hello zákazníka. Oznámení se obvykle dochází, odesláním zabezpečení e-mailu toohello kontakty zadané v Azure Security Center nebo hello vlastníka předplatného Azure, pokud není zadán a obraťte se na zabezpečení.

Security Center je služba Azure, která nepřetržitě monitoruje prostředí Azure hello zákazníka a použije analytics tooautomatically zjistit širokou škálu potenciálně škodlivé aktivity. Tyto detekce jsou prezentované jako výstrahy zabezpečení v řídicím panelu Security Center hello.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Prostředky, ke kterým Azure jsou monitorovány pomocí Azure Security Center?
Azure Security Center monitoruje hello následující prostředky Azure:

* Virtuální počítače (VM) (včetně [cloudové služby](../cloud-services/cloud-services-choose-me.md))
* Virtuální sítě Azure
* Služba Azure SQL
* Účet služby Azure Storage
* Webové aplikace Azure (v [služby App Service Environment](../app-service/app-service-app-service-environments-readme.md))
* Partnerských řešení integrovaných ve vašem předplatném Azure například brány firewall webových aplikací na virtuálních počítačích a na [App Service Environment](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtuální počítače
### <a name="what-types-of-virtual-machines-are-supported"></a>Jaké typy virtuálních počítačů jsou podporovány?
Monitorování a doporučení jsou k dispozici pro virtuální počítače (VM) vytvořený pomocí obou hello [classic a modelech nasazení Resource Manager](../azure-classic-rm.md).

V tématu [podporovaných platforem v Azure Security Center](security-center-os-coverage.md) seznam podporovaných platforem.

### <a name="why-doesnt-azure-security-center-recognize-hello-antimalware-solution-running-on-my-azure-vm"></a>Proč Azure Security Center nerozpoznal hello antimalwarové řešení, která je spuštěna na virtuálním počítači Azure?
Azure Security Center obsahuje přehled antimalwarové nainstalované prostřednictvím rozšíření Azure. Například Security Center není možné toodetect proti malwaru, který byl předem nainstalovaná na bitovou kopii, kterou jste zadali, nebo pokud jste nainstalovali antimalwarových na virtuální počítače pomocí vlastní procesy (například systémy pro správu konfigurace).

### <a name="why-do-i-get-hello-message-missing-scan-data-for-my-vm"></a>Proč se zobrazila zpráva hello "Chybí Data kontroly" pro virtuální počítač?
Tato zpráva se zobrazí, když nejsou žádná data kontroly pro virtuální počítač. Po povolení shromažďování dat v Azure Security Center může trvat nějakou dobu (méně než jednu hodinu) toopopulate data kontroly. Po hello počáteční naplnění dat kontroly může zobrazit tato zpráva, protože chybí data kontroly vůbec nebo nejsou žádná data poslední kontroly. Prohledávání naplnit není pro virtuální počítač v zastaveném stavu. Tato zpráva se může zobrazit i v případě, že data skenování nebylo naplněno nedávno (v souladu s hello zásady uchovávání informací pro agenta Windows hello, což je výchozí hodnota 30 dní).

### <a name="why-do-i-get-hello-message-vm-agent-is-missing"></a>Proč se zobrazila zpráva hello "Agenta virtuálního počítače je chybějící?"
Hello agenta virtuálního počítače musí být nainstalován na virtuálních počítačích tooenable shromažďování dat. Hello agenta virtuálního počítače je nainstalována ve výchozím nastavení pro virtuální počítače, které byly nasazeny pomocí hello Azure Marketplace. Informace o tom, jak tooinstall hello agenta virtuálního počítače na ostatních virtuálních počítačů, najdete v příspěvku blogu hello [agenta virtuálního počítače a rozšíření](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
