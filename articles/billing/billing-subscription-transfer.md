---
title: "Přenos vlastnictví předplatného Azure na jiný účet | Microsoft Docs"
description: "Popisuje, jak převést předplatné Azure do jiného uživatele a některé nejčastější dotazy (FAQ) o procesu"
keywords: "přenos předplatného přenos předplatného azure, azure, přesunout do jiné vlastník předplatného změnu účtu azure předplatné azure, přenos předplatného azure na jiný účet"
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
ms.openlocfilehash: 8a856c39eb11546f35cb4e8c21e6bdcce98857a8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="transfer-ownership-of-an-azure-subscription-to-another-account"></a>Přenos vlastnictví předplatného služby Azure na jiný účet

Vaše předplatné můžou přenášet do jiného uživatele průběžné platby, Visual Studio, akce sady nebo BizSpark odběry v centru účtů. Podporujeme přenos externí služby Azure pro tyto předplatné typy. 

Můžete chtít přenos vlastnictví předplatného Azure, když jste:

* Třeba k ruční přes fakturace vlastnictví předplatného Azure někomu jinému.
* Chcete změnit účet použitý k registraci do Azure. Možná používá Account Microsoft ale určená použijte pracovní nebo školní účet.
* Chcete přesunout do jiného předplatného Azure z jednoho adresáře.
* V jiných klientů Azure a Office 365 a konsolidovat.

Chcete-li změnit předplatné na jinou nabídku, [vašeho předplatného Azure přepnout na jinou nabídku](billing-how-to-switch-azure-offer.md). 

## <a name="transfer-ownership-of-an-azure-subscription"></a>Přenos vlastnictví předplatného Azure
> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]
>
>

1. Přihlaste se na <https://account.windowsazure.com/Subscriptions>. Musíte být správce účtu provést převod vlastnictví. Chcete-li zjistit, kdo je správce účtu předplatného, přečtěte si téma [nejčastější dotazy](#faq).

2. Vyberte předplatné pro přenos.

