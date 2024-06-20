# KFI-File2Str

There are some differences to be noted: class File which, to some extend, plays
the role of the class `Path` from the java.nio.file package. Notice also, that BufferedReader must be created in two steps: first we create ‘raw’ byte stream of type, e.g.,
FileInputStream, then we pass it to the constructor of InputStreamReader with, as
the second argument, the required encoding, and then this to the constructor of the
BufferedReader. In the example above there are only two steps: to the constructor
of BufferedReader we pass directly an object of type FileReader, but then there is
no way to specify the encoding — default system encoding will be assumed. Note how
try-with-resources simplifies operations on IO streams; in the program above, we don’t
use it and therefore the somewhat complicated finally clause is needed (as close may
itself also throw an exception).  
  
It is sometimes useful to a read text file into a single string (for example, to process it with regular exceptions). This is a very common task, and it can be accomplished, for example, as shown below

## Listing 79 KFI-File2Str/File2Str.java

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import static java.nio.charset.StandardCharsets.UTF_8;

public class File2Str {
    public static void main(String[] args) {
        String text = null;

        try {
            byte[] bytes =
                Files.readAllBytes(Paths.get("pangram.txt"));
            text = new String(bytes, UTF_8);
        } catch(IOException e) {
            System.out.println(e.getMessage());
            System.exit(1);
        }
        System.out.println("***** File read (1):\n" + text);

          // simpler way, since java 11
        try {
            text = Files.readString(
                            Paths.get("pangram.txt"), UTF_8);
        } catch(IOException e) {
            System.out.println(e.getMessage());
            System.exit(1);
        }
        System.out.println("***** File read (2):\n" + text);
    }
}
```

pangram.txt

```
Sentences that contain all letters commonly used in a language
--------------------------------------------------------------
Danish (da)
  Quizdeltagerne spiste jordbær med fløde, mens cirkusklovnen
  Wolther spillede på xylofon.
  (= Quiz contestants were eating strawbery with cream while Wolther
  the circus clown played on xylophone.)

German (de)
-----------
  Falsches Üben von Xylophonmusik quält jeden größeren Zwerg
  (= Wrongful practicing of xylophone music tortures every larger dwarf)

  Zwölf Boxkämpfer jagten Eva quer über den Sylter Deich
  (= Twelve boxing fighters hunted Eva across the dike of Sylt)

  Heizölrückstoßabdämpfung
  (= fuel oil recoil absorber)
  (jqvwxy missing, but all non-ASCII letters in one word)

English (en)
------------
  The quick brown fox jumps over the lazy dog

Spanish (es)
------------
  El pingüino Wenceslao hizo kilómetros bajo exhaustiva lluvia y
  frío, añoraba a su querido cachorro.
  (Contains every letter and every accent, but not every combination
  of vowel + acute.)

French (fr)
-----------
  Portez ce vieux whisky au juge blond qui fume sur son île intérieure, à
  côté de l'alcôve ovoïde, où les bûches se consument dans l'âtre, ce
  qui lui permet de penser à la cænogenèse de l'être dont il est question
  dans la cause ambiguë entendue à Moÿ, dans un capharnaüm qui,
  pense-t-il, diminue çà et là la qualité de son œuvre.

  l'île exiguë
  Où l'obèse jury mûr
  Fête l'haï volapük,
  Âne ex aéquo au whist,
  Ôtez ce vœu déçu.

  Le cœur déçu mais l'âme plutôt naïve, Louÿs rêva de crapaüter en
  canoë au delà des îles, près du mälström où brûlent les novæ.

Irish Gaelic (ga)
-----------------
  D'fhuascail Íosa, Úrmhac na hÓighe Beannaithe, pór Éava agus Ádhaimh

Hungarian (hu)
--------------
  Árvíztűrő tükörfúrógép
  (= flood-proof mirror-drilling machine, only all non-ASCII letters)

Icelandic (is)
--------------
  Kæmi ný öxi hér ykist þjófum nú bæði víl og ádrepa

  Sævör grét áðan því úlpan var ónýt
  (some ASCII letters missing)

Japanese (jp)
-------------
  Hiragana: (Iroha)

  いろはにほへとちりぬるを
  わかよたれそつねならむ
  うゐのおくやまけふこえて
  あさきゆめみしゑひもせす

  Katakana:

  イロハニホヘト チリヌルヲ ワカヨタレソ ツネナラム
  ウヰノオクヤマ ケフコエテ アサキユメミシ ヱヒモセスン

Hebrew (iw)
-----------
  ? דג סקרן שט בים מאוכזב ולפתע מצא לו חברה איך הקליטה

Polish (pl)
-----------
  Pchnąć w tę łódź jeża lub ośm skrzyń fig
  (= To push a hedgehog or eight bins of figs in this boat)

Russian (ru)
------------
  В чащах юга жил бы цитрус? Да, но фальшивый экземпляр!
  (= Would a citrus live in the bushes of south? Yes, but only a fake one!)

Thai (th)
---------
  [--------------------------|------------------------]
  ๏ เป็นมนุษย์สุดประเสริฐเลิศคุณค่า  กว่าบรรดาฝูงสัตว์เดรัจฉาน
  จงฝ่าฟันพัฒนาวิชาการ           อย่าล้างผลาญฤๅเข่นฆ่าบีฑาใคร
  ไม่ถือโทษโกรธแช่งซัดฮึดฮัดด่า     หัดอภัยเหมือนกีฬาอัชฌาสัย
  ปฏิบัติประพฤติกฎกำหนดใจ        พูดจาให้จ๊ะๆ จ๋าๆ น่าฟังเอย ฯ

  [The copyright for the Thai example is owned by The Computer
  Association of Thailand under the Royal Patronage of His Majesty the
  King.]
```
