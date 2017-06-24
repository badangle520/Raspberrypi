# nano命令
**nano**是一个字符终端的文本编辑器，有点像DOS下的editor程序。它比vi/vim要简单得多，比较适合Linux初学者使用。某些Linux发行版的默认编辑器就是nano。

nano命令可以打开指定文件进行编辑，默认情况下它会自动断行，即在一行中输入过长的内容时自动拆分成几行，但用这种方式来处理某些文件可能会带来问题，比如Linux系统的配置文件，自动断行就会使本来只能写在一行上的内容折断成多行了，有可能造成系统不灵了。因此，如果你想避免这种情况出现，就加上`-w`选项吧。

##编辑命令

- `^` 表示 `Ctrl`，`^G` 表示 `Ctrl + G`
- `M` 表示 `Alt`，`M-W` 表示 `Alt + W`

**光标操作**

```
^P              Move up one line                                    向上移动一行
^N              Move down one line                                  向下移动一行
^F              Move forward one character                          向前移动一格
^B              Move back one character                             向后移动一格
^A              Move to the beginning of the current line           移动到当前行开头
^E              Move to the end of the current line                 移动到当前行末尾
^_ (F13) (M-G)  Go to a specific line number                        跳转到某行
^Space          Move forward one word                               前进一个单词
M-Space         Move backward one word                              后退一个单词
```

**屏幕操作**

```
^Y (F7)         Move to the previous screen                         上一屏幕
^V (F8)         Move to the next screen                             下一屏幕
```


**复制粘贴**

```
^D              Delete the character under the cursor               删除光标后一个字母
^H              Delete the character to the left of the cursor      向左边删一个字母
^R (F5)         Insert another file into the current one            插入其他的文件到当前的文件，而且查找文件的时候支持tab
^I              Insert a tab character                              插入一个tab值
^M              Insert a carriage return at the cursor position     插入一个回车
^6              Mark the current line                               标记当前命令行
^K (F9)         Cut the current line and store it in the cutbuffer  剪切当前命令行并保存在缓冲区
^U (F10)        Uncut from the cutbuffer into the current line      粘贴缓冲区到当前命令行
M-6             copy the current line and store it in the cutbuffer 复制当前整行
```

**搜索替换**

```
^W (F6)         Search for text within the editor                   查找
^.              Replace text within the editor替换
^\ (F14) (M-R)  Search & Replace text within the editor             查找并且替换
M-W             Search for the next text within the editor          定位下一个关键字
M-]             Find other bracket                                  搜索下一个括号
```

**保存退出**

```
^X (F2)         Close currently loaded file/Exit from nano          退出
^O (F3)         Write the current file to disk == ^O WriteOut       保存
^C                                                                  取消返回
```

**其他**

```
^G              Get Help                                            帮助菜单
^J (F4)         Justify the current paragraph                       调整当前段落（配置文件勿用）
^T (F12)        Invoke the spell checker, if available              调用拼写检查程序
^L              Refresh (redraw) the current screen                 刷新当前屏幕
M-<             Open previously loaded file                         打开先前加载的文件
M->             Open next loaded file                               打开下一个加载的文件
M-Z             Suspend enable/disable                              是否悬挂
M-X             Help mode enable/disable                            帮助模式
M-M             Mouse support enable/disable                        鼠标支持
M-Y             Color syntax highlighting enable/disable            语法加亮
```


## 语法
`nano [选项] [[+行,列] 文件名]...`

选项

```
 -h, -?         --help                  显示此信息
 +行,列                                 从所指列数与行数开始
 -A             --smarthome             启用智能 HOME 键
 -B             --backup                储存既有文件的备份
 -C <目录>      --backupdir=<目录>      用以储存独一备份文件的目录
 -D             --boldtext              用粗体替代颜色反转
 -E             --tabstospaces          将已输入的制表符转换为空白
 -F             --multibuffer           启用多重文件缓冲区功能
 -H             --historylog            记录与读取搜索/替换的历史字符串
 -I             --ignorercfiles         不要参考nanorc 文件
 -K             --rebindkeypad          修正数字键区按键混淆问题
 -L             --nonewlines            不要将换行加到文件末端
 -N             --noconvert             不要从 DOS/Mac 格式转换
 -O             --morespace             编辑时多使用一行
 -Q <字符串>    --quotestr=<字符串>     引用代表字符串
 -R             --restricted            限制模式
 -S             --smooth                按行滚动而不是半屏
 -T <#列数>     --tabsize=<#列数>       设定制表符宽度为 #列数
 -U             --quickblank            状态行快速闪动
 -V             --version               显示版本资讯并离开
 -W             --wordbounds            更正确地侦测单字边界
 -Y <字符串>    --syntax=<字符串>       用于加亮的语法定义
 -c             --const                 持续显示游标位置
 -d             --rebinddelete          修正退格键/删除键混淆问题
 -i             --autoindent            自动缩进新行
 -k             --cut                   从游标剪切至行尾
 -l             --nofollow              不要依照符号连结，而是覆盖
 -m             --mouse                 启用鼠标功能
 -o <目录>      --operatingdir=<目录>   设定操作目录
 -p             --preserve              保留XON (^Q) 和XOFF (^S) 按键
 -q             --quiet                 沉默忽略启动问题, 比如rc 文件错误
 -r <#列数>     --fill=<#列数>          设定折行宽度为 #列数
 -s <程序>      --speller=<程序>        启用替代的拼写检查程序
 -t             --tempfile              离开时自动储存，不要提示
 -u             --undo                  允许通用撤销[试验性特性]
 -v             --view                  查看(只读)模式
 -w             --nowrap                不要自动换行
 -x             --nohelp                不要显示辅助区
 -z             --suspend               启用暂停功能
 -$             --softwrap              启用软换行
 -a, -b, -e,
 -f, -g, -j                             (忽略，为与pico 相容)
```


