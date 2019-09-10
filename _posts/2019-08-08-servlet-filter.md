---
layout: post
title:  "@WebFilter로 필터설정하기"
categories: servlet
tags: java,servlet
author: moai
---

출처 : [https://dololak.tistory.com/605][출처]  

### @WebFilter 어노테이션으로 필터 설정하기
서블릿 스펙 3.0 이전까지는 web.xml을 통해 Servlet과 Filter를 등록하고 URL 맵핑등을 설정하여 사용하였습니다. 그러나 서블릿 3.0 부터는 web.xml에서의 서블릿, 필터 설정을 자바 소스상에서 대체할 수 있는 어노테이션이 추가되었습니다.

@WebFilter 어노테이션은 필터를 등록하고 설정하는 어노테이션입니다. 서블릿 3.0은 톰캣을 기준으로 7 버전부터 지원하므로 톰캣7 이상의 서블릿컨테이너를 사용한다면 @WebFilter 어노테이션을 사용하여 필터를 등록할 수 있습니다.

기존과 다른점은 web.xml에 설정해둔 <filter> 태그를 대신해 필터 클래스에 @WebFilter 어노테이션을 걸어 설정해 주었다는 것 뿐입니다. web.xml에는 아무런 추가 설정이 필요없고 단지 @WebFilter를 필터 클래스에 걸어두기만 하면 됩니다.




<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="width:100%;margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(<span style="color:#63a35c">"*.jsp"</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#a71d5d">public</span>&nbsp;<span style="color:#a71d5d">class</span>&nbsp;LoggingFilter&nbsp;<span style="color:#a71d5d">implements</span>&nbsp;Filter&nbsp;{</div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

### @WebFilter 어노테이션의 설정들
@WebFilter 어노테이션은 web.xml의 <filter>태그나 <filter-mapping> 태그를 통해 사용하던 설정들을 모두 그대로 지원합니다. 설정들을 알아보도록 하겠습니다.


### 필터이름 지정 방법
web.xml에서 <filter> 태그에는 <filter-name> 이라는 태그를 지원하여 필터이름을 설정했습니다. @WebFilter은 filterName이라는 속성을 통해 설정할 수 있습니다. 다만 filterName속성은 필수값이 아닙니다.
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(filterName&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span><span style="color:#63a35c">"loggingFilter"</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#a71d5d">public</span>&nbsp;<span style="color:#a71d5d">class</span>&nbsp;LoggingFilter&nbsp;<span style="color:#a71d5d">implements</span>&nbsp;Filter&nbsp;{</div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

### 필터링 경로 설정 방법
web.xml에서는 <filter-mapping> 태그의 <url-pattern> 태그등을 통해서 필터링 대상 경로를 설정했었습니다.  @WebFilter 어노테이션의 필터링 대상 설정 방법은 다음과 같이 몇 가지 방법이 있습니다.

1. 어노테이션에 맵핑 URL을 입력하는 방법
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(<span style="color:#63a35c">"/target"</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

2. 어노테이션에 와일드카드를 사용하여 입력하는 방법
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(<span style="color:#63a35c">"/*"</span>)</div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

3. 어노테이션의 value 속성을 이용하는 방법
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(value<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span><span style="color:#63a35c">"/target"</span>)&nbsp;<span style="color:#999999">//또는</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(value<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;{<span style="color:#63a35c">"/target"</span>,&nbsp;<span style="color:#63a35c">"/target2"</span>})&nbsp;<span style="color:#999999">//여러개&nbsp;등록시에</span></div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

4. 어노테이션의 urlPatterns 속성을 이용하는 방법
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(urlPatterns<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span><span style="color:#63a35c">"/target"</span>)&nbsp;<span style="color:#999999">//또는</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(urlPatterns<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;{<span style="color:#63a35c">"/target"</span>,&nbsp;<span style="color:#63a35c">"/target2"</span>})&nbsp;<span style="color:#999999">//여러개&nbsp;등록시에</span></div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

5. 서블릿 등록시 사용한 서블릿 이름을 기준으로 지정하는 방법
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(servletNames<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span><span style="color:#63a35c">"boardController"</span>)&nbsp;<span style="color:#999999">//또는</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(servletNames<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;{<span style="color:#63a35c">"boardController"</span>,&nbsp;<span style="color:#63a35c">"boardController2"</span>})&nbsp;<span style="color:#999999">//여러개&nbsp;등록시에</span></div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

### @WebFilter 초기 파라미터 설정 방법
web.xml에서는 <filter> 태그의 <init-param> 태그를 통해  초기화 파라미터를 설정해 주었습니다. 만약 필터 초기화시에 설정할 파라미터를 지정하고 싶은 경우에는 initParams 속성에 @WebInitParam 어노테이션을 사용하여 속성명과 속성값을 지정해주면 됩니다.
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;value&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;{<span style="color:#63a35c">"/target"</span>,&nbsp;<span style="color:#63a35c">"/target2"</span>},</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;initParams&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;@WebInitParam(name&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;<span style="color:#63a35c">"encoding"</span>,&nbsp;value&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;<span style="color:#63a35c">"UTF-8"</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%">)</div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#a71d5d">public</span>&nbsp;<span style="color:#a71d5d">class</span>&nbsp;EncodingFilter&nbsp;<span style="color:#a71d5d">implements</span>&nbsp;Filter&nbsp;{</div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

만약 설정값을 여러개 주어야 한다면 배열 형태로 지정해 줄 수 있습니다.
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div><div style="line-height:130%">6</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;initParams&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;{</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@WebInitParam(name&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;<span style="color:#63a35c">"encoding"</span>,&nbsp;value&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;<span style="color:#63a35c">"UTF-8"</span>),</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@WebInitParam(name&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;<span style="color:#63a35c">"encoding2"</span>,&nbsp;value&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;<span style="color:#63a35c">"EUC-KR"</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;}</div><div style="padding:0 6px; white-space:pre; line-height:130%">)</div></div><div style="text-align:right;margin-top:-13px;margin-right:5px;font-size:9px;font-style:italic"><a href="http://colorscripter.com/info#e" target="_blank" style="color:#e5e5e5text-decoration:none">Colored by Color Scripter</a></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></td></tr></table></div>

### @WebFilter 필터 순서 지정 방법
아쉽게도 @WebFilter 어노테이션은 필터들 사이의 순서를 지정할 수 있는 속성은 제공하지 않습니다. 따라서 두가지 정도 차선책이 있는데, 하나는 필터를 구현할 때 필터들의 기능을 서로의 순서에 의존하지 않도록 설계 하는것입니다. 즉 어떤 순서로 필터가 수행되어도 상관없이 필터 정책을 사용하는것이죠. 두번째로는 @WebFilter에서 필터이름을 설정해두고 <filter-mapping> 태그를 통해 web.xml에서 순서를 지정하는 방법입니다.
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div><div style="line-height:130%">6</div><div style="line-height:130%">7</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(filterName<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;<span style="color:#63a35c">"firstFilter"</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#a71d5d">public</span>&nbsp;<span style="color:#a71d5d">class</span>&nbsp;FirstFilter&nbsp;<span style="color:#a71d5d">implements</span>&nbsp;Filter&nbsp;{</div><div style="padding:0 6px; white-space:pre; line-height:130%">cs</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(filterName<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;<span style="color:#63a35c">"secondFilter"</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#a71d5d">public</span>&nbsp;<span style="color:#a71d5d">class</span>&nbsp;SecondFilter&nbsp;<span style="color:#a71d5d">implements</span>&nbsp;Filter&nbsp;{</div><div style="padding:0 6px; white-space:pre; line-height:130%">cs</div></div><div style="text-align:right;margin-top:-13px;margin-right:5px;font-size:9px;font-style:italic"><a href="http://colorscripter.com/info#e" target="_blank" style="color:#e5e5e5text-decoration:none">Colored by Color Scripter</a></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>


아래와 같이 web.xml을 설정해두면 서블릿컨테이너가 시작될 때 web.xml의 필터 맵핑 태그를 순서대로 읽어 필터체인의 순서를 판단할것입니다.
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div><div style="line-height:130%">6</div><div style="line-height:130%">7</div><div style="line-height:130%">8</div><div style="line-height:130%">9</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;<span style="color:#010101">&lt;</span><span style="color:#066de2">filter-mapping</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#066de2">filter-name</span><span style="color:#010101">&gt;</span>firstFilter<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">filter-name</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#066de2">url-pattern</span><span style="color:#010101">&gt;</span>/*<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">url-pattern</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">filter-mapping</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#066de2">filter-mapping</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#066de2">filter-name</span><span style="color:#010101">&gt;</span>secondFilter<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">filter-name</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#066de2">url-pattern</span><span style="color:#010101">&gt;</span>/*<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">url-pattern</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">filter-mapping</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">cs</div></div><div style="text-align:right;margin-top:-13px;margin-right:5px;font-size:9px;font-style:italic"><a href="http://colorscripter.com/info#e" target="_blank" style="color:#e5e5e5text-decoration:none">Colored by Color Scripter</a></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

### 디스패쳐 지정하기
필터의 디스패쳐 타입을 지정하고 싶은 경우에는 dispatcherTypes 속성을 사용합니다. 기본값은 DispatcherType.REQUEST 이며 배열로 여러개 지정할 수 있습니다.
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div><div style="line-height:130%">6</div><div style="line-height:130%">7</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">@WebFilter(</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dispatcherTypes<span style="color:#0086b3"></span><span style="color:#a71d5d">=</span>&nbsp;{</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DispatcherType.REQUEST,</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DispatcherType.INCLUDE</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</div><div style="padding:0 6px; white-space:pre; line-height:130%">)</div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#a71d5d">public</span>&nbsp;<span style="color:#a71d5d">class</span>&nbsp;FirstFilter&nbsp;<span style="color:#a71d5d">implements</span>&nbsp;Filter&nbsp;{</div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

[출처]: https://dololak.tistory.com/605