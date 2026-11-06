# Relatório de testes e segurança

## Resultado

**Os fluxos testados passaram.** A criptografia em repouso, o bloqueio, a troca de senha, a cópia portátil e a migração dos dados antigos foram verificados com o navegador simulado e a API criptográfica real do Node.js. Não encontrei erro bloqueante nesses fluxos. Isso não equivale a auditoria independente nem a testes em todos os navegadores.

## Testes executados

- Sintaxe do JavaScript.
- Criar cofre; salvar registros e confirmar que nome e telefone não aparecem no armazenamento local em texto claro.
- Bloquear, rejeitar senha incorreta e desbloquear com a senha certa.
- Exportar cópia criptografada e restaurá-la em um perfil de navegador separado.
- Rejeitar restauração com senha errada e cópia adulterada.
- Trocar a senha e confirmar que a senha antiga deixa de abrir o cofre.
- Migrar registros existentes da versão antiga e remover a versão sem criptografia somente após gravar o cofre protegido.
- Revisão estática: sem scripts/estilos remotos, `innerHTML`, `eval`, `Function`, `fetch`, `XMLHttpRequest`, `WebSocket` ou `sendBeacon`.
- Política CSP: scripts e estilos locais apenas; conexões de rede, objetos e código inline bloqueados.
- Importação de conteúdo HTML de teste: exibido como texto inerte, sem criar elementos de imagem ou script.

Os testes automatizados usam Node.js 24, um DOM simulado e o Web Crypto do Node. O diálogo real de download e a aparência em cada navegador/dispositivo ainda precisam de verificação manual.

## Modelo de segurança

- O armazenamento local e o arquivo de cópia guardam apenas o envelope cifrado: AES-256-GCM, nonce/IV aleatório de 96 bits e chave derivada por PBKDF2-HMAC-SHA-256 com salt aleatório de 128 bits e 600.000 iterações.
- A interface exige senha mestra com pelo menos 12 caracteres; uma frase longa e exclusiva é preferível. A senha não é enviada nem salva pelo app.
- O navegador mantém a chave e os registros abertos em memória até bloqueio manual ou 10 minutos sem atividade.
- A restauração valida formato, versão, limite de 2 MB, 1.000 registros, campos e IDs duplicados antes de substituir o cofre.
- Valores de clientes são renderizados como texto; conteúdo HTML importado não vira markup.
- A CSP no HTML restringe as fontes de script e estilo ao próprio app, bloqueia conexões de rede, plugins e execução inline. Como é aplicada por uma meta tag, ela não oferece proteção contra enquadramento da página; isso exigiria cabeçalho do servidor.

## Limites e riscos restantes

- Se a senha mestra for perdida e não houver uma sessão já desbloqueada, não há recuperação dos dados.
- Malware, extensões maliciosas, keyloggers ou alguém usando o aparelho enquanto o cofre está aberto podem capturar informações. A criptografia não protege uma sessão ativa comprometida.
- Se o código público do site for alterado de forma maliciosa, ele poderia capturar a senha na próxima vez que for digitada. Revise mudanças antes de publicar atualizações.
- Não há sincronização automática, conta de usuário ou cópia na nuvem. A transferência entre aparelhos é manual e depende de guardar o arquivo criptografado.
- Ainda não foi feito teste visual em Chrome, Firefox e Safari nem uma auditoria profissional.

## Referências técnicas

- OWASP recomenda PBKDF2-HMAC-SHA-256 com 600.000 iterações quando PBKDF2 é usado: [Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html).
- A documentação do Web Crypto informa que AES-GCM normalmente usa um IV aleatório de 96 bits: [MDN — AesGcmParams](https://developer.mozilla.org/en-US/docs/Web/API/AesGcmParams).
- OWASP recomenda CSP como camada adicional contra XSS e descreve restrições para fontes de script: [Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html).
- OWASP recomenda inserir dados não confiáveis com `textContent` em vez de `innerHTML`: [Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).
- OWASP alerta que qualquer script da mesma origem pode acessar `localStorage`, então o cofre mantém apenas dados cifrados ali: [HTML5 Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html).

