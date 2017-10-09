Některé balíčky se při spuštění v Azure nemusí pomocí systému pip nainstalovat.  Může být tento balíček hello není k dispozici na hello indexu balíčků Pythonu.  Může být, že je požadován kompilátor (kompilátor není k dispozici na hello počítač běžící hello webové aplikace v Azure App Service).

V této části se podíváme způsoby toodeal s tímto problémem.

### <a name="request-wheels"></a>Vyžádání souborů wheel
Pokud hello instalace balíčku vyžaduje kompilátor, pokuste se kontaktovat vlastníka toorequest balíček hello, která souborů Wheel dostupná pro balíček hello.

S nedávno vydanému hello [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], je teď jednodušší toobuild balíčky, které mají nativní kód pro Python 2.7.

### <a name="build-wheels-requires-windows"></a>Sestavení souborů wheel (vyžaduje Windows)
Poznámka: Pokud použijete tuto možnost, ujistěte se, že balíček hello toocompile pomocí prostředí Python, který odpovídá hello platformě, architektuře a verzi, která se používá v hello webové aplikace v Azure App Service (Windows/32-bit/2.7 nebo 3.4).

Pokud hello balíček nenainstaluje, protože vyžaduje kompilátor, můžete nainstalovat hello kompilátoru na místním počítači a sestavit wheel hello balíčku, který potom zahrnete do úložiště.

Uživatelé Mac/Linux: Pokud nemáte počítač Windows tooa přístup, zobrazit [vytvoření virtuálního počítače s Windows] [ Create a Virtual Machine Running Windows] jak toocreate virtuálního počítače na platformě Azure.  Můžete použít souborů Wheel toobuild hello, přidejte je toohello úložiště a pokud chcete zahodit hello virtuálních počítačů. 

Python 2.7 můžete nainstalovat [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].

Pro Python 3.4 můžete nainstalovat [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].

toobuild souborů Wheel, budete potřebovat balíček wheel hello:

    env\scripts\pip install wheel

Budete používat `pip wheel` toocompile závislost:

    env\scripts\pip wheel azure==0.8.4

Tím se vytvoří soubor .whl ve složce \wheelhouse hello.  Přidáte složku \wheelhouse hello a kolečka soubory tooyour úložiště.

Upravit vaše hello tooadd requirements.txt `--find-links` možnost v horní části hello. Tato hodnota informuje pip toolook pro přesnou shodu v místní složce hello před indexu balíčků pythonu toohello probíhající.

    --find-links wheelhouse
    azure==0.8.4

Pokud chcete tooinclude všechny svoje závislosti hello \wheelhouse složku a nepoužije hello python package index vůbec, můžete vynutit indexu balíčků hello tooignore pip přidáním `--no-index` toohello začátek souboru requirements.txt.

    --no-index

### <a name="customize-installation"></a>Přizpůsobení instalace
Můžete přizpůsobit hello nasazení skriptu tooinstall balíček ve virtuálním prostředí hello pomocí alternativního instalačního programu, například easy\_nainstalovat.  Příklad tohoto postupu označený jako komentář najdete v souboru deploy.cmd.  Ujistěte se, že tyto balíčky nejsou uvedené v souboru requirements.txt, tooprevent pip instalaci je.

Přidejte tento skript nasazení toohello:

    env\scripts\easy_install somepackage

Může být také možné toouse snadno\_tooinstall nainstalovat z instalačního programu exe (některé jsou zip kompatibilní, takže je easy\_install podporuje).  Přidejte úložiště tooyour hello instalačního programu a spusťte easy\_nainstalovat předáním toohello hello cestu spustitelného souboru.

Přidejte tento skript nasazení toohello:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-hello-virtual-environment-in-hello-repository-requires-windows"></a>Zahrnout virtuální prostředí hello hello úložiště (vyžaduje Windows)
Poznámka: Pokud použijete tuto možnost, ujistěte se, že toouse virtuální prostředí, které odpovídá hello platformě, architektuře a verzi, která se používá v hello webové aplikace v Azure App Service (Windows/32-bit/2.7 nebo 3.4).

Pokud zahrnete virtuální prostředí hello hello úložiště, můžete skript nasazení hello zabránit správě virtuálního prostředí v Azure vytvořením prázdného souboru:

    .skipPythonDeployment

Doporučujeme odstranit existující virtuální prostředí v aplikaci hello zanechání nepotřebných souborů z tooprevent hello, pokud bylo hello virtuální prostředí spravováno automaticky.

[Create a Virtual Machine Running Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler for Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
