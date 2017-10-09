---
title: "aaaManage vašeho účtu úložiště, StorSimple přihlašovací údaje pro řadu zařízení Microsoft Azure StorSimple 8000 | Microsoft Docs"
description: "Vysvětluje, jak můžete použít hello tooadd stránku konfigurace Správce zařízení StorSimple, úpravy, odstranění nebo otáčení hello zabezpečení klíče pro účet úložiště."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 132ee46509b39db4d1b97b0f1077800a253e8da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-your-storage-account-credentials"></a>Použít toomanage služby StorSimple Manager zařízení hello přihlašovací údaje účtu úložiště

## <a name="overview"></a>Přehled

Hello **konfigurace** část v okně service Manager zařízení StorSimple hello představuje všechny parametry hello globální služby, které lze vytvořit v hello služby StorSimple Manager zařízení. Tyto parametry mohou být použité tooall hello zařízení připojené toohello službu a zahrnují:

* Přihlašovací údaje účtu úložiště
* Šablony šířky pásma 
* Záznamy řízení přístupu 

Tento kurz vysvětluje, jak tooadd, upravit, nebo odstranit přihlašovací údaje pro účet úložiště nebo otočit hello zabezpečení klíče pro účet úložiště.

 ![Seznam přihlašovacích údajů účtu úložiště](./media/storsimple-8000-manage-storage-accounts/createnewstorageacct6.png)  

Účty úložiště obsahovat hello přihlašovací údaje, které hello tooaccess používá zařízení StorSimple vašeho účtu úložiště pomocí poskytovatele cloudových služeb. Pro účty úložiště Microsoft Azure jsou tyto přihlašovací údaje, jako je například název účtu hello a hello primární přístupový klíč. 

Na hello **přihlašovacích údajů účtu úložiště** okno, všechny úložiště účty, které jsou vytvořené pro hello fakturace předplatného se zobrazí ve formátu tabulky obsahující hello následující informace:

* **Název** – hello účet toohello přiřazen jedinečný název, pokud byla vytvořena.
* **Povoleno zabezpečení SSL** – jestli hello je povolen protokol SSL a zařízení cloud komunikace přes zabezpečený kanál hello.
* **Používá** – hello počtu svazků, pomocí účtu úložiště hello.

Hello nejběžnější úlohy související toostorage účty, které lze provést jsou:

* Přidání účtu úložiště 
* Upravit účet úložiště 
* Odstranění účtu úložiště 
* Střídání klíče účtů úložiště 

## <a name="types-of-storage-accounts"></a>Typy účtů úložiště

Existují tři typy účtů úložiště, které lze použít s zařízení StorSimple.

