#include <stdio.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <unistd.h>
#include <string.h>

void read_command(char cmd[], char *par[])
{
  char line[1024];
  int count = 0, i = 0;
  char *array[100], *pch;

  // читать одну строку
  for ( ;; ) 
  {
    int c = fgetc(stdin);
    if (c == -1)
      return;

    line[count++] = (char)c;
    if (c == '\n')
      break;
  }
  if (count == 1) 
    return;

  pch = strtok(line, " \n");

  // разобрать строку в слова 
  while (pch != NULL) 
  {
    array[i++] = strdup(pch);
    pch = strtok(NULL, " \n");
  }
  // первое слово это команда
  strcpy(cmd, array[0]);
  
  // другие параметры
  for (int j = 0; j < i; j++ )
    par[j] = array[j];
  par[i] = NULL;  // NULL-завершить список параметров
}

void type_prompt()
{
  static int first_time = 1;
  unsigned int w;

  if (first_time) 
  {   //очистить экран в первый раз
    const char* CLEAR_SCREE_ANSI = " \e[1;1H\e[2J";
    w = write(STDOUT_FILENO,CLEAR_SCREE_ANSI,12); 
    if (w == -1)
      return;

    first_time = 0;
  }

  printf("𝔻𝕖𝕟𝕚𝕤:$$$ ");   //вывод приглашения
}

int main()
{
  char cmd[100], command[100], *parameters[20];
  char *envp[] = {(char *) "PATH=/bin", 0 };
  unsigned int stat;

  while(1) 
  {        
    type_prompt();              //вывод приглашения на экран на экране
    read_command(command, parameters); // чтение ввода с терминала
    if (fork() != 0)          //отвлетвление дочернего процесса 
      wait(NULL);            
    else
    {
      strcpy(cmd, "/bin/"); //присоединяет "/bin/"  к cmd
      strcat(cmd, command); //присоединяет command к cmd
      stat = execve(cmd, parameters, envp); // выподнения command
      if (stat == -1)
        printf("command: %s not found\n", command);
    }
  }
  return 0;
}
