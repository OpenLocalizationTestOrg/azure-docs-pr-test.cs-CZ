#### <a name="toocreate-a-new-service"></a>toocreate novou službu

1.  Pomocí svých přihlašovacích údajů účtu Microsoft, přihlásit toohello portál Azure na této adrese URL: <https://portal.azure.com/>. Pokud nasazení zařízení hello Government portálu, přihlaste se na: <https://portal.azure.us/>

2.  V hello portálu Azure, klikněte na **+ nový** &gt; **úložiště** &gt; **virtuální řady StorSimple**.

    ![Vytvoření nové služby](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  V hello **Manager zařízení StorSimple** okno, které se otevře, hello následující:

    1.  Zadejte pro službu jedinečný **Název prostředku**. název prostředku Hello je popisný název, který lze použít tooidentify hello služby. Název Hello může mít 2 až 50 znaků, které mohou být písmena, číslice a pomlčky. Hello název musí začínat a končit písmenem nebo číslicí.

    2.  Vyberte **předplatné** z rozevíracího seznamu hello. předplatné Hello je propojené tooyour fakturace účtu. Toto pole není dostupné, pokud máte pouze jedno předplatné.

    3.  Pro **skupiny prostředků**, vyberte existující, nebo vytvořte novou skupinu. Další informace najdete v tématu [Skupiny prostředků Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

    4.  Zadejte **Umístění** služby. V tématu [oblasti Azure](https://azure.microsoft.com/regions/#services) Další informace o tom, které služby jsou dostupné v konkrétních oblastech. Obecně platí, vyberte **umístění** nejbližší geografické oblasti toohello místo toodeploy zařízení. Můžete také toofactor v hello následující:

        -   Pokud máte existující úlohy v Azure také chcete toodeploy s vaším zařízením StorSimple, doporučujeme vám, že používáte stejné datové centrum.

        -   Správce zařízení StorSimple a Azure úložiště může být na dvě samostatná místa. V takovém případě jste požadované toocreate hello Správce zařízení StorSimple a účet úložiště Azure odděleně. toocreate účtu úložiště Azure, přejděte toohello služby Azure Storage v hello portál Azure a postupujte podle kroků hello v [vytvoření účtu úložiště Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account). Po vytvoření tohoto účtu, přidejte ho služby StorSimple Manager zařízení toohello podle následujících kroků hello v [konfigurace nového účtu úložiště pro službu hello](https://azure.microsoft.com/en-us/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service).

        -   Pokud nasazení hello virtuálního zařízení v hello Government portál, hello služby StorSimple Manager zařízení je k dispozici v nám Iowa a Virginia nám umístění.

    5.  Vyberte **vytvořit nový účet úložiště Azure** tooautomatically vytvořit účet úložiště se službou hello. Zadejte **název účtu úložiště**. Pokud potřebujete mít data v jiném umístění, zaškrtnutí políčka zrušte.

    6.  Zkontrolujte **Pin toodashboard** Pokud chcete službu toothis rychlý odkaz na řídicím panelu.

    7.  Klikněte na tlačítko **vytvořit** toocreate hello Správce zařízení StorSimple.

        ![Vytvoření nové služby](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

Jsou řízené toohello **služby** cílová stránka. vytvoření služby Hello trvá několik minut. Po úspěšném vytvoření služby hello, budete odpovídajícím způsobem upozorněni a stav hello služby hello změní příliš**Active**.


