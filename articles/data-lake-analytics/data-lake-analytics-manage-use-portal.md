---
title: "hello aaaManage Azure Data Lake Analytics pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage počtech Data Lake Analytics, datové zdroje, uživatelé a úlohy."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: f63ccdfae79772c92e92462194e8cdc636a73dc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-hello-azure-portal"></a>Správa Azure Data Lake Analytics pomocí portálu Azure hello
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Zjistěte, jak hello toomanage účtů Azure Data Lake Analytics, účet zdroje dat, uživatele a úlohy pomocí portálu Azure. témata týkající se řízení toosee o pomocí jiných nástrojů, klikněte na karty v horní části hello hello stránky.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Správa účtů Data Lake Analytics

### <a name="create-an-account"></a>Vytvoření účtu

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na **Nový** > **Inteligentní funkce a analýzy** > **Data Lake Analytics**.
3. Vyberte hodnoty pro hello následující položky: 
   1. **Název**: název hello hello účtu Data Lake Analytics.
   2. **Předplatné**: hello předplatné Azure použité pro účet hello.
   3. **Skupina prostředků**: Skupina prostředků Azure hello v účtu, který toocreate hello. 
   4. **Umístění**: hello datové centrum Azure pro účet Data Lake Analytics hello. 
   5. **Data Lake Store**: hello výchozí úložiště toobe použité pro účet Data Lake Analytics hello. účet Azure Data Lake Store Hello a hello účet musí být v Data Lake Analytics hello stejné umístění.
4. Klikněte na možnost **Vytvořit**. 

### <a name="delete-a-data-lake-analytics-account"></a>Odstranění účtu Data Lake Analytics

Před odstraněním účtu Data Lake Analytics, odstraňte jeho výchozí účet Data Lake Store.

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Klikněte na **Odstranit**.
3. Zadejte název účtu hello.
4. Klikněte na **Odstranit**.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Správa zdrojů dat

Data Lake Analytics podporuje hello následující zdroje dat:

* Data Lake Store
* Azure Storage

Můžete použít zdroje dat toobrowse Průzkumníku dat a provádět operace správy základního souboru. 

### <a name="add-a-data-source"></a>Přidání zdroje dat

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Klikněte na tlačítko **zdroje dat**.
3. Klikněte na tlačítko **přidat zdroj dat**.
    
   * tooadd účtu Data Lake Store, potřebujete účet hello názvem a přístupovým toohello účet toobe možné tooquery ho.
   * tooadd úložiště objektů Blob v Azure, potřebujete účet úložiště hello a klíč účtu hello. toofind účtu úložiště toohello je, přejděte na portál hello.

## <a name="set-up-firewall-rules"></a>Nastavení pravidel brány firewall

Data Lake Analytics toofurther uzamčení tooyour přístup k účtu Data Lake Analytics můžete použít na úrovni sítě hello. Můžete povolit bránu firewall, zadejte IP adresu nebo zadejte rozsah IP adres pro klienty důvěryhodné. Povolíte-li tato opatření, můžete připojit pouze klienti, kteří mají hello IP adresy v rozsahu hello definované toohello úložiště.

Pokud jinými službami Azure, jako je Azure Data Factory nebo virtuální počítače, připojit toohello účet Data Lake Analytics, ujistěte se, že **povolit služby Azure** je zapnuta **na**. 

### <a name="set-up-a-firewall-rule"></a>Nastavit pravidlo brány firewall

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. V nabídce hello na levé straně hello, klikněte na tlačítko **brány Firewall**.

## <a name="add-a-new-user"></a>Přidání nového uživatele

Můžete použít hello **Průvodce přidáním uživatele** tooeasily zřídit nové uživatele Data Lake.

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Na levé části hello **Začínáme**, klikněte na tlačítko **Průvodce přidáním uživatele**.
3. Vyberte uživatele a pak klikněte na **vyberte**.
4. Vyberte roli a pak klikněte na tlačítko **vyberte**. tooset si nové toouse vývojáře Azure Data Lake, vyberte hello **Data Lake Analytics vývojáře** role.
5. Vyberte seznamy řízení přístupu (ACL) hello databází hello U-SQL. Až budete spokojeni s vaší volby, klikněte na tlačítko **vyberte**.
6. Vyberte hello seznamy řízení přístupu pro soubory. Pro výchozí úložiště hello, neměnit hello seznamy ACL pro kořenovou složku hello "/" a ve složce/System hello. Klikněte na **Vybrat**.
7. Zkontrolujte všechny vybrané změny a pak klikněte na tlačítko **spustit**.
8. Po dokončení Průvodce hello klikněte na tlačítko **provádí**.

