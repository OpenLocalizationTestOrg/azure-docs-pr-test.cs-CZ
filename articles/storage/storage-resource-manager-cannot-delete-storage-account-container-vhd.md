---
title: "Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky | Microsoft Docs"
description: "Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky"
services: storage
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 318d7146ea53a806baf813ff7de2fe91f18becc8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a>Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky

Taky může docházet k chybám při pokusu o odstranění účtu úložiště Azure, kontejneru nebo virtuální pevný disk (VHD) v [portál Azure](https://portal.azure.com). Tento článek obsahuje pokyny k odstraňování problémů pomáhající při řešení problému v nasazení Azure Resource Manager.

Pokud v tomto článku není vaší Azure potíže vyřešit, navštivte fórech Azure na [MSDN a Stack Overflow](https://azure.microsoft.com/support/forums/). Problém můžete účtovat na tyto fóra nebo na @AzureSupport na Twitteru. Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na [podporu Azure](https://azure.microsoft.com/support/options/) lokality.

## <a name="symptoms"></a>Příznaky
### <a name="scenario-1"></a>Scénář 1
Když se pokusíte odstranit virtuální pevný disk v účtu úložiště v nasazení Resource Manager, zobrazí se následující chybová zpráva:

**Nepodařilo se odstranit objekt blob, vhds/BlobName.vhd'. Chyba: Není aktuálně zapůjčení u objektu blob a žádné ID zapůjčení zadaná v žádosti.**

Tento problém může vzniknout, protože virtuální počítač (VM) má zapůjčení na virtuální pevný disk, který se pokoušíte odstranit.

### <a name="scenario-2"></a>Scénář 2
Když se pokusíte odstranit kontejner v účtu úložiště v nasazení Resource Manager, zobrazí se následující chybová zpráva:

**Nepodařilo se odstranit virtuální pevné disky, úložiště kontejneru'. Chyba: Není aktuálně k zapůjčení adresy v kontejneru a žádné ID zapůjčení zadaná v žádosti.**

Tento problém může vzniknout, protože má kontejneru VHD, který je uzamčen ve stavu zapůjčení.

### <a name="scenario-3"></a>Scénář 3
Když se pokusíte odstranit účet úložiště v nasazení Resource Manager, zobrazí se následující chybová zpráva:

**Odstranění účtu úložiště 'StorageAccountName' se nezdařilo. Chyba: Účet úložiště nelze odstranit, protože některé jeho artefakty se používají.**

Tento problém může vzniknout, protože účet úložiště obsahuje virtuální pevný disk, který je ve stavu zapůjčení.

## <a name="solution"></a>Řešení 
Chcete-li tyto problémy vyřešit, musíte určit virtuální pevný disk, který je příčinou chyby a přidružený virtuální počítač. Potom odpojte virtuální pevný disk z virtuálního počítače (pro datové disky) nebo odstranění virtuálního počítače, který je pomocí virtuálního pevného disku (pro disky operačního systému). Tato zapůjčení odebere z virtuálního pevného disku a umožňuje pro odstranění. 

K tomuto účelu použijte jednu z následujících metod:

### <a name="method-1---use-azure-storage-explorer"></a>Metoda 1 - použijte Azure storage Exploreru

### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a>Krok 1 identifikace virtuálního pevného disku, které zabrání odstranění účtu úložiště

1. Pokud odstraníte účet úložiště, zobrazí se dialogové okno zprávy například následující: 

    ![Při odstranění účtu úložiště se zobrazí zpráva](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Zkontrolujte **disku URL** k identifikaci úložiště k účtu a virtuální pevný disk, který brání odstranění účtu úložiště. V následujícím příkladu, řetězec před ". blob.core.windows.net" je název účtu úložiště a "SCCM2012-2015-08-28.vhd" je název disku VHD.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-the-vhd-by-using-azure-storage-explorer"></a>Krok 2 odstranit virtuální pevný disk pomocí Průzkumníka úložiště Azure

1. Stáhněte a nainstalujte nejnovější verzi [Azure Storage Explorer](http://storageexplorer.com/). Tento nástroj je samostatná aplikace od společnosti Microsoft, která umožňuje snadno pracovat s daty Azure Storage ve Windows, systému macOS a Linux.
2. Otevřete Azure Storage Explorer, vyberte ![Ikona účtu](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) v levém panelu vyberte prostředí Azure a pak se přihlaste.

3. Vyberte všechny odběry nebo odběr, který obsahuje účet úložiště, které chcete odstranit.

    ![Přidat předplatné](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. Přejděte na účet úložiště, který jsme získali z adresy URL disku starší, vyberte **kontejnery objektů Blob** > **virtuální pevné disky** a vyhledávání pro virtuální pevný disk, který brání odstranění účtu úložiště.
5. Pokud se najde virtuální pevný disk, zkontrolujte **název virtuálního počítače** sloupec, který se najít virtuální počítač, který používá tento virtuální pevný disk.

    ![Zkontrolujte virtuálních počítačů](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. Odebrání zapůjčení virtuální pevný disk pomocí portálu Azure. Další informace najdete v tématu [zapůjčení odebrání virtuálního pevného disku](#remove-the-lease-from-the-vhd). 

7. Přejděte do Azure Storage Explorer, klikněte pravým tlačítkem na virtuální pevný disk a potom vyberte možnost odstranit.

8. Umožňuje odstranit účet úložiště.

### <a name="method-2---use-azure-portal"></a>Metoda 2 - používat portál Azure 

#### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a>Krok 1: Identifikace virtuálního pevného disku, které zabrání odstranění účtu úložiště

1. Pokud odstraníte účet úložiště, zobrazí se dialogové okno zprávy například následující: 

    ![Při odstranění účtu úložiště se zobrazí zpráva](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Zkontrolujte **disku URL** k identifikaci úložiště k účtu a virtuální pevný disk, který brání odstranění účtu úložiště. V následujícím příkladu, řetězec před ". blob.core.windows.net" je název účtu úložiště a "SCCM2012-2015-08-28.vhd" je název disku VHD.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
3. V nabídce centra vyberte **všechny prostředky**. Přejděte k účtu úložiště a pak vyberte **objekty BLOB** > **virtuální pevné disky**.

    ![Snímek obrazovky portálu, se účet úložiště a kontejneru "VHD" zvýrazněná](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. Najděte virtuální pevný disk, který jsme získali dříve z adresy URL disku. Pak určit, které je virtuální počítač pomocí virtuálního pevného disku. Obvykle můžete určit, která virtuální počítač obsahuje virtuální pevný disk kontrolou název virtuálního pevného disku:

Virtuální počítač v modelu Resource Manager vývoj

   * Disky operačního systému postupujte podle obecně tyto zásady vytváření názvů: VMName-rrrr-MM-DD-HHMMSS.vhd
   * Datové disky obecně postupujte podle zásad vytváření názvů: VMName-rrrr-MM-DD-HHMMSS.vhd

Virtuální počítač v klasickém modelu vývoj

   * Disky operačního systému postupujte podle obecně tyto zásady vytváření názvů: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd
   * Datové disky obecně postupujte podle zásad vytváření názvů: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd

#### <a name="step-2-remove-the-lease-from-the-vhd"></a>Krok 2: Odeberte zapůjčení z virtuálního pevného disku

[Odebrání virtuálního pevného disku zapůjčení](#remove-the-lease-from-the-vhd)a potom odstraňte účet úložiště.

## <a name="what-is-a-lease"></a>Co je zapůjčení?
Zapůjčení je uzamčen, který můžete použít k řízení přístupu do objektu blob (například VHD). Když je pronajatý objekt blob, jenom vlastníci zapůjčení přistupovat k objektu blob. Zapůjčení je důležité z následujících důvodů:

* Zabrání poškození dat pokud více vlastníky zkuste k zápisu do stejnou část objektu blob ve stejnou dobu.
* Objekt blob zabrání odstraňuje Pokud něco aktivně používá ho (například počítač).
* Účet úložiště zabrání odstraňuje Pokud něco aktivně používá ho (například počítač).

### <a name="remove-the-lease-from-the-vhd"></a>Odeberte zapůjčení z virtuálního pevného disku
Pokud virtuální pevný disk je disk s operačním systémem, je nutné odstranit virtuální počítač odebrat zapůjčení:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na **rozbočovače** nabídce vyberte možnost **virtuální počítače**.
3. Vyberte virtuální počítač, který obsahuje zapůjčení na virtuální pevný disk.
4. Ujistěte se, že nic aktivně používá virtuální počítač a virtuální počítač už nepotřebujete.
5. V horní části **podrobnosti virtuálního počítače** vyberte **odstranit**a potom klikněte na **Ano** k potvrzení.
6. Virtuální počítač by měl být odstraněn, ale uchovávání může být virtuální pevný disk. Ale VHD už měli zapůjčení na něm. To může trvat několik minut, než zapůjčení k uvolnění. Chcete-li ověřit, že zapůjčení vydání, přejděte na **všechny prostředky** > **název účtu úložiště** > **objekty BLOB** > **virtuální pevné disky**. V **Blob vlastnosti** podokně **zapůjčení stav** hodnota by měla být **Odemknutý**.

Pokud virtuální pevný disk je datový disk, odpojte virtuální pevný disk z virtuálního počítače odebrat zapůjčení:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na **rozbočovače** nabídce vyberte možnost **virtuální počítače**.
3. Vyberte virtuální počítač, který obsahuje zapůjčení na virtuální pevný disk.
4. Vyberte **disky** na **podrobnosti virtuálního počítače** okno.
5. Vyberte datový disk, který obsahuje zapůjčení na virtuální pevný disk. Můžete určit, která je připojena virtuálního pevného disku v disku kontrolou adresu URL virtuálního pevného disku.
6. Určení s jistotou určit, že nic aktivně používá datový disk.
7. Klikněte na tlačítko **odpojení** na **disku podrobnosti** okno.
8. Z virtuálního počítače, by měl nyní odpojit disk a virtuální pevný disk na něm už měli zapůjčení. To může trvat několik minut, než zapůjčení k uvolnění. Chcete-li ověřit, že byla vydána zapůjčení, přejděte na **všechny prostředky** > **název účtu úložiště** > **objekty BLOB** > **virtuální pevné disky**. V **Blob vlastnosti** podokně **zapůjčení stav** hodnota by měla být **Odemknutý**.

## <a name="next-steps"></a>Další kroky
* [Odstranit účet úložiště](storage-create-storage-account.md#delete-a-storage-account)
* [K ukončení uzamčení zapůjčení úložiště objektů blob ve službě Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
