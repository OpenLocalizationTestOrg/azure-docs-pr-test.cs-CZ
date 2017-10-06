---
title: "aaaManage přihlašovacích údajů účtu úložiště pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak můžete použít hello tooadd stránku konfigurace Správce zařízení StorSimple, úpravy, odstranění nebo otáčení hello zabezpečení klíčů pro přihlašovací údaje účtu úložiště přidružené k hello pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 234bf8bb-d5fe-40be-9d25-721d7482bc3b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: 22a0341eae0b89020065be4dbfaae77999f8be0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-storage-account-credentials-for-storsimple-virtual-array"></a>Pomocí Správce zařízení StorSimple toomanage přihlašovacích údajů účtu úložiště pro pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled
Hello **konfigurace** části okno služby StorSimple Manager zařízení hello pole virtuální zařízení StorSimple uvede parametry hello globální služby, které lze vytvořit v hello služby StorSimple Manager. Tyto parametry mohou být použité tooall hello zařízení připojené toohello službu a zahrnují:

* Přihlašovací údaje účtu úložiště
* Záznamy řízení přístupu
  
  ![Řídicí panel služby Správce zařízení](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccts-dashboard.png)  

Tento kurz vysvětluje, jak můžete přidat, upravit nebo odstranit přihlašovací údaje účtu úložiště pro pole virtuální zařízení StorSimple. Hello informace v tomto kurzu se vztahuje pouze na toohello pole virtuální zařízení StorSimple. Informace o tom, jak účtů úložiště toomanage řady 8000 najdete v tématu [použití hello toomanage služby StorSimple Manager účtu úložiště](storsimple-manage-storage-accounts.md).

Účet úložiště, že přihlašovací údaje obsahují hello přihlašovací údaje, které hello zařízení používá tooaccess účtu úložiště s poskytovatele cloudových služeb. Pro účty úložiště Microsoft Azure jsou tyto přihlašovací údaje, jako je například název účtu hello a hello primární přístupový klíč.

Na hello **přihlašovacích údajů účtu úložiště** okno, všechny účet úložiště přihlašovacích údajů, které jsou vytvořené pro hello fakturace předplatného se zobrazí v tabulkovém formátu obsahující hello následující informace:

* **Název** – hello účet toohello přiřazen jedinečný název, pokud byla vytvořena.
* **Povoleno zabezpečení SSL** – jestli hello je povolen protokol SSL a zařízení cloud komunikace přes zabezpečený kanál hello.
  
  ![Konfigurační oddíl](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccountcredentials-blade.png)

Hello běžné úkoly související s toostorage pověření účtu, které lze provést na hello **přihlašovacích údajů účtu úložiště** okna jsou:

* Přidání přihlašovacích údajů účtu úložiště
* Upravit přihlašovací údaje účtu úložiště
* Odstranit přihlašovací údaje účtu úložiště

## <a name="types-of-storage-account-credentials"></a>Typy přihlašovacích údajů účtu úložiště
Existují tři typy přihlašovacích údajů účtu úložiště, které lze použít s zařízení StorSimple.

