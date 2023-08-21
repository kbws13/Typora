# Java拼接字符串并以逗号分隔

问题描述

生成20个随机数将其按照逗号分隔拼接成字符串，且数字不能重复，再以逗号分隔成整数存入数组

解决方法

使用StringBuilder的append方法以及字符串的indexOf方法

最终代码

```java
Random r = new Random();
StringBuilder text = new StringBuilder();
while (i<20){
    int num = r.nextInt(80)+10;
    if(text.indexOf(String.valueOf(num))!=-1){
        continue;
    }
    text.append(num).append(",");
    i++;
}

String[] result = text.toString().split(",");
int[] num = new int[20];
for(String s : result){
    num[j] = Integer.parseInt(s);
    System.out.print(num[j]+" ");
    j++;
}
System.out.println();
Arrays.sort(num);//数组有序化
```

# 分别获取年月日

问题描述

需要分别获取当前日期的年月日

解决方法

获取当前的日期后，使用Calendar获取相应的参数

最终代码

```java
Date date = new Date();
Calendar cal = Calendar.getInstance();  
cal.setTime(date);
cal.get(Calendar.YEAR);
cal.get(Calendar.MONTH)+1;//月份需要+1
cal.get(Calendar.DAY_OF_MONTH);
```

# 以HTML为模板生成PDF

问题描述

需要将一个Word表格生成PDF，同时根据不同的用户，往里面填入不同的数据

解决方法

使用iText工具生成PDF，Freemarker引擎渲染数据，xmlworker解析HTML。使用原生的方法构建PDF太过繁琐，所以先用HTML和CSS构建一个相似的表格，以HTML文件为模板，并往里面填充数据，以此来生成正确的PDF

最终代码

首先引入需要的包

```xml
itext工具包
<dependency>
    <groupId>com.itextpdf</groupId>
    <artifactId>itextpdf</artifactId>
    <version>5.5.9</version>
</dependency>
模板引擎
<dependency>
    <groupId>org.freemarker</groupId>
    <artifactId>freemarker</artifactId>
    <version>2.3.23</version>
</dependency>
XML转PDF
<dependency>
    <groupId>com.itextpdf.tool</groupId>
    <artifactId>xmlworker</artifactId>
    <version>5.5.9</version>
</dependency>
引入中文字体包
<dependency>
    <groupId>com.itextpdf</groupId>
    <artifactId>itext-asian</artifactId>
    <version>5.2.0</version>
</dependency>
HTML转PDF
<dependency>
    <groupId>org.xhtmlrenderer</groupId>
    <artifactId>flying-saucer-pdf-itext5</artifactId>
    <version>9.0.3</version>
</dependency>
```

创建工具类

JavaToPDF.java

