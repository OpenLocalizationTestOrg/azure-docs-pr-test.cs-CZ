---
title: "aaaHow toomanage služby, konfigurace a profily | Microsoft Docs"
description: "Zjistěte, jak se služba Konfigurace profilů a konfigurační soubory toowork | které uložit nastavení pro hello nasazení prostředí a nastavení publikování pro cloudové služby."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7da8c551-fb06-4057-b5c7-c77f4b39d803
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/11/2017
ms.author: kraigb
ms.openlocfilehash: 1dba9df2fa57fe94dacc90ae74b05ccdc28270c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-service-configurations-and-profiles"></a>Jak toomanage služby, konfigurace a profily
## <a name="overview"></a>Přehled
Při publikování cloudové služby Visual Studio ukládá informace o konfiguraci v dva druhy konfigurační soubory: služby konfigurace a profily. Konfigurace služby (soubory .cscfg) uložit nastavení pro hello nasazení prostředí pro cloudové služby Azure. Azure používá tyto konfigurační soubory, pokud spravuje vaše cloudové služby. Na dobrý den druhé straně, profilů (soubory .azurePubxml) úložiště nastavení publikování pro cloudové služby. Tato nastavení jsou záznam co zvolíte, že použijete hello Průvodce publikováním a jsou místně využívá sada Visual Studio. Toto téma vysvětluje, jak toowork s oběma typy konfigurační soubory.

## <a name="service-configurations"></a>Konfigurace služby
Můžete vytvořit více toouse konfigurace služby pro každé prostředí nasazení. Například může vytvořit konfiguraci služby pro místní prostředí hello použít toorun a testování aplikací Azure a jiné konfigurace služby pro produkční prostředí.

Můžete přidat, odstranění, přejmenování a upravit tyto konfigurace služby podle svých požadavků. Ze sady Visual Studio, můžete spravovat tyto konfigurace služby, jak ukazuje následující obrázek hello.

