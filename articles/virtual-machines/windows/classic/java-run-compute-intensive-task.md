---
title: "aplikace Java aaaCompute náročných na virtuálním počítači | Microsoft Docs"
description: "Zjistěte, jak se dá sledovat toocreate Azure virtuální počítač, který spouští aplikaci Java náročné, který ho jiná aplikace Java."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: ae6f2737-94c7-4569-9913-d871450c2827
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 02a198802a8d78bd444cd5a9197a78cb94f48e3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Jak toorun výpočetně náročných úloh v jazyce Java ve virtuálním počítači
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

S Azure můžete použít virtuální počítač toohandle náročné úlohy. Virtuální počítač můžete například zpracování úlohy a poskytovat výsledky tooclient počítače nebo mobilní aplikace. Po přečtení tohoto článku, budete mít představu o tom, jak toocreate virtuální počítač, který spouští aplikaci Java náročné, který se dá sledovat jinou aplikací Java.

Tento kurz předpokládá, že víte, jak toocreate konzolové aplikace Java, můžete importovat aplikaci Java tooyour knihovny a může generovat Java archivu (JAR). Předpokládají se žádné znalosti ve službě Microsoft Azure.

Co se dozvíte:

* Jak toocreate virtuální počítač s Java Development Kit (JDK) už nainstalovaná.
* Jak tooremotely přihlásit tooyour virtuálního počítače.
* Jak toocreate služby sběrnici oboru názvů.
* Jak toocreate aplikaci Java, která provede náročné úlohy.
* Jak toocreate aplikaci Java, která monitoruje hello průběh úlohy náročné hello.
* Jak toorun hello aplikací v jazyce Java.
* Jak toostop hello aplikací v jazyce Java.

V tomto kurzu použije hello Traveling prodejce problém pro hello výpočetně náročných úloh. Hello tady je příklad hello Java aplikace spuštěné hello náročné úlohy.

![Řešitel problém Traveling prodejce][solver_output]

Hello následuje příklad hello Java aplikace monitorování hello výpočetně náročných úloh.