* **Přihlašovací údaje účtu úložiště automaticky generovaný** – jak hello název naznačuje, tento typ přihlašovacích údajů účtu úložiště se automaticky generuje při prvním vytvoření služby hello. toolearn Další informace o vytvoření tohoto pověření pro účet úložiště, najdete v části [vytvoření nové služby](storsimple-virtual-array-manage-service.md#create-a-service).
* **přihlašovací údaje účtu úložiště v předplatném služby hello** – jedná se o účtu úložiště Azure hello přihlašovací údaje, které jsou přidruženy hello stejného předplatného jako u služby hello. toolearn Další informace o vytváření těchto přihlašovacích údajů účtu úložiště, najdete v části [o účtech úložiště Azure](../storage/common/storage-create-storage-account.md).
* **přihlašovací údaje účtu úložiště mimo předplatné služby hello** – jedná se o hello pověření účtu úložiště Azure, které nejsou přidruženy služby, a pravděpodobně byl vytvořen před hello služba byla vytvořena.

## <a name="add-a-storage-account-credential"></a>Přidání přihlašovacích údajů účtu úložiště
Konfigurace úložiště účet pověření tooyour Správce zařízení StorSimple služby můžete přidat tím, že poskytuje jedinečný popisný název a přístup k pověření, které jsou propojené toohello účet úložiště. Máte také možnost hello povolení hello secure Sockets Layer (SSL) layer režimu toocreate zabezpečený kanál pro síťovou komunikaci mezi vaše zařízení a hello cloudu.

Můžete vytvořit více účtů pro daný cloud poskytovatele služeb. Při uložení přihlašovacích údajů účtu úložiště hello hello služba pokusí o toocommunicate s poskytovatele cloudových služeb. v tuto chvíli se ověřují přihlašovací údaje Hello a materiálu hello přístupu, kterou jste zadali. Pověření účtu úložiště je vytvořen jen v případě, že hello ověření úspěšné. Pokud hello ověřování selže, se zobrazí příslušná chybová zpráva.

Použijte následující postupy přihlašovací údaje účtu úložiště Azure tooadd hello:

* hello tooadd pověření účtu úložiště, který má stejné předplatné jako služba hello Správce zařízení
* tooadd pověření účtu úložiště Azure, který je mimo předplatné služby hello Správce zařízení

#### <a name="tooadd-a-storage-account-credential-that-has-hello-same-azure-subscription-as-hello-device-manager-service"></a>hello tooadd pověření účtu úložiště, který má stejné předplatné jako služba hello Správce zařízení

1. Přejděte tooyour služby Správce zařízení, vyberte a dvakrát na ni klikněte. Tím se otevře hello **přehled** okno.
2. Vyberte **přihlašovacích údajů účtu úložiště** v rámci hello **konfigurace** části.
3. Klikněte na tlačítko **Přidat**.
4. V hello **přidání účtu úložiště** okně hello následující:
   
    1. Pro **předplatné**, vyberte **aktuální**.
    2. Zadejte název hello svého účtu úložiště Azure.
    3. Vyberte **povolit** toocreate zabezpečený kanál pro síťovou komunikaci mezi StorSimple zařízení a hello cloudu. Vyberte **zakázat** pouze v případě, že pracujete v privátním cloudu.
    4. Klikněte na tlačítko **Přidat**. Upozornění se zobrazí po úspěšném vytvoření účtu úložiště hello.<br></br>
   
        ![Přidat existující přihlašovací údaje účtu úložiště](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

#### <a name="tooadd-an-azure-storage-account-credential-that-is-outside-of-hello-device-manager-service-subscription"></a>tooadd pověření účtu úložiště Azure, který je mimo předplatné služby hello Správce zařízení

1. Přejděte tooyour služby Správce zařízení, vyberte a dvakrát na ni klikněte. Tím se otevře hello **přehled** okno.
2. Vyberte **přihlašovacích údajů účtu úložiště** v rámci hello **konfigurace** části. Rutina Vypíše seznam existující pověření účtu úložiště přidružené hello služby StorSimple Manager zařízení.
3. Klikněte na tlačítko **Přidat**.
4. V hello **přidání účtu úložiště** okně hello následující:
   
    1. Pro **předplatné**, vyberte **jiných**.
   
    2. Zadejte jméno hello svoje přihlašovací údaje účtu úložiště Azure.
   
    3. V hello **přístupový klíč účtu úložiště** textového pole zadejte hello primární přístupový klíč pro svoje přihlašovací údaje účtu úložiště Azure. tooget tomto klíče, přejděte toohello služby Azure Storage, vyberte svoje přihlašovací údaje účtu úložiště a klikněte na tlačítko **spravovat klíče účtu**. Nyní můžete zkopírovat hello primární přístupový klíč.
   
    4. tooenable SSL, klikněte na tlačítko hello **povolit** tlačítko toocreate zabezpečený kanál pro síťovou komunikaci mezi vaší cloudové služby a hello Správce zařízení StorSimple. Klikněte na tlačítko hello **zakázat** tlačítko pouze v případě, že pracujete v privátním cloudu.
   
    5. Klikněte na tlačítko **Přidat**. Upozornění se zobrazí po hello přihlašovací údaje účtu úložiště se úspěšně vytvořil.

5. nově vytvořený Hello přihlašovací údaje účtu úložiště se zobrazí v okně služby Správce konfigurace zařízení StorSimple hello pod **přihlašovacích údajů účtu úložiště**.
   
    ![Přidat přihlašovací údaje účtu úložiště mimo hello předplatné služeb Správce zařízení](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-outside-storageacct.png)

## <a name="edit-a-storage-account-credential"></a>Upravit přihlašovací údaje účtu úložiště
Můžete upravit přihlašovací údaje účtu úložiště používané zařízení. Pokud chcete upravit přihlašovací údaje účtu úložiště, který se právě používá, jsou k dispozici toomodify pole hello hello přístupový klíč a hello režimu protokolu SSL pro přihlašovací údaje účtu úložiště hello. Můžete zadat hello nový přístupový klíč úložiště nebo upravovat hello **režim povolit šifrování SSL** výběr a uložte nastavení hello aktualizovat.

#### <a name="tooedit-a-storage-account-credential"></a>tooedit pověření účtu úložiště
1. Přejděte tooyour služby Správce zařízení, vyberte a dvakrát na ni klikněte. Tím se otevře hello **přehled** okno.
2. Vyberte **přihlašovacích údajů účtu úložiště** v rámci hello **konfigurace** části. Rutina Vypíše seznam existující pověření účtu úložiště přidružené hello služby StorSimple Manager zařízení.
3. V hello tabulkového seznamu přihlašovacích údajů účtu úložiště vyberte a dvakrát klikněte na hello účtu, který má toomodify.
4. V přihlašovací údaje účtu úložiště hello **vlastnosti** okně hello následující:
   
   1. Pokud potřeby můžete upravit hello **povolit šifrování SSL** režim výběru.
   2. Můžete tooregenerate klíče pro přístup k pověření účtu úložiště. Další informace najdete v tématu [obnovit klíče účtu úložiště hello](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys). Zadejte klíč přihlašovacích údajů účtu nové úložiště hello. Pro účet úložiště Azure je hello primární přístupový klíč.
   3. Klikněte na tlačítko **Uložit** hello horní části hello **vlastnosti** okno toosave hello nastavení. nastavení Hello jsou aktualizovány na hello **přihlašovacích údajů účtu úložiště** okno.
      
      ![Upravit přihlašovací údaje účtu úložiště](./media/storsimple-virtual-array-manage-storage-accounts/ova-edit-storageacct.png)

## <a name="delete-a-storage-account-credential"></a>Odstranit přihlašovací údaje účtu úložiště
> [!IMPORTANT]
> Pověření účtu úložiště můžete odstranit pouze v případě, že není používán. Pokud pověření účtu úložiště se používá, budete upozorněni.
> 
> 

#### <a name="toodelete-a-storage-account-credential"></a>toodelete pověření účtu úložiště
1. Přejděte tooyour služby Správce zařízení, vyberte a dvakrát na ni klikněte. Tím se otevře hello **přehled** okno.
2. Vyberte **přihlašovacích údajů účtu úložiště** v rámci hello **konfigurace** části. Rutina Vypíše seznam existující pověření účtu úložiště přidružené hello služby StorSimple Manager zařízení.
3. V hello tabulkového seznamu přihlašovacích údajů účtu úložiště vyberte a dvakrát klikněte na hello účtu, který má toodelete.
4. V přihlašovací údaje účtu úložiště hello **vlastnosti** okně hello následující:
   
   1. Klikněte na tlačítko **odstranit** toodelete hello pověření.
   2. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** toocontinue s hello odstranění. tabulkový výčet Hello je aktualizovaný tooreflect hello změny.
      
      ![Odstranit přihlašovací údaje účtu úložiště](./media/storsimple-virtual-array-manage-storage-accounts/ova-del-storageacct.png)

## <a name="synchronizing-storage-account-credential-keys"></a>Synchronizace přihlašovacích údajů klíče účtu úložiště
Z bezpečnostních důvodů střídání klíčů je často požadavek v datových centrech. Microsoft Azure správce můžete znovu vygenerovat nebo změnit primární nebo sekundární klíč hello tak, že přímý přístup k pověřením účtu úložiště hello (prostřednictvím služby Microsoft Azure Storage hello). Hello služby StorSimple Manager zařízení automaticky nezná tuto změnu.

tooinform služby StorSimple Manager zařízení hello hello změny, je nutné službu StorSimple Manager zařízení hello tooaccess, přístup k úložišti hello účet přihlašovací údaje a potom synchronizovat primární nebo sekundární klíč hello (podle toho, který byl změněn). Služba Hello pak získá hello nejnovější klíč, zašifruje hello klíče a odešle hello šifrované klíče toohello zařízení.

#### <a name="toosynchronize-keys-for-storage-account-credentials-in-hello-same-subscription-as-hello-service-azure-only"></a>hello toosynchronize klíče pro přihlašovací údaje účtu úložiště ve stejném předplatném jako služba hello (pouze Azure)
1. V okně Cílová služba hello, vyberte svoji službu, klikněte dvakrát na hello název služby a potom v hello **konfigurace** klikněte na tlačítko **přihlašovacích údajů účtu úložiště**.
2. Na hello **přihlašovacích údajů účtu úložiště** okno, v hello seznam přihlašovacích údajů účtu úložiště, vyberte přihlašovací údaje účtu úložiště hello jejíž klíče, které chcete toosynchronize.
3. V hello **vlastnosti** okně hello vybraná přihlašovací údaje účtu úložiště, hello následující:
   
    1. Klikněte na tlačítko **Další**a potom klikněte na **synchronizace přístupový klíč**.
   
    2. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **klíč synchronizace** toocomplete hello synchronizace.
    
4. Hello služby StorSimple Manager zařízení je nutné tooupdate hello klíč, který byl dříve změněn v hello služby Microsoft Azure Storage. V hello **klíč účtu úložiště synchronizovat** okno, pokud byl změněn hello primární přístupový klíč (znova vygeneroval), klikněte na primární a pak klikněte na **klíč synchronizace**. Pokud byl změněn hello sekundární klíč, klikněte na tlačítko **sekundární**a potom klikněte na **klíč synchronizace**.
   
    ![Synchronizace přístupového klíče](./media/storsimple-virtual-array-manage-storage-accounts/ova-sync-acess-key.png)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[spravovat vaše pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).

