# TelaBox — veja a tela da TV Box no navegador do notebook

Como funciona no dia a dia:

1. Liga a TV Box na tomada (cabo de rede ou Wi-Fi já configurado).
2. Espera uns 30 segundos. O TelaBox abre sozinho e começa a transmitir.
3. No notebook, abre o navegador e digita `http://IP-DA-TVBOX:8080`
   (ou abre o arquivo `Encontrar-TVBox.html`, que acha a TV Box sozinho).
4. A tela aparece ao vivo. Tem botão de **Tela cheia** e de **Foto**.

---

## 1. Gerar o APK (uma vez só)

O projeto está pronto, só falta compilar. Escolha um jeito:

**Jeito A — GitHub (sem instalar nada no notebook)**
1. Crie uma conta grátis em github.com.
2. Crie um repositório novo e envie todos os arquivos desta pasta
   (botão "Add file" → "Upload files"; arraste a pasta inteira, inclusive a pasta `.github`).
3. Vá na aba **Actions**. A tarefa "Gerar APK" roda sozinha (uns 3 minutos).
4. Clique na execução que ficou verde e baixe **TelaBox-apk** no fim da página.
   Dentro do zip está o `TelaBox.apk`.

**Jeito B — Android Studio**
Abra esta pasta no Android Studio e use *Build → Build APK(s)*.

## 2. Configuração inicial na TV Box (uma vez só)

O Android exige que alguém autorize a captura de tela. Isso precisa ser feito
**uma única vez**, e há dois caminhos:

### Caminho 1 — Com uma tela emprestada por alguns minutos
Ligue a TV Box numa TV ou monitor qualquer (de um vizinho, parente, etc.):

1. Instale o `TelaBox.apk` (pendrive + gerenciador de arquivos).
2. Abra o TelaBox. Vai aparecer o aviso de captura de tela:
   - **Android 9 ou mais antigo:** marque **"Não mostrar novamente"** e toque em **Iniciar agora**.
     Pronto, nunca mais pergunta.
   - **Android 10 ou mais novo:** esse aviso não tem a opção "não mostrar novamente".
     Toque em Iniciar agora e depois faça também o Caminho 2 (só os comandos `appops`).
3. No Android 10+, o app abre a tela **"Sobrepor a outros apps"**: ative para o TelaBox.
   Isso permite que ele abra sozinho quando a TV Box ligar.
4. Anote o IP que aparece na tela do TelaBox.

### Caminho 2 — Pelo notebook, com ADB (sem precisar de tela)
Funciona se a TV Box tiver a depuração de rede ligada (muitas genéricas vêm assim).
Instale as "Android Platform Tools" no notebook e, no terminal:

```
adb connect IP-DA-TVBOX:5555
adb install TelaBox.apk
adb shell appops set com.telabox.app PROJECT_MEDIA allow
adb shell appops set com.telabox.app SYSTEM_ALERT_WINDOW allow
adb shell am start -n com.telabox.app/.MainActivity
```

O primeiro `appops` faz o Android parar de perguntar a autorização.
O segundo deixa o TelaBox abrir sozinho ao ligar.

## 3. Descobrir o IP sem ver a tela

- Abra o `Encontrar-TVBox.html` no notebook e clique em **Procurar TV Box**; ou
- Entre na página do roteador (geralmente `192.168.0.1` ou `192.168.1.1`)
  e procure a TV Box na lista de dispositivos conectados.

Dica: na página do roteador dá para **reservar um IP fixo** para a TV Box
(procure por "DHCP estático" ou "reserva de IP"). Assim o endereço nunca muda.

## Problemas comuns

| Problema | O que fazer |
|---|---|
| Página não abre | Notebook e TV Box precisam estar no mesmo roteador. Espere 30 s depois de ligar. |
| Abre mas fica "Sem sinal" | A autorização de captura não foi dada. Refaça o passo 2. |
| Não inicia sozinho ao ligar | No Android 10+, falta a permissão "Sobrepor a outros apps" (passo 2). |
| Imagem lenta | Use cabo de rede na TV Box. Dá para baixar `MAX_WIDTH` em `ScreenService.java`. |
| Imagem preta em apps de filme | Netflix, Prime e afins bloqueiam captura de propósito (proteção de conteúdo). |

O TelaBox só mostra a imagem; não transmite o som nem controla a TV Box.
