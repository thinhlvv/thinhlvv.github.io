---
layout: post
published: true
title: Note for golang logging
---

// Change the flags on the default logger
log.SetFlags(log.LstdFlags | log.Lshortfile)

// Print struct and value
fmt.Printf("%+v", iamstruct)
