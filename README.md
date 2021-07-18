# Sxmo-Nexus7-2012-rev-e1565-wifi-3G
PostmarketOS

DE Sxmo(gần giống i3wm) lên nexus 7 2012 wifi/3G 8Gb/16Gb/32Gb asus-grouper(trên 300 thiết bị được hỗ trợ ngay thời điểm này) mainline kernel linux 5.6.0-rc2-next-20200218



hps://twitter.com/postmarketOS



Trước tiên kiểm tra máy là PM269 hay E1565 bằng cách:



Variants

grouper rev. PM269 - without GSM (oldest)
grouper rev. E1565 - without GSM (modern revision)
tilapia rev. E1565 - with GSM

Do I have grouper or tilapia?



TWRP (adb shell) $ grep androidboot.baseband=unknown /proc/cmdline && echo grouper || echo tilapia

Which hardware revision of grouper do I have?

TWRP (adb shell) $ find /sys/devices/ | grep -c max776 && echo You have E1565





TWRP (adb shell) $ find /sys/devices/ | grep -c tps6591 && echo You have PM269



Để PC nhận Nexus 7 2012 và flash kernel và push rootfs nhanh thì nên rút hết các thiết bị usb khác ngoài Nexus 7



[MEDIA=youtube]1_bsycXrFcI[/MEDIA]



Thêm repositories để cài package trong terminal



$ nano /etc/apk/repositories



Bỏ dấu “#” trước http://dl-cdn.alpinelinux.org/alpine/edge/testing/



Cài pmbootstrap trên alpine linux trên user đã tạo, chú ý các bước



$ sudo apk add python3 coreutils procps git android-tools pmbootstrap



$ pmbootstrap init



$ […]: edge



$ […]: mainline



$ […]: asus



$ […]: grouper



$ […]: e1565 (PM269 như trên)



$ […]: sxmo



$ […, extra packages]: nano,vim



$ pmbootstrap status



$ pmbootstrap --details-to-stdout install --android-recovery-zip --recovery-install-partition=data



$ pmbootstrap export



File .img, kernel và .zip trong home/<username>/.local/var/pmbootstrap/ thư mục chroot-buildroot-armv7/var/lib và chroot-asus-grouper/boot hoặc chroot-asus-grouper/home/<user>/



Root và unlock bootloader Nexus 7 dùng tool



http://www.mediafire.com/file/d9kssrtg1dtnx3l/NRT_v2.1.9.sfx.zip/file



Cài đặt và mở TWRP-3.3.1-0-grouper.img tải tại đây:



https://dl.twrp.me/grouper/



*** CÀI ĐẶT VÀO /userdata partition ***



Phải sửa file deviceinfo trong:



$ nano /home/<username>/.local/var/pmbootstrap/cache_git/pmaports/device/testing/deviceinfo



Sửa dòng: deviceinfo_flash_fastboot_max_size="650" thành "13773" (Nexus 7 8gb là 5829,

nexus 7 32gb là 27944)



Flash kernel vào boot partition trên máy:



$ pmbootstrap flasher flash_kernel



Mở TWRP trên nexus 7(nhấn nút nguồn + nút giảm âm lượng) và cắm vào máy tính, làm theo các bước sau:



(computer) $ adb shell

(twrp) $ df # look for the data partition. should be something like /dev/block/mmcblk0p9 or p10 with /data next to it

(twrp) $ umount /dev/block/mmcblk0p__ <- fill paron number

(twrp) $ umount /dev/block/mmcblk0p__ <- fill paron number # again, it can be mounted as /sdcard and as /data

(computer again) $ adb push /tmp/postmarketOS-export/asus-grouper.img /dev/block/mmcblk0p__ <- fill partition number



Với điều kiện:

grouper(bản wifi) /dev/block/mmcblk0p9

tilapia(bản 3G) /dev/block/mmcblk0p10



Config sysctl.conf có sự chỉnh sửa theo KTweak trên github của tytydraco (xda-developers)



Tham khảo:

https://github.com/tytydraco/KTweak/tree/latency



$ sudo nano /etc/sysctl.conf



# content of this file will override /etc/sysctl.d/*



vm.swappiness=20

vm.vfs_cache_pressure=80

vm.dirty_background_bytes=16777216

vm.dirty_bytes=33554432

vm.dirty_background_ratio=1

vm.dirty_ratio=2

vm.dirty_writeback_centisecs=2000

vm.dirty_expire_centisecs=1000

vm.lowmem_reserve_ratio=256 32 32

vm.min_free_kbytes=4096

vm.user_reserve_kbytes=8192

vm.admin_reserve_kbytes=4096

vm.panic_on_oom=1

kernel.panic=1

kernel.panic_on_oops=1

vm.overcommit_memory=0

