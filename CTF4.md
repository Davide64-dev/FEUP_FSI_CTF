Não te esqueças de escrever um writeup de como conseguiste resolver este desafio, por exemplo num ficheiro CTF4.md ou no próprio LOGBOOK4.md no repositório git do teu grupo. Esse writeup deve incluir não apenas o resultado final, mas como construíste o ataque e de que vulnerabilidade abusaste e/ou que contramedidas poderiam ter evitado que tivesses tido sucess

resultado final: flag{c26b13e1f19414fd9a50a0e37197382c}

como construímos: mudança das variaveis de ambiente, criando um ficheiro com permissoes sudo para aceder à flag e dar output para ficheiro.

vulnerabilidade abusada: Almost always the /tmp has very open privileges, such that any process can write to them (chmod 777)

contramedidas: Do NOT use tmp folder to save files.