```java
package xyz.kbws.util;

import com.itextpdf.text.BaseColor;
import com.itextpdf.text.Document;
import com.itextpdf.text.Font;
import com.itextpdf.text.pdf.BaseFont;
import com.itextpdf.text.pdf.PdfWriter;
import com.itextpdf.tool.xml.XMLWorkerFontProvider;
import com.itextpdf.tool.xml.XMLWorkerHelper;
import freemarker.template.Configuration;
import freemarker.template.DefaultObjectWrapper;
import freemarker.template.Template;
import org.springframework.stereotype.Component;

import java.io.*;
import java.nio.charset.Charset;
import java.util.Map;

@Component
public class JavaToPDF {
    private String tempFilePath;
    private String tempFileName;

    public JavaToPDF() {}

    public JavaToPDF(String tempFilePath, String tempFileName) {
        this.tempFilePath=tempFilePath;
        this.tempFileName=tempFileName;
    }

    /**
     * 填充模板
     * @param paramMap
     * @throws Exception
     */
    public  String  fillTemplate(Map<String, Object> paramMap,String userId) throws Exception {
        File modelFile = new File(tempFilePath);
        if(!modelFile.exists()) {
            modelFile.mkdirs();
        }
        Configuration configuration = new Configuration(Configuration.VERSION_2_3_23);
        configuration.setDirectoryForTemplateLoading(modelFile);
        configuration.setObjectWrapper(new DefaultObjectWrapper(Configuration.VERSION_2_3_23));
        configuration.setDefaultEncoding("UTF-8");   //这个一定要设置，不然在生成的页面中 会乱码
        //获取或创建一个模版。
        Template template = configuration.getTemplate(tempFileName);

        StringWriter stringWriter = new StringWriter();
        BufferedWriter writer = new BufferedWriter(stringWriter);
        template.process(paramMap, writer);

        String htmlStr = stringWriter.toString();
        writer.flush();
        writer.close();

        String tmpPath = tempFilePath;
        File tmepFilePath = new File(tmpPath);
        if (!tmepFilePath.exists()) {
            tmepFilePath.mkdirs();
        }
        String  tmpFileName =userId+".pdf";
        String outputFile = tmpPath + File.separatorChar + tmpFileName;
        FileOutputStream outFile = new FileOutputStream(outputFile);
        createPDFFile(htmlStr, outFile);

        return outputFile;
    }


    /**
     * 根据HTML字符串创建PDF文件
     * @param htmlStr
     * @param
     * @throws Exception
     */
    private  void createPDFFile(String htmlStr,OutputStream os) throws Exception{
        ByteArrayInputStream bais = new ByteArrayInputStream(htmlStr.getBytes("UTF-8"));
        // step 1
        Document document = new Document();
        try {
            // step 2
            PdfWriter writer = PdfWriter.getInstance(document, os);
            // step 3
            document.open();
            FontProvider  provider = new FontProvider();
            XMLWorkerHelper.getInstance().parseXHtml(writer, document, bais, Charset.forName("UTF-8"),provider);
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }finally {
            try {
                document.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
            try {
                bais.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 字体
     */
    private  class FontProvider extends XMLWorkerFontProvider {

        public Font getFont(final String fontname, final String encoding,
                            final boolean embedded, final float size, final int style,
                            final BaseColor color) {
            BaseFont bf = null;
            try {
                bf = BaseFont.createFont("STSong-Light", "UniGB-UCS2-H", BaseFont.NOT_EMBEDDED);
            } catch (Exception e) {
                e.printStackTrace();
            }
            Font font = new Font(bf, 12, Font.NORMAL);
            font.setColor(color);
            return font;
        }
    }
    /**
     * 生成html模板
     * @param content
     * @return
     */
    public String createdHtmlTemplate(String content){
        String fileName = tempFilePath + "/" + tempFileName;
        try{
            File file = new File(tempFilePath);
            if(!file.isDirectory()) {
                file.mkdir();
            }
            file = new File(fileName);
            if(!file.isFile()) {
                file.createNewFile();
            }
            //打开文件
            PrintStream printStream = new PrintStream(new FileOutputStream(fileName));

            //将HTML文件内容写入文件中
            printStream.println(content);
            printStream.flush();
            printStream.close();
            System.out.println("生成html模板成功!");
        }catch(Exception e){
            e.printStackTrace();
        }
        return fileName;
    }

}
```

调用工具类方法

```java
public R exportPdf(String id, HttpServletRequest request, HttpServletResponse response) {
        //模板路径
        String path = "D:/test";
        //模板文件名
        String name = "index.html";

        Student student = studentMapper.queryById(id);
        User user = userMapper.queryUserById(id);
        Map<String,Object> paraMap = user.data(student,user);
        //创建PDF文件
        JavaToPDF pdfUtil = new JavaToPDF(path,name);
        try {
            //模板渲染数据
            String uploadFile = pdfUtil.fillTemplate(paraMap,user.getName());
            System.out.println(uploadFile);
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        fileUtil.fileDownload(user.getName()+".pdf",response);
        fileUtil.fileDelete(user.getName()+".pdf");
        return R.success("导出PDF成功");
    }
```

模板文件

index.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>专业分流表</title>
    <style>
        h1 {
          font-size: 24px;
          font-weight: bold;
        }
    </style>
</head>

