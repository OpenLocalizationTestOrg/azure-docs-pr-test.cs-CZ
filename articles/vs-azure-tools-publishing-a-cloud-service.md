---
title: "cloudové služby pomocí nástroje Azure hello aaaPublishing | Microsoft Docs"
description: "Informace o tom, jak toopublish Azure cloud projekty služeb pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/14/2017
ms.author: kraigb
ms.openlocfilehash: 31ede8308146de2bb128b768f23f64eb85bc7548
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publishing-a-cloud-service-using-hello-azure-tools"></a>Publikování pomocí nástroje Azure hello cloudové služby
Pomocí nástroje hello Azure pro sadu Microsoft Visual Studio, můžete publikovat aplikaci Azure přímo ze sady Visual Studio. Visual Studio podporuje integrované publikování tooeither hello pracovní nebo hello provozním prostředí cloudové služby.

Než můžete publikovat aplikaci Azure, musí mít předplatné Azure. Také musíte vytvořit cloudové služby a úložiště účet toobe používá vaše aplikace. Můžete nastavit tyto v hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!IMPORTANT]
> Když publikujete, můžete vybrat hello nasazení prostředí pro cloudové služby. Také musíte vybrat účet úložiště, který je použité toostore hello balíček aplikace pro nasazení. Po nasazení odeberou se z účtu úložiště hello balíčku aplikace hello.
> 
> 

Při vývoji a testování aplikací Azure, můžete použít Web Deploy změny toopublish přírůstkově pro webové role. Po publikování prostředí pro nasazení tooa aplikací nasazení webu umožňuje nasadit změny přímo toohello virtuálního počítače se systémem hello webovou roli. Nemáte mít toopackage a publikovat aplikace celý Azure pokaždé, když chcete tooupdate vaší webové role tootest hello mění. S tímto přístupem může mít webové role změny k dispozici v hello cloudu pro testování bez čekání na toohave prostředí pro nasazení tooa publikované aplikace.

Použijte následující postupy toopublish hello aplikací Azure a tooupdate webové role pomocí nástroje nasazení webu:

* Publikování nebo balíčků aplikací Azure ze sady Visual Studio
* Aktualizovat webovou roli jako součást hello vývoj a testování cyklu

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publikování nebo balíčků aplikací Azure ze sady Visual Studio
Když publikujete aplikaci Azure, můžete provést jednu z hello následující úlohy:

