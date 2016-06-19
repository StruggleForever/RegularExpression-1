##简单了解:
>    正则表达式(regular expression)描述了一种字符串匹配的模式，可以用来检查一个串是否含有某种子串、将匹配的子串做替换或者从某个串中取出符合某个条件的子串等。
      正则表达式是由普通字符（例如:字符 a 到 z）以及特殊字符（称为"元字符"）组成的文字模式。模式描述在搜索文本时要匹配的一个或多个字符串。正则表达式作为一个模板，将某个字符模式与所搜索的字符串进行匹配。

---
##简单示例(说明基础语法)
    - (BOOL)checkTheTestString:(NSString *)testString {
        NSString *number=@"^[0-9]+$";
        NSPredicate *numberPre = [NSPredicate predicateWithFormat:@"SELF MATCHES %@",number];
        return [numberPre evaluateWithObject: testString];
    }
1. 首先我们撇开语法，看看一个正则表达式里包含了一些什么

    * "^"和"$"分别指出了一个字符串的开始和结束，如：

          "^YJZ.*"：表示所有以"YJZ"开头的字符串("YJZ001", "YJZ is a coder",  "YJZ's Blog"等等, 其中"."表示任意字符,"*"在后面会提到)
          ".*a single dog$"：表示所有以"a single dog"结尾的字符字符串("I'm a single dog"等等)
          "^iPhone$"：表示开始和结束都是"iPhone"的字符串，这是唯一的，没有别的结果，去掉两旁的符号得到的结果一样
    * "+"、"*"和"?"这三个符号属于一类，它们表示一个或N个字符重复出现的次数，下面我用数学上的区间说明
          "WoW+"：表示一个字符串"Wo"后面跟着至少一个"W"([1, +∞]),
                  ("WoW", "WoWWWWW")
          "WoW*"：表示一个字符串"Wo"后面跟着零个或若干个"W"([0, +∞]),
                  ("Wo", "WoW", "WoWWWW")
          "WoW?"：表示一个字符串"Wo"后面跟着零个或者一个b[0, 1],
                  ("Wo", "WoW",只有这两种结果)

    * "+", "*", "?"可以用"{}"替代, "{}"表示一个字符串重复的具体范围
          "+"：可以用"{1,}"表示; 
          "*"：可以用"{0,}"表示;
          "?"：可以用"{0,1}"表示;

          "WoW{3}"：表示一个字符串"Wo"后跟着4个"W"("WoWWW")
          "WoW{1,}"：表示一个字符串"Wo"后面跟着至少一个"W"([1, +∞]),
                      ("WoW", "WoWWWWW")
          "WoW{3,4}"：表示一个字符串@"Wo"跟着3到4个"W",
                      ("WoWWW", "WoWWWW")

          注意：一个"{}"内可以没有上限，但不能没有下限， 括号内不允许有空格，不然程序会崩溃
          崩溃原因如下:
          Terminating app due to uncaught exception 'NSInternalInconsistencyException', reason: 'Can't do regex matching, reason: Can't open pattern U_REGEX_BAD_INTERVAL (string WoW123, pattern WoW{3, 4}, case 0, canon 0)'
!["{}"中有空格时会使程序崩溃，报错如图.png](http://upload-images.jianshu.io/upload_images/2061725-575c37db0b644d9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    * [0-9]表示一个字符串包含0到9中的一个
          "[a-d]"：表示一个字符串包含小写的'a'到'd'中的一个(相当于"a|b|c|d"或者"[abcd]");
          "^[a-zA-Z].*"：表示一个以字母开头的字符串；
          "[0-9]a"：表示a前有一位的数字；　　   
          ".*[a-zA-Z0-9]$"：表示一个字符串以一个字母或数字结束。

今天就学到这儿了，扛不住了，睡觉去~
