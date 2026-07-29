# League of Legends player-team graph analysis

Bu depo, profesyonel League of Legends oyuncuları ile takım lineage'ları
arasındaki ilişkileri yönsüz ve ağırlıklı bir bipartite graph olarak inceler.
Ana çalışma yüzeyi `code/league-graph.ipynb` dosyasıdır.

## Kapsam

Mevcut veri 2015-2019 sezonlarını ve şu on ligi kapsar:

`CBLOL`, `LCK`, `LCL`, `LCS`, `LEC`, `LJL`, `LMS`, `LPL`, `OPL`, `TCL`.

Notebook:

1. `data/processed/affiliations.csv` dosyasını yükler ve veri sözleşmesini
   doğrular.
2. Tarihsel/rebrand satırlarını oyuncu-takım lineage kenarları altında
   toplulaştırır.
3. NetworkX ile bipartite graph kurar.
4. Yoğunluk, derece, bileşenler, bipartite clustering ve ağırlıklı strength
   ölçülerini hesaplar.
5. Lig içi başarı ile kaydedilmiş kadro sürekliliği arasındaki gözlemsel
   ilişkiyi inceler.

## Kurulum ve çalıştırma

Python 3.12 önerilir.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
jupyter notebook code/league-graph.ipynb
```

Notebook varsayılan olarak dosya üretmez. Çıktıları yazmak için son bölümdeki
`EXPORT_FILES` değerini `True` yapın; oluşturulan `outputs/` klasörü Git
tarafından izlenmez.

## Veri kaynağı ve yeniden üretilebilirlik

İşlenmiş tablolar Leaguepedia'nın Fandom üzerindeki MediaWiki Cargo API
verilerinden türetilmiştir. Orijinal ham yanıtlar ve veri toplama scripti
korunmadığı için tam sorgu parametreleri bugün kesin olarak doğrulanamıyor.
Mevcut sütunların işaret ettiği muhtemel toplama ve dönüştürme zinciri,
kanıt/çıkarım ayrımıyla [DATA_PROVENANCE.md](DATA_PROVENANCE.md) içinde
belgelenmiştir.

Bu sınırlama nedeniyle depodaki CSV'ler sabit bir araştırma snapshot'ı olarak
değerlendirilmelidir; güncel Leaguepedia verisinin eksiksiz bir aynası değildir.

## Veri dosyaları

- `data/processed/affiliations_clean.csv`: İlk tekilleştirilmiş oyuncu-takım
  tablosu; 3.622 satır.
- `data/processed/team_alias_map.csv`: Takım adı/lineage eşleme kararları;
  otomatik ve elle gözden geçirilmiş kuralları içerir.
- `data/processed/affiliations.csv`: Alias/lineage eşlemeleri uygulandıktan
  sonraki analiz girdisi; 3.561 satır, 2.029 oyuncu ve 244 takım lineage'ı.

Veri alanlarının ayrıntıları için [data/README.md](data/README.md) dosyasına
bakın.

## Lisans ve atıf

Leaguepedia kaynaklı veri ve türetilmiş CSV'ler için
[DATA_LICENSE.md](DATA_LICENSE.md) geçerlidir. Leaguepedia, lisanslayabildiği
metinsel ve grafik içeriği CC BY-SA 3.0 altında yayımlar; bu nedenle veri
dosyaları da aynı lisansla ve kaynak atfıyla sunulmaktadır.

Kaynak kod için henüz ayrı bir açık kaynak lisansı seçilmemiştir. Bir lisans
eklenene kadar kodun yeniden kullanımına otomatik izin verilmiş sayılmaz.

Bu proje Riot Games tarafından desteklenmez veya onaylanmaz. League of Legends
ve ilgili markalar kendi hak sahiplerine aittir.
