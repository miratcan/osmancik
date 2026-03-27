# Osmancik

Osmancik, **TahtLang** DSL ile yazilmis bir Osmanli saray yonetim oyunudur (Reigns tarzi kart oyunu). Oyuncu padisah olarak kararlar verir, 4 temel sayac (hazine, ordu, reaya, din) dengelemeye calisir.

## TahtLang Nedir

TahtLang, Reigns tarzi kart oyunlari icin tasarlanmis bir domain-specific language. `.taht` dosyalari yazarsin, toolchain bunlari validate eder, JSON'a derler, terminalde oynatir.

Toolchain: `pip install -e /path/to/tahtlang` ile kurulur.

## CLI Komutlari

```bash
tahtlang main.taht                # Oyunu terminalde oyna (implicit play)
tahtlang play main.taht --debug   # Debug panelleriyle oyna
tahtlang main.taht --validate     # Sadece validate et (parse + semantik kontrol)
tahtlang compile main.taht        # JSON'a derle
tahtlang compile main.taht --compact  # Minified JSON
tahtlang stats main.taht          # 100 simulasyon calistir, denge analizi
tahtlang stats main.taht --runs 500  # 500 simulasyon
tahtlang merge main.taht          # Import'lari tek dosyaya birlestir
tahtlang split main.taht          # Kartlari bearer'a gore dosyalara bol
```

Herhangi bir degisiklikten sonra `tahtlang main.taht --validate` calistir. Hata varsa duzelt.

## Dosya Yapisi

```
osmancik/
  main.taht          # Ana dosya: settings, counter, flag, character, variant tanimlari + import'lar
  kartlar/
    giris.taht        # Tutorial kartlari (4 kart)
    sadrazam.taht     # Sadrazam kartlari
    seyhulislam.taht  # Seyhulislam kartlari
    kaptaniderya.taht # Kaptan-i Derya kartlari
    haremagasi.taht   # Harem Agasi kartlari
    haseki.taht       # Haseki Sultan kartlari
    yeniceri.taht     # Yeniceri kartlari
    valide.taht       # Valide Sultan kartlari
    soytari.taht      # Soytari kartlari
    ormancini.taht    # Orman Cini kartlari
    afetler.taht      # Dogal afet kartlari
    salginlar.taht    # Salgin kartlari
    kardes.taht       # Kardes veraset krizi (en buyuk zincir, 19 kart)
    absurt.taht       # Absurt olaylar
```

Yeni kart dosyasi eklendiginde `main.taht`'a `import "kartlar/dosya.taht"` satiri da eklenmeli.

## Mevcut Entity'ler (main.taht'ta tanimli)

### Sayaclar (counters)

| ID | Modifier | Start | Aciklama |
|----|----------|-------|----------|
| `counter:hazine` | killer | 50 | Devlet hazinesi |
| `counter:ordu` | killer | 50 | Askeri guc |
| `counter:reaya` | killer | 50 | Halk memnuniyeti |
| `counter:din` | killer | 50 | Dini otorite |
| `counter:dilek` | - | 0 | Orman cini dilek hakki |
| `counter:cami` | keep | 0 | Insa edilen cami sayisi |
| `counter:turbe` | keep | 0 | Insa edilen turbe sayisi |
| `counter:mesire` | keep | 0 | Yapilan mesire yeri sayisi |
| `counter:haremziyaret` | - | 0 | Harem ziyaret sayisi |
| `counter:sehzade` | - | 0 | Sehzade sayisi |
| `counter:toplam_imar` | virtual | - | source: cami+turbe+mesire, aggregate: sum |
| `counter:yas` | keep | 18 | Padisahin yasi |

`killer` olan sayaclar 0 veya 100'e ulasinca oyun biter.
`keep` olan sayaclar reign'ler arasi korunur.
Virtual counter'larin degeri source'lardan hesaplanir, dogrudan degistirilemez.

### Flag'ler

