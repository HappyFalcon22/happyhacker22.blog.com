+++ {{ $seed := now.Unix }} {{ $slice := slice "blue" "orange" "green" "red" "pink" }} {{ $item := index $slice (mod $seed (len $slice)) }}
title = "Insert here"
date = {{ .Date }}
author = "HappyHacker22"
draft = false
katex = true
color = "{{ $item }}"
description = "Insert here"
tag = ["here", "here", "here"]
enableEmoji = true
+++

