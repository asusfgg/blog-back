<!--
 * @Author: 千仞无锋
 * @Date: 2022-07-16 09:06:40
 * @LastEditors: 千仞无锋
 * @LastEditTime: 2022-07-16 11:22:51
 * @FilePath: \fgBlog\source\artitalk\index.md
-->
---
title: artitalk
type: artitalk
noDate: 'true'
comments: 'false'
---

<head>
  <script src="https://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
</head>
  <body>
      <!-- 引用 artitalk -->
      <script type="text/javascript" src="https://unpkg.com/artitalk"></script>
      <!-- 存放说说的容器 -->
      <div id="artitalk_main"></div>
      <script>
      new Artitalk({
          appId: 'DFMiaKtOxxdks4VaqW4tWvDz-gzGzoHsz', // Your LeanCloud appId
          appKey: 'OQgbQXUUiKMklT2L43gJM4yk',// Your LeanCloud appKey
          serverURL: 'https://fengge.site/',
          pageSize: 10,
          motion: 1,
          atEmoji: {
                  baiyan: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/baiyan.png",
                  bishi: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/bishi.png",
                  bizui: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/bizui.png",
                  chan: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/chan.png",
                  daku: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/daku.png",
                  dalao: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/dalao.png",
                  dalian: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/dalian.png",
                  dianzan: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/dianzan.png",
                  doge: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/doge.png",
                  facai: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/facai.png",
                  fadai: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/fadai.png",
                  fanu: "https://cdn.jsdelivr.net/gh/Artitalk/Artitalk-emoji/fanu.png",
            },
      })
      </script>
      <div id="lazy"></div>
      <div id="artitalk"></div>
      <script type="text/javascript" src="https://unpkg.com/artitalk"></script>
  </body>