| ID | Aciklama |
|----|----------|
| `flag:start` | Oyun baslangici (starting_flags) |
| `flag:savas` | Savas durumu |
| `flag:verem` | Verem salgini var |
| `flag:veba` | Veba salgini var |
| `flag:isyan` | Halk isyani var |
| `flag:deprem` | Deprem oldu |
| `flag:kitlik` | Kitlik var |
| `flag:bolluk` | Bolluk var |
| `flag:katarakt` | Padisah katarakt oldu |
| `flag:evlilik` | Padisah evli |
| `flag:asiksin` | Padisah asik |
| `flag:ormancini` | Orman cini ile taninildi |
| `flag:kardeskafeste` | Kardes kafeste |
| `flag:kardesuzakta` | Kardes sancakta |
| `flag:kardesbitti` | Kardes meselesi kapandi |
| `flag:kardespapagan` | Kardes papagana donustu |
| `flag:simit_gunu` | Simit tutulmasi yasandi |
| `flag:metal_sadrazam` | Kurmali sadrazam aktif |
| `flag:hava_kuvvetleri` | Ucan yeniceriler aktif |
| `flag:ai` | Yapay zeka geldi |
| `flag:kuzeyde_savas` | Kuzey cephesi |
| `flag:guneyde_savas` | Guney cephesi |
| `flag:doguda_savas` | Dogu cephesi |
| `flag:batida_savas` | Bati cephesi |

Yeni flag eklerken `main.taht`'a tanimini yaz. Kartlarda kullanilan her flag tanimlanmis olmali, validator yakalar.

### Karakterler

| ID | Ad |
|----|-----|
| `character:aksakallidede` | Ak Sakalli Dede |
| `character:sadrazam` | Sadrazam |
| `character:seyhulislam` | Seyhulislam |
| `character:kaptaniderya` | Kaptan-i Derya |
| `character:yeniceriagasi` | Yeniceri Agasi |
| `character:validesultan` | Valide Sultan |
| `character:hasekisultan` | Haseki Sultan |
| `character:sancakbeyi` | Sancak Beyi |
| `character:soytari` | Soytari |
| `character:ormancini` | Orman Cini |
| `character:haremagasi` | Harem Agasi |
| `character:anlatici` | Anlatici |

### Variant'lar (duygu/durum)

| ID | Ad |
|----|-----|
| `variant:ofkeli` | Ofkeli |
| `variant:memnun` | Memnun |
| `variant:korkmus` | Korkmus |
| `variant:kurnaz` | Kurnaz |

## TahtLang Syntax Referansi (kaynak koddan)

### Entity Tanimlari

Tum entity'ler `main.taht`'in ust kisminda, kartlardan once tanimlanir.

```taht
# Counter
Hazine (counter:hazine, killer)
	start: 50

# Keep modifier: reign'ler arasi korunur
Cami (counter:cami, keep)
	start: 0

# Virtual counter (aggregate)
Toplam Imar (counter:toplam_imar)
	source: counter:cami, counter:turbe, counter:mesire
	aggregate: sum

# Flag
Savas Var (flag:savas)

# Karakter
Sadrazam (character:sadrazam)

# Variant
Ofkeli (variant:ofkeli)

# Ayarlar
Game Settings (settings:main)
	description: "Oyun aciklamasi"
	starting_flags: flag:start
	game_over_on_zero: "Devlet coktu!"
	game_over_on_max: "Saltanat sona erdi!"
```

**Entity header formati:** `Gorünen Ad (type:snake_case_id, modifier1, modifier2)`

**Gecerli modifier'lar:**
- Counter: `killer`, `keep`
- Flag: `keep`
- Card: `ring`

**Counter property'leri:** `start`, `icon`, `color`, `source`, `aggregate` (average/sum/min/max), `track` (yes/no)

**Aggregate tipleri:** `average`, `sum`, `min`, `max`

**ID kurallari:** snake_case zorunlu. camelCase validator hatasi verir.

### Kart Yapisi

```taht
Kart Adi (card:kart_id)
	bearer: character:sadrazam (variant:korkmus)
	weight: 1.0
	weight: 2.0 when counter:hazine < 30
	require: !flag:savas, counter:ordu > 40
	lockturn: 10
	> Kart metni. Oyuncuya gosterilen metin.
	* Sol secim: counter:hazine 15, counter:reaya -10
	* Sag secim: counter:din 5, +flag:savas
```

Her property tab ile indent edilmis olmali.

### Bearer

```taht
bearer: character:sadrazam                   # Sadece karakter
bearer: character:sadrazam (variant:korkmus) # Karakter + variant
```