![Problém Traveling prodejce klienta][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a>toocreate virtuálního počítače
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Klikněte na tlačítko **nový**, klikněte na tlačítko **výpočetní**, klikněte na tlačítko **virtuálního počítače**a potom klikněte na **z Galerie**.
3. V hello **vyberte bitovou kopii virtuálního počítače** dialogové okno, vyberte **JDK 7 Windows serveru 2012**.
   Všimněte si, že **JDK 6 systému Windows Server 2012** je k dispozici v případě, že máte starší aplikace, které ještě nejsou připraveny toorun v JDK 7.
4. Klikněte na **Další**.
5. V hello **konfigurace virtuálního počítače** dialogové okno:
   1. Zadejte název pro virtuální počítač hello.
   2. Zadejte velikost toouse hello hello virtuálního počítače.
   3. Zadejte název pro správce hello v hello **uživatelské jméno** pole. Název a hello heslo, které budou vedle zadejte si zapamatujte, je budou používat při vzdálené přihlášení toohello virtuálního počítače.
   4. Zadejte heslo do hello **nové heslo** pole a zadejte jej znovu v hello **potvrdit** pole. Toto je heslo účtu správce hello.
   5. Klikněte na **Další**.
6. V hello Další **konfigurace virtuálního počítače** dialogové okno:
   1. Pro **Cloudová služba**, použít výchozí hello **vytvořte novou cloudovou službu**.
   2. Hello hodnotu **název cloudové služby DNS** musí být jedinečný v rámci cloudapp.net. V případě potřeby upravte tuto hodnotu tak, že Azure označuje, že je jedinečný.
   3. Zadejte oblast, skupiny vztahů nebo virtuální sítě. Pro účely tohoto kurzu, zadejte v oblasti **západní USA**.
   4. Pro **účet úložiště**, vyberte **použít účet úložiště automaticky generované**.
   5. Pro **sadu dostupnosti**, vyberte **(None)**.
   6. Klikněte na **Další**.
7. V posledním hello **konfigurace virtuálního počítače** dialogové okno:
   1. Přijměte hello výchozí koncový bod položky.
   2. Klikněte na **Dokončit**.

## <a name="tooremotely-log-in-tooyour-virtual-machine"></a>tooremotely přihlášení tooyour virtuálního počítače
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Klikněte na tlačítko **virtuální počítače**.
3. Klikněte na název hello hello virtuálního počítače, který chcete toolog v.
4. Klikněte na **Připojit**.
5. Odpověď výzvy toohello jako potřebné tooconnect toohello virtuální počítač. Po zobrazení výzvy k hello správce jména a hesla, použijte hello hodnoty, které jste zadali při vytváření hello virtuálního počítače.

Všimněte si, že hello funkce Azure Service Bus vyžaduje toobe certifikát Baltimore CyberTrust Root hello nainstalované v rámci vašeho prostředí JRE **cacerts** uložit. Tento certifikát je automaticky součástí hello Java Runtime Environment (JRE) používané v tomto kurzu. Pokud jste tento certifikát ve vašem prostředí JRE **cacerts** úložiště najdete v tématu [přidání certifikátu toohello, úložiště certifikátů certifikační Autority Java] [ add_ca_cert] informace o přidání (a také informace o zobrazení hello certifikáty v úložišti cacerts).

## <a name="how-toocreate-a-service-bus-namespace"></a>Jak toocreate služby sběrnici obor názvů
fronty toobegin pomocí služby Service Bus v Azure, musíte nejdřív vytvořit obor názvů služby. Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

toocreate oboru názvů služby:

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. V levém navigačním podokně hello hello portál Azure classic, klikněte na tlačítko **Service Bus, řízení přístupu a ukládání do mezipaměti**.
3. V levém podokně hello hello portál Azure classic, klikněte na hello **Service Bus** uzel a potom klikněte na hello **nový** tlačítko.  
   ![Snímek obrazovky uzel Service Bus][svc_bus_node]
4. V hello **vytvoření nové služby Namespace** dialogovém okně zadejte **Namespace**, a potom klikněte na toomake se, že je jedinečný, **zkontrolovat dostupnost** tlačítko.  
   ![Vytvořit nový Namespace snímek][create_namespace]
5. Ověřte název oboru názvů hello je k dispozici, vyberte zemi nebo oblast, ve kterém by měl být hostován vašeho oboru názvů a klikněte na položku hello **vytvořit Namespace** tlačítko.  
   
   vytvořený obor názvů Hello se potom zobrazí v hello portál Azure classic a tooactivate chvíli trvá. Počkejte, dokud je stav hello **Active** než budete pokračovat v dalším kroku hello.

## <a name="obtain-hello-default-management-credentials-for-hello-namespace"></a>Získání hello výchozí pověření pro správu oboru názvů hello
Operace správy tooperform pořadí, jako je například vytváření fronty, na hello nový obor názvů je nutné tooobtain hello správy pověření pro obor názvů.

1. V levém navigačním podokně hello, klikněte na tlačítko hello **Service Bus** uzel k zobrazení hello seznam dostupných oborů názvů.
   ![K dispozici snímek obory názvů][avail_namespaces]
2. Vyberte obor názvů hello, který jste právě vytvořili, z v zobrazeném seznamu hello.
   ![Snímek obrazovky seznamu Namespace][namespace_list]
3. Hello pravém **vlastnosti** podokně zobrazí hello vlastnosti pro nový obor názvů.
   ![Snímek obrazovky podokna Vlastnosti][properties_pane]
4. Hello **výchozí klíč** skryt. Klikněte na tlačítko hello **zobrazení** tlačítko toodisplay hello zabezpečovací pověření.
   ![Výchozí klíč – snímek obrazovky][default_key]
5. Poznamenejte si hello **výchozí vystavitele** a hello **výchozí klíč** jako použije tyto informace níže tooperform operací s oborem názvů.

## <a name="how-toocreate-a-java-application-that-performs-a-compute-intensive-task"></a>Jak toocreate aplikaci Java, která provede náročné úlohy
1. Na počítači pro vývoj (který nemá toobe hello virtuální počítač, který jste vytvořili), stažení hello [Azure SDK pro jazyk Java](https://azure.microsoft.com/develop/java/).
2. Vytvořte konzolovou aplikaci Java pomocí hello ukázkový kód na konci hello v této části. V tomto kurzu použijeme **TSPSolver.java** jako název souboru hello Java. Upravit hello **vaše\_služby\_sběrnice\_obor názvů**, **vaše\_služby\_sběrnice\_vlastníka**a **vaše\_služby\_sběrnice\_klíč** toouse zástupné symboly služby service bus **obor názvů**, **výchozí vystavitele** a  **Výchozí klíč** hodnoty v uvedeném pořadí.
3. Po kódování, export hello aplikace tooa spustitelného Java archivu (JAR) a balíček hello požadované knihovny do hello generované JAR. V tomto kurzu použijeme **TSPSolver.jar** jako název JAR hello vygenerovat.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often tooprovide an update toohello console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as hello default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing toooccur other than creating hello queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing toooccur other than deleting hello queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume hello value passed in is hello number of cities toosolve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-toocreate-a-java-application-that-monitors-hello-progress-of-hello-compute-intensive-task"></a>Jak toocreate aplikaci Java, která monitoruje hello průběh úlohy náročné hello
1. Na vývojovém počítači vytvořte konzolovou aplikaci Java pomocí hello ukázkový kód na konci hello v této části. V tomto kurzu použijeme **TSPClient.java** jako název souboru hello Java. Jak je uvedeno výše, upravte hello **vaše\_služby\_sběrnice\_obor názvů**, **vaše\_služby\_sběrnice\_vlastníka**, a **vaše\_služby\_sběrnice\_klíč** toouse zástupné symboly služby service bus **obor názvů**, **výchozí vystavitele**a **výchozí klíč** hodnoty v uvedeném pořadí.
2. Export tooa aplikace hello knihovny do hello generované JAR vyžaduje spustitelného JAR a balíček hello. V tomto kurzu použijeme **TSPClient.jar** jako název JAR hello vygenerovat.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as hello default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display hello queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing toooccur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // hello queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-toorun-hello-java-applications"></a>Jak toorun hello aplikací v jazyce Java
Spuštění aplikace hello náročné, první toocreate hello fronty a potom toosolve hello cestě Saleseman problém, který přidá hello aktuální nejlepší trasy toohello frontou service bus. Při hello náročné aplikace je spuštěná (nebo později) spusťte hello klienta toodisplay výsledky z hello frontou service bus.

### <a name="toorun-hello-compute-intensive-application"></a>toorun hello náročné aplikace
1. Přihlaste se tooyour virtuálního počítače.
2. Vytvořte složku, kde budete spouštět aplikace. Například **c:\TSP**.
3. Kopírování **TSPSolver.jar** příliš**c:\TSP**,
4. Vytvořte soubor s názvem **c:\TSP\cities.txt** s hello následující obsah.
   
        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33
5. Na příkazovém řádku změňte adresáře tooc:\TSP.
6. Zkontrolujte, zda je složka bin hello JRE v proměnné prostředí PATH hello.
7. Před spuštěním hello TSP solver permutací, budete potřebovat frontou service bus toocreate hello. Spusťte následující příkaz toocreate hello service bus fronty hello.
   
        java -jar TSPSolver.jar createqueue
8. Je teď vytvořená hello fronty, můžete spustit hello TSP solver permutací. Například spusťte následující příkaz toorun hello solver pro 8 města hello.
   
        java -jar TSPSolver.jar 8
   
   Pokud nezadáte číslo, bude spuštěna pro 10 měst. Jako hello solver vyhledá aktuální nejkratší tras, bude je přidat toohello fronty.

> [!NOTE]
> číslo, kterou zadáte, se spustí hello delší hello solver text hello Hello větší. Například běžet 14 města může trvat několik minut a spuštěna 15 města může trvat několik hodin. Zvýšení too16 nebo více měst může mít za následek dnů modulu runtime (nakonec týdny, měsíce a letopočty). Toto je z důvodu toohello rychlý nárůst hello počet permutací vyhodnotit v Řešiteli hello jako hello počet zvyšuje měst.
> 
> 

### <a name="how-toorun-hello-monitoring-client-application"></a>Jak toorun hello monitorování klientské aplikace
1. Přihlaste se na tooyour počítači, kde budete spouštět hello klientské aplikace. To není nutné toobe hello stejný počítač s hello **TSPSolver** aplikace, i když může být.
2. Vytvořte složku, kde budete spouštět aplikace. Například **c:\TSP**.
3. Kopírování **TSPClient.jar** příliš**c:\TSP**,
4. Zkontrolujte, zda je složka bin hello JRE v proměnné prostředí PATH hello.
5. Na příkazovém řádku změňte adresáře tooc:\TSP.
6. Spusťte následující příkaz hello.
   
        java -jar TSPClient.jar
   
    Volitelně zadejte hello počet minut toosleep mezi kontroly fronty hello předáním v argument příkazového řádku. Hello výchozí režim spánku období kontroly fronty hello je 3 minuty, který se používá, pokud je příliš předán žádný argument příkazového řádku**TSPClient**. Pokud chcete toouse jinou hodnotu pro interval hello režimu spánku, například jednu minutu, spusťte hello následující příkaz.
   
        java -jar TSPClient.jar 1
   
    Hello klienta se spustí, dokud ho uvidí zprávu fronty služby "Dokončeno". Všimněte si, že pokud spustíte vícenásobné výskyty hello solver bez spuštění klienta hello, může být nutné toorun hello klienta několikrát toocompletely prázdný hello fronty. Alternativně můžete odstranit hello fronty a poté jej znovu vytvořit. toodelete hello fronty, spusťte následující hello **TSPSolver** (ne **TSPClient**) příkaz.
   
        java -jar TSPSolver.jar deletequeue
   
    Řešitel Hello se spustí, dokud neskončí zkoumání všechny trasy.

## <a name="how-toostop-hello-java-applications"></a>Jak toostop hello aplikací v jazyce Java
Pro hello solver a klientské aplikace, můžete stisknout **Ctrl + C** tooexit, pokud chcete, aby tooend předchozí toonormal dokončení.

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
