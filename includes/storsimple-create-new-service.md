<!--author=alkohli last changed:01/14/2016-->


#### <a name="toocreate-a-new-service"></a>toocreate novou službu
1. Pomocí svých přihlašovacích údajů účtu Microsoft, přihlásit toohello portál Azure classic na této adrese URL: [https://manage.windowsazure.com/](https://manage.windowsazure.com/).
2. V hello portál Azure classic, klikněte na **nový** > **datové služby** > **StorSimple Manager** > **rychlé Vytvoření**.
3. Ve formuláři hello, který se zobrazí hello následující:
   
   1. Zadejte jedinečný **Název** služby. Toto je popisný název, který lze použít tooidentify hello služby. Název Hello může mít 2 až 50 znaků, které mohou být písmena, číslice a pomlčky. Hello název musí začínat a končit písmenem nebo číslicí.
   2. Zadejte **Umístění** služby. Obecně platí zvolte umístění nejbližší toohello zeměpisné oblasti místo toodeploy zařízení. Můžete také toofactor v hello následující: 
      
      * Pokud máte existující úlohy v Azure také chcete toodeploy s vaším zařízením StorSimple, používejte stejné datové centrum.
      * Služby StorSimple Manager a úložiště Azure mohou být ve dvou různých umístěních. V takovém případě jste hello požadované toocreate StorSimple Manager a účet úložiště Azure odděleně. toocreate účtu úložiště Azure, přejděte toohello služby Azure Storage v hello portál Azure classic a postupujte podle kroků hello v [vytvoření účtu úložiště Azure](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account). Po vytvoření tohoto účtu, přidejte ho služby StorSimple Manager toohello podle následujících kroků hello v [konfigurace nového účtu úložiště pro službu hello](../articles/storsimple/storsimple-deployment-walkthrough.md#configure-a-new-storage-account-for-the-service).
   3. Vyberte **předplatné** z rozevíracího seznamu hello. předplatné Hello je propojené tooyour fakturace účtu. Toto pole není dostupné, pokud máte pouze jedno předplatné.
   4. Vyberte **vytvořit nový účet úložiště** tooautomatically vytvořit účet úložiště se službou hello. Takový účet úložiště bude mít speciální název, například storsimplebwv8c6dcnf. Pokud potřebujete mít data v jiném umístění, zaškrtnutí políčka zrušte. 
   5. Klikněte na tlačítko **vytvořit StorSimple Manager** toocreate hello služby.
   
   ![Vytvoření služby StorSimple Manager](./media/storsimple-create-new-service/HCS_CreateAService-include.png)
   
   Bude směrovanou toohello **služby** cílová stránka. vytvoření služby Hello může trvat několik minut. Po úspěšném vytvoření služby hello, budete odpovídajícím způsobem upozorněni a stav hello služby hello změní příliš**Active**.
   
   ![Vytvoření služby](./media/storsimple-create-new-service/HCS_StorSimpleManagerServicePage-include.png)

![Dostupné video](./media/storsimple-create-new-service/Video_icon.png)**Dostupné video**

toowatch video, které ukazuje, jak toocreate novou službu StorSimple Manager, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/create-a-storsimple-manager-service/).

