
<p align="center">
  <a href="" rel="noopener">
 <img max-width=400px height=100px src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/45/Logo_CompassoUOL_Positivo.png/1200px-Logo_CompassoUOL_Positivo.png" alt="Project logo"></a>
</p>

<h1 align="center">Instalando Ansible no Oracle Linux</h1> 
<p align="center"><i>Fazendo a instalação Ansible no Oracle Linux e criando um arquivo YAML de ping para o IP de outra Máquina (VM)</i></p>

## 📝 Tabela de conteúdos
- [Fazendo a instalação do Ansible (Passo 1)](#step1)
- [Criando e exceutando um ansible-playbook (Passo 2)](#step2)
- [Referências](#documentation)

## 🔽 Fazendo a instalação do Ansible (Passo 1)<a name = "step1"></a>

Observação: O Ansible faz parte do repositório EPEL. E o Oracle Linux disponibiliza isso em 'yum.oracle.com'.

1. Habilitando o respositório EPEL

    ```
    sudo dnf install -y epel-release
    ```

2. Instalando o Ansible:

    ```
    sudo dnf install -y ansible
    ```

- Verifique a versão do Ansible instalada:

    ```
    ansible --version
    ```

- Você terá um resultado semelhante a esse:

    ```
    # ansible --version
    ansible 2.9.27
      config file = /etc/ansible/ansible.cfg
      configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
      ansible python module location = /usr/lib/python3.6/site-packages/ansible
      executable location = /usr/bin/ansible
      python version = 3.6.8 (default, Nov 10 2021, 06:50:23) [GCC 8.5.0 20210514 (Red Hat 8.5.0-3.0.2)]
    #
    ```


## ⚙️ Criando e exceutando um ansible-playbook (Passo 2)<a name = "step2"></a>

1. Vamos configurar o arquivo de inventário do Ansible

- Esse arquivo contém informações sobre os hosts (Máquinas) nos quais você deseja executar tarefas ou playbooks. Ele é usado para definir e organizar os host em grupos.

    ```
    $ inventory.ini
    $ nano inventory.ini
    ```

- Dentro do arquivo:

    ```yaml
    [desktop]
    192.168.100.83 ansible_user=root ansible_ssh_pass=Senha-Do-Usuario
    ```
    
    - Especifique o IP da outra máquina
    - Especifique também o usuário dessa máquina.
    - Especifique a senha do usuário.

    </br>

    - (No meu caso, utilizei o usuário root e sua senha para fazer a conexão ssh e executar o comando, devido a problemas com a senha do usuário da outra VM, não consegui fazer tal conexão com o usuário comum (FICA A DICA!))

- Outra observação: Caso encontre problemas para se conectar a outra máquina, verifique se a porta 22 da máquina de destino está aberta, verifique o firewall do seu sistema.

2. Criando um playbook para executar um ping em outra máquina.

- Crie um arquivo chamado "ping.yaml" (ou qualquer nome que você preferir)

    ```
    $ touch ping.yaml
    $ nano ping.yaml
    ```

- Dentro do arquivo:

    ```yaml
    ---
    - name: Ping Desktop Linux
      hosts: nome-do-host-ou-ip-da-outra-maquina
      tasks:
        - name: Executar o ping
          ping:
    ```

    Algumas observações sobre o playbook:
    - **name:** Descreve o que o playbook está fazendo. No exemplo acima, é "Ping Desktop Linux".
    - **hosts:** Especifica o nome do host ou o endereço IP da máquina remota que você deseja "pingar".
    - **tasks:** Lista de tarefas a serem executadas.
    - **name:** Descreve a tarefa.
    - **ping:** Módulo Ansible para executar o comando ping.

3. Execute o playbook.

    ```
    ansible-playbook -i inventory.ini ping.yaml
    ```

- No caso de resultado bem sucedido:

    <img src="./Screenshots/playbook-result.png" width="80%">


## Referências utilizadas:<a name="documentation"></a>

- [Instalando Ansible no Oracle Linux](https://oracle-base.com/articles/misc/ansible-first-steps)

- [Playbook Syntax - Docs Ansible](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html#playbook-syntax) 

- [Ping module - Docs Ansible](https://docs.ansible.com/ansible/2.8/modules/ping_module.html#examples)

