# E. Evidence Handling — Chain of Custody

## 1. Зорилго

Windows Server болон Ubuntu Linux лабораторийн орчноос цуглуулсан тоон нотлох баримтын гарал үүсэл, бүрэн бүтэн байдал, хадгалалт, шалгалтын үр дүнг бүртгэх.

## 2. Нотлох баримтын бүртгэл

| ID | Нотлох баримт | Эх үүсвэр | SHA256 |
|---|---|---|---|
| EV-01 | procs.csv | WIN-EP01 | D98F49771C4A2141048966A83570FC982CD0F696AD6BDD87367AA8E677A9A183 |
| EV-02 | netconns.csv | WIN-EP01 | FAAD2AFDBC344585B7869830E765B43E4F8F23A672AA4104BEA4A68F94B57162 |
| EV-03 | startup.csv | WIN-EP01 | 0C087698CE28588076E8291CBE3B168AA9E81332561F78ADDE13E767B9400941 |
| EV-04 | users.txt | WIN-EP01 | 3C22C9E92AD75A2819F52D434A5D0FB98038340304C3FEBE9180206CC286AE98 |
| EV-05 | evidence.raw | Ubuntu Linux VM | 1b5dc314d1929b9ace86e7322ea657e45e95084105139bd64b733ec39d17d1be |
| EV-06 | evidence-copy.raw | Ubuntu Linux VM | 1b5dc314d1929b9ace86e7322ea657e45e95084105139bd64b733ec39d17d1be |
| EV-07 | 04-timeline.md | Windows Downloads | 65AEA70234AD598192187AA3AD9EC294DDC5A8146D270E5A09E7BCDA993317 |

## 3. Chain of Custody

- **Цуглуулсан орчин:** WIN-EP01, Ubuntu Linux VM
- **Цуглуулсан арга:** PowerShell, dd, cp
- **Бүрэн бүтэн байдлын шалгалт:** SHA256
- **Windows нотлох баримтын хадгалалт:** C:\Forensics\
- **Linux нотлох баримтын хадгалалт:** ~/forensics-lab/
- **Timeline файлын хадгалалт:** C:\Users\USER\Downloads\
- **Цуглуулсан огноо:** 2026-11-04
- **Хариуцагч:** Лабораторийн ажил гүйцэтгэгч
- **Шилжүүлсэн этгээд, цаг, зорилго:** Бүртгэгдээгүй

## 4. Баталгаажуулалтын үр дүн

Ubuntu Linux дээр evidence.raw болон evidence-copy.raw файлуудын SHA256 hash ижил гарсан.

`sha256sum -c evidence.raw.sha256` шалгалтаар `evidence.raw: OK` үр дүн гарсан.

`cmp` командаар хоёр файл байт түвшинд ижил болохыг баталгаажуулсан.

Windows дээр volatile CSV болон timeline файлын SHA256 утгуудыг бүртгэсэн.

## 5. Дүгнэлт

Лабораторийн хүрээнд цуглуулсан нотлох баримтуудын SHA256 hash-ийг бүртгэж, дискний image-ийн хуулбар эх файлтай ижил болохыг баталгаажуулсан. Эдгээр hash нь цаашид файлын өөрчлөлтийг илрүүлэх суурь үзүүлэлт болно. Бүрэн chain of custody бүрдүүлэхийн тулд нотлох баримт бүрийн цуглуулсан цаг, хариуцагч, хадгалалтын болон шилжүүлгийн түүхийг нэмж бүртгэх шаардлагатай.

**Төлөв:** Hash бүртгэл хийгдсэн; chain of custody-ийн зарим мэдээлэл дутуу.
