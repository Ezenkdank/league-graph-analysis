# Veri provenansı ve yeniden oluşturma notları

## Kesin olarak doğrulanabilenler

Depodaki dosyalardan aşağıdaki bilgiler doğrudan doğrulanabiliyor:

- Veriler 2015-2019 sezonlarını kapsıyor. Tarih aralığı
  `2014-11-15T00:00:00Z` ile `2019-09-16T00:00:00Z`; ilk tarih 2015 sezonu
  öncesi kayıtları içeriyor.
- Ligler `CBLOL`, `LCK`, `LCL`, `LCS`, `LEC`, `LJL`, `LMS`, `LPL`, `OPL` ve
  `TCL`.
- Her oyuncu-takım kaydı için maç, galibiyet, süre, ilk/son tarih ve aktif yıl
  bilgileri; domestic ve international ayrımıyla tutulmuş.
- `affiliations_clean.csv` içindeki sayısal toplamlar alias/lineage
  birleştirmesinden sonra `affiliations.csv` içinde korunmuş:
  `149937` oyuncu-maç, `75104` oyuncu-galibiyet ve yaklaşık
  `5294622.88` oyuncu-dakika.
- `team_alias_map.csv`, `normalized_name`, `cargo_short_plus_roster` ve
  `manual_review` gibi yöntem etiketleri taşıyor. Bu, takım adlarının yalnızca
  basit metin normalizasyonuyla değil Cargo kısa adları, kadro örtüşmesi ve
  elle inceleme ile lineage düzeyinde birleştirildiğini gösteriyor.
- Notebook yalnızca son işlenmiş `affiliations.csv` dosyasından başlıyor; ham
  API yanıtını oluşturan kod veya ham dosya depoda yok.

## Kaynak sistemi hakkında doğrulanabilenler

Leaguepedia'nın API'si MediaWiki API üzerindeki Cargo `cargoquery` aksiyonudur:

`https://lol.fandom.com/api.php?action=cargoquery&format=json&...`

Leaguepedia'nın yayımlanmış Cargo şeması mevcut veri alanlarıyla uyumludur:

- `ScoreboardPlayers`: `Link`, `Team`, `PlayerWin`, `DateTime_UTC`,
  `OverviewPage`, `GameId` ve `GameTeamId` alanlarını sağlar.
- `ScoreboardGames`: `DateTime_UTC`, `Gamelength`,
  `Gamelength_Number`, `OverviewPage` ve oyun/takım alanlarını sağlar.
- `Tournaments`: `OverviewPage`, `DateStart`, `League` ve `Region` alanlarını
  sağlar.
- `Teams` ve `Teamnames`: takımın uzun/kısa adlarını ve takım sayfası
  kimliklerini sağlar.

Şema belgeleri:

- https://lol.fandom.com/wiki/Module:CargoDeclare/ScoreboardPlayers
- https://lol.fandom.com/wiki/Module:CargoDeclare/ScoreboardGames
- https://lol.fandom.com/wiki/Module:CargoDeclare/Tournaments
- https://lol.fandom.com/wiki/Module:CargoDeclare/Teams
- https://lol.fandom.com/wiki/Module:CargoDeclare/Teamnames

## Güçlü çıkarım: muhtemel toplama zinciri

Aşağıdaki bölüm eldeki şemadan yapılan bir yeniden yapılandırmadır; kayıp
scriptin birebir kaydı değildir.

1. `ScoreboardPlayers`, oyun başına oyuncu ve takım kimliği ile galibiyet
   bilgisini almak için sorgulandı.
2. Oyun süresi ve tarih için `ScoreboardGames`, turnuvanın lig bilgisi için
   `Tournaments` tablosu `GameId` ve `OverviewPage` anahtarları üzerinden
   birleştirildi.
3. Kayıtlar 2015-2019 ve seçilen on ligle sınırlandı. Domestic/international
   ayrımı büyük olasılıkla turnuva/lig sınıflandırmasından üretildi.
4. Oyun süresi dakikaya çevrildi; oyuncu-takım bazında maç, galibiyet ve süre
   toplamları ile ilk/son tarih ve aktif yıllar hesaplandı.
5. Oyuncu adları kararlı `player_node` değerlerine dönüştürüldü.
6. Takım kısa adları ve ad varyasyonları `Teams`/`Teamnames` verisi, kadro
   örtüşmesi ve elle inceleme kullanılarak lineage kümelerine eşlendi.
7. Bu eşleme `team_alias_map.csv` olarak saklandı ve nihai
   `affiliations.csv` üretildi.

## Bilinmeyen ve yeniden üretim için tamamlanması gerekenler

Şunlar ham veri veya eski script bulunmadan kesinleştirilemez:

- Cargo sorgularının tam `fields`, `where`, `join_on`, `limit` ve sayfalama
  parametreleri.
- Domestic/international turnuva listesinin tam tanımı.
- Eksik/iptal/yeniden oynanan maçların ele alınma kuralları.
- Oyuncu adı normalizasyonunun tam algoritması.
- `manual_review` takım lineage kararlarının gerekçeleri.
- API'nin çağrıldığı tarih ve o tarihteki Leaguepedia snapshot'ı.

Gelecekte yeniden toplama yapılırsa ham JSON yanıtları değişmez bir
`data/raw/<collection-date>/` dizininde saklanmalı; sorgu parametreleri, UTC
toplama zamanı, sayfalama, User-Agent ve dosya SHA-256 özetleri bir manifestte
yazılmalıdır. API'ye tekrar tekrar aynı sorguyu göndermek yerine yanıtlar
önbelleğe alınmalı ve makul istek aralığı kullanılmalıdır.
