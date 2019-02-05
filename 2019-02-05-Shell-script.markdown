# Troubleshootings in Shell Script 

### "cd" 명령이 shell script에서 작동하지 않는 것처럼 보이는 이유

Shell scripts are run inside a subshell, and each subshell has its own concept of what the current directory is. The cd succeeds, but as soon as the subshell exits, you're back in the interactive shell and nothing ever changed there.

One way to get around this is to use an alias instead:

alias proj="cd /home/tree/projects/java"