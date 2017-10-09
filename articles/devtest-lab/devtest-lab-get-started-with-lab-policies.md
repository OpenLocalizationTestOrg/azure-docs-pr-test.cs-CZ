---
title: "zásady aaaManage základní testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tooset některé základní zásady hello (nastavení) pro testovací prostředí v DevTest Labs"
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
ms.date: 06/29/2017
ms.author: tarcher
ms.openlocfilehash: 792c0d19cfe73964be9c03d3de7751a868fd86e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a>Spravovat základní zásady pro testovací prostředí v Azure DevTest Labs

Azure DevTest Labs vám umožní toocontrol náklady a minimalizovat odpady ve vaší laboratoře podle Správa zásad (nastavení) pro každý testovací prostředí. V tomto článku jste Začínáme se zásadami podle učení jak tooset dva nejdůležitější zásady hello – omezení hello počet virtuálních počítačů (VM), které můžou vytvořit nebo nárokován jednoho uživatele a konfiguraci automatického vypnutí. tooview jak tooset každé zásadě testovacího prostředí, najdete v článku hello [definovat zásady testovacího prostředí v Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a>Přístup k zásady testovacího prostředí v Azure DevTest Labs
Hello následující postup vás provede nastavením zásad pro testovací prostředí v Azure DevTest Labs:

tooview (a změňte) hello zásady pro testovací prostředí, postupujte takto:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.

1. Ze seznamu hello labs vyberte požadované prostředí hello.   

1. Vyberte **konfiguraci a zásady**.

    ![Okna nastavení zásad](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Hello **konfiguraci a zásady** okno obsahuje nabídky nastavení, které můžete zadat. Tento článek se týká pouze hello nastavení pro **virtuálních počítačů na uživatele** a **Auto-shutdown**. toolearn o hello zbývající nastavení, najdete v části [Správa všech zásad pro testovací prostředí v Azure DevTest Labs](./devtest-lab-set-lab-policy.md). 
   
## <a name="set-virtual-machines-per-user"></a>Sada virtuálních počítačů na uživatele
Hello zásady pro **virtuálních počítačů na uživatele** vám umožní toospecify hello maximální počet virtuálních počítačů, které je možné vytvářet podle jednotlivých uživatelů. Pokud se uživatel pokusí toocreate nebo deklarace identity virtuálního počítače při byla splněna hello limit počtu uživatelů, chybová zpráva označuje tento virtuální počítač nelze vytvořit nebo žádat hello. 

1. V testovacím hello **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na uživatele**.
   
    ![Virtuální počítače na uživatele](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Vyberte **Ano** toolimit hello počet virtuálních počítačů na uživatele. Pokud nechcete, aby toolimit hello počet virtuálních počítačů na uživatele, vyberte **ne**. Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel hello. 

1. Vyberte **Ano** toolimit hello počet virtuálních počítačů, které můžete použít SSD (solid-state disk). Pokud nechcete, aby toolimit hello počet virtuálních počítačů, které můžete použít SSD, vyberte **ne**. Pokud vyberete **Ano**, zadejte hodnotu, která určuje maximální počet virtuálních počítačů, které lze vytvořit pomocí SSD hello. 

1. Vyberte **Uložit**.

## <a name="set-auto-shutdown"></a>Sada auto vypnutí
vypnutí automatického zásada Hello pomáhá toominimize testovacím nakládání s tím, že jste toospecify hello dobu, po kterou vypnout virtuální počítače v tomto testovacím prostředí.

1. V testovacím hello **konfiguraci a zásady** vyberte **Auto-shutdown**.
   
    ![Vypnutí automatického](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Vyberte **na** tooenable tuto zásadu a **vypnout** toodisable ho.

1. Pokud povolíte tuto zásadu, zadejte hello (a časové pásmo) tooshut dolů všech virtuálních počítačů v testovacím prostředí aktuální hello.

1. Zadejte **Ano** nebo **ne** pro toosend možnost hello předchozí toohello oznámení 15 minut zadaný čas ukončení automaticky. Pokud zadáte **Ano**, zadejte adresu URL webhooku koncový bod tooreceive hello oznámení. Další informace o webhooky najdete v tématu [vytvoření webhooku nebo funkce rozhraní API Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Vyberte **Uložit**.

    Ve výchozím nastavení, až bude povolená tato zásada platí tooall virtuálních počítačů v testovacím prostředí aktuální hello. tooremove toto nastavení z konkrétní virtuální počítač, otevřete okno hello Virtuálního počítače a změňte jeho **Auto-shutdown** nastavení 

## <a name="set-auto-start"></a>Sada automatického – spuštění
Zásady automatické spuštění Hello povoluje toospecify při hello virtuálních počítačů v testovacím prostředí aktuální hello by měl být spuštěn.  

1. V testovacím hello **konfiguraci a zásady** vyberte **automatického spuštění**.
   
    ![Automatické spuštění](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Vyberte **na** tooenable tuto zásadu a **vypnout** toodisable ho.

3. Pokud povolíte tuto zásadu, zadejte hello naplánovaný čas zahájení, časové pásmo a hello dny v týdnu hello, pro které hello doba platí. 

4. Vyberte **Uložit**.

    Jakmile bude povoleno, tato zásada není automaticky použité tooany virtuálních počítačů v testovacím prostředí aktuální hello. tooapply tato nastavení tooa konkrétní virtuální počítač, okno otevřít hello Virtuálního počítače a změňte jeho **automatického spuštění** nastavení 

## <a name="next-steps"></a>Další kroky

- [Definovat zásady testovacího prostředí v Azure DevTest Labs](devtest-lab-set-lab-policy.md) – zjistěte, jak toomodify jiných zásad testovacího prostředí 
