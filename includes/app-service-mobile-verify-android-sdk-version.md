Z důvodu pokračujícího vývoje hello verze sady SDK pro Android nainstalovaná v Android Studio nemusí odpovídat verzi hello v kódu hello. Hello Android SDK, kterou se odkazuje v tomto kurzu je verze 23, hello nejnovější v době psaní hello. jako nové verze hello SDK zobrazí může zvýšit číslo verze Hello a doporučujeme používat nejnovější verzi hello k dispozici.

Dvěma příznaky neshoda verze jsou:

- Při vytvoření nebo znovu sestavte projekt hello, může dojít k Gradle chybové zprávy jako "**se nezdařilo toofind cíl Google Inc.:Google APIs:n**".
- Standardní Android objekty v kódu, který by měl směrovat na základě `import` příkazy může být generování chybové zprávy.

Pokud se zobrazí některá z nich, nemusí odpovídat hello verzi hello sady SDK pro Android nainstalovaná v Android Studio hello SDK cíl hello stáhli projektu. verze hello tooverify, ujistěte se, hello následující změny:

1. V Android Studio, klikněte na **nástroje** > **Android** > **SDK Manager**. Pokud jste nenainstalovali hello nejnovější verzi hello SDK platformy, pak klikněte na tooinstall ho. Poznamenejte si číslo verze hello.
2. Na hello **Project Exploreru** v části **Gradle skripty**, otevřete soubor hello **build.gradle (modeule: aplikace)**. Ujistěte se, že hello **compileSdkVersion** a **buildToolsVersion** jsou nastaveny toohello SDK nainstalovanou nejnovější verzi. značky Hello může vypadat například takto:

             compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
3. V hello Android Studio Project Exploreru, klikněte pravým tlačítkem na uzel projektu hello, zvolte **vlastnosti**a v levém sloupci hello zvolte **Android**. Ujistěte se, že hello **cíl sestavení projektu** nastavena toohello stejné verze sady SDK jako hello **targetSdkVersion**.

V nástroji Android Studio hello soubor manifestu je už použitých toospecify hello cíle SDK a minimální verze SDK, na rozdíl od případu hello se Eclipse.
