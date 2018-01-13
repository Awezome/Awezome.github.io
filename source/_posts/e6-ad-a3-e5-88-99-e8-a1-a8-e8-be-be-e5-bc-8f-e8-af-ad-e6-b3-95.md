---
title: 正则表达式语法
tags:
  - 正则表达式
id: 1779
categories:
  - Other
date: 2014-04-30 15:51:37
---

<table width="100%">
<tbody>
<tr>
<th style="color: #636363;">表达式</th>
<th style="color: #636363;">匹配</th>
</tr>
<tr>
<td style="color: #2a2a2a;">/^\s*$/</td>
<td style="color: #2a2a2a;">匹配空行。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">/\d{2}-\d{5}/</td>
<td style="color: #2a2a2a;">验证由两位数字、一个连字符再加 5 位数字组成的 ID 号。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">/&lt;\s*(\S+)(\s[^&gt;]*)?&gt;[\s\S]*&lt;\s*\/\1\s*&gt;/</td>
<td style="color: #2a2a2a;">匹配 HTML 标记。</td>
</tr>
</tbody>
</table>

<!--more-->

<table width="100%">
<tbody>
<tr>
<th style="color: #636363;">字符</th>
<th style="color: #636363;">说明</th>
</tr>
<tr>
<td style="color: #2a2a2a;">\</td>
<td style="color: #2a2a2a;">将下一字符标记为特殊字符、文本、反向引用或八进制转义符。例如，“n”匹配字符“n”。“\n”匹配换行符。序列“\\”匹配“\”，“\(”匹配“(”。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">^</td>
<td style="color: #2a2a2a;">匹配输入字符串开始的位置。如果设置了 **RegExp** 对象的 **Multiline** 属性，^ 还会与“\n”或“\r”之后的位置匹配。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">$</td>
<td style="color: #2a2a2a;">匹配输入字符串结尾的位置。如果设置了 **RegExp** 对象的 **Multiline** 属性，$ 还会与“\n”或“\r”之前的位置匹配。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">*</td>
<td style="color: #2a2a2a;">零次或多次匹配前面的字符或子表达式。例如，zo* 匹配“z”和“zoo”。* 等效于 {0,}。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">+</td>
<td style="color: #2a2a2a;">一次或多次匹配前面的字符或子表达式。例如，“zo+”与“zo”和“zoo”匹配，但与“z”不匹配。+ 等效于 {1,}。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">?</td>
<td style="color: #2a2a2a;">零次或一次匹配前面的字符或子表达式。例如，“do(es)?”匹配“do”或“does”中的“do”。? 等效于 {0,1}。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">{_n_}</td>
<td style="color: #2a2a2a;">_n _是非负整数。正好匹配 _n_ 次。例如，“o{2}”与“Bob”中的“o”不匹配，但与“food”中的两个“o”匹配。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">{_n_,}</td>
<td style="color: #2a2a2a;">_n _是非负整数。至少匹配 _n _次。例如，“o{2,}”不匹配“Bob”中的“o”，而匹配“foooood”中的所有 o。“o{1,}”等效于“o+”。“o{0,}”等效于“o*”。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">{_n_,_m_}</td>
<td style="color: #2a2a2a;">_M_ 和 _n_ 是非负整数，其中 _n_ &lt;= _m_。匹配至少 _n_ 次，至多 _m_ 次。例如，“o{1,3}”匹配“fooooood”中的头三个 o。'o{0,1}' 等效于 'o?'。注意：您不能将空格插入逗号和数字之间。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">?</td>
<td style="color: #2a2a2a;">当此字符紧随任何其他限定符（*、+、?、{_n_}、{_n_,}、{_n_,_m_}）之后时，匹配模式是“非贪心的”。“非贪心的”模式匹配搜索到的、尽可能短的字符串，而默认的“贪心的”模式匹配搜索到的、尽可能长的字符串。例如，在字符串“oooo”中，“o+?”只匹配单个“o”，而“o+”匹配所有“o”。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">.</td>
<td style="color: #2a2a2a;">匹配除“\n”之外的任何单个字符。若要匹配包括“\n”在内的任意字符，请使用诸如“[\s\S]”之类的模式。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">(_pattern_)</td>
<td style="color: #2a2a2a;">匹配 _pattern_ 并捕获该匹配的子表达式。可以使用 **$0…$9** 属性从结果“匹配”集合中检索捕获的匹配。若要匹配括号字符 ( )，请使用“\(”或者“\)”。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">(?:_pattern_)</td>
<td style="color: #2a2a2a;">匹配 _pattern_ 但不捕获该匹配的子表达式，即它是一个非捕获匹配，不存储供以后使用的匹配。这对于用“or”字符 (|) 组合模式部件的情况很有用。例如，'industr(?:y|ies) 是比 'industry|industries' 更经济的表达式。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">(?=_pattern_)</td>
<td style="color: #2a2a2a;">执行正向预测先行搜索的子表达式，该表达式匹配处于匹配 _pattern_ 的字符串的起始点的字符串。它是一个非捕获匹配，即不能捕获供以后使用的匹配。例如，'Windows (?=95|98|NT|2000)' 匹配“Windows 2000”中的“Windows”，但不匹配“Windows 3.1”中的“Windows”。预测先行不占用字符，即发生匹配后，下一匹配的搜索紧随上一匹配之后，而不是在组成预测先行的字符后。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">(?!_pattern_)</td>
<td style="color: #2a2a2a;">执行反向预测先行搜索的子表达式，该表达式匹配不处于匹配 _pattern_ 的字符串的起始点的搜索字符串。它是一个非捕获匹配，即不能捕获供以后使用的匹配。例如，'Windows (?!95|98|NT|2000)' 匹配“Windows 3.1”中的 “Windows”，但不匹配“Windows 2000”中的“Windows”。预测先行不占用字符，即发生匹配后，下一匹配的搜索紧随上一匹配之后，而不是在组成预测先行的字符后。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">_x_|_y_</td>
<td style="color: #2a2a2a;">匹配 _x_ 或 _y_。例如，'z|food' 匹配“z”或“food”。'(z|f)ood' 匹配“zood”或“food”。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">[_xyz_]</td>
<td style="color: #2a2a2a;">字符集。匹配包含的任一字符。例如，“[abc]”匹配“plain”中的“a”。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">[^_xyz_]</td>
<td style="color: #2a2a2a;">反向字符集。匹配未包含的任何字符。例如，“[^abc]”匹配“plain”中的“p”。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">[_a-z_]</td>
<td style="color: #2a2a2a;">字符范围。匹配指定范围内的任何字符。例如，“[a-z]”匹配“a”到“z”范围内的任何小写字母。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">[^_a-z_]</td>
<td style="color: #2a2a2a;">反向范围字符。匹配不在指定的范围内的任何字符。例如，“[^a-z]”匹配任何不在“a”到“z”范围内的任何字符。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\b</td>
<td style="color: #2a2a2a;">匹配一个字边界，即字与空格间的位置。例如，“er\b”匹配“never”中的“er”，但不匹配“verb”中的“er”。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\B</td>
<td style="color: #2a2a2a;">非字边界匹配。“er\B”匹配“verb”中的“er”，但不匹配“never”中的“er”。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\c_x_</td>
<td style="color: #2a2a2a;">匹配 _x_ 指示的控制字符。例如，\cM 匹配 Control-M 或回车符。_x_ 的值必须在 A-Z 或 a-z 之间。如果不是这样，则假定 c 就是“c”字符本身。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\d</td>
<td style="color: #2a2a2a;">数字字符匹配。等效于 [0-9]。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\D</td>
<td style="color: #2a2a2a;">非数字字符匹配。等效于 [^0-9]。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\f</td>
<td style="color: #2a2a2a;">换页符匹配。等效于 \x0c 和 \cL。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\n</td>
<td style="color: #2a2a2a;">换行符匹配。等效于 \x0a 和 \cJ。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\r</td>
<td style="color: #2a2a2a;">匹配一个回车符。等效于 \x0d 和 \cM。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\s</td>
<td style="color: #2a2a2a;">匹配任何空白字符，包括空格、制表符、换页符等。与 [ \f\n\r\t\v] 等效。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\S</td>
<td style="color: #2a2a2a;">匹配任何非空白字符。与 [^ \f\n\r\t\v] 等效。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\t</td>
<td style="color: #2a2a2a;">制表符匹配。与 \x09 和 \cI 等效。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\v</td>
<td style="color: #2a2a2a;">垂直制表符匹配。与 \x0b 和 \cK 等效。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\w</td>
<td style="color: #2a2a2a;">匹配任何字类字符，包括下划线。与“[A-Za-z0-9_]”等效。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\W</td>
<td style="color: #2a2a2a;">与任何非单词字符匹配。与“[^A-Za-z0-9_]”等效。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\x_n_</td>
<td style="color: #2a2a2a;">匹配 _n_，此处的 _n_ 是一个十六进制转义码。十六进制转义码必须正好是两位数长。例如，“\x41”匹配“A”。“\x041”与“\x04”&amp;“1”等效。允许在正则表达式中使用 ASCII 代码。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\_num_</td>
<td style="color: #2a2a2a;">匹配 _num_，此处的 _num_ 是一个正整数。到捕获匹配的反向引用。例如，“(.)\1”匹配两个连续的相同字符。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\_n_</td>
<td style="color: #2a2a2a;">标识一个八进制转义码或反向引用。如果 \_n_ 前面至少有 _n_ 个捕获子表达式，那么 _n_ 是反向引用。否则，如果 _n_ 是八进制数 (0-7)，那么 _n_ 是八进制转义码。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\_nm_</td>
<td style="color: #2a2a2a;">标识一个八进制转义码或反向引用。如果 \_nm_ 前面至少有 _nm_ 个捕获子表达式，那么 _nm_ 是反向引用。如果 \_nm_ 前面至少有 _n_ 个捕获，则 _n_ 是反向引用，后面跟有字符 _m_。如果两种前面的情况都不存在，则 \_nm_ 匹配八进制值 _nm_，其中 _n _和 _m_ 是八进制数字 (0-7)。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\<span class="parameter" style="font-style: italic;">nml</span></td>
<td style="color: #2a2a2a;">当 _n_ 是八进制数 (0-3)，_m_ 和 _l_ 是八进制数 (0-7) 时，匹配八进制转义码 _nml_。</td>
</tr>
<tr>
<td style="color: #2a2a2a;">\u_n_</td>
<td style="color: #2a2a2a;">匹配 _n_，其中 _n_ 是以四位十六进制数表示的 Unicode 字符。例如，\u00A9 匹配版权符号 (©)。</td>
</tr>
</tbody>
</table>

<!--more-->

http://msdn.microsoft.com/zh-cn/library/ae5bf541(VS.80).aspx