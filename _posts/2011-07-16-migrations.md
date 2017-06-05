Title: Migrations
Date: 2011-07-16
Tags: Techie Rant

To the cloud!
I'm officially done hosting my own Wordpress blog. Not because it's particularly hard, but because it's quite boring. I would have done a straight export/import into a wordpress.com blog, but their options for hosting on a personal domain are pretty insane - if you want to host your blog on domain.com or www.domain.com you have to just point the entire domain at the wordpress.com DNS servers.
I'm not prepared to trust my domain to a bunch of PHP bloggers, so instead I've shoved the blog over to Blogger (by way of a very helpful [online conversion tool](http://wordpress2blogger.appspot.com/)), but this still presents a few niggles around URLs.
You can have Blogger send 404s to another vhost, so for now I just have a tiny little vhost somewhere else which uses mod\_rewrite to catch the old page names and attempt to catch the blog post names. Ideally I'd fetch all the old post URLs and make a proper map to the new ones, but I can't really be bothered to do that, so I just went for the approximate:

```
RewriteRule ^/archives/(\[0-9\]{4})/(\[0-9\]{2})/(\[0-9\]{2})/(\[a-zA-Z0-9\\-\]{1,39}).\*$ http://www.tenshu.net/$1/$2/$4.html \[R=301,L\]
```

Another obvious sticking point is that Wordpress categories become Blogger labels, so another rewrite rule can take care of them (although not so much if you've used nested categories, but again I can't really be bothered to account for that):

```
RewriteRule ^/archives/category/(.)(.\*) http://www.tenshu.net/search/label/${upmap:$1}$2 \[R=301,L\]
```

Also cloudified so far is the DNS for tenshu.net - I'm trying out Amazon's Route53 and it seems to be pretty good so far. Next up will be email and then I can pretty much entirely stop faffing around running my own infrastructure :)
