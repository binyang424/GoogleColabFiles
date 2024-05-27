由于之前的博客首页和文章封面图的api失效，不得不更换新的图像随机api。上次设置过了太长时间，导致对图片api的设置方法已经完全忘记了，最终又一个一个的设置界面尝试，因此再本博客中记下，以防止未来api再次失效。

# 1. 当前图像api

Bing搜索的壁纸一直在线，且避免了个人搭建的图像api存在不可名状的图片的风险。因此选用bing的图片库是最简单省心的选择。这里十分感谢[Mikelin](https://bing.img.run/)搭建的bing图库api。所有接口都是支持直接使用的链接，可以直接把它当做一个图片url链接来用，插入如下代码：

**Bing当日壁纸**

    <img src="https://bing.img.run/uhd.php" alt="Bing每日壁纸UHD超高清原图" />
    <img src="https://bing.img.run/1920x1080.php" alt="Bing每日壁纸1080P高清" />
    <img src="https://bing.img.run/1366x768.php" alt="Bing每日壁纸普清" />
    <img src="https://bing.img.run/m.php" alt="Bing每日壁纸手机版1080P高清" />

**随机获取Bing历史壁纸**

    <img src="https://bing.img.run/rand_uhd.php" alt="随机获取Bing历史壁纸UHD超高清原图" />
    <img src="https://bing.img.run/rand.php" alt="随机获取Bing历史壁纸1080P高清" />
    <img src="https://bing.img.run/rand_1366x768.php" alt="随机获取Bing历史壁纸普清" />
    <img src="https://bing.img.run/rand_m.php" alt="随机获取Bing历史壁纸手机版1080P高清" />

# 2. 设置主页背景图

在如下后台界面设置外部API随机图片地址即可。

![](https://raw.githubusercontent.com/binyang424/GoogleColabFiles/main/mdimage/%E4%B8%BB%E9%A2%98%E9%9A%8F%E6%9C%BA%E5%9B%BE%E7%89%87api%E8%AE%BE%E7%BD%AE/image-20240527172211756.png)

# 3. 文章封面图片

在以下界面进行图片API设置：

![](https://raw.githubusercontent.com/binyang424/GoogleColabFiles/main/mdimage/%E4%B8%BB%E9%A2%98%E9%9A%8F%E6%9C%BA%E5%9B%BE%E7%89%87api%E8%AE%BE%E7%BD%AE/image-20240527173700100.png)

# 4. 备份设置

所有设置可以进行备份，以便在有需要时导入恢复当前设置：

![](https://raw.githubusercontent.com/binyang424/GoogleColabFiles/main/mdimage/%E4%B8%BB%E9%A2%98%E9%9A%8F%E6%9C%BA%E5%9B%BE%E7%89%87api%E8%AE%BE%E7%BD%AE/image-20240527173813974.png)

# 参考/引用链接

1. [开放API接口 - Bing每日壁纸档案库](https://bing.img.run/api.html)
