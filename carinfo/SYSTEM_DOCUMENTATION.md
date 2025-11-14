# CarInfo Veri Yapısı ve Path Sistemi Dokümantasyonu

## Genel Bakış

Bu sistem, araç markaları, seriler, modeller ve varyantlar için hiyerarşik bir JSON dosya yapısı kullanır. Tüm path'ler mutlak (absolute) path formatında tutulur ve `brands/` klasöründen başlar.

## Dosya Yapısı Hiyerarşisi

```
carinfo.json (Ana index - tüm markalar)
└── brands/
    └── {marka_slug}/
        └── models.json (Marka serileri index'i)
            └── brands/{marka_slug}/models/
                └── {seri_slug}/
                    └── {seri_slug}.json (Seri modelleri index'i)
                        └── brands/{marka_slug}/models/{seri_slug}/{model_slug}/
                            └── {model_slug}.json (Model varyantları index'i)
                                └── brands/{marka_slug}/models/{seri_slug}/{model_slug}/{variant_slug}.json (Varyant detayları)
```

## 1. Ana Index: `carinfo.json`

**Konum:** Proje kök dizini

**Yapı:**
```json
{
  "source": "carinfo.com",
  "extraction_date": "2025-11-10",
  "brand": [
    {
      "ad": "BMW",
      "path": "brands/bmw/models.json"
    },
    {
      "ad": "Audi",
      "path": "brands/audi/models.json"
    }
    // ... diğer markalar A-Z sıralı
  ]
}
```

**Özellikler:**
- `brand` array'i alfabetik olarak sıralıdır (A-Z)
- Her marka için `ad` (Türkçe: ad) ve `path` (mutlak path) içerir
- Path formatı: `brands/{marka_slug}/models.json`

## 2. Marka Index: `brands/{marka_slug}/models.json`

**Örnek:** `brands/bmw/models.json`

**Yapı:**
```json
{
  "brand": "BMW",
  "series": [
    {
      "ad": "1 Series (2004 -)",
      "path": "brands/bmw/models/1_series_2004/1_series_2004.json"
    },
    {
      "ad": "3 Series (1975 -)",
      "path": "brands/bmw/models/3_series_1975/3_series_1975.json"
    }
    // ... diğer seriler
  ]
}
```

**Özellikler:**
- `brand`: Marka adı (string)
- `series`: Seri listesi (array)
- Her seri için `ad` ve mutlak `path` içerir
- Path formatı: `brands/{marka_slug}/models/{seri_slug}/{seri_slug}.json`

## 3. Seri Index: `brands/{marka_slug}/models/{seri_slug}/{seri_slug}.json`

**Örnek:** `brands/bmw/models/1_series_2004/1_series_2004.json`

**Yapı:**
```json
{
  "ad": "1 Serisi (2004 -)",
  "modeller": [
    {
      "ad": "1 Serisi 5-kapi (2004-2024)",
      "path": "brands/bmw/models/1_series_2004/1_series_5_door_2004_2024/1_series_5_door_2004_2024.json"
    },
    {
      "ad": "1 Serisi 3-kapi (2007-2019)",
      "path": "brands/bmw/models/1_series_2004/1_series_3_door_2007_2019/1_series_3_door_2007_2019.json"
    }
    // ... diğer modeller
  ]
}
```

**Özellikler:**
- `ad`: Seri adı (Türkçe)
- `modeller`: Model listesi (array)
- Her model için `ad` ve mutlak `path` içerir
- Path formatı: `brands/{marka_slug}/models/{seri_slug}/{model_slug}/{model_slug}.json`

## 4. Model Index: `brands/{marka_slug}/models/{seri_slug}/{model_slug}/{model_slug}.json`

**Örnek:** `brands/bmw/models/1_series_2004/1_series_5_door_2004_2024/1_series_5_door_2004_2024.json`

**Yapı:**
```json
{
  "ad": "1 Serisi 5-kapi (2004-2024)",
  "varyantlar": [
    {
      "ad": "116i",
      "path": "brands/bmw/models/1_series_2004/1_series_5_door_2004_2024/116i.json",
      "yakit_tipi": "B",
      "yillar": "2004-2007",
      "guc_bg": 116,
      "hizlanma_0_100_kmh": "10.80 sec",
      "cekis": "Iki Ceker",
      "sanzimanlar": ["M"],
      "nesil": "E87"
    }
    // ... diğer varyantlar
  ]
}
```

**Özellikler:**
- `ad`: Model adı (Türkçe)
- `varyantlar`: Varyant listesi (array)
- Her varyant için özet bilgiler ve mutlak `path` içerir
- Path formatı: `brands/{marka_slug}/models/{seri_slug}/{model_slug}/{variant_slug}.json`

