---
title: "aaaManage vašeho účtu úložiště, StorSimple | Microsoft Docs"
description: "Vysvětluje, jak můžete použít hello tooadd stránku konfigurace StorSimple Manager, úpravy, odstranění nebo otáčení hello zabezpečení klíče pro účet úložiště."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 93207c40-e0eb-489e-8724-59fb94907081
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/29/2016
ms.author: v-sharos
ms.openlocfilehash: 78f408818ee8532dfaac445200048145547c987c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-your-storage-account"></a>Použít toomanage služby StorSimple Manager hello účtu úložiště
## <a name="overview"></a>Přehled
Hello **konfigurace** stránky uvede všechny parametry hello globální služby, které lze vytvořit v hello služby StorSimple Manager. Tyto parametry mohou být použité tooall hello zařízení připojené toohello službu a zahrnují:

* Účty úložiště 
* Šablony šířky pásma 
* Záznamy řízení přístupu 

Tento kurz vysvětluje, jak je možné používat hello **konfigurace** tooadd, upravit nebo odstranit účty úložiště nebo otočit hello zabezpečení klíče pro účet úložiště.

 ![Konfigurace stránky](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Účty úložiště obsahovat hello přihlašovací údaje, které hello zařízení používá tooaccess vašeho účtu úložiště pomocí poskytovatele cloudových služeb. Pro účty úložiště Microsoft Azure jsou tyto přihlašovací údaje, jako je například název účtu hello a hello primární přístupový klíč. 

Na hello **konfigurace** stránky, všech úložiště účty, které jsou vytvořené pro hello fakturace předplatného se zobrazí ve formátu tabulky obsahující hello následující informace:

* **Název** – hello účet toohello přiřazen jedinečný název, pokud byla vytvořena.
* **Povoleno zabezpečení SSL** – jestli hello je povolen protokol SSL a zařízení cloud komunikace přes zabezpečený kanál hello.
* **Používá** – hello počtu svazků, pomocí účtu úložiště hello.

Hello běžné úkoly související s toostorage účty, které lze provést na hello **konfigurace** jsou stránky:

* Přidání účtu úložiště 
* Upravit účet úložiště 
* Odstranění účtu úložiště 
* Střídání klíče účtů úložiště 

## <a name="types-of-storage-accounts"></a>Typy účtů úložiště
Existují tři typy účtů úložiště, které lze použít s zařízení StorSimple.

* **Účty úložiště automaticky generovaný** – jak hello název naznačuje, tento typ účtu úložiště se automaticky generuje při prvním vytvoření služby hello. toolearn Další informace o tom, jak tento účet úložiště je vytvořen, najdete v části [krok 1: vytvoření nové služby](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) v [nasazení místního zařízení StorSimple](storsimple-deployment-walkthrough.md). 
* **Účty úložiště v předplatném služby hello** – Toto jsou účty hello úložiště Azure, které jsou přidruženy hello stejného předplatného jako u služby hello. toolearn Další informace o vytváření tyto účty úložiště, najdete v části [o účtech úložiště Azure](../storage/common/storage-create-storage-account.md). 
* **Účty úložiště mimo předplatné služby hello** – jedná se o hello účty úložiště Azure, které nejsou přidruženy služby, a pravděpodobně byl vytvořen před hello služba byla vytvořena.

## <a name="add-a-storage-account"></a>Přidání účtu úložiště
Můžete přidat účet úložiště poskytuje jedinečný popisný název a přístup k pověření, které jsou propojené toohello účet úložiště (s hello poskytovatele zadané cloudové služby). Máte také možnost hello povolení hello secure Sockets Layer (SSL) layer režimu toocreate zabezpečený kanál pro síťovou komunikaci mezi vaše zařízení a hello cloudu.

Můžete vytvořit více účtů pro daný cloud poskytovatele služeb. Mějte na paměti, ale, že po vytvoření účtu úložiště nemůžete změnit hello poskytovatele cloudové služby.

Při uložení hello účet úložiště služby hello pokusí toocommunicate s poskytovatele cloudových služeb. v tuto chvíli bude ověřen Hello přihlašovací údaje a materiálu hello přístupu, kterou jste zadali. Účet úložiště se vytvoří pouze v případě, že hello ověření úspěšné. Pokud hello ověřování selže, se zobrazí příslušná chybová zpráva.

Účty úložiště správce prostředků vytvořené na portálu Azure jsou podporovány také StorSimple. Hello Resource Manager účty úložiště nezobrazí v hello rozevíracího seznamu pro výběr při pokusu o toocreate kontejner svazků pouze hello úložiště, které se zobrazí účtů vytvořených v rámci hello portál Azure classic. Účty správce prostředků úložiště, bude nutné toobe přidána pomocí tooadd postup hello účet úložiště popsané dole.

> [!NOTE]
> Hello postup pro přidání účtu úložiště se liší podle verze hello StorSimple softwaru, který používáte. Být jisti toofollow hello správný postup pro vaši verzi zařízení StorSimple.
> 
> 

[!INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[!INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Upravit účet úložiště
Můžete upravit účet úložiště, který je používán kontejner svazků. Pokud chcete upravit účet úložiště, který je aktuálně používán, hello pouze pole k dispozici toomodify je hello přístupový klíč pro účet úložiště hello. Můžete zadat hello nové úložiště přístupový klíč a uložit hello aktualizovat nastavení.

#### <a name="tooedit-a-storage-account"></a>tooedit účet úložiště
1. Na cílovou stránku hello služby, vyberte svoji službu, dvakrát klikněte na název služby hello a pak klikněte na tlačítko **konfigurace**.
2. Klikněte na tlačítko **přidat či upravit účty úložiště**.
3. V hello **účty úložiště, přidat či upravit** dialogové okno:
   
   1. V rozevíracím seznamu hello **účty úložiště**, zvolte existující účet, které chcete toomodify. To může zahrnovat hello účty úložiště, které byly automaticky generovány při hello služby byl vytvořen.
   2. Pokud potřeby můžete upravit hello **povolit režim SSL** výběr.
   3. Můžete toorotate klíče pro přístup k účtu úložiště. V tématu [klíče otočení účtů úložiště](#key-rotation-of-storage-accounts) Další informace o tom, jak tooperform klíče otočení.
   4. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) toosave hello nastavení. nastavení Hello bude aktualizována na hello **konfigurace** stránky. Klikněte na tlačítko **Uložit** toosave hello nově aktualizovat nastavení.
      
      ![Upravit účet úložiště](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)

## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště
> [!IMPORTANT]
> Účet úložiště můžete odstranit pouze v případě, že není používán kontejner svazků. Pokud účet úložiště je používán kontejner svazků, nejprve odstranit kontejner svazků hello a pak odstraňte hello přidruženého účtu úložiště.
> 
> 

#### <a name="toodelete-a-storage-account"></a>toodelete účet úložiště
1. Na stránce Cílová služba hello StorSimple Manager vyberte svoji službu, dvakrát klikněte na název služby hello a pak klikněte na tlačítko **konfigurace**.
2. V hello tabulkového seznamu účtů úložiště pozastavte ukazatel myši nad hello účet chcete toodelete.
3. Ikony odstranění (**x**) se zobrazí v hello extrémně pravém sloupci pro daný účet úložiště. Klikněte na tlačítko hello **x** ikonu toodelete hello pověření.
4. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** toocontinue s hello odstranění. tabulkový výčet Hello bude aktualizované tooreflect hello změny.

## <a name="key-rotation-of-storage-accounts"></a>Střídání klíče účtů úložiště
Z bezpečnostních důvodů střídání klíčů je často požadavek v datových centrech. 

> [!NOTE]
> Následující postup střídání klíče informace a hello střídání Hello použít jenom účty úložiště Azure tooMicrosoft. Pokud používáte jiné poskytovatele cloudové služby, můžete spravovat klíče účtu úložiště prostřednictvím řídicího panelu tohoto zprostředkovatele.
> 
> 

Každé předplatné Microsoft Azure může mít jeden nebo více účtů úložiště. toothese účty pro přístup k Hello řídí hello předplatné a přístupové klíče pro každý účet úložiště. 

Při vytváření účtu úložiště vygeneruje Microsoft Azure dva 512bitové přístupové klíče k úložišti používané pro ověřování při přístupu k účtu úložiště hello. Máte dva přístupové klíče k úložišti můžete tooregenerate hello klíče bez přerušení tooyour úložiště služby a služby toothat přístup. Hello klíč, který je aktuálně používán je hello *primární* klíč a hello klíč ze zálohy je hello odkazované tooas *sekundární* klíč. Jeden z těchto dvou klíčů musí zadat, když zařízení s Microsoft Azure StorSimple přistupuje k poskytovatele cloudových služeb úložiště.

## <a name="what-is-key-rotation"></a>Co je střídání klíčů?
Obvykle se aplikace použít pouze jednu z hello klíče tooaccess vaše data. Po určité době dobu může mít vaše aplikace přejít toousing hello druhý klíč. Po Přepnuli jste sekundární klíč vaší aplikace toohello, můžete vyřadit z provozu hello první klíč a pak vygenerovat nový klíč. Pomocí dvou klíčů hello tímto způsobem umožňuje vaší aplikace přístup k toohello datům bez by docházelo k výpadkům.

klíče účtu úložiště Hello byly vždy uloženy ve hello služby v šifrovaném formátu. To však můžete resetovat prostřednictvím hello služby StorSimple Manager. Hello služby můžete získat hello primární klíč a sekundární klíč pro všechny účty úložiště ve hello stejného předplatného, včetně účtů vytvořených v rámci služby úložiště hello a také účty úložiště výchozí hello generované hello při hello služby StorSimple Manager Služba byla poprvé vytvořena. Hello služby StorSimple Manager bude vždy získat tyto klíče z hello portál Azure classic a uložit je šifrovaný způsobem.

## <a name="rotation-workflow"></a>Otočení pracovního postupu
Správce s Microsoft Azure můžete znovu vygenerovat nebo změnit primární nebo sekundární klíč hello tak, že přímý přístup k účtu úložiště hello (prostřednictvím služby Microsoft Azure Storage hello). Hello služby StorSimple Manager automaticky nezná tuto změnu.

Služba StorSimple Manager tooinform hello hello změny, budete potřebovat služby StorSimple Manager hello tooaccess, přístup k účtu úložiště hello a potom synchronizovat primární nebo sekundární klíč hello (podle toho, který byl změněn). Služba Hello pak získá hello nejnovější klíč, zašifruje hello klíče a odešle hello šifrované klíče toohello zařízení.

#### <a name="toosynchronize-keys-for-storage-accounts-in-hello-same-subscription-as-hello-service-azure-only"></a>hello toosynchronize klíče pro účty úložiště ve stejném předplatném jako služba hello (pouze Azure)
1. Na hello **služby** klikněte na tlačítko hello **konfigurace** kartě.
2. Klikněte na tlačítko **přidat či upravit účty úložiště**.
3. V dialogu hello hello následující:
   
   1. Vyberte hello účet úložiště s hello klíče, které chcete toosynchronize. klíče účtu úložiště Hello se šifrují, pokud jsou zobrazeny.
   2. Hello služby StorSimple Manager je nutné tooupdate hello klíč, který byl dříve změněn v hello služby Microsoft Azure Storage. Pokud byl změněn hello primární přístupový klíč (znova vygeneroval), klikněte na tlačítko **synchronizovat primární klíč**. Pokud byl změněn hello sekundární klíč, klikněte na tlačítko **synchronizovat sekundární klíč**.
      
      ![Synchronizovat klíče](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="toosynchronize-keys-for-storage-accounts-outside-of-hello-service-subscription"></a>toosynchronize klíčů pro účty úložiště mimo předplatné služby hello
1. Na hello **služby** klikněte na tlačítko hello **konfigurace** kartě.
2. Klikněte na tlačítko **přidat či upravit účty úložiště**.
3. V dialogu hello hello následující:
   
   1. Vyberte hello účet úložiště s hello přístupový klíč, které chcete tooupdate.
   2. Budete potřebovat tooupdate hello přístupový klíč k úložišti v hello služby StorSimple Manager. V takovém případě se zobrazí hello úložiště přístupový klíč. Zadejte nový klíč hello v hello **přístupový klíč účtu úložiště**y pole. 
   3. Uložte provedené změny. Nyní je třeba aktualizovat přístupový klíč účtu úložiště.

## <a name="next-steps"></a>Další kroky
* Další informace o [zabezpečení zařízení StorSimple](storsimple-security.md).
* Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

