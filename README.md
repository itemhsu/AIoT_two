# AIoT_two
## ESP-WHO 是基於樂鑫芯片的圖像處理開發平台，其中包含了實際應用中可能出現的開發示例。

### 概述
ESP-WHO 提供了如人臉檢測、人臉識別、貓臉檢測和手勢識別等示例。您可以基於這些示例，衍生出豐富的實際應用。ESP-WHO 的運行基於 
* ESP-IDF
* ESP-DL https://github.com/espressif/esp-dl 為 ESP-WHO 提供了豐富的深度學習相關接口，配合各種外設可以實現許多有趣的應用。

### 軟件准備
#### 獲取 ESP-IDF
* 請確你的路徑有設定好，可以執行idf.py
#### 獲取 ESP-WHO
在終端運行以下命令，下載 ESP-WHO：
```
git clone --recursive https://github.com/espressif/esp-who.git
```
請記得使用 git submodule update --recursive --init 拉取和更新 ESP-WHO 的所有子模塊。

### 運行示例
ESP-WHO 的所有示例都存放在 examples 中。該文件夾架構如下所示：


```
├── examples
│   ├── cat_face_detection          // 貓臉檢測示例
│   │   ├── lcd                     // 結果顯示方式為 LCD 屏
│   │   └── terminal                // 結果顯示方式為終端
│   ├── code_recognition            // 一維碼/二維碼識別示例
│   ├── human_face_detection        // 人臉檢測示例
│   │   ├── lcd
│   │   └── terminal
│   ├── human_face_recognition      // 人臉識別示例
│   │   ├── lcd
│   │   ├── terminal
│   │   └── README.md               // 示例的具體說明
│   └── motion_detection            // 移動偵測示例
│       ├── lcd 
│       ├── terminal
│       ├── web
│       └── README.rst
```
              
#### 步驟 1：設定目標芯片
打開終端，進入一個示例（例如：examples/human_face_detection/lcd），運行以下命令設定目標芯片：
```
idf.py set-target esp32s3
```
#### （可選）步驟 2：Wi-Fi 配置
若您選擇的示例輸出顯示方式為網頁，可選擇 Wi-Fi Configuration 進入 Wi-Fi 配置，配置 Wi-Fi 密碼等參數，如下圖所示：
#### 步驟 3 : 編譯
```
idf.py fullclean
idf.py build
```
* 請注意編譯輸出指令
```
[100%] Built target app_check_size
[100%] Built target app

Project build complete. To flash, run:
 idf.py flash
or
 idf.py -p PORT flash
or
 python -m esptool --chip esp32s3 -b 460800 --before default_reset --after hard_reset --no-stub write_flash --flash_mode dio --flash_size 8MB --flash_freq 80m 0x0 build/bootloader/bootloader.bin 0x8000 build/partition_table/partition-table.bin 0x10000 build/motion_detection_terminal.bin
or from the "/home/ec2-user/SageMaker/esp/esp-who/examples/motion_detection/terminal/build" directory
 python -m esptool --chip esp32s3 -b 460800 --before default_reset --after hard_reset --no-stub write_flash "@flash_args"
sh-4.2$
```
#### 步驟 4 ：運行和監視
* 下載 bootloader.bin patition.bin 你的app.bin
* 燒錄程序(範例 motion_detection_terminal)，請確保minicom 關閉
```
esptool.py --chip esp32s3 -p /dev/tty.usbmodem11301 erase_flash
esptool.py --chip esp32s3 -p /dev/tty.usbmodem11301 -b 460800 --before default_reset --after hard_reset write_flash --flash_mode dio --flash_size 4MB --flash_freq 40m 0x0 bootloader.bin 0x8000 partition-table.bin 0x10000 motion_detection_terminal.bi
```
* 燒錄成功顯示
```
...
ash_frequency=40m in order to remove this warning, or use the --dont-append-digest option for the elf2image command in order to generate an image file without a hash checksum
Warning: Image file at 0x0 is protected with a hash checksum, so not changing the flash size setting. Use the --flash_size=keep option instead of --flash_size=4MB in order to remove this warning, or use the --dont-append-digest option for the elf2image command in order to generate an image file without a hash checksum
Compressed 22384 bytes to 13820...
Wrote 22384 bytes (13820 compressed) at 0x00000000 in 0.2 seconds (effective 787.1 kbit/s)...
Hash of data verified.
Compressed 3072 bytes to 103...
Wrote 3072 bytes (103 compressed) at 0x00008000 in 0.0 seconds (effective 951.5 kbit/s)...
Hash of data verified.
Compressed 562960 bytes to 266848...
Wrote 562960 bytes (266848 compressed) at 0x00010000 in 3.1 seconds (effective 1441.9 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
```
* minicom 觀察
```
I (17150) motion_detection: Something moved!
I (17390) motion_detection: Something moved!
I (17630) motion_detection: Something moved!
I (18110) motion_detection: Something moved!
I (18350) motion_detection: Something moved!
I (18590) motion_detection: Something moved!
I (18830) motion_detection: Something moved!
```











## Reference
Detail about creating a visual AI Applicaiton

https://docs.espressif.com/projects/esp-dl/zh_CN/latest/esp32/tutorials/deploying-models-through-tvm.html

https://github.com/espressif/esp-dl

https://blog.tensorflow.org/2020/08/announcing-tensorflow-lite-micro-esp32.html

https://github.com/espressif/esp-tflite-micro

https://www.tensorflow.org/lite/microcontrollers?hl=zh-tw


