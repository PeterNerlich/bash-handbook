# bash-Handbuch

[![CC 4.0][cc-image]][cc-url]
[![NPM version][npm-image]][npm-url]
[![Gitter][gitter-image]][gitter-url]

Dieses Dokument wurde für die geschrieben, die Bash lernen wollen, ohne zu tief einzusteigen.

> **Tipp**: Probieren Sie [**learnyourbash**](https://git.io/learnyoubash) — ein interaktiver Workshop, der auf diesem Handbuch basiert!

# Installation in Node

Dieses Handbuch kann mit `npm` einfach über diesen Befehl installiert werden:

```
$ npm install -g bash-handbook
```

Jetzt sollte sich `bash-handbook` in der Kommandozeile ausführen lassen. Das wird das Handbuch im ausgewählten `$PAGER` öffnen. Ansonsten kann natürlich hier weitergelesen werden.

Die Quelle ist hier verfügbar: <https://github.com/denysdovhan/bash-handbook>

# Inhalt

- [Einleitung](#einleitung)
- [Shells und Modi](#shells-und-modi)
  - [Interaktiv](#interaktiv)
  - [Nicht-interaktiv](#nicht-interaktiv)
  - [Exit-Codes](#exit-codes)
- [Kommentare](#kommentare)
- [Variablen](#variablen)
  - [Lokale Variablen](#lokale-variablen)
  - [Umgebungsvariablen](#umgebungs-variablen)
  - [Positionale Parameter](#positionale-parameter)
- [Shell-Erweiterungen](#shell-erweiterungen)
  - [Klammerausdehnung](#klammerausdehnung)
  - [Befehlssubstitution](#command-substitution)
  - [Arithmetische Erweiterung](#arithmetische-erweiterung)
  - [Doppelte und einfache Hochkommata](#doppelte-und-einfache-hochkommata)
- [Listen](#listen)
  - [Listendeklaration](#listendeklaration)
  - [Listenerweiterung](#listenerweiterungen)
  - [Array slice](#array-slice)
  - [Elemente zu einer Liste hinzufügen](#elemente-zu-einer-liste-hinzufügen)
  - [Elemente aus einer Liste löschen](#elemente-aus-einer-liste-löschen)
- [Streams, pipes and lists](#streams-pipes-and-lists)
  - [Streams](#streams)
  - [Pipes](#pipes)
  - [Lists of commands](#lists-of-commands)
- [Konditionale Aussagen](#konditionale-ausdrücke)
  - [Primäre und kombinierende Ausdrücke](#primäre-und-kombinierende-ausdrücke)
  - [Den `if`-Ausdruck benutzen](#den-if-ausdruck-benutzen)
  - [Den `case`-Ausdruck benutzen](#den-case-ausdruck-benutzen)
- [Schleifen](#scleifen)
  - [`for`-Schleife](#for-schleife)
  - [`while`-Schleife](#while-schleife)
  - [`until`-Schleife](#until-schleife)
  - [`select`-Schleife](#select-schleife)
  - [Loop control](#loop-control)
- [Funktionen](#funktionen)
- [Debugging](#debugging)
- [Nachwort](#nachwort)
- [Weitere Ressourcen](#weitere-ressourcen)
- [Lizenz](#lizenz)

# Einleitung

Wenn Sie ein Entwickler sind, wissen Sie, wie wertvoll Zeit ist. Den Arbeitsprozess zu optimieren ist einer der wichtigsten Aspekte dieses Berufes.

Auf diesem Weg der Effizienz und Produktivität begegnen uns oft Schritte, die wieder und wieder wiederholt werden müssen, wie z.B.:

* Bildschirmfoto schießen und auf Server hochladen
* Text verarbeiten, der in vielen verschiedenen Formen und Formaten kommen kann
* Dateien in andere Formate kovertieren
* Programmausgabe analysieren

Betrete **Bash**, unseren Retter.

Bash ist eine Unix-Shell, geschrieben von [Brian Fox][] für das GNU Projekt als ein freier Software-Ersatz für die [Bourne Shell](https://en.wikipedia.org/wiki/Bourne_shell). Es wurde 1989 herausgegeben und wurde seit langer Zeit als die Standard-Shell für Linux und macOS verbreitet.

[Brian Fox]: https://en.wikipedia.org/wiki/Brian_Fox_(computer_programmer)
<!-- link this format, because some MD processors handle '()' in URLs poorly -->

Aber warum müssen wir etwas lernen, das vor mehr als 30 Jahren geschrieben wurde? Die Antwort ist einfach: dieses _Etwas_ ist heute eins der mächtigsten und portablen Werkzeuge, effiziente Skripte für alle Unix-basierten Systeme zu schreiben. Und deswegen sollten Sie Bash lernen. Punkt.

In diesem Handbuch werde ich die wichtigsten Konzepte in Bash mit Beispielen beschreiben. Ich hoffe, dass dieses Kompendium Ihnen hilfreich sein wird.

# Shells und Modi

Die Nutzer Bash Shell kann in zwei Modi arbeiten — interaktiv und nicht-interaktiv.

## Interaktiv

Wenn Sie auf Ubuntu arbeiten, werden Sie sieben virtuelle Terminals für Sie zur Verfügung gesehen haben.
Die Benutzerumgebung findet im siebenten virtuellen Terminal statt, somit können Sie zu einer freundlichen grafischen Umgebung mit der Tastenkombination `Ctrl-Alt-F7` zurückkehren.

Die Shell können Sie mit der Tastenkombination `Ctrl-Alt-F1` öffnen. Danach wird die bekannte GUI verschwinden und eines der virtuellen Terminals wird angezeigt werden.

Wenn Sie so etwas ähnliches sehen, sind Sie im interaktiven Modus:

    benutzer@host:~$

Hier können Sie eine Vielzahl von Unix-Befehlen eingeben, z.B. `ls`, `grep`, `cd`, `mkdir`, `rm`, und das Ergebnis Ihrer Ausführung betrachten.

Wir nennen diese Shell interaktiv, weil sie direkt mit dem Nutzer interagiert.

Ein virtuelles Terminal zu nutzen ist nicht wirklich bequem. Zum Beispiel, wenn Sie ein Dokument bearbeiten und gleichzeitig einen anderen Befehl ausführen wollen, nutzen Sie besser virtual-Terminal-Emulatoren wie:

- [GNOME Terminal](https://de.wikipedia.org/wiki/GNOME_Terminal)
- [Terminator](https://en.wikipedia.org/wiki/Terminator_(terminal_emulator))
- [iTerm2](https://en.wikipedia.org/wiki/ITerm2)
- [ConEmu](https://en.wikipedia.org/wiki/ConEmu)

## Nicht-interaktiv

Im nicht-interaktiven Modus liest die Shell Befehle aus einer Datei oder Leitung und führt sie aus. Wenn der Interpreter das Ende der Datei erreicht, beendet der Shell-Prozess die Sitzung und kehrt zum Elternprozess zurück.

Die folgenden Befehle werden zur Ausführung der Shell im nicht-interaktiven Modus genommen:

    sh /pfad/zum/skript.sh
    bash /pfad/zum/skript.sh

Im obigen Beispiel ist `script.sh` nur eine gewöhnliche Textdatei, die aus Befehlen besteht, welche der Shell-Interpreter auswerten kann und `sh` oder `bash` ist das Interpreterprogramm der Shell. `script.sh` kann mit Ihrem bevorzugten Texteditor erstellt werden (z.B. vim, nano, Sublime Text, Atom, usw.).

Den Aufruf des Skriptes kann über folgenden Befehl auch vereinfacht werden, indem es mit `chmod` als ausführbar markiert wird:

    chmod +x /pfad/zum/skript.sh

Zusätzlich muss die erste Zeile des Skripts das Programm mitteilen, mit dem es ausgeführt werden soll:

```bash
#!/bin/bash
echo "Hallo Welt!"
```

Oder wenn `sh` dem `bash` vorgezogen wird, ändern Sie `#!/bin/bash` zu `#!/bin/sh`. Das `#!` wird [shebang](http://en.wikipedia.org/wiki/Shebang_%28Unix%29) genannt. Jetzt kann das Skript wie folgt ausgeführt werden:

    /pfad/zum/skript.sh

Ein handlicher Trick ist, `echo` zu benutzen, um Text im Terminal auszugeben.

Eine weitere Möglichkeit, die shebang-Zeile einzusetzen lautet folgendemaßen:

```bash
#!/usr/bin/env bash
echo "Hallo Welt!"
```

Der Vorteil dieser shebang-Zeile ist, dass es nach dem Programm (in diesem Fall `bash`) auf der `PATH`-Umgebungsvariable basiert suchen wird. Das ist oft der erstgenannten Methode vorzuziehen, da der Ort des Programms im Dateisystem nicht immer angenommen werden kann. Es ist auch hilfreich, wenn die `PATH`-Variable eines Systems darauf konfiguriert wurde, auf eine alternative Version des Programms zu weisen. Zum Beispiel könnte jemand eine neuere Version von `bash` installieren, das Original aber behalten und den Ort der neuen Version einfach in die `PATH`-Variable einfügen. `#!/bin/bash` würde dann das original `bash` nutzen, während `#!/usr/bin/env bash` die neuere Version verwenden würde.


## Exit-Codes

Jeder Befehl gibt einen **Exit-Code** (**return status** oder **exit status**) zurück. Ein erfolgreicher Befehl wird immer `0` (zero-code) zurückkgeben, während ein fehlgeschlagener Befehl einen Wert ungleich 0 zurückgibt (error code). Diese Fehlercodes müssen positive Ganzzahlen zwischen 1 und 255 sein.

Ein weiterer nützlicher Befehl, den wir beim Schreiben von Skripten verwenden können, lautet `exit`. Dieser Befehl beendet die aktuelle Ausführung und gibt einen Exit-Code an die Shell zurück. `exit` ohne folgende Argumente wird das laufende Skript abbrechenund den Exit-Code des im Skript zuletzt ausgeführten Befehls vor `exit` zurückgeben.

Wenn sich ein Programm beendet, ordnet die Shell den **Exit-Code** der `$?`Umgebungsvariable zu. Die `$?`-Variable ist der typische Weg, den Erfolg eines Skriptes abzufragen.

So wie wir `exit` nutzen können, um ein Skript zu beenden, wird der `return`-Befehl zum Beenden einer Funktion eingesetzt und gibt einen **Exit-Code** an den Aufruf zurück. Innerhalb einer Funktion kann auch `exit` benutzt werden und es wird die Funktion _und_ das Programm beenden.

# Kommentare

Skripte können _Kommentare_ enthalten. Kommentare sind spezielle Aussagen, die von dem `shell`-Interpreter ignoriert werden. Sie beginnen mit dem Symbol `#` und gehen bis zum Ende der Zeile.

Zum Beispiel:

```bash
#!/bin/bash
# Dieses Skript wird den Benutzernamen ausgeben.
whoami
```

> **Tipp**: Nutzen Sie Kommentare, um zu erklären, was das Skript macht und _warum_.

# Variablen

Wie in den meisten Programmiersprachen können auch in bash Variablen gesetzt werden.

Bash kennt keine Datentypen. Variablen können nur Zahlen oder Zeichenketten der Länge 1 oder mehr enthalten. Es gibt drei Arten von Variablen: lokale Variablen, Umgebungsvariablen und Variablen als _Positionsargumente_.

## Lokale Variablen

**Lokale Variablen** sind Variablen, die nur in einem einzelnen Skript existieren. Für andere Programme und Skripte sind sie nicht zugänglich.

Eine lokale Variable kann mit dem Zeichen `=` deklariert werden (als Regel, es **dürfen keine** Leerzeichen zwischen dem Namen der Variable, dem `=` und dem Wert stehen) und ihr Wert kann mit dem `$`-Zeichen abgefragt werden. Zum Beispiel:

```bash
benutzername="denysdovhan"  # Variable deklarieren
echo $benutzername          # Wert anzeigen
unset benutzername          # Variable löschen
```

Eine Variable kann auch für nur in einer Funktion zugänglich deklariert werden, über da Schlüsselwort `local`. Dadurch verschwindet die Variable, subald die Funktion beendet wird.

```bash
local local_var="Ich bin ein lokaler Wert"
```

## Umgebungsvariablen

**Umgebungsvariablen** sind allen Programmen oder Skripten, die in einer gemeinsamen Shell-Sitzung laufen, zugänglich. Sie werden wie lokale Variablen erstellt, nur mit dem Schlüsselwort `export`.

```bash
export GLOBAL_VAR="Ich bin eine globale Variable"
```

Es gibt _ziemlich viele_ globale Variablen in Bash. Darauf werden Sie häufig stoßen, darum hier eine schnelle Übersicht über die nützlichsten:

| Variable     | Beschreibung                                                  |
| :----------- | :------------------------------------------------------------ |
| `$HOME`      | Das Home-Verzeichnis des aktuellen Nutzers.                   |
| `$PATH`      | Eine Komma-separierte Liste von Verzeichnissen, in denen die Shell nach Befehlen sucht. |
| `$PWD`       | Das aktuelle Arbeitsverzeichnis.                              |
| `$RANDOM`    | Zufällige Ganzzahl zwischen 0 und 32767.                      |
| `$UID`       | Die numerische, echte Nutzer-ID des aktuellen Nutzers.        |
| `$PS1`       | The primary prompt string.                                    |
| `$PS2`       | The secondary prompt string.                                  |

Unter [diesem Link](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_02.html#sect_03_02_04) finden Sie eine erweiterte Liste von Umgebungsvariablen in Bash.

## Positionale Parameter

**Positionale Parameter** sind Variablen, die beim Aufruf einer Funktion zugeordnet werden und werden nach Position vergeben. Die folgende Tabelle zählt die Variablen der positionellen Parameter, andere spezielle Variablen und ihre Bedeutung innerhalb der Funktion auf.

| Parameter      | Beschreibung                                                |
| :------------- | :---------------------------------------------------------- |
| `$0`           | Name des Skriptes.                                          |
| `$1 … $9`      | Der Parameter zählt die Elemente von 1 bis 9 auf.           |
| `${10} … ${N}` | Der Parameter zählt die Elemente von 10 bis N auf.          |
| `$*` or `$@`   | Alle positionalen Parameter, außer `$0`.                    |
| `$#`           | Die Anzahl der Parameters, ohne `$0`.                       |
| `$FUNCNAME`    | Der Funktionsname (nur innerhalb einer Funktion belegt).    |

Im untenstehenden Beispiel werden die positionalen Parameter `$0='./script.sh'`,  `$1='foo'` und `$2='bar'` sein:

    ./script.sh foo bar

Variablen können auch _Standardwerte_ besitzen. Diese können mit folgender Syntax definiert werden:

```bash
 # falls die Variablen leer sind, Standardwert zuweisen
: ${VAR:='standard'}
: ${$1:='erste'}
# or
FOO=${FOO:-'standard'}
```

# Shell-Erweiterungen

_Erweiterungen_ werden auf die Kommandozeile angewendet, nachdem sie in _Zeichen_ geteilt wurde. In anderen Worten sind diese Erweiterungen ein Mechanismus, arithmetische Operationen zu berechnen, um die Ergebnisse der Ausführungen der Kommandos zu speichern und so weiter.

Wenn Sie interessiert sind, können Sie [hier mehr über Shell-Erweiterungen lesen](https://www.gnu.org/software/bash/manual/bash.html#Shell-Expansions).

## Klammerausdehnung

Die Klammerausdehnung erlaubt uns, beliebige Zeichenketten zu erzeugen. Es ähnelt der _Dateinamenerweiterung_. Zum Beispiel:

```bash
echo beg{i,a,u}n # begin began begun
```

Klammerausdehnungen können auch benutzt werden, um Folgen zu definieren, durch die in Schleifen iteriert wird.

```bash
echo {0..5} # 0 1 2 3 4 5
echo {00..8..2} # 00 02 04 06 08
```

## Befehlssubstitution

Die Befehlssubstitution erlaubt uns, einen Befehl auszuwerten und seinen Wert in einen anderen Befehl oder Variablenbelegung zu setzen. Sie wird ausgeführt, wenn der Befehl von ``` `` ``` oder `$()` umschlossen ist. Das kann beispielsweise wie folgt genutzt werden:

```bash
now=`date +%T`
# or
now=$(date +%T)

echo $now # 19:08:26
```

## Arithmetische Erweiterung

In bash können wir jede arithmetische Operation anwenden. Der Ausdruck muss allerdings durch `$(( ))` eingeschlossen sein. Das Format arithmetischer Erweiterungen ist:

```bash
ergebnis=$(( ((10 + 5*3) - 7) / 2 ))
echo $ergebnis # 9
```

Innerhalb einer arithmetischen Erweiterung, Variablen sollten für gewöhnlich ohne `$`–Präfix stehen:

```bash
x=4
y=7
echo $(( x + y ))     # 11
echo $(( ++x + y++ )) # 12
echo $(( x + y ))     # 13
```

## Doppelte und einfache Anführungszeichen

Es gibt einen wichtigen Unterschied zwischen dopplten und einfachen Anführungszeichen. In doppelten Anführungszeichen werden Variablen oder Befehlssubstitutionen ausgedehnt, in einfachen nicht. Zum Beispiel:

```bash
echo "Dein home-Verzeichnis: $HOME" # Dein home-Verzeichnis: /Users/<username>
echo 'Dein home-Verzeichnis: $HOME' # Dein home-Verzeichnis: $HOME
```

Vorsicht beim Ausdehnen lokaler Variablen und Umgebungsvariablen in Anführungszeichen, sollten sie Leerzeichen enthalten. Als harmloses Beispiel, Benutzereingaben mit `echo` ausgeben:

```bash
INPUT="Ein String  mit   seltsamen    Leerzeichen."
echo $INPUT   # Ein String mit seltsamen Leerzeichen.
echo "$INPUT" # Ein String  mit   seltsamen    Leerzeichen.
```

Das erste `echo` wird mit 5 separaten Argumenten aufgerufen — $INPUT wird in separate Worte aufgeteilt, zwischen denen `echo` nur ein einziges Leerzeichen ausgibt. Im zweiten Fall wird `echo` mit einem einzigen Argument aufgerufen (der ganze $INPUT–Wert, inklusive Leerzeichen).

Nun ein ernsteres Beispiel:

```bash
DATEI="Coole Sachen.txt"
cat $DATEI   # versucht, zwei Dateien auszugeben: `Coole` und `Sachen.txt`
cat "$DATEI" # gibt 1 Datei aus: `Coole Sachen.txt`
```

Während das Problem in diesem Beispiel aus dem Weg geräumt werden könnte, in dem die Datei zu `Coole-Sachen.txt` umbenannt wird, könnte die Eingabe auch über eine Umgebungsvariable, einen positionalen Parameter oder die Ausgabe eines anderen Befehls erfolgen (`find`, `cat`, usw). Wenn die Eingabe Leerzeichen ennthalten *könnte*, ist Vorsicht beim umranden mit Hochkommata geboten.

# Listen

Wie in anderen Programmiersprachen auch ist eine Liste in Bash eine Variable, die es erlaubt, auf mehrere Werte zuzugreifen. In Bash sind Listen auch Null-basiert, das bedeutet, dass das erste Element den Index 0 hat.

Beim Arbeiten mit Listen muss auf die besondere Umgebungsvariable `IFS` geachtet werden. **IFS**, oder **Input Field Separator (Eingabefeldseparator)** ist das Zeichen, das die Element einer Liste trennt. Der Standardwert dafür ist das Leerzeichen `IFS=' '`.

## Listendeklaration

In Bash werden Listen durch einfaches Zuweisen eines Wertes zu einem Index der Listenvariable deklariert:

```bash
fruechte[0]=Apfel
fruechte[1]=Birne
fruechte[2]=Pflaume
```

Listenvariablen können auch durch Verbundzuweisungen erstellt werden:

```bash
fruechte=(Apfel Birne Pflaume)
```

## Listenerweiterung

Einzelne Listenelemente können, anderen Variablen ähnlich, erweitert werden:

```bash
echo ${fruechte[1]} # Birne
```

Die ganze Liste kann mit `*` oder `@` statt des numerischen Indexes erweitert werden:

```bash
echo ${fruechte[*]} # Apfel Birne Pflaume
echo ${fruechte[@]} # Apfel Birne Pflaume
```

Es gibt einen wichtigen (und unscheinbaren) Unterschied zwischen den beiden Zeilen: Mit einer Liste, die Leerzeichen enthält:

```bash
fruechte[0]=Apfel
fruechte[1]="Abb: Nachtisch"
fruechte[2]=Pflaume
```

Jedes Element soll auf einer neuen Zeile ausgegeben werden, also versuchen wir, `printf` zu nutzen:

```bash
printf "+ %s\n" ${fruechte[*]}
# + Apfel
# + Abb:
# + Nachtisch
# + Pflaume
```

Warum wurden `Abb:` und `Nachtisch` auf verschiedenen Zeilen ausgegeben? Noch ein Versuch, mit Hochkommata:

```bash
printf "+ %s\n" "${fruechte[*]}"
# + Apfel Abb: Nachtisch Pflaume
```

Jetzt ist alles wieder auf einer einzigen Zeile — das wollten wir aber nicht! Hier kommt `${fruits[@]}` ins Spiel:

```bash
printf "+ %s\n" "${fruechte[@]}"
# + Apfel
# + Abb: Nachtisch
# + Pflaume
```

In doppelten Hochkommata erweitert `${fruits[@]}` in ein separates Argument für jedes Element in der Liste; Leerzeichen in den Listenelementen bleiben erhalten.

## Listen teilen

Außerdem können wir einen Teil der Liste mit dem _slice_–Operator extrahieren:

```bash
echo ${fruechte[@]:0:2} # Apfel Abb: Nachtisch
```

Im obigen Beispiel erweitert `${fruechte[@]}` zum gesamten Inhalts der Liste und `:0:2` extrahiert den Teil der Länge 2, der am Index 0 beginnt.

## Elemente einer Liste hinzufügen

Elemente hinzuzufügen ist auch zimlich einfach. Verbundzuweisungen sind in diesem Fall besonders hilfreich. Sie können folgendermaßen genutzt werden:

```bash
fruechte=(Orange "${fruechte[@]}" Banane Kirsche)
echo ${fruechte[@]} # Orange Apfel Abb: Nachtisch Pflaume Banane Kirsche
```

Im obigen Beispiel erweitert `${fruechte[@]}` zum gesamten Inhalt der Liste und setzt es in eine Verbundzuweisung, weist den neuen Wert dann der `fruechte`–Liste zu, was ihren ursprünglichen Wert verändert.

## Elemente aus einer Liste löschen

Zum löschen eines Elements wird der `unset`–Befehl genutzt:

```bash
unset fruechte[0]
echo ${fruechte[@]} # Apfel Abb: Nachtisch Pflaume Banane Kirsche
```

# Streams, pipes and lists

Bash has powerful tools for working with other programs and their outputs. Using streams we can send the output of a program into another program or file and thereby write logs or whatever we want.

Pipes give us opportunity to create conveyors and control the execution of commands.

It is paramount we understand how to use this powerful and sophisticated tool.

## Streams

Bash receives input and sends output as sequences or **streams** of characters. These streams may be redirected into files or one into another.

There are three descriptors:

| Code | Descriptor | Description          |
| :--: | :--------: | :------------------- |
| `0`  | `stdin`    | The standard input.  |
| `1`  | `stdout`   | The standard output. |
| `2`  | `stderr`   | The errors output.   |

Redirection makes it possible to control where the output of a command goes to, and where the input of a command comes from. For redirecting streams these operators are used:

| Operator | Description                                  |
| :------: | :------------------------------------------- |
| `>`      | Redirecting output                           |
| `&>`     | Redirecting output and error output          |
| `&>>`    | Appending redirected output and error output |
| `<`      | Redirecting input                            |
| `<<`     | [Here documents](http://tldp.org/LDP/abs/html/here-docs.html) syntax |
| `<<<`    | [Here strings](http://www.tldp.org/LDP/abs/html/x17837.html) |

Here are few examples of using redirections:

```bash
# output of ls will be written to list.txt
ls -l > list.txt

# append output to list.txt
ls -a >> list.txt

# all errors will be written to errors.txt
grep da * 2> errors.txt

# read from errors.txt
less < errors.txt
```

## Pipes

We could redirect standard streams not only in files, but also to other programs. **Pipes** let us use the output of a program as the input of another.

In the example below, `command1` sends its output to `command2`, which then passes it on to the input of `command3`:

    command1 | command2 | command3

Constructions like this are called **pipelines**.

In practice, this can be used to process data through several programs. For example, here the output of `ls -l` is sent to the `grep` program, which  prints only files with a `.md` extension, and this output is finally sent to the `less` program:

    ls -l | grep .md$ | less

The exit status of a pipeline is normally the exit status of the last command in the pipeline. The shell will not return a status until all the commands in the pipeline have completed. If you want your pipelines to be considered a failure if any of the commands in the pipeline fail, you should set the pipefail option with:

    set -o pipefail

## Lists of commands

A **list of commands** is a sequence of one or more pipelines separated by `;`, `&`, `&&` or `||` operator.

If a command is terminated by the control operator `&`, the shell executes the command asynchronously in a subshell. In other words, this command will be executed in the background.

Commands separated by a `;` are executed sequentially: one after another. The shell waits for the finish of each command.

```bash
# command2 will be executed after command1
command1 ; command2

# which is the same as
command1
command2
```

Lists separated by `&&` and `||` are called _AND_ and _OR_ lists, respectively.

The _AND-list_ looks like this:

```bash
# command2 will be executed if, and only if, command1 finishes successfully (returns 0 exit status)
command1 && command2
```

The _OR-list_ has the form:

```bash
# command2 will be executed if, and only if, command1 finishes unsuccessfully (returns code of error)
command1 || command2
```

The return code of an _AND_ or _OR_ list is the exit status of the last executed command.

# Conditional statements

Like in other languages, Bash conditionals let us decide to perform an action or not.  The result is determined by evaluating an expression, which should be enclosed in `[[ ]]`.

Conditional expression may contain `&&` and `||` operators, which are _AND_ and _OR_ accordingly. Besides this, there many [other handy expressions](#primary-and-combining-expressions).

There are two different conditional statements: `if` statement and `case` statement.

## Primary and combining expressions

Expressions enclosed inside `[[ ]]` (or `[ ]` for `sh`) are called **test commands** or **primaries**. These expressions help us to indicate results of a conditional. In the tables below, we are using `[ ]`, because it works for `sh` too. Here is an answer about [the difference between double and single square brackets in bash](http://serverfault.com/a/52050).

**Working with the file system:**

| Primary       | Meaning                                                      |
| :-----------: | :----------------------------------------------------------- |
| `[ -e FILE ]` | True if `FILE` **e**xists.                                   |
| `[ -f FILE ]` | True if `FILE` exists and is a regular **f**ile.             |
| `[ -d FILE ]` | True if `FILE` exists and is a **d**irectory.                |
| `[ -s FILE ]` | True if `FILE` exists and not empty (**s**ize more than 0).  |
| `[ -r FILE ]` | True if `FILE` exists and is **r**eadable.                   |
| `[ -w FILE ]` | True if `FILE` exists and is **w**ritable.                   |
| `[ -x FILE ]` | True if `FILE` exists and is e**x**ecutable.                 |
| `[ -L FILE ]` | True if `FILE` exists and is symbolic **l**ink.              |
| `[ FILE1 -nt FILE2 ]` | FILE1 is **n**ewer **t**han FILE2.                   |
| `[ FILE1 -ot FILE2 ]` | FILE1 is **o**lder **t**han FILE2.                   |

**Working with strings:**

| Primary        | Meaning                                                     |
| :------------: | :---------------------------------------------------------- |
| `[ -z STR ]`   | `STR` is empty (the length is **z**ero).                    |
| `[ -n STR ]`   |`STR` is not empty (the length is **n**on-zero).             |
| `[ STR1 == STR2 ]` | `STR1` and `STR2` are equal.                            |
| `[ STR1 != STR2 ]` | `STR1` and `STR2` are not equal.                        |

**Arithmetic binary operators:**

| Primary             | Meaning                                                |
| :-----------------: | :----------------------------------------------------- |
| `[ ARG1 -eq ARG2 ]` | `ARG1` is **eq**ual to `ARG2`.                         |
| `[ ARG1 -ne ARG2 ]` | `ARG1` is **n**ot **e**qual to `ARG2`.                 |
| `[ ARG1 -lt ARG2 ]` | `ARG1` is **l**ess **t**han `ARG2`.                    |
| `[ ARG1 -le ARG2 ]` | `ARG1` is **l**ess than or **e**qual to `ARG2`.        |
| `[ ARG1 -gt ARG2 ]` | `ARG1` is **g**reater **t**han `ARG2`.                 |
| `[ ARG1 -ge ARG2 ]` | `ARG1` is **g**reater than or **e**qual to `ARG2`.     |

Conditions may be combined using these **combining expressions:**

| Operation      | Effect                                                      |
| :------------: | :---------------------------------------------------------- |
| `[ ! EXPR ]`   | True if `EXPR` is false.                                    |
| `[ (EXPR) ]`   | Returns the value of `EXPR`.                                |
| `[ EXPR1 -a EXPR2 ]` | Logical _AND_. True if `EXPR1` **a**nd `EXPR2` are true. |
| `[ EXPR1 -o EXPR2 ]` | Logical _OR_. True if `EXPR1` **o**r `EXPR2` are true.|

Sure, there are more useful primaries and you can easily find them in the [Bash man pages](http://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html).

## Using an `if` statement

`if` statements work the same as in other programming languages. If the expression within the braces is true, the code between `then` and `fi` is executed.  `fi` indicates the end of the conditionally executed code.

```bash
# Single-line
if [[ 1 -eq 1 ]]; then echo "true"; fi

# Multi-line
if [[ 1 -eq 1 ]]; then
  echo "true"
fi
```

Likewise, we could use an `if..else` statement such as:

```bash
# Single-line
if [[ 2 -ne 1 ]]; then echo "true"; else echo "false"; fi

# Multi-line
if [[ 2 -ne 1 ]]; then
  echo "true"
else
  echo "false"
fi
```

Sometimes `if..else` statements are not enough to do what we want to do. In this case we shouldn't forget about the existence of `if..elif..else` statements, which always come in handy.

Look at the example below:

```bash
if [[ `uname` == "Adam" ]]; then
  echo "Do not eat an apple!"
elif [[ `uname` == "Eva" ]]; then
  echo "Do not take an apple!"
else
  echo "Apples are delicious!"
fi
```

## Using a `case` statement

If you are confronted with a couple of different possible actions to take, then using a `case` statement may be more useful than nested `if` statements. For more complex conditions use `case` like below:

```bash
case "$extension" in
  "jpg"|"jpeg")
    echo "It's image with jpeg extension."
  ;;
  "png")
    echo "It's image with png extension."
  ;;
  "gif")
    echo "Oh, it's a giphy!"
  ;;
  *)
    echo "Woops! It's not image!"
  ;;
esac
```

Each case is an expression matching a pattern. The `|` sign is used for separating multiple patterns, and the `)` operator terminates a pattern list. The commands for the first match are executed. `*` is the pattern for anything else that doesn't match the defined patterns. Each block of commands should be divided with the `;;` operator.

# Loops

Here we won't be surprised. As in any programming language, a loop in bash is a block of code that iterates as long as the control conditional is true.

There are four types of loops in Bash: `for`, `while`, `until` and `select`.

## `for` loop

The `for` is very similar to its sibling in C. It looks like this:

```bash
for arg in elem1 elem2 ... elemN
do
  # statements
done
```

During each pass through the loop, `arg` takes on the value from `elem1` to `elemN`. Values may also be wildcards or [brace expansions](#brace-expansion).

Also, we can write `for` loop in one line, but in this case there needs to be a semicolon before `do`, like below:

```bash
for i in {1..5}; do echo $i; done
```

By the way, if `for..in..do` seems a little bit weird to you, you can also write `for` in C-like style such as:

```bash
for (( i = 0; i < 10; i++ )); do
  echo $i
done
```

`for` is handy when we want to do the same operation over each file in a directory. For example, if we need to move all `.bash` files into the `script` folder and then give them execute permissions, our script would look like this:

```bash
#!/bin/bash

for FILE in $HOME/*.bash; do
  mv "$FILE" "${HOME}/scripts"
  chmod +x "${HOME}/scripts/${FILE}"
done
```

## `while` loop

The `while` loop tests a condition and loops over a sequence of commands so long as that condition is _true_. A condition is nothing more than a [primary](#primary-and-combining-expressions) as used in `if..then` conditions. So a `while` loop looks like this:

```bash
while [[ condition ]]
do
  # statements
done
```

Just like in the case of the `for` loop, if we want to write `do` and condition in the same line, then we must use a semicolon before `do`.

A working example might look like this:

```bash
#!/bin/bash

# Squares of numbers from 0 through 9
x=0
while [[ $x -lt 10 ]]; do # value of x is less than 10
  echo $(( x * x ))
  x=$(( x + 1 )) # increase x
done
```

## `until` loop

The `until` loop is the exact opposite of the `while` loop. Like a `while` it checks a test condition, but it keeps looping as long as this condition is _false_:

```bash
until [[ condition ]]; do
  #statements
done
```

## `select` loop

The `select` loop helps us to organize a user menu. It has almost the same syntax as the `for` loop:

```bash
select answer in elem1 elem2 ... elemN
do
  # statements
done
```

The `select` prints all `elem1..elemN` on the screen with their sequence numbers, after that it prompts the user. Usually it looks like `$?` (`PS3` variable). The answer will be saved in `answer`. If `answer` is the number between `1..N`, then `statements` will execute and `select` will go to the next iteration — that's because we should use the `break` statement.

A working example might look like this:

```bash
#!/bin/bash

PS3="Choose the package manager: "
select ITEM in bower npm gem pip
do
  echo -n "Enter the package name: " && read PACKAGE
  case $ITEM in
    bower) bower install $PACKAGE ;;
    npm)   npm   install $PACKAGE ;;
    gem)   gem   install $PACKAGE ;;
    pip)   pip   install $PACKAGE ;;
  esac
  break # avoid infinite loop
done
```

This example, asks the user what package manager {s,he} would like to use. Then, it will ask what package we want to install and finally proceed to install it.

If we run this, we will get:

```
$ ./my_script
1) bower
2) npm
3) gem
4) pip
Choose the package manager: 2
Enter the package name: bash-handbook
<installing bash-handbook>
```

## Loop control

There are situations when we need to stop a loop before its normal ending or step over an iteration. In these cases, we can use the shell built-in `break` and `continue` statements. Both of these work with every kind of loop.

The `break` statement is used to exit the current loop before its ending. We have already met with it.

The `continue` statement steps over one iteration. We can use it as such:

```bash
for (( i = 0; i < 10; i++ )); do
  if [[ $(( i % 2 )) -eq 0 ]]; then continue; fi
  echo $i
done
```

If we run the example above, it will print all odd numbers from 0 through 9.

# Functions

In scripts we have the ability to define and call functions. As in any programming language, functions in bash are chunks of code, but there are differences.

In bash, functions are a sequence of commands grouped under a single name, that is the _name_ of the function. Calling a function is the same as calling any other program, you just write the name and the function will be _invoked_.

We can declare our own function this way:

```bash
my_func () {
  # statements
}

my_func # call my_func
```

We must declare functions before we can invoke them.

Functions can take on arguments and return a result — exit code. Arguments, within functions, are treated in the same manner as arguments given to the script in [non-interactive](#non-interactive-mode) mode — using [positional parameters](#positional-parameters). A result code can be _returned_ using the `return` command.

Below is a function that takes a name and returns `0`, indicating successful execution.

```bash
# function with params
greeting () {
  if [[ -n $1 ]]; then
    echo "Hello, $1!"
  else
    echo "Hello, unknown!"
  fi
  return 0
}

greeting Denys  # Hello, Denys!
greeting        # Hello, unknown!
```

We already discussed [exit codes](#exit-codes). The `return` command without any arguments returns the exit code of the last executed command. Above, `return 0` will return a successful exit code. `0`.

## Debugging

The shell gives us tools for debugging scripts. If we want to run a script in debug mode, we use a special option in our script's shebang:

```bash
#!/bin/bash options
```

These options are settings that change shell behavior. The following table is a list of options which might be useful to you:

| Short | Name        | Description                                            |
| :---: | :---------- | :----------------------------------------------------- |
| `-f`  | noglob      | Disable filename expansion (globbing).                 |
| `-i`  | interactive | Script runs in _interactive_ mode.                     |
| `-n`  | noexec      | Read commands, but don't execute them (syntax check).  |
|       | pipefail    | Make pipelines fail if any commands fail, not just if the final command fail. |
| `-t`  | —           | Exit after first command.                              |
| `-v`  | verbose     | Print each command to `stderr` before executing it.    |
| `-x`  | xtrace      | Print each command and its expanded arguments to `stderr` before executing it. |

For example, we have script with `-x` option such as:

```bash
#!/bin/bash -x

for (( i = 0; i < 3; i++ )); do
  echo $i
done
```

This will print the value of the variables to `stdout` along with other useful information:

```
$ ./my_script
+ (( i = 0 ))
+ (( i < 3 ))
+ echo 0
0
+ (( i++  ))
+ (( i < 3 ))
+ echo 1
1
+ (( i++  ))
+ (( i < 3 ))
+ echo 2
2
+ (( i++  ))
+ (( i < 3 ))
```

Sometimes we need to debug a part of a script. In this case using the `set` command is convenient. This command can enable and disable options. Options are turned on using `-` and turned off using `+`:

```bash
#!/bin/bash

echo "xtrace is turned off"
set -x
echo "xtrace is enabled"
set +x
echo "xtrace is turned off again"
```

# Afterword

I hope this small handbook was interesting and helpful. To be honest, I wrote this handbook for myself so as to not forget the bash basics. I tried to write concisely but meaningfully, and I hope you will appreciate that.

This handbook narrates my own experience with Bash. It does not purport to be comprehensive, so if you still want more, please run `man bash` and start there.

Contributions are absolutely welcome and I will be grateful for any corrections or questions you can send my way. For all of that create a new [issue](https://github.com/denysdovhan/bash-handbook/issues).

Thanks for reading this handbook!

# Want to learn more?

Here's a list of other literature covering Bash:

* Bash man page.  In many environments that you can run Bash, the help system `man` can display information about Bash, by running the command `man bash`.  For more information on the `man` command, see the web page ["The man Command"](http://www.linfo.org/man.html) hosted at [The Linux Information Project](http://www.linfo.org/).
* ["Bourne-Again SHell manual"](https://www.gnu.org/software/bash/manual/) in many formats, including HTML, Info, TeX, PDF, and Texinfo.  Hosted at <https://www.gnu.org/>.  As of 2016/01, this covers version 4.3, last updated 2015/02/02.

# Other resources

* [awesome-bash](https://github.com/awesome-lists/awesome-bash) is a curated list of Bash scripts and resources
* [awesome-shell](https://github.com/alebcay/awesome-shell) is another curated list of shell resources
* [bash-it](https://github.com/Bash-it/bash-it) provides a solid framework for using, developing and maintaining shell scripts and custom commands for your daily work.
* [dotfiles.github.io](http://dotfiles.github.io/) is a good source of pointers to the various dotfiles collections and shell frameworks available for bash and other shells.
* [learnyoubash](https://github.com/denysdovhan/learnyoubash) helps you write your first bash script
* [shellcheck](https://github.com/koalaman/shellcheck) is a static analysis tool for shell scripts. You can either use it from a web page at [www.shellcheck.net](http://www.shellcheck.net/) or run it from the command line. Installation instructions are on the [koalaman/shellcheck](https://github.com/koalaman/shellcheck) github repository page.

Finally, Stack Overflow has many questions that are [tagged as bash](https://stackoverflow.com/questions/tagged/bash) that you can learn from and is a good place to ask if you're stuck.

# License

[![CC 4.0][cc-image]][cc-url]

&copy; [Denys Dovhan](http://denysdovhan.com)

[cc-url]: http://creativecommons.org/licenses/by/4.0/
[cc-image]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg?style=flat-square

[npm-url]: https://npmjs.org/package/bash-handbook
[npm-image]: https://img.shields.io/npm/v/bash-handbook.svg?style=flat-square

[gitter-url]: https://gitter.im/denysdovhan/bash-handbook
[gitter-image]: https://img.shields.io/gitter/room/nwjs/nw.js.svg?style=flat-square
