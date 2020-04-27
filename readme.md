### SSM框架基于xml构建商品检索和积分排名系统（Spring+SpringMVC+Mybatis+Lucene+Redis+Maven）

- AjaxController

  - ```
    getExcel:查询商品列表前台页面生成excel报表
    ```

  - ```
    uploadExcel：前台上传文件，后台解析报表，将数据存储到List集合中,插入数据库
    ```

- BackstageController

  - ```
    login：前台登录，登录成功，将登录标识和时间戳放入Map中，通过jsonwebToken生成sessionID,将sessionId添加到cookie中。之后访问只需通过拦截器拦截获取cookie中的信息，判断是否有访问权限。
    	cookie:客户端保存用户信息的一种机制
    		问题：HTTP客户端软件（包括curl、Node.js）都可以发送任意的HTTP请求，			Cookie是可以被篡改的！
    	session:服务端保存的一个数据结构
    		Cookie为sessionId=xxxxxx|checksum并发送HTTP响应，sessionID 的生成		方式Hash(‘x$sfz32’+‘true’)
    	jwt:服务端根据一个密码和算法生成Token,然后发给客户端，客户端后面每次请求都在Http header里面带上token（跨服务器验证，只需密钥和算法相同）
    	jwt比session好处：CSRF(跨域访问)
    ```

- GagController

  - ```
    testRedis：实现redis缓存功能
    ```

- GoodController

  - ```
    findGoodByName：根据商品名称，使用lucene搜索
    ```

- ScoreController

  - ```
    updateScore：更新用户积分，此处涉及积分排行榜
    	通过redis缓存top 20的积分用户，更新用户积分时，需要判断缓存中是否有该用户，如果原本有则修改原有的；如果没有，则取出缓存中最后一名，判断积分是否大于，若大于则直接丢尽缓存，否则不用管。
    ```