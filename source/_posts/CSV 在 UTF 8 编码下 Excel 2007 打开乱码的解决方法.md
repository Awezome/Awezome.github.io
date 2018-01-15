---
title: CSV 在 UTF 8 编码下 Excel 2007 打开乱码的解决方法
id: 2454
categories:
  - PHP
date: 2015-03-08 15:46:17
tags:
---

首先说这个问题太变态了，Office自己都和自己不兼容，正常的utf8编码下 07 局然打开中文乱乱码。当然首选想到是转成gbk这样中文系编码，这样中文没有了问题，但是对于其它小语种也是有同样乱码的问题。最后的最后还是Google到了解决方法。

先下这个网页，[CSV tests encoding and column separator](http://wiki.scn.sap.com/wiki/display/ABAP/CSV+tests+of+encoding+and+column+separator?original_fqdn=wiki.sdn.sap.com "CSV tests encoding and column separator")
上面对比测试了csv不同编码不同头不同分隔符在 excel 2003 和 excel 2007 乱码情况，也就是说在unicode系下，要保证没有乱码要做到3点：utf16le编码，tab分隔，加bom 。

下面是PHP版的最终代码，参考这里　[excel　mangles　diacritics　in　csv](http://stackoverflow.com/questions/155097/microsoft-excel-mangles-diacritics-in-csv-files/10969702 "excel　mangles　diacritics　in　csv")
```php
/**
   * Export an array as downladable Excel CSV
   * @param array   $header
   * @param array   $data
   * @param string  $filename
        */
          function toCSV($header, $data, $filename) {
    $sep  = "\t";
    $eol  = "\n";
    $csv  =  count($header) ? '"'. implode('"'.$sep.'"', $header).'"'.$eol : '';
    foreach($data as $line) {
      $csv .= '"'. implode('"'.$sep.'"', $line).'"'.$eol;
    }
    $encoded_csv = mb_convert_encoding($csv, 'UTF-16LE', 'UTF-8');
    header('Content-Description: File Transfer');
    header('Content-Type: application/vnd.ms-excel');
    header('Content-Disposition: attachment; filename="'.$filename.'.csv"');
    header('Content-Transfer-Encoding: binary');
    header('Expires: 0');
    header('Cache-Control: must-revalidate, post-check=0, pre-check=0');
    header('Pragma: public');
    header('Content-Length: '. strlen($encoded_csv));
    echo chr(255) . chr(254) . $encoded_csv;
    exit;
  }
```