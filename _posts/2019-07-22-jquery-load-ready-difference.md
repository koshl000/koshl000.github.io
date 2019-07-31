---
layout: post
title:  "Jquery load/ready 차이점"
categories: Jquery
tags: Jquery,HTML,DOM
author: moai
---

##$(document).ready()

- HTML의 `document.addEventListener("DOMContentLoaded", function(event) {})`와 동일

- DOM (document object model) 트리를 생성 직후 실행

- 중복 사용시 선언 순서대로 사용




##$(window).load()

- HTML의 `window.onLoad=function(){...}`와 동일
 
- 모든 리소스들이 로드된 다음 실행

- 외부 링크나 include시 그 안에 `window.load` 스크립트가 있으면 둘 중 하나만 적용