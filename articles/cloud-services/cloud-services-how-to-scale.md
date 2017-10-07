---
title: "aaaAuto škálování cloudové služby na portálu classic hello | Microsoft Docs"
description: "(klasické) Zjistěte, jak toouse hello portálu classic tooconfigure automatické škálování pravidla pro cloudové služby webovou roli nebo role pracovního procesu v Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: ddb5816d4d22192c6d2f51d7508e390779742078
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-classic-portal"></a>Jak tooconfigure automatické škálování pro cloudové služby na portálu classic hello
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-scale-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-scale.md)

Na stránce škálování hello hello portál Azure classic můžete nakonfigurovat nastavení automatického škálování pro vaši webovou roli nebo role pracovního procesu. Alternativně můžete nakonfigurovat, ruční škálování místo založeného na pravidlech automatické škálování.

> [!NOTE]
> Tento článek se zaměřuje na webových a pracovních rolí cloudové služby. Když vytvoříte virtuální počítač (klasický) přímo, je hostovaná v cloudové službě. Některé z těchto informací se vztahuje toothese typy virtuálních počítačů. Škálování skupinu dostupnosti virtuálních počítačů se právě vypíná je zapnout a vypnout na základě pravidel škálování hello, kterou konfigurujete. Další informace o virtuální počítače a skupiny dostupnosti najdete v tématu [hello Správa dostupnosti virtuálních počítačů](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

Měli byste zvážit hello před konfigurací škálování pro vaši aplikaci následující informace:

* Škálování má vliv základní použití.

    Větší instance rolí pomocí více jader. Je možné škálovat aplikaci pouze v rámci limitu hello jader pro předplatné. Řekněme například, že vaše předplatné může mít 20 jader. Pokud spouštíte aplikaci pomocí dvou středních cloudové služby (celkem 4 jádra), můžete pouze postupně škálovat ostatních nasazení cloudové služby ve vašem předplatném ve zbývajících 16 jader hello. Další informace o velikosti najdete v tématu [cloudové služby velikosti](cloud-services-sizes-specs.md).

* Musíte vytvořit frontu a přidružte ji k roli předtím, než je možné škálovat aplikaci pro zprávu prahovou hodnotu. Další informace najdete v tématu [jak toouse hello fronty úložiště služby](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Je možné škálovat prostředky, které jsou propojené tooyour Cloudová služba. Další informace o propojení prostředků, najdete v tématu [postupy: odkaz prostředků tooa Cloudová služba](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

* tooenable vysokou dostupnost vaší aplikace, ujistěte se, že je nasazený s dvěma nebo více instancí role. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

## <a name="schedule-scaling"></a>Plánování škálování
Ve výchozím nastavení všechny role neprovádějte určitého plánu. Proto nastavení změnit platit tooall časy a všechny dny po celý rok hello. Pokud chcete, můžete nastavit ruční nebo automatické škálování pro jednu z následujících režimů hello:

* Dny v týdnu
* Víkendů
* Týden noci
* Ráno týden
* Konkrétní kalendářní data
* Konkrétní rozsahy dat

Hello plán je nastavena v hello [portál Azure classic](https://manage.windowsazure.com/) na hello  
**Cloudové služby** > **\[cloudové služby\]** > **škálování**  >   **\[Produkční nebo pracovní\]**  stránky.

Klikněte na tlačítko hello **časů plánu** tlačítko pro každou roli chcete toochange.

![Cloudové služby Automatické škálování podle plánu][scale_schedules]

## <a name="manual-scale"></a>Ruční škálování
Na hello **škálování** stránky, můžete ručně zvýšit nebo snížit počet hello spuštěné instance v cloudové službě. Toto nastavení je nakonfigurovat pro každý plán, který jste vytvořili nebo tooall čas, pokud jste dosud nevytvořili plánu.

1. V hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a potom klikněte na název hello hello řídicí panel hello tooopen cloudové služby.
   
   > [!TIP]
   > Pokud nevidíte cloudové služby, pravděpodobně bude třeba toochange z **produkční** příliš**pracovní** nebo naopak.

2. Klikněte na tlačítko **škálování**.
3. Vyberte plán hello chcete toochange možnosti škálování. *Ne naplánovaném čase* je výchozí hello, pokud máte žádné plány definované.
4. Najde hello **škálování podle metriky** a vyberte **NONE**. Toto nastavení je výchozí hello u všech rolí.
5. Ke každé roli v hello cloudové služby jsou pro změnu hello počet instancí toouse jezdce.
   
    ![Ručně škálovat, roli cloudové služby][manual_scale]
   
    Pokud potřebujete další instance, pravděpodobně bude třeba toochange hello [cloudu velikost virtuálního počítače služby](cloud-services-sizes-specs.md).
6. Klikněte na **Uložit**.  
   Instance role jsou přidat nebo odebrat závislosti na vybrané položky.

> [!TIP]
> Vždy, když se zobrazí ![][tip_icon] přesunout vaše tooit myši a může získat nápovědu ke konkrétní nastavení nemá.

## <a name="automatic-scale---cpu"></a>Automatické škálování - procesoru
Tento režim škáluje v případě hello průměrnou procentuální hodnotu využití procesoru nad nebo pod zadaných prahových hodnot. Instance role jsou vytvořené nebo odstraněné v takovém případě.

1. V hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a potom klikněte na název hello hello řídicí panel hello tooopen cloudové služby.
   
   > [!TIP]
   > Pokud nevidíte cloudové služby, pravděpodobně bude třeba toochange z **produkční** příliš**pracovní** nebo naopak.

2. Klikněte na tlačítko **škálování**.
3. Vyberte plán hello chcete toochange možnosti škálování. *Ne naplánovaném čase* je výchozí hello, pokud máte žádné plány definované.
4. Najde hello **škálování podle metriky** a vyberte **procesoru**.
5. Teď můžete nakonfigurovat minimální a maximální rozsah instancí role, využití procesoru cílového hello (tootrigger a rozšiřování škálování využívajících) a kolik instancí tooscale nahoru a dolů podle.

![Škálování role cloudové služby podle zatížení procesoru][cpu_scale]

> [!TIP]
> Vždy, když se zobrazí ![][tip_icon] přesunout vaše tooit myši a může získat nápovědu ke konkrétní nastavení nemá.

## <a name="automatic-scale---queue"></a>Automatické škálování - fronty
Tento režim automaticky přizpůsobí Pokud hello počet zpráv ve frontě přejde nad nebo pod zadanou prahovou hodnotu. Instance role jsou vytvořené nebo odstraněné v takovém případě.

1. V hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a potom klikněte na název hello hello řídicí panel hello tooopen cloudové služby.
   
   > [!TIP]
   > Pokud nevidíte cloudové služby, pravděpodobně bude třeba toochange z **produkční** příliš**pracovní** nebo naopak.

2. Klikněte na tlačítko **škálování**.
3. Najde hello **škálování podle metriky** a vyberte **fronty**.
4. Teď můžete nakonfigurovat minimální a maximální rozsah instancí role, hello fronty a počet tooprocess zprávy fronty pro každou instanci a kolik instancí tooscale vertikálně a horizontálně podle.

![Škálovat roli cloudové služby, protože fronta zpráv][queue_scale]

> [!TIP]
> Vždy, když se zobrazí ![][tip_icon] přesunout vaše tooit myši a může získat nápovědu ke konkrétní nastavení nemá.

## <a name="scale-linked-resources"></a>Škálování odkazované zdroje.
Často při změně měřítka roli, je výhodné tooscale hello databázi, která také používá aplikace hello. Pokud jste hello databáze toohello cloudovou službu, můžete přistupovat hello škálování nastavení pro daný prostředek kliknutím na příslušný odkaz hello.

1. V hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a potom klikněte na název hello hello řídicí panel hello tooopen cloudové služby.
   
   > [!TIP]
   > Pokud nevidíte cloudové služby, pravděpodobně bude třeba toochange z **produkční** příliš**pracovní** nebo naopak.

2. Klikněte na tlačítko **škálování**.
3. Najde hello **propojené prostředky** části a klikněte na tlačítko **spravovat škálování pro tuto databázi**.
   
   > [!NOTE]
   > Pokud se nezobrazí **propojené prostředky** části pravděpodobně nemáte žádné propojené prostředky.

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
