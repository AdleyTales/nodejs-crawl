# nodejs爬虫

> 需要的包： request cheerio

- request: 发送http请求，获取页面数据。
- cheerio: 类似jquery的语法，操作dom，选取元素。

## 安装：

    cnpm install request cheerio --save-dev

## 下面是爬取豆瓣的数据实例：

  ```js
      const cheerio = require('cheerio');
      const request = require('request');

      //获取内容
      request.get('https://book.douban.com/tag/%E5%BB%BA%E7%AD%91',(err,res,body) => {
        console.log('error:',err);
        console.log('statusCode:',res.statusCode); //200
        // console.log('body:',body); //html页面的字符串

        let $ = cheerio.load(body);

        let imgArr = [],
            titleArr = [],
            rateArr = [],
            len = $('.subject-item').length;
        for (var i = 0; i < len; i++) {
          imgArr.push($('.subject-item').eq(i).find('img').attr('src'));
          titleArr.push($('.subject-item').eq(i).find('h2 a').attr('title'));
          rateArr.push($('.subject-item').eq(i).find('.rating_nums').text());
        }

        console.log(imgArr); //打印所有的图片地址
        console.log(titleArr); //打印所有书籍的名称
        console.log(rateArr); //打印所有书籍的评分

        //打印所有大于9评分的书籍名称
        rateArr.map((i,j)=>{
          if(i>9){
            // console.log(i,j);
            console.log(titleArr[j]);
          }
        });

      });

  ```  
