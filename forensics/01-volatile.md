# A. Order of Volatility — Амьд мэдээллийн шинжилгээ

## 1. Зорилго

WIN-EP01 Windows Server виртуал машинаас дэгдэмхий мэдээлэл цуглуулж, ажиллаж буй процесс, сүлжээний холболт, нэвтэрсэн хэрэглэгч болон автоматаар ажилладаг програмуудыг шинжлэх.

## 2. Ашигласан орчин

- Үйлдлийн систем: Windows Server 2022
- Endpoint: WIN-EP01
- Хэрэгсэл: PowerShell (Administrator)
- Хадгалах хавтас: `C:\Forensics`

## 3. Гүйцэтгэсэн командууд

### 3.1 Процессын мэдээлэл

```powershell
Get-CimInstance Win32_Process |
Select-Object ProcessId, ParentProcessId, Name, CommandLine |
Export-Csv "C:\Forensics\procs.csv" -NoTypeInformation
```

**Үр дүн:** `procs.csv` файл үүссэн. Windows-ийн системийн процессууд, PowerShell, Sysmon болон Wazuh agent бүртгэгдсэн.

### 3.2 Сүлжээний холболт

```powershell
Get-NetTCPConnection |
Where-Object State -eq Established |
Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, OwningProcess |
Export-Csv "C:\Forensics\netconns.csv" -NoTypeInformation
```

**Үр дүн:**

| Local IP | Remote IP | Port | Process |
|---|---|---|---|
| 192.168.56.20 | 192.168.56.10 | 1514 | wazuh-agent.exe |

PID: 2412

Энэ нь Windows endpoint-оос Wazuh сервер рүү үүссэн лабораторийн хэвийн холболт.

### 3.3 Нэвтэрсэн хэрэглэгч

```powershell
query user
query user | Out-File "C:\Forensics\users.txt"
```

**Үр дүн:** Administrator хэрэглэгч console session-д Active төлөвтэй байсан.

### 3.4 Startup мэдээлэл

```powershell
Get-CimInstance Win32_StartupCommand |
Select-Object Name, Command, Location |
Export-Csv "C:\Forensics\startup.csv" -NoTypeInformation
```

**Үр дүн:** `SecurityHealth` бүртгэл илэрсэн.

Команд: `%windir%\system32\SecurityHealthSystray.exe`

## 4. Шинжилгээний дүгнэлт

- Процессын жагсаалтаас шууд хортой гэж батлагдсан процесс илрээгүй.
- Бүртгэгдсэн Established TCP холболт нь Wazuh agent болон Wazuh серверийн хооронд байсан.
- Administrator хэрэглэгч системд идэвхтэй нэвтэрсэн байсан.
- Startup бүртгэлд Windows Security-ийн `SecurityHealth` бүрэлдэхүүн илэрсэн.
- Шалгасан мэдээлэлд сэжигтэй гадаад IP холболт тогтоогдоогүй.

## 5. Хязгаарлалт

Энэ нь мэдээлэл цуглуулсан тухайн агшны төлөвийг харуулна. Өмнө ажиллаад дууссан процесс, тасарсан холболт болон нуугдмал үйл ажиллагааг зөвхөн эдгээр CSV файлаар бүрэн үгүйсгэх боломжгүй.

## 6. Дүгнэлт

WIN-EP01 төхөөрөмжөөс процесс, сүлжээний холболт, хэрэглэгч болон startup мэдээллийг амжилттай цуглуулсан. Шалгасан агшинд илэрсэн Wazuh холболт нь лабораторийн хэвийн ажиллагаатай тохирч байсан. Цуглуулсан файлуудыг цаашдын forensic шинжилгээ болон нотлох баримтын бүртгэлд ашиглах боломжтой.
