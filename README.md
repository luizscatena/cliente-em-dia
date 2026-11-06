# Cliente em Dia

Painel para prestadores de serviço acompanharem clientes, orçamentos e retornos. Funciona em navegadores atuais com Web Crypto, sem conta, servidor ou assinatura. A interface está em português e usa reais.

## Abrir

Em hospedagem HTTPS, abra o endereço do site e crie uma senha mestra com pelo menos 12 caracteres. O app criptografa os registros antes de salvá-los neste navegador. Ele bloqueia após 10 minutos sem atividade; também há um botão **Bloquear**.

Para usar em outro aparelho, baixe **Baixar cópia criptografada**, leve o arquivo com segurança, abra o app no outro aparelho e use **Restaurar cópia criptografada** com a mesma senha. Não há sincronização automática.

## Proteção dos dados

- O cofre usa AES-256-GCM. A chave é derivada da senha com PBKDF2-HMAC-SHA-256, salt aleatório de 16 bytes e 600.000 iterações. Cada gravação usa um IV aleatório de 12 bytes.
- A senha mestra e os registros em texto claro não são salvos no armazenamento local. A senha permanece apenas em memória enquanto o cofre está desbloqueado.
- O app não envia registros a um servidor nem carrega bibliotecas de terceiros.
- O botão **WhatsApp** prepara uma mensagem de retorno e só abre o serviço quando você clica. O telefone e o texto seguem para o WhatsApp para revisão; o app não envia a mensagem automaticamente.
- Uma política de segurança do navegador permite apenas os arquivos locais do app e bloqueia conexões de rede, objetos incorporados e código inline.
- As cópias de segurança também são criptografadas. Sem a senha mestra, não há recuperação. Guarde a senha separadamente do arquivo de cópia.
- Quem controla o computador enquanto o cofre está desbloqueado, ou consegue executar código malicioso no navegador, ainda pode ver os dados. A criptografia protege principalmente o cofre e as cópias armazenadas.
- A migração automática detecta registros sem criptografia da versão anterior; remove a cópia antiga somente depois de salvar e conferir o novo cofre.

Consulte `RELATORIO-DE-REVISAO.md` para os testes realizados e os limites conhecidos.

## Hipótese de negócio

A versão gratuita pode atrair prestadores que precisam organizar retornos. Antes de criar recursos pagos, valide com 10 a 20 profissionais quais funções economizariam tempo suficiente para justificar pagamento. O app atual não cobra, não sincroniza dados e não gera renda automaticamente. Uma proposta comercial está em `../kit-do-prestador/PLANO-DE-VALIDACAO-E-RECEITA.md`.

