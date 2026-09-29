# schoolverify_code 用于计算破解皆成守护软件的校验码逻辑


#原理
##变换函数 E(x)
E(x) = 取 MD5(x) 的 32 位小写十六进制串
       将字母 a→1, b→2, c→3, d→4, e→5, f→6 进行替换
       取替换后结果的前 6 位

##挑战码生成
challenge = E(salt + 当前毫秒时间戳)
App 启动或刷新验证界面时，取当前时间的毫秒级时间戳（System.currentTimeMillis()）
将 salt 字符串和时间戳拼接后传入 E() 函数
输出的 6 位数字就是界面上显示的"请求码/挑战码

##响应码生成
response = E(challenge + salt)
用户输入的"请求码"就是上面的 challenge
将 challenge 和 salt 拼接后传入 E() 函数
输出的 6 位数字就是需要输入的"响应码"



salt 是从 native 库 libp4bu.so 的只读数据段（.rodata@0x472f）中读取的字符串常量
逆向分析得出其值为 "fundot.ii."