**Varyant Özet Alanları:**
- `yakit_tipi`: "B" (Benzin), "D" (Dizel), "E" (Elektrik), "H" (Hibrit), "MH/B" (Mild Hybrid/Benzin)
- `yillar`: Üretim yılları (örn: "2004-2007")
- `guc_bg`: Güç (beygir gücü - integer)
- `hizlanma_0_100_kmh`: 0-100 km/h hızlanma süresi (string)
- `cekis`: "Iki Ceker", "Dort Ceker", "Onden Cekis", "Arkadan Cekis"
- `sanzimanlar`: ["M"] (Manuel), ["O"] (Otomatik), ["M", "O"] (Her ikisi)
- `nesil`: Nesil kodu (örn: "E87", "F20")

## 5. Varyant Detay: `brands/{marka_slug}/models/{seri_slug}/{model_slug}/{variant_slug}.json`

**Örnek:** `brands/bmw/models/1_series_2004/1_series_5_door_2004_2024/116i.json`

**Yapı:**
```json
{
  "yakit_tipi": "B",
  "ad": "116i",
  "yillar": "2004-2007",
  "hizlanma_0_100_kmh": "10.80 sec",
  "guc_bg": 116,
  "cekis": "Iki Ceker",
  "sanzimanlar": ["M"],
  "nesil": "E87",
  "detaylar": {
    "genel_bakis": { /* ... */ },
    "motor_ve_performans": { /* ... */ },
    "sanziman_ve_aktarma": { /* ... */ },
    "yakit_ve_emisyon": { /* ... */ },
    "boyutlar_ve_agirlik": { /* ... */ },
    "guvenlik": { /* ... */ },
    "donanim": { /* ... */ },
    // ... diğer detay bölümleri
  }
}
```

