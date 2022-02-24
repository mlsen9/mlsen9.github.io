---
title: 通过 Shell 脚本批量移动文件到指定目录
date: 2022-02-13 11:53:33 +0800
categories: [DevTools, Shell]
tags: [shell-file]
---

通过 Shell 脚本批量移动文件到指定目录

```shell
#!/bin/bash

src_dir="/home/src/*"
dist_dir="/home/dist"

for file in $src_dir
do
    if test -f $file
    then
        filename=${file##*/}
        ext=${file##*.}
		# 过滤掉其他文件，只处理jpg文件
		if [ "$ext" != "jpg" ]
		then
			continue
		fi

    # 处理文件
		dist_file="$dist_dir/$filename"
		echo "$file --mv--> $dist_file"
		mv -i $file $dist_file
    fi
done
```