## <a name="manage-role-based-access-control"></a>Správa řízení přístupu na základě rolí

Jako jinými službami Azure můžete použít toocontrol řízení přístupu na základě Role (RBAC), jak uživatelé pracují se službou hello.

standardní role RBAC Hello mít hello následující možnosti:
* **Vlastník**: můžete odeslat úlohy sledování úloh, zrušit úlohy z libovolného uživatele a nakonfigurovat účet hello.
* **Přispěvatel**: můžete odeslat úlohy sledování úloh, zrušit úlohy z libovolného uživatele a nakonfigurovat účet hello.
* **Čtečka**: můžete sledovat úlohy.

Pomocí hello Data Lake Analytics vývojáře role tooenable U-SQL vývojáři toouse hello Data Lake Analytics služby. Můžete použít role Data Lake Analytics vývojáře hello:
* Odeslání úlohy.
* Monitorování úlohy stavu a hello průběh úlohy, odeslané žádný uživatel.
* V tématu hello skriptů U-SQL z úlohy, odeslané žádný uživatel.
* Zrušte jenom vlastní úlohy.

### <a name="add-users-or-security-groups-tooa-data-lake-analytics-account"></a>Přidejte uživatele nebo skupiny tooa zabezpečení účtu Data Lake Analytics

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** > **přidat**.
3. Vyberte roli.
4. Přidání uživatele.
5. Klikněte na **OK**.

