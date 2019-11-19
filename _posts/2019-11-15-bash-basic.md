---
layout: post
title:  "bash basic"
categories: Linux
tags: Linux,Shell
author: moai
---

출처 : [https://blog.mwaysolutions.com/2014/06/05/10-best-practices-for-better-restful-api/][출처]  


|문법|설명|  
|:--------|--------:|  
|\>|                 &nbsp;&nbsp;&nbsp;출력 리다이렉션. 
                   명령 실행의 표준 출력(stdout)을 파일로 저장합니다. 
                   유닉스계열 운영체제는 장치도 파일로 처리하기 때문에 명령 실행 결과를 특정 장치로 보낼 수도 있습니다.|
```shell script
$ echo "hello" > ./hello.txt
$ echo "hello" > /dev/null
```
---
<	입력 리다이렉션. 파일의 내용을 읽어 명령의 표준 입력(stdin)으로 사용합니다.
$ cat < ./hello.txt
---

\>>	명령 실행의 표준 출력(stdout)을 파일에 추가합니다. >는 이미 있는 파일에 내용을 덮어쓰지만 >>는 파일 뒷부분에 내용을 추가합니다.
$ echo "world" >> ./hello.txt

2>	명령 실행의 표준 에러(stderr)를 파일로 저장합니다.

2>>	명령 실행의 표준 에러(stderr)를 파일에 추가합니다.

&>	표준 출력과 표준 에러를 모두 파일로 저장합니다.

1>&2	표준 출력을 표준 에러로 보냅니다. echo 명령으로 문자열을 표준 출력으로 출력했지만 표준 에러로 보냈기 때문에 변수에는 문자열이 들어가지 않습니다.
$ hello=$(echo "Hello World" 1>&2)
$ echo $hello
 
2>&1	표준 에러를 표준 출력으로 보냅니다. abcd라는 명령은 없으므로 에러가 발생하지만 에러를 표준 출력으로 보낸 뒤 다시 /dev/null로 보냈기 때문에 아무것도 출력되지 않습니다.
$ abcd > /dev/null 2>&1

|	파이프. 명령 실행의 표준 출력을 다른 명령의 표준 입력으로 보냅니다. 즉 첫 번째 명령의 출력 값을 두 번째 명령에서 처리합니다.
$ ls -al | grep .txt
$	Bash의 변수입니다. 값을 저장할 때는 $를 붙이지 않고, 변수를 가져다 쓸 때만 $를 붙입니다.
$ hello="Hello World"
$ echo $hello
Hello World

$()	명령 실행 결과를 변수화합니다. 명령 실행 결과를 변수에 저장하거나 다른 명령의 매개 변수로 넘겨줄 때 사용합니다. 또는 문자열안에 명령의 실행 결과를 넣을 때 사용합니다.
$ sudo docker rm $(docker ps -aq)
$ echo $(date)
Tue Sep 9 21:24:30 KST 2014

` `	$()과 마찬가지로 명령 실행 결과를 변수화합니다.
$ sudo docker rm `docker ps -aq`
$ echo `date`
Tue Sep 9 21:24:30 KST 2014

&&	한 줄에서 명령을 여러 개 실행합니다. 단 앞에 있는 명령이 에러 없이 실행되어야 뒤에 오는 명령이 실행됩니다.
$ make && make install

;	한 줄에서 명령을 여러 개 실행합니다. 앞에 있는 명령이 실패를 해도 뒤에 오는 명령이 실행됩니다.
$ false; echo "Hello"
Hello

' '	문자열입니다. ' '안에 들어있는 변수는 처리되지 않고 변수명 그대로 사용됩니다. 또한 ` `와 $()도 처리되지 않고 그대로 사용됩니다.
$ echo '$USER'
$USER
$USER가 그대로 출력됩니다.

" "	문자열입니다. 명령에 문자열 매개변수를 입력하거나 변수에 저장할 때 주로 사용합니다. ' '와는 달리 " "안에 변수가 들어있으면 변수의 내용으로 바뀝니다. 또한 ` `와 $()도 실행 결과 값이 사용됩니다.
$ echo "Hello World"
Hello World
$ echo "$USER"
pyrasis
$ echo "Host name is $(hostname)"
Host name is ubuntu
$ echo "Time: `date`"
Time: Tue Sep  9 21:28:10 KST 2014


" ' ' "	" "안에 ' '가 들어갈 수 있습니다. 명령 안에서 다시 명령을 실행하고 매개 변수를 지정할 때 사용합니다.
$ bash -c "/bin/echo Hello 'World'"
Hello World


\"
\$hello	' '안에서 "를 사용할 때는 \"처럼 앞에 \를 붙여줍니다.
$ bash -c "/bin/echo '{ \"user\": \"$USER\" }'"
{ "user": "pyrasis" }
" "안에서 ", $, ` 등의 특수문자를 그대로 사용하려면 앞에 \를 붙여줍니다.
$ echo "\$hello \" \`"
$hello " `


${}	변수 치환(substitution)입니다. " " 문자열 안에서 변수를 출력할 때 주로 사용합니다. ${} 대신 $만 사용해도 됩니다.
$ str="World"
$ echo "Hello ${str}"
Hello World
스크립트에서 변수의 기본 값을 설정할 때도 사용합니다. 다음은 HELLO 변수가 있으면 그대로 사용하고 변수가 없으면 기본 값으로 설정한 abcd를 대입합니다.
$ HELLO=
$ HELLO=${HELLO-"abcd"}
$ echo $HELLO
값이 NULL인 HELLO 변수가 이미 있기 때문에 기본 값을 대입하지 않습니다. 다음은 변수에 값이 있으면 그대로 사용하고, 값이 NULL이면 기본 값으로 설정한 abcd를 대입합니다.
$ WORLD=
$ WORLD=${WORLD:-"abcd"}
$ echo $WORLD
abcd
변수에 값이 NULL이므로 기본 값을 대입합니다.



\	한 줄로된 명령을 여러 줄로 표현할 때 사용합니다.
$ sudo docker run -d --name hello busybox:latest
$ sudo docker run \
    -d \
    --name hello \
    busybox:latest



{1..10}	연속된 숫자를 표현합니다. {시작 숫자..끝 숫자} 형식입니다.
$ echo {1..10}
1 2 3 4 5 6 7 8 9 10



{문자열1,문자열2}	{}안에 문자열을 여러 개 지정하여 명령 실행 횟수를 줄입니다. 다음은 hello.txt, world.txt 두 파일을 한번에 hello-dir 디렉터리 아래에 복사합니다.
$ cp ./{hello.txt,world.txt} hello-dir/



if	if 조건문입니다. 변수와 변수끼리 또는 문자열과 비교할 때 사용합니다.
if [ $a -eq $b ]; then
  echo $a
fi

 숫자 비교
-eq: 같다
-ne: 같지 않다
-gt: 초과
-ge: 이상
-lt: 미만
-le: 이하
문자열 비교
=, ==: 같다
!=: 같지 않다
-z: 문자열이 NULL일 때
-n: 문자열이 NULL이 아닐 때



for	for 반복문입니다. 변수안에 있는 값을 반복하거나 범위를 지정하여 반복할 수 있습니다.
for i in $(ls)
do
  echo $i
done

for (( i=0; i < 10; i++ ))
do
  echo $i
done

NUM=(1 2 3)
for i in ${NUM[@]}
do
  echo $i
done



while	while 반복문입니다.
while :
do
  echo "Hello World";
  sleep 1;
done



<<<	문자열을 명령(프로세스)의 표준 입력으로 보냅니다.
$ cat <<< "User name is $USER"
User name is pyrasis



<<EOF
EOF	여러 줄의 문자열을 명령(프로세스)의 표준 입력으로 보냅니다.
cat > ./hello.txt <<EOF
Hello World
Host name is $(hostname)
User name is $(USER)
EOF
cat은 파일이나 표준 입력의 내용을 출력하는 명령입니다. cat의 표준 출력을 ./hello.txt로 저장하고, <<EOF로 문자열을 cat의 표준 입력으로 보냅니다. 이렇게 하면 문자열 3줄이 ./hello.txt 파일에 저장됩니다.


export	설정한 값을 환경 변수로 만듭니다. export <변수>=<값> 형식입니다.
$ export HELLO=world



printf	지정한 형식대로 값을 출력합니다. 파이프와 연동하여 명령(프로세스)에 값을 입력하는 효과를 낼 수 있습니다.
$ printf 80\\nexampleuser\\ny | example-config
Port: 80
User: exampleuser
Save Configuration (y/n): y
예를 들어 example-config는 Port, User, Save Configuration을 사용자에게 입력을 받습니다. printf로 미리 값을 설정하여 파이프로 example-config에 넘겨주면 사용자가 입력하지 않아도 자동으로 값이 입력됩니다. 줄바꿈(개행)은 \\n으로 표현합니다.



sed	텍스트 파일에서 문자열을 변경합니다. hello.txt 파일의 내용 중에서 hello라는 문자열을 찾아서 world 문자열로 바꾸려면 다음과 같이 실행합니다.
$ sed -i "s/hello/world/g" hello.txt
sed -i "s/<찾을 문자열><바꿀 문자열>/g" <파일명> 형식입니다. /와 같은 특수 문자는 앞에 \를 붙여 \/로 입력합니다.



#	주석입니다. 스크립트에 설명을 추가하거나, 명령이 실행되지 않도록 합니다.
# echo "Hello World"