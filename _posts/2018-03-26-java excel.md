---
layout: post
title: java读取修改excel
keywords:
category: java
author: 晴天
description:
---

<p>分析脚本执行结果，写入excel文件中。</p>

<p>引入jar包</p>

```
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi</artifactId>
            <version>3.17</version>
        </dependency>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>3.17</version>
        </dependency>
```

<p>工具类以后遇到需求再补。</p>

```
public class ExcelUtil {
    //读取excel文件 以.xlsx结尾
    static List<List<String>> readExcel(String filePath) throws IOException {
        List<List<String>>result = new ArrayList<>();
        InputStream is = new FileInputStream(new File(filePath));
//        book表示一个excel文件
        XSSFWorkbook book = new XSSFWorkbook(is);
//        sheet表示一页
        for (Sheet sheet : book){
            if (null == sheet){
                continue;
            }
            //每一行
            for(int rowNum = 1;rowNum <= sheet.getLastRowNum();rowNum++){
                XSSFRow xssfRow = (XSSFRow) sheet.getRow(rowNum);
                int minColIx = xssfRow.getFirstCellNum();
                int maxColIx = xssfRow.getLastCellNum();
                List<String>rowList = new ArrayList<>();
//                每一列
                for (int colIx = minColIx;colIx < maxColIx;colIx++){
                    XSSFCell cell = xssfRow.getCell(colIx);
                    if(cell == null){
                        continue;
                    }
                    rowList.add(cell.toString());
                }
                result.add(rowList);
            }
        }
        return result;
    }
//    修改excel 第几页 第几列 以及修改后的内容
    static void modifyExcel(String filePath,int sheetNum,int colNum,String content) throws Exception {
        InputStream is = new FileInputStream(new File(filePath));
        XSSFWorkbook work = new XSSFWorkbook(is);
        FileOutputStream fos = new FileOutputStream(new File(filePath));
        XSSFSheet sheet = work.getSheetAt(sheetNum);
        for(Row row : sheet){
            for (Cell cell : row){
                if (cell.getColumnIndex() == colNum){
                    cell.setCellValue(content);
                }
            }
        }
        work.write(fos);
        work.close();
        fos.close();
        is.close();
    }
}

```

<p>在读取并修改后，报错。</p>

![EmptyFileException](/images/EmptyFileException.png)