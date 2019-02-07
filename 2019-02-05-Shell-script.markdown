# Troubleshootings in Shell Script 

### 1. "cd" doesn't work in shell script?

temporary 파일들이 쌓이는 어떤 폴더를 비워주는 간단한 shell script를 짜다가 다음과 같은 문제가 발생했다. 아래와 같이 shell script를 작성 후 실행시켰지만 working 디렉토리는 그대로. 아무 변화가 없었다.
```bash
$ vim remover.sh

#!/bin/bash
cd ~/Documents/example
:wq

$ chmod 755 remover.sh
```

그래서 알아본 결과 stackoverflow에서 다음과 같은 답을 얻을 수 있었다.

> Shell scripts are run inside a subshell, and each subshell has its own concept of what the current directory is. The cd succeeds, but as soon as the subshell exits, you're back in the interactive shell and nothing ever changed there.
> 
> One way to get around this is to use an alias instead:
> 
> alias proj="cd /home/tree/projects/java"
> [Ref]: https://askubuntu.com/questions/481715/why-doesnt-cd-work-in-a-shell-script

shell script 내의 명령들을 실행할 때는 bash child process가 하나 더 생기고 새로 생성된 bash에서 script를 실행한다. 또한 이외에도 (), $(), ``와 같은 명령 치환, | (파이프), & (bg 명령) 을 이용할때도 새로운 shell을 먼저 생성하는데 이와같은 shell을 subshell이라고 한다. 위와같은 경우 subshell에서 cd 명령이 실행되었지만 subshell의 working 디렉토리가 변경된 것일 뿐이다. script 실행이 종료되면 subshell이 종료될 것이고 parent shell에는 아무런 변화가 없다. 

다음과 같은 예제를 실행해보자
<subshell1.sh>
```bash
#!/bin/bash

echo "--------- step 1 ---------"
echo "Working directory is $(pwd)"
echo "Current running process is"
echo "$(ps)"

echo "--------- step 2 ---------"
./subshell2.sh

echo "--------- step 3 ---------"
cd Documents
echo "Working directory is $(pwd)"
```

<subshell2.sh>
```bash
#!/bin/bash

echo "Current running process is"
echo "$(ps)"  // terminal 하나를 더 열어 top 명령을 통해 모니터링해보자
sleep 3
```

이제 subshell1.sh 를 실행하면 다음과 같은 결과를 얻을 것이다. 

```bash
$ ./subshell1.sh

--------- step 1 ---------
Working directory is /Users/ik
Current running process is
  PID TTY           TIME CMD
 8525 ttys000    0:00.35 /bin/zsh -l
 9754 ttys001    0:00.40 -zsh
 9812 ttys001    0:00.00 /bin/bash ./subshell1.sh
--------- step 2 ---------
Current running process is
  PID TTY           TIME CMD
 8525 ttys000    0:00.35 /bin/zsh -l
 9754 ttys001    0:00.40 -zsh
 9812 ttys001    0:00.00 /bin/bash ./subshell1.sh
 9815 ttys001    0:00.00 /bin/bash ./subshell2.sh
--------- step 3 ---------
Working directory is /Users/ik/Documents

$
```

`step1, step2`: subshell이 생성된 것을 확인할 수 있다. (/bin/bash)
`step3`: cd 명령이 작동하여 working 디렉토리가 옮겨진 것을 확인할 수 있다.