**Özellikler:**
- Üst seviye özet bilgiler (varyant index'teki gibi)
- `detaylar`: Tüm teknik detaylar (nested object)

## Slug Oluşturma Kuralları

### Marka Slug'ı
- Küçük harfe çevir
- Boşlukları ve özel karakterleri alt çizgiye çevir (`_`)
- Türkçe karakterleri normalize et (ı→i, ğ→g, ü→u, ş→s, ö→o, ç→c)
- Birden fazla alt çizgiyi tek alt çizgiye çevir
- Başta ve sonda alt çizgi varsa temizle

**Örnekler:**
- "Mercedes-Benz" → `mercedes_benz`
- "Alfa Romeo" → `alfa_romeo`
- "DS Automobiles" → `ds_automobiles`
- "Lynk & Co" → `lynk_co`
- "Tofaş" → `tofas`

### Seri/Model/Varyant Slug'ı
- Aynı kurallar geçerli
- Yıl bilgisi varsa ekle (örn: `1_series_2004`)
- Özel karakterler ve boşluklar alt çizgiye çevrilir
- Dosya adı olarak kullanılır: `{slug}.json`

**Örnekler:**
- "1 Series (2004 -)" → `1_series_2004`
- "1 Serisi 5-kapi (2004-2024)" → `1_series_5_door_2004_2024`
- "116i 3-door" → `116i_3_door`

## Path Formatı Kuralları

1. **Tüm path'ler mutlak (absolute) path formatında:**
   - `brands/` ile başlar
   - Proje kök dizininden itibaren tam yol

2. **Path formatları:**
   - Marka: `brands/{marka_slug}/models.json`
   - Seri: `brands/{marka_slug}/models/{seri_slug}/{seri_slug}.json`
   - Model: `brands/{marka_slug}/models/{seri_slug}/{model_slug}/{model_slug}.json`
   - Varyant: `brands/{marka_slug}/models/{seri_slug}/{model_slug}/{variant_slug}.json`

3. **Path'ler her zaman forward slash (`/`) kullanır** (Unix/Linux formatı)

## Türkçe Anahtar İsimleri

Sistem Türkçe anahtar isimleri kullanır:

| İngilizce | Türkçe |
|-----------|--------|
| `name` | `ad` |
| `models` | `modeller` |
| `variants` | `varyantlar` |
| `series` | `seriler` (bazı yerlerde) |
| `fuel_type` | `yakit_tipi` |
| `power_hp` | `guc_bg` |
| `drive` | `cekis` |
| `transmissions` | `sanzimanlar` |
| `generation` | `nesil` |
| `years` | `yillar` |
| `acceleration_0_100_kmh` | `hizlanma_0_100_kmh` |
| `details` | `detaylar` |
| `overview` | `genel_bakis` |
| `engine_and_performance` | `motor_ve_performans` |
| `transmission_and_drivetrain` | `sanziman_ve_aktarma` |
| `fuel_and_emissions` | `yakit_ve_emisyon` |
| `dimensions_and_weight` | `boyutlar_ve_agirlik` |
| `safety_and_security` | `guvenlik` |
| `equipment` | `donanim` |

## Değer Çevirileri

### Yakıt Tipleri
- `P` → `B` (Benzin)
- `D` → `D` (Dizel)
- `E` → `E` (Elektrik)
- `H` → `H` (Hibrit)
- `MH/P` → `MH/B` (Mild Hybrid/Benzin)

### Çekiş Tipleri
- `2WD` → `Iki Ceker`
- `4WD` → `Dort Ceker`
- `AWD` → `Dort Ceker`
- `FWD` → `Onden Cekis`
- `RWD` → `Arkadan Cekis`

### Şanzıman Tipleri
- `M` → `M` (Manuel)
- `A` → `O` (Otomatik)
- `AM` → `MO` (Manuel/Otomatik)
- `AT` → `OT` (Otomatik)
- `MT` → `MT` (Manuel)
- `CVT` → `CVT` (CVT)

## Veri Çekme ve İşleme İçin Öneriler

### 1. Marka Listesi Çekme
```python
# carinfo.json'dan tüm markaları oku
with open('carinfo.json', 'r', encoding='utf-8') as f:
    data = json.load(f)
    brands = data['brand']  # Alfabetik sıralı marka listesi
```

### 2. Marka Slug'ından Path Oluşturma
```python
def get_brand_path(brand_slug):
    return f"brands/{brand_slug}/models.json"
```

### 3. Seri Path'ini Oluşturma
```python
def get_series_path(brand_slug, series_slug):
    return f"brands/{brand_slug}/models/{series_slug}/{series_slug}.json"
```

### 4. Model Path'ini Oluşturma
```python
def get_model_path(brand_slug, series_slug, model_slug):
    return f"brands/{brand_slug}/models/{series_slug}/{model_slug}/{model_slug}.json"
```

### 5. Varyant Path'ini Oluşturma
```python
def get_variant_path(brand_slug, series_slug, model_slug, variant_slug):
    return f"brands/{brand_slug}/models/{series_slug}/{model_slug}/{variant_slug}.json"
```

### 6. Slug Oluşturma Fonksiyonu
```python
import re
import unicodedata

def create_slug(text):
    # Türkçe karakterleri normalize et
    text = unicodedata.normalize('NFD', text)
    text = text.lower()
    
    # Türkçe karakter dönüşümleri
    replacements = {
        'ı': 'i', 'ğ': 'g', 'ü': 'u', 'ş': 's', 'ö': 'o', 'ç': 'c',
        'İ': 'i', 'Ğ': 'g', 'Ü': 'u', 'Ş': 's', 'Ö': 'o', 'Ç': 'c'
    }
    for old, new in replacements.items():
        text = text.replace(old, new)
    
    # Özel karakterleri temizle ve alt çizgiye çevir
    text = re.sub(r'[^\w\s-]', '', text)
    text = re.sub(r'[-\s]+', '_', text)
    text = re.sub(r'_+', '_', text)
    text = text.strip('_')
    
    return text
```

### 7. Veri Ekleme Akışı

1. **Marka Ekleme:**
   - `carinfo.json`'a marka ekle (alfabetik sıraya göre)
   - `brands/{slug}/` klasörü oluştur
   - `brands/{slug}/models.json` oluştur: `{"brand": "Marka Adı", "series": []}`

2. **Seri Ekleme:**
   - `brands/{slug}/models.json`'a seri ekle
   - `brands/{slug}/models/{seri_slug}/` klasörü oluştur
   - `brands/{slug}/models/{seri_slug}/{seri_slug}.json` oluştur: `{"ad": "...", "modeller": []}`

3. **Model Ekleme:**
   - Seri index'ine model ekle
   - `brands/{slug}/models/{seri_slug}/{model_slug}/` klasörü oluştur
   - Model index dosyası oluştur: `{"ad": "...", "varyantlar": []}`

4. **Varyant Ekleme:**
   - Model index'ine varyant özeti ekle
   - Varyant detay dosyası oluştur: `{variant_slug}.json`

## Önemli Notlar

1. **Path Tutarlılığı:** Tüm path'ler mutlak olmalı ve `brands/` ile başlamalı
2. **Slug Tutarlılığı:** Aynı isim için her zaman aynı slug üretilmeli
3. **Türkçe Karakterler:** Dosya adlarında İngilizce karakterler kullanılır, JSON içeriğinde Türkçe kullanılır
4. **Alfabetik Sıralama:** `carinfo.json` içindeki markalar A-Z sıralı olmalı
5. **Boş Array'ler:** Yeni oluşturulan index dosyalarında array'ler boş başlar: `[]`

## Örnek Kullanım Senaryosu

GitHub'dan veri çekerken:

1. Marka listesini `carinfo.json`'dan al
2. Her marka için `brands/{slug}/models.json` dosyasını kontrol et
3. Eğer seri yoksa, GitHub'dan seri listesini çek
4. Her seri için klasör ve index dosyası oluştur
5. Seri içindeki modelleri çek ve model klasörleri oluştur
6. Model içindeki varyantları çek ve varyant dosyaları oluştur
7. Her seviyede path'leri mutlak formatında tut

Bu yapı sayesinde hiyerarşik veri yapısı korunur ve her seviyede index dosyaları üzerinden navigasyon yapılabilir.

