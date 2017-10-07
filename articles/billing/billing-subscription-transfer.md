---
title: "aaaTransfer účet tooanother vlastnictví předplatného Azure | Microsoft Docs"
description: "Popisuje, jak tootransfer uživatelé tooanother předplatného Azure a některé nejčastější dotazy (FAQ) o procesu hello"
keywords: "přenos předplatného přenos předplatného azure, azure, přesuňte tooanother účtu předplatného azure, vlastník předplatného azure změnu, účtu tooanother předplatného azure přenosu"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: c8ecdc1e-c9c5-468c-a024-94ae41e64702
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2d9f5d9ccd34738701493e5f31c2f83a02f5d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-ownership-of-an-azure-subscription-tooanother-account"></a>Převést vlastnictví tooanother účtu předplatného Azure

Můžete přenést předplatné tooanother uživatelů pro odběry průběžné platby, Visual Studio, sada akce nebo BizSpark ve hello centra účtů. Podporujeme hello přenos externí služby Azure pro tyto předplatné typy. 

Můžete chtít tootransfer vlastnictví předplatného Azure, pokud je:

* Potřebují toohand přes vlastnictví fakturace vašeho předplatného Azure toosomeone else.
* Chcete účet hello toochange používat toosign pro službu Azure. Možná používá Account Microsoft ale určená toouse pracovní nebo školní účet místo.
* Chcete toomove vašeho předplatného Azure z tooanother jeden adresář.
* V jiných klientů Azure a Office 365 a chcete tooconsolidate.

toochange nabízí vaše předplatné tooa jiný, najdete v části [přepínač vaši nabídku tooanother předplatné](billing-how-to-switch-azure-offer.md). 

## <a name="transfer-ownership-of-an-azure-subscription"></a>Přenos vlastnictví předplatného Azure
> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]
>
>

1. Přihlaste se na <https://account.windowsazure.com/Subscriptions>. Máte toobe hello správce účtu tooperform převod vlastnictví. toofind, který je hello správce účtu předplatného hello, najdete v části hello [nejčastější dotazy](#faq).

2. Vyberte předplatné tootransfer hello.

