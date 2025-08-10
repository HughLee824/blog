---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
slug: "{{ .Name }}"
tags: []
categories: []
summary: ""
description: ""
# 可选字段（按需填）
author: "Hugh"                           # 若与 site 不同
license: "CC BY-NC-SA 4.0"               # 覆盖站点默认
license_url: "https://creativecommons.org/licenses/by-nc-sa/4.0/"
reprint: true                            # 设为 false 可对单篇关闭

# PaperMod 常用开关（按需保留）
showToc: true
showBreadCrumbs: true
showReadingTime: true
comments: true

---

{{< reprint >}}
