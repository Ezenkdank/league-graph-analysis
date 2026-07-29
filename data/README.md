# İşlenmiş veri sözlüğü

## `affiliations.csv`

Notebook'un doğrudan kullandığı nihai tablo. Bir oyuncu-takım lineage çifti
birden fazla tarihsel takım adıyla göründüğünde birden fazla satır bulunabilir;
notebook bu satırları tek graph kenarı altında toplar.

Temel alanlar:

- `player_node`, `team_node`: kararlı graph düğüm kimlikleri.
- `player_name`, `team_name`: gösterim etiketleri.
- `team_name_at_time`: ilgili maçlarda görülen takım adı.
- `team_name_history`: lineage'a eşlenen ad geçmişi.
- `group`: `major` veya `wildcard`.
- `leagues`: lig kodu.
- `first_date`, `last_date`, `active_years`: gözlenen zaman aralığı.
- `games_*`, `wins_*`, `minutes_*`: domestic, international ve total
  toplulaştırmalar.
- `domestic_win_rate`: `wins_domestic / games_domestic`.

Boyut: 3.561 satır, 2.029 benzersiz oyuncu ve 244 takım lineage'ı.

## `affiliations_clean.csv`

Takım lineage eşlemesinden önceki tekilleştirilmiş oyuncu-takım tablosu.
3.622 satır, 2.076 oyuncu ve 278 takım kimliği içerir. Oyuncu-takım çifti bu
dosyada benzersizdir.

## `team_alias_map.csv`

Orijinal takım adlarını canonical takım adı ve düğümüne eşler.

- `original_team_name`, `original_team_node`: kaynak kayıttaki kimlik.
- `canonical_team_name`, `canonical_team_node`: analizde kullanılan lineage.
- `changed`: eşlemenin kimliği değiştirip değiştirmediği.
- `cluster_size`: aynı lineage'a bağlanan ad sayısı.
- `method`: kullanılan kanıtların birleşimi:
  `normalized_name`, `cargo_short_plus_roster`, `manual_review` veya bunların
  `|` ile birleştirilmiş biçimi.

## Sınırlamalar

- Bunlar ham API yanıtları değildir.
- Oyuncu adları kamuya açık profesyonel/espor kimlikleridir; bazı gösterim
  etiketleri gerçek adları parantez içinde içerebilir.
- `international` alanı uluslararası katılımı gösterir fakat Worlds/MSI
  ayrımını tek başına sağlamaz.
- Kaydedilmiş kadrolar kısa süreli yedekleri de içerebilir.
- Takım lineage eşlemelerinin bir bölümü elle gözden geçirilmiştir ve bu
  kararların ayrı gerekçe günlüğü korunmamıştır.
