---
title: "zásady aaaManage testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Další informace o velikosti zásad testovacího toodefine například virtuálních počítačů, maximální virtuálních počítačů na uživatele a vypnutí automatizace."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 351b3645a1fd729455884e5d177877c2986bd853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Správa všech zásad pro testovací prostředí v Azure DevTest Labs

Azure DevTest Labs umožňuje řídit náklady a minimalizovat odpady ve vaší laboratoře podle Správa zásad (nastavení) pro každý testovací prostředí. Tento článek krok za krokem podrobně vysvětluje, jak tooset každou zásadu.  

## <a name="set-allowed-virtual-machine-sizes"></a>Sada povolené velikosti virtuálních počítačů
Hello zásady pro nastavení hello povolené velikosti virtuálních počítačů pomáhá toominimize testovacím nakládání s tím, že vám toospecify jsou povoleny které velikosti virtuálních počítačů v testovacím prostředí hello. Pokud tato zásada je aktivována, může být pouze velikosti virtuálních počítačů z tohoto seznamu použité toocreate virtuálních počítačů.

1. V testovacím hello **konfiguraci a zásady** vyberte **povolené velikosti virtuálních počítačů**.
   
    ![Velikosti povolené virtuální počítače](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Vyberte **na** tooenable tuto zásadu a **vypnout** toodisable ho.

1. Pokud povolíte tuto zásadu, vyberte jeden nebo více velikosti virtuálních počítačů, které lze vytvořit ve svém testovacím prostředí.

1. Vyberte **Uložit**.

## <a name="set-virtual-machines-per-user"></a>Sada virtuálních počítačů na uživatele
Hello zásady pro **virtuálních počítačů na uživatele** vám umožní toospecify hello maximální počet virtuálních počítačů, které je možné vytvářet podle jednotlivých uživatelů. Pokud se uživatel pokusí toocreate nebo deklarace identity virtuálního počítače při byla splněna hello limit počtu uživatelů, chybová zpráva označuje tento virtuální počítač nelze vytvořit nebo žádat hello. 

1. V testovacím hello **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na uživatele**.
   
    ![Virtuální počítače na uživatele](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Vyberte **Ano** toolimit hello počet virtuálních počítačů na uživatele. Pokud nechcete, aby toolimit hello počet virtuálních počítačů na uživatele, vyberte **ne**. Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel hello. 

1. Vyberte **Ano** toolimit hello počet virtuálních počítačů, které můžete použít SSD (solid-state disk). Pokud nechcete, aby toolimit hello počet virtuálních počítačů, které můžete použít SSD, vyberte **ne**. Pokud vyberete **Ano**, zadejte hodnotu, která určuje maximální počet virtuálních počítačů, které lze vytvořit pomocí SSD hello. 

1. Vyberte **Uložit**.

## <a name="set-virtual-machines-per-lab"></a>Sada virtuálních počítačů na testovacího prostředí
Hello zásady pro **virtuálních počítačů na testovacím** vám umožní toospecify hello maximální počet virtuálních počítačů, které lze vytvořit pro aktuální prostředí hello. Pokud se uživatel pokusí toocreate virtuálního počítače při byla splněna limit hello testovacího prostředí, chybovou zprávu označuje, že hello virtuální počítač nelze vytvořit. 

1. V testovacím hello **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na testovacím**.
   
    ![Virtuální počítače na testovacího prostředí](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Vyberte **Ano** toolimit hello počet virtuálních počítačů za testovacího prostředí. Pokud nechcete, aby toolimit hello počet virtuálních počítačů za testovacího prostředí, vyberte **ne**. Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel hello. 

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

## <a name="set-expiration-date"></a>Nastavit datum vypršení platnosti
Můžete nastavit vypršení platnosti datum, kdy jste [vytvořit hello virtuálních počítačů](devtest-lab-add-vm.md). V **upřesňující nastavení**, zvolte hello kalendáře ikonu toospecify datum, na kterém hello virtuální počítač se automaticky odstraní.  Ve výchozím nastavení hello virtuálních počítačů nikdy nevyprší.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
Jakmile jste definovány a použity hello různá nastavení zásad virtuálních počítačů pro testovací prostředí, tady jsou některé věci tootry dále:

* [Pochopení sdílené IP adresy](devtest-lab-shared-ip.md) -vysvětluje, jak sdílené IP adresy se používají v DevTest Labs toominimize hello počet veřejných IP adres vyžaduje tooconnect tooyour prostředí virtuálních počítačů.
* [Konfigurovat náklady na správu](devtest-lab-configure-cost-management.md) -ukazuje, jak toouse hello **měsíční odhadované náklady Trend** grafu  
  tooview hello aktuálního měsíce odhadované náklady na začátku a náklady na koncový měsíc hello k projekci.
* [Vytvořit vlastní image](devtest-lab-create-template.md) – při vytváření virtuálního počítače, zadejte základ, který může být buď vlastní image nebo image pořízenou prostřednictvím Marketplace. Tento článek ukazuje, jak toocreate vlastní obrázek z soubor virtuálního pevného disku.
* [Konfigurace Marketplace image](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace. Tento článek ukazuje, jak toospecify, který, pokud existuje, Azure Marketplace bitové kopie lze použít při vytváření virtuálních počítačů v testovacím prostředí.
* [Vytvoření virtuálního počítače v testovacím prostředí](devtest-lab-add-vm-with-artifacts.md) -ukazuje, jak toocreate virtuální počítač z bitové kopie základní (buď vlastní nebo Marketplace) a jak toowork s artefakty ve vašem virtuálním počítači.