Bearer olmayan kartlar anlatici/olay kartlaridir.

### Weight (agirlik)

```taht
weight: 1.0                              # Sabit agirlik
weight: 2.0 when counter:hazine < 30     # Kosullu agirlik
weight: 0.5 when flag:savas              # Flag kosullu
```

Birden fazla weight satiri **toplanir**:
- `weight: 1.0` + `weight: 2.0 when X` = normalde 1.0, X dogru ise 3.0

Weight'i olmayan kartlar otomatik olarak ring gibi davranir (random pool'a girmez).

### Require (kosullar)

```taht
require: flag:savas                      # Flag set olmali
require: !flag:savas                     # Flag set OLMAMALI
require: counter:hazine < 30             # Sayac karsilastirmasi
require: counter:ordu >= 50              # >= desteklenir
require: counter:hazine = 50             # Tam esitlik
require: flag:savas, counter:ordu > 40   # Birden fazla: AND mantigi
```

**Operatorler:** `<`, `>`, `<=`, `>=`, `=`

Virgul ile ayrilmis kosullar AND ile birlesir. OR yok.

### Lockturn (bekleme suresi)

| Deger | Anlam |
|-------|-------|
| `lockturn: 10` | 10 tur boyunca tekrar gosterme |
| `lockturn: once` | Bu reign boyunca tekrar gosterme |
| `lockturn: dispose` | Bir kez goster, sonra kaldir (tutorial kartlari icin) |

### Secimler ve Efektler

```taht
* Secim metni: efekt1, efekt2, efekt3
* Bos secim:                              # Efektsiz secim
```

**Counter degistirme:**
```taht
counter:hazine 20       # +20 ekle
counter:hazine -20      # 20 cikar
counter:ordu 10?30      # 10 ile 30 arasi rastgele
counter:ordu -20?-10    # -20 ile -10 arasi rastgele
counter:ordu -5?10      # -5 ile +10 arasi rastgele
```

Sayaclar her zaman [0, 100] araligina clamp edilir.

**Flag degistirme:**
```taht
+flag:savas             # Flag'i set et
-flag:savas             # Flag'i kaldir
```

**Kart kuyruga ekleme:**
```taht
card:kart_id            # Bir sonraki turda goster (kuyruga ekle)
card:kart_id@5          # 5 tur sonra goster (zamanlama)
```

**Dallanma (branch):**
```taht
[card:_yol_a, card:_yol_b]   # Ilk require'i gecen karti sec
```

Runtime sirayla bakar, require'i gecen ilk karti kuyruga ekler. Hicbiri gecmezse atlanir.

**Trigger:**
```taht
trigger:response "Metin"      # Secimden sonra metin goster
trigger:sound "dosya.wav"     # Ses cal
```

**Kombine:**
```taht
* Vergi topla: counter:hazine 20, counter:reaya -15, +flag:high_tax, card:_sonuc@3
```

### Ring Kartlar (Zincir Kartlari)

Random pool'a girmeyen, sadece baska kartlardan tetiklenen kartlar.

```taht
Darbe Girisimi (card:_kafes_darbe, ring)
	bearer: character:yeniceriagasi (variant:korkmus)
	> Darbe bastirildi sultanim!
	* Burnu iyilesir: counter:ordu -10, +flag:kardesbitti
```

**Kurallar:**
- ID `_` ile baslamali: `card:_darbe`, `card:_sonuc`
- `ring` modifier zorunlu
- `_` ile baslayip `ring` olmayan veya `ring` olup `_` ile baslamayan kart validator hatasi verir
- Tetiklenme yollari: `card:_id`, `card:_id@N`, `[card:_a, card:_b]`

### Import

```taht
import "kartlar/sadrazam.taht"
```

Yollar import eden dosyaya goredir (relative path).

### Yorumlar

```taht
# Bu bir yorumdur
```

### Metadata

```taht
Sadrazam (character:sadrazam)
	meta.portrait: sadrazam.png
	meta.voice: deep

Kart (card:kart_id)
	meta.image: resim.png
	meta.mood: dark
```

Metadata validate edilmez, oldugu gibi JSON'a aktarilir.

## Runtime Davranisi (engine.py'den)

### Kart Secim Onceligi

