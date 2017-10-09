<!--author=alkohli last changed:02/10/2017-->


#### <a name="toocreate-a-new-service"></a>toocreate novou službu

1. Použijte váš toolog přihlašovací údaje účtu Microsoft na toohello [portál Azure](https://portal.azure.com/).

2. V hello portálu Azure, klikněte na  **+**  a hello Marketplace, klikněte na tlačítko **zobrazit všechny**.

    ![Vytvoření Správce zařízení StorSimple](./media/storsimple-8000-create-new-service/createssdevman1.png)

    Vyhledejte _Fyzické zařízení StorSimple_. Vyberte a klikněte na **Řada fyzických zařízení StorSimple** a pak klikněte na **Vytvořit**. Můžete taky v hello portálu Azure klikněte na  **+**  a potom v části **úložiště**, klikněte na tlačítko **fyzického zařízení řady StorSimple**.

    ![Vytvoření Správce zařízení StorSimple](./media/storsimple-8000-create-new-service/createssdevman11.png)

3. V hello **Manager zařízení StorSimple** okně hello následující kroky:
   
   1. Zadejte pro službu jedinečný **Název prostředku**. Tento název je popisný název, který lze použít tooidentify hello služby. Název Hello může mít 2 až 50 znaků, které mohou být písmena, číslice a pomlčky. Hello název musí začínat a končit písmenem nebo číslicí.

   2. Vyberte **předplatné** z rozevíracího seznamu hello. předplatné Hello je propojené tooyour fakturace účtu. Toto pole není dostupné, pokud máte pouze jedno předplatné.

   3. V části **Skupina prostředků** vyberte možnost **Použít existující** nebo **Vytvořit novou** skupinu. Další informace najdete v tématu [Skupiny prostředků Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).
   
   4. Zadejte **Umístění** služby. Obecně platí zvolte umístění nejbližší toohello zeměpisné oblasti místo toodeploy zařízení. Můžete také toofactor v hello následující aspekty: 
      
      * Pokud máte existující úlohy v Azure také chcete toodeploy s vaším zařízením StorSimple, používejte stejné datové centrum.
      * Služba Správce zařízení StorSimple a úložiště Azure můžou být ve dvou různých umístěních. V takovém případě jste požadované toocreate hello Správce zařízení StorSimple a účet úložiště Azure odděleně. toocreate účtu úložiště Azure, přejděte toohello služby Azure Storage v hello portál Azure a postupujte podle kroků hello v [vytvoření účtu úložiště Azure](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account). Po vytvoření tohoto účtu, přidejte ho služby StorSimple Manager zařízení toohello podle následujících kroků hello v [konfigurace nového účtu úložiště pro službu hello](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#configure-a-new-storage-account-for-the-service).

   5. Vyberte **vytvořit nový účet úložiště** tooautomatically vytvořit účet úložiště se službou hello. Zadejte název tohoto účtu úložiště. Pokud potřebujete mít data v jiném umístění, zaškrtnutí políčka zrušte.

   6. Zkontrolujte **Pin toodashboard** Pokud chcete službu toothis rychlý odkaz na řídicím panelu.
      
   7. Klikněte na tlačítko **vytvořit** toocreate hello Správce zařízení StorSimple.

       ![Vytvoření Správce zařízení StorSimple](./media/storsimple-8000-create-new-service/createssdevman2.png)
   
vytvoření služby Hello trvá několik minut. Po úspěšném vytvoření služby hello, zobrazí se oznámení a otevře se nové okno služby hello.
   
![Vytvoření Správce zařízení StorSimple](./media/storsimple-8000-create-new-service/createssdevman5.png)