3. Klikněte na tlačítko hello **přenos předplatného** možnost. V tématu [– nejčastější dotazy](#no-button) Pokud se nezobrazí tlačítko hello.

   ![Karta odběry účet Azure](./media/billing-subscription-transfer/image1.png)
4. Zadejte příjemce hello.

   ![Dialogové okno přenos předplatného](./media/billing-subscription-transfer/image2.PNG)
5. příjemce Hello automaticky získá e-mail s odkazem přijetí.

   ![Předplatné přenosu e-mailu toorecipient](./media/billing-subscription-transfer/image3.png)
6. příjemce Hello klikne na odkaz hello a postupuje hello pokynů, včetně zadáním jejich platebních údajů.

   ![První předplatné přenos webové stránky](./media/billing-subscription-transfer/image4.png)

   ![Druhý předplatné přenos webové stránky](./media/billing-subscription-transfer/image5.png)
7. Úspěšné! Nyní se přenáší Hello předplatného.

## <a name="transfer-subscription-ownership-for-enterprise-agreement-ea-customers"></a>Přenos vlastnictví předplatného pro zákazníky Enterprise Agreement (EA)
Hello podnikového správce může převést vlastnictví předplatných v rámci zápisu. tooget začít, najdete v části [přenos vlastnictví účet](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) hello EA portálu.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Další kroky následující po přijetí vlastnictví předplatného
1. Nyní jste hello správce účtu. Zkontrolovat a aktualizovat hello Správce služeb a Spolusprávci. Správa správců v hello [portál Azure classic](https://manage.windowsazure.com) přechodem tooSettings. [Další informace o rolích správce](billing-add-change-azure-subscription-administrator.md).

2. Řízení přístupu na základě role (RBAC) můžete také použít pro vaše předplatné a služby. Navštivte hello [portál Azure](https://portal.azure.com). [Další informace o RBAC](../active-directory/role-based-access-control-configure.md)

3. Aktualizujte přihlašovací údaje související s toto předplatné služby, včetně:
   
   * Certifikáty pro správu, která udělují oprávnění správce uživatele hello toosubscription prostředky. Další informace najdete v tématu [vytvoření a nahrání certifikátu správy pro Azure.](../cloud-services/cloud-services-certs-create.md)
   
   * Přístupové klávesy pro služby jako úložiště. Další informace najdete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md)
   
   * Pověření vzdáleného přístupu pro služby, jako virtuální počítače Azure. 

4. [Aktualizovat fakturace výstrahy pro toto předplatné](billing-set-up-alerts.md) v hello [centra účtů Azure](https://account.windowsazure.com/Subscriptions). 

5. Pokud pracujete s partnerem, zvažte aktualizaci ID partnera hello u tohoto předplatného. Můžete aktualizovat ID partnera hello v hello [centra účtů Azure](https://account.windowsazure.com/Subscriptions).

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Nejčastější dotazy
* <a name="whoisaa"></a>**Kdo je správce účtu hello hello předplatného?**

  Správce účtu Hello je hello osobě, která registraci aplikace nebo koupili hello předplatného Azure. Jsou oprávnění tooaccess hello [centra účtů](https://account.windowsazure.com/Home/Index) a provádět různé úlohy správy, jako jsou vytvářet odběry, zrušit předplatné, změnit hello fakturace pro předplatné nebo změnit hello Správce služeb. Pokud si nejste jistí, který správce účtu hello je pro předplatné, použijte následující kroky toofind out hello.

  1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
  2. V nabídce centra hello vyberte **předplatné**.
  3. Vyberte předplatné hello chcete toocheck a pak hledejte v části **nastavení**.
  4. Vyberte **vlastnosti**. Správce účtu Hello hello předplatného se zobrazí v hello **správce účtu** pole.  

* **Všechno přenosu? Včetně skupin prostředků, virtuálních počítačů, disků a další spuštěné služby?**

  Ano, všechny prostředky, například virtuálních počítačů, disků, a weby přenosu toohello nového vlastníka. Však žádné [role správce](billing-add-change-azure-subscription-administrator.md) a [řízení přístupu na základě rolí (RBAC)](../active-directory/role-based-access-control-configure.md) zásady, které jste nastavili nepřenášejí mezi různé adresáře.

* <a id="no-button"></a>**Proč nevidím hello přenos předplatného tlačítko?**

  Pokud nevidíte hello **přenos předplatného** tlačítko, pak přenos předplatného se nepodporuje pro vaši nabídku. [Obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

* **Přenos předplatného důsledkem výpadek služeb?**

  Není žádná služba toohello dopad. Přenos předplatného hello zruší hello předplatného pod hello aktuálního účtu správce a vytvoří odběr pod účtem hello příjemce. nové předplatné Hello je přidružené toohello základní služby Azure. zůstanou ID předplatného Hello hello stejné.

* **Jak používat tento proces toochange hello adresář pro předplatné?**

  Předplatné Azure je vytvořen v adresáři hello této hello, ke kterému patří účet správce. toochange hello adresář, přenos hello předplatné tooa uživatelský účet v hello cílový adresář. Po dokončení tohoto uživatele hello kroky tooaccept přenos předplatného hello je automaticky přesunutý toohello cílový adresář.

* **Pokud převzít vlastnictví fakturace předplatného z jiné organizace, že k pokračovat toohave přístup toomy prostředky?**

  Pokud je předplatné hello přenášená tooanother klienta, uživatelé hello přidružený předchozí klientovi hello ztratit přístup toohello předplatné. I když uživatel není správce služby nebo spolusprávce už, mohou stále mít předplatné toohello přístup prostřednictvím jiné mechanismy zabezpečení, včetně:

  * Certifikáty pro správu, která udělují oprávnění správce uživatele hello toosubscription prostředky. Další informace najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure](../cloud-services/cloud-services-certs-create.md).
  * Přístupové klávesy pro služby jako úložiště. Další informace najdete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).
  * Pověření vzdáleného přístupu pro služby, jako virtuální počítače Azure.

 Pokud příjemce hello potřebuje toorestrict přístup k tootheir prostředkům, se zvažte aktualizaci všech tajných klíčů související se službou hello. Většina prostředků můžete aktualizovat pomocí hello následující kroky:

    1. Přejděte toohello [portál Azure](https://portal.azure.com).
    2. V nabídce centra hello vyberte **všechny prostředky**.
    3. Vyberte prostředek hello. 
    4. V okně prostředků hello, klikněte na tlačítko **nastavení**. Zde si můžete zobrazit a aktualizovat existující tajných klíčů.

* **Pokud přenést předplatné hello uprostřed hello hello fakturační cyklus, hello příjemce platím pro fakturaci hello celého cyklu?**

  Hello sender je odpovědná za platebních pro jakékoli použití, která byla nahlášena až toohello bodu ukončení přenosu hello je. příjemce Hello je zodpovědná za využití nahlásila hello čas přenosu a vyšší. Může mít některé využití, která byla provedena před přenosu ale ohlásil později. využití Hello je součástí hello příjemce faktury.

* **Má hello příjemce toousage přístup a fakturace historie?**

  Hello pouze informace k dispozici toohello příjemce je hello množství hello poslední faktury nebo pokud hello předplatné bylo přeneseno před první faktury hello byla vygenerována, hello aktuální vyrovnávání. zbytek Hello hello využití a historie fakturace k přenosu s předplatným hello.

* **Lze hello nabídka změnit během přenosu?**

  Hello nabídka musí zůstat stejné hello. toochange vaši nabídku, najdete v části [přepínač vaši nabídku tooanother předplatné](billing-how-to-switch-azure-offer.md).

* **Můžete převést předplatné tooa uživatelský účet v jiné zemi?**

  Ne, přenos předplatného tooa uživatelský účet v jiné zemi není podporován. příjemce Hello uživatelský účet musí být v hello stejné země.

* **Hello příjemce, můžete použít jiný způsob platby?**

  Ano. Ale historie fakturace předplatného hello je rozdělit mezi dva účty.  

* **Je způsob platby hello dopad po přesunu předplatné Azure?**

  tooaccept přenos předplatného, platební karty, podobně jako způsob platby musí se zadat nebo toopay pro předplatné hello. Například pokud Bob přenáší tooJane předplatného a Jana přijímá přenos hello, zadejte Jana platebních metoda toopay pro předplatné hello. Po dokončení přenosu hello Jana se fakturuje hello předplatného není Bob.

* **Jak migraci dat a služeb pro Moje předplatné toonew předplatné Azure?**

  Pokud nelze převést vlastnictví předplatného, můžete migrovat ručně vašich prostředků. V tématu [přesunout skupiny prostředků toonew prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md).



## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém. 