>[!NOTE]
>Pokud uživatel nebo skupina zabezpečení potřebuje toosubmit úlohy, budou také potřebovat oprávnění na účtu úložiště hello. Další informace najdete v tématu [zabezpečení dat uložených v Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>Správa úloh

### <a name="submit-a-job"></a>Odeslání úlohy

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.

2. Klikněte na tlačítko **nová úloha**. Pro každou úlohu konfigurace:

    1. **Název úlohy**: název hello hello úlohy.
    2. **Priorita**: nižší čísla mají vyšší prioritu. Pokud dvě úlohy jsou zařazeny do fronty, spustí se první hello, jeden s nižší hodnotou priority.
    3. **Paralelismus**: hello maximální počet výpočetních procesů tooreserve pro tuto úlohu.

3. Klikněte na **Odeslat úlohu**.

### <a name="monitor-jobs"></a>Monitorování úloh

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Klikněte na tlačítko **vidět všechny úlohy**. Se zobrazí seznam všech hello aktivní a nedávno dokončení úloh v účtu hello.
3. Volitelně klikněte na **filtru** toohelp najít hello úloh podle **časový rozsah**, **název úlohy**, a **Autor** hodnoty. 

### <a name="monitoring-pipeline-jobs"></a>Sledování úloh kanálu
Úlohy, které jsou součástí kanálu fungují společně, obvykle postupně tooaccomplish na konkrétní scénář. Například můžete mít kanál, který odstraní, extrahuje, transformuje, agreguje využití přehledů zákazníka. Úlohy kanálu se identifikují pomocí vlastnosti "Kanál" hello, pokud hello úloha byla odeslána. Pomocí ADF V2 naplánované úlohy mít tato vlastnost vyplní automaticky. 

tooview seznam úloh U-SQL, které jsou součástí kanály: 

1. V hello portálu Azure přejděte tooyour účtů Data Lake Analytics.
2. Klikněte na tlačítko **úlohy Insights**. Hello "Všechny úlohy" karta bude uvedena, zobrazující seznam spuštění ve frontě a skončila úlohy.
3. Klikněte na tlačítko hello **kanálu úlohy** kartě. Zobrazí se seznam úloh kanálu spolu s souhrnných statistik pro každý kanál.

### <a name="monitoring-recurring-jobs"></a>Monitorování opakované úlohy
Opakování úlohy je ten, který má hello stejné obchodní logiku, ale používá jiné vstupní data pokaždé, když ji spustí. V ideálním případě opakované úlohy doporučujeme vždy úspěšné a mají relativně stabilní čas provádění; monitorování těchto chování pomůže zajistit, že úloha hello je v pořádku. Opakované úlohy se identifikují pomocí vlastnosti "Recurrence" hello. Pomocí ADF V2 naplánované úlohy mít tato vlastnost vyplní automaticky.

seznam úloh U-SQL, které jsou opakovaného tooview: 

1. V hello portálu Azure přejděte tooyour účtů Data Lake Analytics.
2. Klikněte na tlačítko **úlohy Insights**. Hello "Všechny úlohy" karta bude uvedena, zobrazující seznam spuštění ve frontě a skončila úlohy.
3. Klikněte na tlačítko hello **opakovaných úloh** kartě. Společně s souhrnných statistik pro každou úlohu opakující se zobrazí seznam opakované úlohy.

## <a name="manage-policies"></a>Správa zásad

### <a name="account-level-policies"></a>Zásady na úrovni účtu

Tyto zásady použít tooall úloh v účtu Data Lake Analytics.

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Maximální počet Austrálie v účtu Data Lake Analytics
Zásady řídí hello celkový počet jednotek Analytics (Austrálie) můžete použít váš účet Data Lake Analytics. Ve výchozím nastavení je nastavena hodnota hello too250. Například pokud je tato hodnota nastavená too250 Austrálie, vám může mít jednu úlohu s 250 tooit Austrálie přiřazené nebo běží s 25 10 úlohy Austrálie každý. Další úlohy, které jsou odeslány jsou zařazeny do fronty, dokud se všechny spuštěné úlohy hello. Po dokončení probíhající úlohy jsou Austrálie jsou uvolněny pro hello zařazených do fronty úloh toorun.

toochange hello počet Austrálie pro váš účet Data Lake Analytics:

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Klikněte na **Vlastnosti**.
3. V části **maximální Austrálie**, přesuňte jezdec tooselect hello hodnotu nebo zadejte hodnotu hello hello textového pole. 
4. Klikněte na **Uložit**.

> [!NOTE]
> Pokud budete potřebovat víc než hello výchozí (250) Austrálie, hello portálu, klikněte na tlačítko **podpora + nápovědy** toosubmit žádosti o podporu. je možné zvýšit počet Hello Austrálie k dispozici v účtu Data Lake Analytics.
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Maximální počet úloh, které můžou běžet současně
Zásady řídí, kolik úlohy můžete spustit na hello stejnou dobu. Ve výchozím nastavení je tato hodnota nastavena too20. Pokud vaše Data Lake Analytics má Austrálie k dispozici, nové úlohy jsou naplánované toorun až hello celkový počet spuštěných úloh nedosáhne hello hodnoty těchto zásad. Když se dostanete hello maximální počet úloh, které můžou běžet současně, následné úlohy jsou zařazeny do fronty v pořadí podle priority, dokud jeden nebo více spuštěné úlohy dokončení (v závislosti na dostupnosti AU).

toochange hello počet úloh, které můžou běžet současně:

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Klikněte na **Vlastnosti**.
3. V části **maximální číslo z spuštění úlohy**, přesuňte jezdec tooselect hello hodnotu nebo zadejte hodnotu hello hello textového pole. 
4. Klikněte na **Uložit**.

> [!NOTE]
> Pokud potřebujete více než hello výchozí (20) počet úloh, hello portálu, klikněte na tlačítko toorun **podpora + nápovědy** toosubmit žádosti o podporu. může být zvýšena Hello počet úloh, které můžou běžet současně v účtu Data Lake Analytics.
>

#### <a name="how-long-tookeep-job-metadata-and-resources"></a>Jak dlouho metadata tookeep úlohy a prostředky 
Když uživatelé spustí úloh U-SQL, hello služby Data Lake Analytics uchovává všechny související soubory. Související soubory zahrnují hello U-SQL skriptu, soubory knihoven DLL hello odkazovaný ve skriptu hello U-SQL, kompilované prostředky a statistiky. Hello soubory jsou ve složce /system/ hello hello výchozí účet Azure Data Lake Storage. Tato zásada určuje, jak dlouho tyto prostředky jsou uložené před automaticky odstraní (hello výchozí hodnota je 30 dní). Tyto soubory můžete použít pro ladění a optimalizace výkonu úloh, které budete znovu spustit v budoucnu hello.

toochange jak dlouho metadata tookeep úlohy a prostředky:

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Klikněte na **Vlastnosti**.
3. V části **tooRetain dní úloha se dotazuje**, přesuňte jezdec tooselect hello hodnotu nebo zadejte hodnotu hello hello textového pole.  
4. Klikněte na **Uložit**.

### <a name="job-level-policies"></a>Zásady na úrovni úlohy
Pomocí zásad na úrovni úlohy, můžete řídit hello maximální Austrálie a maximální priority, kterou jednotlivé uživatele (nebo členy určité skupiny zabezpečení) můžete nastavit na úlohy, které se odesílají hello. Tato umožňuje řídit náklady hello způsobené uživatele. Umožňuje vám také může mít vliv hello ovládací prvek naplánovaných úlohách na s vysokou prioritou provozní úlohy, které jsou spuštěny v hello stejný účet Data Lake Analytics.

Data Lake Analytics má dvě zásady, které můžete nastavit na úrovni hello úlohy:

* **AU limit na jednu úlohu**: uživatelé lze odeslat pouze úlohy, které jste si toothis počet Austrálie. Ve výchozím nastavení je tento limit hello stejné jako maximální limit AU hello hello účtu.
* **Priorita**: uživatelé lze odeslat pouze úlohy, které mají hodnotu nižší než nebo rovna toothis priority. Všimněte si, že vyšší číslo znamená s nižší prioritou. Ve výchozím nastavení je nastavena too1, což je nejvyšší možná priorita hello.

Existuje výchozí zásada, nastavte pro každý účet. Hello výchozí zásada tooall uživatelé hello účtu. Další zásady můžete nastavit pro konkrétní uživatele a skupiny. 

> [!NOTE]
> Zásady na úrovni účtu a zásady na úrovni úlohy použít současně.
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a>Přidání zásad pro konkrétního uživatele nebo skupiny

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Klikněte na **Vlastnosti**.
3. V části **úlohy odeslání omezení**, klikněte na tlačítko hello **přidat zásadu** tlačítko. Potom vyberte nebo zadejte hello následující nastavení:
    1. **Název zásady výpočetní**: Zadejte název zásady, tooremind o účelu hello hello zásad.
    2. **Vyberte uživatele nebo skupiny**: Vyberte hello uživatele nebo skupiny, tato zásada se vztahuje na.
    3. **Nastavit hello Limit AU úloh**: nastaveného limitu hello AU, která se použije toohello vybrané uživatele nebo skupinu.
    4. **Nastavit hello Priority Limit**: nastavte hello priority limit, která se použije toohello vybrané uživatele nebo skupinu.

4. Klikněte na tlačítko **OK**.

5. nové zásady Hello je uvedena v hello **výchozí** zásad v části tabulky **úlohy odeslání omezení**. 

#### <a name="delete-or-edit-an-existing-policy"></a>Odstraňte nebo upravte existující zásady

1. V hello portálu Azure přejděte tooyour účet Data Lake Analytics.
2. Klikněte na **Vlastnosti**.
3. V části **úlohy odeslání omezení**, najít hello zásady chcete tooedit.
4.  toosee hello **odstranit** a **upravit** klikněte na možnosti v hello pravou krajní sloupec v tabulce hello **...** .

### <a name="additional-resources-for-job-policies"></a>Další prostředky pro úlohy zásady
* [Příspěvek blogu přehled zásad](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Zásady na úrovni účtu příspěvku na blogu](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Zásady na úrovni úlohy příspěvku na blogu](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Další kroky

* [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Začínáme s Data Lake Analytics pomocí portálu Azure hello](data-lake-analytics-get-started-portal.md)
* [Správa Azure Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-manage-use-powershell.md)