<body>
    <h1 style="text-align: center;">河北地质大学专业分流审批表</h1>
    <h4 style="text-align: justify;">
        <div style="float: left;">2022 — 2023&nbsp;&nbsp;&nbsp;学年&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;第 一 学期</div>
        <div style="float: right;">
            填表日期：${year?c}年${month}月${day}日
        </div>
    </h4>
    <table align="center" border="1" cellspacing="0" width="100%" height="100%" style="text-align: center;">
        <tr style="height: 40px;">
            <th style="width: 50px;">
                学号
            </th>
            <th style="width: 120px;">
                ${id}
            </th>
            <th style="width: 50px;">
                姓名
            </th>
            <th style="width: 100px;">
                ${name}
            </th>
            <th style="width: 50px;">
                性别
            </th>
            <th style="width: 100px;">
                ${sex}
            </th>
            <th style="width: 50px;">
                10位班号
            </th>
            <th style="width: 120px;">
                ${sClass}
            </th>
            <th rowspan="4" style="width: 100px; Writing-mode:tb-rl;">
                小二寸电子彩照
            </th>
        </tr>
    </table>
    <div style="padding-top: 15px;padding-left: 0px;">
        说明：1.此表经学院审批后以学院为单位送交学籍科，教务处审批后返还所在学院存留；
    </div>
    <div style="padding-top:5px;padding-left: 48px;">
        2.奖惩及能力情况，包括校外各种比赛、竞赛和奖学金情况，证书取得情况。
    </div>
</body>

</html>
```

> 注意：itext对HTML的检查非常严格，每一个标签必须闭合，就算是单个标签例如< br >也必须有结束标签，像< meta >标签可以在后面加 / 表示结束。表格的大小最好使用百分比来设定。

# SpringBoot实现文件下载删除

问题描述

服务器生成文件后传输给客户端，然后将本地生成的文件删除

解决方法

使用IO流和HttpServletResponse进行文件操控

最终代码

配置文件

application.yaml中配置的文件存在的位置

```yaml
file:
  path: D:/test/
```

FileUtil工具类

```java
package xyz.kbws.util;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletResponse;
import java.io.*;

/**
 * @author hsy
 * @date 2023/3/4
 * 文件工具类，实现文件的下载和删除
 */
@Slf4j
@Component
public class FileUtil {

    @Value("${file.path}")
    private String filePath;//文件存放路径
    /**
     * 下载文件
     * @param fileName
     * @param response
     */
    public void fileDownload(String fileName, HttpServletResponse response){
        //1.检查是否存在文件
        File file = new File(filePath+fileName);
        if (!file.exists()){
            log.error("文件不存在");
        }
        //2.下载文件
        try {
            down(response,file);
        }catch (Exception e){
            log.error("文件下载异常:{}",e.getMessage());
        }
    }

    /**
     * 删除文件
     * @param fileName
     * @return
     */
    public String fileDelete(String fileName){
        File file = new File(filePath+fileName);
        if (!file.exists()){
            log.error("文件不存在");
        }
        try {
            if (file.delete()){
                //log.info("{}删除成功",fileName);
                return fileName;
            }
        }catch (Exception e){
            log.error("文件删除异常:{}",e.getMessage());
        }
        return "文件删除失败";
    }
    private String down(HttpServletResponse response,File file){
        if(!file.exists()){
            return "下载文件不存在";
        }
        response.reset();
        response.setContentType("application/octet-stream");
        response.setCharacterEncoding("utf-8");
        response.setContentLength((int) file.length());
        response.setHeader("Content-Disposition", "attachment;filename=" + file.getName() );

        try(BufferedInputStream bis = new BufferedInputStream(new FileInputStream(file));) {
            byte[] buff = new byte[1024];
            OutputStream os  = response.getOutputStream();
            int i = 0;
            while ((i = bis.read(buff)) != -1) {
                os.write(buff, 0, i);
                os.flush();
            }
        } catch (IOException e) {
            log.error("{}",e.getMessage());
            return "下载失败";
        }
        return "下载成功";
    }
}
```

调用方法

```java
fileUtil.fileDownload(user.getName()+".pdf",response);
fileUtil.fileDelete(user.getName()+".pdf");
```