vm.overcommit_ratio=50

vm.drop_caches=3

vm.laptop_mode=5

vm.mmap_min_addr=8192

vm.oom_kill_allocating_task=1

vm.extfrag_threshold=750

vm.oom_dump_tasks=0

vm.page-cluster=0

vm.stat-interval=10

vm.compact_unevictable_allowed=0

vm.highmem_is_dirtyable=1



kernel.tainted=0

kernel.threads-max=5000

kernel.usermodehelper.bset=4294967295 4294967295

kernel.usermodehelper.inheritable=4294967295 4294967295

kernel.printk=4 4 1 7

kernel.kptr_restrict=1

kernel.randomize_va_space=2

kernel.keys.root_maxkeys=200

kernel.keys.root_maxbytes=20000

kernel.perf_event_paranoid=1

kernel.perf_cpu_time_max_percent=3

kernel.shmmax=268435456

kernel.shmall=2097152

kernel.msgmni=2048

kernel.msgmax=64000

kernel.sem=500 512000 64 2048

kernel.auto_msgmni=1

kernel.sched_domain.cpu0.domain0.min_interval=1

kernel.sched_domain.cpu0.domain0.max_interval=4

kernel.sched_domain.cpu0.domain0.busy_factor=64

kernel.sched_domain.cpu1.domain0.min_interval=1

kernel.sched_domain.cpu1.domain0.max_interval=4

kernel.sched_domain.cpu1.domain0.busy_factor=64

kernel.sched_domain.cpu2.domain0.min_interval=1

kernel.sched_domain.cpu2.domain0.max_interval=4

kernel.sched_domain.cpu2.domain0.busy_factor=64

kernel.sched_domain.cpu3.domain0.min_interval=1

kernel.sched_domain.cpu3.domain0.max_interval=4

kernel.sched_domain.cpu3.domain0.busy_factor=64

kernel.sched_child_runs_first=0

kernel.sched_tunable_scaling=0

kernel.sched_latency_ns=18000000

kernel.sched_min_granularity_ns=1500000

kernel.sched_migration_cost_ns=5000000

kernel.sched_nr_migrate=4

kernel.sched_wakeup_granularity_ns=3000000

kernel.hung_task_timeout_secs=0



net.ipv4.tcp_ecn=1

net.ipv4.tcp_fastopen=3

net.ipv4.tcp_timestamps=0

net.ipv4.tcp_tw_reuse=1

net.ipv4.tcp_wmem=6144 87380 524288

net.ipv4.tcp_rmem=6144 87380 524288

net.core.wmem_max=524288

net.core.rmem_max=524288



$ sudo sysctl -p



$ sudo nano /etc/local.d/cpufreq.start



# Set the governor to ondemand for all processors

for cpu in /sys/devices/system/cpu/cpufreq/policy*; do

echo ondemand > ${cpu}/scaling_governor

done



# Reduce the boost ignore_nice_load to 0

echo 0 > /sys/devices/system/cpu/cpufreq/ondemand/ignore_nice_load



# Reduce the boost io_is_busy to 1

echo 1 > /sys/devices/system/cpu/cpufreq/ondemand/io_is_busy



# Reduce the boost powersave_bias to 0 <-- tăng giảm xung của cpu/gpu

echo 0 > /sys/devices/system/cpu/cpufreq/ondemand/powersave_bias



# Reduce the boost sampling_down_factor to 10

echo 10 > /sys/devices/system/cpu/cpufreq/ondemand/sampling_down_factor



# Reduce the boost sampling_rate to 40000

echo 40000 > /sys/devices/system/cpu/cpufreq/ondemand/sampling_rate



# Reduce the boost threshold to 95%

echo 95 > /sys/devices/system/cpu/cpufreq/ondemand/up_threshold



for queue in /sys/block/*/queue

do

# Choose the first governor available

avail_scheds="$(cat "$queue/scheduler")"

for sched in cfq noop kyber bfq mq-deadline none

do

if [[ "$avail_scheds" == *"$sched"* ]]

then

echo "$sched" > "$queue/scheduler"

break

fi

done



# Do not use I/O as a source of randomness

# echo 0 > "$queue/add_random"



# Disable I/O statistics accounting

echo 0 > "$queue/iostats"



# Reduce heuristic read-ahead in exchange for I/O latency

echo 256 > "$queue/read_ahead_kb"



# Reduce the maximum number of I/O requests in exchange for latency

echo 512 > "$queue/nr_requests"



echo 128 > "$queue/max_sectors_kb"



echo 2 > "$queue/rq_affinity"

done



$ sudo chmod +x /etc/local.d/cpufreq.start

$ sudo rc-update add local default

$ sudo lbu commit



[MEDIA=youtube]QgbKK5xPog8[/MEDIA]