3. Klikněte **přenos předplatného** možnost. V tématu [– nejčastější dotazy](#no-button) Pokud se nezobrazí tlačítko.

   ![Karta odběry účet Azure](./media/billing-subscription-transfer/image1.png)
4. Zadejte příjemce.

   ![Dialogové okno přenos předplatného](./media/billing-subscription-transfer/image2.PNG)
5. Příjemce automaticky dostane e-mail s odkazem pro akceptaci.

   ![Předplatné přenosu e-mailu k příjemce](./media/billing-subscription-transfer/image3.png)
6. Příjemce klikne na odkaz a postupuje podle pokynů, včetně zadání platebních údajů.

   ![První předplatné přenos webové stránky](./media/billing-subscription-transfer/image4.png)

   ![Druhý předplatné přenos webové stránky](./media/billing-subscription-transfer/image5.png)
7. Úspěšné! Předplatné je nyní přenést.

## <a name="transfer-subscription-ownership-for-enterprise-agreement-ea-customers"></a>Přenos vlastnictví předplatného pro zákazníky Enterprise Agreement (EA)
Správce podnikové sítě může převést vlastnictví předplatných v rámci zápisu. Abyste mohli začít, najdete v části [přenos vlastnictví účet](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) EA portálu.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Další kroky následující po přijetí vlastnictví předplatného
1. Nyní jste správce účtu. Zkontrolovat a aktualizovat Správce služeb a Spolusprávci. Spravovat správce ve [portál Azure classic](https://manage.windowsazure.com) přechodem na nastavení. [Další informace o rolích správce](billing-add-change-azure-subscription-administrator.md).

2. Řízení přístupu na základě role (RBAC) můžete také použít pro vaše předplatné a služby. Navštivte [Azure Portal](https://portal.azure.com). [Další informace o RBAC](../active-directory/role-based-access-control-configure.md)

3. Aktualizujte přihlašovací údaje související s toto předplatné služby, včetně:
   
   * Certifikáty pro správu, které udělit oprávnění správce pro prostředky předplatného. Další informace najdete v tématu [vytvoření a nahrání certifikátu správy pro Azure.](../cloud-services/cloud-services-certs-create.md)
   
   * Přístupové klávesy pro služby jako úložiště. Další informace najdete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md)
   
   * Pověření vzdáleného přístupu pro služby, jako virtuální počítače Azure. 

4. [Aktualizovat fakturace výstrahy pro toto předplatné](billing-set-up-alerts.md) na [centra účtů Azure](https://account.windowsazure.com/Subscriptions). 

5. Pokud pracujete s partnerem, zvažte aktualizaci ID partnera u tohoto předplatného. Můžete aktualizovat ID partnera v [centra účtů Azure](https://account.windowsazure.com/Subscriptions).

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Nejčastější dotazy
* <a name="whoisaa"></a>**Kdo je správce účtu předplatného?**

  Správce účtu je osoba, která registraci aplikace nebo kód zakoupili předplatné Azure. Mají autorizaci pro přístup [centra účtů](https://account.windowsazure.com/Home/Index) a provádět různé úlohy správy, jako jsou vytvářet odběry, zrušit předplatné, změnit fakturace pro předplatné nebo změnit správce služeb. Pokud si nejste jistí, který je účet správce pro předplatné, použijte následující kroky a zjistěte.

  1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
  2. V nabídce centra vyberte **předplatné**.
  3. Vyberte předplatné, které chcete kontrolovat, a pak hledejte v části **nastavení**.
  4. Vyberte **vlastnosti**. Správce účtu předplatného se zobrazí v **správce účtu** pole.  

* **Všechno přenosu? Včetně skupin prostředků, virtuálních počítačů, disků a další spuštěné služby?**

  Ano, všechny prostředky, například virtuálních počítačů, disků a weby přenos do nového vlastníka. Však žádné [role správce](billing-add-change-azure-subscription-administrator.md) a [řízení přístupu na základě rolí (RBAC)](../active-directory/role-based-access-control-configure.md) zásady, které jste nastavili nepřenášejí mezi různé adresáře.

* <a id="no-button"></a>**Proč nezobrazí tlačítko přenos předplatného?**

  Pokud nevidíte **přenos předplatného** tlačítko, pak přenos předplatného se nepodporuje pro vaši nabídku. [Obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

* **Přenos předplatného důsledkem výpadek služeb?**

  Neexistuje žádný vliv na službu. Přenos předplatného zruší předplatného v rámci aktuálního účtu správce a vytvoří odběr pod účtem příjemce. Nový odběr je přidružena k základní služby Azure. ID předplatného se nezmění.

* **Jak používat tento proces a změnit adresář pro předplatné?**

  Předplatné Azure je vytvořen v adresáři, který je součástí správce účtu. Chcete-li změnit adresář, přeneste předplatné do účet uživatele v cílovém adresáři. Po dokončení tohoto uživatele kroky tak, aby přijímal přenos předplatného je automaticky přesunuta do cílový adresář.

* **Pokud převzít vlastnictví fakturace předplatného z jiné organizace, se nadále přístup k mé prostředkům?**

  Pokud předplatné se přenese do jiného klienta, uživatelé, přidružený k předchozí klientovi ztratit přístup k předplatnému. I když uživatel není správce služby nebo spolusprávce už, mohou mít stále přístup k předplatnému pomocí jiné mechanismy zabezpečení, včetně:

  * Certifikáty pro správu, které udělit oprávnění správce pro prostředky předplatného. Další informace najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure](../cloud-services/cloud-services-certs-create.md).
  * Přístupové klávesy pro služby jako úložiště. Další informace najdete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).
  * Pověření vzdáleného přístupu pro služby, jako virtuální počítače Azure.

 Pokud příjemce potřebuje k omezení přístupu ke svým prostředkům, se zvažte aktualizaci všech tajných klíčů spojené s touto službou. Většina prostředků můžete aktualizovat pomocí následujících kroků:

    1. Přejděte na [portál Azure](https://portal.azure.com).
    2. V nabídce centra vyberte **všechny prostředky**.
    3. Vyberte prostředek. 
    4. V okně prostředků klikněte na **nastavení**. Zde si můžete zobrazit a aktualizovat existující tajných klíčů.

* **Pokud přenést předplatné uprostřed fakturačního cyklu, příjemce platím pro celý fakturaci cyklu?**

  Odesílatel je zodpovědná za platbu žádné využití, které byla nahlášena až do chvíle, že dokončení přenosu. Příjemce je zodpovědná za využití nahlásila čas přenosu a vyšší. Může mít některé využití, která byla provedena před přenosu ale ohlásil později. Využití je součástí příjemce faktury.

* **Má příjemce přístup k využití a fakturace historie?**

  Veškeré informace, které jsou k dispozici k příjemce je množství poslední faktury nebo pokud předplatné bylo přeneseno před první faktury generuje, aktuální stav. Využití a historie fakturace zbytek k přenosu k předplatnému.

* **Lze změnit nabídku během přenosu?**

  Nabídka musí zůstat stejné. Chcete-li změnit nabídku, [vašeho předplatného Azure přepnout na jinou nabídku](billing-how-to-switch-azure-offer.md).

* **Můžou přenášet předplatné na uživatelský účet v jiné zemi?**

  Ne, není podporován přenosu předplatné na uživatelský účet v jiné zemi. Příjemce uživatelský účet musí být ve stejné zemi.

* **Příjemce, můžete použít jiný způsob platby?**

  Ano. Ale historie fakturace předplatného je rozdělit mezi dva účty.  

* **Je způsob platby dopad po přesunu předplatné Azure?**

  Metoda pro platit pro odběr je třeba zadat tak, aby přijímal přenos předplatného, platební karty nebo podobné platby. Například pokud Bob přenese předplatné Jana a Jana přijímá přenos, Jana musí poskytnout způsob platby a zaplatit předplatné. Po dokončení přenosu Jana se fakturuje pro předplatné není Bob.

* **Jak migrovat data a služby pro Moje předplatné Azure do nového předplatného?**

  Pokud nelze převést vlastnictví předplatného, můžete migrovat ručně vašich prostředků. V tématu [přesunutím prostředků do nové skupiny prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md).



## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém. 


