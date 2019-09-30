# Fake UNAME

uname command is often used in scripts to tell which OS is it running on.
Sometimes, when Linux gets updated too ofter, scripts may fail in abrupt
manner, receiving non-expected values from system uname.

In my exact case, I was building JRE 8 using Yocto's meta-java. 
And in one of the Makefiles, it asks Linux if it's too old or not. 
And my Linux 5.x is something the authors didn't expect, waiting
for 2.6, 3.x or 4.x instead.

In order to bypass this issue without writing any patches, I decided
to make a long-term solution to the issue: fake uname.

It just tells it's a some 4.x version of Linux, I didn't really care
which one exactly.

So how to use it?

I run build docker system using this command:
```
docker run --rm -it -v /home/yury:/home/pokyuser \
	-v /data/yocto:/data/yocto \
	-v /home/yury/uname:/bin/uname \
	crops/poky:ubuntu-16.04 \
	--workdir=/home/pokyuser
```

As you can see, uname from /home/yury is presented as /bin/uname
in a running Docker instance.

And it actually did trick for me. I hope, you may also find it
useful in such situations.
