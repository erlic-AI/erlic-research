# Erlic AI — Research Overview

## Kısa tanım

Erlic AI, Türkçe ve morfolojik açıdan zengin dillerde dil modelleme ve muhakeme performansını hesaplama bütçesi başına iyileştirmeyi araştıran açık kaynaklı bir girişimdir.

## Problem

Türkçe; eklemeli morfolojisi, uzun sözcük biçimleri ve serbest sözdizimi nedeniyle çok dilli modellerde veri temsili ve muhakeme açısından özel değerlendirme gerektirir. Bu proje, Türkçe odaklı veri hazırlama ve değerlendirmeyi seyrek Mixture-of-Experts (MoE) mimarisi ve doğrulanabilir post-training yöntemleriyle incelemeyi amaçlar.

## Mevcut teknik prototip

`config.py` içindeki `proto_ayar()` yapılandırması analitik olarak 3,916,275,712 toplam ve 1,217,241,088 aktif parametre verir. Mimari kodunda 24 Transformer katmanı, GQA, RoPE, RMSNorm, top-3 yönlendirmeli 16 uzman ve paylaşılan uzman bulunur.

Bu sayı yapılandırma hesabıdır. Modelin eğitildiği, dil kalitesinin kanıtlandığı veya ölçekli performans elde edildiği anlamına gelmez.

## Araştırma sorusu

Aynı veri ve hesaplama bütçesinde:
1. Türkçe odaklı veri karışımı,
2. seyrek MoE uzman yönlendirmesi,
3. supervised fine-tuning (SFT),
4. doğrulanabilir ödül tabanlı öğrenme (RLVR),

Türkçe matematik, mantık ve kod görevlerinde performansı artırabilir mi?

## Deney planı

- Dense baseline ile MoE karşılaştırması.
- Aynı token bütçesinde veri karışımı ablation'ı.
- SFT öncesi/sonrası karşılaştırma.
- RLVR öncesi/sonrası karşılaştırma.
- Türkçe ve İngilizce sonuçların ayrı raporlanması.
- GPU saat başına başarı, token/s, tepe VRAM ve eğitim kararlılığının raporlanması.

## Ölçümler

- Validation cross-entropy/perplexity.
- Matematik ve mantık doğruluğu.
- Kod görevlerinde test geçme oranı.
- Router yük dengesi.
- Token/s ve adım süresi.
- GPU saati başına benchmark gelişimi.
- Eğitim kaybı, gradient normu ve başarısız adım sayısı.

## Açık araştırma taahhüdü

Kod, deney konfigürasyonları, benchmark tanımları ve lisansı uygun veri/model artefaktları yayımlanacaktır. Kaynak veri lisansları ayrı ayrı incelenecek; yeniden dağıtım izni olmayan ham veriler depoya eklenmeyecektir.

## Şu anki sınırlar

- Ölçekli pretraining sonucu henüz raporlanmamıştır.
- 20B sentetik token üretimi tamamlanmış bir çıktı değildir.
- SWE-Bench/Terminal-Bench entegrasyonu veya güvenli Docker sandbox'ı tamamlanmış kabul edilmemelidir.
- Mevcut çoklu GPU eğitim yolu DDP'dir; DDP her GPU'da tam model kopyası tutar.
- FSDP/ZeRO/tensor-parallel desteği ayrıca geliştirilip test edilmelidir.
