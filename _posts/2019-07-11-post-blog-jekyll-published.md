---
title: "지킬(Jekyll) 포스트 비공개로 발행 하기"
comment: true
date: 2019-07-11
categories: [Blogging, Jekyll]
tags: [published]

---

### 지킬(Jekyll) 포스트 비공개로 발행 하기


수정 할 것
- Jekyll
- YAML


#### Post.md 머리말에 있는 published 변수

포스트를 만들 때마다 머리말에 있는 yaml을 설정해줘야 하는데 해당 `yaml`에서 `published` 변수를 설정하고 값을 설정해주면 된다.

- published : true : 포스트 공개
- published : false : 포스트 비공개


``` yml
---
layout : post
title : [title post]
...
published : false
...
---
``` 



[참고자료 - https://jekyllrb.com/docs/frontmatter/](https://jekyllrb.com/docs/frontmatter/)