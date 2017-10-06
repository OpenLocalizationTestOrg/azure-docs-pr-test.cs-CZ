---
title: "aaaAuto škálování cloudové služby na portálu hello | Microsoft Docs"
description: "Zjistěte, jak toouse hello portálu tooconfigure automatické škálování pravidla pro cloudové služby webovou roli nebo role pracovního procesu v Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 265f4c8ec5e1ec2f85585df25f18cd0d0c9946a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-portal"></a>Jak tooconfigure automatické škálování pro cloudové služby v rámci portálu hello
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-scale-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-scale.md)

Podmínky lze nastavit pro roli pracovního procesu cloudové služby, které aktivovat škálování nebo výstupní operace. Hello podmínky pro hello role může být založená na hello procesoru, disku nebo zatížení sítě hello role. Můžete také nastavit podmínku podle zprávy fronty nebo hello metrikou jiný Azure prostředek spojené s vaším předplatným.

> [!NOTE]
> Tento článek se zaměřuje na webových a pracovních rolí cloudové služby. Když vytvoříte virtuální počítač (klasický) přímo, je hostovaná v cloudové službě. Standardní virtuálního počítače je možné škálovat jeho s přidružením [skupinu dostupnosti](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) a ručně je zapnout nebo vypnout.

## <a name="considerations"></a>Požadavky
Měli byste zvážit hello před konfigurací škálování pro vaši aplikaci následující informace:

* Škálování má vliv základní použití.

    Větší instance rolí pomocí více jader. Je možné škálovat aplikaci pouze v rámci limitu hello jader pro předplatné. Řekněme například, že vaše předplatné může mít 20 jader. Pokud spouštíte aplikaci pomocí dvou středních cloudové služby (celkem 4 jádra), můžete pouze postupně škálovat ostatních nasazení cloudové služby ve vašem předplatném ve zbývajících 16 jader hello. Další informace o velikosti najdete v tématu [cloudové služby velikosti](cloud-services-sizes-specs.md).

* Je možné škálovat podle prahová hodnota fronty zpráv. Další informace o tom, najdete v části front toouse [jak toouse hello fronty úložiště služby](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Můžete škálovat také další prostředky spojené s vaším předplatným.

* tooenable vysokou dostupnost vaší aplikace, ujistěte se, že je nasazený s dvěma nebo více instancí role. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).


## <a name="where-scale-is-located"></a>Kde se nachází škálování
Po výběru cloudové služby, měli byste mít hello cloudové služby okno viditelné.

1. V okně služby cloud hello, na hello **rolí a instancí** dlaždice, vyberte hello název hello cloudové služby.   
   **Důležité**: Ujistěte se, že tooclick hello cloudové služby role, není hello instanci role, která je nižší než hello role.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. Vyberte hello **škálování** dlaždici.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatické škálování
Nastavení škálování v roli lze nakonfigurovat buď dva režimy **ruční** nebo **automatické**. Ruční je, jak jste zvyklí, nastavíte hello absolutní počet instancí. Automatické ale umožňuje tooset, které by měl škálování pravidla, které řídí způsob a jak mnohem je.

Sada hello **škálovat podle** možnost příliš**pravidla plánu a výkonu**.

![Nastavení škálování cloudové služby s profil a pravidla](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Existující profil.
2. Přidáte pravidlo pro profil nadřazené hello.
3. Přidejte jiný profil.

Vyberte **přidat profil**. Hello profil určuje režimu, který chcete použít pro škálování hello toouse: **vždy**, **opakování**, **pevným datem**.

Po dokončení konfigurace profilu hello a pravidla, vyberte hello **Uložit** ikona v horní části hello.

#### <a name="profile"></a>Profil
profil Hello Nastaví minimální a maximální počet instancí pro hello škálování a když tento rozsah škálování je také aktivní.

* **Vždy**

    Vždy mít tento rozsah instancí, které jsou k dispozici.  

    ![Cloudová služba, která vždy škálování](./media/cloud-services-how-to-scale-portal/select-always.png)
* **Opakování**

    Vyberte sadu dny v týdnu tooscale hello.

    ![Cloudové služby škálování s plán opakování](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* **Pevné datum**

    Pevné datum rozsahu tooscale hello roli.

    ![Cloudové služby škálování s pevnou datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Po nakonfigurování hello profil, vyberte hello **OK** tlačítko v hello dolní části okna profil hello.

#### <a name="rule"></a>Pravidlo
Pravidla se přidají tooa profil a představují podmínku, která aktivuje hello škálování.

aktivační událost pravidlo Hello je založena na metrika hello cloudové služby (využití procesoru, disková aktivita nebo síťové aktivity) toowhich podmíněného hodnotu můžete přidat. Kromě toho může mít hello aktivační události podle zprávy fronty nebo hello metrikou jiný Azure prostředek spojené s vaším předplatným.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Po nakonfigurování hello pravidla, vyberte hello **OK** tlačítko v hello dolní části okna pravidlo hello.

## <a name="back-toomanual-scale"></a>Zpět toomanual škálování
Přejděte toohello [nastavení škálování](#where-scale-is-located) a sadu hello **škálovat podle** možnost příliš**počet instancí, který nastavím ručně**.

![Nastavení škálování cloudové služby s profil a pravidla](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Toto nastavení se odebere z hello role automatické škálování a potom můžete nastavit počet instancí hello přímo.

1. možnost Hello škálování (ruční nebo automatické).
2. Role instance posuvníku tooset hello instancí tooscale k.
3. Instance role tooscale hello k.

Po nakonfigurování nastavení škálování hello vyberte hello **Uložit** ikona v horní části hello.
