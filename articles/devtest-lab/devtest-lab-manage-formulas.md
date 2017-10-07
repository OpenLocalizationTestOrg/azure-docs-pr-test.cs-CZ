---
title: "vzorce aaaManage v Azure DevTest Labs toocreate virtuální počítače | Microsoft Docs"
description: "Zjistěte, jak tooupdate a odebrat vzorce Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 855debe46f3b70cc45ea5d55869663b64e225124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-devtest-labs-formulas"></a>Správa Azure DevTest Labs vzorce

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

Tento článek ukazuje, jak toocreate vzorec ze základní (vlastní image, Marketplace image nebo jiného vzorce) nebo stávajícího virtuálního počítače. Tento článek také vás provede Správa existující vzorce.

## <a name="create-a-formula"></a>Vytvoření vzorce
Každý, kdo má DevTest Labs *uživatelé* oprávnění je možné toocreate virtuálních počítačů pomocí vzorce jako základ. Existují dva způsoby toocreate vzorce: 

* Od základní - použijte, pokud chcete toodefine všechny hello charakteristiky hello vzorec.
* Z existujícího testovacího prostředí virtuálního počítače – použijte v případě, že chcete toocreate vzorec na základě hello nastavení existujícího virtuálního počítače.

Další informace o přidávání uživatelů a oprávnění najdete v tématu [přidat vlastníků a uživatelé v Azure DevTest Labs](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>Vytvoření vzorce od základní
Hello následující kroky vás provede procesem hello vytvoření vzorec z vlastní image, Marketplace image nebo jiného vzorce.

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

2. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.

3. Ze seznamu hello labs vyberte požadované prostředí hello.  

4. V okně prostředí hello vyberte **vzorce (opakovaně použitelné základů)**.
   
    ![Vzorce nabídky](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. Na hello **vzorce** vyberte **+ přidat**.
   
    ![Přidat vzorec.](./media/devtest-lab-create-formulas/add-formula.png)

6. Na hello **zvolte na základní** okně vyberte hello základní (vlastní image, Marketplace image nebo vzorec) ze kterého mají být toocreate hello vzorec.
   
    ![Základní seznam](./media/devtest-lab-create-formulas/base-list.png)

7. Na hello **vytvořit vzorec** okno, zadejte hello následující hodnoty:
   
    * **Název vzorce** – zadejte název vzorce. Tato hodnota se zobrazí v seznamu hello základní Image při vytváření virtuálního počítače. Název Hello je ověřit, protože jej zadáte, a pokud není platný, zprávu označuje hello požadavky platný název.
    * **Popis** -Zadejte výstižný popis vzorce. Tato hodnota je k dispozici hello vzorec kontextové nabídky, při vytvoření virtuálního počítače.
    * **Uživatelské jméno** -zadejte uživatelské jméno, které jsou udělena oprávnění správce.
    * **Heslo** – zadejte - nebo vyberte z rozevíracího seznamu hello - hodnotu, která souvisí s hello heslo, že chcete toouse pro zadaného uživatele hello. Další informace o hello tajných klíčů najdete v tématu [Azure DevTest Labs: osobním úložišti tajný](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).
    * **Typ disku virtuálního počítače** – zadejte buď HDD (jednotku pevného disku) nebo tooindicate SSD (solid-state jednotka), které úložiště typ disku je povoleno pro hello virtuální počítače zřízené tuto základní image používá.
    * ** Virtuální počítač velikost ** – vyberte jednu z předdefinovaných hello položky, které zadejte hello jader procesoru, velikost paměti RAM a velikost pevného disku hello toocreate hello virtuálních počítačů. 
    * **Artefakty** -vyberte tooopen hello **přidat artefakty** okno, ve kterém můžete vybrat a nakonfigurovat hello artefakty, že chcete tooadd toohello základní image. Další informace o artefakty najdete v tématu [artefakty Správa virtuálních počítačů v Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).
    * **Upřesňující nastavení** -vyberte tooopen hello **Upřesnit** okno, kde můžete konfigurovat hello následující nastavení:
        * **Virtuální síť** -určit hello požadován virtuální sítě.
        * **Podsíť** -zadejte podsíť hello potřeby.    
        * **Konfiguraci IP adresy** – určete, zda mají hello veřejné, privátní nebo sdílet IP adresy. Další informace o sdílené IP adresy najdete v tématu [Rady pro pochopení sdílet IP adresy v Azure DevTest Labs](./devtest-lab-shared-ip.md).
        * **Nastavit tento počítač vymahatelných** -provádění na počítač "vymahatelných" znamená, že jej nebude přiřadit vlastnictví v době vytvoření hello. Místo toho budou uživatelé testovacího prostředí počítače hello možné tootake vlastnictví (dále jen "deklarace identity") v okně prostředí hello.     
    * **Obrázek** – toto pole zobrazuje název hello základní bitovou kopii, jste vybrali na předchozí okno hello. 
     
       ![Vytvoření vzorce](./media/devtest-lab-create-formulas/create-formula.png)

8. Vyberte **vytvořit** toocreate hello vzorec.

9. Po vytvoření hello vzorec, se zobrazí v seznamu hello na hello **vzorce** okno.

### <a name="create-a-formula-from-a-vm"></a>Vytvoření vzorce z virtuálního počítače
Hello následující kroky vás provedou procesem hello vytvoření vzorec podle stávajícího virtuálního počítače. 

> [!NOTE]
> toocreate vzorec z virtuálního počítače, hello virtuálního počítače musí být vytvořen po 30. března 2016. 
> 
> 

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
3. Ze seznamu hello labs vyberte požadované prostředí hello.  
4. V testovacím hello **přehled** okně vyberte hello virtuálních počítačů, ze kterého chcete toocreate hello vzorec.
   
    ![Virtuální laboratoře počítače](./media/devtest-lab-create-formulas/my-vms.png)
5. V okně hello Virtuálního počítače, vyberte **vytvořit vzorec (opakovaně použitelné base)**.
   
    ![Vytvoření vzorce](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. Na hello **vytvořit vzorec** okno, zadejte **název** a **popis** pro nový vzorec.
   
    ![Vzorce okno vytvořit](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Vyberte **OK** toocreate hello vzorec.

## <a name="modify-a-formula"></a>Upravit vzorec.
toomodify vzorec, postupujte takto:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
3. Ze seznamu hello labs vyberte požadované prostředí hello.  
4. V okně prostředí hello vyberte **vzorce (opakovaně použitelné základů)**.
   
    ![Vzorce nabídky](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Na hello **testovacím vzorce** okně, vyberte hello vzorec chcete toomodify.
6. Na hello **aktualizovat vzorec** okně hello potřeby úpravy a vyberte **aktualizace**.

## <a name="delete-a-formula"></a>Odstranit vzorec.
toodelete vzorec, postupujte takto:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
3. Ze seznamu hello labs vyberte požadované prostředí hello.  
4. V testovacím hello **nastavení** vyberte **vzorce**.
   
    ![Vzorce nabídky](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Na hello **testovacím vzorce** okně, vyberte hello toohello třemi tečkami napravo od hello vzorec chcete toodelete.
   
    ![Vzorce nabídky](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. V místní nabídce hello vzorec, vyberte **odstranit**.
   
    ![Vzorce kontextové nabídky](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Vyberte **Ano** toohello dialogové okno potvrzení odstranění.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související
* [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Další kroky
Po vytvoření vzorec pro použití při vytváření virtuálního počítače, je dalším krokem hello příliš[přidání testovacího prostředí virtuálních počítačů tooyour](devtest-lab-add-vm-with-artifacts.md).

