Conditional Job Runner

Este repositório contém um workflow GitHub Actions que permite a execução condicional de comandos SSH, SFTP ou SCP com base nos inputs fornecidos pelo usuário. O workflow é acionado manualmente via a funcionalidade workflow_dispatch.

Configuração

Antes de usar este workflow, você precisa configurar alguns secrets no repositório do GitHub:

GitHub Secrets:
Vá para a aba Settings do seu repositório.
Selecione Secrets and variables > Actions.
Adicione um novo secret com o nome que será utilizado nos inputs (por exemplo, SSH_PRIVATE_KEY) e cole o valor do secret (por exemplo, a chave privada SSH).
Uso

Para acionar o workflow, siga os seguintes passos:

Vá para a aba Actions no seu repositório GitHub.
Selecione o workflow Conditional Job Runner.
Clique no botão Run workflow.
Preencha os campos solicitados:
message: A mensagem que você deseja usar no job.
url: A URL de destino (por exemplo, endereço do servidor).
secret_name: O nome do Secret configurado no GitHub (por exemplo, SSH_PRIVATE_KEY).
job_type: Selecione o tipo de job que deseja executar (ssh, sftp, scp).
Exemplo de Comando

Dependendo do tipo de job selecionado, o workflow executará um dos seguintes comandos:

SSH: Executará um comando remoto via SSH.
SFTP: Transferirá um arquivo via SFTP.
SCP: Copiará um arquivo via SCP.
SSH
yaml
Copy code
steps:
  - name: Execute SSH Command
    run: |
      ssh -i ${{ secrets[github.event.inputs.secret_name] }} user@${{ github.event.inputs.url }} "echo ${{ github.event.inputs.message }}"
SFTP
yaml
Copy code
steps:
  - name: Execute SFTP Command
    run: |
      sftp -i ${{ secrets[github.event.inputs.secret_name] }} user@${{ github.event.inputs.url }} <<EOF
      put somefile.txt
      bye
      EOF
SCP
yaml
Copy code
steps:
  - name: Execute SCP Command
    run: |
      scp -i ${{ secrets[github.event.inputs.secret_name] }} somefile.txt user@${{ github.event.inputs.url }}:~
Observações

Certifique-se de que o user e o caminho do arquivo (somefile.txt) estão configurados corretamente para o seu ambiente.
A chave SSH deve ter permissões adequadas para acessar o servidor de destino.