* **Účty úložiště automaticky generovaný** – jak hello název naznačuje, tento typ účtu úložiště se automaticky generuje při prvním vytvoření služby hello. toolearn Další informace o tom, jak tento účet úložiště je vytvořen, najdete v části [krok 1: vytvoření nové služby](storsimple-8000-deployment-walkthrough-u2.md#step-1-create-a-new-service) v [nasazení místního zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md). 
* **Účty úložiště v předplatném služby hello** – Toto jsou účty hello úložiště Azure, které jsou přidruženy hello stejného předplatného jako u služby hello. toolearn Další informace o vytváření tyto účty úložiště, najdete v části [o účtech úložiště Azure](../storage/common/storage-create-storage-account.md). 
* **Účty úložiště mimo předplatné služby hello** – jedná se o hello účty úložiště Azure, které nejsou přidruženy služby, a pravděpodobně byl vytvořen před hello služba byla vytvořena.

## <a name="add-a-storage-account"></a>Přidání účtu úložiště

Můžete přidat účet úložiště poskytuje jedinečný popisný název a přístup k pověření, které jsou propojené toohello účet úložiště (s hello poskytovatele zadané cloudové služby). Máte také možnost hello povolení hello secure Sockets Layer (SSL) layer režimu toocreate zabezpečený kanál pro síťovou komunikaci mezi vaše zařízení a hello cloudu.

Můžete vytvořit více účtů pro daný cloud poskytovatele služeb. Mějte na paměti, ale, že po vytvoření účtu úložiště nemůžete změnit hello poskytovatele cloudové služby.

Při uložení hello účet úložiště služby hello pokusí toocommunicate s poskytovatele cloudových služeb. v tuto chvíli bude ověřen Hello přihlašovací údaje a materiálu hello přístupu, kterou jste zadali. Účet úložiště se vytvoří pouze v případě, že hello ověření úspěšné. Pokud hello ověřování selže, se zobrazí příslušná chybová zpráva.

Použijte následující postupy přihlašovací údaje účtu úložiště Azure tooadd hello:

* hello tooadd pověření účtu úložiště, který má stejné předplatné jako služba hello Správce zařízení
* tooadd pověření účtu úložiště Azure, který je mimo předplatné služby hello Správce zařízení

[!INCLUDE [add-a-storage-account-update2](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

#### <a name="tooadd-an-azure-storage-account-credential-outside-of-hello-storsimple-device-manager-service-subscription"></a>tooadd pověření účtu úložiště Azure mimo předplatné služby StorSimple Manager zařízení hello

1. Přejděte tooyour služby StorSimple Manager zařízení, vyberte a dvakrát na ni klikněte. Tím se otevře hello **přehled** okno.
2. Vyberte **přihlašovacích údajů účtu úložiště** v rámci hello **konfigurace** části. Rutina Vypíše seznam existující pověření účtu úložiště přidružené hello služby StorSimple Manager zařízení.
3. Klikněte na tlačítko **Přidat**.
4. V hello **přidat přihlašovací údaje účtu úložiště** okně hello následující:
   
    1. Pro **předplatné**, vyberte **jiných**.
   
    2. Zadejte jméno hello svoje přihlašovací údaje účtu úložiště Azure.
   
    3. V hello **přístupový klíč účtu úložiště** textového pole zadejte hello primární přístupový klíč pro svoje přihlašovací údaje účtu úložiště Azure. tooget tomto klíče, přejděte toohello služby Azure Storage, vyberte svoje přihlašovací údaje účtu úložiště a klikněte na tlačítko **spravovat klíče účtu**. Nyní můžete zkopírovat hello primární přístupový klíč.
   
    4. tooenable SSL, klikněte na tlačítko hello **povolit** tlačítko toocreate zabezpečený kanál pro síťovou komunikaci mezi vaší cloudové služby a hello Správce zařízení StorSimple. Klikněte na tlačítko hello **zakázat** tlačítko pouze v případě, že pracujete v privátním cloudu.
   
    5. Klikněte na tlačítko **Přidat**. Upozornění se zobrazí po hello přihlašovací údaje účtu úložiště se úspěšně vytvořil.

5. nově vytvořený Hello přihlašovací údaje účtu úložiště se zobrazí v okně služby Správce konfigurace zařízení StorSimple hello pod **přihlašovacích údajů účtu úložiště**.
   


## <a name="edit-a-storage-account"></a>Upravit účet úložiště

Můžete upravit účet úložiště, který je používán kontejner svazků. Pokud chcete upravit účet úložiště, který je aktuálně používán, hello pouze pole k dispozici toomodify je hello přístupový klíč pro účet úložiště hello. Můžete zadat hello nové úložiště přístupový klíč a uložit hello aktualizovat nastavení.

#### <a name="tooedit-a-storage-account"></a>tooedit účet úložiště

1. Přejděte služby StorSimple Manager zařízení tooyour. V hello **konfigurace** klikněte na tlačítko **přihlašovacích údajů účtu úložiště**.

    ![Přihlašovací údaje účtu úložiště](./media/storsimple-8000-manage-storage-accounts/editstorageacct1.png)

2. V hello **přihlašovacích údajů účtu úložiště** , ze seznamu hello přihlašovacích údajů účtu úložiště, vyberte a klikněte na jednu chcete tooedit hello. 

3. Můžete upravit hello **povolit šifrování SSL** výběr. Můžete také kliknout na **více...**  a pak vyberte **synchronizace přístup klíče toorotate** klíče pro přístup k účtu úložiště. Přejděte příliš[klíče otočení účtů úložiště](#key-rotation-of-storage-accounts) Další informace o tom, jak tooperform klíče otočení. Po změně nastavení hello kliknutím **Uložit**. 

    ![Uložte upravený úložiště pověření](./media/storsimple-8000-manage-storage-accounts/editstorageacct3.png)

4. Po zobrazení výzvy k potvrzení klikněte na **Ano**. 

    ![Potvrďte změny](./media/storsimple-8000-manage-storage-accounts/editstorageacct4.png)

nastavení Hello bude aktualizován a uložit pro váš účet úložiště. 

## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště

> [!IMPORTANT]
> Účet úložiště můžete odstranit pouze v případě, že není používán kontejner svazků. Pokud účet úložiště je používán kontejner svazků, nejprve odstranit kontejner svazků hello a pak odstraňte hello přidruženého účtu úložiště.

#### <a name="toodelete-a-storage-account"></a>toodelete účet úložiště

1. Přejděte služby StorSimple Manager zařízení tooyour. V hello **konfigurace** klikněte na tlačítko **přihlašovacích údajů účtu úložiště**.

2. V hello tabulkového seznamu účtů úložiště pozastavte ukazatel myši nad hello účet chcete toodelete. Klikněte pravým tlačítkem na tooinvoke hello kontextovou nabídku a klikněte na tlačítko **odstranit**.

    ![Odstranit přihlašovací údaje pro účet úložiště](./media/storsimple-8000-manage-storage-accounts/deletestorageacct1.png)

3. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** toocontinue s hello odstranění. tabulkový výčet Hello bude aktualizované tooreflect hello změny.

    ![Potvrzení odstranění](./media/storsimple-8000-manage-storage-accounts/deletestorageacct2.png)

## <a name="key-rotation-of-storage-accounts"></a>Střídání klíče účtů úložiště

Z bezpečnostních důvodů střídání klíčů je často požadavek v datových centrech. Každé předplatné Microsoft Azure může mít jeden nebo více účtů úložiště. toothese účty pro přístup k Hello řídí hello předplatné a přístupové klíče pro každý účet úložiště. 

Při vytváření účtu úložiště vygeneruje Microsoft Azure dva 512bitové přístupové klíče k úložišti používané pro ověřování při přístupu k účtu úložiště hello. Máte dva přístupové klíče k úložišti můžete tooregenerate hello klíče bez přerušení tooyour úložiště služby a služby toothat přístup. Hello klíč, který je aktuálně používán je hello *primární* klíč a hello klíč ze zálohy je hello odkazované tooas *sekundární* klíč. Jeden z těchto dvou klíčů musí zadat, když zařízení s Microsoft Azure StorSimple přistupuje k poskytovatele cloudových služeb úložiště.

## <a name="what-is-key-rotation"></a>Co je střídání klíčů?

Obvykle se aplikace použít pouze jednu z hello klíče tooaccess vaše data. Po určité době dobu může mít vaše aplikace přejít toousing hello druhý klíč. Po Přepnuli jste sekundární klíč vaší aplikace toohello, můžete vyřadit z provozu hello první klíč a pak vygenerovat nový klíč. Pomocí dvou klíčů hello tímto způsobem umožňuje vaší aplikace přístup k toohello datům bez by docházelo k výpadkům.

klíče účtu úložiště Hello byly vždy uloženy ve hello služby v šifrovaném formátu. To však můžete resetovat prostřednictvím hello služby StorSimple Manager zařízení. Hello služby můžete získat hello primární klíč a sekundární klíč pro všechny účty úložiště ve hello stejného předplatného, včetně účtů vytvořených v rámci služby úložiště hello a také účty úložiště výchozí hello generované hello při hello Správce zařízení StorSimple Služba byla nejprve vytvořit. Hello služby StorSimple Manager zařízení bude vždy získat tyto klíče z hello portál Azure classic a uložit je šifrovaný způsobem.

## <a name="rotation-workflow"></a>Otočení pracovního postupu

Správce s Microsoft Azure můžete znovu vygenerovat nebo změnit primární nebo sekundární klíč hello tak, že přímý přístup k účtu úložiště hello (prostřednictvím služby Microsoft Azure Storage hello). Hello služby StorSimple Manager zařízení automaticky nezná tuto změnu.

tooinform služby StorSimple Manager zařízení hello hello změny, budete potřebovat službu StorSimple Manager zařízení hello tooaccess, přístup k účtu úložiště hello a potom synchronizovat primární nebo sekundární klíč hello (podle toho, který byl změněn). Služba Hello pak získá hello nejnovější klíč, zašifruje hello klíče a odešle hello šifrované klíče toohello zařízení.

#### <a name="toosynchronize-keys-for-storage-accounts-in-hello-same-subscription-as-hello-service"></a>hello toosynchronize klíče pro účty úložiště ve stejném předplatném jako služba hello 
1. Přejděte služby StorSimple Manager zařízení tooyour. V hello **konfigurace** klikněte na tlačítko **přihlašovacích údajů účtu úložiště**.
2. Z hello tabulkové seznam účtů úložiště, klikněte na tlačítko hello jeden, které chcete toomodify. 

    ![Synchronizovat klíče](./media/storsimple-8000-manage-storage-accounts/syncaccesskey1.png)

3. Klikněte na tlačítko **... Další** a pak vyberte **synchronizace přístup klíče toorotate**.   

    ![Synchronizovat klíče](./media/storsimple-8000-manage-storage-accounts/syncaccesskey2.png)

4. Hello služby StorSimple Manager zařízení je nutné tooupdate hello klíč, který byl dříve změněn v hello služby Microsoft Azure Storage. Pokud byl změněn hello primární přístupový klíč (znova vygeneroval), vyberte **primární** klíč. Pokud byl změněn hello sekundární klíč, vyberte **sekundární** klíč. Klikněte na tlačítko **klíč synchronizace**.
      
      ![Synchronizovat klíče](./media/storsimple-8000-manage-storage-accounts/syncaccesskey3.png)

Po úspěšně sycnhronized hello klíč, budete upozorněni.

#### <a name="toosynchronize-keys-for-storage-accounts-outside-of-hello-service-subscription"></a>toosynchronize klíčů pro účty úložiště mimo předplatné služby hello
1. Na hello **služby** klikněte na tlačítko hello **konfigurace** kartě.
2. Klikněte na tlačítko **přidat či upravit účty úložiště**.
3. V dialogu hello hello následující:
   
   1. Vyberte hello účet úložiště s hello přístupový klíč, které chcete tooupdate.
   2. Budete potřebovat tooupdate hello přístupový klíč k úložišti v hello služby StorSimple Manager zařízení. V takovém případě se zobrazí hello úložiště přístupový klíč. Zadejte nový klíč hello v hello **přístupový klíč účtu úložiště** pole. 
   3. Uložte provedené změny. Nyní je třeba aktualizovat přístupový klíč účtu úložiště.

## <a name="next-steps"></a>Další kroky
* Další informace o [zabezpečení zařízení StorSimple](storsimple-8000-security.md).
* Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