![Správa konfigurace služby](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Můžete také otevřít hello **spravovat konfigurace** dialogové okno ze stránky vlastností hello role. tooopen hello vlastnosti pro roli v projektu Azure otevřete hello místní nabídku pro tuto roli a potom zvolte **vlastnosti**. Na hello **nastavení** rozbalte hello **konfigurace služby** seznamu a pak vyberte **spravovat** tooopen hello **spravovat konfigurace**dialogové okno.

### <a name="tooadd-a-service-configuration"></a>tooadd konfigurace služby
1. V Průzkumníku řešení otevřete hello místní nabídku pro hello projektu Azure a pak vyberte **spravovat konfigurace**.
   
    Hello **konfigurace služby Správa** zobrazí se dialogové okno.
2. tooadd konfigurace služby, musíte vytvořit kopii stávající konfigurace. toodo, zvolte hello konfiguraci chcete toocopy hello název seznamu a pak vyberte **vytvořit kopii**.
3. Konfigurace služby (volitelné) toogive hello jiný název, vyberte novou konfiguraci služby hello hello název seznamu a pak vyberte **přejmenovat**. V hello **název** textového pole, název typu hello má toouse pro tuto konfiguraci služby a potom vyberte **OK**.
   
    Nové služby konfiguračního souboru, který je s názvem objekt ServiceConfiguration. [Nový název] .cscfg je přidána toohello projektu Azure v Průzkumníku řešení.

### <a name="toodelete-a-service-configuration"></a>toodelete konfigurace služby
1. V Průzkumníku řešení otevřete hello místní nabídku pro hello projektu Azure a pak vyberte **spravovat konfigurace**.
   
    Hello **konfigurace služby Správa** zobrazí se dialogové okno.
2. toodelete konfigurace služby, zvolte hello konfiguraci, které chcete toodelete z hello **název** seznamu a pak vyberte **odebrat**. Zobrazí se dialogové okno tooverify má toodelete tuto konfiguraci.
3. Vyberte **Odstranit**.
   
     konfigurační soubor služby Hello se odebere ze hello projektu Azure v Průzkumníku řešení.

### <a name="toorename-a-service-configuration"></a>toorename konfigurace služby
1. V Průzkumníku řešení otevřete hello místní nabídku pro hello projektu Azure a pak vyberte **spravovat konfigurace**.
   
    Hello **konfigurace služby Správa** zobrazí se dialogové okno.
2. toorename konfigurace služby, vyberte novou konfiguraci služby hello z hello **název** seznamu a pak vyberte **přejmenovat**. V hello **název** textového pole, název typu hello má toouse pro tuto konfiguraci služby a pak vyberte **OK**.
   
    Název Hello konfigurační soubor služby hello se změnilo v hello projektu Azure v Průzkumníku řešení.

### <a name="toochange-a-service-configuration"></a>toochange konfigurace služby
* Pokud chcete, aby toochange konfigurace služby, otevřete hello místní nabídku pro určité role hello má toochange v hello projektu Azure a pak vyberte **vlastnosti**. V tématu [postupy: Konfigurace rolí hello pro cloudové služby Azure pomocí sady Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) Další informace.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Zkontrolujte nastavení různé kombinace s použitím profilů
Pomocí profilu můžete automaticky vyplnit hello **Průvodci publikováním** nabízela jinou kombinaci nastavení pro jiné účely. Například může mít jeden profil pro ladění a druhý pro verzi sestavení. V takovém případě vaše **ladění** profil by měla mít **IntelliTrace** povolené a hello **ladění** konfigurace vybrali a **verze** profil by měla mít **IntelliTrace** zakázána a hello **verze** konfigurace vybraná. Můžete také použít různé profily toodeploy služby pomocí jiný účet úložiště.

Když spustíte Průvodce hello hello poprvé, vytvoří se výchozí profil. Visual Studio ukládá hello profil v souboru, který má příponu .azurePubXml, který se přidal tooyour projektu Azure v části hello **profily** složky. Pokud ručně zadáte různé možnosti, když spustíte Průvodce hello později, soubor hello se automaticky aktualizuje. Před spuštěním hello postupem, měli jste již publikovali cloudové služby alespoň jednou.

### <a name="tooadd-a-profile"></a>tooadd profil
1. Otevřete hello místní nabídky projektu Azure a pak vyberte **publikovat**.
2. Další toohello **cíle profil** seznamu, vyberte hello **uložit profil** tlačítko jako hello následující obrázek ukazuje. Tím se vytvoří profil pro vás.
   
    ![Vytvořit nový profil](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)
3. Po vytvoření profilu hello vyberte **< spravovat... >** v hello **cíle profil** seznamu.
   
    Hello **spravovat profily** dialogové okno se zobrazí jako hello následující obrázek ukazuje.
   
    ![Dialogové okno profily spravovat](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)
4. V hello **název** seznam, vyberte profil a pak vyberte **vytvořit kopii**.
5. Zvolte hello **Zavřít** tlačítko.
   
    nový profil Hello se zobrazí v seznamu cíl profilu hello.
6. V hello **cíle profil** seznamu, vyberte hello profil, který jste právě vytvořili. Nastavení Průvodce publikování Hello se vyplní hello volby z hello profilu, který jste vybrali.
7. Vyberte hello **předchozí** a **Další** tlačítka toodisplay každou stránku hello Průvodce Publikovat a pak přizpůsobit hello nastavení pro tento profil. V tématu [Průvodci publikováním aplikace Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) informace.
8. Po dokončení přizpůsobení hello nastavení, vyberte **Další** toogo back toohello nastavení stránky. profil Hello je uložen při publikování služby hello pomocí těchto nastavení, nebo pokud jste vybrali **Uložit** další toohello seznam profilů.

### <a name="toorename-or-delete-a-profile"></a>toorename nebo odstranění profilu
1. Otevřete hello místní nabídky projektu Azure a pak vyberte **publikovat**.
2. V hello **cíle profil** seznamu, vyberte **spravovat**.
3. V hello **spravovat profily** dialogové okno, vyberte hello profil má toodelete a pak vyberte **odebrat**.
4. V hello potvrzení zobrazeném dialogu, vyberte **OK**.
5. Vyberte **Zavřít**.

### <a name="toochange-a-profile"></a>toochange profil
1. Otevřete hello místní nabídky projektu Azure a pak vyberte **publikovat**.
2. V hello **cíle profil** seznamu, vyberte hello profilu, které chcete toochange.
3. Vyberte hello **předchozí** a **Další** tlačítka toodisplay každou stránku hello Průvodci publikováním a potom chcete změnit hello nastavení. V tématu [Průvodci publikováním aplikace Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) informace.
4. Po dokončení změn hello nastavení, vyberte **Další** toogo back toohello **nastavení** stránky.
5. (Volitelné) vyberte **publikovat** toopublish hello cloudové služby pomocí nového nastavení hello. Pokud nechcete, aby toopublish cloudové služby v tuto chvíli a zavřete hello Průvodci publikováním, Visual Studio požádá, pokud chcete, aby toosave hello změny toohello profilu.

## <a name="next-steps"></a>Další kroky
toolearn o konfiguraci dalších částí projektu Azure ze sady Visual Studio, najdete v části [konfigurace projektu Azure](http://go.microsoft.com/fwlink/p/?LinkID=623075)