1. **Zamanlanmis kartlar:** `card:id@N` delay'i dolmus kartlar
2. **Kuyruk:** `card:id` ile eklenmis kartlar (FIFO)
3. **Random pool:** Eligible kartlardan agirlikli rastgele secim

### Eligibility (random pool icin)

Bir kartin pool'a girmesi icin:
- `ring` olmamali
- Lockturn kilidi aktif olmamali
- Tum `require` kosullari gecmeli
- Toplam weight > 0

### Game Over

`killer` modifier'li sayaclar kontrol edilir:
- Deger <= 0 veya >= 100 ise oyun biter
- Her secim uygulandiktan sonra kontrol yapilir

### Sayac Siniri

Tum sayaclar [0, 100] araligina clamp edilir. -50 bile olsa 0'in altina dusmez.

## Validasyon Kurallari

Validator su kontrolleri yapar (kaynak koddan):

1. **ID'ler snake_case olmali.** camelCase algılanır ve hata verilir.
2. **Duplicate ID yasak.** Ayni tipte ayni ID iki kez tanimlanamaz.
3. **Ring karti `_` ile baslamali**, `_` ile baslayan kart `ring` modifier'a sahip olmali.
4. **Referans butunlugu:** Kartlardaki tum counter, flag, character, variant, card referanslari tanimli olmali.
5. **Bearer'daki karakter ve variant tanimli olmali.**
6. **Flag bind referansi gecerli karakter olmali.**
7. **Virtual counter source'lari:** Aggregate icin `counter:` referanslari, tracking icin `character:` referanslari olmali.
8. **starting_flags icindeki flag'ler tanimli olmali.**
9. **Circular import yasak.**

## Yeni Icerik Ekleme Rehberi

### Yeni kart eklemek

1. Uygun kart dosyasini sec (bearer karaktere gore) veya yeni dosya olustur
2. Kart header'i yaz: `Kart Adi (card:kart_id)`
3. Gerekli property'leri ekle (bearer, weight, require, lockturn)
4. Kart metnini `>` ile yaz
5. Secimleri `*` ile yaz, efektleri `:` den sonra virgul ile ayir
6. Yeni dosya olusturduysan `main.taht`'a `import` ekle
7. `tahtlang main.taht --validate` calistir

### Yeni zincir (hikaye arci) eklemek

1. Giris kartini normal weight ile yaz (random pool'a girer)
2. Zincirdeki kartlari `ring` modifier ile yaz, ID'leri `_` ile baslat
3. Giris kartindan ring kartlara `card:_id` veya `card:_id@N` ile bagla
4. Dallanma icin `[card:_a, card:_b]` kullan, her dalda farkli `require` koy
5. Zincirin sonunda flag'leri temizle (ornegin `-flag:savas`)

### Yeni karakter eklemek

1. `main.taht`'a karakter tanimi ekle: `Ad (character:id)`
2. `main.taht`'a import satirini ekle (gerekiyorsa yeni dosya icin)
3. Yeni dosya olustur: `kartlar/karakter.taht`
4. Kartlarda `bearer: character:id` ile kullan

### Yeni sayac eklemek

1. `main.taht`'a counter tanimi ekle: `Ad (counter:id)` + `start: N`
2. `killer` mi `keep` mi karar ver
3. Kartlarda `counter:id N` ile kullan

### Yeni flag eklemek

1. `main.taht`'a flag tanimi ekle: `Ad (flag:id)`
2. Kartlarda `+flag:id` / `-flag:id` ile set/clear et
3. Kosullarda `flag:id` veya `!flag:id` ile kontrol et

## Dikkat Edilecekler

- Sayac efektlerinde isarete dikkat et: `counter:hazine 20` ekler, `counter:hazine -20` cikarir
- `flag:` prefix'i her yerde zorunlu: `require:`, `+`/`-` komutlari, `weight: when`
- `counter:` prefix'i her yerde zorunlu: `require:`, efektler, `weight: when`
- `card:` prefix'i her yerde zorunlu: kuyruk, zamanlama, dallanma
- `character:` prefix'i bearer'da zorunlu
- `variant:` prefix'i bearer'da zorunlu
- Indent icin tab kullan
- Her kart en az bir secim (`*`) icermeli
- Ring kartlarin `_` prefix'i ve `ring` modifier'i birlikte olmali
