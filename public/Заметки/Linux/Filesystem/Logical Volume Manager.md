LVM (Logical Volume Manager) – инструмент для гибкого управления хранилкой

## Physical Volumes

PV (Physical Volume) – блочное устройство, подготовленное для использования LVM.
Одно устройство <=> один physical volume

Инициализация включает:
- Запись метаданных
- Разделение диска на extents. Это аналог блоков/секторов

Создать можно так:
```bash
sudo pvcreate /dev/vdc1
```

Посмотреть все:
```bash
sudo pvs
sudo pvdisplay
```

Удалить PV с диска:
```bash
sudo pvremove /dev/vdc1
```

## Volume Group
VG (Volume Group) – объединение нескольких Physical Volume в один пул (де факто диск) дискового пространства, из которого в будущем можно будет выделять разделы (Logical Volumes)

Создать VG:
```bash
sudo vgcreate vg_name /dev/vdc1 /dev/vdd1
```

## Logical Volume
LV (Logical Volume) – созданный из ресурсов Volume Group раздел (partition), с которым уже можно работать как с обычным partition (те создать ФС и так далее)