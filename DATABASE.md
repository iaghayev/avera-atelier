# AVERA ATELIER — Supabase Bazası Qurulması

## 1. Köhnə cədvəlləri silin

Supabase Dashboard → **SQL Editor** → **New Query** → aşağıdakını yapışdırın və **Run** basın:

```sql
DROP TABLE IF EXISTS finances CASCADE;
DROP TABLE IF EXISTS products CASCADE;
DROP TABLE IF EXISTS collections CASCADE;
DROP TABLE IF EXISTS app_settings CASCADE;
```

---

## 2. Yeni cədvəlləri yaradın

Eyni SQL Editor-da bu kodu yapışdırıb **Run** basın:

```sql
-- Sayt parametrləri (1 sətir)
CREATE TABLE app_settings (
  id int PRIMARY KEY DEFAULT 1,
  hero_bg text,
  hero_title jsonb DEFAULT '{"az":"","ru":"","en":""}',
  hero_sub jsonb DEFAULT '{"az":"","ru":"","en":""}',
  hero_btn jsonb DEFAULT '{"az":"","ru":"","en":""}',
  topbar_text jsonb DEFAULT '{"az":"","ru":"","en":""}',
  about_title jsonb DEFAULT '{"az":"","ru":"","en":""}',
  about_text jsonb DEFAULT '{"az":"","ru":"","en":""}',
  whatsapp text,
  instagram text,
  phone text
);

-- İlk sətri yaradın
INSERT INTO app_settings (id, hero_title, hero_sub, hero_btn, topbar_text, about_title)
VALUES (
  1,
  '{"az":"Lüksü İcarəyə Götür","ru":"Арендуй Роскошь","en":"Rent The Luxury"}',
  '{"az":"Premium Kolleksiya","ru":"Премиум Коллекция","en":"Premium Collection"}',
  '{"az":"Kəşf Et","ru":"Исследовать","en":"Explore"}',
  '{"az":"MÖHTƏŞƏM DONLARI İCARƏYƏ GÖTÜRÜN","ru":"АРЕНДА РОСКОШНЫХ ПЛАТЬЕВ","en":"RENT GORGEOUS DRESSES"}',
  '{"az":"Zamansız Zəriflik","ru":"Вечная Элегантность","en":"Timeless Elegance"}'
);

-- Kolleksiyalar
CREATE TABLE collections (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  name jsonb NOT NULL DEFAULT '{"az":"","ru":"","en":""}',
  status text DEFAULT 'active',
  created_at timestamptz DEFAULT now()
);

-- Məhsullar
CREATE TABLE products (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  name jsonb NOT NULL DEFAULT '{"az":"","ru":"","en":""}',
  description jsonb DEFAULT '{"az":"","ru":"","en":""}',
  price numeric DEFAULT 0,
  rental_price numeric DEFAULT 0,
  brand text DEFAULT 'AVERA',
  fabric text,
  tags text,
  sizes text[] DEFAULT '{}',
  images text[] DEFAULT '{}',
  collection_id uuid REFERENCES collections(id) ON DELETE SET NULL,
  status text DEFAULT 'active',
  created_at timestamptz DEFAULT now()
);

-- Maliyyə
CREATE TABLE finances (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  type text NOT NULL,
  amount numeric NOT NULL DEFAULT 0,
  category text,
  description text,
  date date DEFAULT CURRENT_DATE,
  created_at timestamptz DEFAULT now()
);
```

---

## 3. RLS (Row Level Security) qaydaları

Hər cədvəl üçün public oxuma/yazma icazəsi verin:

```sql
-- app_settings
ALTER TABLE app_settings ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public access" ON app_settings FOR ALL USING (true) WITH CHECK (true);

-- collections
ALTER TABLE collections ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public access" ON collections FOR ALL USING (true) WITH CHECK (true);

-- products
ALTER TABLE products ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public access" ON products FOR ALL USING (true) WITH CHECK (true);

-- finances
ALTER TABLE finances ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public access" ON finances FOR ALL USING (true) WITH CHECK (true);
```

---

## 4. Cloudinary Ayarları

Cloudinary hesabınız var:
- Cloud name: `dxigokm01`
- Upload preset: `avera_preset`

Bu artıq admin paneldə konfiqurasiya olunub.

---

## 5. Hazırdır!

İndi admin paneldən (admin.html) bütün məlumatları əlavə edə bilərsiniz.
