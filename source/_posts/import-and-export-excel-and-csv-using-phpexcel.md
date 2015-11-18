title: 用 PHPExcel 导入导出 Excel 和 csv 文件
---
下载最新的 PHPExcel
在项目中引入 PHPExcel:
```php
require('PHPExcel/PHPExcel.php');
```
导出：
```php
        // Create new PHPExcel object
        $objPHPExcel = new PHPExcel();

        // Set document properties
        $objPHPExcel-&gt;getProperties()-&gt;setCreator("Yong Liu")
            -&gt;setLastModifiedBy("Yong Liu")
            -&gt;setTitle('export')
            -&gt;setSubject('export demo')
            -&gt;setDescription('export ')
            -&gt;setKeywords("excel csv")
            -&gt;setCategory('file');

        // Add data
        $objPHPExcel-&gt;setActiveSheetIndex(0)-&gt;setCellValue('A1', 'column1');
        $objPHPExcel-&gt;setActiveSheetIndex(0)-&gt;setCellValue('B1', 'column2');
        $objPHPExcel-&gt;setActiveSheetIndex(0)-&gt;setCellValue('C1', 'column3');
        $objPHPExcel-&gt;setActiveSheetIndex(0)-&gt;setCellValue('D1', 'column4');
        ...

        // Rename worksheet
        $objPHPExcel-&gt;getActiveSheet()-&gt;setTitle('export demo');

        // Set active sheet index to the first sheet, so Excel opens this as the first sheet
        $objPHPExcel-&gt;setActiveSheetIndex(0);

        // Redirect output to a client’s web browser (Excel2007)
        header('Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet');
        header('Content-Disposition: attachment;filename="export demo.xlsx"');
        header('Cache-Control: max-age=0');
        $objWriter = PHPExcel_IOFactory::createWriter($objPHPExcel, 'Excel2007');

        // Redirect output to a client’s web browser (Excel5)
        /*
        header('Content-Type: application/vnd.ms-excel');
        header('Content-Disposition: attachment;filename="' . $name . '.xls"');
        header('Cache-Control: max-age=0');
        $objWriter = PHPExcel_IOFactory::createWriter($objPHPExcel, 'Excel5');
        */

        // Redirect output to a client’s web browser (csv)
        /*
        header('Content-Type: text/csv');
        header('Content-Disposition: attachment;filename="' . $filename . '.csv"');
        header('Cache-Control: max-age=0');
        $objWriter = PHPExcel_IOFactory::createWriter($objPHPExcel, 'CSV')
        ->setDelimiter(',')
        ->setEnclosure('"')
        ->setLineEnding("\r\n")
        ->setSheetIndex(0);
        */

        $objWriter-&gt;save('php://output');
        exit;
```
导入：
```php

        // fox xlsx
        $inputFileType = 'Excel2007';
        // fox xls
        // $inputFileType = 'Excel5';
        // fox csv
        // $inputFileType = 'CSV';
        $path = $_FILES['upload_file']['tmp_name'];
        $reader = PHPExcel_IOFactory::createReader($inputFileType);
        $excelObj = $reader->load($path);
        // $result 为导入内容的数组
        $result = $excelObj->getActiveSheet()->toArray(null,true,true,true);
```
操作过程中可能会出现如下错误：
```
Unknown codepage: 10008
PHPExcel_Shared_CodePage::NumberToName(%d)
PHPExcel/Shared/CodePage.php        98        break()
```
原因是 NumberToName() 方法中没有 10008 对应的情况处理，所以会抛出“Unknown codepage: 10008”异常。
解决方法:
在 NumberToName() 方法中加入 10008 对应的处理：
```php
    case 10008: return 'MAC';   
```
