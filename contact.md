---
layout: page
title: Contact
tagline: 
---


* [点击安装](itms-services://?action=download-manifest&url=https://raw.githubusercontent.com/1ilI/TestMyipa_Resource/master/resource/MyTest/manifest.plist)

<div id="gitalk-container"></div>
<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
<script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
<script>
var gitalk = new Gitalk({
    id: '{{ page.url }}',
    clientID: '{{ site.gitalk.clientID }}',
    clientSecret: '{{ site.gitalk.clientSecret }}',
    repo: '{{ site.gitalk.repo }}',
    owner: '{{ site.gitalk.owner }}',
    admin: ['{{ site.gitalk.owner }}'],
    labels: ['TestMyipa'],
    perPage: 50,
})
gitalk.render('gitalk-container')
</script>