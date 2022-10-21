#Slugs(短标签)
---
Slugs 是可以在 URL 中使用的字符串。  Slugs必须满足以下所有规则： 
- Slugs 的长度必须至少为 1 个字符且最多为 64 个字符。 
- Slugs 只能包含字母、数字、点（"."）、破折号（"-"）和下划线（"_"）。 
- 所有字母必须小写并且来自基本的拉丁字母。 

###示范:
>[!TIP]
>正确的示范
>- my-mod
>- version-1.3.4
>- test-6.4.1_RC-3

>[!attention]
>错误的示范:
>- be$t-mod(不允许使用美元符号)
>- café-mod("é"  不是拉丁字母)
>- \<script\>alert('xss')\</script\> ("<>()'/" 不允许使用)
>- my mod 2 (不允许使用空格)
>- a-very-very-very-very-very-very-very-very-very-very-very-very-very-long-name (超过 64 个字符)
