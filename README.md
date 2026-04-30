# Osmancık

Reigns-tarzı bir Osmanlı saray yönetim oyunu. [TahtLang](https://github.com/tahtlang/tahtlang) DSL ile yazılmış.

Padişah olarak tahta çıkıyorsun. Sadrazam, Şeyhülislam, Yeniçeri Ağası, Valide Sultan ve diğer saray mensupları sana gelip karar vermeni istiyor. Hazine, Ordu, Reaya ve Din — dört sayacı dengede tut, yoksa devlet çöker.

## Oyna

```bash
pip install tahtlang
git clone https://github.com/miratcan/osmancik.git
cd osmancik
tahtlang main.taht
```

## İçerik

- **81 kart** — siyasi entrika, savaş, salgın, absürt olaylar
- **12 karakter** — Sadrazam, Şeyhülislam, Kaptan-ı Derya, Valide Sultan, Soytarı, Orman Cini...
- **Hikaye zincirleri** — kardeş veraseti krizi (18 kart), savaş cepheleri, salgınlar
- **Absürt olaylar** — kurmali sadrazam, uçan yeniçeriler, yapay zeka, simit güneş tutulması

## Yapı

```
main.taht           # Sayaçlar, flag'ler, karakterler, import'lar
kartlar/
  giris.taht        # Tutorial kartları
  sadrazam.taht     # Siyasi kararlar
  seyhulislam.taht  # Dini meseleler
  kaptaniderya.taht # Denizcilik ve ticaret
  yeniceri.taht     # Askeri kararlar
  valide.taht       # Saray içi entrikalar
  haseki.taht       # Harem
  haremagasi.taht   # Saray yönetimi
  soytari.taht      # Komik/absürt olaylar
  ormancini.taht    # Gizemli orman cini
  kardes.taht       # Veraset krizi zinciri
  afetler.taht      # Deprem, kıtlık
  salginlar.taht    # Veba, verem
  absurt.taht       # Zamansız absürt olaylar
```

## TahtLang Nedir?

[TahtLang](https://github.com/tahtlang/tahtlang), Reigns-tarzı kart oyunları için tasarlanmış bir domain-specific language. `.taht` dosyaları yazarsın, terminalde oynarsın, JSON'a derleyip Unity/Godot'ta kullanırsın. Tree-sitter grammar ve LSP desteği var.

## Lisans

MIT
