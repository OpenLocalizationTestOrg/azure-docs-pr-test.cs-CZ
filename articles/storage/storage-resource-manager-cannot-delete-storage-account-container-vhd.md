---
title: "aaaTroubleshoot chyby při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky | Microsoft Docs"
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
ms.openlocfilehash: 77361593e2c924d39aba853e0807dc3188f50e60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a>Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky

Taky může docházet k chybám při pokusu toodelete účtu úložiště Azure, kontejner nebo virtuální pevný disk (VHD) v hello [portál Azure](https://portal.azure.com). Tento článek obsahuje řešení potíží s pokyny toohelp vyřešte hello problém v nasazení Azure Resource Manager.

Pokud v tomto článku není vaší Azure potíže vyřešit, navštivte hello fóra Azure na [MSDN a Stack Overflow](https://azure.microsoft.com/support/forums/). Problém můžete zveřejnit na tyto fóra nebo too@AzureSupport na Twitteru. Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na hello [podporu Azure](https://azure.microsoft.com/support/options/) lokality.

## <a name="symptoms"></a>Příznaky
### <a name="scenario-1"></a>Scénář 1
Když zkusíte toodelete virtuální pevný disk v účtu úložiště v nasazení Resource Manager, zobrazí se hello následující chybová zpráva:

**Nepodařilo toodelete blob 'vhds/BlobName.vhd'. Chyba: Není aktuálně zapůjčení u objektu blob hello a žádné ID zapůjčení byl zadaný v požadavku hello.**

Tento problém může vzniknout, protože virtuální počítač (VM) má zapůjčení na hello pokoušíte toodelete virtuálního pevného disku.

### <a name="scenario-2"></a>Scénář 2
Když zkusíte toodelete kontejneru v účtu úložiště v nasazení Resource Manager, zobrazí se hello následující chybová zpráva:

**Nepodařilo virtuální pevné disky, toodelete úložiště kontejneru'. Chyba: Není aktuálně k zapůjčení adresy v kontejneru hello a žádné ID zapůjčení byl zadaný v požadavku hello.**

Tento problém může vzniknout, protože má hello kontejneru VHD, který je v stavu zapůjčení hello uzamčena.

### <a name="scenario-3"></a>Scénář 3
Když zkusíte toodelete účet úložiště v nasazení Resource Manager, zobrazí se hello následující chybová zpráva:

**Účet úložiště toodelete 'StorageAccountName' se nezdařil Chyba: účet úložiště hello nelze odstranit z důvodu tooits artefakty se používají.**

Tento problém může vzniknout, protože účet úložiště hello obsahuje virtuální pevný disk, který je ve stavu zapůjčení hello.

## <a name="solution"></a>Řešení 
tooresolve tyto problémy musíte určit hello virtuálního pevného disku, který je příčinou chyby hello a hello přidružené virtuálních počítačů. Potom odpojit hello virtuálního pevného disku z hello virtuálního počítače (pro datové disky) nebo odstranění hello virtuální počítač, který používá hello virtuálního pevného disku (pro disky operačního systému). Tato zapůjčení hello odebere hello virtuálního pevného disku a povolí jeho toobe odstranit. 

toodo, použijte některou z následujících metod hello:

### <a name="method-1---use-azure-storage-explorer"></a>Metoda 1 - použijte Azure storage Exploreru

### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a>Krok 1 identifikace hello virtuálního pevného disku, které zabrání odstranění účtu úložiště hello

1. Pokud odstraníte účet úložiště hello, zobrazí se dialogové okno zprávy například hello následující: 

    ![zpráva při odstraňování účtu úložiště hello](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Zkontrolujte hello **disku URL** účet úložiště hello tooidentify a hello virtuálního pevného disku, který brání odstranit účet úložiště hello. V následujícím příkladu hello, hello řetězec před ". blob.core.windows.net" je název účtu úložiště hello a "SCCM2012-2015-08-28.vhd" je název disku VHD hello.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-hello-vhd-by-using-azure-storage-explorer"></a>Krok 2 odstranění hello virtuálního pevného disku pomocí Průzkumníka úložiště Azure

1. Stažení a instalace hello nejnovější verzi [Azure Storage Explorer](http://storageexplorer.com/). Tento nástroj je samostatná aplikace od společnosti Microsoft, který vám umožní tooeasily práci s daty Azure Storage ve Windows, systému macOS a Linux.
2. Otevřete Azure Storage Explorer, vyberte ![Ikona účtu](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) v levém panelu hello vyberte prostředí Azure a pak se přihlaste.

3. Vyberte všechny odběry nebo hello odběr, který obsahuje hello úložiště účet, že který má toodelete.

    ![Přidat předplatné](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. Přejděte toohello účet úložiště, který jsme získali z hello starší, vyberte disk URL hello **kontejnery objektů Blob** > **virtuální pevné disky** a vyhledejte řetězec hello virtuálního pevného disku, který brání účet úložiště odstranit hello.
5. Pokud je nalezen hello virtuální pevný disk, zkontrolujte hello **název virtuálního počítače** sloupec toofind hello virtuální počítač, který používá tento virtuální pevný disk.

    ![Zkontrolujte virtuálních počítačů](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. Odebrání hello zapůjčení hello virtuální pevný disk pomocí portálu Azure. Další informace najdete v tématu [odebrat hello zapůjčení ze hello virtuálního pevného disku](#remove-the-lease-from-the-vhd). 

7. Přejděte toohello Azure Storage Explorer, klikněte pravým tlačítkem na hello virtuální pevný disk a potom vyberte možnost odstranit.

8. Odstraňte účet úložiště hello.

### <a name="method-2---use-azure-portal"></a>Metoda 2 - používat portál Azure 

#### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a>Krok 1: Identifikace hello virtuálního pevného disku, které zabrání odstranění účtu úložiště hello

1. Pokud odstraníte účet úložiště hello, zobrazí se dialogové okno zprávy například hello následující: 

    ![zpráva při odstraňování účtu úložiště hello](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Zkontrolujte hello **disku URL** účet úložiště hello tooidentify a hello virtuálního pevného disku, který brání odstranit účet úložiště hello. V následujícím příkladu hello, hello řetězec před ". blob.core.windows.net" je název účtu úložiště hello a "SCCM2012-2015-08-28.vhd" je název disku VHD hello.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. Přihlaste se toohello [portál Azure](https://portal.azure.com).
3. V nabídce centra hello vyberte **všechny prostředky**. Přejděte toohello účet úložiště a potom vyberte **objekty BLOB** > **virtuální pevné disky**.

    ![Snímek obrazovky portálu hello s hello účet úložiště a kontejneru "VHD" hello zvýrazněná](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. Vyhledejte hello virtuálního pevného disku, který jsme získali dříve z adresy URL hello disku. Pak určit, které je virtuální počítač pomocí hello virtuálního pevného disku. Obvykle můžete určit, která obsahuje virtuální počítač hello virtuálního pevného disku kontrolou název hello virtuálního pevného disku:

Virtuální počítač v modelu Resource Manager vývoj

   * Disky operačního systému postupujte podle obecně tyto zásady vytváření názvů: VMName-rrrr-MM-DD-HHMMSS.vhd
   * Datové disky obecně postupujte podle zásad vytváření názvů: VMName-rrrr-MM-DD-HHMMSS.vhd

Virtuální počítač v klasickém modelu vývoj

   * Disky operačního systému postupujte podle obecně tyto zásady vytváření názvů: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd
   * Datové disky obecně postupujte podle zásad vytváření názvů: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd

#### <a name="step-2-remove-hello-lease-from-hello-vhd"></a>Krok 2: Odebrání hello zapůjčení hello virtuálního pevného disku

[Odebrání hello zapůjčení hello virtuálního pevného disku](#remove-the-lease-from-the-vhd)a potom odstraňte účet úložiště hello.

## <a name="what-is-a-lease"></a>Co je zapůjčení?
Zapůjčení je uzamčen, kterou lze použít toocontrol přístup tooa blob (například VHD). Když je pronajatý objekt blob, pouze hello vlastníci hello zapůjčení přistupovat k objektu blob hello. Zapůjčení je důležité pro hello následujících důvodů:

* Brání poškození dat, pokud více vlastníky zkuste toowrite toohello stejnou část hello blob v hello stejnou dobu.
* Objekt blob hello zabrání odstraňuje Pokud něco aktivně používá ho (například počítač).
* Účet úložiště hello zabrání odstraňuje Pokud něco aktivně používá ho (například počítač).

### <a name="remove-hello-lease-from-hello-vhd"></a>Odebrání hello zapůjčení hello virtuálního pevného disku
Pokud hello virtuálního pevného disku je disk s operačním systémem, je nutné odstranit hello virtuálních počítačů tooremove hello zapůjčení:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na hello **rozbočovače** nabídce vyberte možnost **virtuální počítače**.
3. Vyberte hello virtuální počítač, který obsahuje zapůjčení na hello virtuálního pevného disku.
4. Ujistěte se, že nic aktivně používá hello virtuálního počítače a že jste už potřebovat hello virtuálního počítače.
5. Hello horní části hello **podrobnosti virtuálního počítače** vyberte **odstranit**a potom klikněte na **Ano** tooconfirm.
6. měla by být odstraněna Hello virtuálních počítačů, ale uchovávání hello virtuální pevný disk může být. Ale hello VHD už měli zapůjčení na něm. To může trvat několik minut, než toobe zapůjčení hello vydané. vydání tooverify, který hello zapůjčení, přejděte příliš**všechny prostředky** > **název účtu úložiště** > **objekty BLOB**  >  **virtuální pevné disky**. V hello **Blob vlastnosti** podokně, hello **zapůjčení stav** hodnota by měla být **Odemknutý**.

Pokud hello virtuálního pevného disku je datový disk, odpojte hello virtuálního pevného disku z hello virtuálních počítačů tooremove hello zapůjčení:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na hello **rozbočovače** nabídce vyberte možnost **virtuální počítače**.
3. Vyberte hello virtuální počítač, který obsahuje zapůjčení na hello virtuálního pevného disku.
4. Vyberte **disky** na hello **podrobnosti virtuálního počítače** okno.
5. Vyberte hello datový disk, který obsahuje zapůjčení na hello virtuálního pevného disku. Můžete určit, která je připojena virtuálního pevného disku v hello disku kontrolou hello URL hello virtuálního pevného disku.
6. Určení s jistotou určit, že nic aktivně používá hello datový disk.
7. Klikněte na tlačítko **odpojení** na hello **disku podrobnosti** okno.
8. Hello disku by měl nyní odpojit od hello virtuálních počítačů a hello virtuálního pevného disku na něm už měli zapůjčení. To může trvat několik minut, než toobe zapůjčení hello vydané. byla vydána tooverify, který hello zapůjčení, přejděte příliš**všechny prostředky** > **název účtu úložiště** > **objekty BLOB**  >  **virtuální pevné disky**. V hello **Blob vlastnosti** podokně, hello **zapůjčení stav** hodnota by měla být **Odemknutý**.

## <a name="next-steps"></a>Další kroky
* [Odstranit účet úložiště](storage-create-storage-account.md#delete-a-storage-account)
* [Jak toobreak hello uzamčení zapůjčení úložiště objektů blob ve službě Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
