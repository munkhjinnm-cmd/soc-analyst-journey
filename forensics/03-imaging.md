# C. Disk Imaging and Hash Verification

## 1. Зорилго
Ubuntu Linux орчинд 50 MiB хэмжээтэй туршилтын нотолгоо үүсгэж, хуулбар авч, SHA256 болон byte-by-byte харьцуулалтаар өгөгдлийн бүрэн бүтэн байдлыг баталгаажуулах.

## 2. Ашигласан орчин
- Үйлдлийн систем: Ubuntu Linux VM
- Хэрэгсэл: dd, sha256sum, cp, cmp
- Ажлын хавтас: `~/forensics-lab/`
- Эх файл: `evidence.raw`
- Хуулбар: `evidence-copy.raw`

## 3. Гүйцэтгэсэн командууд

```bash
mkdir -p ~/forensics-lab
cd ~/forensics-lab
dd if=/dev/zero of=evidence.raw bs=1M count=50
echo "secret data" >> evidence.raw
ls -lh evidence.raw

sha256sum evidence.raw > evidence.raw.sha256
cp evidence.raw evidence-copy.raw
sha256sum evidence.raw evidence-copy.raw
sha256sum -c evidence.raw.sha256
cmp evidence.raw evidence-copy.raw && echo "IMAGE VERIFIED: IDENTICAL"
```

## 4. Шинжилгээний үр дүн

| Үзүүлэлт | Үр дүн |
|---|---|
| Файлын хэмжээ | 52,428,812 bytes |
| Hash алгоритм | SHA256 |
| Эх файлын hash | `1b5dc314d1929b9ace86e7322ea657e45e95084105139bd64b733ec39d17d1be` |
| Хуулбарын hash | Эх файлтай ижил |
| Hash verification | `evidence.raw: OK` |
| Byte comparison | `IMAGE VERIFIED: IDENTICAL` |

## 5. Hash шалгахын ач холбогдол

Imaging хийхийн өмнө эх нотолгооны hash-ийг бүртгэснээр өгөгдлийн анхны төлөвийг дараа нь харьцуулах боломж бүрдэнэ.

Imaging хийсний дараа хуулбарын hash-ийг эх нотолгоотой харьцуулснаар хуулбарлах явцад өгөгдөл өөрчлөгдсөн эсэхийг шалгана.

Энэ лабораторид хоёр файлын SHA256 hash ижил гарсан бөгөөд `cmp` команд байт бүрээрээ ижил болохыг баталсан.

## 6. Дүгнэлт

Ubuntu Linux VM дээр 50 MiB хэмжээтэй туршилтын файл үүсгэж, түүнд нэмэлт өгөгдөл бичин хуулбар авсан. SHA256 болон `cmp` шалгалтаар эх файл, хуулбар хоёр ижил болох нь батлагдсан.

Энэ туршилт нь файлын түвшний хуулбарлалт бөгөөд бодит дискний forensic acquisition-ийн бүх ажиллагааг орлохгүй.

## 7. Нотлох баримт
### Screenshot 1. Disk imaging болон SHA256 баталгаажуулалт
<img width="902" height="381" alt="Screenshot 2026-11-04 174353" src="https://github.com/user-attachments/assets/1feca480-b94d-4364-8afb-123b686d540d" />
**Тайлбар:** Ubuntu Linux дээр evidence.raw файлыг
үүсгэж, evidence-copy.raw хуулбар авсан.
Хоёр файлын SHA256 hash ижил гарсан.
Мөн evidence.raw: OK болон
IMAGE VERIFIED: IDENTICAL үр дүнгээр
бүрэн бүтэн байдал баталгаажсан.
