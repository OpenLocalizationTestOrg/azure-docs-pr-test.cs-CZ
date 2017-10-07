---
title: "Možnosti – cloudové služby výpočetního prostředí aaaAzure | Microsoft Docs"
description: "Další informace o Azure výpočetní hostování možnosti a funkce: služby App Service, cloudové služby a virtuální počítače"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: e7f109a54c61cc2f37644d39a61d2d932a374587
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="should-i-choose-cloud-services-or-something-else"></a>Mám zvolit cloudové služby, nebo něco jiného?
Volba hello Azure Cloud Services je pro vás? Azure poskytuje různé hostování modely pro spouštění aplikací. Každé z nich poskytuje jinou sadu služeb, které z nich zvolíte, závisí na přesně to, na co se snažíte toodo.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-cloud-services"></a>Informace o cloudové služby
Cloudové služby je příkladem [platforma jako služba](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Jako [služby App Service](../app-service-web/app-service-web-overview.md), tato technologie je navrženou toosupport aplikace, které jsou škálovatelný, spolehlivý a levných toooperate. Stejně jako App Service je hostovaná na virtuálních počítačích, takže příliš jsou cloudové služby, ale máte větší kontrolu nad hello virtuálních počítačů. Vlastní software můžete nainstalovat na virtuální počítače cloudové služby a můžete do nich vzdálené.

![cs_diagram](./media/cloud-services-choose-me/diagram.png)

Další ovládací prvek také znamená menší snadné použití. Pokud budete potřebovat možnosti hello další ovládací prvek, je obvykle rychlejší a snadnější tooget webové aplikace a spuštěna ve službě Web Apps ve službě App Service porovná tooCloud služby.

Existují dva typy rolí cloudové služby. Hello jenom rozdíl mezi hello dva je jak vaši roli hostitelem hello virtuálního počítače.

* **Webová role**  
Automaticky nasadí a je hostitelem vaší aplikace pomocí služby IIS.

* **Role pracovního procesu**  
Nepoužívá službu IIS a spustí vaší samostatné aplikace.

Například jednoduchou aplikaci může použít pouze jeden webové role, obsluhující webu. Složitější aplikaci může použít webové role toohandle příchozí žádosti od uživatelů, pak předejte tyto požadavky na roli pracovního procesu tooa pro zpracování. (Tato komunikace může používat [Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) nebo [front Azure](../storage/common/storage-introduction.md).)

Jako hello doporučuje předchozí obrázek, všechny hello virtuálních počítačů v jedné aplikaci spustit v hello stejné cloudové služby. Aplikace hello přístup uživatelů prostřednictvím jednu veřejnou IP adresu, s požadavky automaticky skupině s vyrovnáváním zatížení napříč virtuálními počítači hello aplikace. Platforma Hello [Škáluje a nasadí](cloud-services-how-to-scale.md) hello virtuální počítače v cloudové služby aplikaci způsobem, který zabraňuje jediný bod selhání hardwaru.

I když aplikace spustit ve virtuálních počítačích, je důležité toounderstand, že cloudové služby poskytuje PaaS, není IaaS. Tady je jedním ze způsobů toothink o něm: V modelu IaaS, jako je například virtuální počítače Azure, nejprve vytvořit a nakonfigurovat vaše aplikace běží v prostředí hello, pak nasazení aplikace do tohoto prostředí. Jste zodpovědní za správu velkou část této world plnění úkolů, jako je nasazení nové verze opravou hello operačního systému v jednotlivých virtuálních počítačů. V PaaS naopak je jako hello prostředí již existuje. Všechny máte toodo je nasazení aplikace. Správa hello platformy, které běží na, včetně nasazení nové verze operačního systému hello, je zajistit sami.

## <a name="scaling-and-management"></a>Škálování a Správa
S cloudovými službami nevytvářejte virtuálních počítačů. Místo toho zadat konfigurační soubor informuje Azure, kolik jednotlivých chcete, jako například **tři instance webových rolí** a **dvě instance role pracovního procesu**, a platformy hello je pro vás vytvoří.  Stále zvolíte [jakou velikost](cloud-services-sizes-specs.md) by měla být ty zálohování virtuálních počítačů, ale nevytvoříte explicitně je sami. Pokud aplikace potřebuje toohandle větší zatížení, můžete požádat o další virtuální počítače a Azure vytvoří těchto instancí. Pokud snížení zatížení hello, je možné vypnout těchto instancí a zastavit platícího pro ně.

Cloudové služby aplikace obvykle přišla k dispozici toousers prostřednictvím ve dvou krocích. Vývojář první [nahrávání hello aplikace](cloud-services-how-to-create-deploy.md) toohello platformy je pracovní oblasti. Když hello vývojáře je připraven aplikace hello toomake za provozu, používají pracovní Azure portálu tooswap hello s produkčním. To [přepínat mezi pracovní a provozní](cloud-services-nodejs-stage-application.md) lze provést bez výpadků, která umožní spuštěné aplikace se nová verze upgradovaný tooa bez narušení svým uživatelům.

## <a name="monitoring"></a>Monitorování
Cloudové služby poskytuje také monitorování. Jako virtuální počítače Azure, zjistí selhání fyzického serveru a restartuje hello virtuálních počítačů, které byly spuštěné na daném serveru na nový počítač. Ale cloudové služby také zjistí selhání virtuální počítače a aplikace, ne jenom selhání hardwaru. Na rozdíl od virtuálních počítačů má agenta v každé webové a pracovní role, a proto je možné toostart nové virtuální počítače a instance aplikací, když dojde k selhání.

Hello PaaS povaha cloudové služby má jiné důsledky příliš. Jedním z nejdůležitějších hello je, že aplikace založené na tuto technologii budou zasílány toorun správně při jakoukoli instanci role web nebo pracovní proces se nezdaří. tooachieve to cloudové služby, aplikace by neměl Udržovat stav v hello systému souborů svůj vlastní virtuální počítače. Na rozdíl od virtuální počítače vytvořené pomocí Azure Virtual Machines nejsou zápisy provedené virtuálních počítačů služby tooCloud trvalé; není nic jako datový disk virtuálního počítače. Místo toho cloudové služby, aplikace by měly explicitně zápisu stavu tooSQL všechny databáze, objekty BLOB, tabulek nebo jiné externí úložiště. Vytváření aplikací s tímto způsobem díky je snazší tooscale a odolnější toofailure i důležité cíle cloudové služby.

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace cloudové služby v rozhraní .NET](cloud-services-dotnet-get-started.md)  
[Vytvoření aplikace cloudové služby v Node.js](cloud-services-nodejs-develop-deploy-app.md)  
[Vytvoření cloudové služby aplikace v jazyce PHP](../cloud-services-php-create-web-role.md)  
[Vytvoření aplikace cloudové služby v Pythonu](cloud-services-python-ptvs.md)

