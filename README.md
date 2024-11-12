配合cloud flare workers实现GitHub图库的随机背景

```workers.js
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {

var background_urls = [
'https://cdn.jsdelivr.net/gh/tanmoumou252/bingwallpaper@main/pic/light/bingwallpaper_01.jpg',
'https://cdn.jsdelivr.net/gh/tanmoumou252/bingwallpaper@main/pic/light/bingwallpaper_02.jpg',
'https://cdn.jsdelivr.net/gh/tanmoumou252/bingwallpaper@main/pic/light/bingwallpaper_03.jpg'
]
var index = Math.floor((Math.random()*background_urls.length));
res = await fetch(background_urls[index])
  return new Response(res.body, {
      headers: { 'content-type': 'image/jpeg' },
  })
}
```
如此,在workers的自定义域 路由中配置一下,就可以打开同一个地址每次出现不同的图了

比如下面的 Alist 自定义头部
```html
<style>
/* 去除通知栏 右上角 X */
.notify-render .hope-close-button {
    display: none;
}

/*白天背景图*/
.hope-ui-light {
    background-image: url("https://***/bing-light/") !important;
    background-repeat:no-repeat;
    background-size:cover;
    background-attachment:fixed;
    background-position-x:center;
    --hope-colors-background: #f7f8fa00 !important;    
}

/*夜间背景图*/
.hope-ui-dark {
    background-image: url("https://***/bing-dark/") !important;
    background-repeat:no-repeat;
    background-size:cover;
    background-attachment:fixed;
    background-position-x:center;
    --hope-colors-background: #f7f8fa00 !important;
}
</style>
```