* Vytvořit balíček služby: toopublish soubor konfigurace tento balíček a hello služby můžete použít prostředí pro nasazení tooa aplikace z hello [portálu Azure Classic](http://go.microsoft.com/fwlink/?LinkID=213885).
* Publikování projektu Azure ze sady Visual Studio: toopublish aplikace přímo tooAzure, použijete hello Průvodce publikování. Informace najdete v tématu [Průvodci publikováním aplikace Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="toocreate-a-service-package-from-visual-studio"></a>toocreate balíček služby ze sady Visual Studio
1. Když jsou připravené toopublish vaší aplikace, otevřete Průzkumníka řešení, otevřete hello místní nabídku pro hello Azure projekt, který obsahuje vaše role a volba možnosti publikovat.
2. toocreate balíčku služby, postupujte takto:  
   
   1. V místní nabídce hello pro hello Azure projektu, zvolte **balíček**.
   2. V hello **balíčku aplikace Azure** dialogové okno, zvolte hello služby konfigurace, pro které chcete toocreate balíček a potom konfiguraci sestavení hello.
   3. (volitelné) tooturn na vzdálené plochy pro cloudovou službu hello po publikování, vyberte hello **povolení vzdálené plochy u všech rolí** zaškrtněte políčko a potom vyberte **nastavení** tooconfigure vzdálené plochy. Pokud chcete toodebug cloudové služby po publikování, zapnout vzdálené ladění výběrem **povolení vzdáleného ladicího programu u všech rolí**.
      
      Další informace najdete v tématu [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).
   4. toocreate hello balíček, zvolte hello **balíček** odkaz.
      
      Průzkumník souborů ukazuje umístění souboru hello hello nově vytvořený balíček. Toto umístění můžete zkopírovat tak, aby je bylo možné použít ze hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
   5. toopublish tento balíček tooa nasazení prostředí, je nutné použít toto umístění jako hello umístění balíčku, při vytváření cloudové služby a nasazení tohoto balíčku tooan prostředí s hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
3. Proces nasazení hello (volitelné) toocancel, v místní nabídce hello hello řádku položky v protokolu aktivit hello, zvolte **zrušit a odeberte**. To zastaví proces nasazení hello a odstraní prostředí nasazení hello z Azure.
   
   > [!NOTE]
   > tooremove bylo toto nasazení prostředí po jeho nasazení, je nutné použít hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
   > 
   > 
4. (Volitelné) Po spuštění instance role mít Visual Studio automaticky zobrazí hello nasazení prostředí v hello **cloudové služby** uzlu v Průzkumníku serveru. Tady můžete zobrazit stav hello instance jednotlivých rolí hello. V tématu [Správa Azure prostředků pomocí Průzkumníka cloudu](vs-azure-tools-resources-managing-with-cloud-explorer.md).hello následující obrázek ukazuje instance rolí hello době, kdy jsou stále v hello Probíhá inicializace stavu:
   
    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-hello-development-and-testing-cycle"></a>Aktualizovat webovou roli jako součást hello vývoj a testování cyklu
Pokud je vaše aplikace back-end infrastruktura stabilní, ale hello webové role třeba více často aktualizovat, můžete Web Deploy tooupdate webovou roli ve vašem projektu. To je užitečné, když nechcete toorebuild použít a znovu nasaďte rolí pracovního procesu hello back-end, nebo pokud máte více webové role a chcete tooupdate pouze jeden z hello webové role.

### <a name="requirements"></a>Požadavky
Zde jsou hello požadavky toouse Web Deploy tooupdate vaši webovou roli:

* **Pouze pro vývoj a testování účely:** hello provedení změn přímo toohello virtuálního počítače se spuštěným systémem hello webové role. Pokud tento virtuální počítač má toobe recyklované, hello změny jsou ztraceny, protože je použité toorecreate hello virtuálního počítače pro roli hello hello původní balíčku, kterou jste publikovali. Je potřeba publikovat znovu aplikace tooget hello nejnovější změny pro hello webové role.
* **Aktualizovat lze pouze webové role:** rolí pracovního procesu nelze aktualizovat. Kromě toho nelze aktualizovat hello RoleEntryPoint v web role.cs.
* **Podporuje pouze jednu instanci webové role:** nemůže mít víc instancí žádné webové role v prostředí pro nasazení. Víc webových rolí každý se pouze jedna instance jsou však podporovány.
* **Je nutné povolit připojení ke vzdálené ploše:** to je potřeba, aby se Web Deploy používat hello uživatele a heslo tooconnect toohello virtuální počítač toodeploy hello změny toohello serveru se spuštěnou Internetové informační služby (IIS). Kromě toho může být nutné tooconnect toohello virtuálního počítače tooadd tooIIS důvěryhodný certifikát na tomto virtuálním počítači. (To zajišťuje, že hello vzdáleného připojení pro službu IIS, který je používán Web Deploy zabezpečení.)

Hello následujícím postupu se předpokládá, že používáte hello **publikování aplikaci Azure** průvodce.

### <a name="tooenable-web-deploy-when-you-publish-your-application"></a>tooEnable webové nasazení při můžete publikovat vaše aplikace
1. tooenable hello **povolit nasazení webu** pro všechny webové role zaškrtněte políčko, musíte nejdřív nakonfigurovat připojení ke vzdálené ploše. Vyberte **povolení vzdálené plochy** pro všechny role a potom zadejte hello pověření, které budou použité tooconnect vzdáleně v hello **konfigurace vzdálené plochy** pole, které se zobrazí. V tématu [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md) Další informace.
2. tooenable nasazení webu pro všechny hello webové role ve vaší aplikaci, vyberte **povolit nasazení webu pro všechny webové role**.
   
    Žlutý trojúhelník upozornění se zobrazí. Nasazení webu používá certifikát podepsaný svým držitelem, nedůvěryhodné ve výchozím nastavení, která se nedoporučuje pro nahrávání citlivá data. Pokud budete potřebovat toosecure tento proces pro citlivá data, můžete přidat toobe certifikát SSL používá pro nasazení webu připojení. Tento certifikát musí toobe důvěryhodný certifikát. Informace o toodo, najdete v tématu hello **tooMake nasazení zabezpečit webové** dál v tomto tématu.
3. Zvolte **Další** tooshow hello **Souhrn** obrazovce a potom vyberte **publikovat** toodeploy hello cloudové služby.
   
    Hello Cloudová služba je publikován. Hello virtuální počítač, který se vytvoří má vzdálená připojení, které jsou povolené pro službu IIS tak, aby nasazení webu lze použít tooupdate webové role znovu publikovat je.
   
   > [!NOTE]
   > Pokud máte více než jednu instanci nakonfigurována pro webovou roli, zobrazí se zpráva upozornění oznamující, že každý webovou roli budou omezené tooone instance pouze v hello balíčku, který vytvořil toopublish vaší aplikace. Vyberte **OK** toocontinue. Jak je uvedeno v části požadavky na dobrý den, můžete mít více než jednu webovou roli, ale jenom jednu instanci každé role.
   > 
   > 

### <a name="tooupdate-your-web-role-by-using-web-deploy"></a>tooUpdate vaše webové Role pomocí pomocí nasazení webu
1. toouse Web Deploy, ujistěte se, projekt toohello změny kódu pro všechny webové role v sadě Visual Studio, můžete toopublish, mají ve vašem řešení klikněte pravým tlačítkem na tento uzel projektu a bodu příliš**publikovat**. Hello **Publikovat Web** zobrazí se dialogové okno.
2. (Volitelné) Pokud jste přidali důvěryhodné toouse certifikát SSL pro vzdálená připojení pro službu IIS, můžete vymazat hello **Povolit nedůvěryhodný certifikát** zaškrtávací políčko. Informace o tom, jak tooadd certifikát toomake, Web Deploy zabezpečení, najdete v tématu hello **tooMake webové nasazení zabezpečené** dál v tomto tématu.
3. toouse nasazení webu, publikovat hello mechanismus musí hello uživatelské jméno a heslo, které jste nastavili pro vaše připojení k vzdálené ploše při prvním publikování balíčku hello.
   
   1. V **uživatelské jméno**, zadejte uživatelské jméno hello.
   2. V **heslo**, zadejte heslo hello.
   3. (Volitelné) Pokud chcete toto heslo v tomto profilu toosave, zvolte **uložit heslo**.
4. toopublish hello změny tooyour webovou roli vyberte **publikovat**.
   
    Zobrazí stav řádku Hello **publikovat spuštění**. Po dokončení publikování hello **publikování bylo úspěšné** se zobrazí. Hello změny byly nasazené toohello webovou roli na virtuálním počítači. Nyní můžete začít aplikaci Azure v prostředí Azure tootest hello změny.

### <a name="toomake-web-deploy-secure"></a>tooMake nasazení webové zabezpečení
1. Nasazení webu používá certifikát podepsaný svým držitelem, nedůvěryhodné ve výchozím nastavení, která se nedoporučuje pro nahrávání citlivá data. Pokud budete potřebovat toosecure tento proces pro citlivá data, můžete přidat toobe certifikát SSL používá pro nasazení webu připojení. Tento certifikát musí toobe důvěryhodný certifikát, který můžete získat od certifikační autority (CA).
   
    toomake nasazení webu pro každý virtuální počítač pro jednotlivé webové role zabezpečení, musíte nahrát hello důvěryhodný certifikát, který má toouse pro nasazení webu toohello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). Tím je zajištěno, že tento certifikát hello byla přidána toohello virtuální počítač, který se vytvoří pro hello webovou roli při publikování aplikace.
2. tooadd důvěryhodné toouse tooIIS certifikát SSL pro vzdálená připojení, postupujte takto:
   
   1. tooconnect toohello virtuální počítač, který běží hello webovou roli vyberte hello instanci hello webové role v **Průzkumník cloudu** nebo **Průzkumníka serveru**a potom zvolte hello **připojit pomocí Vzdálená plocha** příkaz. Podrobné pokyny o tom, tooconnect toohello virtuálního počítače, najdete v části [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).
      
      Prohlížeč zobrazí výzvu toodownload. Soubor RDP.
   2. tooadd certifikátu SSL, otevřete hello služba správy ve Správci služby IIS. Ve Správci služby IIS povolit protokol SSL ve otevírání hello **vazby** odkaz v hello **akce** podokně. Hello **přidat vazbu webu** zobrazí se dialogové okno. Zvolte **přidat**a potom vyberte HTTPS v hello **typ** rozevíracího seznamu. V hello **certifikát SSL** vyberte hello certifikát SSL, aby měl podepsaný Certifikační autoritou a že jste nahráli toohello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). Další informace najdete v tématu [konfigurace nastavení připojení pro hello služba správy](http://go.microsoft.com/fwlink/?LinkId=215824).
      
      > [!NOTE]
      > Pokud chcete přidat důvěryhodný certifikát SSL, hello žlutý trojúhelník upozornění se již v hello **Průvodci publikováním**.
      > 
      > 

## <a name="include-files-in-hello-service-package"></a>Vložené soubory v balíčku služby hello
Můžete potřebovat tooinclude konkrétní soubory v balíčku služby, aby byly k dispozici na hello virtuální počítač, který se vytvoří pro roli. Můžete například tooadd .exe nebo soubor .msi, který je používán balíček služby tooyour spuštění skriptu. Nebo může být nutné tooadd sestavení, které vyžaduje projekt webové role nebo pracovní role. tooinclude soubory, které musí být přidán toohello řešení pro vaše aplikace Azure.

### <a name="tooinclude-files-in-hello-service-package"></a>tooinclude soubory v balíčku služby hello
1. tooadd balíček služby tooa sestavení hello použijte následující kroky:
   
   1. V **Průzkumníku řešení** otevřete hello uzel projektu pro hello projekt, který chybí hello odkazovaná sestavení.
   2. Projekt toohello tooadd hello sestavení, otevřete hello místní nabídku pro hello **odkazy** složku a potom zvolte **přidat odkaz na**. Otevře se dialogové okno Přidat odkaz na Hello.
   3. Zvolte možnost hello odkaz má tooadd a potom zvolte hello **OK** tlačítko.
      
      odkaz Hello přidán toohello seznamu v části hello **odkazy** složky.
   4. Otevřete hello místní nabídku pro hello sestavení, které jste přidali a zvolte **vlastnosti**. Hello **vlastnosti** se zobrazí v okně.
      
      tooinclude toto sestavení v hello service balíčku v hello **kopie místního seznamu** zvolte **True**.
2. V **Průzkumníku řešení** otevřete hello uzel projektu pro hello projekt, který chybí hello odkazovaná sestavení.
3. Projekt toohello tooadd hello sestavení, otevřete hello místní nabídku pro hello **odkazy** složku a potom zvolte **přidat odkaz na**. Hello **přidat odkaz na** otevře se dialogové okno.
4. Zvolte možnost hello odkaz má tooadd a potom zvolte hello **OK** tlačítko.
   
    odkaz Hello přidán toohello seznamu v části hello **odkazy** složky.
5. Otevřete hello místní nabídku pro hello sestavení, které jste přidali a zvolte **vlastnosti**. Zobrazí se okno Vlastnosti Hello.
6. tooinclude toto sestavení v hello service balíčku v hello **místní kopie** vyberte **True**.
7. Otevřete hello místní nabídky souboru hello tooinclude soubory v balíčku služby hello, které byly přidány tooyour projekt webové role, a potom zvolte **vlastnosti**. Z hello **vlastnosti** okně zvolte **obsahu** z hello **akce sestavení** pole se seznamem.
8. Otevřete hello místní nabídky souboru hello tooinclude soubory v balíčku služby hello, které byly přidány tooyour projekt role pracovního procesu, a potom zvolte **vlastnosti**. Z hello **vlastnosti** okně zvolte **kopírovat, pokud je novější** z hello **adresáře toooutput kopie** pole se seznamem.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o publikování tooAzure ze sady Visual Studio, najdete v části [Průvodci publikováním aplikace Azure](vs-azure-tools-publish-azure-application-wizard.md).

